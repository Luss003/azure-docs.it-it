---
title: Usare C# con MapReduce in Hadoop in HDInsight - Azure
description: Informazioni su come usare C# per creare soluzioni di MapReduce con Apache Hadoop in Azure HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/15/2019
ms.author: hrasheed
ms.openlocfilehash: 5784fb4f4ab0f46d2db7e5e8cfe9deeafabb4e90
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/17/2019
ms.locfileid: "71066952"
---
# <a name="use-c-with-mapreduce-streaming-on-apache-hadoop-in-hdinsight"></a>Usare C# con lo streaming di MapReduce su Apache Hadoop in HDInsight

Informazioni su come usare C# per creare una soluzione di MapReduce su HDInsight.

> [!IMPORTANT]
> Linux è l'unico sistema operativo usato in HDInsight versione 3.4 o successiva. Per altre informazioni, vedere l'articolo sul [controllo delle versioni del componente di HDInsight](../hdinsight-component-versioning.md).

Apache Hadoop Streaming è un'utilità che consente di eseguire processi MapReduce tramite uno script o un eseguibile. In questo esempio, .NET è usato per implementare il mapper e il reducer per una soluzione di conteggio parole.

## <a name="net-on-hdinsight"></a>.NET su HDInsight

Per eseguire applicazioni .NET, i cluster __HDInsight basati su Linux__ usano [Mono (https://mono-project.com)](https://mono-project.com)). La versione Mono 4.2.1 è inclusa nella versione 3.6 di HDInsight. Per altre informazioni sulla versione Mono compresa in HDInsight, vedere [Componenti e versioni di Hadoop disponibili in HDInsight](../hdinsight-component-versioning.md). 

Per altre informazioni sulla compatibilità Mono con le versioni di .NET Framework, vedere il documento relativo alla [compatibilità Mono](https://www.mono-project.com/docs/about-mono/compatibility/).

## <a name="how-hadoop-streaming-works"></a>Come funziona lo streaming di Hadoop

Il processo di base usato per il flusso in questo documento è il seguente:

1. Hadoop passa i dati al mapper (mapper.exe in questo esempio) su STDIN.
2. Il mapper elabora i dati ed emette una coppia chiave/valore delimitata da tabulazione su STDOUT.
3. L'output viene letto da Hadoop e quindi passato al riduttore (reducer.exe in questo esempio) su STDIN.
4. Il riduttore legge le coppie chiave/valore delimitate da tabulazioni, elabora i dati e quindi genera il risultato come coppie chiave/valore delimitate da tabulazione su STDOUT.
5. L'output viene letto da Hadoop e scritto nella directory di output.

Per altre informazioni sullo streaming, vedere [Hadoop Streaming](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).

## <a name="prerequisites"></a>Prerequisiti

* Una familiarità nello scrivere e nel compilare il codice C# destinato a .NET Framework 4.5. Nella procedura di questo documento viene usato Visual Studio 2017.

* Un modo per caricare i file .exe sul cluster. La procedura in questo documento usa gli strumenti Data Lake per Visual Studio per caricare i file nell'archiviazione primaria per il cluster.

* Azure PowerShell o un client SSH.

* Un cluster Hadoop in HDInsight. Per altre informazioni sulla creazione di un cluster, vedere [Creare cluster Hadoop in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).

## <a name="create-the-mapper"></a>Creare il mapper

In Visual Studio creare una nuova __applicazione console__ denominata __mapper__. Usare il codice seguente per l'applicazione:

```csharp
using System;
using System.Text.RegularExpressions;

namespace mapper
{
    class Program
    {
        static void Main(string[] args)
        {
            string line;
            //Hadoop passes data to the mapper on STDIN
            while((line = Console.ReadLine()) != null)
            {
                // We only want words, so strip out punctuation, numbers, etc.
                var onlyText = Regex.Replace(line, @"\.|;|:|,|[0-9]|'", "");
                // Split at whitespace.
                var words = Regex.Matches(onlyText, @"[\w]+");
                // Loop over the words
                foreach(var word in words)
                {
                    //Emit tab-delimited key/value pairs.
                    //In this case, a word and a count of 1.
                    Console.WriteLine("{0}\t1",word);
                }
            }
        }
    }
}
```

Dopo aver creato l'applicazione, compilarla per produrre il file `/bin/Debug/mapper.exe` nella directory del progetto.

## <a name="create-the-reducer"></a>Creare il reducer

In Visual Studio creare una nuova __applicazione console__ denominata __reducer__. Usare il codice seguente per l'applicazione:

```csharp
using System;
using System.Collections.Generic;

namespace reducer
{
    class Program
    {
        static void Main(string[] args)
        {
            //Dictionary for holding a count of words
            Dictionary<string, int> words = new Dictionary<string, int>();

            string line;
            //Read from STDIN
            while ((line = Console.ReadLine()) != null)
            {
                // Data from Hadoop is tab-delimited key/value pairs
                var sArr = line.Split('\t');
                // Get the word
                string word = sArr[0];
                // Get the count
                int count = Convert.ToInt32(sArr[1]);

                //Do we already have a count for the word?
                if(words.ContainsKey(word))
                {
                    //If so, increment the count
                    words[word] += count;
                } else
                {
                    //Add the key to the collection
                    words.Add(word, count);
                }
            }
            //Finally, emit each word and count
            foreach (var word in words)
            {
                //Emit tab-delimited key/value pairs.
                //In this case, a word and a count of 1.
                Console.WriteLine("{0}\t{1}", word.Key, word.Value);
            }
        }
    }
}
```

Dopo aver creato l'applicazione, compilarla per produrre il file `/bin/Debug/reducer.exe` nella directory del progetto.

## <a name="upload-to-storage"></a>Caricare nella risorsa di archiviazione

1. In Visual Studio aprire **Esplora server**.

2. Espandere **Azure** e quindi **HDInsight**.

3. Se richiesto, immettere le credenziali della sottoscrizione di Azure, quindi fare clic su **Accedi**.

4. Espandere il cluster HDInsight in cui si desidera distribuire l'applicazione. Viene elencata una voce con il testo __(Account di archiviazione predefinito)__ .

    ![Esplora server con account di archiviazione per il cluster](./media/apache-hadoop-dotnet-csharp-mapreduce-streaming/hdinsight-storage-account.png)

    * Se è possibile espandere questa voce, si usa un __Account di archiviazione di Azure__ come risorsa di archiviazione predefinita per il cluster. Per visualizzare i file nel percorso di archiviazione predefinito per il cluster, espandere la voce e quindi fare doppio clic su __(Contenitore predefinito)__ .

    * Se non è possibile espandere questa voce, si usa __Azure Data Lake Storage__ come archivio predefinito per il cluster. Per visualizzare i file nel percorso di archiviazione predefinito per il cluster, fare doppio clic sulla voce __(Account di archiviazione predefinito)__ .

5. Per caricare i file con estensione .exe, usare uno dei metodi seguenti:

   * Se si usa un __Account di Archiviazione di Azure__, fare clic sull'icona per il caricamento, quindi passare alla cartella **bin\debug** per il progetto **mapper**. Selezionare infine il file **mapper.exe** e fare clic su **Ok**.

        ![Icona di caricamento HDInsight per Mapper](./media/apache-hadoop-dotnet-csharp-mapreduce-streaming/hdinsight-upload-icon.png)
    
   * Se si usa __Azure Data Lake Storage__, fare clic con il pulsante destro del mouse su un'area vuota nell'elenco di file e quindi scegliere __Carica__. Selezionare infine il file **mapper.exe** e fare clic su **Apri**.

     Una volta terminato il caricamento __mapper.exe__, ripetere il processo di caricamento per il file __reducer.exe__.

## <a name="run-a-job-using-an-ssh-session"></a>Eseguire un processo: Uso di una sessione SSH

1. Connettersi al cluster HDInsight usando SSH. Per altre informazioni, vedere [Usare SSH con HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Usare uno dei comandi seguenti per avviare il processo MapReduce:

   * Se si usa __Azure Data Lake Storage Gen2__ come risorsa di archiviazione predefinita:

       ```bash
       yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files abfs:///mapper.exe,abfs:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
       ```

   * Se si usa __Azure Data Lake Storage Gen1__ come risorsa di archiviazione predefinita:

       ```bash
       yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files adl:///mapper.exe,adl:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
       ```
    
   * Se si usa __Archiviazione di Azure__ come risorsa di archiviazione predefinita:

       ```bash
       yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files wasb:///mapper.exe,wasb:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
       ```

     L'elenco seguente descrive le operazioni eseguite da ogni parametro:

   * `hadoop-streaming.jar`: Il file con estensione jar che contiene la funzionalità di streaming MapReduce.
   * `-files`: Aggiunge i file `mapper.exe` e `reducer.exe` a questo processo. `abfs:///`, `adl:///` o `wasb:///` prima di ogni file rappresenta il percorso della radice di archiviazione predefinita per il cluster.
   * `-mapper`: Specifica il file che implementa il mapper.
   * `-reducer`: Specifica il file che implementa il reducer.
   * `-input`: I dati di input.
   * `-output`: La directory di output.

3. Dopo il completamento del processo di MapReduce, usare il comando seguente per visualizzare i risultati:

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    L'elenco seguente è un esempio dei dati restituiti da questo comando:

        you     1128
        young   38
        younger 1
        youngest        1
        your    338
        yours   4
        yourself        34
        yourselves      3
        youth   17

## <a name="run-a-job-using-powershell"></a>Eseguire un processo: Tramite PowerShell

Usare il seguente script di PowerShell per eseguire un processo MapReduce e scaricare i risultati.

[!code-powershell[main](../../../powershell_scripts/hdinsight/use-csharp-mapreduce/use-csharp-mapreduce.ps1?range=5-87)]

Questo script richiede l'account di accesso del cluster e la password, insieme al nome del cluster HDInsight. Al termine del processo, l'output viene scaricato in un file denominato `output.txt`. Il testo seguente è un esempio dei dati nel file `output.txt`:

    you     1128
    young   38
    younger 1
    youngest        1
    your    338
    yours   4
    yourself        34
    yourselves      3
    youth   17

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sull'uso di MapReduce con HDInsight, vedere l'articolo [Usare MapReduce in Hadoop su HDInsight](hdinsight-use-mapreduce.md).

Per informazioni sull'uso di C# con Hive e Pig, vedere [Usare le funzioni definite dall'utente C# con Apache Hive e Apache Pig](apache-hadoop-hive-pig-udf-dotnet-csharp.md).

Per informazioni sull'uso di C# con Storm in HDInsight, vedere [Sviluppare topologie C# per Apache Storm in HDInsight](../storm/apache-storm-develop-csharp-visual-studio-topology.md).
