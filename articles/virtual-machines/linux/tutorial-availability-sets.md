---
title: 'Esercitazione: Disponibilità elevata per le macchine virtuali Linux in Azure | Microsoft Docs'
description: In questa esercitazione si apprenderà come usare l'interfaccia della riga di comando di Azure per distribuire macchine virtuali a disponibilità elevata nei set di disponibilità
documentationcenter: ''
services: virtual-machines-linux
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: tutorial
ms.date: 08/24/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 8857e93aec883dc4b7fe0b71093184c3b604b24a
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2019
ms.locfileid: "70103587"
---
# <a name="tutorial-create-and-deploy-highly-available-virtual-machines-with-the-azure-cli"></a>Esercitazione: Creare e distribuire macchine virtuali a disponibilità elevata con l'interfaccia della riga di comando di Azure

In questa esercitazione si apprenderà come aumentare la disponibilità e l'affidabilità delle soluzioni delle proprie macchine virtuali in Azure tramite una funzionalità denominata set di disponibilità. I set di disponibilità assicurano che le macchine virtuali distribuite in Azure vengano distribuite tra più cluster hardware isolati. Questa operazione assicura che, se si verifica un errore hardware o software all'interno di Azure, solo un subset delle macchine virtuali viene interessato e che nel complesso la soluzione rimane disponibile e operativa.

In questa esercitazione si apprenderà come:

> [!div class="checklist"]
> * Creare un set di disponibilità
> * Creare una macchina virtuale in un set di disponibilità
> * Controllare le dimensioni delle macchine virtuali disponibili

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se si sceglie di installare e usare l'interfaccia della riga di comando in locale, per questa esercitazione è necessario eseguire l'interfaccia della riga di comando di Azure versione 2.0.30 o successiva. Eseguire `az --version` per trovare la versione. Se è necessario eseguire l'installazione o l'aggiornamento, vedere [Installare l'interfaccia della riga di comando di Azure]( /cli/azure/install-azure-cli).

## <a name="high-availability-in-azure-overview"></a>Panoramica della disponibilità elevata in Azure
La disponibilità elevata in Azure può essere creata in molti modi diversi. Due opzioni disponibili sono i set di disponibilità e le zone di disponibilità. Usando i set di disponibilità, le macchine virtuali vengono protette dagli errori che possono verificarsi in un data center. Sono inclusi gli errori hardware e gli errori software di Azure. Usando le zone di disponibilità, le macchine virtuali vengono inserite in un'infrastruttura separata fisicamente senza risorse condivise e di conseguenza sono protette da errori dell'intero data center.

Usare set di disponibilità o zone di disponibilità quando si vuole distribuire soluzioni affidabili basate su macchine virtuali all'interno di Azure.

### <a name="availability-set-overview"></a>Informazioni generali sui set di disponibilità

Un set di disponibilità è una funzionalità di raggruppamento logico che è possibile usare in Azure per garantire che le risorse delle macchine virtuali inserite dall'utente siano isolate tra loro quando vengono distribuite all'interno di un data center di Azure. Azure garantisce che le macchine virtuali inserite all'interno di un set di disponibilità vengano eseguite tra più server fisici, rack di calcolo, unità di archiviazione e commutatori di rete. In caso di guasto hardware o errore software in Azure, viene interessato solo un subset delle macchine virtuali. L'applicazione nel suo complesso rimarrà attiva e disponibile per i clienti. I set di disponibilità sono una funzionalità essenziale da sfruttare quando si vogliono creare soluzioni cloud affidabili.

Si consideri una soluzione tipica basata su macchine virtuali, in cui sono disponibili quattro server Web front-end e vengono usate due macchine virtuali di back-end che ospitano un database. Con Azure è possibile definire due set di disponibilità prima di distribuire le macchine virtuali: un set di disponibilità per il livello "Web" e un set di disponibilità per il livello "database". Quando si crea una nuova macchina virtuale, è quindi possibile specificare il set di disponibilità come parametro per il comando az vm create. Azure garantisce automaticamente che le macchine virtuali create all'interno del set di disponibilità vengano isolate tramite installazione in più risorse hardware fisiche. Se l'hardware fisico in cui è in esecuzione una delle macchine virtuali dei server di database o dei server Web presenta un problema, le altre istanze delle macchine virtuali dei server Web e di database rimangono in esecuzione, perché si trovano all'interno di risorse hardware diverse.

### <a name="availability-zone-overview"></a>Panoramica delle zone di disponibilità

Le zone di disponibilità offrono una soluzione a disponibilità elevata che consente di proteggere le applicazioni e i dati da eventuali guasti del data center. Le zone di disponibilità sono località fisiche esclusive all'interno di un'area di Azure. Ogni zona è costituita da uno o più data center dotati di impianti indipendenti per l'alimentazione, il raffreddamento e la connettività di rete. Per garantire la resilienza, devono essere presenti almeno tre zone separate in tutte le aree abilitate. La separazione fisica delle zone di disponibilità all'interno di un'area consente di proteggere le applicazioni e i dati da eventuali guasti del data center. I servizi con ridondanza della zona replicano le applicazioni e i dati tra aree di disponibilità per garantire la protezione da singoli punti di errore. Con le zone di disponibilità Azure offre un contratto di servizio tra i migliori del settore, con tempo di attività delle macchine virtuali del 99,99%.

Si consideri una tipica soluzione basata su macchine virtuali in cui sono disponibili quattro server Web front-end e vengono usate due macchine virtuali di back-end che ospitano un database. Analogamente ai set di disponibilità, è necessario distribuire le macchine virtuali in due zone di disponibilità separate, una per il livello "Web" e una per il livello "database". Quando si crea una nuova macchina virtuale e si specifica la zona di disponibilità come parametro per il comando az vm create, Azure garantisce che le macchine virtuali create vengano automaticamente isolate in zone di disponibilità completamente diverse. Se l'intero data center in cui è in esecuzione una delle macchine virtuali dei server di database o dei server Web presenta un problema, le altre istanze delle macchine virtuali dei server Web e di database restano in esecuzione, perché vengono eseguite in data center completamente separati.

## <a name="create-an-availability-set"></a>Creare un set di disponibilità

È possibile creare un set di disponibilità usando il comando [az vm availability-set create](/cli/azure/vm/availability-set). In questo esempio il numero di domini di aggiornamento e di errore viene impostato su *2* per il set di disponibilità denominato *myAvailabilitySet* nel gruppo di risorse *myResourceGroupAvailability*.

Creare prima un gruppo di risorse con il comando [az group create](/cli/azure/group#az-group-create), quindi creare il set di disponibilità:

```azurecli-interactive
az group create --name myResourceGroupAvailability --location eastus

az vm availability-set create \
    --resource-group myResourceGroupAvailability \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

I set di disponibilità consentono di isolare le risorse in domini di errore e in domini di aggiornamento. Un **dominio di errore** rappresenta una raccolta isolata di server + rete + risorse di archiviazione. Nell'esempio precedente il set di disponibilità viene distribuito in almeno due domini di errore quando vengono distribuite le macchine virtuali. Il set di disponibilità viene distribuito anche in due **domini di aggiornamento**. Due domini di aggiornamento assicurano che, quando vengono eseguiti automaticamente gli aggiornamenti software, le risorse delle macchine virtuali siano isolate, impedendo che tutto il software in esecuzione nelle macchine virtuali venga aggiornato contemporaneamente.


## <a name="create-vms-inside-an-availability-set"></a>Creare macchine virtuali in un set di disponibilità

Per garantire la corretta distribuzione delle macchine virtuali in tutto l'hardware, le VM devono essere create all'interno del set di disponibilità. Non è possibile aggiungere una macchina virtuale esistente a un set di disponibilità dopo la sua creazione.

Quando si crea una macchina virtuale con [az vm create](/cli/azure/vm), usare il parametro `--availability-set` per specificare il nome del set di disponibilità.

```azurecli-interactive
for i in `seq 1 2`; do
   az vm create \
     --resource-group myResourceGroupAvailability \
     --name myVM$i \
     --availability-set myAvailabilitySet \
     --size Standard_DS1_v2  \
     --vnet-name myVnet \
     --subnet mySubnet \
     --image UbuntuLTS \
     --admin-username azureuser \
     --generate-ssh-keys
done
```

Sono ora disponibili due macchine virtuali all'interno del set di disponibilità. Poiché si trovano nello stesso set di disponibilità, Azure assicura che le macchine virtuali e tutte le relative risorse (inclusi i dischi dati) vengano distribuite in risorse hardware fisiche isolate. Questa distribuzione consente di garantire una disponibilità molto maggiore della soluzione complessiva delle macchine virtuali.

La distribuzione del set di disponibilità può essere esaminata nel portale accedendo a Gruppi di risorse > myResourceGroupAvailability > myAvailabilitySet. Le macchine virtuali vengono distribuite tra i due domini di errore e di aggiornamento, come illustrato nell'esempio seguente:

![Set di disponibilità nel portale](./media/tutorial-availability-sets/fd-ud.png)

## <a name="check-for-available-vm-sizes"></a>Controllare le dimensioni delle macchine virtuali disponibili

È possibile aggiungere più macchine virtuali al set di disponibilità in un secondo momento, se le dimensioni delle macchine virtuali sono disponibili nell'hardware. Usare il comando [az vm availability-set list-sizes](/cli/azure/vm/availability-set#az-vm-availability-set-list-sizes) per elencare tutte le dimensioni disponibili nel cluster hardware per il set di disponibilità:

```azurecli-interactive
az vm availability-set list-sizes \
     --resource-group myResourceGroupAvailability \
     --name myAvailabilitySet \
     --output table
```

## <a name="next-steps"></a>Passaggi successivi

Questa esercitazione illustra come:

> [!div class="checklist"]
> * Creare un set di disponibilità
> * Creare una macchina virtuale in un set di disponibilità
> * Controllare le dimensioni delle macchine virtuali disponibili

Passare all'esercitazione successiva per informazioni sui set di scalabilità di macchine virtuali.

> [!div class="nextstepaction"]
> [Creare un set di scalabilità di macchine virtuali](tutorial-create-vmss.md)

* Per altre informazioni sulle zone di disponibilità, vedere la [documentazione delle zone di disponibilità](../../availability-zones/az-overview.md).
* Altre informazioni sui set di disponibilità e sulle zone di disponibilità sono disponibili [qui](./manage-availability.md).
* Per provare le zone di disponibilità, vedere [Creare una macchina virtuale Linux in una zona di disponibilità con l'interfaccia della riga di comando di Azure](./create-cli-availability-zone.md)
