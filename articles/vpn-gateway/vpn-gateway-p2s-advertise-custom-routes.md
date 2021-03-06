---
title: 'Gateway VPN di Azure: annunciare route personalizzate per client VPN P2S'
description: Passaggi per annunciare le route personalizzate ai client da punto a sito
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 11/11/2019
ms.author: cherylmc
ms.openlocfilehash: 7a904857b8aa0ed2aa18fc2a1b81fe31541e6f9e
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/17/2019
ms.locfileid: "74151903"
---
# <a name="advertise-custom-routes-for-p2s-vpn-clients"></a>Annunciare route personalizzate per client VPN P2S

Si consiglia di annunciare route personalizzate a tutti i client VPN da punto a sito. Ad esempio, quando sono stati abilitati gli endpoint di archiviazione nella VNet e si vuole che gli utenti remoti possano accedere a questi account di archiviazione tramite la connessione VPN. È possibile annunciare l'indirizzo IP dell'endpoint di archiviazione a tutti gli utenti remoti in modo che il traffico verso l'account di archiviazione venga spostato sul tunnel VPN e non sulla rete Internet pubblica.

![Esempio di connessione gateway VPN di Azure multisito](./media/vpn-gateway-p2s-advertise-custom-routes/custom-routes.png)

## <a name="to-advertise-custom-routes"></a>Per annunciare route personalizzate

Per annunciare route personalizzate, usare il `Set-AzVirtualNetworkGateway cmdlet`. Nell'esempio seguente viene illustrato come annunciare l'IP per le [tabelle dell'account di archiviazione contoso](https://contoso.table.core.windows.net).

1. Ping *contoso.Table.Core.Windows.NET* e annotare l'indirizzo IP. Ad esempio:

    ```cmd
    C:\>ping contoso.table.core.windows.net
    Pinging table.by4prdstr05a.store.core.windows.net [13.88.144.250] with 32 bytes of data:
    ```

2. Eseguire i comandi di PowerShell seguenti:

    ```azurepowershell-interactive
    $gw = Get-AzVirtualNetworkGateway -Name <name of gateway> -ResourceGroupName <name of resource group>
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -CustomRoute 13.88.144.250/32
    ```

3. Per aggiungere più route personalizzate, usare un coma e spazi per separare gli indirizzi. Ad esempio:

    ```azurepowershell-interactive
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -CustomRoute x.x.x.x/xx , y.y.y.y/yy
    ```
## <a name="to-view-custom-routes"></a>Per visualizzare le route personalizzate

Usare l'esempio seguente per visualizzare le route personalizzate:

  ```azurepowershell-interactive
  $gw = Get-AzVirtualNetworkGateway -Name <name of gateway> -ResourceGroupName <name of resource group>
  $gw.CustomRoutes | Format-List
  ```
## <a name="to-delete-custom-routes"></a>Per eliminare route personalizzate

Per eliminare route personalizzate, usare l'esempio seguente:

  ```azurepowershell-interactive
  $gw = Get-AzVirtualNetworkGateway -Name <name of gateway> -ResourceGroupName <name of resource group>
  Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -CustomRoute @0
  ```
## <a name="next-steps"></a>Passaggi successivi

Per ulteriori informazioni sul routing di P2S, vedere informazioni [sul routing da punto a sito](vpn-gateway-about-point-to-site-routing.md).
