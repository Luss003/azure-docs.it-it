---
title: Come usare il pacchetto di contenuto Power BI di Azure Active Directory | Microsoft Docs
description: Informazioni su come usare il pacchetto di contenuto Power BI di Azure Active Directory
services: active-directory
author: cawrites
manager: daveba
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: chadam
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4d56802e96028b6b01b6be749405c56df2648161
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/13/2019
ms.locfileid: "68988243"
---
# <a name="how-to-use-the-azure-active-directory-power-bi-content-pack"></a>Come usare il pacchetto di contenuto Power BI di Azure Active Directory

|  |
|--|
|Attualmente, il pacchetto di contenuto Power BI di Azure AD usa le API Graph di Azure AD per recuperare dati dal tenant di Azure AD. Di conseguenza, potrebbe notarsi una certa disparità tra i dati disponibili nel pacchetto di contenuto e i dati recuperati tramite le [API Microsoft Graph per la creazione di report](concept-reporting-api.md). |
|  |

Il pacchetto di contenuto Power BI per Azure Active Directory (Azure AD) contiene report pronti per capire come gli utenti adottano e usano le funzionalità di Azure AD. In questo modo è possibile ottenere informazioni dettagliate su tutte le attività all'interno della directory, usando l'esperienza di visualizzazione avanzata in Power BI. È anche possibile creare dashboard personalizzati e condividerli con altri utenti nell'organizzazione. 

## <a name="prerequisites"></a>Prerequisiti

Per usare il pacchetto di contenuto, è necessario avere una licenza di Azure AD Premium (P1/P2). vedere [Procedura: Effettuare l'iscrizione alle edizioni Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) per aggiornare l'edizione di Azure Active Directory in uso.

## <a name="install-the-content-pack"></a>Installare il pacchetto di contenuto

Leggere la [guida introduttiva](quickstart-install-power-bi-content-pack.md) per installare il pacchetto di contenuto Power BI per Azure AD.

### <a name="reports-included-in-this-version-of-azure-ad-logs-content-pack"></a>Report inclusi in questa versione del pacchetto di contenuto dei log di Azure AD

I report seguenti sono inclusi nel pacchetto di contenuto di Azure AD Power BI. I report contengono dati per gli **ultimi 30 giorni**.

**Report di utilizzo app e tendenze**:  Questo report offre informazioni dettagliate sulle applicazioni usate all'interno dell'organizzazione. È possibile ottenere un elenco delle applicazioni più diffuse o comprendere come viene usata un'applicazione distribuita di recente nell'organizzazione. In questo modo è possibile tenere traccia dell'utilizzo nel tempo e introdurre miglioramenti.

**Accessi in base a posizioni e utenti**: Questo report include dati su tutti gli accessi eseguiti con l'identità di Azure. Con questo report è possibile eseguire il drill down di singoli accessi e rispondere a domande come:

- Da dove ha effettuato l'accesso questo utente?
- Quale utente ha il maggior numero di accessi e da dove accede? 
- L'accesso è stato eseguito correttamente?  
 
È anche possibile filtrare i risultati selezionando una data o una posizione specifica.

**Utenti univoci per ogni app**:  Questo report offre una visualizzazione di tutti gli utenti univoci che usano un'app specifica. Include solo gli utenti che hanno completato "*correttamente*" l'accesso a un'applicazione.

**Accessi dispositivi**: Questo report consente di conoscere i diversi profili dei dispositivi usati all'interno dell'organizzazione e di determinare i criteri dei dispositivi in base all'utilizzo. Fornisce dati relativi al tipo di sistema operativo e browser usati per accedere alle applicazioni, oltre a informazioni dettagliate sugli utenti, inclusi:

- Nome utente
- Indirizzo IP
- Location 
- Stato accesso 

**Grafico a imbuto SSPR**: Questo report consente di comprendere come viene usato lo strumento SSPR all'interno dell'organizzazione. È possibile scoprire il numero di reimpostazioni delle password tentate tramite lo strumento SSPR e quante di esse hanno avuto esito positivo. È anche possibile analizzare gli errori di reimpostazione delle password e comprendere perché si sono verificati determinati errori. 

## <a name="customize-azure-ad-activity-content-pack"></a>Personalizzare il pacchetto di contenuto per le attività di Azure AD

**Modificare la visualizzazione**:  È possibile modificare la visualizzazione di un report facendo clic su **Modifica Report** e selezionando la visualizzazione desiderata.
 
![Pacchetto di contenuto Power BI di Azure Active Directory](./media/howto-power-bi-content-pack/09.png) 
 
![Pacchetto di contenuto Power BI di Azure Active Directory](./media/howto-power-bi-content-pack/10.png) 

**Aggiungere altri campi**:  È possibile aggiungere o rimuovere un campo al report selezionando l'oggetto visivo per il quale si intende aggiungere o rimuovere il campo. Ad esempio, è possibile aggiungere il campo "Sign-in Status" (Stato di accesso) alla visualizzazione tabella, come illustrato di seguito. 
 
![Pacchetto di contenuto Power BI di Azure Active Directory](./media/howto-power-bi-content-pack/11.png) 

**Aggiungere visualizzazioni al dashboard**:  È possibile personalizzare il dashboard includendo visualizzazioni personalizzate del report e aggiungendole a esso. 

![Pacchetto di contenuto Power BI di Azure Active Directory](./media/howto-power-bi-content-pack/13.png) 
 
**Condivisione del dashboard**: È anche possibile condividere il dashboard con gli utenti nell'organizzazione. Dopo aver condiviso il report, gli utenti possono vedere i campi selezionati nel report stesso.
 
![Pacchetto di contenuto Power BI di Azure Active Directory](./media/howto-power-bi-content-pack/14.png) 

## <a name="schedule-a-daily-refresh-of-your-power-bi-report"></a>Pianificare un aggiornamento giornaliero dei report di Power BI

Per pianificare un aggiornamento giornaliero dei report di Power BI, passare a **Set di dati** > **Impostazioni** > **Pianifica aggiornamenti** e procedere come illustrato di seguito.
 
![Pacchetto di contenuto Power BI di Azure Active Directory](./media/howto-power-bi-content-pack/15.png) 

## <a name="update-to-newer-version-of-content-pack"></a>Aggiornare il pacchetto di contenuto a una versione più recente

Se si vuole aggiornare il pacchetto di contenuto a una versione più recente:

- Scaricare il nuovo pacchetto di contenuto e configurarlo in base alle istruzioni riportate in questo articolo.

- Dopo aver configurato il pacchetto, passare a **Origine dati** > **Impostazioni** > **Credenziali origine dati** e immettere nuovamente le credenziali.

    ![Pacchetto di contenuto Power BI di Azure Active Directory](./media/howto-power-bi-content-pack/16.png) 

Dopo aver verificato che la nuova versione del pacchetto di contenuto funziona come previsto, è possibile rimuovere la versione precedente, se necessario, eliminando i report e i set di dati sottostanti associati.

## <a name="troubleshoot-content-pack-errors"></a>Risoluzione dei problemi relativi ai pacchetti di contenuto

Quando si usa il pacchetto di contenuto, è possibile che si verifichino gli errori seguenti: 

- [Aggiornamento non riuscito](#refresh-failed) 
- [L'aggiornamento delle credenziali dell'origine dati non è riuscito](#failed-to-update-data-source-credentials) 
- [L'importazione dei dati richiede troppo tempo](#data-import-is-too-slow) 

Per informazioni generali su Power BI, vedere questi [articoli della Guida](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/).

### <a name="refresh-failed"></a>Aggiornamento non riuscito 
 
**Modalità di esposizione dell’errore**: Messaggio di posta elettronica inviato da Power BI o stato di errore nella cronologia aggiornamenti. 


| Causa | Modalità di correzione |
| ---   | ---        |
| Gli errori di aggiornamento non riuscito possono verificarsi quando le credenziali degli utenti che si connettono al pacchetto di contenuto sono state reimpostate ma non sono state aggiornate nelle impostazioni di connessione del pacchetto di contenuto. | Individuare in Power BI il set di dati corrispondente al dashboard dei log attività di Azure AD (**Log attività di Azure Active Directory**), scegliere Pianifica aggiornamenti e quindi immettere le credenziali di Azure AD. |
| Un aggiornamento può non riuscire a causa di problemi relativi ai dati nel pacchetto di contenuto sottostante. | [Inviare un ticket di supporto](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 
 
### <a name="failed-to-update-data-source-credentials"></a>L'aggiornamento delle credenziali dell'origine dati non è riuscito 
 
**Modalità di esposizione dell’errore**: In Power BI durante la connessione al pacchetto di contenuto dei log attività di Azure AD. 

| Causa | Modalità di correzione |
| ---   | ---        |
| L'utente che sta tentando di connettersi non è un amministratore globale, non dispone di un ruolo con autorizzazioni di lettura per la sicurezza né è un amministratore della sicurezza. | Per accedere ai pacchetti di contenuto, usare un account di amministratore globale, di amministratore della sicurezza o con autorizzazioni di lettura per la sicurezza. |
| Il tenant non è un tenant Premium o non include almeno un utente con un file di licenza Premium. | [Inviare un ticket di supporto](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 
### <a name="data-import-is-too-slow"></a>L'importazione dei dati è troppo lenta 
 
**Modalità di esposizione dell’errore**: In Power BI, dopo la connessione del pacchetto di contenuto, viene avviato il processo di importazione dei dati per preparare il dashboard per il log attività di Azure AD. Viene visualizzato il messaggio: **Importazione di dati...** senza alcun ulteriore avanzamento.  

| Causa | Modalità di correzione |
| ---   | ---        |
| A seconda delle dimensioni del tenant, questo passaggio può richiedere da alcuni minuti a 30 minuti. | Se entro un'ora il messaggio non cambia e non viene visualizzato il dashboard, [inviare un ticket di supporto](../fundamentals/active-directory-troubleshooting-support-howto.md).|
  
## <a name="next-steps"></a>Passaggi successivi

* [Installare un pacchetto di contenuto di Power BI](quickstart-install-power-bi-content-pack.md).
* [Informazioni sui report di Azure AD](overview-reports.md).
