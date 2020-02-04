---
title: Stati della sottoscrizione di Azure
description: Descrive i diversi stati di una sottoscrizione di Azure
keywords: stato sottoscrizione di azure
author: anuragdalmia
manager: andalmia
tags: billing
ms.service: cost-management-billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/26/2019
ms.author: andalmia
ms.openlocfilehash: 741e7cf0869e36b5788d0a883a4e982caf041512
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/31/2020
ms.locfileid: "75995936"
---
# <a name="azure-subscription-states"></a>Stati della sottoscrizione di Azure
Questo articolo descrive i vari stati di una sottoscrizione di Azure. Questi stati vengono visualizzati come "Stato" nei pannelli delle sottoscrizioni.

| Stato sottoscrizione | Descrizione |
|-------------| ----------------|
| **Attivo** | La sottoscrizione di Azure è attiva. È possibile usare questa sottoscrizione per distribuire nuove risorse e gestire quelle esistenti.|
| **Scaduta** | Per la sottoscrizione di Azure è presente un pagamento in sospeso. La sottoscrizione è ancora attiva, ma il mancato pagamento delle quote può causare la disabilitazione della sottoscrizione. [Risolvere problemi relativi a un saldo dovuto non pagato per la sottoscrizione di Azure.](https://docs.microsoft.com/azure/billing/billing-azure-subscription-past-due-balance) |
| **Disabilitato** | La sottoscrizione di Azure è disabilitata e non può più essere usata per creare o gestire le risorse di Azure. In questo stato, le macchine virtuali vengono deallocate, gli indirizzi IP temporanei vengono liberati, la risorsa di archiviazione diventa di sola lettura e altri servizi vengono disabilitati. La sottoscrizione può essere disabilitata perché il credito potrebbe essere scaduto, potrebbe essere stato raggiunto il limite di spesa, è scaduto un pagamento, è stato raggiunto il limite della carta di credito oppure la sottoscrizione è stata disabilitata/annullata esplicitamente. A seconda del tipo di sottoscrizione e del motivo della disabilitazione, una sottoscrizione può rimanere disabilitata da 1 a 90 giorni, periodo dopo il quale viene eliminata definitivamente. [Riattivare una sottoscrizione di Azure disabilitata.](https://docs.microsoft.com/azure/billing/billing-subscription-become-disable)|
| **Eliminata** | La sottoscrizione di Azure è stata eliminata insieme a tutti i dati e le risorse sottostanti. |
