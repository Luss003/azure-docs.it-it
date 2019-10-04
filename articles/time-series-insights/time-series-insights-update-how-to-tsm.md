---
title: Modellazione dei dati in Anteprima di Azure Time Series Insights | Microsoft Docs
description: Informazioni sulla modellazione dei dati in Anteprima di Azure Time Series Insights.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 09/04/2019
ms.custom: seodec18
ms.openlocfilehash: 245a69f5e5834e68bbbd17a96859a93bc16eacbe
ms.sourcegitcommit: 86d49daccdab383331fc4072b2b761876b73510e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/06/2019
ms.locfileid: "70744169"
---
# <a name="data-modeling-in-azure-time-series-insights-preview"></a>Modellazione dei dati in Anteprima di Azure Time Series Insights

Questo documento descrive come usare i modelli di serie temporali seguendo Anteprima di Azure Time Series Insights e illustra in dettaglio alcuni scenari di dati comuni.

Per altre informazioni su come usare l'aggiornamento, vedere [Strumento di esplorazione di Anteprima di Azure Time Series Insights](./time-series-insights-update-explorer.md).

## <a name="types"></a>Tipi

### <a name="create-a-single-type"></a>Creare un singolo tipo

1. Passare al pannello di selezione dei modelli di serie temporali e scegliere **Tipi** dal menu. Comprimere il pannello per esaminare i tipi di modelli di serie temporali.

    [![Creazione di un singolo tipo](media/v2-update-how-to-tsm/portal-one.png)](media/v2-update-how-to-tsm/portal-one.png#lightbox)

1. Selezionare **+ Aggiungi**.
1. Immettere tutti i dettagli relativi ai tipi e selezionare **Crea**. Questa azione crea i tipi nell'ambiente.

    [![Aggiungere un tipo](media/v2-update-how-to-tsm/portal-two.png)](media/v2-update-how-to-tsm/portal-two.png#lightbox)

### <a name="bulk-upload-one-or-more-types"></a>Caricamento in blocco di uno o più tipi

1. Selezionare **Carica JSON**.
1. Selezionare il file che contiene il payload del tipo.
1. Selezionare **Carica**.

    [![Carica JSON](media/v2-update-how-to-tsm/portal-three.png)](media/v2-update-how-to-tsm/portal-three.png#lightbox)

### <a name="edit-a-single-type"></a>Modificare un singolo tipo

1. Selezionare il tipo e quindi selezionare **Modifica**. 
1. Apportare le modifiche necessarie e quindi selezionare **Salva**.

    [![Modificare un tipo](media/v2-update-how-to-tsm/portal-four.png)](media/v2-update-how-to-tsm/portal-four.png#lightbox)

### <a name="delete-a-type"></a>Eliminare un tipo

1. Selezionare il tipo e quindi selezionare **Elimina**.
1. Se nessuna istanza è associata i tipi, viene eliminato.

    [![Eliminare un tipo](media/v2-update-how-to-tsm/portal-five.png)](media/v2-update-how-to-tsm/portal-five.png#lightbox)

## <a name="hierarchies"></a>Gerarchie

### <a name="create-a-single-hierarchy"></a>Creare una singola gerarchia

1. Passare al pannello di selezione dei modelli di serie temporali e scegliere **Gerarchie** dal menu. Comprimere il pannello per esaminare i tipi di gerarchie di serie temporali.

    [![Selezione gerarchie](media/v2-update-how-to-tsm/portal-six.png)](media/v2-update-how-to-tsm/portal-six.png#lightbox)

1. Selezionare **+ Aggiungi**.

    [![Aggiungere una gerarchia](media/v2-update-how-to-tsm/portal-seven.png)](media/v2-update-how-to-tsm/portal-seven.png#lightbox)

1. Selezionare **+ Aggiungi livello** nel riquadro di destra.

    [![Aggiungi un livello](media/v2-update-how-to-tsm/portal-eight.png)](media/v2-update-how-to-tsm/portal-eight.png#lightbox)

1. Immettere i dettagli della gerarchia e selezionare **Crea**.

    [![Creazione di un livello](media/v2-update-how-to-tsm/portal-nine.png)](media/v2-update-how-to-tsm/portal-nine.png#lightbox)

### <a name="bulk-upload-one-or-more-hierarchies"></a>Caricamento in blocco di una o più gerarchie

1. Selezionare **Carica JSON**.
1. Selezionare il file che contiene il payload della gerarchia.
1. Selezionare **Carica**.

    [![Gerarchie di caricamento bulk](media/v2-update-how-to-tsm/portal-ten.png)](media/v2-update-how-to-tsm/portal-ten.png#lightbox)

### <a name="edit-a-single-hierarchy"></a>Modificare una singola gerarchia

1. Selezionare la gerarchia e quindi selezionare **Modifica**.
1. Apportare le modifiche necessarie e quindi selezionare **Salva**.

    [![Modificare una singola gerarchia](media/v2-update-how-to-tsm/portal-eleven.png)](media/v2-update-how-to-tsm/portal-eleven.png#lightbox)

### <a name="delete-a-hierarchy"></a>Eliminare una gerarchia

1. Selezionare la gerarchia e quindi selezionare **Elimina**. 
1. Se nessuna istanza è associata alla gerarchia, viene eliminata.

    [![Eliminare una gerarchia](media/v2-update-how-to-tsm/portal-twelve.png)](media/v2-update-how-to-tsm/portal-twelve.png#lightbox)

## <a name="instances"></a>Istanze

### <a name="create-a-single-instance"></a>Creare una singola istanza

1. Passare al pannello di selezione dei modelli di serie temporali e scegliere **Istanze** dal menu. Comprimere il pannello per esaminare le istanze di modelli di serie temporali.

    [![Creare una singola istanza](media/v2-update-how-to-tsm/portal-thirteen.png)](media/v2-update-how-to-tsm/portal-thirteen.png#lightbox)

1. Selezionare **Aggiungi**.

    [![Aggiungere un'istanza](media/v2-update-how-to-tsm/portal-fourteen.png)](media/v2-update-how-to-tsm/portal-fourteen.png#lightbox)

1. Immettere i dettagli dell'istanza, selezionare l'associazione con il tipo e la gerarchia e selezionare **Crea**.

### <a name="bulk-upload-one-or-more-instances"></a>Caricamento in blocco di una o più istanze

1. Selezionare **Carica JSON**.
1. Selezionare il file che contiene il payload delle istanze.

    [![Caricamento bulk di una o più istanze](media/v2-update-how-to-tsm/portal-fifteen.png)](media/v2-update-how-to-tsm/portal-fifteen.png#lightbox)

1. Selezionare **Carica**.

### <a name="edit-a-single-instance"></a>Modificare una singola istanza

1. Selezionare l'istanza e quindi selezionare **Modifica**. 
1. Apportare le modifiche necessarie e quindi selezionare **Salva**.

    [![Modificare una singola istanza](media/v2-update-how-to-tsm/portal-sixteen.png)](media/v2-update-how-to-tsm/portal-sixteen.png#lightbox)

## <a name="next-steps"></a>Passaggi successivi

- Per altre informazioni sui modelli di serie temporali, vedere [Modellazione di dati](./time-series-insights-update-tsm.md).

- Per altre informazioni sull'anteprima, vedere [Visualizzare dati nello strumento di esplorazione di Anteprima di Azure Time Series Insights](./time-series-insights-update-explorer.md).

- Per informazioni sulle forme JSON supportate, vedere [Forme JSON supportate](./time-series-insights-send-events.md#json).
