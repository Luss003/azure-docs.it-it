---
title: Creare clip con Azure Media Clipper | Microsoft Docs
description: Panoramica di Azure Media Clipper, uno strumento per la creazione di clip multimediali da asset
services: media-services
keywords: clip;clip secondarie;codifica;file multimediali
author: Juliako
manager: femila
ms.author: juliako
ms.date: 03/14/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 51f85dffd48e451b477018ef20491f8619a30f25
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73685007"
---
# <a name="create-clips-with-azure-media-clipper"></a>Creare clip con Azure Media Clipper 

Azure Media Clipper è una libreria JavaScript gratuita che consente agli sviluppatori Web di offrire agli utenti un'interfaccia per la creazione di clip multimediali. Questo strumento può essere integrato in qualsiasi pagina Web e offre API per il caricamento di asset e l'invio di processi di ritaglio.

Azure Media Clipper consente di:
- Tagliare pre-slate e post-slate da archivi live 
- Comporre sintesi video da eventi live AMS, archivi live o file VOD fMP4 
- Concatenare video di più origini 
- Creare clip di riepilogo dagli asset di file multimediali AMS 
- Ritagliare video con precisione al fotogramma 
- Generare filtri manifesto dinamico su asset VOD e live esistenti con precisione al Group of Pictures (GOP) 
- Creare processi di codifica per gli asset nell'account Servizi multimediali

Per richiedere nuove funzionalità e sottoporre idee o commenti e suggerimenti, usare il sito [UserVoice per Servizi multimediali di Azure](https://aka.ms/amsvoice/). In caso di domande o problemi specifici o se si riscontrano bug, scrivere al team di Servizi multimediali all'indirizzo amcinfo@microsoft.com.

L'immagine seguente illustra l'interfaccia di Clipper: ![Azure Media Clipper](media/media-services-azure-media-clipper-overview/media-services-azure-media-clipper-interface.PNG)

## <a name="release-notes"></a>Note sulla versione
Nell'elenco seguente sono riportati il post del blog Clipper, vari problemi noti e il log delle modifiche per l'ultima versione di Clipper:
- [Post di blog](https://azure.microsoft.com/blog/azure-media-clipper/)
- [Elenco dei problemi noti](https://amp.azure.net/libs/amc/latest/docs/known_issues.html)
- [Log delle modifiche](https://amp.azure.net/libs/amc/latest/docs/changelog.html)

## <a name="browser-support"></a>Supporto browser
Azure Media Clipper è basato su moderne tecnologie HTML5 e supporta i browser seguenti:

- Microsoft Edge 13+
- Internet Explorer 11+
- Chrome 54+
- Safari 10+
- Firefox 50+

> [!NOTE]
> Attualmente è supportata solo la riproduzione HTML5 di flussi da Servizi multimediali di Azure.

## <a name="language-support"></a>Supporto per le lingue
Il widget Clipper è disponibile nelle 18 lingue seguenti:
- Cinese (semplificato)
- Cinese (tradizionale)
- Ceco
- Olandese, fiammingo
- Inglese
- Francese
- Tedesco
- Ungherese
- Italiano
- Giapponese
- Coreano
- Polacco
- Portoghese (Brasile)
- Portoghese (Portogallo)
- Russo
- Spagnolo
- Svedese
- Turco

## <a name="next-steps"></a>Passaggi successivi
Per iniziare a usare Azure Media Clipper, vedere l'[articolo introduttivo](media-services-azure-media-clipper-getting-started.md) per informazioni dettagliate su come distribuire il widget.
