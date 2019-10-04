---
title: File di inclusione
description: File di inclusione
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 10/09/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 98d765e2f6909f00f8dfe76d06aef017aad67adf
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2019
ms.locfileid: "71174963"
---
## <a name="terminology"></a>Terminologia

Un'immagine del Marketplace in Azure presenta gli attributi seguenti:

* **Publisher** (Editore): L'organizzazione che ha creato l'immagine. Esempi: Canonical, MicrosoftWindowsServer
* **Offer** (Offerta): Il nome di un gruppo di immagini correlate create da un server di pubblicazione. Esempi: UbuntuServer, WindowsServer
* **SKU**: Un'istanza di un'offerta, ad esempio una versione principale di una distribuzione. Esempi: 18.04-LTS, 2019-Datacenter
* **Versione**: Il numero di versione dello SKU di un'immagine. 

Per identificare un'immagine del Marketplace quando si distribuisce una macchina virtuale a livello di codice, specificare questi valori singolarmente come parametri. Alcuni strumenti accettano un *URN* dell'immagine, che combina questi valori, separati dai due punti: *Server di pubblicazione*:*Offerta*:*SKU*:*Versione*. In un URN è possibile sostituire il numero di versione con "latest", che seleziona la versione più recente dell'immagine. 

Se l'editore dell'immagine fornisce condizioni di licenza e di acquisto aggiuntive, è necessario accettare tali condizioni e abilitare la distribuzione a livello di codice. È anche necessario fornire i parametri del *piano di acquisto* quando si distribuisce una macchina virtuale a livello di codice. Vedere [Distribuire un'immagine con le condizioni del Marketplace](#deploy-an-image-with-marketplace-terms).
