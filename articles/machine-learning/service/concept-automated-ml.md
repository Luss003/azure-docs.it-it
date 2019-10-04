---
title: Che cos'è il Machine Learning automatico/automl
titleSuffix: Azure Machine Learning
description: Informazioni su come Azure Machine Learning possibile selezionare automaticamente un algoritmo e generare un modello da esso per risparmiare tempo usando i parametri e i criteri forniti per selezionare l'algoritmo migliore per il modello.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nacharya1
ms.author: nilesha
ms.date: 06/20/2019
ms.custom: seodec18
ms.openlocfilehash: 8b38b359821d3d4926085fee8e412fbe06155739
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71350622"
---
# <a name="what-is-automated-machine-learning"></a>Informazioni sulle funzionalità automatizzate di Machine Learning

Il Machine Learning automatizzato, detto anche ML automatizzato, è il processo che consente di automatizzare le attività iterative che richiedono molto tempo per lo sviluppo di modelli di machine learning. Consente a data scientist, analisti e sviluppatori di creare modelli ML con scalabilità, efficienza e produttività elevate, garantendo al tempo stesso la qualità del modello. Il Machine Learning automatico si basa su un'innovazione della [divisione Microsoft Research](https://arxiv.org/abs/1705.05355).

Lo sviluppo tradizionale di modelli di Machine Learning prevede un uso intensivo delle risorse, che richiede una conoscenza significativa del dominio e tempi per produrre e confrontare dozzine di modelli. Applicare il Machine Learning automatico quando si desidera Azure Machine Learning per eseguire il training e l'ottimizzazione di un modello utilizzando la metrica di destinazione specificata. Il servizio esegue quindi l'iterazione degli algoritmi ML abbinati alle selezioni di funzionalità, in cui ogni iterazione produce un modello con un punteggio di training. Maggiore è il punteggio, maggiore è il modello considerato "adatta" ai dati.

Grazie alla funzionalità di Machine Learning automatizzata, potrai accelerare il tempo necessario per ottenere modelli di ML pronti per la produzione con grande facilità ed efficienza.

## <a name="when-to-use-automated-ml"></a>Quando usare Machine Learning automatiche

Automatizzato ML democratizza il processo di sviluppo di modelli di machine learning e consente agli utenti, indipendentemente dalla loro esperienza di data science, di identificare una pipeline di Machine Learning end-to-end per eventuali problemi.

I data scientist, gli analisti e gli sviluppatori di tutti i settori possono usare il Machine Learning automatico per:

+ Implementare soluzioni di Machine Learning senza una conoscenza approfondita della programmazione
+ Risparmiare tempo e risorse
+ Utilizzare data science procedure consigliate
+ Fornire risoluzione dei problemi agile

## <a name="how-automated-ml-works"></a>Funzionamento del Machine Learning automatico

Con **Azure Machine Learning**è possibile progettare ed eseguire gli esperimenti di training di Machine Learning automatici con i passaggi seguenti:

1. **Identificare il problema di ml** da risolvere: classificazione, previsione o regressione

1. **Specificare l'origine e il formato dei dati di training con etichetta**: Matrici numpy o dataframe Pandas

1. **Configurare la destinazione di calcolo per il training del modello**, ad esempio il [computer locale, Azure Machine Learning calcoli, le macchine virtuali remote o Azure Databricks](how-to-set-up-training-targets.md).  Informazioni sulla formazione automatica [su una risorsa remota](how-to-auto-train-remote.md).

1. **Configurare i parametri automatici di Machine Learning** che determinano il numero di iterazioni su modelli diversi, le impostazioni iperparametri, la pre-elaborazione avanzata/conteggi e le metriche da esaminare quando si determina il modello migliore.  È possibile configurare le impostazioni per l'esperimento di training automatico in [portale di Azure](how-to-create-portal-experiments.md), [la pagina di destinazione dell'area di lavoro (anteprima)](https://ml.azure.com)o [con l'SDK](how-to-configure-auto-train.md). 

1. **Inviare l'esecuzione del training.**

  ![Machine Learning automatizzato](./media/how-to-automated-ml/automl-concept-diagram2.png)

Durante il training, Azure Machine Learning crea alcune pipeline parallele che tentano algoritmi e parametri diversi. Si arresterà quando si raggiungeranno i criteri di uscita definiti nell'esperimento.

È anche possibile esaminare le informazioni sull'esecuzione registrata, che [contengono le metriche](how-to-understand-automated-ml.md) raccolte durante l'esecuzione. L'esecuzione del training produce un oggetto (`.pkl` file) serializzato Python che contiene il modello e la pre-elaborazione dei dati.

Sebbene la creazione di modelli sia automatizzata, è anche possibile [scoprire quanto siano importanti o rilevanti le funzionalità](how-to-configure-auto-train.md#explain) dei modelli generati.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Xc9t]

<a name="preprocess"></a>

## <a name="preprocessing"></a>Pre-elaborazione

In ogni esperimento di Machine Learning automatizzato, i dati vengono pre-elaborati usando i metodi predefiniti e, facoltativamente, tramite la pre-elaborazione avanzata.

> [!NOTE]
> I passaggi di pre-elaborazione di Machine Learning automatizzati (normalizzazione delle funzionalità, gestione dei dati mancanti, conversione di valori di testo nel formato numerico e così via) diventano parte del modello sottostante. Quando si usa il modello per le previsioni, gli stessi passaggi di pre-elaborazione applicati durante il training vengono automaticamente applicati ai dati di input.

### <a name="automatic-preprocessing-standard"></a>Pre-elaborazione automatica (standard)

In ogni esperimento di Machine Learning automatizzato, i dati vengono ridimensionati o normalizzati automaticamente per consentire agli algoritmi di ottenere risultati ottimali.  Durante il training del modello, viene applicata una delle tecniche di ridimensionamento o di normalizzazione seguenti a ogni modello.

|Normalizzazione scalabile&nbsp;&&nbsp;| Descrizione |
| ------------- | ------------- |
| [StandardScaleWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)  | Standardizzare le funzionalità rimuovendo la media e il ridimensionamento in varianza unità  |
| [MinMaxScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html)  | Trasforma le funzionalità ridimensionando ogni funzionalità in base al valore minimo e massimo della colonna  |
| [MaxAbsScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MaxAbsScaler.html#sklearn.preprocessing.MaxAbsScaler) |Ridimensionare ogni funzionalità in base al valore assoluto massimo |
| [RobustScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html) |Questa funzionalità scaler in base al rispettivo intervallo di quantile |
| [PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) |Riduzione della dimensionalità lineare mediante scomposizione di valori singolari dei dati per proiettarla in uno spazio di dimensioni inferiori |
| [TruncatedSVDWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html) |Questo trasformatore esegue la riduzione della dimensionalità lineare per mezzo della scomposizione del valore singolare troncato (SVD). Contrariamente a PCA, questo estimatore non centra i dati prima di calcolare la scomposizione del valore singolare. Ciò significa che può funzionare in modo efficiente con matrici SciPy. sparse |
| [SparseNormalizer](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Normalizer.html) | Ogni esempio, ovvero ogni riga della matrice di dati, con almeno un componente diverso da zero, viene riscalato in modo indipendente dagli altri esempi, in modo che la relativa norma (L1 o L2) sia uguale a uno |

### <a name="advanced-preprocessing-optional-featurization"></a>Pre-elaborazione avanzata: conteggi facoltativo

Sono disponibili anche funzionalità avanzate di pre-elaborazione e conteggi, ad esempio la mancata imputazione, la codifica e le trasformazioni dei valori mancanti. [Scopri di più su cosa è incluso conteggi](how-to-create-portal-experiments.md#preprocess). Abilitare questa impostazione con:

+ Portale di Azure: Selezionare la casella di controllo **pre-elaborazione** nelle **Impostazioni avanzate** [con questi passaggi](how-to-create-portal-experiments.md).

+ SDK per Python: Oggetto `"preprocess": True` che specifica per la [ `AutoMLConfig` classe](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py).


## <a name="time-series-forecasting"></a>Previsione di serie temporali
La compilazione di previsioni è parte integrante di qualsiasi azienda, sia che si tratti di ricavi, scorte, vendite o richieste dei clienti. È possibile utilizzare Machine Learning Machine Learning per combinare tecniche e approcci e ottenere una previsione della serie temporale consigliata e di alta qualità.

Un esperimento di serie temporali automatizzato viene considerato un problema di regressione MultiVariante. I valori delle serie temporali precedenti sono "trasformati tramite pivot" per diventare dimensioni aggiuntive per il regressore insieme ad altri predittori. Questo approccio, a differenza dei metodi classici della serie temporale, ha il vantaggio di incorporare naturalmente più variabili contestuali e la relazione tra loro durante il training. Machine Learning Machine Learning apprende un singolo modello, ma spesso con rami internamente, per tutti gli elementi del set di dati e gli orizzonti di stima. Sono pertanto disponibili più dati per stimare i parametri del modello e la generalizzazione per la serie non visibile diventa possibile.

Per altre informazioni, vedere un esempio di [Machine Learning automatizzato per la previsione delle serie temporali](how-to-auto-train-forecast.md). In alternativa, vedere il [notebook della domanda di energia](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb) per esempi di codice dettagliati sulla configurazione di previsione avanzata, tra cui:

* rilevamento festività e conteggi
* convalida incrociata di origine in sequenza
* ritardi configurabili
* funzionalità di aggregazione della finestra in sequenza

## <a name="ensemble"></a>Modelli di ensemble

Machine Learning automatizzato supporta i modelli di Ensemble, che sono abilitati per impostazione predefinita. Ensemble Learning migliora i risultati di machine learning e le prestazioni predittive combinando più modelli anziché l'uso di singoli modelli. Le iterazioni insieme vengono visualizzate come le iterazioni finali dell'esecuzione. Il Machine Learning automatico usa i metodi Ensemble di voto e di stacking per combinare i modelli:

* **Voto**: stima in base alla media ponderata delle probabilità della classe stimata (per le attività di classificazione) o alle destinazioni di regressione stimate (per le attività di regressione).
* **Stacking**: lo stacking combina i modelli eterogeneo e addestra un metamodello in base all'output dei singoli modelli. I metadati predefiniti correnti sono LogisticRegression per le attività di classificazione e ElasticNet per le attività di regressione/previsione.

L' [algoritmo di selezione dell'ensemble di Caruana](http://www.niculescu-mizil.org/papers/shotgun.icml04.revised.rev2.pdf) con inizializzazione dell'insieme ordinato viene usato per decidere quali modelli usare nell'insieme. A un livello elevato, questo algoritmo Inizializza l'insieme con un massimo di 5 modelli con i punteggi individuali migliori e verifica che questi modelli siano entro il 5% di soglia del punteggio migliore per evitare un insieme iniziale scadente. Per ogni iterazione di ensemble viene quindi aggiunto un nuovo modello all'insieme esistente e il punteggio risultante viene calcolato. Se un nuovo modello ha migliorato il Punteggio di ensemble esistente, l'insieme viene aggiornato in modo da includere il nuovo modello.

Vedere le [procedure](how-to-configure-auto-train.md#ensemble) per modificare le impostazioni di ensemble predefinite in Machine Learning automatico.

## <a name="imbalance"></a>Dati sbilanciati

I dati sbilanciati sono comunemente presenti nei dati per gli scenari di classificazione di machine learning e si riferiscono a dati che contengono un rapporto sproporzionato delle osservazioni in ogni classe. Questo squilibrio può comportare un effetto positivo, erroneamente, dell'accuratezza di un modello, perché i dati di input hanno una distorsione verso una classe, il che comporta il training del modello per simulare tale distorsione. 

Come parte del suo obiettivo di semplificare il flusso di lavoro di Machine Learning, l'apprendimento automatico è integrato in funzionalità che consentono di gestire dati sbilanciati, ad esempio 

- Una **colonna di ponderazione**: la ml automatizzata supporta una colonna ponderata come input, causando la ponderazione delle righe nei dati, che possono rendere una classe più o meno "importante". Vedere questo [esempio di notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/sample-weight/auto-ml-sample-weight.ipynb) 

- Gli algoritmi usati da Machine Learning automatiche possono gestire in modo corretto squilibrio fino a 20:1, ovvero la classe più comune può avere più di 20 volte più righe nei dati rispetto alla classe meno comune.

### <a name="identify-models-with-imbalanced-data"></a>Identificare i modelli con dati sbilanciati

Poiché gli algoritmi di classificazione vengono in genere valutati in base all'accuratezza, il controllo del Punteggio di accuratezza di un modello è un modo efficace per identificare se è stato influenzato da dati sbilanciati. Ha avuto un'accuratezza molto elevata o un'accuratezza molto bassa per determinate classi?

Inoltre, le esecuzioni automatiche di ML generano automaticamente i grafici seguenti, che consentono di comprendere la correttezza delle classificazioni del modello e di identificare i modelli potenzialmente interessati da dati sbilanciati.

Grafico| Descrizione
---|---
[Matrice di confusione](how-to-understand-automated-ml.md#confusion-matrix)| Valuta le etichette classificate correttamente rispetto alle etichette effettive dei dati. 
[Precisione-richiamo](how-to-understand-automated-ml.md#precision-recall-chart)| Valuta il rapporto tra le etichette corrette e il rapporto tra le istanze di etichette trovate dei dati 
[Curve ROC](how-to-understand-automated-ml.md#roc)| Valuta il rapporto tra le etichette corrette e il rapporto tra le etichette false.

### <a name="handle-imbalanced-data"></a>Gestire i dati sbilanciati 

Le tecniche seguenti sono opzioni aggiuntive per la gestione di dati sbilanciati al di fuori del Machine Learning automatico. 

- Ripetizione del campionamento anche allo squilibrio della classe, eseguendo il campionamento delle classi più piccole o sottocampionando le classi più grandi. Questi metodi richiedono competenze per elaborare e analizzare.

- Usare una metrica delle prestazioni più adatta ai dati sbilanciati. Il Punteggio F1, ad esempio, è una media ponderata di precisione e richiamo. La precisione misura l'esattezza di un classificatore: la precisione bassa indica un numero elevato di falsi positivi--, mentre richiama misura la completezza di un classificatore. il richiamo minimo indica un numero elevato di falsi negativi. 

## <a name="use-with-onnx-in-c-apps"></a>Usare con ONNX nelle C# app

Con Azure Machine Learning, è possibile usare il Machine Learning automatico per compilare un modello Python e convertirlo nel formato ONNX. Il runtime di ONNX C#supporta, quindi è possibile usare il modello creato automaticamente nelle C# app senza la necessità di ricodificare o una delle latenze di rete introdotte dagli endpoint REST. Provare un esempio di questo flusso [nel notebook di Jupyter](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-with-onnx/auto-ml-classification-with-onnx.ipynb).

## <a name="automated-ml-across-microsoft"></a>Automatizzato ML in Microsoft

Il Machine Learning automatico è disponibile anche in altre soluzioni Microsoft, ad esempio:

|Integrazioni|Descrizione|
|------------|-----------|
|[ML.NET](https://docs.microsoft.com/dotnet/machine-learning/automl-overview)|Selezione e training automatici dei modelli nelle app .NET con Visual Studio e Visual Studio Code con ML.NET Automatic Machine Learning (anteprima).|
|[HDInsight](../../hdinsight/spark/apache-spark-run-machine-learning-automl.md)|Ridimensionare i processi di training di Machine Learning automatici in Spark nei cluster HDInsight in parallelo.|
|[Power BI](https://docs.microsoft.com/power-bi/service-machine-learning-automated)|Richiama i modelli di apprendimento automatico direttamente in Power BI (anteprima).|
|[SQL Server](https://cloudblogs.microsoft.com/sqlserver/2019/01/09/how-to-automate-machine-learning-on-sql-server-2019-big-data-clusters/)|Creare nuovi modelli di apprendimento automatico sui dati in 2019 SQL Server cluster Big Data.|

## <a name="next-steps"></a>Passaggi successivi

Vedere gli esempi e informazioni su come creare modelli usando Machine Learning automatico:

+ Seguire l' [esercitazione: Eseguire automaticamente il training di un modello di regressione con Machine Learning automatizzato di Azure](tutorial-auto-train-models.md)

+ Configurare le impostazioni per l'esperimento di training automatico:
  + In portale di Azure interfaccia o nella pagina di destinazione dell'area di lavoro (anteprima) [usare questi passaggi](how-to-create-portal-experiments.md).
  + Con Python SDK, [seguire questa procedura](how-to-configure-auto-train.md).

+ Per informazioni su come eseguire il training automatico usando i dati delle serie temporali, [seguire questa procedura](how-to-auto-train-forecast.md).

+ Prova [Jupyter notebook esempi per Machine Learning automatizzato](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/)
