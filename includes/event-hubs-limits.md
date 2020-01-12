---
title: File di inclusione
description: File di inclusione
services: event-hubs
author: sethmanheim
ms.service: event-hubs
ms.topic: include
ms.date: 05/22/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 2aca4f2c236112b80e9fc985cf80ccad6d82bde3
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2020
ms.locfileid: "75901975"
---
Le tabelle seguenti forniscono le quote e i limiti specifici di [Hub eventi di Azure](https://azure.microsoft.com/services/event-hubs/). Per informazioni sui prezzi di Hub eventi, vedere [Prezzi di Hub eventi](https://azure.microsoft.com/pricing/details/event-hubs/).

I limiti seguenti sono comuni nei livelli Basic, standard e dedicati. 

| Limite | Ambito | Note | Valore |
| --- | --- | --- | --- |
| Numero di spazi dei nomi di Hub eventi per sottoscrizione |Sottoscrizione |- |100 |
| Numero di hub eventi per ogni spazio dei nomi |Spazio dei nomi |Le successive richieste di creazione di un nuovo hub eventi vengono rifiutate. |10 |
| Numero di partizioni per hub eventi |Persona giuridica |- |32 |
| Dimensione massima del nome di un hub eventi |Persona giuridica |- |50 caratteri |
| Numero di ricevitori non epoch per gruppo consumer |Persona giuridica |- |5 |
| Numero massimo di unità elaborate |Spazio dei nomi |Il superamento del limite di unità elaborate causa la limitazione della velocità dei dati e la generazione di un' [eccezione server occupato](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception). Per richiedere un numero maggiore di unità di velocità effettiva per un livello standard, archiviare una [richiesta di supporto](/azure/azure-portal/supportability/how-to-create-azure-support-request). Le [unità elaborate aggiuntive](../articles/event-hubs/event-hubs-auto-inflate.md) sono disponibili in blocchi da 20 in base a un acquisto con impegno. |20 |
| Numero di regole di autorizzazione per spazio dei nomi |Spazio dei nomi|Le successive richieste di creazione della regola di autorizzazione vengono rifiutate.|12 |
| Numero di chiamate al Metodo GetRuntimeInformation | Persona giuridica | - | 50 al secondo | 
| Numero di regole di rete virtuale (VNet) e di configurazione IP | Persona giuridica | - | 128 | 

### <a name="event-hubs-basic-and-standard---quotas-and-limits"></a>Hub eventi Basic e standard-quote e limiti
| Limite | Ambito | Note | Basic | Standard |
| --- | --- | --- | -- | --- |
| Dimensione massima degli eventi di Hub eventi|Persona giuridica | &nbsp; | 256 KB | 1 MB |
| Numero di gruppi consumer per hub eventi |Persona giuridica | &nbsp; |1 |20 |
| Numero di connessioni AMQP per spazio dei nomi |Spazio dei nomi |Le successive richieste di connessioni aggiuntive verranno rifiutate e il codice chiamante riceverà un'eccezione. |100 |5\.000|
| Periodo di conservazione massimo dei dati dell'evento |Persona giuridica | &nbsp; |1 giorno |1-7 giorni |
|Spazio dei nomi abilitato Apache Kafka|Spazio dei nomi |Flussi di spazio dei nomi di hub eventi con protocollo Kafka |No | Sì |
|Acquisizione |Persona giuridica | Se abilitata, micro-batch nello stesso flusso |No |Sì |


### <a name="event-hubs-dedicated---quotas-and-limits"></a>Hub eventi Dedicato-quote e limiti
L'offerta Hub eventi Dedicato viene fatturata a un prezzo mensile fisso, con un minimo di 4 ore di utilizzo. Il livello dedicato offre tutte le funzionalità del piano standard, ma con capacità e limiti di scalabilità aziendale per i clienti con carichi di lavoro complessi. 

| Funzionalità | Limiti |
| --- | ---|
| Larghezza di banda |  20 CUs |
| Spazi dei nomi | 50 per CU |
| Hub eventi |  1000 per spazio dei nomi |
| Eventi in ingresso | Incluso |
| Dimensione messaggi | 1 MB |
| Partizioni | 2000 per CU |
| Gruppi consumer | Nessun limite per CU, 1000 per hub eventi |
| Connessioni negoziate | 100 K incluse |
| Conservazione di messaggi | 90 giorni, 10 TB inclusi per CU |
| Acquisizione | Incluso |
