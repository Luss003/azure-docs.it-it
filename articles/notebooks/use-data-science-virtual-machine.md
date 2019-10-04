---
title: Usare le macchine virtuali di analisi scientifica dei dati di Azure
description: Connettersi a un Azure Data Science Virtual Machine (DSVM) per estendere la potenza di calcolo disponibile per notebook di Azure.
services: app-service
documentationcenter: ''
author: getroyer
manager: andneil
ms.assetid: 0ccc2529-e17f-4221-b7c7-9496d6a731cc
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2019
ms.author: getroyer
ms.openlocfilehash: fe9886429a5e894f40c04b1f65094e412c1dc9e2
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/28/2019
ms.locfileid: "67441194"
---
# <a name="use-azure-data-science-virtual-machines"></a>Usare le macchine virtuali di analisi scientifica dei dati di Azure

Per impostazione predefinita, i progetti eseguiti **calcolo gratuito** livello, che è limitato a 4 GB di memoria e 1 GB di dati di evitare abusi. È possibile aggirare queste limitazioni usando un'altra macchina virtuale per cui è stato effettuato il provisioning in una sottoscrizione di Azure. A tale scopo, la scelta migliore è una Data Science Virtual Machine (DSVM) di Azure usando il **macchina virtuale Data Science per Linux (Ubuntu)** immagine. Questo tipo una DSVM è preconfigurata con tutto ciò che è necessario per i notebook di Azure e viene visualizzato automaticamente sul **eseguire** elenco a discesa nel notebook di Azure.

> [!Note]
> Notebook di Azure è supportata solo su create con l'immagine di Linux Ubuntu on DSVMs. I notebook non sono supportati nelle immagini di Windows 2012, Windows 2016 o CentOS di Linux.

## <a name="create-a-dsvm-instance"></a>Creare un'istanza di macchina virtuale data SCIENCE

Per creare una nuova istanza di DSVM, seguire le istruzioni in [Creare una macchina virtuale data science Ubuntu](/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro). Per altre informazioni, inclusi i dettagli sui prezzi, vedere [macchine virtuali Data Science](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a name="connect-to-the-dsvm"></a>Connettere la macchina virtuale data SCIENCE

Dopo aver creato la macchina virtuale data SCIENCE, selezionare la **eseguire** elenco a discesa nel notebook di Azure del dashboard del progetto e selezionare l'istanza di macchina virtuale data SCIENCE appropriata. L'elenco di riepilogo a discesa Mostra le istanze di macchina virtuale data SCIENCE se vengono soddisfatte le condizioni seguenti:

- Si è connessi ad Azure Notebooks con un account che usa Azure Active Directory (AAD), ad esempio un account aziendale.
- L'account è connesso a una sottoscrizione di Azure.
- Hai una o più macchine virtuali in tale sottoscrizione, con almeno accesso in lettura, che usa la macchina virtuale di Data Science per l'immagine Linux (Ubuntu).)

![Istanze di Data Science Virtual Machine nell'elenco a discesa nel dashboard del progetto](media/project-compute-tier-dsvm.png)

Quando si seleziona un'istanza di DSVM, Azure Notebooks può richiedere le credenziali specifiche del computer usate durante la creazione della macchina virtuale.

Se una delle condizioni non vengono soddisfatti, è comunque possibile connettersi a DSVM. Nell'elenco a discesa, selezionare la **calcolo diretta** opzione che richiede un nome (da visualizzare nell'elenco), indirizzo IP della macchina virtuale e porta (in genere 8000, la porta predefinita da cui è in ascolto JupyterHub) e le credenziali della macchina virtuale:

![Richiesta di raccolta informazioni sul server per l'opzione di calcolo diretto](media/project-compute-tier-direct.png)

Ottenere questi valori dalla pagina di macchina virtuale data SCIENCE nel portale di Azure.

## <a name="accessing-azure-notebooks-files-from-the-dsvm"></a>L'accesso ai file di Azure Notebooks da DSVM

Accesso al file system è supportato per le versioni di macchina virtuale data SCIENCE 19.06.15 o versione successiva. Per controllare la versione, prima di tutto connettersi tramite SSH nella dsvm, quindi eseguire il comando seguente: `curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2018-10-01"` (è necessario usare l'indirizzo IP esatto illustrato di seguito). Il numero di versione viene visualizzato nell'output per "version".

Per mantenere la parità dei percorsi di file con il **calcolo gratuito** livello, si è in grado di aprire solo un unico progetto contemporaneamente in una macchina virtuale data SCIENCE. Per aprire un nuovo progetto, è necessario chiudere il progetto aperto prima di tutto.

Quando un progetto viene eseguito in una macchina virtuale, i file vengono montati nella directory radice del server di Jupyter (la directory mostrato in JupyterHub), sostituire i file di Azure Notebooks predefinito. Quando si arresta la macchina virtuale usando il **arresto** pulsante del notebook dell'interfaccia utente, i notebook di Azure consente di ripristinare i file predefiniti.

![Pulsante di arresto nel notebook di Azure](media/shutdown.png)

## <a name="create-new-dsvm-users"></a>Creare nuovi utenti di macchina virtuale data SCIENCE

Se più utenti condividano un DSVM, è possibile evitare il blocco tra loro tramite la creazione e uso di un utente di macchina virtuale data SCIENCE per ogni utente notebook:

1. Nel [portale di Azure](https://portal.azure.com), passare alla macchina virtuale.
1. Sotto **supporto + risoluzione dei problemi** sul margine sinistro, selezionare **Reimposta password**.
1. Immettere un nuovo nome utente e una password e selezionare **Update**. (I nomi utente esistenti non sono interessati).
1. Ripetere il passaggio precedente per tutti gli altri utenti.

## <a name="next-steps"></a>Passaggi successivi

Ulteriori informazioni su DSVMs presso [Introduzione a Data Science macchine virtuali di Azure](/azure/machine-learning/data-science-virtual-machine/overview).
