---
title: Usare il riconoscimento di entità con l'API Analisi del testo
titleSuffix: Azure Cognitive Services
description: Informazioni su come riconoscere le entità usando l'API REST Analisi del testo.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 07/30/2019
ms.author: aahi
ms.openlocfilehash: 642b21624ce3ffc993d5f29a413845044d703fd7
ms.sourcegitcommit: 909ca340773b7b6db87d3fb60d1978136d2a96b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2019
ms.locfileid: "70984274"
---
# <a name="how-to-use-named-entity-recognition-in-text-analytics"></a>Come usare il riconoscimento delle entità denominate in Analisi del testo

L' [API di riconoscimento entità denominata](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634) accetta un testo non strutturato e per ogni documento JSON restituisce un elenco di entità ambiguità con collegamenti ad altre informazioni sul Web (Wikipedia e Bing).

## <a name="entity-linking-and-named-entity-recognition"></a>Collegamento di entità e riconoscimento di entità denominate

L'endpoint analisi del testo `entities` ' supporta sia il riconoscimento delle entità denominate (ner) che il collegamento di entità.

### <a name="entity-linking"></a>Collegamento delle entità
Per collegamento delle entità si intende la capacità di identificare senza ambiguità l'identità di un'entità trovata nel testo, ad esempio per determinare se "Marte" viene usato per indicare il pianeta o il dio romano della guerra. Questo processo richiede la presenza di una knowledge base a cui sono collegate le entità riconosciute. Wikipedia è usata come knowledge base per l'endpoint `entities` di Analisi del testo.

### <a name="named-entity-recognition-ner"></a>Riconoscimento delle entità denominate (NER)
Il riconoscimento delle entità denominate (NER) è la possibilità di identificare le diverse entità in formato testo e classificarle in classi già definite. Le classi di entità supportate sono elencate di seguito.

In Analisi del testo [versione 2,1](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)sono disponibili sia il collegamento di entità sia il riconoscimento delle entità denominate (ner) per diverse lingue. Per ulteriori informazioni, vedere l'articolo [supporto della lingua](../language-support.md#sentiment-analysis-key-phrase-extraction-and-named-entity-recognition) .

### <a name="language-support"></a>Supporto per la lingua

L'uso del collegamento delle entità in varie lingue richiede l'uso di una corrispondente knowledge base in ciascuna lingua. Il collegamento delle entità in Analisi del testo significa che ogni lingua supportata dall'endpoint `entities` si collegherà al corpus Wikipedia corrispondente in tale lingua. Poiché le dimensioni del corpora variano tra le lingue, è prevista anche una variazione dell'entità collegata al richiamo della funzionalità.

## <a name="supported-types-for-named-entity-recognition"></a>Tipi supportati per il riconoscimento delle entità denominate

| Type  | SubType | Esempio |
|:-----------   |:------------- |:---------|
| Persona        | N/D\*         | "Jeff", "Bill Gates"     |
| Location      | N/D\*         | "Redmond, Washington", "Parigi"  |
| Azienda  | N/D\*         | "Microsoft"   |
| Quantità      | Numero        | "6", "sei"     |
| Quantità      | Percentuale    | "50%", "cinquanta percento"|
| Quantità      | Ordinale       | "2°", "secondo"     |
| Quantità      | Tempo di risoluzione           | "90 giorni", "30 anni"    |
| Quantità      | Currency      | "$ 10,99"     |
| Quantità      | Dimensione     | "10 miglia", "40 cm"     |
| Quantità      | Temperatura   | "32 gradi"    |
| DateTime      | N/D\*         | "4 febbraio 2012 18:30"      |
| DateTime      | Date          | "2 maggio 2017", "02/05/2017"   |
| DateTime      | Time          | "8", "8:00"  |
| DateTime      | Intervallo di date     | "Dal 2 maggio al 5 maggio"    |
| DateTime      | Intervallo orario     | "dalle 18 alle 19"     |
| DateTime      | Duration      | "1 minuto e 45 secondi"   |
| DateTime      | Configurazione           | "ogni martedì"     |
| URL           | N/D\*         | "https:\//www.bing.com"    |
| Email         | N/D\*         | "support@contoso.com" |

\*A seconda delle entità immesse ed estratte, alcune entità possono omettere `SubType`.  Tutti i tipi di entità supportati elencati sono disponibili solo per le lingue inglese, cinese semplificato, francese, tedesco e spagnolo.



## <a name="preparation"></a>Operazioni preliminari

È necessario disporre di documenti JSON nel formato seguente: ID, testo, lingua

Per le lingue attualmente supportate, vedere [questo elenco](../text-analytics-supported-languages.md).

Le dimensioni dei documenti devono essere inferiori a 5.120 caratteri per documento e ogni raccolta può contenere fino a 1.000 elementi (ID). La raccolta viene inviata nel corpo della richiesta. L'esempio seguente illustra il contenuto che è possibile inviare al collegamento delle entità.

```json
    {
        "documents": [
            {
                "id": "1",
                "language": "en",
                "text": "Jeff bought three dozen eggs because there was a 50% discount."
            },
            {
                "id": "2",
                "language": "en",
                "text": "The Great Depression began in 1929. By 1933, the GDP in America fell by 25%."
            }
        ]
    }
```

## <a name="step-1-structure-the-request"></a>Passaggio 1: Strutturare la richiesta

I dettagli sulla definizione della richiesta sono reperibili in [How to call the Text Analytics API](text-analytics-how-to-call-api.md) (Come chiamare l'API Analisi del testo). Per comodità si ridefiniscono i punti seguenti:

+ Creare una richiesta **POST**. Esaminare la documentazione dell'API per la richiesta: [API di entità](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)

+ Impostare l'endpoint HTTP per l'estrazione delle frasi chiave usando una risorsa di Analisi del testo in Azure oppure un'istanza di un [contenitore di Analisi del testo](text-analytics-how-to-install-containers.md). È necessario includere `/text/analytics/v2.1/entities`. Ad esempio: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v2.1/entities`.

+ Impostare un'intestazione della richiesta per includere [la chiave di accesso](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) per analisi del testo operazioni.

+ Nel corpo della richiesta specificare la raccolta di documenti JSON preparata per l'analisi.

> [!Tip]
> Usare [Postman](text-analytics-how-to-call-api.md) oppure aprire la **console di test dell'API** nella [documentazione](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634) per strutturare una richiesta e inviarla tramite POST al servizio.

## <a name="step-2-post-the-request"></a>Passaggio 2: Pubblicare la richiesta

Al momento della ricezione della richiesta viene eseguita l'analisi. Vedere la sezione relativa ai [limiti dei data](../overview.md#data-limits) nella panoramica per informazioni sulle dimensioni e il numero di richieste che è possibile inviare al minuto e al secondo.

Tenere presente che il servizio è senza stato. Nessun dato viene archiviato nell'account. I risultati vengono restituiti immediatamente nella risposta.

## <a name="step-3-view-results"></a>Passaggio 3: Visualizza risultati

Tutte le richieste POST restituiscono una risposta JSON formattata con gli ID e le proprietà rilevate.

L'output viene restituito immediatamente. Si possono trasmettere i risultati in un'applicazione che accetta JSON o salvare l'output in un file nel sistema locale e quindi importarlo in un'applicazione che consente di ordinare, cercare e modificare i dati.

Un esempio dell'output del collegamento delle entità è il seguente:

```json
    {
        "Documents": [
            {
                "Id": "1",
                "Entities": [
                    {
                        "Name": "Jeff",
                        "Matches": [
                            {
                                "Text": "Jeff",
                                "Offset": 0,
                                "Length": 4
                            }
                        ],
                        "Type": "Person"
                    },
                    {
                        "Name": "three dozen",
                        "Matches": [
                            {
                                "Text": "three dozen",
                                "Offset": 12,
                                "Length": 11
                            }
                        ],
                        "Type": "Quantity",
                        "SubType": "Number"
                    },
                    {
                        "Name": "50",
                        "Matches": [
                            {
                                "Text": "50",
                                "Offset": 49,
                                "Length": 2
                            }
                        ],
                        "Type": "Quantity",
                        "SubType": "Number"
                    },
                    {
                        "Name": "50%",
                        "Matches": [
                            {
                                "Text": "50%",
                                "Offset": 49,
                                "Length": 3
                            }
                        ],
                        "Type": "Quantity",
                        "SubType": "Percentage"
                    }
                ]
            },
            {
                "Id": "2",
                "Entities": [
                    {
                        "Name": "Great Depression",
                        "Matches": [
                            {
                                "Text": "The Great Depression",
                                "Offset": 0,
                                "Length": 20
                            }
                        ],
                        "WikipediaLanguage": "en",
                        "WikipediaId": "Great Depression",
                        "WikipediaUrl": "https://en.wikipedia.org/wiki/Great_Depression",
                        "BingId": "d9364681-98ad-1a66-f869-a3f1c8ae8ef8"
                    },
                    {
                        "Name": "1929",
                        "Matches": [
                            {
                                "Text": "1929",
                                "Offset": 30,
                                "Length": 4
                            }
                        ],
                        "Type": "DateTime",
                        "SubType": "DateRange"
                    },
                    {
                        "Name": "By 1933",
                        "Matches": [
                            {
                                "Text": "By 1933",
                                "Offset": 36,
                                "Length": 7
                            }
                        ],
                        "Type": "DateTime",
                        "SubType": "DateRange"
                    },
                    {
                        "Name": "Gross domestic product",
                        "Matches": [
                            {
                                "Text": "GDP",
                                "Offset": 49,
                                "Length": 3
                            }
                        ],
                        "WikipediaLanguage": "en",
                        "WikipediaId": "Gross domestic product",
                        "WikipediaUrl": "https://en.wikipedia.org/wiki/Gross_domestic_product",
                        "BingId": "c859ed84-c0dd-e18f-394a-530cae5468a2"
                    },
                    {
                        "Name": "United States",
                        "Matches": [
                            {
                                "Text": "America",
                                "Offset": 56,
                                "Length": 7
                            }
                        ],
                        "WikipediaLanguage": "en",
                        "WikipediaId": "United States",
                        "WikipediaUrl": "https://en.wikipedia.org/wiki/United_States",
                        "BingId": "5232ed96-85b1-2edb-12c6-63e6c597a1de",
                        "Type": "Location"
                    },
                    {
                        "Name": "25",
                        "Matches": [
                            {
                                "Text": "25",
                                "Offset": 72,
                                "Length": 2
                            }
                        ],
                        "Type": "Quantity",
                        "SubType": "Number"
                    },
                    {
                        "Name": "25%",
                        "Matches": [
                            {
                                "Text": "25%",
                                "Offset": 72,
                                "Length": 3
                            }
                        ],
                        "Type": "Quantity",
                        "SubType": "Percentage"
                    }
                ]
            }
        ],
        "Errors": []
    }
```

## <a name="summary"></a>Riepilogo

In questo articolo si sono appresi i concetti e il flusso di lavoro per il collegamento delle entità usando Analisi del testo nei Servizi cognitivi. In sintesi:

+ L'[API entità](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634) è disponibile per le lingue selezionate.
+ I documenti JSON nel corpo della richiesta includono un ID, il testo e il codice della lingua.
+ La richiesta POST è a un endpoint `/entities`, usando [una chiave di accesso e un endpoint](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) personalizzati validi per la sottoscrizione.
+ L'output di risposta, che consiste di entità collegate (inclusi punteggi di attendibilità, offset e collegamenti Web per ogni ID di documento) può essere usato in qualsiasi applicazione

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [API Analisi del testo](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)

* [Panoramica di Analisi del testo](../overview.md)
* [Domande frequenti (FAQ)](../text-analytics-resource-faq.md)</br>
* [Pagina del prodotto Analisi del testo](//go.microsoft.com/fwlink/?LinkID=759712)
