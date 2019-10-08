---
title: 'Avvio rapido: Creare app - LUIS'
titleSuffix: Azure Cognitive Services
description: Creare un'app LUIS che usa il dominio predefinito `HomeAutomation` per accendere e spegnere luci ed elettrodomestici. Questo dominio predefinito fornisce all'utente finalità, entità ed espressioni di esempio. Al termine, sarà disponibile un endpoint LUIS in esecuzione nel cloud.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 748c51e74db20ac101dc2dff0d924567acded114
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703229"
---
# <a name="quickstart-use-prebuilt-home-automation-app"></a>Avvio rapido: Usare l'app di domotica predefinita

In questa guida introduttiva si crea un'app LUIS che usa il dominio predefinito `HomeAutomation` per accendere e spegnere luci ed elettrodomestici. Questo dominio predefinito fornisce all'utente finalità, entità ed espressioni di esempio. Al termine, sarà disponibile un endpoint LUIS in esecuzione nel cloud.

## <a name="prerequisites"></a>Prerequisiti

Per questo articolo è necessario un account LUIS gratuito, creato nel portale LUIS all'indirizzo [https://www.luis.ai](https://www.luis.ai). 

[!INCLUDE [Sign in to LUIS](./includes/sign-in-process.md)]

## <a name="create-a-new-app"></a>Creare una nuova app
È possibile creare e gestire le applicazioni in **My Apps** (App personali). 

1. Selezionare **Create new app** (Crea nuova app).

    [![Screenshot dell'elenco di app](media/luis-quickstart-new-app/app-list.png "Screenshot dell'elenco di app")](media/luis-quickstart-new-app/app-list.png)

1. Nella finestra di dialogo assegnare all'applicazione il nome "Home Automation".

    [![Screenshot della finestra di dialogo popup Crea nuova app](media/luis-quickstart-new-app/create-new-app-dialog.png "Screenshot della finestra di dialogo popup Crea nuova app")](media/luis-quickstart-new-app/create-new-app-dialog.png)

1. Scegliere le impostazioni cultura dell'applicazione. Per questa app Home Automation scegliere English (Inglese). Al termine selezionare **Done** (Fine). Viene creata l'app Home Automation. 

    >[!NOTE]
    >Le impostazioni cultura non possono essere modificate dopo la creazione dell'applicazione. 

## <a name="add-prebuilt-domain"></a>Aggiungi dominio predefinito

Selezionare **Prebuilt domains** (Domini predefiniti) nel riquadro di spostamento a sinistra e quindi cercare "Home". Selezionare **Add domain** (Aggiungi dominio).

[![Screenshot del dominio di domotica chiamato nel menu Prebuilt domains (Domini predefiniti)](media/luis-quickstart-new-app/home-automation.png "Screenshot del dominio di domotica chiamato nel menu Prebuilt domains (Domini predefiniti)")](media/luis-quickstart-new-app/home-automation.png)

Dopo che il dominio è stato aggiunto, nella casella del dominio predefinito viene visualizzato un pulsante **Remove domain** (Rimuovi dominio).

[![Screenshot del dominio di domotica con il pulsante di rimozione](media/luis-quickstart-new-app/remove-domain.png "Screenshot del dominio di domotica con il pulsante di rimozione")](media/luis-quickstart-new-app/remove-domain.png)

## <a name="intents-and-entities"></a>Finalità ed entità

Selezionare **Intents** (Finalità) nel riquadro di spostamento a sinistra per esaminare le finalità del dominio HomeAutomation. Per ogni finalità sono definite espressioni di esempio.

![Elenco di screenshot della finalità HomeAutomation](media/luis-quickstart-new-app/home-automation-intents.png "Elenco di screenshot della finalità HomeAutomation")]

> [!NOTE]
> **None** (Nessuna) è una finalità fornita da tutte le app LUIS. Viene usata per gestire le espressioni che non corrispondono alle funzionalità offerte dall'app. 

Selezionare la finalità **HomeAutomation.TurnOff**. È possibile vedere che la finalità contiene un elenco di espressioni con etichette di entità.

[![Screenshot della finalità HomeAutomation.TurnOff](media/luis-quickstart-new-app/home-automation-turnoff.png "Screenshot della finalità HomeAutomation.TurnOff")](media/luis-quickstart-new-app/home-automation-turnoff.png)

## <a name="train-the-luis-app"></a>Eseguire il training dell'app di Language Understanding

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="test-your-app"></a>Test dell'app
Dopo aver eseguito il training dell'app, è possibile eseguirne il test. Selezionare **Test** nella barra di spostamento superiore. Digitare un'espressione di test, ad esempio "Turn off the lights" nel riquadro di test interattivo e premere INVIO. 

```
Turn off the lights
```

Verificare che la finalità con il punteggio più elevato corrisponda a quella prevista per ogni espressione di test.

In questo esempio `Turn off the lights` viene correttamente identificata come la finalità con il punteggio più elevato per **HomeAutomation.TurnOff**.

[![Screenshot del pannello Test con le espressioni evidenziate](media/luis-quickstart-new-app/test.png "Screenshot del pannello Test con le espressioni evidenziate")](media/luis-quickstart-new-app/test.png)


Selezionare **Inspect** (Esamina) per esaminare altre informazioni sulla previsione.

![Screenshot del pannello Test con l'espressione evidenziata](media/luis-quickstart-new-app/review-test-inspection-pane-in-portal.png)

Selezionare nuovamente **Test** per comprimere il riquadro di test. 

<a name="publish-your-app"></a>

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Pubblicare l'app per ottenere l'URL endpoint

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-v2-api-prediction-endpoint"></a>Eseguire le query sull'endpoint di previsione dell'API V2

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

1. Andare alla fine dell'URL nell'indirizzo e immettere `turn off the living room light`, quindi premere Invio. 

    #### <a name="v2-prediction-endpointtabv2"></a>[Endpoint di previsione V2](#tab/V2)

    `https://<region>.api.cognitive.microsoft.com/luis/**v2.0**/apps/<appID>?subscription-key=<YOUR_KEY>&**q=<user-utterance-text>**`

    Il browser mostra la versione dell'**API V2** della risposta JSON dell'endpoint HTTP.

    ```json
    {
      "query": "turn off the lights",
      "topScoringIntent": {
        "intent": "HomeAutomation.TurnOff",
        "score": 0.995867
      },
      "entities": [
        {
          "entity": "lights",
          "type": "HomeAutomation.DeviceType",
          "startIndex": 13,
          "endIndex": 18,
          "resolution": {
            "values": [
              "light"
            ]
          }
        }
      ]
    }
    ```
    
    #### <a name="v3-prediction-endpointtabv3"></a>[Endpoint di previsione V3](#tab/V3)

    Per una [query dell'API V3](luis-migration-api-v3.md), nel browser modificare la richiesta HTTPS del metodo GET, sostituendo i valori tra parentesi acute con valori personalizzati.     

    `https://<region>.api.cognitive.microsoft.com/luis/**v3.0-preview**/apps/<appID>/**slots**/**production**/**predict**?subscription-key=<YOUR_KEY>&**query=<user-utterance-text>**`

    ```json
    {
        "query": "turn off the lights",
        "prediction": {
            "normalizedQuery": "turn off the lights",
            "topIntent": "HomeAutomation.TurnOff",
            "intents": {
                "HomeAutomation.TurnOff": {
                    "score": 0.99649024
                }
            },
            "entities": {
                "HomeAutomation.DeviceType": [
                    [
                        "light"
                    ]
                ]
            }
        }
    }
    ```


    Altre informazioni sull'[endpoint di previsione V3](luis-migration-api-v3.md).
    
    * * * 

## <a name="clean-up-resources"></a>Pulire le risorse

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Passaggi successivi

È possibile chiamare l'endpoint dal codice:

> [!div class="nextstepaction"]
> [Chiamare un endpoint LUIS tramite codice](luis-get-started-cs-get-intent.md)
