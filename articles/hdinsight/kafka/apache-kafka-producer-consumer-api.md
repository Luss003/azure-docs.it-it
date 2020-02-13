---
title: 'Esercitazione: API Apache Kafka Producer e Consumer - Azure HDInsight'
description: Informazioni su come usare l'API Apache Kafka Producer e Consumer con Kafka in HDInsight. In questa esercitazione si apprenderà come usare queste API con Kafka in HDInsight da un'applicazione Java.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 10/08/2019
ms.openlocfilehash: 65fc3259b0bc5fce61ccd1ceb8df30f1bba49b19
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2020
ms.locfileid: "77161716"
---
# <a name="tutorial-use-the-apache-kafka-producer-and-consumer-apis"></a>Esercitazione: Usare le API Apache Kafka Producer e Consumer

Informazioni su come usare l'API Apache Kafka Producer e Consumer con Kafka in HDInsight.

L'API Kafka Producer consente alle applicazioni di inviare flussi di dati al cluster Kafka. L'API Kafka Consumer consente alle applicazioni di leggere flussi di dati dal cluster Kafka.

In questa esercitazione verranno illustrate le procedure per:

> [!div class="checklist"]
> * Prerequisites
> * Informazioni sul codice
> * Compilare e distribuire l'applicazione
> * Eseguire l'applicazione nel cluster

Per altre informazioni sulle API, vedere la documentazione Apache sull'[API Producer](https://kafka.apache.org/documentation/#producerapi) e l'[API Consumer](https://kafka.apache.org/documentation/#consumerapi).

## <a name="prerequisites"></a>Prerequisites

* Apache Kafka in HDInsight 3.6. Per informazioni su come creare un cluster Kafka in HDInsight, vedere [Iniziare a usare Apache Kafka in HDInsight](apache-kafka-get-started.md).

* [Java Developer Kit (JDK) versione 8](https://aka.ms/azure-jdks) o equivalente, ad esempio OpenJDK.

* [Apache Maven](https://maven.apache.org/download.cgi) correttamente [installato](https://maven.apache.org/install.html) in base alle indicazioni di Apache.  Maven è un sistema di compilazione per progetti Java.

* Un client SSH. Per altre informazioni, vedere [Connettersi a HDInsight (Apache Hadoop) con SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="understand-the-code"></a>Informazioni sul codice

L'applicazione di esempio si trova in [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) nella sottodirectory `Producer-Consumer`. L'applicazione è costituita principalmente da quattro file:

* `pom.xml`: Questo file definisce le dipendenze progetto, la versione Java e i metodi di creazione pacchetti.
* `Producer.java`: Questo file invia frasi casuali a Kafka usando l'API Producer.
* `Consumer.java`: Questo file usa l'API Consumer per leggere i dati da Kafka ed esportarli in STDOUT.
* `Run.java`: L'interfaccia della riga di comando usata per eseguire il codice producer e consumer.

### <a name="pomxml"></a>Pom.xml

Gli aspetti importanti da comprendere nel file `pom.xml` sono:

* Dipendenze: Questo progetto si basa sulle API Producer e Consumer Kafka, che sono fornite dal pacchetto `kafka-clients`. Il codice XML seguente definisce questa dipendenza:

    ```xml
    <!-- Kafka client for producer/consumer operations -->
    <dependency>
            <groupId>org.apache.kafka</groupId>
            <artifactId>kafka-clients</artifactId>
            <version>${kafka.version}</version>
    </dependency>
    ```

    La voce `${kafka.version}` viene dichiarata nella sezione `<properties>..</properties>` di `pom.xml` ed è configurata per la versione Kafka del cluster HDInsight.

* Plug-in: I plug-in Maven offrono varie funzionalità. In questo progetto vengono usati i plug-in seguenti:

    * `maven-compiler-plugin`: Usato per impostare su 8 la versione Java usata dal progetto. Si tratta della versione di Java usata da HDInsight 3.6.
    * `maven-shade-plugin`: Usato per generare un file uberjar che contiene questa applicazione, nonché eventuali dipendenze. Viene inoltre usato per impostare il punto di ingresso dell'applicazione, in modo che sia possibile eseguire il file con estensione jar direttamente senza dover specificare la classe principale.

### <a name="producerjava"></a>Producer.java

Il producer comunica con gli host broker di Kafka (nodi di lavoro) e invia i dati a un argomento Kafka. Il frammento di codice seguente è tratto dal file [Producer.java](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/blob/master/Producer-Consumer/src/main/java/com/microsoft/example/Producer.java) del [repository GitHub](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) e mostra come impostare le proprietà del producer:

```java
Properties properties = new Properties();
// Set the brokers (bootstrap servers)
properties.setProperty("bootstrap.servers", brokers);
// Set how to serialize key/value pairs
properties.setProperty("key.serializer","org.apache.kafka.common.serialization.StringSerializer");
properties.setProperty("value.serializer","org.apache.kafka.common.serialization.StringSerializer");
KafkaProducer<String, String> producer = new KafkaProducer<>(properties);
```

### <a name="consumerjava"></a>Consumer.java

Il consumer comunica con gli host broker Kafka (nodi di lavoro) e legge i record in un ciclo. Il frammento di codice seguente tratto dal file [Consumer.java](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/blob/master/Producer-Consumer/src/main/java/com/microsoft/example/Consumer.java) imposta le proprietà del consumer:

```java
KafkaConsumer<String, String> consumer;
// Configure the consumer
Properties properties = new Properties();
// Point it to the brokers
properties.setProperty("bootstrap.servers", brokers);
// Set the consumer group (all consumers must belong to a group).
properties.setProperty("group.id", groupId);
// Set how to serialize key/value pairs
properties.setProperty("key.deserializer","org.apache.kafka.common.serialization.StringDeserializer");
properties.setProperty("value.deserializer","org.apache.kafka.common.serialization.StringDeserializer");
// When a group is first created, it has no offset stored to start reading from. This tells it to start
// with the earliest record in the stream.
properties.setProperty("auto.offset.reset","earliest");

consumer = new KafkaConsumer<>(properties);
```

In questo codice, il consumer è configurato per la lettura dall'inizio dell'argomento (`auto.offset.reset` è impostato su `earliest`.)

### <a name="runjava"></a>Run.java

Il file [Run.java](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/blob/master/Producer-Consumer/src/main/java/com/microsoft/example/Run.java) fornisce un'interfaccia della riga di comando che esegue il codice del producer o del consumer. È necessario fornire le informazioni sull'host broker di Kafka come parametro. È facoltativamente possibile includere un valore di ID gruppo che viene usato dal processo consumer. Se si creano più istanze di consumer usando lo stesso ID gruppo, esse bilanceranno il carico durante la lettura dall'argomento.

## <a name="build-and-deploy-the-example"></a>Creare e distribuire l'esempio

1. Scaricare ed estrarre gli esempi da [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started).

2. Impostare la directory corrente sul percorso della directory `hdinsight-kafka-java-get-started\Producer-Consumer` e usare il comando seguente:

    ```cmd
    mvn clean package
    ```

    Questo comando crea una directory denominata `target`, che contiene un file denominato `kafka-producer-consumer-1.0-SNAPSHOT.jar`.

3. Sostituire `sshuser` con il nome utente SSH del cluster e sostituire `CLUSTERNAME` con il nome del cluster. Immettere il comando seguente per copiare il file `kafka-producer-consumer-1.0-SNAPSHOT.jar` nel cluster HDInsight. Quando richiesto, immettere la password per l'utente SSH.

    ```cmd
    scp ./target/kafka-producer-consumer-1.0-SNAPSHOT.jar sshuser@CLUSTERNAME-ssh.azurehdinsight.net:kafka-producer-consumer.jar
    ```

## <a id="run"></a> Eseguire l'esempio

1. Sostituire `sshuser` con il nome utente SSH del cluster e sostituire `CLUSTERNAME` con il nome del cluster. Per aprire una connessione SSH al cluster, immettere il comando seguente. Quando richiesto, immettere la password per l'account utente SSH.

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Installare [jq](https://stedolan.github.io/jq/), un processore JSON da riga di comando. Dalla connessione SSH aperta, immettere il comando seguente per installare `jq`:

    ```bash
    sudo apt -y install jq
    ```

1. Configurare la variabile di password. Sostituire `PASSWORD` con la password di accesso al cluster e quindi immettere il comando:

    ```bash
    export password='PASSWORD'
    ```

1. Estrarre il nome del cluster con l'uso corretto di maiuscole e minuscole. L'uso effettivo di maiuscole e minuscole nel nome del cluster può differire dal previsto, a seconda della modalità di creazione del cluster. Questo comando otterrà la combinazione di maiuscole e minuscole effettiva e quindi la archivierà in una variabile. Immettere il comando seguente:

    ```bash
    export clusterName=$(curl -u admin:$password -sS -G "http://headnodehost:8080/api/v1/clusters" | jq -r '.items[].Clusters.cluster_name')
    ```
    > [!Note]  
    > Se si esegue questo processo dall'esterno del cluster, è disponibile una procedura diversa per l'archiviazione del nome del cluster. Recuperare il nome del cluster in lettere minuscole dal portale di Azure. Sostituire quindi `<clustername>` con il nome del cluster nel comando seguente ed eseguire il comando: `export clusterName='<clustername>'`.  

1. Per ottenere gli host del broker Kafka, usare il comando seguente:

    ```bash
    export KAFKABROKERS=$(curl -sS -u admin:$password -G https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2);
    ```

    > [!Note]  
    > Questo comando richiede l'accesso ad Ambari. Se il cluster è protetto da un gruppo NSG, eseguire questo comando da un computer in grado di accedere ad Ambari.

1. Per creare l'argomento Kafka `myTest`, immettere il comando seguente:

    ```bash
    java -jar kafka-producer-consumer.jar create myTest $KAFKABROKERS
    ```

1. Per eseguire il producer e scrivere i dati nell'argomento, usare il comando seguente:

    ```bash
    java -jar kafka-producer-consumer.jar producer myTest $KAFKABROKERS
    ```

1. Quando l'esecuzione del producer è terminata, usare il comando seguente per leggere dall'argomento:

    ```bash
    java -jar kafka-producer-consumer.jar consumer myTest $KAFKABROKERS
    ```

    Verranno visualizzati i record letti e il numero di record.

1. Usare __Ctrl + C__ per uscire dal consumer.

### <a name="multiple-consumers"></a>Consumer multipli

I consumer Kafka usano un gruppo di consumer durante la lettura dei record. L'uso dello stesso gruppo con più consumer consente di ottenere operazioni di lettura con bilanciamento del carico da un argomento. Ogni consumer nel gruppo riceve una parte dei record.

L'applicazione Consumer accetta un parametro che viene usato come ID del gruppo. Ad esempio, il comando seguente avvia un consumer usando un ID gruppo denominato `myGroup`:

```bash
java -jar kafka-producer-consumer.jar consumer myTest $KAFKABROKERS myGroup
```

Usare __Ctrl + C__ per uscire dal consumer.

Per verificare il funzionamento del processo, usare il comando seguente:

```bash
tmux new-session 'java -jar kafka-producer-consumer.jar consumer myTest $KAFKABROKERS myGroup' \
\; split-window -h 'java -jar kafka-producer-consumer.jar consumer myTest $KAFKABROKERS myGroup' \
\; attach
```

Questo comando usa `tmux` per suddividere il terminale in due colonne. Viene avviato un consumer in ogni colonna, con lo stesso valore di ID di gruppo. Dopo che i consumer hanno completato la lettura, si noti che ognuno di essi ha letto solo una parte dei record. Usare __Ctrl + C__ due volte per uscire da `tmux`.

Il consumo da parte dei client dello stesso gruppo viene gestito tramite le partizioni dell'argomento. In questo esempio di codice per l'argomento `test` creato in precedenza sono presenti otto partizioni. Se si avviano otto consumer, ognuno di essi legge i record da una singola partizione dell'argomento.

> [!IMPORTANT]  
> Un gruppo di consumer non può contenere un numero di istanze di consumer maggiore del numero di partizioni. In questo esempio, un gruppo di consumer può contenere fino a otto consumer perché è questo il numero di partizioni nell'argomento. In alternativa è possibile avere più gruppi di consumer, ognuno con al massimo otto consumer.

I record vengono archiviati in Kafka nell'ordine in cui vengono ricevuti in una partizione. Per ottenere il recapito dei record nell'ordine *all'interno di una partizione*, creare un gruppo di consumer con un numero di istanze corrispondente al numero di partizioni. Per ottenere il recapito dei record nell'ordine *all'interno dell'argomento*, creare un gruppo di consumer con una sola istanza.

## <a name="clean-up-resources"></a>Pulire le risorse

Per pulire le risorse create da questa esercitazione, eliminare il gruppo di risorse. Se si elimina il gruppo di risorse, vengono eliminati anche il cluster HDInsight associato e tutte le altre risorse correlate al gruppo di risorse.

Per rimuovere il gruppo di risorse usando il portale di Azure:

1. Nel portale di Azure espandere il menu a sinistra per aprire il menu dei servizi e quindi scegliere __Gruppi di risorse__ per visualizzare l'elenco dei gruppi di risorse.
2. Individuare il gruppo di risorse da eliminare e quindi fare clic con il pulsante destro del mouse su __Altro__ (...) a destra dell'elenco.
3. Scegliere __Elimina gruppo di risorse__ e quindi confermare.

## <a name="next-steps"></a>Passaggi successivi

In questo documento si è appreso come usare le API Apache Kafka Producer e Consumer con Kafka in HDInsight. Per altre informazioni sull'uso di Kafka, vedere:

> [!div class="nextstepaction"]
> [Analizzare i log di Apache Kafka](apache-kafka-log-analytics-operations-management.md)
