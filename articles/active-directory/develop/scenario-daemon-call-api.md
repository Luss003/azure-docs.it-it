---
title: Chiamare un'API Web da un'app daemon-piattaforma di identità Microsoft | Azure
description: Informazioni su come creare un'app daemon che chiama API Web
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 5a234e0b5d3a88b722257aa80249f4d80182d12f
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/23/2020
ms.locfileid: "76702233"
---
# <a name="daemon-app-that-calls-web-apis---call-a-web-api-from-the-app"></a>App daemon che chiama le API Web: chiama un'API Web dall'app

Un'app daemon può chiamare un'API Web da un'applicazione daemon .NET o chiamare diverse API Web pre-approvate.

## <a name="calling-a-web-api-daemon-application"></a>Chiamata di un'applicazione daemon API Web

Ecco come usare il token per chiamare un'API:

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

# <a name="pythontabpython"></a>[Python](#tab/python)

```Python
endpoint = "url to the API"
http_headers = {'Authorization': 'Bearer ' + result['access_token'],
                'Accept': 'application/json',
                'Content-Type': 'application/json'}
data = requests.get(endpoint, headers=http_headers, stream=False).json()
```

# <a name="javatabjava"></a>[Java](#tab/java)

```Java
HttpURLConnection conn = (HttpURLConnection) url.openConnection();

// Set the appropriate header fields in the request header.
conn.setRequestProperty("Authorization", "Bearer " + accessToken);
conn.setRequestProperty("Accept", "application/json");

String response = HttpClientHelper.getResponseStringFromConn(conn);

int responseCode = conn.getResponseCode();
if(responseCode != HttpURLConnection.HTTP_OK) {
    throw new IOException(response);
}

JSONObject responseObject = HttpClientHelper.processResponse(responseCode, response);
```

---

## <a name="calling-several-apis"></a>Chiamata di diverse API

Per le app daemon, le API Web chiamate devono essere pre-approvate. Non ci sarà alcun consenso incrementale con le app daemon (non c'è alcuna interazione con l'utente). L'amministratore del tenant deve pre-concedere il consenso dell'applicazione e di tutte le autorizzazioni dell'API. Se si vogliono chiamare diverse API, è necessario acquisire un token per ogni risorsa, ogni volta che si chiama `AcquireTokenForClient`. MSAL utilizzerà la cache del token dell'applicazione per evitare chiamate al servizio non necessarie.

## <a name="next-steps"></a>Passaggi successivi

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

> [!div class="nextstepaction"]
> [App daemon-sposta in produzione](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-production?tabs=dotnet)

# <a name="pythontabpython"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [App daemon-sposta in produzione](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-production?tabs=python)

# <a name="javatabjava"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [App daemon-sposta in produzione](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-production?tabs=java)

---
