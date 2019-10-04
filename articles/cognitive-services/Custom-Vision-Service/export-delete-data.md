---
title: Esportare o eliminare i dati - Servizio visione artificiale personalizzato
titleSuffix: Azure Cognitive Services
description: Informazioni su come esportare o eliminare i dati nel Servizio visione artificiale personalizzato.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: pafarley
ms.openlocfilehash: b885f359d9416fbc5f778b094610260342a75f65
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/26/2019
ms.locfileid: "68564220"
---
# <a name="export-or-delete-user-data-in-custom-vision"></a>Esportare o eliminare i dati in Visione personalizzata

Visione personalizzata raccoglie i dati utente per il funzionamento del servizio, ma i clienti hanno il controllo completo sulla visualizzazione, l'esportazione e l'eliminazione dei dati usando le [API di training](https://go.microsoft.com/fwlink/?linkid=865446)di visione personalizzata.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Per informazioni su come esportare ed eliminare i dati utente in Visione personalizzata, vedere la tabella seguente.

| Data | Operazione di esportazione | Operazione di eliminazione |
| ---- | ---------------- | ---------------- |
| Informazioni sull'account (chiavi di sottoscrizione) | [GetAccountInfo](https://go.microsoft.com/fwlink/?linkid=865446) | Eliminare tramite il portale di Azure (sottoscrizioni di Azure). In alternativa, usare il pulsante "Elimina l'account" nella pagina delle impostazioni di CustomVision.ai (sottoscrizioni di account Microsoft) | 
| Dettagli sull'iterazione | [GetIteration](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Dettagli sulle prestazioni dell'iterazione | [GetIterationPerformance](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Elenco delle iterazioni | [GetIterations](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Progetti e dettagli dei progetti | [GetProject](https://go.microsoft.com/fwlink/?linkid=865446) e [GetProjects](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteProject](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Tag immagine | [GetTag](https://go.microsoft.com/fwlink/?linkid=865446) e [GetTags](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteTag](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Immagini | [GetTaggedImages](https://go.microsoft.com/fwlink/?linkid=865446) (fornisce l'URI per il download di immagini) e [GetUntaggedImages](https://go.microsoft.com/fwlink/?linkid=865446) (fornisce l'URI per il download di immagini) | [DeleteImages](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Modelli esportati | [GetExports](https://go.microsoft.com/fwlink/?linkid=865446) | Eliminati con l'eliminazione dell'account |
