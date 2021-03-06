---
title: REPLICA in linguaggio Azure Cosmos DB query
description: Informazioni sulla replica della funzione di sistema SQL in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 19fcde522c5cb0355e53a5616145f27fada7dad9
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2020
ms.locfileid: "78302186"
---
# <a name="replicate-azure-cosmos-db"></a>REPLICA (Azure Cosmos DB)
 Ripete un valore stringa il numero di volte specificato.
  
## <a name="syntax"></a>Sintassi
  
```sql
REPLICATE(<str_expr>, <num_expr>)
```  
  
## <a name="arguments"></a>Argomenti
  
*str_expr*  
   Espressione stringa.
  
*num_expr*  
   Espressione numerica. Se *num_expr* è negativo o non finito, il risultato è indefinito.
  
## <a name="return-types"></a>Tipi restituiti
  
  Restituisce un'espressione di stringa.
  
## <a name="remarks"></a>Note
  La lunghezza massima del risultato è di 10.000 caratteri, ad esempio (length (*str_expr*) * *num_expr*) < = 10.000.

## <a name="examples"></a>Esempi
  
  Nell'esempio seguente viene illustrato come utilizzare `REPLICATE` in una query.
  
```sql
SELECT REPLICATE("a", 3) AS replicate
```  
  
 Set di risultati:
  
```json
[{"replicate": "aaa"}]
```  

## <a name="remarks"></a>Note

Questa funzione di sistema non utilizzerà l'indice.

## <a name="next-steps"></a>Passaggi successivi

- [Funzioni stringa Azure Cosmos DB](sql-query-string-functions.md)
- [Funzioni di sistema Azure Cosmos DB](sql-query-system-functions.md)
- [Introduzione a Azure Cosmos DB](introduction.md)
