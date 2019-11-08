---
title: 'C#Esercitazione: indicizzare più origini dati'
titleSuffix: Azure Cognitive Search
description: Informazioni su come importare dati da più origini dati in un unico indice di ricerca cognitiva di Azure.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 69b18cdd4d0bb8e3d13bbacd5d21764004308786
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/07/2019
ms.locfileid: "73795648"
---
# <a name="c-tutorial-combine-data-from-multiple-data-sources-in-one-azure-cognitive-search-index"></a>C#Esercitazione: combinare i dati di più origini dati in un indice ricerca cognitiva di Azure

Azure ricerca cognitiva può importare, analizzare e indicizzare i dati da più origini dati in un unico indice di ricerca combinato. Questa funzionalità supporta situazioni in cui i dati strutturati vengono aggregati in dati meno strutturati o anche in testo normale di altre origini, come documenti di testo, HTML o JSON.

Questa esercitazione descrive come indicizzare i dati di hotel provenienti da un'origine dati di Azure Cosmos DB e come unirli ai dettagli delle camere ricavati da documenti di Archiviazione BLOB di Azure. Il risultato sarà un indice di ricerca combinato degli hotel contenente tipi di dati complessi.

Questa esercitazione USA C#, .NET SDK per Azure ricerca cognitiva e il portale di Azure per eseguire le attività seguenti:

> [!div class="checklist"]
> * Caricare dati di esempio e creare le origini dati
> * Identificare la chiave dei documenti
> * Definire e creare l'indice
> * Indicizzare i dati dell'hotel da Azure Cosmos DB
> * Unire i dati delle camere provenienti da Archiviazione BLOB

## <a name="prerequisites"></a>Prerequisiti

In questa guida di avvio rapido vengono usati i servizi, gli strumenti e i dati seguenti. 

- [Creare un servizio ricerca cognitiva di Azure](search-create-service-portal.md) o [trovare un servizio esistente](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) nella sottoscrizione corrente. È possibile usare un servizio gratuito per questa esercitazione.

- [Creare un account di Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/create-cosmosdb-resources-portal) per l'archiviazione dei dati di esempio dell'hotel.

- [Creare un account di archiviazione di Azure](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) per l'archiviazione dei dati BLOB JSON di esempio.

- [Installare Visual Studio](https://visualstudio.microsoft.com/) da usare come IDE.

### <a name="install-the-project-from-github"></a>Installare il progetto da GitHub

1. Individuare il repository di esempi in GitHub: [azure-search-dotnet-samples](https://github.com/Azure-Samples/azure-search-dotnet-samples).
1. Selezionare **Clone or download** (Clona o scarica) e creare una copia privata locale del repository.
1. Aprire Visual Studio e installare il pacchetto NuGet Microsoft Azure ricerca cognitiva, se non è già installato. Nel menu **strumenti** selezionare **Gestione pacchetti NuGet** e quindi **Gestisci pacchetti NuGet per la soluzione...** . Nella scheda **Sfoglia** trovare e quindi installare **Microsoft. Azure. search** (versione 9.0.1 o successiva). Sarà necessario completare altre finestra di dialogo per terminare l'installazione.

    ![Uso di NuGet per aggiungere librerie di Azure](./media/tutorial-csharp-create-first-app/azure-search-nuget-azure.png)

1. Usando Visual Studio, passare al repository locale e aprire il file della soluzione **AzureSearchMultipleDataSources.sln**.

## <a name="get-a-key-and-url"></a>Ottenere una chiave e un URL

Per interagire con il servizio ricerca cognitiva di Azure, sono necessari l'URL del servizio e una chiave di accesso. Viene creato un servizio di ricerca con entrambi, quindi, se è stata aggiunta la ricerca cognitiva di Azure alla sottoscrizione, seguire questa procedura per ottenere le informazioni necessarie:

1. [Accedere al portale di Azure](https://portal.azure.com/) e ottenere l'URL nella pagina **Panoramica** del servizio di ricerca. Un endpoint di esempio potrebbe essere simile a `https://mydemo.search.windows.net`.

1. In **Impostazioni** > **Chiavi** ottenere una chiave amministratore per diritti completi sul servizio. Sono disponibili due chiavi amministratore interscambiabili, fornite per continuità aziendale nel caso in cui sia necessario eseguire il rollover di una di esse. È possibile usare la chiave primaria o secondaria nelle richieste per l'aggiunta, la modifica e l'eliminazione di oggetti.

![Ottenere un endpoint HTTP e una chiave di accesso](media/search-get-started-postman/get-url-key.png "Ottenere un endpoint HTTP e una chiave di accesso")

Per ogni richiesta inviata al servizio è necessario specificare una chiave API. La presenza di una chiave valida stabilisce una relazione di trust, in base a singole richieste, tra l'applicazione che invia la richiesta e il servizio che la gestisce.

## <a name="prepare-sample-azure-cosmos-db-data"></a>Preparare i dati di esempio di Azure Cosmos DB

Questo esempio usa due piccoli set di dati che descrivono sette hotel fittizi. Un set descrive gli hotel stessi e verrà caricato in un database Azure Cosmos DB. L'altro set contiene i dettagli delle camere di hotel ed è suddiviso in sette file JSON separati da caricare in Archiviazione BLOB di Azure.

1. [Accedere al portale di Azure](https://portal.azure.com) e passare alla pagina Panoramica dell'account Azure Cosmos DB.

1. Fare clic su Aggiungi contenitore sulla barra dei menu. Specificare "Crea nuovo database" e assegnare il nome **hotel-rooms-db**. Immettere **hotels** come nome della raccolta e **/HotelId** come chiave della partizione. Fare clic su **OK** per creare il database e il contenitore.

   ![Aggiungi contenitore Azure Cosmos DB](media/tutorial-multiple-data-sources/cosmos-add-container.png "Aggiungere un contenitore di Azure Cosmos DB")

1. Passare a Esplora dati di Cosmos DB e selezionare l'elemento **items** nel contenitore **hotels** all'interno del database **hotel-rooms-db**. Quindi fare clic su **Upload Item** (Carica elemento).

   ![Carica nella raccolta Azure Cosmos DB](media/tutorial-multiple-data-sources/cosmos-upload.png "Carica nella raccolta Cosmos DB")

1. Nel riquadro di caricamento fare clic sul pulsante della cartella e passare al file **cosmosdb/HotelsDataSubset_CosmosDb.json** nella cartella del progetto. Fare clic su **OK** per avviare il caricamento.

   ![Selezionare il file da caricare](media/tutorial-multiple-data-sources/cosmos-upload2.png "Selezionare un file da caricare")

1. Premere Aggiorna per aggiornare la visualizzazione degli elementi nella raccolta di hotel. Dovrebbero essere visualizzati sette nuovi documenti di database.

## <a name="prepare-sample-blob-data"></a>Preparare i dati BLOB di esempio

1. [Accedere al portale di Azure](https://portal.azure.com), passare all'account di archiviazione di Azure, fare clic su **BLOB** e quindi su **+ Contenitore**.

1. [Creare un contenitore BLOB](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) denominato **hotel-rooms** in cui archiviare i file JSON delle camere di hotel di esempio. È possibile impostare il livello di accesso pubblico su uno qualsiasi dei relativi valori validi.

   ![Creare un contenitore BLOB](media/tutorial-multiple-data-sources/blob-add-container.png "Creare un contenitore BLOB")

1. Dopo aver creato il contenitore, aprirlo e selezionare **Carica** nella barra dei comandi.

   ![Carica sulla barra del comando](media/search-semi-structured-data/upload-command-bar.png "Carica sulla barra del comando")

1. Passare alla cartella contenente i file di esempio. Selezionare tutti i file e quindi fare clic su **Carica**.

   ![Caricare file](media/tutorial-multiple-data-sources/blob-upload.png "Caricare file")

Dopo aver completato il caricamento, i file dovrebbero essere visualizzati nell'elenco relativo al contenitore di dati.

## <a name="set-up-connections"></a>Configurare le connessioni

Le informazioni sulla connessione per il servizio di ricerca e le origini dati vengono specificate nel file **appsettings.json** nella soluzione. 

1. In Visual Studio aprire il file **AzureSearchMultipleDataSources.sln**.

1. In Esplora soluzioni modificare il file **appsettings.json**.  

```json
{
  "SearchServiceName": "Put your search service name here",
  "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
  "BlobStorageAccountName": "Put your Azure Storage account name here",
  "BlobStorageConnectionString": "Put your Azure Blob Storage connection string here",
  "CosmosDBConnectionString": "Put your Cosmos DB connection string here",
  "CosmosDBDatabaseName": "hotel-rooms-db"
}
```

Le prime due voci usano l'URL e le chiavi di amministrazione per il servizio ricerca cognitiva di Azure. Con `https://mydemo.search.windows.net` come endpoint, ad esempio, il nome del servizio da specificare sarà `mydemo`.

Per le voci successive specificare i nomi di account e le informazioni sulla stringa di connessione per le origini dati di Archiviazione BLOB di Azure e Azure Cosmos DB.

### <a name="identify-the-document-key"></a>Identificare la chiave dei documenti

In ricerca cognitiva di Azure il campo chiave identifica in modo univoco ogni documento nell'indice. Ogni indice di ricerca deve avere esclusivamente un campo chiave di tipo `Edm.String`. Questo campo deve essere presente per ogni documento di un'origine dati aggiunto all'indice. È l'unico campo obbligatorio.

Quando vengono indicizzati i dati di più origini dati, il valore della chiave di ogni origine dati deve corrispondere allo stesso campo della chiave nell'indice combinato. È spesso necessaria una pianificazione anticipata per identificare una chiave di documento significativa per l'indice e assicurarsi che esista in ogni origine dati.

Gli indicizzatori di Azure ricerca cognitiva possono usare i mapping dei campi per rinominare e persino riformattare i campi dati durante il processo di indicizzazione, in modo che i dati di origine possano essere indirizzati al campo indice corretto.

Ad esempio, nei dati di Azure Cosmos DB di esempio, l'identificatore di hotel è denominato **HotelId**. Tuttavia, nei file BLOB JSON per le chat Hotel l'identificatore dell'hotel è denominato **ID**. Il programma gestisce questa operazione eseguendo il mapping del campo **ID** dai BLOB al **campo chiave con** i puntini di indicizzazione nell'indice.

> [!NOTE]
> Nella maggior parte dei casi, le chiavi di documenti generate automaticamente, come quelle create per impostazione predefinita da alcuni indicizzatori, non sono valide per gli indici combinati. In generale, è consigliabile usare un valore di chiave univoco e significativo che esiste già nelle origini dati o può essere aggiunto facilmente.

## <a name="understand-the-code"></a>Informazioni sul codice

Dopo aver definito dati e impostazioni di configurazione, dovrebbe essere possibile compilare ed eseguire il programma di esempio in **AzureSearchMultipleDataSources.sln**.

Questa semplice app console in C#/.NET esegue le attività seguenti:
* Crea un nuovo indice di ricerca cognitiva di Azure in base alla struttura dei C# dati della classe di Hotel (che fa riferimento anche alle classi Address e room).
* Crea un'origine dati di Azure Cosmos DB e un indicizzatore che esegue il mapping dei dati di Azure Cosmos DB con i campi dell'indice.
* Esegue l'indicizzatore di Azure Cosmos DB per caricare i dati degli hotel.
* Crea un'origine dati di Archiviazione BLOB di Azure e un indicizzatore che esegue il mapping dei dati BLOB JSON con i campi dell'indice.
* Esegue l'indicizzatore di Archiviazione BLOB di Azure per caricare i dati delle camere.

 Prima di eseguire il programma, esaminare il codice e le definizioni di indice e indicizzatore per questo esempio. Il codice rilevante è disponibile in due file:

  + **Hotel.cs** contiene lo schema che definisce l'indice
  + **Program.cs** contiene funzioni che creano l'indice, le origini dati e gli indicizzatori di Azure ricerca cognitiva e caricano i risultati combinati nell'indice.

### <a name="define-the-index"></a>Definire l'indice

Questo programma di esempio USA .NET SDK per definire e creare un indice di ricerca cognitiva di Azure. Sfrutta la classe [FieldBuilder](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.fieldbuilder) per generare una struttura di indice da una classe di modello di dati C#.

Il modello di dati è definito dalla classe Hotel, che contiene anche i riferimenti alle classi Address e Rooms. FieldBuilder esegue il drill-down attraverso più definizioni di classi per generare una struttura dei dati complessa per l'indice. I tag di metadati vengono usati per definire gli attributi di ogni campo, ad esempio se è ricercabile o ordinabile.

I frammenti di codice seguenti del file **Hotel.cs** illustrano come sia possibile specificare un singolo campo e un riferimento a un'altra classe di modello di dati.

```csharp
. . . 
[IsSearchable, IsFilterable, IsSortable]
public string HotelName { get; set; }
. . .
public Room[] Rooms { get; set; }
. . .
```

Nel file **Program.cs** l'indice è definito con un nome e una raccolta di campi generata dal metodo `FieldBuilder.BuildForType<Hotel>()` e viene quindi creato come segue:

```csharp
private static async Task CreateIndex(string indexName, SearchServiceClient searchService)
{
    // Create a new search index structure that matches the properties of the Hotel class.
    // The Address and Room classes are referenced from the Hotel class. The FieldBuilder
    // will enumerate these to create a complex data structure for the index.
    var definition = new Index()
    {
        Name = indexName,
        Fields = FieldBuilder.BuildForType<Hotel>()
    };
    await searchService.Indexes.CreateAsync(definition);
}
```

### <a name="create-azure-cosmos-db-data-source-and-indexer"></a>Creare un'origine dati e un indicizzatore di Azure Cosmos DB

Il programma principale include la logica per creare l'origine dati di Azure Cosmos DB per i dati degli hotel.

Concatena prima di tutto il nome del database Azure Cosmos DB alla stringa di connessione. Quindi definisce l'oggetto origine dati, incluse le impostazioni specifiche per le origini di Azure Cosmos DB, ad esempio la proprietà [useChangeDetection].

  ```csharp
private static async Task CreateAndRunCosmosDbIndexer(string indexName, SearchServiceClient searchService)
{
    // Append the database name to the connection string
    string cosmosConnectString = 
        configuration["CosmosDBConnectionString"]
        + ";Database=" 
        + configuration["CosmosDBDatabaseName"];

    DataSource cosmosDbDataSource = DataSource.CosmosDb(
        name: configuration["CosmosDBDatabaseName"], 
        cosmosDbConnectionString: cosmosConnectString,
        collectionName: "hotels",
        useChangeDetection: true);

    // The Azure Cosmos DB data source does not need to be deleted if it already exists,
    // but the connection string might need to be updated if it has changed.
    await searchService.DataSources.CreateOrUpdateAsync(cosmosDbDataSource);
  ```

Dopo aver creato l'origine dati, il programma configura un indicizzatore di Azure Cosmos DB denominato **hotel-rooms-cosmos-indexer**.

```csharp
    Indexer cosmosDbIndexer = new Indexer(
        name: "hotel-rooms-cosmos-indexer",
        dataSourceName: cosmosDbDataSource.Name,
        targetIndexName: indexName,
        schedule: new IndexingSchedule(TimeSpan.FromDays(1)));
    
    // Indexers keep metadata about how much they have already indexed.
    // If we already ran this sample, the indexer will remember that it already
    // indexed the sample data and not run again.
    // To avoid this, reset the indexer if it exists.
    bool exists = await searchService.Indexers.ExistsAsync(cosmosDbIndexer.Name);
    if (exists)
    {
        await searchService.Indexers.ResetAsync(cosmosDbIndexer.Name);
    }
    await searchService.Indexers.CreateOrUpdateAsync(cosmosDbIndexer);
```
Il programma eliminerà eventuali indicizzatori esistenti con lo stesso nome prima di creare quello nuovo, nel caso si voglia eseguire questo esempio più volte.

Questo esempio definisce una pianificazione per l'indicizzatore, in modo che venga eseguito una volta al giorno. È possibile rimuovere la proprietà relativa alla pianificazione da questa chiamata se non si vuole che l'indicizzatore venga eseguito di nuovo automaticamente in futuro.

### <a name="index-azure-cosmos-db-data"></a>Indicizzare i dati di Azure Cosmos DB

Dopo la creazione dell'origine dati e dell'indicizzatore, il codice che esegue l'indicizzatore è semplice:

```csharp
    try
    {
        await searchService.Indexers.RunAsync(cosmosDbIndexer.Name);
    }
    catch (CloudException e) when (e.Response.StatusCode == (HttpStatusCode)429)
    {
        Console.WriteLine("Failed to run indexer: {0}", e.Response.Content);
    }
```

Questo esempio include un semplice blocco try-catch per segnalare gli eventuali errori che potrebbero verificarsi durante l'esecuzione.

Dopo l'esecuzione dell'indicizzatore di Azure Cosmos DB, l'indice di ricerca conterrà un set completo di documenti di hotel di esempio. Tuttavia, il campo delle camere per ogni hotel sarà una matrice vuota, perché l'origine dati di Azure Cosmos DB non contiene i relativi dettagli. Quindi, il programma esegue il pull da Archiviazione BLOB per caricare e unire i dati delle camere.

### <a name="create-blob-storage-data-source-and-indexer"></a>Creare l'origine dati e l'indicizzatore di Archiviazione BLOB

Per ottenere i dettagli delle camere, il programma configura prima di tutto un'origine dati di Archiviazione BLOB per fare riferimento a un set di singoli file BLOB JSON.

```csharp
private static async Task CreateAndRunBlobIndexer(string indexName, SearchServiceClient searchService)
{
    DataSource blobDataSource = DataSource.AzureBlobStorage(
        name: configuration["BlobStorageAccountName"],
        storageConnectionString: configuration["BlobStorageConnectionString"],
        containerName: "hotel-rooms");

    // The blob data source does not need to be deleted if it already exists,
    // but the connection string might need to be updated if it has changed.
    await searchService.DataSources.CreateOrUpdateAsync(blobDataSource);
```

Dopo aver creato l'origine dati, il programma configura un indicizzatore BLOB denominato **hotel-rooms-blob-indexer**.

```csharp
    // Add a field mapping to match the Id field in the documents to 
    // the HotelId key field in the index
    List<FieldMapping> map = new List<FieldMapping> {
        new FieldMapping("Id", "HotelId")
    };

    Indexer blobIndexer = new Indexer(
        name: "hotel-rooms-blob-indexer",
        dataSourceName: blobDataSource.Name,
        targetIndexName: indexName,
        fieldMappings: map,
        parameters: new IndexingParameters().ParseJson(),
        schedule: new IndexingSchedule(TimeSpan.FromDays(1)));

    // Reset the indexer if it already exists
    bool exists = await searchService.Indexers.ExistsAsync(blobIndexer.Name);
    if (exists)
    {
        await searchService.Indexers.ResetAsync(blobIndexer.Name);
    }
    await searchService.Indexers.CreateOrUpdateAsync(blobIndexer);
```

I BLOB JSON contengono un campo di chiave denominato **Id** invece di **HotelId**. Il codice usa la classe `FieldMapping` per indicare all'indicizzatore di indirizzare il valore del campo **Id** alla chiave di documento **HotelId** nell'indice.

Gli indicizzatori di Archiviazione BLOB possono usare parametri che identificano la modalità di analisi da usare. La modalità di analisi per i BLOB che rappresentano un singolo documento è diversa da quella per più documenti all'interno dello stesso BLOB. In questo esempio ogni BLOB rappresenta un singolo documento di indice, quindi il codice usa il parametro `IndexingParameters.ParseJson()`.

Per altre informazioni sui parametri di analisi dell'indicizzatore per i BLOB JSON, vedere [Indicizzare i BLOB JSON](search-howto-index-json-blobs.md). Per altre informazioni su come specificare questi parametri con .NET SDK, vedere la classe [IndexerParametersExtension](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexingparametersextensions).

Il programma eliminerà eventuali indicizzatori esistenti con lo stesso nome prima di creare quello nuovo, nel caso si voglia eseguire questo esempio più volte.

Questo esempio definisce una pianificazione per l'indicizzatore, in modo che venga eseguito una volta al giorno. È possibile rimuovere la proprietà relativa alla pianificazione da questa chiamata se non si vuole che l'indicizzatore venga eseguito di nuovo automaticamente in futuro.

### <a name="index-blob-data"></a>Indicizzare i dati BLOB

Dopo la creazione dell'origine dati e dell'indicizzatore di Archiviazione BLOB, il codice che esegue l'indicizzatore è semplice:

```csharp
    try
    {
        await searchService.Indexers.RunAsync(cosmosDbIndexer.Name);
    }
    catch (CloudException e) when (e.Response.StatusCode == (HttpStatusCode)429)
    {
        Console.WriteLine("Failed to run indexer: {0}", e.Response.Content);
    }
```

Poiché l'indice è già stato popolato con i dati degli hotel del database Azure Cosmos DB, l'indicizzatore BLOB aggiorna i documenti esistenti nell'indice e aggiunge i dettagli delle camere.

> [!NOTE]
> Se entrambe le origini dati contengono gli stessi campi non di chiave e i dati all'interno di tali campi non corrispondono, l'indice conterrà i valori di qualsiasi indicizzatore eseguito più di recente. In questo esempio entrambe le origini dati contengono un campo **HotelName**. Se per qualche motivo i dati di questo campo sono diversi per i documenti con lo stesso valore di chiave, il valore archiviato nell'indice corrisponderà ai dati **HotelName** dell'origine dati data indicizzata più di recente.

## <a name="search-your-json-files"></a>Cercare i file JSON

È possibile esaminare l'indice di ricerca popolato dopo l'esecuzione del programma usando [**Esplora ricerche**](search-explorer.md) nel portale.

Nel portale di Azure aprire la pagina **Panoramica** del servizio di ricerca e individuare l'indice **hotel-rooms-sample** nell'elenco **Indici**.

  ![Elenco di indici di ricerca cognitiva di Azure](media/tutorial-multiple-data-sources/index-list.png "Elenco di indici di ricerca cognitiva di Azure")

Fare clic sull'indice hotel-rooms-sample nell'elenco. Verrà visualizzata un'interfaccia di Esplora ricerche per l'indice. Immettere una query per un termine tipo "Luxury". I risultati dovrebbero contenere almeno un documento, che dovrebbe mostrare un elenco di oggetti camera nella matrice di camere.

## <a name="clean-up-resources"></a>Pulire le risorse

Il modo più rapido per eseguire la pulizia dopo un'esercitazione consiste nell'eliminare il gruppo di risorse che contiene il servizio ricerca cognitiva di Azure. È possibile eliminare ora il gruppo di risorse per eliminare definitivamente tutti gli elementi in esso contenuti. Nel portale il nome del gruppo di risorse si trova nella pagina Panoramica del servizio ricerca cognitiva di Azure.

## <a name="next-steps"></a>Passaggi successivi

Esistono diversi approcci e più opzioni all'indicizzazione dei BLOB JSON. Se l'origine dati include contenuto JSON, è possibile esaminare queste opzioni per verificare quali sono ottimali per lo scenario.

> [!div class="nextstepaction"]
> [Come indicizzare i BLOB JSON con l'indicizzatore BLOB di Azure ricerca cognitiva](search-howto-index-json-blobs.md)

È possibile scegliere di aumentare i dati dell'indice strutturato di un'origine dati con dati che presentano arricchimenti cognitivi provenienti da BLOB non strutturati o contenuto full-text. L'esercitazione seguente illustra come usare servizi cognitivi insieme ad Azure ricerca cognitiva, usando .NET SDK.

> [!div class="nextstepaction"]
> [Chiamare API Servizi cognitivi in una pipeline di indicizzazione ricerca cognitiva di Azure](cognitive-search-tutorial-blob-dotnet.md)
