---
title: "Script di Azure PowerShell di esempio: modificare l'intervallo di porte di Remote Desktop Protocol | Microsoft Docs"
description: "Script di Azure PowerShell di esempio: modifica l'intervallo di porte di Remote Desktop Protocol di un cluster distribuito."
services: service-fabric
tags: azure-service-management
author: athinanthny
ms.author: atsenthi
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
ms.date: 03/19/2018
ms.custom: sfrev
ms.openlocfilehash: b3d922cb5a7cdf34e63731ab32f21c94d7b6bca0
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/15/2020
ms.locfileid: "76025013"
---
# <a name="update-the-rdp-port-range-values"></a>Aggiornare i valori dell'intervallo di porte RDP

Questo script di esempio modifica i valori dell'intervallo di porte RDP nelle macchine virtuali del nodo del cluster dopo la distribuzione del cluster stesso.  Azure PowerShell viene usato in modo che le macchine virtuali sottostanti non eseguano un ciclo.  Lo script ottiene la risorsa `Microsoft.Network/loadBalancers` nel gruppo di risorse cluster e aggiorna i valori `inboundNatPools.frontendPortRangeStart` e `inboundNatPools.frontendPortRangeEnd`. Personalizzare i parametri in base alle esigenze.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Se necessario, installare Azure PowerShell usando l'istruzione presente nella [Guida di Azure PowerShell](/powershell/azure/overview).

## <a name="sample-script"></a>Script di esempio

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-port-range/change-rdp-port-range.ps1 "Update the RDP port range values")]

## <a name="script-explanation"></a>Spiegazione dello script

Questo script usa i comandi seguenti. Ogni comando della tabella include collegamenti alla documentazione specifica del comando.

| Comando | Note |
|---|---|
| [Get-AzResource](/powershell/module/az.resources/get-azresource) | Ottiene la risorsa `Microsoft.Network/loadBalancers`. |
|[Set-AzResource](/powershell/module/az.resources/set-azresource)|Aggiorna la risorsa `Microsoft.Network/loadBalancers`.|

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sul modulo Azure PowerShell, vedere la [documentazione di Azure PowerShell](/powershell/azure/overview).

Altri esempi di Azure PowerShell per Azure Service Fabric sono disponibili in [Esempi di Azure PowerShell](../service-fabric-powershell-samples.md).
