---
title: Run Azure Kubernetes Service - Text Analytics
titleSuffix: Azure Cognitive Services
description: Deploy the Text Analytics container image to Azure Kubernetes Service, and test it in a web browser.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: dapine
ms.openlocfilehash: 3264ec5a83277e6bb4befad46cd1337175e911c5
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/22/2019
ms.locfileid: "74383433"
---
# <a name="deploy-a-text-analytics-container-to-azure-kubernetes-service"></a>Deploy a Text Analytics container to Azure Kubernetes Service

Learn how to deploy the Azure Cognitive Services [Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers) container image to Azure Kubernetes Service (AKS). This procedure shows how to create a Text Analytics resource, how to create an associated sentiment analysis image, and how to exercise this orchestration of the two from a browser. Using containers can shift your attention away from managing infrastructure to instead focusing on application development.

## <a name="prerequisites"></a>Prerequisiti

Questa procedura richiede diversi strumenti che devono essere installati ed eseguiti in locale. Don't use Azure Cloud Shell. Sono necessari gli elementi seguenti:

* Una sottoscrizione di Azure. Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/) prima di iniziare.
* A text editor, for example, [Visual Studio Code](https://code.visualstudio.com/download).
* The [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) installed.
* The [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/) installed.
* Una risorsa di Azure con il piano tariffario corretto. Non tutti i piani tariffari funzionano con questo contenitore:
    * **Azure Text Analytics** resource with F0 or standard pricing tiers only.
    * **Azure Cognitive Services** resource with the S0 pricing tier.

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics container on Azure Kubernetes Service (AKS)](../../containers/includes/create-aks-resource.md)]

#### <a name="key-phrase-extractiontabkeyphrase"></a>[Estrazione frasi chiave](#tab/keyphrase)

[!INCLUDE [Key Phrase Extraction Kubernetes config and deploy steps](../includes/key-phrase-extraction-kubernetes-config-deploy.md)]

[!INCLUDE [Verify the Key Phrase Extraction container instance](../includes/verify-key-phrase-extraction-container.md)]

#### <a name="language-detectiontablanguage"></a>[Rilevamento lingua](#tab/language)

[!INCLUDE [Language Detection Kubernetes config and deploy steps](../includes/language-detection-kubernetes-config-deploy.md)]

[!INCLUDE [Verify the Language Detection container instance](../includes/verify-language-detection-container.md)]

#### <a name="sentiment-analysistabsentiment"></a>[Analisi del sentiment](#tab/sentiment)

[!INCLUDE [Sentiment Analysis Kubernetes config and deploy steps](../includes/sentiment-analysis-kubernetes-config-deploy.md)]

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

***

## <a name="next-steps"></a>Passaggi successivi

* Use more [Cognitive Services containers](../../cognitive-services-container-support.md)
* Use the [Text Analytics Connected Service](../vs-text-connected-service.md)
