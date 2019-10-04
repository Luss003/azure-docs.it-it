---
title: Come scegliere il tipo di applicazione da usare durante l'aggiunta di un'applicazione | Microsoft Docs
description: Informazioni sui tipi di applicazioni supportate che è possibile integrare in Azure AD e sulle opzioni di configurazione correlate
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/03/2019
ms.author: mimart
ms.collection: M365-identity-device-management
ROBOTS: NOINDEX
ms.openlocfilehash: d5bd2397c345a4f670bde343f751cd69f825ecb9
ms.sourcegitcommit: ca359c0c2dd7a0229f73ba11a690e3384d198f40
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/17/2019
ms.locfileid: "71056071"
---
# <a name="choosing-the-application-type-when-adding-an-application-in-azure-active-directory"></a>Scelta del tipo di applicazione al momento dell'aggiunta di un'applicazione in Azure Active Directory

Informazioni sui quattro tipi di applicazioni che è possibile aggiungere ad Azure Active Directory (Azure AD). Quando si aggiunge un'applicazione in Azure Active Directory, viene chiesto di scegliere uno tra quattro tipi disponibili.

## <a name="what-are-the-types-of-applications"></a>Cosa sono i tipi di applicazione?

Azure AD supporta quattro principali tipi di applicazione che è possibile aggiungere tramite la funzionalità **Aggiungi** disponibile in **Applicazioni Enterprise**. Sono inclusi:

- **Applicazioni della raccolta di Azure AD**: un'applicazione che è già stata integrata per un accesso Single Sign-On con Azure AD.

- **Applicazioni del proxy applicazioni**: un'applicazione in esecuzione nell'ambiente locale per cui si vuole specificare un punto di accesso singolo esterno.

- **Applicazioni personalizzate**: un'applicazione che l'organizzazione vuole sviluppare nella piattaforma di sviluppo delle applicazioni di Azure AD, ma che non è ancora disponibile.

- **Applicazioni non incluse nella raccolta**: tutte le altre applicazioni. Qualsiasi collegamento Web o qualsiasi applicazione che usa un campo di nome utente e password supporta i protocolli SAML o OpenID Connect oppure supporta SCIM che può essere integrato per un accesso Single Sign-On con Azure AD.

## <a name="features-and-capabilities-supported-by-the-application-types"></a>Caratteristiche e funzionalità supportate dai tipi di applicazione

Le funzionalità seguenti sono supportate dai quattro tipi di applicazione indicati in precedenza in Azure AD:

- **Guida introduttiva** : iniziare a usare rapidamente un'applicazione seguendo [semplici passaggi di distribuzione](https://docs.microsoft.com/azure/active-directory/active-directory-integrating-applications-getting-started)

- **Gestione generale delle proprietà** : consente di ottenere un [collegamento diretto diretto](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) a un'applicazione, [personalizzare la](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-change-app-logo-user-azure-portal) personalizzazione di un'applicazione o [disabilitare l'applicazione](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) per tutti gli utenti.

- **Gestione utenti e gruppi**: consente di [assegnare](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) o [rimuovere](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) utenti e gruppi da un'applicazione e, facoltativamente, assegnare i ruoli specifici dell'applicazione a cui gli utenti e i gruppi hanno accesso

- **Accesso alle applicazioni self-service**: consente agli utenti di richiedere l'[accesso alle applicazioni self-service](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) per un'applicazione dai pannelli di accesso dell'applicazione aggiungendo direttamente un'applicazione o, facoltativamente, [aggiungendola a un gruppo abilitato al self-service](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) che richiede l'approvazione business lungo il percorso

- **Log di accesso**: consentono di vedere [tutti gli accessi a un'applicazione](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins) o tutte le applicazioni

- **Log di controllo**: consentono di vedere i [log di controllo dettagliati sulle modifiche a un'applicazione](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs) o per tutte le applicazioni

- **Accesso condizionale e basato sui rischi**: consente di impostare potenti [regole di accesso basate sulle condizioni](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) che vengono applicate quando gli utenti tentano di accedere a un'applicazione specifica

- **Visualizzazione autorizzazioni** : consente di visualizzare le [autorizzazioni OAuth2](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent) a cui ha accesso un'applicazione nella directory da un unico punto

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a>Modalità Single Sign-On e di provisioning supportate da tipi di applicazione specifici

La tabella seguente descrive le modalità Single Sign-On e di provisioning supportate da ognuno dei tipi di applicazione precedenti. È possibile usare questa tabella per comprendere quale applicazione è necessario aggiungere per supportare un obiettivo specifico.

  ![Tabella: Modalità di provisioning e SSO diverse supportate da ogni tipo di app](./media/choose-application-type/table1.png)

## <a name="how-to-choose-a-single-sign-on-mode"></a>Come scegliere una modalità Single Sign-On

Le modalità **Single Sign-On** supportate per le applicazioni Azure AD sono elencate di seguito.

- **Single Sign-On di Azure AD disabilitato**: scegliere la **modalità Single Sign-On di Azure AD disabilitato** se non si è ancora pronti per integrare questa applicazione con Single Sign-On con Azure AD o semplicemente testandola

- **Accesso collegato**: scegliere la **modalità Single Sing-On**[Accesso collegato](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) se si dispone di un'applicazione che è già connessa con una soluzione Single Sign-On esistente o se si desidera pubblicare un semplice collegamento per gli utenti nel [Pannello di accesso dell'applicazione](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) o nell'[utilità di avvio applicazioni di Office 365](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)

- **Accesso basato su password**: scegliere la **modalità Single Sign-On** [Accesso basato su password](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) se l'applicazione esegue il rendering di un campo di nome utente e password HTML e si desidera archiviare il nome utente e la password in modo sicuro per essere riprodotti all'applicazione in un secondo momento

- **Accesso basato su SAML**: scegliere la modalità Single Sign-On [Accesso basato su SAML](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) se l'applicazione supporta i protocolli SAML oppure OpenID Connect o se si desidera poter eseguire il mapping degli utenti a ruoli specifici dell'applicazione in base alle regole definite nelle attestazioni SAML *

  >[!NOTE]
  >Questa opzione non è disponibile quando il proxy dell'applicazione è configurato per un'applicazione.

- **Accesso basato su intestazione**: scegliere la modalità Single Sign-On [Accesso basato su intestazione](application-proxy-configure-single-sign-on-with-ping-access.md) se si dispone di un'applicazione che usa PingAccess che supporta l'autenticazione basata sull'intestazione HTTP per la quale si vuole eseguire l'accesso Single Sign-On

  >[!NOTE]
  >Questa opzione è disponibile solo quando il proxy dell'applicazione e PingAccess sono configurati per un'applicazione.

- **Autenticazione integrata di Windows**: scegliere la modalità Single Sign-On [Autenticazione integrata di Windows](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) quando si espone un'applicazione WIA locale per la quale si desidera eseguire l'accesso Single Sign-On

  >[!NOTE]
  >Questa opzione è disponibile solo quando il proxy dell'applicazione è configurato per un'applicazione.

## <a name="single-sign-on-modes-for-custom-developed-applications"></a>Modalità Single Sign-On per le applicazioni personalizzate

Le applicazioni personalizzate sviluppate tramite l'esperienza Applicazione personalizzata supportano anche modalità Single Sign-On aggiuntive non elencate in precedenza, tra cui:

- Accesso basato su [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code)

- Accesso basato su [OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code)

- Accesso basato su [WS-Federation 1.2](https://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html)

- Accesso basato su [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference)

Leggere [Guida per gli sviluppatori di Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) per maggiori informazioni su come creare un'applicazione personalizzata che supporta queste modalità Single Sign-On.

## <a name="how-to-set-an-applications-single-sign-on-mode"></a>Come impostare la modalità Single Sign-On di un'applicazione

Per impostare la modalità di Single Sign-On di un'applicazione, seguire queste istruzioni:

1. Aprire il [**portale di Azure**](https://portal.azure.com/) e accedere come **Amministratore globale** o **Coamministratore**.
1. Aprire l'**estensione Azure Active Directory** facendo clic su **Tutti i servizi** nella parte superiore del menu di spostamento principale a sinistra.
1. Digitare "**Azure Active Directory**" nella casella di ricerca filtro e selezionare l'elemento **Azure Active Directory**.
1. Fare clic su **Applicazioni aziendali** nel menu di spostamento di sinistra di Azure Active Directory.
1. Fare clic su **Tutte le applicazioni** per visualizzare un elenco di tutte le applicazioni.

   * Se l'applicazione non è inclusa nell'elenco, usare il controllo **Filtro** all'inizio dell'elenco **Tutte le applicazioni** e impostare l'opzione **Mostra** su **Tutte le applicazioni**.

1. Selezionare l'applicazione per cui si desidera configurare l'accesso Single Sign-on.
1. Dopo il caricamento dell'applicazione, fare clic su **Single Sign-On** nel menu di spostamento di sinistra dell'applicazione.

## <a name="how-to-choose-a-provisioning-mode"></a>Come scegliere una modalità di provisioning

- **Provisioning manuale**: scegliere la modalità di provisioning [manuale](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#provisioning-modes) se si dispone di account esistenti o si desidera gestire gli account per l'applicazione all'esterno di Azure AD.

- **Provisioning automatico**: scegliere la **modalità di provisioning** [automatica](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#configuring-automatic-user-account-provisioning)se si desidera abilitare il provisioning automatico basato su API e/o il deprovisioning degli account utente per questa applicazione 

- **Provisioning automatico basato su SCIM**: usare il [provisioning automatico basato su SCIM](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning) se un'applicazione supporta il protocollo SCIM per il rilevamento di modifiche a utenti e gruppi, emessi automaticamente per le modifiche a tutte le applicazioni integrate in Azure AD 

  >[!NOTE]
  >Questa opzione non è elencata come una modalità specifica di provisioning, ma è abilitata per impostazione predefinita per tutte le applicazioni integrate in Azure AD.

## <a name="how-to-set-an-applications-provisioning-mode"></a>Come impostare la modalità di provisioning di un'applicazione

Per impostare la modalità di **provisioning** di un'applicazione, seguire queste istruzioni:

1. Aprire il [**portale di Azure**](https://portal.azure.com/) e accedere come **Amministratore globale** o **Coamministratore**.
1. Aprire l'**estensione Azure Active Directory** facendo clic su **Tutti i servizi** nella parte superiore del menu di spostamento principale a sinistra.
1. Digitare "**Azure Active Directory**" nella casella di ricerca filtro e selezionare l'elemento **Azure Active Directory**.
1. Fare clic su **Applicazioni aziendali** nel menu di spostamento di sinistra di Azure Active Directory.
1. Fare clic su **Tutte le applicazioni** per visualizzare un elenco di tutte le applicazioni.

   * Se l'applicazione non è inclusa nell'elenco, usare il controllo **Filtro** all'inizio dell'elenco **Tutte le applicazioni** e impostare l'opzione **Mostra** su **Tutte le applicazioni**.

1. Selezionare l'applicazione per cui si desidera configurare il provisioning.
1. Dopo il caricamento dell'applicazione, fare clic su **Provisioning** nel menu di spostamento di sinistra dell'applicazione.

## <a name="next-steps"></a>Passaggi successivi

[Gestione di applicazioni con Azure Active Directory](what-is-application-management.md)
