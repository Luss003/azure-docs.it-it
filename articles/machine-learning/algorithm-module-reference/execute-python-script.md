---
title: 'Esegui script Python: informazioni di riferimento sui moduli'
titleSuffix: Azure Machine Learning
description: Informazioni su come usare il modulo Execute Python script (Esegui script Python) in Azure Machine Learning per eseguire il codice Python.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 03/10/2020
ms.openlocfilehash: 52eb3bdb463389d075421661610b5ee94d14d77d
ms.sourcegitcommit: b8d0d72dfe8e26eecc42e0f2dbff9a7dd69d3116
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/10/2020
ms.locfileid: "79037069"
---
# <a name="execute-python-script-module"></a>Eseguire il modulo di script Python

Questo articolo descrive un modulo in Azure Machine Learning Designer (anteprima).

Usare questo modulo per eseguire il codice Python. Per altre informazioni sull'architettura e i principi di progettazione di Python, vedere [l'articolo seguente](https://docs.microsoft.com/azure/machine-learning/machine-learning-execute-python-scripts).

Con Python è possibile eseguire attività che non sono attualmente supportate dai moduli esistenti, ad esempio:

+ Visualizzazione dei dati tramite `matplotlib`
+ Uso delle librerie Python per enumerare i set di impostazioni e i modelli nell'area di lavoro
+ Lettura, caricamento e manipolazione di dati da origini non supportate dal modulo [Import Data](./import-data.md)
+ Esegui il tuo codice di apprendimento approfondito 


Azure Machine Learning usa la distribuzione anaconda di Python, che include molte utilità comuni per l'elaborazione dei dati. La versione Anaconda verrà aggiornata automaticamente. La versione corrente è:
 -  Distribuzione di Anaconda 4.5 + per Python 3,6 

I pacchetti preinstallati sono:
-  asn1crypto==0.24.0
- attrs==19.1.0
- azure-common==1.1.18
- azure-storage-blob==1.5.0
- azure-storage-common==1.4.0
- certifi==2019.3.9
- cffi==1.12.2
- chardet==3.0.4
- crittografia = = 2.6.1
- distro==1.4.0
- IDNA = = 2.8
- jsonschema==3.0.1
- lightgbm==2.2.3
- more-itertools==6.0.0
- numpy==1.16.2
- pandas==0.24.2
- Pillow==6.0.0
- pip==19.0.3
- pyarrow==0.12.1
- pycparser = = 2.19
- pycryptodomex==3.7.3
- pyrsistent==0.14.11
- python-dateutil==2.8.0
- pytz==2018.9
- requests==2.21.0
- scikit-learn==0.20.3
- scipy==1.2.1
- setuptools==40.8.0
- six==1.12.0
- torch==1.0.1.post2
- torchvision==0.2.2.post3
- urllib3 = = 1.24.1
- Wheel = = 0.33.1 

 Per installare altri pacchetti non inclusi nell'elenco pre-installato, ad esempio *Scikit-misc*, aggiungere il codice seguente allo script: 

 ```python
import os
os.system(f"pip install scikit-misc")
```

## <a name="upload-files"></a>Caricare file
Lo **script Execute Python** supporta il caricamento di file con [Azure Machine Learning Python SDK](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py#upload-file-name--path-or-stream-).

L'esempio seguente illustra come caricare un file di immagine nel modulo **Execute Python script** :

```Python

# The script MUST contain a function named azureml_main
# which is the entry point for this module.

# imports up here can be used to
import pandas as pd

# The entry point function can contain up to two input arguments:
#   Param<dataframe1>: a pandas.DataFrame
#   Param<dataframe2>: a pandas.DataFrame
def azureml_main(dataframe1 = None, dataframe2 = None):

    # Execution logic goes here
    print(f'Input pandas.DataFrame #1: {dataframe1}')

    from matplotlib import pyplot as plt
    plt.plot([1, 2, 3, 4])
    plt.ylabel('some numbers')
    img_file = "line.png"
    plt.savefig(img_file)

    from azureml.core import Run
    run = Run.get_context(allow_offline=True)
    run.upload_file(f"graphics/{img_file}", img_file)

    # Return value must be of a sequence of pandas.DataFrame
    # E.g.
    #   -  Single return value: return dataframe1,
    #   -  Two return values: return dataframe1, dataframe2
    return dataframe1,
}
```

Dopo che la pipeline è stata inviata correttamente, è possibile visualizzare l'anteprima dell'immagine nel pannello di destra del modulo ![immagine caricata](media/module/upload-image-in-python-script.png)

## <a name="how-to-configure-execute-python-script"></a>Come configurare Execute Python script

Il modulo **Execute Python script** contiene codice Python di esempio che è possibile usare come punto di partenza. Per configurare il modulo **Execute Python script (Esegui script Python** ), fornire un set di input e codice Python da eseguire nella casella di testo **script Python** .

1. Aggiungere il modulo **Execute Python script (Esegui script Python** ) alla pipeline.

2. Aggiungere e connettersi a **DataSet1** tutti i set di dati della finestra di progettazione che si desidera utilizzare per l'input. Fare riferimento a questo set di dati nello script Python come **DataFrame1**.

    L'uso di un set di dati è facoltativo se si vuole generare dati con Python o usare il codice Python per importare i dati direttamente nel modulo.

    Questo modulo supporta l'aggiunta di un secondo set di dati in **Dataset2**. Fare riferimento al secondo set di dati nello script Python come DataFrame2.

    I set di dati archiviati in Azure Machine Learning vengono convertiti automaticamente in **Pandas** data. frames quando vengono caricati con questo modulo.

    ![Esegui mappa di input Python](media/module/python-module.png)

4. Per includere nuovi pacchetti o codice Python, aggiungere il file compresso contenente queste risorse personalizzate nel **bundle di script**. L'input per il **bundle di script** deve essere un file compresso caricato nell'area di lavoro come set di dati di tipo file. 

    Qualsiasi file contenuto nell'archivio compresso caricato può essere usato durante l'esecuzione della pipeline. Se l'archivio include una struttura di directory, la struttura viene mantenuta, ma è necessario anteporre una directory denominata **src** al percorso.

5. Nella casella di testo **script Python** Digitare o incollare uno script Python valido.

    La casella di testo **script Python** è già popolata con alcune istruzioni nei commenti e il codice di esempio per l'accesso ai dati e l'output. È necessario modificare o sostituire questo codice. Assicurarsi di seguire le convenzioni Python relative a rientri e maiuscole e minuscole.

    + Lo script deve contenere una funzione denominata `azureml_main` come punto di ingresso per questo modulo.
    + La funzione del punto di ingresso può contenere un massimo di due argomenti di input: `Param<dataframe1>` e `Param<dataframe2>`
    + I file compressi connessi alla terza porta di input vengono decompressi e archiviati nella directory, `.\Script Bundle`, che viene aggiunta anche al `sys.path`di Python. 

    Se quindi il file zip contiene `mymodule.py`, importarlo usando `import mymodule`.

    + Due set di impostazioni possono essere restituiti alla finestra di progettazione, che deve essere una sequenza di tipo `pandas.DataFrame`. È possibile creare altri output nel codice Python e scriverli direttamente in archiviazione di Azure.

6. Eseguire la pipeline oppure selezionare il modulo e fare clic su **Esegui selezione** per eseguire solo lo script Python.

    Tutti i dati e il codice vengono caricati in una macchina virtuale ed eseguiti usando l'ambiente Python specificato.

## <a name="results"></a>Risultati

I risultati di tutti i calcoli eseguiti dal codice Python incorporato devono essere forniti come Pandas. Dataframe, che viene automaticamente convertito nel formato Azure Machine Learning set di dati, in modo che sia possibile usare i risultati con altri moduli della pipeline.

Il modulo restituisce due set di impostazioni:  
  
+ **Set di dati results 1**, definito dal primo dataframe Pandas restituito nello script Python

+ **Set di dati di risultati 2**, definito dal secondo dataframe Pandas restituito nello script Python


## <a name="next-steps"></a>Passaggi successivi

Vedere il [set di moduli disponibili](module-reference.md) per Azure Machine Learning. 