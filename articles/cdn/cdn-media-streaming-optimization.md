---
title: Ottimizzazione dello streaming multimediale con la rete CDN di Azure
description: Ottimizzazione dei file di streaming multimediale per una distribuzione uniforme
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2018
ms.author: magattus
ms.openlocfilehash: c6ed546735058e330368151adb0df7323f943050
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593660"
---
# <a name="media-streaming-optimization-with-azure-cdn"></a>Ottimizzazione dello streaming multimediale con la rete CDN di Azure 
 
L'uso di video ad alta definizione sta aumentando in Internet e questo crea problemi per la distribuzione efficiente dei file di grandi dimensioni. L'aspettativa dei clienti è quella di poter riprodurre in modo uniforme gli asset video live o i video on demand su una vasta gamma di client e reti in tutto il mondo. Un meccanismo di distribuzione veloce ed efficiente per i file di streaming multimediale è fondamentale per garantire un'esperienza utente uniforme e piacevole.  

La distribuzione dello streaming live è particolarmente difficile perché i file hanno grandi dimensioni e il numero di utenti simultanei è elevato. Gli utenti abbandonano a causa di ritardi eccessivi. Poiché lo streaming live non può essere memorizzato in anticipo nella cache e le latenze di grandi dimensioni non sono accettabili per i visualizzatori, i frammenti video devono essere distribuiti in modo tempestivo. 

I modelli di richiesta dello streaming pongono alcune nuove sfide. Quando vengono rilasciati uno streaming live famoso o una nuova serie di video on demand, migliaia o persino milioni di visualizzatori potrebbero richiedere lo streaming nello stesso momento. In questo caso, il consolidamento intelligente delle richieste è essenziale per non sovraccaricare i server di origine quando gli asset non sono stati ancora memorizzati nella cache.
 

## <a name="media-streaming-optimizations-for-azure-cdn-from-microsoft"></a>Ottimizzazioni dello streaming multimediale per la rete CDN di Azure di Microsoft

Gli endpoint della **rete CDN Standard di Azure di Microsoft** distribuiscono risorse multimediali in streaming direttamente usando il tipo di ottimizzazione offerto dalla distribuzione Web generale. 

L'ottimizzazione dello streaming multimediale per la **rete CDN Standard di Azure di Microsoft** è efficace per lo streaming multimediale live o di video on demand che usa singoli frammenti multimediali per la distribuzione. Questo processo è diverso da quello di trasferimento di un singolo asset di grandi dimensioni tramite il download progressivo oppure tramite l'uso di richieste di intervallo di byte. Per informazioni su questo stile di distribuzione di file multimediali, vedere [Ottimizzazione del download di file di grandi dimensioni mediante la rete CDN di Azure](cdn-large-file-optimization.md).

I tipi di ottimizzazione basati sulla distribuzione di file multimediali generale e di video on demand usano una rete per la distribuzione di contenuti (CDN, Content Delivery Network) con ottimizzazioni back-end per distribuire le risorse multimediali più velocemente. Vengono anche usate le configurazioni per le risorse di file multimediali basate sulle procedure consigliate apprese nel tempo.

### <a name="partial-cache-sharing"></a>Condivisione della cache parziale
La condivisione della cache parziale consente alla rete per la distribuzione di contenuti di offrire contenuto memorizzato nella cache parziale per le nuove richieste. Ad esempio, la prima richiesta alla rete CDN genera un mancato riscontro nella cache e la richiesta viene inviata all'origine. Sebbene questo contenuto incompleto sia caricato nella cache della rete CDN, altre richieste alla rete CDN possono iniziare a recuperare questi dati. 


## <a name="media-streaming-optimizations-for-azure-cdn-from-verizon"></a>Ottimizzazioni dello streaming multimediale per la rete CDN di Azure fornita da Verizon

Gli endpoint della **rete CDN Standard di Azure di Verizon** e della **rete CDN Premium di Azure di Verizon** distribuiscono le risorse multimediali in streaming direttamente usando il tipo di ottimizzazione offerto dalla distribuzione Web generale. Per impostazione predefinita, nella rete CDN sono disponibili anche alcune funzionalità utili per la distribuzione di asset multimediali.

### <a name="partial-cache-sharing"></a>Condivisione della cache parziale

La condivisione della cache parziale consente alla rete per la distribuzione di contenuti di offrire contenuto memorizzato nella cache parziale per le nuove richieste. Ad esempio, la prima richiesta alla rete CDN genera un mancato riscontro nella cache e la richiesta viene inviata all'origine. Sebbene questo contenuto incompleto sia caricato nella cache della rete CDN, altre richieste alla rete CDN possono iniziare a recuperare questi dati. 

### <a name="cache-fill-wait-time"></a>Tempo di attesa di riempimento della cache

 La funzionalità di tempo di attesa di riempimento della cache forza il server perimetrale a contenere le richieste successive per la stessa risorsa fino alla ricezione delle intestazioni di risposta HTTP da parte del server di origine. Se le intestazioni di risposta HTTP dal server di origine vengono ricevute prima della scadenza del timer, tutte le richieste messe in attesa verranno servite dalla cache con i dati in aumento. Allo stesso tempo la cache viene riempita con i dati provenienti dall'origine. Per impostazione predefinita, il tempo di attesa di riempimento della cache è 3.000 millisecondi. 

 
## <a name="media-streaming-optimizations-for-azure-cdn-from-akamai"></a>Ottimizzazioni dello streaming multimediale per la rete CDN di Azure fornita da Akamai
 
La **rete CDN Standard di Azure di Akamai** offre una funzionalità per la distribuzione efficiente delle risorse multimediali in streaming su scala agli utenti in tutto il mondo. Questa funzionalità riduce le latenze perché riduce il carico sui server di origine. Questa funzionalità è disponibile con il piano tariffario dello standard Akamai. 

L'ottimizzazione dello streaming multimediale per la **rete CDN Standard di Azure di Akamai** è efficace per lo streaming multimediale live o di video on demand che usa singoli frammenti multimediali per la distribuzione. Questo processo è diverso da quello di trasferimento di un singolo asset di grandi dimensioni tramite il download progressivo oppure tramite l'uso di richieste di intervallo di byte. Per informazioni su questo stile di distribuzione di file multimediali, vedere [Ottimizzazione di file di grandi dimensioni](cdn-large-file-optimization.md).

I tipi di ottimizzazione per la distribuzione di file multimediali generali e di video on demand usano una rete per la distribuzione di contenuti con ottimizzazioni back-end per distribuire le risorse multimediali più velocemente. Vengono anche usate le configurazioni per le risorse di file multimediali basate sulle procedure consigliate apprese nel tempo.

### <a name="configure-an-akamai-cdn-endpoint-to-optimize-media-streaming"></a>Configurare un endpoint della rete CDN Akamai per l'ottimizzazione dello streaming di file multimediali
 
È possibile configurare l'endpoint della rete di distribuzione di contenuti (CDN) per ottimizzare il recapito per i file di grandi dimensioni tramite il portale di Azure. A questo scopo, è anche possibile usare le API REST o qualsiasi SDK client. I passaggi seguenti mostrano il processo tramite il portale di Azure per un profilo di **rete CDN Standard di Azure di Akamai**:

1. Per aggiungere un nuovo endpoint, nella pagina **Profilo CDN** di Akamai selezionare **Endpoint**.
  
    ![Nuovo endpoint](./media/cdn-media-streaming-optimization/cdn-new-akamai-endpoint.png)

2. Nell'elenco a discesa **Optimized for** (Ottimizzato per) selezionare **Streaming multimediale video on demand** per le risorse video on demand. Se si desidera creare una combinazione di streaming di video on demand e live, selezionare **Streaming multimediale generale**.

    ![Streaming selezionato](./media/cdn-media-streaming-optimization/02_Creating.png) 
 
Dopo avere creato l'endpoint, questo applicherà l'ottimizzazione per tutti i file che corrispondono a determinati criteri. La sezione seguente descrive questo processo. 

### <a name="caching"></a>Memorizzazione nella cache

Se la **rete CDN Standard di Azure di Akamai** rileva che la risorsa è un manifesto o un frammento di streaming, usa tempi di scadenza per la memorizzazione nella cache diversi rispetto alla distribuzione Web generale. Vedere l'elenco completo nella tabella seguente. Come sempre, vengono rispettate i controlli della cache o le intestazioni di scadenza inviate dall'origine. Se la risorsa non è una risorsa multimediale, viene memorizza nella cache con i tempi di scadenza per la distribuzione Web generale.

I tempi negativi e brevi di memorizzazione nella cache sono utili per l'offload di origine quando molti utenti richiedono un frammento che non esiste ancora. Ne è un esempio uno streaming live in cui i pacchetti non sono disponibili dall'origine in quel momento. Un intervallo di memorizzazione nella cache più lungo consente anche l'offload delle richieste dall'origine poiché il contenuto video in genere non viene modificato.
 

|   | Distribuzione Web generale | Streaming multimediale generale | Streaming multimediale di video on demand  
--- | --- | --- | ---
Caching: Positive <br> HTTP 200, 203, 300, <br> 301, 302 e 410 | 7 giorni |365 giorni | 365 giorni   
Caching: Negative <br> HTTP 204, 305, 404, <br> e 405 | Nessuna | 1 secondo | 1 secondo
 
### <a name="deal-with-origin-failure"></a>Gestire gli errori di origine  

La distribuzione di file multimediali generali e di video on demand prevede il timeout dell'origine e il log di tentativi in base alle procedure consigliate per i modelli di richiesta tipici. Ad esempio, poiché la distribuzione di file multimediali generale riguarda contenuti sia live che di video on demand, il timeout di connessione è più breve poiché lo streaming live dipende dall'ora.

Quando si verifica il timeout di una connessione, la rete CDN esegue un certo numero di tentativi prima di inviare l'errore "504 - Timeout gateway" al client. 

Quando un file corrisponde all'elenco di condizioni per la dimensione e il tipo di file, la rete per la distribuzione di contenuti usa il comportamento per lo streaming multimediale. In caso contrario, usa la distribuzione Web generale.
   
### <a name="conditions-for-media-streaming-optimization"></a>Condizioni per l'ottimizzazione dello streaming multimediale 

La tabella seguente elenca il set di criteri che devono essere soddisfatti per l'ottimizzazione dello streaming multimediale: 
 
Tipi di streaming supportati | Estensioni di file  
--- | ---  
Apple HLS | m3u8, m3u, m3ub, key, ts, aac
Adobe HDS | f4m, f4x, drmmeta, bootstrap, f4f,<br>Struttura URL Seg-Frag <br> (espressione regolare corrispondente: ^(/.*)Seq(\d+)-Frag(\d+)
DASH | mpd, dash, divx, ismv, m4s, m4v, mp4, mp4v, <br> sidx, webm, mp4a, m4a, isma
Smooth Streaming | /manifest/, /QualityLevels/Fragments/
  
 
