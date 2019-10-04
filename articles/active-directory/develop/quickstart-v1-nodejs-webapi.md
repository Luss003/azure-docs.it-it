---
title: "Guida introduttiva: Proteggere l'API Web Node.js con Azure Active Directory"
description: Informazioni su come compilare un'API Web REST per Node.js che si integra con Azure AD per l'autenticazione.
services: active-directory
documentationcenter: nodejs
author: rwike77
manager: CelesteDG
ms.assetid: 7654ab4c-4489-4ea5-aba9-d7cdc256e42a
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: ryanwi
ms.custom: seo-javascript-september2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7307afb2ce9e10326c42f96733d3f80979c4a4b3
ms.sourcegitcommit: ca359c0c2dd7a0229f73ba11a690e3384d198f40
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/17/2019
ms.locfileid: "71056550"
---
# <a name="quickstart-secure-a-web-api-with-azure-active-directory"></a>Guida introduttiva: Proteggere l'API Web Node.js con Azure Active Directory

[!INCLUDE [active-directory-develop-applies-v1](../../../includes/active-directory-develop-applies-v1.md)]

In questa guida introduttiva si apprenderà come proteggere un endpoint dell'API [Restify](http://restify.com/) con [Passport](http://passportjs.org/) usando il modulo [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad) per gestire le comunicazioni con Azure Active Directory (Azure AD).

Questa guida introduttiva è incentrata sui vari aspetti correlati alla protezione degli endpoint dell'API. Gli aspetti che riguardano l'accesso e il mantenimento dei token di autenticazione non sono implementati negli esempi qui riportati e sono di responsabilità di un'applicazione client. Per informazioni dettagliate sugli aspetti correlati a un'implementazione client, vedere [Accesso e disconnessione all'app Web di Node.js con Azure AD](quickstart-v1-openid-connect-code.md).

L'esempio di codice completo associato all'articolo è disponibile in [GitHub](https://github.com/Azure-Samples/active-directory-node-webapi-basic).

## <a name="prerequisites"></a>Prerequisiti

Per iniziare, completare questi prerequisiti.

### <a name="create-the-sample-project"></a>Creare il progetto di esempio

Per questa applicazione server sono necessarie alcune dipendenze di pacchetto per supportare Restify e Passport, oltre a informazioni per l'account che vengono passate ad Azure AD.

Per iniziare, aggiungere il codice seguente in un file denominato `package.json`:

```Shell
{
  "name": "active-directory-webapi-nodejs",
  "version": "0.0.1",
  "scripts": {
    "start": "node app.js"
  },
  "dependencies": {
    "passport": "0.4.0",
    "passport-azure-ad": "4.0.0",
    "restify": "7.7.0"
  }
}
```

Dopo aver creato `package.json`, eseguire `npm install` nel prompt dei comandi per installare le dipendenze di pacchetto.

#### <a name="configure-the-project-to-use-active-directory"></a>Configurare il progetto per l'uso di Active Directory

Per iniziare la configurazione dell'applicazione, esistono alcuni valori specifici dell'account che è possibile ottenere tramite l'interfaccia della riga di comando di Azure. Il modo più semplice per iniziare ad acquisire familiarità con l'interfaccia della riga di comando consiste nell'usare Azure Cloud Shell.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Immettere il comando seguente in Cloud Shell:

```azurecli-interactive
az ad app create --display-name node-aad-demo --homepage http://localhost --identifier-uris http://node-aad-demo
```

Gli [argomenti](/cli/azure/ad/app?view=azure-cli-latest#az-ad-app-create) per il comando `create` includono:

| Argomento  | DESCRIZIONE |
|---------|---------|
|`display-name` | Nome descrittivo della registrazione |
|`homepage` | URL a cui possono accedere gli utenti per usare l'applicazione |
|`identifier-uris` | URI univoci delimitati da spazio che possono essere usati da Azure AD per questa applicazione |

Prima di connettersi ad Azure Active Directory, sono necessarie le informazioni seguenti:

| NOME  | DESCRIZIONE | Nome della variabile nel file di configurazione |
| ------------- | ------------- | ------------- |
| Nome del tenant  | [Nome del tenant](quickstart-create-new-tenant.md) che si vuole usare per l'autenticazione | `tenantName`  |
| ID client  | ID client è il termine di OAuth usato per l'_ID applicazione_ di AAD. |  `clientID`  |

Copiare il valore `appId` dalla risposta di registrazione in Azure Cloud Shell e creare un nuovo file denominato `config.js`. Aggiungere quindi il codice seguente e sostituire i valori con i token tra parentesi:

```JavaScript
const tenantName    = //<YOUR_TENANT_NAME>;
const clientID      = //<YOUR_APP_ID_FROM_CLOUD_SHELL>;
const serverPort    = 3000;

module.exports.serverPort = serverPort;

module.exports.credentials = {
  identityMetadata: `https://login.microsoftonline.com/${tenantName}.onmicrosoft.com/.well-known/openid-configuration`, 
  clientID: clientID
};
```

Per altre informazioni sulle singole impostazioni di configurazione, vedere la documentazione per il modulo [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad#5-usage).

### <a name="implement-the-server"></a>Implementare il server

Il modulo [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad#5-usage) include due strategie di autenticazione: [OIDC](https://github.com/AzureAD/passport-azure-ad#51-oidcstrategy) e [Bearer](https://github.com/AzureAD/passport-azure-ad#52-bearerstrategy). Il server implementato in questo articolo usa la strategia Bearer per proteggere l'endpoint dell'API.

### <a name="step-1-import-dependencies"></a>Passaggio 1: Importare le dipendenze

Creare un nuovo file denominato `app.js` e incollare il testo seguente:

```JavaScript
const
      restify = require('restify')
    , restifyPlugins = require ('restify').plugins
    , passport = require('passport')
    , BearerStrategy = require('passport-azure-ad').BearerStrategy
    , config = require('./config')
    , authenticatedUserTokens = []
    , serverPort = process.env.PORT || config.serverPort
;
```

In questa sezione del codice:

- Viene fatto riferimento ai moduli `restify` e plugins per configurare un server Restify.
- I moduli `passport` e `passport-azure-ad` sono responsabili per la comunicazione con Azure AD.
- La variabile `config` viene inizializzata con i valori dal file `config.js` creato nel passaggio precedente.
- Viene creata una matrice per `authenticatedUserTokens` per archiviare i token dell'utente passati negli endpoint protetti.
- Il valore `serverPort` viene definito in base alla porta dell'ambiente di elaborazione o dal file di configurazione.

### <a name="step-2-instantiate-an-authentication-strategy"></a>Passaggio 2: Creare un'istanza di una strategia di autenticazione

Per la protezione di un endpoint è necessario specificare una strategia responsabile di determinare se la richiesta corrente proviene o meno da un utente autenticato. In questo caso la variabile `authenticatonStrategy` è un'istanza della classe `BearerStrategy` di `passport-azure-ad`. Aggiungere il codice seguente dopo le istruzioni `require`.

```JavaScript
const authenticationStrategy = new BearerStrategy(config.credentials, (token, done) => {
    let currentUser = null;

    let userToken = authenticatedUserTokens.find((user) => {
        currentUser = user;
        user.sub === token.sub;
    });

    if(!userToken) {
        authenticatedUserTokens.push(token);
    }

    return done(null, currentUser, token);
});
```

Questa implementazione usa la registrazione automatica aggiungendo i token di autenticazione nella matrice `authenticatedUserTokens` se non esistono già.

Dopo aver creato una nuova istanza della strategia, è necessario passarla a Passport tramite il metodo `use`. Aggiungere il seguente codice in `app.js` per usare la strategia in Passport.

```JavaScript
passport.use(authenticationStrategy);
```

### <a name="step-3-server-configuration"></a>Passaggio 3: Configurazione del server

Dopo aver definito la strategia di autenticazione, è ora possibile configurare il server Restify con alcune impostazioni di base e impostarlo per l'uso di Passport per la sicurezza.

```JavaScript
const server = restify.createServer({ name: 'Azure Active Directory with Node.js Demo' });
server.use(restifyPlugins.authorizationParser());
server.use(passport.initialize());
server.use(passport.session());
```
Questo server viene inizializzato e configurato in modo da analizzare le intestazioni di autorizzazione e quindi impostato per l'uso di Passport.

### <a name="step-4-define-routes"></a>Passaggio 4: Definire le route

È ora possibile definire le route e decidere quali proteggere con Azure AD. Questo progetto include due route con livello radice aperto e la route `/api` è impostata per richiedere l'autenticazione.

In `app.js` aggiungere il codice seguente alla route del livello radice:

```JavaScript
server.get('/', (req, res, next) => {
    res.send(200, 'Try: curl -isS -X GET http://127.0.0.1:3000/api');
    next();
});
```

La route radice consente tutte le richieste attraverso la route e restituisce un messaggio che include un comando per testare la route `/api`. Al contrario, la route `/api` è bloccata tramite [`passport.authenticate`](http://passportjs.org/docs/authenticate). Aggiungere il codice seguente dopo la route radice.

```JavaScript
server.get('/api', passport.authenticate('oauth-bearer', { session: false }), (req, res, next) => {
    res.json({ message: 'response from API endpoint' });
    return next();
});
```

Questa configurazione consente solo le richieste autenticate che includono l'accesso con token di connessione a `/api`. L'opzione `session: false` viene usata per disabilitare le sessioni per richiedere il passaggio di un token con ogni richiesta all'API.

Il server viene infine impostato per l'ascolto sulla porta configurata tramite la chiamata del metodo `listen`.

```JavaScript
server.listen(serverPort);
```

## <a name="run-the-sample"></a>Eseguire l'esempio

Ora che il server è implementato, è possibile avviare il server. Aprire un prompt dei comandi e immettere:

```shell
npm start
```

Con il server in esecuzione, è possibile inviare una richiesta al server per testare i risultati. Per una dimostrazione di risposta dalla route radice, aprire una shell Bash e immettere il codice seguente:

```shell
curl -isS -X GET http://127.0.0.1:3000/
```

Se il server è stato configurato correttamente, la risposta sarà simile alla seguente:

```shell
HTTP/1.1 200 OK
Server: Azure Active Directory with Node.js Demo
Content-Type: application/json
Content-Length: 49
Date: Tue, 10 Oct 2017 18:35:13 GMT
Connection: keep-alive

Try: curl -isS -X GET http://127.0.0.1:3000/api
```

A questo punto è possibile testare la route che richiede l'autenticazione immettendo il comando seguente nella shell Bash:

```shell
curl -isS -X GET http://127.0.0.1:3000/api
```

Se il server è stato configurato correttamente, dovrebbe rispondere con lo stato `Unauthorized`.

```shell
HTTP/1.1 401 Unauthorized
Server: Azure Active Directory with Node.js Demo
WWW-Authenticate: token is not found
Date: Tue, 10 Oct 2017 16:22:03 GMT
Connection: keep-alive
Content-Length: 12

Unauthorized
```

Dopo avere creato un'API protetta, è possibile implementare un client in grado di passare i token di autenticazione all'API.

## <a name="next-steps"></a>Passaggi successivi

* È necessario implementare una controparte client per la connessione al server che gestisce l'accesso, la disconnessione e la gestione dei token. Per esempi basati su codice, fare riferimento alle applicazioni client in [iOS](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios) e [Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android).
* Per un'esercitazione dettagliata, vedere [Accesso e disconnessione all'app Web di Node.js con Azure AD](quickstart-v1-openid-connect-code.md).
