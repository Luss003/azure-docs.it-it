---
title: "Guida introduttiva: Connettere un'app MongoDB Node.js ad Azure Cosmos DB"
description: Questo Avvio rapido illustra come connettere un'app MongoDB esistente scritta in Node. js ad Azure Cosmos DB.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 05/21/2019
ms.custom: seo-javascript-september2019, seo-javascript-october2019
ms.openlocfilehash: c2a689f7c3ac1308e12d0e371a9ad7f7187417d6
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2019
ms.locfileid: "72792183"
---
# <a name="quickstart-migrate-an-existing-mongodb-nodejs-web-app-to-azure-cosmos-db"></a>Guida introduttiva: Eseguire la migrazione di un'app Web Node.js MongoDB esistente ad Azure Cosmos DB 

> [!div class="op_single_selector"]
> * [.NET](create-mongodb-dotnet.md)
> * [Java](create-mongodb-java.md)
> * [Node.js](create-mongodb-nodejs.md)
> * [Python](create-mongodb-flask.md)
> * [Xamarin](create-mongodb-xamarin.md)
> * [Golang](create-mongodb-golang.md)
>  

Questo argomento di avvio rapido illustra come usare un'app MongoDB esistente scritta in Node.js e connetterla al database Azure Cosmos, che supporta il client MongoDB. In altre parole, il fatto che i dati siano archiviati in un database Cosmos è trasparente per l'applicazione.

Azure Cosmos DB è il servizio di database di Microsoft multimodello distribuito a livello globale. È possibile creare rapidamente database di documenti, chiave/valore e a grafo ed eseguire query su di essi sfruttando in ognuno dei casi i vantaggi offerti dalle funzionalità di scalabilità orizzontale e distribuzione globale alla base di Cosmos DB.

Al termine, si avrà un'applicazione MEAN (MongoDB, Express, Angular e Node.js) in esecuzione in [Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). 

![App MEAN.js in esecuzione nel Servizio app di Azure](./media/create-mongodb-nodejs/meanjs-in-azure.png)


[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Se si sceglie di installare e usare l'interfaccia della riga di comando in locale, per questo argomento è necessario eseguire la versione 2.0 o successiva dell'interfaccia della riga di comando di Azure. Eseguire `az --version` per trovare la versione. Se è necessario eseguire l'installazione o l'aggiornamento, vedere [Installare l'interfaccia della riga di comando di Azure]( /cli/azure/install-azure-cli). 

## <a name="prerequisites"></a>Prerequisiti 
Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare. 
[!INCLUDE [cosmos-db-emulator-mongodb](../../includes/cosmos-db-emulator-mongodb.md)]

Oltre all'interfaccia della riga di comando di Azure, è necessario che [Node.js](https://nodejs.org/) e [Git](https://www.git-scm.com/downloads) siano installati localmente per eseguire i comandi `npm` e `git`.

È necessario saper usare Node.js. Questa guida di avvio rapido non ha lo scopo di offrire informazioni generali sullo sviluppo di applicazioni Node.js.

## <a name="clone-the-sample-application"></a>Clonare l'applicazione di esempio

Eseguire i comandi seguenti per clonare il repository di esempio. Questo archivio di esempio contiene l'applicazione [MEAN.js](https://meanjs.org/) predefinita.

1. Aprire un prompt dei comandi, creare una nuova cartella denominata git-samples e quindi chiudere il prompt dei comandi.

    ```bash
    mkdir "C:\git-samples"
    ```

2. Aprire una finestra del terminale Git, ad esempio Git Bash, ed eseguire il comando `cd` per passare a una nuova cartella in cui installare l'app di esempio.

    ```bash
    cd "C:\git-samples"
    ```

3. Eseguire il comando seguente per clonare l'archivio di esempio. Questo comando crea una copia dell'app di esempio nel computer in uso. 

    ```bash
    git clone https://github.com/prashanthmadi/mean
    ```

## <a name="run-the-application"></a>Eseguire l'applicazione

Installare i pacchetti necessari e avviare l'applicazione.

```bash
cd mean
npm install
npm start
```
L'applicazione tenterà di connettersi a un'origine MongoDB e non riuscirà. Continuare e uscire dall'applicazione quando l'output restituisce "[MongoError: connect ECONNREFUSED 127.0.0.1:27017]".

## <a name="log-in-to-azure"></a>Accedere ad Azure

Se si usa un'interfaccia della riga di comando di Azure installata, accedere alla sottoscrizione di Azure con il comando [az login](/cli/azure/reference-index#az-login) e seguire le istruzioni visualizzate. Se si usa Azure Cloud Shell, è possibile ignorare questo passaggio.

```azurecli
az login 
``` 
   
## <a name="add-the-azure-cosmos-db-module"></a>Aggiungere il modulo Azure Cosmos DB

Se si usa un'interfaccia della riga di comando di Azure installata, verificare se il componente `cosmosdb` è già installato eseguendo il comando `az`. Se `cosmosdb` è nell'elenco di comandi di base, passare al comando successivo. Se si usa Azure Cloud Shell, è possibile ignorare questo passaggio.

Se `cosmosdb` non è nell'elenco dei comandi di base, reinstallare l'[interfaccia della riga di comando di Azure]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Creare un gruppo di risorse

Creare un [gruppo di risorse](../azure-resource-manager/resource-group-overview.md) con il comando [az group create](/cli/azure/group#az-group-create). Un gruppo di risorse di Azure è un contenitore logico in cui vengono distribuite e gestite risorse di Azure come app Web, database e account di archiviazione. 

L'esempio seguente crea un gruppo di risorse nell'area Europa occidentale. Scegliere un nome univoco per il gruppo di risorse.

Se si usa Azure Cloud Shell, selezionare **Prova**, seguire le istruzioni visualizzate per eseguire l'accesso e quindi copiare il comando nel prompt dei comandi.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

## <a name="create-an-azure-cosmos-db-account"></a>Creare un account Azure Cosmos DB

Creare un account Cosmos con il comando [az cosmosdb create](/cli/azure/cosmosdb#az-cosmosdb-create).

Nel comando seguente sostituire il segnaposto `<cosmosdb-name>` con il nome univoco dell'account Cosmos. Questo nome univoco verrà usato come parte dell'endpoint Cosmos DB, `https://<cosmosdb-name>.documents.azure.com/`, quindi deve essere univoco tra tutti gli account Cosmos in Azure. 

```azurecli-interactive
az cosmosdb create --name <cosmosdb-name> --resource-group myResourceGroup --kind MongoDB
```

Il parametro `--kind MongoDB` consente le connessioni al client MongoDB.

Dopo la creazione dell'account Azure Cosmos DB, l'interfaccia della riga di comando di Azure mostra informazioni simili all'esempio seguente. 

> [!NOTE]
> Questo esempio usa JSON come formato di output dell'interfaccia della riga di comando di Azure, ovvero l'impostazione predefinita. Per usare un altro formato di output, vedere [Formati di output per i comandi dell'interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/format-output-azure-cli).

```json
{
  "databaseAccountOfferType": "Standard",
  "documentEndpoint": "https://<cosmosdb-name>.documents.azure.com:443/",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Document
DB/databaseAccounts/<cosmosdb-name>",
  "kind": "MongoDB",
  "location": "West Europe",
  "name": "<cosmosdb-name>",
  "readLocations": [
    {
      "documentEndpoint": "https://<cosmosdb-name>-westeurope.documents.azure.com:443/",
      "failoverPriority": 0,
      "id": "<cosmosdb-name>-westeurope",
      "locationName": "West Europe",
      "provisioningState": "Succeeded"
    }
  ],
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.DocumentDB/databaseAccounts",
  "writeLocations": [
    {
      "documentEndpoint": "https://<cosmosdb-name>-westeurope.documents.azure.com:443/",
      "failoverPriority": 0,
      "id": "<cosmosdb-name>-westeurope",
      "locationName": "West Europe",
      "provisioningState": "Succeeded"
    }
  ]
} 
```

## <a name="connect-your-nodejs-application-to-the-database"></a>Connettere l'applicazione Node.js al database

In questo passaggio si connette l'applicazione di esempio MEAN.js al database Cosmos appena creato. 

<a name="devconfig"></a>
## <a name="configure-the-connection-string-in-your-nodejs-application"></a>Configurare la stringa di connessione nell'applicazione Node.js

Nel repository di MEAN.js aprire `config/env/local-development.js`.

Sostituire il contenuto del file con il codice seguente. Assicurarsi di sostituire anche i due segnaposto `<cosmosdb-name>` con il nome dell'account Cosmos.

```javascript
'use strict';

module.exports = {
  db: {
    uri: 'mongodb://<cosmosdb-name>:<primary_master_key>@<cosmosdb-name>.documents.azure.com:10255/mean-dev?ssl=true&sslverifycertificate=false'
  }
};
```

## <a name="retrieve-the-key"></a>Recuperare la chiave

Per connettersi a un database Cosmos, è necessaria la chiave del database. Usare il comando [az cosmosdb keys list](/cli/azure/cosmosdb/keys#az-cosmosdb-keys-list) per recuperare la chiave primaria.

```azurecli-interactive
az cosmosdb keys list --name <cosmosdb-name> --resource-group myResourceGroup --query "primaryMasterKey"
```

L'interfaccia della riga di comando di Azure restituisce informazioni simili all'esempio seguente. 

```json
"RUayjYjixJDWG5xTqIiXjC..."
```

Copiare il valore di `primaryMasterKey`. Incollarlo tramite `<primary_master_key>` in `local-development.js`.

Salvare le modifiche.

### <a name="run-the-application-again"></a>Eseguire di nuovo l'applicazione.

Eseguire di nuovo `npm start`. 

```bash
npm start
```

Un messaggio della console dovrebbe ora indicare che l'ambiente di sviluppo è operativo. 

Passare a `http://localhost:3000` in un browser. Selezionare **Iscrizione** nel menu in alto e provare a creare due utenti fittizi. 

L'applicazione di esempio MEAN.js archivia i dati utente nel database. Se l'operazione ha esito positivo e l'applicazione MEAN.js accede automaticamente all'utente creato, la connessione ad Azure Cosmos DB funziona. 

![MEAN.js si connette correttamente a MongoDB](./media/create-mongodb-nodejs/mongodb-connect-success.png)

## <a name="view-data-in-data-explorer"></a>Visualizzare i dati in Esplora dati

I dati archiviati in un database Cosmos sono disponibili per la visualizzazione e le query nel portale di Azure.

Per visualizzare e usare i dati utente creati nel passaggio precedente, nonché eseguire query su di essi, accedere al [portale di Azure](https://portal.azure.com) nel Web browser.

Nella casella di ricerca nella parte superiore immettere **Azure Cosmos DB**. Quando viene aperto il pannello dell'account Cosmos, selezionare l'account Cosmos. Nel riquadro di spostamento a sinistra selezionare **Esplora dati**. Espandere la raccolta nel riquadro Raccolte. Sarà quindi possibile visualizzare i documenti nella raccolta, eseguire query sui dati e anche creare ed eseguire stored procedure, trigger e funzioni definite dall'utente. 

![Esplora dati nel portale di Azure](./media/create-mongodb-nodejs/cosmosdb-connect-mongodb-data-explorer.png)


## <a name="deploy-the-nodejs-application-to-azure"></a>Distribuire l'applicazione Node.js in Azure

In questo passaggio si distribuisce l'applicazione Node.js in Cosmos DB.

Si sarà probabilmente notato che il file di configurazione modificato in precedenza è per l'ambiente di sviluppo (`/config/env/local-development.js`). Quando si distribuisce l'applicazione nel servizio app, per impostazione predefinita viene eseguita nell'ambiente di produzione. A questo punto, è quindi necessario apportare la stessa modifica al file di configurazione corrispondente.

Nel repository di MEAN.js aprire `config/env/production.js`.

Nell'oggetto `db` sostituire il valore di `uri` come mostrato nell'esempio seguente. Assicurarsi di sostituire i segnaposto come fatto in precedenza.

```javascript
'mongodb://<cosmosdb-name>:<primary_master_key>@<cosmosdb-name>.documents.azure.com:10255/mean?ssl=true&sslverifycertificate=false',
```

> [!NOTE] 
> L'opzione `ssl=true` è importante perché [Cosmos DB richiede l'SSL](connect-mongodb-account.md#connection-string-requirements). 
>
>

Nel terminale eseguire il commit tutte le modifiche in Git. È possibile copiare entrambi i comandi per eseguirli insieme.

```bash
git add .
git commit -m "configured MongoDB connection string"
```
## <a name="clean-up-resources"></a>Pulire le risorse

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Passaggi successivi

In questo argomento di avvio rapido si è appreso come creare un account Cosmos, creare una raccolta ed eseguire un'app console. È ora possibile importare dati aggiuntivi nel database Cosmos. 

> [!div class="nextstepaction"]
> [Importare i dati di MongoDB in Azure Cosmos DB](mongodb-migrate.md)
