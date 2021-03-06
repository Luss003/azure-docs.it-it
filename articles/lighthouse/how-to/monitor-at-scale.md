---
title: Monitorare le risorse delegate su larga scala
description: Informazioni su come usare efficacemente i log di monitoraggio di Azure in modo scalabile nei tenant dei clienti gestiti.
ms.date: 02/03/2020
ms.topic: conceptual
ms.openlocfilehash: 217df3c55ab54b6569bae8cacb338764ecb7125d
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/11/2020
ms.locfileid: "77122896"
---
# <a name="monitor-delegated-resources-at-scale"></a>Monitorare le risorse delegate su larga scala

I provider di servizi potrebbero aver integrato più tenant di clienti per la gestione risorse delegate di Azure. [Azure Lighthouse](../overview.md) consente ai provider di servizi di eseguire operazioni su larga scala tra più tenant contemporaneamente, rendendo più efficienti le attività di gestione.

Questo argomento illustra come usare i [log di monitoraggio di Azure](../../azure-monitor/platform/data-platform-logs.md) in modo scalabile tra i tenant dei clienti gestiti.

## <a name="create-log-analytics-workspaces"></a>Creare aree di lavoro Log Analytics

Per raccogliere i dati, è necessario creare Log Analytics aree di lavoro. Queste aree di lavoro Log Analytics sono ambienti univoci per i dati raccolti da monitoraggio di Azure. Ogni area di lavoro ha un proprio repository di dati e una propria configurazione, mentre le origini dati e le soluzioni sono configurate per l'archiviazione dei dati in un'area di lavoro specifica.

Si consiglia di creare queste aree di lavoro direttamente nei tenant del cliente. In questo modo i dati rimangono nei tenant anziché essere esportati nel proprio. Questo consente anche il monitoraggio centralizzato di risorse o servizi supportati da Log Analytics, garantendo una maggiore flessibilità sui tipi di dati monitorati.

È possibile creare un'area di lavoro Log Analytics usando il [portale di Azure](../../azure-monitor/learn/quick-create-workspace.md), usando l'interfaccia della riga di comando di [Azure](../../azure-monitor/learn/quick-create-workspace-cli.md)oppure [Azure PowerShell](../../azure-monitor/learn/quick-create-workspace-posh.md).

## <a name="deploy-policies-that-log-data"></a>Distribuire i criteri che registrano i dati

Dopo aver creato le aree di lavoro di Log Analytics, è possibile distribuire i [criteri di Azure](../../governance/policy/index.yml) tra le gerarchie dei clienti in modo che i dati di diagnostica vengano inviati all'area di lavoro appropriata in ogni tenant. I criteri esatti distribuiti possono variare a seconda dei tipi di risorse che si desidera monitorare.

Per altre informazioni sulla creazione di criteri, vedere [esercitazione: creare e gestire criteri per applicare la conformità](../../governance/policy/tutorials/create-and-manage.md). Questo [strumento della community](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/tools/azure-diagnostics-policy-generator) fornisce uno script che consente di creare criteri per monitorare i tipi di risorse specifici scelti.

Una volta determinati i criteri da distribuire, è possibile [distribuirli nelle sottoscrizioni delegate su larga scala](policy-at-scale.md).

## <a name="analyze-the-gathered-data"></a>Analizzare i dati raccolti

Dopo aver distribuito i criteri, i dati verranno registrati nelle aree di lavoro Log Analytics create in ogni tenant del cliente. Per ottenere informazioni approfondite su tutti i clienti gestiti, è possibile usare strumenti come le [cartelle di lavoro di monitoraggio di Azure](../../azure-monitor/platform/workbooks-overview.md) per raccogliere e analizzare le informazioni provenienti da più origini dati.

## <a name="next-steps"></a>Passaggi successivi

- Informazioni su [Monitoraggio di Azure](../../azure-monitor/index.yml).
- Informazioni sui [log di monitoraggio di Azure](../../azure-monitor/platform/data-platform-logs.md).
- Informazioni sulle [esperienze di gestione tra tenant](../concepts/cross-tenant-management-experience.md).
