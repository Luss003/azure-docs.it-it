---
title: Apache Spark & Apache Kafka con Cosmos DB-Azure HDInsight
description: Informazioni su come usare lo streaming strutturato Apache Spark per leggere i dati da Apache Kafka e quindi archiviarli in Azure Cosmos DB. In questo esempio i dati vengono trasmessi in streaming tramite un notebook Jupyter da Spark in HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: hrasheed
ms.openlocfilehash: faae65c6664123bd673711674a36edc928c74278
ms.sourcegitcommit: 38251963cf3b8c9373929e071b50fd9049942b37
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2019
ms.locfileid: "73044900"
---
# <a name="use-apache-spark-structured-streaming-with-apache-kafka-and-azure-cosmos-db"></a>Usare Apache Spark Structured Streaming con Apache Kafka e Azure Cosmos DB

Informazioni su come usare [Apache Spark](https://spark.apache.org/) [Structured Streaming](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html) per leggere i dati da [Apache Kafka](https://kafka.apache.org/) in Azure HDInsight e quindi archiviarli in Azure Cosmos DB.

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) è un database multimodello distribuito a livello globale. In questo esempio viene usato un modello di database dell'API SQL. Per altre informazioni, vedere il documento [Introduzione ad Azure Cosmos DB](../cosmos-db/introduction.md).

Lo streaming strutturato Spark è un motore di elaborazione del flusso basato su Spark SQL. Consente di esprimere i calcoli di streaming come il calcolo di batch in dati statici. Per altre informazioni sullo streaming strutturato, vedere la pagina [Structured Streaming Programming Guide](https://spark.apache.org/docs/2.2.0/structured-streaming-programming-guide.html) di Apache.org.

> [!IMPORTANT]  
> In questo esempio viene usato Spark 2.2 in HDInsight 3.6.
>
> La procedura descritta in questo documento permette di creare un gruppo di risorse di Azure che contiene sia un cluster Spark in HDInsight che un cluster Kafka in HDInsight. Entrambi questi cluster si trovano all'interno di una rete virtuale di Azure, che consente al cluster Spark di comunicare direttamente con il cluster Kafka.
>
> Al termine della procedura descritta in questo documento, eliminare i cluster per evitare costi supplementari.

## <a name="create-the-clusters"></a>Creare i cluster

Apache Kafka in HDInsight non fornisce l'accesso ai broker Kafka tramite Internet pubblico. Tutto ciò che comunica con Kafka deve trovarsi nella stessa rete virtuale di Azure dei nodi del cluster Kafka. Per questo esempio, i cluster Spark e Kafka si trovano entrambi in una rete virtuale di Azure. Il diagramma seguente illustra il flusso delle comunicazioni tra i cluster:

![Diagramma dei cluster Spark e Kafka in una rete virtuale di Azure](./media/apache-kafka-spark-structured-streaming-cosmosdb/apache-spark-kafka-vnet.png)

> [!NOTE]  
> Il servizio Kafka è limitato alle comunicazioni all'interno della rete virtuale. Altri servizi nel cluster, ad esempio SSH e Ambari, sono accessibili tramite Internet. Per altre informazioni sulle porte pubbliche disponibili con HDInsight, vedere [Porte e URI usati da HDInsight](hdinsight-hadoop-port-settings-for-services.md).

Anche se è possibile creare manualmente cluster Spark e Kafka e una rete virtuale di Azure, è più semplice usare un modello di Azure Resource Manager. Seguire questa procedura per distribuire cluster Spark e Kafka e una rete virtuale di Azure nella sottoscrizione di Azure.

1. Usare il pulsante seguente per accedere ad Azure e aprire il modello nel portale di Azure.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fhdinsight-spark-scala-kafka-cosmosdb%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="./media/apache-kafka-spark-structured-streaming-cosmosdb/resource-manager-deploy.png" alt="Deploy to Azure"/>
    </a>

    Il modello di Azure Resource Manager si trova nel repository GitHub per questo progetto ([https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb)).

    Questo modello crea le risorse seguenti:

   * Kafka nel cluster HDInsight 3.6.

   * Spark nel cluster HDInsight 3.6.

   * Una rete virtuale Azure contenente i cluster HDInsight.

       > [!NOTE]  
       > La rete virtuale creata dal modello usa lo spazio degli indirizzi 10.0.0.0/16.

   * Un database dell'API SQL di Azure Cosmos DB.

     > [!IMPORTANT]  
     > Il notebook di streaming strutturato che è stato usato in questo esempio richiede Spark in HDInsight 3.6. Se si usa una versione precedente di Spark in HDInsight, si ricevono errori durante l'uso del notebook.

2. Usare le informazioni seguenti per popolare le voci nel pannello **Distribuzione personalizzata**:

    ![HDInsight valori di distribuzione personalizzati](./media/apache-kafka-spark-structured-streaming-cosmosdb/hdi-custom-parameters.png)

    * **Sottoscrizione**: selezionare una sottoscrizione di Azure.

    * **Gruppo di risorse**: creare un gruppo o selezionarne uno esistente. Questo gruppo contiene il cluster HDInsight.

    * **Località**: scegliere una località geograficamente vicina.

    * **Nome dell'account Cosmos DB**: questo valore viene usato come nome per l'account Cosmos DB.

    * **Base Cluster Name** (Nome di base del cluster): questo valore viene usato come nome di base per i cluster Spark e Kafka. Ad esempio, se si immette **myhdi** viene creato un cluster Spark denominato __spark-myhdi__ e un cluster Kafka denominato **kafka-myhdi**.

    * **Versione del cluster**: versione del cluster HDInsight.

        > [!IMPORTANT]  
        > In questo esempio è stato eseguito un test con HDInsight 3.6 e potrebbe non funzionare con altri tipi di cluster.

    * **Cluster Login User Name** (Nome utente di accesso del cluster): nome utente amministratore per i cluster Spark e Kafka.

    * **Cluster Login Password** (Password di accesso del cluster): password amministratore per i cluster Spark e Kafka.

    * **SSH User Name** (Nome utente SSH): utente SSH da creare per i cluster Spark e Kafka.

    * **SSH Password** (Password SSH): password dell'utente SSH per i cluster Spark e Kafka.

3. Leggere le **Condizioni** e quindi selezionare **Accetto le condizioni riportate sopra**.

4. Infine selezionare **Acquisto**. La creazione dei cluster richiede circa 20 minuti.

> [!IMPORTANT]  
> Potrebbero occorrere fino a 45 minuti per creare i cluster, la rete virtuale e l'account Cosmos DB.

## <a name="create-the-cosmos-db-database-and-collection"></a>Creare il database e la raccolta di Cosmos DB

Il progetto usato in questo documento archivia i dati in Cosmos DB. Prima di eseguire il codice è necessario innanzitutto creare un _database_ e una _raccolta_ nell'istanza di Cosmos DB. È necessario recuperare anche l'endpoint documento e la _chiave_ usati per autenticare le richieste a Cosmos DB. 

Un modo per eseguire questa operazione consiste nell'usare l'[interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest). Lo script seguente crea un database denominato `kafkadata` e una raccolta denominata `kafkacollection`, per poi restituire la chiave primaria.

```azurecli
#!/bin/bash

# Replace 'myresourcegroup' with the name of your resource group
resourceGroupName='myresourcegroup'
# Replace 'mycosmosaccount' with the name of your Cosmos DB account name
name='mycosmosaccount'

# WARNING: If you change the databaseName or collectionName
#          then you must update the values in the Jupyter notebook
databaseName='kafkadata'
collectionName='kafkacollection'

# Create the database
az cosmosdb database create --name $name --db-name $databaseName --resource-group $resourceGroupName
# Create the collection
az cosmosdb collection create --collection-name $collectionName --name $name --db-name $databaseName --resource-group $resourceGroupName

# Get the endpoint
az cosmosdb show --name $name --resource-group $resourceGroupName --query documentEndpoint

# Get the primary key
az cosmosdb list-keys --name $name --resource-group $resourceGroupName --query primaryMasterKey
```

I dati relativi all'endpoint documento e alla chiave primaria sono simili al testo seguente:

```text
# endpoint
"https://mycosmosaccount.documents.azure.com:443/"
# key
"YqPXw3RP7TsJoBF5imkYR0QNA02IrreNAlkrUMkL8EW94YHs41bktBhIgWq4pqj6HCGYijQKMRkCTsSaKUO2pw=="
```

> [!IMPORTANT]  
> Salvare i valori di endpoint e chiave, che serviranno nel Notebook di Jupyter.

## <a name="get-the-apache-kafka-brokers"></a>Recuperare i broker di Apache Kafka

Il codice in questo esempio consente di connettersi agli host dei broker Kafka nel cluster Kafka. Per trovare l'indirizzo dei due host broker Kafka, usare l'esempio di PowerShell o Bash seguente:

```powershell
$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
$clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER" `
    -Credential $creds `
    -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$brokerHosts = $respObj.host_components.HostRoles.host_name[0..1]
($brokerHosts -join ":9092,") + ":9092"
```

> [!NOTE]  
> Questo Bash di esempio prevede che `$CLUSTERNAME` contenga il nome del cluster Kafka.
>
> Questo esempio usa l'utilità [jq](https://stedolan.github.io/jq/) per analizzare i dati del documento JSON.

```bash
curl -u admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2
```

Quando richiesto, immettere la password dell'account (admin) di accesso al cluster

L'output è simile al testo seguente:

`wn0-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn1-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092`

Salvare queste informazioni, perché vengono usate nelle sezioni seguenti di questo documento.

## <a name="get-the-notebooks"></a>Ottenere i notebook

Il codice di esempio descritto in questo documento è disponibile all'indirizzo [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb).

## <a name="upload-the-notebooks"></a>Caricare i notebook

Usare i passaggi seguenti per caricare i notebook dal progetto a Spark nel cluster HDInsight:

1. Nel Web browser connettersi al notebook Jupyter nel cluster Spark. Nell'URL seguente sostituire `CLUSTERNAME` con il nome del cluster __Spark__:

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    Quando richiesto, immettere l'account di accesso (amministratore) e la password usati durante la creazione del cluster.

2. Usare il pulsante __Carica__ in alto a destra nella pagina per caricare il file __Stream-taxi-data-to-kafka.ipynb__ nel cluster. Selezionare __Apri__ per avviare il caricamento.

3. Trovare la voce __Stream-taxi-data-to-kafka.ipynb__ nell'elenco dei notebook e selezionare il pulsante __Carica__ accanto.

4. Ripetere i passaggi da 1 a 3 per caricare il notebook __Stream-data-from-Kafka-to-Cosmos-DB.ipynb__.

## <a name="load-taxi-data-into-kafka"></a>Caricare i dati relativi ai taxi in Kafka

Dopo aver caricato i file, selezionare la voce __Stream-taxi-data-to-kafka.ipynb__ per aprire il notebook. Seguire i passaggi del notebook per caricare i dati in Kafka.

## <a name="process-taxi-data-using-spark-structured-streaming"></a>Elaborare i dati relativi ai taxi con lo streaming strutturato Spark

Nella home page di [Jupyter Notebook](https://jupyter.org/) selezionare la voce __Stream-data-from-Kafka-to-Cosmos-DB.ipynb__. Seguire i passaggi del notebook per eseguire lo streaming dei dati da Kafka e in Azure Cosmos DB tramite lo streaming strutturato Spark.

## <a name="next-steps"></a>Passaggi successivi

Ora che si è appreso come usare Apache Spark Structured Streaming, vedere i documenti seguenti per trovare altre informazioni su come usare Apache Spark, Apache Kafka e Azure Cosmos DB:

* [Come usare il flusso Apache Spark (DStream) con Apache Kafka](hdinsight-apache-spark-with-kafka.md).
* [Iniziare a usare Jupyter Notebook e Apache Spark in HDInsight](spark/apache-spark-jupyter-spark-sql.md)
* [Introduzione ad Azure Cosmos DB](../cosmos-db/introduction.md)
