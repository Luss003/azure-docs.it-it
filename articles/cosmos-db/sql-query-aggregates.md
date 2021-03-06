---
title: Funzioni di aggregazione in Azure Cosmos DB
description: Informazioni sulla sintassi della funzione di aggregazione SQL, sui tipi di funzioni di aggregazione supportate da Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/05/2020
ms.author: tisande
ms.openlocfilehash: df9700dd51c8915ff28c34cf0a29c2f5e48baa44
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/07/2020
ms.locfileid: "78897822"
---
# <a name="aggregate-functions-in-azure-cosmos-db"></a>Funzioni di aggregazione in Azure Cosmos DB

Le funzioni di aggregazione eseguono un calcolo su un set di valori nella clausola SELECT e restituiscono un singolo valore. Ad esempio, la query seguente restituisce il numero di elementi all'interno del contenitore `Families`:

## <a name="examples"></a>Esempi

```sql
    SELECT COUNT(1)
    FROM Families f
```

I risultati sono:

```json
    [{
        "$1": 2
    }]
```

È inoltre possibile restituire solo il valore scalare dell'aggregazione utilizzando la parola chiave VALUE. Ad esempio, la query seguente restituisce il numero di valori come un singolo numero:

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
```

I risultati sono:

```json
    [ 2 ]
```

È inoltre possibile combinare le aggregazioni con i filtri. Ad esempio, la query seguente restituisce il numero di elementi con lo stato dell'indirizzo di `WA`.

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
    WHERE f.address.state = "WA"
```

I risultati sono:

```json
    [ 1 ]
```

## <a name="types-of-aggregate-functions"></a>Tipi di funzioni di aggregazione

L'API SQL supporta le funzioni di aggregazione seguenti. SUM e AVG operano su valori numerici e conteggio, MIN e MAX funzionano su numeri, stringhe, valori booleani e valori null.

| Funzione | Descrizione |
|-------|-------------|
| COUNT | Restituisce il numero di elementi nell'espressione. |
| SUM   | Restituisce la somma dei valori nell'espressione. |
| MIN   | Restituisce il valore minimo nell'espressione. |
| MAX   | Restituisce il valore massimo dell'espressione. |
| AVG   | Restituisce la media dei valori nell'espressione. |

È anche possibile aggregare i risultati di un'iterazione di matrice.

> [!NOTE]
> Nella Esplora dati del portale di Azure le query di aggregazione possono aggregare risultati parziali su una sola pagina di query. L'SDK genera un singolo valore cumulativo in tutte le pagine. Per eseguire query di aggregazione usando il codice, è necessario .NET SDK 1.12.0, .NET Core SDK 1.1.0 o Java SDK 1.9.5 o versione successiva.

## <a name="next-steps"></a>Passaggi successivi

- [Introduzione a Azure Cosmos DB](introduction.md)
- [Funzioni di sistema](sql-query-system-functions.md)
- [Funzioni definite dall'utente](sql-query-udfs.md)