---
title: Usare Azure Machine Learning Services in Azure Notebooks
description: Panoramica dei notebook di esempio per Azure Machine Learning Services che è possibile usare con Azure Notebooks.
services: app-service
documentationcenter: ''
author: kraigb
manager: barbkess
ms.assetid: 0dc4fc31-ae1c-422c-ac34-7b025e6651b4
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: f591758fa6e51c420a090aa62d5160320fe15fe8
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/05/2019
ms.locfileid: "71973023"
---
# <a name="use-azure-machine-learning-service-in-a-notebook"></a>Usare il servizio Azure Machine Learning in un notebook

Azure Notebooks è preconfigurato con l'ambiente necessario per interagire con il [servizio Azure Machine Learning](/azure/machine-learning/service/). È possibile clonare facilmente un progetto di esempio nell'account di Notebooks per esplorare un'ampia gamma di scenari di Machine Learning.

## <a name="clone-the-sample-into-your-account"></a>Clonare l'esempio nell'account personale

1. Accedere ad [Azure Notebooks](https://notebooks.azure.com/).
1. Selezionare **progetti personali** per passare al dashboard progetti.
1. Selezionare il pulsante **carica repository GitHub** (freccia su) per aprire il popup **caricare il repository GitHub** .
1. Nella finestra popup immettere `Azure/MachineLearningNotebooks` in **GitHub repository** (Repository di GitHub), specificare un nome per il progetto in **Project Name** (Nome progetto), ad esempio "Azure Machine Learning service", specificare un identificatore in **Project ID** (ID progetto), deselezionare la casella **Public** (Pubblico), se si vuole, e quindi selezionare **Import** (Importa).

    ![Importare un esempio di Azure Machine Learning Notebook nell'account di Notebooks](media/azureml-import-project.png)

1. Dopo un minuto o due, Azure Notebooks aprirà automaticamente il dashboard del nuovo progetto.

## <a name="run-a-sample-notebook"></a>Eseguire un notebook di esempio

1. Selezionare **00 - configuration.ipynb** per avviare la sezione di configurazione del notebook e seguire le istruzioni per creare un'area di lavoro di Azure Machine Learning.

    - Poiché Azure Notebooks contiene già i pacchetti Python necessari, è sufficiente eseguire il frammento di codice disponibile nel passaggio 2 dei prerequisiti per verificare la versione dell'SDK di Azure Machine Learning.

1. Al termine della configurazione, selezionare **01. Getting-Started** per aprire la cartella contenente tredici diversi notebook di esempio, ciascuno dei quali è di facile comprensione.

## <a name="next-steps"></a>Passaggi successivi

Nella documentazione di Azure Machine Learning Services sono disponibili molte altre risorse che illustrano l'utilizzo di Machine Learning Service con i notebook:

- [Avvio rapido: Iniziare a usare Azure Machine Learning con Python](https://docs.microsoft.com/azure/machine-learning/service/quickstart-create-workspace-with-python)
- [Esercitazione n. 1: Eseguire il training di un modello di classificazione delle immagini con il servizio Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/tutorial-train-models-with-aml)
- [Esercitazione n. 2: Distribuire un modello di classificazione di immagini nell'istanza di contenitore di Azure](https://docs.microsoft.com/azure/machine-learning/service/tutorial-deploy-models-with-aml)
- [Esercitazione: Eseguire il training di un modello di classificazione con apprendimento automatico nel servizio Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/tutorial-auto-train-models)

Vedere anche la documentazione relativa ad [Azure Machine Learning SDK per Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).
