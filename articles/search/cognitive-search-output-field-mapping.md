---
title: Eseguire il mapping di campi di input arricchiti con intelligenza artificiale ai campi di output
titleSuffix: Azure Cognitive Search
description: Estrarre e arricchire i campi dei dati di origine ed eseguire il mapping ai campi di output in un indice di ricerca cognitiva di Azure.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 77f378f4e0c4a8e4827523e244f7b18c2a9ba336
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2019
ms.locfileid: "72792115"
---
# <a name="how-to-map-ai-enriched-fields-to-a-searchable-index"></a>Come eseguire il mapping di campi arricchiti di intelligenza artificiale a un indice ricercabile

In questo articolo si apprenderà come eseguire il mapping dei campi di input migliorati ai campi di output in un indice ricercabile. Dopo avere [definito un set di competenze](cognitive-search-defining-skillset.md), è necessario eseguire il mapping dei campi di output di qualsiasi competenza che contribuisce direttamente ai valori di un determinato campo nell'indice di ricerca. Per spostare il contenuto dai documenti migliorati all'indice è necessario eseguire i mapping.


## <a name="use-outputfieldmappings"></a>Usare outputFieldMappings
Per eseguire il mapping dei campi, aggiungere `outputFieldMappings` alla definizione dell'indicizzatore, come illustrato di seguito:

```http
PUT https://[servicename].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
api-key: [admin key]
Content-Type: application/json
```

Il corpo della richiesta è strutturato nel modo seguente:

```json
{
    "name": "myIndexer",
    "dataSourceName": "myDataSource",
    "targetIndexName": "myIndex",
    "skillsetName": "myFirstSkillSet",
    "fieldMappings": [
        {
            "sourceFieldName": "metadata_storage_path",
            "targetFieldName": "id",
            "mappingFunction": {
                "name": "base64Encode"
            }
        }
    ],
    "outputFieldMappings": [
        {
            "sourceFieldName": "/document/content/organizations/*/description",
            "targetFieldName": "descriptions"
        },
        {
            "sourceFieldName": "/document/content/organizations",
            "targetFieldName": "orgNames"
        },
        {
            "sourceFieldName": "/document/content/sentiment",
            "targetFieldName": "sentiment"
        }
    ]
}
```
Per ogni mapping di campi di output, impostare il nome del campo migliorato (sourceFieldName) e il nome del campo referenziato nell'indice (targetFieldName).

Il percorso in sourceFieldName può rappresentare uno o più elementi. Nell'esempio precedente, ```/document/content/sentiment``` rappresenta un valore numerico singolo, mentre ```/document/content/organizations/*/description``` rappresenta più descrizioni di organizzazioni. Nei casi in cui sono presenti più elementi, questi vengono resi "flat" in una matrice che li contiene tutti. Più in concreto, nell'esempio ```/document/content/organizations/*/description``` i dati nel campo delle *descrizioni* dovrebbero essere come una matrice flat prima di essere indicizzati:

```
 ["Microsoft is a company in Seattle","LinkedIn's office is in San Francisco"]
```
## <a name="next-steps"></a>Passaggi successivi
Dopo che è stato eseguito il mapping dei campi migliorati ai campi ricercabili, è possibile impostare gli attributi per ogni campo ricercabile [come parte della definizione dell'indice](search-what-is-an-index.md).

Per altre informazioni sul mapping dei campi, vedere [mapping dei campi in Azure ricerca cognitiva Indexers](search-indexer-field-mappings.md).
