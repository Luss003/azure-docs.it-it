---
title: Monitorare e ottimizzare nel Database di Azure per PostgreSQL - Server singolo
description: Questo articolo descrive le funzionalità di monitoraggio e ottimizzazione nel Database di Azure per PostgreSQL - singolo Server.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/19/2019
ms.openlocfilehash: c69ffb30a37de8e6dc3e15aa1f7dcd6a9311d614
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67274294"
---
# <a name="monitor-and-tune-azure-database-for-postgresql---single-server"></a>Monitorare e ottimizzare Database di Azure per PostgreSQL - Server singolo
Il monitoraggio dei dati relativi ai server facilita la risoluzione dei problemi e l'ottimizzazione in relazione al carico di lavoro. Database di Azure per PostgreSQL offre varie opzioni di monitoraggio che consentono di ottenere informazioni dettagliate sul comportamento del server.

## <a name="metrics"></a>metrics
Database di Azure per PostgreSQL offre varie metriche che consentono di ottenere informazioni approfondite sul comportamento delle risorse che supportano il server PostgreSQL. Ogni metrica viene emessa con una frequenza di un minuto e ha una cronologia che può arrivare fino a 30 giorni. È possibile configurare avvisi in base alle metriche. Per indicazioni dettagliate, vedere l'articolo su come [configurare gli avvisi](howto-alert-on-metric.md). Le altre attività includono la configurazione di azioni automatiche, l'esecuzione di analisi avanzate e l'archiviazione della cronologia. Per altre informazioni, vedere [Panoramica delle metriche in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Elenco delle metriche
Per Database di Azure per PostgreSQL sono disponibili le metriche seguenti:

|Metrica|Nome visualizzato per la metrica|Unità|Descrizione|
|---|---|---|---|
|cpu_percent|Percentuale CPU|Percent|Percentuale di CPU in uso.|
|memory_percent|Percentuale memoria|Percent|Percentuale di memoria in uso.|
|io_consumption_percent|IO percent (Percentuale IO)|Percent|Percentuale di I/O in uso.|
|storage_percent|Percentuale archiviazione|Percent|Percentuale di spazio di archiviazione usata rispetto al massimo del server.|
|storage_used|Uso archiviazione|Byte|Quantità di spazio di archiviazione in uso. Lo spazio di archiviazione usato dal servizio può includere file di database, log delle transazioni e log del server.|
|storage_limit|Limite archiviazione|Byte|Spazio di archiviazione massimo per il server.|
|serverlog_storage_percent|Percentuale di archiviazione dei log del server|Percent|Percentuale di spazio di archiviazione dei log del server usata rispetto allo spazio di archiviazione massimo dei log del server per il server.|
|serverlog_storage_usage|Archiviazione dei log del server usata|Byte|Quantità di spazio di archiviazione dei log del server in uso.|
|serverlog_storage_limit|Limite di archiviazione dei log del server|Byte|Spazio di archiviazione massimo dei log del server per il server.|
|active_connections|Connessioni attive|Conteggio|Numero di connessioni al server attive.|
|connections_failed|Connessioni non riuscite|Conteggio|Numero di connessioni al server non riuscite.|
|network_bytes_egress|Rete in uscita|Byte|Rete in uscita tra connessioni attive.|
|network_bytes_ingress|Rete in ingresso|Byte|Rete in ingresso tra connessioni attive.|
|backup_storage_used|Risorse di backup in uso|Byte|Quantità di risorse dell'archivio di backup usate.|
|pg_replica_log_delay_in_bytes|Max Lag Across Replicas (Ritardo massimo tra repliche)|Byte|L'intervallo di byte tra il master e la replica di massimo ritardo. Questa metrica è disponibile solo nel server master.|
|pg_replica_log_delay_in_seconds|Replica Lag (Ritardo replica)|Secondi|Il periodo trascorso dall'ultima riproduzione delle transazioni. Questa metrica è disponibile per i server di replica solo.|

## <a name="server-logs"></a>Log del server
È possibile abilitare l'accesso al server. Questi log sono disponibili anche tramite i log di diagnostica di Azure in [monitoraggio di Azure registra](../azure-monitor/log-query/log-query-overview.md), hub eventi e Account di archiviazione. Per altre informazioni sull'accesso, visitare la pagina dei [log del server](concepts-server-logs.md).

## <a name="query-store"></a>Archivio query
[Query Store](concepts-query-store.md) tiene traccia delle prestazioni nel corso del tempo tra cui le statistiche di runtime di query e attendere gli eventi di query. La funzionalità salva in modo permanente le informazioni sulle prestazioni dei runtime di query in un database di sistema denominato **azure_sys** nello schema query_store. È possibile controllare la raccolta e l'archiviazione dei dati tramite vari controlli di configurazione.

## <a name="query-performance-insight"></a>Informazioni dettagliate prestazioni query
[Informazioni dettagliate prestazioni query](concepts-query-performance-insight.md) funziona in combinazione con Query Store per fornire visualizzazioni accessibili dal portale di Azure. Questi grafici consentono di identificare le principali query che influiscono sulle prestazioni. Query Performance Insightis accessibile dal **supporto + risoluzione dei problemi** sezione del Database di Azure per la pagina del portale del server PostgreSQL.

## <a name="performance-recommendations"></a>Raccomandazioni per le prestazioni
La funzionalità [Raccomandazioni per le prestazioni](concepts-performance-recommendations.md) identifica le opportunità per migliorare le prestazioni dei carichi di lavoro. Raccomandazioni sulle prestazioni vengono fornite indicazioni per la creazione di nuovi indici che potrebbero migliorare le prestazioni dei carichi di lavoro. Per generare le raccomandazioni sugli indici, la funzionalità prende in considerazione diverse caratteristiche del database, tra cui lo schema e il carico di lavoro segnalati da Query Store. Dopo avere implementato le raccomandazioni per le prestazioni, i clienti devono testare le prestazioni per valutare l'impatto di tali modifiche. 

## <a name="next-steps"></a>Passaggi successivi
- Per informazioni sulla creazione di un avviso per una metrica, vedere [Come configurare gli avvisi](howto-alert-on-metric.md).
- Per altre informazioni sull'accesso alle metriche e la relativa esportazione con il portale di Azure, l'API REST o l'interfaccia della riga di comando, vedere [Panoramica delle metriche in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
- Leggere il blog Microsoft sulle [procedure consigliate per il monitoraggio del server](https://azure.microsoft.com/blog/best-practices-for-alerting-on-metrics-with-azure-database-for-postgresql-monitoring/).
