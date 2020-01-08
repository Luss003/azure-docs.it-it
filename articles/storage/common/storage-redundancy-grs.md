---
title: Archiviazione con ridondanza geografica (GRS) per la durabilità tra più aree
titleSuffix: Azure Storage
description: Con l'archiviazione con ridondanza geografica i dati vengono replicati tra due aree a centinaia di chilometri di distanza. L'archiviazione con ridondanza geografica protegge dagli errori hardware nel data center e dalle situazioni di emergenza a livello di area.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 01/02/2020
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: 6bb93c3fb6599a05978e11ef5fbc179ccfaa9ec2
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/02/2020
ms.locfileid: "75614897"
---
# <a name="geo-redundant-storage-grs-cross-regional-replication-for-azure-storage"></a>Archiviazione con ridondanza geografica: replica tra più aree per Archiviazione di Azure

[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-grs.md)]

## <a name="read-access-geo-redundant-storage"></a>Archiviazione con ridondanza geografica e accesso in lettura

L'archiviazione con ridondanza geografica e accesso in lettura (RA-GRS) ottimizza la disponibilità per l'account di archiviazione. L'archiviazione con ridondanza geografica e accesso in lettura fornisce l'accesso in sola lettura ai dati nella località secondaria, oltre alla replica geografica tra due aree.

Se si abilita l'accesso in sola lettura ai dati nell'area secondaria, i dati saranno disponibili in un endpoint secondario, oltre che nell'endpoint primario dell'account di archiviazione. L'endpoint secondario è simile a quello primario, ma al nome dell'account viene aggiunto il suffisso `–secondary` . Se ad esempio l'endpoint primario per il servizio BLOB è `myaccount.blob.core.windows.net`, l'endpoint secondario sarà `myaccount-secondary.blob.core.windows.net`. Le chiavi di accesso per l'account di archiviazione sono identiche per gli endpoint primario e secondario.

Alcune considerazioni da ricordare quando si usa l'archiviazione con ridondanza geografica e accesso in lettura:

- L'applicazione deve gestire l'endpoint con cui interagire quando usa l'archiviazione con ridondanza geografica e accesso in lettura.
- Poiché la replica asincrona implica un ritardo, è possibile che le modifiche non ancora replicate nell'area secondaria vadano perse se non è possibile recuperare i dati dall'area primaria.
- È possibile controllare la proprietà **ora ultima sincronizzazione** per l'account di archiviazione. **L'ora dell'ultima sincronizzazione** è un valore di data/ora GMT. Tutte le Scritture primarie effettuate prima dell' **ora dell'ultima sincronizzazione** sono state scritte correttamente nella posizione secondaria, vale a dire che sono disponibili per la lettura dalla posizione secondaria. Le Scritture primarie dopo l' **ora dell'ultima sincronizzazione** potrebbero non essere ancora disponibili per le letture. È possibile eseguire query su questo valore usando PowerShell, l'interfaccia della riga di comando di Azure o una delle librerie client di archiviazione di Azure. Per altre informazioni, vedere **ottenere l'ora dell'ultima sincronizzazione** nella [progettazione di applicazioni a disponibilità elevata tramite l'archiviazione con ridondanza geografica e accesso in lettura](storage-designing-ha-apps-with-ragrs.md#getting-the-last-sync-time).
- Se si avvia il failover di un account (anteprima) nell'area secondaria, per un account di archiviazione con ridondanza geografica oppure con ridondanza geografica e accesso in lettura, l'accesso in scrittura a tale account viene ripristinato al termine del failover. Per altre informazioni, vedere [Ripristino di emergenza e failover dell'account di archiviazione (anteprima)](storage-disaster-recovery-guidance.md).
- L'archiviazione con ridondanza geografica e accesso in lettura è pensata per rispondere a requisiti di disponibilità elevata. Per indicazioni sulla scalabilità, vedere l'[elenco di controllo delle prestazioni](storage-performance-checklist.md).
- Per suggerimenti su come progettare la disponibilità elevata con l'archiviazione con ridondanza geografica e accesso in lettura, vedere [Progettazione di applicazioni a disponibilità elevata con RA-GRS](storage-designing-ha-apps-with-ragrs.md).

## <a name="what-is-the-rpo-and-rto-with-grs"></a>Quali sono gli obiettivi RPO e RTO con l'archiviazione con ridondanza geografica?

**Obiettivo del punto di ripristino (RPO)** : nel caso dell'archiviazione con ridondanza geografica e dell'archiviazione con ridondanza geografica e accesso in lettura, il servizio di archiviazione esegue la replica geografica asincrona dalla località primaria a quella secondaria. Nel caso in cui l'area primaria diventi non disponibile, è possibile eseguire il failover di un account (anteprima) nell'area secondaria. Quando si avvia un failover, le modifiche recenti non ancora replicate geograficamente potrebbero andare perse. Il numero di minuti di potenziale perdita di dati è detto obiettivo del punto di ripristino e indica il punto nel tempo in cui è possibile recuperare i dati. L'obiettivo del punto di ripristino di Archiviazione di Azure è in genere inferiore a 15 minuti, anche se attualmente non è previsto alcun contratto di servizio sulla durata della replica geografica.

**Obiettivo del tempo di ripristino:** l'obiettivo del tempo di ripristino è una misura del tempo necessario per eseguire il failover e riportare online l'account di archiviazione. Il tempo necessario per eseguire il failover include le azioni seguenti:

- Il tempo che trascorre prima che il cliente avvii il failover dell'account di archiviazione dall'area primaria a quella secondaria.
- Il tempo necessario ad Azure per eseguire il failover modificando le voci relative al DNS primario in modo che puntino alla località secondaria.

## <a name="paired-regions"></a>Aree abbinate

L'area primaria viene selezionata durante la creazione di un account di archiviazione. L'area secondaria associata viene determinata in base all'area primaria e non è possibile modificarla. Per informazioni aggiornate sulle aree supportate da Azure, vedere [Continuità aziendale e ripristino di emergenza nelle aree geografiche abbinate di Azure](../../best-practices-availability-paired-regions.md).

## <a name="see-also"></a>Vedi anche

- [Replica di Archiviazione di Azure](storage-redundancy.md)
- [Archiviazione con ridondanza locale: ridondanza dei dati a basso costo per Archiviazione di Azure](storage-redundancy-lrs.md)
- [Archiviazione con ridondanza della zona (ZRS): applicazioni di Archiviazione di Azure a disponibilità elevata](storage-redundancy-zrs.md)
