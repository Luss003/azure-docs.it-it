---
title: Riferimento alle impostazioni di roaming di Windows 10 | Microsoft Docs
description: Elenco completo di tutte le impostazioni di cui verrà effettuato il roaming o il backup in Windows 10.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: troubleshooting
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: na
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9489228b6ba81ae8d09e0f6880634532d0c836c6
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/01/2019
ms.locfileid: "67481799"
---
# <a name="windows-10-roaming-settings-reference"></a>Riferimento alle impostazioni di roaming di Windows 10
Di seguito è riportato un elenco completo di tutte le impostazioni di cui verrà effettuato il roaming o il backup in Windows 10. 

## <a name="devices-and-endpoints"></a>Dispositivi ed endpoint
La tabella seguente contiene un riepilogo dei dispositivi e dei tipi di account supportati dal framework di sincronizzazione, backup e ripristino in Windows 10.

| Tipo di account e operazione | Desktop | Cellulare |
| --- | --- | --- |
| Azure Active Directory: sincronizzazione |Yes |No |
| Azure Active Directory: backup e ripristino |No |No |
| Account Microsoft: sincronizzazione |Yes |Yes |
| Account Microsoft: backup e ripristino |No |Yes |

## <a name="what-is-backup"></a>Cosa si intende per backup
Le impostazioni di Windows vengono in genere sincronizzate per impostazione predefinita. Di alcune impostazioni, come ad esempio l'elenco delle applicazioni installate in un dispositivo, viene tuttavia eseguito solo il backup. Al momento il backup è disponibile solo per i dispositivi mobili e non è fruibile dagli utenti del servizio Enterprise State Roaming. Il backup usa un account Microsoft e archivia i dati delle impostazioni e delle applicazioni in OneDrive. Se un utente disabilita la sincronizzazione nel dispositivo usando l'app Impostazioni, i dati dell'applicazione che normalmente vengono sincronizzati diventano solo di backup. L'accesso ai dati di backup è possibile solo con l'operazione di ripristino durante la prima esecuzione di un nuovo dispositivo. I backup possono essere disattivati con le impostazioni del dispositivo e possono essere gestiti ed eliminati tramite l'account OneDrive dell'utente.

## <a name="windows-settings-overview"></a>Panoramica sulle impostazioni di Windows
I gruppi di impostazioni seguenti permettono agli utenti finali di abilitare o disabilitare la sincronizzazione delle impostazioni nei dispositivi Windows 10.

* Tema: sfondo del desktop, icona utente, posizione della barra delle applicazioni e così via. 
* Impostazioni di Internet Explorer: cronologia esplorazioni, URL tipizzati, Preferiti e così via. 
* Password: gestione delle credenziali di Windows, inclusi i profili Wi-Fi 
* Preferenze lingua: dizionario per il controllo ortografico, impostazioni della lingua del sistema. 
* Accessibilità: Assistente vocale, tastiera su schermo, lente di ingrandimento. 
* Altre impostazioni di Windows: vedere la sezione Dettagli relativi alle impostazioni di Windows.
* Impostazioni del browser Microsoft Edge: Preferiti di Microsoft Edge,Elenco di lettura e altre impostazioni

![Sincronizzare le impostazioni](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-syncyoursettings.png)

La sincronizzazione del gruppo di impostazioni del browser Microsoft Edge (Preferiti, Elenco di lettura) può essere abilitata o disabilitata dagli utenti finali tramite la relativa opzione del menu Impostazioni del browser Microsoft Edge.

![Account](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-edge.png)

Per Windows 10 versione 1803 o successiva, la sincronizzazione dei gruppi di impostazioni di Internet Explorer (preferiti, URL digitati) può essere abilitata o disabilitata dagli utenti finali tramite l'opzione del menu Impostazioni di Internet Explorer. 

![Impostazioni](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-ie.png)

## <a name="windows-settings-details"></a>Dettagli relativi alle impostazioni di Windows
Nella tabella seguente, la voce Altre nella colonna Gruppo di impostazioni fa riferimento alle impostazioni che possono essere disabilitate da Impostazioni > Account > Sincronizza le impostazioni > Altre impostazioni di Windows. 

La voce Interne nella colonna Gruppo di impostazioni fa riferimento alle impostazioni e alle app che possono essere disabilitate soltanto tramite la sincronizzazione dall'interno dell'app stessa o disabilitando la sincronizzazione per l'intero dispositivo con la gestione dei dispositivi mobili (software MDM) o le impostazioni dei Criteri di gruppo.
Le impostazioni di cui non viene effettuato il roaming o la sincronizzazione non appartengono a un gruppo.

| Impostazioni | Desktop | Cellulare | Group |
| --- | --- | --- | --- |
| **Account**: immagine dell'account |sync |X |Tema |
| **Account**: altre impostazioni account |X |X | |
| **Banda larga mobile avanzata**: nome della rete di condivisione della connessione Internet, che consente il rilevamento automatico degli hotspot Wi-Fi per dispositivi mobili tramite Bluetooth |X |X |Password |
| **Dati app**: singole app possono sincronizzare i dati |backup sincronizzazione |backup sincronizzazione |Interne |
| **Elenco app**: elenco delle app installate |X |backup |Altre |
| **Bluetooth**: tutte le impostazioni Bluetooth |X |X | |
| **Prompt dei comandi**: impostazioni dei valori predefiniti del prompt dei comandi |sync |X |Interne |
| **Credenziali**: Casella di sicurezza delle credenziali |sync |sync |password |
| **Data, ora e opzioni internazionali**: ora automatica (sincronizzazione con l'ora Internet) |sync |sync |language |
| **Data, ora e area**: formato 24 ore |sync |X |language |
| **Data, ora e opzioni internazionali**: data e ora |sync |X |language |
| **Data, ora e opzioni internazionali**: fuso orario | |X |language |
| **Data, ora e opzioni internazionali**: ora legale |sync |X |language |
| **Data, ora e opzioni internazionali**: paese/area geografica |sync |X |language |
| **Data, ora e opzioni internazionali**: primo giorno della settimana |sync |X |language |
| **Data, ora e opzioni internazionali**: formato regionale (impostazioni locali) |sync |X |language |
| **Data, ora e opzioni internazionali**: data breve |sync |X |language |
| **Data, ora e opzioni internazionali**: data estesa |sync |X |language |
| **Data, ora e opzioni internazionali**: ora breve |sync |X |Lingua |
| **Data, ora e opzioni internazionali**: ora estesa |sync |X |language |
| **Personalizzazione del desktop**: tema del desktop (sfondo, colore di sistema, segnali acustici sistema predefiniti, screen saver) |sync |X |Tema |
| **Personalizzazione del desktop**: sfondo presentazione |sync |X |Tema |
| **Personalizzazione del desktop**: impostazioni della barra delle applicazioni (posizione, Nascondi automaticamente e così via) |sync |X |Tema |
| **Personalizzazione del desktop**: layout della schermata Start |X |backup | |
| **Dispositivi**: stampanti condivise a cui è stata eseguita la connessione |X |X |Altre |
| **Browser Microsoft Edge**: Elenco di lettura |sync |sync |Interne |
| **Browser Microsoft Edge**: Preferiti |sync |sync |Interne |
| **Browser Microsoft Edge**: siti principali <sup>[[1]](#footnote-1)</sup> |sync |sync |Interne |
| **Browser Microsoft Edge**: URL digitati <sup>[[1]](#footnote-1)</sup> |sync |sync |Interne |
| **Browser Microsoft Edge**: impostazioni barra Preferiti <sup>[[1]](#footnote-1)</sup> |sync |sync |Interne |
| **Browser Microsoft Edge**: mostra pulsante Home <sup>[[1]](#footnote-1)</sup> |sync |sync |Interne |
| **Browser Microsoft Edge**: Blocca popup <sup>[[1]](#footnote-1)</sup> |sync |sync |Interne |
| **Browser Microsoft Edge**: richiedi l'operazione da eseguire con ogni download <sup>[[1]](#footnote-1)</sup> |sync |sync |Interne |
| **Browser Microsoft Edge**: offri la possibilità di salvare le password <sup>[[1]](#footnote-1)</sup> |sync |sync |Interne |
| **Browser Microsoft Edge**: invia richieste Do Not Track <sup>[[1]](#footnote-1)</sup> |sync |sync |Interne |
| **Browser Microsoft Edge**: salva i dati immessi nei moduli <sup>[[1]](#footnote-1)</sup> |sync |sync |Interne |
| **Browser Microsoft Edge**: mostra suggerimenti per la ricerca e i siti durante la digitazione <sup>[[1]](#footnote-1)</sup> |sync |sync |Interne |
| **Browser Microsoft Edge**: preferenze cookie <sup>[[1]](#footnote-1)</sup> |sync |sync |Interne |
| **Browser Microsoft Edge**: consenti ai siti di archiviare le licenze per i contenuti multimediali protetti nel dispositivo <sup>[[1]](#footnote-1)</sup> |sync |sync |Interne |
| **Browser Microsoft Edge**: impostazione utilità per la lettura dello schermo <sup>[[1]](#footnote-1)</sup> |sync |sync |Interne |
| **Contrasto elevato**: ON oppure OFF |sync |X |Accessibilità |
| **Contrasto elevato**: Impostazioni tema |sync |X |Accessibilità |
| **Internet Explorer**: schede aperte (URL e titolo) |sync |sync |Internet Explorer |
| **Internet Explorer**: Elenco di lettura |sync |sync |Internet Explorer |
| **Internet Explorer**: URL digitati |sync |sync |Internet Explorer |
| **Internet Explorer**: cronologia esplorazioni |sync |sync |Internet Explorer |
| **Internet Explorer**: Preferiti |sync |sync |Internet Explorer |
| **Internet Explorer**: URL esclusi |sync |sync |Internet Explorer |
| **Internet Explorer**: home page |sync |sync |Internet Explorer |
| **Internet Explorer**: suggerimenti dominio |sync |sync |Internet Explorer |
| **Tastiera**: abilitazione/disabilitazione della tastiera su schermo |sync |X |Accessibilità |
| **Tastiera**: Attiva Tasti permanenti (disabilitati per impostazione predefinita) |sync |X |Accessibilità |
| **Tastiera**: Attiva Filtro tasti (disabilitato per impostazione predefinita) |sync |X |Accessibilità |
| **Tastiera**: Attiva Segnali acustici (disabilitati per impostazione predefinita) |sync |X |Accessibilità |
| **Internet Explorer**: Lingua dominio: Cinese (CHS) QWERTY - abilitazione dell'apprendimento automatico |sync |X |Linguaggio |
| **Lingua**: cinese (QWERTY), abilitazione della classificazione dinamica dei candidati |sync |X |Linguaggio |
| **Lingua**: cinese (QWERTY), set di caratteri cinese semplificato |sync |X |Linguaggio |
| **Lingua**: cinese (QWERTY), set di caratteri cinese tradizionale |sync |X |Linguaggio |
| **Lingua**: cinese (QWERTY), Pinyin fuzzy |sync |backup |Linguaggio |
| **Lingua**: cinese (QWERTY), coppie fuzzy |sync |backup |Linguaggio |
| **Lingua**: cinese (QWERTY), Pinyin completo |sync |X |Linguaggio |
| **Lingua**: cinese (QWERTY), Pinyin doppio |sync |X |Linguaggio |
| **Lingua**: cinese (QWERTY), correzione automatica lettura |sync |X |Linguaggio |
| **Lingua**: cinese (QWERTY), cambio tastiera C/E, MAIUSC |sync |X |Linguaggio |
| **Lingua**: cinese (QWERTY), cambio tastiera C/E, CTRL |sync |X |Linguaggio |
| **Lingua**: cinese (WUBI), modalità di input a singolo carattere |sync |X |Linguaggio |
| **Lingua**: cinese (WUBI), mostra il codice rimanente del candidato |sync |X |Linguaggio |
| **Lingua**: cinese (WUBI), emetti un segnale acustico se la codifica 4 non è valida |sync |X |Linguaggio |
| **Lingua**: Bopomofo cinese tradizionale, incluso CJK estensione A |sync |X |Linguaggio |
| **Lingua**: IME giapponese, digitazione predittiva e parole personalizzate |sync |sync |Linguaggio |
| **Lingua**: Coreano (Corea) IME |X |X |Linguaggio |
| **Lingua**: riconoscimento della grafia |X |X |Linguaggio |
| **Lingua**: profilo lingua |sync |backup |Linguaggio |
| **Lingua**: controllo ortografico, correzione automatica ed evidenziazione degli errori di ortografia |sync |backup |Linguaggio |
| **Lingua**: elenco delle tastiere |sync |backup |Linguaggio |
| **Schermata di blocco**: tutte le impostazioni della schermata di blocco |X |X | |
| **Lente di ingrandimento**: abilitazione/disabilitazione (master) |X |X |Accessibilità |
| **Lente di ingrandimento**: Attiva inversione colori (disattivata per impostazione predefinita) |sync |X |Accessibilità |
| **Lente di ingrandimento**: rilevamento, Segui la tastiera |sync |X |Accessibilità |
| **Lente di ingrandimento**: rilevamento, Segui il puntatore del mouse |sync |X |Accessibilità |
| **Lente di ingrandimento**: avvio all'accesso dell'utente (disabilitato per impostazione predefinita) |sync |X |Accessibilità |
| **Mouse**: modifica delle dimensioni del cursore del mouse |sync |X |Altre |
| **Mouse**: modifica del colore del cursore del mouse |sync |X |Altre |
| **Mouse**: tutte le altre impostazioni |X |X | |
| **Assistente vocale**: avvio veloce |sync |X |Accessibilità |
| **Assistente vocale**: gli utenti possono modificare la tonalità di voce dell'Assistente vocale |sync |X |Accessibilità |
| **Assistente vocale**: gli utenti possono abilitare/disabilitare la lettura di suggerimenti per gli elementi comuni da parte dell'Assistente vocale (abilitata per impostazione predefinita) |sync |X |Accessibilità |
| **Assistente vocale**: gli utenti possono abilitare/disabilitare la lettura dei caratteri digitati (abilitata per impostazione predefinita) |sync |X |Accessibilità |
| **Assistente vocale**: gli utenti possono abilitare/disabilitare la lettura delle parole digitate (abilitata per impostazione predefinita) |sync |X |Accessibilità |
| **Assistente vocale**: il cursore di inserimento segue l'Assistente vocale (abilitata per impostazione predefinita) |sync |X |Accessibilità |
| **Assistente vocale**: Abilita evidenziazione visiva del cursore dell'Assistente vocale (abilitata per impostazione predefinita) |sync |X |Accessibilità |
| **Assistente vocale**: Riproduci segnali acustici (abilitata per impostazione predefinita) |sync |X |Accessibilità |
| **Assistente vocale**: Attiva tasti della tastiera virtuale quando l'utente solleva il dito (disattivata per impostazione predefinita) |sync |X |Accessibilità |
| **Accessibilità**: Imposta lo spessore del cursore intermittente |sync |X |Accessibilità |
| **Accessibilità**: Rimuovi immagini di sfondo (disabilitata per impostazione predefinita) |sync |X |Accessibilità |
| **Alimentazione e sospensione**: tutte le impostazioni |X |X | |
| **Personalizzazione della schermata Start**: colore principale (solo telefono) |X |sync |Tema |
| **Digitazione**: dizionario per il controllo ortografico |sync |backup |Linguaggio |
| **Digitazione**: Correggi automaticamente errori di ortografia |sync |backup |Linguaggio |
| **Digitazione**: Evidenzia errori di ortografia |sync |backup |Linguaggio |
| **Digitazione**: Mostra suggerimenti di testo durante la digitazione |sync |backup |Linguaggio |
| **Digitazione**: Aggiungi uno spazio quando scelgo un suggerimento di testo |sync |backup |Linguaggio |
| **Digitazione**: Aggiungi un punto dopo un doppio tocco sulla BARRA SPAZIATRICE |sync |backup |Linguaggio |
| **Digitazione**: Applica maiuscola a inizio di frase |sync |backup |Linguaggio |
| **Digitazione**: Usa tutte maiuscole dopo doppio tocco del tasto MAIUSC |sync |backup |Linguaggio |
| **Digitazione**: Riproduci suoni tasti durante la digitazione |sync |backup |Linguaggio |
| **Digitazione**: dati di personalizzazione per la tastiera virtuale |sync |backup |Linguaggio |
| **Wi-Fi**: profili Wi-Fi (solo WPA) |sync |sync |Password |

###### <a name="footnote-1"></a>Nota 1
Versione minima sistema operativo supportata di Windows Creators Update (Build 15063). 

## <a name="next-steps"></a>Passaggi successivi

Per informazioni generali, vedere [Panoramica di Enterprise State Roaming](enterprise-state-roaming-overview.md).
