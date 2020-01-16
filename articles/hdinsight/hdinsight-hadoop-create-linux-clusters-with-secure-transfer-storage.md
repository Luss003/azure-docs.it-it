---
title: Apache Hadoop & archiviazione con trasferimento sicuro-Azure HDInsight
description: Informazioni su come creare cluster HDInsight con account di archiviazione di Azure con trasferimento sicuro abilitato.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/04/2019
ms.openlocfilehash: bcb0e9551f4415b2aac9eb2d641c91df9f692437
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/15/2020
ms.locfileid: "75979111"
---
# <a name="create-apache-hadoop-cluster-with-secure-transfer-storage-accounts-in-azure-hdinsight"></a>Creare un cluster Apache Hadoop con account di archiviazione con trasferimento sicuro in Azure HDInsight

La funzionalità [Trasferimento sicuro obbligatorio](../storage/common/storage-require-secure-transfer.md) aumenta la sicurezza dell'account di archiviazione di Azure perché consente l'invio di tutte le richieste all'account solo tramite connessioni sicure. Questa funzionalità e lo schema wasbs sono supportati solo dal cluster HDInsight versione 3.6 o successiva.

## <a name="prerequisites"></a>Prerequisiti

Prima di iniziare questo articolo, è necessario disporre di:

* Sottoscrizione di Azure: per creare un account di valutazione gratuito di un mese, passare a [Azure.Microsoft.com/Free](https://azure.microsoft.com/free).
* Un account di archiviazione di Azure con trasferimento sicuro abilitato. Per istruzioni, vedere [Creare un account di archiviazione](../storage/common/storage-account-create.md) e [Richiedere il trasferimento sicuro](../storage/common/storage-require-secure-transfer.md). L'abilitazione del trasferimento di archiviazione protetta dopo la creazione di un cluster richiede passaggi aggiuntivi non trattati in questo articolo.
* Un contenitore BLOB nell'account di archiviazione.

## <a name="create-cluster"></a>Creare cluster

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

In questa sezione viene creato un cluster Hadoop in HDInsight usando un [modello di Azure Resource Manager](../azure-resource-manager/templates/deploy-powershell.md). Il modello è disponibile in [GitHub](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-existing-default-storage-account/). L'esperienza del modello Gestione risorse non è necessaria per seguire questo articolo. Per altri metodi di creazione di cluster e per comprendere le proprietà usate in questo articolo, vedere [creare cluster HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Fare clic sull'immagine seguente per accedere ad Azure e aprire il modello di Resource Manager nel portale di Azure.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-existing-default-storage-account%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hadoop-create-linux-clusters-with-secure-transfer-storage/hdi-deploy-to-azure1.png" alt="Deploy to Azure button for new cluster"></a>

2. Seguire le istruzioni per creare il cluster con le specifiche seguenti:

    * Specificare HDInsight versione 3.6. È necessaria la versione 3.6 o successiva.
    * Specificare un account di archiviazione con trasferimento sicuro abilitato.
    * Usare un nome breve per l'account di archiviazione.
    * È necessario creare prima l'account di archiviazione e il contenitore BLOB.

      Per le istruzioni, vedere [Creare cluster](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).

Se si usa l'azione di script per fornire i file di configurazione, è necessario usare wasbs nelle impostazioni seguenti:

* fs.defaultFS (core-site)
* spark.eventLog.dir
* spark.history.fs.logDirectory

## <a name="add-additional-storage-accounts"></a>Aggiungere altri account di archiviazione

Sono disponibili diverse opzioni per aggiungere altri account di archiviazione con trasferimento sicuro abilitato:

* Modificare il modello di Azure Resource Manager nell'ultima sezione.
* Creare un cluster usando il [portale di Azure](https://portal.azure.com) e specificare l'account di archiviazione collegato.
* Usare l'azione script per aggiungere altri account di archiviazione con trasferimento sicuro abilitato a un cluster HDInsight esistente. Per altre informazioni, vedere [Aggiungere altri account di archiviazione a HDInsight](hdinsight-hadoop-add-storage.md).

## <a name="next-steps"></a>Passaggi successivi

In questo articolo si è appreso come creare un cluster HDInsight e come abilitare il trasferimento sicuro per gli account di archiviazione.

Per altre informazioni sull'analisi dei dati con HDInsight, vedere gli articoli seguenti:

* Per altre informazioni sull'uso di [Apache hive](https://hive.apache.org/) con HDInsight, inclusa la procedura per eseguire query hive da Visual Studio, vedere [usare Apache hive con HDInsight](hadoop/hdinsight-use-hive.md).
* Per informazioni su [Apache Hadoop MapReduce](https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html), un modo per scrivere programmi che elaborano i dati in Hadoop, vedere [usare Apache Hadoop MapReduce con HDInsight](hadoop/hdinsight-use-mapreduce.md).
* Per informazioni sull'uso di HDInsight Tools per Visual Studio per analizzare i dati in HDInsight, vedere [Introduzione all'uso di Apache Hadoop Tools per Visual Studio per HDInsight](hadoop/apache-hadoop-visual-studio-tools-get-started.md).

Per altre informazioni sulle modalità di archiviazione dei dati in HDInsight o sull'importazione dei dati in HDInsight, vedere gli articoli seguenti:

* Per informazioni sul modo in cui HDInsight usa Archiviazione di Azure, vedere [Usare Archiviazione di Azure con HDInsight](hdinsight-hadoop-use-blob-storage.md).
* Per informazioni su come caricare i dati in HDInsight, vedere [Caricare dati in HDInsight](hdinsight-upload-data.md).

Per altre informazioni sulla creazione o la gestione di un cluster HDInsight, vedere gli articoli seguenti:

* Per altre informazioni sulla gestione di cluster HDInsight basati su Linux, vedere [Gestire i cluster HDInsight tramite Apache Ambari](hdinsight-hadoop-manage-ambari.md).
* Per altre informazioni sulle opzioni che è possibile selezionare durante la creazione di un cluster HDInsight, vedere [Creare cluster Hadoop basati su Linux in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
* Se si ha familiarità con Linux e Apache Hadoop, ma si vuole conoscere le specifiche di Hadoop in HDInsight, vedere [uso di HDInsight in Linux](hdinsight-hadoop-linux-information.md). In questo articolo sono disponibili informazioni quali:

  * URL per i servizi ospitati nel cluster, ad esempio [Apache Ambari](https://ambari.apache.org/) e [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat)
  * Il percorso dei file [Apache Hadoop](https://hadoop.apache.org/) e gli esempi nel file system locale
  * L'uso di archiviazione di Azure (WASB) anziché di [Apache Hadoop HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsUserGuide.html) come archivio predefinito di dati
