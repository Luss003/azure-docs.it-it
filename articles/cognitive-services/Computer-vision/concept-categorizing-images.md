---
title: Categorizzazione immagini-Visione artificiale
titleSuffix: Azure Cognitive Services
description: Informazioni sui concetti relativi alla funzione di classificazione delle immagini dell'API Visione artificiale.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 2b9b8da550d80b027da919ba0834e43e2c83d4b4
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/10/2019
ms.locfileid: "68945316"
---
# <a name="categorize-images-by-subject-matter"></a>Categorizzare le immagini in base al soggetto

Oltre ai tag e a una descrizione, Visione artificiale restituisce le categorie basate su tassonomia rilevate in un'immagine. A differenza dei tag, le categorie sono organizzate in una gerarchia ereditaria padre/figlio e ne è disponibile un numero inferiore (86, rispetto alle migliaia di tag). Tutti i nomi di categoria sono in inglese. La categorizzazione può essere eseguita da sola o insieme al più recente modello basato sui tag.

## <a name="the-86-category-concept"></a>Categoria basata su 86 concetti

Visione artificiale consente di categorizzare un'immagine in modo esteso o specifico, usando l'elenco di 86 categorie nel diagramma seguente. Per la classificazione completa in formato testo, vedere [Category Taxonomy](category-taxonomy.md) (Tassonomia delle categorie).

![Elenchi raggruppati di tutte le categorie nella tassonomia delle categorie](./Images/analyze_categories-v2.png)

## <a name="image-categorization-examples"></a>Esempi di classificazione delle immagini

La risposta JSON seguente illustra le informazioni restituite da Visione artificiale quando esegue la classificazione dell'immagine di esempio in base alle caratteristiche visive rilevate.

![Donna sul tetto di un edificio](./Images/woman_roof.png)

```json
{
    "categories": [
        {
            "name": "people_",
            "score": 0.81640625
        }
    ],
    "requestId": "bae7f76a-1cc7-4479-8d29-48a694974705",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

La tabella seguente illustra una serie di immagini tipiche e la categoria restituita da Visione artificiale per ogni immagine.

| Image | Category |
|-------|----------|
| ![Quattro persone che posano insieme come una famiglia](./Images/family_photo.png) | people_group |
| ![Cucciolo seduto in un prato](./Images/cute_dog.png) | animal_dog |
| ![Persona in piedi su una roccia al tramonto](./Images/mountain_vista.png) | outdoor_mountain |
| ![Pila di panini su un tavolo](./Images/bread.png) | food_bread |

## <a name="next-steps"></a>Passaggi successivi

Concetti relativi all'[assegnazione di tag alle immagini](concept-tagging-images.md) e alla [descrizione delle immagini](concept-describing-images.md).
