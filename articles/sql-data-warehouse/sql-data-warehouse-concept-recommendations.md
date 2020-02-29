---
title: Suggerimenti per analisi SQL
description: Informazioni sulle raccomandazioni di SQL Analytics e su come vengono generate
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 02/05/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: 5471236c09737eeef2d4cb7542c245d3087e726c
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/29/2020
ms.locfileid: "78195958"
---
# <a name="sql-analytics-recommendations"></a>Suggerimenti per analisi SQL

Questo articolo descrive i consigli di analisi SQL serviti tramite Azure Advisor.  

Analisi SQL fornisce consigli per garantire che il carico di lavoro data warehouse sia costantemente ottimizzato per le prestazioni. Le raccomandazioni sono strettamente integrate con [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations) per fornire le procedure consigliate direttamente nel [portale di Azure](https://aka.ms/Azureadvisor). SQL Analytics raccoglie i dati di telemetria e i consigli relativi alle superfici per il carico di lavoro attivo su cadenza giornaliera. Gli scenari di raccomandazione supportati sono descritti di seguito insieme a come applicare le azioni consigliate.

Puoi [controllare subito le tue raccomandazioni](https://aka.ms/Azureadvisor) . Questa funzionalità è attualmente applicabile solo ai data warehouse di seconda generazione. 

## <a name="data-skew"></a>Asimmetria dati

Durante l'esecuzione del carico di lavoro, l'asimmetria dei dati può causare uno spostamento dati aggiuntivo o colli di bottiglia nelle risorse. La documentazione seguente descrive come identificare le asimmetrie dei dati e come evitarle selezionando una chiave di distribuzione ottimale.

- [Identificare e rimuovere un'asimmetria](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice) 

## <a name="no-or-outdated-statistics"></a>Statistiche No o obsolete

La presenza di statistiche non ottimali può influire gravemente sulle prestazioni delle query in quanto può causare la generazione di piani di query non ottimali da parte di SQL Query Optimizer. La documentazione seguente descrive le procedure consigliate per la creazione e l'aggiornamento delle statistiche:

- [Creazione e aggiornamento delle statistiche delle tabelle](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics)

Per visualizzare l'elenco delle tabelle interessate da questi consigli, eseguire il comando [script T-SQL](https://github.com/Microsoft/sql-data-warehouse-samples/blob/master/samples/sqlops/MonitoringScripts/ImpactedTables) seguente. Advisor esegue in modo continuo lo stesso script T-SQL per generare tali consigli.

## <a name="replicate-tables"></a>Replicare tabelle

Per consigli sulla tabella replicata, Advisor rileva candidati di tabella basati sulle caratteristiche fisiche seguenti:

- Dimensione tabella replicata
- Numero di colonne
- Tipo di distribuzione della tabella
- Numero di partizioni

Advisor sfrutta in modo continuativo l'euristica basata sul carico di lavoro, ad esempio frequenza di accesso alla tabella, righe restituite in media e soglie sulle dimensioni e l'attività dei data warehouse, per verificare che vengano generati consigli di alta qualità. 

Di seguito vengono descritti l'euristica basata sul carico di lavoro che è possibile trovare nel portale di Azure per ogni consiglio relativo alla tabella replicata:

- Media analisi: percentuale media di righe restituite dalla tabella per ogni accesso alla tabella negli ultimi sette giorni.
- Lettura frequente, nessun aggiornamento: indica che la tabella non è stata aggiornata negli ultimi sette giorni durante la visualizzazione dell'attività di accesso.
- Percentuale di lettura/aggiornamento: la percentuale della frequenza di accesso alla tabella rispetto all'aggiornamento negli ultimi sette giorni.
- Attività: misura l'utilizzo in base all'attività di accesso. Viene confrontata l'attività di accesso alla tabella rispetto all'attività media di accesso alla tabella nel data warehouse negli ultimi sette giorni. 

Attualmente Advisor visualizza al massimo quattro candidati di tabella replicata alla volta con indici columnstore cluster con priorità per l'attività più elevata.

> [!IMPORTANT]
> I consigli relativi alla tabella replicata non costituiscono una prova completa e non prendono in considerazione operazioni di spostamento dei dati. Microsoft sta lavorando per l'aggiunta di questa opzione come elemento euristico, ma nel frattempo è sempre opportuno convalidare il carico di lavoro dopo aver applicato il consiglio. Contattare sqldwadvisor@service.microsoft.com se si individuano consigli relativi alla tabella replicata che causano una regressione del carico di lavoro. Per altre informazioni sulle tabelle replicate, consultare la [documentazione](https://docs.microsoft.com/azure/sql-data-warehouse/design-guidance-for-replicated-tables#what-is-a-replicated-table) seguente.
