---
title: Quali sono le opzioni di architettura di Azure Firewall Manager?
description: Confrontare e contrapporre usando la rete virtuale Hub o le architetture di hub virtuale protette con gestione firewall di Azure.
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: article
ms.date: 02/18/2020
ms.author: victorh
ms.openlocfilehash: b946a360ced05500a4ef89cda7c623d8ae16658e
ms.sourcegitcommit: 6e87ddc3cc961945c2269b4c0c6edd39ea6a5414
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/18/2020
ms.locfileid: "77444576"
---
# <a name="what-are-the-azure-firewall-manager-architecture-options"></a>Quali sono le opzioni di architettura di Azure Firewall Manager?

Azure Firewall Manager può fornire la gestione della sicurezza per due tipi di architettura di rete:

- **hub virtuale protetto**

   Un [Hub WAN virtuale di Azure](../virtual-wan/virtual-wan-about.md#resources) è una risorsa gestita da Microsoft che consente di creare facilmente architetture Hub e spoke. Quando i criteri di sicurezza e routing sono associati a un hub di questo tipo, viene definito *[hub virtuale protetto](secured-virtual-hub.md)* . 
- **rete virtuale Hub**

   Si tratta di una rete virtuale di Azure standard che è possibile creare e gestire autonomamente. Quando i criteri di sicurezza sono associati a un hub di questo tipo, viene definito *rete virtuale dell'hub*. A questo punto, sono supportati solo i criteri del firewall di Azure. È possibile parlare di reti virtuali che contengono i servizi e i server del carico di lavoro. È anche possibile gestire i firewall in reti virtuali autonome senza peering a qualsiasi spoke.

## <a name="comparison"></a>Confronto

Nella tabella seguente vengono confrontate queste due opzioni di architettura che consentono di decidere quale sia la scelta più adatta ai requisiti di sicurezza dell'organizzazione:


|  |**Rete virtuale Hub**|**Hub virtuale protetto**  |
|---------|---------|---------|
|**Risorsa sottostante**     |Rete virtuale|Hub WAN virtuale|
|**Hub & spoke**     |Usa il peering di rete virtuale|Connessione automatizzata tramite la rete virtuale dell'hub|
|**Connettività locale**     |Gateway VPN fino a 10 Gbps e 30 connessioni S2S; ExpressRoute|Gateway VPN più scalabile fino a 20 Gbps e 1000 connessioni S2S. Express Route|
|**Connettività del ramo automatizzata con SDWAN**      |Non supportate|Supportato|
|**Hub per area**     |Più reti virtuali per area|Hub virtuale singolo per area. Più hub possibili con più WAN virtuali|
|**Firewall di Azure: più indirizzi IP pubblici**      |Fornito dal cliente|Generato automaticamente. Per essere disponibile da GA.|
|**zone di disponibilità del firewall di Azure**     |Supportato|Non disponibile in anteprima. Per essere disponibile da GA|
|**Sicurezza Internet avanzata con partner di sicurezza di terze parti come partner di servizi**     |Connettività VPN gestita e stabilita dal cliente al servizio partner preferito|Automatizzato tramite l'esperienza di gestione dei partner e del flusso di sicurezza attendibile|
|**Gestione centralizzata delle route per instradare il traffico all'hub**     |Route definita dall'utente gestita dal cliente|Supportato tramite BGP|
|**Web Application Firewall sul gateway applicazione** |Supportato nella rete virtuale|Attualmente supportata nella rete spoke|
|**Appliance virtuale di rete**|Supportato nella rete virtuale|Attualmente supportata nella rete spoke|

## <a name="next-steps"></a>Passaggi successivi

- Esaminare [Panoramica della distribuzione di Anteprima di Gestione firewall di Azure](deployment-overview.md)
- Informazioni sugli [hub virtuali protetti](secured-virtual-hub.md).