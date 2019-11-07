---
title: Panoramica dei gruppi di sicurezza di Azure
titlesuffix: Azure Virtual Network
description: Informazioni sui gruppi di sicurezza di rete e delle applicazioni. I gruppi di sicurezza consentono di filtrare il traffico di rete tra le risorse di Azure.
services: virtual-network
documentationcenter: na
author: malopMSFT
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2018
ms.author: malop
ms.reviewer: kumud
ms.openlocfilehash: 6046ab98e657cd14a2ac883cd32709c9a1b5da57
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/07/2019
ms.locfileid: "73721490"
---
# <a name="security-groups"></a>Gruppi di sicurezza
<a name="network-security-groups"></a>

È possibile filtrare il traffico di rete da e verso le risorse di Azure in una [rete virtuale](virtual-networks-overview.md) di Azure con un gruppo di sicurezza di rete. Un gruppo di sicurezza di rete contiene [regole di sicurezza](#security-rules) che consentono o rifiutano il traffico di rete in ingresso o in uscita da diversi tipi di risorse di Azure. Per informazioni sulle risorse di Azure che possono essere distribuite in una rete virtuale e sui gruppi di sicurezza di rete ad esse associati, vedere [Integrazione della rete virtuale per i servizi di Azure](virtual-network-for-azure-services.md). Per ogni regola è possibile specificare l'origine e la destinazione, la porta e il protocollo.

Questo articolo spiega i concetti dei gruppi di sicurezza di rete per un uso efficace. Se non è mai stato creato un gruppo di sicurezza di rete, è possibile completare una breve [esercitazione](tutorial-filter-network-traffic.md) per acquisire esperienza nella creazione di tale gruppo. Se si ha familiarità con i gruppi di sicurezza di rete e si ha la necessità di gestirli, vedere [Gestire un gruppo di sicurezza di rete](manage-network-security-group.md). Se si verificano problemi di comunicazione ed è necessario risolvere i problemi dei gruppi di sicurezza di rete, vedere [Diagnosticare problemi di filtro del traffico di rete di una macchina virtuale](diagnose-network-traffic-filter-problem.md). È possibile abilitare i [log di flusso dei gruppi di sicurezza di rete](../network-watcher/network-watcher-nsg-flow-logging-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) per [analizzare il traffico di rete](../network-watcher/traffic-analytics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) da e verso le risorse che hanno un gruppo di sicurezza di rete associato.

## <a name="security-rules"></a>Regole di sicurezza

Un gruppo di sicurezza di rete può contenere zero regole o il numero di regole desiderato, entro i [limiti](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) della sottoscrizione di Azure. Ogni regola specifica le proprietà seguenti:

|Proprietà  |Spiegazione  |
|---------|---------|
|Name|Nome univoco all'interno del gruppo di sicurezza di rete.|
|Priorità | Numero compreso tra 100 e 4096. Le regole vengono elaborate in ordine di priorità. I numeri più bassi vengono elaborati prima di quelli più elevati perché hanno priorità più alta. Quando il traffico corrisponde a una regola, l'elaborazione viene interrotta. Di conseguenza, le regole con priorità più bassa (numeri più elevati) che hanno gli stessi attributi di regole con priorità più elevata non vengono elaborate.|
|Origine o destinazione| Qualsiasi indirizzo IP, blocco CIDR (Classless Inter-Domain Routing), ad esempio 10.0.0.0/24, [tag di servizio](service-tags-overview.md) o [gruppo di sicurezza delle applicazioni](#application-security-groups). Se si specifica un indirizzo per una risorsa di Azure, specificare l'indirizzo IP privato assegnato alla risorsa. I gruppi di sicurezza della rete vengono elaborati dopo che Azure ha convertito un indirizzo IP pubblico in un indirizzo IP privato per il traffico in ingresso e prima che Azure converta un indirizzo IP privato in un indirizzo IP pubblico per il traffico in uscita. Vedere altre informazioni sugli [indirizzi IP](virtual-network-ip-addresses-overview-arm.md) di Azure. Specificando un intervallo, un tag di servizio o un gruppo di sicurezza delle applicazioni è possibile creare un minor numero di regole di sicurezza. La possibilità di specificare più intervalli e indirizzi IP singoli in una regola è detta [regola di sicurezza ottimizzata](#augmented-security-rules). Non si possono specificare più tag di servizio o gruppi di applicazioni. È possibile creare regole di sicurezza ottimizzate solo in gruppi di sicurezza di rete creati tramite il modello di distribuzione Resource Manager. Non si possono specificare più indirizzi IP e intervalli di indirizzi IP nei gruppi di sicurezza di rete creati tramite il modello di distribuzione classica. Vedere altre informazioni sui [modelli di distribuzione](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) di Azure.|
|Protocollo     | TCP, UDP, ICMP o any.|
|Direzione| Definisce se la regola si applica al traffico in ingresso o in uscita.|
|Intervallo di porte     |È possibile specificare una singola porta o un intervallo di porte. Ad esempio, è possibile specificare 80 oppure 10000-10005. Specificando intervalli è possibile creare un minor numero di regole di sicurezza. È possibile creare regole di sicurezza ottimizzate solo in gruppi di sicurezza di rete creati tramite il modello di distribuzione Resource Manager. Non si possono specificare più porte o intervalli di porte nella stessa regola di sicurezza nei gruppi di sicurezza di rete creati tramite il modello di distribuzione classica.   |
|Azione     | Consentire o impedire.        |

Le regole di sicurezza del gruppo di sicurezza di rete vengono valutate in base alla priorità, usando informazioni a 5 tuple (origine, porta di origine, destinazione, porta di destinazione e protocollo) per consentire o negare il traffico. Viene creato un record di flusso per le connessioni esistenti. La comunicazione è consentita o negata in base allo stato di connessione del record di flusso. Il record di flusso consente al gruppo di sicurezza di avere uno stato. Se si specifica una regola di sicurezza in uscita per qualsiasi indirizzo sulla porta 80, ad esempio, non è necessario specificare una regola di sicurezza in ingresso per la risposta al traffico in uscita. È necessario specificare una regola di sicurezza in ingresso solo se la comunicazione viene avviata all'esterno. Questa considerazione si applica anche al contrario. Se il traffico in ingresso è consentito su una porta, non è necessario specificare una regola di sicurezza in uscita per rispondere al traffico sulla porta.
Le connessioni esistenti non possono essere interrotte quando si rimuove una regola di sicurezza che abilita il flusso. I flussi di traffico vengono interrotti quando le connessioni vengono arrestate e non è presente alcun flusso di traffico in entrambe le direzioni almeno per alcuni minuti.

Il numero di regole di sicurezza che è possibile creare in un gruppo di sicurezza di rete è limitato. Per informazioni dettagliate, vedere [Limiti di Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="augmented-security-rules"></a>Regole di sicurezza ottimizzate

Le regole di sicurezza ottimizzate semplificano la definizione della sicurezza per le reti virtuali, perché consentono di definire criteri di sicurezza di rete più estesi e complessi con un minor numero di regole. È possibile combinare più porte e più indirizzi e intervalli IP espliciti in un'unica regola di sicurezza facilmente comprensibile. Usare regole ottimizzate nei campi relativi a origine, destinazione e porte di una regola. Per semplificare la gestione della definizione delle regole di sicurezza, combinare le regole di sicurezza ottimizzate con [tag di servizio](service-tags-overview.md) o [gruppi di sicurezza delle applicazioni](#application-security-groups). Il numero di indirizzi, intervalli e porte che è possibile specificare in una regola è limitato. Per informazioni dettagliate, vedere [Limiti di Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="service-tags"></a>Tag di servizio

Un tag di servizio rappresenta un gruppo di prefissi di indirizzi IP da un determinato servizio di Azure. Consente di ridurre al minimo la complessità degli aggiornamenti frequenti sulle regole di sicurezza di rete.

Per altre informazioni, vedere [tag dei servizi di Azure](service-tags-overview.md). 

## <a name="default-security-rules"></a>Regole di sicurezza predefinite

Azure crea le regole predefinite seguenti in ogni gruppo di sicurezza di rete creato:

### <a name="inbound"></a>In ingresso

#### <a name="allowvnetinbound"></a>AllowVNetInBound

|Priorità|Source|Porte di origine|Destination|Porte di destinazione|Protocollo|Accesso|
|---|---|---|---|---|---|---|
|65000|VirtualNetwork|0-65535|VirtualNetwork|0-65535|Qualsiasi|Consenti|

#### <a name="allowazureloadbalancerinbound"></a>AllowAzureLoadBalancerInBound

|Priorità|Source|Porte di origine|Destination|Porte di destinazione|Protocollo|Accesso|
|---|---|---|---|---|---|---|
|65001|AzureLoadBalancer|0-65535|0.0.0.0/0|0-65535|Qualsiasi|Consenti|

#### <a name="denyallinbound"></a>DenyAllInbound

|Priorità|Source|Porte di origine|Destination|Porte di destinazione|Protocollo|Accesso|
|---|---|---|---|---|---|---|
|65500|0.0.0.0/0|0-65535|0.0.0.0/0|0-65535|Qualsiasi|NEGA|

### <a name="outbound"></a>In uscita

#### <a name="allowvnetoutbound"></a>AllowVnetOutBound

|Priorità|Source|Porte di origine| Destination | Porte di destinazione | Protocollo | Accesso |
|---|---|---|---|---|---|---|
| 65000 | VirtualNetwork | 0-65535 | VirtualNetwork | 0-65535 | Qualsiasi | Consenti |

#### <a name="allowinternetoutbound"></a>AllowInternetOutBound

|Priorità|Source|Porte di origine| Destination | Porte di destinazione | Protocollo | Accesso |
|---|---|---|---|---|---|---|
| 65001 | 0.0.0.0/0 | 0-65535 | Internet | 0-65535 | Qualsiasi | Consenti |

#### <a name="denyalloutbound"></a>DenyAllOutBound

|Priorità|Source|Porte di origine| Destination | Porte di destinazione | Protocollo | Accesso |
|---|---|---|---|---|---|---|
| 65500 | 0.0.0.0/0 | 0-65535 | 0.0.0.0/0 | 0-65535 | Qualsiasi | NEGA |

Nelle colonne **Origine** e **Destinazione**, *VirtualNetwork*, *AzureLoadBalancer* e *Internet* sono [tag di servizio](service-tags-overview.md), anziché indirizzi IP. Nella colonna protocollo, **any** include TCP, UDP e ICMP. Quando si crea una regola, è possibile specificare TCP, UDP, ICMP o any. Nelle colonne *Origine* e **Destinazione**, **0.0.0.0/0** rappresenta tutti gli indirizzi. I client come portale di Azure, l'interfaccia della riga di comando di Azure o PowerShell possono usare * o any per questa espressione.
 
Non è possibile rimuovere le regole predefinite, ma è possibile eseguirne l'override creando regole con priorità più alta.

## <a name="application-security-groups"></a>Gruppi di sicurezza delle applicazioni

I gruppi di sicurezza delle applicazioni consentono di configurare la sicurezza di rete come un'estensione naturale della struttura di un'applicazione, raggruppando le macchine virtuali e definendo i criteri di sicurezza di rete in base a tali gruppi. È possibile riusare i criteri di sicurezza su larga scala senza gestire manualmente indirizzi IP espliciti. La piattaforma gestisce la complessità degli indirizzi IP espliciti e di più set di regole, consentendo all'utente di concentrarsi sulla logica di business. Per comprendere meglio i gruppi di sicurezza delle applicazioni, considerare l'esempio seguente:

![Gruppi di sicurezza delle applicazioni](./media/security-groups/application-security-groups.png)

Nell'immagine precedente, *NIC1* e *NIC2* sono membri del gruppo di sicurezza delle applicazioni *AsgWeb*. *NIC3* è un membro del gruppo di sicurezza delle applicazioni *AsgLogic*. *NIC4* è un membro del gruppo di sicurezza delle applicazioni *AsgDb*. Anche se in questo esempio ogni interfaccia di rete è membro di un solo gruppo di sicurezza delle applicazioni, un'interfaccia di rete può essere membro di più gruppi di sicurezza delle applicazioni, fino ai [limiti di Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Nessuna delle interfacce di rete ha un gruppo di sicurezza di rete associato. *NSG1* è associato a entrambe le subnet e contiene le regole seguenti:

### <a name="allow-http-inbound-internet"></a>Allow-HTTP-Inbound-Internet

Questa regola è necessaria per consentire il traffico da Internet verso i server Web. Dato che il traffico in ingresso da Internet viene negato dalla regola di sicurezza predefinita [DenyAllInbound](#denyallinbound), non sono necessarie regole aggiuntive per i gruppi di sicurezza delle applicazioni *AsgLogic* o *AsgDb*.

|Priorità|Source|Porte di origine| Destination | Porte di destinazione | Protocollo | Accesso |
|---|---|---|---|---|---|---|
| 100 | Internet | * | AsgWeb | 80 | TCP | Consenti |

### <a name="deny-database-all"></a>Deny-Database-All

Dato che la regola di sicurezza predefinita [AllowVNetInBound](#allowvnetinbound) consente tutte le comunicazioni tra le risorse nella stessa rete virtuale, questa regola è necessaria per negare il traffico da tutte le risorse.

|Priorità|Source|Porte di origine| Destination | Porte di destinazione | Protocollo | Accesso |
|---|---|---|---|---|---|---|
| 120 | * | * | AsgDb | 1433 | Qualsiasi | NEGA |

### <a name="allow-database-businesslogic"></a>Allow-Database-BusinessLogic

Questa regola consente il traffico dal gruppo di sicurezza delle applicazioni *AsgLogic* al gruppo di sicurezza delle applicazioni *AsgDb*. La priorità di questa regola è superiore a quella della regola *Deny-Database-All*. Di conseguenza, questa regola viene elaborata prima della regola *Deny-Database-All*, quindi il traffico dal gruppo di sicurezza delle applicazioni *AsgLogic* è consentito, mentre tutto il resto del traffico è bloccato.

|Priorità|Source|Porte di origine| Destination | Porte di destinazione | Protocollo | Accesso |
|---|---|---|---|---|---|---|
| 110 | AsgLogic | * | AsgDb | 1433 | TCP | Consenti |

Le regole che specificano un gruppo di sicurezza delle applicazioni come origine o destinazione vengono applicate solo alle interfacce di rete che sono membri del gruppo di sicurezza delle applicazioni. Se l'interfaccia di rete non è membro di un gruppo di sicurezza delle applicazioni, la regola non viene applicata all'interfaccia di rete, anche se il gruppo di sicurezza di rete è associato alla subnet.

I gruppi di sicurezza delle applicazioni hanno i vincoli seguenti:

-   Esistono limiti al numero di gruppi di sicurezza delle applicazioni in una sottoscrizione, oltre ad altri limiti correlati ai gruppi di sicurezza delle applicazioni. Per informazioni dettagliate, vedere [Limiti di Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Si può specificare un unico gruppo di sicurezza delle applicazioni come origine e destinazione in una regola di sicurezza. Non è possibile specificare più gruppi di sicurezza delle applicazioni nell'origine o nella destinazione.
- Tutte le interfacce di rete assegnate a un gruppo di sicurezza delle applicazioni devono esistere nella stessa rete virtuale in cui si trova la prima interfaccia di rete assegnata al gruppo di sicurezza delle applicazioni. Se ad esempio la prima interfaccia di rete assegnata a un gruppo di sicurezza delle applicazioni denominato *AsgWeb* si trova nella rete virtuale denominata *VNet1*, tutte le interfacce di rete successive assegnate a *ASGWeb* devono trovarsi in *VNet1*. Non è possibile aggiungere interfacce di rete da reti virtuali diverse allo stesso gruppo di sicurezza delle applicazioni.
- Se si specifica un gruppo di sicurezza delle applicazioni come origine e destinazione in una regola di sicurezza, le interfacce di rete in entrambi i gruppi di sicurezza delle applicazioni devono trovarsi nella stessa rete virtuale. Se ad esempio *AsgLogic* contenesse interfacce di rete di *VNet1* e *AsgDb* contenesse interfacce di rete di *VNet2*, non sarebbe possibile assegnare *AsgLogic* come origine e *AsgDb* come destinazione in una regola. Tutte le interfacce di rete per i gruppi di sicurezza delle applicazioni di origine e di destinazione devono esistere nella stessa rete virtuale.

> [!TIP]
> Per ridurre al minimo il numero di regole di sicurezza e la necessità di modificarle, pianificare i gruppi di sicurezza delle applicazioni necessari e creare regole usando tag di servizio o gruppi di sicurezza delle applicazioni, anziché singoli indirizzi IP o intervalli di indirizzi IP, quando possibile.

## <a name="how-traffic-is-evaluated"></a>Modalità di valutazione del traffico

È possibile distribuire le risorse di diversi servizi di Azure in una rete virtuale di Azure. Per un elenco completo, vedere [Servizi distribuibili in una rete virtuale](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network). È possibile associare zero o un gruppo di sicurezza di rete a ogni [subnet](virtual-network-manage-subnet.md#change-subnet-settings) e [interfaccia di rete](virtual-network-network-interface.md#associate-or-dissociate-a-network-security-group) della rete virtuale in una macchina virtuale. Lo stesso gruppo di sicurezza di rete può essere associato a un numero qualsiasi di subnet e interfacce di rete.

L'immagine seguente illustra diversi scenari per l'implementazione dei gruppi di sicurezza di rete per consentire il traffico di rete da e verso Internet tramite la porta TCP 80:

![Elaborazione dei gruppi di sicurezza di rete](./media/security-groups/nsg-interaction.png)

Vedere l'immagine precedente e il testo seguente per comprendere come Azure elabora le regole in ingresso e in uscita per i gruppi di sicurezza di rete:

### <a name="inbound-traffic"></a>Traffico in ingresso

Per il traffico in ingresso, Azure elabora prima le regole di un gruppo di sicurezza di rete associato a una subnet, se presente, quindi le regole di un gruppo di sicurezza di rete associato all'interfaccia di rete, se presente.

- **VM1**: le regole di sicurezza in *NSG1* vengono elaborate perché associate a *Subnet1* e *VM1* si trova in *Subnet1*. A meno che non sia stata creata una regola che consenta l'ingresso alla porta 80, il traffico viene negato dalla regola di sicurezza predefinita [DenyAllInbound](#denyallinbound) e non viene mai valutato da *NSG2*, perché *NSG2* è associato all'interfaccia di rete. Se *NSG1* ha una regola di sicurezza che consente la porta 80, il traffico verrà quindi elaborato da *NSG2*. Per consentire la porta 80 per la macchina virtuale, sia *NSG1* che *NSG2* devono avere una regola che consenta la porta 80 da Internet.
- **VM2**: le regole in *NSG1* vengono elaborate perché *VM2* si trova anche in *Subnet1*. Poiché *VM2* non ha un gruppo di sicurezza di rete associato alla propria interfaccia di rete, riceve tutto il traffico consentito attraverso *NSG1* oppure tutto il traffico viene negato da *NSG1*. Quando un gruppo di sicurezza di rete è associato a una subnet, il traffico è consentito o negato a tutte le risorse della stessa subnet.
- **VM3**: poiché a *Subnet2* non è associato alcun gruppo di sicurezza di rete, il traffico è consentito nella subnet ed elaborato da *NSG2*, perché *NSG2* è associato all'interfaccia di rete collegata a *VM3*.
- **VM4**: il traffico è consentito in *VM4,* perché a *Subnet3*o all'interfaccia di rete della macchina virtuale non è associato un gruppo di sicurezza di rete. È consentito tutto il traffico di rete attraverso una subnet e un'interfaccia di rete se questi componenti non hanno un gruppo di sicurezza di rete associato.

### <a name="outbound-traffic"></a>Traffico in uscita

Per il traffico in uscita, Azure elabora prima le regole di un gruppo di sicurezza di rete associato a un'interfaccia di rete, se presente, quindi le regole di un gruppo di sicurezza di rete associato alla subnet, se presente.

- **VM1**: vengono elaborate le regole di sicurezza presenti in *NSG2*. A meno che non si crei una regola di sicurezza che nega la porta 80 in uscita verso Internet, il traffico è consentito dalla regola di sicurezza predefinita [AlllowInternetOutbound](#allowinternetoutbound) in *NSG1* e *NSG2*. Se *NSG2* ha una regola di sicurezza che nega la porta 80, il traffico viene negato e mai valutato da *NSG1*. Per negare la porta 80 dalla macchina virtuale, uno o entrambi i gruppi di sicurezza di rete devono avere una regola che nega la porta 80 verso Internet.
- **VM2**: tutto il traffico viene inviato attraverso l'interfaccia di rete alla subnet perché l'interfaccia di rete collegata a *VM2* non ha un gruppo di sicurezza di rete associato. Vengono elaborate le regole in *NSG1*.
- **VM3**: se *NSG2* ha una regola di sicurezza che nega la porta 80, il traffico viene negato. Se *NSG2* ha una regola di sicurezza che consente la porta 80, la porta 80 è consentita in uscita verso Internet perché a *Subnet2* non è associato un gruppo di sicurezza di rete.
- **VM4**: tutto il traffico di rete è consentito da *VM4* perché a *Subnet3* o all'interfaccia di rete collegata alla macchina virtuale non è associato un gruppo di sicurezza di rete.


### <a name="intra-subnet-traffic"></a>Traffico all'interno della subnet

È importante notare che le regole di sicurezza in un NSG associato a una subnet possono influire sulla connettività tra le macchine virtuali. Se, ad esempio, viene aggiunta una regola a *NSG1* che nega tutto il traffico in ingresso e in uscita, *VM1* e *VM2* non saranno più in grado di comunicare tra loro. È necessario aggiungere un'altra regola in modo specifico per consentire questa operazione. 



Le regole di aggregazione applicate a un'interfaccia di rete possono essere verificate facilmente visualizzando le [regole di sicurezza effettive](virtual-network-network-interface.md#view-effective-security-rules) per un'interfaccia di rete. È anche possibile usare la funzionalità di [verifica del flusso IP](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json) in Azure Network Watcher per determinare se è consentita la comunicazione da o verso un'interfaccia di rete. La verifica del flusso IP indica se la comunicazione è consentita o negata e quale regola di sicurezza di rete consente o nega il traffico.

> [!NOTE]
> I gruppi di sicurezza di rete sono associati a subnet o a macchine virtuali e servizi cloud distribuiti nel modello di distribuzione classica e a subnet o interfacce di rete nel modello di distribuzione Gestione risorse. Per altre informazioni in proposito, vedere le [informazioni sui modelli di distribuzione di Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

> [!TIP]
> A meno che non ci siano motivi specifici, è consigliabile associare un gruppo di sicurezza di rete a una subnet o a un'interfaccia di rete, ma non a entrambe. Dato che le regole di un gruppo di sicurezza di rete associato a una subnet possono entrare in conflitto con quelle di un gruppo di sicurezza di rete associato a un'interfaccia di rete, è possibile che si verifichino problemi di comunicazione imprevisti che devono essere risolti.

## <a name="azure-platform-considerations"></a>Considerazioni sulla piattaforma Azure

- **IP virtuale del nodo host**: i servizi di infrastruttura di base come DHCP, DNS, IMDS e il monitoraggio dello stato vengono forniti tramite gli indirizzi IP host virtualizzati 168.63.129.16 e 169.254.169.254. Questi indirizzi IP appartengono a Microsoft e sono gli unici indirizzi IP virtualizzati usati in tutte le aree a questo scopo.
- **Licenze (servizio di gestione delle chiavi):** le immagini Windows in esecuzione nelle macchine virtuali devono essere concesse in licenza. Per verificare la concessione della licenza, viene inviata una richiesta ai server host del Servizio di gestione delle chiavi che gestiscono le query di questo tipo. La richiesta viene inviata in uscita tramite la porta 1688. Per le distribuzioni tramite la configurazione di [route predefinita 0.0.0.0/0](virtual-networks-udr-overview.md#default-route), questa regola di piattaforma sarà disabilitata.
- **Macchine virtuali in pool con carico bilanciato**: l'intervallo di porte e indirizzi di origine applicato è quello del computer di origine e non quello del servizio di bilanciamento del carico. L'intervallo di porte e indirizzi di destinazione riguarda il computer di destinazione e non il servizio di bilanciamento del carico.
- **Istanze di servizi di Azure**: nelle subnet delle reti virtuali vengono distribuite istanze di diversi servizi di Azure, ad esempio HDInsight, ambienti del servizio app e set di scalabilità di macchine virtuali. Per un elenco completo dei servizi che è possibile distribuire nelle reti virtuali, vedere l'articolo relativo alla [rete virtuale per i servizi di Azure](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network). Assicurarsi di acquisire familiarità con i requisiti relativi alle porte per ogni servizio prima di applicare un gruppo di sicurezza di rete alla subnet in cui è distribuita la risorsa. Se si bloccano le porte richieste dal servizio, il servizio non funzionerà correttamente.
- **Invio di messaggi di posta elettronica in uscita**: per inviare posta elettronica da macchine virtuali di Azure è consigliabile usare servizi di inoltro SMTP autenticato, in genere connessi tramite la porta TCP 587 ma spesso anche con altre. Sono disponibili servizi di inoltro SMTP specializzati per la reputazione del mittente, per ridurre al minimo la possibilità che provider di posta elettronica di terze parti rifiutino i messaggi. Tali servizi di inoltro SMTP includono, ad esempio, Exchange Online Protection e SendGrid. L'uso di servizi di inoltro SMTP non è soggetto ad alcuna restrizione in Azure, indipendentemente dal tipo di sottoscrizione. 

  Se la sottoscrizione di Azure è stata creata prima del 15 novembre 2017, oltre a poter usare servizi di inoltro SMTP è possibile inviare posta elettronica direttamente sulla porta TCP 25. Se la sottoscrizione è stata creata dopo il 15 novembre 2017, potrebbe non essere possibile inviare posta elettronica direttamente sulla porta 25. Il comportamento della comunicazione in uscita sulla porta 25 dipende dal tipo di sottoscrizione, come illustrato di seguito.

     - **Enterprise Agreement**: la comunicazione in uscita sulla porta 25 è consentita. È possibile inviare messaggi di posta elettronica in uscita direttamente dalle macchine virtuali a provider di posta elettronica esterni, senza restrizioni dalla piattaforma Azure. 
     - **Pagamento in base al consumo:** la comunicazione in uscita sulla porta 25 è bloccata per tutte le risorse. Se è necessario inviare posta elettronica direttamente da un macchina virtuale a provider di posta elettronica esterni, senza un inoltro SMTP autenticato, è necessario richiedere la rimozione della restrizione. Le richieste vengono esaminate e approvate a discrezione di Microsoft e vengono soddisfatte solo in seguito a controlli anti-frode. Per effettuare una richiesta, aprire un caso di supporto con il tipo di problema *Tecnico*, *Virtual Network Connectivity* (Connettività di rete virtuale), *Cannot send e-mail (SMTP/Port 25)* (Impossibile inviare posta elettronica - SMTP/porta 25). Nel caso di supporto includere informazioni dettagliate sui motivi per cui è necessario inviare posta elettronica dalla sottoscrizione a provider di posta direttamente anziché tramite un inoltro SMTP autenticato. Se la sottoscrizione viene esentata, potranno comunicare in uscita sulla porta 25 solo le macchine virtuali create dopo la data di esenzione.
     - **MSDN, Azure Pass, Azure in Open, Education, BizSpark e versione di prova gratuita**: la comunicazione in uscita sulla porta 25 è bloccata per tutte le risorse. Non è possibile richiedere la rimozione della restrizione, perché le richieste non verranno soddisfatte. Se è necessario inviare e-mail dalla macchina virtuale, è necessario usare un servizio di inoltro SMTP.
     - **Provider di servizi cloud**: i clienti che utilizzano le risorse di Azure tramite un provider di servizi cloud possono creare un caso di supporto presso tale provider e richiedergli di creare un caso di sblocco per loro conto, se non può essere usato un inoltro SMTP sicuro.

  Se Azure consente di inviare posta elettronica sulla porta 25, Microsoft non può garantire che i provider di posta elettronica accetteranno i messaggi in ingresso provenienti dalla macchina virtuale. Se un provider specifico rifiuta la posta dalla macchina virtuale, collaborare direttamente con il provider per risolvere eventuali problemi di invio dei messaggi o di filtro antispam oppure usare un servizio di inoltro SMTP autenticato.

## <a name="next-steps"></a>Passaggi successivi

* Informazioni su come [creare un gruppo di sicurezza di rete](tutorial-filter-network-traffic.md).
