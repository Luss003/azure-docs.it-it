---
title: Clausola WHERE in Azure Cosmos DB
description: Informazioni sulla clausola WHERE SQL per Azure Cosmos DB
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: tisande
ms.openlocfilehash: 31653b598f0f3a79bf7f9c09231b1d111f167a16
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/04/2020
ms.locfileid: "76982230"
---
# <a name="where-clause-in-azure-cosmos-db"></a>Clausola WHERE in Azure Cosmos DB

La clausola WHERE facoltativa (`WHERE <filter_condition>`) specifica le condizioni che devono essere soddisfatte dagli elementi JSON di origine affinché la query li includa nei risultati. Un elemento JSON deve valutare le condizioni specificate per `true` da considerare per il risultato. Il livello di indice utilizza la clausola WHERE per determinare il subset più piccolo di elementi di origine che può far parte del risultato.
  
## <a name="syntax"></a>Sintassi
  
```sql  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
## <a name="arguments"></a>Argomenti

- `<filter_condition>`  
  
   Specifica la condizione che deve essere soddisfatta per i documenti da restituire.  
  
- `<scalar_expression>`  
  
   Espressione che rappresenta il valore da calcolare. Per informazioni dettagliate, vedere [espressioni scalari](sql-query-scalar-expressions.md) .  
  

## <a name="remarks"></a>Osservazioni
  
  Affinché il documento venga restituito, un'espressione specificata come condizione di filtro deve restituire true. Solo un valore booleano true soddisferà la condizione, i valori non definiti, null, false, numero, matrice o oggetto non soddisfano la condizione. 

## <a name="examples"></a>Esempi

La query seguente richiede elementi che contengono una proprietà `id` il cui valore è `AndersenFamily`. Esclude tutti gli elementi che non dispongono di una proprietà `id` o il cui valore non corrisponde `AndersenFamily`.

```sql
    SELECT f.address
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

I risultati sono:

```json
    [{
      "address": {
        "state": "WA",
        "county": "King",
        "city": "Seattle"
      }
    }]
```

### <a name="scalar-expressions-in-the-where-clause"></a>Espressioni scalari nella clausola WHERE

Nell'esempio precedente è stata illustrata una semplice query di uguaglianza. L'API SQL supporta inoltre varie [espressioni scalari](sql-query-scalar-expressions.md). Quelle di uso più comune sono le espressioni binarie e unarie. Anche i riferimenti di proprietà dell'oggetto JSON sono espressioni valide.

È possibile utilizzare gli operatori binari supportati seguenti:  

|**Tipo di operatore**  | **Valori** |
|---------|---------|
|Aritmetico | +,-,*,/,% |
|Bit per bit    | \|, &, ^, <<, >>, >>> (spostamento a destra riempimento zero) |
|Logico    | AND, OR, NOT      |
|Confronto | =, !=, &lt;, &gt;, &lt;=, &gt;=, <> |
|string     |  \|\| (concatenazione) |

Nelle query seguenti vengono utilizzati operatori binari:

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5    -- matching grades == 5
```

È anche possibile usare gli operatori unari +,-, ~ e non nelle query, come illustrato negli esempi seguenti:

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5
```

È anche possibile usare i riferimenti alle proprietà nelle query. Ad esempio, `SELECT * FROM Families f WHERE f.isRegistered` restituisce l'elemento JSON che contiene la proprietà `isRegistered` con valore uguale a `true`. Qualsiasi altro valore, ad esempio `false`, `null`, `Undefined`, `<number>`, `<string>`, `<object>`o `<array>`, esclude l'elemento dal risultato.

## <a name="next-steps"></a>Passaggi successivi

- [Introduzione](sql-query-getting-started.md)
- [Parola chiave IN](sql-query-keywords.md#in)
- [Clausola FROM](sql-query-from.md)