---
title: Personalizzare la reimpostazione della password self-service-Azure Active Directory
description: Opzioni di personalizzazione per la reimpostazione della password self-service di Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0dfd035f73ea529ddb55bac6ce601185fda51a4d
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/22/2019
ms.locfileid: "74381930"
---
# <a name="customize-the-azure-ad-functionality-for-self-service-password-reset"></a>Personalizzare la funzionalità di Azure AD per la reimpostazione della password self-service

I professionisti IT che vogliono distribuire la reimpostazione della password self-service in Azure Active Directory (Azure AD) possono personalizzare l'esperienza in modo che corrisponda alle esigenze dei propri utenti.

## <a name="customize-the-contact-your-administrator-link"></a>Personalizzare il collegamento "Contatta l'amministratore"

Per gli utenti con reimpostazione della password self-service è disponibile un collegamento "contattare l'amministratore" nel portale di reimpostazione della password. Se un utente seleziona questo collegamento, effettuerà una delle due operazioni seguenti:

* Se lasciato nello stato predefinito:
   * Viene inviato un messaggio di posta elettronica agli amministratori e viene chiesto di fornire assistenza per la modifica della password dell'utente. Vedere il [messaggio di posta elettronica di esempio](#sample-email) seguente.
* Se personalizzato:
   * Invia l'utente a una pagina Web o a un indirizzo di posta elettronica specificato dall'amministratore per assistenza.

> [!TIP]
> Se si Personalizza questa impostazione, è consigliabile impostarla su un elemento con cui gli utenti hanno già familiarità per il supporto

> [!WARNING]
> Se si Personalizza questa impostazione con un indirizzo di posta elettronica e un account che richiedono la reimpostazione della password, l'utente potrebbe non essere in grado di richiedere assistenza.

### <a name="sample-email"></a>Esempio di messaggio di posta elettronica

![Richiesta di esempio per reimpostare la posta elettronica inviata all'amministratore][Contact]

Questo contatto di posta elettronica viene inviato ai destinatari seguenti nell'ordine seguente:

1. Se viene assegnato il ruolo **Amministratore password**, agli amministratori con questo ruolo viene inviata una notifica.
2. Se non è stato assegnato nessun amministratore password, la notifica verrà inviata agli amministratori che hanno il ruolo **Amministratore utenti**.
3. Se nessuno dei ruoli precedenti è assegnato, la notifica verrà inviata agli **Amministratori globali**.

In tutti i casi, viene inviata una notifica a un massimo di 100 destinatari totali.

Per altre informazioni sui diversi ruoli di amministratore e su come assegnarli, vedere [Assegnazione dei ruoli di amministratore in Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md).

### <a name="disable-contact-your-administrator-emails"></a>Disabilitare i messaggi di posta elettronica "Contatta l'amministratore"

Se l'organizzazione non vuole inviare notifiche agli amministratori circa le richieste di reimpostazione della password, è possibile abilitare la configurazione seguente:

* Abilitare la reimpostazione self-service delle password per tutti gli utenti finali. Questa opzione è in **Reimpostazione password** > **Proprietà**. Se non si vuole che gli utenti reimpostino le proprie password, è possibile definire l'ambito di accesso a un gruppo vuoto. *Questa opzione non è consigliata.*
* Personalizzare il collegamento al supporto tecnico per fornire un URL Web o mailto: indirizzo che gli utenti possono usare per ottenere assistenza. Questa opzione si trova in **Reimpostazione password** > **Personalizzazione** > **Indirizzo di posta elettronica o URL del supporto tecnico**.

## <a name="customize-the-ad-fs-sign-in-page-for-sspr"></a>Personalizzare la pagina di accesso di AD FS per la reimpostazione della password self-service

Gli amministratori di Active Directory Federation Services (ADFS) possono aggiungere un collegamento alla pagina di accesso usando le indicazioni contenute nell'articolo [Aggiungere una descrizione della pagina di accesso](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description).

Per aggiungere un collegamento alla pagina di accesso di AD FS, usare il comando seguente nel server AD FS. Gli utenti possono usare questa pagina per immettere il flusso di lavoro di reimpostazione della password self-service.

``` powershell
Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href='https://passwordreset.microsoftonline.com' target='_blank'>Can’t access your account?</A></p>"
```

## <a name="customize-the-sign-in-page-and-access-panel-look-and-feel"></a>Personalizzare l'aspetto della pagina di accesso e del pannello di accesso

È possibile personalizzare la pagina di accesso. È possibile aggiungere un logo da visualizzare con l'immagine appropriata per le informazioni personalizzate distintive dell'azienda.

Gli elementi grafici scelti vengono visualizzati nelle circostanze seguenti:

* Dopo che l'utente immette il proprio nome utente
* Se l'utente accede all'URL personalizzato:
   * Passando il parametro `whr` alla pagina di reimpostazione della password, ad esempio `https://login.microsoftonline.com/?whr=contoso.com`
   * Passando il parametro `username` alla pagina di reimpostazione della password, ad esempio `https://login.microsoftonline.com/?username=admin@contoso.com`

Per Informazioni dettagliate su come configurare le informazioni personalizzate distintive dell'azienda, vedere l'articolo [Aggiungere informazioni personalizzate distintive dell'azienda alla pagina di accesso](../fundamentals/customize-branding.md).

### <a name="directory-name"></a>Nome della directory

È possibile modificare l'attributo del nome della directory in **Azure Active Directory** > **Proprietà**. È possibile visualizzare un nome descrittivo per l'organizzazione nel portale e nelle comunicazioni automatizzate. Questa opzione è la più visibile nei messaggi di posta elettronica automatizzati nei formati seguenti:

* Nome descrittivo nel messaggio di posta elettronica, ad esempio "Microsoft per conto di demo CONTOSO"
* Riga dell'oggetto nel messaggio di posta elettronica, ad esempio "Codice di verifica dell'indirizzo di posta elettronica dell'account demo CONTOSO"

## <a name="next-steps"></a>Passaggi successivi

* [Come completare l'implementazione della reimpostazione della password self-service per gli utenti](howto-sspr-deployment.md)
* [Reimpostare o modificare la password](../user-help/active-directory-passwords-update-your-own-password.md)
* [Registrarsi per la reimpostazione della password self-service](../user-help/active-directory-passwords-reset-register.md)
* [Domande sulle licenze](concept-sspr-licensing.md)
* [Dati usati dalla reimpostazione della password self-service e dati da immettere per gli utenti](howto-sspr-authenticationdata.md)
* [Metodi di autenticazione disponibili per gli utenti](concept-sspr-howitworks.md#authentication-methods)
* [Opzioni dei criteri per la reimpostazione della password self-service](concept-sspr-policy.md)
* [Panoramica del writeback delle password](howto-sspr-writeback.md)
* [Come creare un report sull'attività relativa alla reimpostazione della password self-service](howto-sspr-reporting.md)
* [Informazioni sulle opzioni della reimpostazione della password self-service](concept-sspr-howitworks.md)
* [Credo che qualcosa sia rotto. Ricerca per categorie risolvere i problemi di SSPR?](active-directory-passwords-troubleshoot.md)
* [Altre informazioni non illustrate altrove](active-directory-passwords-faq.md)

[Contact]: ./media/concept-sspr-customization/sspr-contact-admin.png "Contattare l'amministratore per assistenza per la reimpostazione dell'esempio di indirizzo di posta elettronica della password"
