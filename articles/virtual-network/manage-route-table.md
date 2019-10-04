---
title: Creare, modificare o eliminare una tabella di route di Azure
titlesuffix: Azure Virtual Network
description: Informazioni su come creare, modificare o eliminare una tabella di route.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2018
ms.author: kumud
ms.openlocfilehash: a39d9f9c5a138ece5d40cc5afe1d1dcdd8e7a41a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "65849791"
---
# <a name="create-change-or-delete-a-route-table"></a>Creare, modificare o eliminare una tabella di route

Azure effettua il routing automatico del traffico tra subnet di Azure, reti virtuali e reti locali. Per modificare il routing predefinito di Azure è necessario creare una tabella di route. Se non si ha familiarità con il routing nelle reti virtuali, è possibile ottenere altre informazioni nella [panoramica del routing](virtual-networks-udr-overview.md) oppure completando un'[esercitazione](tutorial-create-route-table-portal.md).

## <a name="before-you-begin"></a>Prima di iniziare

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Prima di completare i passaggi di qualsiasi sezione di questo articolo, eseguire le attività seguenti:

* Se non si ha un account Azure, registrarsi per ottenere un [account per la versione di prova gratuita](https://azure.microsoft.com/free).<br>
* Se si usa il portale, aprire https://portal.azure.com e accedere con l'account di Azure.<br>
* Se si usano i comandi di PowerShell per completare le attività in questo articolo, eseguire i comandi in [Azure Cloud Shell](https://shell.azure.com/powershell) o tramite PowerShell dal computer in uso. Azure Cloud Shell è una shell interattiva gratuita che può essere usata per eseguire la procedura di questo articolo. Include strumenti comuni di Azure preinstallati e configurati per l'uso con l'account. Questa esercitazione richiede il modulo Azure PowerShell 1.0.0 o versioni successive. Eseguire `Get-Module -ListAvailable Az` per trovare la versione installata. Se è necessario eseguire l'aggiornamento, vedere [Installare e configurare Azure PowerShell](/powershell/azure/install-az-ps). Se si esegue PowerShell in locale, è anche necessario eseguire `Connect-AzAccount` per creare una connessione con Azure.<br>
* Se si usano i comandi dell'interfaccia della riga di comando di Azure per completare le attività in questo articolo, eseguire i comandi in [Azure Cloud Shell](https://shell.azure.com/bash) o tramite l'interfaccia della riga di comando dal computer in uso. Questa esercitazione richiede l'interfaccia della riga di comando di Azure 2.0.31 o versioni successive. Eseguire `az --version` per trovare la versione installata. Se è necessario eseguire l'installazione o l'aggiornamento, vedere [Installare l'interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli). Se si esegue l'interfaccia della riga di comando di Azure in locale, è anche necessario eseguire `az login` per creare una connessione con Azure.

L'account è accedere o connettersi ad Azure, deve essere assegnato per il [collaboratore rete](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) ruolo o a un [ruolo personalizzato](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) assegnato le azioni appropriate elencate nella [autorizzazioni ](#permissions).

## <a name="create-a-route-table"></a>Creare una tabella di route

È previsto un limite al numero di tabelle di route che è possibile creare per ogni sottoscrizione e località di Azure. Per informazioni dettagliate, vedere [Limiti di Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

1. Nell'angolo superiore sinistro del portale selezionare **+ Crea una risorsa**.
1. Selezionare **Rete** e quindi **Tabella di route**.
1. Immettere un **Nome** per la tabella di route, selezionare **Sottoscrizione**, creare un nuovo **Gruppo di risorse** o selezionare un gruppo di risorse esistente, selezionare una **Località** e quindi scegliere **Crea**. Se si intende associare la tabella di route a una subnet in una rete virtuale connessa alla rete locale tramite un gateway VPN e si disabilita **propagazione delle route gateway di rete virtuale**, non sono le route locali propagate alle interfacce di rete nella subnet.

### <a name="create-route-table---commands"></a>Creare la tabella di route - comandi

* Interfaccia della riga di comando di Azure: [az network route-table create](/cli/azure/network/route-table/route)<br>
* PowerShell: [New-AzRouteTable](/powershell/module/az.network/new-azroutetable)

## <a name="view-route-tables"></a>Visualizzare tabelle di route

Nella casella di ricerca nella parte superiore del portale immettere *tabelle di route*. Selezionare **Tabelle di route** quando viene visualizzato nei risultati della ricerca. Vengono elencate le tabelle di route presenti nella sottoscrizione.

### <a name="view-route-table---commands"></a>Tabella di route visualizzazione - comandi

* Interfaccia della riga di comando di Azure: [az network route-table list](/cli/azure/network/route-table/route)<br>
* PowerShell: [Get-AzRouteTable](/powershell/module/az.network/get-azroutetable)

## <a name="view-details-of-a-route-table"></a>Visualizzare i dettagli di una tabella di route

1. Nella casella di ricerca nella parte superiore del portale immettere *tabelle di route*. Selezionare **Tabelle di route** quando viene visualizzato nei risultati della ricerca.
1. Selezionare nell'elenco la tabella di route di cui si vuole visualizzare i dettagli. In **IMPOSTAZIONI** è possibile visualizzare le **Route** presenti nella tabella di route e le **Subnet** a cui essa è associata.
1. Per altre informazioni sulle impostazioni comuni di Azure, vedere le informazioni seguenti:

    * [Log attività](../azure-monitor/platform/activity-logs-overview.md)<br>
    * [Controllo di accesso (IAM)](../role-based-access-control/overview.md)<br>
    * [Tag](../azure-resource-manager/resource-group-using-tags.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br>
    * [Blocchi](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br>
    * [Script di automazione](../azure-resource-manager/manage-resource-groups-portal.md#export-resource-groups-to-templates)

### <a name="view-details-of-route-table---commands"></a>Visualizzare i dettagli della tabella di route - comandi

* Interfaccia della riga di comando di Azure: [az network route-table show](/cli/azure/network/route-table/route)<br>
* PowerShell: [Get-AzRouteTable](/powershell/module/az.network/get-azroutetable)

## <a name="change-a-route-table"></a>Modificare una tabella di route

1. Nella casella di ricerca nella parte superiore del portale immettere *tabelle di route*. Selezionare **Tabelle di route** quando viene visualizzato nei risultati della ricerca.
1. Selezionare la tabella di route che si vuole modificare. Le modifiche più comuni sono l'[aggiunta](#create-a-route) o la [rimozione](#delete-a-route) di route e l'[associazione](#associate-a-route-table-to-a-subnet) e la [dissociazione](#dissociate-a-route-table-from-a-subnet) di tabelle di route a e da subnet.

### <a name="change-a-route-table---commands"></a>Modificare una tabella di route - comandi

* Interfaccia della riga di comando di Azure: [az network route-table update](/cli/azure/network/route-table/route)<br>
* PowerShell: [Set-AzRouteTable](/powershell/module/az.network/set-azroutetable)

## <a name="associate-a-route-table-to-a-subnet"></a>Associare una route a una subnet

A una subnet può essere associata una o nessuna tabella di route. Una tabella di route può essere associata a nessuna o a più subnet. Poiché le tabelle di route non sono associate a reti virtuali, è necessario associare una tabella di route a ogni subnet a cui si vuole associare la tabella di route. Traffico tutte in uscita viene instradato basato su route create all'interno di tabelle di route [route predefinite](virtual-networks-udr-overview.md#default), e le route propagate da una rete locale, se la rete virtuale è connessa a un (gateway di rete virtuale di Azure ExpressRoute o VPN). È possibile associare solo una tabella di route alle subnet delle reti virtuali presenti nella stessa località e sottoscrizione di Azure della tabella di route.

1. Nella casella di ricerca nella parte superiore del portale immettere *reti virtuali*. Selezionare **Reti virtuali** quando viene visualizzato nei risultati della ricerca.
1. Selezionare nell'elenco la rete virtuale contenente la subnet a cui si vuole associare una tabella di route.
1. Selezionare **Subnet** in **IMPOSTAZIONI**.
1. Selezionare la subnet a cui si vuole associare la tabella di route.
1. Selezionare **Tabella di route**, scegliere la tabella di route che si vuole associare alla subnet e quindi selezionare **Salva**.

Se la rete virtuale è connessa a un gateway VPN di Azure, non associare una tabella di route alla [subnet del gateway](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub) che include una route con una destinazione 0.0.0.0/0. Ciò potrebbe impedire il corretto funzionamento del gateway. Per altre informazioni sull'uso di 0.0.0.0/0 in una route, vedere [Routing del traffico di rete virtuale](virtual-networks-udr-overview.md#default-route).

### <a name="associate-a-route-table---commands"></a>Associare una tabella di route - comandi

* Interfaccia della riga di comando di Azure: [az network vnet subnet update](/cli/azure/network/vnet/subnet?view=azure-cli-latest)<br>
* PowerShell: [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig)

## <a name="dissociate-a-route-table-from-a-subnet"></a>Annullare l'associazione di una tabella di route da una subnet

Quando si annulla l'associazione di una tabella di route da una subnet, Azure instrada il traffico in base alle relative [route predefinite](virtual-networks-udr-overview.md#default).

1. Nella casella di ricerca nella parte superiore del portale immettere *reti virtuali*. Selezionare **Reti virtuali** quando viene visualizzato nei risultati della ricerca.
1. Selezionare la rete virtuale contenente la subnet da cui si vuole disassociare una tabella di route.
1. Selezionare **Subnet** in **IMPOSTAZIONI**.
1. Selezionare la subnet da cui si vuole annullare l'associazione della tabella di route.
1. Selezionare **Tabella di route**, **Nessuno** e quindi **Salva**.

### <a name="dissociate-a-route-table---commands"></a>Annullare l'associazione di una tabella di route - comandi

* Interfaccia della riga di comando di Azure: [az network vnet subnet update](/cli/azure/network/vnet/subnet?view=azure-cli-latest)<br>
* PowerShell: [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig)

## <a name="delete-a-route-table"></a>Eliminare una tabella route

Se una tabella di route è associata a una subnet, non può essere eliminata. [Annullare l'associazione](#dissociate-a-route-table-from-a-subnet) di una tabella di route da tutte le subnet prima di tentare di eliminarla.

1. Nella casella di ricerca nella parte superiore del portale immettere *tabelle di route*. Selezionare **Tabelle di route** quando viene visualizzato nei risultati della ricerca.
1. Selezionare **...**  sul lato destro della tabella di route che si vuole eliminare.
1. Selezionare **Elimina** e quindi scegliere **Sì**.

### <a name="delete-a-route-table---commands"></a>Eliminare una tabella di route - comandi

* Interfaccia della riga di comando di Azure: [az network route-table delete](/cli/azure/network/route-table/route)<br>
* PowerShell: [Remove-AzRouteTable](/powershell/module/az.network/remove-azroutetable)

## <a name="create-a-route"></a>Creare una route

È previsto un limite al numero di route per tabella di route che è possibile creare per ogni sottoscrizione e località di Azure. Per informazioni dettagliate, vedere [Limiti di Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

1. Nella casella di ricerca nella parte superiore del portale immettere *tabelle di route*. Selezionare **Tabelle di route** quando viene visualizzato nei risultati della ricerca.
1. Selezionare nell'elenco la tabella di route a cui si vuole aggiungere una route.
1. Selezionare **Route** in **IMPOSTAZIONI**.
1. Selezionare **+ Aggiungi**.
1. Immettere un **Nome** univoco per la route all'interno della tabella di route.
1. Immettere il valore **Prefisso indirizzo**, sotto forma di notazione CIDR, a cui si vuole indirizzare il traffico. Il prefisso non può essere duplicato in più di una route all'interno della tabella di route, ma può essere contenuto in un altro prefisso. Se, ad esempio, è stato definito 10.0.0.0/16 come prefisso di una route, è possibile definire un'altra route con il prefisso di indirizzo 10.0.0.0/24. Azure seleziona una route per il traffico in base all'algoritmo LPM (Longest Prefix Match). Per altre informazioni su come vengono selezionate le route in Azure, vedere [Panoramica del routing](virtual-networks-udr-overview.md#how-azure-selects-a-route).
1. Selezionare una voce per **Tipo hop successivo**. Per una descrizione dettagliata di tutti i tipi di hop successivi, vedere [Panoramica del routing](virtual-networks-udr-overview.md).
1. Immettere un indirizzo IP per **Indirizzo hop successivo**. Se per *Indirizzo hop successivo* è stata selezionata la voce **Appliance virtuale**, è possibile immettere solo un indirizzo.
1. Selezionare **OK**.

### <a name="create-a-route---commands"></a>Creare una route - comandi

* Interfaccia della riga di comando di Azure: [az network route-table route create](/cli/azure/network/route-table/route?view=azure-cli-latest)<br>
* PowerShell: [New-AzRouteConfig](/powershell/module/az.network/new-azrouteconfig)

## <a name="view-routes"></a>Visualizzare le route

Una tabella di route può contenere nessuna o più route. Per saperne di più sulle informazioni elencate quando vengono visualizzate le route, vedere [Panoramica del routing](virtual-networks-udr-overview.md).

1. Nella casella di ricerca nella parte superiore del portale immettere *tabelle di route*. Selezionare **Tabelle di route** quando viene visualizzato nei risultati della ricerca.
1. Selezionare nell'elenco la tabella di route di cui si vogliono visualizzare le route.
1. Selezionare **Route** in **IMPOSTAZIONI**.

### <a name="view-routes---commands"></a>Visualizzare le route - comandi

* Interfaccia della riga di comando di Azure: [az network route-table route list](/cli/azure/network/route-table/route?view=azure-cli-latest)<br>
* PowerShell: [Get-AzRouteConfig](/powershell/module/az.network/get-azrouteconfig)

## <a name="view-details-of-a-route"></a>Visualizzare i dettagli di una route

1. Nella casella di ricerca nella parte superiore del portale immettere *tabelle di route*. Selezionare **Tabelle di route** quando viene visualizzato nei risultati della ricerca.
1. Selezionare la tabella di route in cui è contenuta la route di cui si vogliono visualizzare i dettagli.
1. Selezionare **Route**.
1. Selezionare le route di cui si vogliono visualizzare i dettagli.

### <a name="view-details-of-a-route---commands"></a>Visualizzare i dettagli di una route - comandi

* Interfaccia della riga di comando di Azure: [az network route-table route show](/cli/azure/network/route-table/route?view=azure-cli-latest)<br>
* PowerShell: [Get-AzRouteConfig](/powershell/module/az.network/get-azrouteconfig)

## <a name="change-a-route"></a>Modificare una route

1. Nella casella di ricerca nella parte superiore del portale immettere *tabelle di route*. Selezionare **Tabelle di route** quando viene visualizzato nei risultati della ricerca.
1. Selezionare la tabella di route per cui si vuole modificare una route.
1. Selezionare **Route**.
1. Selezionare la route che si vuole modificare.
1. Modificare le impostazioni esistenti con le nuove impostazioni e quindi selezionare **Salva**.

### <a name="change-a-route---commands"></a>Modificare una route - comandi

* Interfaccia della riga di comando di Azure: [az network route-table route update](/cli/azure/network/route-table/route?view=azure-cli-latest)<br>
* PowerShell: [Set-AzRouteConfig](/powershell/module/az.network/set-azrouteconfig)

## <a name="delete-a-route"></a>Eliminare una route

1. Nella casella di ricerca nella parte superiore del portale immettere *tabelle di route*. Selezionare **Tabelle di route** quando viene visualizzato nei risultati della ricerca.
1. Selezionare la tabella di route di cui si vuole eliminare una route.
1. Selezionare **Route**.
1. Nell'elenco di route selezionare **...**  sul lato destro della route che si vuole eliminare.
1. Selezionare **Elimina** e quindi scegliere **Sì**.

### <a name="delete-a-route---commands"></a>Eliminare una route - comandi

* Interfaccia della riga di comando di Azure: [az network route-table route delete](/cli/azure/network/route-table/route?view=azure-cli-latest)<br>
* PowerShell: [Remove-AzRouteConfig](/powershell/module/az.network/remove-azrouteconfig)

## <a name="view-effective-routes"></a>Visualizzare le route valide

Le route valide per ogni interfaccia di rete associata a una macchina virtuale sono costituite da una combinazione delle tabelle di route create, delle route predefinite di Azure e di eventuali route propagate da reti locali tramite BGP attraverso un gateway di rete virtuale di Azure. Conoscere le route valide per un'interfaccia di rete può essere utile durante la risoluzione dei problemi di routing. È possibile visualizzare le route valide per qualsiasi interfaccia di rete associata a una macchina virtuale in esecuzione.

1. Nella casella di ricerca nella parte superiore del portale immettere il nome di una macchina virtuale per la quale si vogliono visualizzare le route valide. Se non si conosce il nome della macchina virtuale, immettere *macchine virtuali* nella casella di ricerca. Quando **Macchine virtuali** viene visualizzato nei risultati della ricerca, selezionarlo e scegliere una macchina virtuale dall'elenco.
1. Selezionare **Rete** in **IMPOSTAZIONI**.
1. Selezionare il nome di un'interfaccia di rete.
1. Selezionare **Route valide** in **SUPPORTO + RISOLUZIONE DEI PROBLEMI**.
1. Esaminare l'elenco delle route valide per determinare se è presente la route corretta per la destinazione a cui si vuole indirizzare il traffico. Per altre informazioni sui tipi di hop successivi visualizzati nell'elenco, vedere [Panoramica del routing](virtual-networks-udr-overview.md).

### <a name="view-effective-routes---commands"></a>Visualizzare le route valide - comandi

* Interfaccia della riga di comando di Azure: [az network nic show-effective-route-table](/cli/azure/network/nic?view=azure-cli-latest)<br>
* PowerShell: [Get-AzEffectiveRouteTable](/powershell/module/az.network/get-azeffectiveroutetable)

## <a name="validate-routing-between-two-endpoints"></a>Convalidare il routing tra due endpoint

È possibile determinare il tipo di hop successivo tra una macchina virtuale e l'indirizzo IP di un'altra risorsa di Azure, una risorsa locale o una risorsa in Internet. Determinare il routing di Azure può essere utile durante la risoluzione dei problemi di routing. Per completare questa attività, è necessario disporre di un Network Watcher esistente. Se non è disponibile, crearne uno eseguendo la procedura descritta in [Creare un'istanza di Network Watcher](../network-watcher/network-watcher-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

1. Nella casella di ricerca nella parte superiore del portale immettere *network watcher*. Selezionare **Network Watcher** quando viene visualizzato tra i risultati della ricerca.
1. Selezionare **Hop successivo** in **Strumenti di diagnostica di rete**.
1. Selezionare la **Sottoscrizione** e il **Gruppo di risorse** della macchina virtuale di origine di cui si vuole convalidare il routing.
1. Selezionare la **Macchina virtuale**, l'**Interfaccia di rete** associata alla macchina virtuale e l'**Indirizzo IP di origine** assegnato all'interfaccia di rete di cui si vuole convalidare il routing.
1. Immettere l'**Indirizzo IP di destinazione** di cui si vuole convalidare il routing.
1. Selezionare **Hop successivo**.
1. Dopo un breve periodo di attesa, vengono restituite alcune informazioni che indicano il tipo di hop successivo e l'ID della route che ha indirizzato il traffico. Per altre informazioni sui tipi di hop successivi restituiti, vedere [Panoramica del routing](virtual-networks-udr-overview.md).

### <a name="validate-routing-between-two-endpoints---commands"></a>Convalidare il routing tra due endpoint - comandi

* Interfaccia della riga di comando di Azure: [az network watcher show-next-hop](/cli/azure/network/watcher?view=azure-cli-latest)<br>
* PowerShell: [Get-AzNetworkWatcherNextHop](/powershell/module/az.network/get-aznetworkwatchernexthop)

## <a name="permissions"></a>Autorizzazioni

Per eseguire attività nelle route e nelle tabelle di route, l'account deve essere assegnato al ruolo [Collaboratore Rete](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) o a un ruolo [personalizzato](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) a cui sono assegnate le operazioni appropriate elencate nella tabella seguente:

| Azione                                                          |   Name                                                  |
|--------------------------------------------------------------   |   -------------------------------------------           |
| Microsoft.Network/routeTables/read                              |   Leggere una tabella di route                                    |
| Microsoft.Network/routeTables/write                             |   Creare o aggiornare una tabella di route                        |
| Microsoft.Network/routeTables/delete                            |   Eliminare una tabella route                                  |
| Microsoft.Network/routeTables/join/action                       |   Associare una route a una subnet                   |
| Microsoft.Network/routeTables/routes/read                       |   Leggere una route                                          |
| Microsoft.Network/routeTables/routes/write                      |   Creare o aggiornare una route                              |
| Microsoft.Network/routeTables/routes/delete                     |   Eliminare una route                                        |
| Microsoft.Network/networkInterfaces/effectiveRouteTable/action  |   Ottenere la tabella di route valida per un'interfaccia di rete |
| Microsoft.Network/networkWatchers/nextHop/action                |   Ottenere l'hop successivo da una macchina virtuale                           |

## <a name="next-steps"></a>Passaggi successivi

* Creare una tabella di route usando gli script di esempio di [PowerShell](powershell-samples.md) o dell'[interfaccia della riga di comando di Azure](cli-samples.md) oppure i [modelli di Azure Resource Manager](template-samples.md)<br>
* Creare e applicare i [criteri di Azure](policy-samples.md) per le reti virtuali
