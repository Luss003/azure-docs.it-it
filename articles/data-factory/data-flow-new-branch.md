---
title: Trasformazione Nuovo ramo per il flusso di dati di mapping di Azure Data Factory
description: Trasformazione Nuovo ramo per il flusso di dati di mapping di Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 3f443396627a4bbaba2a3eeb0a3ac05cc1597c85
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029281"
---
# <a name="azure-data-factory-mapping-data-flow-new-branch-transformation"></a>Trasformazione Nuovo ramo per il flusso di dati di mapping di Azure Data Factory



![Opzioni della trasformazione Nuovo ramo](media/data-flow/menu.png "menu")

Con la creazione di un ramo, il flusso di dati corrente viene replicato in un altro flusso. Usare la trasformazione Nuovo ramo per eseguire più set di operazioni e trasformazioni sullo stesso flusso di dati.

Esempio: Il flusso di dati include una trasformazione Origine con un set selezionato di colonne e conversioni di tipi di dati. È quindi possibile posizionare una colonna derivata subito dopo tale origine. Nell'elenco Derived Column (Colonna derivata) viene creato un nuovo campo che combina nome e cognome per creare un nuovo campo "Nome completo".

È possibile intervenire sul nuovo flusso con un set di trasformazioni e un sink su una riga, quindi usare la trasformazione Nuovo ramo per creare una copia del flusso in cui è possibile trasformare gli stessi dati con un set diverso di trasformazioni. Trasformando i dati copiati in un ramo separato, è possibile successivamente effettuare il sink di tali dati in un percorso distinto.

> [!NOTE]
> "New Branch" (Nuovo ramo) comparirà come azione nel menu Transformation (Trasformazione) solo in presenza di una trasformazione successiva dopo la posizione corrente in cui si tenta di creare un ramo. Questo significa che non sarà disponibile un'opzione "New Branch" (Nuovo ramo) alla fine, fino a quando non si aggiunge un'altra trasformazione dopo la trasformazione di selezione.

![Ramo](media/data-flow/branch2.png "Ramo 2")
