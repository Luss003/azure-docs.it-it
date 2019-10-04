---
title: Che cos'è Interactive Query in Azure HDInsight?
description: Introduzione a Interactive Query, denominato anche Apache Hive LLAP, in Azure HDInsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: overview
ms.date: 09/17/2019
ms.openlocfilehash: 3e0fddc03ad880d36876276827198b0ac180a00d
ms.sourcegitcommit: 8ef0a2ddaece5e7b2ac678a73b605b2073b76e88
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/17/2019
ms.locfileid: "71076366"
---
# <a name="what-is-interactive-query-in-azure-hdinsight"></a>Che cos'è Interactive Query in Azure HDInsight

Interactive Query (chiamato anche Apache Hive LLAP o [Low Latency Analytical Processing](https://cwiki.apache.org/confluence/display/Hive/LLAP)) è un [tipo di cluster](../hdinsight-hadoop-provision-linux-clusters.md#cluster-types) di Azure HDInsight. Interactive Query supporta la memorizzazione nella cache in memoria, che rende le query Apache Hive più veloci e molto più interattive. I clienti usano Interactive Query per eseguire query sui dati archiviati in archiviazione di Azure e in Azure Data Lake Storage in modo estremamente veloce. Le query interattive permettono agli sviluppatori e ai data scientist di lavorare con maggiore facilità con i Big Data usando gli strumenti BI che preferiscono. HDInsight Interactive Query supporta diversi strumenti per accedere ai Big Data con facilità.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

Un cluster Interactive Query è diverso da un cluster Apache Hadoop in quanto contiene solo il servizio Hive.

È possibile accedere al servizio Hive nel cluster Interactive Query solo tramite la vista Apache Hive di Ambari, Beeline e il driver Microsoft Hive Open Database Connectivity (Hive ODBC). Non è possibile accedervi tramite la console Hive, Templeton, CLI classica di Azure o Azure PowerShell.

## <a name="create-an-interactive-query-cluster"></a>Creare un cluster Interactive Query

Per informazioni sulla creazione di un cluster HDInsight, vedere [Creare cluster Apache Hadoop in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md). Scegliere un tipo di cluster Interactive Query.

> [!IMPORTANT]
> La dimensione minima del nodo head per i cluster Interactive Query è Standard_D13_v2. Per altre informazioni, vedere la [tabella delle dimensioni delle VM di Azure](../../cloud-services/cloud-services-sizes-specs.md#dv2-series).

## <a name="execute-apache-hive-queries-from-interactive-query"></a>Eseguire le query Apache Hive da Interactive Query

Per eseguire query Hive, sono disponibili le opzioni seguenti:

* Usare Microsoft Power BI

    Vedere [Visualizzare i dati Apache Hive di Interactive Query con Power BI in Azure HDInsight](./apache-hadoop-connect-hive-power-bi-directquery.md) Vedere [Visualizzare Big Data con Power BI in Azure HDInsight](../hadoop/apache-hadoop-connect-hive-power-bi.md).

* Usare Visual Studio

    Vedere [Connettersi ad Azure HDInsight ed eseguire query Apache Hive usando Strumenti Data Lake per Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md#run-interactive-apache-hive-queries).

* Usare Visual Studio Code

    Visualizzare [Usare Visual Studio Code per Apache Hive, LLAP o pySpark](../hdinsight-for-vscode.md).
* Eseguire Apache Hive tramite la vista Apache Hive di Ambari.
  
    Vedere [Usare la vista Apache Hive di Apache Ambari con Apache Hadoop in Azure HDInsight](../hadoop/apache-hadoop-use-hive-ambari-view.md).

* Eseguire Apache Hive tramite Beeline.
  
    Vedere [Usare Apache Hive con Apache Hadoop in HDInsight con Beeline](../hadoop/apache-hadoop-use-hive-beeline.md).
  
    È possibile usare Beeline dal nodo head o da un nodo perimetrale vuoto. È consigliabile usare Beeline da un nodo perimetrale vuoto. Per informazioni sulla creazione di un cluster HDInsight usando un nodo perimetrale vuoto, vedere [Usare i nodi perimetrali vuoti in HDInsight](../hdinsight-apps-use-edge-node.md).
* Eseguire Apache Hive tramite Hive ODBC.
  
    Vedere [Connettere Excel ad Apache Hadoop mediante Microsoft Hive ODBC Driver](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).

Per trovare la stringa di connessione JDBC (Java Database Connectivity):

1. Accedere ad Apache Ambari con il seguente URL: `https://<cluster name>.AzureHDInsight.net`.
2. Nel menu a sinistra selezionare **Hive**.
3. Per copiare l'URL, selezionare l'icona degli Appunti:

   ![JDBC di HDInsight Hadoop Interactive Query LLAP](./media/apache-interactive-query-get-started/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="next-steps"></a>Passaggi successivi

* Leggere le informazioni su come [creare cluster Interactive Query in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).
* Leggere le informazioni su come [visualizzare Big Data con Power BI in Azure HDInsight](../hadoop/apache-hadoop-connect-hive-power-bi.md).
* Leggere le informazioni su come [usare Apache Zeppelin per eseguire query Apache Hive in Azure HDInsight](../interactive-query/hdinsight-connect-hive-zeppelin.md).
