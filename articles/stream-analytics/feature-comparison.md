---
title: Confronto delle funzionalità di analisi di flusso di Azure
description: Questo articolo mette a confronto le funzionalità supportate per i processi cloud e IoT Edge di analisi di flusso di Azure in portale di Azure, Visual Studio e Visual Studio Code.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: 8cdba27ac949584e1fa96e3f7b0874f4fc0d4212
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75443649"
---
# <a name="azure-stream-analytics-feature-comparison"></a>Confronto delle funzionalità di analisi di flusso di Azure

Con analisi di flusso di Azure è possibile creare soluzioni di streaming nel cloud e in IoT Edge usando [portale di Azure](stream-analytics-quick-create-portal.md), [Visual Studio](stream-analytics-quick-create-vs.md)e [Visual Studio Code](quick-create-vs-code.md). Nelle tabelle di questo articolo vengono illustrate le funzionalità supportate da ogni piattaforma per entrambi i tipi di processo.

## <a name="cloud-job-features"></a>Funzionalità del processo cloud


|Funzionalità  |Portale  |Visual Studio  |Visual Studio Code  |
|---------|---------|---------|---------|
|Multipiattaforma     |Mac</br>Linux</br>Windows         |Windows        |Mac</br>Linux</br>Windows          |
|Creazione di script     |Sì         |Sì         |Sì         |
|Script IntelliSense     |Evidenziazione della sintassi         |Evidenziazione della sintassi</br>Completamento del codice</br>Marcatore errore         |Evidenziazione della sintassi</br>Completamento del codice</br>Marcatore errore         |
|Definire tutti i tipi di input, output e configurazioni di processo     |Sì         |Sì         |Sì         |
|Controllo del codice sorgente     |No         |Sì         |Sì         |
|Supporto CI/CD     |Parziale         |Sì         |Sì         |
|Condividere input e output tra più query     |No         |Sì         |Sì         |
|Test di query con un file di esempio     |Sì         |Sì        |Sì         |
|Test locali dei dati in tempo reale     |No         |Sì       |Sì      |
|Elencare i processi e visualizzare le entità del processo     |Sì         |Sì        |Sì         |
|Esportare un processo in un progetto locale     |No         |Sì         |Sì         |
|Inviare, avviare e arrestare processi     |Sì         |Sì         |Sì         |
|Visualizzare le metriche e il diagramma del processo     |Sì         |Sì         |Apri nel portale         |
|Visualizzare gli errori di runtime del processo     |Sì         |Sì         |No         |
|Log di diagnostica     |Sì         |No         |No         |
|Proprietà personalizzate del messaggio     |Sì         |Sì         |No       |
|C#funzione e deserializzatore del codice personalizzato|Modalità di sola lettura|Sì|No|
|UDF e UDA JavaScript     |Sì         |Sì         |Solo Windows         |
|Servizio Machine Learning     |Sì, ma non è possibile testare la query        |Sì         |No         |
|Machine Learning Studio     |Sì, ma non è possibile testare la query        |Sì |No         |
|Livello di compatibilità     |1.0</br>1.1</br>1,2 (impostazione predefinita)         |1.0</br>1.1</br>1,2 (impostazione predefinita)           |1.0</br>1.1</br>1,2 (impostazione predefinita)           |
|Funzioni di rilevamento di anomalie basate su ML predefinite     |Sì         |Sì         |Sì         |
|Funzioni geospaziali predefinite     |Sì         |Sì         |Sì         |



## <a name="iot-edge-job-features"></a>Funzionalità del processo di IoT Edge

|Funzionalità  |Portale  |Visual Studio  |Visual Studio Code  |
|---------|---------|---------|---------|
|Creazione di processi     |Sì         |Sì         |No         |
|Controllo del codice sorgente     |No         |Sì         |No         |
|Esportare un processo in un progetto locale     |No         |Sì         |No         |
|Test di query con un file di esempio     |Sì         |Sì         |No         |
|Condividere input e output tra più query     |No         |Sì         |No         |
|C#UDF     |No         |Sì         |No         |
|Inviare i processi     |Sì         |Sì         |No         |
|Elencare i processi e visualizzare le entità del processo     |Sì         |Sì         |No         |
|Visualizzare le metriche e il diagramma del processo     |Sì         |Parziale         |No         |
|Visualizzare gli errori di runtime del processo     |Sì         |Parziale         |No         |
|Supporto CI/CD     |No         |No         |No         |


## <a name="next-steps"></a>Passaggi successivi

* [Analisi di flusso di Azure in IoT Edge](stream-analytics-edge.md)
* [Esercitazione: scrivere una C# funzione definita dall'utente per l'analisi di flusso di Azure IOT Edge processo (anteprima)](stream-analytics-edge-csharp-udf.md)
* [Sviluppare analisi di flusso IoT Edge processi usando gli strumenti di Visual Studio](stream-analytics-tools-for-visual-studio-edge-jobs.md)
* [Usare Visual Studio per visualizzare i processi di Analisi di flusso di Azure](stream-analytics-vs-tools.md)
* [Esplora analisi di flusso di Azure con Visual Studio Code (anteprima)](visual-studio-code-explore-jobs.md)


