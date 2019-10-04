---
title: Aggiungere librerie Apache Hive durante la creazione del cluster HDInsight - Azure
description: Informazioni su come aggiungere librerie Apache Hive (file con estensione jar) a un cluster HDInsight durante la creazione del cluster.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: hrasheed
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: c3ef5362c4d97b8d805212f9cf813c7bc9c8c18c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "67059438"
---
# <a name="add-custom-apache-hive-libraries-when-creating-your-hdinsight-cluster"></a>Aggiungere librerie Apache Hive personalizzate durante la creazione del cluster HDInsight

Informazioni su come precaricare le librerie [Apache Hive](https://hive.apache.org/) in HDInsight. Questo documento contiene informazioni sull'uso di un'Azione Script per precaricare le librerie durante la creazione del cluster. Le librerie aggiunte usando i passaggi in questo documento sono disponibili in modo globale in Hive, non è necessario utilizzare [ADD JAR](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) per caricarli.

## <a name="how-it-works"></a>Funzionamento

Quando si crea un cluster, è possibile usare un'Azione Script per modificare i nodi del cluster al momento della creazione. Lo script in questo documento accetta un solo parametro, ovvero la posizione delle librerie. Questa posizione deve essere in un Account di archiviazione di Azure e le librerie devono essere archiviate come file con estensione jar.

Durante la creazione del cluster, lo script enumera i file, li copia nella directory `/usr/lib/customhivelibs/` nei nodi head e di lavoro, quindi li aggiunge alla proprietà `hive.aux.jars.path` nel file `core-site.xml`. Nei cluster basati su Linux, aggiorna anche il file `hive-env.sh` con il percorso dei file.

> [!NOTE]  
> L'uso delle azioni script in questo articolo rende le librerie disponibili negli scenari seguenti:
>
> * **HDInsight basato su Linux**: quando si usa un client Hive, **WebHCat**, e **HiveServer2**.
> * **HDInsight basato su Windows**, quando si usa un client Hive e **WebHCat**.

## <a name="the-script"></a>Lo script

**Percorso dello script**

Per i **cluster basati su Linux**: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)

Per i **cluster basati su Windows**: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

**Requisiti**

* Gli script devono essere applicati ai **nodi head** e ai **nodi di lavoro**.

* I file con estensione jar da installare devono essere memorizzati nell'archivio BLOB di Azure in un **singolo contenitore**.

* L'account di archiviazione contenente la libreria dei file con estensione jar **deve** essere collegato al cluster HDInsight durante la creazione. Deve essere l'account di archiviazione predefinito o un account aggiunto tramite la __configurazione facoltativa__.

* Il percorso WASB al contenitore deve essere specificato come parametro dell'azione script. Ad esempio, se il file con estensione jar sono archiviati in un contenitore denominato **libs** su un account di archiviazione denominato **mystorage**, il parametro sarebbe **wasb://libs\@ mystorage.BLOB.Core.Windows.NET/** .

  > [!NOTE]  
  > In questo documento si presuppone che un account di archiviazione e un contenitore BLOB siano già stati creati e che i file siano stati caricati nel contenitore.
  >
  > Se non è stato creato un account di archiviazione, è possibile farlo tramite il [Portale di Azure](https://portal.azure.com). È quindi possibile usare un'utilità, ad esempio [Azure Storage Explorer](https://storageexplorer.com/) (Esplora archivi di Azure), per creare un contenitore nell'account e caricarvi i file.

## <a name="create-a-cluster-using-the-script"></a>Creare un cluster usando lo script

> [!NOTE]  
> La procedura seguente consente di creare un cluster HDInsight basato su Linux. Per creare un cluster basato su Windows, selezionare **Windows** come sistema operativo del cluster durante la creazione del cluster e usare lo script di Windows (PowerShell) invece dello script bash.
>
> È anche possibile usare Azure PowerShell o HDInsight .NET SDK per creare un cluster con questo script. Per altre informazioni sull'uso di questi metodi, vedere [Personalizzare i cluster HDInsight con azioni script](hdinsight-hadoop-customize-cluster-linux.md).

1. Avviare il provisioning di un cluster seguendo i passaggi descritti in [Effettuare il provisioning di cluster HDInsight in Linux](hdinsight-hadoop-provision-linux-clusters.md) senza completarlo.

2. Nella sezione **Configurazione facoltativa** selezionare **Azioni script** e specificare le informazioni seguenti:

   * **NOME**: Immettere un nome descrittivo per l'azione script.

   * **URI SCRIPT**: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh.

   * **HEAD**: Selezionare questa opzione.

   * **RUOLO DI LAVORO**: selezionare questa opzione.

   * **ZOOKEEPER**: Lasciare vuoto.

   * **PARAMETRI**: immettere l'indirizzo WASB per l'account di archiviazione e il contenitore che contiene i file con estensione jar. Ad esempio, **wasb://libs\@mystorage.blob.core.windows.net/** .

3. Nella parte inferiore di **Azioni di script** usare il pulsante **Seleziona** per salvare la configurazione.

4. Nella sezione **Configurazione facoltativa** selezionare **Account di archiviazione collegati** e il collegamento **Aggiungi una chiave di archiviazione**. Selezionare l'account di archiviazione che contiene i JAR. Usare quindi i pulsanti **seleziona** per salvare le impostazioni e tornare alla **Configurazione facoltativa**.

5. Per salvare la configurazione facoltativa, usare il pulsante **Seleziona** nella parte inferiore della sezione **Configurazione facoltativa**.

6. Continuare il provisioning del cluster come descritto in [Effettuare il provisioning dei cluster HDInsight in Linux](hdinsight-hadoop-provision-linux-clusters.md).

Al termine della creazione del cluster, sarà possibile usare i file con estensione JAR aggiunti tramite questo script da Hive senza dover usare l'istruzione `ADD JAR`.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sull'uso di Hive, vedere [Usare Apache Hive con HDInsight](hadoop/hdinsight-use-hive.md)
