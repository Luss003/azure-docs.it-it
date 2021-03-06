---
title: Che cos'è Collegamento privato di Azure?
description: Panoramica su funzionalità, architettura e implementazione di Collegamento privato di Azure. Informazioni sul funzionamento di Endpoint privati di Azure e del servizio Collegamento privato di Azure e su come usarli.
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: overview
ms.date: 02/27/2020
ms.author: allensu
ms.custom: fasttrack-edit
ms.openlocfilehash: 710c5a780841135344d92e93a02f97963b36b09e
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/28/2020
ms.locfileid: "77921391"
---
# <a name="what-is-azure-private-link"></a>Che cos'è Collegamento privato di Azure? 
Collegamento privato di Azure consente di accedere ai servizi PaaS di Azure, ad esempio:
 
 - **Archiviazione di Azure**
 - **Azure Cosmos DB**
 - **Database SQL di Azure**

Collegamento privato consente di accedere a servizi di clienti e partner ospitati tramite un [endpoint privato](private-endpoint-overview.md) nella rete virtuale.

Il traffico tra la rete virtuale e il servizio attraversa la rete del backbone Microsoft. L'esposizione del servizio sulla rete Internet pubblica non è più necessaria. È possibile creare un [servizio Collegamento privato](private-link-service-overview.md) personale nella rete virtuale e distribuirlo ai clienti. Collegamento privato di Azure offre un'esperienza coerente di configurazione e utilizzo per i servizi PaaS di Azure, i servizi di proprietà dei clienti e quelli condivisi dei partner.

> [!IMPORTANT]
> Collegamento privato di Azure è ora disponibile a livello generale. L'endpoint privato e Collegamento privato (il servizio alla base di Load Balancer Standard) sono disponibili a livello generale. L'onboarding di soluzioni Azure PaaS diverse in Collegamento privato di Azure verrà eseguito in base a pianificazioni diverse. Per informazioni accurate sullo stato di Azure PaaS in Collegamento privato, vedere la sezione sulla [disponibilità](https://docs.microsoft.com/azure/private-link/private-link-overview#availability). Per informazioni sulle limitazioni note, vedere [Endpoint privato](private-endpoint-overview.md#limitations) e [Servizio Collegamento privato](private-link-service-overview.md#limitations). 

![Panoramica dell'endpoint privato](media/private-link-overview/private-endpoint.png)

## <a name="key-benefits"></a>Vantaggi principali
Collegamento privato di Azure offre i vantaggi descritti di seguito.  
- **Accesso privato ai servizi nella piattaforma Azure**: connettere la rete virtuale ai servizi in Azure senza un indirizzo IP pubblico all'origine o alla destinazione. I provider possono offrire i servizi nella loro rete virtuale e i clienti possono accedervi nella loro rete virtuale locale. La piattaforma di Collegamento privato gestirà la connettività tra l'utente e i servizi tramite la rete backbone di Azure. 
 
- **Reti locali e con peering**: accedere ai servizi in esecuzione in Azure dall'ambiente locale tramite peering privato ExpressRoute, tunnel VPN e reti virtuali con peering usando endpoint privati. Non è necessario configurare il peering pubblico o attraversare Internet per raggiungere il servizio. Collegamento privato offre un modo sicuro per eseguire la migrazione dei carichi di lavoro ad Azure.
 
- **Protezione dall'esfiltrazione di dati**: un endpoint privato viene mappato a un'istanza di una risorsa PaaS invece che all'intero servizio. Gli utenti possono connettersi solo alla risorsa specifica. L'accesso a qualsiasi altra risorsa del servizio è bloccato. Questo meccanismo offre protezione dai rischi di perdita dei dati. 
 
- **Copertura globale**: è possibile connettersi privatamente a servizi in esecuzione in altre aree. La rete virtuale dell'utente può pertanto trovarsi nell'area A e connettersi ai servizi con Collegamento privato nell'area B.  
 
- **Estensione a servizi personalizzati**: la stessa esperienza e le stesse funzionalità possono essere sfruttate per offrire privatamente un servizio personalizzato agli utenti in Azure. Posizionando un servizio dietro Azure Load Balancer, è possibile abilitarlo per Collegamento privato. L'utente potrà quindi connettersi direttamente al servizio usando un endpoint privato nella propria rete virtuale. È possibile gestire queste richieste di connessione con un semplice flusso di chiamate di approvazione. Collegamento privato di Azure funziona anche per utenti e servizi appartenenti a tenant di Azure Active Directory diversi. 

## <a name="availability"></a>Disponibilità 
 La tabella seguente elenca i servizi di Collegamento privato e le aree in cui sono disponibili. 

|Scenario  |Servizi supportati  |Aree disponibili | Stato  |
|:---------|:-------------------|:-----------------|:--------|
|Collegamento privato per servizi di proprietà dei clienti|Servizi di Collegamento privato dietro Azure Load Balancer | Tutte le aree pubbliche  | GA <br/> [Altre informazioni](https://docs.microsoft.com/azure/private-link/private-link-service-overview) |
|Collegamento privato per servizi PaaS di Azure   | Archiviazione di Azure        |  Tutte le aree pubbliche      | Anteprima <br/> [Altre informazioni](/azure/storage/common/storage-private-endpoints)  |
|  | Azure Data Lake Storage Gen2        |  Tutte le aree pubbliche      | Anteprima <br/> [Altre informazioni](/azure/storage/common/storage-private-endpoints)  |
|  |  database SQL di Azure         | Tutte le aree pubbliche      |   Anteprima <br/> [Altre informazioni](https://docs.microsoft.com/azure/sql-database/sql-database-private-endpoint-overview)      |
||Azure SQL Data Warehouse| Tutte le aree pubbliche |Anteprima <br/> [Altre informazioni](https://docs.microsoft.com/azure/sql-database/sql-database-private-endpoint-overview)|
||Azure Cosmos DB| Stati Uniti centro-occidentali, Stati Uniti occidentali, Stati Uniti centro-settentrionali |Anteprima <br/> [Altre informazioni](https://docs.microsoft.com/azure/cosmos-db/how-to-configure-private-endpoints)|
|  |  Database di Azure per PostgreSQL - Server singolo         | Tutte le aree pubbliche      |   Anteprima <br/> [Altre informazioni](https://docs.microsoft.com/azure/postgresql/concepts-data-access-and-security-private-link)      |
|  |  Database di Azure per MySQL         | Tutte le aree pubbliche      |   Anteprima <br/> [Altre informazioni](https://docs.microsoft.com/azure/mysql/concepts-data-access-security-private-link)     |
|  |  Database di Azure per MariaDB         | Tutte le aree pubbliche      |   Anteprima <br/> [Altre informazioni](https://docs.microsoft.com/azure/mariadb/concepts-data-access-security-private-link)      |
|  |  Insieme di credenziali chiave di Azure         | Tutte le aree pubbliche      |   Anteprima   <br/> [Altre informazioni](https://docs.microsoft.com/azure/key-vault/private-link-service)   |

Per le notifiche più aggiornate, vedere la [pagina degli aggiornamenti relativi a Rete virtuale di Azure](https://azure.microsoft.com/updates/?product=virtual-network).

## <a name="logging-and-monitoring"></a>Registrazione e monitoraggio

Collegamento privato di Azure prevede l'integrazione con Monitoraggio di Azure. Questa combinazione consente:

 - Archiviazione di log in un account di archiviazione.
 - Streaming di eventi nell'hub eventi.
 - Registrazione di Monitoraggio di Azure.

In Monitoraggio di Azure è possibile accedere alle informazioni seguenti. 
- **Endpoint privato**: 
    - dati elaborati dall'endpoint privato (IN/OUT)
 
- **Servizio Collegamento privato**:
    - Dati elaborati dal servizio Collegamento privato (IN/OUT)
    - Disponibilità di porte NAT  
 
## <a name="pricing"></a>Prezzi   
Per informazioni dettagliate sui prezzi, vedere [Prezzi di Collegamento privato di Azure](https://azure.microsoft.com/pricing/details/private-link/).
 
## <a name="faqs"></a>Domande frequenti  
Per domande frequenti, vedere [Domande frequenti su Collegamento privato di Azure](private-link-faq.md).
 
## <a name="limits"></a>Limiti  
Per informazioni sui limiti, vedere [Limiti di Collegamento privato di Azure](../azure-resource-manager/management/azure-subscription-service-limits.md#private-link-limits).

## <a name="service-level-agreement"></a>Contratto di servizio
Per informazioni, vedere [Contratto di servizio per Collegamento privato di Azure](https://azure.microsoft.com/support/legal/sla/private-link/v1_0/).

## <a name="next-steps"></a>Passaggi successivi

- [Avvio rapido: Creare un endpoint privato con il portale di Azure](create-private-endpoint-portal.md)
- [Avvio rapido: Creare un servizio Collegamento privato usando il portale di Azure](create-private-link-service-portal.md)


 
