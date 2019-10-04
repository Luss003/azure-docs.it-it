---
title: Usare gli strumenti da riga di comando per avviare e arrestare macchine virtuali di Azure DevTest Labs | Microsoft Docs
description: Informazioni su come usare gli strumenti da riga di comando per avviare e arrestare le macchine virtuali in Azure DevTest Labs.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: spelluru
ms.openlocfilehash: a8132735d1af08055e9341608dcac0564ed4b927
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "60236688"
---
# <a name="use-command-line-tools-to-start-and-stop-azure-devtest-labs-virtual-machines"></a>Usare gli strumenti da riga di comando per avviare e arrestare le macchine virtuali di Azure DevTest Labs
Questo articolo illustra come usare Azure PowerShell o CLI di Azure per avviare o arrestare le macchine virtuali in un lab in Azure DevTest Labs. È possibile creare script di PowerShell/CLI per automatizzare queste operazioni. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="overview"></a>Panoramica
Azure DevTest Labs è un modo per creare ambienti di sviluppo/test veloci, facili ed efficienti. Consente di gestire i costi, rapidamente il provisioning di macchine virtuali e ridurre al minimo la bonifica dei rifiuti.  Sono disponibili funzionalità incorporate nel portale di Azure che consentono di configurare le macchine virtuali in un laboratorio per avviare e arrestare automaticamente la in momenti specifici. 

Tuttavia, in alcuni scenari, è possibile automatizzare l'avvio e arresto di macchine virtuali dagli script di PowerShell/CLI. Offre una certa flessibilità con avvio e arresto di singoli computer in qualsiasi momento anziché in momenti specifici. Ecco alcune delle situazioni in cui eseguire queste attività tramite script sarebbe utile.

- Quando si usa un'applicazione a 3 livelli come parte di un ambiente di test, i livelli dovranno essere avviate in una sequenza. 
- Disattivare una macchina virtuale quando viene soddisfatto un criterio personalizzato per risparmiare denaro. 
- Usarlo come attività all'interno di un flusso di lavoro di integrazione continua/recapito Continuo per all'inizio del flusso di, utilizzare le macchine virtuali come computer di compilazione, computer o l'infrastruttura di test, quindi arrestare le macchine virtuali, una volta completato il processo. Un esempio di questo sarebbe il factory di immagini personalizzate con Azure DevTest Labs.  

## <a name="azure-powershell"></a>Azure PowerShell
Il seguente script PowerShell avvia una macchina virtuale in un lab. [Invoke-AzResourceAction](/powershell/module/az.resources/invoke-azresourceaction?view=azps-1.7.0) è l'obiettivo principale dello script. Il **ResourceId** parametro è l'ID risorsa completo per la macchina virtuale nel lab. Il **azione** parametro in cui è il **avviare** oppure **arrestare** vengono impostate le opzioni a seconda di ciò che è necessario.

```powershell
# The id of the subscription
$subscriptionId = "111111-11111-11111-1111111"

# The name of the lab
$devTestLabName = "yourlabname"

# The name of the virtual machine to be started
$vMToStart = "vmname"

# The action on the virtual machine (Start or Stop)
$vmAction = "Start"

# Select the Azure subscription
Select-AzSubscription -SubscriptionId $subscriptionId

# Get the lab information
if ($(Get-Module -Name AzureRM).Version.Major -eq 6) {
    $devTestLab = Get-AzResource -ResourceType 'Microsoft.DevTestLab/labs' -Name $devTestLabName
} else {
    $devTestLab = Find-AzResource -ResourceType 'Microsoft.DevTestLab/labs' -ResourceNameEquals $devTestLabName
}

# Start the VM and return a succeeded or failed status
$returnStatus = Invoke-AzResourceAction `
                    -ResourceId "$($devTestLab.ResourceId)/virtualmachines/$vMToStart" `
                    -Action $vmAction `
                    -Force

if ($returnStatus.Status -eq 'Succeeded') {
    Write-Output "##[section] Successfully updated DTL machine: $vMToStart, Action: $vmAction"
}
else {
    Write-Error "##[error]Failed to update DTL machine: $vMToStart, Action: $vmAction"
}
```


## <a name="azure-cli"></a>Interfaccia della riga di comando di Azure
Il [CLI Azure](/cli/azure/get-started-with-azure-cli?view=azure-cli-latest) un altro modo per automatizzare l'avvio e arresto di macchine virtuali di DevTest Labs. Può essere Azure CLI [installato](/cli/azure/install-azure-cli?view=azure-cli-latest) su sistemi operativi diversi. Lo script seguente offre i comandi per avviare e arrestare una macchina virtuale in un lab. 

```azurecli
# Sign in to Azure
az login 

## Get the name of the resource group that contains the lab
az resource list --resource-type "Microsoft.DevTestLab/labs" --name "yourlabname" --query "[0].resourceGroup" 

## Start the VM
az lab vm start --lab-name yourlabname --name vmname --resource-group labResourceGroupName

## Stop the VM
az lab vm stop --lab-name yourlabname --name vmname --resource-group labResourceGroupName
```


## <a name="next-steps"></a>Passaggi successivi
Vedere l'articolo seguente per usare il portale di Azure per eseguire queste operazioni: [Riavviare una macchina virtuale](devtest-lab-restart-vm.md).
