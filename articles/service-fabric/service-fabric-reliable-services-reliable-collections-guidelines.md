---
title: Linee guida e consigli per Reliable Collections in Azure Service Fabric | Microsoft Docs
description: Linee guida e consigli per l'uso di Reliable Collections in Service Fabric
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: masnider,rajak,zhol
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 12/10/2017
ms.author: atsenthi
ms.openlocfilehash: dc7d60cb846aa16f2facd41f5b6b7ce52bcc8f41
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/29/2019
ms.locfileid: "68599346"
---
# <a name="guidelines-and-recommendations-for-reliable-collections-in-azure-service-fabric"></a>Linee guida e consigli per Reliable Collections in Azure Service Fabric
Questa sezione fornisce le linee guida per l'uso di Reliable State Manager e Reliable Collections. L'obiettivo è quello di aiutare gli utenti a evitare errori comuni.

Le linee guida sono organizzate come semplici consigli*su cosa fare* , *prendere in considerazione* , *evitare* e*non fare*.

* Non modificare un oggetto di tipo personalizzato restituito dalle operazioni di lettura, ad esempio `TryPeekAsync` o `TryGetValueAsync`. Le raccolte Reliable Collections, così come le raccolte Concurrent Collections, restituiscono un riferimento agli oggetti, non una copia.
* Eseguire una copia completa dell'oggetto di tipo personalizzato restituito prima di modificarlo. Poiché gli struct e i tipi predefiniti vengono passati per valore, non è necessario eseguirne una copia completa, a meno che non contengano campi di tipo Reference o proprietà che si intende modificare.
* Non usare `TimeSpan.MaxValue` per i timeout. I timeout devono essere usati per rilevare i deadlock.
* Non usare una transazione dopo che ne è stato eseguito il commit, è stata interrotta o eliminata.
* Non usare un'enumerazione all'esterno dell'ambito di transazione nella quale è stata creata.
* Non creare una transazione all'interno dell'istruzione `using` di un'altra transazione. Questa operazione può causare deadlock.
* Non creare lo stato affidabile con `IReliableStateManager.GetOrAddAsync` e utilizzare lo stato affidabile nella stessa transazione. Viene restituito un valore InvalidOperationException.
* Verificare che l'implementazione di `IComparable<TKey>` sia corretta. Il sistema presenta dipendenze su `IComparable<TKey>` per l'unione di checkpoint e righe.
* Usare il blocco di aggiornamento durante la lettura di un elemento con l'intenzione di aggiornarlo in modo da evitare una determinata classe di deadlock.
* Si consiglia di mantenere un numero di raccolte Reliable Collections per partizione inferiore a 1000. Preferire le raccolte Reliable Collections con più elementi.
* Prendere in considerazione l'opportunità di mantenere gli elementi (ad esempio TKey + TValue per Reliable Dictionary) sotto gli 80 Kbyte: più sono piccoli, meglio è. In questo modo, è possibile ridurre l'uso di heap di oggetti di grandi dimensioni, oltre che i requisiti di dischi e I/O di rete. Spesso si riduce anche la replica dei dati duplicati quando viene aggiornata solo una piccola parte del valore. Un modo comune per ottenere questo in Reliable Dictionary è interrompere le righe in più righe.
* Prendere in considerazione l'uso della funzionalità di backup e ripristino per il ripristino di emergenza.
* Evitare di combinare nella stessa transazione le operazioni a singola entità e a più entità, ad esempio `GetCountAsync`, `CreateEnumerableAsync`, a causa dei diversi livelli di isolamento.
* Gestire InvalidOperationException. Le transazioni utente possono essere interrotte dal sistema per diversi motivi. Ad esempio, quando cambia il Gestore dello stato affidabile cambia il proprio ruolo da ruolo primario o quando una transazione a esecuzione prolungata blocca il troncamento del log delle transazioni. In questi casi, l'utente può ricevere un evento InvalidOperationException, che indica che la transazione è già stata terminata. Supponendo che l'interruzione della transazione non è stata richiesta dall'utente, il modo migliore per gestire questa eccezione è di eliminare la transazione, verificare se il token di annullamento è stato segnalato (o se il ruolo della replica è stato modificato) e in caso contrario creare una nuova transazione e riprovare.  

Occorre tenere presente i concetti seguenti:

* Il timeout predefinito di tutte le API Reliable Collections è di quattro secondi. La maggior parte degli utenti deve usare il timeout predefinito.
* Il token di annullamento predefinito è `CancellationToken.None` in tutte le API di Reliable Collections.
* Il parametro di tipo di chiave (*TKey*) per un oggetto ReliableDictionary deve implementare correttamente `GetHashCode()` e `Equals()`. Le chiavi non devono essere modificabili.
* Per ottenere una disponibilità elevata per le raccolte Reliable Collections, ogni servizio deve avere almeno un set di repliche di destinazione costituito da un minimo di 3 repliche.
* Le operazioni di lettura sul secondario possono leggere le versioni che non sono vincolate a un quorum.
  Ciò significa che una versione dei dati che viene letta da un singolo secondario potrebbe essere elaborata in modo non corretto.
  Le letture della replica primaria sono sempre stabili: non sono mai elaborate in modo non corretto.
* La sicurezza e la privacy dei dati resi persistenti tramite l'applicazione in una raccolta affidabile sono decisioni dell'utente e sono soggette a misure di protezione fornite dalla gestione dell'archiviazione, ad esempio la crittografia del disco del sistema operativo potrebbe essere usata per proteggere i dati inattivi.  

### <a name="next-steps"></a>Passaggi successivi
* [Lavorare con le raccolte Reliable Collections](service-fabric-work-with-reliable-collections.md)
* [Transazioni e blocchi](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* Gestione dei dati
  * [Backup e ripristino](service-fabric-reliable-services-backup-restore.md)
  * [Notifications](service-fabric-reliable-services-notifications.md)
  * [Serializzazione e aggiornamento](service-fabric-application-upgrade-data-serialization.md)
  * [Reliable State Manager configuration (Configurazione di Reliable State Manager)](service-fabric-reliable-services-configuration.md)
* Altro
  * [Guida introduttiva a Reliable Services di Microsoft Azure Service Fabric](service-fabric-reliable-services-quick-start.md)
  * [Guida di riferimento per gli sviluppatori per Reliable Collections](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
