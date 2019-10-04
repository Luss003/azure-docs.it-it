---
title: Lingue supportate - API Traduzione vocale
titlesuffix: Azure Cognitive Services
description: Informazioni sulle lingue supportate dall'API Traduzione vocale.
services: cognitive-services
author: nitinme
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-speech
ms.topic: conceptual
ms.date: 3/5/2018
ms.author: nitinme
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 2f5b48f2dbc2d109c03613676c6a119fd971603b
ms.sourcegitcommit: fbea2708aab06c19524583f7fbdf35e73274f657
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2019
ms.locfileid: "70965436"
---
# <a name="languages-supported-by-the-translator-speech-api"></a>Lingue supportate dall'API Traduzione vocale

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-translator-speech-deprecation-note.md)]

Di seguito sono elencate le lingue supportate per la traduzione vocale. Se per la traduzione vocale sono supportate entrambe le lingue, sono disponibili i servizi di traduzione da contenuto vocale a contenuto vocale o da contenuto vocale a testo. Se la lingua di destinazione non è supportata per la traduzione vocale, è disponibile solo la traduzione da contenuto vocale a testo.

| Lingua per la sintesi vocale    |
|:----------- |
| Arabo (standard moderno)      |
| Portoghese brasiliano     |
| Cinese (mandarino)      |
| Inglese      |
| Francese      |
| Tedesco      |
| Italiano      |
| Giapponese      |
| Russo      |
| Spagnolo      |

L'API Traduzione vocale supporta le lingue seguenti come lingue di destinazione per la traduzione da contenuto vocale a testo.

| Lingua per il testo    | Codice lingua |
|:----------- |:-------------:|
| Afrikaans      | `af`          |
| Arabo       | `ar`          |
| Bengali      | `bn`          |
| Bosniaco (latino)      | `bs`          |
| Bulgaro      | `bg`          |
| Cantonese (tradizionale)      | `yue`          |
| Catalano      | `ca`          |
| Cinese semplificato      | `zh-Hans`          |
| Cinese tradizionale      | `zh-Hant`          |
| Croato      | `hr`          |
| Ceco      | `cs`          |
| Danese      | `da`          |
| Olandese      | `nl`          |
| Inglese      | `en`          |
| Estone      | `et`          |
| Figiano      | `fj`          |
| Filippino      | `fil`          |
| Finlandese      | `fi`          |
| Francese      | `fr`          |
| Tedesco      | `de`          |
| Greco      | `el`          |
| Creolo haitiano      | `ht`          |
| Ebraico      | `he`          |
| Hindi      | `hi`          |
| Hmong Daw      | `mww`          |
| Ungherese      | `hu`          |
|Islandese|`is`          |
| Indonesiano      | `id`          |
| Italiano      | `it`          |
| Giapponese      | `ja`          |
| Kiswahili      | `sw`          |
| Klingon      | `tlh`          |
| Klingon (plqaD)      | `tlh-Qaak`          |
| Coreano      | `ko`          |
| Lettone      | `lv`          |
| Lituano      | `lt`          |
| Malgascio      | `mg`          |
| Malese      | `ms`          |
| Maltese      | `mt`          |
| Norvegese      | `nb`          |
| Persiano      | `fa`          |
| Polacco      | `pl`          |
| Portoghese      | `pt`          |
| Querétaro Otomi      | `otq`          |
| Rumeno      | `ro`          |
| Russo      | `ru`          |
| Samoano      | `sm`          |
| Serbo (alfabeto cirillico)      | `sr-Cyrl`          |
| Serbo (alfabeto latino)      | `sr-Latn`          |
| Slovacco     | `sk`          |
| Sloveno      | `sl`          |
| Spagnolo      | `es`          |
| Svedese      | `sv`          |
| Tahitiano      | `ty`          |
| Tamil      | `ta`          |
| Thai      | `th`          |
| Tongano      | `to`          |
| Turco      | `tr`          |
| Ucraino      | `uk`          |
| Urdu      | `ur`          |
| Vietnamita      | `vi`          |
| Gallese      | `cy`          |
| Yucatec Maya      | `yua`          |

## <a name="access-the-list-programmatically"></a>Accedere all'elenco a livello di codice

È possibile accedere all'elenco di lingue supportate a livello di codice usando la risorsa Languages. L'elenco fornice il codice della lingua e il nome della lingua in inglese o in qualsiasi altra lingua supportata. L'elenco viene aggiornato automaticamente dal servizio Traduzione vocale quando vengono rese disponibili nuove lingue.

La risorsa Languages restituisce l'elenco di lingue supportate per riconoscimento vocale, testo e sintesi vocale. La risorsa Languages non richiede l'autenticazione.

[Vedere le informazioni di riferimento sull'API per provare il metodo per le lingue](languages-reference.md)

## <a name="access-the-list-on-the-microsoft-translator-website"></a>Accedere all'elenco nel sito Web Microsoft Translator

Per una rapida panoramica delle lingue, il sito Web Microsoft Translator offre l'elenco di tutte le lingue supportate dalle API Traduzione testuale e Traduzione vocale. L'elenco non include informazioni specifiche per sviluppatori, ad esempio i codici delle lingue.

[Visualizzare l'elenco delle lingue](https://www.microsoft.com/translator/languages.aspx)
