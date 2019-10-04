---
title: 'Azure Toolkit for IntelliJ: debug delle applicazioni Spark da remoto tramite SSH '
description: Istruzioni dettagliate su come usare gli strumenti HDInsight in Azure Toolkit for IntelliJ per eseguire il debug remoto di applicazioni in cluster di HDInsight tramite SSH
keywords: eseguire debug remoto di intellij, debug remoto di intellij, ssh, intellij, hdinsight, debug di intellij, debug
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 11/25/2017
ms.openlocfilehash: 1f3f08385ac4f7044f4e6c4e4110be701e8f576d
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/25/2019
ms.locfileid: "71266215"
---
# <a name="debug-apache-spark-applications-on-an-hdinsight-cluster-with-azure-toolkit-for-intellij-through-ssh"></a>Eseguire il debug di applicazioni Apache Spark in un cluster HDInsight con Azure Toolkit for IntelliJ tramite SSH

Questo articolo contiene istruzioni dettagliate su come usare gli strumenti HDInsight in [Azure Toolkit for IntelliJ](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij?view=azure-java-stable) per eseguire il debug remoto di applicazioni in un cluster di HDInsight. Per eseguire il debug del progetto, è anche possibile guardare il video [Debug HDInsight Spark applications with Azure Toolkit for IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) (Eseguire il debug delle applicazioni Spark di HDInsight con Azure Toolkit for IntelliJ).

**Prerequisiti**
* **Strumenti HDInsight in Azure Toolkit per IntelliJ**. Questo strumento fa parte di Azure Toolkit for IntelliJ. Per altre informazioni, vedere [Installare Azure Toolkit for IntelliJ](https://docs.microsoft.com/azure/azure-toolkit-for-intellij-installation). **Azure Toolkit for IntelliJ**. Usare questo toolkit per creare applicazioni Apache Spark per cluster HDInsight. Per altre informazioni, seguire le istruzioni in [Usare Azure Toolkit per IntelliJ per creare applicazioni Apache Spark per il cluster HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).

* **Servizio SSH di HDInsight con gestione di nome utente e password**. Per altre informazioni, vedere [Connettersi a HDInsight (Apache Hadoop) con SSH](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) e [Usare il tunneling SSH per accedere all'interfaccia utente Web di Ambari, JobHistory, NameNode, Apache Oozie e altre interfacce utente Web](https://docs.microsoft.com/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel). 
 
## <a name="learn-how-to-perform-local-run-and-debugging"></a>Informazioni su esecuzione e debug in modalità locale
### <a name="scenario-1-create-a-spark-scala-application"></a>Scenario 1: Creare un'applicazione Spark Scala 

1. Avviare IntelliJ IDEA e creare un progetto. Nella finestra di dialogo **New Project** (Nuovo progetto) seguire questa procedura:

   a. Selezionare **Azure Spark/HDInsight**. 

   b. Selezionare un modello Java o Scala in base alle preferenze. Scegliere una delle opzioni seguenti:

   - **Progetto Spark (Java)**

   - **Progetto Spark (scala)**

   - **Progetto Spark con esempi (scala)**

   - **Esempi di debug di progetto Spark con attività non riuscita (anteprima) (scala)**

     Questo esempio usa un **progetto Spark con il modello Samples (scala)** .

   c. Nell'elenco **Build tool** (Strumento di compilazione) selezionare uno degli strumenti seguenti, in base alla necessità:

   - **Maven**, per il supporto della creazione guidata del progetto scala.

   - **SBT**, per la gestione delle dipendenze e la compilazione per il progetto scala.

     ![IntelliJ Crea nuovo progetto Spark](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-create-projectfor-debug-remotely.png)

   d. Selezionare **Avanti**.

1. Nella finestra **New Project** (Nuovo progetto) successiva seguire questa procedura:

   ![IntelliJ nuovo progetto selezionare versione Spark](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-new-project.png)

   a. Specificare un nome per il progetto e il relativo percorso.

   b. Nell'elenco a discesa **Project SDK** (SDK progetto) selezionare **Java 1.8** per il cluster **Spark 2.x** oppure **Java 1.7** per il cluster **Spark 1.x**.

   c. Nell'elenco a discesa **Spark version** (Versione di Spark) la creazione guidata del progetto Scala inserisce la versione corretta per Spark SDK e Scala SDK. Se la versione del cluster Spark è precedente alla 2.0, selezionare **Spark 1.x**. In caso contrario, selezionare **Spark 2.x**. In questo esempio viene usata la versione **Spark 2.0.2 (Scala 2.11.8)** .

   d. Selezionare **Fine**.

1. Selezionare **src** > **main** > **scala** per aprire il codice nel progetto. In questo esempio viene usato lo script **SparkCore_wasbloTest**.

### <a name="prerequisite-for-windows"></a>Prerequisito per Windows
Quando si esegue l'applicazione Spark Scala locale in un computer Windows, potrebbe essere restituita un'eccezione, come spiegato in [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356), che si verifica a causa di un file WinUtils.exe mancante in Windows.

Per risolvere questo errore, [scaricare il file eseguibile](https://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) in un percorso come **C:\WinUtils\bin**. È quindi necessario aggiungere una variabile di ambiente **HADOOP_HOME** e impostare il valore della variabile su **C:\WinUtils**.

### <a name="scenario-2-perform-local-run"></a>Scenario 2: Eseguire l'esecuzione in locale

1. Aprire lo script **SparkCore_wasbloTest**, fare clic con il pulsante destro del mouse sull'editor di script e quindi selezionare l'opzione **Run '[Spark Job]XXX'** (Esegui '[Processo Spark]XXX') per avviare l'esecuzione in modalità locale.

1. Dopo aver completato l'esecuzione in locale, è possibile visualizzare il file di output salvato nella directory **data** >  **__default__** della finestra di gestione del progetto corrente.

    ![Risultato dell'esecuzione locale del progetto IntelliJ](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/spark-local-run-result.png)

1. Per questi strumenti la configurazione dell'esecuzione in locale predefinita viene impostata automaticamente quando l'esecuzione e il debug vengono eseguiti in modalità locale. Aprire la configurazione **[Spark in HDInsight] xxx** nell'angolo superiore destro. è possibile vedere **[Spark in HDInsight] xxx** già creato in **Apache Spark su HDInsight**. Passare alla scheda **Locally Run** (Esecuzione in locale).

    ![IntelliJ eseguire configurazioni di debug esecuzione locale](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/local-run-configuration.png)

    - [Environment variables](#prerequisite-for-windows) (Variabili di ambiente): se la variabile di ambiente di sistema **HADOOP_HOME** è già stata impostata su **C:\WinUtils**, viene automaticamente rilevato che l'aggiunta manuale non è necessaria.
    - [WinUtils.exe Location](#prerequisite-for-windows) (Posizione WinUtils.exe): se la variabile di ambiente di sistema non è stata impostata, per trovare la posizione è sufficiente fare clic sul relativo pulsante.
    - È sufficiente scegliere una delle due opzioni, che non sono necessarie in MacOS e Linux.

1. È anche possibile impostare la configurazione manualmente prima di eseguire l'esecuzione e il debug in modalità locale. Nello screenshot precedente selezionare il segno più ( **+** ). Selezionare quindi l'opzione **Apache Spark on HDInsight** . Immettere le informazioni per il **nome** e il **nome della classe principale** da salvare e quindi fare clic sul pulsante Esecuzione in locale.

### <a name="scenario-3-perform-local-debugging"></a>Scenario 3: Eseguire il debug in locale
1. Aprire lo script **SparkCore_wasbloTest** e impostare i punti di interruzione.
1. Fare clic con il pulsante destro del mouse sull'editor di script e quindi selezionare l'opzione **debug ' [Spark on HDInsight] xxx '** per eseguire il debug locale.

## <a name="learn-how-to-perform-remote-run-and-debugging"></a>Informazioni su esecuzione e debug in modalità remota
### <a name="scenario-1-perform-remote-run"></a>Scenario 1: Esecuzione remota

1. Per accedere al menu **Edit Configurations** (Modifica configurazioni) selezionare l'icona nell'angolo in alto a destra. Da questo menu è possibile creare o modificare le configurazioni per il debug remoto.

   ![Configurazioni di modifica IntelliJ HDI](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-edit-configurations.png)

1. Nella finestra di dialogo **Run/Debug Configurations** (Esegui/Debug delle configurazioni) selezionare il segno più ( **+** ). Selezionare quindi l'opzione **Apache Spark on HDInsight** .

   ![IntelliJ aggiungere una nuova configurazione](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-add-new-Configuration.png)

1. Passare alla scheda **Remotely Run in Cluster** (Esecuzione remota nel cluster). Immettere le informazioni per **Nome**, **Cluster Spark**, e **Nome della classe principale**. Fare quindi clic su **configurazione avanzata (debug remoto)** . Questi strumenti supportano il debug con **executor**. Il valore predefinito della chiave **numExectors** è 5. È consigliabile non impostarlo su un valore maggiore di 3.

   ![IntelliJ eseguire configurazioni di debug](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-run-debug-configurations.png)

1. Nella parte **configurazione avanzata (debug remoto)** selezionare **Abilita debug remoto Spark**. Immettere il nome utente SSH e inserire una password oppure usare un file di chiave privata. Se si desidera eseguire il debug in modalità remota, è necessario impostare questa opzione. Non è necessario impostarla se si desidera usare l'esecuzione in modalità remota.

   ![La configurazione avanzata di IntelliJ Abilita il debug remoto Spark](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-enable-spark-remote-debug.png)

1. A questo punto, la configurazione viene salvata con il nome specificato. Per visualizzare i dettagli della configurazione, selezionare il relativo nome. Per apportare modifiche, selezionare **Modifica configurazioni**.

1. Dopo aver completato le impostazioni di configurazione, è possibile eseguire il progetto con il cluster remoto oppure eseguire il debug remoto.

   ![Pulsante di esecuzione remota del processo Spark remoto di IntelliJ](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/perform-remote-run-button.png)

1. Fare clic sul pulsante **Disconnetti**. I log di invio non vengono visualizzati nel riquadro sinistro, ma l'esecuzione è ancora in corso nel back-end.

   ![Risultato dell'esecuzione remota del processo Spark remoto di IntelliJ debug](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/spark-remote-run-result.png)

### <a name="scenario-2-perform-remote-debugging"></a>Scenario 2: Eseguire l'esecuzione da remoto
1. Impostare i punti di interruzione e quindi fare clic sull'icona **Remote debug** (Debug remoto). La differenza rispetto all'invio remoto risiede nel fatto che non è necessario configurare il nome utente/password SSH.

   ![Icona di debug del processo Spark remoto di IntelliJ debug](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debug-icon.png)

1. Quando l'esecuzione del programma raggiunge il punto di interruzione, nel riquadro **Debugger** vengono visualizzate la scheda **Diver** e le due schede **Executor**. Selezionare l'icona **Resume Program** (Riprendi programma) per continuare l'esecuzione del codice, che raggiungerà il punto di interruzione successivo. È necessario passare alla scheda **Executor** (Esecutore) corretta per trovare l'esecutore di destinazione da sottoporre a debug. È possibile visualizzare i log di esecuzione nella scheda **Console** corrispondente.

   ![IntelliJ debug-scheda debug processo Spark remoto](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debugger-tab.png)

### <a name="scenario-3-perform-remote-debugging-and-bug-fixing"></a>Scenario 3: Eseguire il debug e la correzione di bug da remoto

1. Impostare due punti di interruzione e quindi selezionare l'icona **Debug** per avviare il processo di debug remoto.

1. Il codice si interrompe in corrispondenza del primo punto di interruzione e le informazioni di parametro e variabile vengono visualizzate nella finestra **Variables** (Variabili).

1. Selezionare l'icona **Resume Program** (Riprendi programma) per continuare. Il codice si interrompe in corrispondenza del secondo punto. L'eccezione viene rilevata come previsto.

   ![Errore di generazione del processo Spark remoto di IntelliJ debug](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-throw-error.png)

1. Selezionare di nuovo l'icona **Resume Program** (Riprendi programma). La finestra **HDInsight Spark Submission** (Invio Spark in HDInsight) mostra un errore di "esecuzione processo non riuscita".

   ![Invio errore processo Spark remoto di IntelliJ debug](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-error-submission.png)

1. Per aggiornare dinamicamente il valore della variabile usando la funzionalità di debug di IntelliJ, selezionare nuovamente **Debug**. Viene visualizzato di nuovo il riquadro **Variables** (Variabili).

1. Fare clic con il pulsante destro del mouse sulla destinazione nella scheda **Debug**, quindi selezionare **Imposta valore**. Successivamente inserire un nuovo valore per la variabile. e selezionare **Invia** per salvarlo.

   ![Valore del set di processi Spark remoto di IntelliJ debug](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-set-value1.png)

1. Selezionare l'icona **Resume Program** (Riprendi programma) per continuare a eseguire il programma. Questa volta, non viene rilevata alcuna eccezione. È possibile notare come il progetto venga eseguito correttamente senza eccezioni.

   ![Processo Spark remoto di IntelliJ debug senza eccezione](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debug-without-exception.png)

## <a name="seealso"></a>Passaggi successivi

* [Panoramica: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="demo"></a>Demo
* Creare il progetto Scala (video): [Create Apache Spark Scala Applications](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) (Creare applicazioni Apache Spark in Scala)
* Debug remoto (video): [Use Azure Toolkit for IntelliJ to debug Apache Spark applications remotely on HDInsight Cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) (Usare Azure Toolkit for IntelliJ per il debug remoto di applicazioni Apache Spark nel cluster HDInsight)

### <a name="scenarios"></a>Scenari
* [Apache Spark con BI: eseguire l’analisi interattiva dei dati con strumenti di Business Intelligence mediante Spark in HDInsight](apache-spark-use-bi-tools.md)
* [Apache Spark con apprendimento automatico: usare Spark in HDInsight per analizzare la temperatura di un edificio con dati HVAC](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark con apprendimento automatico: usare Spark in HDInsight per stimare i risultati di controllo degli alimenti](apache-spark-machine-learning-mllib-ipython.md)
* [Analisi dei log del sito Web con Apache Spark in HDInsight](../hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Creare ed eseguire applicazioni
* [Creare un'applicazione autonoma con Scala](../hdinsight-apache-spark-create-standalone-application.md)
* [Eseguire processi in modalità remota in un cluster Apache Spark usando Apache Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Strumenti ed estensioni
* [Usare Azure Toolkit for IntelliJ per creare applicazioni Apache Spark per un cluster HDInsight](apache-spark-intellij-tool-plugin.md)
* [Usare Azure Toolkit for IntelliJ per il debug remoto di applicazioni Apache Spark tramite VPN](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Use HDInsight Tools in Azure Toolkit for Eclipse to create Apache Spark applications (Usare gli strumenti HDInsight nel Toolkit di Azure per Eclipse per creare applicazioni Apache Spark)](../hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Usare i notebook di Apache Zeppelin con cluster Apache Spark in HDInsight](apache-spark-zeppelin-notebook.md)
* [Kernel disponibili per notebook di Jupyter nel cluster Apache Spark per HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Usare pacchetti esterni con i notebook Jupyter](apache-spark-jupyter-notebook-use-external-packages.md)
* [Installare Jupyter Notebook nel computer e connetterlo a un cluster HDInsight Spark](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Gestisci risorse
* [Gestire le risorse del cluster Apache Spark in Azure HDInsight](apache-spark-resource-manager.md)
* [Tenere traccia ed eseguire il debug di processi in esecuzione nel cluster Apache Spark in Azure HDInsight](apache-spark-job-debugging.md)
