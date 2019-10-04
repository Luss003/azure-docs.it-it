---
title: Scheda Ottimizza per i flussi di dati di mapping di Azure Data Factory
description: Ottimizzare Azure Data Factory il Mapping di flusso dei dati usando la scheda ottimizzare con le impostazioni della partizione
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 40023931b2a0b3788a583a5b5240e7916b187e34
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190652"
---
# <a name="mapping-data-flow-transformation-optimize-tab"></a>Mapping dei dati di trasformazione del flusso di ottimizzare scheda

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Ogni trasformazione di un flusso di dati è associata a una scheda "Ottimizza". La scheda Ottimizza contiene impostazioni facoltative per configurare gli schemi di partizionamento per i flussi di dati.

<img src="media/data-flow/opt001.png" width="800">

L'impostazione predefinita è "Use current partitioning" (Usa partizionamento corrente). Il partizionamento corrente indica ad Azure Data Factory di usare lo schema di partizionamento nativo per i flussi di dati in esecuzione su Spark in Azure Databricks. In genere, questo è l'approccio consigliato.

Tuttavia, in alcuni casi è preferibile modificare il partizionamento. Ad esempio, se si vuole restituire le trasformazioni in un singolo file nel lake, scegliere "Partizione singola" nella scheda Ottimizza per il partizionamento nella trasformazione sink.

È utile avere il controllo sugli schemi di partizionamento usati per le trasformazioni dei dati anche in termini di prestazioni. La modifica del partizionamento dei dati offre un livello di controllo sulla distribuzione dei dati tra nodi di calcolo e ottimizzazioni della località dei dati che può avere effetti sia positivi sia negativi sulle prestazioni complessive del flusso di dati.

Se si vuole modificare il partizionamento in qualsiasi trasformazione, è sufficiente fare clic sulla scheda Ottimizza e selezionare il pulsante di opzione "Set Partitioning" (Imposta partizionamento). Verrà visualizzata una serie di opzioni per il partizionamento. Il metodo di partizionamento migliore da implementare differisce in base a volumi di dati, chiavi candidate, valori Null e cardinalità. La procedura consigliata consiste nell'iniziare con il partizionamento predefinito e quindi provare opzioni di partizionamento diverse. È possibile testare il partizionamento usando l'esecuzione del debug nella pipeline e quindi visualizzare il tempo impiegato in ogni raggruppamento di trasformazioni, nonché l'utilizzo della partizioni nella visualizzazione di monitoraggio.

<img src="media/data-flow/opt002.png" width="600">

### <a name="round-robin"></a>Round robin

Questa opzione corrisponde a una semplice partizione che distribuisce automaticamente i dati in modo uniforme tra partizioni. Usare questa opzione quando non sono disponibili chiavi candidate adeguate per implementare una strategia di partizionamento intelligente e affidabile. È possibile impostare il numero di partizioni fisiche.

### <a name="hash"></a>Hash

Azure Data Factory genera un hash di colonne per produrre partizioni uniformi in modo che le righe con valori simili rientrino nella stessa partizione. Quando si usa questa opzione, testare la soluzione per individuare possibili asimmetrie delle partizioni. È possibile impostare il numero di partizioni fisiche.

### <a name="dynamic-range"></a>Dynamic Range (Intervallo dinamico)

Questa opzione usa intervalli dinamici di Spark in base alle colonne o alle espressioni fornite. È possibile impostare il numero di partizioni fisiche. 

### <a name="fixed-range"></a>Fixed Range (Intervallo fisso)

È necessario creare un'espressione che fornisca un intervallo fisso per i valori all'interno delle colonne di dati partizionati. Prima di usare questa opzione, è bene avere una buona comprensione dei dati, per evitare l'asimmetria delle partizioni. I valori che immessi per l'espressione da utilizzare come parte di una funzione di partizione. È possibile impostare il numero di partizioni fisiche.

### <a name="key"></a>Chiave

Se si ha una buona comprensione della cardinalità dei dati, il partizionamento con chiavi può essere una buona strategia di partizionamento. Il partizionamento con chiavi crea partizioni per ogni valore univoco nella colonna. Non è possibile impostare il numero di partizioni, perché questo numero è basato su valori univoci nei dati.

## <a name="next-steps"></a>Passaggi successivi

[Guida alle prestazioni del flusso di dati di mapping](concepts-data-flow-performance.md)
[del flusso di dati di monitoraggio](concepts-data-flow-monitoring.md)
