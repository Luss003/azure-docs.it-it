---
title: File di inclusione
description: File di inclusione
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: include
ms.date: 06/17/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 29ab9b3c33aae6005510c34b207c7f87714149e5
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2019
ms.locfileid: "67608230"
---
### <a name="preview"></a>Come si partecipa all'anteprima pubblica?

Per partecipare all'anteprima pubblica, è necessario eseguire l'onboarding. Seguire i passaggi di [questo articolo](../articles/bastion/bastion-create-host-portal.md) per creare una nuova risorsa di Azure Bastion. Attualmente, per accedere e usare il servizio, è necessario usare il [Portale di Azure - anteprima](https://aka.ms/BastionHost) invece del nomale portale di Azure.

### <a name="regions"></a>Quali aree sono disponibili durante l'anteprima?

È possibile distribuire e usare la risorsa Bastion in una qualsiasi di queste aree di anteprima tramite il [collegamento al portale di Azure - anteprima](https://aka.ms/BastionHost).

[!INCLUDE [region](bastion-regions-include.md)]

### <a name="portal"></a>Non è possibile trovare la risorsa Bastion nel portale di Azure. Cosa devo fare?

Assicurarsi di usare il [collegamento al portale di Azure - anteprima](https://aka.ms/BastionHost), non il normale portale di Azure.

### <a name="publicip"></a>È necessario un indirizzo IP pubblico nella macchina virtuale?

NON è necessario un indirizzo IP pubblico nella macchina virtuale di Azure a cui ci si connette con il servizio Azure Bastion. Il servizio Bastion aprirà la sessione/connessione RDP/SSH con la macchina virtuale tramite l'indirizzo IP privato della macchina virtuale, all'interno della rete virtuale.

### <a name="rdpssh"></a>È necessario un client RDP o SSH?

Non è necessario un client RDP o SSH per accedere alla macchina virtuale di Azure tramite RDP/SSH nel portale di Azure. Usare il [collegamento al Portale di Azure - anteprima](https://aka.ms/BastionHost) per accedere alla versione in anteprima del portale. In questo modo si potrà accedere alla macchina virtuale tramite RDP/SSH direttamente nel browser.

### <a name="agent"></a>È necessario un agente in esecuzione nella macchina virtuale di Azure?

Non è necessario installare un agente o altro software nel browser o nella macchina virtuale di Azure. Il servizio Bastion è senza agente e non richiede alcun software aggiuntivo per la connettività RDP/SSH.

### <a name="browsers"></a>Quali browser sono supportati?

Durante l'anteprima pubblica, usare il browser Microsoft Edge o Google Chrome in Windows. Per Apple Mac, usare il browser Google Chrome. Anche Microsoft Edge Chromium è supportato sia in Windows sia in Mac.

### <a name="roles"></a>Sono necessari ruoli specifici per accedere alla macchina virtuale?

Per stabilire una connessione, sono necessari i ruoli seguenti:

* Ruolo Lettore nella macchina virtuale
* Ruolo Lettore nella scheda di interfaccia di rete con l'indirizzo IP privato della macchina virtuale
* Ruolo Lettore nella risorsa Azure Bastion

### <a name="previewbill"></a>Prezzi - È previsto un addebito per la partecipazione alla versione di anteprima?

Durante l'anteprima pubblica è previsto solo un addebito parziale. Tuttavia, insieme alla distribuzione non è previsto alcun contratto di servizio. Per altre informazioni vedere la [pagina dei prezzi](https://aka.ms/BastionHostPricing).

### <a name="previewbill"></a>Perché viene visualizzato il messaggio di errore "Sessione utente scaduta" prima dell'avvio della sessione di Bastion?

Una sessione deve essere avviata solo dal portale di Azure. Accedere al portale di Azure e avviare di nuovo la sessione. Questo errore è previsto se si passa all'URL direttamente da un'altra sessione o scheda del browser. Consente di assicurarsi che la sessione sia più sicura e che sia accessibile solo tramite il portale di Azure.
