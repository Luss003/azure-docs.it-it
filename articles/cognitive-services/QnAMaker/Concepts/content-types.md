---
title: Tipi di contenuto-QnA Maker
description: I tipi di contenuto includono molti documenti strutturati standard, ad esempio PDF, DOC e TXT.
services: cognitive-services
ms.topic: conceptual
ms.date: 02/24/2020
ms.openlocfilehash: 7c78f9ea261fa636cce50b69524802d0900e9d7b
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/27/2020
ms.locfileid: "77650198"
---
# <a name="content-types-of-documents-you-can-add-to-a-knowledge-base"></a>Tipi di contenuto dei documenti che è possibile aggiungere a una Knowledge base
I tipi di contenuto includono molti documenti strutturati standard, ad esempio PDF, DOC e TXT.

## <a name="file-and-url-data-types"></a>Tipi di dati di file e URL

La tabella seguente riepiloga i tipi di contenuto e di formato di file supportati da QnA Maker.

|Tipo di origine|Content Type| Esempi|
|--|--|--|
|URL|Domande frequenti<br> (con struttura piatta, a sezioni o con collegamenti ad altre pagine)<br>Pagine del supporto <br> (singola pagina di procedure dettagliate, articoli sulla risoluzione dei problemi e così via).|[Domande frequenti semplici](https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs), <br>[domande frequenti con collegamenti](https://www.microsoft.com/en-us/software-download/faq),<br> [domande frequenti con home page degli argomenti](https://www.microsoft.com/Licensing/servicecenter/Help/Faq.aspx)<br>[articolo del supporto tecnico](https://docs.microsoft.com/azure/cognitive-services/qnamaker/concepts/best-practices)|
|PDF/DOC|domande frequenti,<br> manuale del prodotto,<br> brochure,<br> documento,<br> volantino,<br> guida di supporto,<br> file domanda-risposta strutturato,<br> e così via.|**Senza multiturni**<br>[File domanda-risposta strutturato](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/structured.docx),<br> [Sample Product Manual.pdf](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf),<br> [Sample semi-structured.doc](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/semi-structured.docx),<br> [Esempio white paper. pdf](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/white-paper.pdf),<br><br>**Multiturn**:<br>[Surface Pro (docx)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/multi-turn.docx)<br>[Vantaggi di Contoso (docx)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-ContosoBenefits.docx)<br>[Vantaggi di Contoso (PDF)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-ContosoBenefits.pdf)|
|\* Excel|File domanda-risposta strutturato<br> (tra cui RTF, supporto HTML)|[Sample QnA FAQ.xls](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/QnA%20Maker%20Sample%20FAQ.xlsx)|
|\* TXT/TSV|File domanda-risposta strutturato|[Esempio chit-chat.tsv](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Scenario_Responses_Friendly.tsv)|

Se è necessaria l'autenticazione per l'origine dati, considerare i metodi seguenti per ottenere tale contenuto in QnA Maker:

* Scaricare manualmente il file e importarlo in QnA Maker
* Aggiungere un file da un [percorso di SharePoint](../how-to/add-sharepoint-datasources.md) autenticato

## <a name="url-content"></a>Contenuto URL

È possibile importare due tipi di documenti tramite l' **URL** in QnA Maker:

* Indirizzo Web di domande frequenti
* URL di supporto

Ogni tipo indica un formato previsto.

## <a name="file-based-content"></a>Contenuto basato su file

È possibile aggiungere file a una Knowledge base da un'origine pubblica o dalla file system locale nel [portale di QnA Maker](https://www.qnamaker.ai).

## <a name="content-format-guidelines"></a>Linee guida per il formato del contenuto

Altre informazioni sulle [linee guida](../reference-document-format-guidelines.md) per il formato dei diversi file.

## <a name="next-steps"></a>Passaggi successivi

Informazioni sulle informazioni archiviate in un [set di domande e risposte (QnA)](question-answer-set.md).