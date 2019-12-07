---
title: Trasformare i dati usando l'attività hive di Hadoop
description: Informazioni su come usare l'attività Hive in una data factory di Azure per eseguire query Hive in un cluster HDInsight su richiesta o nel proprio cluster HDInsight.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/15/2019
author: nabhishek
ms.author: abnarain
manager: craigg
ms.openlocfilehash: 84c7ff8268934c771a056e41cd2bf484b8674302
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/06/2019
ms.locfileid: "74893825"
---
# <a name="transform-data-using-hadoop-hive-activity-in-azure-data-factory"></a>Trasformare i dati usando l'attività Hive di Hadoop in Azure Data Factory
> [!div class="op_single_selector" title1="Selezionare uSelezionare la versione del servizio di Azure Data Factory in uso:"]
> * [Versione 1](v1/data-factory-hive-activity.md)
> * [Versione corrente](transform-data-using-hadoop-hive.md)

L'attività Hive di HDInsight in una [pipeline](concepts-pipelines-activities.md) di Data Factory esegue query Hive sul [proprio](compute-linked-services.md#azure-hdinsight-linked-service) cluster HDInsight o sul cluster HDInsight [on demand](compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Questo articolo si basa sull'articolo relativo alle [attività di trasformazione dei dati](transform-data.md) che presenta una panoramica generale della trasformazione dei dati e le attività di trasformazione supportate.

Se non si ha familiarità con Azure Data Factory, leggere l'[Introduzione ad Azure Data Factory](introduction.md) ed eseguire l'[Esercitazione: Trasformare i dati](tutorial-transform-data-spark-powershell.md) prima di leggere questo articolo. 

## <a name="syntax"></a>Sintassi

```json
{
    "name": "Hive Activity",
    "description": "description",
    "type": "HDInsightHive",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "scriptLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "scriptPath": "MyAzureStorage\\HiveScripts\\MyHiveSript.hql",
        "getDebugInfo": "Failure",
        "arguments": [
            "SampleHadoopJobArgument1"
        ],
        "defines": {
            "param1": "param1Value"
        }
    }
}
```
## <a name="syntax-details"></a>Dettagli sintassi
| Proprietà            | Description                                                  | Obbligatoria |
| ------------------- | ------------------------------------------------------------ | -------- |
| name                | Nome dell'attività                                         | SÌ      |
| description         | Testo descrittivo per lo scopo dell'attività                | No       |
| type                | Per l'Attività Hive, il tipo di attività è HDinsightHive        | SÌ      |
| linkedServiceName   | Riferimento al cluster HDInsight registrato come servizio collegato in Data Factory. Per informazioni su questo servizio collegato, vedere l'articolo [Servizi collegati di calcolo](compute-linked-services.md). | SÌ      |
| scriptLinkedService | Riferimento a un servizio collegato Archiviazione di Azure usato per memorizzare lo script Hive da eseguire. Se non si specifica questo servizio collegato, viene usato il servizio collegato Archiviazione di Azure definito nel servizio collegato HDInsight. | No       |
| scriptPath          | Specificare il percorso del file di script archiviato nel servizio Archiviazione di Azure indicato da scriptLinkedService. Il nome del file distingue tra maiuscole e minuscole. | SÌ      |
| getDebugInfo        | Specifica quando i file di log vengono copiati nell'Archiviazione di Azure usata dal cluster HDInsight (o) indicata da scriptLinkedService. Valori consentiti: None, Always o Failure. Valore predefinito: None. | No       |
| arguments           | Specifica una matrice di argomenti per un processo Hadoop. Gli argomenti vengono passati a ogni attività come argomenti della riga di comando. | No       |
| defines             | Nei riferimenti all'interno dello script Hive, specificare i parametri come coppie chiave/valore. | No       |
| queryTimeout        | Valore di timeout di query (in minuti). Applicabile se il cluster HDInsight è abilitato per Enterprise Security Package. | No       |

## <a name="next-steps"></a>Passaggi successivi
Vedere gli articoli seguenti che illustrano come trasformare i dati in altri modi: 

* [Attività U-SQL](transform-data-using-data-lake-analytics.md)
* [Attività Pig](transform-data-using-hadoop-pig.md)
* [Attività MapReduce](transform-data-using-hadoop-map-reduce.md)
* [Attività di streaming di Hadoop](transform-data-using-hadoop-streaming.md)
* [Attività Spark](transform-data-using-spark.md)
* [Attività personalizzata .NET](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning Bach Execution Activity](transform-data-using-machine-learning.md) (Attività di esecuzione batch di Machine Learning)
* [Attività stored procedure](transform-data-using-stored-procedure.md)
