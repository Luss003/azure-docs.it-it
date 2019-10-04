---
title: Configurare indirizzi IP privati per le macchine virtuali (classiche) - Interfaccia della riga di comando classica di Azure
titlesuffix: Azure Virtual Network
description: Informazioni su come configurare gli indirizzi IP privati per le macchine virtuali (classiche) usando l'interfaccia della riga di comando classica di Azure.
services: virtual-network
documentationcenter: na
author: genlin
manager: dcscontentpm
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: genli
ms.openlocfilehash: 64342c6dcf4c9a052442a6f4b0afb7a56994f496
ms.sourcegitcommit: ca359c0c2dd7a0229f73ba11a690e3384d198f40
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/17/2019
ms.locfileid: "71059232"
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-the-azure-classic-cli"></a>Configurare gli indirizzi IP privati per una macchina virtuale (classica) usando l'interfaccia della riga di comando classica di Azure

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

In questo articolo viene illustrato il modello di distribuzione classica. È inoltre possibile [gestire un indirizzo IP statico privato nel modello di distribuzione di gestione delle risorse](virtual-networks-static-private-ip-arm-cli.md).

I comandi di esempio dell'interfaccia della riga di comando classica di Azure riportati di seguito prevedono un ambiente semplice già creato. Se si desidera eseguire i comandi illustrati in questo documento, creare innanzitutto l'ambiente di prova descritto in [creare una rete virtuale](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Come specificare un indirizzo IP statico privato durante la creazione di una macchina virtuale.
Per creare una nuova VM denominata *DNS01* in un nuovo servizio cloud denominato *TestService* in base allo scenario precedente, attenersi alla procedura seguente:

1. Se l'interfaccia della riga di comando di Azure non è mai stata usata, vedere [Installare e configurare l'interfaccia della riga di comando di Azure](/cli/azure/install-cli-version-1.0) e seguire le istruzioni fino al punto in cui si selezionano l'account e la sottoscrizione di Azure.
2. Eseguire il **azure service create** per creare il servizio cloud.
   
        azure service create TestService --location uscentral
   
    Output previsto:
   
        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name TestService
        info:    service create command OK
3. Eseguire il comando **azure create vm** per creare la VM. Si noti il valore per un indirizzo IP statico privato. Nell'elenco riportato dopo l'output sono indicati i parametri usati.
   
        azure vm create -l centralus -n DNS01 -w TestVNet -S "192.168.1.101" TestService bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 adminuser AdminP@ssw0rd
   
    Output previsto:
   
        info:    Executing command vm create
        warn:    --vm-size has not been specified. Defaulting to "Small".
        info:    Looking up image bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
        info:    Looking up virtual network
        info:    Looking up cloud service
        warn:    --location option will be ignored
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Retrieving storage accounts
        info:    Creating VM
        info:    OK
        info:    vm create command OK
   
   * **-l (o --location)** . Area di Azure in cui verrà creata la VM. Per questo scenario, *centralus*.
   * **-n (o --vm-name)** . Nome della VM da creare.
   * **-w (o --virtual-network-name)** . Nome della VNet in cui verrà creata la VM. 
   * **-S (o --static-ip)** . Indirizzo IP privato statico per il gruppo VM.
   * **TestService**. Nome del servizio cloud in cui verrà creata la VM.
   * **bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2**. Immagine utilizzata per creare la VM.
   * **adminuser**. Amministratore locale della VM di Windows.
   * <strong>AdminP@ssw0rd</strong>. Amministratore locale della password della VM di Windows.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Come recuperare le informazioni relative all'indirizzo IP privato statico per una VM
Per visualizzare le informazioni relative all'indirizzo IP interno statico per la VM creata con lo script precedente, eseguire il comando dell’interfaccia di riga di comando di Azure seguente e osservare i valori per *Network StaticIp*:

    azure vm static-ip show DNS01

Output previsto:

    info:    Executing command vm static-ip show
    info:    Getting virtual machines
    data:    Network StaticIP "192.168.1.101"
    info:    vm static-ip show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Come rimuovere un indirizzo IP statico privato da una macchina virtuale
Per rimuovere l'indirizzo IP privato statico aggiunto alla VM nello script precedente, eseguire il seguente comando dell’interfaccia di riga di comando di Azure:

    azure vm static-ip remove DNS01

Output previsto:

    info:    Executing command vm static-ip remove
    info:    Getting virtual machines
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip remove command OK

## <a name="how-to-add-a-static-private-ip-to-an-existing-vm"></a>Come aggiungere un indirizzo IP statico privato a una VM esistente
Per aggiungere un indirizzo IP privato statico alla macchina virtuale creata usando lo script precedente, eseguire il comando seguente:

    azure vm static-ip set DNS01 192.168.1.101

Output previsto:

    info:    Executing command vm static-ip set
    info:    Getting virtual machines
    info:    Looking up virtual network
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip set command OK

## <a name="set-ip-addresses-within-the-operating-system"></a>Impostare gli indirizzi IP all'interno del sistema operativo

È consigliabile non assegnare staticamente l'indirizzo IP privato assegnato alla macchina virtuale di Azure all'interno del sistema operativo di una macchina virtuale, se non necessario. Se si imposta manualmente l'indirizzo IP privato all'interno del sistema operativo, assicurarsi che sia uguale all'indirizzo IP privato assegnato alla macchina virtuale di Azure. In caso contrario, si può perdere la connettività alla macchina virtuale. Non assegnare manualmente l'indirizzo IP pubblico assegnato a una macchina virtuale di Azure all'interno del sistema operativo della macchina virtuale.

## <a name="next-steps"></a>Passaggi successivi
* Informazioni su [indirizzi IP pubblici riservati](virtual-networks-reserved-public-ip.md) .
* Informazioni su [indirizzi IP pubblici a livello di istanza (ILPIP)](virtual-networks-instance-level-public-ip.md) .
* Consultare le [API REST dell'indirizzo IP riservato](https://msdn.microsoft.com/library/azure/dn722420.aspx).