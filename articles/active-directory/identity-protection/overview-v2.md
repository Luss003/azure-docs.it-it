---
title: Cos'è Azure Active Directory Identity Protection (aggiornato)? | Microsoft Docs
description: Cos'è Azure Active Directory Identity Protection (aggiornato)?
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: overview
ms.date: 08/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3129027da0f28d9c89f7afe75d9531df9bae499e
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2019
ms.locfileid: "70125644"
---
# <a name="what-is-azure-active-directory-identity-protection-refreshed"></a>Cos'è Azure Active Directory Identity Protection (aggiornato)?

L'esperienza di Identity Protection è stata aggiornata per migliorare la protezione delle identità dell'organizzazione. L’esperienza aggiornata offre:

- Esperienza di amministrazione riprogettata, incentrata sui rischi più rilevanti in termini di rischio utente e rischio di accesso
- Potenti funzionalità di indagine con supporto per opzioni di filtro, ordinamento e download intelligenti
- Miglioramento del calcolo del rischio utente per assegnare la priorità agli interventi verso gli utenti con probabilità di compromissione più elevata
- Supporto di una nuova API per abilitare l'accesso a livello di codice ai dati sui rischi
- Processo di feedback di amministrazione semplificato, che consente di proteggere immediatamente gli utenti
- Nuova funzionalità di apprendimento automatico con supervisione per migliorare l'accuratezza delle valutazioni dei rischi

La sicurezza è una priorità assoluta per le organizzazioni di oggi. La maggior parte delle violazioni della sicurezza si verifica quando utenti malintenzionati ottengono l'accesso a un ambiente impadronendosi dell'identità di un utente. Nel corso degli anni gli utenti malintenzionati hanno messo a punto tecniche sempre più efficaci per sfruttare le violazioni di terze parti e sferrare sofisticati attacchi di phishing. L'accesso a un account utente, anche quelli con privilegi limitati, permette agli utenti malintenzionati di accedere immediatamente a risorse aziendali importanti in modo piuttosto semplice tramite il movimento laterale. 

Per rispondere a queste minacce, Azure AD Identity Protection consente di: 

- Impedire in modo proattivo l'uso improprio delle identità compromesse 
- Contenere automaticamente i rischi quando viene rilevata un'attività sospetta 
- Analizzare gli utenti e gli accessi a rischio per risolvere potenziali vulnerabilità  
- Essere avvisati qualora il rischio di un utente raggiunge una soglia specificata 

Azure AD Identity Protection è una funzionalità di Azure Active Directory Premium P2 che consente di configurare i criteri per rispondere automaticamente quando viene compromessa l'identità di un utente o quando un utente diverso dal proprietario dell'account sta tentando di accedere usando l’identità di quest’ultimo. Questi criteri, in aggiunta ad altri controlli di accesso condizionale forniti da Azure AD, possono applicare il blocco automatico dell'accesso o avviare azioni di mitigazione, come la reimpostazione della password o l'imposizione dell'autenticazione a più fattori. Inoltre, Identity Protection offre funzionalità di monitoraggio e creazione di report per ottenere informazioni più approfondite sui rischi e sulle potenziali compromissioni all'interno dell'organizzazione. 

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RWsS6Q]

## <a name="risk-detections"></a>Rilevamenti dei rischi

Azure AD Identity Protection esegue i rilevamenti dei rischi seguenti: 

| Tipo di rilevamento dei rischi | DESCRIZIONE | Tipo di rilevamento |
| --- | --- | --- |
| Trasferimento atipico | Accesso da una posizione insolita in base agli accessi recenti dell'utente. | Offline |
| Indirizzo IP anonimo | Accesso da indirizzo IP anonimo (ad esempio Tor Browser, VPN per navigazione in anonimato). | Tempo reale |
| Proprietà di accesso insolite | Accesso con proprietà non osservate di recente per l'utente specificato. | Tempo reale |
| Indirizzo IP collegato a malware | Accesso da indirizzo IP collegato a malware | Offline |
| Credenziali perse | Questo rilevamento dei rischi indica che le credenziali valide dell'utente sono andate perse | Offline |

## <a name="types-of-risk"></a>Tipi di rischio 

Identity Protection si basa su due tipi di rischio:

- Rischio di accesso
- Rischio utente

### <a name="sign-in-risk"></a>Rischio di accesso

Un rischio di accesso rappresenta la probabilità che una richiesta di autenticazione specificata non sia stata autorizzata dal proprietario dell'identità.

Esistono due valutazioni del rischio di accesso: 

- **Rischio di accesso (in tempo reale)** - Il rischio di accesso (in tempo reale) si basa su tutti i rilevamenti in tempo reale che si attivano durante l'elaborazione dell’accesso.  
- **Rischio di accesso (aggregazione)** - Il rischio di accesso (aggregazione) è il rischio complessivo di un accesso. Viene calcolato tramite un modello di machine learning che prende in considerazione:
   - Rilevamenti in tempo reale (descritti in precedenza)
   - Rilevamenti offline (che vengono generati dopo l’esecuzione dell'accesso) 
   - Tutte le altre funzionalità dell’accesso

### <a name="user-risk"></a>Rischio utente

Un rischio utente rappresenta la probabilità che un'identità specificata sia compromesso. 

Il rischio utente viene calcolato considerando tutti i rischi associati all'utente:

- Tutti gli accessi a rischio
- Tutti i rilevamenti dei rischi non collegati a un accesso
- Il rischio utente corrente
- Eventuali azioni di correzione o rimozione dei rischi eseguite per l'utente fino alla data corrente

## <a name="how-identity-protection-detects-risk"></a>Rilevamento del rischio in Identity Protection  

Azure AD usa l'apprendimento automatico per rilevare anomalie ed eventuali attività sospette, sfruttando sia i segnali rilevati in tempo reale durante gli accessi, sia quelli non in tempo reale correlati agli utenti e alle relative attività di accesso. Usando questi dati, Identity Protection calcola un rischio di accesso in tempo reale ogni volta che un utente esegue l'autenticazione, oltre a determinare un livello di rischio utente complessivo per ogni utente. Identity Protection consente di eseguire azioni automatiche sui rilevamenti di rischio tramite la configurazione dei criteri per rischi utente e rischi di accesso.  

Per capire come Identity Protection rileva i rischi, è necessario spiegare due concetti importanti: rischio utente e rischio di accesso. Un rischio di accesso rappresenta la probabilità che una richiesta di autenticazione specificata non sia stata autorizzata dal proprietario dell'identità. Esistono due tipi di rischi di accesso: in tempo reale e totale. Il rischio di accesso in tempo reale viene rilevato durante il tentativo di accesso specificato (ad esempio, accessi da indirizzi IP anonimi). Il rischio di accesso totale è dato dall'aggregazione dei rischi di accesso in tempo reale rilevati, nonché da eventuali rilevamenti di rischi non in tempo reale successivi associati agli accessi dell'utente (ad esempio impossibilità di comunicare). Il rischio utente rappresenta la probabilità complessiva che una determinata identità sia stata compromessa da un utente malintenzionato. Il rischio utente contiene tutte le attività di rischio per un determinato utente, tra cui:

- Rischio di accesso in tempo reale
- Rischi di accesso successivi
- Rilevamenti di utenti a rischio.   

 ![Flusso](./media/overview-v2/01.png)

Il flusso di base per il rilevamento dei rischi e la risposta di Identity Protection in relazione a qualsiasi accesso specificato è riepilogato nella figura precedente.  

## <a name="common-scenarios"></a>Scenari comuni 

Esaminiamo l'esempio di un dipendente di Contoso. 

1. Un dipendente prova ad accedere a Exchange Online da Tor Browser. Al momento dell'accesso, Azure AD esegue i rilevamenti dei rischi in tempo reale. 
2. Azure AD rileva che il dipendente esegue l'accesso da un indirizzo IP anonimo, attivando un livello di rischio di accesso medio. 
3. Il dipendente visualizza una richiesta di verifica tramite autenticazione a più fattori, perché l'amministratore IT di Contoso ha configurato i criteri di accesso condizionale per il rischio di accesso di Identity Protection. I criteri richiedono l'autenticazione a più fattori per un rischio di accesso di livello medio o superiore. 
4. Il dipendente completa l'autenticazione a più fattori e accede a Exchange Online, con un livello di rischio utente inalterato. 

Cosa è successo dietro le quinte? Il tentativo di accesso da Tor Browser ha attivato un rischio di accesso in tempo reale in Azure AD per l'indirizzo IP anonimo. Durante l'elaborazione della richiesta, Azure AD ha applicato il criterio di rischio di accesso configurato in Identity Protection perché il livello di rischio di accesso del dipendente corrispondeva alla soglia (Medio). Dal momento che il dipendente aveva effettuato in precedenza la registrazione per l'autenticazione a più fattori, è riuscito a rispondere e a completare l'autenticazione a più fattori. Il completamento dell'autenticazione a più fattori ha indicato ad Azure AD che probabilmente si tratta del legittimo proprietario dell'identità, di conseguenza il livello di rischio utente non viene aumentato. 

Ma cosa sarebbe successo se il dipendente non fosse stato l'utente che effettua il tentativo di accesso? 

1. Un utente malintenzionato con le credenziali del dipendente prova ad accedere all'account Exchange Online da Tor Browser perché intende nascondere il proprio indirizzo IP. 
2. Azure AD rileva che il tentativo di accesso avviene da un indirizzo IP anonimo, attivando un rischio di accesso in tempo reale. 
3. L’utente malintenzionato visualizza una richiesta di verifica tramite prompt di autenticazione a più fattori (MFA), perché l’amministratore IT di Contoso ha configurato i criteri di accesso condizionale per il rischio di accesso di Identity Protection in modo richiedere l’autenticazione a più fattori quando il rischio di accesso è medio o superiore. 
4. L'utente malintenzionato non riesce a completare l'autenticazione a più fattori e non può accedere all'account Exchange Online del dipendente. 
5. Una richiesta di Multi-Factor Authentication non completata ha attivato la registrazione di un rilevamento di rischio, aumentando il rischio utente per gli accessi futuri. 

Ora che un utente malintenzionato ha provato ad accedere all'account del dipendente, vediamo cosa accade al tentativo di accesso successivo. 

1. Il dipendente prova ad accedere a Exchange Online da Outlook. Al momento dell'accesso, Azure AD esegue i rilevamenti dei rischi in tempo reale, nonché di qualsiasi rischio utente precedente. 
2. Azure AD non rileva alcun rischio di accesso in tempo reale, ma un rischio utente più elevato dovuto all’attività rischiosa degli scenari precedenti.  
3. Il dipendente visualizza una richiesta di verifica tramite prompt di reimpostazione della password, perché l'amministratore IT di Contoso ha configurato i criteri di rischio utente di Identity Protection affinché sia richiesta la modifica della password all'accesso di un utente ad alto rischio. 
4. Dal momento che il dipendente ha effettuato la registrazione per la reimpostazione della password self-service e l'autenticazione a più fattori, può reimpostare correttamente la propria password. 
5. Reimpostando la password, le credenziali del dipendente non sono più compromesse e la sua identità risulta nuovamente sicura. 
6. I rilevamenti dei rischi precedenti del dipendente vengono risolti e il suo livello di rischio utente viene reimpostato automaticamente in risposta alla mitigazione del rischio di compromissione delle credenziali. 

## <a name="how-do-i-configure-identity-protection"></a>Come si configura Identity Protection? 

Per iniziare a usare Identity Protection, per prima cosa è necessario configurare i criteri di rischio utente e di rischio di accesso. Una volta configurati questi criteri e applicati a un gruppo di test, è possibile simulare rilevamenti dei rischi per capire come risponderà Identity Protection nell'ambiente in uso. Le guide di avvio rapido seguenti offrono una procedura dettagliata su come configurare i criteri indicati in precedenza ed effettuarne il test nel proprio ambiente. 

Identity Protection supporta tre ruoli in Azure AD per bilanciare le attività di gestione nella distribuzione: 

| Ruolo | Operazione consentita | Operazione non consentita |
| --- | --- | --- |
| Amministratore globale | Accesso completo a Identity Protection, implementazione di Identity Protection | |
| Amministratore della sicurezza | Accesso completo a Identity Protection | Implementazione di Identity Protection, reimpostazione delle password per un utente |
| Ruolo con autorizzazioni di lettura per la sicurezza | Accesso in sola lettura a Identity Protection | Implementazione di Identity Protection, correzione degli utenti, configurazione dei criteri, reimpostazione delle password| 

Per altre informazioni, vedere [Assegnazione dei ruoli di amministratore in Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md)
 
## <a name="licensing"></a>Licenze

>[!NOTE]
> Durante l'anteprima pubblica di Identity Protection (aggiornato), solo i clienti di Azure AD Premium P2 avranno accesso ai report degli utenti a rischio e degli accessi a rischio.

| Funzionalità | Dettagli | Azure AD Premium P2 | Azure AD Premium P1 | Azure AD Basic/Gratuito |
| --- | --- | --- | --- | --- |
| Criteri di rischio | Criteri di rischio utente (tramite Identity Protection) | Sì | No | No |
| Criteri di rischio | Criteri di rischio di accesso (tramite Identity Protection o accesso condizionale) | Sì | No | No |
| Report sulla sicurezza | Panoramica | Sì | No | No |
| Report sulla sicurezza | Utenti a rischio | Accesso completo | Informazioni limitate | Informazioni limitate |
| Report sulla sicurezza | Accessi a rischio | Accesso completo | Informazioni limitate | Informazioni limitate |
| Report sulla sicurezza | Rilevamenti dei rischi | Accesso completo | Informazioni limitate | No |
| Notifiche | Avvisi relativi a utenti a rischio rilevati | Sì | No | No |
| Notifiche | Digest settimanale | Sì | No | No |
| | Criteri di registrazione MFA | Sì | No | No |

## <a name="next-steps"></a>Passaggi successivi 

Per iniziare a usare Identity Protection, vedere [Configurare i criteri di rischio di accesso](quickstart-sign-in-risk-policy.md). 
