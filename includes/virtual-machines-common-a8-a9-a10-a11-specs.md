---
title: File di inclusione
description: File di inclusione
services: virtual-machines
author: vermagit
ms.service: virtual-machines
ms.topic: include
ms.date: 05/29/2018
ms.author: azcspmt;jonbeck;cynthn;amverma
ms.custom: include file
ms.openlocfilehash: 8de1163ab68b394b6eeaaad6412995156dbe7212
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2020
ms.locfileid: "75901596"
---
## <a name="deployment-considerations"></a>Considerazioni sulla distribuzione
* **Sottoscrizione di Azure**: per distribuire numerose istanze a elevato utilizzo di calcolo, prendere in considerazione una sottoscrizione con pagamento in base al consumo o altre opzioni di acquisto. Con un [account gratuito di Azure](https://azure.microsoft.com/free/)è possibile usare solo un numero limitato di core di calcolo di Azure.

* **Prezzi e disponibilità**: queste dimensioni di VM sono offerte solo con il piano tariffario Standard. Per informazioni sulla disponibilità nelle aree di Azure, vedere [Prodotti disponibili in base all'area](https://azure.microsoft.com/global-infrastructure/services/) . 
* **Quota di core**: potrebbe essere necessario aumentare la quota di core nella sottoscrizione di Azure rispetto al valore predefinito. La sottoscrizione può anche limitare il numero di core che è possibile distribuire in alcune famiglie di dimensioni di macchina virtuale, inclusa la serie H. Per richiedere un aumento della quota, è possibile [aprire una richiesta di assistenza clienti online](../articles/azure-portal/supportability/how-to-create-azure-support-request.md) senza alcun addebito. I limiti predefiniti possono variare in base alla categoria della sottoscrizione.
  
  > [!NOTE]
  > Se si hanno esigenze di capacità su larga scala, contattare il supporto di Azure. Le quote di Azure sono limiti di credito e non garanzie di capacità. A prescindere dalla quota, viene addebitato solo l'uso dei core effettivamente impiegati.
  > 
  > 
* **Rete virtuale**: non è necessaria una [rete virtuale](https://azure.microsoft.com/documentation/services/virtual-network/) di Azure per usare le istanze a elevato utilizzo di calcolo. Per molte distribuzioni è tuttavia necessaria almeno una rete virtuale di Azure basata sul cloud. Per l'accesso alle risorse locali, è necessaria anche una connessione da sito a sito. Quando è necessaria, creare una nuova rete virtuale per distribuire le istanze. L'aggiunta di una VM a elevato uso di calcolo a una rete virtuale in un gruppo di affinità non è supportata.
* **Ridimensionamento**: a causa dell'hardware specializzato, è possibile ridimensionare solo le istanze a elevato uso di calcolo nella stessa famiglia di dimensioni (serie H o serie A a elevato uso di calcolo). Ad esempio, è possibile ridimensionare una VM della serie H solo da una dimensione della serie H a un'altra. Inoltre, non è supportato il ridimensionamento da una dimensione non a elevato uso di calcolo.  

## <a name="rdma-capable-instances"></a>Istanze con supporto per RDMA
Un subset delle istanze a elevato utilizzo di calcolo (A8, A9, H16r, H16mr, HB e HC) è caratterizzato da un'interfaccia di rete per la connettività di accesso diretto a memoria remota (RDMA). Anche le dimensioni selezionate della serie N designate con ' r ', ad esempio le configurazioni NC24rs (NC24rs_v2 e NC24rs_v3), sono compatibili con RDMA. Questa interfaccia è un'aggiunta all'interfaccia di rete di Azure standard disponibile per gli altri formati di VM. 
  
Questa interfaccia consente alle istanze con supporto per RDMA di comunicare attraverso una rete InfiniBand (IB), che opera a tariffe EDR per HB, HC, le tariffe FDR per H16r, H16mr e le macchine virtuali serie N con supporto per RDMA e le tariffe QDR per le macchine virtuali a8 e A9. Queste funzionalità RDMA possono migliorare la scalabilità e le prestazioni di determinate applicazioni di interfaccia MPI (Message Passing Interface). Per ulteriori informazioni sulla velocità, vedere i dettagli nelle tabelle in questa pagina.

> [!NOTE]
> In Azure, IP over IB è supportato solo nelle VM abilitate per SR-IOV (SR-IOV per InfiniBand, attualmente HB e HC). RDMA su IB è supportato per tutte le istanze con supporto per RDMA.
>

