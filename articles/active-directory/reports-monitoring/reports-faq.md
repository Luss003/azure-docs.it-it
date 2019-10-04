---
title: Domande frequenti sui report di Azure Active Directory | Microsoft Docs
description: Domande frequenti sui report di Azure Active Directory.
services: active-directory
documentationcenter: ''
author: cawrites
manager: daveba
ms.assetid: 534da0b1-7858-4167-9986-7a62fbd10439
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: chadam
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: e8c3138b82c7dc4a7217e8cb67448a5d824398ba
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2019
ms.locfileid: "70127015"
---
# <a name="frequently-asked-questions-around-azure-active-directory-reports"></a>Domande frequenti sui report di Azure Active Directory

Questo articolo include risposte alle domande frequenti sulla creazione di report in Azure Active Directory (Azure AD). Per altre informazioni, vedere [Creazione di report in Azure Active Directory](overview-reports.md). 

## <a name="getting-started"></a>Introduzione 

**D: Attualmente si utilizzano le `https://graph.windows.net/<tenant-name>/reports/` API dell'endpoint per Azure ad estrarre i report di controllo e di utilizzo delle applicazioni integrati nei sistemi di creazione di report a livello di codice. a quale API occorre passare?**

**R:** consultare la [documentazione di riferimento sulle API](https://developer.microsoft.com/graph/) per informazioni su come [usare le API per accedere ai report attività](concept-reporting-api.md). Questo endpoint dispone di due report (**Controllo** e **Accessi**) che forniscono tutti i dati che si ottengono nell'endpoint delle API precedente. Anche questo nuovo endpoint include un report sugli accessi con la licenza Azure AD Premium, che è possibile usare per ottenere informazioni sull'utilizzo dell'app, l'utilizzo dei dispositivi e gli accessi degli utenti.

---

**D: Attualmente si usano le `https://graph.windows.net/<tenant-name>/reports/` API dell'endpoint per estrarre i report di sicurezza Azure ad (tipi specifici di rilevamento, ad esempio le credenziali perse o gli accessi da indirizzi IP anonimi) nei sistemi di report a livello di codice. a quale API occorre passare?**

**R:** È possibile usare l' [API](../identity-protection/graph-get-started.md) di rilevamento dei rischi di Identity Protection per accedere ai rilevamenti di sicurezza tramite Microsoft Graph. Questo nuovo formato offre maggiore flessibilità nel modo in cui è possibile eseguire query sui dati, con filtri avanzati, selezione dei campi e altro ancora, e standardizza i rilevamenti dei rischi in un unico tipo per semplificare l'integrazione in SIEM e altri strumenti di raccolta dati. Poiché i dati sono un formato diverso, non è possibile sostituire le query precedenti con una nuova query. Tuttavia, [la nuova API usa Microsoft Graph](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityriskevent), ovvero lo standard Microsoft per API come Office 365 o Azure AD. Saranno quindi necessari interventi per estendere gli investimenti esistenti per Microsoft Graph o per avviare la transizione a questa nuova piattaforma standard.

---

**D: come ottenere una licenza Premium?**

**R:** vedere [Procedura: Effettuare l'iscrizione alle edizioni Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) per aggiornare l'edizione di Azure Active Directory in uso.

---

**D: In quanto tempo è possibile visualizzare le attività dopo aver acquistato una licenza Premium?**

**R:** Se si dispone già di dati sulle attività come licenza gratuita, è possibile visualizzarli immediatamente. Se non si dispone di dati, i dati verranno visualizzati nei report dopo uno o due giorni.

---

**D: È possibile visualizzare i dati del mese precedente dopo avere acquistato una licenza Azure AD Premium?**

**R:** Se di recente si è passati alla versione Premium (anche di valutazione), inizialmente è possibile visualizzare i dati degli ultimi 7 giorni. Man mano che i dati si accumulano, è possibile visualizzare quelli degli ultimi 30 giorni.

---

**D: è necessario essere amministratore globale per visualizzare gli accessi al portale di Azure o per ottenere i dati tramite l'API?**

**R:** no, è possibile accedere ai dati dei report tramite il portale o l'API anche se si ha il **Ruolo con autorizzazioni di lettura per la sicurezza** o **Amministratore della sicurezza** per il tenant. Naturalmente anche gli **amministratori globali** hanno accesso a questi dati.

---


## <a name="activity-logs"></a>Log attività


**D: qual è la conservazione dei dati per i log attività (controllo e accessi) nel portale di Azure?** 

**R:** la tabella seguente indica il periodo di conservazione dei dati per i log attività. Per altre informazioni, vedere [Criteri di conservazione dei report di Azure Active Directory](reference-reports-data-retention.md).

| Report                 | Azure AD Gratuito | Azure AD P1 Premium | Azure AD P2 Premium |
| :--                    | :--           | :--                 | :--                 |
| Log di controllo             | 7 giorni        | 30 giorni             | 30 giorni             |
| Accessi               | N/D           | 30 giorni             | 30 giorni             |
| Uso di Azure MFA        | 30 giorni       | 30 giorni             | 30 giorni             |

---

**D: quanto tempo occorre per la visualizzazione dei dati sull'attività dopo il completamento della stessa?**

**R:** i log di controllo hanno una latenza compresa tra 15 minuti e un'ora. Per la visualizzazione dei log delle attività di accesso possono essere necessari da 15 minuti fino a 2 ore per alcuni record.

---

**D: è possibile ottenere informazioni sui log attività di Office 365 tramite il portale di Azure?**

**R:** Anche se l'attività di Office 365 e i log attività Azure AD condividono numerose risorse della directory, se si desidera una visualizzazione completa dei log attività di Office 365, è necessario accedere all'interfaccia di [amministrazione di Microsoft 365](https://admin.microsoft.com) per ottenere informazioni sul log attività di Office 365.

---

**D: quali API è necessario usare per ottenere informazioni sui log attività di Office 365?**

**R:** usare le [API Gestione di Office 365](https://docs.microsoft.com/office/office-365-management-api/office-365-management-apis-overview) per accedere ai log attività di Office 365 tramite un'API.

---

**D: quanti record è possibile scaricare dal portale di Azure?**

**R:** dal portale di Azure è possibile scaricare fino a 5000 record. Per impostazione predefinita, vengono scaricati gli ultimi 5.000 record, ordinati partendo dai record *più recenti*.

---

## <a name="risky-sign-ins"></a>Accessi a rischio

**D: È presente un rilevamento dei rischi in Identity Protection, ma non è possibile visualizzare l'accesso corrispondente nel report degli accessi. È normale?**

**R:** sì, la protezione dell'identità valuta il rischio per tutti i flussi di autenticazione, sia interattivi che non interattivi. Tuttavia, tutti i report degli accessi mostrano solo gli accessi interattivi.

---

**D: come sapere perché un accesso o un utente è stato contrassegnato come rischioso nel portale di Azure?**

**R:** Se si dispone di una sottoscrizione di **Azure ad Premium** , è possibile ottenere altre informazioni sui rilevamenti dei rischi sottostanti selezionando l'utente in **utenti contrassegnati per il rischio** o selezionando un record nel report degli accessi a **rischio** . Se si dispone di una sottoscrizione **gratuita** o **Basic** , è possibile visualizzare i report utenti a rischio e accessi a rischio, ma non è possibile visualizzare le informazioni di rilevamento dei rischi sottostanti.

---

**D: come vengono calcolati gli indirizzi IP nel report degli accessi e degli accessi a rischio?**

**R:** gli indirizzi IP vengono rilasciati in modo che non esista una connessione certa tra un IP e la posizione in cui si trova fisicamente il computer con tale indirizzo. Il mapping degli indirizzi IP è ulteriormente complicato da fattori come la possibilità che provider di telefonia mobile e VPN rilascino indirizzi IP da pool centrali spesso molto distanti dal luogo in cui viene effettivamente usato il dispositivo client. Attualmente nei report di Azure AD la conversione di un indirizzo IP in una posizione fisica è un'approssimazione basata su tracce, dati del Registro di sistema, ricerche inverse e altre informazioni. 

---

**D: Qual è il significato del rilevamento dei rischi "accesso con rischi aggiuntivi"?**

**R:** per offrire informazioni dettagliate su tutti gli accessi rischiosi effettuati nell'ambiente, "È stato rilevato un accesso con rischi aggiuntivi" funziona come segnaposto per gli accessi associati a rilevamenti esclusivi per i sottoscrittori di Azure AD Identity Protection.

---

## <a name="conditional-access"></a>Accesso condizionale

**D: quali sono le novità di questa funzionalità?**

**R:** I clienti possono ora risolvere i problemi relativi ai criteri di accesso condizionale tramite tutti i report degli accessi. I clienti possono esaminare lo stato di accesso condizionale e approfondire i dettagli dei criteri applicati all'accesso e il risultato per ogni criterio.

**D: come iniziare?**

**R:** Attività iniziali

* Andare al report degli accessi nel [portale di Azure](https://portal.azure.com).
* Fare clic sull'accesso di cui si desiderano risolvere i problemi.
* Passare alla scheda **accesso condizionale** . In questa scheda è possibile visualizzare tutti i criteri che hanno interessato l'accesso e il risultato per ogni criterio. 
    
**D: Quali sono tutti i valori possibili per lo stato di accesso condizionale?**

**R:** Lo stato di accesso condizionale può includere i valori seguenti:

* **Non applicato**: significa che non è presente alcun criterio di accesso condizionale con l'utente e l'app inclusi nell'ambito. 
* **Operazione riuscita**: significa che è presente un criterio di accesso condizionale con l'utente e con l'app previsti e che i criteri di accesso condizionale sono stati soddisfatti correttamente. 
* **Operazione non riuscita**: significa che è presente un criterio di accesso condizionale con l'utente e con l'app previsti e che i criteri di accesso condizionale non sono stati soddisfatti correttamente. 
    
**D: Quali sono tutti i valori possibili per il risultato dei criteri di accesso condizionale?**

**R:** I criteri di accesso condizionale possono avere i seguenti risultati:

* **Operazione riuscita**: i criteri sono stati soddisfatti.
* **Operazione non riuscita**: i criteri non sono stati soddisfatti.
* **Non applicato**: è possibile che le condizioni dei criteri non siano soddisfatte.
* **Non abilitato**: la causa è il fatto che i criteri sono in stato disattivato. 
    
**D: il nome dei criteri nel report di tutti gli accessi non corrisponde al nome dei criteri nell'accesso condizionale. Perché?**

**R:** il nome del criterio nel report di tutti gli accessi è basato sul nome del criterio di accesso condizionale al momento dell'accesso. Ciò può essere incoerente con il nome del criterio nell'accesso condizionale se è stato aggiornato il nome del criterio in un secondo momento, vale a dire, dopo l'accesso.

**D: L'accesso è stato bloccato a causa di un criterio di accesso condizionale, ma il report delle attività di accesso Mostra che l'accesso è riuscito. Perché?**

**R:** Attualmente, il report di accesso potrebbe non visualizzare risultati accurati per gli scenari di Exchange ActiveSync quando viene applicato l'accesso condizionale. Possono verificarsi casi in cui il risultato dell'accesso nel report Mostra un accesso riuscito, ma l'accesso non è riuscito a causa di un criterio di accesso condizionale. 
