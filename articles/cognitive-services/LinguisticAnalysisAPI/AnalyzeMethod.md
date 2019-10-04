---
title: Metodo Analyze - API Analisi linguistica
titlesuffix: Azure Cognitive Services
description: Come usare il metodo analyze nell'API Analisi linguistica per analizzare input specifici in linguaggio naturale.
services: cognitive-services
author: RichardSunMS
manager: nitinme
ms.service: cognitive-services
ms.subservice: linguistic-analysis
ms.topic: conceptual
ms.date: 12/13/2016
ms.author: lesun
ROBOTS: NOINDEX
ms.openlocfilehash: 02c41e2510fd77f4bb65143faf62737f0985d2b7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "61401187"
---
# <a name="analyze-method"></a>Metodo analyze

> [!IMPORTANT]
> L'anteprima di Analisi linguistica è stata ritirata il 9 agosto 2018. Per l'elaborazione e l'analisi del testo è consigliabile usare i [moduli di analisi del testo di Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/text-analytics).

L'API REST **analyze** API viene usata per analizzare un input specificato in linguaggio naturale.
Che può implicare trovare solo il [frasi e token](Sentences-and-Tokens.md) all'interno di tale input, la ricerca i [tag parti del discorso](POS-tagging.md), o la ricerca del [albero degli elementi costitutivi](Constituency-Parsing.md).
È possibile specificare i risultati desiderati selezionando gli analizzatori pertinenti.
Per ottenere un elenco di tutti gli analizzatori disponibili, esaminare gli **[analizzatori](AnalyzersMethod.md)** .

Si noti che è necessario specificare la lingua della stringa di input.

**Endpoint REST:**
```
https://westus.api.cognitive.microsoft.com/linguistics/v1.0/analyze
```
<br>

## <a name="request-parameters"></a>Parametri della richiesta

Name | Type | Obbligatorio | Descrizione
-----|-------|----------|------------
**language**    | string | Yes | Codice di lingua ISO di due lettere da usare per l'analisi. Ad esempio il codice per l'italiano è "it".
**analyzerIds** | elenco di stringhe | Yes | Elenco di GUID degli analizzatori da applicare. Per altre informazioni, vedere la documentazione sugli analizzatori.
**text**        | string | Yes | Input non elaborato da analizzare. Potrebbe trattarsi di una stringa breve, ad esempio una parola o frase, una frase completa o un paragrafo completo o discorso.

## <a name="response-json"></a>Risposta (JSON)

Una matrice di output di analisi, uno per ogni attributo specificato nella richiesta.

I risultati sono simili ai seguenti:

NOME | Type | Descrizione
-----|------|--------------
analyzerId | string | GUID dell'analizzatore specificato
risultato | object | risultato dell'analizzatore

Si noti che il tipo del risultato dipende dal tipo di analizzatore di input.

### <a name="tokens-response-json"></a>Risposta token (JSON)

NOME | Type | Descrizione
-----|------|-------------
risultato | elenco di oggetti frase | delimitatori di frase identificati all'interno del testo |
result[x].Offset | int | offset di caratteri iniziale di ogni frase |
result[x].Len | int | lunghezza in caratteri di ogni frase |
result[x].Tokens | elenco di oggetti token | limiti dei token identificati all'interno della frase |
result[x].Tokens[y].Offset | int | offset di caratteri iniziale del token |
result[x].Tokens[y].Len | int | lunghezza in caratteri del token |
result[x].Tokens[y].RawToken | string | caratteri all'interno di tale token, prima della normalizzazione |
result[x].Tokens[y].NormalizedToken | string | forma normalizzata del carattere, utilizzabile in modo sicuro in un [albero di analisi](Constituency-Parsing.md); ad esempio, un carattere di parentesi di apertura '(' diventa '-LRB-' |

Input di esempio: 'This is a test. Hello.'
Risposta di esempio JSON:
```json
[
  {
    "Len": 15,
    "Offset": 0,
    "Tokens": [
      {
        "Len": 4,
        "NormalizedToken": "This",
        "Offset": 0,
        "RawToken": "This"
      },
      {
        "Len": 2,
        "NormalizedToken": "is",
        "Offset": 5,
        "RawToken": "is"
      },
      {
        "Len": 1,
        "NormalizedToken": "a",
        "Offset": 8,
        "RawToken": "a"
      },
      {
        "Len": 4,
        "NormalizedToken": "test",
        "Offset": 10,
        "RawToken": "test"
      },
      {
        "Len": 1,
        "NormalizedToken": ".",
        "Offset": 14,
        "RawToken": "."
      }
    ]
  },
  {
    "Len": 6,
    "Offset": 16,
    "Tokens": [
      {
        "Len": 5,
        "NormalizedToken": "Hello",
        "Offset": 16,
        "RawToken": "Hello"
      },
      {
        "Len": 1,
        "NormalizedToken": ".",
        "Offset": 21,
        "RawToken": "."
      }
    ]
  }
]
```


### <a name="pos-tags-response-json"></a>Risposta tag parte del discorso (JSON)

Il risultato è un elenco di elenchi di stringhe.
Per ogni frase esiste un elenco di tag delle parti del discorso, un tag delle parti del discorso per ogni token.
Per trovare il token corrispondente a ciascun tag delle parti del discorso, è opportuno richiedere anche un oggetto di tokenizzazione.

### <a name="constituency-tree-response-json"></a>Risposta albero sintattico (JSON)

Il risultato è un elenco di stringhe, un albero di analisi per ogni frase trovata nell'input.
Gli alberi di analisi sono rappresentati in un formato con parentesi.

## <a name="example"></a>Esempio

`POST /analyze`

Corpo della richiesta: payload JSON
```json
{
  "language": "en",
  "analyzerIds": [
    "4FA79AF1-F22C-408D-98BB-B7D7AEEF7F04",
    "22A6B758-420F-4745-8A3C-46835A67C0D2" ],
  "text": "Hi, Tom! How are you today?"
}
```

Risposta: JSON
```json
[
  {
    "analyzerId": "4FA79AF1-F22C-408D-98BB-B7D7AEEF7F04",
    "result": [ ["NNP",",","NNP","."], ["WRB","VBP","PRP","NN","."] ]
  },
  {
    "analyzerId": "22A6B758-420F-4745-8A3C-46835A67C0D2",
    "result":["(TOP (S (NNP Hi) (, ,) (NNP Tom) (. !)))","(TOP (SBARQ (WHADVP (WRB How)) (SQ (VP (VBP are)) (NP (PRP you)) (NN today) (. ?))))"]
  }
]
```
