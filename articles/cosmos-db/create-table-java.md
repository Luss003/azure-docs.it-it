---
title: Usare l'API Tabella e Java per creare un'app - Azure Cosmos DB
description: Questa guida introduttiva illustra come usare l'API Tabelle di Azure Cosmos DB per creare un'applicazione con il portale di Azure e Java
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: java
ms.topic: quickstart
ms.date: 04/10/2018
ms.author: sngun
ms.custom: seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 14742984fb993679abc87e279f3ad9882ec77ce3
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/25/2019
ms.locfileid: "71266032"
---
# <a name="quickstart-build-a-java-app-to-manage-azure-cosmos-db-table-api-data"></a>Guida introduttiva: Creare un'app Java per gestire i dati dell'API Tabella di Azure Cosmos DB

> [!div class="op_single_selector"]
> * [.NET](create-table-dotnet.md)
> * [Java](create-table-java.md)
> * [Node.js](create-table-nodejs.md)
> * [Python](create-table-python.md)
> 

Questa guida introduttiva mostra come usare Java e l'[API Tabelle](table-introduction.md) di Azure Cosmos DB per creare un'app clonando un esempio di GitHub. Illustra come creare un account Azure Cosmos DB e come usare Esplora dati per creare tabelle ed entità nel portale di Azure basato sul Web.

Azure Cosmos DB è il servizio di database di Microsoft multimodello distribuito a livello globale. È possibile creare ed eseguire rapidamente query su database di documenti, coppie chiave-valore e grafi, sfruttando in ognuno dei casi i vantaggi offerti dalle funzionalità di scalabilità orizzontale e distribuzione globale alla base di Azure Cosmos DB. 

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Eseguire anche queste operazioni: 

* [Java Development Kit (JDK) 8](https://aka.ms/azure-jdks)
    * Assicurarsi di impostare la variabile di ambiente JAVA_HOME in modo che faccia riferimento alla cartella di installazione di JDK.
* [Scaricare](https://maven.apache.org/download.cgi) e [installare](https://maven.apache.org/install.html) un archivio binario [Maven](https://maven.apache.org/)
    * In Ubuntu è possibile eseguire `apt-get install maven` per installare Maven.
* [Git](https://www.git-scm.com/)
    * In Ubuntu è possibile eseguire `sudo apt-get install git` per installare Git.

## <a name="create-a-database-account"></a>Creare un account di database

> [!IMPORTANT] 
> Per lavorare con gli SDK dell'API di tabella disponibili a livello generale, è necessario creare un nuovo account dell'API di tabella. Gli account dell'API di tabella creati durante l'anteprima non sono supportati dagli SDK disponibili a livello generale.
>

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="add-a-table"></a>Aggiungere una tabella

[!INCLUDE [cosmos-db-create-table](../../includes/cosmos-db-create-table.md)]

## <a name="add-sample-data"></a>Aggiungere dati di esempio

[!INCLUDE [cosmos-db-create-table-add-sample-data](../../includes/cosmos-db-create-table-add-sample-data.md)]

## <a name="clone-the-sample-application"></a>Clonare l'applicazione di esempio

A questo punto è possibile clonare un'app Table da GitHub, impostare la stringa di connessione ed eseguirla. Come si noterà, è facile usare i dati a livello di codice. 

1. Aprire un prompt dei comandi, creare una nuova cartella denominata git-samples e quindi chiudere il prompt dei comandi.

    ```bash
    md "C:\git-samples"
    ```

2. Aprire una finestra del terminale Git, ad esempio Git Bash, ed eseguire il comando `cd` per passare a una nuova cartella in cui installare l'app di esempio.

    ```bash
    cd "C:\git-samples"
    ```

3. Eseguire il comando seguente per clonare l'archivio di esempio. Questo comando crea una copia dell'app di esempio nel computer in uso.

    ```bash
    git clone https://github.com/Azure-Samples/storage-table-java-getting-started.git 
    ```

## <a name="update-your-connection-string"></a>Aggiornare la stringa di connessione

Tornare ora al portale di Azure per recuperare le informazioni sulla stringa di connessione e copiarle nell'app. Questo consente all'app di comunicare con il database ospitato. 

1. Nel [portale di Azure](https://portal.azure.com/) selezionare **Stringa di connessione**. 

   ![Visualizzare informazioni sulla stringa di connessione nel riquadro Stringa di connessione](./media/create-table-java/cosmos-db-quickstart-connection-string.png)

2. Copiare la STRINGA DI CONNESSIONE PRIMARIA usando il pulsante Copia a destra.

3. Aprire config.properties dalla cartella C:\git-samples\storage-table-java-getting-started\src\main\resources. 

5. Trasformare in commento la riga 1 e rimuovere il commento dalla riga 2. L'aspetto delle prime due righe dovrebbe essere simile al seguente.

    ```xml
    #StorageConnectionString = UseDevelopmentStorage=true
    StorageConnectionString = DefaultEndpointsProtocol=https;AccountName=[ACCOUNTNAME];AccountKey=[ACCOUNTKEY]
    ```

6. Incollare la STRINGA DI CONNESSIONE PRIMARIA dal portale nel valore StorageConnectionString sulla riga 2. 

    > [!IMPORTANT]
    > Se l'endpoint usa documents.azure.com, ovvero si dispone di un account di anteprima, è necessario creare un [nuovo account dell'API di tabella](#create-a-database-account) per lavorare con l'SDK dell'API di tabella disponibile a livello generale.
    >

7. Salvare il file config.properties.

L'app è stata aggiornata con tutte le informazioni necessarie per comunicare con Azure Cosmos DB. 

## <a name="run-the-app"></a>Esecuzione dell'app

1. Nella finestra del terminale Git eseguire il comando `cd` per passare alla cartella storage-table-java-getting-started.

    ```git
    cd "C:\git-samples\storage-table-java-getting-started"
    ```

2. Nella finestra del terminale Git eseguire i comandi seguenti per eseguire l'applicazione Java.

    ```git
    mvn compile exec:java 
    ```

    La finestra della console mostra i dati della tabella aggiunti al nuovo database della tabella in Azure Cosmos DB.

    È ora possibile tornare a Esplora dati e visualizzare, modificare e usare questi nuovi dati, nonché eseguire query su di essi. 

## <a name="review-slas-in-the-azure-portal"></a>Esaminare i contratti di servizio nel portale di Azure

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Pulire le risorse

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Passaggi successivi

In questa guida di avvio rapido si è appreso come creare un account Azure Cosmos DB, come creare una tabella con Esplora dati e come eseguire un'app.  È ora possibile eseguire query sui dati tramite l'API di tabella.  

> [!div class="nextstepaction"]
> [Importare i dati delle tabelle nell'API Tabelle](table-import.md)
