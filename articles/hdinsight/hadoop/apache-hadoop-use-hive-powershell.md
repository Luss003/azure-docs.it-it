---
title: Usare Apache Hive con PowerShell in HDInsight - Azure
description: Usare PowerShell per eseguire query Apache Hive in Apache Hadoop in Azure HDInsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: hrasheed
ms.openlocfilehash: 89fa7976b922ba0e40e97b72de5d4eb9a02f0dfd
ms.sourcegitcommit: 97605f3e7ff9b6f74e81f327edd19aefe79135d2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/06/2019
ms.locfileid: "70736074"
---
# <a name="run-apache-hive-queries-using-powershell"></a>Eseguire query Apache Hive usando PowerShell
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Questo documento fornisce un esempio di come usare Azure PowerShell nel gruppo di risorse Azure per eseguire query Hive in un cluster Apache Hadoop in HDInsight.

> [!NOTE]  
> Questo documento non fornisce una descrizione dettagliata delle operazioni eseguite dalle istruzioni HiveQL usate negli esempi. Per informazioni sul codice HiveQL usato in questo esempio, vedere [Usare Apache Hive con Apache Hadoop in HDInsight](hdinsight-use-hive.md).

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

* Un cluster Apache Hadoop basato su Linux in HDInsight versione 3.4 o successiva.

* Un client con Azure PowerShell.

[!INCLUDE [upgrade-powershell](../../../includes/hdinsight-use-latest-powershell.md)]

## <a name="run-a-hive-query"></a>Eseguire una query Hive

Azure PowerShell fornisce *cmdlet* che consentono di eseguire in modalità remota query Hive in HDInsight. I cmdlet effettuano internamente chiamate REST a [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) sul cluster HDInsight.

Durante l'esecuzione di query Hive in un cluster HDInsight remoto, vengono usati i seguenti cmdlet:

* `Connect-AzAccount`: esegue l'autenticazione di Azure PowerShell nella sottoscrizione di Azure.
* `New-AzHDInsightHiveJobDefinition`: crea una *definizione del processo* usando le istruzioni HiveQL specificate.
* `Start-AzHDInsightJob`: invia la definizione del processo ad HDInsight e avvia il processo. Viene restituito un oggetto del *processo*.
* `Wait-AzHDInsightJob`: usa l'oggetto processo per verificare lo stato del processo. Attende che il processo venga completato o che scada il periodo di attesa previsto.
* `Get-AzHDInsightJobOutput`: usato per recuperare l'output del processo.
* `Invoke-AzHDInsightHiveJob`: usato per eseguire le istruzioni HiveQL. Questo cmdlet blocca il completamento della query, quindi restituisce i risultati.
* `Use-AzHDInsightCluster`: imposta il cluster corrente da usare per il comando `Invoke-AzHDInsightHiveJob`.

La seguente procedura illustra come usare questi cmdlet per eseguire un processo nel cluster HDInsight:

1. Usando un editor salvare il codice seguente come `hivejob.ps1`.

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=5-42)]

2. Quindi, aprire un nuovo prompt dei comandi di **Azure PowerShell** . Passare alla directory del file `hivejob.ps1` e quindi usare il comando seguente per eseguire lo script:

        .\hivejob.ps1

    Quando viene eseguito lo script, viene richiesto di immettere il nome cluster e le credenziali dell'account HTTPS/Amministratore del cluster. Potrebbe anche essere richiesto di accedere alla sottoscrizione di Azure.

3. Al termine, il processo restituisce informazioni simili al testo seguente:

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. Come accennato in precedenza, è possibile usare `Invoke-Hive` per eseguire una query e attendere la risposta. Usare lo script seguente per verificare il funzionamento di Invoke-Hive:

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=50-71)]

    L'output ha un aspetto simile al testo seguente:

        2012-02-03    18:35:34    SampleClass0    [ERROR]    incorrect    id
        2012-02-03    18:55:54    SampleClass1    [ERROR]    incorrect    id
        2012-02-03    19:25:27    SampleClass4    [ERROR]    incorrect    id

   > [!NOTE]  
   > Per query HiveQL più lunghe, è possibile usare il cmdlet **Here-Strings** di Azure PowerShell o un file di script HiveQL. Il frammento di codice seguente illustra come usare il cmdlet `Invoke-Hive` per eseguire un file di script HiveQL. Il file di script HiveQL deve essere caricato in wasb://.
   >
   > `Invoke-AzHDInsightHiveJob -File "wasb://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
   >
   > Per altre informazioni su **Here-Strings**, vedere <a href="https://technet.microsoft.com/library/ee692792.aspx" target="_blank">Using Windows PowerShell "Here-Strings"</a> (Uso di Here-Strings di Windows PowerShell).

## <a name="troubleshooting"></a>Risoluzione dei problemi

Se al termine del processo non viene restituita alcuna informazione, visualizzare i log degli errori. Per visualizzare le informazioni sugli errori relative a questo processo, aggiungere il codice seguente alla fine del file `hivejob.ps1`, salvare il file e quindi eseguirlo nuovamente.

```powershell
# Print the output of the Hive job.
Get-AzHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Questo cmdlet restituisce le informazioni scritte in STDERR durante l'esecuzione del processo.

## <a name="summary"></a>Riepilogo

Come è possibile notare, Azure PowerShell fornisce un modo semplice per eseguire query Hive in un cluster HDInsight, monitorare lo stato del processo e recuperare l'output.

## <a name="next-steps"></a>Passaggi successivi

Per informazioni generali su Hive in HDInsight:

* [Usare Apache Hive con Apache Hadoop su HDInsight](hdinsight-use-hive.md)

Per informazioni su altre modalità d'uso di Hadoop in HDInsight:

* [Usare Apache Pig con Apache Hadoop su HDInsight](hdinsight-use-pig.md)
* [Usare MapReduce con Apache Hadoop su HDInsight](hdinsight-use-mapreduce.md)
