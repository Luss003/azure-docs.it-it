---
title: Informazioni sui risultati automatici di ML
titleSuffix: Azure Machine Learning
description: Informazioni su come visualizzare e comprendere i grafici e le metriche per ognuna delle esecuzioni automatiche di machine learning.
services: machine-learning
author: nilesha
ms.author: nilesha
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 07/22/2019
ms.openlocfilehash: b0024bc12f29a76da02c9f7e62af7727b9af7249
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71350635"
---
# <a name="understand-automated-machine-learning-results"></a>Informazioni sui risultati automatici di Machine Learning

Questo articolo illustra come visualizzare e comprendere i grafici e le metriche per ognuna delle esecuzioni automatiche di machine learning. 

Altre informazioni su:
+ [Metriche, grafici e curve per i modelli di classificazione](#classification)
+ [Metriche, grafici e grafici per i modelli di regressione](#regression)
+ [Interpretazione dei modelli e importanza delle funzionalità](#explain-model)

## <a name="prerequisites"></a>Prerequisiti

* Una sottoscrizione di Azure. Se non è disponibile una sottoscrizione di Azure, creare un account gratuito prima di iniziare. Provare la [versione gratuita o a pagamento di Azure Machine Learning](https://aka.ms/AMLFree).

* Creare un esperimento per l'esecuzione automatica di Machine Learning, con l'SDK, nella portale di Azure o nella pagina di destinazione dell'area di lavoro (anteprima).

    * Usare l'SDK per compilare un modello di [classificazione](how-to-auto-train-remote.md) o un [modello](tutorial-auto-train-models.md) di regressione
    * Usare la [portale di Azure o la pagina di destinazione dell'area di lavoro (anteprima)](how-to-create-portal-experiments.md) per creare un modello di classificazione o regressione caricando i dati appropriati.

## <a name="view-the-run"></a>Visualizza l'esecuzione

Dopo l'esecuzione di un esperimento di Machine Learning automatizzato, è possibile trovare una cronologia delle esecuzioni nell'area di lavoro del servizio Machine Learning. 

1. Passa all'area di lavoro.

1. Nel riquadro sinistro dell'area di lavoro selezionare **esperimenti**.

   ![Screenshot del menu Esperimenti](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-menu.png)

1. Nell'elenco degli esperimenti selezionare quello che si desidera esplorare.

   [![Elenco degli esperimenti](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-list.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-list-expanded.png)

1. Nella tabella inferiore selezionare il numero di **esecuzione**.

   Esecuzione dell'esperimento) [ ![](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-run.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-run-expanded.png)

1. Nella tabella iterazioni selezionare il numero di **iterazione** per il modello che si desidera esplorare ulteriormente.

   [![Modello di esperimento](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-model.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-model-expanded.png)

Gli stessi risultati vengono visualizzati anche durante un'esecuzione quando si usa il `RunDetails` [widget Jupyter](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py).

## <a name="classification"></a>Risultati della classificazione

Le metriche e i grafici seguenti sono disponibili per ogni modello di classificazione compilato usando le funzionalità automatiche di Machine Learning di Azure Machine Learning

+ [Metriche](#classification-metrics)
+ [Matrice di confusione](#confusion-matrix)
+ [Grafico di precisione-recupero](#precision-recall-chart)
+ [ROC (Receiver Operating Characteristics)](#roc)
+ [Curva di accuratezza](#lift-curve)
+ [Curva del guadagno](#gains-curve)
+ [Tracciato di calibrazione](#calibration-plot)

### <a name="classification-metrics"></a>Metriche per la classificazione

Le metriche seguenti vengono salvate in ogni iterazione di esecuzione per un'attività di classificazione.

|Metrica|Descrizione|Calcolo|Parametri aggiuntivi
--|--|--|--|
AUC_Macro| AUC è l'area sottesa alla curva ROC (Receiver Operating Characteristic). Macro è la media aritmetica dell'area AUC per ogni classe.  | [Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | average="macro"|
AUC_Micro| AUC è l'area sottesa alla curva ROC (Receiver Operating Characteristic). Micro viene calcolato a livello globale combinando i veri positivi e i falsi positivi da ogni classe.| [Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | average="micro"|
AUC_Weighted  | AUC è l'area sottesa alla curva ROC (Receiver Operating Characteristic). Ponderato è la media aritmetica del punteggio per ogni classe, ponderata in base al numero di istanze true in ogni classe.| [Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html)|average="weighted"
precisione|accuracy è la percentuale di etichette stimate che corrispondono esattamente alle etichette vere. |[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html) |Nessuna|
average_precision_score_macro|average_precision riepiloga una curva di precisione-recupero come media ponderata delle precisioni ottenute in corrispondenza di ogni soglia, usando come valore di ponderazione l'incremento nel recupero rispetto alla soglia precedente. Macro è la media aritmetica del Punteggio di precisione media di ogni classe.|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|average="macro"|
average_precision_score_micro|average_precision riepiloga una curva di precisione-recupero come media ponderata delle precisioni ottenute in corrispondenza di ogni soglia, usando come valore di ponderazione l'incremento nel recupero rispetto alla soglia precedente. Micro viene calcolato a livello globale combinando i veri positivi e i falsi positivi a ogni cutoff.|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|average="micro"|
average_precision_score_weighted|average_precision riepiloga una curva di precisione-recupero come media ponderata delle precisioni ottenute in corrispondenza di ogni soglia, usando come valore di ponderazione l'incremento nel recupero rispetto alla soglia precedente. Ponderata è la media aritmetica del Punteggio di precisione media per ogni classe, ponderata in base al numero di istanze true in ogni classe.|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|average="weighted"|
balanced_accuracy|balanced_accuracy è la media aritmetica del recupero per ogni classe.|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="macro"|
f1_score_macro|f1_score è la media armonica di precisione e recupero. Macro è la media aritmetica del Punteggio F1 per ogni classe.|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|average="macro"|
f1_score_micro|f1_score è la media armonica di precisione e recupero. Micro viene calcolato a livello globale contando i veri positivi totali, i falsi negativi e i falsi positivi.|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|average="micro"|
f1_score_weighted|f1_score è la media armonica di precisione e recupero. weighted è la media calcolata in base alla frequenza di F1_score per ogni classe.|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|average="weighted"|
log_loss|Questa è la funzione di perdita usata per la regressione logistica (multinomiale) e le relative estensioni, ad esempio le reti neurali, definita come probabilità logaritmica negativa delle etichette vere date le stime del classificatore probabilistico. Per un singolo esempio con true label YT in {0,1} e probabilità stimata YP che YT = 1, la perdita di log è-log P&#124;(YT YP) =-(log di YT (YP) + (1-YT) log (1-YP).|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.log_loss.html)|Nessuna|
norm_macro_recall|norm_macro_recall è il recupero macro normalizzato in modo che le prestazioni causali abbiano un punteggio 0 e le prestazioni perfette abbiano un punteggio 1. Questa operazione viene eseguita da norm_macro_recall: = (recall_score_macro-R)/(1-R), dove R è il valore previsto di recall_score_macro per le stime casuali (ad esempio, R = 0,5 per la classificazione binaria e R = (1/C) per i problemi di classificazione di classe C).|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Average = "macro" |
precision_score_macro|precision è la percentuale degli elementi cui è assegnata un'etichetta come una determinata classe e che si trovano effettivamente in tale classe. La macro è la media aritmetica di precisione per ogni classe.|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average="macro"|
precision_score_micro|precision è la percentuale degli elementi cui è assegnata un'etichetta come una determinata classe e che si trovano effettivamente in tale classe. Micro viene calcolato a livello globale contando i veri positivi totali e i falsi positivi.|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average="micro"|
precision_score_weighted|precision è la percentuale degli elementi cui è assegnata un'etichetta come una determinata classe e che si trovano effettivamente in tale classe. Ponderato è la media aritmetica di precisione per ogni classe, ponderata in base al numero di istanze true in ogni classe.|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average="weighted"|
recall_score_macro|recall è la percentuale di elementi effettivamente presenti in una determinata classe cui è assegnata un'etichetta corretta. Macro è la media aritmetica da richiamare per ogni classe.|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="macro"|
recall_score_micro|recall è la percentuale di elementi effettivamente presenti in una determinata classe cui è assegnata un'etichetta corretta. Micro viene calcolato a livello globale contando il totale dei veri positivi, falsi negativi e falsi positivi|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="micro"|
recall_score_weighted|recall è la percentuale di elementi effettivamente presenti in una determinata classe cui è assegnata un'etichetta corretta. Ponderato è la media aritmetica di richiamo per ogni classe, ponderata in base al numero di istanze true in ogni classe.|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="weighted"|
weighted_accuracy|L'accuratezza ponderata è l'accuratezza con cui il peso assegnato a ogni esempio è uguale alla proporzione di istanze true nella classe true di tale esempio.|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html)|sample_weight è un vettore equivalente alla proporzione di tale classe per ogni elemento nel target|

### <a name="confusion-matrix"></a>Matrice di confusione

Una matrice di confusione viene usata per descrivere le prestazioni di un modello di classificazione. Ogni riga visualizza le istanze della classe vera e ogni colonna rappresenta le istanze della classe stimata. La matrice di confusione mostra le etichette classificate correttamente e quelle classificate in modo errato per un determinato modello.

Per i problemi di classificazione, Azure Machine Learning offre automaticamente una matrice di confusione per ogni modello creato. Per ogni matrice di confusione, Machine Learning automatizzata Visualizza la frequenza di ogni etichetta stimata e ogni intersezione di etichetta vera. Più scuro è il colore, maggiore è il conteggio in quella particolare parte della matrice. Idealmente, i colori più scuri si trovavano lungo la diagonale della matrice. 

Esempio 1: modello di classificazione con scarsa accuratezza ![modello di classificazione con scarsa accuratezza](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-confusion-matrix1.png)

Esempio 2 modello di classificazione con elevata accuratezza (ideale) ![modello di classificazione con elevata accuratezza](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-confusion-matrix2.png)


### <a name="precision-recall-chart"></a>Grafico di precisione-recupero

Con questo grafico è possibile confrontare le curve di precisione-recupero per ogni modello al fine di determinare quale modello ha una relazione accettabile tra precisione e recupero per uno specifico problema aziendale. Questo grafico mostra la media macro di precisione-recupero, la media micro di precisione-recupero e il valore di precisione-recupero associato a tutte le classi per un modello.

Il termine "precisione" indica la capacità di un algoritmo di classificazione di etichettare correttamente tutte le istanze. Il termine "recupero" indica la capacità di un algoritmo di classificazione di trovare tutte le istanze di una determinata etichetta. La curva di precisione-recupero mostra la relazione tra questi due concetti. In una situazione ideale, il modello ha 100% di precisione e 100% di accuratezza.

Esempio 1: modello di classificazione con valori ridotti di precisione e recupero ![modello di classificazione con valori ridotti di precisione e recupero](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-precision-recall1.png)

Esempio 2 modello di classificazione con ~100% di precisione e ~100% di recupero (ideale) ![modello di classificazione con valori elevati di precisione e recupero](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-precision-recall2.png)

### <a name="roc"></a>ROC

ROC (Receiver Operating Characteristics) è un tracciato che mette a confronto le etichette classificate correttamente con quelle classificate in modo errato per un determinato modello. La curva ROC può essere meno informativa quando si esegue il training di modelli su set di dati con elevata distorsione perché non visualizza le etichette false positive.

Esempio 1: modello di classificazione con un numero ridotto di etichette vere e un numero elevato di etichette false ![modello di classificazione con un numero ridotto di etichette vere e un numero elevato di etichette false](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-roc-1.png)

Esempio 2 modello di classificazione con un numero elevato di etichette vere e un numero ridotto di etichette false ![modello di classificazione con un numero elevato di etichette vere e un numero ridotto di etichette false](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-roc-2.png)

### <a name="lift-curve"></a>Curva di accuratezza

È possibile confrontare l'accuratezza del modello creato automaticamente con Azure Machine Learning rispetto al modello di base per visualizzare il guadagno di valore di tale modello.

I grafici di accuratezza vengono usati per valutare le prestazioni di un modello di classificazione. Mostrano il miglioramento che ci si può aspettare di ottenere usando un modello rispetto all'apprendimento senza modello. 

Esempio 1: risultati del modello peggiori rispetto un modello di selezione casuale ![risultati del modello di classificazione peggiori rispetto un modello di selezione casuale](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-lift-curve1.png)

Esempio 2 risultati del modello migliori rispetto un modello di selezione casuale ![risultati del modello di classificazione migliori](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-lift-curve2.png)

### <a name="gains-curve"></a>Curva del guadagno

Un grafico del guadagno valuta le prestazioni di un modello di classificazione in base alle singole porzioni dei dati. Per ogni percentile del set di dati, mostra il miglioramento che ci si può aspettare di ottenere rispetto a un modello di selezione casuale.

L'uso del grafico del guadagno cumulativo consente di scegliere il limite di classificazione usando un valore percentuale corrispondente al guadagno che si vuole ottenere dal modello. Queste informazioni offrono un modo alternativo di esaminare i risultati nel grafico di accuratezza associato.

Esempio 1: modello di classificazione con guadagno minimo ![modello di classificazione con guadagno minimo](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-gains-curve1.png)

Esempio 2 modello di classificazione con guadagno significativo ![modello di classificazione con guadagno significativo](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-gains-curve2.png)

### <a name="calibration-plot"></a>Tracciato di calibrazione

Per tutti i problemi di classificazione, è possibile esaminare la linea di calibrazione per media micro, media macro e ogni classe in un determinato modello predittivo. 

Un tracciato di calibrazione viene usato per visualizzare il grado di fiducia di un modello predittivo. A tale scopo, il tracciato mostra la relazione tra probabilità stimata e probabilità effettiva, dove per "probabilità" si intende la possibilità che una particolare istanza appartenga a una determinata etichetta. Un modello ben calibrato è allineato alla linea y=x, che rappresenta una ragionevole fiducia nelle stime. Un modello con eccessiva fiducia è allineato alla linea y=0, dove la probabilità stimata è presente ma non esiste alcuna probabilità effettiva.

Esempio 1: modello ben calibrato ![ modello ben calibrato](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-calib-curve1.png)

Esempio 2 modello con eccessiva fiducia ![modello con eccessiva fiducia](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-calib-curve2.png)

## <a name="regression"></a>Risultati regressione

Le metriche e i grafici seguenti sono disponibili per ogni modello di regressione compilato usando le funzionalità automatiche di Machine Learning di Azure Machine Learning

+ [Metriche](#reg-metrics)
+ [Valori stimato rispetto a valore vero](#pvt)
+ [Istogramma dei valori residui](#histo)


### <a name="reg-metrics"></a>Metriche di regressione

Le metriche seguenti vengono salvate in ogni iterazione di esecuzione per un'attività di regressione o previsione.

|Metrica|Descrizione|Calcolo|Parametri aggiuntivi
--|--|--|--|
explained_variance|explained_variance è la proporzione in base alla quale un modello matematico tiene conto della variazione di un determinato set di dati. Si tratta della riduzione percentuale della varianza dei dati originali rispetto alla varianza degli errori. Quando la media degli errori è 0, la varianza è perfettamente spiegata.|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.explained_variance_score.html)|Nessuna|
r2_score|R2 è il coefficiente di determinazione o la riduzione percentuale di errori quadratici rispetto a un modello di base che restituisce la media. |[Calcolo](https://scikit-learn.org/0.16/modules/generated/sklearn.metrics.r2_score.html)|Nessuna|
spearman_correlation|La correlazione di Spearman è una misura non parametrica della monotonicità della relazione tra due set di dati. A differenza della correlazione di Pearson, quella di Spearman non presuppone che entrambi i set di dati siano normalmente distribuiti. Come altri coefficienti di correlazione, questo coefficiente varia da -1 a + 1, con 0 che implica l'assenza di correlazione. Le correlazioni con coefficiente -1 o + 1 implicano una relazione monotonica esatta. In caso di correlazione positiva, un incremento di x corrisponde a un incremento di y. In caso di correlazione negativa, un incremento di x corrisponde a un decremento di y.|[Calcolo](https://docs.scipy.org/doc/scipy-0.16.1/reference/generated/scipy.stats.spearmanr.html)|Nessuna|
mean_absolute_error|mean_absolute_error è il valore stimato del valore assoluto della differenza tra il target e la stima.|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Nessuna|
normalized_mean_absolute_error|normalized_median_absolute_error è l'errore assoluto medio diviso per l'intervallo dei dati.|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Dividere per l'intervallo dei dati|
median_absolute_error|median_absolute_error è il valore mediano di tutte le differenze assolute tra il target e la stima. Questa perdita è significativa per gli outlier.|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Nessuna|
normalized_median_absolute_error|normalized_median_absolute_error è l'errore assoluto mediano diviso per l'intervallo dei dati.|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Dividere per l'intervallo dei dati|
root_mean_squared_error|root_mean_squared_error è la radice quadrata della differenza quadratica stimata tra il target e la stima.|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Nessuna|
normalized_root_mean_squared_error|normalized_root_mean_squared_error è la radice dell'errore quadratico medio divisa per l'intervallo dei dati.|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Dividere per l'intervallo dei dati|
root_mean_squared_log_error|root_mean_squared_log_error è la radice quadrata dell'errore logaritmico quadratico stimato.|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Nessuna|
normalized_root_mean_squared_log_error|normalized_root_mean_squared_log_error è la radice dell'errore logaritmico quadratico medio divisa per l'intervallo dei dati|[Calcolo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Dividere per l'intervallo dei dati|

### <a name="pvt"></a>Confronto tra stimato e True

Questo grafico mostra la relazione tra un valore stimato e il valore vero correlato per un problema di regressione. Può essere usato per misurare le prestazioni di un modello poiché la vicinanza dei valori stimati alla linea y=x è direttamente proporzionale all'accuratezza di un modello predittivo.

Dopo ogni esecuzione, è possibile visualizzare un grafico del valore stimato rispetto al valore vero per ogni modello di regressione. Per proteggere la privacy dei dati, i valori sono raggruppati in contenitori e la dimensione di ogni contenitore viene visualizzata in un grafico a barre nella parte inferiore dell'area del grafico. È possibile confrontare il modello predittivo, con l'area più chiara indicante i margini di errore, rispetto al valore ideale che dovrebbe raggiungere il modello.

Esempio 1: modello di regressione con accuratezza delle stime ridotta ![modello di regressione con accuratezza delle stime ridotta](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression1.png)

Esempio 2 Modello di regressione con accuratezza elevata nelle stime [ ![di un modello di regressione con accuratezza elevata nelle stime](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression2.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression2-expanded.png)



### <a name="histo"></a>Istogramma dei residui

Un valore residuo rappresenta la differenza tra un valore y osservato e il valore y stimato. Per mostrare un margine di errore con distorsione ridotta, l'istogramma dei valori residui deve avere la forma di una curva a campana, con il centro vicino allo 0. 

Esempio 1: modello di regressione con distorsione negli errori ![modello di regressione con distorsione negli errori](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression3.png)

Esempio 2 modello di regressione con una distribuzione degli errori più uniforme![modello di regressione con una distribuzione degli errori più uniforme](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression4.png)

## <a name="explain-model"></a>Interpretazione dei modelli e importanza delle funzionalità

L'importanza della funzionalità consente di visualizzare il valore di ogni caratteristica nella costruzione di un modello. Questo calcolo è disattivato per impostazione predefinita, in quanto può aumentare significativamente il tempo di esecuzione.   È possibile abilitare la spiegazione del modello per tutti i modelli o spiegare solo il modello più adatto.

È possibile esaminare l'importanza delle caratteristiche per l'intero modello e per classe di un modello predittivo. È inoltre possibile visualizzare il confronto dell'importanza di ogni caratteristica rispetto alle singole classi e al modello complessivo.

![Spiegabilità delle caratteristiche](./media/how-to-understand-automated-ml/feature-importance.gif)

Per altre informazioni sull'abilitazione delle funzionalità di interpretazione, vedere [configurare esperimenti di Machine Learning automatici in Python](how-to-configure-auto-train.md#explain-the-model-interpretability).  Per un esempio in cui viene illustrato il modello migliore, vedere la [Descrizione del modello migliore](how-to-auto-train-remote.md#explain).

## <a name="next-steps"></a>Passaggi successivi

+ Scopri di più [su Machine](concept-automated-ml.md) learning in Azure Machine Learning.
+ Provare il notebook di esempio per la [spiegazione del modello di Machine Learning automatizzato](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning/model-explanation) .
