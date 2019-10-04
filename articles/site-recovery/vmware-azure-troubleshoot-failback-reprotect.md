---
title: Risoluzione dei problemi relativi al failback in un'istanza locale durante il ripristino di emergenza di macchine virtuali VMware in Azure con Azure Site Recovery | Microsoft Docs
description: Questo articolo descrive le soluzioni agli errori di failback e riprotezione che possono verificarsi durante il ripristino di emergenza di macchine virtuali VMware in Azure con Azure Site Recovery.
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: 20cb7a446befb1d31f0e069d91d0230fc4a2a901
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "60565600"
---
# <a name="troubleshoot-failback-to-on-premises-from-azure"></a>Risolvere i problemi di failback da Azure all'ambiente locale

In questo articolo viene illustrato come risolvere i problemi che possono verificarsi durante il failback di macchine virtuali di Azure all'infrastruttura VMware locale, dopo il failover ad Azure mediante [Azure Site Recovery](site-recovery-overview.md).

Il failback è composto essenzialmente da due passaggi. Innanzitutto, dopo il failover, è necessario riproteggere le macchine virtuali di Azure in locale, in modo da avviarne la replica. Il secondo passaggio consiste nell'eseguire un failover da Azure, in modo da eseguire il failback al sito locale.

## <a name="troubleshoot-reprotection-errors"></a>Risolvere gli errori di riprotezione

Questa sezione illustra nel dettaglio i più comuni errori di riprotezione, con la relativa risoluzione.

### <a name="error-code-95226"></a>Codice di errore 95226

**La riprotezione non è riuscita perché la macchina virtuale di Azure non è in grado di raggiungere il server di configurazione locale.**

Questo errore si verifica quando:

* La macchina virtuale di Azure non riesce a connettersi al server di configurazione locale. Non è possibile individuare la macchina virtuale e registrarla nel server di configurazione.
* Il servizio dell'applicazione InMage Scout non è in esecuzione nella macchina virtuale di Azure dopo il failover. Il servizio è necessario per le comunicazioni con il server di configurazione locale.

Per risolvere il problema:

* Verificare che la rete delle macchine virtuali di Azure consenta alla macchina virtuale di Azure di comunicare con il server di configurazione locale. È possibile configurare una VPN da sito a sito nel data center locale o configurare una connessione Azure ExpressRoute con peering privato sulla rete virtuale della macchina virtuale di Azure.
* Se la macchina virtuale è in grado di comunicare con il server di configurazione locale, accedere alla macchina virtuale. Controllare quindi il servizio dell'applicazione InMage Scout. Se non risulta in esecuzione, avviare il servizio manualmente. Verificare che il tipo di avvio del servizio sia impostato su **Automatico**.

### <a name="error-code-78052"></a>Codice di errore 78052

**Non è stato possibile completare l'abilitazione della protezione per la macchina virtuale.**

Questo problema può verificarsi se esiste già una macchina virtuale con lo stesso nome nel server di destinazione master a cui si sta eseguendo il failback.

Per risolvere il problema:

* Selezionare un server di destinazione master diverso in un host diverso, in modo che la riprotezione crei la macchina virtuale in un host diverso in cui i nomi non siano in conflitto.
* È anche possibile usare vMotion per spostare il server di destinazione master in un host diverso, in cui non si verifica il conflitto di nomi. Se la macchina virtuale esistente è una macchina rimasta inutilizzata, è possibile rinominarla in modo che la nuova macchina virtuale possa essere creata nello stesso host ESXi.


### <a name="error-code-78093"></a>Codice di errore 78093

**La macchina virtuale non è in esecuzione, è bloccata o non è accessibile.**

Per risolvere il problema:

Per riproteggere la macchina virtuale sottoposta a failover, la machina virtuale di Azure deve essere eseguita in modo che il servizio Mobility venga registrato con il server di configurazione locale e possa avviare la replica mediante la comunicazione con il server di elaborazione. Se la macchina si trova in una rete non corretta o non è in esecuzione (non risponde o arrestato), il server di configurazione non riesce a raggiungere il servizio Mobility nella macchina virtuale per avviare la riprotezione.

* Riavviare la macchina virtuale in modo che possa a ristabilire la comunicazione in locale.
* Riavviare il processo di riprotezione dopo l'avvio della macchina virtuale di Azure.

### <a name="error-code-8061"></a>Codice di errore 8061

**L'archivio dati non è accessibile dall'host ESXi.**

Verificare i [prerequisiti del server di destinazione master](vmware-azure-reprotect.md#deploy-a-separate-master-target-server) e gli archivi dati supportati per il failback.


## <a name="troubleshoot-failback-errors"></a>Risolvere gli errori di failback

Questa sezione descrive i più comuni errori che possono verificarsi durante il failback.

### <a name="error-code-8038"></a>Codice di errore 8038

**Non è stato possibile accedere alla macchina virtuale locale a causa dell'errore.**

Questo problema si verifica quando si accede alla macchina virtuale locale su un host in cui non è stato effettuato il provisioning di una quantità di memoria sufficiente. 

Per risolvere il problema:

* Effettuare il provisioning di una quantità maggiore di memoria nell'host ESXi.
* Inoltre, è possibile usare vMotion per spostare la macchina virtuale su un altro host ESXi in cui sia disponibile memoria sufficiente per avviare la macchina virtuale.
