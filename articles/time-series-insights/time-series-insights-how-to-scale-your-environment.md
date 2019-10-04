---
title: Come scalare l'ambiente Time Series Insights di Azure | Microsoft Docs
description: Questo articolo spiega come ridimensionare l'ambiente Time Series Insights. Per aggiungere o sottrarre capacità in uno SKU di prezzo, usare il portale di Azure.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 08/27/2019
ms.custom: seodec18
ms.openlocfilehash: 13fa2b892013cf4a3fb96220c901030c6b0aee0b
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2019
ms.locfileid: "70129097"
---
# <a name="how-to-scale-your-time-series-insights-environment"></a>Come scalare l'ambiente Time Series Insights

Questo articolo spiega come modificare la capacità dell'ambiente Time Series Insights con il portale di Azure. Per capacità si intende il moltiplicatore applicato alla velocità in ingresso, alla capacità di archiviazione e ai costi associati allo SKU selezionato.

È possibile usare il portale di Azure per aumentare o diminuire la capacità in uno SKU di prezzo specifico.

Non è però consentita la modifica dello SKU di piano tariffario. Ad esempio, un ambiente con uno SKU di prezzo S1 non può essere convertito in un ambiente con SKU S2 o viceversa.

## <a name="s1-sku-ingress-rates-and-capacities"></a>Capacità e velocità di ingresso dello SKU S1

| Capacità SKU S1 | Velocità in ingresso | Capacità massima di archiviazione
| --- | --- | --- |
| 1 | 1 GB (1 milione di eventi) | 30 GB (30 milioni di eventi) al mese |
| 10 | 10 GB (10 milioni di eventi) | 300 GB (300 milioni di eventi) al mese |

## <a name="s2-sku-ingress-rates-and-capacities"></a>Capacità e velocità di ingresso dello SKU S2

| Capacità SKU S2 | Velocità in ingresso | Capacità massima di archiviazione
| --- | --- | --- |
| 1 | 10 GB (10 milioni di eventi) | 300 GB (300 milioni di eventi) al mese |
| 10 | 100 GB (100 milioni di eventi) | 3 TB (3 miliardi di eventi) al mese |

La capacità ha una scalabilità lineare, pertanto uno SKU S1 con capacità 2 supporta una velocità in ingresso di 2 GB (2 milioni) di eventi al giorno e 60 GB (60 milioni) di eventi al mese.

## <a name="change-the-capacity-of-your-environment"></a>Modificare la capacità dell'ambiente

1. Nel portale di Azure individuare selezionare l'ambiente Time Series Insights in uso.

1. Nel menu per l'ambiente Time Series Insights, selezionare **Configura**.

   [![configure.png](media/scale-your-environment/configure.png)](media/scale-your-environment/configure.png#lightbox)

1. Regolare il dispositivo di scorrimento **Capacità** per selezionare una capacità che soddisfi i requisiti in termini di velocità in ingresso e capacità di archiviazione. Si noti che la **velocità in ingresso**, la **capacità di archiviazione** e i **costi stimati** vengono aggiornati in modo dinamico per illustrare l'impatto della modifica.

   [![Cursore](media/scale-your-environment/slider.png)](media/scale-your-environment/slider.png#lightbox)

   In alternativa, è possibile digitare il numero del moltiplicatore di capacità nella casella di testo a destra del dispositivo di scorrimento.

1. Selezionare **Salva** per ridimensionare l'ambiente. Finché non viene eseguito il commit della modifica, viene visualizzato temporaneamente l'indicatore di stato.

## <a name="next-steps"></a>Passaggi successivi

- Verificare che la nuova capacità sia [sufficiente per evitare la limitazione delle richieste](time-series-insights-diagnose-and-solve-problems.md).