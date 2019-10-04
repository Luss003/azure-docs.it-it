---
title: Indicizzazione nell'API di Azure Cosmos DB per MongoDB
description: Presenta una panoramica delle funzionalità di indicizzazione nell'API di Azure Cosmos DB per MongoDB.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 12/26/2018
author: sivethe
ms.author: sivethe
ms.openlocfilehash: de037316efa50dd25ea04c370fa0e5878fb52ba1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "60448504"
---
# <a name="indexing-using-azure-cosmos-dbs-api-for-mongodb"></a>Indicizzazione con l'API di Azure Cosmos DB per MongoDB

L'API di Azure Cosmos DB per MongoDB sfrutta le funzionalità di gestione automatica degli indici di Cosmos DB. Di conseguenza, gli utenti hanno accesso ai criteri di indicizzazione predefiniti di Cosmos DB. Se l'utente non ha definito indici o non sono stati eliminati indici, tutti i campi verranno automaticamente indicizzati per impostazione predefinita quando vengono inseriti in una raccolta. Per la maggior parte degli scenari è consigliabile usare i criteri di indicizzazione predefiniti impostati per l'account.

## <a name="dropping-the-default-indexes"></a>Eliminazione degli indici predefiniti

Il comando seguente consente di eliminare gli indici predefiniti per una raccolta ```coll```:

```JavaScript
> db.coll.dropIndexes()
{ "_t" : "DropIndexesResponse", "ok" : 1, "nIndexesWas" : 3 }
```

## <a name="creating-compound-indexes"></a>Creazione di indici composti

Gli indici composti contengono riferimenti a più campi di un documento. Dal punto di vista logico equivalgono alla creazione di più indici per ogni campo. Per sfruttare le ottimizzazioni fornite dalle tecniche di indicizzazione di Cosmos DB, è consigliabile creare più indici anziché un singolo indice composto non univoco.

## <a name="creating-unique-indexes"></a>Creazione di indici univoci

Gli [indici univoci](unique-keys.md) sono utili per impedire che due o più documenti contengano lo stesso valore per i campi indicizzati. 
>[!important] 
> Attualmente, gli indici univoci possono essere creati solo quando la raccolta è vuota, ovvero non contiene alcun documento. 

Il comando seguente consente di creare un indice univoco sul campo "student_id":

```JavaScript
globaldb:PRIMARY> db.coll.createIndex( { "student_id" : 1 }, {unique:true} ) 
{
        "_t" : "CreateIndexesResponse",
        "ok" : 1,
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 1,
        "numIndexesAfter" : 4
}
```

Per le raccolte partizionate, in base al comportamento di MongoDB è necessario fornire la chiave di partizione per creare un indice univoco. In altre parole, tutti gli indici univoci in una raccolta partizionata sono indici composti in cui uno dei campi è la chiave di partizione.

I comandi seguenti creano una raccolta partizionata ```coll``` (la chiave di partizione è ```university```) con un indice univoco sui campi student_id e university:

```JavaScript
globaldb:PRIMARY> db.runCommand({shardCollection: db.coll._fullName, key: { university: "hashed"}});
{
        "_t" : "ShardCollectionResponse",
        "ok" : 1,
        "collectionsharded" : "test.coll"
}
globaldb:PRIMARY> db.coll.createIndex( { "student_id" : 1, "university" : 1 }, {unique:true})
{
        "_t" : "CreateIndexesResponse",
        "ok" : 1,
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 3,
        "numIndexesAfter" : 4
}
```

Nell'esempio precedente, se la clausola ```"university":1``` venisse omessa verrebbe restituito un errore con il messaggio seguente:

```"cannot create unique index over {student_id : 1.0} with shard key pattern { university : 1.0 }"```

## <a name="ttl-indexes"></a>Indici TTL

Per abilitare la scadenza dei documenti in una determinata raccolta, è necessario creare un ["indice TTL" (time-to-live)](../cosmos-db/time-to-live.md). Un indice TTL è un indice sul campo _ts con un valore "expireAfterSeconds".
 
Esempio:
```JavaScript
globaldb:PRIMARY> db.coll.createIndex({"_ts":1}, {expireAfterSeconds: 10})
```

Il comando precedente causerà l'eliminazione di tutti i documenti della raccolta ```db.coll``` che non sono stati modificati negli ultimi 10 secondi. 
 
> [!NOTE]
> **_ts** è un campo specifico di Cosmos DB e non è accessibile dai client di MongoDB. Si tratta di una proprietà di sistema riservata che contiene il timestamp dell'ultima modifica del documento.
>

## <a name="migrating-collections-with-indexes"></a>Migrazione di raccolte con indici

Attualmente, gli indici univoci possono essere creati solo quando la raccolta non contiene alcun documento. Gli strumenti di migrazione più diffusi di MongoDB provano a creare gli indici univoci dopo l'importazione dei dati. Per aggirare questo problema, è consigliabile creare manualmente le raccolte e gli indici univoci corrispondenti, invece di far eseguire l'operazione allo strumento di migrazione. Per ```mongorestore``` questo comportamento si ottiene usando il flag --noIndexRestore nella riga di comando.

## <a name="next-steps"></a>Passaggi successivi
* [Indicizzazione in Azure Cosmos DB](../cosmos-db/index-policy.md)
* [Impostare la scadenza automatica dei dati in Azure Cosmos DB con la durata (TTL)](../cosmos-db/time-to-live.md)
