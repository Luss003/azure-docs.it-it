---
title: Informazioni su come proteggere l'accesso ai dati in Azure Cosmos DB
description: Informazioni sui concetti di controllo di accesso in Azure Cosmos DB, tra cui chiavi master, chiavi di sola lettura, utenti e autorizzazioni.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.openlocfilehash: ec32dfbe9aac13a9b3c08922b73b9a23d668d744
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/30/2019
ms.locfileid: "73152283"
---
# <a name="secure-access-to-data-in-azure-cosmos-db"></a>Proteggere l'accesso ai dati in Azure Cosmos DB

Questo articolo offre una panoramica della protezione dell'accesso ai dati archiviati in [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).

Azure Cosmos DB usa due tipi di chiavi per autenticare gli utenti e fornire accesso ai dati e alle risorse. 

|Tipo di chiave|resources|
|---|---|
|[Chiavi master](#master-keys) |Usate per risorse amministrative, ovvero account di database, database, utenti e autorizzazioni|
|[Token delle risorse](#resource-tokens)|Usati per le risorse dell'applicazione, ovvero contenitori, documenti, allegati, stored procedure, trigger e funzioni definite dall'utente|

<a id="master-keys"></a>

## <a name="master-keys"></a>Chiavi master 

Le chiavi master consentono di accedere a tutte le risorse amministrative per l'account del database. Chiavi master:  
- Consentono di accedere ad account, database, utenti e autorizzazioni. 
- Non possono essere usate per fornire l'accesso granulare a contenitori e documenti.
- Vengono create durante la creazione di un account.
- Possono essere rigenerate in qualsiasi momento.

Ogni account è costituito da due chiavi master: una chiave primaria e una chiave secondaria. Lo scopo delle due chiavi è consentire la rigenerazione o la rotazione delle chiavi mantenendo l'accesso continuo ai dati e all'account. 

Oltre alle due chiavi master per l'account Cosmos DB, sono disponibili due chiavi di sola lettura. Queste chiavi consentono solo operazioni di lettura per l'account. Le chiavi di sola lettura non forniscono l'accesso in lettura alle risorse di autorizzazione.

Le chiavi master primaria, secondaria, di sola lettura e di lettura/scrittura possono essere recuperate e rigenerate nel portale di Azure. Per istruzioni, vedere [Visualizzare, copiare e rigenerare le chiavi di accesso](manage-with-cli.md#regenerate-account-key).

![Controllo di accesso (IAM) nel portale di Azure: dimostrazione della sicurezza del database NoSQL](./media/secure-access-to-data/nosql-database-security-master-key-portal.png)

Il processo di rotazione della chiave master è semplice. Passare al portale di Azure per recuperare la chiave secondaria, quindi sostituire la chiave primaria con la chiave secondaria nell'applicazione e ruotare la chiave primaria nel portale di Azure.

![Rotazione delle chiavi master nel portale di Azure: dimostrazione della sicurezza del database NoSQL](./media/secure-access-to-data/nosql-database-security-master-key-rotate-workflow.png)

### <a name="code-sample-to-use-a-master-key"></a>Esempio di codice per l'uso di una chiave master

L'esempio di codice seguente illustra come usare un endpoint dell'account Cosmos DB e la chiave master per creare un'istanza di DocumentClient e un database. 

```csharp
//Read the Azure Cosmos DB endpointUrl and authorization keys from config.
//These values are available from the Azure portal on the Azure Cosmos DB account blade under "Keys".
//NB > Keep these values in a safe and secure location. Together they provide Administrative access to your DocDB account.

private static readonly string endpointUrl = ConfigurationManager.AppSettings["EndPointUrl"];
private static readonly string authorizationKey = ConfigurationManager.AppSettings["AuthorizationKey"];

client = new DocumentClient(new Uri(endpointUrl), authorizationKey);

// Create Database
Database database = await client.CreateDatabaseAsync(
    new Database
    {
        Id = databaseName
    });
```

<a id="resource-tokens"></a>

## <a name="resource-tokens"></a>Token delle risorse

I token delle risorse consentono di accedere alle risorse dell'applicazione all'interno di un database. Token delle risorse:
- Consentono di accedere a contenitori, chiavi di partizioni, documenti, allegati, stored procedure, trigger e funzioni definite dall'utente.
- Vengono creati quando a un [utente](#users) vengono concesse [autorizzazioni](#permissions) per una risorsa specifica.
- Vengono ricreati quando si interviene su una risorsa di autorizzazione tramite chiamata POST, GET o PUT.
- Usano un token di risorsa hash costruito specificamente per l'utente, la risorsa e l'autorizzazione.
- Hanno un limite di tempo con un periodo di validità personalizzabile. L'intervallo di tempo valido predefinito è un'ora. La durata del token può essere tuttavia specificata in modo esplicito, fino a un massimo di cinque ore.
- Rappresentano un'alternativa sicura alla divulgazione della chiave master. 
- Consentono ai client di leggere, scrivere ed eliminare risorse nell'account Cosmos DB in base alle autorizzazioni concesse.

È possibile usare un token delle risorse (creando utenti e autorizzazioni di Cosmos DB) quando si vuole fornire l'accesso alle risorse dell'account Cosmos DB a un client non attendibile con la chiave master.  

I token delle risorse di Cosmos DB costituiscono un'alternativa sicura che consente ai client di leggere, scrivere ed eliminare risorse nell'account Cosmos DB sulla base delle autorizzazioni concesse e senza richiedere la chiave master o di sola lettura.

Di seguito è riportato un tipico schema progettuale in cui i token delle risorse possono essere richiesti, generati e forniti ai client:

1. Un servizio di livello intermedio viene configurato per gestire un'applicazione per dispositivi mobili per condividere le foto dell'utente. 
2. Il servizio di livello intermedio ha la chiave master dell'account Cosmos DB.
3. L'app per le foto viene installata nei dispositivi mobili dell'utente finale. 
4. All'accesso, l'app per le foto stabilisce l'identità dell'utente con il servizio di livello intermedio. Questo meccanismo per stabilire l'identità dipende completamente dall'applicazione.
5. Una volta stabilita l'identità, il servizio di livello intermedio richiede le autorizzazioni in base all'identità.
6. Il servizio di livello intermedio invia di nuovo un token delle risorse all'app per il telefono.
7. L'app per il telefono può continuare a usare il token delle risorse per accedere direttamente alle risorse di Cosmos DB con le autorizzazioni definite dal token delle risorse e per l'intervallo consentito dal token delle risorse. 
8. Quando il token delle risorse scade, le richieste successive ricevono un'eccezione 401 Non autorizzato.  A questo punto, l'app per il telefono ristabilisce l'identità e richiede un nuovo token delle risorse.

    ![Flusso di lavoro dei token delle risorse di Azure Cosmos DB](./media/secure-access-to-data/resourcekeyworkflow.png)

La generazione e la gestione dei token delle risorse vengono gestite dalle librerie client di Cosmos DB native. Se tuttavia si usa REST, è necessario creare le intestazioni di richiesta/autenticazione. Per altre informazioni sulla creazione di intestazioni di autenticazione per REST, vedere [Access Control on Cosmos DB Resources](https://docs.microsoft.com/rest/api/cosmos-db/access-control-on-cosmosdb-resources) (Controllo di accesso per le risorse di Cosmos DB) oppure il [codice sorgente per gli SDK](https://github.com/Azure/azure-documentdb-node/blob/master/source/lib/auth.js).

Per un esempio di servizio di livello intermedio usato per generare o negoziare i token delle risorse, vedere l'app [ResourceTokenBroker](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker/Controllers).

<a id="users"></a>

## <a name="users"></a>Utenti
Cosmos DB utenti sono associati a un database Cosmos.  Ogni database può contenere zero o più utenti di Cosmos DB.  L'esempio di codice seguente illustra come creare una risorsa utente di Cosmos DB.

```csharp
//Create a user.
User docUser = new User
{
    Id = "mobileuser"
};

docUser = await client.CreateUserAsync(UriFactory.CreateDatabaseUri("db"), docUser);
```

> [!NOTE]
> Ogni utente di Cosmos DB ha una proprietà PermissionsLink che può essere usata per recuperare l'elenco di [autorizzazioni](#permissions) associate all'utente.
> 
> 

<a id="permissions"></a>

## <a name="permissions"></a>autorizzazioni
Una risorsa autorizzazione di Cosmos DB è associata a un utente di Cosmos DB.  Ogni utente può contenere zero o più autorizzazioni di Cosmos DB.  Una risorsa autorizzazione fornisce l'accesso a un token di sicurezza di cui l'utente ha bisogno quando deve accedere a una risorsa applicazione specifica.
Sono disponibili due livelli di accesso che possono essere forniti da una risorsa di autorizzazione:

* Tutto: l'utente ha l'autorizzazione completa per la risorsa.
* Lettura: l'utente può solo leggere i contenuti della risorsa, ma non può eseguire operazioni di scrittura, aggiornamento o eliminazione sulla risorsa.

> [!NOTE]
> Per eseguire le stored procedure di Cosmos DB, l'utente deve avere l'autorizzazione All per i contenitori in cui verrà eseguita la stored procedure.
> 
> 

### <a name="code-sample-to-create-permission"></a>Esempio di codice per la creazione dell'autorizzazione

L'esempio di codice seguente illustra come creare una risorsa di autorizzazione, leggere il token delle risorse della risorsa di autorizzazione e associare le autorizzazioni all'[utente](#users) creato in precedenza.

```csharp
// Create a permission.
Permission docPermission = new Permission
{
    PermissionMode = PermissionMode.Read,
    ResourceLink = UriFactory.CreateDocumentCollectionUri("db", "collection"),
    Id = "readperm"
};
  
docPermission = await client.CreatePermissionAsync(UriFactory.CreateUserUri("db", "user"), docPermission);
Console.WriteLine(docPermission.Id + " has token of: " + docPermission.Token);
```

Se è stato specificata una chiave di partizione per la raccolta, l'autorizzazione per le risorse di raccolta, documenti e allegati deve includere anche ResourcePartitionKey oltre a ResourceLink.

### <a name="code-sample-to-read-permissions-for-user"></a>Codice di esempio per la lettura di autorizzazioni dell'utente

Per ottenere facilmente tutte le risorse autorizzazione associate a un particolare utente, Cosmos DB rende disponibile un feed di autorizzazioni per ogni oggetto utente.  Il frammento di codice seguente illustra come recuperare l'autorizzazione associata all'utente creato in precedenza, costruire un elenco di autorizzazioni e creare un'istanza di un nuovo DocumentClient per conto dell'utente.

```csharp
//Read a permission feed.
FeedResponse<Permission> permFeed = await client.ReadPermissionFeedAsync(
  UriFactory.CreateUserUri("db", "myUser"));
 List<Permission> permList = new List<Permission>();

foreach (Permission perm in permFeed)
{
    permList.Add(perm);
}

DocumentClient userClient = new DocumentClient(new Uri(endpointUrl), permList);
```

## <a name="add-users-and-assign-roles"></a>Aggiungere utenti e assegnare ruoli

Per aggiungere l'accesso in lettura dell'account Azure Cosmos DB al proprio account utente, è necessario che il proprietario di una sottoscrizione esegua la procedura seguente nel portale di Azure.

1. Aprire il portale di Azure e selezionare l'account Azure Cosmos DB.
2. Fare clic sulla scheda **Controllo di accesso (IAM)** e quindi su **+ Aggiungi assegnazione di ruolo**.
3. Nel riquadro **Aggiungi assegnazione di ruolo**, nella casella **Ruolo**, selezionare **Ruolo Lettore dell'account Cosmos DB**.
4. Nella casella **Assegna accesso a** selezionare **Applicazione, gruppo o utente di Azure AD**.
5. Selezionare l'utente, il gruppo o l'applicazione nella directory a cui si vuole concedere l'accesso.  È possibile eseguire ricerche nella directory in base al nome visualizzato, all'indirizzo di posta elettronica o all'identificatore dell'oggetto.
    L'applicazione, il gruppo o l'utente selezionato viene visualizzato nell'elenco dei membri selezionati.
6. Fare clic su **Salva**

L'entità può ora leggere le risorse di Azure Cosmos DB.

## <a name="delete-or-export-user-data"></a>Eliminare o esportare i dati utente
Azure Cosmos DB consente di eseguire la ricerca, selezionare, modificare ed eliminare tutti i dati personali che si trovano nel database o nelle raccolte. Azure Cosmos DB offre API per trovare ed eliminare i dati personali; tuttavia, è necessario usare le API e definire la logica necessaria per cancellare i dati personali. Ogni API multimodello (SQL, MongoDB, Gremlin, Cassandra, Tabella) fornisce SDK per linguaggi diversi che contengono metodi per cercare ed eliminare i dati personali. È inoltre possibile abilitare la funzionalità [time-to live (TTL)](time-to-live.md) per eliminare automaticamente i dati dopo un periodo specificato, senza incorrere in costi aggiuntivi.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="next-steps"></a>Passaggi successivi
* Per ulteriori informazioni sulla sicurezza di Cosmos database, vedere [Cosmos DB: sicurezza del database](database-security.md).
* Per informazioni su come costruire i token di autorizzazione di Azure Cosmos DB, vedere [Access Control on Azure Cosmos DB Resources](https://docs.microsoft.com/rest/api/cosmos-db/access-control-on-cosmosdb-resources) (Controllo di accesso per le risorse di Azure Cosmos DB).
