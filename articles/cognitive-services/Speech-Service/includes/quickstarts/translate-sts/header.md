---
title: 'Guida introduttiva: Tradurre la voce in voce - Servizio Voce'
titleSuffix: Azure Cognitive Services
description: Da definire
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 12/09/2019
ms.author: yulili
ms.openlocfilehash: c5f0a0fe032d18cd4f01aebe9a5c736d6d511a74
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/10/2019
ms.locfileid: "74980832"
---
In questo argomento di avvio rapido si userà [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) per tradurre in modo interattivo la voce da una lingua a un'altra. Dopo aver soddisfatto alcuni prerequisiti, la traduzione della voce in voce richiede solo sei passaggi:
> [!div class="checklist"]
> * Creare un oggetto ````SpeechTranslationConfig```` dalla chiave e dall'area di sottoscrizione.
> * Aggiornare l'oggetto ````SpeechTranslationConfig```` per specificare le lingue di origine e di destinazione.
> * Aggiornare l'oggetto ````SpeechTranslationConfig```` per specificare il nome della voce di output vocale.
> * Creare un oggetto ````TranslationRecognizer```` tramite l'oggetto ````SpeechTranslationConfig```` specificato.
> * Usando l'oggetto ````TranslationRecognizer````, avviare il processo di riconoscimento per una singola espressione.
> * Controllare l'elemento ````TranslationRecognitionResult```` restituito.
