---
title: Chiamare un'API Web da un'app per dispositivi mobili | Azure
titleSuffix: Microsoft identity platform
description: Informazioni su come creare un'app per dispositivi mobili che chiama API Web (chiamando un'API Web)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: aaddev
ms.openlocfilehash: 6b87809e29940b343a395ffb461c0829295fcd8a
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/23/2020
ms.locfileid: "76702063"
---
# <a name="mobile-app-that-calls-web-apis---call-a-web-api"></a>App per dispositivi mobili che chiama API Web-chiamare un'API Web

Dopo che l'app ha eseguito l'accesso a un utente e ricevuto i token, MSAL espone diverse informazioni sull'utente, l'ambiente dell'utente e i token emessi. L'app può usare questi valori per chiamare un'API Web o visualizzare un messaggio di benvenuto all'utente.

In primo luogo, si esaminerà il risultato di MSAL. Si esaminerà quindi come usare un token di accesso dal `AuthenticationResult` o `result` per chiamare un'API Web protetta.

## <a name="msal-result"></a>Risultato di MSAL
MSAL fornisce i valori seguenti: 

- `AccessToken`: usato per chiamare le API Web protette in una richiesta di portatore HTTP.
- `IdToken`: contiene informazioni utili sull'utente che ha eseguito l'accesso, ad esempio il nome dell'utente, il tenant principale e un identificatore univoco per l'archiviazione.
- `ExpiresOn`: ora di scadenza del token. MSAL gestisce l'aggiornamento automatico per le app.
- `TenantId`: identificatore del tenant con cui l'utente ha eseguito l'accesso. Per gli utenti Guest (Azure Active Directory B2B), questo valore identificherà il tenant con cui l'utente ha eseguito l'accesso, non il tenant principale dell'utente.  
- `Scopes`: gli ambiti concessi con il token. Gli ambiti concessi possono essere un subset degli ambiti richiesti.

MSAL fornisce anche un'astrazione per un `Account`. Una `Account` rappresenta l'account di accesso dell'utente corrente.

- `HomeAccountIdentifier`: identificatore del tenant Home dell'utente.
- `UserName`: il nome utente preferito dell'utente. Questa operazione potrebbe essere vuota per Azure Active Directory B2C utenti.
- `AccountIdentifier`: identificatore dell'utente che ha eseguito l'accesso. Questo valore sarà uguale al valore `HomeAccountIdentifier` nella maggior parte dei casi, a meno che l'utente non sia un guest in un altro tenant.

## <a name="call-an-api"></a>Chiamare un'API

Dopo aver avuto il token di accesso, è facile chiamare un'API Web. L'app userà il token per costruire una richiesta HTTP e quindi eseguirà la richiesta.

### <a name="android"></a>Android

```Java
        RequestQueue queue = Volley.newRequestQueue(this);
        JSONObject parameters = new JSONObject();

        try {
            parameters.put("key", "value");
        } catch (Exception e) {
            // Error when constructing.
        }
        JsonObjectRequest request = new JsonObjectRequest(Request.Method.GET, MSGRAPH_URL,
                parameters,new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                // Successfully called Graph. Process data and send to UI.
            }
        }, new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {
                // Error.
            }
        }) {
            @Override
            public Map<String, String> getHeaders() throws AuthFailureError {
                Map<String, String> headers = new HashMap<>();
                
                // Put access token in HTTP request.
                headers.put("Authorization", "Bearer " + authResult.getAccessToken());
                return headers;
            }
        };

        request.setRetryPolicy(new DefaultRetryPolicy(
                3000,
                DefaultRetryPolicy.DEFAULT_MAX_RETRIES,
                DefaultRetryPolicy.DEFAULT_BACKOFF_MULT));
        queue.add(request);
```

### <a name="msal-for-ios-and-macos"></a>MSAL per iOS e MacOS

I metodi per acquisire i token restituiscono un oggetto `MSALResult`. `MSALResult` espone una proprietà `accessToken` che può essere usata per chiamare un'API Web. Il token di accesso deve essere aggiunto all'intestazione di autorizzazione HTTP, prima di effettuare la chiamata per accedere all'API Web protetta.

Objective-C:

```objc
NSMutableURLRequest *urlRequest = [NSMutableURLRequest new];
urlRequest.URL = [NSURL URLWithString:"https://contoso.api.com"];
urlRequest.HTTPMethod = @"GET";
urlRequest.allHTTPHeaderFields = @{ @"Authorization" : [NSString stringWithFormat:@"Bearer %@", accessToken] };
        
NSURLSessionDataTask *task =
[[NSURLSession sharedSession] dataTaskWithRequest:urlRequest
     completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {}];
[task resume];
```

Swift:

```swift
let urlRequest = NSMutableURLRequest()
urlRequest.url = URL(string: "https://contoso.api.com")!
urlRequest.httpMethod = "GET"
urlRequest.allHTTPHeaderFields = [ "Authorization" : "Bearer \(accessToken)" ]
     
let task = URLSession.shared.dataTask(with: urlRequest as URLRequest) { (data: Data?, response: URLResponse?, error: Error?) in }
task.resume()
```

### <a name="xamarin"></a>Xamarin

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

## <a name="making-several-api-requests"></a>Esecuzione di diverse richieste API

Se è necessario chiamare la stessa API più volte o se è necessario chiamare più API, tenere in considerazione quanto segue quando si compila l'app:

- **Consenso incrementale**: la piattaforma di identità Microsoft consente alle app di ottenere il consenso dell'utente perché sono necessarie le autorizzazioni, anziché tutte all'inizio. Ogni volta che l'app è pronta per chiamare un'API, deve richiedere solo gli ambiti che deve usare.
- **Accesso condizionale**: in alcuni scenari è possibile ottenere ulteriori requisiti di accesso condizionale quando si effettuano diverse richieste API. Questo problema può verificarsi se alla prima richiesta non sono applicati criteri di accesso condizionale e l'app tenta di accedere automaticamente a una nuova API che richiede l'accesso condizionale. Per gestire questo scenario, assicurarsi di intercettare gli errori dalle richieste automatiche e prepararsi a eseguire una richiesta interattiva.  Per altre informazioni, vedere [linee guida per l'accesso condizionale](conditional-access-dev-guide.md).

## <a name="calling-several-apis-in-xamarin-or-uwp---incremental-consent-and-conditional-access"></a>Chiamata di diverse API in Novell o UWP-consenso incrementale e accesso condizionale

Se è necessario chiamare diverse API per lo stesso utente, dopo aver acquisito un token per un utente, è possibile evitare di chiedere ripetutamente le credenziali dell'utente chiamando in seguito `AcquireTokenSilent` per ottenere un token.

```csharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
```

I casi in cui è necessaria l'interazione sono i seguenti:

- L'utente ha acconsentito alla prima API, ma ora deve fornire il consenso per più ambiti (consenso incrementale)
- La prima API non richiede l'autenticazione a più fattori, ma la successiva.

```csharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

try
{
 result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
}
catch(MsalUiRequiredException ex)
{
 result = await app.AcquireTokenInteractive("scopeApi2")
                  .WithClaims(ex.Claims)
                  .ExecuteAsync();
}
```

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Passa all'ambiente di produzione](scenario-mobile-production.md)
