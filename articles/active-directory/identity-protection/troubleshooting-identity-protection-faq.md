---
title: Domande frequenti sulla protezione delle identità in Azure Active Directory
description: Domande frequenti Azure AD Identity Protection
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: troubleshooting
ms.date: 12/13/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 140ad45d9c4f6b6f49a4ea4aefb9298e58a2cf10
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75443579"
---
# <a name="frequently-asked-questions-identity-protection-in-azure-active-directory"></a>Domande frequenti sulla protezione delle identità in Azure Active Directory

## <a name="dismiss-user-risk-known-issues"></a>Ignora problemi noti relativi al rischio utente

**Ignorare i rischi** per gli utenti in classica Identity Protection imposta l'attore nella cronologia dei rischi dell'utente in Identity protection per **Azure ad**.

**Ignorare i rischi** per gli utenti in Identity Protection imposta l'attore nella cronologia dei rischi dell'utente in Identity protection per **\<nome dell'amministratore con un collegamento ipertestuale che punta al pannello dell'utente\>** .

Si è verificato un problema noto corrente che causa la latenza nel flusso di rischio dell'utente. Se si hanno "criteri di rischio utente", questi criteri smetteranno di venire applicati agli utenti rimossi entro alcuni minuti da quando si fa clic su "Ignora rischio utente". Tuttavia, ci sono ritardi noti nell'esperienza utente di aggiornamento dello "stato del rischio" degli utenti rimossi. Come soluzione alternativa, aggiornare la pagina a livello di browser per visualizzare lo "stato del rischio" più recente per l'utente.

## <a name="risky-users-report-known-issues"></a>Segnalazione di problemi noti da parte degli utenti rischiosi

Le query sul campo del **nome utente** fanno distinzione tra maiuscole e minuscole, mentre le query sul campo del **Nome** non fanno distinzione tra maiuscole e minuscole.

Attivando **Visualizza date come** viene nascosta la colonna **ULTIMO AGGIORNAMENTO RISCHIO**. Per aggiungere nuovamente la colonna fare clic su **Colonne** nella parte superiore del pannello Utenti a rischio.

**Ignora tutti gli eventi** in classica Identity Protection imposta lo stato dei rilevamenti dei rischi su **chiuso (risolto)** .

## <a name="risky-sign-ins-report-known-issues"></a>Problemi noti segnalati dagli accessi a rischio

**Risolvi** in un rilevamento dei rischi imposta lo stato sugli utenti che hanno superato l'autenticazione a più fattori **basata sui criteri basati sul rischio**.

## <a name="frequently-asked-questions"></a>Domande frequenti

### <a name="why-is-a-user-is-at-risk"></a>Perché un utente è a rischio?

Se si è un cliente Azure AD Identity Protection, passare alla visualizzazione [utenti a rischio](howto-identity-protection-investigate-risk.md#risky-users) e fare clic su un utente a rischio. Nel cassetto nella parte inferiore della scheda ' cronologia dei rischi ' visualizzerà tutti gli eventi che hanno portato a una modifica dei rischi dell'utente. Per visualizzare tutti gli accessi a rischio per l'utente, fare clic su "accessi a rischio utente". Per visualizzare tutti i rilevamenti dei rischi per questo utente, fare clic su "rilevamento rischi utente".

### <a name="how-can-i-get-a-report-of-detections-of-a-specific-type"></a>Come è possibile ottenere un report dei rilevamenti di un tipo specifico?

Passare alla visualizzazione rilevamento dei rischi e filtrare per "tipo di rilevamento". È quindi possibile scaricare il report in. CSV o. Formato JSON con il pulsante **download** nella parte superiore. Per altre informazioni, vedere l'articolo [procedura: analizzare i rischi](howto-identity-protection-investigate-risk.md#risk-detections).

### <a name="why-cant-i-set-my-own-risk-levels-for-each-risk-detection"></a>Perché non è possibile impostare i propri livelli di rischio per ogni rilevamento dei rischi?

I livelli di rischio in Identity Protection si basano sulla precisione del rilevamento e si avvalgono della tecnologia di Machine Learning. Per personalizzare ciò che viene presentato agli utenti, l'amministratore può includere o escludere determinati utenti o gruppi dai criteri di rischio utente e di rischio di accesso.

### <a name="why-does-the-location-of-a-sign-in-not-match-where-the-user-truly-signed-in-from"></a>Cause per cui la posizione di accesso non corrisponde a quella in cui l'utente ha effettivamente eseguito l'accesso.

Il mapping di georilevazione IP costituisce una sfida a livello di settore. Se si ritiene che la località elencata nel report degli accessi non corrisponda alla posizione effettiva, contattare il supporto tecnico Microsoft. 

### <a name="how-can-i-close-specific-risk-detections-like-i-did-in-the-old-ui"></a>Come è possibile chiudere specifici rilevamenti di rischio come nell'interfaccia utente precedente?

È possibile inviare commenti e suggerimenti sui rilevamenti dei rischi confermando l'accesso collegato come compromesso o sicuro. Il feedback fornito durante l'accesso si riduce a tutti i rilevamenti effettuati su tale accesso. Se si desidera chiudere i rilevamenti non collegati a un accesso, è possibile fornire tali commenti a livello di utente. Per altre informazioni, vedere l'articolo [How to: Give Risk feedback in Azure ad Identity Protection](howto-identity-protection-risk-feedback.md).

### <a name="how-far-can-i-go-back-in-time-to-understand-whats-going-on-with-my-user"></a>Fino a che punto è possibile tornare indietro nel tempo per capire cosa accade con l'utente?

- La visualizzazione [utenti a rischio](howto-identity-protection-investigate-risk.md#risky-users) indica il rischio di un utente in base a tutti gli accessi passati. 
- La visualizzazione [accessi a rischio](howto-identity-protection-investigate-risk.md#risky-sign-ins) Mostra segni a rischio negli ultimi 30 giorni. 
- La visualizzazione [rilevamento](howto-identity-protection-investigate-risk.md#risk-detections) dei rischi Mostra i rilevamenti dei rischi effettuati negli ultimi 90 giorni.

### <a name="how-can-i-learn-more-about-a-specific-detection"></a>Come è possibile ottenere ulteriori informazioni su un rilevamento specifico?

Tutti i rilevamenti dei rischi sono documentati nell'articolo relativo al [rischio](concept-identity-protection-risks.md#risk-types-and-detection). È possibile passare il puntatore del mouse sul simbolo (i) accanto al rilevamento sul portale di Azure per ottenere altre informazioni su un rilevamento.

### <a name="how-do-the-feedback-mechanisms-in-identity-protection-work"></a>Come funzionano i meccanismi dei feedback in Identity Protection?

**Conferma compromesso** (in fase di accesso) - indica ad Azure Active Directory Identity Protection che l'accesso non è stato eseguito dal proprietario dell'identità e indica un compromesso.

- Dopo aver ricevuto il feedback, lo stato di rischio di accesso e di rischio utente verrà modificato in **Confermato compromesso** e il livello di rischio in **Elevato**.

- Inoltre, offriamo le informazioni ai sistemi di Machine Learning per futuri miglioramenti nella valutazione dei rischi.

    > [!NOTE]
    > Se l'utente è già stato salvaguardato, non fare clic su **Conferma compromesso** poiché modificherà lo stato di rischio di accesso e di rischio utente in **Confermato compromesso**, e il livello di rischio in **Elevato**.

**Conferma sicuro** (in fase di accesso) - indica ad Azure Active Directory Identity Protection che l'accesso è stato eseguito dal proprietario dell'identità e non indica un compromesso.

- Dopo aver ricevuto il feedback, lo stato di rischio di accesso (non di rischio utente) verrà modificato in **Confermato sicuro** e il livello di rischio in **-** .

- Inoltre, offriamo le informazioni ai sistemi di Machine Learning per futuri miglioramenti nella valutazione dei rischi.

    > [!NOTE]
    > Se si ritiene che l'utente non sia compromesso, usare **Ignora rischio utente** sul livello utente anziché **Confermato sicuro** a livello di accesso. Un **rischio** per l'utente di eliminazione a livello di utente chiude il rischio dell'utente e tutti gli accessi a rischio e i rilevamenti dei rischi precedenti.

### <a name="why-am-i-seeing-a-user-with-a-low-or-above-risk-score-even-if-no-risky-sign-ins-or-risk-detections-are-shown-in-identity-protection"></a>Perché viene visualizzato un utente con un punteggio di rischio basso o superiore, anche se non vengono visualizzati accessi rischiosi o rilevamenti dei rischi in Identity Protection?

Dato che i rischi per l'utente sono cumulativi e non scadono, un utente può avere un rischio utente minore o superiore anche se non sono presenti accessi rischiosi o rilevamenti rischiosi recenti in Identity Protection. Questa situazione può verificarsi se l'unica attività dannosa per un utente ha avuto luogo oltre l'intervallo di tempo per cui vengono archiviati i dettagli degli accessi a rischio e dei rilevamenti dei rischi. Il rischio utente non ha scadenza poiché gli attori dannosi sono noti rimanere nell'ambiente dei clienti per oltre 140 giorni con un'identità compromessa prima di lanciare l'attacco. I clienti possono esaminare la sequenza temporale del rischio utente per comprendere il motivo per cui un utente è a rischio, recandosi a: `Azure Portal > Azure Active Directory > Risky users’ report > Click on an at-risk user > Details’ drawer > Risk history tab`

### <a name="why-does-a-sign-in-have-a-sign-in-risk-aggregate-score-of-high-when-the-detections-associated-with-it-are-of-low-or-medium-risk"></a>Cause per cui un accesso presenta un punteggio Elevato di "rischio di accesso (aggregato)" quando l'attività di rilevamento associata presenta un rischio basso o medio.

Il punteggio elevato di rischio aggregato potrebbe essere basato su altre funzionalità di accesso o sul fatto che sono stati generati più rilevamenti per tale accesso. E viceversa, un accesso potrebbe avere un rischio di accesso (aggregato) di livello Medio anche se i rilevamenti associati a tale accesso sono di rischio Elevato. 
