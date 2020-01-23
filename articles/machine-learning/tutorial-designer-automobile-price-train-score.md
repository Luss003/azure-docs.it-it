---
title: "Esercitazione: Stimare il prezzo di un'automobile con la finestra di progettazione"
titleSuffix: Azure Machine Learning
description: Informazioni su come eseguire il training, assegnare punteggi e distribuire un modello di Machine Learning usando un'interfaccia basata su trascinamento della selezione. Questa esercitazione è la prima parte di una serie in due parti su come stimare i prezzi delle automobili con la regressione lineare.
author: peterclu
ms.author: peterlu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: 917ded03892f3a8a5812948bcbfe31f029fc5cf8
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2020
ms.locfileid: "76314981"
---
# <a name="tutorial-predict-automobile-price-with-the-designer"></a>Esercitazione: Stimare il prezzo di un'automobile con la finestra di progettazione
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

In questa esercitazione in due parti si apprenderà come usare la finestra di progettazione di Azure Machine Learning per sviluppare e distribuire una soluzione di analisi predittiva che stima il prezzo di qualsiasi automobile. 

Nella prima parte verrà configurato l'ambiente, quindi i moduli verranno trascinati in un canvas interattivo e collegati tra loro per creare una pipeline di Azure Machine Learning.

Nella prima parte dell'esercitazione si è apprenderà come:

> [!div class="checklist"]
> * Creare una nuova pipeline.
> * Importare i dati.
> * Preparare i dati.
> * Eseguire il training di un modello di Machine Learning.
> * Valutare un modello di Machine Learning.

Nella [seconda parte](tutorial-designer-automobile-price-deploy.md) dell'esercitazione si apprenderà come distribuire il modello predittivo come endpoint di inferenza in tempo reale per stimare il prezzo di qualsiasi automobile in base alle specifiche tecniche inviate. 

> [!NOTE]
>Una versione completa dell'esercitazione è disponibile come pipeline di esempio.
>
>Per trovarla, passare alla finestra di progettazione nell'area di lavoro. Nella sezione **New pipeline** (Nuova pipeline) selezionare **Sample 1 - Regression: Automobile Price Prediction(Basic)** .

## <a name="create-a-new-pipeline"></a>Creare una nuova pipeline

Le pipeline di Azure Machine Learning organizzano più passaggi dipendenti di apprendimento automatico ed elaborazione dati in un'unica risorsa. Le pipeline consentono di organizzare, gestire e riutilizzare flussi di lavoro di machine learning complessi in più progetti e utenti. Per creare una pipeline di Azure Machine Learning, è necessaria un'area di lavoro di Azure Machine Learning. In questa sezione viene descritto come creare entrambe queste risorse.

### <a name="create-a-new-workspace"></a>Creazione di una nuova area di lavoro

Se è già disponibile un'area di lavoro di Azure Machine Learning con un'edizione Enterprise, [passare alla sezione successiva](#create-the-pipeline).

[!INCLUDE [aml-create-portal](../../includes/aml-create-in-portal-enterprise.md)]

### <a name="create-the-pipeline"></a>Creare la pipeline

1. Accedere a [ml.azure.com](https://ml.azure.com) e selezionare l'area di lavoro che si vuole usare.

1. Selezionare **Designer** (Finestra di progettazione).

    ![Screenshot dell'area di lavoro visiva che mostra come accedere alla finestra di progettazione](./media/tutorial-designer-automobile-price-train-score/launch-designer.png)

1. Selezionare **Easy-to-use prebuilt modules** (Moduli predefiniti facili da usare).

1. Selezionare il nome predefinito della pipeline, **Pipeline-Created-on** nella parte superiore del canvas, e sostituirlo con un nome significativo, ad esempio *Automobile price prediction*. Il nome non deve essere univoco.

## <a name="import-data"></a>Importa dati

Nella finestra di progettazione sono disponibili diversi set di dati di esempio con cui sperimentare. Per questa esercitazione, usare **Automobile price data (Raw)** . 

1. A sinistra del canvas della pipeline è presente un pannello di set di dati e moduli. Selezionare **Datasets** (Set di dati), quindi nella sezione **Samples** (Esempi) visualizzare i set di dati di esempio disponibili.

1. Selezionare il set di dati **Automobile price data (Raw)** e trascinarlo nel canvas.

   ![Trascinare i dati nell'area di disegno](./media/tutorial-designer-automobile-price-train-score/drag-data.gif)

### <a name="visualize-the-data"></a>Visualizzare i dati

È possibile visualizzare i dati per comprendere il set di dati che verrà usato.

1. Selezionare il modulo **Automobile price data (Raw)** .

1. Nel riquadro delle proprietà a destra del canvas selezionare **Outputs**.

1. Selezionare l'icona del grafico per visualizzare i dati.

    ![Visualizzare i dati](./media/tutorial-designer-automobile-price-train-score/visualize-data.png)

1. Selezionare le diverse colonne nella finestra dei dati per visualizzare le informazioni relative a ciascuna.

    Ogni riga rappresenta un'automobile e le variabili associate a ogni automobile sono rappresentate da colonne. In questo set di dati sono presenti 205 righe e 26 colonne.

## <a name="prepare-data"></a>Preparazione dei dati

I set di dati in genere richiedono una pre-elaborazione prima dell'analisi. Durante l'ispezione del set di dati si potrebbe aver notato che mancano alcuni valori. Per consentire al modello di analizzare correttamente i dati, è necessario eseguire la pulizia di questi valori mancanti.

### <a name="remove-a-column"></a>Rimuovere una colonna

Quando si esegue il training di un modello, occorre fare qualcosa in merito ai dati mancanti. In questo set di dati, la colonna **normalized-losses** ha molti valori mancanti, pertanto verrà esclusa completamente dal modello.

1. Immettere **Select** nella casella di ricerca nella parte superiore del pannello per trovare il modulo **Select Columns in Dataset** (Seleziona colonne nel set di dati).

1. Trascinare il modulo **Select Columns in Dataset** (Seleziona colonne nel set di dati) nel canvas. Rilasciare il modulo sotto il modulo del set di dati.

1. Connettere il set di dati **Automobile price data (Raw)** al modulo **Select Columns in Dataset** (Seleziona colonne nel set di dati). Trascinare il mouse dalla porta di output del set di dati, ovvero il piccolo cerchio nella parte inferiore del set di dati nel canvas, fino alla porta di input di **Select Columns in Dataset** (Seleziona colonne nel set di dati), ovvero il piccolo cerchio nella parte superiore del modulo.

    > [!TIP]
    > Viene creato un flusso di dati attraverso la pipeline quando si connette la porta di output di un modulo alla porta di input di un altro.
    >

    ![Connettere i moduli](./media/tutorial-designer-automobile-price-train-score/connect-modules.gif)

1. Selezionare il modulo **Select Columns in Dataset** (Seleziona colonne nel set di dati).

1. Nel riquadro delle proprietà a destra del canvas selezionare **Parameters** > **Edit column** (Parametri > Modifica colonne).

1. Selezionare il segno **+** per aggiungere una nuova regola.

1. Nel menu a discesa selezionare **Exclude** (Escludi) e **Column names** (Nomi di colonna).
    
1. Immettere *normalized-losses* nella casella di testo.

1. Nell'angolo in basso a destra selezionare **Save** (Salva) per chiudere il selettore di colonne.

    ![Escludere una colonna](./media/tutorial-designer-automobile-price-train-score/exclude-column.png)
        
    Il riquadro delle proprietà mostra che la colonna **normalized-losses** è stata esclusa.

1. Selezionare il modulo **Select Columns in Dataset** (Seleziona colonne nel set di dati). 

1. Nel riquadro delle proprietà selezionare **Parameters** > **Comment** (Parametri > Commento) e immettere *Exclude normalized losses* (Escludi perdite normalizzate).

### <a name="clean-missing-data"></a>Pulire i dati mancanti

Dopo la rimozione della colonna **normalized-losses**, il set di dati contiene ancora colonne con valori mancanti. È possibile rimuovere i rimanenti dati mancanti usando il modulo **Clean Missing Data** (Pulisci dati mancanti).

> [!TIP]
> La pulizia dei valori mancanti dai dati di input è un prerequisito per l'uso della maggior parte dei moduli nella finestra di progettazione.

1. Immettere **Clean** nella casella di ricerca per trovare il modulo **Clean Missing Data** (Pulisci dati mancanti).

1. Trascinare il modulo **Clean Missing Data** (Pulisci dati mancanti) nel canvas della pipeline. Connetterlo al modulo **Select Columns in Dataset** (Seleziona colonne nel set di dati). 

1. Nel riquadro delle proprietà selezionare **Remove entire row** (Rimuovi riga intera) in **Cleaning mode** (Modalità pulizia).

1. Nella casella **Comment** (Commento) del riquadro delle proprietà immettere *Remove missing value rows* (Rimuovi righe con valori mancanti). 

    La pipeline avrà ora un aspetto analogo al seguente:
    
    ![Select-column](./media/tutorial-designer-automobile-price-train-score/pipeline-clean.png)

## <a name="train-a-machine-learning-model"></a>Eseguire il training di un modello di Machine Learning

Una volta elaborati i dati, è possibile eseguire il training di un modello predittivo.

### <a name="select-an-algorithm"></a>Selezionare un algoritmo

*Classificazione* e *regressione* sono due tipi di algoritmi di Machine Learning sottoposti a supervisione. La classificazione stima una risposta da un set di categorie definito, ad esempio un colore come rosso, blu o verde. La regressione viene usata per stimare un numero.

Poiché si vuole stimare il prezzo, ovvero un numero, è possibile usare un algoritmo di regressione. Per questo esempio si userà un modello di regressione lineare.

### <a name="split-the-data"></a>Dividere i dati

Dividere i dati in due set di dati distinti per il training e i test del modello.

1. Immettere **split data** nella casella di ricerca per trovare il modulo **Split data** (Dividi i dati). Connetterlo alla porta sinistra del modulo **Clean Missing Data** (Pulisci dati mancanti).

1. Selezionare il modulo **Split Data**.

1. Nel riquadro delle proprietà impostare **Fraction of rows in the first output dataset** (Frazione di righe nel primo set di dati di output) su 0,7.

    Con questa opzione, per il training del modello verrà usato il 70% dei dati, mentre il restante 30% verrà usato per i test.

1. Nella casella **Comment** (Commento) del riquadro delle proprietà immettere *Split the dataset into training set(0.7) and test set(0.3)* (Suddividi il set di dati in set per il training (0,7) e set per i test (0,3)).

### <a name="train-the-model"></a>Eseguire il training del modello

Eseguire il training del modello assegnando un set di dati che include il prezzo. Il modello analizza i dati e cerca le correlazioni tra le caratteristiche di un'automobile e il relativo prezzo per creare un modello.

1. Per selezionare l'algoritmo di apprendimento, cancellare la casella di ricerca della tavolozza dei moduli.

1. Espandere **Machine Learning Algorithms** (Algoritmi di Machine Learning).
    
    Questa opzione visualizza diverse categorie di moduli che è possibile usare per inizializzare gli algoritmi di Machine Learning.

1. Selezionare il modulo **Regression** > **Linear Regression** (Regressione > Regressione lineare) e trascinarlo nel canvas della pipeline.

1. Trovare e trascinare il modulo **Train Model** (Training modello) nel canvas della pipeline. 

1. Connettere l'output del modulo **Linear Regression** (Regressione lineare) alla porta di input sinistra del modulo **Train Model** (Training modello).

1. Connettere l'output dei dati di training (porta sinistra) del modulo **Split Data** (Divisione dati) alla porta di input destra del modulo **Train Model** (Training modello).

    ![Screenshot della configurazione corretta del modulo Train Model Il modulo Linear Regression si connette alla porta sinistra e il modulo Split Data alla porta destra del modulo Train Model](./media/tutorial-designer-automobile-price-train-score/pipeline-train-model.png)

1. Selezionare il modulo **Train Model**.

1. Nel riquadro delle proprietà selezionare il selettore **Edit column** (Modifica colonna).

1. Nella finestra di dialogo **Label column** (Colonna etichetta) espandere il menu a discesa e selezionare **Column names** (Nomi di colonna). 

1. Nella casella di testo immettere *price* (prezzo). Il prezzo è il valore che si intende stimare con il modello.

    La pipeline dovrebbe avere un aspetto simile al seguente:

    ![Screenshot della configurazione corretta della pipeline dopo l'aggiunta del modulo Train Model.](./media/tutorial-designer-automobile-price-train-score/pipeline-train-graph.png)

## <a name="evaluate-a-machine-learning-model"></a>Valutare un modello di Machine Learning

Dopo aver eseguito il training del modello usando il 70% dei dati, è possibile usarlo per assegnare punteggi al restante 30% e verificarne il funzionamento.

1. Immettere *score model* nella casella di ricerca per trovare il modulo **Score Model** (Punteggio modello) e trascinarlo nel canvas della pipeline. 

1. Connettere l'output del modulo **Train Model** alla porta di input sinistra del modulo **Score Model**. Connettere l'output dei dati di test (porta destra) del modulo **Split Data** alla porta di input destra di **Score Model**.

1. Immettere *evaluate* nella casella di ricerca per trovare il modulo **Evaluate Model** (Valutazione modello) e trascinarlo nel canvas della pipeline. 

1. Connettere l'output del modulo **Score Model** all'input sinistro di **Evaluate Model**. 

    La pipeline finale avrà un aspetto analogo al seguente:

    ![Screenshot della configurazione corretta della pipeline.](./media/tutorial-designer-automobile-price-train-score/pipeline-final-graph.png)

### <a name="run-the-pipeline"></a>Eseguire la pipeline

[!INCLUDE [aml-ui-create-training-compute](../../includes/aml-ui-create-training-compute.md)]

### <a name="view-results"></a>Visualizzazione dei risultati

Al termine dell'esecuzione, è possibile visualizzare i risultati dell'esecuzione della pipeline. 

1. Selezionare il modulo **Score Model** (Punteggio modello) per visualizzare il relativo output.

1. Nel riquadro delle proprietà selezionare **Outputs** > **Visualize** (Output > Visualizza).

    Qui è possibile visualizzare i prezzi stimati e i prezzi effettivi dai dati di test.

    ![Screenshot della visualizzazione di output con la colonna Scored Label evidenziata](./media/tutorial-designer-automobile-price-train-score/score-result.png)

1. Selezionare il modulo **Evaluate Model** (Valutazione modello) per visualizzare il relativo output.

1. Nel riquadro delle proprietà selezionare **Output** > **Visualize** (Output > Visualizza).

Per il modello vengono visualizzate le seguenti statistiche:

* **Errore assoluto medio** (MAE): la media degli errori assoluti. Un errore è la differenza tra il valore stimato e quello effettivo.
* **Radice dell'errore quadratico medio** (RMSE): Radice quadrata della media degli errori quadratici delle stime effettuate sul set di dati di test.
* **Errore assoluto relativo**: Media degli errori assoluti relativamente alla differenza assoluta tra i valori effettivi e la media di tutti i valori effettivi.
* **Errore quadratico relativo**: Media degli errori quadratici relativamente alla differenza quadratica tra i valori effettivi e la media di tutti i valori effettivi.
* **Coefficiente di determinazione**: noto anche come valore quadratico R, è una metrica statistica che indica l'esattezza del modello rispetto ai dati.

Per ogni statistica di errore, sono preferibili i valori più piccoli. Un valore più piccolo indica che le stime sono più vicine ai valori effettivi. Per il coefficiente di determinazione, più il valore si avvicina a uno (1,0) più le stime sono precise.

## <a name="clean-up-resources"></a>Pulire le risorse

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Passaggi successivi

Nella prima parte dell'esercitazione sono state completate queste attività:

* Creare una pipeline
* Preparare i dati
* Eseguire il training del modello
* Assegnare punteggi e valutare il modello

Nella seconda parte si apprenderà come distribuire il modello come endpoint in tempo reale.

> [!div class="nextstepaction"]
> [Continuare a distribuire modelli](tutorial-designer-automobile-price-deploy.md)
