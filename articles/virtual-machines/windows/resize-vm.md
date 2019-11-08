---
title: Usare PowerShell per ridimensionare una macchina virtuale Windows in Azure | Documentazione Microsoft
description: Ridimensionare una macchina virtuale Windows creata nel modello di distribuzione Resource Manager usando Azure PowerShell.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 057ff274-6dad-415e-891c-58f8eea9ed78
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 05/30/2018
ms.author: cynthn
ms.openlocfilehash: 1f5f8f3a315b894ab8bc972d36008b5bce85d8e7
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/07/2019
ms.locfileid: "73749252"
---
# <a name="resize-a-windows-vm"></a>Ridimensionare una VM Windows

Questo articolo illustra come spostare una macchina virtuale a un'altra [dimensione](sizes.md) con Azure PowerShell.

Dopo aver creato una macchina virtuale (VM), è possibile scalarla in verticale o in orizzontale modificandone le dimensioni. In alcuni casi, è necessario prima deallocare la macchina virtuale. Questa situazione può verificarsi se le nuove dimensioni non sono disponibili nel cluster hardware che attualmente ospita la VM.

Se la macchina virtuale usa l'archiviazione Premium, per ottenere il supporto per questo tipo di archiviazione assicurarsi di aver scelto una versione **s** delle dimensioni, ad esempio, Standard_E4**s**_v3 anziché Standard_E4_v3.

 

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a>Ridimensionare una VM Windows non inclusa in un set di disponibilità

Impostare alcune variabili. Sostituire i valori con i propri.

```powershell
$resourceGroup = "myResourceGroup"
$vmName = "myVM"
```

Elencare le dimensioni di VM disponibili nel cluster hardware in cui la VM è ospitata. 
   
```powershell
Get-AzVMSize -ResourceGroupName $resourceGroup -VMName $vmName 
```

Se la dimensione voluta è inclusa nell'elenco, per ridimensionare la VM eseguire i comandi seguenti. Se la dimensione desiderata non è inclusa nell'elenco, andare al passaggio 3.
   
```powershell
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName
$vm.HardwareProfile.VmSize = "<newVMsize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```

Se la dimensione voluta non è nell'elenco, eseguire i comandi seguenti per deallocare la VM, ridimensionarla e quindi riavviarla. Sostituire **\<newVMsize >** con le dimensioni desiderate.
   
```powershell
Stop-AzVM -ResourceGroupName $resourceGroup -Name $vmName -Force
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName
$vm.HardwareProfile.VmSize = "<newVMSize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
Start-AzVM -ResourceGroupName $resourceGroup -Name $vmName
```

> [!WARNING]
> La deallocazione della VM rilascia qualsiasi indirizzo IP dinamico assegnato alla VM. I dischi del sistema operativo e dei dati non sono coinvolti. 
> 
> 

## <a name="resize-a-windows-vm-in-an-availability-set"></a>Ridimensionare una VM Windows inclusa in un set di disponibilità

Se la nuova dimensione di una VM inclusa in un set di disponibilità non è disponibile nel cluster hardware che attualmente ospita la VM in questione, per ridimensionare tale VM sarà necessario deallocare tutte le VM incluse nel set di disponibilità. Dopo il ridimensionamento di una VM, può inoltre essere necessario aggiornare le dimensioni delle altre VM incluse nel gruppo di disponibilità. Per ridimensionare una VM inclusa in un gruppo di disponibilità, seguire questa procedura.

```powershell
$resourceGroup = "myResourceGroup"
$vmName = "myVM"
```

Elencare le dimensioni di VM disponibili nel cluster hardware in cui la VM è ospitata. 
   
```powershell
Get-AzVMSize -ResourceGroupName $resourceGroup -VMName $vmName 
```

Se la dimensione desiderata è inclusa nell'elenco, per ridimensionare la VM eseguire i comandi seguenti. Se non è inclusa nell'elenco, passare alla prossima sezione.
   
```powershell
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName 
$vm.HardwareProfile.VmSize = "<newVmSize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```
    
Se la dimensione voluta non è elencata, seguire la procedura seguente per deallocare tutte le VM incluse nel set di disponibilità, ridimensionare le VM e quindi riavviarle.

Arrestare tutte le VM nel set di disponibilità.
   
```powershell
$as = Get-AzAvailabilitySet -ResourceGroupName $resourceGroup
$vmIds = $as.VirtualMachinesReferences
foreach ($vmId in $vmIDs){
    $string = $vmID.Id.Split("/")
    $vmName = $string[8]
    Stop-AzVM -ResourceGroupName $resourceGroup -Name $vmName -Force
    } 
```

Ridimensionare e riavviare tutte le VM nel set di disponibilità.
   
```powershell
$newSize = "<newVmSize>"
$as = Get-AzAvailabilitySet -ResourceGroupName $resourceGroup
$vmIds = $as.VirtualMachinesReferences
  foreach ($vmId in $vmIDs){
    $string = $vmID.Id.Split("/")
    $vmName = $string[8]
    $vm = Get-AzVM -ResourceGroupName $resourceGroup -Name $vmName
    $vm.HardwareProfile.VmSize = $newSize
    Update-AzVM -ResourceGroupName $resourceGroup -VM $vm
    Start-AzVM -ResourceGroupName $resourceGroup -Name $vmName
    }
```

## <a name="next-steps"></a>Passaggi successivi

Per una maggiore scalabilità, eseguire più istanze di VM e scalare in orizzontale. Per altre informazioni, vedere [ridimensionare automaticamente i computer Windows in un set di scalabilità di macchine virtuali](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).

