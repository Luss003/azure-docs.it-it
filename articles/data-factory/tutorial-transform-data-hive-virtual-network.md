---
title: Trasformare dati con Hive nella rete virtuale di Azure
description: Questa esercitazione offre istruzioni dettagliate per la trasformazione dei dati tramite l'attività Hive in Azure Data Factory.
services: data-factory
ms.service: data-factory
ms.workload: data-services
author: nabhishek
ms.author: abnarain
manager: anandsub
ms.topic: tutorial
ms.custom: seo-dt-2019
ms.date: 01/22/2018
ms.openlocfilehash: ab8df188027ada2119334e058ffc5a10cca23914
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75439160"
---
# <a name="transform-data-in-azure-virtual-network-using-hive-activity-in-azure-data-factory"></a>Trasformare dati nella rete virtuale di Azure usando l'attività Hive in Azure Data Factory

In questa esercitazione si usa Azure PowerShell per creare una pipeline di Data Factory che trasforma i dati con un'attività Hive in un cluster HDInsight che si trova in una rete virtuale di Azure. In questa esercitazione vengono completati i passaggi seguenti:

> [!div class="checklist"]
> * Creare una data factory. 
> * Creare e configurare il runtime di integrazione self-hosted
> * Creare e distribuire servizi collegati.
> * Creare e distribuire una pipeline che contiene un'attività Hive.
> * Avviare un'esecuzione della pipeline.
> * Monitorare l'esecuzione della pipeline 
> * Verificare l'output. 

Se non si ha una sottoscrizione di Azure, creare un account [gratuito](https://azure.microsoft.com/free/) prima di iniziare.

## <a name="prerequisites"></a>Prerequisites

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Account di archiviazione di Azure**. Creare uno script Hive e caricarlo nell'archivio di Azure. L'output dello script Hive viene archiviato in questo account di archiviazione. In questo esempio, il cluster HDInsight usa questo account di archiviazione di Azure come risorsa di archiviazione primaria. 
- **Rete virtuale di Azure.** Se non è disponibile una rete virtuale di Azure, crearla seguendo [queste istruzioni](../virtual-network/quick-create-portal.md). In questo esempio, il cluster HDInsight si trova in una rete virtuale di Azure. Ecco una configurazione di esempio della rete virtuale di Azure. 

    ![Creare una rete virtuale](media/tutorial-transform-data-using-hive-in-vnet/create-virtual-network.png)
- **Cluster HDInsight.** Creare un cluster HDInsight e aggiungerlo alla rete virtuale creata nel passaggio precedente seguendo questo articolo: [Estendere Azure HDInsight usando Rete virtuale di Azure](../hdinsight/hdinsight-extend-hadoop-virtual-network.md). Ecco una configurazione di esempio di HDInsight in una rete virtuale. 

    ![HDInsight in una rete virtuale](media/tutorial-transform-data-using-hive-in-vnet/hdinsight-in-vnet-configuration.png)
- **Azure PowerShell**. Seguire le istruzioni in [Come installare e configurare Azure PowerShell](/powershell/azure/install-Az-ps).

### <a name="upload-hive-script-to-your-blob-storage-account"></a>Caricare lo script Hive nell'account di archiviazione BLOB

1. Creare un file Hive SQL denominato **hivescript.hql** con il contenuto seguente:

   ```sql
   DROP TABLE IF EXISTS HiveSampleOut; 
   CREATE EXTERNAL TABLE HiveSampleOut (clientid string, market string, devicemodel string, state string)
   ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' 
   STORED AS TEXTFILE LOCATION '${hiveconf:Output}';

   INSERT OVERWRITE TABLE HiveSampleOut
   Select 
       clientid,
       market,
       devicemodel,
       state
   FROM hivesampletable
   ```
2. Nell'Archivio BLOB di Azure creare un contenitore denominato **adftutorial**, se non esiste.
3. Creare una cartella denominata **hivescripts**.
4. Caricare il file **hivescript.hql** nella sottocartella **hivescripts**.

  

## <a name="create-a-data-factory"></a>Creare una data factory


1. Impostare il nome del gruppo di risorse. Come parte di questa esercitazione viene creato un gruppo di risorse. È tuttavia possibile usare un gruppo di risorse esistente, se si preferisce. 

    ```powershell
    $resourceGroupName = "ADFTutorialResourceGroup" 
    ```
2. Specificare il nome della data factory. Deve essere univoco a livello globale.

    ```powershell
    $dataFactoryName = "MyDataFactory09142017"
    ```
3. Specificare un nome per la pipeline. 

    ```powershell
    $pipelineName = "MyHivePipeline" # 
    ```
4. Specificare un nome per l'istanza di Integration Runtime (self-hosted). Integration Runtime (self-hosted) è necessario quando Data Factory deve accedere alle risorse (ad esempio il database SQL di Azure) all'interno di una rete virtuale. 
    ```powershell
    $selfHostedIntegrationRuntimeName = "MySelfHostedIR09142017" 
    ```
2. Avviare **PowerShell**. Tenere aperto Azure PowerShell fino al termine di questa guida introduttiva. Se si chiude e si riapre, sarà necessario eseguire di nuovo questi comandi. Per un elenco di aree di Azure in cui Data Factory è attualmente disponibile, selezionare le aree di interesse nella pagina seguente, quindi espandere **Analytics** per individuare **Data Factory**: [Prodotti disponibili in base all'area](https://azure.microsoft.com/global-infrastructure/services/). Gli archivi dati (Archiviazione di Azure, database SQL di Azure e così via) e le risorse di calcolo (HDInsight e così via) usati dalla data factory possono trovarsi in altre aree.

    Eseguire questo comando e immettere il nome utente e la password usati per accedere al portale di Azure:
        
    ```powershell
    Connect-AzAccount
    ```        
    Eseguire questo comando per visualizzare tutte le sottoscrizioni per l'account:

    ```powershell
    Get-AzSubscription
    ```
    Eseguire il comando seguente per selezionare la sottoscrizione da usare. Sostituire **SubscriptionId** con l'ID della sottoscrizione di Azure:

    ```powershell
    Select-AzSubscription -SubscriptionId "<SubscriptionId>"    
    ```  
3. Creare il gruppo di risorse: ADFTutorialResourceGroup se non esiste già nella sottoscrizione. 

    ```powershell
    New-AzResourceGroup -Name $resourceGroupName -Location "East Us" 
    ```
4. Creare la data factory. 

    ```powershell
     $df = Set-AzDataFactoryV2 -Location EastUS -Name $dataFactoryName -ResourceGroupName $resourceGroupName
    ```

    Eseguire questo comando per visualizzare l'output: 

    ```powershell
    $df
    ```

## <a name="create-self-hosted-ir"></a>Creare un runtime di integrazione self-hosted
In questa sezione si crea un runtime di integrazione self-hosted e lo si associa a una macchina virtuale di Azure nella stessa rete virtuale di Azure in cui si trova il cluster HDInsight.

1. Creare un runtime di integrazione self-hosted. Usare un nome univoco nel caso esista un altro runtime di integrazione con lo stesso nome.

   ```powershell
   Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntimeName -Type SelfHosted
   ```
    Questo comando crea una registrazione logica del runtime di integrazione self-hosted. 
2. Usare PowerShell per recuperare le chiavi di autenticazione per registrare il runtime di integrazione self-hosted. Copiare una delle chiavi per registrare il runtime di integrazione self-hosted.

   ```powershell
   Get-AzDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntimeName | ConvertTo-Json
   ```

   Di seguito è riportato l'output di esempio: 

   ```powershell
   {
       "AuthKey1":  "IR@0000000000000000000000000000000000000=",
       "AuthKey2":  "IR@0000000000000000000000000000000000000="
   }
   ```
    Annotare il valore di **AuthKey1** senza virgolette. 
3. Creare una macchina virtuale di Azure e aggiungerla alla stessa rete virtuale che contiene il cluster HDInsight. Per informazioni dettagliate, vedere [come creare macchine virtuali](../virtual-network/quick-create-portal.md#create-virtual-machines). Aggiungerle a una rete virtuale di Azure. 
4. Nella macchina virtuale di Azure, scaricare il [runtime di integrazione self-hosted](https://www.microsoft.com/download/details.aspx?id=39717). Usare la chiave di autenticazione ottenuta nel passaggio precedente per registrare manualmente il runtime di integrazione self-hosted. 

   ![Registrare il runtime di integrazione](media/tutorial-transform-data-using-hive-in-vnet/register-integration-runtime.png)

   Al termine della registrazione del runtime di integrazione self-hosted viene visualizzato il messaggio seguente: ![Registrazione completata](media/tutorial-transform-data-using-hive-in-vnet/registered-successfully.png)

   Quando il nodo viene connesso al servizio cloud, viene visualizzata la pagina seguente: ![Connessione del nodo](media/tutorial-transform-data-using-hive-in-vnet/node-is-connected.png)

## <a name="author-linked-services"></a>Creare servizi collegati

In questa sezione vengono creati e distribuiti due servizi collegati:
- Un servizio collegato di Archiviazione di Azure che collega l'account di archiviazione di Azure alla data factory. Questa risorsa di archiviazione è l'archiviazione primaria usata dal cluster HDInsight. In questo caso, l'account di archiviazione di Azure viene anche usato per conservare lo script Hive e l'output dello script.
- Un servizio collegato HDInsight. Azure Data Factory invia lo script Hive a questo cluster HDInsight per l'esecuzione.

### <a name="azure-storage-linked-service"></a>Servizio collegato Archiviazione di Azure

Creare un file JSON usando l'editor preferito, copiare la definizione JSON seguente di un servizio collegato di Archiviazione di Azure e quindi salvare il file con il nome **MyStorageLinkedService.json**.

```json
{
    "name": "MyStorageLinkedService",
    "properties": {
      "type": "AzureStorage",
      "typeProperties": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=<storageAccountName>;AccountKey=<storageAccountKey>"
      },
      "connectVia": {
        "referenceName": "MySelfhostedIR",
        "type": "IntegrationRuntimeReference"
      }  
    }
}
```

Sostituire **&lt;accountname&gt; e &lt;accountkey&gt;** con il nome e la chiave dell'account di archiviazione di Azure.

### <a name="hdinsight-linked-service"></a>Servizio collegato HDInsight

Creare un file JSON usando l'editor preferito, copiare la definizione JSON seguente di un servizio collegato di Azure HDInsight e quindi salvare il file con il nome **MyHDInsightLinkedService.json**.

```
{
  "name": "MyHDInsightLinkedService",
  "properties": {     
      "type": "HDInsight",
      "typeProperties": {
          "clusterUri": "https://<clustername>.azurehdinsight.net",
          "userName": "<username>",
          "password": {
            "value": "<password>",
            "type": "SecureString"
          },
          "linkedServiceName": {
            "referenceName": "MyStorageLinkedService",
            "type": "LinkedServiceReference"
          }
      },
      "connectVia": {
        "referenceName": "MySelfhostedIR",
        "type": "IntegrationRuntimeReference"
      }
  }
}
```

Aggiornare i valori per le proprietà seguenti nella definizione del servizio collegato:

- **userName**. Nome utente dell'account di accesso del cluster specificato durante la creazione del cluster. 
- **password**. La password dell'utente.
- **clusterUri**. Specificare l'URL del cluster HDInsight nel formato seguente: `https://<clustername>.azurehdinsight.net`.  Questo articolo presuppone che sia disponibile l'accesso al cluster tramite Internet, ad esempio che sia possibile connettersi al cluster all'indirizzo `https://clustername.azurehdinsight.net`. Questo indirizzo usa il gateway pubblico, che non è disponibile se sono stati usati gruppi di sicurezza di rete o route definite dall'utente per limitare l'accesso da Internet. Per consentire a Data Factory di inviare processi ai cluster HDInsight nella rete virtuale di Azure, è necessario configurare la rete virtuale di Azure in modo che l'URL possa essere risolto nell'indirizzo IP privato del gateway usato da HDInsight.

  1. Nel portale di Azure aprire la rete virtuale in cui si trova il cluster HDInsight. Aprire l'interfaccia di rete avente il nome che inizia con `nic-gateway-0`. Annotarne l'indirizzo IP privato, ad esempio 10.6.0.15. 
  2. Se la rete virtuale di Azure ha un server DNS, aggiornare il record DNS in modo che l'URL del cluster HDInsight `https://<clustername>.azurehdinsight.net` possa essere risolto in `10.6.0.15`. Questo è l'approccio consigliato. Se non è disponibile un server DNS nella rete virtuale di Azure, il problema può essere temporaneamente risolto modificando il file hosts (C:\Windows\System32\drivers\etc) di tutte le macchine virtuali registrate come nodi di Integration Runtime (self-hosted) aggiungendo una voce simile alla seguente: 
  
        `10.6.0.15 myHDIClusterName.azurehdinsight.net`

## <a name="create-linked-services"></a>Creare servizi collegati
In PowerShell passare alla cartella in cui sono stati creati i file JSON ed eseguire il comando seguente per distribuire i servizi collegati: 

1. In PowerShell passare alla cartella in cui sono stati creati i file JSON.
2. Eseguire il comando seguente per creare un servizio collegato Archiviazione di Azure. 

    ```powershell
    Set-AzDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "MyStorageLinkedService" -File "MyStorageLinkedService.json"
    ```
3. Eseguire il comando seguente per creare un servizio collegato Azure HDInsight. 

    ```powershell
    Set-AzDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "MyHDInsightLinkedService" -File "MyHDInsightLinkedService.json"
    ```

## <a name="author-a-pipeline"></a>Creare una pipeline
In questo passaggio si crea una nuova pipeline con un'attività Hive. L'attività esegue uno script Hive per restituire i dati da una tabella di esempio e salvarli in un percorso definito dall'utente. Creare un file JSON usando l'editor preferito, copiare la definizione JSON seguente di una pipeline e quindi salvare il file con il nome **MyHivePipeline.json**.


```json
{
  "name": "MyHivePipeline",
  "properties": {
    "activities": [
      {
        "name": "MyHiveActivity",
        "type": "HDInsightHive",
        "linkedServiceName": {
            "referenceName": "MyHDILinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
          "scriptPath": "adftutorial\\hivescripts\\hivescript.hql",
          "getDebugInfo": "Failure",
          "defines": {           
            "Output": "wasb://<Container>@<StorageAccount>.blob.core.windows.net/outputfolder/"
          },
          "scriptLinkedService": {
            "referenceName": "MyStorageLinkedService",
            "type": "LinkedServiceReference"
          }
        }
      }
    ]
  }
}

```

Tenere presente quanto segue:

- **scriptPath** punta al percorso dello script Hive nell'account di archiviazione di Azure usato per MyStorageLinkedService. Per il percorso viene applicata la distinzione tra maiuscole e minuscole.
- **Output** è un argomento usato nello script Hive. Usare il formato `wasb://<Container>@<StorageAccount>.blob.core.windows.net/outputfolder/` in modo che l'output punti a una cartella esistente nell'archivio di Azure. Per il percorso viene applicata la distinzione tra maiuscole e minuscole. 

Passare alla cartella in cui sono stati creati i file JSON ed eseguire questo comando per distribuire la pipeline: 


```powershell
Set-AzDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name $pipelineName -File "MyHivePipeline.json"
```

## <a name="start-the-pipeline"></a>Avviare la pipeline 

1. Avviare un'esecuzione della pipeline. Viene anche acquisito l'ID di esecuzione della pipeline per il monitoraggio futuro.

    ```powershell
    $runId = Invoke-AzDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName $pipelineName
   ```
2. Eseguire questo script per verificare costantemente lo stato di esecuzione della pipeline fino al termine.

    ```powershell
    while ($True) {
        $result = Get-AzDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)

        if(!$result) {
            Write-Host "Waiting for pipeline to start..." -foregroundcolor "Yellow"
        }
        elseif (($result | Where-Object { $_.Status -eq "InProgress" } | Measure-Object).count -ne 0) {
            Write-Host "Pipeline run status: In Progress" -foregroundcolor "Yellow"
        }
        else {
            Write-Host "Pipeline '"$pipelineName"' run finished. Result:" -foregroundcolor "Yellow"
            $result
            break
        }
        ($result | Format-List | Out-String)
        Start-Sleep -Seconds 15
    }
    
    Write-Host "Activity `Output` section:" -foregroundcolor "Yellow"
    $result.Output -join "`r`n"

    Write-Host "Activity `Error` section:" -foregroundcolor "Yellow"
    $result.Error -join "`r`n"
    ```

   Ecco l'output dell'esecuzione di esempio:

    ```json
    Pipeline run status: In Progress
    
    ResourceGroupName : ADFV2SampleRG2
    DataFactoryName   : SampleV2DataFactory2
    ActivityName      : MyHiveActivity
    PipelineRunId     : 000000000-0000-0000-000000000000000000
    PipelineName      : MyHivePipeline
    Input             : {getDebugInfo, scriptPath, scriptLinkedService, defines}
    Output            :
    LinkedServiceName :
    ActivityRunStart  : 9/18/2017 6:58:13 AM
    ActivityRunEnd    :
    DurationInMs      :
    Status            : InProgress
    Error             :
    
    Pipeline ' MyHivePipeline' run finished. Result:
    
    ResourceGroupName : ADFV2SampleRG2
    DataFactoryName   : SampleV2DataFactory2
    ActivityName      : MyHiveActivity
    PipelineRunId     : 0000000-0000-0000-0000-000000000000
    PipelineName      : MyHivePipeline
    Input             : {getDebugInfo, scriptPath, scriptLinkedService, defines}
    Output            : {logLocation, clusterInUse, jobId, ExecutionProgress...}
    LinkedServiceName :
    ActivityRunStart  : 9/18/2017 6:58:13 AM
    ActivityRunEnd    : 9/18/2017 6:59:16 AM
    DurationInMs      : 63636
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    Activity Output section:
    "logLocation": "wasbs://adfjobs@adfv2samplestor.blob.core.windows.net/HiveQueryJobs/000000000-0000-47c3-9b28-1cdc7f3f2ba2/18_09_2017_06_58_18_023/Status"
    "clusterInUse": "https://adfv2HivePrivate.azurehdinsight.net"
    "jobId": "job_1505387997356_0024"
    "ExecutionProgress": "Succeeded"
    "effectiveIntegrationRuntime": "MySelfhostedIR"
    Activity Error section:
    "errorCode": ""
    "message": ""
    "failureType": ""
    "target": "MyHiveActivity"
    ```
4. Cercare nella cartella `outputfolder` il nuovo file creato come risultato della query Hive. L'output del file sarà simile all'esempio seguente: 

   ```
   8 en-US SCH-i500 California
   23 en-US Incredible Pennsylvania
   212 en-US SCH-i500 New York
   212 en-US SCH-i500 New York
   212 en-US SCH-i500 New York
   212 en-US SCH-i500 New York
   212 en-US SCH-i500 New York
   212 en-US SCH-i500 New York
   212 en-US SCH-i500 New York
   212 en-US SCH-i500 New York
   212 en-US SCH-i500 New York
   212 en-US SCH-i500 New York
   212 en-US SCH-i500 New York
   212 en-US SCH-i500 New York
   246 en-US SCH-i500 District Of Columbia
   246 en-US SCH-i500 District Of Columbia
   ```

## <a name="next-steps"></a>Passaggi successivi
In questa esercitazione sono stati eseguiti i passaggi seguenti: 

> [!div class="checklist"]
> * Creare una data factory. 
> * Creare e configurare il runtime di integrazione self-hosted
> * Creare e distribuire servizi collegati.
> * Creare e distribuire una pipeline che contiene un'attività Hive.
> * Avviare un'esecuzione della pipeline.
> * Monitorare l'esecuzione della pipeline 
> * Verificare l'output. 

Passare all'esercitazione successiva per informazioni sulla trasformazione dei dati usando un cluster Spark in Azure:

> [!div class="nextstepaction"]
>[Diramazione e concatenamento del flusso di controllo di Data factory](tutorial-control-flow.md)



