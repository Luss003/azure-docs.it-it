---
title: Servizi e schemi supportati per i log di Diagnostica di Azure
description: Informazioni sullo schema di eventi e i servizi per i log di Diagnostica di Azure.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 10/11/2018
ms.author: robb
ms.subservice: logs
ms.openlocfilehash: fdcfcbaf99d48a345d2be4da297be1c9139da15c
ms.sourcegitcommit: 0486aba120c284157dfebbdaf6e23e038c8a5a15
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/26/2019
ms.locfileid: "71308124"
---
# <a name="supported-services-schemas-and-categories-for-azure-diagnostic-logs"></a>Servizi, schemi e categorie supportati per i log di Diagnostica di Azure

I [log di diagnostica di Monitoraggio di Azure](../../azure-monitor/platform/resource-logs-overview.md) vengono generati dai servizi di Azure e descrivono il funzionamento di tale risorse o servizi. Tutti i log di diagnostica disponibili tramite Monitoraggio di Azure condividono uno schema di primo livello comune, con la flessibilità necessaria affinché ogni servizio crei proprietà univoche per i propri eventi.

Una combinazione del tipo di risorsa (disponibile nella proprietà `resourceId`) e del `category` identifica in modo univoco uno schema. Questo articolo descrive lo schema di primo livello per i log di diagnostica e i collegamenti agli schemi per ogni servizio.

## <a name="top-level-diagnostic-logs-schema"></a>Schema dei log di diagnostica di primo livello

| Attività | Obbligatorio/Facoltativo | Descrizione |
|---|---|---|
| time | Richiesto | Il timestamp dell’evento (fuso UTC). |
| resourceId | Richiesto | ID della risorsa che ha emesso l’evento. Per i servizi di tenant, questo ha la forma /tenants/tenant-id/providers/provider-name. |
| tenantId | Obbligatorio per i log di tenant | L'ID tenant del tenant di Active Directory associato a questo evento. Questa proprietà viene utilizzata solo per i log a livello di tenant, non viene visualizzata nei log a livello di risorsa. |
| operationName | Richiesto | Il nome dell'operazione rappresentata da questo evento. Se l'evento rappresenta un'operazione RBAC, si tratta del nome di operazione RBAC (ad es. Microsoft.Storage/storageAccounts/blobServices/blobs/Read). Tipicamente modellate sotto forma di operazione di Resource Manager, anche se non sono effettivamente operazioni documentate di Resource Manager (`Microsoft.<providerName>/<resourceType>/<subtype>/<Write/Read/Delete/Action>`) |
| operationVersion | Facoltativi | La versione api associata all'operazione, se operationName è stato eseguito utilizzando un'API (ad es. `http://myservice.windowsazure.net/object?api-version=2016-06-01`). Se non esiste un'API corrispondente a questa operazione, la versione rappresenta la versione di tale operazione nel caso in cui le proprietà associate all'operazione cambino in futuro. |
| category | Richiesto | La categoria di log dell'evento. La categoria è la granularità con cui è possibile abilitare o disabilitare i log di una particolare risorsa. Le proprietà che appaiono all'interno del BLOB delle proprietà di un evento sono le stesse all'interno di una particolare categoria di log e tipo di risorsa. Tipiche categorie di log sono "Controllo" "Operativo" "Esecuzione" e "Richiesta". |
| resultType | Facoltativi | Lo stato dell'evento. I valori tipici includono: Started, In Progress, Succeeded, Failed, Active e Resolved. |
| resultSignature | Facoltativi | Lo stato secondario dell'evento. Se questa operazione corrisponde a una chiamata API REST, questo è il codice di stato HTTP della chiamata REST corrispondente. |
| resultDescription | Facoltativi | Il testo statico che descrive questa operazione, ad es. "Recupera file di archiviazione". |
| durationMs | Facoltativi | La durata dell'operazione in millisecondi. |
| callerIpAddress | Facoltativi | L'indirizzo IP del chiamante, se l'operazione corrisponde a una chiamata API proveniente da un'entità con un indirizzo IP accessibile al pubblico. |
| correlationId | Facoltativi | Un GUID utilizzato per raggruppare un set di eventi correlati. In genere, se due eventi hanno lo stesso operationName ma due diversi stati (ad es. "Started" e "Succeeded"), condividono lo stesso ID di correlazione. Ciò può anche rappresentare altre relazioni tra gli eventi. |
| identity | Facoltativi | Un blob JSON che descrive l'identità dell'utente o dell'applicazione che ha eseguito l'operazione. In genere includerà l'autorizzazione e le attestazioni / token JWT da Active Directory. |
| Livello | Facoltativi | Il livello di gravità dell'evento. Deve essere di tipo Informativo, Avviso, Errore o Critico. |
| location | Facoltativi | L'area della risorsa che emette l'evento, ad es. "Stati Uniti orientali" o "Francia meridionale" |
| proprietà | Facoltativi | Eventuali proprietà estese relative a questa particolare categoria di eventi. Tutte le proprietà personali/uniche devono essere inserite all'interno di questa "Parte B" dello schema. |

## <a name="service-specific-schemas-for-resource-diagnostic-logs"></a>Schemi specifici per i servizi per i log di diagnostica delle risorse
Lo schema per i log di diagnostica di risorsa varia a seconda della risorsa e della categoria di log. Questo elenco mostra tutti i servizi che rendono disponibili i log di diagnostica e i collegamenti al servizio e allo schema specifico per categoria in cui è disponibile.

| Service | Schema e documenti |
| --- | --- |
| Azure Active Directory | [Panoramica](../../active-directory/reports-monitoring/concept-activity-logs-azure-monitor.md), schema del [Registro di controllo](../../active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema.md) e [schema degli accessi](../../active-directory/reports-monitoring/reference-azure-monitor-sign-ins-log-schema.md) |
| Analysis Services | https://azure.microsoft.com/blog/azure-analysis-services-integration-with-azure-diagnostic-logs/ |
| Gestione API | [Log di diagnostica di Gestione API](../../api-management/api-management-howto-use-azure-monitor.md#diagnostic-logs) |
| Gateway applicazione |[Registrazione diagnostica per il gateway applicazione](../../application-gateway/application-gateway-diagnostics.md) |
| Automazione di Azure |[Analisi dei log per Automazione di Azure](../../automation/automation-manage-send-joblogs-log-analytics.md) |
| Azure Batch |[Registrazione diagnostica di Azure Batch](../../batch/batch-diagnostics.md) |
| Database di Azure per MySQL | [Log di diagnostica di database di Azure per MySQL](../../mysql/concepts-server-logs.md#diagnostic-logs) |
| Database di Azure per PostgreSQL | [Log di diagnostica di database di Azure per PostgreSQL](../../postgresql/concepts-server-logs.md#diagnostic-logs) |
| Servizi cognitivi | [Registrazione diagnostica per servizi cognitivi di Azure](../../cognitive-services/diagnostic-logging.md) |
| Rete per la distribuzione di contenuti (CDN) | [Log di diagnostica di Azure per la rete CDN](../../cdn/cdn-azure-diagnostic-logs.md) |
| CosmosDB | [Registrazione di Azure Cosmos DB](../../cosmos-db/logging.md) |
| Data factory | [Monitorare le data factory con Monitoraggio di Azure](../../data-factory/monitor-using-azure-monitor.md) |
| Data Lake Analytics |[Accesso ai log di diagnostica per Azure Data Lake Analytics](../../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Data Lake Store |[Accesso ai log di diagnostica per Archivio Data Lake di Azure](../../data-lake-store/data-lake-store-diagnostic-logs.md) |
| Hub eventi |[Log di diagnostica di Hub eventi in Azure](../../event-hubs/event-hubs-diagnostic-logs.md) |
| ExpressRoute | Lo schema non è disponibile. |
| Firewall di Azure | Lo schema non è disponibile. |
| Hub IoT | [Operazioni dell'hub IoT](../../iot-hub/iot-hub-monitor-resource-health.md#use-azure-monitor) |
| Key Vault |[Registrazione dell'insieme di credenziali delle chiavi di Azure](../../key-vault/key-vault-logging.md) |
| Servizio Kubernetes |[Registrazione di Azure Kubernetes](../../aks/view-master-logs.md#log-event-schema) |
| Bilanciamento del carico |[Analisi dei log per il servizio di bilanciamento del carico di Azure](../../load-balancer/load-balancer-monitor-log.md) |
| App per la logica |[Schema di rilevamento personalizzato per le app per la logica B2B](../../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| Gruppi di sicurezza di rete |[Analisi dei log per i gruppi di sicurezza di rete](../../virtual-network/virtual-network-nsg-manage-log.md) |
| Protezione DDoS | [Gestire la protezione DDoS di Azure Standard](../../virtual-network/manage-ddos-protection.md) |
| Power BI dedicato | [Registrazione diagnostica per Power BI Embedded in Azure](https://docs.microsoft.com/power-bi/developer/azure-pbie-diag-logs) |
| Servizi di ripristino | [Modello di dati per Backup di Microsoft Azure](../../backup/backup-azure-reports-data-model.md)|
| Cerca |[Abilitazione e uso di Analisi del traffico di ricerca](../../search/search-traffic-analytics.md) |
| Bus di servizio |[Log di diagnostica del bus di servizio di Azure](../../service-bus-messaging/service-bus-diagnostic-logs.md) |
| Database SQL | [Registrazione diagnostica del database SQL di Azure](../../sql-database/sql-database-metrics-diag-logging.md) |
| Analisi di flusso |[Log di diagnostica dei processi](../../stream-analytics/stream-analytics-job-diagnostic-logs.md) |
| Gestione traffico | [Schema dei log di Gestione traffico](../../traffic-manager/traffic-manager-diagnostic-logs.md) |
| Reti virtuali | Lo schema non è disponibile. |
| Gateway di rete virtuale | Lo schema non è disponibile. |

## <a name="supported-log-categories-per-resource-type"></a>Categorie di log supportate per tipo di risorsa
|Tipo di risorsa|Category|Nome visualizzato della categoria|
|---|---|---|
|Microsoft.AnalysisServices/servers|Motore|Motore|
|Microsoft.AnalysisServices/servers|Service|Service|
|Microsoft.ApiManagement/service|GatewayLogs|Log correlati ad ApiManagement Gateway|
|Microsoft.Automation/automationAccounts|JobLogs|Log del processo|
|Microsoft.Automation/automationAccounts|JobStreams|Flussi del processo|
|Microsoft.Automation/automationAccounts|DscNodeStatus|Stato del nodo Dsc|
|Microsoft.Batch/batchAccounts|ServiceLog|Log del servizio|
|Microsoft.Cdn/profiles/endpoints|CoreAnalytics|Ottiene le metriche dell'endpoint, ad esempio la larghezza di banda, i dati in uscita e così via|
|Microsoft.ClassicNetwork/networksecuritygroups|Evento del flusso di regole del gruppo di sicurezza di rete|Evento del flusso di regole del gruppo di sicurezza di rete|
|Microsoft.CognitiveServices/accounts|Controllo|Log di controllo|
|Microsoft.CognitiveServices/accounts|RequestResponse|Log richieste e risposte|
|Microsoft.ContainerService/managedClusters|kube-apiserver|Server API Kubernetes|
|Microsoft.ContainerService/managedClusters|kube-controller-manager|Gestione controller Kubernetes|
|Microsoft.ContainerService/managedClusters|cluster-autoscaler|Scalabilità automatica del cluster Kubernetes|
|Microsoft.ContainerService/managedClusters|kube-scheduler|Utilità di pianificazione Kubernetes|
|Microsoft.ContainerService/managedClusters|guard|Webhook di autenticazione|
|Microsoft.CustomerInsights/hubs|AuditEvents|Eventi di controllo|
|Microsoft.DataFactory/factories|ActivityRuns|Log delle esecuzioni di attività pipeline|
|Microsoft.DataFactory/factories|PipelineRuns|Log delle esecuzioni di pipeline|
|Microsoft.DataFactory/factories|TriggerRuns|Log delle esecuzioni trigger|
|Microsoft.DataLakeAnalytics/accounts|Controllo|Log di controllo|
|Microsoft.DataLakeAnalytics/accounts|Requests|Log delle richieste|
|Microsoft.DataLakeStore/accounts|Controllo|Log di controllo|
|Microsoft.DataLakeStore/accounts|Requests|Log delle richieste|
|Microsoft.DBforMySQL/servers|MySqlSlowLogs|Log server MySQL|
|Microsoft.DBforPostgreSQL/servers|PostgreSQLLogs|Log del server PostgreSQL|
|Microsoft.Devices/IotHubs|Connessioni|Connessioni|
|Microsoft.Devices/IotHubs|DeviceTelemetry|Telemetria dei dispositivi|
|Microsoft.Devices/IotHubs|C2DCommands|Comandi da cloud a dispositivo|
|Microsoft.Devices/IotHubs|DeviceIdentityOperations|Operazioni relative alle identità dei dispositivi|
|Microsoft.Devices/IotHubs|FileUploadOperations|Operazioni di caricamento file|
|Microsoft.Devices/IotHubs|Route|Route|
|Microsoft.Devices/IotHubs|D2CTwinOperations|D2CTwinOperations|
|Microsoft.Devices/IotHubs|C2DTwinOperations|Operazioni da cloud a dispositivi gemelli|
|Microsoft.Devices/IotHubs|TwinQueries|Query dei dispositivi gemelli|
|Microsoft.Devices/IotHubs|JobsOperations|Operazioni dei processi|
|Microsoft.Devices/IotHubs|DirectMethods|Metodi diretti|
|Microsoft.Devices/IotHubs|E2EDiagnostics|E2E Diagnostics (anteprima)|
|Microsoft.Devices/IotHubs|Configurazioni|Configurazioni|
|Microsoft.Devices/provisioningServices|DeviceOperations|Operazioni del dispositivo|
|Microsoft.Devices/provisioningServices|ServiceOperations|Operazioni di servizio|
|Microsoft.DocumentDB/databaseAccounts|DataPlaneRequests|DataPlaneRequests|
|Microsoft.DocumentDB/databaseAccounts|MongoRequests|MongoRequests|
|Microsoft.DocumentDB/databaseAccounts|QueryRuntimeStatistics|QueryRuntimeStatistics|
|Microsoft.EventHub/namespaces|ArchiveLogs|Log di archiviazione|
|Microsoft.EventHub/namespaces|OperationalLogs|Log operativi|
|Microsoft.EventHub/namespaces|AutoScaleLogs|Log di scalabilità automatica|
|Microsoft.Insights/AutoscaleSettings|AutoscaleEvaluations|Valutazioni sulla scalabilità automatica|
|Microsoft.Insights/AutoscaleSettings|AutoscaleScaleActions|Azioni di ridimensionamento per la scalabilità automatica|
|Microsoft.IoTSpaces/Graph|Trace|Trace|
|Microsoft.IoTSpaces/Graph|Operativo|Operativo|
|Microsoft.IoTSpaces/Graph|Controllo|Controllo|
|Microsoft.IoTSpaces/Graph|UserDefinedFunction|UserDefinedFunction|
|Microsoft.IoTSpaces/Graph|Ingress|Ingress|
|Microsoft.IoTSpaces/Graph|Egress|Egress|
|Microsoft.KeyVault/vaults|AuditEvent|Log di controllo|
|Microsoft.Logic/workflows|WorkflowRuntime|Eventi di diagnostica del runtime del flusso di lavoro|
|Microsoft.Logic/integrationAccounts|IntegrationAccountTrackingEvents|Eventi di rilevamento degli account di integrazione|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Event del gruppo di sicurezza di rete|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Contatore di regole del gruppo di sicurezza di rete|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|Eventi di avviso del servizio di bilanciamento del carico|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|Stato di integrità dei probe del servizio di bilanciamento del carico|
|Microsoft.Network/publicIPAddresses|DDoSProtectionNotifications|Notifiche di protezione DDoS|
|Microsoft.Network/publicIPAddresses|DDoSMitigationFlowLogs|Flusso di log di decisioni di mitigazione DDoS|
|Microsoft.Network/publicIPAddresses|DDoSMitigationReports|Report soluzioni di prevenzione DDoS|
|Microsoft.Network/virtualNetworks|VMProtectionAlerts|Avvisi di protezione VM|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Log di accesso del gateway applicazione|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Log delle prestazioni del gateway applicazione|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|Log del firewall del gateway applicazione|
|Microsoft.Network/securegateways|AzureFirewallApplicationRule|Regola di applicazione di Firewall di Azure|
|Microsoft.Network/securegateways|AzureFirewallNetworkRule|Regola di rete di Firewall di Azure|
|Microsoft.Network/azurefirewalls|AzureFirewallApplicationRule|Regola di applicazione di Firewall di Azure|
|Microsoft.Network/azurefirewalls|AzureFirewallNetworkRule|Regola di rete di Firewall di Azure|
|Microsoft.Network/virtualNetworkGateways|GatewayDiagnosticLog|Log di diagnostica del gateway|
|Microsoft.Network/virtualNetworkGateways|TunnelDiagnosticLog|Log di diagnostica del tunnel|
|Microsoft.Network/virtualNetworkGateways|RouteDiagnosticLog|Log di diagnostica della route|
|Microsoft.Network/virtualNetworkGateways|IKEDiagnosticLog|Log di diagnostica IKE|
|Microsoft.Network/virtualNetworkGateways|P2SDiagnosticLog|Log di diagnostica P2S|
|Microsoft.Network/trafficManagerProfiles|ProbeHealthStatusEvents|Evento dei risultati di integrità dei probe di Traffic Manager|
|Microsoft.Network/expressRouteCircuits|PeeringRouteLog|Log delle tabelle di routing di peering|
|Microsoft.Network/frontdoors|FrontdoorAccessLog|Log di accesso a Frontdoor|
|Microsoft.Network/frontdoors|FrontdoorWebApplicationFirewallLog|Log del Web Application Firewall di Frontdoor|
|Microsoft.PowerBIDedicated/capacities|Motore|Motore|
|Microsoft.RecoveryServices/Vaults|AzureBackupReport|Dati dei report di Backup di Azure|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryJobs|Processi di Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryEvents|Eventi di Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicatedItems|Elementi replicati di Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationStats|Statistiche di replica di Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryRecoveryPoints|Punti di ripristino di Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationDataUploadRate|Velocità di caricamento dei dati di replica di Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryProtectedDiskDataChurn|Varianza dei dati del disco protetti di Azure Site Recovery|
|Microsoft.Search/searchServices|OperationLogs|Log operazioni|
|Microsoft.ServiceBus/namespaces|OperationalLogs|Log operativi|
|Microsoft.Sql/servers/databases|SQLInsights|Informazioni dettagliate su SQL|
|Microsoft.Sql/servers/databases|AutomaticTuning|Ottimizzazione automatica|
|Microsoft.Sql/servers/databases|QueryStoreRuntimeStatistics|Statistiche di runtime di Query Store|
|Microsoft.Sql/servers/databases|QueryStoreWaitStatistics|Statistiche relative alle attese di Query Store|
|Microsoft.Sql/servers/databases|Errori|Errori|
|Microsoft.Sql/servers/databases|DatabaseWaitStatistics|Statistiche relative alle attese del database|
|Microsoft.Sql/servers/databases|Timeout|Timeout|
|Microsoft.Sql/servers/databases|Blocchi|Blocchi|
|Microsoft.Sql/servers/databases|Deadlock|Deadlock|
|Microsoft.Sql/servers/databases|Controllo|Log di controllo|
|Microsoft.Sql/servers/databases|SQLSecurityAuditEvents|Evento di controllo di sicurezza SQL|
|Microsoft.Sql/servers/databases|DmsWorkers|Ruoli di lavoro del servizio di Migrazione del database|
|Microsoft.Sql/servers/databases|ExecRequests|Richieste di esecuzione|
|Microsoft.Sql/servers/databases|RequestSteps|Procedura per la richiesta|
|Microsoft.Sql/servers/databases|SqlRequests|Richieste SQL|
|Microsoft.Sql/servers/databases|In attesa|In attesa|
|Microsoft.Sql/managedInstances|ResourceUsageStats|Statistiche di utilizzo delle risorse|
|Microsoft.Sql/managedInstances|SQLSecurityAuditEvents|Evento di controllo di sicurezza SQL|
|Microsoft.Sql/managedInstances/databases|SQLInsights|Informazioni dettagliate su SQL|
|Microsoft.Sql/managedInstances/databases|QueryStoreRuntimeStatistics|Statistiche di runtime di Query Store|
|Microsoft.Sql/managedInstances/databases|QueryStoreWaitStatistics|Statistiche relative alle attese di Query Store|
|Microsoft.Sql/managedInstances/databases|Errori|Errori|
|Microsoft.StreamAnalytics/streamingjobs|Esecuzione|Esecuzione|
|Microsoft.StreamAnalytics/streamingjobs|Creazione|Creazione|
|microsoft.web/sites|FunctionExecutionLogs|Log di esecuzione delle funzioni|
|microsoft.web/sites/slots|FunctionExecutionLogs|Log di esecuzione delle funzioni|

## <a name="next-steps"></a>Passaggi successivi

* [Altre informazioni sui log di diagnostica](../../azure-monitor/platform/resource-logs-overview.md)
* [Trasmettere log di diagnostica di Azure a **Hub eventi**](../../azure-monitor/platform/resource-logs-stream-event-hubs.md)
* [Modificare le impostazioni di diagnostica di risorsa usando l'API REST di Monitoraggio di Azure](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)
* [Analizzare i log di Archiviazione di Azure con Log Analytics](../../azure-monitor/platform/collect-azure-metrics-logs.md)
