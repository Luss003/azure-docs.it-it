---
title: 'Guida introduttiva: Libreria client di Personalizza esperienze per .NET | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: Imparare a usare la libreria client di Personalizza esperienze per .NET con un ciclo di apprendimento.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: quickstart
ms.date: 10/24/2019
ms.author: diberry
ms.openlocfilehash: 411bd82ade2ca7b904b36a3a4408c1a00852fc2c
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/08/2019
ms.locfileid: "74927831"
---
# <a name="quickstart-personalizer-client-library-for-net"></a>Guida introduttiva: Libreria client di Personalizza esperienze per .NET

Visualizzare il contenuto personalizzato in questa guida di avvio rapido di C# con il servizio Personalizza esperienze.

Introduzione alla libreria client di Personalizza esperienze per .NET. Seguire questi passaggi per installare il pacchetto e provare il codice di esempio per le attività di base.

 * Classificare un elenco di azioni per la personalizzazione.
 * Segnalare il punteggio di ricompensa che indica l'esito positivo dell'azione nella posizione più alta in classifica.

[Documentazione di riferimento](https://docs.microsoft.com/dotnet/api/Microsoft.Azure.CognitiveServices.Personalizer?view=azure-dotnet-preview) | [Codice sorgente della libreria](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Personalizer) | [Pacchetto (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Personalizer/) | [Esempi](https://github.com/Azure-Samples/cognitive-services-personalizer-samples)

## <a name="prerequisites"></a>Prerequisiti

* Sottoscrizione di Azure: [creare un account gratuito](https://azure.microsoft.com/free/)
* Versione corrente di [.NET Core](https://dotnet.microsoft.com/download/dotnet-core)

## <a name="using-this-quickstart"></a>Uso di questo avvio rapido

Per usare questo avvio rapido, è necessario eseguire diversi passaggi:

* Nel portale di Azure creare una risorsa di Personalizza esperienze
* Nella pagina **Configurazione** della risorsa di personalizza esperienze nel portale di Azure modificare la frequenza di aggiornamento del modello
* In un editor di codice creare un file di codice e modificarlo
* Dalla riga di comando o dal terminale installare l'SDK dalla riga di comando
* Dalla riga di comando o dal terminale eseguire il file di codice

## <a name="create-a-personalizer-azure-resource"></a>Creare una risorsa di Azure di Personalizza esperienze

Creare una risorsa per Personalizza esperienze usando il [portale di Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) o l'[interfaccia della riga di comando di Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) nel computer locale. È anche possibile:

* Ottenere un [codice della versione di valutazione gratuita](https://azure.microsoft.com/try/cognitive-services), valido per 7 giorni. Dopo aver eseguito l'iscrizione, sarà disponibile sul [sito Web di Azure](https://azure.microsoft.com/try/cognitive-services/my-apis/).  
* Visualizzare questa risorsa nel [portale di Azure](https://portal.azure.com/).

Dopo aver ottenuto una chiave dalla sottoscrizione di valutazione o dalla risorsa, creare due [variabili di ambiente](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication):

* `PERSONALIZER_RESOURCE_KEY` per la chiave della risorsa.
* `PERSONALIZER_RESOURCE_ENDPOINT` per l'endpoint della risorsa.

Nel portale di Azure i valori della chiave e dell'endpoint sono disponibili nella pagina **Avvio rapido**.

## <a name="change-the-model-update-frequency"></a>Modificare la frequenza di aggiornamento del modello

Nella pagina **Configurazione** della risorsa di Personalizza esperienze nel portale di Azure impostare **Frequenza di aggiornamento del modello** su 10 secondi. Con questa breve durata il training del servizio verrà eseguito rapidamente e sarà possibile visualizzare il cambiamento dell'azione più alta in classifica per ogni iterazione.

![Modificare la frequenza di aggiornamento del modello](./media/settings/configure-model-update-frequency-settings.png)

Quando viene creata per la prima volta un'istanza di un ciclo di Personalizza esperienze, non è disponibile alcun modello, perché non sono state effettuate chiamate all'API per le ricompense da cui eseguire il training. Le chiamate a Classifica restituiranno uguali probabilità per ogni elemento. L'applicazione dovrà sempre classificare il contenuto usando l'output di RewardActionId.

## <a name="create-a-new-c-application"></a>Creare una nuova applicazione C#

Creare una nuova applicazione .NET Core nell'ambiente di sviluppo integrato o nell'editor preferito. 

Nella finestra di una console (ad esempio cmd, PowerShell o Bash) usare il comando dotnet `new` per creare una nuova app console con il nome `personalizer-quickstart`. Questo comando crea un semplice progetto C# "Hello World" con un singolo file di origine: `Program.cs`. 

```console
dotnet new console -n personalizer-quickstart
```

Spostarsi nella cartella dell'app appena creata. È possibile compilare l'applicazione con il comando seguente:

```console
dotnet build
```

L'output di compilazione non deve contenere alcun avviso o errore. 

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

## <a name="install-the-sdk"></a>Installare l'SDK

Nella directory dell'applicazione installare la libreria client di Personalizza esperienze per .NET con il comando seguente:

```console
dotnet add package Microsoft.Azure.CognitiveServices.Personalizer --version 0.8.0-preview
```

Se si usa l'ambiente di sviluppo integrato di Visual Studio, la libreria client è disponibile come pacchetto NuGet scaricabile.

## <a name="object-model"></a>Modello a oggetti

Il client di Personalizza esperienze è un oggetto [PersonalizerClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclient?view=azure-dotnet) che esegue l'autenticazione ad Azure con Microsoft.Rest.ServiceClientCredentials, che contiene la chiave.

Per chiedere una classificazione del contenuto, creare un oggetto [RankRequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.models.rankrequest?view=azure-dotnet-preview) e passarlo al metodo [client.Rank](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclientextensions.rank?view=azure-dotnet-preview). Il metodo Rank restituisce un oggetto RankResponse che contiene il contenuto classificato. 

Per inviare una ricompensa a Personalizza esperienze, creare un oggetto [RewardRequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.models.rewardrequest?view=azure-dotnet-preview), quindi passarlo al metodo [client.Reward](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclientextensions.reward?view=azure-dotnet-preview). 

In questo argomento di avvio rapido, determinare la ricompensa è un'operazione banale. In un sistema di produzione invece, stabilire cosa influisce sul [punteggio di ricompensa](concept-rewards.md) e in quale misura può essere un processo complesso, che si può decidere di cambiare nel tempo. Questa dovrebbe essere una delle principali decisioni di progettazione da prendere nell'architettura di Personalizza esperienze. 

## <a name="code-examples"></a>Esempi di codice

Questi frammenti di codice mostrano come eseguire le operazioni seguenti con la libreria client di Personalizza esperienze per .NET:

* [Creare un client di Personalizza esperienze](#create-a-personalizer-client)
* [Richiedere una classificazione](#request-a-rank)
* [Inviare una ricompensa](#send-a-reward)

## <a name="add-the-dependencies"></a>Aggiungere le dipendenze

Dalla directory del progetto aprire il file **Program.cs** nell'ambiente di sviluppo integrato o nell'editor preferito. Sostituire il codice `using` esistente con le direttive `using` seguenti:

[!code-csharp[Using statements](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=Dependencies)]

## <a name="add-personalizer-resource-information"></a>Aggiungere le informazioni sulla risorsa di Personalizza esperienze

Nella classe **Program** creare le variabili per la chiave e l'endpoint di Azure della risorsa estratti dalle variabili di ambiente, denominate `PERSONALIZER_RESOURCE_KEY` e `PERSONALIZER_RESOURCE_ENDPOINT`. Se le variabili di ambiente sono state create dopo l'avvio dell'applicazione, per accedere alle variabili sarà necessario chiudere e ricaricare l'editor, la shell o l'ambiente di sviluppo integrato in cui sono in esecuzione. I metodi verranno creati in un secondo momento in questo argomento di avvio rapido.

[!code-csharp[Create variables to hold the Personalizer resource key and endpoint values found in the Azure portal.](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=classVariables)]

## <a name="create-a-personalizer-client"></a>Creare un client di Personalizza esperienze

Si creerà ora un metodo per restituire un client di Personalizza esperienze. Il parametro per il metodo è `PERSONALIZER_RESOURCE_ENDPOINT` e l'ApiKey è `PERSONALIZER_RESOURCE_KEY`.

[!code-csharp[Create the Personalizer client](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=authorization)]

## <a name="get-food-items-as-rankable-actions"></a>Recuperare gli elementi alimentari come azioni classificabili

Le azioni rappresentano le scelte di contenuto che si vuole classificare con Personalizza esperienze. Aggiungere i metodi seguenti alla classe Program per rappresentare il set di azioni da classificare.

[!code-csharp[Food items as actions](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=createAction)]

## <a name="get-user-preferences-for-context"></a>Recuperare le preferenze dell'utente per il contesto

Aggiungere i metodi seguenti alla classe Program per ottenere l'input di un utente dalla riga di comando per l'ora del giorno e la preferenza alimentare attuale. Questi valori verranno usati come contesto per la classificazione delle azioni.

[!code-csharp[Present time out day preference to the user](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=createUserFeatureTimeOfDay)]

[!code-csharp[Present food taste preference to the user](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=createUserFeatureTastePreference)]

Entrambi i metodi usano il metodo `GetKey` per leggere la selezione dell'utente dalla riga di comando. 

[!code-csharp[Read user's choice from the command line](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=readCommandLine)]

## <a name="create-the-learning-loop"></a>Creare il ciclo di apprendimento

Il ciclo di apprendimento di Personalizza esperienze è un ciclo di chiamate di classificazione e ricompensa. In questa guida di avvio rapido ogni chiamata di classificazione, per personalizzare il contenuto, è seguita da una chiamata di ricompensa, per indicare a Personalizza esperienze come il servizio ha classificato il contenuto. 

Il codice seguente nel metodo `main` del programma esegue un ciclo in cui chiede all'utente di indicare le sue preferenze dalla riga di comando, invia tali informazioni a Personalizza esperienze per la classificazione, presenta all'utente una selezione in forma di classifica da cui scegliere, quindi invia una ricompensa a Personalizza esperienze per segnalare l'accuratezza del servizio nella classificazione della selezione.

[!code-csharp[Learning loop](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=mainLoop)]

Aggiungere i metodi seguenti, che [ottengono le scelte di contenuto](#get-food-items-as-rankable-actions), prima di eseguire il file di codice:

* `GetActions`
* `GetUsersTimeOfDay`
* `GetUsersTastePreference`
* `GetKey`

## <a name="request-a-rank"></a>Richiedere una classificazione

Per completare la richiesta di classificazione, il programma richiede le preferenze dell'utente per creare un `currentContent` delle scelte di contenuto. Il processo può creare contenuto da escludere dalla classificazione, indicato come `excludeActions`. La richiesta di classificazione ha bisogno delle azioni, di currentContext, excludeActions e di un ID evento di classificazione univoco (come GUID) per ricevere la risposta classificata. 

In questo argomento di avvio rapido sono state usate funzionalità di contesto semplici come l'ora del giorno e le preferenze alimentari dell'utente. Nei sistemi di produzione, determinare e [valutare](concept-feature-evaluation.md) [azioni e funzionalità](concepts-features.md) può non essere altrettanto semplice.  

[!code-csharp[The Personalizer learning loop ranks the request.](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=rank)]

## <a name="send-a-reward"></a>Inviare una ricompensa

Per completare la richiesta di ricompensa, il programma ottiene la selezione dell'utente dalla riga di comando, assegna un valore numerico a ogni selezione, quindi invia l'ID evento di classificazione univoco e il valore numerico al metodo reward.

Questo argomento di avvio rapido assegna un numero semplice come ricompensa, ovvero zero o 1. Nei sistemi di produzione, determinare quando e cosa inviare alla chiamata [reward](concept-rewards.md) può essere un'operazione non banale, a seconda delle esigenze specifiche. 

[!code-csharp[The Personalizer learning loop ranks the request.](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=reward)]

## <a name="run-the-program"></a>Eseguire il programma

Eseguire l'applicazione con il comando dotnet `run` dalla directory dell'applicazione.

```console
dotnet run
```

![Il programma di avvio rapido pone alcune domande per raccogliere le preferenze utente, note come funzionalità, quindi restituisce l'azione più comune.](media/csharp-quickstart-commandline-feedback-loop/quickstart-program-feedback-loop-example.png)

Il [codice sorgente per questo argomento di avvio rapido](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/blob/master/quickstarts/csharp/PersonalizerExample/Program.cs) è disponibile nel repository GitHub di esempi di Personalizza esperienze.

## <a name="clean-up-resources"></a>Pulire le risorse

Se si vuole pulire e rimuovere una sottoscrizione a Servizi cognitivi, è possibile eliminare la risorsa o il gruppo di risorse. Eliminando il gruppo di risorse vengono eliminate anche tutte le altre risorse associate.

* [Portale](../cognitive-services-apis-create-account.md#clean-up-resources)
* [Interfaccia della riga di comando di Azure](../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
>[Funzionamento di Personalizza esperienze](how-personalizer-works.md)

* [Informazioni su Personalizza esperienze](what-is-personalizer.md)
* [In quali situazioni è possibile usare Personalizza esperienze?](where-can-you-use-personalizer.md)
* [Risoluzione dei problemi](troubleshooting.md)

