---
title: Creazione di un criterio di accesso condizionale-Azure Active Directory
description: Quali sono tutte le opzioni disponibili per la creazione di criteri di accesso condizionale e cosa significano?
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 02/11/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2d2ebcc885b4018f4d9c3ff1b525ffc19b1abdda
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78671935"
---
# <a name="building-a-conditional-access-policy"></a>Creazione di un criterio di accesso condizionale

Come illustrato nell'articolo relativo all' [accesso condizionale](overview.md), i criteri di accesso condizionale sono un'istruzione if-then, di **assegnazioni** e **controlli di accesso**. I criteri di accesso condizionale riuniscono i segnali, per prendere decisioni e applicare i criteri dell'organizzazione.

In che modo un'organizzazione crea questi criteri? Cosa è necessario?

![Accesso condizionale (segnali + decisioni + applicazione = criteri)](./media/concept-conditional-access-policies/conditional-access-signal-decision-enforcement.png)

## <a name="assignments"></a>Assegnazioni

La parte relativa alle assegnazioni controlla gli utenti, gli elementi e i criteri di accesso condizionale.

### <a name="users-and-groups"></a>Utenti e gruppi

[Utenti e gruppi](concept-conditional-access-users-groups.md) assegnano i criteri da includere o escludere. Questa assegnazione può includere tutti gli utenti, gruppi specifici di utenti, ruoli della directory o utenti Guest esterni. 

### <a name="cloud-apps-or-actions"></a>App o azioni cloud

Le [app o le azioni cloud](concept-conditional-access-cloud-apps.md) possono includere o escludere le applicazioni cloud o le azioni dell'utente che saranno soggette ai criteri.

### <a name="conditions"></a>Condizioni

Un criterio può contenere più [condizioni](concept-conditional-access-conditions.md).

#### <a name="sign-in-risk"></a>Rischio di accesso

Per le organizzazioni con [Azure ad Identity Protection](../identity-protection/overview.md), i rilevamenti dei rischi generati possono influenzare i criteri di accesso condizionale.

#### <a name="device-platforms"></a>Piattaforme del dispositivo

Le organizzazioni con più piattaforme del sistema operativo per dispositivi potrebbero voler applicare criteri specifici su piattaforme diverse. 

Le informazioni utilizzate per calcolare la piattaforma del dispositivo provengono da origini non verificate, ad esempio stringhe agente utente che possono essere modificate.

#### <a name="locations"></a>Posizioni

I dati relativi alla posizione sono forniti dai dati di georilevazione IP. Gli amministratori possono scegliere di definire le località e scegliere di contrassegnare alcuni come attendibili come quelli per i percorsi di rete dell'organizzazione.

#### <a name="client-apps"></a>App client

Per impostazione predefinita, i criteri di accesso condizionale si applicano a app browser, app per dispositivi mobili e client desktop che supportano l'autenticazione moderna. 

Questa condizione di assegnazione consente ai criteri di accesso condizionale di destinare applicazioni client specifiche che non usano l'autenticazione moderna. Queste applicazioni includono i client Exchange ActiveSync, le applicazioni di Office meno recenti che non usano l'autenticazione moderna e protocolli di posta elettronica come IMAP, MAPI, POP e SMTP.

#### <a name="device-state"></a>Stato del dispositivo

Questo controllo viene usato per escludere i dispositivi ibridi Azure AD Uniti o contrassegnati come conformi in Intune. Questa esclusione può essere eseguita per bloccare i dispositivi non gestiti. 

## <a name="access-controls"></a>Controlli di accesso

La parte controlli di accesso dei criteri di accesso condizionale controlla il modo in cui viene applicato un criterio.

### <a name="grant"></a>Concedi

[Grant](concept-conditional-access-grant.md) fornisce agli amministratori un metodo per l'applicazione dei criteri, in cui è possibile bloccare o concedere l'accesso.

#### <a name="block-access"></a>Blocca accesso

Blocca l'accesso consente di bloccare l'accesso in base alle assegnazioni specificate. Il controllo del blocco è potente e deve essere maneggiato con le informazioni appropriate.

#### <a name="grant-access"></a>Concedi accesso

Il controllo Grant può attivare l'applicazione di uno o più controlli. 

- Richiedi autenticazione a più fattori (Multi-Factor Authentication di Azure)
- Richiedi che il dispositivo sia contrassegnato come conforme (Intune)
- Richiedi dispositivo aggiunto ad Azure AD ibrido
- Richiedere app client approvata
- Richiedere criteri di protezione dell'app

Gli amministratori possono scegliere di richiedere uno dei controlli precedenti o tutti i controlli selezionati usando le opzioni seguenti. Il valore predefinito per più controlli consiste nel richiedere all.

- Richiedi tutti i controlli selezionati (controllo e controllo)
- Richiedi uno dei controlli selezionati (controllo o controllo)

### <a name="session"></a>Sessione

I [controlli della sessione](concept-conditional-access-session.md) possono limitare l'esperienza 

- Usa restrizioni imposte dalle app
   - Attualmente funziona solo con Exchange Online e SharePoint Online.
      - Passa informazioni sul dispositivo per consentire il controllo dell'esperienza di concessione di accesso completo o limitato.
- Usa Controllo app per l'accesso condizionale
   - USA i segnali di Microsoft Cloud App Security per eseguire operazioni come: 
      - Blocca il download, il taglio, la copia e la stampa di documenti riservati.
      - Monitorare il comportamento della sessione rischiosa.
      - Richiedere l'assegnazione di etichette ai file riservati.
- Frequenza di accesso
   - Possibilità di modificare la frequenza di accesso predefinita per l'autenticazione moderna.
- Sessione del browser persistente
   - Consente agli utenti di rimanere connessi dopo la chiusura e la riapertura della finestra del browser.

## <a name="simple-policies"></a>Criteri semplici

Un criterio di accesso condizionale deve contenere almeno quanto segue per essere applicato:

- **Nome** dei criteri.
- **Assegnazioni**
   - **Utenti e/o gruppi** ai quali applicare i criteri.
   - **App Cloud o azioni** a cui applicare i criteri.
- **Controlli di accesso**
   - **Concedere** o **bloccare** i controlli

![Criteri di accesso condizionale vuoti](./media/concept-conditional-access-policies/conditional-access-blank-policy.png)

L'articolo [criteri di accesso condizionale comuni](concept-conditional-access-policy-common.md) include alcuni criteri che riteniamo utili per la maggior parte delle organizzazioni.

## <a name="next-steps"></a>Passaggi successivi

[Simulare il comportamento di accesso usando lo strumento di What If dell'accesso condizionale](troubleshoot-conditional-access-what-if.md)

[Pianificazione di una distribuzione di Azure Multi-Factor Authentication basata sul cloud](../authentication/howto-mfa-getstarted.md)

[Gestione della conformità dei dispositivi con Intune](/intune/device-compliance-get-started)

[Microsoft Cloud App Security e accesso condizionale](/cloud-app-security/proxy-intro-aad)
