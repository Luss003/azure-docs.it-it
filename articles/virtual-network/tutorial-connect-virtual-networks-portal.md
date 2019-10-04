---
title: Connettere reti virtuali con il peering reti virtuali - Esercitazione - Portale di Azure | Microsoft Docs
description: Questa esercitazione illustra come connettere reti virtuali con il peering di rete virtuale usando il portale di Azure.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
Customer intent: I want to connect two virtual networks so that virtual machines in one virtual network can communicate with virtual machines in the other virtual network.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 08/16/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: 943cad871330e2f3b6e13b33dca582ab545fe4be
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2019
ms.locfileid: "64726561"
---
# <a name="tutorial-connect-virtual-networks-with-virtual-network-peering-using-the-azure-portal"></a>Esercitazione: Connettere reti virtuali con il peering reti virtuali usando il portale di Azure

È possibile connettere due reti virtuali tra loro con il peering reti virtuali. Queste reti virtuali possono trovarsi in aree geografiche uguali o diverse (detto anche peering di reti virtuali globale). Dopo che è stato eseguito il peering, le risorse delle due reti virtuali possono comunicare tra loro con la stessa larghezza di banda e la stessa latenza che sarebbero disponibili se si trovassero nella stessa rete virtuale. In questa esercitazione si apprenderà come:

> [!div class="checklist"]
> * Creare due reti virtuali
> * Connettere due reti virtuali con un peering di reti virtuali
> * Distribuire una macchina virtuale in ogni rete virtuale
> * Stabilire la comunicazione tra le macchine virtuali

Se si preferisce, è possibile completare questa esercitazione usando l'[interfaccia della riga di comando di Azure](tutorial-connect-virtual-networks-cli.md) oppure [Azure PowerShell](tutorial-connect-virtual-networks-powershell.md).

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.

## <a name="log-in-to-azure"></a>Accedere ad Azure 

Accedere al portale di Azure all'indirizzo https://portal.azure.com.

## <a name="create-virtual-networks"></a>Creare reti virtuali

1. Selezionare **+ Crea una risorsa** nell'angolo in alto a sinistra del portale di Azure.
2. Selezionare **Rete** e quindi **Rete virtuale**.
3. Immettere o selezionare le informazioni seguenti, accettare le impostazioni predefinite rimanenti e quindi selezionare **Crea**:

    |Impostazione|Valore|
    |---|---|
    |NOME|myVirtualNetwork1|
    |Spazio degli indirizzi|10.0.0.0/16|
    |Sottoscrizione| Selezionare la propria sottoscrizione.|
    |Gruppo di risorse| Selezionare **Crea nuovo** e immettere *myResourceGroup*.|
    |Località| Selezionare **Stati Uniti orientali**.|
    |Nome della subnet|Subnet1|
    |Intervallo di indirizzi subnet|10.0.0.0/24|

      ![Crea rete virtuale](./media/tutorial-connect-virtual-networks-portal/create-virtual-network.png)

4. Ripetere i passaggi da 1 a 3, con le modifiche seguenti.

    |Impostazione|Valore|
    |---|---|
    |NOME|myVirtualNetwork2|
    |Spazio degli indirizzi|10.1.0.0/16|
    |Gruppo di risorse| Selezionare **Usa esistente** e quindi **myResourceGroup**.|
    |Intervallo di indirizzi subnet|10.1.0.0/24|

## <a name="peer-virtual-networks"></a>Eseguire il peering delle reti virtuali

1. Nella casella di ricerca nella parte superiore del portale di Azure iniziare a digitare *MyVirtualNetwork1*. Selezionare **myVirtualNetwork1** quando viene visualizzato nei risultati della ricerca.
2. Selezionare **Peer** in **IMPOSTAZIONI** e quindi **+ Aggiungi**, come illustrato nell'immagine seguente:

    ![Creare un peering](./media/tutorial-connect-virtual-networks-portal/create-peering.png)

3. Immettere o selezionare le informazioni seguenti, accettare le impostazioni predefinite rimanenti e quindi scegliere **OK**.

    |Impostazione|Valore|
    |---|---|
    |NOME|myVirtualNetwork1-myVirtualNetwork2|
    |Sottoscrizione| Selezionare la propria sottoscrizione.|
    |Rete virtuale|myVirtualNetwork2 - per selezionare la rete virtuale *myVirtualNetwork2*, selezionare **Rete virtuale** e quindi **myVirtualNetwork2**. È possibile selezionare una rete virtuale nella stessa area o in un'area differente.|

    ![Impostazioni del peering](./media/tutorial-connect-virtual-networks-portal/peering-settings.png)

    Lo stato riportato sotto **STATO PEERING** è *Avviato*, come illustrato nell'immagine seguente:

    ![Stato peering](./media/tutorial-connect-virtual-networks-portal/peering-status.png)

    Se lo stato non viene visualizzato, aggiornare il browser.

4. Nella **casella di ricerca** nella parte superiore del portale di Azure iniziare a digitare *MyVirtualNetwork2*. Selezionare **myVirtualNetwork2** quando viene visualizzato nei risultati della ricerca.
5. Ripetere i passaggi da 2 a 3, con le modifiche seguenti, quindi scegliere **OK**:

    |Impostazione|Valore|
    |---|---|
    |NOME|myVirtualNetwork2-myVirtualNetwork1|
    |Rete virtuale|myVirtualNetwork1|

    Lo stato riportato sotto **STATO PEERING** è *Connesso*. Azure avrà modificato anche lo stato del peering *myVirtualNetwork2-myVirtualNetwork1* da *Avviato* a *Connesso*. Il peering reti virtuali non è completamente stabilito finché lo stato del peering per entrambe le reti virtuali non è *Connesso*. 

## <a name="create-virtual-machines"></a>Creare macchine virtuali

Creare una macchina virtuale in ogni rete virtuale per poter stabilire la comunicazione tra di esse in un passaggio successivo.

### <a name="create-the-first-vm"></a>Creare la prima VM

1. Selezionare **+ Crea una risorsa** nell'angolo in alto a sinistra del portale di Azure.
2. Selezionare **Calcolo** e quindi **Windows Server 2016 Datacenter**. È possibile selezionare un sistema operativo differente, ma i passaggi rimanenti presuppongono che sia stato selezionato **Windows Server 2016 Datacenter**. 
3. Immettere o selezionare le informazioni seguenti in **Basics** (Generale), accettare le impostazioni predefinite rimanenti e quindi selezionare **Crea**:

    |Impostazione|Valore|
    |---|---|
    |NOME|myVm1|
    |Nome utente| Immettere un nome utente a scelta.|
    |Password| Immettere una password a scelta. La password deve contenere almeno 12 caratteri e soddisfare i [requisiti di complessità definiti](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Gruppo di risorse| Selezionare **Usa esistente** e quindi **myResourceGroup**.|
    |Località| Selezionare **Stati Uniti orientali**.|
4. Selezionare le dimensioni della macchina virtuale in **Scegli una dimensione**.
5. Selezionare i valori seguenti in **Impostazioni**, quindi scegliere **OK**:

    |Impostazione|Valore|
    |---|---|
    |Rete virtuale| myVirtualNetwork1 - se non è già selezionato, selezionare **Rete virtuale**, quindi selezionare **myVirtualNetwork1** in **Scegli rete virtuale**.|
    |Subnet| Subnet1 - se non è già selezionato, selezionare **Subnet**, quindi selezionare **Subnet1** in **Scegli subnet**.|
    
    ![Impostazioni della macchina virtuale](./media/tutorial-connect-virtual-networks-portal/virtual-machine-settings.png)
 
6. In **Crea** in **Riepilogo** selezionare **Crea** per avviare la distribuzione della macchina virtuale.

### <a name="create-the-second-vm"></a>Creare la seconda VM

Ripetere i passaggi da 1 a 6, con le modifiche seguenti:

|Impostazione|Valore|
|---|---|
|NOME | myVM2|
|Rete virtuale | myVirtualNetwork2|

La creazione delle macchine virtuali può richiedere alcuni minuti. Non procedere con i passaggi rimanenti finché non sono state create entrambe le macchine virtuali.

## <a name="communicate-between-vms"></a>Stabilire la comunicazione tra le macchine virtuali

1. Nella casella *Cerca* nella parte superiore del portale iniziare a digitare *myVm1*. Selezionare **myVm1** quando viene visualizzato nei risultati della ricerca.
2. Creare una connessione Desktop remoto alla macchina virtuale *myVm1* selezionando **Connetti**, come illustrato nell'immagine seguente:

    ![Connettersi alla macchina virtuale](./media/tutorial-connect-virtual-networks-portal/connect-to-virtual-machine.png)  

3. Per connettersi alla macchina virtuale, aprire il file con estensione rdp scaricato. Quando richiesto, selezionare **Connetti**.
4. Immettere il nome utente e la password specificati durante la creazione della macchina virtuale (potrebbe essere necessario selezionare **Altre opzioni**, quindi **Usa un account diverso** per specificare le credenziali immesse quando è stata creata la macchina virtuale), quindi scegliere **OK**.
5. Durante il processo di accesso potrebbe essere visualizzato un avviso relativo al certificato. Selezionare **Sì** per procedere con la connessione.
6. In un passaggio successivo verrà usato il ping per comunicare con la macchina virtuale *myVm2* dalla macchina virtuale *myVm1*. Il ping usa il protocollo ICMP (Internet Control Message Protocol), che viene rifiutato tramite Windows Firewall per impostazione predefinita. Nella macchina virtuale *myVm1* abilitare il protocollo ICMP attraverso Windows Firewall, in modo che sia possibile effettuare il ping della macchina virtuale da *myVm2* in un passaggio successivo tramite PowerShell:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```
    
    Anche se in questa esercitazione viene usato il ping per la comunicazione tra le macchine virtuali, non è consigliabile consentire il protocollo ICMP tramite Windows Firewall per le distribuzioni di produzione.

7. Per stabilire la connessione alla macchina virtuale *myVm2*, immettere il comando seguente da un prompt dei comandi nella macchina virtuale *myVm1*:

    ```
    mstsc /v:10.1.0.4
    ```
    
8. Dato che è stato abilitato il ping in *myVm1*, è ora possibile effettuare il ping di tale macchina virtuale in base all'indirizzo IP:

    ```
    ping 10.0.0.4
    ```
    
9. Disconnettere le sessioni RDP con *myVm1* e *myVm2*.

## <a name="clean-up-resources"></a>Pulire le risorse

Quando non sono più necessari, eliminare il gruppo di risorse e tutte le risorse in esso contenute: 

1. Immettere *myResourceGroup* nella casella di **ricerca** nella parte superiore del portale. Selezionare **myResourceGroup** quando viene visualizzato nei risultati della ricerca.
2. Selezionare **Elimina gruppo di risorse**.
3. Immettere *myResourceGroup* in **DIGITARE IL NOME DEL GRUPPO DI RISORSE** e selezionare **Elimina**.

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione è stato illustrato come connettere due reti nella stessa area di Azure con il peering di rete virtuale. È anche possibile creare un peering di reti virtuali in [aree supportate](virtual-network-manage-peering.md#cross-region) diverse e in [sottoscrizioni di Azure diverse](create-peering-different-subscriptions.md#portal) e creare [progettazioni di rete di tipo hub-spoke](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) con peering. Per altre informazioni sul peering di rete virtuale, vedere [Peering di rete virtuale](virtual-network-peering-overview.md) e [Gestire un peering di rete virtuale](virtual-network-manage-peering.md).

Per connettere il proprio computer a una rete virtuale tramite una VPN e interagire con le risorse in una rete virtuale o in reti virtuali associate tramite peering, vedere [Connettere un computer a una rete virtuale](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
