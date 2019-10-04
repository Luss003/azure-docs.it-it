---
title: Inserire l'applicazione nella raccolta di applicazioni di Azure Active Directory | Microsoft Docs
description: Informazioni su come inserire un'applicazione che supporta l'accesso Single Sign-On nella raccolta di app di Azure Active Directory
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/16/2019
ms.author: ryanwi
ms.reviewer: elisol, bryanla
ms.custom: aaddev, seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: 88d74fe794f4de95b7ba8b0dd5575ca56d2016e5
ms.sourcegitcommit: 83df2aed7cafb493b36d93b1699d24f36c1daa45
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2019
ms.locfileid: "71176919"
---
# <a name="how-to-list-your-application-in-the-azure-active-directory-application-gallery"></a>Procedura: Inserire l'applicazione nella raccolta di applicazioni di Azure Active Directory

Questo articolo illustra come elencare un'applicazione nella raccolta di applicazioni di Azure AD, implementare l'accesso Single Sign-on (SSO) e gestire l'elenco.

## <a name="what-is-the-azure-ad-application-gallery"></a>Definizione della raccolta di applicazioni di Azure AD

- Esperienza Single Sign-On ottimale per i clienti.
- Configurazione minima e semplice dell'applicazione.
- Ricerca rapida per l'individuazione dell'applicazione nella raccolta.
- Uso di questa integrazione per tutti i clienti con account Free, Basic e Premium di Azure AD.
- Esercitazione dettagliata per la configurazione per i clienti reciproci.
- Provisioning per la stessa app per i clienti che usano SCIM.

## <a name="prerequisites"></a>Prerequisiti

- Per le applicazioni federate (Open ID e SAML/WS-Fed), l'applicazione deve supportare il modello SaaS per entrare a far parte della raccolta di Azure AD. Le applicazioni della raccolta enterprise devono supportare più configurazioni dei clienti e non un cliente specifico.

- Per Open ID Connect, l'applicazione deve essere multi-tenant e il [framework di consenso di Azure AD](consent-framework.md) deve essere implementato correttamente per l'applicazione. L'utente può inviare la richiesta di accesso all'endpoint comune, in modo che qualsiasi cliente possa fornire il consenso all'applicazione. È possibile controllare l'accesso utente in base all'ID del tenant e all'UPN dell'utente ricevuti nel token.

- Per SAML 2.0/WS-Fed l'applicazione deve supportare l'integrazione SSO SAML/WS-Fed in modalità SP o IDP. Assicurarsi che questa funzioni correttamente prima di inviare la richiesta.

- Per l'accesso SSO con password, assicurarsi che l'applicazione supporti l'autenticazione basata su modulo in modo che sia possibile eseguire l'insieme di credenziali delle password affinché l'accesso SSO funzioni come previsto.

- È necessario un account permanente per il test con almeno 2 utenti registrati.

## <a name="submit-the-request-in-the-portal"></a>Inviare la richiesta nel portale

Dopo aver verificato il funzionamento dell'integrazione dell'applicazione con Azure AD, inviare la richiesta per l'accesso nel [portale di rete delle applicazioni](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Se si dispone di un account Office 365, usarlo per accedere a questo portale. In caso contrario, accedere con l'account Microsoft (ad esempio, Outlook o Hotmail).

Se dopo l'accesso viene visualizzata la pagina seguente, contattare il [team di integrazione SSO di Azure AD](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) e specificare l'account e-mail che si vuole usare per inviare la richiesta. Il team di Azure AD aggiungerà quindi l'account nel portale di rete delle applicazioni Microsoft.

![Richiesta di accesso nel portale di SharePoint](./media/howto-app-gallery-listing/errorimage.png)

Dopo aver aggiunto l'account, è possibile accedere al portale di rete delle applicazioni Microsoft.

Se dopo l'accesso viene visualizzata la pagina seguente, specificare una motivazione aziendale per la richiesta di accesso nella casella di testo e quindi selezionare **Richiedi accesso**.

  ![Richiesta di accesso nel portale di SharePoint](./media/howto-app-gallery-listing/accessrequest.png)

Il team esamina i dettagli e fornisce l'accesso di conseguenza. Dopo l'approvazione della richiesta, è possibile accedere al portale e inviare la richiesta facendo clic sul riquadro **Submit Request (ISV)** (Invia richiesta - ISV) nella home page.

![Home page del portale SharePoint](./media/howto-app-gallery-listing/homepage.png)

> [!NOTE]
> In caso di problemi di accesso, contattare il [team di integrazione SSO di Azure AD](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

## <a name="implementing-sso-using-federation-protocol"></a>Implementazione dell'SSO usando il protocollo di federazione

Per inserire un'applicazione nella raccolta di app di Azure AD, è innanzitutto necessario implementare uno dei protocolli di federazione seguenti supportati da Azure AD e accettare i termini e condizioni della raccolta di applicazioni di Azure AD. Leggere [qui](https://azure.microsoft.com/support/legal/active-directory-app-gallery-terms/) le condizioni della raccolta di applicazioni di Azure AD.

- **OpenID Connect**: per integrare l'applicazione con Azure AD mediante il protocollo Open ID Connect, seguire le [istruzioni per gli sviluppatori](authentication-scenarios.md).

    ![Sequenza temporale dell'inserimento di un'applicazione OpenID Connect nella raccolta](./media/howto-app-gallery-listing/openid.png)

    * Se si desidera aggiungere l'applicazione all'elenco nella raccolta usando OpenID Connect, selezionare **OpenID Connect & OAuth 2.0** (OpenID Connect e OAuth 2.0) come indicato in precedenza.
    * In caso di problemi di accesso, contattare il [team di integrazione SSO di Azure AD](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

- **SAML 2.0** o **WS-Fed**: Se l'app supporta SAML 2.0, può essere integrata direttamente con un tenant di Azure AD usando le [istruzioni per l'aggiunta di un'applicazione personalizzata](../active-directory-saas-custom-apps.md).

  ![Sequenza temporale per l'inserimento di un'applicazione SAML 2.0 o WS-Fed nella raccolta](./media/howto-app-gallery-listing/saml.png)

  * Se si desidera aggiungere l'applicazione all'elenco nella raccolta usando **SAML 2.0** o **WS-Fed**, selezionare **SAMl 2.0/WS-Fed** come indicato in precedenza.
  * In caso di problemi di accesso, contattare il [team di integrazione SSO di Azure AD](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

## <a name="implementing-sso-using-password-sso"></a>Implementazione dell'SSO con l'accesso SSO con password

Creare un'applicazione Web con una pagina di accesso HTML per configurare l'[accesso Single Sign-On basato su password](../manage-apps/what-is-single-sign-on.md). L'SSO basato su password, definito anche insieme di credenziali delle password, consente di gestire l'accesso degli utenti e le password per le applicazioni Web che non supportano la federazione delle identità. Risulta utile anche negli scenari in cui più utenti devono condividere un unico account, ad esempio agli account di app di social media dell'organizzazione.

![Sequenza temporale per l'inserimento di un'applicazione SSO con password nella raccolta](./media/howto-app-gallery-listing/passwordsso.png)

* Se si desidera aggiungere l'applicazione all'elenco nella raccolta usando SSO con password selezionare **Password SSO** (SSO con password) come indicato in precedenza.
* In caso di problemi di accesso, contattare il [team di integrazione SSO di Azure AD](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

## <a name="requesting-for-user-provisioning"></a>Richiesta di provisioning utenti

Seguire il processo seguente per richiedere il provisioning dell'utente-

   ![Tempistica per la presentazione di un'applicazione SAML nella raccolta](./media/howto-app-gallery-listing/user-provisioning.png)

## <a name="updateremove-existing-listing"></a>Aggiornare e rimuovere un elenco esistente

Per aggiornare o rimuovere un'applicazione esistente nella raccolta di app di Azure AD, è prima necessario inviare la richiesta nel [portale di rete delle applicazioni](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Se si dispone di un account Office 365, usarlo per accedere a questo portale. In caso contrario, accedere con l'account Microsoft (ad esempio, Outlook o Hotmail).

- Selezionare l'opzione appropriata come illustrato nell'immagine seguente:

    ![Tempistica per la presentazione di un'applicazione SAML nella raccolta](./media/howto-app-gallery-listing/updateorremove.png)

    * Se si desidera aggiornare un'applicazione esistente, selezionare l'opzione appropriata in base ai requisiti.
    * Se si vuole rimuovere un'applicazione esistente dalla raccolta di Azure AD, selezionare **Rimuovi l'elenco di applicazioni dalla raccolta**.
    * In caso di problemi di accesso, contattare il [team di integrazione SSO di Azure AD](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

## <a name="listing-requests-by-customers"></a>Elenco di richieste da clienti

I clienti possono inviare la richiesta di elencare un'applicazione facendo clic su **richieste di app da** -> parte dei clienti per**inviare una nuova richiesta**.

![Mostra il riquadro app richieste dal cliente](./media/howto-app-gallery-listing/customer-submit-request.png)

Di seguito è riportato il flusso di applicazioni richieste dal cliente:

![Mostra il flusso delle app richieste dal cliente](./media/howto-app-gallery-listing/customer-request.png)

## <a name="timelines"></a>Tempistica

La tempistica del processo di inserimento di un'applicazione SAML 2.0 o WS-Fed nella raccolta è di 7-10 giorni lavorativi.

  ![Sequenza temporale dell'elenco di applicazioni SAML nella raccolta](./media/howto-app-gallery-listing/timeline.png)

La tempistica del processo di inserimento di un'applicazione OpenID Connect nella raccolta è di 2-5 giorni lavorativi.

  ![Sequenza temporale dell'elenco di applicazioni SAML nella raccolta](./media/howto-app-gallery-listing/timeline2.png)

## <a name="escalations"></a>Escalation

Per le escalation, inviare un messaggio di posta elettronica al [team di integrazione SSO di Azure AD](mailto:SaaSApplicationIntegrations@service.microsoft.com) (SaaSApplicationIntegrations@service.microsoft.com) che fornirà assistenza nel più breve tempo possibile.
