---
title: Metriche della piattaforma di monitoraggio di Azure esportabili tramite le impostazioni di diagnostica
description: Elenco delle metriche disponibili per ogni tipo di risorsa con il monitoraggio di Azure.
services: azure-monitor
ms.topic: reference
ms.date: 02/10/2020
ms.subservice: metrics
ms.openlocfilehash: 7a75655d1707dd2491065974ed8addc4c2da1a6a
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/27/2020
ms.locfileid: "77661363"
---
# <a name="azure-monitor-platform-metrics-exportable-via-diagnostic-settings"></a>Metriche della piattaforma di monitoraggio di Azure esportabili tramite le impostazioni di diagnostica

Monitoraggio di Azure offre [metriche della piattaforma](data-platform-metrics.md) per impostazione predefinita senza configurazione. Offre diversi modi per interagire con le metriche della piattaforma, inclusa la creazione di grafici nel portale, l'accesso tramite l'API REST o l'esecuzione di query su di essi tramite PowerShell o l'interfaccia della riga di comando. Vedere [metriche: supportato](metrics-supported.md) per un elenco completo delle metriche della piattaforma attualmente disponibili con la pipeline metrica consolidata di monitoraggio di Azure. Per eseguire una query e accedere a queste metriche, usare la [versione api 2018-01-01](https://docs.microsoft.com/rest/api/monitor/metricdefinitions). Altre metriche potrebbero essere disponibili nel portale o tramite le API legacy.

È possibile esportare le metriche della piattaforma dalla pipeline di monitoraggio di Azure in altre posizioni in uno dei due modi seguenti.
1. Uso [delle impostazioni di diagnostica](diagnostic-settings.md) per inviare log Analytics, Hub eventi o archiviazione di Azure.
2. Usare l' [API REST delle metriche](https://docs.microsoft.com/rest/api/monitor/metrics/list)

A causa delle complessità del back-end di monitoraggio di Azure, non tutte le metriche sono esportabili usando le impostazioni di diagnostica. La tabella seguente elenca le opzioni che possono e non possono essere esportate usando le impostazioni di diagnostica.

## <a name="change-to-behavior-for-nulls-and-zero-values"></a>Modificare il comportamento per i valori NULL e zero 
 
Per le metriche della piattaforma che possono essere esportate tramite le impostazioni di diagnostica, sono disponibili alcune metriche per cui il monitoraggio di Azure interpreta ' 0' come ' null '. Ciò ha causato una certa confusione tra' 0' reale (emesso dalla risorsa) è 0' interpretato (null). A partire dal **1 ° aprile 2020** le metriche della piattaforma esportate tramite le impostazioni di diagnostica non esporteranno più' 0', a meno che non siano state effettivamente emesse dalla risorsa sottostante. Nota bene:

1.  Se si elimina un gruppo di risorse o una risorsa specifica, i dati delle metriche dalle risorse applicate non verranno più inviati alle destinazioni di esportazione delle impostazioni di diagnostica. Ovvero non verrà più visualizzato negli hub eventi, negli account di archiviazione e nelle aree di lavoro Log Analytics.
2.  Questo miglioramento sarà disponibile in tutti i cloud pubblici e privati.
3.  Questa modifica non avrà alcun effetto sul comportamento di una delle seguenti esperienze: 
   - Log delle risorse della piattaforma esportati tramite impostazioni di diagnostica
   - Creazione di grafici delle metriche in Esplora metriche
   - Avvisi sulle metriche della piattaforma
 
## <a name="metrics-exportable-table"></a>Tabella delle metriche esportabili 

La tabella contiene le seguenti colonne. 
- Esportabili tramite le impostazioni di diagnostica? 
- Applicato da NULL/0 
- ResourceType 
- Metrica 
- MetricDisplayName
- Unità 
- AggregationType


> [!NOTE]
> La tabella seguente può avere una barra di scorrimento orizzontale in basso. Se si ritiene di non avere informazioni, verificare che la barra di scorrimento sia a sinistra.  


| Esportabili tramite le impostazioni di diagnostica?  | Genera valori NULL |  ResourceType  |  Metrica  |  MetricDisplayName  |  Unità  |  AggregationType | 
|---|---| ---- | ----- | ------ | ---- | ---- | 
| Sì * * * *  | No |  Microsoft.AnalysisServices/servers  |  CleanerCurrentPrice  |  Memoria: prezzo corrente pulitura memoria  |  Conteggio  |  Media | 
| Sì * * * *  | No |  Microsoft.AnalysisServices/servers  |  CleanerMemoryNonshrinkable  |  Memoria: pulitura memoria non compattabile  |  Byte  |  Media | 
| Sì * * * *  | No |  Microsoft.AnalysisServices/servers  |  CleanerMemoryShrinkable  |  Memoria: pulitura memoria compattabile  |  Byte  |  Media | 
| Sì * * * *  | No |  Microsoft.AnalysisServices/servers  |  CommandPoolBusyThreads  |  Thread: thread occupati nel pool di comandi  |  Conteggio  |  Media | 
| Sì * * * *  | No |  Microsoft.AnalysisServices/servers  |  CommandPoolIdleThreads  |  Thread: Thread inattivi pool di comandi  |  Conteggio  |  Media | 
| Sì * * * *  | No |  Microsoft.AnalysisServices/servers  |  CommandPoolJobQueueLength  |  Lunghezza coda processi nel pool di comandi  |  Conteggio  |  Media | 
| Sì * * * *  | No |  Microsoft.AnalysisServices/servers  |  CurrentConnections  |  Connessione: connessioni correnti  |  Conteggio  |  Media | 
| Sì * * * *  | No |  Microsoft.AnalysisServices/servers  |  CurrentUserSessions  |  Sessioni utente correnti  |  Conteggio  |  Media | 
| Sì * * * *  | No |  Microsoft.AnalysisServices/servers  |  LongParsingBusyThreads  |  Thread: thread occupati per analisi dei thread lunghi  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  LongParsingIdleThreads  |  Thread: Thread inattivi per analisi dei thread lunghi  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  LongParsingJobQueueLength  |  Thread: lunghezza coda processi di analisi dei thread lunghi  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  mashup_engine_memory_metric  |  Memoria Motore M  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  mashup_engine_private_bytes_metric  |  Byte privati motore M  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  mashup_engine_qpu_metric  |  QPU Motore M  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  mashup_engine_virtual_bytes_metric  |  Byte virtuali del motore M  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  memory_metric  |  Memoria  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  memory_thrashing_metric  |  Thrashing di memoria  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  MemoryLimitHard  |  Memoria: limite memoria assoluto  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  MemoryLimitHigh  |  Memoria: limite memoria massimo  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  MemoryLimitLow  |  Memoria: limite memoria minimo  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  MemoryLimitVertiPaq  |  Memoria: limite memoria VertiPaq  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  MemoryUsage  |  Memoria: utilizzo memoria  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  private_bytes_metric  |  Byte privati  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  ProcessingPoolBusyIOJobThreads  |  Thread: thread di processi di I/O occupati nel pool di elaborazione  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  ProcessingPoolBusyNonIOThreads  |  Thread: Thread non di I/O occupati nel pool di elaborazione  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  ProcessingPoolIdleIOJobThreads  |  Thread: thread di processi di I/O inattivi nel pool di elaborazione  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  ProcessingPoolIdleNonIOThreads  |  Thread: thread non di I/O inattivi nel pool di elaborazione  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  ProcessingPoolIOJobQueueLength  |  Thread: lunghezza coda processi di I/O nel pool di elaborazione  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  ProcessingPoolJobQueueLength  |  Lunghezza coda processi nel pool di elaborazione  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  qpu_metric  |  QPU  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  QueryPoolBusyThreads  |  Thread occupati pool di query  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  QueryPoolIdleThreads  |  Thread: thread inattivi nel pool di query  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  QueryPoolJobQueueLength  |  Thread: lunghezza della coda processi nel pool di query  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  Quota  |  Memoria: quota  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  QuotaBlocked  |  Memoria: Richieste di quota bloccate  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  RowsConvertedPerSec  |  Elaborazione: righe convertite al secondo  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  RowsReadPerSec  |  Elaborazione: righe lette al secondo  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  RowsWrittenPerSec  |  Elaborazione: righe scritte al secondo  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  ShortParsingBusyThreads  |  Thread: thread occupati per analisi dei thread brevi  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  ShortParsingIdleThreads  |  Thread: thread inattivi per analisi dei thread brevi  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  ShortParsingJobQueueLength  |  Thread: lunghezza coda processi di analisi dei thread brevi  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  SuccessfullConnectionsPerSec  |  Connessioni riuscite al secondo  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  TotalConnectionFailures  |  Numero totale di errori di connessione  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  TotalConnectionRequests  |  Numero totale di richieste di connessione  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  VertiPaqNonpaged  |  Memoria: VertiPaq non di paging  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  VertiPaqPaged  |  Memoria: VertiPaq di paging  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.AnalysisServices/servers  |  virtual_bytes_metric  |  Byte virtuali  |  Byte  |  Media | 
| **Sì**  | **Sì** |  Microsoft.ApiManagement/service  |  BackendDuration  |  Durata delle richieste back-end  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.ApiManagement/service  |  Capacity  |  Capacity  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.ApiManagement/service  |  Durata  |  Durata complessiva delle richieste del gateway  |  Millisecondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.ApiManagement/service  |  EventHubDroppedEvents  |  Eventi EventHub eliminati  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.ApiManagement/service  |  EventHubRejectedEvents  |  Eventi EventHub rifiutati  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.ApiManagement/service  |  EventHubSuccessfulEvents  |  Eventi EventHub riusciti  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.ApiManagement/service  |  EventHubThrottledEvents  |  Eventi EventHub limitati  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.ApiManagement/service  |  EventHubTimedoutEvents  |  Timeout degli eventi di EventHub  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.ApiManagement/service  |  EventHubTotalBytesSent  |  Dimensioni degli eventi di EventHub  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.ApiManagement/service  |  EventHubTotalEvents  |  Totale eventi EventHub  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.ApiManagement/service  |  EventHubTotalFailedEvents  |  Eventi EventHub non riusciti  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.ApiManagement/service  |  FailedRequests  |  Richieste gateway non riuscite (deprecate)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.ApiManagement/service  |  OtherRequests  |  Altre richieste del gateway (deprecate)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.ApiManagement/service  |  Richieste  |  Richieste  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.ApiManagement/service  |  SuccessfulRequests  |  Richieste gateway riuscite (deprecate)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.ApiManagement/service  |  TotalRequests  |  Totale richieste Gateway (deprecato)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.ApiManagement/service  |  UnauthorizedRequests  |  Richieste del gateway non autorizzate (deprecate)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. AppPlatform/Spring  |  AppCpuUsagePercentage  |  Percentuale di utilizzo della CPU dell'app  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft. AppPlatform/Spring  |  AppMemoryCommitted  |  Memoria app assegnata  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft. AppPlatform/Spring  |  AppMemoryMax  |  Memoria app Max  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft. AppPlatform/Spring  |  AppMemoryUsed  |  Memoria app utilizzata  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft. AppPlatform/Spring  |  GCPauseTotalCount  |  Conteggio delle pause GC  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. AppPlatform/Spring  |  GCPauseTotalTime  |  Tempo totale pausa GC  |  Millisecondi  |  Totale | 
| **Sì**  | No |  Microsoft. AppPlatform/Spring  |  MaxOldGenMemoryPoolBytes  |  Dimensioni massime dei dati di generazione obsoleti disponibili  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft. AppPlatform/Spring  |  OldGenMemoryPoolBytes  |  Dimensioni dati di generazione precedenti  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft. AppPlatform/Spring  |  OldGenPromotedBytes  |  Innalzamento di livello alle dimensioni dei dati di generazione precedente  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft. AppPlatform/Spring  |  SystemCpuUsagePercentage  |  Percentuale di utilizzo CPU di sistema  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft. AppPlatform/Spring  |  TomcatErrorCount  |  Errore globale Tomcat  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. AppPlatform/Spring  |  TomcatReceivedBytes  |  Totale byte ricevuti Tomcat  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft. AppPlatform/Spring  |  TomcatRequestMaxTime  |  Tempo massimo richiesta Tomcat  |  Millisecondi  |  Massimo | 
| **Sì**  | No |  Microsoft. AppPlatform/Spring  |  TomcatRequestTotalCount  |  Conteggio totale richieste Tomcat  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. AppPlatform/Spring  |  TomcatRequestTotalTime  |  Tempo totale richiesta Tomcat  |  Millisecondi  |  Totale | 
| **Sì**  | No |  Microsoft. AppPlatform/Spring  |  TomcatResponseAvgTime  |  Tempo medio richiesta Tomcat  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft. AppPlatform/Spring  |  TomcatSentBytes  |  Totale byte inviati Tomcat  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft. AppPlatform/Spring  |  TomcatSessionActiveCurrentCount  |  Conteggio attivo della sessione Tomcat  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. AppPlatform/Spring  |  TomcatSessionActiveMaxCount  |  Numero massimo attivo sessione Tomcat  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. AppPlatform/Spring  |  TomcatSessionAliveMaxTime  |  Tempo massimo attivo della sessione Tomcat  |  Millisecondi  |  Massimo | 
| **Sì**  | No |  Microsoft. AppPlatform/Spring  |  TomcatSessionCreatedCount  |  Conteggio creazione sessione Tomcat  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. AppPlatform/Spring  |  TomcatSessionExpiredCount  |  Conteggio scaduto sessione Tomcat  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. AppPlatform/Spring  |  TomcatSessionRejectedCount  |  Conteggio rifiutato sessione Tomcat  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. AppPlatform/Spring  |  YoungGenPromotedBytes  |  Innalzamento di livello a dimensioni dati di generazione giovani  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.Automation/automationAccounts  |  TotalJob  |  Processi totali  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Automation/automationAccounts  |  TotalUpdateDeploymentMachineRuns  |  Totale esecuzioni computer di distribuzione aggiornamenti  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Automation/automationAccounts  |  TotalUpdateDeploymentRuns  |  Totale esecuzioni di aggiornamenti della distribuzione  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Batch/batchAccounts  |  CoreCount  |  Numero di core dedicati  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Batch/batchAccounts  |  CreatingNodeCount  |  Numero nodi creati  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Batch/batchAccounts  |  IdleNodeCount  |  Numero nodi inattivi  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Batch/batchAccounts  |  JobDeleteCompleteEvent  |  Eventi eliminazione processi completati  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Batch/batchAccounts  |  JobDeleteStartEvent  |  Eventi eliminazione processi avviati  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Batch/batchAccounts  |  JobDisableCompleteEvent  |  Eventi disabilitazione processi completati  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Batch/batchAccounts  |  JobDisableStartEvent  |  Eventi disabilitazione processi avviati  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Batch/batchAccounts  |  JobStartEvent  |  Eventi di avvio processi  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Batch/batchAccounts  |  JobTerminateCompleteEvent  |  Eventi terminazione processi completati  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Batch/batchAccounts  |  JobTerminateStartEvent  |  Eventi terminazione processi avviati  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Batch/batchAccounts  |  LeavingPoolNodeCount  |  Numero nodi usciti dal pool  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Batch/batchAccounts  |  LowPriorityCoreCount  |  Numero di core a bassa priorità  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Batch/batchAccounts  |  OfflineNodeCount  |  Numero nodi offline  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Batch/batchAccounts  |  PoolCreateEvent  |  Eventi pool creati  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Batch/batchAccounts  |  PoolDeleteCompleteEvent  |  Eventi eliminazione pool completati  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Batch/batchAccounts  |  PoolDeleteStartEvent  |  Eventi eliminazione pool avviati  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Batch/batchAccounts  |  PoolResizeCompleteEvent  |  Eventi ridimensionamento pool completati  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Batch/batchAccounts  |  PoolResizeStartEvent  |  Eventi ridimensionamento pool avviati  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Batch/batchAccounts  |  PreemptedNodeCount  |  Numero di nodi annullati  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Batch/batchAccounts  |  RebootingNodeCount  |  Numero nodi riavviati  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Batch/batchAccounts  |  ReimagingNodeCount  |  Numero nodi con immagine ricreata  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Batch/batchAccounts  |  RunningNodeCount  |  Numero nodi eseguiti  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Batch/batchAccounts  |  StartingNodeCount  |  Numero nodi avviati  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Batch/batchAccounts  |  StartTaskFailedNodeCount  |  Numero nodi con attività di avvio non riuscita  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Batch/batchAccounts  |  TaskCompleteEvent  |  Eventi attività completate  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Batch/batchAccounts  |  TaskFailEvent  |  Eventi attività non riuscite  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Batch/batchAccounts  |  TaskStartEvent  |  Eventi attività avviate  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Batch/batchAccounts  |  TotalLowPriorityNodeCount  |  Numero di nodi a bassa priorità  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Batch/batchAccounts  |  TotalNodeCount  |  Numero di nodi dedicati  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Batch/batchAccounts  |  UnusableNodeCount  |  Numero nodi non usabili  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Batch/batchAccounts  |  WaitingForStartTaskNodeCount  |  Numero nodi in attesa dell'attività di avvio  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. BatchAI/Workspaces  |  Core attivi  |  Core attivi  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. BatchAI/Workspaces  |  Nodi attivi  |  Nodi attivi  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. BatchAI/Workspaces  |  Core inattivi  |  Core inattivi  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. BatchAI/Workspaces  |  Nodi inattivi  |  Nodi inattivi  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. BatchAI/Workspaces  |  Processo completato  |  Processo completato  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. BatchAI/Workspaces  |  Processo inviato  |  Processo inviato  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. BatchAI/Workspaces  |  Uscita da Core  |  Uscita da Core  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. BatchAI/Workspaces  |  Uscita da nodi  |  Uscita da nodi  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. BatchAI/Workspaces  |  Core con precedenza  |  Core con precedenza  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. BatchAI/Workspaces  |  Nodi con precedenza  |  Nodi con precedenza  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. BatchAI/Workspaces  |  Percentuale di utilizzo della quota  |  Percentuale di utilizzo della quota  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. BatchAI/Workspaces  |  Core totali  |  Core totali  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. BatchAI/Workspaces  |  Totale nodi  |  Totale nodi  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. BatchAI/Workspaces  |  Core inutilizzabili  |  Core inutilizzabili  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. BatchAI/Workspaces  |  Nodi inutilizzabili  |  Nodi inutilizzabili  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. blockchain/blockchainMembers  |  ConnectionAccepted  |  Connessioni accettate  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. blockchain/blockchainMembers  |  ConnectionActive  |  Connessioni attive  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. blockchain/blockchainMembers  |  ConnectionHandled  |  Connessioni gestite  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. blockchain/blockchainMembers  |  CpuUsagePercentageInDouble  |  Percentuale di utilizzo CPU  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft. blockchain/blockchainMembers  |  IOReadBytes  |  Byte lettura IO  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft. blockchain/blockchainMembers  |  IOWriteBytes  |  Byte scritti IO  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft. blockchain/blockchainMembers  |  MemoryLimit  |  Limite di memoria  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft. blockchain/blockchainMembers  |  MemoryUsage  |  Utilizzo memoria  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft. blockchain/blockchainMembers  |  MemoryUsagePercentageInDouble  |  Percentuale di utilizzo della memoria  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft. blockchain/blockchainMembers  |  PendingTransactions  |  Transazioni in sospeso  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. blockchain/blockchainMembers  |  ProcessedBlocks  |  Blocchi elaborati  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. blockchain/blockchainMembers  |  ProcessedTransactions  |  Transazioni elaborate  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. blockchain/blockchainMembers  |  QueuedTransactions  |  Transazioni in coda  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. blockchain/blockchainMembers  |  RequestHandled  |  Richieste gestite  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. blockchain/blockchainMembers  |  StorageUsage  |  Utilizzo dell'archiviazione  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cachehits  |  Riscontri cache  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cachehits0  |  Riscontri cache (partizione 0)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cachehits1  |  Riscontri cache (partizione 1)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cachehits2  |  Riscontri cache (partizione 2)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cachehits3  |  Riscontri cache (partizione 3)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cachehits4  |  Riscontri cache (partizione 4)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cachehits5  |  Riscontri cache (partizione 5)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cachehits6  |  Riscontri cache (partizione 6)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cachehits7  |  Riscontri cache (partizione 7)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cachehits8  |  Riscontri cache (partizione 8)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cachehits9  |  Riscontri cache (partizione 9)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cacheLatency  |  Microsecondi di latenza della cache (anteprima)  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cachemisses  |  Mancati riscontri nella cache  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cachemisses0  |  Mancati riscontri nella cache (partizione 0)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cachemisses1  |  Mancati riscontri nella cache (partizione 1)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cachemisses2  |  Mancati riscontri nella cache (partizione 2)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cachemisses3  |  Mancati riscontri nella cache (partizione 3)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cachemisses4  |  Mancati riscontri nella cache (partizione 4)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cachemisses5  |  Mancati riscontri nella cache (partizione 5)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cachemisses6  |  Mancati riscontri nella cache (partizione 6)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cachemisses7  |  Mancati riscontri nella cache (partizione 7)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cachemisses8  |  Mancati riscontri nella cache (partizione 8)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cachemisses9  |  Mancati riscontri nella cache (partizione 9)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cacheRead  |  Lettura da cache  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cacheRead0  |  Lettura da cache (partizione 0)  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cacheRead1  |  Lettura da cache (partizione 1)  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cacheRead2  |  Lettura da cache (partizione 2)  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cacheRead3  |  Lettura da cache (partizione 3)  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cacheRead4  |  Lettura da cache (partizione 4)  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cacheRead5  |  Lettura da cache (partizione 5)  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cacheRead6  |  Lettura da cache (partizione 6)  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cacheRead7  |  Lettura da cache (partizione 7)  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cacheRead8  |  Lettura da cache (partizione 8)  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cacheRead9  |  Lettura da cache (partizione 9)  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cacheWrite  |  Scrittura nella cache  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cacheWrite0  |  Scrittura nella cache (partizione 0)  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cacheWrite1  |  Scrittura nella cache (partizione 1)  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cacheWrite2  |  Scrittura nella cache (partizione 2)  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cacheWrite3  |  Scrittura nella cache (partizione 3)  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cacheWrite4  |  Scrittura nella cache (partizione 4)  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cacheWrite5  |  Scrittura nella cache (partizione 5)  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cacheWrite6  |  Scrittura nella cache (partizione 6)  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cacheWrite7  |  Scrittura nella cache (partizione 7)  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cacheWrite8  |  Scrittura nella cache (partizione 8)  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  cacheWrite9  |  Scrittura nella cache (partizione 9)  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  connectedclients  |  Client connessi  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  connectedclients0  |  Client connessi (partizione 0)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  connectedclients1  |  Client connessi (partizione 1)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  connectedclients2  |  Client connessi (partizione 2)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  connectedclients3  |  Client connessi (partizione 3)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  connectedclients4  |  Client connessi (partizione 4)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  connectedclients5  |  Client connessi (partizione 5)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  connectedclients6  |  Client connessi (partizione 6)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  connectedclients7  |  Client connessi (partizione 7)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  connectedclients8  |  Client connessi (partizione 8)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  connectedclients9  |  Client connessi (partizione 9)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  errori  |  Errori  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  evictedkeys  |  Chiavi rimosse  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  evictedkeys0  |  Chiavi rimosse (partizione 0)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  evictedkeys1  |  Chiavi rimosse (partizione 1)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  evictedkeys2  |  Chiavi rimosse (partizione 2)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  evictedkeys3  |  Chiavi rimosse (partizione 3)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  evictedkeys4  |  Chiavi rimosse (partizione 4)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  evictedkeys5  |  Chiavi rimosse (partizione 5)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  evictedkeys6  |  Chiavi rimosse (partizione 6)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  evictedkeys7  |  Chiavi rimosse (partizione 7)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  evictedkeys8  |  Chiavi rimosse (partizione 8)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  evictedkeys9  |  Chiavi rimosse (partizione 9)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  expiredkeys  |  Chiavi scadute  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  expiredkeys0  |  Chiavi scadute (partizione 0)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  expiredkeys1  |  Chiavi scadute (partizione 1)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  expiredkeys2  |  Chiavi scadute (partizione 2)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  expiredkeys3  |  Chiavi scadute (partizione 3)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  expiredkeys4  |  Chiavi scadute (partizione 4)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  expiredkeys5  |  Chiavi scadute (partizione 5)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  expiredkeys6  |  Chiavi scadute (partizione 6)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  expiredkeys7  |  Chiavi scadute (partizione 7)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  expiredkeys8  |  Chiavi scadute (partizione 8)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  expiredkeys9  |  Chiavi scadute (partizione 9)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  getcommands  |  Ottiene  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  getcommands0  |  Operazioni Get (partizione 0)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  getcommands1  |  Operazioni Get (partizione 1)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  getcommands2  |  Operazioni Get (partizione 2)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  getcommands3  |  Operazioni Get (partizione 3)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  getcommands4  |  Operazioni Get (partizione 4)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  getcommands5  |  Operazioni Get (partizione 5)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  getcommands6  |  Operazioni Get (partizione 6)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  getcommands7  |  Operazioni Get (partizione 7)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  getcommands8  |  Operazioni Get (partizione 8)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  getcommands9  |  Operazioni Get (partizione 9)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  operationsPerSecond  |  Operazioni al secondo  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  operationsPerSecond0  |  Operazioni al secondo (partizione 0)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  operationsPerSecond1  |  Operazioni al secondo (partizione 1)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  operationsPerSecond2  |  Operazioni al secondo (partizione 2)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  operationsPerSecond3  |  Operazioni al secondo (partizione 3)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  operationsPerSecond4  |  Operazioni al secondo (partizione 4)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  operationsPerSecond5  |  Operazioni al secondo (partizione 5)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  operationsPerSecond6  |  Operazioni al secondo (partizione 6)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  operationsPerSecond7  |  Operazioni al secondo (partizione 7)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  operationsPerSecond8  |  Operazioni al secondo (partizione 8)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  operationsPerSecond9  |  Operazioni al secondo (partizione 9)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  percentProcessorTime  |  CPU  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  percentProcessorTime0  |  CPU (partizione 0)  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  percentProcessorTime1  |  CPU (partizione 1)  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  percentProcessorTime2  |  CPU (partizione 2)  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  percentProcessorTime3  |  CPU (partizione 3)  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  percentProcessorTime4  |  CPU (partizione 4)  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  percentProcessorTime5  |  CPU (partizione 5)  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  percentProcessorTime6  |  CPU (partizione 6)  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  percentProcessorTime7  |  CPU (partizione 7)  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  percentProcessorTime8  |  CPU (partizione 8)  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  percentProcessorTime9  |  CPU (partizione 9)  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  serverLoad  |  Carico server  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  serverLoad0  |  Carico server (partizione 0)  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  serverLoad1  |  Carico server (partizione 1)  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  serverLoad2  |  Carico server (partizione 2)  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  serverLoad3  |  Carico server (partizione 3)  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  serverLoad4  |  Carico server (partizione 4)  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  serverLoad5  |  Carico server (partizione 5)  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  serverLoad6  |  Carico server (partizione 6)  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  serverLoad7  |  Carico server (partizione 7)  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  serverLoad8  |  Carico server (partizione 8)  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  serverLoad9  |  Carico server (partizione 9)  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  setcommands  |  Set  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  setcommands0  |  Operazioni Set (partizione 0)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  setcommands1  |  Operazioni Set (partizione 1)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  setcommands2  |  Operazioni Set (partizione 2)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  setcommands3  |  Operazioni Set (partizione 3)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  setcommands4  |  Operazioni Set (partizione 4)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  setcommands5  |  Operazioni Set (partizione 5)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  setcommands6  |  Operazioni Set (partizione 6)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  setcommands7  |  Operazioni Set (partizione 7)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  setcommands8  |  Operazioni Set (partizione 8)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  setcommands9  |  Operazioni Set (partizione 9)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  totalcommandsprocessed  |  Totale operazioni  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  totalcommandsprocessed0  |  Totale operazioni (partizione 0)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  totalcommandsprocessed1  |  Totale operazioni (partizione 1)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  totalcommandsprocessed2  |  Totale operazioni (partizione 2)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  totalcommandsprocessed3  |  Totale operazioni (partizione 3)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  totalcommandsprocessed4  |  Totale operazioni (partizione 4)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  totalcommandsprocessed5  |  Totale operazioni (partizione 5)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  totalcommandsprocessed6  |  Totale operazioni (partizione 6)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  totalcommandsprocessed7  |  Totale operazioni (partizione 7)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  totalcommandsprocessed8  |  Totale operazioni (partizione 8)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  totalcommandsprocessed9  |  Totale operazioni (partizione 9)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Cache/redis  |  totalkeys  |  Totale chiavi  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  totalkeys0  |  Totale chiavi (partizione 0)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  totalkeys1  |  Totale chiavi (partizione 1)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  totalkeys2  |  Totale chiavi (partizione 2)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  totalkeys3  |  Totale chiavi (partizione 3)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  totalkeys4  |  Totale chiavi (partizione 4)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  totalkeys5  |  Totale chiavi (partizione 5)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  totalkeys6  |  Totale chiavi (partizione 6)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  totalkeys7  |  Totale chiavi (partizione 7)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  totalkeys8  |  Totale chiavi (partizione 8)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  totalkeys9  |  Totale chiavi (partizione 9)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  usedmemory  |  Used Memory  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  usedmemory0  |  Memoria usata (partizione 0)  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  usedmemory1  |  Memoria usata (partizione 1)  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  usedmemory2  |  Memoria usata (partizione 2)  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  usedmemory3  |  Memoria usata (partizione 3)  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  usedmemory4  |  Memoria usata (partizione 4)  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  usedmemory5  |  Memoria usata (partizione 5)  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  usedmemory6  |  Memoria usata (partizione 6)  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  usedmemory7  |  Memoria usata (partizione 7)  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  usedmemory8  |  Memoria usata (partizione 8)  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  usedmemory9  |  Memoria usata (partizione 9)  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  usedmemorypercentage  |  Percentuale memoria usata  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  usedmemoryRss  |  Memoria utilizzata RSS  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  usedmemoryRss0  |  Memoria usata RSS (partizione 0)  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  usedmemoryRss1  |  Memoria usata RSS (partizione 1)  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  usedmemoryRss2  |  Memoria usata RSS (partizione 2)  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  usedmemoryRss3  |  Memoria usata RSS (partizione 3)  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  usedmemoryRss4  |  Memoria usata RSS (partizione 4)  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  usedmemoryRss5  |  Memoria usata RSS (partizione 5)  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  usedmemoryRss6  |  Memoria usata RSS (partizione 6)  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  usedmemoryRss7  |  Memoria usata RSS (partizione 7)  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  usedmemoryRss8  |  Memoria usata RSS (partizione 8)  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.Cache/redis  |  usedmemoryRss9  |  Memoria usata RSS (partizione 9)  |  Byte  |  Massimo | 
| No  | No |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Disk Read Bytes/Sec  |  Lettura disco  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Operazioni lettura disco/sec  |  Operazioni lettura disco/sec  |  Conteggio al secondo  |  Media | 
| No  | No |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Disk Write Bytes/Sec  |  Scrittura disco  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Operazioni scrittura disco/sec  |  Operazioni scrittura disco/sec  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Rete in ingresso  |  Rete in ingresso  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Rete in uscita  |  Rete in uscita  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.ClassicCompute/domainNames/slots/roles  |  CPU percentuale  |  CPU percentuale  |  Percentuale  |  Media | 
| No  | No |  Microsoft.ClassicCompute/virtualMachines  |  Disk Read Bytes/Sec  |  Lettura disco  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft.ClassicCompute/virtualMachines  |  Operazioni lettura disco/sec  |  Operazioni lettura disco/sec  |  Conteggio al secondo  |  Media | 
| No  | No |  Microsoft.ClassicCompute/virtualMachines  |  Disk Write Bytes/Sec  |  Scrittura disco  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft.ClassicCompute/virtualMachines  |  Operazioni scrittura disco/sec  |  Operazioni scrittura disco/sec  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.ClassicCompute/virtualMachines  |  Rete in ingresso  |  Rete in ingresso  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.ClassicCompute/virtualMachines  |  Rete in uscita  |  Rete in uscita  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.ClassicCompute/virtualMachines  |  CPU percentuale  |  CPU percentuale  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.ClassicStorage/storageAccounts  |  Disponibilità  |  Disponibilità  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.ClassicStorage/storageAccounts  |  Egress  |  Egress  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.ClassicStorage/storageAccounts  |  Dati in ingresso  |  Dati in ingresso  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.ClassicStorage/storageAccounts  |  SuccessE2ELatency  |  Latenza end-to-end per richieste con esito positivo  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.ClassicStorage/storageAccounts  |  SuccessServerLatency  |  Latenza server per richieste con esito positivo  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.ClassicStorage/storageAccounts  |  Transazioni  |  Transazioni  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.ClassicStorage/storageAccounts  |  UsedCapacity  |  Capacità usata  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  Disponibilità  |  Disponibilità  |  Percentuale  |  Media | 
| No  | No |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  BlobCapacity  |  Capacità BLOB  |  Byte  |  Media | 
| No  | No |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  BlobCount  |  Numero di BLOB  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  ContainerCount  |  Numero di contenitori BLOB  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  Egress  |  Egress  |  Byte  |  Totale | 
| No  | No |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  IndexCapacity  |  Capacità indice  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  Dati in ingresso  |  Dati in ingresso  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  SuccessE2ELatency  |  Latenza end-to-end per richieste con esito positivo  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  SuccessServerLatency  |  Latenza server per richieste con esito positivo  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  Transazioni  |  Transazioni  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/FileService  |  Disponibilità  |  Disponibilità  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/FileService  |  Egress  |  Egress  |  Byte  |  Totale | 
| No  | No |  Microsoft. ClassicStorage/storageAccounts/FileService  |  FileCapacity  |  Capacità file  |  Byte  |  Media | 
| No  | No |  Microsoft. ClassicStorage/storageAccounts/FileService  |  FileCount  |  Numero di file  |  Conteggio  |  Media | 
| No  | No |  Microsoft. ClassicStorage/storageAccounts/FileService  |  FileShareCount  |  Numero di condivisione file  |  Conteggio  |  Media | 
| No  | No |  Microsoft. ClassicStorage/storageAccounts/FileService  |  FileShareQuota  |  Dimensioni della quota di condivisione file  |  Byte  |  Media | 
| No  | No |  Microsoft. ClassicStorage/storageAccounts/FileService  |  FileShareSnapshotCount  |  Conteggio degli snapshot di condivisione file  |  Conteggio  |  Media | 
| No  | No |  Microsoft. ClassicStorage/storageAccounts/FileService  |  FileShareSnapshotSize  |  Dimensioni snapshot della condivisione file  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/FileService  |  Dati in ingresso  |  Dati in ingresso  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/FileService  |  SuccessE2ELatency  |  Latenza end-to-end per richieste con esito positivo  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/FileService  |  SuccessServerLatency  |  Latenza server per richieste con esito positivo  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/FileService  |  Transazioni  |  Transazioni  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  Disponibilità  |  Disponibilità  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  Egress  |  Egress  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  Dati in ingresso  |  Dati in ingresso  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  QueueCapacity  |  Capacità coda  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  QueueCount  |  Numero di coda  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  QueueMessageCount  |  Numero di messaggi in coda  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  SuccessE2ELatency  |  Latenza end-to-end per richieste con esito positivo  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  SuccessServerLatency  |  Latenza server per richieste con esito positivo  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  Transazioni  |  Transazioni  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  Disponibilità  |  Disponibilità  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  Egress  |  Egress  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  Dati in ingresso  |  Dati in ingresso  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  SuccessE2ELatency  |  Latenza end-to-end per richieste con esito positivo  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  SuccessServerLatency  |  Latenza server per richieste con esito positivo  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  TableCapacity  |  Capacità tabella  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  TableCount  |  Numero di tabella  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  TableEntityCount  |  Numero di entità di tabella  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  Transazioni  |  Transazioni  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.CognitiveServices/accounts  |  BlockedCalls  |  Chiamate bloccate  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.CognitiveServices/accounts  |  CharactersTrained  |  Caratteri sottoposti a training  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.CognitiveServices/accounts  |  CharactersTranslated  |  Caratteri convertiti  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.CognitiveServices/accounts  |  ClientErrors  |  Errori client  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.CognitiveServices/accounts  |  DataIn  |  Dati in entrata  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.CognitiveServices/accounts  |  DataOut  |  Dati in uscita  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.CognitiveServices/accounts  |  Latenza  |  Latenza  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.CognitiveServices/accounts  |  ServerErrors  |  Errori server  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.CognitiveServices/accounts  |  SpeechSessionDuration  |  Durata della sessione vocale  |  Secondi  |  Totale | 
| **Sì**  | No |  Microsoft.CognitiveServices/accounts  |  SuccessfulCalls  |  Chiamate riuscite  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.CognitiveServices/accounts  |  TotalCalls  |  Totale chiamate  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.CognitiveServices/accounts  |  TotalErrors  |  Totale errori  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.CognitiveServices/accounts  |  TotalTokenCalls  |  Totale chiamate token  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.CognitiveServices/accounts  |  TotalTransactions  |  Totale transazioni  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Crediti CPU usati  |  Crediti CPU usati  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Crediti CPU rimanenti  |  Crediti CPU rimanenti  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Profondità della coda del disco dati  |  Profondità coda del disco dati (anteprima)  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Byte letti dal disco dati/sec  |  Byte letti da disco/sec per un disco dati (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Operazioni di lettura disco dati/sec  |  Operazioni di lettura da disco/sec per un disco dati (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Byte scritti disco dati/sec  |  Byte scritti su disco/sec per un disco dati (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Operazioni di scrittura su disco dati/sec  |  Operazioni di scrittura su disco/sec per un disco dati (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Byte letti da disco  |  Byte letti da disco  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Operazioni lettura disco/sec  |  Operazioni lettura disco/sec  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Byte scritti su disco  |  Byte scritti su disco  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Operazioni scrittura disco/sec  |  Operazioni scrittura disco/sec  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Flussi in ingresso  |  Flussi in ingresso  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Velocità di creazione massima dei flussi in ingresso  |  Velocità di creazione massima dei flussi in ingresso (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Rete in ingresso  |  Rete in fatturabile (deprecata)  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Rete totale  |  Rete totale  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Rete in uscita  |  Rete in uscita fatturabile (deprecata)  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Totale rete in uscita  |  Totale rete in uscita  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Profondità coda del disco del sistema operativo  |  Profondità coda del disco del sistema operativo (anteprima)  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Byte letti dal disco del sistema operativo/sec  |  Byte letti da disco/sec per un disco del sistema operativo (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Operazioni di lettura disco del sistema operativo/sec  |  Operazioni di lettura da disco/sec per un disco del sistema operativo (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Byte scritti disco del sistema operativo/sec  |  Byte scritti su disco/sec per un disco del sistema operativo (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Operazioni di scrittura disco del sistema operativo/sec  |  Operazioni di scrittura su disco/sec per un disco del sistema operativo (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Profondità coda per disco del sistema operativo  |  Disco del sistema operativo QD (deprecato)  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Byte letti da disco/sec per disco del sistema operativo  |  Byte letti dal disco del sistema operativo/sec (deprecato)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Operazioni di lettura da disco/sec per disco del sistema operativo  |  Operazioni di lettura disco del sistema operativo/sec (deprecate)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Byte scritti su disco/sec per disco del sistema operativo  |  Byte scritti su disco del sistema operativo/sec (deprecato)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Operazioni di scrittura su disco/sec per disco del sistema operativo  |  Operazioni di scrittura disco del sistema operativo/sec (deprecate)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Flussi in uscita  |  Flussi in uscita  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Velocità di creazione massima dei flussi in uscita  |  Velocità di creazione massima dei flussi in uscita (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Profondità coda per disco dati  |  Disco dati QD (deprecato)  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Byte letti da disco/sec per disco  |  Byte letti dal disco dati/sec (deprecato)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Operazioni di lettura da disco/sec per disco  |  Operazioni di lettura disco dati/sec (deprecate)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Byte scritti su disco/sec per disco  |  Byte scritti disco dati/sec (deprecato)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Operazioni di scrittura su disco/sec per disco  |  Operazioni di scrittura su disco dati/sec (deprecate)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  CPU percentuale  |  CPU percentuale  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Hit lettura cache disco dati Premium  |  Hit Read cache del disco dati Premium (anteprima)  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Lettura della cache del disco dati Premium  |  Lettura della cache del disco dati Premium (anteprima)  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Hit lettura cache del disco del sistema operativo Premium  |  Hit lettura cache del disco del sistema operativo Premium (anteprima)  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachines  |  Cache del disco del sistema operativo Premium Read Miss  |  Cache del disco del sistema operativo Premium lettura mancata (anteprima)  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Crediti CPU usati  |  Crediti CPU usati  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Crediti CPU rimanenti  |  Crediti CPU rimanenti  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Profondità della coda del disco dati  |  Profondità coda del disco dati (anteprima)  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Byte letti dal disco dati/sec  |  Byte letti da disco/sec per un disco dati (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Operazioni di lettura disco dati/sec  |  Operazioni di lettura da disco/sec per un disco dati (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Byte scritti disco dati/sec  |  Byte scritti su disco/sec per un disco dati (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Operazioni di scrittura su disco dati/sec  |  Operazioni di scrittura su disco/sec per un disco dati (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Byte letti da disco  |  Byte letti da disco  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Operazioni lettura disco/sec  |  Operazioni lettura disco/sec  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Byte scritti su disco  |  Byte scritti su disco  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Operazioni scrittura disco/sec  |  Operazioni scrittura disco/sec  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Flussi in ingresso  |  Flussi in ingresso  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Velocità di creazione massima dei flussi in ingresso  |  Velocità di creazione massima dei flussi in ingresso (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Rete in ingresso  |  Rete in fatturabile (deprecata)  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Rete totale  |  Rete totale  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Rete in uscita  |  Rete in uscita fatturabile (deprecata)  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Totale rete in uscita  |  Totale rete in uscita  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Profondità coda del disco del sistema operativo  |  Profondità coda del disco del sistema operativo (anteprima)  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Byte letti dal disco del sistema operativo/sec  |  Byte letti da disco/sec per un disco del sistema operativo (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Operazioni di lettura disco del sistema operativo/sec  |  Operazioni di lettura da disco/sec per un disco del sistema operativo (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Byte scritti disco del sistema operativo/sec  |  Byte scritti su disco/sec per un disco del sistema operativo (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Operazioni di scrittura disco del sistema operativo/sec  |  Operazioni di scrittura su disco/sec per un disco del sistema operativo (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Profondità coda per disco del sistema operativo  |  Disco del sistema operativo QD (deprecato)  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Byte letti da disco/sec per disco del sistema operativo  |  Byte letti dal disco del sistema operativo/sec (deprecato)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Operazioni di lettura da disco/sec per disco del sistema operativo  |  Operazioni di lettura disco del sistema operativo/sec (deprecate)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Byte scritti su disco/sec per disco del sistema operativo  |  Byte scritti su disco del sistema operativo/sec (deprecato)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Operazioni di scrittura su disco/sec per disco del sistema operativo  |  Operazioni di scrittura disco del sistema operativo/sec (deprecate)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Flussi in uscita  |  Flussi in uscita  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Velocità di creazione massima dei flussi in uscita  |  Velocità di creazione massima dei flussi in uscita (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Profondità coda per disco dati  |  Disco dati QD (deprecato)  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Byte letti da disco/sec per disco  |  Byte letti dal disco dati/sec (deprecato)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Operazioni di lettura da disco/sec per disco  |  Operazioni di lettura disco dati/sec (deprecate)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Byte scritti su disco/sec per disco  |  Byte scritti disco dati/sec (deprecato)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Operazioni di scrittura su disco/sec per disco  |  Operazioni di scrittura su disco dati/sec (deprecate)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  CPU percentuale  |  CPU percentuale  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Hit lettura cache disco dati Premium  |  Hit Read cache del disco dati Premium (anteprima)  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Lettura della cache del disco dati Premium  |  Lettura della cache del disco dati Premium (anteprima)  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Hit lettura cache del disco del sistema operativo Premium  |  Hit lettura cache del disco del sistema operativo Premium (anteprima)  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Cache del disco del sistema operativo Premium Read Miss  |  Cache del disco del sistema operativo Premium lettura mancata (anteprima)  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Crediti CPU usati  |  Crediti CPU usati  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Crediti CPU rimanenti  |  Crediti CPU rimanenti  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Profondità della coda del disco dati  |  Profondità coda del disco dati (anteprima)  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Byte letti dal disco dati/sec  |  Byte letti da disco/sec per un disco dati (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Operazioni di lettura disco dati/sec  |  Operazioni di lettura da disco/sec per un disco dati (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Byte scritti disco dati/sec  |  Byte scritti su disco/sec per un disco dati (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Operazioni di scrittura su disco dati/sec  |  Operazioni di scrittura su disco/sec per un disco dati (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Byte letti da disco  |  Byte letti da disco  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Operazioni lettura disco/sec  |  Operazioni lettura disco/sec  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Byte scritti su disco  |  Byte scritti su disco  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Operazioni scrittura disco/sec  |  Operazioni scrittura disco/sec  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Flussi in ingresso  |  Flussi in ingresso  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Velocità di creazione massima dei flussi in ingresso  |  Velocità di creazione massima dei flussi in ingresso (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Rete in ingresso  |  Rete in fatturabile (deprecata)  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Rete totale  |  Rete totale  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Rete in uscita  |  Rete in uscita fatturabile (deprecata)  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Totale rete in uscita  |  Totale rete in uscita  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Profondità coda del disco del sistema operativo  |  Profondità coda del disco del sistema operativo (anteprima)  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Byte letti dal disco del sistema operativo/sec  |  Byte letti da disco/sec per un disco del sistema operativo (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Operazioni di lettura disco del sistema operativo/sec  |  Operazioni di lettura da disco/sec per un disco del sistema operativo (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Byte scritti disco del sistema operativo/sec  |  Byte scritti su disco/sec per un disco del sistema operativo (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Operazioni di scrittura disco del sistema operativo/sec  |  Operazioni di scrittura su disco/sec per un disco del sistema operativo (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Profondità coda per disco del sistema operativo  |  Disco del sistema operativo QD (deprecato)  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Byte letti da disco/sec per disco del sistema operativo  |  Byte letti dal disco del sistema operativo/sec (deprecato)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Operazioni di lettura da disco/sec per disco del sistema operativo  |  Operazioni di lettura disco del sistema operativo/sec (deprecate)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Byte scritti su disco/sec per disco del sistema operativo  |  Byte scritti su disco del sistema operativo/sec (deprecato)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Operazioni di scrittura su disco/sec per disco del sistema operativo  |  Operazioni di scrittura disco del sistema operativo/sec (deprecate)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Flussi in uscita  |  Flussi in uscita  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Velocità di creazione massima dei flussi in uscita  |  Velocità di creazione massima dei flussi in uscita (anteprima)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Profondità coda per disco dati  |  Disco dati QD (deprecato)  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Byte letti da disco/sec per disco  |  Byte letti dal disco dati/sec (deprecato)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Operazioni di lettura da disco/sec per disco  |  Operazioni di lettura disco dati/sec (deprecate)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Byte scritti su disco/sec per disco  |  Byte scritti disco dati/sec (deprecato)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Operazioni di scrittura su disco/sec per disco  |  Operazioni di scrittura su disco dati/sec (deprecate)  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  CPU percentuale  |  CPU percentuale  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Hit lettura cache disco dati Premium  |  Hit Read cache del disco dati Premium (anteprima)  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Lettura della cache del disco dati Premium  |  Lettura della cache del disco dati Premium (anteprima)  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Hit lettura cache del disco del sistema operativo Premium  |  Hit lettura cache del disco del sistema operativo Premium (anteprima)  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Cache del disco del sistema operativo Premium Read Miss  |  Cache del disco del sistema operativo Premium lettura mancata (anteprima)  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.ContainerInstance/containerGroups  |  CpuUsage  |  Utilizzo CPU  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.ContainerInstance/containerGroups  |  MemoryUsage  |  Utilizzo memoria  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.ContainerInstance/containerGroups  |  NetworkBytesReceivedPerSecond  |  Byte di rete ricevuti al secondo  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.ContainerInstance/containerGroups  |  NetworkBytesTransmittedPerSecond  |  Byte di rete trasmessi al secondo  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.ContainerRegistry/registries  |  RunDuration  |  Durata esecuzione  |  Millisecondi  |  Totale | 
| **Sì**  | No |  Microsoft.ContainerRegistry/registries  |  SuccessfulPullCount  |  Conteggio pull riuscito  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.ContainerRegistry/registries  |  SuccessfulPushCount  |  Conteggio push riuscito  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.ContainerRegistry/registries  |  TotalPullCount  |  Conteggio pull totale  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.ContainerRegistry/registries  |  TotalPushCount  |  Numero totale di push  |  Conteggio  |  Media | 
| No  | No |  Microsoft.ContainerService/managedClusters  |  kube_node_status_allocatable_cpu_cores  |  Numero totale di core CPU disponibili in un cluster gestito  |  Conteggio  |  Media | 
| No  | No |  Microsoft.ContainerService/managedClusters  |  kube_node_status_allocatable_memory_bytes  |  Numero totale di memoria disponibile in un cluster gestito  |  Byte  |  Media | 
| No  | No |  Microsoft.ContainerService/managedClusters  |  kube_node_status_condition  |  Stati per diverse condizioni dei nodi  |  Conteggio  |  Media | 
| No  | No |  Microsoft.ContainerService/managedClusters  |  kube_pod_status_phase  |  Numero di pod per fase  |  Conteggio  |  Media | 
| No  | No |  Microsoft.ContainerService/managedClusters  |  kube_pod_status_ready  |  Numero di pod in stato Pronto  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  AvailableCapacity  |  Capacità disponibile  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  BytesUploadedToCloud  |  Byte cloud caricati (dispositivo)  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  BytesUploadedToCloudPerShare  |  Byte cloud caricati (condivisione)  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  CloudReadThroughput  |  Velocità effettiva di download del cloud  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  CloudReadThroughputPerShare  |  Velocità effettiva di download del cloud (condivisione)  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  CloudUploadThroughput  |  Velocità effettiva di caricamento cloud  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  CloudUploadThroughputPerShare  |  Velocità effettiva di caricamento cloud (condivisione)  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  HyperVMemoryUtilization  |  Calcolo Edge-utilizzo memoria  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  HyperVVirtualProcessorUtilization  |  Calcolo Edge-percentuale CPU  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  NICReadThroughput  |  Velocità effettiva di lettura (rete)  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  NICWriteThroughput  |  Velocità effettiva scrittura (rete)  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  Archiviazione  |  Capacità totale:  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.DataFactory/datafactories  |  FailedRuns  |  Esecuzioni non riuscite  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.DataFactory/datafactories  |  SuccessfulRuns  |  Esecuzioni riuscite  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.DataFactory/factories  |  ActivityCancelledRuns  |  Metriche delle esecuzioni di attività annullate  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.DataFactory/factories  |  ActivityFailedRuns  |  Failed activity runs metrics (Metrica esecuzioni attività non riuscite)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.DataFactory/factories  |  ActivitySucceededRuns  |  Succeeded activity runs metrics (Metrica esecuzioni attività riuscite)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.DataFactory/factories  |  FactorySizeInGbUnits  |  Dimensioni totali Factory (unità GB)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.DataFactory/factories  |  IntegrationRuntimeAvailableMemory  |  Memoria disponibile di Integration Runtime  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.DataFactory/factories  |  IntegrationRuntimeAverageTaskPickupDelay  |  Durata coda di Integration Runtime  |  Secondi  |  Media | 
| **Sì**  | No |  Microsoft.DataFactory/factories  |  IntegrationRuntimeCpuPercentage  |  Uso della CPU di Integration runtime  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.DataFactory/factories  |  IntegrationRuntimeQueueLength  |  Lunghezza coda runtime di integrazione  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.DataFactory/factories  |  MaxAllowedFactorySizeInGbUnits  |  Dimensioni massime consentite per le factory (unità GB)  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.DataFactory/factories  |  MaxAllowedResourceCount  |  Conteggio entità massime consentite  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.DataFactory/factories  |  PipelineCancelledRuns  |  Metriche delle esecuzioni di pipeline annullate  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.DataFactory/factories  |  PipelineFailedRuns  |  Failed pipeline runs metrics (Metrica esecuzioni pipeline non riuscite)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.DataFactory/factories  |  PipelineSucceededRuns  |  Succeeded pipeline runs metrics (Metrica esecuzioni pipeline riuscite)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.DataFactory/factories  |  ResourceCount  |  Conteggio entità totali  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.DataFactory/factories  |  TriggerCancelledRuns  |  Il trigger annullato esegue le metriche  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.DataFactory/factories  |  TriggerFailedRuns  |  Failed trigger runs metrics (Metrica esecuzioni trigger non riuscite)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.DataFactory/factories  |  TriggerSucceededRuns  |  Succeeded trigger runs metrics (Metrica esecuzioni trigger riuscite)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.DataLakeAnalytics/accounts  |  JobAUEndedCancelled  |  Tempo di aggiornamenti automatici annullati  |  Secondi  |  Totale | 
| **Sì**  | No |  Microsoft.DataLakeAnalytics/accounts  |  JobAUEndedFailure  |  Tempo di aggiornamenti automatici non riusciti  |  Secondi  |  Totale | 
| **Sì**  | No |  Microsoft.DataLakeAnalytics/accounts  |  JobAUEndedSuccess  |  Tempo di aggiornamenti automatici con esito positivo  |  Secondi  |  Totale | 
| **Sì**  | No |  Microsoft.DataLakeAnalytics/accounts  |  JobEndedCancelled  |  Processi annullati  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.DataLakeAnalytics/accounts  |  JobEndedFailure  |  Processi non riusciti  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.DataLakeAnalytics/accounts  |  JobEndedSuccess  |  Processi completati  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.DataLakeStore/accounts  |  DataRead  |  Dati letti  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.DataLakeStore/accounts  |  DataWritten  |  Dati scritti  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.DataLakeStore/accounts  |  ReadRequests  |  Richieste di lettura  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.DataLakeStore/accounts  |  TotalStorage  |  Spazio di archiviazione totale  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.DataLakeStore/accounts  |  WriteRequests  |  Richieste di scrittura  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.DBforMariaDB/servers  |  active_connections  |  Connessioni attive  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.DBforMariaDB/servers  |  backup_storage_used  |  Archiviazione di backup utilizzata  |  Byte  |  Media | 
| **Sì**  | **Sì** |  Microsoft.DBforMariaDB/servers  |  connections_failed  |  Connessioni non riuscite  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.DBforMariaDB/servers  |  cpu_percent  |  Percentuale CPU  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.DBforMariaDB/servers  |  io_consumption_percent  |  Percentuale IO  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.DBforMariaDB/servers  |  memory_percent  |  Percentuale memoria  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.DBforMariaDB/servers  |  network_bytes_egress  |  Rete in uscita  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.DBforMariaDB/servers  |  network_bytes_ingress  |  Rete in ingresso  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.DBforMariaDB/servers  |  seconds_behind_master  |  Intervallo di replica in secondi  |  Conteggio  |  Massimo | 
| **Sì**  | **Sì** |  Microsoft.DBforMariaDB/servers  |  serverlog_storage_limit  |  Limite di archiviazione dei log del server  |  Byte  |  Media | 
| **Sì**  | **Sì** |  Microsoft.DBforMariaDB/servers  |  serverlog_storage_percent  |  Percentuale di archiviazione dei log del server  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.DBforMariaDB/servers  |  serverlog_storage_usage  |  Archiviazione dei log del server usata  |  Byte  |  Media | 
| **Sì**  | **Sì** |  Microsoft.DBforMariaDB/servers  |  storage_limit  |  Limite archiviazione  |  Byte  |  Massimo | 
| **Sì**  | **Sì** |  Microsoft.DBforMariaDB/servers  |  storage_percent  |  Percentuale di archiviazione  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.DBforMariaDB/servers  |  storage_used  |  Uso archiviazione  |  Byte  |  Media | 
| **Sì**  | **Sì** |  Microsoft.DBforMySQL/servers  |  active_connections  |  Connessioni attive  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.DBforMySQL/servers  |  backup_storage_used  |  Archiviazione di backup utilizzata  |  Byte  |  Media | 
| **Sì**  | **Sì** |  Microsoft.DBforMySQL/servers  |  connections_failed  |  Connessioni non riuscite  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.DBforMySQL/servers  |  cpu_percent  |  Percentuale CPU  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.DBforMySQL/servers  |  io_consumption_percent  |  Percentuale IO  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.DBforMySQL/servers  |  memory_percent  |  Percentuale memoria  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.DBforMySQL/servers  |  network_bytes_egress  |  Rete in uscita  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.DBforMySQL/servers  |  network_bytes_ingress  |  Rete in ingresso  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.DBforMySQL/servers  |  seconds_behind_master  |  Intervallo di replica in secondi  |  Conteggio  |  Massimo | 
| **Sì**  | **Sì** |  Microsoft.DBforMySQL/servers  |  serverlog_storage_limit  |  Limite di archiviazione dei log del server  |  Byte  |  Massimo | 
| **Sì**  | **Sì** |  Microsoft.DBforMySQL/servers  |  serverlog_storage_percent  |  Percentuale di archiviazione dei log del server  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.DBforMySQL/servers  |  serverlog_storage_usage  |  Archiviazione dei log del server usata  |  Byte  |  Media | 
| **Sì**  | **Sì** |  Microsoft.DBforMySQL/servers  |  storage_limit  |  Limite archiviazione  |  Byte  |  Massimo | 
| **Sì**  | **Sì** |  Microsoft.DBforMySQL/servers  |  storage_percent  |  Percentuale di archiviazione  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.DBforMySQL/servers  |  storage_used  |  Uso archiviazione  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.DBforPostgreSQL/servers  |  active_connections  |  Connessioni attive  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.DBforPostgreSQL/servers  |  backup_storage_used  |  Archiviazione di backup utilizzata  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.DBforPostgreSQL/servers  |  connections_failed  |  Connessioni non riuscite  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.DBforPostgreSQL/servers  |  cpu_percent  |  Percentuale CPU  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.DBforPostgreSQL/servers  |  io_consumption_percent  |  Percentuale IO  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.DBforPostgreSQL/servers  |  memory_percent  |  Percentuale memoria  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.DBforPostgreSQL/servers  |  network_bytes_egress  |  Rete in uscita  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.DBforPostgreSQL/servers  |  network_bytes_ingress  |  Rete in ingresso  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.DBforPostgreSQL/servers  |  pg_replica_log_delay_in_bytes  |  Max Lag Across Replicas (Ritardo massimo tra repliche)  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.DBforPostgreSQL/servers  |  pg_replica_log_delay_in_seconds  |  Replica Lag (Ritardo replica)  |  Secondi  |  Massimo | 
| **Sì**  | No |  Microsoft.DBforPostgreSQL/servers  |  serverlog_storage_limit  |  Limite di archiviazione dei log del server  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.DBforPostgreSQL/servers  |  serverlog_storage_percent  |  Percentuale di archiviazione dei log del server  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.DBforPostgreSQL/servers  |  serverlog_storage_usage  |  Archiviazione dei log del server usata  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.DBforPostgreSQL/servers  |  storage_limit  |  Limite archiviazione  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.DBforPostgreSQL/servers  |  storage_percent  |  Percentuale di archiviazione  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.DBforPostgreSQL/servers  |  storage_used  |  Uso archiviazione  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.DBforPostgreSQL/serversv2  |  active_connections  |  Connessioni attive  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.DBforPostgreSQL/serversv2  |  cpu_percent  |  Percentuale CPU  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.DBforPostgreSQL/serversv2  |  IOPS  |  IOPS  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.DBforPostgreSQL/serversv2  |  memory_percent  |  Percentuale memoria  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.DBforPostgreSQL/serversv2  |  network_bytes_egress  |  Rete in uscita  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.DBforPostgreSQL/serversv2  |  network_bytes_ingress  |  Rete in ingresso  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.DBforPostgreSQL/serversv2  |  storage_percent  |  Percentuale di archiviazione  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.DBforPostgreSQL/serversv2  |  storage_used  |  Uso archiviazione  |  Byte  |  Media | 
| **Sì**  | **Sì** |  Microsoft. Devices/account  |  digitaltwins. telemetria. Nodes  |  Segnaposto di telemetria del nodo di gemelli digitali  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  c2d.commands.egress.abandon.success  |  Messaggi C2D abbandonati  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  c2d.commands.egress.complete.success  |  Recapito messaggi C2D completati  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  c2d.commands.egress.reject.success  |  Messaggi C2D rifiutati  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  c2d.methods.failure  |  Chiamate a metodi diretti non riuscite  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  c2d.methods.requestSize  |  Dimensioni delle richieste di chiamate a metodi diretti  |  Byte  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  c2d.methods.responseSize  |  Dimensioni delle risposte a chiamate a metodi diretti  |  Byte  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  c2d.methods.success  |  Chiamate a metodi diretti riuscite  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  c2d.twin.read.failure  |  Letture dei dispositivi gemelli non riuscite per il back-end  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  c2d.twin.read.size  |  Dimensioni delle risposte di letture dei dispositivi gemelli dal back-end  |  Byte  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  c2d.twin.read.success  |  Letture dei dispositivi gemelli completate dal back-end  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  c2d.twin.update.failure  |  Aggiornamenti dei dispositivi gemelli non riusciti per il back-end  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  c2d.twin.update.size  |  Dimensioni degli aggiornamenti dei dispositivi gemelli dal back-end  |  Byte  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  c2d.twin.update.success  |  Aggiornamenti dei dispositivi gemelli completati dal back-end  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  C2DMessagesExpired  |  Messaggi C2D scaduti (anteprima)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  configurazioni  |  Metriche di configurazione  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Devices/IotHubs  |  connectedDeviceCount  |  Dispositivi connessi (anteprima)  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.builtIn.events  |  Routing: messaggi recapitati a messaggi/eventi  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.eventHubs  |  Routing: messaggi inviati all'hub eventi  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.serviceBusQueues  |  Routing: messaggi recapitati alla coda del bus di servizio  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.serviceBusTopics  |  Routing: messaggi recapitati all'argomento del bus di servizio  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.storage  |  Routing: messaggi recapitati all'archiviazione  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.storage.blobs  |  Routing: BLOB recapitati all'archiviazione  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.storage.bytes  |  Routing: dati recapitati all'archiviazione  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.latency.builtIn.events  |  Routing: latenza dei messaggi per messaggi/eventi  |  Millisecondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.latency.eventHubs  |  Routing: latenza dei messaggi per l'hub eventi  |  Millisecondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.latency.serviceBusQueues  |  Routing: latenza dei messaggi per la coda del bus di servizio  |  Millisecondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.latency.serviceBusTopics  |  Routing: latenza dei messaggi per l'argomento del bus di servizio  |  Millisecondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.latency.storage  |  Routing: latenza dei messaggi per l'archiviazione  |  Millisecondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.egress.dropped  |  Routing: messaggi di telemetria eliminati   |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.egress.fallback  |  Routing: messaggi recapitati al fallback  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.egress.invalid  |  Routing: messaggi di telemetria incompatibili  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.egress.orphaned  |  Routing: messaggi di telemetria orfani   |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.egress.success  |  Routing: messaggi di telemetria recapitati  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.ingress.allProtocol  |  Tentativi di invio di messaggi di telemetria  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.ingress.sendThrottle  |  Number of throttling errors (Numero di errori di limitazione)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.ingress.success  |  Messaggi di telemetria inviati  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.twin.read.failure  |  Letture dei dispositivi gemelli non riuscite per i dispositivi  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.twin.read.size  |  Dimensioni delle risposte di letture dei dispositivi gemelli dai dispositivi  |  Byte  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.twin.read.success  |  Letture dei dispositivi gemelli completate dai dispositivi  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.twin.update.failure  |  Aggiornamenti dei dispositivi gemelli non riusciti per i dispositivi  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.twin.update.size  |  Dimensioni degli aggiornamenti dei dispositivi gemelli dai dispositivi  |  Byte  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  d2c.twin.update.success  |  Aggiornamenti dei dispositivi gemelli completati dai dispositivi  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  dailyMessageQuotaUsed  |  Total number of messages used (Numero totale di messaggi usati)  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  deviceDataUsage  |  Utilizzo totale dei dati del dispositivo  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  deviceDataUsageV2  |  Uso totale dei dati del dispositivo (anteprima)  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  devices.connectedDevices.allProtocol  |  Dispositivi connessi (deprecati)   |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  devices.totalDevices  |  Dispositivi totali (deprecati)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  EventGridDeliveries  |  Recapiti di griglia di eventi (anteprima)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  EventGridLatency  |  Latenza griglia di eventi (anteprima)  |  Millisecondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  jobs.cancelJob.failure  |  Annullamenti di processi non riusciti  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  jobs.cancelJob.success  |  Annullamenti di processi riusciti  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  jobs.completed  |  Processi completati  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  jobs.createDirectMethodJob.failure  |  Creazioni di processi di chiamata al metodo non riuscite  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  jobs.createDirectMethodJob.success  |  Creazioni di processi di chiamata al metodo completate  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  jobs.createTwinUpdateJob.failure  |  Creazioni di processi di aggiornamento dei dispositivi gemelli non riuscite  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  jobs.createTwinUpdateJob.success  |  Creazioni di processi di aggiornamento dei dispositivi gemelli completate  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  jobs.failed  |  Processi non riusciti  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  jobs.listJobs.failure  |  Chiamate per elencare i processi non riuscite  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  jobs.listJobs.success  |  Chiamate per elencare i processi riuscite  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  jobs.queryJobs.failure  |  Query sui processi non riuscite  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  jobs.queryJobs.success  |  Query sui processi riuscite  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Devices/IotHubs  |  totalDeviceCount  |  Dispositivi totali (anteprima)  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  twinQueries.failure  |  Query dei dispositivi gemelli non riuscite  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  twinQueries.resultSize  |  Dimensioni dei risultati delle query dei dispositivi gemelli  |  Byte  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Devices/IotHubs  |  twinQueries.success  |  Query dei dispositivi gemelli completate  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/provisioningServices  |  AttestationAttempts  |  Tentativi di attestazione  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/provisioningServices  |  DeviceAssignments  |  Dispositivi assegnati  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Devices/provisioningServices  |  RegistrationAttempts  |  Tentativi di registrazione  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  AvailableStorage  |  Archiviazione disponibile  |  Byte  |  Totale | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  CassandraConnectionClosures  |  Chiusure di connessione Cassandra  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  CassandraRequestCharges  |  Addebiti richieste Cassandra  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  CassandraRequests  |  Richieste di Cassandra  |  Conteggio  |  Conteggio | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  DataUsage  |  Utilizzo dei dati  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.DocumentDB/databaseAccounts  |  DeleteVirtualNetwork  |  DeleteVirtualNetwork  |  Conteggio  |  Conteggio | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  DocumentCount  |  Conteggio documenti  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  DocumentQuota  |  Quota documenti  |  Byte  |  Totale | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  IndexUsage  |  Utilizzo indice  |  Byte  |  Totale | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  MetadataRequests  |  Metadata Requests (Richieste di metadati)  |  Conteggio  |  Conteggio | 
| **Sì**  | **Sì** |  Microsoft.DocumentDB/databaseAccounts  |  MongoRequestCharge  |  Mongo Request Charge (Addebito richiesta Mongo)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.DocumentDB/databaseAccounts  |  MongoRequests  |  Richieste Mongo  |  Conteggio  |  Conteggio | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  MongoRequestsCount  |  Frequenza di richieste Mongo  |  Conteggio al secondo  |  Media | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  MongoRequestsDelete  |  Frequenza delle richieste di eliminazione Mongo  |  Conteggio al secondo  |  Media | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  MongoRequestsInsert  |  Frequenza di richieste di inserimento Mongo  |  Conteggio al secondo  |  Media | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  MongoRequestsQuery  |  Frequenza di richieste di query Mongo  |  Conteggio al secondo  |  Media | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  MongoRequestsUpdate  |  Frequenza di richieste di aggiornamento Mongo  |  Conteggio al secondo  |  Media | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  ProvisionedThroughput  |  Velocità effettiva sottoposta a provisioning  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.DocumentDB/databaseAccounts  |  ReplicationLatency  |  Latenza di replica P99  |  Millisecondi  |  Media | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  ServiceAvailability  |  Disponibilità dei servizi  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.DocumentDB/databaseAccounts  |  TotalRequests  |  Totale richieste  |  Conteggio  |  Conteggio | 
| **Sì**  | **Sì** |  Microsoft.DocumentDB/databaseAccounts  |  TotalRequestUnits  |  Total Request Units (Unità richiesta totali)  |  Conteggio  |  Totale | 
| No  | No |  Microsoft. EnterpriseKnowledgeGraph/Services  |  FailureCount  |  Conteggio errori  |  Conteggio  |  Conteggio | 
| No  | No |  Microsoft. EnterpriseKnowledgeGraph/Services  |  SuccessCount  |  Conteggio completo  |  Conteggio  |  Conteggio | 
| No  | No |  Microsoft. EnterpriseKnowledgeGraph/Services  |  SuccessLatency  |  Latenza riuscita  |  Millisecondi  |  Media | 
| No  | No |  Microsoft. EnterpriseKnowledgeGraph/Services  |  TransactionCount  |  Conteggio transazioni  |  Conteggio  |  Conteggio | 
| **Sì**  | **Sì** |  Microsoft.EventGrid/domains  |  DeadLetteredCount  |  Eventi di messaggi non recapitati  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.EventGrid/domains  |  DeliveryAttemptFailCount  |  Eventi con recapito non riuscito  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventGrid/domains  |  DeliverySuccessCount  |  Eventi recapitati  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.EventGrid/domains  |  DestinationProcessingDurationInMs  |  Durata di elaborazione della destinazione  |  Millisecondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.EventGrid/domains  |  DroppedEventCount  |  Eventi eliminati  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventGrid/domains  |  MatchedEventCount  |  Eventi abbinati  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventGrid/domains  |  PublishFailCount  |  Pubblica eventi non riusciti  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventGrid/domains  |  PublishSuccessCount  |  Eventi pubblicati  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventGrid/domains  |  PublishSuccessLatencyInMs  |  Latenza pubblicazione riuscita  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventGrid/eventSubscriptions  |  DeadLetteredCount  |  Eventi di messaggi non recapitati  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.EventGrid/eventSubscriptions  |  DeliveryAttemptFailCount  |  Eventi con recapito non riuscito  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventGrid/eventSubscriptions  |  DeliverySuccessCount  |  Eventi recapitati  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.EventGrid/eventSubscriptions  |  DestinationProcessingDurationInMs  |  Durata di elaborazione della destinazione  |  Millisecondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.EventGrid/eventSubscriptions  |  DroppedEventCount  |  Eventi eliminati  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventGrid/eventSubscriptions  |  MatchedEventCount  |  Eventi abbinati  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventGrid/extensionTopics  |  PublishFailCount  |  Pubblica eventi non riusciti  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventGrid/extensionTopics  |  PublishSuccessCount  |  Eventi pubblicati  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventGrid/extensionTopics  |  PublishSuccessLatencyInMs  |  Latenza pubblicazione riuscita  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventGrid/extensionTopics  |  UnmatchedEventCount  |  Eventi senza corrispondenza  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventGrid/topics  |  PublishFailCount  |  Pubblica eventi non riusciti  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventGrid/topics  |  PublishSuccessCount  |  Eventi pubblicati  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventGrid/topics  |  PublishSuccessLatencyInMs  |  Latenza pubblicazione riuscita  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventGrid/topics  |  UnmatchedEventCount  |  Eventi senza corrispondenza  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.EventHub/clusters  |  ActiveConnections  |  ActiveConnections  |  Conteggio  |  Media | 
| No  | No |  Microsoft.EventHub/clusters  |  AvailableMemory  |  Memoria disponibile  |  Percentuale  |  Massimo | 
| No  | No |  Microsoft.EventHub/clusters  |  CaptureBacklog  |  Backlog acquisiti.  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.EventHub/clusters  |  CapturedBytes  |  Byte acquisiti.  |  Byte  |  Totale | 
| No  | No |  Microsoft.EventHub/clusters  |  CapturedMessages  |  Messaggi acquisiti.  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.EventHub/clusters  |  ConnectionsClosed  |  Connessioni chiuse.  |  Conteggio  |  Media | 
| No  | No |  Microsoft.EventHub/clusters  |  ConnectionsOpened  |  Connessioni aperte.  |  Conteggio  |  Media | 
| No  | No |  Microsoft.EventHub/clusters  |  CPU  |  CPU  |  Percentuale  |  Massimo | 
| **Sì**  | **Sì** |  Microsoft.EventHub/clusters  |  IncomingBytes  |  Byte in ingresso.  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/clusters  |  IncomingMessages  |  Messaggi in ingresso  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/clusters  |  IncomingRequests  |  Richieste in ingresso  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/clusters  |  OutgoingBytes  |  Byte in uscita.  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/clusters  |  OutgoingMessages  |  Messaggi in uscita  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.EventHub/clusters  |  QuotaExceededErrors  |  Errori di superamento quota.  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.EventHub/clusters  |  ServerErrors  |  Errori del server  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.EventHub/clusters  |  SuccessfulRequests  |  Richieste riuscite  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.EventHub/clusters  |  ThrottledRequests  |  Richieste limitate.  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.EventHub/clusters  |  UserErrors  |  Errori utente.  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.EventHub/namespaces  |  ActiveConnections  |  ActiveConnections  |  Conteggio  |  Media | 
| No  | No |  Microsoft.EventHub/namespaces  |  CaptureBacklog  |  Backlog acquisiti.  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.EventHub/namespaces  |  CapturedBytes  |  Byte acquisiti.  |  Byte  |  Totale | 
| No  | No |  Microsoft.EventHub/namespaces  |  CapturedMessages  |  Messaggi acquisiti.  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.EventHub/namespaces  |  ConnectionsClosed  |  Connessioni chiuse.  |  Conteggio  |  Media | 
| No  | No |  Microsoft.EventHub/namespaces  |  ConnectionsOpened  |  Connessioni aperte.  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.EventHub/namespaces  |  EHABL  |  Archivia messaggi backlog (deprecato)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/namespaces  |  EHAMBS  |  Archiviazione della velocità effettiva del messaggio (deprecata)  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/namespaces  |  EHAMSGS  |  Messaggi di archivio (deprecati)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/namespaces  |  EHINBYTES  |  Byte in ingresso (deprecata)  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/namespaces  |  EHINMBS  |  Byte in ingresso (obsoleti) (deprecato)  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/namespaces  |  EHINMSGS  |  Messaggi in ingresso (deprecata)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/namespaces  |  EHOUTBYTES  |  Byte in uscita (deprecata)  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/namespaces  |  EHOUTMBS  |  Byte in uscita (obsoleti) (deprecato)  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/namespaces  |  EHOUTMSGS  |  Messaggi in uscita (deprecata)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/namespaces  |  FAILREQ  |  Richieste non riuscite (deprecate)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/namespaces  |  IncomingBytes  |  Byte in ingresso.  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/namespaces  |  IncomingMessages  |  Messaggi in ingresso  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/namespaces  |  IncomingRequests  |  Richieste in ingresso  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/namespaces  |  INMSGS  |  Messaggi in ingresso (obsoleti) (deprecato)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/namespaces  |  INREQS  |  Richieste in ingresso (deprecate)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/namespaces  |  INTERR  |  Errori interni del server (deprecati)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/namespaces  |  MISCERR  |  Altri errori (deprecato)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/namespaces  |  OutgoingBytes  |  Byte in uscita.  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/namespaces  |  OutgoingMessages  |  Messaggi in uscita  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/namespaces  |  OUTMSGS  |  Messaggi in uscita (obsoleti) (deprecato)  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.EventHub/namespaces  |  QuotaExceededErrors  |  Errori di superamento quota.  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.EventHub/namespaces  |  ServerErrors  |  Errori del server  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.EventHub/namespaces  |  Dimensione  |  Dimensione  |  Byte  |  Media | 
| No  | No |  Microsoft.EventHub/namespaces  |  SuccessfulRequests  |  Richieste riuscite  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/namespaces  |  SUCCREQ  |  Richieste riuscite (deprecate)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.EventHub/namespaces  |  SVRBSY  |  Errori di server occupato (deprecato)  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.EventHub/namespaces  |  ThrottledRequests  |  Richieste limitate.  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.EventHub/namespaces  |  UserErrors  |  Errori utente.  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.HDInsight/clusters  |  CategorizedGatewayRequests  |  Richieste del gateway per categoria  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.HDInsight/clusters  |  GatewayRequests  |  Richieste del gateway  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.HDInsight/clusters  |  NumActiveWorkers  |  Numero di thread di lavoro attivi  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.HDInsight/clusters  |  ScalingRequests  |  Ridimensionamento di richieste  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Insights/AutoscaleSettings  |  MetricThreshold  |  Soglia della metrica  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Insights/AutoscaleSettings  |  ObservedCapacity  |  Capacità osservata  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Insights/AutoscaleSettings  |  ObservedMetricValue  |  Valore della metrica osservato  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Insights/AutoscaleSettings  |  ScaleActionsInitiated  |  Azioni di ridimensionamento avviate  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Insights/Components  |  availabilityResults/availabilityPercentage  |  Disponibilità  |  Percentuale  |  Media | 
| No  | No |  Microsoft.Insights/Components  |  availabilityResults/count  |  Test della disponibilità  |  Conteggio  |  Conteggio | 
| **Sì**  | No |  Microsoft.Insights/Components  |  availabilityResults/duration  |  Durata del test di disponibilità  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.Insights/Components  |  browserTimings/networkDuration  |  Tempo di connessione alla rete per il caricamento della pagina  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.Insights/Components  |  browserTimings/processingDuration  |  Tempo di elaborazione client  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.Insights/Components  |  browserTimings/receiveDuration  |  Tempo per la ricezione della risposta  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.Insights/Components  |  browserTimings/sendDuration  |  Tempo per l'invio della richiesta  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.Insights/Components  |  browserTimings/totalDuration  |  Tempo di caricamento della pagina del browser  |  Millisecondi  |  Media | 
| No  | No |  Microsoft.Insights/Components  |  dependencies/count  |  Chiamate di dipendenza  |  Conteggio  |  Conteggio | 
| **Sì**  | No |  Microsoft.Insights/Components  |  dependencies/duration  |  Durata della dipendenza  |  Millisecondi  |  Media | 
| No  | No |  Microsoft.Insights/Components  |  dependencies/failed  |  Errori delle chiamate di dipendenza  |  Conteggio  |  Conteggio | 
| No  | No |  Microsoft.Insights/Components  |  exceptions/browser  |  Eccezioni del browser  |  Conteggio  |  Conteggio | 
| **Sì**  | **Sì** |  Microsoft.Insights/Components  |  exceptions/count  |  Eccezioni  |  Conteggio  |  Conteggio | 
| No  | No |  Microsoft.Insights/Components  |  exceptions/server  |  Eccezioni del server  |  Conteggio  |  Conteggio | 
| **Sì**  | **Sì** |  Microsoft.Insights/Components  |  pageViews/count  |  Visualizzazioni pagina  |  Conteggio  |  Conteggio | 
| **Sì**  | No |  Microsoft.Insights/Components  |  pageViews/duration  |  Tempo di caricamento della visualizzazione pagina  |  Millisecondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Insights/Components  |  performanceCounters/exceptionsPerSecond  |  Frequenza di eccezioni  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Insights/Components  |  performanceCounters/memoryAvailableBytes  |  Memoria disponibile  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.Insights/Components  |  performanceCounters/processCpuPercentage  |  CPU processo  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Insights/Components  |  performanceCounters/processIOBytesPerSecond  |  Velocità di I/O di elaborazione  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Insights/Components  |  performanceCounters/processorCpuPercentage  |  Tempo processore  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Insights/Components  |  performanceCounters/processPrivateBytes  |  Byte privati processo  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.Insights/Components  |  performanceCounters/requestExecutionTime  |  Tempo di esecuzione della richiesta HTTP  |  Millisecondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Insights/Components  |  performanceCounters/requestsInQueue  |  Richieste HTTP nella coda dell'applicazione  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Insights/Components  |  performanceCounters/requestsPerSecond  |  Frequenza delle richieste HTTP  |  Conteggio al secondo  |  Media | 
| No  | No |  Microsoft.Insights/Components  |  requests/count  |  Richieste server  |  Conteggio  |  Conteggio | 
| **Sì**  | No |  Microsoft.Insights/Components  |  requests/duration  |  Tempo di risposta del server  |  Millisecondi  |  Media | 
| No  | No |  Microsoft.Insights/Components  |  requests/failed  |  Richieste non riuscite  |  Conteggio  |  Conteggio | 
| No  | No |  Microsoft.Insights/Components  |  richieste/frequenza  |  Frequenza richieste server  |  Conteggio al secondo  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Insights/Components  |  traces/count  |  Traces  |  Conteggio  |  Conteggio | 
| **Sì**  | No |  Microsoft.KeyVault/vaults  |  ServiceApiHit  |  Totale di accessi all'API del servizio  |  Conteggio  |  Conteggio | 
| **Sì**  | No |  Microsoft.KeyVault/vaults  |  ServiceApiLatency  |  Latenza complessiva dell'API del servizio  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.KeyVault/vaults  |  ServiceApiResult  |  Totale dei risultati dell'API del servizio  |  Conteggio  |  Conteggio | 
| **Sì**  | **Sì** |  Microsoft.Kusto/Clusters  |  CacheUtilization  |  Utilizzo della cache  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Kusto/Clusters  |  ContinuousExportMaxLatenessMinutes  |  Minuti di ritardo massimo esportazione continua  |  Conteggio  |  Massimo | 
| **Sì**  | **Sì** |  Microsoft.Kusto/Clusters  |  ContinuousExportNumOfRecordsExported  |  Esportazione continua: numero di record esportati  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Kusto/Clusters  |  ContinuousExportPendingCount  |  Conteggio in sospeso esportazione continua  |  Conteggio  |  Massimo | 
| **Sì**  | **Sì** |  Microsoft.Kusto/Clusters  |  ContinuousExportResult  |  Risultato dell'esportazione continua  |  Conteggio  |  Conteggio | 
| **Sì**  | **Sì** |  Microsoft.Kusto/Clusters  |  CPU  |  CPU  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Kusto/Clusters  |  EventsProcessedForEventHubs  |  Eventi elaborati (per gli hub eventi/tutto)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Kusto/Clusters  |  ExportUtilization  |  Esporta utilizzo  |  Percentuale  |  Massimo | 
| **Sì**  | **Sì** |  Microsoft.Kusto/Clusters  |  IngestionLatencyInSeconds  |  Latenza inserimento (in secondi)  |  Secondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Kusto/Clusters  |  IngestionResult  |  Risultato inserimento  |  Conteggio  |  Conteggio | 
| **Sì**  | **Sì** |  Microsoft.Kusto/Clusters  |  IngestionUtilization  |  Utilizzo dell'inserimento  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Kusto/Clusters  |  IngestionVolumeInMB  |  Volume di inserimento (in MB)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Kusto/Clusters  |  KeepAlive  |  Keep-Alive  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Kusto/Clusters  |  QueryDuration  |  Durata delle query  |  Millisecondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Kusto/Clusters  |  SteamingIngestRequestRate  |  Frequenza delle richieste di inserimento di flussi  |  Conteggio  |  RateRequestsPerSecond | 
| **Sì**  | **Sì** |  Microsoft.Kusto/Clusters  |  StreamingIngestDataRate  |  Velocità dati di inserimento dei flussi  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Kusto/Clusters  |  StreamingIngestDuration  |  Durata inserimento flusso  |  Millisecondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Kusto/Clusters  |  StreamingIngestResults  |  Risultato inserimento flusso  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Logic/integrationServiceEnvironments  |  ActionLatency  |  Latenza azioni   |  Secondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Logic/integrationServiceEnvironments  |  ActionsCompleted  |  Azioni completate   |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/integrationServiceEnvironments  |  ActionsFailed  |  Azioni non riuscite   |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/integrationServiceEnvironments  |  ActionsSkipped  |  Azioni ignorate   |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/integrationServiceEnvironments  |  ActionsStarted  |  Azioni avviate   |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/integrationServiceEnvironments  |  ActionsSucceeded  |  Azioni riuscite   |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Logic/integrationServiceEnvironments  |  ActionSuccessLatency  |  Latenza azioni riuscite   |  Secondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Logic/integrationServiceEnvironments  |  ActionThrottledEvents  |  Eventi limitati azione  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Logic/integrationServiceEnvironments  |  IntegrationServiceEnvironmentConnectorMemoryUsage  |  Utilizzo memoria connettore per ambiente del servizio di integrazione  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Logic/integrationServiceEnvironments  |  IntegrationServiceEnvironmentConnectorProcessorUsage  |  Utilizzo del processore del connettore per ambiente del servizio di integrazione  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Logic/integrationServiceEnvironments  |  IntegrationServiceEnvironmentWorkflowMemoryUsage  |  Utilizzo della memoria del flusso di lavoro per ambiente del servizio di integrazione  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Logic/integrationServiceEnvironments  |  IntegrationServiceEnvironmentWorkflowProcessorUsage  |  Utilizzo del processore del flusso di lavoro per ambiente del servizio di integrazione  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Logic/integrationServiceEnvironments  |  RunFailurePercentage  |  Percentuale di errori di esecuzione  |  Percentuale  |  Totale | 
| **Sì**  | No |  Microsoft.Logic/integrationServiceEnvironments  |  RunLatency  |  Latenza esecuzioni  |  Secondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Logic/integrationServiceEnvironments  |  RunsCancelled  |  Esecuzioni annullate  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/integrationServiceEnvironments  |  RunsCompleted  |  Esecuzioni completate  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/integrationServiceEnvironments  |  RunsFailed  |  Esecuzioni non riuscite  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/integrationServiceEnvironments  |  RunsStarted  |  Esecuzioni avviate  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/integrationServiceEnvironments  |  RunsSucceeded  |  Esecuzioni riuscite  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/integrationServiceEnvironments  |  RunStartThrottledEvents  |  Eseguire gli eventi di avvio limitato  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Logic/integrationServiceEnvironments  |  RunSuccessLatency  |  Latenza esecuzioni riuscite  |  Secondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Logic/integrationServiceEnvironments  |  RunThrottledEvents  |  Eventi limitati esecuzione  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Logic/integrationServiceEnvironments  |  TriggerFireLatency  |  Latenza trigger rimossi   |  Secondi  |  Media | 
| **Sì**  | No |  Microsoft.Logic/integrationServiceEnvironments  |  TriggerLatency  |  Latenza trigger   |  Secondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Logic/integrationServiceEnvironments  |  TriggersCompleted  |  Trigger completati   |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/integrationServiceEnvironments  |  TriggersFailed  |  Trigger non riusciti   |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/integrationServiceEnvironments  |  TriggersFired  |  Trigger rimossi   |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/integrationServiceEnvironments  |  TriggersSkipped  |  Trigger ignorati  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/integrationServiceEnvironments  |  TriggersStarted  |  Trigger avviati   |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/integrationServiceEnvironments  |  TriggersSucceeded  |  Trigger riusciti   |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Logic/integrationServiceEnvironments  |  TriggerSuccessLatency  |  Latenza trigger riusciti   |  Secondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Logic/integrationServiceEnvironments  |  TriggerThrottledEvents  |  Eventi limitati trigger  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Logic/workflows  |  ActionLatency  |  Latenza azioni   |  Secondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  ActionsCompleted  |  Azioni completate   |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  ActionsFailed  |  Azioni non riuscite   |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  ActionsSkipped  |  Azioni ignorate   |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  ActionsStarted  |  Azioni avviate   |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  ActionsSucceeded  |  Azioni riuscite   |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Logic/workflows  |  ActionSuccessLatency  |  Latenza azioni riuscite   |  Secondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  ActionThrottledEvents  |  Eventi limitati azione  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  BillableActionExecutions  |  Esecuzioni di azioni fatturabili  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  BillableTriggerExecutions  |  Esecuzioni di trigger fatturabili  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  BillingUsageNativeOperation  |  Uso fatturazione per esecuzioni di operazioni native  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  BillingUsageNativeOperation  |  Uso fatturazione per esecuzioni di operazioni native  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  BillingUsageStandardConnector  |  Uso fatturazione per esecuzioni del connettore standard  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  BillingUsageStandardConnector  |  Uso fatturazione per esecuzioni del connettore standard  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  BillingUsageStorageConsumption  |  Uso fatturazione per esecuzioni con uso dell'archiviazione  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  BillingUsageStorageConsumption  |  Uso fatturazione per esecuzioni con uso dell'archiviazione  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  RunFailurePercentage  |  Percentuale di errori di esecuzione  |  Percentuale  |  Totale | 
| **Sì**  | No |  Microsoft.Logic/workflows  |  RunLatency  |  Latenza esecuzioni  |  Secondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  RunsCancelled  |  Esecuzioni annullate  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  RunsCompleted  |  Esecuzioni completate  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  RunsFailed  |  Esecuzioni non riuscite  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  RunsStarted  |  Esecuzioni avviate  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  RunsSucceeded  |  Esecuzioni riuscite  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  RunStartThrottledEvents  |  Eseguire gli eventi di avvio limitato  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Logic/workflows  |  RunSuccessLatency  |  Latenza esecuzioni riuscite  |  Secondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  RunThrottledEvents  |  Eventi limitati esecuzione  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  TotalBillableExecutions  |  Esecuzioni fatturabili totali  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Logic/workflows  |  TriggerFireLatency  |  Latenza trigger rimossi   |  Secondi  |  Media | 
| **Sì**  | No |  Microsoft.Logic/workflows  |  TriggerLatency  |  Latenza trigger   |  Secondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  TriggersCompleted  |  Trigger completati   |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  TriggersFailed  |  Trigger non riusciti   |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  TriggersFired  |  Trigger rimossi   |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  TriggersSkipped  |  Trigger ignorati  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  TriggersStarted  |  Trigger avviati   |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  TriggersSucceeded  |  Trigger riusciti   |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Logic/workflows  |  TriggerSuccessLatency  |  Latenza trigger riusciti   |  Secondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Logic/workflows  |  TriggerThrottledEvents  |  Eventi limitati trigger  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.MachineLearningServices/workspaces  |  Core attivi  |  Core attivi  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.MachineLearningServices/workspaces  |  Nodi attivi  |  Nodi attivi  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.MachineLearningServices/workspaces  |  Esecuzioni completate  |  Esecuzioni completate  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.MachineLearningServices/workspaces  |  Esecuzioni non riuscite  |  Esecuzioni non riuscite  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.MachineLearningServices/workspaces  |  Core inattivi  |  Core inattivi  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.MachineLearningServices/workspaces  |  Nodi inattivi  |  Nodi inattivi  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.MachineLearningServices/workspaces  |  Uscita da Core  |  Uscita da Core  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.MachineLearningServices/workspaces  |  Uscita da nodi  |  Uscita da nodi  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.MachineLearningServices/workspaces  |  Distribuzione modello non riuscito  |  Distribuzione modello non riuscito  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.MachineLearningServices/workspaces  |  Distribuzione modello avviata  |  Distribuzione modello avviata  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.MachineLearningServices/workspaces  |  Distribuzione modello completata  |  Distribuzione modello completata  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.MachineLearningServices/workspaces  |  Registrazione modello non riuscita  |  Registrazione modello non riuscita  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.MachineLearningServices/workspaces  |  Registrazione del modello completata  |  Registrazione del modello completata  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.MachineLearningServices/workspaces  |  Core con precedenza  |  Core con precedenza  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.MachineLearningServices/workspaces  |  Nodi con precedenza  |  Nodi con precedenza  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.MachineLearningServices/workspaces  |  Percentuale di utilizzo della quota  |  Percentuale di utilizzo della quota  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.MachineLearningServices/workspaces  |  Esecuzioni avviate  |  Esecuzioni avviate  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.MachineLearningServices/workspaces  |  Core totali  |  Core totali  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.MachineLearningServices/workspaces  |  Totale nodi  |  Totale nodi  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.MachineLearningServices/workspaces  |  Core inutilizzabili  |  Core inutilizzabili  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.MachineLearningServices/workspaces  |  Nodi inutilizzabili  |  Nodi inutilizzabili  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Maps/accounts  |  Disponibilità  |  Disponibilità  |  Percentuale  |  Media | 
| No  | No |  Microsoft.Maps/accounts  |  Utilizzo  |  Utilizzo  |  Conteggio  |  Conteggio | 
| **Sì**  | No |  Microsoft. Media/MediaServices  |  AssetCount  |  Conteggio asset  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. Media/MediaServices  |  AssetQuota  |  Quota asset  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. Media/MediaServices  |  AssetQuotaUsedPercentage  |  Percentuale di utilizzo della quota di asset  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft. Media/MediaServices  |  ContentKeyPolicyCount  |  Conteggio dei criteri della chiave simmetrica  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. Media/MediaServices  |  ContentKeyPolicyQuota  |  Quota dei criteri della chiave simmetrica  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. Media/MediaServices  |  ContentKeyPolicyQuotaUsedPercentage  |  Percentuale di utilizzo della quota dei criteri della chiave simmetrica  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft. Media/MediaServices  |  StreamingPolicyCount  |  Conteggio dei criteri di flusso  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. Media/MediaServices  |  StreamingPolicyQuota  |  Quota dei criteri di streaming  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. Media/MediaServices  |  StreamingPolicyQuotaUsedPercentage  |  Percentuale di utilizzo della quota dei criteri di flusso  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft. Media/MediaServices/le entità streamingendpoint  |  Egress  |  Egress  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft. Media/MediaServices/le entità streamingendpoint  |  Richieste  |  Richieste  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. Media/MediaServices/le entità streamingendpoint  |  SuccessE2ELatency  |  Latenza end-to-end riuscita  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft. Microservices4Spring/appClusters  |  GCPauseTotalCount  |  Conteggio delle pause GC  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. Microservices4Spring/appClusters  |  GCPauseTotalTime  |  Tempo totale pausa GC  |  Millisecondi  |  Totale | 
| **Sì**  | No |  Microsoft. Microservices4Spring/appClusters  |  MaxOldGenMemoryPoolBytes  |  Dimensioni massime dei dati di generazione obsoleti disponibili  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft. Microservices4Spring/appClusters  |  OldGenMemoryPoolBytes  |  Dimensioni dati di generazione precedenti  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft. Microservices4Spring/appClusters  |  OldGenPromotedBytes  |  Innalzamento di livello alle dimensioni dei dati di generazione precedente  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft. Microservices4Spring/appClusters  |  ServiceCpuUsagePercentage  |  Percentuale di utilizzo CPU del servizio  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft. Microservices4Spring/appClusters  |  ServiceMemoryCommitted  |  Memoria del servizio assegnata  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft. Microservices4Spring/appClusters  |  ServiceMemoryMax  |  Memoria del servizio max  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft. Microservices4Spring/appClusters  |  ServiceMemoryUsed  |  Memoria del servizio utilizzata  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft. Microservices4Spring/appClusters  |  SystemCpuUsagePercentage  |  Percentuale di utilizzo CPU di sistema  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft. Microservices4Spring/appClusters  |  TomcatErrorCount  |  Errore globale Tomcat  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. Microservices4Spring/appClusters  |  TomcatReceivedBytes  |  Totale byte ricevuti Tomcat  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft. Microservices4Spring/appClusters  |  TomcatRequestMaxTime  |  Tempo massimo richiesta Tomcat  |  Millisecondi  |  Massimo | 
| **Sì**  | No |  Microsoft. Microservices4Spring/appClusters  |  TomcatRequestTotalCount  |  Conteggio totale richieste Tomcat  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. Microservices4Spring/appClusters  |  TomcatRequestTotalTime  |  Tempo totale richiesta Tomcat  |  Millisecondi  |  Totale | 
| **Sì**  | No |  Microsoft. Microservices4Spring/appClusters  |  TomcatResponseAvgTime  |  Tempo medio richiesta Tomcat  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft. Microservices4Spring/appClusters  |  TomcatSentBytes  |  Totale byte inviati Tomcat  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft. Microservices4Spring/appClusters  |  TomcatSessionActiveCurrentCount  |  Conteggio attivo della sessione Tomcat  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. Microservices4Spring/appClusters  |  TomcatSessionActiveMaxCount  |  Numero massimo attivo sessione Tomcat  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. Microservices4Spring/appClusters  |  TomcatSessionAliveMaxTime  |  Tempo massimo attivo della sessione Tomcat  |  Millisecondi  |  Massimo | 
| **Sì**  | No |  Microsoft. Microservices4Spring/appClusters  |  TomcatSessionCreatedCount  |  Conteggio creazione sessione Tomcat  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. Microservices4Spring/appClusters  |  TomcatSessionExpiredCount  |  Conteggio scaduto sessione Tomcat  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. Microservices4Spring/appClusters  |  TomcatSessionRejectedCount  |  Conteggio rifiutato sessione Tomcat  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. Microservices4Spring/appClusters  |  YoungGenPromotedBytes  |  Innalzamento di livello a dimensioni dati di generazione giovani  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.NetApp/netAppAccounts/capacityPools  |  VolumePoolAllocatedUsed  |  Dimensioni usate del pool di volumi  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.NetApp/netAppAccounts/capacityPools  |  VolumePoolTotalLogicalSize  |  Dimensioni logiche totale del pool di volumi  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft. NetApp/netAppAccounts/capacityPools/volumes  |  AverageReadLatency  |  Latenza media in lettura  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft. NetApp/netAppAccounts/capacityPools/volumes  |  AverageWriteLatency  |  Latenza media in scrittura  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft. NetApp/netAppAccounts/capacityPools/volumes  |  ReadIops  |  Operazioni di I/O in lettura  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft. NetApp/netAppAccounts/capacityPools/volumes  |  VolumeLogicalSize  |  Dimensioni logiche del volume  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft. NetApp/netAppAccounts/capacityPools/volumes  |  VolumeSnapshotSize  |  Dimensioni dello snapshot del volume  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft. NetApp/netAppAccounts/capacityPools/volumes  |  WriteIops  |  Operazioni di I/O in scrittura  |  Conteggio al secondo  |  Media | 
| No  | No |  Microsoft.Network/applicationGateways  |  ApplicationGatewayTotalTime  |  Tempo totale del gateway applicazione  |  Millisecondi  |  Media | 
| No  | No |  Microsoft.Network/applicationGateways  |  AvgRequestCountPerHealthyHost  |  Richieste al minuto per host integro  |  Conteggio  |  Media | 
| No  | No |  Microsoft.Network/applicationGateways  |  BackendConnectTime  |  Tempo di connessione back-end  |  Millisecondi  |  Media | 
| No  | No |  Microsoft.Network/applicationGateways  |  BackendFirstByteResponseTime  |  Tempo di risposta primo byte back-end  |  Millisecondi  |  Media | 
| No  | No |  Microsoft.Network/applicationGateways  |  BackendLastByteResponseTime  |  Tempo di risposta ultimo byte back-end  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.Network/applicationGateways  |  BackendResponseStatus  |  Stato della risposta back-end  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/applicationGateways  |  BlockedCount  |  Distribuzione della regola richieste bloccate del Web Application Firewall  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/applicationGateways  |  BlockedReqCount  |  Conteggio richieste bloccate del Web Application Firewall  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/applicationGateways  |  BytesReceived  |  Byte ricevuti  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Network/applicationGateways  |  BytesSent  |  Byte inviati  |  Byte  |  Totale | 
| No  | No |  Microsoft.Network/applicationGateways  |  CapacityUnits  |  Unità di capacità correnti  |  Conteggio  |  Media | 
| No  | No |  Microsoft.Network/applicationGateways  |  ClientRtt  |  RTT client  |  Millisecondi  |  Media | 
| No  | No |  Microsoft.Network/applicationGateways  |  ComputeUnits  |  Unità di calcolo correnti  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Network/applicationGateways  |  CurrentConnections  |  Connessioni correnti  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/applicationGateways  |  FailedRequests  |  Richieste non riuscite  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/applicationGateways  |  HealthyHostCount  |  Healthy Host Count (Numero di host integri)  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Network/applicationGateways  |  MatchedCount  |  Distribuzione della regola totale del firewall applicazione Web  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/applicationGateways  |  ResponseStatus  |  Stato della risposta  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Network/applicationGateways  |  Velocità effettiva  |  Velocità effettiva  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Network/applicationGateways  |  TlsProtocol  |  Protocollo TLS client  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/applicationGateways  |  TotalRequests  |  Totale richieste  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/applicationGateways  |  UnhealthyHostCount  |  Numero di host non integri  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Network/azurefirewalls  |  ApplicationRuleHit  |  Numero di passaggi delle regole dell'applicazione  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/azurefirewalls  |  DataProcessing  |  Dati elaborati  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Network/azurefirewalls  |  FirewallHealth  |  Stato di integrità del firewall  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Network/azurefirewalls  |  NetworkRuleHit  |  Numero di passaggi delle regole di rete  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/azurefirewalls  |  SNATPortUtilization  |  Utilizzo delle porte SNAT  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Network/connections  |  BitsInPerSecond  |  BitsInPerSecond  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Network/connections  |  BitsOutPerSecond  |  BitsOutPerSecond  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Network/dnszones  |  QueryVolume  |  Volume della query  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Network/dnszones  |  RecordSetCapacityUtilization  |  Uso della capacità di set di record  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/dnszones  |  RecordSetCount  |  Numero di set di record  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/expressRouteCircuits  |  ArpAvailability  |  Disponibilità ARP  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Network/expressRouteCircuits  |  BgpAvailability  |  Disponibilità BGP  |  Percentuale  |  Media | 
| No  | No |  Microsoft.Network/expressRouteCircuits  |  BitsInPerSecond  |  BitsInPerSecond  |  Conteggio al secondo  |  Media | 
| No  | No |  Microsoft.Network/expressRouteCircuits  |  BitsOutPerSecond  |  BitsOutPerSecond  |  Conteggio al secondo  |  Media | 
| No  | No |  Microsoft.Network/expressRouteCircuits  |  GlobalReachBitsInPerSecond  |  GlobalReachBitsInPerSecond  |  Conteggio al secondo  |  Media | 
| No  | No |  Microsoft.Network/expressRouteCircuits  |  GlobalReachBitsOutPerSecond  |  GlobalReachBitsOutPerSecond  |  Conteggio al secondo  |  Media | 
| No  | No |  Microsoft.Network/expressRouteCircuits  |  QosDropBitsInPerSecond  |  DroppedInBitsPerSecond  |  Conteggio al secondo  |  Media | 
| No  | No |  Microsoft.Network/expressRouteCircuits  |  QosDropBitsOutPerSecond  |  DroppedOutBitsPerSecond  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Network/expressRouteCircuits/peerings  |  BitsInPerSecond  |  BitsInPerSecond  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Network/expressRouteCircuits/peerings  |  BitsOutPerSecond  |  BitsOutPerSecond  |  Conteggio al secondo  |  Media | 
| No  | No |  Microsoft. Network/expressRouteGateways  |  ErGatewayConnectionBitsInPerSecond  |  BitsInPerSecond  |  Conteggio al secondo  |  Media | 
| No  | No |  Microsoft. Network/expressRouteGateways  |  ErGatewayConnectionBitsOutPerSecond  |  BitsOutPerSecond  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft. Network/expressRoutePorts  |  AdminState  |  AdminState  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. Network/expressRoutePorts  |  LineProtocol  |  LineProtocol  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. Network/expressRoutePorts  |  PortBitsInPerSecond  |  BitsInPerSecond  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft. Network/expressRoutePorts  |  PortBitsOutPerSecond  |  BitsOutPerSecond  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft. Network/expressRoutePorts  |  RxLightLevel  |  RxLightLevel  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. Network/expressRoutePorts  |  TxLightLevel  |  TxLightLevel  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Network/frontdoors  |  BackendHealthPercentage  |  Percentuale di integrità del back-end  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Network/frontdoors  |  BackendRequestCount  |  Conteggio delle richieste del back-end  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Network/frontdoors  |  BackendRequestLatency  |  Latenza della richiesta del back-end  |  Millisecondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Network/frontdoors  |  BillableResponseSize  |  Dimensioni risposta fatturabile  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Network/frontdoors  |  RequestCount  |  Conteggio richieste  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Network/frontdoors  |  RequestSize  |  Dimensioni della richiesta  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Network/frontdoors  |  ResponseSize  |  Dimensioni della risposta  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Network/frontdoors  |  TotalLatency  |  Latenza totale  |  Millisecondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Network/frontdoors  |  WebApplicationFirewallRequestCount  |  Conteggio delle richieste web application firewall  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Network/loadBalancers  |  AllocatedSnatPorts  |  Porte SNAT allocate (anteprima)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/loadBalancers  |  ByteCount  |  Numero di byte  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/loadBalancers  |  DipAvailability  |  Stato probe di integrità  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Network/loadBalancers  |  PacketCount  |  Numero di pacchetti  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/loadBalancers  |  SnatConnectionCount  |  Numero di connessioni SNAT  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/loadBalancers  |  SYNCount  |  Numero di SYN  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Network/loadBalancers  |  UsedSnatPorts  |  Porte SNAT usate (anteprima)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/loadBalancers  |  VipAvailability  |  Disponibilità del percorso dati  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Network/networkInterfaces  |  BytesReceivedRate  |  Byte ricevuti  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Network/networkInterfaces  |  BytesSentRate  |  Byte inviati  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Network/networkInterfaces  |  PacketsReceivedRate  |  Pacchetti ricevuti  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/networkInterfaces  |  PacketsSentRate  |  Pacchetti inviati  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/networkWatchers/connectionMonitors  |  AverageRoundtripMs  |  Tempo medio di round trip (MS)  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.Network/networkWatchers/connectionMonitors  |  ChecksFailedPercent  |  Verifica percentuale non riuscita (anteprima)  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Network/networkWatchers/connectionMonitors  |  ProbesFailedPercent  |  % di probe non riusciti  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Network/networkWatchers/connectionMonitors  |  RoundTripTimeMs  |  Tempo di round trip (MS) (anteprima)  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  ByteCount  |  Numero di byte  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  BytesDroppedDDoS  |  DDoS byte in ingresso eliminati  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  BytesForwardedDDoS  |  DDoS byte in ingresso inoltrati  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  BytesInDDoS  |  DDoS byte in ingresso  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  DDoSTriggerSYNPackets  |  Inbound SYN packets to trigger DDoS mitigation (Pacchetti SYN in ingresso per attivare la mitigazione DDoS)  |  Conteggio al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  DDoSTriggerTCPPackets  |  Pacchetti TCP in ingresso per attivare la mitigazione DDoS  |  Conteggio al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  DDoSTriggerUDPPackets  |  Pacchetti UDP in ingresso per attivare la mitigazione DDoS  |  Conteggio al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  IfUnderDDoSAttack  |  Sotto attacco DDoS o no  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  PacketCount  |  Numero di pacchetti  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  PacketsDroppedDDoS  |  DDoS pacchetti in ingresso eliminati  |  Conteggio al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  PacketsForwardedDDoS  |  DDoS pacchetti in ingresso inoltrati  |  Conteggio al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  PacketsInDDoS  |  DDoS pacchetti in ingresso  |  Conteggio al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  SynCount  |  Numero di SYN  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  TCPBytesDroppedDDoS  |  DDoS byte TCP in ingresso eliminati  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  TCPBytesForwardedDDoS  |  DDoS byte TCP in ingresso inoltrati  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  TCPBytesInDDoS  |  DDoS byte TCP in ingresso  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  TCPPacketsDroppedDDoS  |  DDoS pacchetti TCP in ingresso eliminati  |  Conteggio al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  TCPPacketsForwardedDDoS  |  DDoS pacchetti TCP in ingresso inoltrati  |  Conteggio al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  TCPPacketsInDDoS  |  DDoS pacchetti TCP in ingresso  |  Conteggio al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  UDPBytesDroppedDDoS  |  DDoS byte UDP in ingresso eliminati  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  UDPBytesForwardedDDoS  |  DDoS byte UDP in ingresso inoltrati  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  UDPBytesInDDoS  |  DDoS byte UDP in ingresso  |  Byte al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  UDPPacketsDroppedDDoS  |  DDoS pacchetti UDP in ingresso eliminati  |  Conteggio al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  UDPPacketsForwardedDDoS  |  DDoS pacchetti UDP in ingresso inoltrati  |  Conteggio al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  UDPPacketsInDDoS  |  DDoS pacchetti UDP in ingresso  |  Conteggio al secondo  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/publicIPAddresses  |  VipAvailability  |  Disponibilità del percorso dati  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Network/trafficManagerProfiles  |  ProbeAgentCurrentEndpointStateByProfileResourceId  |  Stato endpoint per endpoint  |  Conteggio  |  Massimo | 
| **Sì**  | **Sì** |  Microsoft.Network/trafficManagerProfiles  |  QpsByEndpoint  |  Query da endpoint restituite  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/virtualNetworkGateways  |  AverageBandwidth  |  Larghezza di banda S2S gateway  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Network/virtualNetworkGateways  |  P2SBandwidth  |  Larghezza di banda per connessione da punto a sito gateway  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Network/virtualNetworkGateways  |  P2SConnectionCount  |  Numero di connessioni da punto a sito  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Network/virtualNetworkGateways  |  TunnelAverageBandwidth  |  Larghezza di banda tunnel  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Network/virtualNetworkGateways  |  TunnelEgressBytes  |  Byte in uscita tunnel  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Network/virtualNetworkGateways  |  TunnelEgressPacketDropTSMismatch  |  Eliminazione pacchetti per mancata corrispondenza selettore traffico in uscita tunnel  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/virtualNetworkGateways  |  TunnelEgressPackets  |  Pacchetti in uscita tunnel  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/virtualNetworkGateways  |  TunnelIngressBytes  |  Byte in ingresso tunnel  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Network/virtualNetworkGateways  |  TunnelIngressPacketDropTSMismatch  |  Eliminazione pacchetti per mancata corrispondenza selettore traffico in ingresso tunnel  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/virtualNetworkGateways  |  TunnelIngressPackets  |  Pacchetti in ingresso tunnel  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Network/virtualNetworks  |  PingMeshAverageRoundtripMs  |  Tempo di round trip per i ping a una macchina virtuale  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.Network/virtualNetworks  |  PingMeshProbesFailedPercent  |  Ping non riusciti a una macchina virtuale  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  incoming  |  Messaggi in ingresso  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  incoming.all.failedrequests  |  Tutte le richieste in ingresso non riuscite  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  incoming.all.requests  |  Tutte le richieste in ingresso  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  incoming.scheduled  |  Notifiche push pianificate inviate  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  incoming.scheduled.cancel  |  Notifiche push pianificate annullate  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  installation.all  |  Operazioni di gestione installazione  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  installation.delete  |  Elimina operazioni di installazione  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  installation.get  |  Ottieni operazioni di installazione  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  installation.patch  |  Operazioni di installazione patch  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  installation.upsert  |  Crea o aggiorna operazioni di installazione  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  notificationhub.pushes  |  Tutte le notifiche in uscita  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.allpns.badorexpiredchannel  |  Errori canale non valido o scaduto  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.allpns.channelerror  |  Errori canale  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.allpns.invalidpayload  |  Errori payload  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.allpns.pnserror  |  Errori sistema di notifica esterno  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.allpns.success  |  Notifiche riuscite  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.apns.badchannel  |  Errore canale APNS non valido  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.apns.expiredchannel  |  Errore canale APNS scaduto  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.apns.invalidcredentials  |  Errori di autorizzazione del servizio APN  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.apns.invalidnotificationsize  |  Errore dimensioni notifica APNS non valide  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.apns.pnserror  |  Errori APNS  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.apns.success  |  Notifiche APNS completate  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.authenticationerror  |  Errori di autenticazione GCM  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.badchannel  |  Errore canale GCM non valido  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.expiredchannel  |  Errore canale GCM scaduto  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.invalidcredentials  |  Errori di autorizzazione GCM (credenziali non valide)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.invalidnotificationformat  |  Formato notifiche GCM non valido  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.invalidnotificationsize  |  Errore dimensioni notifica GCM non valide  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.pnserror  |  Errori GCM  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.success  |  Notifiche GCM completate  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.throttled  |  Notifiche GCM limitate  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.wrongchannel  |  Errore canale GCM errato  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.authenticationerror  |  Errori di autenticazione MPNS  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.badchannel  |  Errori canale MPNS errato  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.channeldisconnected  |  Canale MPNS disconnesso  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.dropped  |  Notifiche MPNS eliminate  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.invalidcredentials  |  Credenziali MPNS non valide  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.invalidnotificationformat  |  Formato notifiche MPNS non valido  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.pnserror  |  Errori MPNS  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.success  |  Notifiche MPNS completate  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.throttled  |  Notifiche MPNS limitate  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.authenticationerror  |  Errori di autenticazione WNS  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.badchannel  |  Errore canale WNS non valido  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.channeldisconnected  |  Canale WNS disconnesso  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.channelthrottled  |  Canale WNS limitato  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.dropped  |  Notifiche WNS eliminate  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.expiredchannel  |  Errore canale WNS scaduto  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.invalidcredentials  |  Errori di autorizzazione WNS (credenziali non valide)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.invalidnotificationformat  |  Formato notifica WNS non valido  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.invalidnotificationsize  |  Errore dimensioni notifica WNS non valide  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.invalidtoken  |  Errori di autorizzazione WNS (token non valido)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.pnserror  |  Errori WNS  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.success  |  Notifiche WNS completate  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.throttled  |  Notifiche WNS limitate  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.tokenproviderunreachable  |  Errori di autorizzazione WNS (non disponibile)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.wrongtoken  |  Errori di autorizzazione WNS (token errato)  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  registration.all  |  Operazioni di registrazione  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  registration.create  |  Operazioni di creazione registrazione  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  registration.delete  |  Operazioni di eliminazione registrazione  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  registration.get  |  Operazioni di lettura registrazione  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  registration.update  |  Operazioni di aggiornamento registrazione  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  scheduled.pending  |  Notifiche pianificate in sospeso  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Available Memory  |  % Available Memory  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Available Swap Space  |  % Available Swap Space  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Committed Bytes In Use  |  % byte vincolati in uso  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% DPC Time  |  % DPC Time  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Free Inodes  |  % Free Inodes  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Free Space  |  % di spazio libero  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Free Space  |  % di spazio libero  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Idle Time  |  % Tempo inattività  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Interrupt Time  |  % Interrupt Time  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% IO Wait Time  |  % IO Wait Time  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Nice Time  |  % Nice Time  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Privileged Time  |  % Privileged Time  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Processor Time  |  % di tempo del processore  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Processor Time  |  % di tempo del processore  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Used Inodes  |  % Used Inodes  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Used Memory  |  % Used Memory  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Used Space  |  % Used Space  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Used Swap Space  |  % Used Swap Space  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% User Time  |  % User Time  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Available MBytes  |  MByte disponibili  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Available MBytes Memory  |  Available MBytes Memory  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Available MBytes Swap  |  Available MBytes Swap  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Avg. letture disco/sec  |  Media letture disco/sec  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Avg. letture disco/sec  |  Media letture disco/sec  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Avg. trasferimenti disco/sec disco fisico  |  Media sec/trasferimento disco  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Avg. scritture disco/sec. disco fisico  |  Media scritture disco/sec  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Avg. scritture disco/sec. disco fisico  |  Media scritture disco/sec  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Bytes Received/sec  |  Byte ricevuti/sec  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Bytes Sent/sec  |  Byte inviati/sec  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Bytes Total/sec  |  Totale byte/sec  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Current Disk Queue Length  |  Lunghezza coda del disco attuale  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Disk Read Bytes/sec  |  Byte letti da disco/sec  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Disk Reads/sec  |  Letture da disco/sec  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Disk Reads/sec  |  Letture da disco/sec  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Disk Transfers/sec  |  Disk Transfers/sec  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Disk Transfers/sec  |  Disk Transfers/sec  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Disk Write Bytes/sec  |  Byte scritti su disco/sec  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Disk Writes/sec  |  Disk Writes/sec  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Disk Writes/sec  |  Disk Writes/sec  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Free Megabytes  |  Free Megabytes  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Free Megabytes  |  Free Megabytes  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Free Physical Memory  |  Free Physical Memory  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Free Space in Paging Files  |  Free Space in Paging Files  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Free Virtual Memory  |  Free Virtual Memory  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Logical Disk Bytes/sec  |  Logical Disk Bytes/sec  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Page Reads/sec  |  Letture pagina/sec  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Page Writes/sec  |  Scritture pagina/sec  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Pages/sec  |  Pages/sec  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Pct Privileged Time  |  Pct Privileged Time  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Pct User Time  |  Pct User Time  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Physical Disk Bytes/sec  |  Physical Disk Bytes/sec  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Processes  |  Processi  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Processor Queue Length  |  Lunghezza coda processore  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Size Stored In Paging Files  |  Size Stored In Paging Files  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Total Bytes  |  Totale byte  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Total Bytes Received  |  Total Bytes Received  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Total Bytes Transmitted  |  Total Bytes Transmitted  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Total Collisions  |  Total Collisions  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Total Packets Received  |  Total Packets Received  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Total Packets Transmitted  |  Total Packets Transmitted  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Total Rx Errors  |  Total Rx Errors  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Total Tx Errors  |  Total Tx Errors  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Uptime  |  Tempo di attività  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Used MBytes Swap Space  |  Used MBytes Swap Space  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Used Memory kBytes  |  Used Memory kBytes  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Used Memory MBytes  |  Used Memory MBytes  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Users  |  Utenti  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Virtual Shared Memory  |  Virtual Shared Memory  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Evento  |  Evento  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.OperationalInsights/workspaces  |  Heartbeat  |  Heartbeat  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.OperationalInsights/workspaces  |  Aggiornamento  |  Aggiornamento  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.PowerBIDedicated/capacities  |  memory_metric  |  Memoria  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.PowerBIDedicated/capacities  |  memory_thrashing_metric  |  Thrashing di memoria (set di impostazioni)  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.PowerBIDedicated/capacities  |  qpu_high_utilization_metric  |  QPU High Utilization (Utilizzo elevato QPU)  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.PowerBIDedicated/capacities  |  QueryDuration  |  Durata query (set di impostazioni)  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.PowerBIDedicated/capacities  |  QueryPoolJobQueueLength  |  Lunghezza coda processi nel pool di query (set di impostazioni)  |  Conteggio  |  Media | 
| No  | No |  Microsoft.Relay/namespaces  |  ActiveConnections  |  ActiveConnections  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Relay/namespaces  |  ActiveListeners  |  ActiveListeners  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Relay/namespaces  |  BytesTransferred  |  BytesTransferred  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Relay/namespaces  |  ListenerConnections-ClientError  |  ListenerConnections-ClientError  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Relay/namespaces  |  ListenerConnections-ServerError  |  ListenerConnections-ServerError  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Relay/namespaces  |  ListenerConnections-Success  |  ListenerConnections-Success  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Relay/namespaces  |  ListenerConnections-TotalRequests  |  ListenerConnections-TotalRequests  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Relay/namespaces  |  ListenerDisconnects  |  ListenerDisconnects  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Relay/namespaces  |  SenderConnections-ClientError  |  SenderConnections-ClientError  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Relay/namespaces  |  SenderConnections-ServerError  |  SenderConnections-ServerError  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Relay/namespaces  |  Connessioni mittente - Operazione riuscita  |  Connessioni mittente - Operazione riuscita  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Relay/namespaces  |  SenderConnections-TotalRequests  |  SenderConnections-TotalRequests  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Relay/namespaces  |  Disconnessioni mittente  |  Disconnessioni mittente  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Search/searchServices  |  SearchLatency  |  Latenza ricerca  |  Secondi  |  Media | 
| **Sì**  | No |  Microsoft.Search/searchServices  |  SearchQueriesPerSecond  |  Query di ricerca al secondo  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.Search/searchServices  |  ThrottledSearchQueriesPercentage  |  Percentuale query di ricerca limitate  |  Percentuale  |  Media | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  ActiveConnections  |  ActiveConnections  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  ActiveMessages  |  Numero di messaggi attivi contenuti in una coda o in un argomento.  |  Conteggio  |  Media | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  ConnectionsClosed  |  Connessioni chiuse.  |  Conteggio  |  Media | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  ConnectionsOpened  |  Connessioni aperte.  |  Conteggio  |  Media | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  CPUXNS  |  CPU (deprecata)  |  Percentuale  |  Massimo | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  DeadletteredMessages  |  Numero di messaggi non recapitabili in una coda o in un argomento.  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.ServiceBus/namespaces  |  IncomingMessages  |  Messaggi in ingresso  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.ServiceBus/namespaces  |  IncomingRequests  |  Richieste in ingresso  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  Messages  |  Numero di messaggi contenuti in una coda o in un argomento.  |  Conteggio  |  Media | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  NamespaceCpuUsage  |  CPU  |  Percentuale  |  Massimo | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  NamespaceMemoryUsage  |  Utilizzo memoria  |  Percentuale  |  Massimo | 
| **Sì**  | **Sì** |  Microsoft.ServiceBus/namespaces  |  OutgoingMessages  |  Messaggi in uscita  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  ScheduledMessages  |  Numero di messaggi pianificati in una coda o un argomento.  |  Conteggio  |  Media | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  ServerErrors  |  Errori del server  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  Dimensione  |  Dimensione  |  Byte  |  Media | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  SuccessfulRequests  |  Richieste riuscite  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  ThrottledRequests  |  Richieste limitate.  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  UserErrors  |  Errori utente.  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  WSXNS  |  Utilizzo memoria (obsoleto)  |  Percentuale  |  Massimo | 
| No  | No |  Microsoft.ServiceFabricMesh/applications  |  ActualCpu  |  ActualCpu  |  Conteggio  |  Media | 
| No  | No |  Microsoft.ServiceFabricMesh/applications  |  ActualMemory  |  ActualMemory  |  Byte  |  Media | 
| No  | No |  Microsoft.ServiceFabricMesh/applications  |  AllocatedCpu  |  AllocatedCpu  |  Conteggio  |  Media | 
| No  | No |  Microsoft.ServiceFabricMesh/applications  |  AllocatedMemory  |  AllocatedMemory  |  Byte  |  Media | 
| No  | No |  Microsoft.ServiceFabricMesh/applications  |  ApplicationStatus  |  ApplicationStatus  |  Conteggio  |  Media | 
| No  | No |  Microsoft.ServiceFabricMesh/applications  |  ContainerStatus  |  ContainerStatus  |  Conteggio  |  Media | 
| No  | No |  Microsoft.ServiceFabricMesh/applications  |  CpuUtilization  |  CpuUtilization  |  Percentuale  |  Media | 
| No  | No |  Microsoft.ServiceFabricMesh/applications  |  MemoryUtilization  |  MemoryUtilization  |  Percentuale  |  Media | 
| No  | No |  Microsoft.ServiceFabricMesh/applications  |  RestartCount  |  RestartCount  |  Conteggio  |  Media | 
| No  | No |  Microsoft.ServiceFabricMesh/applications  |  ServiceReplicaStatus  |  ServiceReplicaStatus  |  Conteggio  |  Media | 
| No  | No |  Microsoft.ServiceFabricMesh/applications  |  ServiceStatus  |  ServiceStatus  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.SignalRService/SignalR  |  ConnectionCount  |  Numero di connessioni  |  Conteggio  |  Massimo | 
| **Sì**  | **Sì** |  Microsoft.SignalRService/SignalR  |  InboundTraffic  |  Traffico in ingresso  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.SignalRService/SignalR  |  MessageCount  |  Numero messaggi  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.SignalRService/SignalR  |  OutboundTraffic  |  Traffico in uscita  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.SignalRService/SignalR  |  SystemErrors  |  Errori di sistema  |  Percentuale  |  Massimo | 
| **Sì**  | **Sì** |  Microsoft.SignalRService/SignalR  |  UserErrors  |  Errori utente  |  Percentuale  |  Massimo | 
| **Sì**  | **Sì** |  Microsoft.Sql/managedInstances  |  avg_cpu_percent  |  Percentuale CPU Media  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Sql/managedInstances  |  io_bytes_read  |  Byte di I/O letti  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.Sql/managedInstances  |  io_bytes_written  |  Byte di I/O scritti  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.Sql/managedInstances  |  io_requests  |  Numero di richieste I/O  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Sql/managedInstances  |  reserved_storage_mb  |  Spazio di archiviazione riservato  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Sql/managedInstances  |  storage_space_used_mb  |  Spazio di archiviazione usato  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Sql/managedInstances  |  virtual_core_count  |  Numero di core virtuali  |  Conteggio  |  Media | 
| No  | No |  Microsoft.Sql/servers  |  database_dtu_consumption_percent  |  Percentuale di DTU  |  Percentuale  |  Media | 
| No  | No |  Microsoft.Sql/servers  |  database_storage_used  |  Spazio dati utilizzato  |  Byte  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers  |  dtu_consumption_percent  |  Percentuale di DTU  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers  |  dtu_used  |  Uso DTU  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Sql/servers  |  storage_used  |  Spazio dati utilizzato  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.Sql/servers/databases  |  allocated_data_storage  |  Spazio dati allocato  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.Sql/servers/databases  |  app_cpu_billed  |  CPU app fatturata  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Sql/servers/databases  |  app_cpu_percent  |  Percentuale CPU app  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Sql/servers/databases  |  app_memory_percent  |  Percentuale memoria app  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers/databases  |  blocked_by_firewall  |  Blocco da parte del firewall  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers/databases  |  cache_hit_percent  |  Percentuale dei riscontri nella cache  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Sql/servers/databases  |  cache_used_percent  |  Percentuale della cache utilizzata  |  Percentuale  |  Massimo | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers/databases  |  connection_failed  |  Connessioni non riuscite  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers/databases  |  connection_successful  |  Connessioni riuscite  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Sql/servers/databases  |  cpu_limit  |  Limite CPU  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers/databases  |  cpu_percent  |  Percentuale CPU  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers/databases  |  cpu_used  |  CPU utilizzata  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers/databases  |  deadlock  |  Deadlock  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers/databases  |  dtu_consumption_percent  |  Percentuale di DTU  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Sql/servers/databases  |  dtu_limit  |  Limite DTU  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers/databases  |  dtu_used  |  Uso DTU  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers/databases  |  dwu_consumption_percent  |  Percentuale DWU  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Sql/servers/databases  |  dwu_limit  |  Limite DWU  |  Conteggio  |  Massimo | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers/databases  |  dwu_used  |  Uso DWU  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Sql/servers/databases  |  local_tempdb_usage_percent  |  Percentuale di tempdb locale  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers/databases  |  log_write_percent  |  Percentuale I/O registro  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Sql/servers/databases  |  memory_usage_percent  |  Percentuale di memoria  |  Percentuale  |  Massimo | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers/databases  |  physical_data_read_percent  |  Percentuale di I/O di dati  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers/databases  |  sessions_percent  |  Percentuale sessioni  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Sql/servers/databases  |  storage  |  Spazio dati utilizzato  |  Byte  |  Massimo | 
| **Sì**  | No |  Microsoft.Sql/servers/databases  |  storage_percent  |  Percentuale spazio dati utilizzato  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Sql/servers/databases  |  tempdb_data_size  |  Kilobyte dimensioni file di dati tempdb  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Sql/servers/databases  |  tempdb_log_size  |  Kilobyte dimensioni file di log tempdb  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Sql/servers/databases  |  tempdb_log_used_percent  |  Log percentuale tempdb utilizzato  |  Percentuale  |  Massimo | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers/databases  |  workers_percent  |  Percentuale ruoli di lavoro  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers/databases  |  xtp_storage_percent  |  Percentuale di archiviazione OLTP in memoria  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Sql/servers/elasticPools  |  allocated_data_storage  |  Spazio dati allocato  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.Sql/servers/elasticPools  |  allocated_data_storage_percent  |  Percentuale allocata spazio dati  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.Sql/servers/elasticPools  |  cpu_limit  |  Limite CPU  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers/elasticPools  |  cpu_percent  |  Percentuale CPU  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Sql/servers/elasticPools  |  cpu_used  |  CPU utilizzata  |  Conteggio  |  Media | 
| No  | No |  Microsoft.Sql/servers/elasticPools  |  database_allocated_data_storage  |  Spazio dati allocato  |  Byte  |  Media | 
| No  | No |  Microsoft.Sql/servers/elasticPools  |  database_cpu_limit  |  Limite CPU  |  Conteggio  |  Media | 
| No  | No |  Microsoft.Sql/servers/elasticPools  |  database_cpu_percent  |  Percentuale CPU  |  Percentuale  |  Media | 
| No  | No |  Microsoft.Sql/servers/elasticPools  |  database_cpu_used  |  CPU utilizzata  |  Conteggio  |  Media | 
| No  | No |  Microsoft.Sql/servers/elasticPools  |  database_dtu_consumption_percent  |  Percentuale di DTU  |  Percentuale  |  Media | 
| No  | No |  Microsoft.Sql/servers/elasticPools  |  database_eDTU_used  |  Uso eDTU  |  Conteggio  |  Media | 
| No  | No |  Microsoft.Sql/servers/elasticPools  |  database_log_write_percent  |  Percentuale I/O registro  |  Percentuale  |  Media | 
| No  | No |  Microsoft.Sql/servers/elasticPools  |  database_physical_data_read_percent  |  Percentuale di I/O di dati  |  Percentuale  |  Media | 
| No  | No |  Microsoft.Sql/servers/elasticPools  |  database_sessions_percent  |  Percentuale sessioni  |  Percentuale  |  Media | 
| No  | No |  Microsoft.Sql/servers/elasticPools  |  database_storage_used  |  Spazio dati utilizzato  |  Byte  |  Media | 
| No  | No |  Microsoft.Sql/servers/elasticPools  |  database_workers_percent  |  Percentuale ruoli di lavoro  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers/elasticPools  |  dtu_consumption_percent  |  Percentuale di DTU  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Sql/servers/elasticPools  |  eDTU_limit  |  Limite eDTU  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers/elasticPools  |  eDTU_used  |  Uso eDTU  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers/elasticPools  |  log_write_percent  |  Percentuale I/O registro  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers/elasticPools  |  physical_data_read_percent  |  Percentuale di I/O di dati  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers/elasticPools  |  sessions_percent  |  Percentuale sessioni  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Sql/servers/elasticPools  |  storage_limit  |  Dimensioni massime dati  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.Sql/servers/elasticPools  |  storage_percent  |  Percentuale spazio dati utilizzato  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Sql/servers/elasticPools  |  storage_used  |  Spazio dati utilizzato  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.Sql/servers/elasticPools  |  tempdb_data_size  |  Kilobyte dimensioni file di dati tempdb  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Sql/servers/elasticPools  |  tempdb_log_size  |  Kilobyte dimensioni file di log tempdb  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.Sql/servers/elasticPools  |  tempdb_log_used_percent  |  Log percentuale tempdb utilizzato  |  Percentuale  |  Massimo | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers/elasticPools  |  workers_percent  |  Percentuale ruoli di lavoro  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Sql/servers/elasticPools  |  xtp_storage_percent  |  Percentuale di archiviazione OLTP in memoria  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts  |  Disponibilità  |  Disponibilità  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts  |  Egress  |  Egress  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts  |  Dati in ingresso  |  Dati in ingresso  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts  |  SuccessE2ELatency  |  Latenza end-to-end per richieste con esito positivo  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts  |  SuccessServerLatency  |  Latenza server per richieste con esito positivo  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts  |  Transazioni  |  Transazioni  |  Conteggio  |  Totale | 
| No  | No |  Microsoft.Storage/storageAccounts  |  UsedCapacity  |  Capacità usata  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/blobServices  |  Disponibilità  |  Disponibilità  |  Percentuale  |  Media | 
| No  | No |  Microsoft.Storage/storageAccounts/blobServices  |  BlobCapacity  |  Capacità BLOB  |  Byte  |  Media | 
| No  | No |  Microsoft.Storage/storageAccounts/blobServices  |  BlobCount  |  Numero di BLOB  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/blobServices  |  ContainerCount  |  Numero di contenitori BLOB  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/blobServices  |  Egress  |  Egress  |  Byte  |  Totale | 
| No  | No |  Microsoft.Storage/storageAccounts/blobServices  |  IndexCapacity  |  Capacità indice  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/blobServices  |  Dati in ingresso  |  Dati in ingresso  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/blobServices  |  SuccessE2ELatency  |  Latenza end-to-end per richieste con esito positivo  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/blobServices  |  SuccessServerLatency  |  Latenza server per richieste con esito positivo  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/blobServices  |  Transazioni  |  Transazioni  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/fileServices  |  Disponibilità  |  Disponibilità  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/fileServices  |  Egress  |  Egress  |  Byte  |  Totale | 
| No  | No |  Microsoft.Storage/storageAccounts/fileServices  |  FileCapacity  |  Capacità file  |  Byte  |  Media | 
| No  | No |  Microsoft.Storage/storageAccounts/fileServices  |  FileCount  |  Numero di file  |  Conteggio  |  Media | 
| No  | No |  Microsoft.Storage/storageAccounts/fileServices  |  FileShareCount  |  Numero di condivisione file  |  Conteggio  |  Media | 
| No  | No |  Microsoft.Storage/storageAccounts/fileServices  |  FileShareQuota  |  Dimensioni della quota di condivisione file  |  Byte  |  Media | 
| No  | No |  Microsoft.Storage/storageAccounts/fileServices  |  FileShareSnapshotCount  |  Conteggio degli snapshot di condivisione file  |  Conteggio  |  Media | 
| No  | No |  Microsoft.Storage/storageAccounts/fileServices  |  FileShareSnapshotSize  |  Dimensioni snapshot della condivisione file  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/fileServices  |  Dati in ingresso  |  Dati in ingresso  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/fileServices  |  SuccessE2ELatency  |  Latenza end-to-end per richieste con esito positivo  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/fileServices  |  SuccessServerLatency  |  Latenza server per richieste con esito positivo  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/fileServices  |  Transazioni  |  Transazioni  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/queueServices  |  Disponibilità  |  Disponibilità  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/queueServices  |  Egress  |  Egress  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/queueServices  |  Dati in ingresso  |  Dati in ingresso  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/queueServices  |  QueueCapacity  |  Capacità coda  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/queueServices  |  QueueCount  |  Numero di coda  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/queueServices  |  QueueMessageCount  |  Numero di messaggi in coda  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/queueServices  |  SuccessE2ELatency  |  Latenza end-to-end per richieste con esito positivo  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/queueServices  |  SuccessServerLatency  |  Latenza server per richieste con esito positivo  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/queueServices  |  Transazioni  |  Transazioni  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/tableServices  |  Disponibilità  |  Disponibilità  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/tableServices  |  Egress  |  Egress  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/tableServices  |  Dati in ingresso  |  Dati in ingresso  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/tableServices  |  SuccessE2ELatency  |  Latenza end-to-end per richieste con esito positivo  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/tableServices  |  SuccessServerLatency  |  Latenza server per richieste con esito positivo  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/tableServices  |  TableCapacity  |  Capacità tabella  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/tableServices  |  TableCount  |  Numero di tabella  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/tableServices  |  TableEntityCount  |  Numero di entità di tabella  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft.Storage/storageAccounts/tableServices  |  Transazioni  |  Transazioni  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. StorageCache/Caches  |  ClientIOPS  |  Totale IOPS client  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. StorageCache/Caches  |  ClientLatency  |  Latenza media del client  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft. StorageCache/Caches  |  ClientLockIOPS  |  IOPS blocco client  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft. StorageCache/Caches  |  ClientMetadataReadIOPS  |  IOPS lettura metadati client  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft. StorageCache/Caches  |  ClientMetadataWriteIOPS  |  IOPS di scrittura dei metadati client  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft. StorageCache/Caches  |  ClientReadIOPS  |  IOPS lettura client  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft. StorageCache/Caches  |  ClientReadThroughput  |  Velocità effettiva di lettura cache media  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft. StorageCache/Caches  |  ClientWriteIOPS  |  IOPS scrittura client  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft. StorageCache/Caches  |  ClientWriteThroughput  |  Velocità effettiva media scrittura cache  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft. StorageCache/Caches  |  StorageTargetAsyncWriteThroughput  |  Velocità effettiva di scrittura asincrona StorageTarget  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft. StorageCache/Caches  |  StorageTargetFillThroughput  |  Velocità effettiva riempimento StorageTarget  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft. StorageCache/Caches  |  StorageTargetHealth  |  Stato destinazione archiviazione  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. StorageCache/Caches  |  StorageTargetIOPS  |  IOPS StorageTarget totali  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. StorageCache/Caches  |  StorageTargetLatency  |  Latenza StorageTarget  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft. StorageCache/Caches  |  StorageTargetMetadataReadIOPS  |  IOPS lettura metadati StorageTarget  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft. StorageCache/Caches  |  StorageTargetMetadataWriteIOPS  |  IOPS scrittura metadati StorageTarget  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft. StorageCache/Caches  |  StorageTargetReadAheadThroughput  |  Velocità effettiva read-ahead StorageTarget  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft. StorageCache/Caches  |  StorageTargetReadIOPS  |  IOPS lettura StorageTarget  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft. StorageCache/Caches  |  StorageTargetSyncWriteThroughput  |  Velocità effettiva di scrittura sincrona StorageTarget  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft. StorageCache/Caches  |  StorageTargetTotalReadThroughput  |  StorageTarget totale velocità effettiva di lettura  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft. StorageCache/Caches  |  StorageTargetTotalWriteThroughput  |  Velocità effettiva di scrittura totale StorageTarget  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft. StorageCache/Caches  |  StorageTargetWriteIOPS  |  IOPS scrittura StorageTarget  |  Conteggio  |  Media | 
| **Sì**  | No |  Microsoft. StorageCache/Caches  |  Tempo di attività  |  Tempo di attività  |  Conteggio  |  Media | 
| **Sì**  | No |  microsoft.storagesync/storageSyncServices  |  ServerSyncSessionResult  |  Risultato della sessione di sincronizzazione  |  Conteggio  |  Media | 
| **Sì**  | No |  microsoft.storagesync/storageSyncServices  |  StorageSyncBatchTransferredFileBytes  |  Byte sincronizzati  |  Byte  |  Totale | 
| **Sì**  | No |  microsoft.storagesync/storageSyncServices  |  StorageSyncRecalledNetworkBytesByApplication  |  Dimensioni del richiamo di suddivisione in livelli nel cloud per applicazione  |  Byte  |  Totale | 
| **Sì**  | No |  microsoft.storagesync/storageSyncServices  |  StorageSyncRecalledTotalNetworkBytes  |  Dimensioni del richiamo di suddivisione in livelli nel cloud  |  Byte  |  Totale | 
| **Sì**  | No |  microsoft.storagesync/storageSyncServices  |  StorageSyncRecallIOTotalSizeBytes  |  Richiamo cloud a livelli  |  Byte  |  Totale | 
| **Sì**  | No |  microsoft.storagesync/storageSyncServices  |  StorageSyncRecallThroughputBytesPerSecond  |  Velocità effettiva di richiamo di suddivisione in livelli nel cloud  |  Byte al secondo  |  Media | 
| **Sì**  | No |  microsoft.storagesync/storageSyncServices  |  StorageSyncServerHeartbeat  |  Stato online del server  |  Conteggio  |  Massimo | 
| **Sì**  | No |  microsoft.storagesync/storageSyncServices  |  StorageSyncSyncSessionAppliedFilesCount  |  File sincronizzati  |  Conteggio  |  Totale | 
| **Sì**  | No |  microsoft.storagesync/storageSyncServices  |  StorageSyncSyncSessionPerItemErrorsCount  |  File non sincronizzati  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. StorageSync/storageSyncServices/registeredServers  |  ServerHeartbeat  |  Stato online del server  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft. StorageSync/storageSyncServices/registeredServers  |  ServerRecallIOTotalSizeBytes  |  Richiamo cloud a livelli  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft. StorageSync/storageSyncServices/syncGroups  |  SyncGroupBatchTransferredFileBytes  |  Byte sincronizzati  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft. StorageSync/storageSyncServices/syncGroups  |  SyncGroupSyncSessionAppliedFilesCount  |  File sincronizzati  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. StorageSync/storageSyncServices/syncGroups  |  SyncGroupSyncSessionPerItemErrorsCount  |  File non sincronizzati  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. StorageSync/storageSyncServices/syncGroups/serverEndpoints  |  ServerEndpointBatchTransferredFileBytes  |  Byte sincronizzati  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft. StorageSync/storageSyncServices/syncGroups/serverEndpoints  |  ServerEndpointSyncSessionAppliedFilesCount  |  File sincronizzati  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft. StorageSync/storageSyncServices/syncGroups/serverEndpoints  |  ServerEndpointSyncSessionPerItemErrorsCount  |  File non sincronizzati  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  AMLCalloutFailedRequests  |  Richieste di funzioni non riuscite  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  AMLCalloutInputEvents  |  Eventi di funzioni  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  AMLCalloutRequests  |  Richieste di funzioni  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  ConversionErrors  |  Errori di conversione dati  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  DeserializationError  |  Errori di deserializzazione dell'input  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  DroppedOrAdjustedEvents  |  Eventi non in ordine  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  EarlyInputEvents  |  Eventi di input anticipati  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  Errori  |  Errori di runtime  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  InputEventBytes  |  Byte evento di input  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  InputEvents  |  Eventi di input  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  InputEventsSourcesBacklogged  |  Eventi di input con backlog  |  Conteggio  |  Massimo | 
| **Sì**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  InputEventsSourcesPerSecond  |  Origini di input ricevute  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  LateInputEvents  |  Ultimi eventi di input  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  OutputEvents  |  Eventi di output  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  OutputWatermarkDelaySeconds  |  Ritardo limite  |  Secondi  |  Massimo | 
| **Sì**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  ResourceUtilization  |  % utilizzo unità di streaming  |  Percentuale  |  Massimo | 
| **Sì**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  Byte letti da disco  |  Byte letti da disco  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  Operazioni lettura disco/sec  |  Operazioni lettura disco/sec  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  Byte scritti su disco  |  Byte scritti su disco  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  Operazioni scrittura disco/sec  |  Operazioni scrittura disco/sec  |  Conteggio al secondo  |  Media | 
| **Sì**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  DiskReadBytesPerSecond  |  Disk Read Bytes/Sec  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  DiskReadLatency  |  Latenza di lettura da disco  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  DiskReadOperations  |  Operazioni di lettura su disco  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  DiskWriteBytesPerSecond  |  Disk Write Bytes/Sec  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  DiskWriteLatency  |  Latenza di scrittura su disco  |  Millisecondi  |  Media | 
| **Sì**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  DiskWriteOperations  |  Operazioni di scrittura su disco  |  Conteggio  |  Totale | 
| **Sì**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  MemoryActive  |  Memoria attiva  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  MemoryGranted  |  Memoria concessa  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  MemoryUsed  |  Memoria utilizzata  |  Byte  |  Media | 
| **Sì**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  Rete in ingresso  |  Rete in ingresso  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  Rete in uscita  |  Rete in uscita  |  Byte  |  Totale | 
| **Sì**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  NetworkInBytesPerSecond  |  Rete in byte/sec  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  NetworkOutBytesPerSecond  |  Byte di rete in uscita/sec  |  Byte al secondo  |  Media | 
| **Sì**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  CPU percentuale  |  CPU percentuale  |  Percentuale  |  Media | 
| **Sì**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  PercentageCpuReady  |  Percentuale di disponibilità CPU  |  Millisecondi  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  ActiveRequests  |  Richieste attive  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  AverageResponseTime  |  Tempo medio di risposta  |  Secondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  BytesReceived  |  Dati in entrata  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  BytesSent  |  Dati in uscita  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  CpuPercentage  |  Percentuale CPU  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  DiskQueueLength  |  Lunghezza coda del disco  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http101  |  Http 101  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http2xx  |  Http 2xx  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http3xx  |  Http 3xx  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http401  |  Http 401  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http403  |  Http 403  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http404  |  Http 404  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http406  |  Http 406  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http4xx  |  Http 4xx  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http5xx  |  Errori server HTTP  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  HttpQueueLength  |  Lunghezza coda HTTP  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  LargeAppServicePlanInstances  |  Ruoli di lavoro piano di servizio app Large  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  MediumAppServicePlanInstances  |  Ruoli di lavoro piano di servizio app Medium  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  MemoryPercentage  |  Percentuale memoria  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Richieste  |  Richieste  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  SmallAppServicePlanInstances  |  Ruoli di lavoro piano di servizio app Small  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  TotalFrontEnds  |  Front end totali  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/workerPools  |  CpuPercentage  |  Percentuale CPU  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/workerPools  |  MemoryPercentage  |  Percentuale memoria  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/workerPools  |  WorkersAvailable  |  Ruoli di lavoro disponibili  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/workerPools  |  WorkersTotal  |  Ruoli di lavoro totali  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/hostingEnvironments/workerPools  |  WorkersUsed  |  Ruoli di lavoro usati  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/serverfarms  |  BytesReceived  |  Dati in entrata  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/serverfarms  |  BytesSent  |  Dati in uscita  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/serverfarms  |  CpuPercentage  |  Percentuale CPU  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/serverfarms  |  DiskQueueLength  |  Lunghezza coda del disco  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/serverfarms  |  HttpQueueLength  |  Lunghezza coda HTTP  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/serverfarms  |  MemoryPercentage  |  Percentuale memoria  |  Percentuale  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/serverfarms  |  TcpCloseWait  |  Attesa chiusura TCP  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/serverfarms  |  TcpClosing  |  Chiusura TCP  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/serverfarms  |  TcpEstablished  |  TCP stabilito  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/serverfarms  |  TcpFinWait1  |  Attesa pinna TCP 1  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/serverfarms  |  TcpFinWait2  |  Attesa pinna TCP 2  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/serverfarms  |  TcpLastAck  |  Ultimo ACK TCP  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/serverfarms  |  TcpSynReceived  |  TCP SYN ricevuto  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/serverfarms  |  TcpSynSent  |  TCP SYN inviato  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/serverfarms  |  TcpTimeWait  |  Tempo di attesa TCP  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  AppConnections  |  Connessioni  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  AverageMemoryWorkingSet  |  Working set della memoria medio  |  Byte  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  AverageResponseTime  |  Tempo medio di risposta  |  Secondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  BytesReceived  |  Dati in entrata  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  BytesSent  |  Dati in uscita  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  CpuTime  |  Tempo CPU  |  Secondi  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  CurrentAssemblies  |  Assembly correnti  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  FunctionExecutionCount  |  Conteggio delle esecuzioni della funzione  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  FunctionExecutionUnits  |  Unità di esecuzione della funzione  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  Gen0Collections  |  Garbage Collection di generazione 0  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  Gen1Collections  |  Garbage Collection di generazione 1  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  Gen2Collections  |  Garbage Collection di generazione 2  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  Selettori  |  Numero degli handle  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  HealthCheckStatus  |  Stato controllo integrità  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  Http101  |  Http 101  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  Http2xx  |  Http 2xx  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  Http3xx  |  Http 3xx  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  Http401  |  Http 401  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  Http403  |  Http 403  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  Http404  |  Http 404  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  Http406  |  Http 406  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  Http4xx  |  Http 4xx  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  Http5xx  |  Errori server HTTP  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  HttpResponseTime  |  Tempo di risposta  |  Secondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  IoOtherBytesPerSecond  |  I/O - Altri byte al secondo  |  Byte al secondo  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  IoOtherOperationsPerSecond  |  I/O - Altre operazioni al secondo  |  Byte al secondo  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  IoReadBytesPerSecond  |  I/O - Byte in lettura al secondo  |  Byte al secondo  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  IoReadOperationsPerSecond  |  I/O - Operazioni di lettura al secondo  |  Byte al secondo  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  IoWriteBytesPerSecond  |  I/O - Byte in scrittura al secondo  |  Byte al secondo  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  IoWriteOperationsPerSecond  |  I/O - Operazioni di scrittura al secondo  |  Byte al secondo  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  MemoryWorkingSet  |  Working set della memoria  |  Byte  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  PrivateBytes  |  Byte privati  |  Byte  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  Richieste  |  Richieste  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  RequestsInApplicationQueue  |  Richieste nella coda dell'applicazione  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  Thread  |  Numero dei thread  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  TotalAppDomains  |  Totale di domini app  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites  |  TotalAppDomainsUnloaded  |  Totale di domini app scaricati  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  AppConnections  |  Connessioni  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  AverageMemoryWorkingSet  |  Working set della memoria medio  |  Byte  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  AverageResponseTime  |  Tempo medio di risposta  |  Secondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  BytesReceived  |  Dati in entrata  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  BytesSent  |  Dati in uscita  |  Byte  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  CpuTime  |  Tempo CPU  |  Secondi  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  CurrentAssemblies  |  Assembly correnti  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  FunctionExecutionCount  |  Conteggio delle esecuzioni della funzione  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  FunctionExecutionUnits  |  Unità di esecuzione della funzione  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  Gen0Collections  |  Garbage Collection di generazione 0  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  Gen1Collections  |  Garbage Collection di generazione 1  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  Gen2Collections  |  Garbage Collection di generazione 2  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  Selettori  |  Numero degli handle  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  HealthCheckStatus  |  Stato controllo integrità  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  Http101  |  Http 101  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  Http2xx  |  Http 2xx  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  Http3xx  |  Http 3xx  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  Http401  |  Http 401  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  Http403  |  Http 403  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  Http404  |  Http 404  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  Http406  |  Http 406  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  Http4xx  |  Http 4xx  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  Http5xx  |  Errori server HTTP  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  HttpResponseTime  |  Tempo di risposta  |  Secondi  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  IoOtherBytesPerSecond  |  I/O - Altri byte al secondo  |  Byte al secondo  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  IoOtherOperationsPerSecond  |  I/O - Altre operazioni al secondo  |  Byte al secondo  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  IoReadBytesPerSecond  |  I/O - Byte in lettura al secondo  |  Byte al secondo  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  IoReadOperationsPerSecond  |  I/O - Operazioni di lettura al secondo  |  Byte al secondo  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  IoWriteBytesPerSecond  |  I/O - Byte in scrittura al secondo  |  Byte al secondo  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  IoWriteOperationsPerSecond  |  I/O - Operazioni di scrittura al secondo  |  Byte al secondo  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  MemoryWorkingSet  |  Working set della memoria  |  Byte  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  PrivateBytes  |  Byte privati  |  Byte  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  Richieste  |  Richieste  |  Conteggio  |  Totale | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  RequestsInApplicationQueue  |  Richieste nella coda dell'applicazione  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  Thread  |  Numero dei thread  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  TotalAppDomains  |  Totale di domini app  |  Conteggio  |  Media | 
| **Sì**  | **Sì** |  Microsoft.Web/sites/slots  |  TotalAppDomainsUnloaded  |  Totale di domini app scaricati  |  Conteggio  |  Media | 
