---
title: Ottenere un gruppo di risorse gestite e ridimensionare le macchine virtuali - Azure PowerShell
description: Questo articolo include uno script di esempio di Azure PowerShell che ottiene un gruppo di risorse gestito per un'applicazione gestita di Azure. Lo script ridimensiona le VM.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/27/2017
ms.author: tomfitz
ms.openlocfilehash: f11f9b56a8a17330fac3fb50bfd5f560357463e7
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2019
ms.locfileid: "72529411"
---
# <a name="get-resources-in-a-managed-resource-group-and-resize-vms-with-powershell"></a>Ottenere risorse in un gruppo di risorse gestite e ridimensionare le macchine virtuali con PowerShell

Questo script recupera le risorse da un gruppo di risorse gestite e ridimensiona le macchine virtuali in tale gruppo.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script di esempio

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/managed-applications/get-application/get-application.ps1 "Get application")]


## <a name="script-explanation"></a>Spiegazione dello script

Questo script usa i comandi seguenti per distribuire l'applicazione gestita. Ogni comando della tabella include collegamenti alla documentazione specifica del comando.

| Comando | Note |
|---|---|
| [Get-AzManagedApplication](https://docs.microsoft.com/powershell/module/az.resources/get-azmanagedapplication) | Elenca le applicazioni gestite. Fornire il nome del gruppo di risorse per definire i risultati. |
| [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource) | Elenca le risorse. Fornire un gruppo e un tipo di risorse per definire i risultati. |
| [Update-AzVM](https://docs.microsoft.com/powershell/module/az.compute/update-azvm) | Aggiorna le dimensioni di una macchina virtuale. |


## <a name="next-steps"></a>Passaggi successivi

* Per un'introduzione alle applicazioni gestite, vedere [Panoramica di Applicazione gestita di Azure](../overview.md).
* Per altre informazioni su PowerShell, vedere la [documentazione di Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).
