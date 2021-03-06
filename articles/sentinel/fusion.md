---
title: Rilevamento avanzato degli attacchi multifase in Sentinel di Azure
description: Usa la tecnologia Fusion in Sentinel di Azure per ridurre la fatica degli avvisi e creare eventi imprevisti che si basano sul rilevamento di attacchi multifase avanzati.
services: sentinel
documentationcenter: na
author: yelevin
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/18/2020
ms.author: yelevin
ms.openlocfilehash: 87ca322cbdfdd8a53a3ecefcb120a961ea1bb936
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/25/2020
ms.locfileid: "77587924"
---
# <a name="advanced-multistage-attack-detection-in-azure-sentinel"></a>Rilevamento avanzato degli attacchi multifase in Sentinel di Azure


> [!IMPORTANT]
> Alcune funzionalità Fusion in Sentinel di Azure sono attualmente disponibili in anteprima pubblica.
> Queste funzionalità vengono fornite senza un contratto di servizio e non sono consigliate per i carichi di lavoro di produzione. Alcune funzionalità potrebbero non essere supportate o potrebbero presentare funzionalità limitate. Per altre informazioni, vedere [Condizioni supplementari per l'utilizzo delle anteprime di Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).



Grazie alla tecnologia Fusion basata sull'apprendimento automatico, Azure Sentinel può rilevare automaticamente gli attacchi multifase combinando comportamenti anomali e attività sospette osservate in varie fasi della kill chain. Azure Sentinel genera quindi eventi imprevisti che altrimenti sarebbero molto difficili da rilevare. Questi eventi imprevisti racchiuderanno due o più avvisi o attività. Per impostazione predefinita, questi eventi imprevisti sono un volume ridotto, una fedeltà elevata e una gravità elevata.

Personalizzato per l'ambiente in uso, questo rilevamento non solo riduce le percentuali di falsi positivi, ma può anche rilevare attacchi con informazioni limitate o mancanti.

## <a name="configuration-for-advanced-multistage-attack-detection"></a>Configurazione per il rilevamento di attacchi multifase avanzati

Questo rilevamento è abilitato per impostazione predefinita in Sentinel di Azure. Per controllare lo stato o per disabilitarlo probabilmente perché si sta usando una soluzione alternativa per creare eventi imprevisti basati su più avvisi, usare le istruzioni seguenti:

1. Accedere al [portale di Azure](https://portal.azure.com), se questa operazione non è già stata eseguita.

2. Passare ad **Azure Sentinel** > **Configuration** > **Analytics**

3. Selezionare **regole attive** e individuare **Rilevamento attacchi multifase avanzati** nella colonna **nome** . Controllare la colonna **stato** per verificare se il rilevamento è abilitato o disabilitato.

4. Per modificare lo stato, selezionare questa voce e nel pannello **Advanced MultiStage Attack Detection** selezionare Edit ( **modifica**).

5. Nel pannello della **creazione guidata regola** , la modifica dello stato viene selezionata automaticamente, quindi selezionare **Avanti: verifica**, quindi **Salva**. 

I modelli di regole non sono applicabili per il rilevamento degli attacchi multifase avanzati.

> [!NOTE]
> Azure Sentinel usa attualmente i dati cronologici per 30 giorni per eseguire il training dei sistemi di machine learning. Questi dati vengono sempre crittografati usando le chiavi di Microsoft mentre passano attraverso la pipeline di machine learning. Tuttavia, i dati di training non vengono crittografati con le [chiavi gestite dal cliente (CMK)](customer-managed-keys.md) se è stato abilitato CMK nell'area di lavoro di Azure Sentinel. Per rifiutare esplicitamente la fusione, passare ad **Azure Sentinel** \> **configurazione** \> **Analytics \> regole attive \> rilevamento degli attacchi multifase avanzati** e nella colonna **stato** selezionare **Disabilita.**

## <a name="fusion-using-palo-alto-networks-and-microsoft-defender-atp"></a>Fusion con Palo Alto Networks e Microsoft Defender ATP

Questi scenari combinano due dei log fondamentali usati dagli analisti della sicurezza: i log del firewall da Palo Alto Networks e i log di rilevamento dei punti finali da Microsoft Defender ATP. In tutti gli scenari elencati di seguito, viene rilevata un'attività sospetta nell'endpoint che coinvolge un indirizzo IP esterno, quindi questo è seguito da un traffico anomalo dall'indirizzo IP esterno al firewall. Nei registri di palo alto, Azure Sentinel si concentra sui [log delle minacce](https://docs.paloaltonetworks.com/pan-os/8-1/pan-os-admin/monitoring/view-and-manage-logs/log-types-and-severity-levels/threat-logs)e il traffico viene considerato sospetto quando sono consentite le minacce (dati sospetti, file, inondazioni, pacchetti, analisi, spyware, URL, virus, vulnerabilità, incendi-virus, incendi).

### <a name="network-request-to-tor-anonymization-service-followed-by-anomalous-traffic-flagged-by-palo-alto-networks-firewall"></a>Richiesta di rete al servizio TOR anonimato dei seguita da traffico anomalo contrassegnato da Palo Alto Networks firewall.

In questo scenario, Sentinel di Azure rileva prima di tutto un avviso che Microsoft Defender Advanced Threat Protection ha rilevato una richiesta di rete a un servizio TOR anonimato dei che causa un'attività anomala. Questa operazione è stata avviata nell'account {nome account} con ID SID {SID} alle {time}. L'indirizzo IP in uscita per la connessione è {IndividualIp}.
Quindi, l'attività insolita è stata rilevata dal firewall Palo Alto Networks in {TimeGenerated}. Ciò indica che il traffico dannoso è stato immesso nella rete. l'indirizzo IP di destinazione per il traffico di rete è {DestinationIP}.

Questo scenario è attualmente disponibile in anteprima pubblica.


### <a name="powershell-made-a-suspicious-network-connection-followed-by-anomalous-traffic-flagged-by-palo-alto-networks-firewall"></a>PowerShell ha effettuato una connessione di rete sospetta seguita da traffico anomalo contrassegnato da Palo Alto Networks firewall.

In questo scenario, Sentinel di Azure rileva prima di tutto un avviso che Microsoft Defender Advanced Threat Protection ha rilevato che PowerShell ha rilevato una connessione di rete sospetta che causa un'attività anomala rilevata da un firewall di rete palo alto. Questa operazione è stata avviata dall'account {nome account} con ID SID {SID} alle {time}. L'indirizzo IP in uscita per la connessione è {IndividualIp}. Quindi, l'attività insolita è stata rilevata dal firewall Palo Alto Networks in {TimeGenerated}. Ciò indica che il traffico dannoso è stato inserito nella rete. L'indirizzo IP di destinazione per il traffico di rete è {DestinationIP}.

Questo scenario è attualmente disponibile in anteprima pubblica.

### <a name="outbound-connection-to-ip-with-a-history-of-unauthorized-access-attempts-followed-by-anomalous-traffic-flagged-by-palo-alto-networks-firewall"></a>Connessione in uscita a IP con una cronologia di tentativi di accesso non autorizzati seguito da traffico anomalo contrassegnato da Palo Alto Networks firewall

In questo scenario, Sentinel di Azure rileva un avviso che Microsoft Defender Advanced Threat Protection ha rilevato una connessione in uscita a un indirizzo IP con una cronologia dei tentativi di accesso non autorizzati che causano un'attività anomala rilevata da palo alto Firewall Reti. Questa operazione è stata avviata dall'account {nome account} con ID SID {SID} alle {time}. L'indirizzo IP in uscita per la connessione è {IndividualIp}. Dopo questa operazione, è stata rilevata un'attività insolita da Palo Alto Networks firewall in {TimeGenerated}. Ciò indica che il traffico dannoso è stato inserito nella rete. L'indirizzo IP di destinazione per il traffico di rete è {DestinationIP}.

Questo scenario è attualmente disponibile in anteprima pubblica.



## <a name="fusion-using-identity-protection-and-microsoft-cloud-app-security"></a>Fusion con Identity Protection e Microsoft Cloud App Security

Grazie al rilevamento avanzato degli attacchi multifase, Azure Sentinel supporta gli scenari seguenti che combinano eventi di anomalia Azure Active Directory Identity Protection e Microsoft Cloud App Security:

- [Trasferimento Impossibile a una posizione atipica seguita da un'attività anomala di Office 365](#impossible-travel-to-atypical-location-followed-by-anomalous-office-365-activity)
- [Attività di accesso per una posizione non nota seguita da un'attività anomala di Office 365](#sign-in-activity-for-unfamiliar-location-followed-by-anomalous-office-365-activity)
- [Attività di accesso da dispositivo infetto seguita da un'attività anomala di Office 365](#sign-in-activity-from-infected-device-followed-by-anomalous-office-365-activity)
- [Attività di accesso da un indirizzo IP anonimo seguito da un'attività anomala di Office 365](#sign-in-activity-from-anonymous-ip-address-followed-by-anomalous-office-365-activity)
- [Attività di accesso da un utente con credenziali perse seguite da un'attività anomala di Office 365](#sign-in-activity-from-user-with-leaked-credentials-followed-by-anomalous-office-365-activity)

È necessario disporre del [connettore Azure ad Identity Protection data](connect-azure-ad-identity-protection.md) e dei connettori di [cloud app Security](connect-cloud-app-security.md) configurati.

Nelle descrizioni seguenti, Azure Sentinel visualizzerà il valore effettivo dei dati rappresentati in questa pagina come variabili tra parentesi quadre. Ad esempio, il nome visualizzato effettivo di un account anziché il *nome dell'account*\<> e il numero effettivo anziché il numero *\<>* .

### <a name="impossible-travel-to-atypical-location-followed-by-anomalous-office-365-activity"></a>Trasferimento Impossibile a una posizione atipica seguita da un'attività anomala di Office 365

Ci sono sette possibili eventi imprevisti di Azure Sentinel che combinano la trasmissione Impossibile a avvisi di posizione atipica da Azure AD Identity Protection e gli avvisi anomali di Office 365 generati da Microsoft Cloud App Security:

- **Trasferimento Impossibile a posizioni atipiche che portano alla cassetta postale di Office 365 exfiltration**
    
    Questo avviso indica un evento di accesso per *nome account*\<> da un *percorso*non possibile a \<>, una posizione atipica, seguita da una regola di invio di posta in arrivo sospetta, è stata impostata nella posta in arrivo di un utente.
    
    Questo può indicare che l'account è compromesso e che la cassetta postale viene utilizzata per sottrarre le informazioni dell'organizzazione. Il nome dell' *account*utente \<> ha creato o aggiornato una regola di invio della posta in arrivo che invia tutti i messaggi di posta elettronica in arrivo all'indirizzo esterno \<*indirizzo di posta elettronica*>.

- **Trasferimento Impossibile a posizioni atipiche che comportano attività amministrative sospette per le app Cloud**
    
    Questo avviso indica un evento di accesso per *nome account*\<> da un percorso non possibile a un *percorso*di \<>, una località atipica.
    
    Successivamente, l'account \<*nome account*> eseguito \<*numero*> attività amministrative in una singola sessione.

- **Trasferimento Impossibile a posizioni atipiche che comportano l'eliminazione di file di massa**
    
    Questo avviso indica un evento di accesso per *nome account*\<> per \<*percorso*>, una posizione atipica. 
    
    Successivamente, l'account \<*nome account*> eliminato \<*numero di*> file univoci in una singola sessione.

- **Trasferimento Impossibile a posizioni atipiche che portano al download di file di massa**
    
    Questo avviso indica un evento di accesso per *nome account*\<> da un percorso non possibile a un *percorso*di \<>, una località atipica. 
    
    Successivamente, l'account \<*nome account*> scaricato \<*numero di*> file univoci in una singola sessione.

- **Trasferimento Impossibile a posizioni atipiche che comportano la rappresentazione di Office 365**
    
    Questo avviso indica un evento di accesso per *nome account*\<> da un percorso non possibile a un *percorso*di \<>, una località atipica. 
    
    Successivamente, l'account \<*nome account*> eseguito un importo insolito (\<*numero di attività*>) di attività di rappresentazione in una singola sessione.

- **Trasferimento Impossibile a posizioni atipiche che portano alla condivisione di file di massa**
    
    Questo avviso indica un evento di accesso per *nome account*\<> da un percorso non possibile a un *percorso*di \<>, una località atipica. 
    
    Successivamente, l'account \<*nome account*> condiviso \<*numero di*> file univoci in una singola sessione.

- **Trasferimento Impossibile a posizioni atipiche che portano a ransomware nell'app Cloud**
    
    Questo avviso indica un evento di accesso per *nome account*\<> da un percorso non possibile a un *percorso*di \<>, una località atipica. 
    
    Successivamente, l'account \<*nome account*> caricato \<*numero di*file > ed eliminato un totale di \<*numero di*file >. 
    
    Questo modello di attività è indicativo di un potenziale attacco ransomware.


### <a name="sign-in-activity-for-unfamiliar-location-followed-by-anomalous-office-365-activity"></a>Attività di accesso per una posizione non nota seguita da un'attività anomala di Office 365

Esistono sette possibili eventi imprevisti di Azure Sentinel che combinano l'attività di accesso per gli avvisi di posizione non nota da Azure AD Identity Protection e gli avvisi anomali di Office 365 generati da Microsoft Cloud App Security.

- **Evento di accesso da una posizione non nota che conduce alla cassetta postale di Exchange Online exfiltration**
    
    Questo avviso indica un evento di accesso per *nome account*\<> dal *percorso*\<>, una posizione non nota, seguita da una regola di invio sospetta della posta in arrivo è stata impostata nella posta in arrivo di un utente.
    
    Questo può indicare che l'account è compromesso e che la cassetta postale viene utilizzata per sottrarre le informazioni dell'organizzazione. Il nome dell' *account*utente \<> ha creato o aggiornato una regola di invio della posta in arrivo che invia tutti i messaggi di posta elettronica in arrivo all'indirizzo esterno \<*indirizzo di posta elettronica*>. 

- **Evento di accesso da una posizione non nota che causa un'attività amministrativa sospetta per le app Cloud**
    
    Questo avviso indica un evento di accesso per *nome account*\<> dal *percorso*\<>, una posizione non nota. 
    
    Successivamente, l'account \<*nome account*> eseguito \<*numero di*attività amministrative > in una singola sessione.

- **Evento di accesso da una posizione non nota che causa l'eliminazione di file di massa**
    
    Questo avviso indica un evento di accesso per *nome account*\<> dal *percorso*\<>, una posizione non nota. 
    
    Successivamente, l'account \<*nome account*> eliminato \<*numero di*> file univoci in una singola sessione.

- **Evento di accesso da una posizione non nota che conduce al download di file di massa**
    
    Questo avviso indica un evento di accesso per *nome account*\<> dal *percorso*\<>, una posizione non nota. 
    
    Successivamente, l'account \<*nome account*> scaricato \<*numero di*> file univoci in una singola sessione.

- **Evento di accesso da una posizione non nota che conduce alla rappresentazione di Office 365**
    
    Questo avviso indica un evento di accesso per *nome account*\<> dal *percorso*\<>, una posizione non nota.
    
    Successivamente, l'account \<*nome account*> rappresentato \<*numero di*> account diversi in una singola sessione.

- **Evento di accesso da una posizione non nota che conduce alla condivisione di file di massa**
    
    Questo avviso indica un evento di accesso per *nome account*\<> dal *percorso*\<>, una posizione non nota. 
    
    Successivamente, l'account \<*nome account*> condiviso \<*numero di*> file univoci in una singola sessione.

- **Evento di accesso da una posizione non nota che conduce a ransomware nell'app Cloud**
    
    Questo avviso indica un evento di accesso per *nome account*\<> dal *percorso*\<>, una posizione non nota. 
    
    Successivamente, l'account \<*nome account*> caricato \<*numero di*file > ed eliminato un totale di \<*numero di*file >. 
    
    Questo modello di attività è indicativo di un potenziale attacco ransomware.

### <a name="sign-in-activity-from-infected-device-followed-by-anomalous-office-365-activity"></a>Attività di accesso da dispositivo infetto seguita da un'attività anomala di Office 365

Esistono sette possibili eventi imprevisti di Azure Sentinel che combinano l'attività di accesso dagli avvisi di dispositivi infetti da Azure AD Identity Protection e da avvisi anomali di Office 365 generati da Microsoft Cloud App Security:

- **Evento di accesso da un dispositivo infetto che conduce alla cassetta postale di Office 365 exfiltration**
    
    Questo avviso indica un evento di accesso per *nome account*\<> da un dispositivo potenzialmente infetto da malware, seguito da una regola di invio di posta in arrivo sospetta impostata nella posta in arrivo di un utente.
    
    Questo può indicare che l'account è compromesso e che la cassetta postale viene utilizzata per sottrarre le informazioni dell'organizzazione. Il nome dell' *account*utente \<> ha creato o aggiornato una regola di invio della posta in arrivo che invia tutti i messaggi di posta elettronica in arrivo all'indirizzo esterno \<*indirizzo di posta elettronica*>. 

- **Evento di accesso da un dispositivo infetto che causa attività amministrative sospette per le app Cloud**
    
    Questo avviso indica un evento di accesso per *nome account*\<> da un dispositivo potenzialmente infetto da malware.
    
    Successivamente, l'account \<*nome account*> eseguito \<*numero di*attività amministrative > in una singola sessione.

- **Evento di accesso da un dispositivo infetto che causa l'eliminazione di file di massa**
    
    Questo avviso indica un evento di accesso per *nome account*\<> da un dispositivo potenzialmente infetto da malware. 
    
    Successivamente, l'account \<*nome account*> eliminato \<*numero di*> file univoci in una singola sessione.

- **Evento di accesso da un dispositivo infetto che conduce al download di file di massa**
    
    Questo avviso indica un evento di accesso per *nome account*\<> da un dispositivo potenzialmente infetto da malware. 
    
    Successivamente, l'account \<*nome account*> scaricato \<*numero di*> file univoci in una singola sessione.

- **Evento di accesso da un dispositivo infetto che conduce alla rappresentazione di Office 365**
    
    Questo avviso indica un evento di accesso per *nome account*\<> da un dispositivo potenzialmente infetto da malware. 
    
    Successivamente, l'account \<*nome account*> rappresentato \<*numero di*> account diversi in una singola sessione.

- **Evento di accesso da un dispositivo infetto che conduce alla condivisione di file di massa**
    
    Questo avviso indica un evento di accesso per *nome account*\<> da un dispositivo potenzialmente infetto da malware. 
    
    Successivamente, l'account \<*nome account*> condiviso \<*numero di*> file univoci in una singola sessione.

- **Evento di accesso da un dispositivo infetto che conduce a ransomware nell'app Cloud**
    
    Questo avviso indica un evento di accesso per *nome account*\<> da un dispositivo potenzialmente infetto da malware. 
    
    Successivamente, l'account \<*nome account*> caricato \<*numero di*file > ed eliminato un totale di \<*numero di*file >. 
    
    Questo modello di attività è indicativo di un potenziale attacco ransomware.

### <a name="sign-in-activity-from-anonymous-ip-address-followed-by-anomalous-office-365-activity"></a>Attività di accesso da un indirizzo IP anonimo seguito da un'attività anomala di Office 365

Esistono sette possibili eventi imprevisti di Azure Sentinel che combinano l'attività di accesso da avvisi di indirizzi IP anonimi da Azure AD Identity Protection e da avvisi anomali di Office 365 generati da Microsoft Cloud App Security:

- **Evento di accesso da un indirizzo IP anonimo che conduce alla cassetta postale di Office 365 exfiltration**
    
    Questo avviso indica un evento di accesso per *nome account*\<> da un indirizzo IP proxy anonimo \<> *indirizzo IP*, seguito da una regola di inoltro della posta in arrivo sospetta è stata impostata nella posta in arrivo di un utente.
    
    Questo può indicare che l'account è compromesso e che la cassetta postale viene utilizzata per sottrarre le informazioni dell'organizzazione. Il nome dell' *account*utente \<> ha creato o aggiornato una regola di invio della posta in arrivo che invia tutti i messaggi di posta elettronica in arrivo all'indirizzo esterno \<*indirizzo di posta elettronica*>. 

- **Evento di accesso da un indirizzo IP anonimo che causa un'attività amministrativa sospetta dell'app Cloud**
    
    Questo avviso indica un evento di accesso per *nome account*\<> da un indirizzo IP proxy anonimo \<*Indirizzo*IP >. 
    
    Successivamente, l'account \<*nome account*> eseguito \<*numero di*attività amministrative > in una singola sessione.

- **Evento di accesso da un indirizzo IP anonimo che causa l'eliminazione di file di massa**
    
    Questo avviso indica un evento di accesso per *nome account*\<> da un indirizzo IP proxy anonimo \<*Indirizzo*IP >. 
    
    Successivamente, l'account \<*nome account*> eliminato \<*numero di*> file univoci in una singola sessione.

- **Evento di accesso da un indirizzo IP anonimo che conduce al download di file di massa**
    
    Questo avviso indica un evento di accesso per *nome account*\<> da un indirizzo IP proxy anonimo \<*Indirizzo*IP >. 
    
    Successivamente, l'account \<*nome account*> scaricato \<*numero di*> file univoci in una singola sessione.

- **Evento di accesso da un indirizzo IP anonimo che conduce alla rappresentazione di Office 365**
    
    Questo avviso indica un evento di accesso per *nome account*\<> da un indirizzo IP proxy anonimo \<*Indirizzo*IP >. 
    
    Successivamente, l'account \<*nome account*> rappresentato \<*numero di*> account diversi in una singola sessione.

- **Evento di accesso da un indirizzo IP anonimo che conduce alla condivisione di file di massa**
    
    Questo avviso indica un evento di accesso per *nome account*\<> da un indirizzo IP proxy anonimo \<*Indirizzo*IP >. 
    
    Successivamente, l'account \<*nome account*> condiviso \<*numero di*> file univoci in una singola sessione.

- **Evento di accesso da un indirizzo IP anonimo a ransomware nell'app Cloud**
    
    Questo avviso indica un evento di accesso per *nome account*\<> da un indirizzo IP proxy anonimo \<*Indirizzo*IP >. 
    
    Successivamente, l'account \<*nome account*> caricato \<*numero di*file > ed eliminato un totale di \<*numero di*file >. 
    
    Questo modello di attività è indicativo di un potenziale attacco ransomware.

### <a name="sign-in-activity-from-user-with-leaked-credentials-followed-by-anomalous-office-365-activity"></a>Attività di accesso da un utente con credenziali perse seguite da un'attività anomala di Office 365

Esistono sette possibili eventi imprevisti di Azure Sentinel che combinano l'attività di accesso dell'utente con avvisi di credenziali perse da Azure AD Identity Protection e avvisi anomali di Office 365 generati da Microsoft Cloud App Security:

- **Evento di accesso dall'utente con credenziali perse che portano alla cassetta postale di Office 365 exfiltration**
    
    Questo avviso indica che l'evento di accesso per *nome di account*\<> utilizzato le credenziali perse, seguito da una regola di invio sospetta della posta in arrivo è stata impostata nella posta in arrivo di un utente. 
    
    Questo può indicare che l'account è compromesso e che la cassetta postale viene utilizzata per sottrarre le informazioni dell'organizzazione. Il nome dell' *account*utente \<> ha creato o aggiornato una regola di invio della posta in arrivo che invia tutti i messaggi di posta elettronica in arrivo all'indirizzo esterno \<*indirizzo di posta elettronica*>. 

- **Evento di accesso dall'utente con credenziali perse che causano attività amministrative sospette per le app Cloud**
    
    Questo avviso indica che l'evento di accesso per *nome account*\<> utilizzato le credenziali perse.
    
    Successivamente, l'account \<*nome account*> eseguito \<*numero di*attività amministrative > in una singola sessione.

- **Evento di accesso dall'utente con credenziali perse che portano all'eliminazione di file di massa**
    
    Questo avviso indica che l'evento di accesso per *nome account*\<> utilizzato le credenziali perse.
    
    Successivamente, l'account \<*nome account*> eliminato \<*numero di*> file univoci in una singola sessione.

- **Evento di accesso dall'utente con credenziali perse che portano al download di file di massa**
    
    Questo avviso indica che l'evento di accesso per *nome account*\<> utilizzato le credenziali perse.
    
    Successivamente, l'account \<*nome account*> scaricato \<*numero di*> file univoci in una singola sessione.

- **Evento di accesso dall'utente con credenziali perse che portano alla rappresentazione di Office 365**
    
    Questo avviso indica che l'evento di accesso per *nome account*\<> utilizzato le credenziali perse. 
    
    Successivamente, l'account \<*nome account*> rappresentato \<*numero di*> account diversi in una singola sessione.

- **Evento di accesso dall'utente con credenziali perse che portano alla condivisione di file di massa**
    
    Questo avviso indica che l'evento di accesso per *nome account*\<> utilizzato le credenziali perse.
    
    Successivamente, l'account \<*nome account*> condiviso \<*numero di*> file univoci in una singola sessione.

- **Evento di accesso dall'utente con credenziali perse al ransomware nell'app Cloud**
    
    Questo avviso indica che l'evento di accesso per *nome account*\<> utilizzato le credenziali perse. 
    
    Successivamente, l'account \<*nome account*> caricato \<*numero di*file > ed eliminato un totale di \<*numero di*file >. 
    
    Questo modello di attività è indicativo di un potenziale attacco ransomware.

## <a name="next-steps"></a>Passaggi successivi

A questo punto si è appreso come ottenere altre informazioni sul rilevamento di attacchi multifase avanzati. si potrebbe essere interessati alla Guida introduttiva seguente per informazioni su come ottenere la visibilità dei dati e le potenziali minacce: [Introduzione a Sentinel di Azure](quickstart-get-visibility.md).

Se si è pronti per esaminare gli eventi imprevisti creati automaticamente, vedere l'esercitazione seguente: esaminare gli [eventi imprevisti con Sentinel di Azure](tutorial-investigate-cases.md).

