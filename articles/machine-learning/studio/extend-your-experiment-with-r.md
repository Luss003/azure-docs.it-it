---
title: Estendere l'esperimento con R
titleSuffix: ML Studio (classic) - Azure
description: Come estendere le funzionalità di Azure Machine Learning Studio (classico) tramite il linguaggio R usando il modulo Execute R script.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/20/2017
ms.openlocfilehash: 7b4b869695eb2073121a889cd81d99c4fc06d4b9
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2020
ms.locfileid: "79218041"
---
# <a name="azure-machine-learning-studio-classic-extend-your-experiment-with-r"></a>Azure Machine Learning Studio (classico): estendere l'esperimento con R 

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]
È possibile estendere le funzionalità di Azure Machine Learning Studio (classico) tramite il linguaggio R usando il modulo [Execute R script][execute-r-script] .

Questo modulo accetta più set di dati di input e produce un singolo set di dati come output. È possibile digitare uno script R nel parametro **r script** del modulo [Execute R script][execute-r-script] .

Per accedere a ogni porta di input del modulo, usare codice simile al seguente:

    dataset1 <- maml.mapInputPort(1)

## <a name="listing-all-currently-installed-packages"></a>Elenco di tutti i pacchetti installati attualmente
L'elenco dei pacchetti installati può cambiare. È possibile trovare un elenco dei pacchetti attualmente installati nei [pacchetti R supportati da Azure Machine Learning Studio (versione classica)](https://msdn.microsoft.com/library/azure/mt741980.aspx).

È anche possibile ottenere l'elenco completo e aggiornato dei pacchetti installati immettendo il codice seguente nel modulo [Execute R script][execute-r-script] :

    out <- data.frame(installed.packages(,,,fields="Description"))
    maml.mapOutputPort("out")

In questo modo l'elenco dei pacchetti viene inviato alla porta di output del modulo [Execute R script][execute-r-script] .
Per visualizzare l'elenco di pacchetti, connettere un modulo di conversione, ad esempio [Converti in CSV][convert-to-csv] , nell'output sinistro del modulo [Execute R script][execute-r-script] , eseguire l'esperimento, quindi fare clic sull'output del modulo di conversione e selezionare **download**. 

![Scaricare l'output del modulo "Convert to CSV" (Converti in CSV)](./media/extend-your-experiment-with-r/download-package-list.png)


<!--
For convenience, here is the [current full list with version numbers in Excel format](https://az754797.vo.msecnd.net/docs/RPackages.xlsx).
-->

## <a name="importing-packages"></a>Importazione di pacchetti
È possibile importare pacchetti non ancora installati usando i comandi seguenti nel modulo [Execute R script][execute-r-script] :

    install.packages("src/my_favorite_package.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("my_favorite_package", lib.loc = ".", logical.return = TRUE, verbose = TRUE)

Il file `my_favorite_package.zip` contiene il pacchetto.




<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
