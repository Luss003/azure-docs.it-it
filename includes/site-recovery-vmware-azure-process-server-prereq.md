---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 96cba4e077be8b7658c270b09b177a845e16c8b0
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/18/2019
ms.locfileid: "67180673"
---
Questo articolo presuppone quanto segue:

1. È già stata stabilita una connessione **VPN da sito a sito** o **ExpressRoute** tra la rete locale e la rete virtuale di Azure.
2. L'account utente ha le autorizzazioni necessarie per creare una nuova macchina virtuale nella sottoscrizione di Azure in cui è stato eseguito il failover delle macchine virtuali.
3. La sottoscrizione ha un minimo di quattro core disponibili per attivare una nuova macchina virtuale del server di elaborazione.
4. È disponibile la **passphrase del server di configurazione**.

> [!TIP]
> Assicurarsi che sia possibile connettere la porta 443 del server di configurazione, in esecuzione in locale, dalla rete virtuale di Azure in cui è stato eseguito il failover delle macchine virtuali.
