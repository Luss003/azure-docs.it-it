---
title: Abilità cognitiva rilevamento informazioni personali (anteprima)
titleSuffix: Azure Cognitive Search
description: Estrai e maschera le informazioni personali dal testo in una pipeline di arricchimento in Azure ricerca cognitiva. Questa competenza è attualmente disponibile in anteprima pubblica.
manager: nitinme
author: careyjmac
ms.author: chalton
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 1/27/2020
ms.openlocfilehash: 7011d971ddb1888b312173a42ecd4a50f0954395
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2020
ms.locfileid: "76849045"
---
#   <a name="pii-detection-cognitive-skill"></a>Competenze cognitive per il rilevamento delle informazioni personali

> [!IMPORTANT] 
> Questa competenza è attualmente disponibile in anteprima pubblica. La funzionalità di anteprima viene fornita senza contratto di servizio e non è consigliata per i carichi di lavoro di produzione. Per altre informazioni, vedere [Condizioni supplementari per l'utilizzo delle anteprime di Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Attualmente non è disponibile alcun portale o supporto per .NET SDK.

L'abilità di rilevamento delle informazioni **personali** estrae informazioni personali da un testo di input e offre la possibilità di mascherarle da tale testo in diversi modi. Questa competenza usa i modelli di Machine Learning forniti da [Analisi del testo](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) in Servizi cognitivi.

> [!NOTE]
> Se si espande l'ambito aumentando la frequenza di elaborazione, aggiungendo più documenti oppure aggiungendo altri algoritmi di intelligenza artificiale, sarà necessario [collegare una risorsa fatturabile di Servizi cognitivi](cognitive-search-attach-cognitive-services.md). Gli addebiti si accumulano quando si chiamano le API in Servizi cognitivi e per l'estrazione di immagini come parte della fase di cracking dei documenti in Ricerca cognitiva di Azure. Non sono previsti addebiti per l'estrazione di testo dai documenti.
>
> L'esecuzione delle competenze predefinite viene addebitata secondo gli attuali [prezzi con pagamento in base al consumo dei Servizi cognitivi](https://azure.microsoft.com/pricing/details/cognitive-services/). I prezzi per l'estrazione di immagini sono descritti nella [pagina dei prezzi di Ricerca cognitiva di Azure](https://go.microsoft.com/fwlink/?linkid=2042400).


## <a name="odatatype"></a>@odata.type  
Microsoft. Skills. Text. PIIDetectionSkill

## <a name="data-limits"></a>Limiti dei dati
La dimensione massima di un record deve essere di 50.000 caratteri misurata da [`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length). Se è necessario suddividere i dati prima di inviarli alla competenza, provare a usare la [capacità di suddivisione del testo](cognitive-search-skill-textsplit.md).

## <a name="skill-parameters"></a>Parametri della competenza

I parametri fanno distinzione tra maiuscole e minuscole e sono tutti facoltativi.

| Nome parametro     | Description |
|--------------------|-------------|
| defaultLanguageCode | Codice lingua del testo di input. Per il momento è supportata solo `en`. |
| minimumPrecision | Valore compreso tra 0,0 e 1,0. Se il Punteggio di confidenza (nell'output del `piiEntities`) è inferiore al valore impostato `minimumPrecision`, l'entità non viene restituita né mascherata. Il valore predefinito è 0,0. |
| maskingMode | Parametro che fornisce vari modi per mascherare le informazioni personali rilevate nel testo di input. Sono supportate le opzioni seguenti: <ul><li>`none` (impostazione predefinita): ciò significa che non verrà eseguita alcuna maschera e l'output del `maskedText` non verrà restituito. </li><li> `redact`: questa opzione consente di rimuovere le entità rilevate dal testo di input e non di sostituirle con alcun elemento. Si noti che in questo caso, l'offset nell'output del `piiEntities` sarà correlato al testo originale e non al testo mascherato. </li><li> `replace`: questa opzione consente di sostituire le entità rilevate con il carattere specificato nel parametro di `maskingCharacter`.  Il carattere verrà ripetuto fino alla lunghezza dell'entità rilevata, in modo che gli offset corrispondano correttamente sia al testo di input sia alla `maskedText`di output.</li></ul> |
| maskingCharacter | Il carattere che verrà usato per mascherare il testo se il parametro `maskingMode` è impostato su `replace`. Sono supportate le opzioni seguenti: `*` (impostazione predefinita), `#`, `X`. Questo parametro può essere `null` solo se `maskingMode` non è impostato su `replace`. |


## <a name="skill-inputs"></a>Input competenze

| Nome input      | Description                   |
|---------------|-------------------------------|
| languageCode  | Facoltativa. Il valore predefinito è `en`.  |
| text          | Testo da analizzare.          |

## <a name="skill-outputs"></a>Output competenze

| Nome output     | Description                   |
|---------------|-------------------------------|
| piiEntities | Una matrice di tipi complessi, che contiene i campi seguenti: <ul><li>testo (informazioni personali effettive estratte)</li> <li>type</li><li>Sottotipo</li><li>Score (valore più elevato significa che è più probabile che sia un'entità reale)</li><li>offset (nel testo di input)</li><li>length</li></ul> </br> [I tipi e i sottotipi possibili sono disponibili qui.](https://docs.microsoft.com/azure/cognitive-services/text-analytics/named-entity-types?tabs=personal) |
| maskedText | Se `maskingMode` è impostato su un valore diverso da `none`, questo output sarà il risultato della stringa della maschera eseguita sul testo di input, come descritto dal `maskingMode`selezionato.  Se `maskingMode` è impostato su `none`, l'output non sarà presente. |

##  <a name="sample-definition"></a>Definizione di esempio

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.PIIDetectionSkill",
    "defaultLanguageCode": "en",
    "minimumPrecision": 0.5,
    "maskingMode": "replace",
    "maskingCharacter": "*",
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "piiEntities"
      },
      {
        "name": "maskedText"
      }
    ]
  }
```
##  <a name="sample-input"></a>Input di esempio

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "Microsoft employee with ssn 859-98-0987 is using our awesome API's."
           }
      }
    ]
}
```

##  <a name="sample-output"></a>Output di esempio

```json
{
  "values": [
    {
      "recordId": "1",
      "data" : 
      {
        "piiEntities":[ 
           { 
              "text":"859-98-0987",
              "type":"U.S. Social Security Number (SSN)",
              "subtype":"",
              "offset":28,
              "length":11,
              "score":0.65
           }
        ],
        "maskedText": "Microsoft employee with ssn *********** is using our awesome API's."
      }
    }
  ]
}
```


## <a name="error-and-warning-cases"></a>Casi di errore e di avviso
Se il codice della lingua per il documento non è supportato, viene restituito un avviso e non viene estratta alcuna entità.
Se il testo è vuoto, verrà generato un avviso.
Se il testo contiene più di 50.000 caratteri, verranno analizzati solo i primi 50.000 caratteri e verrà generato un avviso.

Se la competenza restituisce un avviso, il `maskedText` di output può essere vuoto.  Ciò significa che se si prevede che l'output esista per l'input in competenze successive, non funzionerà come previsto. Tenere presente questo aspetto quando si scrive la definizione di competenze.

## <a name="see-also"></a>Vedi anche

+ [Competenze predefinite](cognitive-search-predefined-skills.md)
+ [Come definire un set di competenze](cognitive-search-defining-skillset.md)
