---
title: "Guida introduttiva: Creare un progetto di classificazione immagini con l'SDK di Visione personalizzata per Java"
titleSuffix: Azure Cognitive Services
description: Creare un progetto, aggiungere i tag, caricare le immagini, eseguire il training del progetto ed effettuare una stima usando Java SDK.
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 08/08/2019
ms.author: areddish
ms.openlocfilehash: 2f80ba319db6d6d7ce512aba8191584ea5bba86b
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/25/2019
ms.locfileid: "73519456"
---
# <a name="quickstart-create-an-image-classification-project-with-the-custom-vision-sdk-for-java"></a>Guida introduttiva: Creare un progetto di classificazione immagini con l'SDK di Visione personalizzata per Java

Questo articolo fornisce informazioni e codice di esempio utili per iniziare a usare Custom Vision SDK per Java per compilare un modello di classificazione delle immagini. Dopo la creazione, è possibile aggiungere tag, caricare immagini, eseguire il training del progetto, ottenere l'URL dell'endpoint predefinito per la stima del progetto e usare l'endpoint per un test a livello di codice dell'immagine. Usare questo esempio come modello per la creazione di un'applicazione Java personalizzata. Se si preferisce eseguire la procedura di compilazione e utilizzo di un modello di classificazione _senza_ codice, vedere le [indicazioni basate su browser](getting-started-build-a-classifier.md).

## <a name="prerequisites"></a>Prerequisiti

- Un ambiente di sviluppo Java a scelta
- [JDK 7 o 8](https://aka.ms/azure-jdks) installato.
- Maven installato
- [!INCLUDE [create-resources](includes/create-resources.md)]

## <a name="get-the-custom-vision-sdk-and-sample-code"></a>Ottenere Custom Vision SDK e il codice di esempio

Per scrivere un'app Java che usa Visione personalizzata saranno necessari i pacchetti maven di Visione personalizzata. Questi pacchetti sono inclusi nel progetto di esempio che si scaricherà, ma è possibile accedervi singolarmente qui.

È possibile installare Custom Vision SDK dal repository centrale Maven:

- [Training SDK](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-training)
- [Prediction SDK](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-prediction)

Clonare o scaricare il progetto [Cognitive Services Java SDK Samples](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master). Passare alla cartella **Vision/CustomVision/** .

Questo progetto Java crea un nuovo progetto di classificazione immagini di Visione personalizzata denominato __Sample Java Project__, accessibile tramite il [sito Web di Visione personalizzata](https://customvision.ai/). Quindi carica le immagini per eseguire il training e testare un classificatore. In questo progetto il classificatore ha lo scopo di determinare se un albero è un __abete canadese__ o un __ciliegio giapponese__.

[!INCLUDE [get-keys](includes/get-keys.md)]

Il programma è configurato in modo da archiviare i dati chiave come variabili di ambiente. Impostare queste variabili passando alla cartella **Vision/CustomVision** in PowerShell. Quindi immettere i comandi:

```powershell
$env:AZURE_CUSTOMVISION_TRAINING_API_KEY ="<your training api key>"
$env:AZURE_CUSTOMVISION_PREDICTION_API_KEY ="<your prediction api key>"
```

## <a name="understand-the-code"></a>Informazioni sul codice

Caricare il progetto `Vision/CustomVision` nell'ambiente IDE Java e aprire il file _CustomVisionSamples.java_. Trovare il metodo **runSample** e impostare come commento la chiamata al metodo **ObjectDetection_Sample**. In questo modo viene eseguito lo scenario di rilevamento oggetti, che non è illustrato in questa guida. Il metodo **ImageClassification_Sample** implementa la funzionalità principale di questo esempio. Passare alla sua definizione ed esaminare il codice.

### <a name="create-a-custom-vision-service-project"></a>Creare un progetto di Servizio visione artificiale personalizzato

Il primo frammento di codice crea un progetto di classificazione immagini. Il progetto creato verrà visualizzato nel [sito Web di Visione personalizzata](https://customvision.ai/) visitato in precedenza. Per specificare altre opzioni quando si crea il progetto, come illustrato in [Creare un classificatore](getting-started-build-a-classifier.md) nella guida al portale Web, vedere gli overload del metodo [CreateProject](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.customvision.training.trainings.createproject?view=azure-java-stable#com_microsoft_azure_cognitiveservices_vision_customvision_training_Trainings_createProject_String_CreateProjectOptionalParameter_).

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_create)]

### <a name="create-tags-in-the-project"></a>Creare tag nel progetto

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_tags)]

### <a name="upload-and-tag-images"></a>Caricare e contrassegnare le immagini

Le immagini di esempio sono incluse nella cartella **src/main/resources** del progetto. Vengono lette da quella posizione e caricate nel servizio con i tag appropriati.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_upload)]

Il frammento di codice precedente usa due funzioni helper che recuperano le immagini come flussi di risorse e le caricano nel servizio. È possibile caricare fino a 64 immagini in un singolo batch.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_helpers)]

### <a name="train-the-classifier-and-publish"></a>Training del classificatore e pubblicazione

Questo codice crea la prima iterazione del progetto e quindi la pubblica nell'endpoint di stima. Il nome assegnato all'iterazione pubblicata può essere usato per inviare le richieste di stima. L'iterazione è disponibile nell'endpoint di stima solo dopo che è stata pubblicata.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_train)]

### <a name="use-the-prediction-endpoint"></a>Usare l'endpoint di stima

L'endpoint di stima, qui rappresentato dall'oggetto `predictor`, è il riferimento usato per inviare un'immagine al modello corrente e ottenere una stima di classificazione. In questo esempio `predictor` è definito altrove tramite la variabile di ambiente della chiave di stima.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_predict)]

## <a name="run-the-application"></a>Eseguire l'applicazione

Per compilare ed eseguire la soluzione tramite maven, eseguire il comando seguente nella directory del progetto in PowerShell:

```powershell
mvn compile exec:java
```

L'output della console dell'applicazione dovrebbe essere simile al testo seguente:

```console
Creating project...
Adding images...
Adding image: hemlock_1.jpg
Adding image: hemlock_2.jpg
Adding image: hemlock_3.jpg
Adding image: hemlock_4.jpg
Adding image: hemlock_5.jpg
Adding image: hemlock_6.jpg
Adding image: hemlock_7.jpg
Adding image: hemlock_8.jpg
Adding image: hemlock_9.jpg
Adding image: hemlock_10.jpg
Adding image: japanese_cherry_1.jpg
Adding image: japanese_cherry_2.jpg
Adding image: japanese_cherry_3.jpg
Adding image: japanese_cherry_4.jpg
Adding image: japanese_cherry_5.jpg
Adding image: japanese_cherry_6.jpg
Adding image: japanese_cherry_7.jpg
Adding image: japanese_cherry_8.jpg
Adding image: japanese_cherry_9.jpg
Adding image: japanese_cherry_10.jpg
Training...
Training status: Training
Training status: Training
Training status: Completed
Done!
        Hemlock: 93.53%
        Japanese Cherry: 0.01%
```

È quindi possibile verificare che la stima dell'immagine di test (le ultime righe dell'output) sia corretta.

[!INCLUDE [clean-ic-project](includes/clean-ic-project.md)]

## <a name="next-steps"></a>Passaggi successivi

In questa guida si è appreso come eseguire ogni passaggio del processo di classificazione delle immagini nel codice. Questo esempio esegue una sola iterazione del training, ma spesso è necessario eseguire il training e il test del modello più volte per ottenere una maggiore precisione.

> [!div class="nextstepaction"]
> [Testare un modello e ripeterne il training](test-your-model.md)
