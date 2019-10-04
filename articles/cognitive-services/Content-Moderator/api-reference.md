---
title: Riferimento API - Content Moderator
titleSuffix: Azure Cognitive Services
description: Informazioni sulle varie API di moderazione del contenuto e di revisione per Content Moderator.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: reference
ms.date: 05/29/2019
ms.author: sajagtap
ms.openlocfilehash: 3ad911a95dbe6209fcf55adcac3cf2937b06d1ff
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/26/2019
ms.locfileid: "68565622"
---
# <a name="content-moderator-api-reference"></a>Informazioni di riferimento per l'API Content Moderator

È possibile iniziare a usare le API di Azure Content Moderator nei modi seguenti:

- Nel portale di Azure sottoscrivere [l'API content moderator](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator).
- Vedere [provare content moderator sul Web](quick-start.md) per iscriversi con lo [strumento content moderator Review](https://contentmoderator.cognitive.microsoft.com/).

## <a name="moderation-apis"></a>API per la moderazione

È possibile usare le API Content Moderator seguenti per configurare i flussi di lavoro post-moderazione.

| DESCRIZIONE | Riferimenti |
| -------------------- |-------------|
| **API Moderazione immagini**<br /><br />Consente di analizzare le immagini e rilevare potenziali contenuti spinti e per adulti usando tag, punteggi di attendibilità e altre informazioni estratte. <br /><br />Usare queste informazioni per pubblicare, rifiutare o rivedere il contenuto nel flusso di lavoro post-moderazione. <br /><br />| [Informazioni di riferimento per l'API Moderazione immagini](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c "Informazioni di riferimento per l'API Moderazione immagini")   |
| **API Moderazione testo**<br /><br />Consente di analizzare il contenuto di testo. Vengono restituiti i termini di volgarità e i dati personali. <br /><br />Usare queste informazioni per pubblicare, rifiutare o rivedere il contenuto nel flusso di lavoro post-moderazione.<br /><br /> | [Informazioni di riferimento per l'API Moderazione testo](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f "Informazioni di riferimento per l'API Moderazione testo")   |
| **API Moderazione video**<br /><br />Consente di analizzare i video e rilevare potenziali contenuti spinti e per adulti. <br /><br />Usare queste informazioni per pubblicare, rifiutare o rivedere il contenuto nel flusso di lavoro post-moderazione.<br /><br /> | [Panoramica dell'API Moderazione video](video-moderation-api.md "Panoramica dell'API Moderazione video")   |
| **API Gestione elenchi**<br /><br />Consente di creare e gestire elenchi di esclusione e inclusione personalizzati di immagini e testo. Se abilitata, le operazioni **Image - Match** (Immagine - Corrispondenza) e **Text - Screen** (Testo - Filtra) eseguono una corrispondenza fuzzy del contenuto inviato rispetto agli elenchi personalizzati. <br /><br />Per una maggiore efficienza, è possibile saltare il passaggio relativo alla moderazione basata su machine learning.<br /><br /> | [Informazioni di riferimento per l'API Gestione elenchi](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f675 "Informazioni di riferimento per l'API Gestione elenchi")   |

## <a name="review-apis"></a>Verificare le API

Le API di revisione includono i componenti seguenti:

| DESCRIZIONE | Riferimenti |
| -------------------- |-------------|
| **Processi**<br /><br /> È possibile avviare flussi di lavoro di moderazione di analisi e revisione per il contenuto di immagini e testo. Il processo di moderazione analizza il contenuto usando l'API Moderazione immagini e l'API Moderazione testo. Usa i flussi di lavoro definiti e predefiniti per generare revisioni. <br /><br />Dopo che un moderatore umano ha esaminato i tag assegnati automaticamente e i dati di stima e ha inviato una decisione relativa alla moderazione del contenuto, l'API di revisione invia tutte le informazioni all'endpoint API.<br /><br /> | [Informazioni di riferimento per i processi](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5 "Informazioni di riferimento per i processi")   |
| **Revisioni**<br /><br />Usare lo strumento di revisione per creare direttamente revisioni di immagini o testo per i moderatori umani.<br /><br /> Dopo che un moderatore umano ha esaminato i tag assegnati automaticamente e i dati di stima e ha inviato una decisione relativa alla moderazione del contenuto, l'API di revisione invia tutte le informazioni all'endpoint API.<br /><br /> | [Informazioni di riferimento per le revisioni](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4 "Informazioni di riferimento per le revisioni")   |
| **Flussi di lavoro**<br /><br />È possibile creare, aggiornare e ottenere i dettagli sui flussi di lavoro personalizzati creati dal team. Per definire i flussi di lavoro si usa lo strumento di revisione. <br /> <br />I flussi di lavoro in genere usano Content Moderator, ma possono usare anche determinate altre API disponibili come connettori nello strumento di revisione.<br /><br /> | [Informazioni di riferimento per i flussi di lavoro](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59 "Informazioni di riferimento per i flussi di lavoro")   |