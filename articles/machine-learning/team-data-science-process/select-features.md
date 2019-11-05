---
title: Selezione delle funzionalità nel Processo di analisi scientifica dei dati per i team
description: Illustra le finalità della selezione delle funzioni e fornisce esempi del relativo ruolo nel processo di miglioramento dei dati di Machine Learning.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/21/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: da5da64538ceaf906388c49963c0d5115e1b5ab9
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/04/2019
ms.locfileid: "73480220"
---
# <a name="feature-selection-in-the-team-data-science-process-tdsp"></a>Selezione delle funzionalità nel Processo di analisi scientifica dei dati per i team (TDSP)
Questo articolo illustra la selezione della funzione e fornisce esempi del relativo ruolo nel processo di miglioramento dei dati di Machine Learning. Questi esempi sono tratti da Azure Machine Learning Studio.

La progettazione e la selezione delle funzionalità costituiscono una parte del processo TDSP descritto nell'articolo di [introduzione al processo di data science per i team](overview.md). La progettazione e la selezione delle funzionalità sono parti del passaggio **Sviluppare funzionalità** del processo TDSP.

* **Progettazione di funzionalità**: questo processo tenta di creare altre funzioni rilevanti dalle funzioni non elaborate esistenti nei dati e di aumentare le potenzialità predittive dell'algoritmo di apprendimento.
* **Selezione di funzionalità**: questo processo seleziona il subset principale delle funzionalità dei dati originali nel tentativo di ridurre la dimensionalità del problema di training.

In genere, la **progettazione di funzionalità** viene applicata prima di tutto per generare altre funzionalità e quindi viene eseguito il passaggio di **selezione delle funzionalità** per eliminare quelle irrilevanti, ridondanti o altamente correlate.

## <a name="filter-features-from-your-data---feature-selection"></a>Filtro delle funzioni dai dati: selezione di funzioni
La selezione delle funzioni è un processo applicato comunemente per la costruzione di set di dati di training per le attività di modellazione predittive, come la classificazione o la regressione. L'obiettivo consiste nel selezionare un subset di funzioni dal set di dati originale, che riducono le dimensioni usando un set minimo di funzioni per rappresentare la quantità massima di varianza nei dati. Questo subset di funzioni viene usato per il training del modello. La selezione delle funzioni ha due scopi principali.

* La selezione di funzioni migliora spesso la  precisione della classificazione eliminando le funzioni irrilevanti, ridondanti o altamente correlate.
* In secondo luogo, riduce il numero di funzioni rendendo più efficiente il processo di training del modello. L'efficienza è particolarmente importante per gli strumenti di apprendimento, come le macchine a vettori di supporto, il cui training risulta costoso.

Anche se la selezione delle funzioni cerca di ridurre il numero di funzioni nel set di dati usato per il training del modello, non viene definita con il termine "riduzione della dimensionalità". I metodi di selezione delle funzioni estraggono un subset delle funzioni originali presenti nei dati senza apportarvi modifiche.  I metodi di riduzione della dimensionalità usano funzioni progettate che possono trasformare le funzioni originali e quindi modificarle. Analisi in componenti principali, analisi di correlazione canonica e scomposizione di valori singolari sono esempi di metodi di riduzione della dimensionalità.

Tra le altre, una categoria di metodi di selezione delle funzioni ampiamente applicata in un contesto supervisionato è chiamata "selezione di funzioni in base al filtro". Tramite la valutazione della correlazione tra ogni funzione e l'attributo di destinazione, questi metodi applicano una misura statistica per assegnare un punteggio a ogni funzione. Queste funzioni vengono quindi classificate in base al punteggio, che può essere usato per facilitare l'impostazione della soglia per mantenere o eliminare una funzione specifica. Correlazione di Pearson, informazione mutua e test del chi quadrato sono esempi di misure statistiche usate in questi metodi.

In Azure Machine Learning Studio sono disponibili moduli per la selezione delle funzioni. Come illustrato nella figura seguente, questi moduli includono la [selezione delle funzioni basata su filtri][filter-based-feature-selection] e l' [analisi discriminante lineare di Fisher][fisher-linear-discriminant-analysis].

![Moduli di selezione delle funzioni](./media/select-features/feature-Selection.png)

Si consideri, ad esempio, l'uso del modulo [Filter-based feature selection][filter-based-feature-selection] . Per praticità, continuare a usare l'esempio di data mining di testo. Si supponga di voler compilare un modello di regressione dopo aver creato un set di 256 funzionalità con il modulo [feature hashing][feature-hashing] e che la variabile di risposta sia "col1" che contiene le classificazioni di revisione del libro comprese tra 1 e 5. Impostando "Feature scoring method" su "Pearson Correlation", "Target column" su "Col1" e "Number of desired features" su 50, Quindi, la [selezione delle funzioni basata su filtro][filter-based-feature-selection] del modulo produce un set di dati contenente le funzionalità di 50 insieme all'attributo di destinazione "col1". Nella figura seguente è illustrato il flusso dell'esperimento e i parametri di input:

![Proprietà del modulo Selezione delle funzioni basata su filtro](./media/select-features/feature-Selection1.png)

Nella figura seguente sono illustrati i set di dati risultanti:

![Set di dati risultante dal modulo Selezione delle funzioni basata su filtro](./media/select-features/feature-Selection2.png)

Il punteggio di ogni funzione viene calcolato in base alla correlazione di Pearson tra la funzione stessa e l'attributo di destinazione "Col1". Vengono mantenute le funzioni con i punteggi più alti.

I punteggi corrispondenti delle funzioni selezionate sono illustrati nella figura seguente:

![Punteggi del modulo Selezione delle funzioni basata su filtro](./media/select-features/feature-Selection3.png)

Applicando questo modulo di [selezione delle funzionalità basato su filtro][filter-based-feature-selection] , vengono selezionate 50 di 256 funzionalità che includono le funzionalità più correlate con la variabile di destinazione "col1", in base al metodo di assegnazione dei punteggi "correlazione di Pearson".

## <a name="conclusion"></a>Conclusione
La progettazione delle funzionalità e la selezione delle funzionalità sono due funzionalità progettate e selezionate che migliorano l'efficienza del processo di training che tenta di estrarre le informazioni essenziali contenute nei dati. Migliorano anche le potenzialità di questi modelli per la classificazione accurata dei dati di input e per la stima più affidabile dei risultati di interesse. Progettazione e selezione delle funzioni possono anche combinarsi per rendere l'apprendimento più computazionalmente trattabile. Questa operazione viene eseguita tramite il miglioramento e la successiva riduzione del numero di funzioni richieste per calibrare o eseguire il training di un modello. Da un punto di vista matematico, le funzioni selezionate per eseguire il training di un modello sono costituite da un set minimo di variabili indipendenti che spiegano i modelli nei dati e quindi stimano correttamente i risultati.

Non sempre è necessario eseguire la progettazione o la selezione delle funzioni. La necessità o meno di questi passaggi dipende dai dati raccolti, dall'algoritmo selezionato e dall'obiettivo dell'esperimento.

<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/

