---
title: 'Guida introduttiva: Pubblicare una knowledge base in REST, Python - QnA Maker'
titleSuffix: Azure Cognitive Services
description: Questa guida introduttiva di Python basata su REST illustra la procedura di pubblicazione della knowledge base per eseguire il push della versione più recente della knowledge base testata su un indice di Ricerca di Azure dedicato che rappresenta la knowledge base pubblicata. Viene inoltre creato un endpoint che può essere chiamato nell'applicazione o nel chat bot.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 09/03/2019
ms.author: diberry
ms.openlocfilehash: 97e28165702e352c7840f12a3214776dcc8642bc
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/04/2019
ms.locfileid: "70308093"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-python"></a>Avvio rapido: Pubblicare una knowledge base in QnA Maker usando Python

Questa guida introduttiva basata su REST illustra la procedura di pubblicazione della knowledge base (KB) a livello di codice. Con la pubblicazione viene eseguito il push dell'ultima versione della knowledge base in un indice dedicato di Ricerca di Azure e viene creato un endpoint che può essere chiamato nell'applicazione o nel chatbot.

In questa guida introduttiva viene chiamata l'API QnA Maker seguente:
* [Publish](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish) (Pubblicazione): con questa API non sono richieste informazioni nel corpo della richiesta.

## <a name="prerequisites"></a>Prerequisiti

* [Python 3.7](https://www.python.org/downloads/)
* È necessario disporre di un servizio QnA Maker. Per recuperare la chiave, selezionare Chiavi in Gestione risorse nel dashboard.
* ID della knowledge base (KB) QnA Maker trovato nell'URL nel parametro della stringa di query kbid come mostrato di seguito.

    ![ID della knowledge base di QnA Maker](../media/qnamaker-quickstart-kb/qna-maker-id.png)

    Se non si ha ancora una knowledge base, è possibile crearne una di esempio da usare per questa guida introduttiva: [Creare una nuova knowledge base](create-new-kb-nodejs.md).

> [!NOTE] 
> I file di soluzione completi sono disponibili nel [**repository GitHub**  Azure-Samples/cognitive-services-qnamaker-python](https://github.com/Azure-Samples/cognitive-services-qnamaker-python/tree/master/documentation-samples/quickstarts/publish-knowledge-base).

## <a name="create-a-knowledge-base-python-file"></a>Creare un file Python per la knowledge base

Creare un file denominato `publish-kb-3x.py`.

## <a name="add-the-required-dependencies"></a>Aggiungere le dipendenze richieste

All'inizio di `publish-kb-3x.py` inserire le righe seguenti per aggiungere le dipendenze necessarie al progetto:

[!code-python[Add the required dependencies](~/samples-qnamaker-python/documentation-samples/quickstarts/publish-knowledge-base/publish-kb-3x.py?range=1-1 "Add the required dependencies")]

## <a name="add-required-constants"></a>Aggiungere le costanti obbligatorie

Dopo le dipendenze obbligatorie precedenti, aggiungere le costanti obbligatorie per accedere a QnA Maker. Sostituire i valori con quelli personalizzati.

[!code-python[Add the required constants](~/samples-qnamaker-python/documentation-samples/quickstarts/publish-knowledge-base/publish-kb-3x.py?range=5-15 "Add the required constants")]

## <a name="add-post-request-to-publish-knowledge-base"></a>Aggiungere la richiesta POST per pubblicare la knowledge base

Dopo le costanti richieste, aggiungere il codice seguente, che effettua una richiesta HTTPS all'API di QnA Maker per pubblicare una knowledge base e riceve la risposta:

[!code-python[Add a POST request to publish knowledge base](~/samples-qnamaker-python/documentation-samples/quickstarts/publish-knowledge-base/publish-kb-3x.py?range=17-26 "Add a POST request to publish knowledge base")]

In caso di pubblicazione riuscita la chiamata API restituisce uno stato 204 senza alcun contenuto nel corpo della risposta. Il codice aggiunge il contenuto per le risposte con stato 204.

Per qualsiasi altra risposta, tale risposta viene restituita senza variazioni.

## <a name="build-and-run-the-program"></a>Compilare ed eseguire il programma

Per eseguire il programma, immettere il comando seguente a una riga di comando. Invierà la richiesta all'API di QnA Maker per pubblicare la knowledge base, quindi stamperà lo stato 204 con esito positivo o errori.

```bash
python publish-kb-3x.py
```

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Passaggi successivi

Dopo la pubblicazione della knowledge base, è necessario l'[URL dell'endpoint per generare una risposta](../Tutorials/create-publish-answer.md#generating-an-answer). 

> [!div class="nextstepaction"]
> [Informazioni di riferimento sull'API REST QnA Maker (V4)](https://go.microsoft.com/fwlink/?linkid=2092179)

[Panoramica di QnA Maker](../Overview/overview.md)
