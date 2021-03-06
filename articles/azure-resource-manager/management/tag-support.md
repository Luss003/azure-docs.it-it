---
title: Supporto dei tag per le risorse
description: Informazioni sui tipi di risorse di Azure che supportano i tag. Include informazioni dettagliate per tutti i servizi di Azure.
ms.topic: conceptual
ms.date: 02/26/2020
ms.openlocfilehash: 6100c667c7df0b3e1740777565d260af9fa818a3
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/27/2020
ms.locfileid: "77657574"
---
# <a name="tag-support-for-azure-resources"></a>Supporto dei tag per le risorse di Azure
Questo articolo descrive se un tipo di risorsa supporta [tag](tag-resources.md). La colonna con etichetta **supporta i tag** indica se il tipo di risorsa ha una proprietà per il tag. La colonna etichetta **nel report dei costi** indica se il tipo di risorsa passa il tag al report dei costi. È possibile visualizzare i costi in base ai tag nell' [analisi dei costi di gestione costi](../../cost-management-billing/costs/quick-acm-cost-analysis.md#understanding-grouping-and-filtering-options) e la [fattura e i dati di utilizzo giornalieri di Azure](../../cost-management-billing/manage/download-azure-invoice-daily-usage-date.md).

Per ottenere gli stessi dati come file con valori delimitati da virgole, scaricare [tag-support.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/tag-support.csv).

Passare a uno spazio dei nomi del provider di risorse:
> [!div class="op_single_selector"]
> - [Microsoft. AAD](#microsoftaad)
> - [Microsoft. addons](#microsoftaddons)
> - [Microsoft. ADHybridHealthService](#microsoftadhybridhealthservice)
> - [Microsoft. Advisor](#microsoftadvisor)
> - [Microsoft. AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft. AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Microsoft. AppConfiguration](#microsoftappconfiguration)
> - [Microsoft. AppPlatform](#microsoftappplatform)
> - [Microsoft. attestazione](#microsoftattestation)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft.Automation](#microsoftautomation)
> - [Microsoft. Azconfig](#microsoftazconfig)
> - [Microsoft. Azure. Geneva](#microsoftazuregeneva)
> - [Microsoft. AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft. AzureData](#microsoftazuredata)
> - [Microsoft. AzureStack](#microsoftazurestack)
> - [Microsoft. batch](#microsoftbatch)
> - [Microsoft. Billing](#microsoftbilling)
> - [Microsoft. BingMaps](#microsoftbingmaps)
> - [Microsoft. blockchain](#microsoftblockchain)
> - [Microsoft. Blueprint](#microsoftblueprint)
> - [Microsoft. BotService](#microsoftbotservice)
> - [Microsoft.Cache](#microsoftcache)
> - [Microsoft. Capacity](#microsoftcapacity)
> - [Microsoft.Cdn](#microsoftcdn)
> - [Microsoft. CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft. ClassicCompute](#microsoftclassiccompute)
> - [Microsoft. ClassicInfrastructureMigrate](#microsoftclassicinfrastructuremigrate)
> - [Microsoft. ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft. ClassicStorage](#microsoftclassicstorage)
> - [Microsoft. CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft. Commerce](#microsoftcommerce)
> - [Microsoft. Compute](#microsoftcompute)
> - [Microsoft. consumer](#microsoftconsumption)
> - [Microsoft. ContainerInstance](#microsoftcontainerinstance)
> - [Microsoft.ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft.ContainerService](#microsoftcontainerservice)
> - [Microsoft. CortanaAnalytics](#microsoftcortanaanalytics)
> - [Microsoft. CostManagement](#microsoftcostmanagement)
> - [Microsoft. CustomerLockbox](#microsoftcustomerlockbox)
> - [Microsoft. CustomProviders](#microsoftcustomproviders)
> - [Microsoft. DataBox](#microsoftdatabox)
> - [Microsoft. DataBoxEdge](#microsoftdataboxedge)
> - [Microsoft. databricks](#microsoftdatabricks)
> - [Microsoft. datacatalog](#microsoftdatacatalog)
> - [Microsoft.DataFactory](#microsoftdatafactory)
> - [Microsoft.DataLakeAnalytics](#microsoftdatalakeanalytics)
> - [Microsoft.DataLakeStore](#microsoftdatalakestore)
> - [Microsoft. DataMigration](#microsoftdatamigration)
> - [Microsoft. DataShare](#microsoftdatashare)
> - [Microsoft. DBforMariaDB](#microsoftdbformariadb)
> - [Microsoft. DBforMySQL](#microsoftdbformysql)
> - [Microsoft. DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft. DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft. DesktopVirtualization](#microsoftdesktopvirtualization)
> - [Microsoft.Devices](#microsoftdevices)
> - [Microsoft. DevOps](#microsoftdevops)
> - [Microsoft. DevSpaces](#microsoftdevspaces)
> - [Microsoft.DevTestLab](#microsoftdevtestlab)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft. DomainRegistration](#microsoftdomainregistration)
> - [Microsoft. DynamicsLcs](#microsoftdynamicslcs)
> - [Microsoft. EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft. EventGrid](#microsofteventgrid)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Microsoft. Features](#microsoftfeatures)
> - [Microsoft. Gallery](#microsoftgallery)
> - [Microsoft. genomica](#microsoftgenomics)
> - [Microsoft.GuestConfiguration](#microsoftguestconfiguration)
> - [Microsoft. HanaOnAzure](#microsofthanaonazure)
> - [Microsoft. HardwareSecurityModules](#microsofthardwaresecuritymodules)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [Microsoft. HealthcareApis](#microsofthealthcareapis)
> - [Microsoft.HybridCompute](#microsofthybridcompute)
> - [Microsoft. HybridData](#microsofthybriddata)
> - [Microsoft. Hydra](#microsofthydra)
> - [Microsoft. ImportExport](#microsoftimportexport)
> - [Microsoft. Intune](#microsoftintune)
> - [Microsoft. IoTCentral](#microsoftiotcentral)
> - [Microsoft. IoTSpaces](#microsoftiotspaces)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Microsoft. kusto](#microsoftkusto)
> - [Microsoft. LabServices](#microsoftlabservices)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft. MachineLearning](#microsoftmachinelearning)
> - [Microsoft. MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft. ManagedIdentity](#microsoftmanagedidentity)
> - [Microsoft. ManagedServices](#microsoftmanagedservices)
> - [Microsoft. Management](#microsoftmanagement)
> - [Microsoft. Maps](#microsoftmaps)
> - [Microsoft. Marketplace](#microsoftmarketplace)
> - [Microsoft. MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft. MarketplaceOrdering](#microsoftmarketplaceordering)
> - [Microsoft.Media](#microsoftmedia)
> - [Microsoft. Microservices4Spring](#microsoftmicroservices4spring)
> - [Microsoft. migrate](#microsoftmigrate)
> - [Microsoft. MixedReality](#microsoftmixedreality)
> - [Microsoft. NetApp](#microsoftnetapp)
> - [Microsoft. Network](#microsoftnetwork)
> - [Microsoft. Notebooks](#microsoftnotebooks)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft. ObjectStore](#microsoftobjectstore)
> - [Microsoft. OffAzure](#microsoftoffazure)
> - [Microsoft. OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft. OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft. peering](#microsoftpeering)
> - [Microsoft.PolicyInsights](#microsoftpolicyinsights)
> - [Microsoft. Portal](#microsoftportal)
> - [Microsoft. Power bi](#microsoftpowerbi)
> - [Microsoft. PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft. ProjectBabylon](#microsoftprojectbabylon)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft. Relay](#microsoftrelay)
> - [Microsoft. RemoteApp](#microsoftremoteapp)
> - [Microsoft. ResourceGraph](#microsoftresourcegraph)
> - [Microsoft. ResourceHealth](#microsoftresourcehealth)
> - [Microsoft.Resources](#microsoftresources)
> - [Microsoft. SaaS](#microsoftsaas)
> - [Microsoft.Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft. SecurityGraph](#microsoftsecuritygraph)
> - [Microsoft. SecurityInsights](#microsoftsecurityinsights)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft.ServiceFabric](#microsoftservicefabric)
> - [Microsoft. ServiceFabricMesh](#microsoftservicefabricmesh)
> - [Microsoft. Services](#microsoftservices)
> - [Microsoft. SignalRService](#microsoftsignalrservice)
> - [Microsoft. SiteRecovery](#microsoftsiterecovery)
> - [Microsoft. SoftwarePlan](#microsoftsoftwareplan)
> - [Microsoft. Solutions](#microsoftsolutions)
> - [Microsoft. SpoolService](#microsoftspoolservice)
> - [Microsoft. SQL](#microsoftsql)
> - [Microsoft. SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft. StorageCache](#microsoftstoragecache)
> - [Microsoft. StorageReplication](#microsoftstoragereplication)
> - [Microsoft. StorageSync](#microsoftstoragesync)
> - [Microsoft. StorageSyncDev](#microsoftstoragesyncdev)
> - [Microsoft. StorageSyncInt](#microsoftstoragesyncint)
> - [Microsoft. StorSimple](#microsoftstorsimple)
> - [Microsoft. StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft. Subscription](#microsoftsubscription)
> - [Microsoft. TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft. VMwareCloudSimple](#microsoftvmwarecloudsimple)
> - [Microsoft. VnfManager](#microsoftvnfmanager)
> - [Microsoft.Web](#microsoftweb)
> - [Microsoft. WindowsDefenderATP](#microsoftwindowsdefenderatp)
> - [Microsoft. WindowsIoT](#microsoftwindowsiot)
> - [Microsoft. WorkloadMonitor](#microsoftworkloadmonitor)

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | DomainServices | Sì | Sì |
> | DomainServices/oucontainer | No | No |

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | supportProviders | No | No |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | aadsupportcases | No | No |
> | addsservices | No | No |
> | agenti | No | No |
> | anonymousapiusers | No | No |
> | configurazione | No | No |
> | log | No | No |
> | report | No | No |
> | servicehealthmetrics | No | No |
> | servizi | No | No |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | configurazioni | No | No |
> | generateRecommendations | No | No |
> | metadata | No | No |
> | indicazioni | No | No |
> | suppressions | No | No |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | actionRules | Sì | Sì |
> | avvisi | No | No |
> | alertsList | No | No |
> | alertsMetaData | No | No |
> | alertsSummary | No | No |
> | alertsSummaryList | No | No |
> | commenti e suggerimenti | No | No |
> | smartDetectorAlertRules | Sì | Sì |
> | smartDetectorRuntimeEnvironments | No | No |
> | smartGroups | No | No |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | server | Sì | Sì |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | reportFeedback | No | No |
> | service | Sì | Sì |
> | validateServiceName | No | No |

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | configurationStores | Sì | Sì |
> | configurationStores / eventGridFilters | No | No |

## <a name="microsoftappplatform"></a>Microsoft. AppPlatform

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | Spring | Sì | Sì |

## <a name="microsoftattestation"></a>Microsoft.Attestation

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | attestationProviders | No | No |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | classicAdministrators | No | No |
> | dataalias | No | No |
> | denyAssignments | No | No |
> | elevateAccess | No | No |
> | findOrphanRoleAssignments | No | No |
> | blocchi | No | No |
> | autorizzazioni | No | No |
> | policyAssignments | No | No |
> | policyDefinitions | No | No |
> | policySetDefinitions | No | No |
> | providerOperations | No | No |
> | roleAssignments | No | No |
> | roleAssignmentsUsageMetrics | No | No |
> | roleDefinitions | No | No |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | automationAccounts | Sì | Sì |
> | automationAccounts/configurazioni | Sì | Sì |
> | automationAccounts/processi | No | No |
> | automationAccounts/privateEndpointConnectionProxies | No | No |
> | automationAccounts/privateEndpointConnections | No | No |
> | automationAccounts/privateLinkResources | No | No |
> | automationAccounts/manuali operativi | Sì | Sì |
> | automationAccounts/softwareUpdateConfigurations | No | No |
> | automationAccounts/webhook | No | No |

## <a name="microsoftazconfig"></a>Microsoft. Azconfig

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | configurationStores | Sì | Sì |
> | configurationStores / eventGridFilters | No | No |

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Geneva

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | ambienti | No | No |
> | ambienti/account | No | No |
> | ambienti/account/spazi dei nomi | No | No |
> | ambienti/account/spazi dei nomi/configurazioni | No | No |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | b2cDirectories | Sì | No |
> | b2ctenants | No | No |

## <a name="microsoftazuredata"></a>Microsoft. AzureData

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | hybridDataManagers | Sì | Sì |
> | postgresInstances | Sì | Sì |
> | sqlBigDataClusters | Sì | Sì |
> | SQLInstances | Sì | Sì |
> | sqlServerRegistrations | Sì | Sì |
> | sqlServerRegistrations/SQLServers | No | No |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | registrations | Sì | Sì |
> | registrazioni/customerSubscriptions | No | No |
> | registrazioni/prodotti | No | No |
> | verificationKeys | No | No |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | batchAccounts | Sì | Sì |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | billingAccounts | No | No |
> | billingAccounts/contratti | No | No |
> | billingAccounts / billingPermissions | No | No |
> | billingAccounts / billingProfiles | No | No |
> | billingAccounts / billingProfiles / billingPermissions | No | No |
> | billingAccounts / billingProfiles / billingRoleAssignments | No | No |
> | billingAccounts / billingProfiles / billingRoleDefinitions | No | No |
> | billingAccounts / billingProfiles / billingSubscriptions | No | No |
> | billingAccounts / billingProfiles / createBillingRoleAssignment | No | No |
> | billingAccounts/billingProfiles/clienti | No | No |
> | billingAccounts/billingProfiles/istruzioni | No | No |
> | billingAccounts/billingProfiles/fatture | No | No |
> | billingAccounts/billingProfiles/fatture/pricesheets | No | No |
> | billingAccounts/billingProfiles/fatture/transazioni | No | No |
> | billingAccounts / billingProfiles / invoiceSections | No | No |
> | billingAccounts / billingProfiles / invoiceSections / billingPermissions | No | No |
> | billingAccounts / billingProfiles / invoiceSections / billingRoleAssignments | No | No |
> | billingAccounts / billingProfiles / invoiceSections / billingRoleDefinitions | No | No |
> | billingAccounts / billingProfiles / invoiceSections / billingSubscriptions | No | No |
> | billingAccounts / billingProfiles / invoiceSections / createBillingRoleAssignment | No | No |
> | billingAccounts / billingProfiles / invoiceSections / initiateTransfer | No | No |
> | billingAccounts/billingProfiles/invoiceSections/Products | No | No |
> | billingAccounts/billingProfiles/invoiceSections/Products/Transfer | No | No |
> | billingAccounts/billingProfiles/invoiceSections/Products/updateAutoRenew | No | No |
> | billingAccounts/billingProfiles/invoiceSections/Transactions | No | No |
> | billingAccounts/billingProfiles/invoiceSections/trasferimenti | No | No |
> | billingAccounts / BillingProfiles / patchOperations | No | No |
> | billingAccounts / billingProfiles / paymentMethods | No | No |
> | billingAccounts/billingProfiles/criteri | No | No |
> | billingAccounts/billingProfiles/pricesheets | No | No |
> | billingAccounts / billingProfiles / pricesheetDownloadOperations | No | No |
> | billingAccounts/billingProfiles/Products | No | No |
> | billingAccounts/billingProfiles/Transactions | No | No |
> | billingAccounts / billingRoleAssignments | No | No |
> | billingAccounts / billingRoleDefinitions | No | No |
> | billingAccounts / billingSubscriptions | No | No |
> | billingAccounts/billingSubscriptions/fatture | No | No |
> | billingAccounts / createBillingRoleAssignment | No | No |
> | billingAccounts / createInvoiceSectionOperations | No | No |
> | billingAccounts/clienti | No | No |
> | billingAccounts/Customers/billingPermissions | No | No |
> | billingAccounts/Customers/billingSubscriptions | No | No |
> | billingAccounts/Customers/initiateTransfer | No | No |
> | billingAccounts/clienti/criteri | No | No |
> | billingAccounts/clienti/prodotti | No | No |
> | billingAccounts/clienti/transazioni | No | No |
> | billingAccounts/clienti/trasferimenti | No | No |
> | billingAccounts/reparti | No | No |
> | billingAccounts / enrollmentAccounts | No | No |
> | billingAccounts/fatture | No | No |
> | billingAccounts / invoiceSections | No | No |
> | billingAccounts / invoiceSections / billingSubscriptionMoveOperations | No | No |
> | billingAccounts / invoiceSections / billingSubscriptions | No | No |
> | billingAccounts/invoiceSections/billingSubscriptions/Transfer | No | No |
> | billingAccounts/invoiceSections/eleva | No | No |
> | billingAccounts / invoiceSections / initiateTransfer | No | No |
> | billingAccounts / invoiceSections / patchOperations | No | No |
> | billingAccounts / invoiceSections / productMoveOperations | No | No |
> | billingAccounts/invoiceSections/Products | No | No |
> | billingAccounts/invoiceSections/Products/Transfer | No | No |
> | billingAccounts/invoiceSections/Products/updateAutoRenew | No | No |
> | billingAccounts/invoiceSections/Transactions | No | No |
> | billingAccounts/invoiceSections/trasferimenti | No | No |
> | billingAccounts / lineOfCredit | No | No |
> | billingAccounts / patchOperations | No | No |
> | billingAccounts / paymentMethods | No | No |
> | billingAccounts/prodotti | No | No |
> | billingAccounts/transazioni | No | No |
> | billingPeriods | No | No |
> | billingPermissions | No | No |
> | billingProperty | No | No |
> | billingRoleAssignments | No | No |
> | billingRoleDefinitions | No | No |
> | createBillingRoleAssignment | No | No |
> | departments | No | No |
> | enrollmentAccounts | No | No |
> | fatture | No | No |
> | transfers | No | No |
> | trasferimenti/acceptTransfer | No | No |
> | trasferimenti/declineTransfer | No | No |
> | trasferimenti/operationStatus | No | No |
> | trasferimenti/validateTransfer | No | No |
> | validateAddress | No | No |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | mapApis | Sì | Sì |
> | updateCommunicationPreference | No | No |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | blockchainMembers | Sì | Sì |
> | cordaMembers | Sì | Sì |
> | controlli | Sì | Sì |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | blueprintAssignments | No | No |
> | blueprintAssignments / assignmentOperations | No | No |
> | blueprintAssignments/operazioni | No | No |
> | blueprints | No | No |
> | progetti/artefatti | No | No |
> | progetti/versioni | No | No |
> | cianografie/versioni/artefatti | No | No |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | botServices | Sì | Sì |
> | botServices/canali | No | No |
> | botServices/connessioni | No | No |
> | linguaggi | No | No |
> | modelli | No | No |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | Redis | Sì | Sì |

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | appliedReservations | No | No |
> | autoQuotaIncrease | No | No |
> | calculateExchange | No | No |
> | calculatePrice | No | No |
> | calculatePurchasePrice | No | No |
> | cataloghi | No | No |
> | commercialReservationOrders | No | No |
> | exchange | No | No |
> | placePurchaseOrder | No | No |
> | reservationOrders | No | No |
> | reservationOrders / calculateRefund | No | No |
> | reservationOrders/merge | No | No |
> | reservationOrders/prenotazioni | No | No |
> | reservationOrders/prenotazioni/revisioni | No | No |
> | reservationOrders/return | No | No |
> | reservationOrders/Split | No | No |
> | reservationOrders/swap | No | No |
> | reservations | No | No |
> | resourceProviders | No | No |
> | risorse | No | No |
> | validateReservationOrder | No | No |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | CdnWebApplicationFirewallManagedRuleSets | No | No |
> | CdnWebApplicationFirewallPolicies | Sì | Sì |
> | edgenodes | No | No |
> | profili | Sì | Sì |
> | profili/endpoint | Sì | Sì |
> | profili/endpoint/customdomains | No | No |
> | profili/endpoint/origini | No | No |
> | validateProbe | No | No |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | certificateOrders | Sì | Sì |
> | Ordini/certificati | No | No |
> | validateCertificateRegistrationInformation | No | No |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | capabilities | No | No |
> | domainNames | No | No |
> | domainNames/funzionalità | No | No |
> | domainNames/internalLoadBalancers | No | No |
> | domainNames/serviceCertificates | No | No |
> | domainNames/slot | No | No |
> | domainNames/slot/ruoli | No | No |
> | domainNames/slot/ruoli/metricDefinitions | No | No |
> | domainNames/slot/ruoli/metriche | No | No |
> | moveSubscriptionResources | No | No |
> | operatingSystemFamilies | No | No |
> | operatingSystems | No | No |
> | quote | No | No |
> | resourceTypes | No | No |
> | validateSubscriptionMoveAvailability | No | No |
> | virtualMachines | No | No |
> | virtualMachines/diagnosticSettings | No | No |
> | virtualMachines/metricDefinitions | No | No |
> | virtualMachines/metriche | No | No |

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | classicInfrastructureResources | No | No |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | capabilities | No | No |
> | expressRouteCrossConnections | No | No |
> | expressRouteCrossConnections/peering | No | No |
> | gatewaySupportedDevices | No | No |
> | networkSecurityGroups | No | No |
> | quote | No | No |
> | reservedIps | No | No |
> | virtualNetworks | No | No |
> | virtualNetworks/remoteVirtualNetworkPeeringProxies | No | No |
> | virtualNetworks/virtualNetworkPeerings | No | No |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | capabilities | No | No |
> | dischi | No | No |
> | immagini | No | No |
> | osImages | No | No |
> | osPlatformImages | No | No |
> | publicImages | No | No |
> | quote | No | No |
> | storageAccounts | No | No |
> | storageAccounts/blobServices | No | No |
> | storageAccounts/FileService | No | No |
> | storageAccounts/metricDefinitions | No | No |
> | storageAccounts/metriche | No | No |
> | storageAccounts/queueServices | No | No |
> | storageAccounts/servizi | No | No |
> | storageAccounts/Services/diagnosticSettings | No | No |
> | storageAccounts/Services/metricDefinitions | No | No |
> | storageAccounts/Servizi/metriche | No | No |
> | storageAccounts/tableServices | No | No |
> | storageAccounts/VMImage | No | No |
> | vmImages | No | No |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | account | Sì | Sì |

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | RateCard | No | No |
> | UsageAggregates | No | No |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | availabilitySets | Sì | Sì |
> | diskEncryptionSets | Sì | Sì |
> | dischi | Sì | Sì |
> | galleries | Sì | Sì |
> | raccolte/applicazioni | No | No |
> | raccolte/applicazioni/versioni | No | No |
> | raccolte/immagini | No | No |
> | raccolte/immagini/versioni | No | No |
> | Gruppi host | Sì | Sì |
> | Gruppi host/host | Sì | Sì |
> | immagini | Sì | Sì |
> | proximityPlacementGroups | Sì | Sì |
> | restorePointCollections | Sì | Sì |
> | restorePointCollections / restorePoints | No | No |
> | sharedVMImages | Sì | Sì |
> | sharedVMImages/versioni | No | No |
> | snapshot | Sì | Sì |
> | virtualMachines | Sì | Sì |
> | virtualMachines/estensioni | Sì | Sì |
> | virtualMachines/metricDefinitions | No | No |
> | virtualMachineScaleSets | Sì | Sì |
> | virtualMachineScaleSets/estensioni | No | No |
> | virtualMachineScaleSets/networkInterfaces | No | No |
> | virtualMachineScaleSets/publicIPAddresses | No | No |
> | virtualMachineScaleSets/virtualMachines | No | No |
> | virtualMachineScaleSets/virtualMachines/networkInterfaces | No | No |

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | AggregatedCost | No | No |
> | Saldi | No | No |
> | Budget | No | No |
> | Charges | No | No |
> | CostTags | No | No |
> | credits | No | No |
> | eventi | No | No |
> | Previsioni | No | No |
> | lots | No | No |
> | Marketplace | No | No |
> | Pricesheets | No | No |
> | products | No | No |
> | ReservationDetails | No | No |
> | ReservationRecommendations | No | No |
> | ReservationSummaries | No | No |
> | ReservationTransactions | No | No |
> | Tag | No | No |
> | tenants | No | No |
> | Termini | No | No |
> | UsageDetails | No | No |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | containerGroups | Sì | Sì |
> | serviceAssociationLinks | No | No |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | registries | Sì | Sì |
> | registri/compilazioni | No | No |
> | registri/compilazioni/Annulla | No | No |
> | registri/compilazioni/getLogLink | No | No |
> | registri/buildTasks | Sì | Sì |
> | registri/buildTasks/passaggi | No | No |
> | registri/eventGridFilters | No | No |
> | registri/generateCredentials | No | No |
> | registri/getBuildSourceUploadUrl | No | No |
> | registri/GetCredentials | No | No |
> | registri/importImage | No | No |
> | registri/privateEndpointConnectionProxies | No | No |
> | registri/privateEndpointConnectionProxies/convalida | No | No |
> | registri/privateEndpointConnections | No | No |
> | registri/privateLinkResources | No | No |
> | registri/queueBuild | No | No |
> | registri/regenerateCredential | No | No |
> | registri/regenerateCredentials | No | No |
> | registri/repliche | Sì | Sì |
> | registri/esecuzioni | No | No |
> | registri/esecuzioni/Annulla | No | No |
> | registri/scheduleRun | No | No |
> | registri/scopeMaps | No | No |
> | registri/taskRuns | Sì | Sì |
> | registri/attività | Sì | Sì |
> | registri/token | No | No |
> | registri/updatePolicies | No | No |
> | registri/webhook | Sì | Sì |
> | registri/webhook/getCallbackConfig | No | No |
> | registri/webhook/ping | No | No |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | containerServices | Sì | Sì |
> | managedClusters | Sì | Sì |
> | openShiftManagedClusters | Sì | Sì |

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | account | Sì | Sì |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | Avvisi | No | No |
> | BillingAccounts | No | No |
> | Budget | No | No |
> | CloudConnectors | No | No |
> | Connettori | Sì | Sì |
> | Departments | No | No |
> | Dimensions | No | No |
> | EnrollmentAccounts | No | No |
> | Esporta | No | No |
> | ExternalBillingAccounts | No | No |
> | ExternalBillingAccounts/avvisi | No | No |
> | ExternalBillingAccounts/dimensioni | No | No |
> | ExternalBillingAccounts/previsione | No | No |
> | ExternalBillingAccounts/query | No | No |
> | ExternalSubscriptions | No | No |
> | ExternalSubscriptions/avvisi | No | No |
> | ExternalSubscriptions/dimensioni | No | No |
> | ExternalSubscriptions/previsione | No | No |
> | ExternalSubscriptions/query | No | No |
> | Previsione | No | No |
> | Query | No | No |
> | registrazione | No | No |
> | Reportconfigs | No | No |
> | Report | No | No |
> | Impostazioni | No | No |
> | showbackRules | No | No |
> | Viste | No | No |

## <a name="microsoftcustomerlockbox"></a>Microsoft.CustomerLockbox

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | richieste | No | No |

## <a name="microsoftcustomproviders"></a>Microsoft.CustomProviders

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | associazioni | No | No |
> | resourceProviders | Sì | Sì |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | processi | Sì | Sì |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | DataBoxEdgeDevices | Sì | Sì |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | aree di lavoro | Sì | No |
> | aree di lavoro/dbWorkspaces | No | No |
> | aree di lavoro/storageEncryption | No | No |
> | aree di lavoro/virtualNetworkPeerings | No | No |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | cataloghi | Sì | Sì |
> | datacatalogs | Sì | Sì |
> | datacatalogs/origini dati | No | No |
> | sottocataloghi/origini dati/analisi | No | No |
> | datacatalogs/DataSources/scansioni/set di dati | No | No |
> | datacatalogs/origini dati/analisi/trigger | No | No |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | dataFactories | Sì | No |
> | datafactories/diagnosticSettings | No | No |
> | datafactories/metricDefinitions | No | No |
> | dataFactorySchema | No | No |
> | factory | Sì | No |
> | Factory/integrationRuntimes | No | No |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | account | Sì | Sì |
> | account/dataLakeStoreAccounts | No | No |
> | account/storageAccounts | No | No |
> | account/storageAccounts/contenitori | No | No |
> | account/transferAnalyticsUnits | No | No |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | account | Sì | Sì |
> | account/eventGridFilters | No | No |
> | account/firewallRules | No | No |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | servizi | No | No |
> | Servizi/progetti | No | No |

## <a name="microsoftdatashare"></a>Microsoft. DataShare

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | account | Sì | Sì |
> | account/condivisioni | No | No |
> | account/condivisioni/set di impostazioni | No | No |
> | account/condivisioni/inviti | No | No |
> | Accounts/shares/providersharesubscriptions | No | No |
> | Accounts/shares/synchronizationSettings | No | No |
> | account/sharesubscriptions | No | No |
> | account/sharesubscriptions/consumerSourceDataSets | No | No |
> | account/sharesubscriptions/datasetmappings | No | No |
> | account/sharesubscriptions/trigger | No | No |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | server | Sì | Sì |
> | Server/Advisor | No | No |
> | server/chiavi | No | No |
> | Server/privateEndpointConnectionProxies | No | No |
> | Server/privateEndpointConnections | No | No |
> | Server/privateLinkResources | No | No |
> | Server/queryTexts | No | No |
> | Server/recoverableServers | No | No |
> | Server/topQueryStatistics | No | No |
> | Server/virtualNetworkRules | No | No |
> | Server/waitStatistics | No | No |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | server | Sì | Sì |
> | Server/Advisor | No | No |
> | server/chiavi | No | No |
> | Server/privateEndpointConnectionProxies | No | No |
> | Server/privateEndpointConnections | No | No |
> | Server/privateLinkResources | No | No |
> | Server/queryTexts | No | No |
> | Server/recoverableServers | No | No |
> | Server/topQueryStatistics | No | No |
> | Server/virtualNetworkRules | No | No |
> | Server/waitStatistics | No | No |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | serverGroups | Sì | Sì |
> | server | Sì | Sì |
> | Server/Advisor | No | No |
> | server/chiavi | No | No |
> | Server/privateEndpointConnectionProxies | No | No |
> | Server/privateEndpointConnections | No | No |
> | Server/privateLinkResources | No | No |
> | Server/queryTexts | No | No |
> | Server/recoverableServers | No | No |
> | Server/topQueryStatistics | No | No |
> | Server/virtualNetworkRules | No | No |
> | Server/waitStatistics | No | No |
> | serversv2 | Sì | Sì |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | artifactSources | Sì | Sì |
> | rollouts | Sì | Sì |
> | serviceTopologies | Sì | Sì |
> | serviceTopologies/servizi | Sì | Sì |
> | serviceTopologies/Services/serviceUnits | Sì | Sì |
> | passaggi | Sì | Sì |

## <a name="microsoftdesktopvirtualization"></a>Microsoft. DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | applicationgroups | Sì | Sì |
> | applicationgroups/applicazioni | No | No |
> | applicationgroups/desktop | No | No |
> | applicationgroups / startmenuitems | No | No |
> | hostpools | Sì | Sì |
> | hostpools / sessionhosts | No | No |
> | hostpools / sessionhosts / usersessions | No | No |
> | hostpools / usersessions | No | No |
> | aree di lavoro | Sì | Sì |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | ElasticPools | Sì | Sì |
> | ElasticPools / IotHubTenants | Sì | Sì |
> | ElasticPools/IotHubTenants/securitySettings | No | No |
> | Hub IoT | Sì | Sì |
> | IotHubs/eventGridFilters | No | No |
> | IotHubs/securitySettings | No | No |
> | ProvisioningServices | Sì | Sì |
> | usages | No | No |

## <a name="microsoftdevops"></a>Microsoft. DevOps

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | pipeline | Sì | Sì |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | controllers | Sì | Sì |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | labcenters | Sì | Sì |
> | labs | Sì | Sì |
> | Lab/ambienti | Sì | Sì |
> | Lab/serviceRunners | Sì | Sì |
> | Lab/virtualMachines | Sì | Sì |
> | pianificazioni | Sì | Sì |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | databaseAccountNames | No | No |
> | databaseAccounts | Sì | Sì |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | domini | Sì | Sì |
> | domini/domainOwnershipIdentifiers | No | No |
> | generateSsoRequest | No | No |
> | topLevelDomains | No | No |
> | validateDomainRegistrationInformation | No | No |

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | lcsprojects | No | No |
> | lcsprojects / clouddeployments | No | No |
> | lcsprojects/connettori | No | No |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | servizi | Sì | Sì |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | domini | Sì | Sì |
> | domini/argomenti | No | No |
> | eventSubscriptions | No | No |
> | extensionTopics | No | No |
> | partnerNamespaces | Sì | Sì |
> | partnerNamespaces/eventChannels | No | No |
> | partnerRegistrations | Sì | Sì |
> | partnerTopics | Sì | Sì |
> | partnerTopics/eventSubscriptions | No | No |
> | systemTopics | Sì | Sì |
> | systemTopics/eventSubscriptions | No | No |
> | argomenti | Sì | Sì |
> | topicTypes | No | No |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | cluster | Sì | Sì |
> | spazi dei nomi | Sì | Sì |
> | spazi dei nomi/AuthorizationRules | No | No |
> | spazi dei nomi/disasterrecoveryconfigs | No | No |
> | spazi dei nomi/Eventhubs | No | No |
> | spazi dei nomi/Eventhubs/AuthorizationRules | No | No |
> | spazi dei nomi/Eventhubs/consumergroups | No | No |
> | spazi dei nomi/networkrulesets | No | No |

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | funzionalità | No | No |
> | provider | No | No |

## <a name="microsoftgallery"></a>Microsoft.Gallery

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | registra | No | No |
> | galleryitems | No | No |
> | generateartifactaccessuri | No | No |
> | myareas | No | No |
> | aree/aree | No | No |
> | aree/aree/area | No | No |
> | aree/aree/area/galleryitems | No | No |
> | aree/galleryitems di rete | No | No |
> | aree/galleryitems | No | No |
> | registrazione | No | No |
> | risorse | No | No |
> | retrieveresourcesbyid | No | No |

## <a name="microsoftgenomics"></a>Microsoft.Genomics

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | account | Sì | Sì |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | autoManagedAccounts | Sì | Sì |
> | autoManagedVmConfigurationProfiles | Sì | Sì |
> | configurationProfileAssignments | No | No |
> | guestConfigurationAssignments | No | No |
> | software | No | No |
> | softwareUpdateProfile | No | No |
> | softwareUpdates | No | No |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | hanaInstances | Sì | Sì |
> | sapMonitors | Sì | Sì |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft.HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | dedicatedHSMs | Sì | Sì |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | cluster | Sì | Sì |
> | cluster/applicazioni | No | No |

## <a name="microsofthealthcareapis"></a>Microsoft.HealthcareApis

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | servizi | Sì | Sì |

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | macchine | Sì | Sì |
> | computer/estensioni | Sì | Sì |

## <a name="microsofthybriddata"></a>Microsoft.HybridData

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | datamanager | Sì | Sì |

## <a name="microsofthydra"></a>Microsoft. Hydra

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | componenti | Sì | Sì |
> | networkScopes | Sì | Sì |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | processi | Sì | Sì |

## <a name="microsoftintune"></a>Microsoft.Intune

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | diagnosticSettings | No | No |
> | diagnosticSettingsCategories | No | No |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | appTemplates | No | No |
> | IoTApps | Sì | Sì |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | Grafico | Sì | Sì |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | deletedVaults | No | No |
> | hsmPools | Sì | Sì |
> | insiemi di credenziali | Sì | Sì |
> | insiemi di credenziali/accessPolicies | No | No |
> | insiemi di credenziali/eventGridFilters | No | No |
> | insiemi di credenziali/segreti | No | No |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | cluster | Sì | Sì |
> | cluster/attacheddatabaseconfigurations | No | No |
> | cluster/database | No | No |
> | cluster, database/DataConnection | No | No |
> | cluster/database/eventhubconnections | No | No |
> | cluster/database/principalassignments | No | No |
> | cluster/principalassignments | No | No |
> | cluster/sharedidentities | No | No |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | labaccounts | Sì | Sì |
> | utenti | No | No |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | hostingEnvironments | Sì | Sì |
> | integrationAccounts | Sì | Sì |
> | integrationServiceEnvironments | Sì | Sì |
> | integrationServiceEnvironments / managedApis | Sì | Sì |
> | isolatedEnvironments | Sì | Sì |
> | flussi di lavoro | Sì | Sì |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | commitmentPlans | Sì | Sì |
> | webServices | Sì | Sì |
> | Aree di lavoro | Sì | Sì |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | aree di lavoro | Sì | Sì |
> | aree di lavoro/calcoli | No | No |
> | aree di lavoro/eventGridFilters | No | No |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | identità | No | No |
> | userAssignedIdentities | Sì | Sì |

## <a name="microsoftmanagedservices"></a>Microsoft. ManagedServices

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | marketplaceRegistrationDefinitions | No | No |
> | registrationAssignments | No | No |
> | registrationDefinitions | No | No |

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | getEntities | No | No |
> | managementGroups | No | No |
> | managementGroups/impostazioni | No | No |
> | risorse | No | No |
> | startTenantBackfill | No | No |
> | tenantBackfillStatus | No | No |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | account | Sì | Sì |
> | account/eventGridFilters | No | No |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | offers | No | No |
> | offerTypes | No | No |
> | offerTypes/Publisher | No | No |
> | offerTypes/Publisher/offerte | No | No |
> | offerTypes/Publisher/offerte/piani | No | No |
> | offerTypes/Publisher/offerte/piani/contratti | No | No |
> | offerTypes/Publisher/offerte/piani/configurazioni | No | No |
> | offerTypes/Publisher/offerte/piani/configurazioni/importImage | No | No |
> | privategalleryitems | No | No |
> | privateStoreClient | No | No |
> | products | No | No |
> | server di pubblicazione | No | No |
> | editori/offerte | No | No |
> | editori/offerte/modifiche | No | No |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | classicDevServices | Sì | Sì |
> | updateCommunicationPreference | No | No |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | contratti | No | No |
> | offertypes | No | No |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | mediaservices | Sì | Sì |
> | MediaServices/accountFilters | No | No |
> | MediaServices/asset | No | No |
> | MediaServices/assets/assetFilters | No | No |
> | MediaServices/contentKeyPolicies | No | No |
> | MediaServices/eventGridFilters | No | No |
> | MediaServices/liveEventOperations | No | No |
> | MediaServices/liveEvents | Sì | Sì |
> | MediaServices/liveEvents/liveOutputs | No | No |
> | MediaServices/liveOutputOperations | No | No |
> | MediaServices/mediaGraphs | No | No |
> | MediaServices/streamingEndpointOperations | No | No |
> | MediaServices/le entità streamingendpoint | Sì | Sì |
> | MediaServices/streamingLocators | No | No |
> | MediaServices/streamingPolicies | No | No |
> | MediaServices/trasformazioni | No | No |
> | MediaServices/trasformazioni/processi | No | No |

## <a name="microsoftmicroservices4spring"></a>Microsoft. Microservices4Spring

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | appClusters | Sì | Sì |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | assessmentProjects | Sì | Sì |
> | migrateprojects | Sì | Sì |
> | moveCollections | Sì | Sì |
> | progetti | Sì | Sì |

## <a name="microsoftmixedreality"></a>Microsoft.MixedReality

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | holographicsBroadcastAccounts | Sì | Sì |
> | objectUnderstandingAccounts | Sì | Sì |
> | remoteRenderingAccounts | Sì | Sì |
> | spatialAnchorsAccounts | Sì | Sì |
> | surfaceReconstructionAccounts | Sì | Sì |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | netAppAccounts | Sì | No |
> | netAppAccounts / capacityPools | Sì | No |
> | netAppAccounts/capacityPools/volumi | Sì | No |
> | netAppAccounts/capacityPools/volumi/snapshot | No | No |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | applicationGateways | Sì | Sì |
> | applicationGatewayWebApplicationFirewallPolicies | Sì | Sì |
> | applicationSecurityGroups | Sì | Sì |
> | azureFirewallFqdnTags | No | No |
> | azureFirewalls | Sì | No |
> | bastionHosts | Sì | Sì |
> | bgpServiceCommunities | No | No |
> | connections | Sì | Sì |
> | ddosCustomPolicies | Sì | Sì |
> | ddosProtectionPlans | Sì | Sì |
> | dnsOperationStatuses | No | No |
> | dnszones | Sì | Sì |
> | dnszones/A | No | No |
> | dnszones/AAAA | No | No |
> | dnszones/tutti | No | No |
> | dnszones/CAA | No | No |
> | dnszones/CNAME | No | No |
> | dnszones/MX | No | No |
> | dnszones/NS | No | No |
> | dnszones/PTR | No | No |
> | dnszones/recordset | No | No |
> | dnszones/SOA | No | No |
> | dnszones/SRV | No | No |
> | dnszones/TXT | No | No |
> | expressRouteCircuits | Sì | Sì |
> | expressRouteCrossConnections | Sì | Sì |
> | expressRouteGateways | Sì | Sì |
> | expressRoutePorts | Sì | Sì |
> | expressRouteServiceProviders | No | No |
> | firewallPolicies | Sì | Sì |
> | frontdoors | Sì, ma limitato (vedere la [Nota di seguito](#frontdoor)) | Sì |
> | frontdoorWebApplicationFirewallManagedRuleSets | Sì, ma limitato (vedere la [Nota di seguito](#frontdoor)) | No |
> | frontdoorWebApplicationFirewallPolicies | Sì, ma limitato (vedere la [Nota di seguito](#frontdoor)) | Sì |
> | getDnsResourceReference | No | No |
> | internalNotify | No | No |
> | loadBalancers | Sì | No |
> | localNetworkGateways | Sì | Sì |
> | natGateways | Sì | Sì |
> | networkIntentPolicies | Sì | Sì |
> | networkInterfaces | Sì | Sì |
> | networkProfiles | Sì | Sì |
> | networkSecurityGroups | Sì | Sì |
> | networkWatchers | Sì | No |
> | networkWatchers / connectionMonitors | Sì | No |
> | networkWatchers/lenti | Sì | No |
> | networkWatchers / pingMeshes | Sì | No |
> | p2sVpnGateways | Sì | Sì |
> | privateDnsOperationStatuses | No | No |
> | privateDnsZones | Sì | Sì |
> | privateDnsZones/A | No | No |
> | privateDnsZones/AAAA | No | No |
> | privateDnsZones/tutti | No | No |
> | privateDnsZones/CNAME | No | No |
> | privateDnsZones/MX | No | No |
> | privateDnsZones/PTR | No | No |
> | privateDnsZones/SOA | No | No |
> | privateDnsZones/SRV | No | No |
> | privateDnsZones/TXT | No | No |
> | privateDnsZones / virtualNetworkLinks | Sì | Sì |
> | privateEndpoints | Sì | Sì |
> | privateLinkServices | Sì | Sì |
> | publicIPAddresses | Sì | Sì |
> | publicIPPrefixes | Sì | Sì |
> | routeFilters | Sì | Sì |
> | routeTables | Sì | Sì |
> | serviceEndpointPolicies | Sì | Sì |
> | trafficManagerGeographicHierarchies | No | No |
> | trafficmanagerprofiles | Sì | Sì |
> | trafficmanagerprofiles/heatMaps | No | No |
> | trafficManagerUserMetricsKeys | No | No |
> | virtualHubs | Sì | Sì |
> | virtualNetworkGateways | Sì | Sì |
> | virtualNetworks | Sì | Sì |
> | virtualNetworkTaps | Sì | Sì |
> | virtualWans | Sì | Sì |
> | vpnGateways | Sì | No |
> | vpnSites | Sì | Sì |
> | webApplicationFirewallPolicies | Sì | Sì |

<a id="frontdoor" />

> [!NOTE]
> Per il servizio front-end di Azure, è possibile applicare tag quando si crea la risorsa, ma non è attualmente supportata l'aggiornamento o l'aggiunta di tag.


## <a name="microsoftnotebooks"></a>Microsoft. Notebooks

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | NotebookProxies | No | No |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | spazi dei nomi | Sì | No |
> | spazi dei nomi/notificationHubs | Sì | No |

## <a name="microsoftobjectstore"></a>Microsoft. ObjectStore

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | osNamespaces | Sì | Sì |

## <a name="microsoftoffazure"></a>Microsoft.OffAzure

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | HyperVSites | Sì | Sì |
> | ImportSites | Sì | Sì |
> | ServerSites | Sì | Sì |
> | VMwareSites | Sì | Sì |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | cluster | Sì | Sì |
> | linkTargets | No | No |
> | storageInsightConfigs | No | No |
> | aree di lavoro | Sì | Sì |
> | aree di lavoro/esportazioni | No | No |
> | aree di lavoro/origini dati | No | No |
> | aree di lavoro/linkedServices | No | No |
> | aree di lavoro/privateEndpointConnectionProxies | No | No |
> | aree di lavoro/privateEndpointConnections | No | No |
> | aree di lavoro/privateLinkResources | No | No |
> | aree di lavoro/query | No | No |
> | aree di lavoro/scopedPrivateLinkProxies | No | No |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | managementassociations | No | No |
> | managementconfigurations | Sì | Sì |
> | soluzioni | Sì | Sì |
> | viste | Sì | Sì |

## <a name="microsoftpeering"></a>Microsoft.Peering

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | legacyPeerings | No | No |
> | peerAsns | No | No |
> | peering | Sì | Sì |
> | peeringServiceCountries | No | No |
> | peeringServiceProviders | No | No |
> | peeringServices | Sì | Sì |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | policyEvents | No | No |
> | policyMetadata | No | No |
> | policyStates | No | No |
> | policyTrackedResources | No | No |
> | remediations | No | No |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | consoles | No | No |
> | dashboard | Sì | Sì |
> | userSettings | No | No |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | workspaceCollections | Sì | Sì |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | capacità | Sì | Sì |

## <a name="microsoftprojectbabylon"></a>Microsoft. ProjectBabylon

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | account | Sì | Sì |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | backupProtectedItems | No | No |
> | insiemi di credenziali | Sì | Sì |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | spazi dei nomi | Sì | Sì |
> | spazi dei nomi/AuthorizationRules | No | No |
> | spazi dei nomi/hybridconnections | No | No |
> | spazi dei nomi/hybridconnections/AuthorizationRules | No | No |
> | spazi dei nomi/wcfrelays | No | No |
> | spazi dei nomi/wcfrelays/AuthorizationRules | No | No |

## <a name="microsoftremoteapp"></a>Microsoft. RemoteApp

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | account | No | No |
> | raccolte | Sì | Sì |
> | raccolte/applicazioni | No | No |
> | raccolte/SecurityPrincipals | No | No |
> | templateImages | No | No |

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | query | Sì | Sì |
> | resourceChangeDetails | No | No |
> | resourceChanges | No | No |
> | risorse | No | No |
> | resourcesHistory | No | No |
> | subscriptionsStatus | No | No |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | availabilityStatuses | No | No |
> | childAvailabilityStatuses | No | No |
> | childResources | No | No |
> | emergingissues | No | No |
> | eventi | No | No |
> | impactedResources | No | No |
> | metadata | No | No |
> | notifiche | No | No |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | distribuzioni | Sì | No |
> | distribuzioni/operazioni | No | No |
> | deploymentScripts | Sì | Sì |
> | deploymentScripts/log | No | No |
> | collegamenti | No | No |
> | notifyResourceJobs | No | No |
> | provider | No | No |
> | resourceGroups | Sì | No |
> | sottoscrizioni | Sì | No |
> | tenants | No | No |

## <a name="microsoftsaas"></a>Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | applicazioni | Sì | Sì |
> | saasresources | No | No |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | resourceHealthMetadata | No | No |
> | searchServices | Sì | Sì |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | adaptiveNetworkHardenings | No | No |
> | advancedThreatProtectionSettings | No | No |
> | avvisi | No | No |
> | allowedConnections | No | No |
> | applicationWhitelistings | No | No |
> | assessmentMetadata | No | No |
> | valutazioni | No | No |
> | autoDismissAlertsRules | No | No |
> | Automazioni | Sì | Sì |
> | AutoProvisioningSettings | No | No |
> | Compliances | No | No |
> | dataCollectionAgents | No | No |
> | deviceSecurityGroups | No | No |
> | discoveredSecuritySolutions | No | No |
> | externalSecuritySolutions | No | No |
> | InformationProtectionPolicies | No | No |
> | iotSecuritySolutions | Sì | Sì |
> | iotSecuritySolutions / analyticsModels | No | No |
> | iotSecuritySolutions / analyticsModels / aggregatedAlerts | No | No |
> | iotSecuritySolutions / analyticsModels / aggregatedRecommendations | No | No |
> | jitNetworkAccessPolicies | No | No |
> | File networkdata | No | No |
> | criteri | No | No |
> | pricings | No | No |
> | regulatoryComplianceStandards | No | No |
> | regulatoryComplianceStandards / regulatoryComplianceControls | No | No |
> | regulatoryComplianceStandards / regulatoryComplianceControls / regulatoryComplianceAssessments | No | No |
> | securityContacts | No | No |
> | securitySolutions | No | No |
> | securitySolutionsReferenceData | No | No |
> | securityStatuses | No | No |
> | securityStatusesSummaries | No | No |
> | serverVulnerabilityAssessments | No | No |
> | impostazioni | No | No |
> | sottovalutazioni | No | No |
> | attività | No | No |
> | topologie | No | No |
> | workspaceSettings | No | No |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | diagnosticSettings | No | No |
> | diagnosticSettingsCategories | No | No |

## <a name="microsoftsecurityinsights"></a>Microsoft. SecurityInsights

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | aggregations | No | No |
> | alertRules | No | No |
> | alertRuleTemplates | No | No |
> | segnalibri | No | No |
> | cases | No | No |
> | DataConnector | No | No |
> | dataConnectorsCheckRequirements | No | No |
> | entities | No | No |
> | entityQueries | No | No |
> | incidenti | No | No |
> | officeConsents | No | No |
> | impostazioni | No | No |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | spazi dei nomi | Sì | No |
> | spazi dei nomi/AuthorizationRules | No | No |
> | spazi dei nomi/disasterrecoveryconfigs | No | No |
> | spazi dei nomi/eventgridfilters | No | No |
> | spazi dei nomi/networkrulesets | No | No |
> | spazi dei nomi/code | No | No |
> | spazi dei nomi/code/AuthorizationRules | No | No |
> | spazi dei nomi/argomenti | No | No |
> | spazi dei nomi/argomenti/AuthorizationRules | No | No |
> | spazi dei nomi/argomenti/sottoscrizioni | No | No |
> | spazi dei nomi/argomenti/sottoscrizioni/regole | No | No |
> | premiumMessagingRegions | No | No |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | applicazioni | Sì | Sì |
> | cluster | Sì | Sì |
> | cluster/applicazioni | No | No |
> | containerGroups | Sì | Sì |
> | containerGroupSets | Sì | Sì |
> | edgeclusters | Sì | Sì |
> | edgeclusters/applicazioni | No | No |
> | managedclusters | Sì | Sì |
> | managedclusters/NodeTypes | No | No |
> | reti | Sì | Sì |
> | secretstores | Sì | Sì |
> | secretstores/certificati | No | No |
> | secretstores/segreti | No | No |
> | volumes | Sì | Sì |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | applicazioni | Sì | Sì |
> | containerGroups | Sì | Sì |
> | gateways | Sì | Sì |
> | reti | Sì | Sì |
> | chiavi private | Sì | Sì |
> | volumes | Sì | Sì |

## <a name="microsoftservices"></a>Microsoft. Services

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | providerRegistrations | No | No |
> | providerRegistrations / resourceTypeRegistrations | No | No |
> | rollouts | Sì | Sì |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | SignalR | Sì | Sì |
> | SignalR/eventGridFilters | No | No |

## <a name="microsoftsiterecovery"></a>Microsoft.SiteRecovery

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | SiteRecoveryVault | Sì | Sì |

## <a name="microsoftsoftwareplan"></a>Microsoft. SoftwarePlan

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | hybridUseBenefits | No | No |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | applicationDefinitions | Sì | Sì |
> | applicazioni | Sì | Sì |
> | jitRequests | Sì | Sì |

## <a name="microsoftspoolservice"></a>Microsoft. SpoolService

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | registeredSubscriptions | No | No |
> | effettua lo spooling | Sì | Sì |


## <a name="microsoftsql"></a>Microsoft.SQL

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | managedInstances | Sì | Sì |
> | managedInstances/database | Sì (vedere la [Nota sotto](#sqlnote)) | Sì |
> | managedInstances/databases/backupShortTermRetentionPolicies | No | No |
> | managedInstances/database/schemi/tabelle/colonne/sensitivityLabels | No | No |
> | managedInstances/databases/vulnerabilityAssessments | No | No |
> | managedInstances/databases/vulnerabilityAssessments/Rules/Baselines | No | No |
> | managedInstances/encryptionProtector | No | No |
> | managedInstances/chiavi | No | No |
> | managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | No | No |
> | managedInstances/vulnerabilityAssessments | No | No |
> | server | Sì | Sì |
> | Server/amministratori | No | No |
> | Server/communicationLinks | No | No |
> | server/database | Sì (vedere la [Nota sotto](#sqlnote)) | Sì |
> | Server/encryptionProtector | No | No |
> | Server/firewallRules | No | No |
> | server/chiavi | No | No |
> | Server/restorableDroppedDatabases | No | No |
> | Server/serviceobjectives | No | No |
> | Server/tdeCertificates | No | No |
> | virtualClusters | No | No |

<a id="sqlnote" />

> [!NOTE]
> Il database master non supporta i tag, ma altri database, inclusi i database di Azure SQL Data Warehouse, supportano i tag. I database di Azure SQL Data Warehouse devono essere nello stato attivo (non in pausa).

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | SqlVirtualMachineGroups | Sì | Sì |
> | SqlVirtualMachineGroups / AvailabilityGroupListeners | No | No |
> | SqlVirtualMachines | Sì | Sì |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | storageAccounts | Sì | Sì |
> | storageAccounts/blobServices | No | No |
> | storageAccounts/FileService | No | No |
> | storageAccounts/queueServices | No | No |
> | storageAccounts/servizi | No | No |
> | storageAccounts/Services/metricDefinitions | No | No |
> | storageAccounts/tableServices | No | No |
> | usages | No | No |

## <a name="microsoftstoragecache"></a>Microsoft.StorageCache

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | cache | Sì | Sì |
> | cache/storageTargets | No | No |
> | usageModels | No | No |

## <a name="microsoftstoragereplication"></a>Microsoft. StorageReplication

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | replicationGroups | No | No |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | storageSyncServices | Sì | Sì |
> | storageSyncServices/registeredServers | No | No |
> | storageSyncServices / syncGroups | No | No |
> | storageSyncServices / syncGroups / cloudEndpoints | No | No |
> | storageSyncServices / syncGroups / serverEndpoints | No | No |
> | storageSyncServices/flussi di lavoro | No | No |

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | storageSyncServices | Sì | Sì |
> | storageSyncServices/registeredServers | No | No |
> | storageSyncServices / syncGroups | No | No |
> | storageSyncServices / syncGroups / cloudEndpoints | No | No |
> | storageSyncServices / syncGroups / serverEndpoints | No | No |
> | storageSyncServices/flussi di lavoro | No | No |

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | storageSyncServices | Sì | Sì |
> | storageSyncServices/registeredServers | No | No |
> | storageSyncServices / syncGroups | No | No |
> | storageSyncServices / syncGroups / cloudEndpoints | No | No |
> | storageSyncServices / syncGroups / serverEndpoints | No | No |
> | storageSyncServices/flussi di lavoro | No | No |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | managers | Sì | Sì |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | streamingjobs | Sì (vedere la nota seguente) | Sì |

> [!NOTE]
> Impossibile aggiungere un tag quando streamingjobs è in esecuzione. Interrompere la risorsa per aggiungere un tag.

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | annulla | No | No |
> | CreateSubscription | No | No |
> | abilitazione | No | No |
> | ridenominazione | No | No |
> | SubscriptionDefinitions | No | No |
> | SubscriptionOperations | No | No |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | ambienti | Sì | No |
> | ambienti/accessPolicies | No | No |
> | ambienti/EventSources | Sì | No |
> | ambienti/referenceDataSets | Sì | No |

## <a name="microsoftvmwarecloudsimple"></a>Microsoft.VMwareCloudSimple

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | dedicatedCloudNodes | Sì | Sì |
> | dedicatedCloudServices | Sì | Sì |
> | virtualMachines | Sì | Sì |

## <a name="microsoftvnfmanager"></a>Microsoft. VnfManager

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | devices | Sì | Sì |
> | registeredSubscriptions | No | No |
> | fornitori | No | No |
> | fornitori/SKU | No | No |
> | fornitori/vnfs | No | No |
> | vnfs | Sì | Sì |

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | apiManagementAccounts | No | No |
> | Account/ACL | No | No |
> | Account/API | No | No |
> | Account/API/ACL | No | No |
> | Account/API/ACL | No | No |
> | Account/API/connessioni | No | No |
> | Account/API/Connections/ACL | No | No |
> | Account/API/localizedDefinitions | No | No |
> | Account/ACL | No | No |
> | Account/connessioni | No | No |
> | billingMeters | No | No |
> | certificati | Sì | Sì |
> | connectionGateways | Sì | Sì |
> | connections | Sì | Sì |
> | customApis | Sì | Sì |
> | deletedSites | No | No |
> | hostingEnvironments | Sì | Sì |
> | hostingEnvironments/eventGridFilters | No | No |
> | hostingEnvironments/multiRolePools | No | No |
> | hostingEnvironments/dei pool | No | No |
> | publishingUsers | No | No |
> | indicazioni | No | No |
> | resourceHealthMetadata | No | No |
> | runtimes | No | No |
> | serverFarms | Sì | Sì |
> | serverFarms/eventGridFilters | No | No |
> | siti | Sì | Sì |
> | siti/configurazione  | No | No |
> | siti/eventGridFilters | No | No |
> | siti/hostNameBindings | No | No |
> | siti/file networkconfig | No | No |
> | siti/premieraddons | Sì | Sì |
> | siti/slot | Sì | Sì |
> | siti/slot/eventGridFilters | No | No |
> | siti/slot/hostNameBindings | No | No |
> | siti/slot/file networkconfig | No | No |
> | sourceControls | No | No |
> | staticSites | Sì | Sì |
> | convalida | No | No |
> | verifyHostingEnvironmentVnet | No | No |

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | diagnosticSettings | No | No |
> | diagnosticSettingsCategories | No | No |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | DeviceServices | Sì | Sì |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Tag supportati | Tag nel report sui costi |
> | ------------- | ----------- | ----------- |
> | componenti | No | No |
> | componentsSummary | No | No |
> | monitorInstances | No | No |
> | monitorInstancesSummary | No | No |
> | monitor | No | No |
> | notificationSettings | No | No |

## <a name="next-steps"></a>Passaggi successivi

Per informazioni su come applicare tag alle risorse, vedere [Usare tag per organizzare le risorse di Azure](tag-resources.md).
