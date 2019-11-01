---
title: Microsoft Cognitive Toolkit con Apache Spark-Azure HDInsight
description: Informazioni su come un modello con training per l'apprendimento approfondito Microsoft Cognitive Toolkit possa essere applicato a un set di dati tramite l'API Python Spark in un cluster Azure HDInsight Spark.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/28/2017
ms.author: hrasheed
ms.openlocfilehash: 0f4172c7a5b287c85c0548c7fe9812305a1ee1e6
ms.sourcegitcommit: 3486e2d4eb02d06475f26fbdc321e8f5090a7fac
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/31/2019
ms.locfileid: "73241525"
---
# <a name="use-microsoft-cognitive-toolkit-deep-learning-model-with-azure-hdinsight-spark-cluster"></a>Usare il modello di apprendimento approfondito Microsoft Cognitive Toolkit con un cluster Azure HDInsight Spark

In questo articolo viene illustrata la procedura seguente.

1. Eseguire uno script personalizzato per installare [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/) in un cluster Azure HDInsight Spark.

2. Caricare un oggetto [Jupyter Notebook](https://jupyter.org/) nel cluster [Apache Spark](https://spark.apache.org/) per vedere come applicare ai file un modello con training di apprendimento approfondito di Microsoft Cognitive Toolkit in un account di Archiviazione BLOB di Azure tramite l'[API Python Spark (PySpark)](https://spark.apache.org/docs/0.9.0/python-programming-guide.html)

## <a name="prerequisites"></a>Prerequisiti

* **Una sottoscrizione di Azure**. Prima di iniziare questo articolo, è necessario disporre di una sottoscrizione di Azure. Vedere [Crea subito il tuo account Azure gratuito](https://azure.microsoft.com/free).

* **Cluster Azure HDInsight Spark**. Per eseguire la procedura dell'articolo, creare un cluster Spark 2.0. Per le istruzioni, vedere [Creare un cluster Apache Spark in Azure HDInsight](apache-spark-jupyter-spark-sql.md).

## <a name="how-does-this-solution-flow"></a>Svolgimento della soluzione

Questa soluzione è divisa tra questo articolo e un notebook di Jupyter caricato come parte di questo articolo. In questo articolo verrà completata la procedura seguente:

* Eseguire un'azione script in un cluster HDInsight Spark per installare i pacchetti Microsoft Cognitive Toolkit e Python.
* Caricare il notebook di Jupyter che esegue la soluzione nel cluster HDInsight Spark.

I passaggi rimanenti elencati sotto vengono trattati nel notebook di Jupyter.

- Caricare immagini di esempio in un set di dati resilienti distribuito di Spark o RDD.
   - Caricare i moduli e definire i set di impostazioni.
   - Scaricare il set di dati in locale nel cluster Spark.
   - Convertire il set di dati in RDD.
- Classificare le immagini tramite un modello con training Cognitive Toolkit.
   - Scaricare il modello con training Cognitive Toolkit nel cluster Spark.
   - Definire le funzioni usate dai nodi del ruolo di lavoro.
   - Classificare le immagini nei nodi del ruolo di lavoro.
   - Valutare l'accuratezza del modello.


## <a name="install-microsoft-cognitive-toolkit"></a>Installare Microsoft Cognitive Toolkit

È possibile installare Microsoft Cognitive Toolkit in un cluster Spark tramite l'azione script. L'azione script usa script personalizzati per installare nel cluster i componenti che non sono disponibili per impostazione predefinita. È possibile usare lo script personalizzato dalla portale di Azure, usando HDInsight .NET SDK oppure Azure PowerShell. È possibile usare lo script anche per installare il toolkit sia nell’ambito della creazione del cluster sia quando il cluster è in esecuzione. 

In questo articolo il toolkit verrà installato dal portale, dopo la creazione del cluster. Per altri modi di eseguire lo script personalizzato, vedere [Personalizzare cluster HDInsight tramite azione script](../hdinsight-hadoop-customize-cluster-linux.md).

### <a name="using-the-azure-portal"></a>Uso del portale di Azure

Per istruzioni su come usare il portale di Azure per eseguire l'azione script, vedere [personalizzare cluster HDInsight mediante azione script](../hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation). Assicurarsi di specificare i dati seguenti per installare Microsoft Cognitive Toolkit.

* Specificare un valore per il nome dell'azione script.

* Per **URI script Bash**, immettere `https://raw.githubusercontent.com/Azure-Samples/hdinsight-pyspark-cntk-integration/master/cntk-install.sh`.

* Assicurarsi di eseguire lo script solo nei nodi head e del ruolo di lavoro e deselezionare tutte le altre caselle di controllo.

* Fare clic su **Create**(Crea).

## <a name="upload-the-jupyter-notebook-to-azure-hdinsight-spark-cluster"></a>Caricare il notebook di Jupyter nel cluster Azure HDInsight Spark

Per usare Microsoft Cognitive Toolkit con il cluster Azure HDInsight Spark, è necessario caricare il notebook di Jupyter **CNTK_model_scoring_on_Spark_walkthrough.ipynb** nel cluster Azure HDInsight Spark. Tale notebook è disponibile in GitHub all'indirizzo [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).

1. Clonare il repository GitHub [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration). Per istruzioni su come eseguire la clonazione, vedere [Cloning a repository](https://help.github.com/articles/cloning-a-repository/) (Clonazione di un repository).

2. Dal portale di Azure aprire il pannello del cluster Spark di cui è già stato effettuato il provisioning, fare clic su **Dashboard cluster**e quindi su **notebook di Jupyter**.

    È anche possibile avviare il notebook di Jupyter accedendo all'URL `https://<clustername>.azurehdinsight.net/jupyter/`. Sostituire \<clustername> con il nome del cluster HDInsight.

3. Dal notebook di Jupyter, fare clic su **Carica** nell'angolo in alto a destra e passare al percorso in cui è stato clonato il repository di GitHub.

    ![Caricare il notebook di Jupyter nel cluster Azure HDInsight Spark](./media/apache-spark-microsoft-cognitive-toolkit/hdinsight-microsoft-cognitive-toolkit-load-jupyter-notebook.png "Caricare il notebook di Jupyter nel cluster Azure HDInsight Spark")

4. Fare ancora clic su **Carica**.

5. Dopo aver caricato il notebook, fare clic sul nome del notebook e quindi seguire le istruzioni nel notebook stesso per caricare il set di dati ed eseguire l'articolo.

## <a name="see-also"></a>Vedi anche
* [Panoramica: Apache Spark su Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scenari
* [Apache Spark con BI: eseguire l'analisi interattiva dei dati mediante Spark in HDInsight con strumenti di BI](apache-spark-use-bi-tools.md)
* [Apache Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data](apache-spark-ipython-notebook-machine-learning.md) (Apache Spark con Machine Learning: usare Spark in HDInsight per analizzare la temperatura di un edificio usando dati HVAC)
* [Apache Spark con Machine Learning: utilizzare Spark in HDInsight per stimare i risultati dell'ispezione cibo](apache-spark-machine-learning-mllib-ipython.md)
* [Analisi dei log del sito Web con Apache Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)
* [Analisi dei dati di telemetria di Application Insights con Apache Spark in HDInsight](apache-spark-analyze-application-insight-logs.md)

### <a name="create-and-run-applications"></a>Creare ed eseguire applicazioni
* [Creare un'applicazione autonoma con Scala](apache-spark-create-standalone-application.md)
* [Eseguire processi in modalità remota in un cluster Apache Spark usando Apache Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Strumenti ed estensioni
* [Usare il plug-in degli strumenti HDInsight per IntelliJ IDEA per creare e inviare applicazioni Spark Scala](apache-spark-intellij-tool-plugin.md)
* [Usare il plug-in Strumenti HDInsight per IntelliJ IDEA per eseguire il debug di applicazioni Apache Spark in remoto](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Usare i notebook di Apache Zeppelin con un cluster Apache Spark in HDInsight](apache-spark-zeppelin-notebook.md)
* [Kernel disponibili per notebook di Jupyter nel cluster Apache Spark per HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Usare pacchetti esterni con i notebook Jupyter](apache-spark-jupyter-notebook-use-external-packages.md)
* [Installare Jupyter nel computer e connetterlo a un cluster HDInsight Spark](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Gestire risorse
* [Gestire le risorse del cluster Apache Spark in Azure HDInsight](apache-spark-resource-manager.md)
* [Tenere traccia ed eseguire il debug di processi in esecuzione nel cluster Apache Spark in Azure HDInsight](apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../../storage/common/storage-create-storage-account.md
