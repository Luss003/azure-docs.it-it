---
title: Domande frequenti su Servizi multimediali di Azure v3 | Microsoft Docs
description: Questo articolo fornisce le risposte alle domande frequenti su Servizi multimediali di Azure v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/09/2020
ms.author: juliako
ms.openlocfilehash: a2619293bf3641cdca370ff528a87ae879460a3b
ms.sourcegitcommit: 20429bc76342f9d365b1ad9fb8acc390a671d61e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/11/2020
ms.locfileid: "79086788"
---
# <a name="media-services-v3-frequently-asked-questions"></a>Domande frequenti su servizi multimediali V3

Questo articolo contiene le risposte alle domande frequenti su Servizi multimediali di Azure (AMS) v3.

## <a name="general"></a>Generale

### <a name="what-azure-roles-can-perform-actions-on-azure-media-services-resources"></a>Quali ruoli di Azure possono eseguire azioni sulle risorse di servizi multimediali di Azure? 

Vedere [controllo degli accessi in base al ruolo (RBAC) per gli account di servizi multimediali](rbac-overview.md).

### <a name="how-do-you-stream-to-apple-ios-devices"></a>Come si esegue lo streaming su dispositivi Apple iOS?

Assicurarsi di avere "(format = m3u8-aapl)" alla fine del percorso (dopo la parte "/manifest" dell'URL) per indicare al server di origine di streaming di restituire il contenuto HLS per l'utilizzo nei dispositivi Apple iOS nativi. per informazioni dettagliate, vedere [distribuzione di contenuto](dynamic-packaging-overview.md).

### <a name="how-do-i-configure-media-reserved-units"></a>Come si esegue la configurazione di Media Reserved Units?

Per i processi di analisi audio e video generati da Servizi multimediali v3 o Video Indexer, è fortemente consigliato effettuare il provisioning dell'account con 10 MRU S3. Se sono necessarie più di 10 MRU S3, aprire un ticket di supporto dal [portale di Azure](https://portal.azure.com/).

Per informazioni dettagliate, vedere [Ridimensionamento dell'elaborazione di contenuti multimediali con l'interfaccia della riga di comando](media-reserved-units-cli-how-to.md).

### <a name="what-is-the-recommended-method-to-process-videos"></a>Qual è il metodo consigliato per l'elaborazione dei processi?

Usare [Trasformazioni](https://docs.microsoft.com/rest/api/media/transforms) per configurare attività comuni relative alla codifica o all'analisi dei video. Ogni **trasformazione** descrive un recipe o un flusso di lavoro di attività per l'elaborazione dei file video o audio. Un [processo](https://docs.microsoft.com/rest/api/media/jobs) è la richiesta effettiva a servizi multimediali di applicare la **trasformazione** a un video di input o a un contenuto audio specifico. Dopo aver creato la trasformazione, è possibile inviare i processi usando le API di Servizi multimediali o uno degli SDK pubblicati. Per altre informazioni, vedere [Trasformazioni e processi](transforms-jobs-concept.md).

### <a name="i-uploaded-encoded-and-published-a-video-what-would-be-the-reason-the-video-does-not-play-when-i-try-to-stream-it"></a>Ho caricato, codificato e pubblicato un video. Quale può essere il motivo per cui il video non viene riprodotto quando provo a trasmetterlo in streaming?

Uno dei motivi più comuni è che non si ha l'endpoint di streaming da cui si tenta di riprodurlo nello stato Running.

### <a name="how-does-pagination-work"></a>Come funziona la paginazione?

Quando si usa la paginazione, usare sempre il collegamento seguente per enumerare la raccolta e non dipendere da una determinata dimensione di pagina. Per informazioni dettagliate ed esempi, vedere [Filtro, ordinamento, restituzione di più pagine](entities-overview.md).

### <a name="what-features-are-not-yet-available-in-azure-media-services-v3"></a>Quali funzionalità non sono ancora disponibili in servizi multimediali di Azure V3?

Per informazioni dettagliate, vedere [gap delle funzionalità rispetto alle API v2](media-services-v2-vs-v3.md#feature-gaps-with-respect-to-v2-apis).

### <a name="what-is-the-process-of-moving-a-media-services-account-between-subscriptions"></a>Qual è il processo di trasferimento di un account di servizi multimediali tra le sottoscrizioni?  

Per informazioni dettagliate, vedere [trasferimento di un account di servizi multimediali tra sottoscrizioni](media-services-account-concept.md).

## <a name="live-streaming"></a>Streaming live 

###  <a name="how-to-insert-breaksvideos-and-image-slates-during-live-stream"></a>Come è possibile inserire interruzioni/video e slate immagine durante lo streaming live?

La codifica live di Servizi multimediali v3 non supporta ancora l'inserimento di slate immagine o video durante lo streaming live. 

È possibile usare un [codificatore locale live](recommended-on-premises-live-encoders.md) per commutare il video di origine. Molte app offrono la possibilità di commutare le origini, tra cui Telestream Wirecast, Switcher Studio (in iOS), OBS Studio (app gratuita) e molte altre.

## <a name="content-protection"></a>Protezione del contenuto

### <a name="should-i-use-an-aes-128-clear-key-encryption-or-a-drm-system"></a>È consigliabile usare una crittografia con chiave non crittografata AES-128 o un sistema DRM?

I clienti spesso si chiedono se devono usare la crittografia AES o un sistema DRM. La differenza principale tra i due sistemi è che con la crittografia AES la chiave simmetrica viene trasmessa al client su TLS, in modo che la chiave sia crittografata in transito, ma senza alcuna crittografia aggiuntiva ("in chiaro"). Di conseguenza, la chiave usata per decrittografare il contenuto è accessibile al lettore client e può essere visualizzata in una traccia di rete sul client in testo normale. Una crittografia con chiave non crittografata AES-128 è adatta per i casi d'uso in cui il visualizzatore è attendibile (ad esempio, la crittografia dei video aziendali distribuiti all'interno di una società per la visualizzazione da parte dei dipendenti).

I sistemi DRM come PlayReady, Widevine e FairPlay forniscono tutti un livello aggiuntivo di crittografia sulla chiave usata per decrittografare il contenuto rispetto a una chiave non crittografata AES-128. La chiave simmetrica viene crittografata in una chiave protetta dal runtime di DRM in aggiunta a qualsiasi crittografia a livello di trasporto fornita da TLS. Inoltre, la decrittografia viene gestita in un ambiente sicuro a livello di sistema operativo, più difficilmente attaccabile da un utente malintenzionato. Il DRM è consigliato per i casi d'uso in cui il visualizzatore potrebbe non essere attendibile ed è necessario il massimo livello di sicurezza.

### <a name="how-to-show-a-video-only-to-users-who-have-a-specific-permission-without-using-azure-ad"></a>Come mostrare un video solo agli utenti che dispongono di un'autorizzazione specifica, senza usare Azure AD?

Non è necessario usare un provider di token specifico, ad esempio Azure AD. È possibile creare un provider [JWT](https://jwt.io/) personalizzato, denominato STS, servizio token di sicurezza, usando la crittografia a chiave asimmetrica. Nel servizio token di protezione personalizzato è possibile aggiungere attestazioni in base alla logica di business.

Verificare che l'autorità emittente, il gruppo di destinatari e le attestazioni corrispondano esattamente a quelli presenti in JWT e ContentKeyPolicyRestriction usati in ContentKeyPolicy.

Per altre informazioni, vedere [proteggere il contenuto usando la crittografia dinamica di servizi multimediali](content-protection-overview.md).

### <a name="how-and-where-to-get-jwt-token-before-using-it-to-request-license-or-key"></a>Come e dove si ottiene il token JWT prima di usarlo per richiedere la licenza o la chiave?

1. Per la produzione, è necessario disporre di un servizio token di sicurezza (STS) (servizio Web) che rilascia il token JWT su una richiesta HTTPS. Per i test, è possibile usare il codice illustrato nel metodo **GetTokenAsync** definito in [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs).
2. Dopo l'autenticazione di un utente, il lettore dovrà richiedere al servizio token di sicurezza tale token e assegnarlo come valore del token. È possibile usare l'[API di Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/).

* Per un esempio di esecuzione del servizio token di sicurezza, con una chiave di crittografia simmetrica e asimmetrica, vedere [https://aka.ms/jwt](https://aka.ms/jwt). 
* Per un esempio di lettore basato su Azure Media Player con tale token JWT, vedere [https://aka.ms/amtest](https://aka.ms/amtest) (espandere il collegamento "player_settings" per visualizzare l'input del token).

### <a name="how-do-you-authorize-requests-to-stream-videos-with-aes-encryption"></a>Come si autorizzano le richieste di streaming di video con la crittografia AES?

L'approccio corretto consiste nell'usare il servizio token di sicurezza:

Nel servizio token di sicurezza, a seconda del profilo dell'utente, aggiungere attestazioni diverse (ad esempio "Utente Premium", "Utente Basic", "Utente di prova gratuita"). Con diverse attestazioni in un token JWT, l'utente può visualizzare diversi tipi di contenuto. Naturalmente, per contenuto/risorse diverse, ContentKeyPolicyRestriction avrà il valore RequiredClaims corrispondente.

Usare le API di servizi multimediali di Azure per la configurazione del recapito di licenze/chiavi e la crittografia degli asset (come illustrato in [questo esempio](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES/Program.cs)).

Per altre informazioni, vedere:

- [Panoramica della protezione del contenuto](content-protection-overview.md)
- [Progettazione di un sistema di protezione del contenuto con DRM multiplo e controllo di accesso](design-multi-drm-system-with-access-control.md)

### <a name="http-or-https"></a>HTTP o HTTPS?
L'applicazione lettore MVC ASP.NET deve supportare quanto segue:

* Autenticazione utente con Azure AD, che deve usare il protocollo HTTPS.
* Scambio di token JWT tra il client e Azure AD, che deve usare il protocollo HTTPS.
* Acquisizione della licenza DRM dal client, che deve essere gestita tramite protocollo HTTPS se la distribuzione delle licenze avviene tramite Servizi multimediali. La famiglia di prodotti PlayReady non obbliga a usare il protocollo HTTPS per la distribuzione delle licenze. Se il server delle licenze PlayReady non rientra nei Servizi multimediali, usare il protocollo HTTP o HTTPS.

L'applicazione lettore ASP.NET usa il protocollo HTTPS come procedura consigliata, quindi Media Player si trova in una pagina in HTTPS. Tuttavia, per lo streaming è preferibile usare il protocollo HTTP. È quindi necessario considerare il problema del contenuto misto.

* Il browser non consente il contenuto misto, ma lo consentono i plug-in come Silverlight e OSMF per Smooth e DASH. Il contenuto misto pone una questione di sicurezza a causa della minaccia derivante dalla possibilità di inserire codice JavaScript dannoso che può mettere a rischio i dati dei clienti. Per impostazione predefinita, il browser blocca questa funzionalità. Finora il solo modo per aggirare il problema è intervenire sul lato server (origine), per consentire tutti i domini (sia HTTPS che HTTP), ma probabilmente nemmeno questa è una buona idea.
* Evitare il contenuto misto. Sia l'applicazione lettore che Media Player devono usare il protocollo HTTP o HTTPS. Quando gestisce il contenuto misto, la tecnologia silverlightSS richiede la cancellazione di un avviso di contenuto misto. La tecnologia flashSS gestisce il contenuto misto senza avvisi di contenuto misto.
* Se l'endpoint di streaming è stato creato prima di agosto 2014, non supporterà il protocollo HTTPS. In questo caso, creare e usare un nuovo endpoint di streaming per il protocollo HTTPS.

### <a name="what-about-live-streaming"></a>Come funziona lo streaming live?

È possibile usare esattamente la stessa progettazione e la stessa implementazione per proteggere lo streaming live in Servizi multimediali, trattando l'asset associato a un programma come asset video on demand (VOD). Per fornire una protezione DRM multipla del contenuto Live, applicare la stessa configurazione/elaborazione all'asset come se si trattasse di un asset VOD prima di associare l'asset all'output Live.

### <a name="what-about-license-servers-outside-media-services"></a>Come funzionano i server licenze all'esterno di Servizi multimediali?

I clienti spesso investono in una farm di server licenze nel proprio data center oppure ospitata da provider di servizi DRM. Con la protezione del contenuto di Servizi multimediali, è possibile operare in modalità ibrida. È possibile ospitare e proteggere dinamicamente i contenuti in Servizi multimediali, mentre le licenze DRM vengono distribuite dal server all'esterno di Servizi multimediali. In questo caso, tenere presenti le modifiche seguenti:

* Il servizio token di sicurezza deve rilasciare token che siano accettabili e verificabili dalla farm di server licenze. Ad esempio, i server licenze Widevine forniti da Axinom richiedono uno specifico token JWT contenente un elemento "entitlement message". Per rilasciare tale token JWT, è quindi necessario un servizio token di sicurezza. 
* Non è più necessario configurare il servizio di distribuzione delle licenze in Servizi multimediali. È sufficiente fornire gli URL di acquisizione delle licenze (per PlayReady, Widevine e FairPlay) quando si configura ContentKeyPolicies.

> [!NOTE]
> Widevine è un servizio fornito da Google Inc. e soggetto alle condizioni per l'utilizzo e all'informativa sulla privacy di Google Inc.

## <a name="media-services-v2-vs-v3"></a>Servizi multimediali v2 e v3 

### <a name="can-i-use-the-azure-portal-to-manage-v3-resources"></a>È possibile usare il portale di Azure per gestire le risorse della versione v3?

Attualmente, è possibile utilizzare il [portale di Azure](https://portal.azure.com/) per:

* gestione [degli eventi live](live-events-outputs-concept.md)di servizi multimediali V3 
* visualizzare (non gestire) gli [Asset](assets-concept.md)V3, 
* [ottenere informazioni sull'accesso alle API](access-api-portal.md). 

Per tutte le altre attività di gestione (ad esempio, [trasformazioni e processi](transforms-jobs-concept.md) e [protezione del contenuto](content-protection-overview.md)), usare l' [API REST](https://aka.ms/ams-v3-rest-ref), l' [interfaccia](https://aka.ms/ams-v3-cli-ref)della riga di comando o uno degli [SDK](media-services-apis-overview.md#sdks)supportati.

### <a name="is-there-an-assetfile-concept-in-v3"></a>È presente un concetto AssetFile nella versione v3?

Le entità AssetFile sono state rimosse dall'API AMS per separare Servizi multimediali dalla dipendenza da Storage SDK. Le informazioni che appartengono a Storage SDK vengono ora conservate in Storage e non in Servizi multimediali. 

Per altre informazioni, vedere [Eseguire la migrazione a Servizi multimediali v3](media-services-v2-vs-v3.md).

### <a name="where-did-client-side-storage-encryption-go"></a>Dove è finita la crittografia di archiviazione lato client?

È ora consigliabile usare la crittografia di archiviazione lato server (che è attivata per impostazione predefinita). Per altre informazioni, vedere [Crittografia del servizio di archiviazione di Azure per dati inattivi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

## <a name="next-steps"></a>Passaggi successivi

[Panoramica di Servizi multimediali v3](media-services-overview.md)
