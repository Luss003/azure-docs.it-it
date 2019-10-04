---
title: Stati e fatturazione LiveEvent in Servizi multimediali di Azure | Microsoft Docs
description: Questo argomento offre una panoramica degli stati e della fatturazione dei Servizi multimediali di Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/28/2019
ms.author: juliako
ms.openlocfilehash: 2907b5be7f8d5fda3d510484179e80b065ab64b0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "67074884"
---
# <a name="live-event-states-and-billing"></a>Stati e fatturazione di eventi live

In Servizi multimediali di Azure, la fatturazione di un evento live non appena lo stato dell'evento live passa a **In esecuzione**. Per interrompere la fatturazione dell'evento live, è necessario interrompere quest'ultimo.

Quando **LiveEventEncodingType** nel [evento Live](https://docs.microsoft.com/rest/api/media/liveevents) è impostato su Standard o Premium1080p, servizi multimediali di auto spegne qualsiasi evento in tempo reale che è ancora nel **in esecuzione** stato 12 ore dopo che il feed di input verrà perso e non esistono nessun **Live Output**s in esecuzione. Tuttavia verrà comunque fatturato il tempo durante il quale lo stato dell'evento live è rimasto impostato su **In esecuzione**.

## <a name="states"></a>Stati

Di seguito sono riportati gli stati possibili per l'evento live.

|Stato|Descrizione|
|---|---|
|**Stopped**| Stato iniziale dell'evento live dopo la creazione, se l'opzione di avvio automatico non è impostata su true. In questo stato non viene eseguita alcuna attività di fatturazione. In questo stato è possibile aggiornare le proprietà dell'evento live ma lo streaming non è consentito.|
|**Avvio**| L'avvio dell'evento live e l'allocazione delle risorse sono in corso. In questo stato non viene eseguita alcuna attività di fatturazione. Durante questo stato non sono consentiti aggiornamenti o streaming. Se si verifica un errore, l'evento live torna allo stato Interrotto.|
|**Running**| Le risorse dell'evento live sono state allocate, gli URL di inserimento e anteprima sono stati generati ed è possibile ricevere flussi live. A questo punto, la fatturazione è attiva. È necessario chiamare esplicitamente Stop sulla risorsa evento live per interrompere la fatturazione.|
|**Arresto**| L'interruzione dell'evento live e il deprovisioning delle risorse sono in corso. In questo stato di transizione non viene eseguita alcuna attività di fatturazione. Durante questo stato non sono consentiti aggiornamenti o streaming.|
|**Eliminazione in corso**| L'eliminazione dell'evento live è in corso. In questo stato di transizione non viene eseguita alcuna attività di fatturazione. Durante questo stato non sono consentiti aggiornamenti o streaming.|

## <a name="next-steps"></a>Passaggi successivi

- [Panoramica dello streaming live](live-streaming-overview.md)
- [Esercitazione sullo streaming live](stream-live-tutorial-with-api.md)
