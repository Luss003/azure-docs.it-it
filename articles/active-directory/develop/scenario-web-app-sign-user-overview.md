---
title: App Web che esegue l'accesso degli utenti (panoramica)-piattaforma di identità Microsoft
description: Informazioni su come creare un'app Web per l'accesso degli utenti (panoramica)
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
ms.date: 09/17/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1f9b6d4ce4048b31f17b50184f90aed4d2a8ba81
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2019
ms.locfileid: "71086564"
---
# <a name="scenario-web-app-that-signs-in-users"></a>Scenario: App Web che offre l'accesso agli utenti

Scopri tutto quello che ti serve per creare un'app Web che esegue l'accesso agli utenti con la piattaforma di identità Microsoft.

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Introduzione

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

Se si vuole creare le prime app Web portabili (ASP.NET Core) che eseguono l'accesso agli utenti, seguire questa Guida introduttiva:

> [!div class="nextstepaction"]
> [Avvio rapido: ASP.NET Core app Web per l'accesso degli utenti](quickstart-v2-aspnet-core-webapp.md)

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

per informazioni su come aggiungere l'accesso a un'applicazione Web ASP.NET legacy, provare l'esercitazione seguente:

> [!div class="nextstepaction"]
> [Avvio rapido: App Web ASP.NET che esegue l'accesso agli utenti](quickstart-v2-aspnet-webapp.md)

# <a name="javatabjava"></a>[Java](#tab/java)

Se si è uno sviluppatore Java, provare la Guida introduttiva seguente:

> [!div class="nextstepaction"]
> [Avvio rapido: Aggiungere l'accesso con Microsoft a un'app Web Java](quickstart-v2-java-webapp.md)

# <a name="pythontabpython"></a>[Python](#tab/python)

Se si sviluppa con Python, provare:

> [!div class="nextstepaction"]
> [Avvio rapido: Aggiungere l'accesso con Microsoft a un'app Web Python](quickstart-v2-python-webapp.md)

---

## <a name="overview"></a>Panoramica

Aggiungere l'autenticazione all'app Web in modo che possa accedere agli utenti. L'aggiunta dell'autenticazione consente all'app Web di accedere a informazioni limitate sul profilo e, ad esempio, di personalizzare l'esperienza offerta agli utenti. App Web autenticare un utente in un Web browser. In questo scenario l'applicazione Web indirizza il browser dell'utente per l'accesso ad Azure AD. Tramite il browser dell'utente Azure AD restituisce una risposta di accesso che contiene attestazioni sull'utente in un token di sicurezza. Gli utenti che effettuano l'accesso sfruttano il protocollo standard [Open ID Connect](./v2-protocols-oidc.md) , semplificato dall'utilizzo delle [librerie](scenario-web-app-sign-user-app-configuration.md#libraries-used-to-protect-web-apps)middleware.

![Utenti che accedono all'app Web](./media/scenario-webapp/scenario-webapp-signs-in-users.svg)

Come seconda fase è anche possibile consentire all'applicazione di chiamare le API Web per conto dell'utente che ha eseguito l'accesso. Questa fase successiva è uno scenario diverso, disponibile nell' [app Web chiama API Web](scenario-web-app-call-api-overview.md)

> [!NOTE]
> Per aggiungere l'accesso a un'app Web, è necessario proteggere l'app Web e convalidare un token utente, ovvero le librerie **middleware** . Nel caso di .NET, questo scenario non richiede ancora le librerie di autenticazione Microsoft (MSAL), che stanno per acquisire un token per chiamare le API protette. Le librerie di autenticazione verranno introdotte solo nello scenario di completamento quando l'app Web deve chiamare le API Web.

## <a name="specifics"></a>Specifiche

- Durante la registrazione dell'applicazione, è necessario specificarne uno o diversi (se si distribuisce l'app in diversi percorsi) URI di risposta. In alcuni casi (ASP.NET/ASP.NET Core), è necessario abilitare il token ID. Infine, è necessario configurare un URI di disconnessione in modo che l'applicazione reagisca alla sottoscrizione degli utenti.
- Nel codice per l'applicazione, è necessario fornire l'autorità a cui l'app Web delega l'accesso. Potrebbe essere necessario personalizzare la convalida del token (in particolare negli scenari ISV).
- Le applicazioni Web supportano qualsiasi tipo di account. Per altre informazioni, vedere [tipi di account supportati](v2-supported-account-types.md).

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Registrazione delle app](scenario-web-app-sign-user-app-registration.md)
