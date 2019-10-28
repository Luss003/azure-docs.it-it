---
title: Supporto per la lingua - API Analisi del testo
titleSuffix: Azure Cognitive Services
description: Ecco un elenco di linguaggi naturali supportati dall'API Analisi del testo. Questo articolo spiega quali lingue sono supportate per le operazioni di analisi del sentiment, estrazione delle frasi chiave, rilevamento della lingua e riconoscimento delle entità.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 06/18/2019
ms.author: aahi
ms.openlocfilehash: 846650fce6701bb7e382df049902f427390b3051
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/25/2019
ms.locfileid: "72931221"
---
# <a name="language-and-region-support-for-the-text-analytics-api"></a>Supporto lingua e area geografica per l'API Analisi del testo

Questo articolo illustra le lingue supportate per ogni operazione: analisi dei sentimenti, estrazione di frasi chiave, rilevamento della lingua e riconoscimento delle entità denominate.

## <a name="language-detection"></a>Rilevamento lingua

Il API Analisi del testo è in grado di rilevare un'ampia gamma di linguaggi, varianti, dialetti e lingue regionali/culturali.  Rilevamento lingua restituisce la lingua in generale. Per la frase "I have a dog", ad esempio, restituisce `en` anziché `en-US`. L'unico caso particolare è rappresentato dalla lingua cinese, per la quale la funzionalità Rilevamento lingua restituisce `zh_CHS` o `zh_CHT`, se è in grado di determinare il tipo di scrittura dal testo fornito. In situazioni in cui non è possibile identificare il tipo di scrittura per un documento in cinese, restituirà semplicemente `zh`.

L'elenco esatto delle lingue per questa funzionalità non viene pubblicato, ma è in grado di rilevare un'ampia gamma di lingue, varianti, dialetti e alcune lingue regionali/culturali. 

Se si ha contenuto espresso in un lingua usata con minore frequenza, si può provare Rilevamento lingua per vedere se viene restituito un codice. La risposta per le lingue che non è possibile rilevare è `unknown`.

## <a name="sentiment-analysis-key-phrase-extraction-and-named-entity-recognition"></a>Analisi del sentiment, Estrazione frasi chiave e il riconoscimento di entità denominate

Per l'analisi del sentiment, l'estrazione delle frasi chiave e il riconoscimento delle entità, l'elenco delle lingue supportate è più selettivo, poiché gli analizzatori sono ottimizzati in base alle regole linguistiche di lingue aggiuntive. Il supporto per il set completo di [tipi di entità](how-tos/text-analytics-how-to-entity-linking.md#supported-types-for-named-entity-recognition) è attualmente limitato alle lingue seguenti: 
* Italiano
* Cinese semplificato
* Francese
* Tedesco
* Spagnolo

Per gli altri linguaggi vengono restituite solo le entità denominate `Person`, `Location` e `Organization`.

## <a name="language-list-and-status"></a>Elenco e stato delle lingue

Il supporto di una lingua viene inizialmente implementato in anteprima e quindi promosso alla disponibilità a livello generale (GA), indipendentemente dalle altre lingue e dal servizio Analisi del testo nel suo complesso. È possibile che una lingua rimanga disponibile in anteprima, anche quando l'API Analisi del testo passa alla disponibilità a livello generale.

| Linguaggio    | Codice lingua | Sentiment | Frase chiave | Riconoscimento di entità denominate |   Note  |
|:----------- |:-------------:|:---------:|:-----------:|:-----------:|:-----------:
| Arabo      | `ar`          |           |             | ✔ \*                     | |
| Ceco       | `cs`          |           |             | ✔ \*                     | |
| Cinese semplificato | `zh-hans`| ✔ \***     |             | ✔         |    |
| Cinese tradizionale | `zh-hant`| ✔ \***     |             |          |    |
| Danese      | `da`          | ✔ \*     | ✔           | ✔ \*            |     |
| Olandese       | `nl`          | ✔ \***     | ✔          |  ✔ \*           |     |
| Italiano     | `en`          | ✔ \***       | ✔           |  ✔ \*\*     |      |
| Finlandese     | `fi`          | ✔ \*     | ✔           |  ✔ \*           |     |
| Francese      | `fr`          | ✔ \***       | ✔           |  ✔            |     |
| Tedesco      | `de`          | ✔ \***     | ✔           |  ✔           |     |
| Greco       | `el`          | ✔ \*     |             |            |     |
| Ungherese   | `hu`          |           |             |  ✔ \*          |     | 
| Italiano     | `it`          | ✔ \***     | ✔           |  ✔ \*           |     |
| Giapponese    | `ja`          | ✔ \***         | ✔           |  ✔ \*          |     |
| Coreano      | `ko`          |          | ✔           |  ✔ \*          |     |
| Norvegese (Bokmål) | `no`  | ✔ \*     |  ✔          | ✔ \*            |     |
| Polacco      | `pl`          | ✔ \*     |  ✔          |  ✔ \*           |     |
| Portoghese (Portogallo) | `pt-PT`| ✔ \***        |  ✔          | ✔ \*      |Accettato anche `pt`|
| Portoghese (Brasile)   | `pt-BR`|          |  ✔   |  ✔ \*       |     |
| Russo     | `ru`          | ✔ \*     | ✔           |  ✔ \*           |     |
| Spagnolo     | `es`          | ✔ \***       | ✔           |   ✔ \*\*      |     | 
| Svedese     | `sv`          | ✔ \*     | ✔           |   ✔ \*          |     |
| Turco     | `tr`          | ✔ \*     |             |   ✔ \*          |  |

il supporto della lingua \* è in anteprima

\*\* il [riconoscimento delle entità denominato](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-ner) e il [collegamento di entità](how-tos/text-analytics-how-to-entity-linking.md#entity-linking) sono entrambi disponibili per questa lingua.  

\** * disponibile nell' [anteprima pubblica analisi del sentiment V3](https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis#sentiment-analysis-v3-public-preview)

## <a name="see-also"></a>Vedi anche

[Documentazione dei servizi cognitivi](https://docs.microsoft.com/azure/cognitive-services/)   
[Pagina del prodotto Servizi cognitivi](https://azure.microsoft.com/services/cognitive-services/)
