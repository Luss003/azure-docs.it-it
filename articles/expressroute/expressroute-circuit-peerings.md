---
title: Circuiti e peering di Azure ExpressRoute| Microsoft Docs
description: Questa pagina fornisce una panoramica dei circuiti e dei domini di routing/peering di ExpressRoute.
services: expressroute
author: mialdrid
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/18/2019
ms.author: mialdrid
ms.custom: seodec18
ms.openlocfilehash: 864b834fcc6810b52f067d8e67b4a48febd0f787
ms.sourcegitcommit: fad368d47a83dadc85523d86126941c1250b14e2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/19/2019
ms.locfileid: "71123485"
---
# <a name="expressroute-circuits-and-peering"></a>Circuiti e peering ExpressRoute

I circuiti ExpressRoute connettono un'infrastruttura locale a Microsoft tramite un provider di connettività. Questo articolo illustra i circuiti e i domini di routing/peering ExpressRoute. La figura riportata di seguito fornisce una rappresentazione logica della connettività fra la rete WAN e Microsoft.

![](./media/expressroute-circuit-peerings/expressroute-basic.png)

> [!IMPORTANT]
> Il peering pubblico di Azure è stato deprecato e non è disponibile per i nuovi circuiti ExpressRoute. I nuovi circuiti supportano il peering Microsoft e il peering privato.  
>

## <a name="circuits"></a>Circuiti ExpressRoute

Un circuito ExpressRoute rappresenta una connessione logica tra un'infrastruttura locale e i servizi cloud Microsoft tramite un provider di connettività. È possibile ordinare più circuiti ExpressRoute. Ciascun circuito può essere nella stessa regione o in regioni diverse, e può essere collegato ai locali tramite provider di connettività diversi.

I circuiti ExpressRoute non sono mappati ad alcuna entità fisica. Un circuito è identificato in modo univoco da un GUID standard denominato chiave di servizio. La chiave di servizio è l'unica informazione scambiata tra Microsoft, il provider di connettività e l'utente. Non è un segreto usato ai fini della sicurezza. Esiste un mapping 1:1 tra un circuito ExpressRoute e la chiave di servizio.

I nuovi circuiti ExpressRoute possono includere due peering indipendenti: peering privato e peering Microsoft. I circuiti ExpressRoute esistenti possono invece contenere tre peering: Pubblico di Azure, Privato di Azure e Microsoft. Ogni peering è una coppia di sessioni BGP indipendenti, ognuna configurata in modo ridondante per garantire la disponibilità elevata. Esiste un mapping 1:N (1 <= N <= 3) tra un circuito ExpressRoute e i domini di routing. Per un circuito ExpressRoute può essere abilitato uno, due o tutti e tre i peering.

Ogni circuito prevede una larghezza di banda fissa (50 Mbps, 100 Mbps, 200 Mbps, 500 Mbps, 1 Gbps, 10 Gbps) ed è mappato a un provider di connettività e a una località di peering. La larghezza di banda selezionata viene condivisa tra tutti i peering del circuito

### <a name="quotas"></a>Quote, limiti e limitazioni

Per ogni circuito ExpressRoute si applicano quote e limiti predefiniti. Per informazioni aggiornate sulle quote, vedere la pagina [Sottoscrizione di Azure e limiti dei servizi, quote e vincoli](../azure-subscription-service-limits.md) .

## <a name="routingdomains"></a>Peering ExpressRoute

A un circuito ExpressRoute sono associati più domini di routing/peering: Pubblico di Azure, Privato di Azure e Microsoft. Ogni peering è configurato in modo identico in una coppia di router (in una configurazione di tipo attivo-attivo o di condivisione del carico) per offrire una disponibilità elevata. Per rappresentare gli schemi di indirizzamento IP, i servizi di Azure sono classificati come *pubblici di Azure* e *privati di Azure*.

![](./media/expressroute-circuit-peerings/expressroute-peerings.png)

### <a name="privatepeering"></a>Peering privato di Azure

I servizi di calcolo di Azure, ovvero le macchine virtuali (IaaS) e i servizi cloud (PaaS), che sono distribuiti all'interno di una rete virtuale possono essere connessi tramite il dominio di peering privato. Il dominio di peering privato viene considerato un'estensione attendibile della rete di base in Microsoft Azure. È possibile configurare la connettività bidirezionale tra la rete di base e le reti virtuali (VNet) di Azure. Questo peering permette la connessione diretta a macchine virtuali e servizi cloud nei rispettivi indirizzi IP privati.  

È possibile connettere più di una rete virtuale al dominio di peering privato. Per informazioni su limiti e limitazioni, vedere la [pagina relativa alle domande frequenti](expressroute-faqs.md) . Per informazioni aggiornate sui limiti, visitare la pagina [Sottoscrizione di Azure e limiti dei servizi, quote e vincoli](../azure-subscription-service-limits.md) .  Per informazioni sulla configurazione del routing, vedere la pagina relativa al [routing](expressroute-routing.md) .

### <a name="microsoftpeering"></a>Peering Microsoft

[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

La connettività a Microsoft Servizi online (Office 365 e i servizi PaaS di Azure) si verifica tramite il peering Microsoft. La connettività bidirezionale tra la rete WAN e i servizi cloud Microsoft verrà abilitata tramite il dominio di routing di peering Microsoft. È necessario connettersi ai servizi cloud Microsoft solo tramite indirizzi IP pubblici di cui si è proprietari o di proprietà del proprio provider di connettività ed è necessario rispettare tutte le regole definite. Per altre informazioni, vedere la pagina [Prerequisiti per ExpressRoute](expressroute-prerequisites.md).

Per altre informazioni sui servizi supportati, sui costi e sui dettagli per la configurazione, vedere la [pagina relativa alle domande frequenti](expressroute-faqs.md) . Per informazioni sull'elenco di provider di connettività che offrono supporto per il peering Microsoft, vedere la pagina relativa alle [località per ExpressRoute](expressroute-locations.md) .

### <a name="publicpeering"></a>Peering pubblico di Azure (deprecato per i circuiti nuovi)

> [!Note]
> Il peering pubblico di Azure ha 1 indirizzo IP NAT associato a ogni sessione BGP. Per più di 2 indirizzi IP NAT, passare al peering Microsoft. Il peering Microsoft consente di configurare le proprie allocazioni NAT, nonché di usare i filtri di route per gli annunci con prefisso selettivo. Per altre informazioni, vedere [passare al peering Microsoft](https://docs.microsoft.com/azure/expressroute/how-to-move-peering).
>

I servizi quali Archiviazione, database SQL e Siti Web di Azure vengono offerti su indirizzi IP pubblici. È possibile connettersi privatamente ai servizi ospitati su indirizzi IP pubblici, inclusi gli indirizzi VIP dei servizi cloud, tramite il dominio di routing di peering pubblico. È possibile connettere il dominio di peering pubblico al DMZ e connettersi a tutti i servizi di Azure sui rispettivi indirizzi IP pubblici dalla rete WAN senza doversi connettere a Internet.

La connettività viene sempre attivata dalla rete WAN verso i servizi di Microsoft Azure. I servizi di Microsoft Azure non potranno attivare connessioni alla rete dell'utente tramite questo dominio di routing. Dopo l'abilitazione del peering pubblico, è possibile connettersi a tutti i servizi di Azure. Non è consentito scegliere in modo selettivo i servizi per cui vengono annunciate route.

È possibile definire filtri di route personalizzati nella propria rete per usare solo le route necessarie. Per informazioni sulla configurazione del routing, vedere la pagina relativa al [routing](expressroute-routing.md) .

Per altre informazioni sui servizi supportati tramite il dominio di routing di peering pubblico, vedere le [domande frequenti](expressroute-faqs.md).

## <a name="peeringcompare"></a>Confronto tra i peering

La tabella seguente confronta i tre peering:

|  | **Peering privato** | **Peering Microsoft** |  **Peering pubblico** (deprecato per i circuiti nuovi) |
| --- | --- | --- | --- |
| **Numero massimo di prefissi supportati per peering** |4000 per impostazione predefinita, 10.000 con ExpressRoute Premium |200 |200 |
| **Intervalli di indirizzi IP supportati** |Qualsiasi indirizzo IP valido entro la rete WAN. |Indirizzi IP pubblici di proprietà dell'utente o del provider di connettività. |Indirizzi IP pubblici di proprietà dell'utente o del provider di connettività. |
| **Requisiti del numero AS** |Numeri AS pubblici e privati. È necessario possedere un numero AS pubblico se si sceglie di usarne uno. |Numeri AS pubblici e privati. È tuttavia necessario dimostrare la proprietà degli indirizzi IP pubblici. |Numeri AS pubblici e privati. È tuttavia necessario dimostrare la proprietà degli indirizzi IP pubblici. |
| **Protocolli IP supportati**| IPv4 |  IPv4, IPv6 | IPv4 |
| **Indirizzi IP per l'interfaccia di routing** |Indirizzi IP pubblici e RFC1918 |Indirizzi IP pubblici registrati a nome dell'utente nei registri di routing. |Indirizzi IP pubblici registrati a nome dell'utente nei registri di routing. |
| **Supporto per Hash MD5** |Yes |Yes |Yes |

È possibile scegliere di abilitare uno o più domini di routing come parte del circuito ExpressRoute. È possibile scegliere di posizionare tutti i domini di routing nella stessa rete VPN se si vuole combinarli in un singolo dominio di routing. È anche possibile posizionarli in domini di routing diversi, in modo analogo a quanto illustrato nel diagramma. La configurazione consigliata consiste nel connettere il peering privato direttamente alla rete di base e i collegamenti del peering pubblico e Microsoft alla rete perimetrale.

Ogni peering richiede sessioni BGP separati (una coppia per ogni tipo di peering). Le coppie di sessioni BGP forniscono un collegamento a disponibilità elevata. Se si stabilisce la connessione tramite provider di connettività di livello 2, l'utente è responsabile della configurazione e della gestione del routing. È possibile ottenere più informazioni esaminando i [flussi di lavoro](expressroute-workflows.md) per la configurazione di ExpressRoute.

## <a name="health"></a>Integrità di ExpressRoute

La disponibilità, la connettività alle reti virtuali e l'utilizzo della larghezza di banda dei circuiti ExpressRoute possono essere monitorati usando [Monitoraggio prestazioni di rete](https://docs.microsoft.com/azure/networking/network-monitoring-overview).

Monitoraggio prestazioni rete verifica l'integrità del peering privato di Azure e del peering Microsoft. Per altre informazioni, vedere questo [post](https://azure.microsoft.com/blog/monitoring-of-azure-expressroute-in-preview/).

## <a name="next-steps"></a>Passaggi successivi

* Trovare un provider di servizi. Vedere l'articolo relativo ai [provider di servizi e alle località ExpressRoute](expressroute-locations.md).
* Verificare che vengano soddisfatti tutti i prerequisiti. Vedere [Prerequisiti per ExpressRoute](expressroute-prerequisites.md).
* Configurare la connessione ExpressRoute.
  * [Creare e gestire circuiti ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md)
  * [Configurare il routing (peering) per circuiti ExpressRoute](expressroute-howto-routing-portal-resource-manager.md)
