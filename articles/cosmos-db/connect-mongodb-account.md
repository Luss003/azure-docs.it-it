---
title: Connettere un'applicazione MongoDB ad Azure Cosmos DB
description: Informazioni su come connettere un'app MongoDB a Azure Cosmos DB ottenendo la stringa di connessione da portale di Azure
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 12/02/2019
ms.reviewer: sngun
ms.openlocfilehash: 0f2cd1b7228f2cc9cadb84232222f658a512a81f
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2020
ms.locfileid: "79246851"
---
# <a name="connect-a-mongodb-application-to-azure-cosmos-db"></a>Connettere un'applicazione MongoDB ad Azure Cosmos DB
Informazioni su come connettere un'app MongoDB ad Azure Cosmos DB usando una stringa di connessione MongoDB. È quindi possibile usare un database di Azure Cosmos DB come archivio dati per l'app MongoDB. 

Questa esercitazione illustra due modi per recuperare le informazioni della stringa di connessione:

- [Metodo di avvio rapido](#QuickstartConnection), per l'uso con i driver .NET, Node.js, MongoDB Shell, Java e Python
- [Metodo basato su stringa di connessione personalizzata](#GetCustomConnection), da usare con altri driver

## <a name="prerequisites"></a>Prerequisiti

- Un account Azure. Se non si ha un account Azure, creare un [account Azure gratuito](https://azure.microsoft.com/free/) ora. 
- Un account Cosmos. Per istruzioni, vedere [Creare un'app Web usando l'API Azure Cosmos DB per MongoDB e .NET SDK](create-mongodb-dotnet.md).

## <a id="QuickstartConnection"></a>Ottenere la stringa di connessione MongoDB usando il metodo di avvio rapido
1. In un browser Internet accedere al [portale di Azure](https://portal.azure.com).
2. Nel pannello **Azure Cosmos DB** selezionare l'API. 
3. Nel riquadro sinistro del pannello dell'account fare clic su **Avvio rapido**. 
4. Scegliere la piattaforma ( **.NET**, **Node.js**, **MongoDB Shell**, **Java**, **Python**). Se il driver o lo strumento non compare nell'elenco, ricordare che altri frammenti di codice di connessione vengono continuamente documentati. Commentare di seguito ciò che si vorrebbe vedere. Per informazioni su come realizzare la propria connessione, leggere [Ottenere informazioni sulla stringa di connessione dell'account](#GetCustomConnection).
5. Copiare e incollare il frammento di codice nell'app MongoDB.

    ![Pannello di avvio rapido](./media/connect-mongodb-account/QuickStartBlade.png)

## <a id="GetCustomConnection"></a> Ottenere la stringa di connessione MongoDB da personalizzare
1. In un browser Internet accedere al [portale di Azure](https://portal.azure.com).
2. Nel pannello **Azure Cosmos DB** selezionare l'API. 
3. Nel riquadro sinistro del pannello dell'account, fare clic su **Stringa di connessione**. 
4. Viene aperto il pannello **Stringa di connessione**. Contiene tutte le informazioni necessarie per connettersi all'account usando un driver per MongoDB, inclusa una stringa di connessione precostruita.

    ![Pannello Stringa di connessione](./media/connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a>Requisiti della stringa di connessione
> [!Important]
> Azure Cosmos DB presenta standard e requisiti di sicurezza restrittivi. Gli account Azure Cosmos DB richiedono l'autenticazione e la comunicazione sicura tramite *SSL*. 
>
>

Azure Cosmos DB supporta il formato URI della stringa di connessione di MongoDB standard, con un paio di requisiti specifici: gli account Azure Cosmos DB richiedono l'autenticazione e la comunicazione sicura tramite SSL. Il formato della stringa di connessione sarà quindi:

    mongodb://username:password@host:port/[database]?ssl=true

I valori di questa stringa sono disponibili nel pannello **Stringa di connessione** mostrato in precedenza:

* Username (obbligatorio): nome dell'account Cosmos.
* Password (obbligatoria): password dell'account Cosmos.
* Host (obbligatorio): nome di dominio completo dell'account Cosmos.
* Porta (obbligatorio): 10255.
* Database (facoltativo): il database utilizzato dalla connessione. Se viene specificato alcun database, il database predefinito è "test".
* ssl=true (obbligatorio)

Si consideri l'account mostrato nel pannello **Stringa di connessione**. Una stringa di connessione valida è la seguente:

    mongodb://contoso123:0Fc3IolnL12312asdfawejunASDF@asdfYXX2t8a97kghVcUzcDv98hawelufhawefafnoQRGwNj2nMPL1Y9qsIr9Srdw==@contoso123.documents.azure.com:10255/mydatabase?ssl=true

## <a name="next-steps"></a>Passaggi successivi

- Informazioni su come [usare Studio 3T](mongodb-mongochef.md) con l'API di Azure Cosmos DB per MongoDB.
- Informazioni su come [usare Robo 3T](mongodb-robomongo.md) con l'API di Azure Cosmos DB per MongoDB.
- Esplorare gli [esempi](mongodb-samples.md) di MongoDB con l'API di Azure Cosmos DB per MongoDB.
