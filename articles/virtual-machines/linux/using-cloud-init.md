---
title: Panoramica del supporto di cloud-init per macchine virtuali Linux in Azure
description: Panoramica delle funzionalità di cloud-init per configurare una macchina virtuale in fase di provisioning in Azure.
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 195c22cd-4629-4582-9ee3-9749493f1d72
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 10/11/2019
ms.author: danis
ms.openlocfilehash: 7b3f64d0629ba5d7aaf85b854e1ee8e5a1410f94
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75458610"
---
# <a name="cloud-init-support-for-virtual-machines-in-azure"></a>Supporto di cloud-init per macchine virtuali in Azure
Questo articolo illustra il supporto esistente per [cloud-init](https://cloudinit.readthedocs.io) per configurare una macchina virtuale (VM) o set di scalabilità di macchine virtuali in fase di provisioning in Azure. Questi script cloud-init vengono eseguiti al primo avvio dopo il provisioning delle risorse da parte di Azure.  

## <a name="cloud-init-overview"></a>Panoramica di cloud-init
[Cloud-init](https://cloudinit.readthedocs.io) è un approccio diffuso per personalizzare una macchina virtuale Linux al primo avvio. Cloud-init consente di installare pacchetti e scrivere file o configurare utenti e impostazioni di sicurezza. Poiché cloud-init viene chiamato durante il processo di avvio iniziale, non è necessario applicare passaggi aggiuntivi o agenti alla configurazione.  Per altre informazioni su come formattare correttamente i file `#cloud-config`, vedere il [sito della documentazione di cloud-init](https://cloudinit.readthedocs.io/en/latest/topics/format.html#cloud-config-data).  I file `#cloud-config` sono file di testo codificati in formato base64.

Cloud-init funziona anche fra distribuzioni. Ad esempio, non si usa **apt-get install** o **yum install** per installare un pacchetto. In alternativa, è possibile definire un elenco di pacchetti da installare. Cloud-init userà automaticamente lo strumento di gestione del pacchetto nativo per la distribuzione selezionata.

Microsoft sta collaborando attivamente con i partner di distribuzione Linux approvati per offrire immagini abilitate per cloud-init in Azure Marketplace. Queste immagini faranno funzionare senza interruzioni e configurazioni di cloud-init con macchine virtuali e set di scalabilità di macchine virtuali. La tabella seguente contiene le attuali immagini abilitate per cloud-init disponibili nella piattaforma Azure:

| Editore | Offerta | SKU | Versione | Pronta per cloud-init |
|:--- |:--- |:--- |:--- |:--- |
|Canonical |UbuntuServer |18.04-LTS |latest |sì | 
|Canonical |UbuntuServer |16.04-LTS |latest |sì | 
|Canonical |UbuntuServer |14.04.5-LTS |latest |sì |
|CoreOS |CoreOS |Stable |latest |sì |
|OpenLogic 7,7 |CentOS |7-CI |7.7.20190920 |anteprima |
|Oracle 7,7 |Oracle-Linux |77-ci |7.7.01|anteprima |
|RedHat 7.6 |RHEL |7-RAW-CI |7.6.2019072418 |sì |
|RedHat 7.7 |RHEL |7-RAW-CI |7.7.2019081601 |anteprima |
    
Attualmente Azure Stack non supporta il provisioning di RHEL 7. x e CentOS 7. x con cloud-init.

* Per RHEL 7,6, pacchetto cloud-init, il pacchetto supportato è: *18.2-1. el7_6.2* 
* Per RHEL 7,7 (anteprima), pacchetto cloud-init, il pacchetto di anteprima è: *18.5 3. EL7*
* Per CentOS 7,7 (anteprima), pacchetto cloud-init, il pacchetto di anteprima è: *18.5 3. EL7. CentOS*
* Per Oracle 7,7 (anteprima), pacchetto cloud-init, il pacchetto di anteprima è: *18,5-3.0.1. EL7*

## <a name="what-is-the-difference-between-cloud-init-and-the-linux-agent-wala"></a>Qual è la differenza tra cloud-init e l'agente Linux (WALA)?
WALA è un agente specifico per la piattaforma Azure usato per il provisioning e la configurazione di macchine virtuali e la gestione delle estensioni di Azure. Microsoft sta migliorando l'attività di configurazione delle macchine virtuali per l'uso di cloud-init al posto dell'agente Linux per consentire agli attuali clienti di cloud-init di usare gli script cloud-init correnti.  Se sono stati effettuati investimenti in script cloud-init per la configurazione di sistemi Linux, **non sono necessarie altre impostazioni** per abilitarli. 

Se non si include l'opzione dell'interfaccia della riga di comando di Azure `--custom-data` in fase di provisioning, WALA accetta i parametri di provisioning delle macchine virtuali minimi necessari per il provisioning della macchina virtuale e il completamento della distribuzione con le impostazioni predefinite.  Se si fa riferimento all'opzione `--custom-data` di cloud-init, qualsiasi contenuto nei dati personalizzati (singole impostazioni o script completo) sostituirà le impostazioni predefinite di WALA. 

Le configurazioni WALA delle macchine virtuali hanno vincoli di tempo che ne impongono l'uso entro l'intervallo di tempo massimo di provisioning delle macchine virtuali.  Le configurazioni cloud-init applicate alle macchine virtuali non hanno vincoli di tempo e non causano la mancata riuscita della distribuzione a causa del timeout. 

## <a name="deploying-a-cloud-init-enabled-virtual-machine"></a>Distribuzione di una macchina virtuale abilitata per cloud-init
Distribuire una macchina virtuale abilitata per cloud-init è facile come fare riferimento a una distribuzione abilitata per cloud-init.  I gestori di distribuzioni Linux devono scegliere se abilitare e integrare cloud-init nelle proprie immagini pubblicate in Azure. Dopo aver verificato che l'immagine che si vuole distribuire sia abilitata per cloud-init, è possibile usare l'interfaccia della riga di comando di Azure per distribuirla. 

Il primo passaggio della distribuzione di questa immagine è creare un gruppo di risorse con il comando [az group create](/cli/azure/group). Un gruppo di risorse di Azure è un contenitore logico in cui le risorse di Azure vengono distribuite e gestite. 

L'esempio seguente crea un gruppo di risorse denominato *myResourceGroup* nella località *stati uniti orientali*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```
Il passaggio successivo consiste nel creare un file denominato *cloud-init.txt* nella shell corrente e incollare la configurazione seguente. Per questo esempio, creare il file in Cloud Shell anziché nel computer locale. È possibile usare qualsiasi editor. Immettere `sensible-editor cloud-init.txt` per creare il file e visualizzare un elenco degli editor disponibili. Scegliere #1 per usare l'editor **nano**. Assicurarsi che l'intero file cloud-init venga copiato correttamente, in particolare la prima riga:

```yaml
#cloud-config
package_upgrade: true
packages:
  - httpd
```
Premere `ctrl-X` per uscire dal file, digitare `y` per salvare il file e premere `enter` per confermare il nome del file alla chiusura.

Il passaggio finale consiste nel creare una macchina virtuale con il comando [az vm create](/cli/azure/vm). 

L'esempio seguente crea una macchina virtuale denominata *centos74* e le chiavi SSH, se non esistono già in un percorso predefinito. Per usare un set specifico di chiavi, utilizzare l'opzione `--ssh-key-value`.  Usare il parametro `--custom-data` per specificare il file di configurazione di cloud-init. Se il file è stato salvato all'esterno della directory di lavoro corrente, specificare il percorso completo della configurazione *cloud-init.txt* . L'esempio seguente crea una macchina virtuale denominata *centos74*:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS-CI:7-CI:latest \
  --custom-data cloud-init.txt \
  --generate-ssh-keys 
```

Dopo la creazione della macchina virtuale, l'interfaccia della riga di comando di Azure mostra informazioni specifiche della distribuzione. Prendere nota di `publicIpAddress`. Questo indirizzo viene usato per accedere alla VM.  Sono necessari alcuni minuti per creare la macchina virtuale, installare il pacchetto e avviare l'app. Sono presenti attività in background la cui esecuzione continua dopo che l'interfaccia della riga di comando di Azure è tornata al prompt. È possibile eseguire SSH nella macchina virtuale e seguire i passaggi descritti nella sezione della risoluzione dei problemi per visualizzare i log di cloud-init. 

## <a name="troubleshooting-cloud-init"></a>Risoluzione dei problemi relativi a cloud-init
Al termine del provisioning della macchina virtuale, cloud-init verrà eseguito in tutti i moduli e gli script definiti in `--custom-data` per configurare la macchina virtuale.  Se è necessario correggere eventuali errori o omissioni della configurazione, cercare il nome del modulo (ad esempio, `disk_setup` o `runcmd`) nel log di cloud-init che si trova in **/var/log/cloud-init.log**.

> [!NOTE]
> Non tutti gli errori dei moduli generano un errore di configurazione di cloud-init completo e irreversibile. Ad esempio, se usando il modulo `runcmd` lo script non riesce, cloud-init segnalerà comunque che il provisioning è riuscito perché il modulo runcmd è stato eseguito.

Per altre informazioni sulla registrazione di cloud-init, fare riferimento alla [documentazione di cloud-init](https://cloudinit.readthedocs.io/en/latest/topics/logging.html) 

## <a name="next-steps"></a>Passaggi successivi
Per esempi cloud-init di modifiche di configurazione, vedere i documenti seguenti:
 
- [Aggiungere un altro utente Linux a una VM](cloudinit-add-user.md)
- [Eseguire uno strumento di gestione di pacchetti per aggiornare pacchetti esistenti al primo avvio](cloudinit-update-vm.md)
- [Modificare il nome host locale della macchina virtuale](cloudinit-update-vm-hostname.md) 
- [Installare un pacchetto dell'applicazione, aggiornare i file di configurazione e inserire le chiavi](tutorial-automate-vm-deployment.md)
