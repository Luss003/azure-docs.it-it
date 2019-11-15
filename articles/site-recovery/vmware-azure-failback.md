---
title: Eseguire il failback di server fisici/VM VMware da Azure con Azure Site Recovery
description: Informazioni su come eseguire il failback nel sito locale dopo il failover in Azure durante il ripristino di emergenza di macchine virtuali VMware e server fisici in Azure.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.date: 01/15/2019
ms.topic: conceptual
ms.author: mayg
ms.openlocfilehash: 2ec2a4a91f4de0761f631bec393bb90c3feb82b9
ms.sourcegitcommit: a22cb7e641c6187315f0c6de9eb3734895d31b9d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/14/2019
ms.locfileid: "74084051"
---
# <a name="fail-back-vmware-vms-and-physical-servers-from-azure-to-an-on-premises-site"></a>Eseguire il failback di macchine virtuali VMware e server fisici da Azure in un sito locale

Questo articolo descrive come eseguire il failback da macchine virtuali di Azure all'ambiente VMware locale. Seguire le istruzioni riportate in questo articolo per eseguire il failback delle macchine virtuali VMware o dei server fisici Windows/Linux dopo avere effettuato il failover dal sito locale ad Azure seguendo l'esercitazione [Failover in Azure Site Recovery](site-recovery-failover.md).

## <a name="prerequisites"></a>prerequisiti
- Leggere i dettagli sui [diversi tipi di failback](concepts-types-of-failback.md) e le rispettive avvertenze.

> [!WARNING]
> Non è possibile eseguire il failback se è stata [completata la migrazione](migrate-overview.md#what-do-we-mean-by-migration), la macchina virtuale è stata spostata in un altro gruppo di risorse o la macchina virtuale di Azure è stata eliminata. Se si disabilita la protezione della macchina virtuale, non è possibile eseguire il failback.

> [!WARNING]
> Non è possibile eseguire il failback di un server fisico Windows Server 2008 R2 SP1 protetto e sottoposto a failover in Azure.

> [!NOTE]
> Se è stato eseguito il failover delle macchine virtuali VMware, non è possibile eseguire il failback a un host Hyper-V.


- Prima di continuare, completare i passaggi di riprotezione in modo che le macchine virtuali siano nello stato replicato e sia possibile avviare un failover in un sito locale. Per altre informazioni, vedere [Come eseguire la riprotezione da Azure al sito locale](vmware-azure-reprotect.md).

- Assicurarsi che vCenter sia in uno stato connesso prima di eseguire un failback. In caso contrario, la disconnessione dei dischi e il ricollegamento alla macchina virtuale non riescono.

- Durante il failover ad Azure, il sito locale potrebbe non essere accessibile e il server di configurazione può essere non disponibile o venire arrestato. Durante la riprotezione e il failback, il server di configurazione locale deve essere in esecuzione e in stato OK connesso. 

- Durante il failback, la macchina virtuale deve esistere nel database del server di configurazione; in caso contrario il failback non ha esisto negativo. Verificare di pianificare backup regolari del server. In caso di emergenza, è necessario ripristinare il server con l'indirizzo IP originale, in modo che il failback funzioni.

- Il server di destinazione master non deve avere snapshot prima di attivare la riprotezione o il failback.

## <a name="overview-of-failback"></a>Panoramica del failback
Dopo avere effettuato il failover ad Azure, è possibile eseguire il failback nel sito locale eseguendo le operazioni seguenti:

1. [Riproteggere](vmware-azure-reprotect.md) le macchine virtuali in Azure in modo che inizino a replicare le macchine virtuali VMware nel sito locale. Come parte di questo processo, è necessario:

    * Impostare una destinazione master locale. Usare la destinazione master Windows per le macchine virtuali Windows e la [destinazione master Linux](vmware-azure-install-linux-master-target.md) per le macchine virtuali Linux.
    * Configurare un [server di elaborazione](vmware-azure-set-up-process-server-azure.md).
    * Eseguire la [riprotezione](vmware-azure-reprotect.md) per disattivare la macchina virtuale locale e sincronizzare i dati della macchina virtuale di Azure con i dischi locali.

2. Dopo la replica delle macchine virtuali in Azure nel sito locale, avviare un failover da Azure al sito locale.

3. Dopo avere effettuato il failback dei dati, eseguire la riprotezione delle macchine virtuali locali, in modo che inizino la replica in Azure.

Per una rapida panoramica, guardare il video seguente che illustra come effettuare il failback a un sito locale:
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]


## <a name="steps-to-fail-back"></a>Passaggi per eseguire il failback

> [!IMPORTANT]
> Prima di iniziare il failback, assicurarsi che la riprotezione delle macchine virtuali sia stata completata. Le macchine virtuali devono essere in stato protetto e l'integrità deve essere **OK**. Per riproteggere le macchine virtuali, leggere [Come eseguire la riprotezione](vmware-azure-reprotect.md).

1. Nella pagina degli elementi replicati, selezionare la macchina virtuale. Fare clic con il pulsante destro del mouse per selezionare **Failover non pianificato**.
2. In **Conferma failover** verificare che la direzione di failover sia da Azure. Selezionare quindi il punto di recupero (più recente o quello coerente con l'app più recente) da usare per il failover. Il punto coerente con l'app deve essere antecedente all'ultimo punto di temporizzazione e causerà la perdita di alcuni dati.
3. Durante il failover, Site Recovery arresta le macchine virtuali in Azure. Assicurarsi che il failback sia stato completato come previsto e quindi verificare che le macchine virtuali di Azure siano state arrestate.
4. Il **commit** è necessario per rimuovere la macchina virtuale di cui è stato eseguito il failover da Azure. Fare clic con il pulsante destro del mouse sull'elemento protetto e quindi selezionare **Commit**. Un processo rimuove le macchine virtuali di cui è stato eseguito il failover in Azure.


## <a name="to-what-recovery-point-can-i-fail-back-the-virtual-machines"></a>Punto di recupero in cui è possibile eseguire il failback delle macchine virtuali

Durante il failback sono disponibili due opzioni per il failback di macchine virtuali/piano di ripristino.

- Se si seleziona l'ultimo periodo di tempo elaborato, viene eseguito il failover di tutte le macchine virtuali nell'ultimo periodo di tempo disponibile. Se è presente un gruppo di replica nel piano di ripristino, viene effettuato il failover di ogni macchina virtuale del gruppo di replica nell'ultimo periodo di tempo indipendente.

  Non è possibile eseguire il failback di una macchina virtuale finché non ha almeno un punto di recupero. Non è possibile eseguire il failback di un piano di ripristino finché tutte le macchine virtuali non hanno almeno un punto di recupero.

  > [!NOTE]
  > Il punto di recupero più recente è un punto di recupero coerente con l'arresto anomalo.

- Se si seleziona il punto di recupero coerente con l'applicazione, un singolo failback della macchina virtuale ripristina l'ultimo punto di recupero coerente con l'applicazione più recente disponibile. In caso di piano di ripristino con un gruppo di replica, ogni gruppo di replica ripristina il punto di recupero disponibile comune.
I punti di recupero coerenti con l'applicazione possono essere indietro nel tempo e potrebbero verificarsi perdite di dati.

## <a name="what-happens-to-vmware-tools-post-failback"></a>Che cosa succede agli strumenti VMware dopo il failback?

Nel caso di una macchina virtuale Windows, Site Recovery disabilita gli strumenti VMware durante il failover. Durante il failback della macchina virtuale Windows, gli strumenti VMware vengono riabilitati. 


## <a name="reprotect-from-on-premises-to-azure"></a>Abilitare la riprotezione dall'ambiente locale ad Azure
Al termine del failback e dopo avere iniziato il commit, le macchine virtuali ripristinate in Azure vengono eliminate. A questo punto la macchina virtuale si trova nuovamente nel sito locale, ma non è protetta. Per avviare nuovamente la replica in Azure, eseguire queste operazioni:

1. Selezionare **Insieme di credenziali** > **Impostazioni** > **Elementi replicati**, selezionare le macchine virtuali di cui è stato eseguito il failback e quindi **Riproteggi**.
2. Specificare il valore del server di elaborazione da usare per inviare i dati ad Azure.
3. Selezionare **OK** per avviare il processo di riprotezione.

> [!NOTE]
> Dopo l'avvio di una macchina virtuale in locale, registrare di nuovo l'agente nel server di configurazione può richiedere tempo, fino a 15 minuti. Durante questo intervallo di tempo la riprotezione non riuscirà e sarà visualizzato un messaggio di errore con l'avviso che l'agente non è installato. Attendere alcuni minuti e ripetere l'operazione di riprotezione.

## <a name="next-steps"></a>Passaggi successivi

Al termine del processo di riprotezione, la macchina virtuale viene replicata in Azure e sarà possibile eseguire il [failover](site-recovery-failover.md) per spostare nuovamente le macchine virtuali in Azure.


