---
title: Microsoft identity platform and OAuth Authorization Code Flow | Azure
description: Building web applications using the Microsoft identity platform implementation of the OAuth 2.0 authentication protocol.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: ae1d7d86-7098-468c-aa32-20df0a10ee3d
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: a20f540d4d220d5cf7171633b817a496d48b8128
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/20/2019
ms.locfileid: "74207721"
---
# <a name="microsoft-identity-platform-and-oauth-20-authorization-code-flow"></a>Microsoft identity platform and OAuth 2.0 authorization code flow

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

La concessione del codice di autorizzazione OAuth 2.0 può essere utilizzata nelle app che vengono installate su un dispositivo per ottenere l'accesso alle risorse protette, come l'API web. Using the Microsoft identity platform implementation of OAuth 2.0, you can add sign in and API access to your mobile and desktop apps. Questa guida, indipendente dal linguaggio, descrive come inviare e ricevere messaggi HTTP senza usare una delle [librerie di autenticazione open source di Azure](reference-v2-libraries.md).

This article describes how to program directly against the protocol in your application.  When possible, we recommend you use the supported Microsoft Authentication Libraries (MSAL) instead to [acquire tokens and call secured web APIs](authentication-flows-app-scenarios.md#scenarios-and-supported-authentication-flows).  Also take a look at the [sample apps that use MSAL](sample-v2-code.md).

> [!NOTE]
> Not all Azure Active Directory scenarios & features are supported by the Microsoft identity platform endpoint. To determine if you should use the Microsoft identity platform endpoint, read about [Microsoft identity platform limitations](active-directory-v2-limitations.md).

Il flusso del codice di autorizzazione di OAuth 2.0 è descritto nella [sezione 4.1 della specifica di OAuth 2.0](https://tools.ietf.org/html/rfc6749). It's used to perform authentication and authorization in the majority of app types, including [web apps](v2-app-types.md#web-apps) and [natively installed  apps](v2-app-types.md#mobile-and-native-apps). The flow enables apps to securely acquire access_tokens that can be used to access resources secured by the Microsoft identity platform endpoint.

## <a name="protocol-diagram"></a>Diagramma di protocollo

In generale, l'intero flusso di autenticazione per un'applicazione nativa/mobile ha un aspetto analogo al seguente:

![Flusso del codice di autenticazione di OAuth](./media/v2-oauth2-auth-code-flow/convergence-scenarios-native.svg)

## <a name="request-an-authorization-code"></a>Richiedere un codice di autorizzazione

Il flusso del codice di autorizzazione ha inizio con il client che indirizza l'utente all'endpoint `/authorize` . In questa richiesta, il client indica le autorizzazioni che deve acquisire dall'utente:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fuser.read
&state=12345
```

> [!TIP]
> Fare clic sul collegamento seguente per eseguire questa richiesta. Dopo l'accesso, il browser deve essere reindirizzato a `https://localhost/myapp/` con un `code` nella barra degli indirizzi.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fuser.read&state=12345" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>

| Parametro    | Obbligatorio/Facoltativo | Description |
|--------------|-------------|--------------|
| `tenant`    | obbligatorio    | Il valore `{tenant}` del percorso della richiesta può essere usato per controllare chi può accedere all'applicazione. I valori consentiti sono `common`, `organizations`, `consumers` e gli identificatori del tenant. Per altre informazioni, vedere le [nozioni di base sul protocollo](active-directory-v2-protocols.md#endpoints).  |
| `client_id`   | obbligatorio    | The **Application (client) ID** that the [Azure portal – App registrations](https://go.microsoft.com/fwlink/?linkid=2083908) experience assigned to your app.  |
| `response_type` | obbligatorio    | Deve includere `code` per il flusso del codice di autorizzazione.       |
| `redirect_uri`  | obbligatorio | URI di reindirizzamento dell'app dove le risposte di autenticazione possono essere inviate e ricevute dall'app. Deve corrispondere esattamente a uno degli URI di reindirizzamento registrati nel portale, ad eccezione del fatto che deve essere codificato come URL. Per le app native e le app per dispositivi mobili è necessario usare il valore predefinito `https://login.microsoftonline.com/common/oauth2/nativeclient`.   |
| `scope`  | obbligatorio    | Elenco separato da spazi di [ambiti](v2-permissions-and-consent.md) a cui si vuole che l'utente dia il consenso.  For the `/authorize` leg of the request, this can cover multiple resources, allowing your app to get consent for multiple web APIs you want to call. |
| `response_mode`   | Consigliato | Specifica il metodo da usare per restituire il token risultante all'app. Può essere uno dei valori seguenti:<br/><br/>- `query`<br/>- `fragment`<br/>- `form_post`<br/><br/>`query` fornisce il codice come parametro della stringa di query nell'URI di reindirizzamento. If you're requesting an ID token using the implicit flow, you can't use `query` as specified in the [OpenID spec](https://openid.net/specs/oauth-v2-multiple-response-types-1_0.html#Combinations). If you're requesting just the code, you can use `query`, `fragment`, or `form_post`. `form_post` esegue un POST contenente il codice all'URI di reindirizzamento. Per altre informazioni, vedere [Protocollo OpenID Connect](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code).  |
| `state`                 | Consigliato | Valore incluso nella richiesta che verrà restituito anche nella risposta del token. Può trattarsi di una stringa di qualsiasi contenuto. Per [evitare gli attacchi di richiesta intersito falsa](https://tools.ietf.org/html/rfc6749#section-10.12), viene in genere usato un valore univoco generato casualmente. Questo valore può essere usato anche per codificare le informazioni sullo stato dell'utente nell'app prima dell'esecuzione della richiesta di autenticazione, ad esempio la pagina o la vista in cui si trovava. |
| `prompt`  | facoltativo    | Indica il tipo di interazione obbligatoria dell'utente. Gli unici valori validi al momento sono `login`, `none` e `consent`.<br/><br/>- `prompt=login` forza l'utente a immettere le sue credenziali alla richiesta, negando l'accesso Single Sign-On.<br/>- `prompt=none` is the opposite - it will ensure that the user isn't presented with any interactive prompt whatsoever. If the request can't be completed silently via single-sign on, the Microsoft identity platform endpoint will return an `interaction_required` error.<br/>- `prompt=consent` attiva la finestra di dialogo di consenso di OAuth dopo l'accesso dell'utente, che chiede all'utente di concedere le autorizzazioni all'app. |
| `login_hint`  | facoltativo    | Consente di pre-compilare il campo nome utente/indirizzo di posta elettronica dell'utente nella pagina di accesso, se già si conosce il nome utente. Le app usano spesso questo parametro durante la riautenticazione, dopo aver estratto il nome utente da un accesso precedente tramite l'attestazione `preferred_username`.   |
| `domain_hint`  | facoltativo    | Può essere uno di `consumers` o `organizations`.<br/><br/>If included, it will skip the email-based discovery process that user goes through on the sign-in page, leading to a slightly more streamlined user experience. Le app usano spesso questo parametro durante la riautenticazione, estraendo `tid` da un accesso precedente. Se il valore dell'attestazione `tid` è `9188040d-6c67-4c5b-b112-36a304b66dad`, usare `domain_hint=consumers`. In caso contrario, usare `domain_hint=organizations`.  |
| `code_challenge_method` | facoltativo    | Metodo usato per codificare `code_verifier` per il parametro `code_challenge`. Può avere uno dei valori seguenti:<br/><br/>- `plain` <br/>- `S256`<br/><br/>Se escluso, `code_challenge` viene considerato testo non crittografato se `code_challenge` è incluso. Microsoft identity platform supports both `plain` and `S256`. Per altre informazioni, vedere il [PKCE RFC](https://tools.ietf.org/html/rfc7636). |
| `code_challenge`  | facoltativo | Usato per proteggere i privilegi concessi sui codici di autorizzazione tramite la chiave di prova per Code Exchange (PKCE) da un client nativo. Obbligatorio con `code_challenge_method` incluso. Per altre informazioni, vedere il [PKCE RFC](https://tools.ietf.org/html/rfc7636). |

A questo punto, all'utente viene chiesto di immettere le credenziali e completare l'autenticazione. The Microsoft identity platform endpoint will also ensure that the user has consented to the permissions indicated in the `scope` query parameter. Se l'utente non ha acconsentito a nessuna di queste autorizzazioni, l'endpoint chiederà all'utente di fornire il consenso per le autorizzazioni obbligatorie. Questo articolo contiene informazioni dettagliate su [autorizzazioni, consenso e app multi-tenant](v2-permissions-and-consent.md).

Once the user authenticates and grants consent, the Microsoft identity platform endpoint will return a response to your app at the indicated `redirect_uri`, using the method specified in the `response_mode` parameter.

#### <a name="successful-response"></a>Risposta con esito positivo

Una risposta con esito positivo che usa `response_mode=query` ha un aspetto simile al seguente:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=12345
```

| Parametro | Description  |
|-----------|--------------|
| `code` | Codice di autorizzazione richiesto dall'app. L'app può usare il codice di autorizzazione per richiedere un token di accesso per la risorsa di destinazione. Authorization_codes are short lived, typically they expire after about 10 minutes. |
| `state` | Se un parametro di stato è incluso nella richiesta, lo stesso valore viene visualizzato nella risposta. L'app deve verificare che i valori dello stato nella richiesta e nella risposta siano identici. |

#### <a name="error-response"></a>Risposta di errore

Le risposte di errore possono essere inviate anche a `redirect_uri` , in modo che l'app possa gestirle adeguatamente:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametro | Description  |
|----------|------------------|
| `error`  | Stringa di codice di errore che può essere usata per classificare i tipi di errori che si verificano e correggerli. |
| `error_description` | Messaggio di errore specifico che consente a uno sviluppatore di identificare la causa principale di un errore di autenticazione. |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Codici per gli errori dell'endpoint di autorizzazione

La tabella seguente descrive i diversi codici errore che possono essere restituiti nel parametro `error` della risposta di errore.

| Codice di errore  | Description    | Azione client   |
|-------------|----------------|-----------------|
| `invalid_request` | Errore del protocollo, ad esempio un parametro obbligatorio mancante. | Correggere e inviare di nuovo la richiesta. Si tratta di un errore di sviluppo rilevato in genere durante il test iniziale. |
| `unauthorized_client` | The client application isn't permitted to request an authorization code. | This error usually occurs when the client application isn't registered in Azure AD or isn't added to the user's Azure AD tenant. L'applicazione può chiedere all'utente di installare l'applicazione e di aggiungerla ad Azure AD. |
| `access_denied`  | Consenso negato dal proprietario della risorsa  | The client application can notify the user that it can't proceed unless the user consents. |
| `unsupported_response_type` | Il server di autorizzazione non supporta il tipo di risposta nella richiesta. | Correggere e inviare di nuovo la richiesta. Si tratta di un errore di sviluppo rilevato in genere durante il test iniziale.  |
| `server_error`  | Errore imprevisto rilevato dal server.| ripetere la richiesta. Questi errori possono dipendere da condizioni temporanee. L'applicazione client può comunicare all'utente che la risposta è stata ritardata a causa di un errore temporaneo. |
| `temporarily_unavailable`   | Il server è temporaneamente troppo occupato per gestire la richiesta. | ripetere la richiesta. The client application might explain to the user that its response is delayed because of a temporary condition. |
| `invalid_resource`  | The target resource is invalid because it does not exist, Azure AD can't find it, or it's not correctly configured. | Questo errore indica che la risorsa, se presente, non è stata configurata nel tenant. L'applicazione può chiedere all'utente di installare l'applicazione e di aggiungerla ad Azure AD. |
| `login_required` | Troppi o nessun utente | Il client ha richiesto l'autenticazione invisibile all'utente (`prompt=none`), ma non è stato trovato un singolo utente. Questo può significare che sono presenti più utenti attivi nella sessione o nessun utente. This takes into account the tenant chosen (for example, if there are two Azure AD accounts active and one Microsoft account, and `consumers` is chosen, silent authentication will work). |
| `interaction_required` | La richiesta richiede l'interazione dell'utente. | È necessario un passaggio di autenticazione o un consenso aggiuntivo. Ripetere la richiesta senza `prompt=none`. |

## <a name="request-an-access-token"></a>Richiedere un token di accesso

Dopo avere ottenuto un codice di autorizzazione e l'autorizzazione dell'utente, è possibile riscattare il valore `code` per un `access_token` per la risorsa desiderata. A tale scopo, inviare una richiesta `POST` all'endpoint `/token`:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fuser.read
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [!TIP]
> Provare a eseguire la richiesta in Postman. (Don't forget to replace the `code`) [![Try running this request in Postman](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)

| Parametro  | Obbligatorio/Facoltativo | Description     |
|------------|-------------------|----------------|
| `tenant`   | obbligatorio   | Il valore `{tenant}` del percorso della richiesta può essere usato per controllare chi può accedere all'applicazione. I valori consentiti sono `common`, `organizations`, `consumers` e gli identificatori del tenant. Per altre informazioni, vedere le [nozioni di base sul protocollo](active-directory-v2-protocols.md#endpoints).  |
| `client_id` | obbligatorio  | The Application (client) ID that the [Azure portal – App registrations](https://go.microsoft.com/fwlink/?linkid=2083908) page assigned to your app. |
| `grant_type` | obbligatorio   | Deve essere `authorization_code` per il flusso del codice di autorizzazione.   |
| `scope`      | obbligatorio   | Elenco di ambiti separati da spazi. Gli ambiti richiesti in questa sezione devono essere equivalenti agli ambiti richiesti nella prima sezione o un sottoinsieme di questi ultimi. The scopes must all be from a single resource, along with OIDC scopes (`profile`, `openid`, `email`). Per una spiegazione più dettagliata degli ambiti, fare riferimento all'argomento relativo ad [autorizzazioni, consenso e ambiti](v2-permissions-and-consent.md). |
| `code`          | obbligatorio  | Codice di autorizzazione acquisito durante la prima sezione del flusso. |
| `redirect_uri`  | obbligatorio  | Stesso valore redirect_uri usato per acquisire il codice di autorizzazione. |
| `client_secret` | Obbligatorio per app Web | Segreto dell'applicazione creato per l'app nel portale di registrazione delle app. You shouldn't use the application secret in a native app because client_secrets can't be reliably stored on devices. It's required for web apps and web APIs, which have the ability to store the client_secret securely on the server side.  Prima di essere inviato, il segreto client deve essere codificato come URL.  |
| `code_verifier` | facoltativo  | Stesso code_verifier usato per ottenere il codice di autorizzazione. Obbligatorio se nella richiesta di concessione del codice di autorizzazione è stato usato PKCE. Per altre informazioni, vedere il [PKCE RFC](https://tools.ietf.org/html/rfc7636). |

### <a name="successful-response"></a>Risposta con esito positivo

Una risposta token con esito positivo ha un aspetto simile al seguente:

```json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fuser.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```

| Parametro     | Description   |
|---------------|------------------------------|
| `access_token`  | Token di accesso richiesto. L'app può usare questo token per l'autenticazione alla risorsa protetta, ad esempio un'API Web.  |
| `token_type`    | Indica il valore del tipo di token. L'unico tipo supportato da Azure AD è 'Bearer'. |
| `expires_in`    | Validità del token di accesso (espressa in secondi). |
| `scope`         | Ambiti per i quali il token di accesso è valido. |
| `refresh_token` | Token di aggiornamento di OAuth 2.0. L'app può usare questo token per acquisire token di accesso aggiuntivi dopo la scadenza del token di accesso corrente. I token di aggiornamento hanno una durata elevata e possono essere usati per mantenere l'accesso alle risorse per lunghi periodi di tempo. Per informazioni dettagliate sull'aggiornamento di un token di accesso, vedere la [sezione successiva](#refresh-the-access-token). <br> **Nota:** viene fornito solo è stato richiesto l'ambito `offline_access`. |
| `id_token`      | Token JSON Web (JWT). L'app può decodificare i segmenti di questo token per richiedere informazioni sull'utente che ha eseguito l'accesso. L'app può memorizzare nella cache i valori e visualizzarli, ma non deve basarsi su di essi per eventuali autorizzazioni o limiti di sicurezza. Per altre informazioni sugli oggetti id_token, vedere [`id_token reference`](id-tokens.md). <br> **Nota:** viene fornito solo è stato richiesto l'ambito `openid`. |

### <a name="error-response"></a>Risposta di errore

Le risposte di errore hanno un aspetto simile al seguente:

```json
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parametro         | Description    |
|-------------------|----------------|
| `error`       | Stringa di codice di errore che può essere usata per classificare i tipi di errori che si verificano e correggerli. |
| `error_description` | Messaggio di errore specifico che consente a uno sviluppatore di identificare la causa principale di un errore di autenticazione. |
| `error_codes` | Elenco dei codici di errore specifici del servizio token di sicurezza utile per la diagnostica.  |
| `timestamp`   | Ora in cui si è verificato l'errore. |
| `trace_id`    | Identificatore univoco per la richiesta utile per la diagnostica. |
| `correlation_id` | Identificatore univoco per la richiesta utile per la diagnostica tra i componenti. |

### <a name="error-codes-for-token-endpoint-errors"></a>Codici per gli errori degli endpoint di token

| Codice di errore         | Description        | Azione client    |
|--------------------|--------------------|------------------|
| `invalid_request`  | Errore del protocollo, ad esempio un parametro obbligatorio mancante. | Correggere e inviare di nuovo la richiesta.   |
| `invalid_grant`    | Il codice di autorizzazione o PKCE non è valido o è scaduto. | Provare una nuova richiesta per l'endpoint `/authorize` e verificare che il parametro code_verifier sia corretto.  |
| `unauthorized_client` | The authenticated client isn't authorized to use this authorization grant type. | This usually occurs when the client application isn't registered in Azure AD or isn't added to the user's Azure AD tenant. L'applicazione può chiedere all'utente di installare l'applicazione e di aggiungerla ad Azure AD. |
| `invalid_client` | Autenticazione client non riuscita.  | The client credentials aren't valid. Per risolvere il problema, l'amministratore applicazione aggiorna le credenziali.   |
| `unsupported_grant_type` | Il server di autorizzazione non supporta il tipo di concessione dell'autorizzazione. | Modificare il tipo di concessione nella richiesta. Questo tipo di errore dovrebbe verificarsi solo durante lo sviluppo ed essere rilevato durante il test iniziale. |
| `invalid_resource` | The target resource is invalid because it does not exist, Azure AD can't find it, or it's not correctly configured. | Indica che la risorsa, se presente, non è stata configurata nel tenant. L'applicazione può chiedere all'utente di installare l'applicazione e di aggiungerla ad Azure AD.  |
| `interaction_required` | La richiesta richiede l'interazione dell'utente. Ad esempio, è necessario un passaggio di autenticazione aggiuntivo. | Ripetere la richiesta con la stessa risorsa.  |
| `temporarily_unavailable` | Il server è temporaneamente troppo occupato per gestire la richiesta. | ripetere la richiesta. The client application might explain to the user that its response is delayed because of a temporary condition. |

## <a name="use-the-access-token"></a>Usare il token di accesso

Dopo aver ottenuto un `access_token` è possibile usarlo in richieste alle API Web includendolo nell'intestazione `Authorization`:

> [!TIP]
> Eseguire la richiesta in Postman. (Replace the `Authorization` header first) [![Try running this request in Postman](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-access-token"></a>Aggiornare il token di accesso

I token di accesso hanno breve durata ed è quindi necessario aggiornarli dopo la scadenza per continuare ad accedere alle risorse. A tale scopo, inviare un'altra richiesta `POST` all'endpoint `/token`, specificando il `refresh_token` anziché il valore `code`.  I token di aggiornamento sono validi per tutte le autorizzazioni per cui il client ha già ottenuto il consenso; in questo modo, un token di aggiornamento emesso su richiesta per `scope=mail.read` può essere usato per richiedere un nuovo token di accesso per `scope=api://contoso.com/api/UseResource`.  

I token di aggiornamento non hanno una durata specificata. La durata dei token di aggiornamento è in genere relativamente lunga. In alcuni casi, tuttavia, i token di aggiornamento scadono, vengono revocati o non hanno privilegi sufficienti per l'azione desiderata. L'applicazione deve prevedere e gestire correttamente gli [errori restituiti dall'endpoint di rilascio del token](#error-codes-for-token-endpoint-errors). 

Although refresh tokens aren't revoked when used to acquire new access tokens, you are expected to discard the old refresh token. The [OAuth 2.0 spec](https://tools.ietf.org/html/rfc6749#section-6) says: "The authorization server MAY issue a new refresh token, in which case the client MUST discard the old refresh token and replace it with the new refresh token. The authorization server MAY revoke the old refresh token after issuing a new refresh token to the client."  

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fuser.read
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&grant_type=refresh_token
&client_secret=JqQX2PNo9bpM0uEihUPzyrh      // NOTE: Only required for web apps
```

> [!TIP]
> Provare a eseguire la richiesta in Postman. (Don't forget to replace the `refresh_token`) [![Try running this request in Postman](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)
> 

| Parametro     |                | Description        |
|---------------|----------------|--------------------|
| `tenant`        | obbligatorio     | Il valore `{tenant}` del percorso della richiesta può essere usato per controllare chi può accedere all'applicazione. I valori consentiti sono `common`, `organizations`, `consumers` e gli identificatori del tenant. Per altre informazioni, vedere le [nozioni di base sul protocollo](active-directory-v2-protocols.md#endpoints).   |
| `client_id`     | obbligatorio    | The **Application (client) ID** that the [Azure portal – App registrations](https://go.microsoft.com/fwlink/?linkid=2083908) experience assigned to your app. |
| `grant_type`    | obbligatorio    | Deve essere `refresh_token` per questa sezione del flusso del codice di autorizzazione. |
| `scope`         | obbligatorio    | Elenco di ambiti separati da spazi. Gli ambiti richiesti in questa sezione devono essere equivalenti agli ambiti richiesti nella sezione di richiesta del codice di autorizzazione originale o un sottoinsieme di questi ultimi. If the scopes specified in this request span multiple resource server, then the Microsoft identity platform endpoint will return a token for the resource specified in the first scope. Per una spiegazione più dettagliata degli ambiti, fare riferimento all'argomento relativo ad [autorizzazioni, consenso e ambiti](v2-permissions-and-consent.md). |
| `refresh_token` | obbligatorio    | Token di aggiornamento acquisito durante la seconda sezione del flusso. |
| `client_secret` | Obbligatorio per app Web | Segreto dell'applicazione creato per l'app nel portale di registrazione delle app. It should not be used in a native  app, because client_secrets can't be reliably stored on devices. It's required for web apps and web APIs, which have the ability to store the client_secret securely on the server side. |

#### <a name="successful-response"></a>Risposta con esito positivo

Una risposta token con esito positivo ha un aspetto simile al seguente:

```json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fuser.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parametro     | Description         |
|---------------|-------------------------------------------------------------|
| `access_token`  | Token di accesso richiesto. L'app può usare questo token per l'autenticazione alla risorsa protetta, ad esempio un'API Web. |
| `token_type`    | Indica il valore del tipo di token. L'unico tipo supportato da Azure AD è 'Bearer'. |
| `expires_in`    | Validità del token di accesso (espressa in secondi).   |
| `scope`         | Ambiti per i quali il token di accesso è valido.    |
| `refresh_token` | Nuovo token di aggiornamento di OAuth 2.0. È necessario sostituire il token di aggiornamento precedente con quello appena acquisito, per garantire che i token di aggiornamento rimangano validi il più a lungo possibile. <br> **Nota:** viene fornito solo è stato richiesto l'ambito `offline_access`.|
| `id_token`      | Token JWT (Token Web JSON) non firmato. L'app può decodificare i segmenti di questo token per richiedere informazioni sull'utente che ha eseguito l'accesso. L'app può memorizzare nella cache i valori e visualizzarli, ma non deve basarsi su di essi per eventuali autorizzazioni o limiti di sicurezza. Per altre informazioni sugli oggetti id_token, vedere [`id_token reference`](id-tokens.md). <br> **Nota:** viene fornito solo è stato richiesto l'ambito `openid`. |

#### <a name="error-response"></a>Risposta di errore

```json
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parametro         | Description                                                                                        |
|-------------------|----------------------------------------------------------------------------------------------------|
| `error`           | Stringa di codice di errore che può essere usata per classificare i tipi di errori che si verificano e correggerli. |
| `error_description` | Messaggio di errore specifico che consente a uno sviluppatore di identificare la causa principale di un errore di autenticazione.           |
| `error_codes` |Elenco dei codici di errore specifici del servizio token di sicurezza utile per la diagnostica. |
| `timestamp` | Ora in cui si è verificato l'errore. |
| `trace_id` | Identificatore univoco per la richiesta utile per la diagnostica. |
| `correlation_id` | Identificatore univoco per la richiesta utile per la diagnostica tra i componenti. |

Per una descrizione dei codici di errore e l'azione consigliata per il client, vedere [Codici per gli errori degli endpoint di token](#error-codes-for-token-endpoint-errors).
