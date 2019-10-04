---
title: Script di PowerShell per creare una risorsa di Application Insights | Documentazione Microsoft
description: Consente di automatizzare la creazione di risorse di Application Insights.
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: f0082c9b-43ad-4576-a417-4ea8e0daf3d9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 11/19/2016
ms.author: mbullwin
ms.openlocfilehash: 8a7b19dd6e5bc08c0c7e278b514ecaa9dc13a00e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "60254488"
---
# <a name="powershell-script-to-create-an-application-insights-resource"></a>Script di PowerShell per creare una risorsa di Application Insights

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Se si vuole monitorare una nuova applicazione oppure una nuova versione di un'applicazione con [Azure Application Insights](https://azure.microsoft.com/services/application-insights/), è possibile configurare una nuova risorsa in Microsoft Azure. Questa risorsa verrà usata per analizzare e visualizzare i dati di telemetria provenienti dall'app. 

Per automatizzare la creazione di una nuova risorsa con PowerShell.

Se, ad esempio, si intende sviluppare un'app per dispositivi mobili, è probabile che, in un dato momento, i clienti usino diverse versioni pubblicate dell'app. Se però si vuole evitare di combinare i risultati di telemetria delle diverse versioni, è possibile fare in modo che il processo di compilazione crei una nuova risorsa per ogni compilazione.

> [!NOTE]
> Se si desidera creare un set di risorse simultaneamente, è consigliabile [creare le risorse tramite un modello di Azure](powershell.md).
> 
> 

## <a name="script-to-create-an-application-insights-resource"></a>Script per creare una risorsa di Application Insights
Vedere le specifiche dei cmdlet:

* [New-AzResource](https://msdn.microsoft.com/library/mt652510.aspx)
* [New-AzRoleAssignment](https://msdn.microsoft.com/library/mt678995.aspx)

*Script di PowerShell*  

```powershell


###########################################
# Set Values
###########################################

# If running manually, uncomment before the first 
# execution to login to the Azure Portal:

# Connect-AzAccount / Connect-AzAccount

# Set the name of the Application Insights Resource

$appInsightsName = "TestApp"

# Set the application name used for the value of the Tag "AppInsightsApp" 

$applicationTagName = "MyApp"

# Set the name of the Resource Group to use.  
# Default is the application name.
$resourceGroupName = "MyAppResourceGroup"

###################################################
# Create the Resource and Output the name and iKey
###################################################

# Select the azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

# Create the App Insights Resource


$resource = New-AzResource `
  -ResourceName $appInsightsName `
  -ResourceGroupName $resourceGroupName `
  -Tag @{ applicationType = "web"; applicationName = $applicationTagName} `
  -ResourceType "Microsoft.Insights/components" `
  -Location "East US" `  # or North Europe, West Europe, South Central US
  -PropertyObject @{"Application_Type"="web"} `
  -Force

# Give owner access to the team

New-AzRoleAssignment `
  -SignInName "myteam@fabrikam.com" `
  -RoleDefinitionName Owner `
  -Scope $resource.ResourceId 


# Display iKey
Write-Host "App Insights Name = " $resource.Name
Write-Host "IKey = " $resource.Properties.InstrumentationKey

```

## <a name="what-to-do-with-the-ikey"></a>Cosa fare con l'iKey
Per identificare le singole risorse, viene usata una chiave di strumentazione (iKey), ovvero l'output dello script di creazione della risorsa. Lo script di compilazione deve fornire l'iKey all'istanza di Application Insights SDK incorporata nell'app.

È possibile fornire l'iKey all'SDK in due modi diversi:

* In [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md): 
  * `<instrumentationkey>`*ikey*`</instrumentationkey>`
* Oppure nel [codice di inizializzazione](../../azure-monitor/app/api-custom-events-metrics.md): 
  * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`

## <a name="see-also"></a>Vedere anche
* [Creare risorse Application Insights e test web da modelli](powershell.md)
* [Impostare il monitoraggio di diagnostica Azure con PowerShell](powershell-azure-diagnostics.md) 
* [Impostare avvisi tramite PowerShell](powershell-alerts.md)

