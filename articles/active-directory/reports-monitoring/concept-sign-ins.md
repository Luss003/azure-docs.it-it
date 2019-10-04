---
title: Report delle attività di accesso nel portale di Azure Active Directory | Microsoft Docs
description: Introduzione ai report delle attività di accesso nel portale di Azure Active Directory
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 07/17/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6121ca6c1636c8839110712310a1b94fe7fada49
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/29/2019
ms.locfileid: "68619228"
---
# <a name="sign-in-activity-reports-in-the-azure-active-directory-portal"></a>Report delle attività di accesso nel portale di Azure Active Directory

L'architettura di report in Azure Active Directory (Azure AD) include i componenti seguenti:

- **Attività** 
    - **Accessi**: informazioni sull'uso delle applicazioni gestite e sulle attività di accesso degli utenti.
    - **Log di controllo** - [log di controllo](concept-audit-logs.md) forniscono informazioni sulle attività di sistema relative a gestione di utenti e gruppi, applicazioni gestite e attività di directory.
- **Sicurezza** 
    - **Accessi a rischio**: un [accesso a rischio](concept-risky-sign-ins.md) indica un tentativo di accesso che potrebbe essere stato eseguito da qualcuno che non è il legittimo proprietario di un account utente.
    - **Utenti contrassegnati per il rischio**: un [utente a rischio](concept-user-at-risk.md) indica un account utente che potrebbe essere stato compromesso.

Questo articolo fornisce una panoramica del report degli accessi.

## <a name="prerequisites"></a>Prerequisiti

### <a name="who-can-access-the-data"></a>Chi può accedere ai dati?
* Utenti con il ruolo di Amministratore della sicurezza oppure con un ruolo con autorizzazioni di lettura per la sicurezza e per i report
* Amministratori globali
* Qualsiasi utente (non amministratore) può inoltre visualizzare i propri accessi 

### <a name="what-azure-ad-license-do-you-need-to-access-sign-in-activity"></a>Quale licenza di Azure AD è necessaria per visualizzare le attività di accesso?
* Per visualizzare il report completo delle attività di accesso, è necessario che al tenant sia associata una licenza di Azure AD Premium. vedere [Procedura: Effettuare l'iscrizione alle edizioni Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) per aggiornare l'edizione di Azure Active Directory in uso. Si noti che se i dati sulle attività non fosseso disponibili prima dell'aggiornamento, saranno necessari un paio di giorni per visualizzare i dati nei report dopo aver eseguito l'aggiornamento a una licenza Premium.

## <a name="sign-ins-report"></a>Report sugli accessi

Il report relativo agli accessi utente fornisce le risposte alle domande seguenti:

* Qual è il modello di accesso di un utente?
* Quanti utenti hanno effettuato l'accesso nell'arco di una settimana?
* Qual è lo stato di questi accessi?

Per accedere al report degli accessi, selezionare **Accessi** nella sezione **Attività** del pannello **Azure Active Directory** nel [portale di Azure](https://portal.azure.com). Si noti che la visualizzazione di alcuni record di accesso nel portale potrebbe richiedere fino a due ore.

![Attività di accesso](./media/concept-sign-ins/61.png "Attività di accesso")

> [!IMPORTANT]
> Il report degli accessi mostra solo gli accessi **interattivi**, ovvero gli accessi eseguiti manualmente da un utente usando nome utente e password. Gli accessi non interattivi, ad esempio l'autenticazione da servizio a servizio, non vengono visualizzati nel report degli accessi. 

Un log di accesso ha una visualizzazione elenco predefinita che include:

- Data di accesso
- Utente correlato
- Applicazione a cui l'utente ha effettuato l'accesso
- Stato dell'accesso
- Stato di rilevamento rischi
- Stato del requisito di autenticazione a più fattori (MFA)

![Attività di accesso](./media/concept-sign-ins/01.png "Attività di accesso")

Per personalizzare la visualizzazione elenco, fare clic su **Colonne** nella barra degli strumenti.

![Attività di accesso](./media/concept-sign-ins/19.png "Attività di accesso")

In questo modo è possibile visualizzare campi aggiuntivi o rimuovere campi già visualizzati.

![Attività di accesso](./media/concept-sign-ins/02.png "Attività di accesso")

Selezionare un elemento nella visualizzazione elenco per ottenere maggiori informazioni dettagliate.

![Attività di accesso](./media/concept-sign-ins/03.png "Attività di accesso")

> [!NOTE]
> I clienti possono ora risolvere i problemi relativi ai criteri di accesso condizionale tramite tutti i report di accesso. Facendo clic sulla scheda **accesso condizionale** per un record di accesso, i clienti possono esaminare lo stato di accesso condizionale e approfondire i dettagli dei criteri applicati all'accesso e il risultato per ogni criterio.
> Per altre informazioni, vedere [Domande frequenti sulle informazioni di CA in tutti gli accessi](reports-faq.md#conditional-access).



## <a name="filter-sign-in-activities"></a>Filtrare le attività di accesso

Per limitare i dati segnalati a un livello che funziona automaticamente, è possibile filtrare i dati di accesso usando il campo data come filtro predefinito. Azure AD offre inoltre un'ampia gamma di filtri aggiuntivi che è possibile impostare.

![Attività di accesso](./media/concept-sign-ins/04.png "Attività di accesso")

Il filtro **Utente** permette di specificare il nome o il nome dell'entità utente (UPN) per l'utente richiesto.

Il filtro **Applicazione** permette di specificare il nome dell'applicazione richiesta.

Il filtro **Stato accesso** permette di selezionare:

- Tutti
- Riuscito
- Errore

Il filtro **Accesso condizionale** consente di selezionare lo stato dei criteri di accesso condizionale per l'accesso:

- Tutti
- Non applicato
- Riuscito
- Errore

Il filtro **Date** (Data) permette di definire un intervallo di tempo per i dati restituiti.  
I valori possibili sono:

- 1 mese
- 7 giorni
- 24 ore
- Intervallo di tempo personalizzato

Quando si seleziona un intervallo di tempo personalizzato, è possibile configurare un'ora di inizio e un'ora di fine.

Se si aggiungono altri campi alla visualizzazione degli accessi, questi campi verranno aggiunti automaticamente all'elenco dei filtri. Ad esempio, se si aggiunge il campo **App client** all'elenco, si otterrà un'altra opzione di filtro che consente di impostare i filtri seguenti:  
![Attività di accesso](./media/concept-sign-ins/12.png "Attività di accesso")

- **Browser**  
    Questo filtro Mostra tutti gli eventi in cui sono stati eseguiti tentativi di accesso usando i flussi del browser.
- **Exchange ActiveSync (supportato)**  
    Questo filtro Mostra tutti i tentativi di accesso in cui è stato eseguito il tentativo del protocollo di Exchange ActiveSync (EAS) dalle piattaforme supportate come iOS, Android e Windows Phone.
- **Exchange ActiveSync (non supportato)**  
    Questo filtro Mostra tutti i tentativi di accesso in cui il protocollo EAS è stato provato da piattaforme non supportate come le distribuzioni Linux.
- **App per dispositivi mobili e client desktop** Questo filtro Mostra tutti i tentativi di accesso che non usavano i flussi del browser. Può trattarsi di app per dispositivi mobili da qualsiasi piattaforma che usa qualsiasi protocollo o da app client desktop come Office in Windows o MacOS.
  
- **Altri client**
    - **IMAP**  
        Un client di posta legacy che usa IMAP per recuperare la posta elettronica.
    - **MAPI**  
        Office 2013, in cui ADAL è abilitato e utilizza MAPI.
    - **Client Office meno recenti**  
        Office 2013 nella configurazione predefinita in cui ADAL non è abilitato ed è in uso MAPI oppure Office 2016 dove ADAL è stato disabilitato.
    - **POP**  
        Un client di posta legacy che usa POP3 per recuperare la posta elettronica.
    - **SMTP**  
        Un client di posta legacy che usa SMTP per inviare messaggi di posta elettronica.

## <a name="download-sign-in-activities"></a>Scaricare le attività di accesso

È possibile [scaricare i dati relativi agli accessi](quickstart-download-sign-in-report.md) per usarli esternamente al portale di Azure. Se si fa clic su **download** è possibile creare un file CSV o JSON dei record 250.000 più recenti.  

![Download](./media/concept-sign-ins/71.png "Download")

> [!IMPORTANT]
> Il numero di record che è possibile scaricare è limitato dai [criteri di conservazione dei report di Azure Active Directory](reference-reports-data-retention.md).  


## <a name="sign-ins-data-shortcuts"></a>Tasti di scelta rapida per i dati degli accessi

Oltre ad Azure AD, il portale di Azure fornisce altri punti di ingresso ai dati relativi agli accessi:

- Panoramica Identity Security e Protection
- Utenti
- Gruppi
- Applicazioni aziendali

### <a name="users-sign-ins-data-in-identity-security-protection"></a>Dati degli accessi degli utenti in Identity Security e Protection

Il grafo di accesso utente nella pagina Panoramica della **protezione della sicurezza delle identità** Mostra le aggregazioni settimanali degli accessi per tutti gli utenti in un determinato periodo di tempo. Il periodo di tempo predefinito è di 30 giorni.

![Attività di accesso](./media/concept-sign-ins/06.png "Attività di accesso")

Quando si fa clic su un giorno nel grafico degli accessi, si ottiene una panoramica delle attività di accesso per tale giorno.

Ogni riga dell'elenco delle attività di accesso mostra:

* Chi ha effettuato l'accesso?
* Qual era l'applicazione di destinazione dell'accesso?
* Qual è lo stato dell'accesso?
* Qual è lo stato MFA dell'accesso?

Facendo clic su un elemento, si ottengono altri dettagli sull'operazione di accesso:

- ID utente
- Utente
- Nome utente
- ID applicazione
- Applicazione
- Client
- Location
- Indirizzo IP
- Date
- Autenticazione MFA obbligatoria
- Stato accesso

> [!NOTE]
> gli indirizzi IP vengono rilasciati in modo che non esista una connessione certa tra un IP e la posizione in cui si trova fisicamente il computer con tale indirizzo. Il mapping degli indirizzi IP è complicato dal fatto che provider di telefonia mobile e VPN possono rilasciare indirizzi IP da pool centrali spesso molto distanti dal luogo in cui viene effettivamente usato il dispositivo client. Attualmente nei report di Azure AD la conversione di un indirizzo IP in una posizione fisica è un'approssimazione basata su tracce, dati del Registro di sistema, ricerche inverse e altre informazioni.

Nella pagina **Utenti** è possibile accedere a una panoramica completa di tutti i accessi degli utenti facendo clic su **Accessi** nella sezione **Attività**.

![Attività di accesso](./media/concept-sign-ins/08.png "Attività di accesso")

## <a name="usage-of-managed-applications"></a>Utilizzo di applicazioni gestite

Con una visualizzazione dei dati di accesso basata sulle applicazioni, è possibile rispondere a domande come:

* Chi sta usando le applicazioni?
* Quali sono le prime 3 applicazioni nell'organizzazione?
* Di recente è stata implementata un'applicazione. Come sta andando?

Il punto di ingresso a questi dati sono le prime 3 applicazioni nell'organizzazione nel report sugli ultimi 30 giorni della sezione **Panoramica** in **Applicazioni aziendali**.

![Attività di accesso](./media/concept-sign-ins/10.png "Attività di accesso")

Le aggregazioni settimanali del grafico sull'utilizzo delle app degli accessi per le prime 3 applicazioni in un determinato periodo di tempo. Il periodo di tempo predefinito è di 30 giorni.

![Attività di accesso](./media/concept-sign-ins/47.png "Attività di accesso")

Se si preferisce, è possibile mettere in evidenza un'applicazione specifica.

![Report](./media/concept-sign-ins/single_spp_usage_graph.png "Report")

Quando si fa clic su un giorno nel grafico dell'utilizzo dell'app, si ottiene un elenco dettagliato delle attività di accesso.

L'opzione **Accessi** offre una panoramica completa di tutti gli eventi di accesso nell'applicazione.

![Attività di accesso](./media/concept-sign-ins/11.png "Attività di accesso")

## <a name="office-365-activity-logs"></a>Log attività di Office 365

È possibile visualizzare i log attività di Office 365 dall'interfaccia di [amministrazione di Microsoft 365](https://docs.microsoft.com/office365/admin/admin-overview/about-the-admin-center). Anche se l'attività di Office 365 e i log attività Azure AD condividono numerose risorse della directory, solo l'interfaccia di amministrazione di Microsoft 365 fornisce una visualizzazione completa dei log attività di Office 365. 

È anche possibile accedere ai log attività di Office 365 a livello di codice usando le [API di gestione di office 365](https://docs.microsoft.com/office/office-365-management-api/office-365-management-apis-overview).

## <a name="next-steps"></a>Passaggi successivi

* [Codici di errore del report delle attività di accesso](reference-sign-ins-error-codes.md)
* [Criteri di conservazione dei report di Azure AD](reference-reports-data-retention.md)
* [Latenze dei report di Azure AD](reference-reports-latencies.md)

