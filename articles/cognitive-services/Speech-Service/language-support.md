---
title: Supporto linguistico-servizio riconoscimento vocale
titleSuffix: Azure Cognitive Services
description: Il servizio di riconoscimento vocale supporta numerose lingue per la conversione di sintesi vocale e sintesi vocale, oltre alla traduzione vocale. Questo articolo fornisce un elenco completo del supporto linguistico per funzionalità del servizio.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/09/2020
ms.author: dapine
ms.custom: seodec18
ms.openlocfilehash: 11149019c609079bb22e043c3aaad7df9ec8393d
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2020
ms.locfileid: "79219808"
---
# <a name="language-and-region-support-for-the-speech-service"></a>Supporto di lingue e aree per il servizio di riconoscimento vocale

Il supporto della lingua varia in base alla funzionalità del servizio vocale. Nelle tabelle seguenti viene riepilogato il supporto delle lingue per le offerte del servizio [riconoscimento](#speech-to-text) [vocale, sintesi vocale](#text-to-speech)e [traduzione vocale](#speech-translation) .

## <a name="speech-to-text"></a>Riconoscimento vocale

Sia Microsoft Speech SDK che l'API REST supportano le seguenti lingue (impostazioni locali). Per migliorare l'accuratezza, la personalizzazione viene offerta per un subset di lingue tramite il caricamento di trascrizioni audio e con etichetta umana o testo correlato: frasi. La personalizzazione della pronuncia è attualmente disponibile solo per `en-US` e `de-DE`. Altre informazioni sulla personalizzazione sono disponibili [qui](how-to-custom-speech.md).

<!--
To get the AM and ML bits:
https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20models%3A/GetSupportedLocalesForModels

To get pronunciation bits:
https://cris.ai -> Click on Adaptation Data -> scroll down to section "Pronunciation Datasets" -> Click on Import -> Locale: the list of locales there correspond to the supported locales
-->

| Impostazioni locali  | Linguaggio                          | Supportato | Personalizzazioni                                    |
|---------|-----------------------------------|-----------|---------------------------------------------------|
| `ar-AE` | Arabo (UAE)                      | Sì       | No                                                |
| `ar-BH` | Arabo (Bahrain), standard moderno | Sì       | Modello linguistico                                    |
| `ar-EG` | Arabo (Egitto)                    | Sì       | Modello linguistico                                    |
| `ar-KW` | Arabo (Kuwait)                   | Sì       | No                                                |
| `ar-QA` | Arabo (Qatar)                    | Sì       | No                                                |
| `ar-SA` | Arabo (Arabia Saudita)             | Sì       | No                                                |
| `ca-ES` | Catalano                           | Sì       | Modello linguistico                                    |
| `da-DK` | Danese (Danimarca)                  | Sì       | Modello linguistico                                    |
| `de-DE` | Tedesco (Germania)                  | Sì       | Modello acustico<br>Modello linguistico<br>Pronuncia |
| `en-AU` | Inglese (Australia)               | Sì       | Modello acustico<br>Modello linguistico                  |
| `en-CA` | Inglese (Canada)                  | Sì       | Modello acustico<br>Modello linguistico                  |
| `en-GB` | Inglese (Regno Unito)          | Sì       | Modello acustico<br>Modello linguistico<br>Pronuncia |
| `en-IN` | Inglese (India)                   | Sì       | Modello acustico<br>Modello linguistico                  |
| `en-NZ` | Inglese (Nuova Zelanda)             | Sì       | Modello acustico<br>Modello linguistico                  |
| `en-US` | Inglese (Stati Uniti)           | Sì       | Modello acustico<br>Modello linguistico<br>Pronuncia |
| `es-ES` | Spagnolo (Spagna)                   | Sì       | Modello acustico<br>Modello linguistico                  |
| `es-MX` | Spagnolo (Messico)                  | Sì       | Modello acustico<br>Modello linguistico                  |
| `fi-FI` | Finlandese (Finlandia)                 | Sì       | Modello linguistico                                    |
| `fr-CA` | Francese (Canada)                   | Sì       | Modello acustico<br>Modello linguistico                  |
| `fr-FR` | Francese (Francia)                   | Sì       | Modello acustico<br>Modello linguistico<br>Pronuncia |
| `gu-IN` | Gujarati (indiano)                 | Sì       | Modello linguistico                                    |
| `hi-IN` | Hindi (India)                     | Sì       | Modello acustico<br>Modello linguistico                  |
| `it-IT` | Italiano (Italia)                   | Sì       | Modello acustico<br>Modello linguistico<br>Pronuncia |
| `ja-JP` | Giapponese (Giappone)                  | Sì       | Modello linguistico                                    |
| `ko-KR` | Coreano (Corea)                    | Sì       | Modello linguistico                                    |
| `mr-IN` | Marathi (India)                   | Sì       | Modello linguistico                                    |
| `nb-NO` | Norvegese (Bokmål) (Norvegia)       | Sì       | Modello linguistico                                    |
| `nl-NL` | Olandese (Paesi Bassi)               | Sì       | Modello linguistico                                    |
| `pl-PL` | Polacco (Polonia)                   | Sì       | Modello linguistico                                    |
| `pt-BR` | Portoghese (Brasile)               | Sì       | Modello acustico<br>Modello linguistico<br>Pronuncia |
| `pt-PT` | Portoghese (Portogallo)             | Sì       | Modello linguistico                                    |
| `ru-RU` | Russo (Russia)                  | Sì       | Modello acustico<br>Modello linguistico                  |
| `sv-SE` | Svedese (Svezia)                  | Sì       | Modello linguistico                                    |
| `ta-IN` | Tamil (India)                     | Sì       | Modello linguistico                                    |
| `te-IN` | Telugu (India)                    | Sì       | No                                                |
| `th-TH` | Thai (Thailandia)                   | Sì       | No                                                |
| `tr-TR` | Turco (Turchia)                  | Sì       | No                                                |
| `zh-CN` | Cinese (mandarino, semplificato)    | Sì       | Modello acustico<br>Modello linguistico                  |
| `zh-HK` | Cinese (cantonese, tradizionale)  | Sì       | Modello linguistico                                    |
| `zh-TW` | Cinese (mandarino taiwanese)      | Sì       | Modello linguistico                                    |

## <a name="text-to-speech"></a>Sintesi vocale

Sia Microsoft Speech SDK che le API REST supportano queste voci, ognuna delle quali supporta una lingua e un dialetto specifici, identificati dalle impostazioni locali.

> [!IMPORTANT]
> I prezzi variano per voci standard, personalizzate e neurali. Per informazioni aggiuntive, visitare la pagina [Prezzi](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="neural-voices"></a>Voci neurali

La sintesi vocale neurale è un nuovo tipo di sintesi vocale basata su reti neurali profonde. Quando si usa una voce neurale, è praticamente impossibile distinguere la sintesi vocale dalle registrazioni umane.

Le voci neurali possono essere usate per rendere più naturali e accattivanti le interazioni con chatbot e gli assistenti vocali, convertire i testi digitali, ad esempio gli e-book in Audiolibri e migliorare i sistemi di navigazione in auto. Con la prosodia naturale simile al linguaggio umano e l'articolazione chiara delle parole, le voci neurali riducono in modo significativo le difficoltà di ascolto durante l'interazione degli utenti con i sistemi di intelligenza artificiale.

Per informazioni sulla disponibilità a livello di area, vedere [Aree](regions.md#standard-and-neural-voices).

| Impostazioni locali  | Linguaggio            | Sesso | Mapping completo del nome del servizio                                               | Nome breve vocale        |
|---------|---------------------|--------|-------------------------------------------------------------------------|-------------------------|
| `de-DE` | Tedesco (Germania)    | Female | "Voce Sintesi vocale vocale di Microsoft Server (de-DE, KatjaNeural)"     | "de-DE-KatjaNeural"     |
| `en-US` | Inglese (Stati Uniti)        | Female | "Microsoft Server Speech Text to Speech Voice (en-US, JessaNeural)"     | "en-US-JessaNeural"     |
| `en-US` | Inglese (Stati Uniti)        | Male   | "Microsoft Server Speech Text to Speech Voice (en-US, GuyNeural)"       | "en-US-GuyNeural"       |
| `it-IT` | Italiano (Italia)     | Female | "Voce Sintesi vocale vocale di Microsoft Server (it-IT, ElsaNeural)"      | "it-IT-ElsaNeural"      |
| `pt-BR` | Portoghese (Brasile) | Female | "Voce Sintesi vocale vocale di Microsoft Server (PT-BR, FranciscaNeural)" | "PT-BR-FranciscaNeural" |
| `zh-CN` | Cinese (Terraferma)  | Female | "Microsoft Server Speech Text to Speech Voice (zh-CN, XiaoxiaoNeural)"  | "zh-CN-XiaoxiaoNeural"  |

Per informazioni su come configurare e modificare le voci neurali, vedere [linguaggio di markup sintesi vocale](speech-synthesis-markup.md#adjust-speaking-styles).

> [!NOTE]
> È possibile usare il mapping del nome completo del servizio o il nome della voce breve nelle richieste di sintesi vocale.

### <a name="standard-voices"></a>Voci standard

Sono disponibili più di 75 voci standard in oltre 45 lingue e impostazioni locali, che consentono di convertire il testo in contenuto vocale sintetizzato. Per informazioni sulla disponibilità a livello di area, vedere [Aree](regions.md#standard-and-neural-voices).

| Impostazioni locali | Linguaggio | Sesso | Mapping completo del nome del servizio | Nome breve |
|--|--|--|--|--|
| <sup>1</sup>`ar-EG` | Arabo (Egitto) | Female | "Microsoft Server Speech Text to Speech Voice (ar-EG, Hoda)" | "ar-EG-un". |
| `ar-SA` | Arabo (Arabia Saudita) | Male | "Microsoft Server Speech Text to Speech Voice (ar-SA, Naayf)" | "ar-SA-Naayf" |
| `bg-BG` | Bulgaro | Male | "Microsoft Server Speech Text to Speech Voice (bg-BG, Ivan)" | "BG-BG-Ivan" |
| `ca-ES` | Catalano (Spagna) | Female | "Microsoft Server Speech Text to Speech Voice (ca-ES, HerenaRUS)" | "ca-ES-HerenaRUS" |
| `cs-CZ` | Ceco | Male | "Microsoft Server Speech Text to Speech Voice (cs-CZ, Jakub)" | "CS-CZ-Jakub" |
| `da-DK` | Danese | Female | "Microsoft Server Speech Text to Speech Voice (da-DK, HelleRUS)" | "da-DK-HelleRUS" |
| `de-AT` | Tedesco (Austria) | Male | "Microsoft Server Speech Text to Speech Voice (de-AT, Michael)" | "de-AT-Michael" |
| `de-CH` | Tedesco (Svizzera) | Male | "Microsoft Server Speech Text to Speech Voice (de-CH, Karsten)" | "de-CH-Karsten" |
| `de-DE` | Tedesco (Germania) | Female | "Microsoft Server Speech Text to Speech Voice (de-DE, Hedda)" | "de-DE-Hedda" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (de-DE, HeddaRUS)" | "de-DE-HeddaRUS" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (de-DE, Stefan, Apollo)" | "de-DE-Stefan-Apollo" |
| `el-GR` | Greco | Male | "Microsoft Server Speech Text to Speech Voice (el-GR, Stefanos)" | "El-GR-Stefanos" |
| `en-AU` | Inglese (Australia) | Female | "Microsoft Server Speech Text to Speech Voice (en-AU, Catherine)" | "en-AU-Catherine" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (en-AU, HayleyRUS)" | "en-AU-HayleyRUS" |
| `en-CA` | Inglese (Canada) | Female | "Microsoft Server Speech Text to Speech Voice (en-CA, Linda)" | "en-CA-Linda" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (en-CA, HeatherRUS)" | "en-CA-HeatherRUS" |
| `en-GB` | Inglese (Regno Unito) | Female | "Microsoft Server Speech Text to Speech Voice (en-GB, Susan, Apollo)" | "en-GB-Susan-Apollo" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (en-GB, HazelRUS)" | "en-GB-HazelRUS" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (en-GB, George, Apollo)" | "en-GB-George-Apollo" |
| `en-IE` | Inglese (Irlanda) | Male | "Microsoft Server Speech Text to Speech Voice (en-IE, Sean)" | "en-IE-Sean" |
| `en-IN` | Inglese (India) | Female | "Microsoft Server Speech Text to Speech Voice (en-IN, Heera, Apollo)" | "en-IN-si-Apollo" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (en-IN, PriyaRUS)" | "en-IN-PriyaRUS" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (en-IN, Ravi, Apollo)" | "en-IN-Ravi-Apollo" |
| `en-US` | Inglese (Stati Uniti) | Female | "Microsoft Server Speech Text to Speech Voice (en-US, ZiraRUS)" | "en-US-ZiraRUS" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)" | "en-US-JessaRUS" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (en-US, BenjaminRUS)" | "en-US-BenjaminRUS" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)" | "en-US-Jessa24kRUS" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)" | "en-US-Guy24kRUS" |
| `es-ES` | Spagnolo (Spagna) | Female | "Microsoft Server Speech Text to Speech Voice (es-ES, Laura, Apollo)" | "es-ES-Laura-Apollo" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (es-ES, HelenaRUS)" | "es-ES-HelenaRUS" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (es-ES, Pablo, Apollo)" | "es-ES-Pablo-Apollo" |
| `es-MX` | Spagnolo (Messico) | Female | "Microsoft Server Speech Text to Speech Voice (es-MX, HildaRUS)" | "es-MX-HildaRUS" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (es-MX, Raul, Apollo)" | "es-MX-Raul-Apollo" |
| `fi-FI` | Finlandese | Female | "Microsoft Server Speech Text to Speech Voice (fi-FI, HeidiRUS)" | "fi-FI-HeidiRUS" |
| `fr-CA` | Francese (Canada) | Female | "Microsoft Server Speech Text to Speech Voice (fr-CA, Caroline)" | "fr-CA-Caroline" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (fr-CA, HarmonieRUS)" | "fr-CA-HarmonieRUS" |
| `fr-CH` | Francese (Svizzera) | Male | "Microsoft Server Speech Text to Speech Voice (fr-CH, Guillaume)" | "fr-CH-Guillaume" |
| `fr-FR` | Francese (Francia) | Female | "Microsoft Server Speech Text to Speech Voice (fr-FR, Julie, Apollo)" | "fr-FR-Julie-Apollo" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (fr-FR, HortenseRUS)" | "fr-FR-HortenseRUS" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (fr-FR, Paul, Apollo)" | "fr-FR-Paul-Apollo" |
| `he-IL` | Ebraico (Israele) | Male | "Microsoft Server Speech Text to Speech Voice (he-IL, Asaf)" | "he-IL-la |
| `hi-IN` | Hindi (India) | Female | "Microsoft Server Speech Text to Speech Voice (hi-IN, Kalpana, Apollo)" | "hi-IN-Kalpana-Apollo" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (hi-IN, Kalpana)" | "hi-IN-Kalpana" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (hi-IN, Hemant)" | "Hi-IN-," |
| `hr-HR` | Croato | Male | "Microsoft Server Speech Text to Speech Voice (hr-HR, Matej)" | "HR-HR-Matej" |
| `hu-HU` | Ungherese | Male | "Microsoft Server Speech Text to Speech Voice (hu-HU, Szabolcs)" | "hu-HU-se" |
| `id-ID` | Indonesiano | Male | "Microsoft Server Speech Text to Speech Voice (id-ID, Andika)" | "ID-ID-Andika" |
| `it-IT` | Italiano | Male | "Microsoft Server Speech Text to Speech Voice (it-IT, Cosimo, Apollo)" | "it-IT-Cosimo-Apollo" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (it-IT, LuciaRUS)" | "it-IT-LuciaRUS" |
| `ja-JP` | Giapponese | Female | "Microsoft Server Speech Text to Speech Voice (ja-JP, Ayumi, Apollo)" | "ja-JP-Ayumi-Apollo" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (ja-JP, Ichiro, Apollo)" | "ja-JP-Ichiro-Apollo" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (ja-JP, HarukaRUS)" | "ja-JP-HarukaRUS" |
| `ko-KR` | Coreano | Female | "Microsoft Server Speech Text to Speech Voice (ko-KR, HeamiRUS)" | "ko-KR-HeamiRUS" |
| `ms-MY` | Malese | Male | "Microsoft Server Speech Text to Speech Voice (ms-MY, Rizwan)" | "MS-MY-per" |
| `nb-NO` | Norvegese | Female | "Microsoft Server Speech Text to Speech Voice (nb-NO, HuldaRUS)" | "nb-NO-HuldaRUS" |
| `nl-NL` | Olandese | Female | "Microsoft Server Speech Text to Speech Voice (nl-NL, HannaRUS)" | "nl-NL-HannaRUS" |
| `pl-PL` | Polacco | Female | "Microsoft Server Speech Text to Speech Voice (pl-PL, PaulinaRUS)" | "pl-PL-PaulinaRUS" |
| `pt-BR` | Portoghese (Brasile) | Female | "Microsoft Server Speech Text to Speech Voice (pt-BR, HeloisaRUS)" | "PT-BR-HeloisaRUS" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (pt-BR, Daniel, Apollo)" | "PT-BR-Daniel-Apollo" |
| `pt-PT` | Portoghese (Portogallo) | Female | "Microsoft Server Speech Text to Speech Voice (pt-PT, HeliaRUS)" | "PT-PT-HeliaRUS" |
| `ro-RO` | Rumeno | Male | "Microsoft Server Speech Text to Speech Voice (ro-RO, Andrei)" | "ro-RO-Andrei" |
| `ru-RU` | Russo | Female | "Microsoft Server Speech Text to Speech Voice (ru-RU, Irina, Apollo)" | "ur-ur-Irina-Apollo" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (ru-RU, Pavel, Apollo)" | "ur-ur-Pavel-Apollo" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (ru-RU, EkaterinaRUS)" | ur-ur-EkaterinaRUS |
| `sk-SK` | Slovacco | Male | "Microsoft Server Speech Text to Speech Voice (sk-SK, Filip)" | "sk-SK-Filip" |
| `sl-SI` | Sloveno | Male | "Microsoft Server Speech Text to Speech Voice (sl-SI, Lado)" | "sl-SI-Lado" |
| `sv-SE` | Svedese | Female | "Microsoft Server Speech Text to Speech Voice (sv-SE, HedvigRUS)" | "sv-SE-HedvigRUS" |
| `ta-IN` | Tamil (India) | Male | "Microsoft Server Speech Text to Speech Voice (ta-IN, Valluvar)" | "TA-IN-Valluvar" |
| `te-IN` | Telugu (India) | Female | "Microsoft Server Speech Text to Speech Voice (te-IN, Chitra)" | "te-IN-Chitle" |
| `th-TH` | Thai | Male | "Microsoft Server Speech Text to Speech Voice (th-TH, Pattara)" | "th-TH-Pattara" |
| `tr-TR` | Turco (Turchia) | Female | "Microsoft Server Speech Text to Speech Voice (tr-TR, SedaRUS)" | "tr-TR-SedaRUS" |
| `vi-VN` | Vietnamita | Male | "Microsoft Server Speech Text to Speech Voice (vi-VN, An)" | "vi-VN-An" |
| `zh-CN` | Cinese (Terraferma) | Female | "Microsoft Server Speech Text to Speech Voice (zh-CN, HuihuiRUS)" | "zh-CN-HuihuiRUS" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (zh-CN, Yaoyao, Apollo)" | "zh-CN-Yaoyao-Apollo" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (zh-CN, Kangkang, Apollo)" | "zh-CN-Kangkang-Apollo" |
| `zh-HK` | Cinese (Hong Kong) | Female | "Microsoft Server Speech Text to Speech Voice (zh-HK, Tracy, Apollo)" | "zh-HK-Tracy-Apollo" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (zh-HK, TracyRUS)" | "zh-HK-TracyRUS" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (zh-HK, Danny, Apollo)" | "zh-HK-Danny-Apollo" |
| `zh-TW` | Cinese (Taiwan) | Female | "Microsoft Server Speech Text to Speech Voice (zh-TW, Yating, Apollo)" | "zh-TW-Yating-Apollo" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (zh-TW, HanHanRUS)" | "zh-TW-HanHanRUS" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (zh-TW, Zhiwei, Apollo)" | "zh-TW-Zhiwei-Apollo" |

**1** *ar-EG supporta l'arabo standard moderno (MSA).*

> [!NOTE]
> È possibile usare il mapping del nome completo del servizio o il nome della voce breve nelle richieste di sintesi vocale.

### <a name="customization"></a>Personalizzazione

La personalizzazione vocale è disponibile per `de-DE`, `en-GB`, `en-IN`, `en-US`, `es-MX`, `fr-FR`, `it-IT`, `pt-BR`e `zh-CN`. Selezionare le impostazioni locali corrette che corrispondono ai dati di training per il training di un modello vocale personalizzato. Se, ad esempio, i dati di registrazione sono pronunciati in inglese con un accento britannico, selezionare `en-GB`.

> [!NOTE]
> Non è supportato il training del modello bilingue in una voce personalizzata, ad eccezione della lingua inglese cinese (BI). Per eseguire il training di una voce cinese in lingua inglese, selezionare "lingua inglese per il cinese". Il training vocale in tutte le impostazioni locali inizia con un set di dati di 2000 + espressioni, ad eccezione del `en-US` e `zh-CN` in cui è possibile iniziare con qualsiasi dimensione dei dati di training.

## <a name="speech-translation"></a>Traduzione vocale

L'API **Traduzione vocale** supporta lingue diverse per la traduzione vocale e con riconoscimento vocale. La lingua di origine deve essere sempre presente nella tabella della lingua di riconoscimento vocale. Le lingue di destinazione disponibili variano a seconda del fatto che siano parlate o scritte. È possibile tradurre la voce in ingresso in più di [60 lingue](https://www.microsoft.com/translator/business/languages/). Per la [sintesi vocale](language-support.md#text-languages)è disponibile un subset di linguaggi.

### <a name="text-languages"></a>Lingue per il testo

| Lingua per il testo           | Codice lingua |
|:------------------------|:-------------:|
| Afrikaans               | `af`          |
| Arabo                  | `ar`          |
| Bengalese                  | `bn`          |
| Bosniaco (latino)         | `bs`          |
| Bulgaro               | `bg`          |
| Cantonese (tradizionale) | `yue`         |
| Catalano                 | `ca`          |
| Cinese semplificato      | `zh-Hans`     |
| Cinese tradizionale     | `zh-Hant`     |
| Croato                | `hr`          |
| Ceco                   | `cs`          |
| Danese                  | `da`          |
| Olandese                   | `nl`          |
| Inglese                 | `en`          |
| Estone                | `et`          |
| Figiano                  | `fj`          |
| Filippino                | `fil`         |
| Finlandese                 | `fi`          |
| Francese                  | `fr`          |
| Tedesco                  | `de`          |
| Greco                   | `el`          |
| Creolo haitiano          | `ht`          |
| Ebraico                  | `he`          |
| Hindi                   | `hi`          |
| Hmong Daw               | `mww`         |
| Ungherese               | `hu`          |
| Indonesiano              | `id`          |
| Irlandese                   | `ga`          |
| Italiano                 | `it`          |
| Giapponese                | `ja`          |
| Kannada                 | `kn`          |
| Kiswahili               | `sw`          |
| Klingon                 | `tlh`         |
| Klingon (plqaD)         | `tlh-Qaak`    |
| Coreano                  | `ko`          |
| Lettone                 | `lv`          |
| Lituano              | `lt`          |
| Malgascio                | `mg`          |
| Malese                   | `ms`          |
| Malayalam               | `ml`          |
| Maltese                 | `mt`          |
| Norvegese               | `nb`          |
| Persiano                 | `fa`          |
| Polacco                  | `pl`          |
| Portoghese (Brasile)     | `pt-br`       |
| Portoghese (Portogallo)   | `pt-pt`       |
| Punjabi                 | `pa`          |
| Querétaro Otomi         | `otq`         |
| Rumeno                | `ro`          |
| Russo                 | `ru`          |
| Samoano                  | `sm`          |
| Serbo (alfabeto cirillico)      | `sr-Cyrl`     |
| Serbo (alfabeto latino)         | `sr-Latn`     |
| Slovacco                  | `sk`          |
| Sloveno               | `sl`          |
| Spagnolo                 | `es`          |
| Svedese                 | `sv`          |
| Tahitiano                | `ty`          |
| Tamil                   | `ta`          |
| Telugu                  | `te`          |
| Thai                    | `th`          |
| Tongano                  | `to`          |
| Turco                 | `tr`          |
| Ucraino               | `uk`          |
| Urdu                    | `ur`          |
| Vietnamita              | `vi`          |
| Gallese                   | `cy`          |
| Yucatec Maya            | `yua`         |

## <a name="next-steps"></a>Passaggi successivi

* [Ricevi la tua sottoscrizione di valutazione del servizio vocale](https://azure.microsoft.com/try/cognitive-services/)
* [Informazioni sul riconoscimento vocale in C#](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-chsarp)
