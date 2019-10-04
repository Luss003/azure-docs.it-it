---
title: Che cosa sono le pipeline di ML
titleSuffix: Azure Machine Learning
description: In questo articolo viene illustrata la pipeline di machine learning che è possibile compilare con SDK di Azure Machine Learning per Python e i vantaggi dell'uso di pipeline. Le pipeline di Machine learning (ML) vengono utilizzate dai data scientist per creare, ottimizzare e gestire i flussi di lavoro di apprendimento automatico.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: sanpil
author: sanpil
ms.date: 08/08/2019
ms.custom: seodec18
ms.openlocfilehash: 07efde7c3664ba1866e59f23c31b9c385ed9c366
ms.sourcegitcommit: 0fab4c4f2940e4c7b2ac5a93fcc52d2d5f7ff367
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/17/2019
ms.locfileid: "71035499"
---
# <a name="what-are-ml-pipelines-in-azure-machine-learning"></a>Che cosa sono le pipeline di ML in Azure Machine Learning?

Informazioni sulle pipeline di Machine Learning che è possibile creare e gestire con Azure Machine Learning. 

Tramite le pipeline di machine learning (ML), i data scientist, i data engineer e i professionisti IT possono collaborare alle fasi di:
+ Preparazione dei dati, ad esempio normalizzazioni e trasformazioni
+ Training dei modelli
+ Valutazione del modello.
+ Distribuzione

Informazioni su come [creare la prima pipeline](how-to-create-your-first-pipeline.md).

![Pipeline di Machine Learning in Azure Machine Learning](./media/concept-ml-pipelines/pipeline-flow.png)

<a name="compare"></a>
### <a name="which-azure-pipeline-technology-should-i-use"></a>Quale tecnologia della pipeline di Azure è consigliabile usare?

Il cloud di Azure fornisce diverse altre pipeline, ognuna con uno scopo diverso. Nella tabella seguente sono elencate le diverse pipeline e le relative operazioni di utilizzo:

| Pipeline | Descrizione | Pipe canonica |
| ---- | ---- | ---- |
| Pipeline di Azure Machine Learning | Definisce flussi di lavoro di Machine Learning riutilizzabili che possono essere usati come modello per gli scenari di apprendimento automatico. | Modello di > dati |
| [Pipeline di Data Factory di Azure](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) | Raggruppa le attività di spostamento, trasformazione e controllo dei dati necessarie per eseguire un'attività.  | Dati > dati |
| [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/) | Integrazione e distribuzione continue dell'applicazione in qualsiasi piattaforma o cloud  | App/servizio > di codice |

## <a name="why-build-pipelines-with-azure-machine-learning"></a>Perché creare delle pipeline con Azure Machine Learning?

Le pipeline di Machine Learning ottimizzano il flusso di lavoro con velocità, portabilità e riutilizzo per potersi concentrare sull'esperienza, sull'apprendimento automatico, anziché sull'infrastruttura e l'automazione.

Le pipeline sono costituite da più **passaggi**, ovvero unità di calcolo distinte nella pipeline. Ogni passaggio può essere eseguito in modo indipendente e usare risorse di calcolo isolate.
I passaggi indipendenti consentono a più data scientist di lavorare contemporaneamente sulla stessa pipeline senza sovraccaricare le risorse di calcolo e di usare diversi tipi/dimensioni di calcolo per ogni passaggio.

Dopo che la pipeline è stata progettata, vi è spesso un'ulteriore ottimizzazione di tutto il ciclo di training della pipeline. Quando si esegue nuovamente una pipeline, l'esecuzione passa ai passaggi distinti che devono essere rieseguiti, ad esempio uno script di training aggiornato, e ignora ciò che non è stato modificato. Lo stesso paradigma si applica agli script invariati usati per l'esecuzione del passaggio. Questa funzionalità di riutilizzo consente di evitare l'esecuzione di passaggi costosi e con tempi intensivi come l'inserimento e la trasformazione dei dati se i dati sottostanti non sono stati modificati.

Con Azure Machine Learning, è possibile usare diversi Toolkit e Framework, ad esempio PyTorch o TensorFlow, per ogni passaggio della pipeline. Azure coordina le varie [destinazioni di calcolo](concept-azure-machine-learning-architecture.md) usate in modo che i dati intermedi possano essere condivisi semplicemente con le destinazioni di calcolo downstream.

È possibile [tenere traccia della metrica per gli esperimenti della pipeline](https://docs.microsoft.com/azure/machine-learning/service/how-to-track-experiments) direttamente in portale di Azure o nella [pagina di destinazione dell'area di lavoro (anteprima)](https://ml.azure.com). Dopo la pubblicazione di una pipeline, è possibile configurare un endpoint REST, che consente di eseguire di nuovo la pipeline da qualsiasi piattaforma o stack.

## <a name="key-advantages"></a>Vantaggi principali

I vantaggi principali dell'uso delle pipeline per i flussi di lavoro di Machine Learning sono:

|Vantaggi principali|Descrizione|
|:-------:|-----------|
|**Esecuzione&nbsp;automatica**|Pianificare i passaggi da eseguire in parallelo o in sequenza in modo affidabile e automatico. La preparazione e la modellazione dei dati possono durare giorni o settimane e le pipeline consentono di concentrarsi su altre attività durante l'esecuzione del processo. |
|**Calcolo eterogeneo**|Usare più pipeline che vengono coordinate in modo affidabile tra le risorse di calcolo eterogenee e scalabili e i percorsi di archiviazione. Usare in modo efficiente le risorse di calcolo disponibili eseguendo singoli passaggi della pipeline su destinazioni di calcolo diverse, ad esempio HDInsight, VM di Data Science GPU e databricks.|
|**Riutilizzo**|Creare modelli di pipeline per scenari specifici, ad esempio la ripetizione del training e l'assegnazione dei punteggi di batch. Attivare pipeline pubblicate da sistemi esterni tramite semplici chiamate REST.|
|**Rilevamento e controllo delle versioni**|Invece di tenere traccia manualmente dei dati e dei percorsi dei risultati durante l'iterazione, usare l'SDK delle pipeline per denominare esplicitamente e controllare la versione di origini dati, input e output. È anche possibile gestire gli script e i dati separatamente per una maggiore produttività.|
|**Collaborazione**|Le pipeline consentono ai data scientist di collaborare in tutte le aree del processo di progettazione di Machine Learning, pur potendo lavorare simultaneamente sui passaggi della pipeline.|

## <a name="the-python-sdk-for-pipelines"></a>Python SDK per le pipeline

[USA Python SDK](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) per creare le tue pipeline di ml nei tuoi Integrated Development Environment preferiti (IDE) o nei notebook di Jupyter. SDK di Azure Machine Learning offre costrutti imperativi per la sequenziazione e l'abbinamento dei passaggi nelle pipeline quando non è presente alcuna dipendenza dei dati. 

Usando le dipendenze dei dati dichiarativi, è possibile ottimizzare le attività. L'SDK include un framework di moduli precompilati per attività comuni, come il trasferimento dei dati e la pubblicazione dei modelli. È possibile estendere il Framework per modellare le convenzioni implementando passaggi personalizzati riutilizzabili tra le pipeline. Anche le destinazioni di calcolo e le risorse di archiviazione possono essere gestite direttamente dall'SDK.

Salvare le pipeline come modelli e distribuirle in un endpoint REST per i processi di assegnazione dei punteggi di batch o di ripetizione del training.

Sono disponibili due pacchetti Python per le pipeline con Azure Machine Learning: [azureml-Pipelines-Core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) e [azureml-pipeline-Steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py). Per iniziare rapidamente, usare uno dei moduli predefiniti, ad esempio:

* Esecuzione di script Python in un passaggio con [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep)
* Trasferimento dei dati tra le opzioni di archiviazione con [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.datatransferstep)
* Creazione di un passaggio della pipeline AutoML con [AutoMLStep](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlstep)

## <a name="next-steps"></a>Passaggi successivi

+ Informazioni su come [creare la prima pipeline](how-to-create-your-first-pipeline.md).

+ Informazioni su come [eseguire stime batch su dati di grandi dimensioni](tutorial-pipeline-batch-scoring-classification.md).

+ Vedere la [documentazione di riferimento dell'SDK per le pipeline](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py).

+ Provare ad esempio Jupyter Notebooks che illustrano [pipeline Azure Machine Learning](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines). Informazioni su come [eseguire i notebook per esplorare questo servizio](samples-notebooks.md).
