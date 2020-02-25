---
title: Selezionare lo SKU di VM corretto per il cluster di Azure Esplora dati
description: Questo articolo descrive come selezionare le dimensioni ottimali dello SKU per il cluster Azure Esplora dati.
author: avneraa
ms.author: avnera
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/14/2019
ms.openlocfilehash: 2d078f9715a0cfa171f0c88776a4ab78c15215a8
ms.sourcegitcommit: dd3db8d8d31d0ebd3e34c34b4636af2e7540bd20
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/22/2020
ms.locfileid: "77561851"
---
# <a name="select-the-correct-vm-sku-for-your-azure-data-explorer-cluster"></a>Selezionare lo SKU di VM corretto per il cluster di Azure Esplora dati 

Quando si crea un nuovo cluster o si ottimizza un cluster per un carico di lavoro modificabile, Azure Esplora dati offre più SKU di macchine virtuali (VM) tra cui scegliere. Le macchine virtuali sono state scelte con attenzione per offrire il costo ottimale per qualsiasi carico di lavoro. 

Le dimensioni e lo SKU di VM del cluster di gestione dati sono completamente gestiti dal servizio Esplora dati di Azure. Sono determinati da fattori quali le dimensioni della macchina virtuale del motore e il carico di lavoro di inserimento. 

È possibile modificare lo SKU di VM per il cluster del motore in qualsiasi momento [scalando il cluster](manage-cluster-vertical-scaling.md). È preferibile iniziare con le dimensioni dello SKU più piccole che si adattano allo scenario iniziale. Tenere presente che la scalabilità verticale del cluster comporta un tempo di inattività massimo di 30 minuti mentre il cluster viene ricreato con il nuovo SKU della macchina virtuale.

> [!TIP]
> Le [istanze riservate](https://docs.microsoft.com/azure/virtual-machines/windows/prepay-reserved-vm-instances) di calcolo sono applicabili al cluster di Azure Esplora dati.  

Questo articolo descrive le varie opzioni dello SKU di VM e fornisce i dettagli tecnici che consentono di effettuare la scelta migliore.

## <a name="select-a-cluster-type"></a>Selezionare un tipo di cluster

Azure Esplora dati offre due tipi di cluster:

* **Produzione**: i cluster di produzione contengono due nodi per i cluster di gestione dei dati e del motore e vengono gestiti con il [contratto](https://azure.microsoft.com/support/legal/sla/data-explorer/v1_0/)di servizio di Azure Esplora dati.

* **Sviluppo/test (nessun contratto**di servizio): i cluster di sviluppo/test includono un singolo nodo D11 V2 per il cluster di motore e un singolo nodo D1 per il cluster di gestione dati. Questo tipo di cluster è la configurazione di costo più basso a causa del numero di istanze basso e nessun addebito per il markup del motore. Non esiste alcun contratto di servizio per questa configurazione cluster, perché manca la ridondanza.

## <a name="sku-types"></a>Tipi di SKU

Quando si crea un cluster di Esplora dati di Azure, selezionare lo SKU di VM *ottimale* per il carico di lavoro pianificato. È possibile scegliere tra le due famiglie di SKU Esplora dati di Azure seguenti:

* **D V2**: lo SKU d è ottimizzato per il calcolo e presenta due varianti:
    * Macchina virtuale stessa
    * VM in bundle con dischi di archiviazione Premium

* **Ls**: lo SKU L è ottimizzato per l'archiviazione. Ha una dimensione SSD molto maggiore rispetto allo SKU D con prezzo analogo.

Nella tabella seguente sono descritte le differenze principali tra i tipi di SKU disponibili:
 
| Attributo | SKU D | SKU L |
|---|---|---
|**SKU di piccole dimensioni**|La dimensione minima è D11 con due core|La dimensione minima è L4 con quattro core |
|**Disponibilità**|Disponibile in tutte le aree (la versione di DS + PS ha una disponibilità più limitata)|Disponibile in alcune aree geografiche |
|**Costo per cache&nbsp;GB per core**|Alta con lo SKU D, bassa con la versione DS + PS|Più basso con l'opzione con pagamento in base al consumo |
|**Prezzi delle istanze riservate**|Sconto elevato (oltre 55&nbsp;percentuale per un impegno di tre anni)|Sconto inferiore (20&nbsp;% per un impegno di tre anni) |  

## <a name="select-your-cluster-vm"></a>Selezionare la macchina virtuale del cluster 

Per selezionare la macchina virtuale del cluster, [configurare la scalabilità verticale](manage-cluster-vertical-scaling.md#configure-vertical-scaling). 

Con varie opzioni di SKU di VM tra cui scegliere, è possibile ottimizzare i costi per i requisiti di prestazioni e della cache a caldo per lo scenario. 
* Se è necessario ottenere prestazioni ottimali per un volume di query elevato, lo SKU ideale dovrebbe essere ottimizzato per il calcolo. 
* Se è necessario eseguire query su grandi volumi di dati con un carico di query relativamente inferiore, lo SKU ottimizzato per l'archiviazione può aiutare a ridurre i costi e a garantire prestazioni ottimali.

Poiché il numero di istanze per cluster per gli SKU di piccole dimensioni è limitato, è preferibile usare macchine virtuali di dimensioni maggiori con RAM maggiore. È necessaria una maggiore quantità di RAM per alcuni tipi di query che inseriscono più richieste sulla risorsa RAM, ad esempio query che usano `joins`. Pertanto, quando si considerano le opzioni di scalabilità, è consigliabile aumentare il numero di istanze, anziché aumentare il numero di istanze.

## <a name="vm-options"></a>Opzioni VM

Nella tabella seguente sono descritte le specifiche tecniche per le macchine virtuali del cluster Esplora dati di Azure:

|**Nome**| **Categoria** | **Dimensioni unità SSD** | **Core** | **RAM** | **Dischi di archiviazione Premium (1&nbsp;TB)**| **Numero minimo di istanze per cluster** | **Numero massimo di istanze per cluster**
|---|---|---|---|---|---|---|---
|D11 V2| ottimizzato per il calcolo | 75&nbsp;GB    | 2 | 14&nbsp;GB | 0 | 1 | 8 (ad eccezione dello SKU sviluppo/test, che è 1)
|D12 v2| ottimizzato per il calcolo | 150&nbsp;GB   | 4 | 28&nbsp;GB | 0 | 2 | 16
|D13 v2| ottimizzato per il calcolo | 307&nbsp;GB   | 8 | 56&nbsp;GB | 0 | 2 | 1\.000
|D14 v2| ottimizzato per il calcolo | 614&nbsp;GB   | 16| 112&nbsp;GB | 0 | 2 | 1\.000
|DS13 v2 + 1&nbsp;TB&nbsp;PS| ottimizzato per l'archiviazione | 1&nbsp;TB | 8 | 56&nbsp;GB | 1 | 2 | 1\.000
|DS13 v2 + 2&nbsp;TB&nbsp;PS| ottimizzato per l'archiviazione | 2&nbsp;TB | 8 | 56&nbsp;GB | 2 | 2 | 1\.000
|DS14 V2 + 3&nbsp;TB&nbsp;PS| ottimizzato per l'archiviazione | 3&nbsp;TB | 16 | 112&nbsp;GB | 2 | 2 | 1\.000
|DS14 V2 + 4&nbsp;TB&nbsp;PS| ottimizzato per l'archiviazione | 4&nbsp;TB | 16 | 112&nbsp;GB | 4 | 2 | 1\.000
|L4S V1| ottimizzato per l'archiviazione | 650&nbsp;GB | 4 | 32&nbsp;GB | 0 | 2 | 16
|L8s V1| ottimizzato per l'archiviazione | 1,3&nbsp;TB | 8 | 64&nbsp;GB | 0 | 2 | 1\.000
|L16s_1| ottimizzato per l'archiviazione | 2,6&nbsp;TB | 16| 128&nbsp;GB | 0 | 2 | 1\.000

* È possibile visualizzare l'elenco di SKU di VM aggiornato per area usando l' [API ListSkus](/dotnet/api/microsoft.azure.management.kusto.clustersoperationsextensions.listskus?view=azure-dotnet)di Azure Esplora dati. 
* Altre informazioni sui [vari SKU](/azure/virtual-machines/windows/sizes). 

## <a name="next-steps"></a>Passaggi successivi

* È possibile [aumentare o ridurre](manage-cluster-vertical-scaling.md) il cluster del motore in qualsiasi momento modificando lo SKU della macchina virtuale, in base alle diverse esigenze. 

* È possibile aumentare [o](manage-cluster-horizontal-scaling.md) ridurre le dimensioni del cluster del motore per modificare la capacità, a seconda delle esigenze di modifica.

