---
title: Eseguire il debug di processi Apache Spark in esecuzione in Azure HDInsight
description: Usare l'interfaccia utente di YARN, l'interfaccia utente di Spark e il server della cronologia di Spark per tenere traccia ed eseguire il debug di processi in esecuzione in un cluster Spark in Azure HDInsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 12/05/2018
ms.openlocfilehash: 0e80aa44652efbc58f8259944058aabe59ca5d1a
ms.sourcegitcommit: e1b6a40a9c9341b33df384aa607ae359e4ab0f53
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71338472"
---
# <a name="debug-apache-spark-jobs-running-on-azure-hdinsight"></a>Eseguire il debug di processi Apache Spark in esecuzione in Azure HDInsight

Questo articolo illustra come tenere traccia ed eseguire il debug di processi [Apache Spark](https://spark.apache.org/) in esecuzione nei cluster HDInsight tramite l'interfaccia utente di [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html), l'interfaccia utente di Spark e il server cronologia Spark. Sarà necessario avviare un processo Spark usando un notebook disponibile nel cluster Spark, **Machine Learning: analisi predittiva dei dati di controllo degli alimenti tramite MLLib**. È possibile usare la procedura seguente anche per tenere traccia di un'applicazione inviata usando qualsiasi altro approccio, ad esempio, **spark-submit**.

## <a name="prerequisites"></a>Prerequisiti

È necessario disporre di quanto segue:

* Una sottoscrizione di Azure. Vedere [Ottenere una versione di prova gratuita di Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Un cluster Apache Spark in HDInsight. Per istruzioni, vedere l'articolo dedicato alla [creazione di cluster Apache Spark in Azure HDInsight](apache-spark-jupyter-spark-sql.md).
* Il notebook dovrebbe essere in esecuzione, **[Machine Learning: analisi predittiva dei dati di controllo degli alimenti tramite MLLib](apache-spark-machine-learning-mllib-ipython.md)** . Per istruzioni su come eseguire questo notebook, seguire il collegamento.  

## <a name="track-an-application-in-the-yarn-ui"></a>Tenere traccia di un'applicazione nell'interfaccia utente di YARN

1. Avviare l'interfaccia utente di YARN Fare clic su **Yarn** in **Dashboard cluster**.

    ![portale di Azure avviare l'interfaccia utente di YARN](./media/apache-spark-job-debugging/launch-apache-yarn-ui.png)

   > [!TIP]  
   > In alternativa, è anche possibile avviare l'interfaccia utente di YARN dall'interfaccia utente di Ambari. Per avviare l'interfaccia utente Ambari, fare clic su **home Ambari** in **Dashboard cluster**. Nell'interfaccia utente di Ambari fare clic su **YARN**, su **Quick Links** (Collegamenti rapidi), sulla funzionalità di gestione risorse attiva e quindi fare clic su **Resource Manager UI** (Interfaccia utente di Gestione risorse).

2. Poiché il processo Spark è stato avviato con Jupyter Notebook, il nome dell'applicazione è **remotesparkmagics**. Si tratta del nome per tutte le applicazioni avviate dai notebook. Per ottenere altre informazioni sul processo, fare clic sull'ID dell'applicazione relativo al nome dell'applicazione. Verrà avviata la visualizzazione dell'applicazione.

    ![Il server della cronologia Spark trova l'ID applicazione Spark](./media/apache-spark-job-debugging/find-application-id1.png)

    Per le applicazioni avviate da Jupyter Notebook lo stato è sempre **IN ESECUZIONE** fino a quando non si esce dal notebook.
3. Nella visualizzazione dell'applicazione è possibile eseguire il drill-down per trovare i contenitori associati all'applicazione e i log (stdout o stderr). È anche possibile avviare l'interfaccia utente di Spark facendo clic sul collegamento corrispondente all' **URL di verifica**, come illustrato di seguito.

    ![Log del contenitore di download del server cronologia Spark](./media/apache-spark-job-debugging/download-container-logs.png)

## <a name="track-an-application-in-the-spark-ui"></a>Tenere traccia di un'applicazione nell'interfaccia utente di Spark

Nell'interfaccia utente di Spark è possibile eseguire il drill-down dei processi Spark generati dall'applicazione avviata in precedenza.

1. Per avviare l'interfaccia utente di Spark, nella visualizzazione dell'applicazione, fare clic sul collegamento relativo a **URL di verifica**, come illustrato nella schermata precedente. È possibile visualizzare tutti i processi Spark avviati dall'applicazione in esecuzione in Jupyter Notebook.

    ![Scheda processi server cronologia Spark](./media/apache-spark-job-debugging/view-apache-spark-jobs.png)

2. Fare clic sulla scheda **Executors** per visualizzare le informazioni di elaborazione e archiviazione per ogni executor. È anche possibile recuperare lo stack di chiamate facendo clic sul collegamento **Thread Dump** .

    ![Scheda esecutori Server cronologia Spark](./media/apache-spark-job-debugging/view-spark-executors.png)

3. Fare clic sulla scheda **Stages** per visualizzare le fasi associate all'applicazione.

    Fasi del ![server della cronologia Spark](./media/apache-spark-job-debugging/view-apache-spark-stages.png "visualizzazione") delle fasi Spark

    Ogni fase può avere più attività per cui è possibile visualizzare le statistiche di esecuzione, come illustrato di seguito.

    Dettagli ![scheda fasi del server cronologia Spark]Dettagli(./media/apache-spark-job-debugging/view-spark-stages-details.png "Visualizzazione fasi Spark dettagli")

4. Nella pagina dei dettagli relativi alle fasi è possibile avviare la visualizzazione DAG. Espandere il collegamento **DAG Visualization** nella parte superiore della pagina, come illustrato di seguito.

    ![Mostrare la visualizzazione DAG delle fasi di Spark](./media/apache-spark-job-debugging/view-spark-stages-dag-visualization.png)

    DAG o Direct Aclyic Graph rappresenta le diverse fasi dell'applicazione. Ogni casella blu nel grafico rappresenta un'operazione Spark richiamata dall'applicazione.

5. Nella pagina dei dettagli relativi alle fasi è anche possibile avviare la visualizzazione della sequenza temporale dell'applicazione. Espandere il collegamento **Event Timeline** nella parte superiore della pagina, come illustrato di seguito.

    ![Visualizzare e la sequenza temporale di eventi delle fasi di Spark](./media/apache-spark-job-debugging/view-spark-stages-event-timeline.png)

    Verranno visualizzati gli eventi di Spark sotto forma di sequenza temporale. La visualizzazione della sequenza temporale è disponibile in tre livelli, tra processi, all'interno di un processo e all'interno di una fase. L'immagine precedente mostra la visualizzazione della sequenza temporale per una determinata fase.

   > [!TIP]  
   > Se si seleziona la casella di controllo **Enable zooming** , è possibile scorrere a sinistra e destra nella visualizzazione della sequenza temporale.

6. Altre schede nell'interfaccia utente di Spark forniscono anche informazioni utili sull'istanza di Spark.

   * Scheda Storage: se l'applicazione crea oggetti RDD, è possibile trovare informazioni corrispondenti in questa scheda.
   * Scheda Environment (ambiente): questa scheda fornisce numerose informazioni utili sull'istanza di Spark, ad esempio:
     * Versione di Scala
     * Directory dei log eventi associata al cluster
     * Numero di core executor per l'applicazione
     * e così via.

## <a name="find-information-about-completed-jobs-using-the-spark-history-server"></a>Trovare informazioni sui processi completati tramite Server cronologia Spark

Una volta completato un processo, le informazioni corrispondenti vengono salvate in modo permanente nel Server cronologia Spark.

1. Per avviare il Server cronologia Spark, nel pannello Panoramica fare clic su **Server cronologia Spark** e quindi su **Dashboard cluster**.

    ![Portale di Azure avviare il server della cronologia Spark]per(./media/apache-spark-job-debugging/launch-spark-history-server.png "avviare la cronologia Spark Server1")

   > [!TIP]  
   > In alternativa, è anche possibile avviare l'interfaccia utente del Server cronologia Spark dall'interfaccia utente di Ambari. Per avviare l'interfaccia utente di Ambari, nel pannello Panoramica fare clic su **home Ambari** in **Dashboard cluster**. Nell'interfaccia utente di Ambari fare clic su **Spark**, fare clic su **Collegamenti rapidi** e quindi sull'interfaccia utente di **Server cronologia Spark**.

2. Vengono elencate tutte le applicazioni completate. Fare clic su un ID applicazione per eseguire il drill-down di un'applicazione per altre informazioni.

    Il ![Server cronologia Spark completa le applicazioni](./media/apache-spark-job-debugging/view-completed-applications.png "Avvia la cronologia Spark Server2")

## <a name="see-also"></a>Vedere anche

* [Gestire le risorse del cluster Apache Spark in Azure HDInsight](apache-spark-resource-manager.md)
* [Eseguire il debug dei processi Apache Spark tramite il Server cronologia Spark esteso](apache-azure-spark-history-server.md)

### <a name="for-data-analysts"></a>Per gli analisti dei dati

* [Apache Spark con Machine Learning: usare Spark in HDInsight per analizzare la temperatura di un edificio con dati HVAC](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark con apprendimento automatico: usare Spark in HDInsight per stimare i risultati di controllo degli alimenti](apache-spark-machine-learning-mllib-ipython.md)
* [Analisi dei log del sito Web con Apache Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)
* [Analisi dei dati di telemetria di Application Insights con Apache Spark in HDInsight](apache-spark-analyze-application-insight-logs.md)
* [Usare Caffe in Azure HDInsight Spark per l'apprendimento avanzato distribuito](apache-spark-deep-learning-caffe.md)

### <a name="for-spark-developers"></a>Per gli sviluppatori di Spark

* [Creare un'applicazione autonoma con Scala](apache-spark-create-standalone-application.md)
* [Eseguire processi in modalità remota in un cluster Apache Spark usando Apache Livy](apache-spark-livy-rest-interface.md)
* [Usare il plug-in degli strumenti HDInsight per IntelliJ IDEA per creare e inviare applicazioni Spark in Scala](apache-spark-intellij-tool-plugin.md)
* [Usare il plug-in Strumenti HDInsight per IntelliJ IDEA per eseguire il debug di applicazioni Apache Spark in remoto](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Usare i notebook di Apache Zeppelin con un cluster Apache Spark in HDInsight](apache-spark-zeppelin-notebook.md)
* [Kernel disponibili per notebook di Jupyter nel cluster Apache Spark per HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Usare pacchetti esterni con i notebook Jupyter](apache-spark-jupyter-notebook-use-external-packages.md)
* [Installare Jupyter Notebook nel computer e connetterlo a un cluster HDInsight Spark](apache-spark-jupyter-notebook-install-locally.md)
