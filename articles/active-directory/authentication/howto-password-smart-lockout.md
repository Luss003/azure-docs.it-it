---
title: Prevenzione degli attacchi di forza bruta usando Azure AD blocco intelligente-Azure Active Directory
description: La funzione di blocco intelligente di Azure Active Directory consente di proteggere le organizzazioni da eventuali attacchi di forza bruta costituiti da tentativi di indovinare le password
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: a762009a7aaf1a965333ac573efe55d792c3f04b
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2019
ms.locfileid: "70125011"
---
# <a name="azure-active-directory-smart-lockout"></a>Blocco intelligente di Azure Active Directory

La funzione di blocco intelligente consente di bloccare gli attori malintenzionati che tentano di indovinare le password degli utenti o usano metodi di forza bruta per ottenere l'accesso. Questa intelligenza è in grado di riconoscere i tentativi di accesso eseguiti da utenti validi e di gestirli in modo diverso da quelli di utenti malintenzionati e di altre origini sconosciute. La funzione di blocco intelligente blocca gli utenti malintenzionati e al tempo stesso consente a quelli validi di accedere ai propri account senza effetti negativi sulla produttività.

Per impostazione predefinita, dopo dieci tentativi non riusciti la funzione di blocco intelligente blocca l'account da nuovi tentativi di accesso per un minuto. Dopo ogni successivo tentativo di accesso non riuscito, l'account viene nuovamente bloccato prima per un minuto e quindi per intervalli più lunghi.

Il blocco intelligente tiene traccia degli ultimi tre hash delle password non validi per evitare l'incremento del contatore dei blocchi per la stessa password. Se un utente immette la stessa password errata più volte, questo comportamento non comporterà il blocco dell'account.

 > [!NOTE]
 > La funzionalità di rilevamento hash non è disponibile per i clienti con autenticazione pass-through abilitata, poiché l'autenticazione avviene in locale e non nel cloud.

Le distribuzioni federate con AD FS 2016 e AF FS 2019 possono offrire vantaggi simili utilizzando il [blocco ad FS Extranet e il blocco Smart Extranet](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-smart-lockout-protection).

Il blocco intelligente è sempre attivo per tutti i clienti di Azure AD con le impostazioni predefinite che offrono la giusta combinazione di sicurezza e usabilità. Per personalizzare le impostazioni di blocco Smart, con valori specifici per l'organizzazione, è necessario disporre di licenze a pagamento Azure AD per gli utenti.

L'uso del blocco intelligente non garantisce che un utente originale non venga mai bloccato. Quando il blocco intelligente blocca un account utente, viene eseguito un tentativo di non bloccare l'utente originale. Il servizio di blocco cerca di garantire che gli utenti non autorizzati non possano accedere all'account degli utenti originali.  

* Ogni data center di Azure Active Directory tiene traccia del blocco in modo indipendente. Un utente disporrà di un numero di tentativi pari a (limite_soglia * conteggio_datacenter), se l'utente esegue l'accesso a ogni data center.
* Il blocco intelligente usa la differenziazione tra posizioni conosciute e sconosciute per individuare gli utenti originali e distinguerli da quelli non autorizzati. Le posizioni conosciute e quelle sconosciute disporranno entrambe di contatori di blocco distinti.

Il blocco intelligente può essere integrato con distribuzioni ibride, tramite la sincronizzazione degli hash delle password o l'autenticazione pass-through, per impedire il blocco degli account locali di Active Directory da parte di utenti malintenzionati. Impostando opportuni criteri di blocco intelligente in Azure AD, è possibile filtrare gli attacchi prima che raggiungano l'istanza locale di Active Directory.

Quando si usa l'[autenticazione pass-through](../hybrid/how-to-connect-pta.md), è necessario verificare quanto segue:

* La soglia di blocco di Azure AD è **inferiore** alla soglia di blocco dell'account Active Directory. Configurare i valori in modo che la soglia di blocco dell'account Active Directory sia almeno due o tre volte superiore rispetto alla soglia di blocco di Azure AD. 
* La durata del blocco di Azure AD deve essere impostata più a lungo rispetto al contatore del blocco dell'account di Active Directory reset dopo la durata. Tenere presente che la durata del Azure AD è impostata in secondi, mentre la durata di Active Directory è impostata in minuti. 

Se ad esempio si vuole che il contatore di Azure AD sia maggiore di AD, Azure AD sarà di 120 secondi (2 minuti) mentre Active Directory locale è impostato su 1 minuto (60 secondi).

> [!IMPORTANT]
> Attualmente, un amministratore non può sbloccare gli account cloud degli utenti se questi sono stati bloccati dalla funzionalità di blocco Smart. L'amministratore deve attendere la scadenza della durata del blocco. Tuttavia, l'utente può sbloccare usando la reimpostazione della password self-service (SSPR) da una posizione o un dispositivo attendibile.

## <a name="verify-on-premises-account-lockout-policy"></a>Verificare i criteri di blocco degli account locali

Per verificare i criteri di blocco degli account di Active Directory, usare le istruzioni seguenti:

1. Aprire lo strumento Gestione criteri di gruppo.
2. Modificare i criteri di gruppo che includono i criteri di blocco degli account dell'organizzazione, ad esempio i **criteri di dominio predefiniti**.
3. Passare a **Configurazione computer** > **Criteri** > **Impostazioni di Windows** > **Impostazioni di sicurezza** > **Criteri account** > **Criterio di blocco account**.
4. Verificare i valori di **Soglia di blocchi dell'account** e **Reimposta contatore blocco account dopo**.

![Modificare i criteri di blocco degli account Active Directory locali](./media/howto-password-smart-lockout/active-directory-on-premises-account-lockout-policy.png)

## <a name="manage-azure-ad-smart-lockout-values"></a>Gestire i valori del blocco intelligente di Azure AD

In base alle esigenze dell'organizzazione, può essere necessario personalizzare i valori del blocco intelligente. Per personalizzare le impostazioni di blocco Smart, con valori specifici per l'organizzazione, è necessario disporre di licenze a pagamento Azure AD per gli utenti.

Per verificare o modificare i valori del blocco intelligente per l'organizzazione, seguire questa procedura:

1. Accedere al [portale di Azure](https://portal.azure.com) e passare a **Azure Active Directory** > **Metodi** > di autenticazione**Password Protection**.
1. Impostare il valore di **Soglia di blocco**, in base al numero di accessi non riusciti consentiti per un account prima che venga applicato il primo blocco. Il valore predefinito è 10.
1. Impostare il valore di **Durata del blocco in secondi** sulla durata in secondi di ogni blocco. Il valore predefinito è 60 secondi (1 minuto).

> [!NOTE]
> Se anche il primo accesso dopo un blocco non riesce, l'account viene bloccato nuovamente. Se un account viene bloccato più volte, la durata del blocco aumenta.

![Personalizzare i criteri di blocco intelligente di Azure Active Directory nel portale di Azure](./media/howto-password-smart-lockout/azure-active-directory-custom-smart-lockout-policy.png)

## <a name="how-to-determine-if-the-smart-lockout-feature-is-working-or-not"></a>Come determinare se la funzionalità di blocco Smart funziona o meno

Quando viene attivata la soglia di blocco Smart, si riceverà il messaggio seguente quando l'account è bloccato:

**L'account è stato temporaneamente bloccato per impedirne l'uso non autorizzato. Riprovare più tardi. Se il problema persiste, contattare l'amministratore.**

## <a name="next-steps"></a>Passaggi successivi

* [Leggere le informazioni su come escludere le password non valide tramite Azure AD](howto-password-ban-bad.md)
* [Configurare la reimpostazione di password self-service per consentire agli utenti di sbloccare i relativi account](quickstart-sspr.md)
