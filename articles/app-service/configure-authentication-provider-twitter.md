---
title: Configurare l'autenticazione di Twitter - Servizio app di Azure
description: Informazioni su come configurare l'autenticazione di Twitter per l'app del servizio app.
services: app-service
documentationcenter: ''
author: mattchenderson
manager: syntaxc4
editor: ''
ms.assetid: c6dc91d7-30f6-448c-9f2d-8e91104cde73
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 04/19/2018
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: f7154da76b41198c208d02b8c563ba26ff8101a1
ms.sourcegitcommit: 909ca340773b7b6db87d3fb60d1978136d2a96b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2019
ms.locfileid: "70983606"
---
# <a name="how-to-configure-your-app-service-application-to-use-twitter-login"></a>Come configurare un'applicazione del servizio app per usare l'account di accesso di Twitter
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Questo argomento descrive come configurare il servizio app di Azure per usare Twitter come provider di autenticazione.

Per completare la procedura descritta in questo argomento è necessario avere un account Twitter con un indirizzo di posta elettronica e un numero di telefono verificati. Per creare un nuovo account Twitter, visitare il sito Web all'indirizzo <a href="https://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.

## <a name="register"></a>Registrare l'applicazione con Twitter
1. Accedere al [portale di Azure], e passare all'applicazione. Copiare l' **URL**. Verrà usato per configurare l'app Twitter.
2. Passare al sito Web [Twitter Developers] , accedere con le credenziali dell'account Twitter e quindi fare clic su **Create New App**(Crea nuova app).
3. Digitare i valori per **Name** (Nome) e **Description** (Descrizione) per la nuova app. Incollare l'**URL** dell'applicazione per il valore **Website** (Sito Web). Quindi, per l' **URL di callback**, digitare l'URL dell'app del servizio app e aggiungere il `/.auth/login/twitter/callback`percorso. Ad esempio `https://contoso.azurewebsites.net/.auth/login/twitter/callback`. Assicurarsi che sia in uso lo schema HTTPS.
4. Nella parte inferiore della pagina, leggere e accettare le condizioni di utilizzo. Fare clic su **Create your Twitter application**. Verranno visualizzati i dettagli dell'applicazione.
5. Fare clic sulla scheda **Settings** (Impostazioni), selezionare **Allow this application to be used to sign in with Twitter** (Consenti l'uso dell'applicazione per l'accesso con Twitter) e quindi fare clic su **Update Settings** (Aggiorna impostazioni).
6. Selezionare la scheda **Keys and Access Tokens** . Prendere nota dei valori di Consumer Key (API Key) (Chiave utente (chiave API)) e **Consumer secret (API Secret)** (Segreto utente (chiave privata API)).
   
   > [!NOTE]
   > Il segreto consumer è un'importante credenziale di sicurezza. Non condividere questo valore con altri né distribuirlo con l'app.
   > 
   > 

## <a name="secrets"></a>Aggiungere informazioni di Twitter all'applicazione
1. Nel [portale di Azure], passare all'applicazione. Fare clic su **Impostazioni** e quindi su **Autenticazione/Autorizzazione**.
2. Se la funzionalità di autenticazione/autorizzazione non è abilitata, impostare l'opzione in modo da **abilitarla**.
3. Fare clic su **Twitter**. Incollare i valori di ID app e segreto app ottenuti in precedenza. Fare quindi clic su **OK**.
   
   ![][1]
   
   Per impostazione predefinita, il servizio app fornisce l'autenticazione ma non limita l'accesso alle API e al contenuto del sito solo agli utenti autorizzati. È necessario autorizzare gli utenti nel codice dell'app.
4. (Facoltativo) Per consentire l'accesso al sito solo agli utenti autenticati da Twitter, impostare il parametro **Azione da eseguire quando la richiesta non è autenticata** su **Twitter**. Per poter utilizzare questa funzione, tuttavia, è necessario che tutte le richieste vengano autenticate e che le richieste non autenticate vengano reindirizzate a Twitter per l'autenticazione.

> [!NOTE]
> La limitazione dell'accesso in questo modo si applica a tutte le chiamate all'app, che potrebbero non essere desiderate per le app che vogliono un home page disponibile pubblicamente, come in molte applicazioni a singola pagina. Per queste applicazioni, **consentire le richieste anonime (nessuna azione)** può essere preferibile, con l'app che avvia manualmente l'accesso, come descritto [qui](overview-authentication-authorization.md#authentication-flow).

5. Fare clic su **Save**.

È ora possibile usare un account Twitter per l'autenticazione nell'app.

## <a name="related-content"></a>Contenuti correlati
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter Developers]: https://go.microsoft.com/fwlink/p/?LinkId=268300
[Portale di Azure]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
