---
title: File di inclusione
description: File di inclusione
services: scheduler
ms.service: scheduler
author: derek1ee
ms.topic: include
ms.date: 08/16/2016
ms.author: deli
ms.custom: include file
ms.openlocfilehash: b3788ede23a423bebf96661ea88b227bfb5fdf4c
ms.sourcegitcommit: cd70273f0845cd39b435bd5978ca0df4ac4d7b2c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2019
ms.locfileid: "67180523"
---
Nella tabella seguente viene descritto ciascuno dei principali quote, limiti, impostazioni predefinite e limitazioni nell'utilità di pianificazione di Azure.

| Risorsa | Descrizione limite |
| -------- | ----------------- |
| **Dimensioni del processo** | Le dimensioni massime del processo sono pari a 16.000. Se un'operazione PUT o PATCH determina un processo di dimensioni maggiori rispetto a questo limite, viene restituito un codice di stato 400 di richiesta non valida. | 
| **Raccolte processi** | Il numero massimo di raccolte processi per ogni sottoscrizione di Azure è 200.000. | 
| **Processi per raccolta** | Per impostazione predefinita, il numero massimo è di cinque processi per le raccolte di processi gratuite e di 50 processi nelle raccolte di processi standard. <p>È possibile modificare il numero massimo di processi nelle raccolte di processi. Tutti i processi in una raccolta sono limitati al valore impostato nella raccolta stessa. Se si prova a creare più processi rispetto alla quota massima, la richiesta ha esito negativo e restituisce un codice di stato 409 conflitto. | 
| **Tempo dall’ora di inizio** | Il massimo "tempo dall'ora di inizio" è di 18 mesi. |
| **Intervallo di ricorrenza** | L'intervallo massimo delle ricorrenze è di 18 mesi. | 
| **Frequenza** | Per impostazione predefinita, la frequenza massima è di un'ora per le raccolte di processi gratuite e di un minuto nelle raccolte di processi standard. <p>È possibile impostare la frequenza massima in una raccolta di processi su un valore inferiore a quello massimo consentito. Tutti i processi nella raccolta sono limitati al valore impostato nella raccolta stessa. Se si prova a creare un processo con una frequenza maggiore rispetto alla frequenza massima nella raccolta, la richiesta ha esito negativo con codice di stato 409 Conflitto. | 
| **Dimensioni del corpo** | La dimensione massima del corpo per una richiesta è 8.192 caratteri. |
| **Dimensioni dell’URL della richiesta** | La dimensione massima per un URL di richiesta è 2.048 caratteri. |
| **Numero di intestazioni** | Il numero massimo di intestazioni è di 50 intestazioni. | 
| **Dimensioni aggregate dell'intestazione** | Le dimensioni massime dell'intestazione di aggregazione sono 4.096 caratteri. |
| **Timeout** | Il timeout della richiesta è statico, ovvero non è configurabile. e è 60 secondi per le azioni HTTP. Per le operazioni con tempi di esecuzione più lunghi, seguire i protocolli asincroni HTTP. Restituire, ad esempio, un errore 202 immediatamente, ma continuare a lavorare in background. | 
| **Cronologia processi** | Il corpo della risposta massimo archiviato nella cronologia processo è pari a 2.048 byte. |
| **Periodo di memorizzazione della cronologia dei processi** | La cronologia processo viene mantenuta per un massimo di due mesi o fino alle ultime 1.000 esecuzioni. | 
| **Memorizzazione di processi completati e con errori** | I processi completati e con errori vengono conservati per 60 giorni. |
||| 

