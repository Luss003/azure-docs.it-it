---
title: File di inclusione
description: File di inclusione
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.custom: include file
ms.date: 02/14/2020
ms.subservice: language-understanding
ms.author: diberry
ms.openlocfilehash: 956aa308bf1cb3736c491031239661ec6b295ddb
ms.sourcegitcommit: 79cbd20a86cd6f516acc3912d973aef7bf8c66e4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2020
ms.locfileid: "77279450"
---
L'applicazione client deve stabilire se un'espressione non è significativa o appropriata per l'applicazione. La finalità **None** (Nessuna) viene aggiunta a ciascuna applicazione come parte del processo di creazione per determinare se l'applicazione client non deve rispondere a un'espressione.

Se LUIS restituisce la finalità **None** per un'espressione, l'applicazione client può chiedere se l'utente intende terminare la conversazione oppure offrire altre indicazioni per continuare la conversazione.

Se si lascia vuota la finalità **None**, un'espressione che deve essere stimata all'esterno del dominio soggetto sarà stimata in una delle finalità del dominio soggetto esistente. Ciò determinerà operazioni non corrette basate su una stima errata da parte dell'applicazione client, ad esempio un chatbot.

1. Selezionare **Intents** (Finalità) dal pannello di sinistra.

1. Selezionare la finalità **None** (Nessuna). Aggiungere tre espressioni che l'utente potrebbe usare, ma che non sono pertinenti per l'app per l'ordinazione di pizze:

    |Espressioni di esempio `None`|
    |--|
    |`Barking dogs are annoying`|
    |`Penguins in the ocean`|

    Questi esempi non devono usare parole che ci si aspetta nel dominio soggetto, ad esempio `pizza`, `cheese`, `crust`, `pickup` `deliver`.