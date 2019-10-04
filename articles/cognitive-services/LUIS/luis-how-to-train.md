---
title: Train app-LUIS
titleSuffix: Azure Cognitive Services
description: Il training è il processo di appredimento per la propria versione applicazione Language Understanding (LUIS) al fine di migliorare la comprensione del linguaggio naturale. Eseguire il training dell'app LUIS dopo gli aggiornamenti al modello, ad esempio aggiunta, modifica, aggiunta di etichette o eliminazione di entità, finalità o espressioni.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: b3841c9d60cf275e423024fc66c15582f95c0a10
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/09/2019
ms.locfileid: "68932764"
---
# <a name="train-your-active-version-of-the-luis-app"></a>Eseguire il training della versione attiva dell'app LUIS 

Il training è il processo di apprendimento per l'app Language Understanding (LUIS) al fine di migliorare la comprensione del linguaggio naturale. Eseguire il training dell'app LUIS dopo gli aggiornamenti al modello, ad esempio aggiunta, modifica, aggiunta di etichette o eliminazione di entità, finalità o espressioni. 

<!--
When you train a LUIS app by example, LUIS generalizes from the examples you have labeled, and it learns to recognize the relevant intents and entities. This teaches LUIS to improve classification accuracy in the future. -->

Il training e il [test](luis-concept-test.md) di un'app costituiscono un processo iterativo. Dopo aver eseguito il training dell'app LUIS, testarla con espressioni di esempio per verificare se le finalità e le entità vengono riconosciute correttamente. Se così non fosse, aggiornare l'app LUIS ed eseguire nuovamente il training e il test. 

Il training viene applicato alla versione attiva nel portale LUIS. 

## <a name="how-to-train-interactively"></a>Come eseguire il training in modo interattivo

Per avviare il processo iterativo nel [portale LUIS](https://www.luis.ai), è prima necessario eseguire il training dell'app LUIS almeno una volta. Prima del training, assicurarsi che ogni finalità abbia almeno un'espressione.

1. Accedere all'app selezionando il relativo nome nella pagina **App personali**. 

2. Nell'app selezionare **Train** (Esegui training) nel riquadro superiore. 

3. Una volta completato il training, viene visualizzata una barra di notifica verde nella parte superiore del browser.

<!-- The following note refers to what might cause the error message "Training failed: FewLabels for model: <ModelName>" -->

>[!NOTE]
>Se nell'app ci sono una o più finalità che non includono espressioni di esempio, non è possibile eseguire il training. Aggiungere espressioni per tutte le finalità. Per altre informazioni, vedere [Add example utterances](luis-how-to-add-example-utterances.md) (Aggiungere espressioni di esempio).

## <a name="training-date-and-time"></a>Data e ora di training

La data e l'ora di training sono GMT + 2. 

## <a name="train-with-all-data"></a>Eseguire il training con tutti i dati

Il training usa una piccola percentuale di campionamento negativo. Per usare tutti i dati invece del campionamento negativo ridotto, usare l'[API delle impostazioni della versione](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) con `UseAllTrainingData` impostato su true per disattivare questa funzionalità. 

## <a name="unnecessary-training"></a>Training non necessario

Non è necessario eseguire il training dopo ogni singola modifica. È opportuno eseguire il training dopo l'applicazione di un gruppo di modifiche al modello, quando il passaggio da eseguire successivamente è il test o la pubblicazione. Se non è necessario testare o pubblicare il modello, il training non è necessario. 

## <a name="training-with-the-rest-apis"></a>Training con le API REST

Il training nel portale LUIS consiste nell'eseguire un unico passaggio, ovvero fare clic sul pulsante **Train** (Esegui training). Il training con le API REST è invece un processo in due passaggi. Il primo passaggio consiste nel [richiedere il training](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) con HTTP POST. Il secondo consiste invece nel richiedere lo [stato del training](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46) con HTTP Get. 

Per sapere quando il training è completo, è necessario eseguire il polling dello stato finché il training non risulta completato per tutti i modelli. 

## <a name="next-steps"></a>Passaggi successivi

* [Etichettare le espressioni suggerite con LUIS](luis-how-to-review-endpoint-utterances.md) 
* [Usare le funzionalità per migliorare le prestazioni dell'app LUIS](luis-how-to-add-features.md) 
