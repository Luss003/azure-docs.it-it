---
title: "Avvio rapido: Chiamare l'endpoint di Ricerca personalizzata Bing usando C# SDK"
titleSuffix: Azure Cognitive Services
description: Usare questo argomento di avvio rapido per iniziare a richiedere risultati della ricerca dall'istanza di Ricerca personalizzata Bing con C# SDK.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 12/09/2019
ms.author: scottwhi
ms.openlocfilehash: 5e932b1ff597fc0170a6bb83841bc6ca7306693a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75448724"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-the-c-sdk"></a>Avvio rapido: Chiamare l'endpoint di Ricerca personalizzata Bing usando C# SDK 

Usare questo argomento di avvio rapido per iniziare a richiedere risultati della ricerca dall'istanza di Ricerca personalizzata Bing con C# SDK. Benché Ricerca personalizzata Bing includa un'API REST compatibile con la maggior parte dei linguaggi di programmazione, l'SDK offre un semplice strumento per integrare il servizio nelle applicazioni. Il codice sorgente per questo esempio è disponibile su [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingCustomWebSearch).

## <a name="prerequisites"></a>Prerequisites

- Un'istanza di Ricerca personalizzata Bing. Vedere [Avvio rapido: Creare la prima istanza di Ricerca personalizzata Bing](quick-start.md) per altre informazioni.
- Microsoft [.NET Core](https://www.microsoft.com/net/download/core)
- Qualsiasi edizione di [Visual Studio 2017 o versioni successive](https://www.visualstudio.com/downloads/)
- Se si usa Linux/MacOS, questa applicazione può essere eseguita tramite [Mono](https://www.mono-project.com/).
- Il pacchetto NuGet [Ricerca personalizzata Bing](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0). 
    - Da **Esplora soluzioni** in Visual Studio fare clic con il pulsante destro del mouse sul progetto e scegliere **Gestisci pacchetti NuGet** dal menu. Installare il pacchetto `Microsoft.Azure.CognitiveServices.Search.CustomSearch`. L'installazione del pacchetto Ricerca personalizzata NuGet comporta anche l'installazione degli assembly seguenti:
        - Microsoft.Rest.ClientRuntime
        - Microsoft.Rest.ClientRuntime.Azure
        - Newtonsoft.Json

[!INCLUDE [cognitive-services-bing-news-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Creare e inizializzare l'applicazione

1. Creare una nuova applicazione console C# in Visual Studio. Aggiungere quindi i pacchetti seguenti al progetto.

    ```csharp
    using System;
    using System.Linq;
    using Microsoft.Azure.CognitiveServices.Search.CustomSearch;
    ```

2. Nel metodo main dell'applicazione creare un'istanza del client di ricerca con la chiave API.

    ```csharp
    var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-SUBSCRIPTION-KEY"));
    ```

## <a name="send-the-search-request-and-receive-a-response"></a>Inviare la richiesta di ricerca e ricevere una risposta
    
1. Inviare una query di ricerca usando il metodo `SearchAsync()` del client e salvare la risposta. Assicurarsi di sostituire `YOUR-CUSTOM-CONFIG-ID` con l'ID di configurazione dell'istanza. L'ID è disponibile nel [portale Ricerca personalizzata Bing](https://www.customsearch.ai/). Questo esempio cerca il termine "Xbox".

    ```csharp
    // This will look up a single query (Xbox).
    var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;
    ```

2. Il metodo `SearchAsync()` restituisce un oggetto `WebData`. Usare l'oggetto per eseguire l'iterazione di tutti gli oggetti `WebPages` trovati. Questo codice consente di trovare la prima pagina Web risultante e di stamparne `Name` e `URL`.

    ```csharp
    if (webData?.WebPages?.Value?.Count > 0)
    {
        // find the first web page
        var firstWebPagesResult = webData.WebPages.Value.FirstOrDefault();

        if (firstWebPagesResult != null)
        {
            Console.WriteLine("Number of webpage results {0}", webData.WebPages.Value.Count);
            Console.WriteLine("First web page name: {0} ", firstWebPagesResult.Name);
            Console.WriteLine("First web page URL: {0} ", firstWebPagesResult.Url);
        }
        else
        {
            Console.WriteLine("Couldn't find web results!");
        }
    }
    else
    {
        Console.WriteLine("Didn't see any Web data..");
    }
    ```csharp

## Next steps

> [!div class="nextstepaction"]
> [Build a Custom Search web app](./tutorials/custom-search-web-page.md)
