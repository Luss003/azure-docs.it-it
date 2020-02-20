---
title: Modificare i flussi nell'API Azure Cosmos DB per MongoDB
description: Informazioni su come usare i flussi di modifiche n Azure Cosmos DB API per MongoDB per ottenere le modifiche apportate ai dati.
author: srchi
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 11/16/2019
ms.author: srchi
ms.openlocfilehash: ec1ec1a8a80953f8988355341ee7128bd29b982d
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77467778"
---
# <a name="change-streams-in-azure-cosmos-dbs-api-for-mongodb"></a>Modificare i flussi nell'API Azure Cosmos DB per MongoDB

Il supporto del [feed delle modifiche](change-feed.md) nell'API Azure Cosmos DB per MongoDB è disponibile tramite l'API Change Streams. Usando l'API Change Streams, le applicazioni possono ottenere le modifiche apportate alla raccolta o agli elementi in una singola partizione. In seguito è possibile eseguire ulteriori azioni in base ai risultati. Le modifiche apportate agli elementi nella raccolta vengono acquisite nell'ordine in cui sono state modificate e il tipo di ordinamento è garantito per ogni chiave di partizione.

> [!NOTE]
> Per usare i flussi delle modifiche, creare l'account con la versione 3,6 dell'API Azure Cosmos DB per MongoDB o una versione successiva. Se si eseguono gli esempi del flusso di modifiche rispetto a una versione precedente, è possibile che venga visualizzato l'errore `Unrecognized pipeline stage name: $changeStream`. 

Nell'esempio seguente viene illustrato come ottenere i flussi delle modifiche in tutti gli elementi della raccolta. Questo esempio crea un cursore per controllare gli elementi quando vengono inseriti, aggiornati o sostituiti. Per ottenere i flussi delle modifiche sono necessari la fase $match, la fase $project e l'opzione fullDocument. Il monitoraggio delle operazioni di eliminazione mediante flussi di modifiche non è attualmente supportato. Come soluzione alternativa, è possibile aggiungere un marcatore Soft sugli elementi da eliminare. Ad esempio, è possibile aggiungere un attributo nell'elemento denominato "Deleted" e impostarlo su "true" e impostare una durata (TTL) per l'elemento, in modo che sia possibile eliminarlo automaticamente e tenerne traccia.

```javascript
var cursor = db.coll.watch(
    [
        { $match: { "operationType": { $in: ["insert", "update", "replace"] } } },
        { $project: { "_id": 1, "fullDocument": 1, "ns": 1, "documentKey": 1 } }
    ],
    { fullDocument: "updateLookup" });

while (!cursor.isExhausted()) {
    if (cursor.hasNext()) {
        printjson(cursor.next());
    }
}
```

Nell'esempio seguente viene illustrato come ottenere le modifiche agli elementi in una singola partizione. Questo esempio Mostra come ottenere le modifiche apportate agli elementi con una chiave di partizione uguale a "a" e il valore della chiave di partizione uguale a "1".

```javascript
var cursor = db.coll.watch(
    [
        { 
            $match: { 
                $and: [
                    { "fullDocument.a": 1 }, 
                    { "operationType": { $in: ["insert", "update", "replace"] } }
                ]
            }
        },
        { $project: { "_id": 1, "fullDocument": 1, "ns": 1, "documentKey": 1} }
    ],
    { fullDocument: "updateLookup" });

```

## <a name="current-limitations"></a>Limitazioni correnti

Quando si usano i flussi di modifiche, si applicano le limitazioni seguenti:

* Le proprietà `operationType` e `updateDescription` non sono ancora supportate nel documento di output.
* I tipi di operazioni `insert`, `update`e `replace` sono attualmente supportati. L'operazione di eliminazione o altri eventi non sono ancora supportati.

A causa di queste limitazioni, le opzioni $match stage, $project stage e fullDocument sono obbligatorie, come illustrato negli esempi precedenti.

## <a name="error-handling"></a>Gestione degli errori

Quando si usano i flussi di modifiche, sono supportati i codici di errore e i messaggi seguenti:

* **Codice di errore HTTP 429** : quando il flusso di modifiche è limitato, viene restituita una pagina vuota.

* **NamespaceNotFound (OperationType Invalidate)** : se si esegue Change Stream sulla raccolta che non esiste o se la raccolta viene eliminata, viene restituito un errore di `NamespaceNotFound`. Poiché il `operationType` proprietà non può essere restituito nel documento di output, invece dell'errore `operationType Invalidate`, viene restituito l'errore `NamespaceNotFound`.

## <a name="next-steps"></a>Passaggi successivi

* [Usare la durata (TTL) per la scadenza automatica dei dati nell'API Azure Cosmos DB per MongoDB](mongodb-time-to-live.md)
* [Indicizzazione nell'API Azure Cosmos DB per MongoDB](mongodb-indexing.md)
