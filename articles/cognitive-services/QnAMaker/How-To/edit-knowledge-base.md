---
title: Modificare una knowledge base - QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker consente di gestire il contenuto della Knowledge Base fornendo un'esperienza di modifica facile da usare.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: cc4ead968a0ee2c9890c1cd24a6b70516b2b2e74
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/22/2019
ms.locfileid: "74326793"
---
# <a name="edit-a-knowledge-base-in-qna-maker"></a>Modificare una knowledge base in QnA Maker

QnA Maker consente di gestire il contenuto della Knowledge Base fornendo un'esperienza di modifica facile da usare.

<a name="add-datasource"></a>

## <a name="edit-your-knowledge-base-content"></a>Modificare il contenuto della Knowledge Base

1.  Selezionare **My Knowledge Bases** (Knowledge Base personali) nella barra di spostamento in alto. 

    È possibile visualizzare tutti i servizi creati o condivisi con l'utente disposti in ordine decrescente in base alla data dell'**ultima modifica**.

    ![Knowledge Base personali](../media/qnamaker-how-to-edit-kb/my-kbs.png)

1. Selezionare una Knowledge Base per modificarla.
 
1. Selezionare **Impostazioni**. Qui è possibile modificare il campo obbligatorio Nome del servizio.
  
    |Obiettivo|Azione|
    |--|--|
    |Aggiungere URL|È possibile aggiungere nuovi URL per aggiungere nuovi contenuti delle domande frequenti alla knowledge base facendo clic sul collegamento **Gestisci Knowledge base -> "+ Aggiungi URL"** .|
    |Eliminare URL|È possibile eliminare gli URL esistenti selezionando l'icona di eliminazione (cestino).|
    |Refresh content|Se si vuole che la knowledge base esegua una ricerca per indicizzazione del contenuto più recente degli URL esistenti, selezionare la casella di controllo **Aggiorna**. This will update the knowledge base with latest URL content once. This is not setting a regular schedule of updates.|
    |Aggiungere file|È possibile aggiungere un documento supportato come parte di una knowledge base selezionando **Gestisci Knowledge base** e quindi **+ Aggiungi file**.|
    |Importa|È anche possibile importare le knowledge base esistenti selezionando il pulsante **Importa Knowledge base**. |
    |Aggiornare|L'aggiornamento della Knowledge base dipende dal **piano tariffario di gestione** usato durante la creazione del servizio QnA Maker associato alla knowledge base. Se necessario, è anche possibile aggiornare il livello di gestione dal portale di Azure.

1. Dopo aver apportato le modifiche alla knowledge base, selezionare **Save and train** (Salva ed esegui training) nell'angolo in alto a destra della pagina per salvare le modifiche in modo permanente.    

    ![Save and train (Salva ed esegui training)](../media/qnamaker-how-to-edit-kb/save-and-train.png)

    >[!CAUTION]
    >Se si lascia la pagina prima di selezionare **Save and train** (Salva ed esegui training), tutte le modifiche andranno perse.

## <a name="add-a-qna-pair"></a>Aggiungere una coppia domanda/risposta

On the **EDIT** page, select **Add QnA pair** to add a new row to the knowledge base table.

![Add QnA pair (Aggiungi coppia domanda/risposta)](../media/qnamaker-how-to-edit-kb/add-qnapair.png)

## <a name="delete-a-qna-pair"></a>Eliminare una coppia domanda/risposta

Per eliminare una coppia domanda/risposta, fare clic sull'icona **Elimina** all'estrema destra della riga. Si tratta di un'operazione definitiva. Non può essere modificata. Prendere in considerazione la possibilità di esportare la knowledge base dalla pagina **Pubblica** prima di eliminare le coppie. 

![Eliminare una coppia domanda/risposta](../media/qnamaker-how-to-edit-kb/delete-qnapair.png)

## <a name="add-alternate-questions"></a>Aggiungere domande alternative

Aggiungere domande alternative a una coppia domanda/risposta esistente per aumentare la probabilità di una corrispondenza con le query degli utenti.

![Aggiungere domande alternative](../media/qnamaker-how-to-edit-kb/add-alternate-question.png)

## <a name="add-metadata"></a>Aggiungere metadati

Add metadata pairs by first selecting **View options**, then selecting **Show metadata**. This displays the metadata column. Next, select the **+** sign to add a metadata pair. This pair consists of one key and one value.

![Aggiungere metadati](../media/qnamaker-how-to-edit-kb/add-metadata.png)

> [!TIP]
> Salvare ed eseguire periodicamente il training della Knowledge Base dopo aver apportato le modifiche per evitare che vadano perse.

## <a name="manage-large-knowledge-bases"></a>Gestire Knowledge Base di grandi dimensioni

* **Data source groups**: The QnAs are grouped by the data source from which they were extracted. È possibile espandere o comprimere l'origine dati.

    ![Usare la barra di origine dati di QnA Maker per comprimere ed espandere le risposte e le domande sull'origine dati](../media/qnamaker-how-to-edit-kb/data-source-grouping.png)

* **Search knowledge base**: You can search the knowledge base by typing in the text box at the top of the Knowledge Base table. Premere INVIO per eseguire la ricerca nelle domande, nelle risposte o nei metadati. Fare clic sull'icona X per rimuovere il filtro di ricerca.

    ![Usare la casella di ricerca di QnA Maker sopra alle domande e risposte, per limitare la visualizzazione solo agli elementi corrispondenti al filtro](../media/qnamaker-how-to-edit-kb/search-paginate-group.png)

* **Pagination**: Quickly move through data sources to manage large knowledge bases

    ![Usare le funzionalità di paginazione QnA Maker sopra alle domande e risposte, per spostarsi tra le pagine di domande e risposte](../media/qnamaker-how-to-edit-kb/pagination.png)

## <a name="delete-knowledge-bases"></a>Eliminare le knowledge base

L'eliminazione di una knowledge base (KB) è un'operazione definitiva. Non può essere modificata. Prima di eliminare una knowledge base è necessario esportarla dalla pagina **Impostazioni** del portale di QnA Maker. 

Se si condivide la KB con i [collaboratori](collaborate-knowledge-base.md) e quindi la si elimina, tutti gli utenti perdono l'accesso alla knowledge base. 

## <a name="delete-azure-resources"></a>Eliminare le risorse di Azure 

Se si elimina una delle risorse di Azure usate per le knowledge base di QnA Maker, le knowledge base non funzioneranno più. Prima di eliminare qualsiasi risorsa, assicurarsi di esportare le knowledge base dalla pagina **Impostazioni**. 

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Collaborare a una Knowledge Base](./collaborate-knowledge-base.md)
