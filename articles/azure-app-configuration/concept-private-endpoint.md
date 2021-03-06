---
title: Uso di endpoint privati per la configurazione di app Azure
description: Proteggere l'archivio di configurazione dell'app usando endpoint privati
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 3/12/2020
ms.author: lcozzens
ms.openlocfilehash: f18672b9e3a368a833fc8cba279d748dfe3c2a9e
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/14/2020
ms.locfileid: "79366769"
---
# <a name="using-private-endpoints-for-azure-app-configuration"></a>Uso di endpoint privati per la configurazione di app Azure

È possibile usare [endpoint privati](../private-link/private-endpoint-overview.md) per la configurazione di app Azure per consentire ai client in una rete virtuale (VNet) di accedere in modo sicuro ai dati tramite un [collegamento privato](../private-link/private-link-overview.md). L'endpoint privato usa un indirizzo IP dallo spazio di indirizzi della VNet per l'archivio di configurazione dell'app. Il traffico di rete tra i client nel VNet e l'archivio di configurazione dell'app attraversa la VNet usando un collegamento privato nella rete dorsale Microsoft, eliminando l'esposizione alla rete Internet pubblica.

L'uso di endpoint privati per l'archivio di configurazione delle app consente di:
- Proteggere i dettagli di configurazione dell'applicazione configurando il firewall in modo da bloccare tutte le connessioni alla configurazione dell'app sull'endpoint pubblico.
- Aumentare la sicurezza per la rete virtuale (VNet) assicurando che i dati non vengano ignorati dal VNet.
- Connettersi in modo sicuro all'archivio di configurazione delle app dalle reti locali che si connettono al VNet tramite [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) o [delle expressroute](../expressroute/expressroute-locations.md) con peering privato.

> [!NOTE]
> App Azure configurazione offre l'uso di endpoint privati come anteprima pubblica. Le offerte di anteprima pubblica consentono ai clienti di sperimentare le nuove funzionalità prima del rilascio della versione ufficiale.  I servizi e le funzionalità di anteprima pubblica non sono destinati all'uso in produzione.

## <a name="conceptual-overview"></a>Informazioni generali

Un endpoint privato è un'interfaccia di rete speciale per un servizio di Azure nella [rete virtuale](../virtual-network/virtual-networks-overview.md) (VNet). Quando si crea un endpoint privato per l'archivio di configurazione dell'app, fornisce connettività sicura tra i client nella VNet e nell'archivio di configurazione. All'endpoint privato viene assegnato un indirizzo IP dall'intervallo di indirizzi IP della VNet. La connessione tra l'endpoint privato e l'archivio di configurazione utilizza un collegamento privato protetto.

Le applicazioni in VNet possono connettersi all'archivio di configurazione tramite l'endpoint privato **utilizzando le stesse stringhe di connessione e i meccanismi di autorizzazione che utilizzerebbe in caso contrario**. Gli endpoint privati possono essere usati con tutti i protocolli supportati dall'archivio di configurazione dell'app.

Sebbene la configurazione dell'app non supporti gli endpoint di servizio, è possibile creare endpoint privati in subnet che usano gli [endpoint del servizio](../virtual-network/virtual-network-service-endpoints-overview.md). I client in una subnet possono connettersi in modo sicuro a un archivio di configurazione dell'app usando l'endpoint privato mentre usano gli endpoint di servizio per accedere ad altri utenti.  

Quando si crea un endpoint privato per un servizio nella VNet, viene inviata una richiesta di consenso per l'approvazione al proprietario dell'account del servizio. Se l'utente che richiede la creazione dell'endpoint privato è anche un proprietario dell'account, questa richiesta di consenso viene approvata automaticamente.

I proprietari degli account del servizio possono gestire le richieste di consenso e gli endpoint privati tramite la scheda `Private Endpoints` dell'archivio di configurazione nel [portale di Azure](https://portal.azure.com).

### <a name="private-endpoints-for-app-configuration"></a>Endpoint privati per la configurazione dell'app 

Quando si crea un endpoint privato, è necessario specificare l'archivio di configurazione dell'app a cui si connette. Se sono presenti più istanze di configurazione dell'app all'interno di un account, è necessario un endpoint privato separato per ogni punto vendita.

### <a name="connecting-to-private-endpoints"></a>Connessione agli endpoint privati

Azure si basa sulla risoluzione DNS per instradare le connessioni da VNet all'archivio di configurazione tramite un collegamento privato. È possibile trovare rapidamente le stringhe di connessione nel portale di Azure selezionando l'archivio di configurazione dell'app, quindi selezionando **impostazioni** > **chiavi di accesso**.  

> [!IMPORTANT]
> Usare la stessa stringa di connessione per connettersi all'archivio di configurazione dell'app usando endpoint privati come si farebbe per un endpoint pubblico. Non connettersi all'account di archiviazione usando l'URL del sottodominio `privatelink`.

## <a name="dns-changes-for-private-endpoints"></a>Modifiche DNS per gli endpoint privati

Quando si crea un endpoint privato, il record di risorse DNS CNAME per l'archivio di configurazione viene aggiornato a un alias in un sottodominio con il prefisso `privatelink`. Azure crea anche una [zona DNS privata](../dns/private-dns-overview.md) corrispondente al sottodominio `privatelink`, con i record di risorse DNS a per gli endpoint privati.

Quando si risolve l'URL dell'endpoint dall'esterno del VNet, questo viene risolto nell'endpoint pubblico dell'archivio. Quando viene risolto dall'interno di VNet che ospita l'endpoint privato, l'URL dell'endpoint viene risolto nell'endpoint privato.

È possibile controllare l'accesso per i client esterni a VNet tramite l'endpoint pubblico usando il servizio firewall di Azure.

Questo approccio consente di accedere all'archivio **usando la stessa stringa di connessione** per i client in VNet che ospitano gli endpoint privati e i client esterni al VNet.

Se si usa un server DNS personalizzato nella rete, i client devono essere in grado di risolvere il nome di dominio completo (FQDN) per l'endpoint del servizio nell'indirizzo IP dell'endpoint privato. Configurare il server DNS per delegare il sottodominio di collegamento privato alla zona DNS privata per la VNet o configurare i record A per `AppConfigInstanceA.privatelink.azconfig.io` con l'indirizzo IP dell'endpoint privato.

> [!TIP]
> Quando si usa un server DNS personalizzato o locale, è necessario configurare il server DNS per risolvere il nome dell'archivio nel sottodominio `privatelink` per l'indirizzo IP dell'endpoint privato. A tale scopo, è possibile delegare il `privatelink` sottodominio alla zona DNS privata del VNet o configurare la zona DNS nel server DNS e aggiungere i record A DNS.

## <a name="pricing"></a>Pricing

Per abilitare endpoint privati è necessario un archivio di configurazione di app di [livello standard](https://azure.microsoft.com/pricing/details/app-configuration/) .  Per informazioni sui prezzi dei collegamenti privati, vedere [prezzi di collegamento privato di Azure](https://azure.microsoft.com/pricing/details/private-link).

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulla creazione di un endpoint privato per l'archivio di configurazione dell'app, vedere gli articoli seguenti:

- [Creare un endpoint privato usando il centro collegamenti privati nel portale di Azure](../private-link/create-private-endpoint-portal.md)
- [Creare un endpoint privato usando l'interfaccia della riga di comando](../private-link/create-private-endpoint-cli.md)
- [Creare un endpoint privato usando Azure PowerShell](../private-link/create-private-endpoint-powershell.md)

Informazioni su come configurare il server DNS con endpoint privati:

- [Risoluzione dei nomi per le risorse in reti virtuali di Azure](/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#name-resolution-that-uses-your-own-dns-server)
- [Configurazione DNS per endpoint privati](/azure/private-link/private-endpoint-overview#dns-configuration)
