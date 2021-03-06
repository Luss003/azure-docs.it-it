---
title: Notebook Zeppelin & cluster Apache Spark-Azure HDInsight
description: Istruzioni dettagliate su come usare i notebook di Zeppelin con cluster Apache Spark in Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 02/18/2020
ms.openlocfilehash: e313048986beca1991e38ce2e65ea12f954170d2
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/25/2020
ms.locfileid: "77598273"
---
# <a name="use-apache-zeppelin-notebooks-with-apache-spark-cluster-on-azure-hdinsight"></a>Usare i notebook di Apache Zeppelin con cluster Apache Spark in Azure HDInsight

I cluster HDInsight Spark includono notebook [Apache Zeppelin](https://zeppelin.apache.org/) che possono essere usati per eseguire processi [Apache Spark](https://spark.apache.org/). Questo articolo illustra come usare il notebook Zeppelin in un cluster HDInsight.

## <a name="prerequisites"></a>Prerequisites

* Un cluster Apache Spark in HDInsight. Per istruzioni, vedere l'articolo dedicato alla [creazione di cluster Apache Spark in Azure HDInsight](apache-spark-jupyter-spark-sql.md).
* Schema URI per l'archiviazione primaria del cluster. Questo è `wasb://` per l'archiviazione BLOB di Azure, `abfs://` per Azure Data Lake Storage Gen2 o `adl://` per Azure Data Lake Storage Gen1. Se il trasferimento sicuro è abilitato per l'archiviazione BLOB, l'URI viene `wasbs://`.  Per altre informazioni, vedere [richiedere il trasferimento sicuro in archiviazione di Azure](../../storage/common/storage-require-secure-transfer.md) .

## <a name="launch-an-apache-zeppelin-notebook"></a>Avviare un notebook di Apache Zeppelin

1. Nella **Panoramica**del cluster Spark selezionare **Zeppelin notebook** da **Dashboard cluster**. Immettere le credenziali di amministratore per il cluster.  

   > [!NOTE]  
   > È anche possibile raggiungere il notebook di Zeppelin per il cluster aprendo l'URL seguente nel browser. Sostituire **CLUSTERNAME** con il nome del cluster:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`

2. Creare un nuovo notebook. Dal riquadro intestazione passare a **Notebook** > **Crea nuova nota**.

    ![Creare un nuovo notebook Zeppelin](./media/apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "Creare un nuovo notebook Zeppelin")

    Immettere un nome per il notebook, quindi selezionare **Crea nota**.

3. Verificare che l'intestazione del notebook mostri uno stato connesso. È indicato da un punto verde nell'angolo superiore destro.

    ![Stato notebook Zeppelin](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "Stato di notebook Zeppelin")

4. Caricare i dati di esempio in una tabella temporanea. Quando si crea un cluster Spark in HDInsight, il file di dati di esempio, `hvac.csv`, viene copiato nell'account di archiviazione associato in `\HdiSamples\SensorSampleData\hvac`.

    Nel paragrafo vuoto creato per impostazione predefinita del nuovo notebook, incollare il frammento di codice riportato di seguito.

    ```scala
    %livy2.spark
    //The above magic instructs Zeppelin to use the Livy Scala interpreter

    // Create an RDD using the default Spark context, sc
    val hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

    // Define a schema
    case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)

    // Map the values in the .csv file to the schema
    val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
        s => Hvac(s(0),
                s(1),
                s(2).toInt,
                s(3).toInt,
                s(6)
        )
    ).toDF()

    // Register as a temporary table called "hvac"
    hvac.registerTempTable("hvac")
    ```

    Premere **MAIUSC + INVIO** oppure selezionare il pulsante **Riproduci** per il paragrafo per eseguire il frammento di codice. Lo stato nell'angolo destro del paragrafo deve passare da PRONTO, IN ATTESA, IN ESECUZIONE, a COMPLETATO. L'output viene visualizzato nella parte inferiore dello stesso paragrafo. Nella schermata è simile al seguente:

    ![Creare una tabella temporanea da dati non elaborati](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "Crea una tabella temporanea dai dati non elaborati")

    È inoltre possibile fornire un titolo a ogni paragrafo. Nell'angolo destro del paragrafo selezionare l'icona **delle impostazioni** (pignone) e quindi selezionare **Mostra titolo**.  

    > [!NOTE]  
    > L'interprete %spark2 non è supportato nei notebook Zeppelin in tutte le versioni di HDInsight e l'interprete %sh non sarà supportato a partire da HDInsight 4.0.

5. È ora possibile eseguire istruzioni Spark SQL nella tabella `hvac`. Incollare la query seguente in un nuovo paragrafo. La query recupera l'ID dell'edificio e la differenza tra le temperature effettive e quelle di destinazione per ogni edificio in una determinata data. Premere **MAIUSC + INVIO**.

    ```sql
    %sql
    select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13"
    ```  

    L'istruzione **%sql** all'inizio indica al notebook di usare l'interprete Livy Scala.

6. Selezionare l'icona del **grafico a barre** per modificare la visualizzazione.  **Impostazioni**, che vengono visualizzate dopo aver selezionato il **grafico a barre**, consentono di scegliere le **chiavi**e **i valori**.  Nella schermata riportata di seguito sono illustrate questo output.

    ![Eseguire un'istruzione SQL Spark usando notebook1](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "Eseguire un'istruzione SQL Spark usando notebook1")

7. È inoltre possibile eseguire istruzioni SQL Spark tramite le variabili nella query. Il frammento di codice successivo Mostra come definire una variabile, `Temp`, nella query con i valori possibili con cui si vuole eseguire una query. Quando si esegue la query per la prima volta, un elenco a tendina viene popolato automaticamente con i valori specificati per la variabile.

    ```sql
    %sql  
    select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}"
    ```

    Incollare questo frammento di codice in un nuovo paragrafo e premere **MAIUSC+INVIO**. Quindi selezionare **65** dall'elenco a discesa **Temp** .

8. Selezionare l'icona del **grafico a barre** per modificare la visualizzazione.  Quindi selezionare **Impostazioni** e apportare le modifiche seguenti:

   * **Gruppi:**  Aggiungere **targettemp**.  
   * **Valori:** 1. Rimuovere la **Data**.  2. Aggiungere **temp_diff**.  3.  Modificare l'aggregatore da **Sum** a **AVG**.  

     Nella schermata riportata di seguito sono illustrate questo output.

     ![Eseguire un'istruzione SQL Spark usando notebook2](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "Eseguire un'istruzione SQL Spark usando notebook2")

## <a name="how-do-i-use-external-packages-with-the-notebook"></a>Come usare pacchetti esterni con il notebook

È possibile configurare il notebook di Zeppelin nel cluster Apache Spark in HDInsight per l'uso di pacchetti esterni creati dalla community che non sono inclusi nel cluster. Per un elenco completo dei pacchetti disponibili, è possibile eseguire ricerche nel [repository Maven](https://search.maven.org/) . È anche possibile ottenere un elenco dei pacchetti disponibili da altre origini. Ad esempio, un elenco completo dei pacchetti creati dalla community è disponibile nel sito Web [spark-packages.org](https://spark-packages.org/).

In questo articolo si vedrà come usare il pacchetto [Spark-CSV](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) con Jupyter notebook.

1. Aprire le impostazioni dell'interprete. Nell'angolo in alto a destra selezionare il nome utente connesso, quindi selezionare **interprete**.

    ![Avvia interprete](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Output di Hive")

2. Scorrere fino a **livy2**e quindi selezionare **modifica**.

    ![Modifica Settings1 dell'interprete](./media/apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "Modifica Settings1 dell'interprete")

3. Passare a `livy.spark.jars.packages`chiave e impostarne il valore nel formato `group:id:version`. Se si vuole usare il pacchetto [spark-csv](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar), è quindi necessario impostare il valore della chiave su `com.databricks:spark-csv_2.10:1.4.0`.

    ![Modifica settings2 dell'interprete](./media/apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "Modifica settings2 dell'interprete")

    Selezionare **Save (Salva** ) e quindi **OK** per riavviare l'interprete Livio.

4. il valore della chiave immesso sopra si determina come illustrato di seguito.

    a. Individuare un pacchetto nel repository Maven. Per questo articolo è stato usato [Spark-CSV](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).

    b. Recuperare dal repository i valori per **GroupId**, **ArtifactId** e **Version**.

    ![Usare pacchetti esterni con Jupyter notebook](./media/apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Usare pacchetti esterni con Jupyter Notebook")

    c. Concatenare i tre valori, separati da due punti ( **:** ).

        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a>Posizione di salvataggio dei notebook Zeppelin

I notebook Zeppelin vengono salvati nei nodi head del cluster. Se si elimina il cluster, verranno quindi eliminati anche i notebook. Se si vogliono mantenere i notebook per usarli successivamente in altri cluster, è necessario esportarli al termine dell'esecuzione dei processi. Per esportare un notebook, selezionare l'icona **Esporta** , come illustrato nell'immagine seguente.

![Scarica Notebook](./media/apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "Scaricare il notebook")

Il notebook verrà così salvato come file JSON nel percorso di download dell'utente.

## <a name="use-shiro-to-configure-access-to-zeppelin-interpreters-in-enterprise-security-package-esp-clusters"></a>Usare Shiro per configurare l'accesso agli interpreti Zeppelin nei cluster Enterprise Security Package (ESP)
Come indicato in precedenza, l'interprete `%sh` non è supportato da HDInsight 4,0 e versioni successive. Inoltre, poiché `%sh` interprete introduce i potenziali problemi di sicurezza, ad esempio l'accesso alle schede di tastiera usando i comandi della shell, è stato rimosso anche dai cluster ESP HDInsight 3,6. Ciò significa che `%sh` interprete non è disponibile quando si fa clic su **Crea nuova nota** o nell'interfaccia utente dell'interprete per impostazione predefinita. 

Gli utenti con privilegi di dominio possono utilizzare il file di `Shiro.ini` per controllare l'accesso all'interfaccia utente dell'interprete. In questo modo, solo questi utenti possono creare nuovi interpreti `%sh` e impostare le autorizzazioni per ogni nuovo interprete di `%sh`. Per controllare l'accesso tramite il file di `shiro.ini`, attenersi alla procedura seguente:

1. Definire un nuovo ruolo utilizzando un nome di gruppo di dominio esistente. Nell'esempio seguente `adminGroupName` è un gruppo di utenti con privilegi in AAD. Non usare caratteri speciali o spazi vuoti nel nome del gruppo. I caratteri dopo `=` forniscono le autorizzazioni per questo ruolo. `*` indica che il gruppo dispone di autorizzazioni complete.

    ```
    [roles]
    adminGroupName = *
    ```

2. Aggiungere il nuovo ruolo per l'accesso agli interpreti Zeppelin. Nell'esempio seguente, a tutti gli utenti in `adminGroupName` viene concesso l'accesso agli interpreti Zeppelin e sono in grado di creare nuovi interpreti. È possibile inserire più ruoli tra le parentesi quadre in `roles[]`, separate da virgole. Quindi, gli utenti che dispongono delle autorizzazioni necessarie possono accedere agli interpreti Zeppelin.

    ```
    [urls]
    /api/interpreter/** = authc, roles[adminGroupName]
    ```

## <a name="livy-session-management"></a>Gestione delle sessioni di Livy

Quando si esegue il primo paragrafo di codice nel notebook Zeppelin, nel cluster HDInsight Spark viene creata una nuova sessione di Livy. Tale sessione sarà condivisa da tutti i notebook Zeppelin creati successivamente. Se per qualche motivo la sessione di Livio viene terminata (riavvio del cluster e così via), non sarà possibile eseguire i processi dal notebook Zeppelin.

In tal caso, per poter eseguire processi dal notebook Zeppelin è prima necessario seguire questa procedura.  

1. Riavviare l'interprete Livy dal notebook Zeppelin. A tale scopo, aprire le impostazioni dell'interprete selezionando il nome dell'utente connesso nell'angolo in alto a destra, quindi selezionare **interprete**.

    ![Avvia interprete](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Output di Hive")

2. Scorrere fino a **livy2**e quindi selezionare **Riavvia**.

    ![Riavviare l'interprete Livio](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "Riavviare l'interprete Zeppelin")

3. Eseguire una cella di codice da un notebook Zeppelin esistente. Verrà così creata una nuova sessione di Livy nel cluster HDInsight.

## <a name="general-information"></a>Informazioni generali

### <a name="validate-service"></a>Convalida servizio

Per convalidare il servizio da Ambari, passare a `https://CLUSTERNAME.azurehdinsight.net/#/main/services/ZEPPELIN/summary` dove CLUSTERname è il nome del cluster.

Per convalidare il servizio dalla riga di comando, connettersi tramite SSH al nodo head. Passare dall'utente allo Zeppelin usando il comando `sudo su zeppelin`. Comandi di stato:

|Comando |Descrizione |
|---|---|
|`/usr/hdp/current/zeppelin-server/bin/zeppelin-daemon.sh status`|Stato del servizio.|
|`/usr/hdp/current/zeppelin-server/bin/zeppelin-daemon.sh --version`|Versione del servizio.|
|`ps -aux | grep zeppelin`|Identificare PID.|

### <a name="log-locations"></a>Percorsi di log

|Service |Path |
|---|---|
|Zeppelin-server|/usr/hdp/current/zeppelin-server/|
|Log del server|/var/log/zeppelin|
|Interprete della configurazione, Shiro, site. XML, log4j|/usr/HDP/Current/Zeppelin-server/conf o/etc/Zeppelin/conf|
|Directory PID|/var/run/zeppelin|

### <a name="enable-debug-logging"></a>Abilitazione della registrazione di debug

1. Passare a `https://CLUSTERNAME.azurehdinsight.net/#/main/services/ZEPPELIN/summary` dove CLUSTERname è il nome del cluster.

1. Passare a **configurazioni** > **Advanced Zeppelin-log4j-Properties** > **log4j_properties_content**.

1. Modificare `log4j.appender.dailyfile.Threshold = INFO` in `log4j.appender.dailyfile.Threshold = DEBUG`.

1. Aggiungere `log4j.logger.org.apache.zeppelin.realm=DEBUG`.

1. Salvare le modifiche e riavviare il servizio.

## <a name="next-steps"></a>Passaggi successivi

[Panoramica: Apache Spark su Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scenari

* [Apache Spark con Business Intelligence: eseguire l'analisi interattiva dei dati con strumenti di Business Intelligence mediante Spark in HDInsight](apache-spark-use-bi-tools.md)
* [Apache Spark con Machine Learning: utilizzare Spark in HDInsight per l'analisi della temperatura di compilazione utilizzando dati HVAC](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark con Machine Learning: utilizzare Spark in HDInsight per prevedere i risultati di un controllo alimentare](apache-spark-machine-learning-mllib-ipython.md)
* [Analisi dei log del sito Web con Apache Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Creare ed eseguire applicazioni

* [Creare un'applicazione autonoma con Scala](apache-spark-create-standalone-application.md)
* [Eseguire processi in modalità remota in un cluster Apache Spark usando Apache Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Strumenti ed estensioni

* [Usare il plug-in degli strumenti HDInsight per IntelliJ IDEA per creare e inviare applicazioni Apache Spark in Scala](apache-spark-intellij-tool-plugin.md)
* [Usare il plug-in Strumenti HDInsight per IntelliJ IDEA per eseguire il debug di applicazioni Apache Spark in remoto](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Kernel disponibili per notebook di Jupyter nel cluster Apache Spark per HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Usare pacchetti esterni con i notebook Jupyter](apache-spark-jupyter-notebook-use-external-packages.md)
* [Installare Jupyter Notebook nel computer e connetterlo a un cluster HDInsight Spark](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Gestione di risorse

* [Gestire le risorse del cluster Apache Spark in Azure HDInsight](apache-spark-resource-manager.md)
* [Tenere traccia ed eseguire il debug di processi in esecuzione nel cluster Apache Spark in Azure HDInsight](apache-spark-job-debugging.md)
