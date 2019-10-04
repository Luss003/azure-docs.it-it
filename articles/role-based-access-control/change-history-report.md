---
title: Visualizzare i log attività per le modifiche del controllo degli accessi in base al ruolo per le risorse di Azure | Microsoft Docs
description: Visualizzare i log attività per le modifiche del controllo degli accessi in base al ruolo per le risorse di Azure per gli ultimi 90 giorni.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 2bc68595-145e-4de3-8b71-3a21890d13d9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/02/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e5758f480c9216cf71e47509682053b39f0b15bf
ms.sourcegitcommit: ee61ec9b09c8c87e7dfc72ef47175d934e6019cc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/30/2019
ms.locfileid: "70172408"
---
# <a name="view-activity-logs-for-rbac-changes-to-azure-resources"></a>Visualizzare i log attività per le modifiche del controllo degli accessi in base al ruolo per le risorse di Azure

È talvolta necessario visualizzare informazioni sulle modifiche del controllo degli accessi in base al ruolo per le risorse di Azure, ad esempio a scopo di controllo o per la risoluzione di problemi. Ogni volta che un utente apporta modifiche alle assegnazioni o alle definizioni di ruolo all'interno delle sottoscrizioni, le modifiche vengono registrate in [Log attività di Azure](../azure-monitor/platform/activity-logs-overview.md). È possibile visualizzare i log attività per vedere tutte le modifiche apportate al controllo degli accessi in base al ruolo negli ultimi 90 giorni.

## <a name="operations-that-are-logged"></a>Operazioni registrate

Ecco le operazioni correlate al controllo degli accessi in base al ruolo che vengono registrate nel log attività:

- Crea assegnazione ruolo
- Elimina assegnazione di ruolo
- Crea o aggiorna la definizione del ruolo personalizzata
- Elimina la definizione del ruolo personalizzata

## <a name="azure-portal"></a>Portale di Azure

Il modo più semplice per iniziare è visualizzare i log attività con il portale di Azure. Lo screenshot seguente mostra un esempio di log attività filtrato in modo da visualizzare le operazioni di definizione e assegnazione di ruolo. Include inoltre un collegamento per scaricare i log in formato CSV.

![Log attività tramite il portale - screenshot](./media/change-history-report/activity-log-portal.png)

Il log attività nel portale include diversi filtri. Di seguito sono elencati i filtri correlati al controllo degli accessi in base al ruolo:

|Applica filtro  |Value  |
|---------|---------|
|Categoria evento     | <ul><li>Amministrativa</li></ul>         |
|Operazione     | <ul><li>Crea assegnazione ruolo</li> <li>Elimina assegnazione di ruolo</li> <li>Crea o aggiorna la definizione del ruolo personalizzata</li> <li>Elimina la definizione del ruolo personalizzata</li></ul>      |


Per altre informazioni sui log attività, vedere [Visualizzare eventi nel log attività](/azure/azure-resource-manager/resource-group-audit?toc=%2fazure%2fmonitoring-and-diagnostics%2ftoc.json).

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

Per visualizzare i log attività con Azure PowerShell, usare il comando [Get-AzLog](/powershell/module/Az.Monitor/Get-AzLog).

Questo comando elenca tutte le modifiche relative alle assegnazioni di ruolo in una sottoscrizione per gli ultimi 7 giorni:

```azurepowershell
Get-AzLog -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/roleAssignments/*'}
```

Questo comando elenca tutte le modifiche relative alle definizioni di ruolo in un gruppo di risorse per gli ultimi 7 giorni:

```azurepowershell
Get-AzLog -ResourceGroupName pharma-sales -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/roleDefinitions/*'}
```

Questo comando elenca tutte le modifiche alle assegnazioni di ruolo e alle definizioni di ruolo in una sottoscrizione per gli ultimi 7 giorni e visualizza i risultati in un elenco:

```azurepowershell
Get-AzLog -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/role*'} | Format-List Caller,EventTimestamp,{$_.Authorization.Action},Properties
```

```Example
Caller                  : alain@example.com
EventTimestamp          : 4/20/2018 9:18:07 PM
$_.Authorization.Action : Microsoft.Authorization/roleAssignments/write
Properties              :
                          statusCode     : Created
                          serviceRequestId: 11111111-1111-1111-1111-111111111111

Caller                  : alain@example.com
EventTimestamp          : 4/20/2018 9:18:05 PM
$_.Authorization.Action : Microsoft.Authorization/roleAssignments/write
Properties              :
                          requestbody    : {"Id":"22222222-2222-2222-2222-222222222222","Properties":{"PrincipalId":"33333333-3333-3333-3333-333333333333","RoleDefinitionId":"/subscriptions/00000000-0000-0000-0000-000000000000/providers
                          /Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c","Scope":"/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"}}

```

## <a name="azure-cli"></a>Interfaccia della riga di comando di Azure

Per visualizzare i log attività usando l'interfaccia della riga di comando di Azure, usare il comando [az monitor activity-log list](/cli/azure/monitor/activity-log#az-monitor-activity-log-list).

Questo comando elenca i log attività in un gruppo di risorse dall'ora di inizio:

```azurecli
az monitor activity-log list --resource-group pharma-sales --start-time 2018-04-20T00:00:00Z
```

Questo comando elenca i log attività per il provider di risorse di autorizzazione dall'ora di inizio:

```azurecli
az monitor activity-log list --resource-provider "Microsoft.Authorization" --start-time 2018-04-20T00:00:00Z
```

## <a name="azure-monitor-logs"></a>Log di Monitoraggio di Azure

[Log di monitoraggio di Azure](../log-analytics/log-analytics-overview.md) è un altro strumento che è possibile usare per raccogliere e analizzare le modifiche RBAC per tutte le risorse di Azure. I log di monitoraggio di Azure offrono i vantaggi seguenti:

- Possibilità di scrivere query e logica complesse
- Integrazione con avvisi, Power BI e altri strumenti
- Salvataggio dei dati per periodi di conservazione più estesi
- Riferimenti incrociati con gli altri log, ad esempio relativi a sicurezza e macchine virtuali o personalizzati

Ecco i passaggi di base per iniziare:

1. [Creare un'area di lavoro Log Analytics](../azure-monitor/learn/quick-create-workspace.md).

1. [Configurare la soluzione Analisi log attività](../azure-monitor/platform/activity-log-collect.md#activity-logs-analytics-monitoring-solution) per la propria area di lavoro.

1. [Visualizzare i log attività](../azure-monitor/platform/activity-log-collect.md#activity-logs-analytics-monitoring-solution). Per passare rapidamente alla pagina Panoramica della soluzione Analisi log attività è possibile fare clic sull'opzione **log Analytics** .

   ![Opzione log di monitoraggio di Azure nel portale](./media/change-history-report/azure-log-analytics-option.png)

1. Facoltativamente è possibile usare la pagina [Ricerca log](../log-analytics/log-analytics-log-search.md) o il [portale Advanced Analytics](../azure-monitor/log-query/get-started-portal.md) per eseguire query e visualizzare i log. Per altre informazioni su queste due opzioni, vedere la sezione [sulla pagina di ricerca log o sul portale Advanced Analytics](../azure-monitor/log-query/portals.md).

Ecco una query che restituisce le nuove assegnazioni di ruolo organizzate in base al provider di risorse di destinazione:

```
AzureActivity
| where TimeGenerated > ago(60d) and OperationNameValue startswith "Microsoft.Authorization/roleAssignments/write" and ActivityStatus == "Succeeded"
| parse ResourceId with * "/providers/" TargetResourceAuthProvider "/" *
| summarize count(), makeset(Caller) by TargetResourceAuthProvider
```

Ecco una query che restituisce le modifiche alle assegnazioni di ruolo visualizzate in un grafico:

```
AzureActivity
| where TimeGenerated > ago(60d) and OperationNameValue startswith "Microsoft.Authorization/roleAssignments"
| summarize count() by bin(TimeGenerated, 1d), OperationNameValue
| render timechart
```

![Log attività tramite il portale Advanced Analytics - screenshot](./media/change-history-report/azure-log-analytics.png)

## <a name="next-steps"></a>Passaggi successivi
* [Visualizzare eventi nel log attività](/azure/azure-resource-manager/resource-group-audit?toc=%2fazure%2fmonitoring-and-diagnostics%2ftoc.json)
* [Monitorare l'attività di sottoscrizione con il log attività di Azure](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)
