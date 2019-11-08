---
title: Ripristino accelerato del database
description: Il database SQL di Azure include una nuova funzionalità che consente un ripristino rapido e coerente del database, un rollback di transazione istantaneo e un troncamento aggressivo del log per i database singoli e i database in pool nel database SQL di Azure, e i database in Azure SQL Data Warehouse.
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: mashamsft
ms.author: mathoma
ms.reviewer: carlrab
ms.date: 01/25/2019
ms.openlocfilehash: eff81693ff4c34dc00f66e9e5ea22e56d3ff9d77
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/08/2019
ms.locfileid: "73808100"
---
# <a name="accelerated-database-recovery"></a>Recupero accelerato del database

Il **ripristino accelerato del database (Accelerated Database Recovery, ADR)** è una nuova funzionalità del motore di database SQL che migliora notevolmente la disponibilità dei database, specialmente in presenza di transazioni a esecuzione prolungata, grazie alla riprogettazione del processo di ripristino del motore di database SQL. ADR è attualmente disponibile per i database singoli e i database in pool nel database SQL di Azure e i database in Azure SQL Data Warehouse (attualmente in anteprima pubblica). I vantaggi principali del ripristino accelerato del database (ADR) sono:

- **Ripristino rapido e coerente del database**

  Con il ripristino accelerato del database (ADR), le transazioni a esecuzione prolungata non influiscono sul tempo di ripristino complessivo consentendo un ripristino del database veloce e coerente, indipendentemente dal numero di transazioni attive nel sistema o dalle loro dimensioni.

- **Rollback di transazione istantaneo**

  Con il ripristino accelerato del database (ADR), il rollback di transazione è istantaneo, indipendentemente dal tempo in cui la transazione è stata attiva o dal numero di aggiornamenti che ha eseguito.

- **Troncamento aggressivo del log**

  Con il ripristino accelerato del database (ADR), il log delle transazioni viene troncato in modo aggressivo, anche in presenza di transazioni attive a esecuzione prolungata, impedendone un aumento delle dimensioni fuori controllo.

## <a name="the-current-database-recovery-process"></a>Processo di ripristino del database corrente

Il ripristino del database in SQL Server segue il modello di ripristino [ARIES](https://people.eecs.berkeley.edu/~brewer/cs262/Aries.pdf) e prevede tre fasi, visualizzate e descritte nel dettaglio nella figura seguente.

![processo di ripristino corrente](./media/sql-database-accelerated-database-recovery/current-recovery-process.png)

- **Fase di analisi**

  Eseguire l'analisi del log delle transazioni a partire dall'inizio dell'ultimo checkpoint riuscito (o dall'LSN di pagina dirty meno recente) fino al termine, per determinare lo stato di ogni transazione al momento dell'arresto SQL Server.

- **Fase di rollforward**

  Eseguire l'analisi del log delle transazioni dalla transazione di cui non è stato eseguito il commit meno recente fino alla fine, per portare il database allo stato in cui si trovava al momento dell'arresto anomalo del sistema ripetendo tutte le operazioni di commit.

- **Fase di rollback**

  Per ogni transazione attiva al momento dell'arresto anomalo, esegue il rollback del log, annullando le operazioni eseguite dalla transazione.

In base a questa progettazione, il tempo necessario per il ripristino del motore di database SQL da un riavvio imprevisto è pressoché proporzionale alla dimensione della transazioni attiva più lunga presente nel sistema al momento dell'arresto anomalo. Il ripristino richiede un rollback di tutte le transazioni incomplete. La quantità di tempo necessaria è proporzionale al lavoro eseguito dalla transazione e al tempo in cui è stata attiva. Per questa ragione, il processo di ripristino di SQL Server può richiedere molto tempo in presenza di transazioni a esecuzione prolunga, ad esempio operazioni di inserimento in blocco di grandi dimensioni o operazioni di generazione degli indici su tabella di grandi dimensioni.

In base a questa progettazione, anche l'annullamento o il rollback di una transazione di grandi dimensioni può richiedere molto tempo poiché viene usata la stessa fase di rollback descritta sopra.

Inoltre, il motore di database SQL non può troncare il log delle transazioni quando sono presenti transazioni a esecuzione prolungata poiché i record del log corrispondenti sono necessari per i processi di ripristino e rollback. In seguito a questa progettazione del motore di database SQL, alcuni clienti affrontano il problema che le dimensioni del log delle transazioni crescono molto e consumano grandi quantità di spazio su disco.

## <a name="the-accelerated-database-recovery-process"></a>Processo di ripristino accelerato del database

Il ripristino accelerato del database (ADR) risolve i problemi descritti sopra riprogettando completamente il processo di ripristino del motore di database SQL per:

- Renderlo costante in termini di tempo o istantaneo senza la necessità di dover analizzare il log da o fino alla transazione attiva meno recente. Con ADR, il log delle transazioni viene elaborato solo dall'ultimo checkpoint completato o dal numero di sequenza del file di log (LSN) della pagina dirty più recente. Di conseguenza, il tempo di ripristino non viene influenzato dalle transazioni a esecuzione prolungata.
- Ridurre al minimo lo spazio del log delle transazioni necessario poiché non è più necessario elaborare il log per l'intera transazione. Di conseguenza, il log delle transazioni può essere troncato in modo aggressivo quando vengono eseguiti checkpoint e backup.

A un livello elevato, ADR consente il ripristino rapido del database tramite il controllo delle versioni di tutte le modifiche fisiche del database e l'annullamento di operazioni logiche, che sono limitate e che possono essere annullate quasi immediatamente. Tutte le transazioni attive al momento dell'arresto anomalo vengono contrassegnate come interrotte e, pertanto, tutte le versioni generate da queste transazioni possono essere ignorate dalle query utente simultanee.

Il processo di ripristino accelerato del database (ADR) prevede le stesse tre fasi del processo di ripristino corrente. Il funzionamento delle tre fasi del ripristino accelerato del database (ADR) è illustrato nella figura seguente e descritto nel dettaglio di seguito.

![Processo di ripristino accelerato del database (ADR)](./media/sql-database-accelerated-database-recovery/adr-recovery-process.png)

- **Fase di analisi**

  Il processo resta identico a quello odierno, con l'aggiunta della ricostruzione e della copia dei record di log per le operazioni senza controllo delle versioni.
  
- **Fase di rollforward**

  Suddivisa in due fasi (P)
  - Fase 1

      Rollforward da sLog (dalla transazione meno recente di cui non è stato eseguito il commit all'ultimo checkpoint). Il rollforward è un'operazione veloce poiché prevede solo l'elaborazione di alcuni record da sLog.
      
  - Fase 2

     Il rollforward dal log delle transazioni inizia dall'ultimo checkpoint (anziché dalla transazione meno recente di cui non è stato eseguito il commit)
     
- **Fase di rollback**

   Con il ripristino accelerato del database (ADR) la fase di rollback viene eseguita quasi istantaneamente usando sLog per annullare le operazioni senza controllo delle versioni e l'archivio versioni permanente (Persisted Version Store, PVS) con ripristino logico per eseguire il rollback basato sulla versione a livello di riga.

## <a name="adr-recovery-components"></a>Componenti del ripristino accelerato del database (ADR)

I quattro componenti del ripristino accelerato del database (ADR) sono:

- **Archivio versioni permanente (Persisted Version Store, PVS)**

  L'archivio versioni permanente è un nuovo meccanismo del motore di database SQL per il mantenimento delle versioni di riga generate nel database anziché nell'archivio versioni `tempdb` tradizionale. L'archivio versioni permanente (PVS) abilita l'isolamento delle risorse e migliora la disponibilità delle repliche secondarie leggibili.

- **Ripristino logico**

  Il ripristino logico è il processo asincrono responsabile dell'esecuzione dell'annullamento basato sulla versione a livello di riga che fornisce il rollback della transazione immediata e Annulla per tutte le operazioni con versione. Il ripristino logico viene eseguito da:

  - Tenere traccia di tutte le transazioni interrotte e contrassegnarle come invisibile ad altre transazioni. 
  - Esecuzione del rollback utilizzando PVS per tutte le transazioni utente, anziché analizzare fisicamente il log delle transazioni e annotare le modifiche una alla volta.
  - Rilascio di tutti i blocchi immediatamente dopo l'interruzione della transazione. Poiché Abort implica semplicemente il contrassegno delle modifiche nella memoria, il processo è molto efficiente e pertanto non è necessario che i blocchi vengano mantenuti per un lungo periodo di tempo.

- **sLog**

  sLog è un flusso di registrazione in memoria secondario che archivia i record del log per le operazioni senza controllo delle versioni, ad esempio l'invalidamento metadati nella cache, le acquisizioni dei blocchi e così via. SLog ha le caratteristiche seguenti:

  - Volume ridotto e in memoria
  - Permanente su disco tramite serializzazione durante il processo di checkpoint
  - Troncamento periodico al momento del commit delle transazioni
  - Accelera le fasi di rollforward e rollback elaborando solo le operazioni senza controllo delle versioni  
  - Abilita un troncamento del log delle transazioni aggressivo mantenendo solo i record del log necessari

- **Pulitura**

  La pulitura è un processo asincrono che viene attivato periodicamente e cancella le versioni di pagina non necessarie.

## <a name="who-should-consider-accelerated-database-recovery"></a>Consigli sull'uso del ripristino accelerato del database

L'abilitazione del ripristino accelerato del database (ADR) è consigliata ai tipi di clienti seguenti:

- Clienti con carichi di lavoro con transazioni a esecuzione prolungata.
- Clienti con casi in cui le transazioni attive causano un aumento significativo delle dimensioni del log delle transazioni.  
- Clienti che hanno sperimentato lunghi periodi di non disponibilità del database a causa di ripristini di SQL Server che richiedono molto tempo, ad esempio un riavvio imprevisto di SQL Server o un rollback manuale delle transazioni.

