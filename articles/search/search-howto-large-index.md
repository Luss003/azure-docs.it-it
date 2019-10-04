---
title: Indicizzare un set di dati di grandi dimensioni usando gli indicizzatori predefiniti - Ricerca di Azure
description: Informazioni sulle strategie per l'indicizzazione di grandi quantità di dati o l'indicizzazione con un utilizzo elevato di risorse di calcolo mediante la modalità batch, l'allocazione delle risorse e tecniche di indicizzazione pianificata, parallela e distribuita.
services: search
author: HeidiSteen
manager: nitinme
ms.service: search
ms.topic: conceptual
ms.date: 09/19/2019
ms.author: heidist
ms.openlocfilehash: aaf0d5edb91d60be85360746f76c4ca1f8db8978
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/25/2019
ms.locfileid: "71257022"
---
# <a name="how-to-index-large-data-sets-in-azure-search"></a>Come indicizzare set di dati di grandi dimensioni in Ricerca di Azure

Man mano che i volumi di dati crescono o si modificano le esigenze, è possibile che le strategie di indicizzazione semplici o predefinite non siano più pratiche. Ricerca di Azure offre diversi metodi per gestire set di dati di grandi dimensioni, dalla strutturazione di una richiesta di caricamento dati all'uso di un indicizzatore specifico dell'origine per carichi di lavoro pianificati e distribuiti.

Le stesse tecniche si applicano anche ai processi con esecuzione prolungata. In particolare, i passaggi descritti nell'[indicizzazione parallela](#parallel-indexing) sono utili per le operazioni di indicizzazione che fanno un uso intensivo di risorse di calcolo, come l'analisi delle immagini o l'elaborazione del linguaggio naturale nelle [pipeline di ricerca cognitiva](cognitive-search-concept-intro.md).

Nelle sezioni seguenti vengono esaminate tre tecniche per l'indicizzazione di grandi quantità di dati.

## <a name="option-1-pass-multiple-documents"></a>Opzione 1: Passare più documenti

Uno dei meccanismi più semplici per l'indicizzazione di set di dati di grandi dimensioni consiste nell'inviare più documenti o record in un'unica richiesta. Se le dimensioni dell'intero payload sono inferiori a 16 MB, una richiesta può gestire fino a 1000 documenti in un'operazione di caricamento in blocco. Questi limiti si applicano se si usa l' [API REST di aggiunta documenti](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) o il [metodo index](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.index?view=azure-dotnet) in .NET SDK. Per entrambe le API, è necessario creare il pacchetto di 1000 documenti nel corpo di ogni richiesta.

L'indicizzazione batch viene implementata per le singole richieste tramite REST o .NET o mediante gli indicizzatori. Alcuni indicizzatori funzionano con limitazioni diverse. In particolare, l'indicizzazione BLOB di Azure limita le dimensioni dei batch a 10 documenti in considerazione delle dimensioni medie superiori dei documenti. Per gli indicizzatori basati sull'[API REST di creazione indicizzatore](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer), è possibile impostare l'argomento `BatchSize` per personalizzare questa impostazione in base alle caratteristiche dei dati. 

> [!NOTE]
> Per ridurre le dimensioni del documento, evitare di aggiungere dati non Queryable a un indice. Le immagini e altri dati binari non sono direttamente disponibili per la ricerca e non devono quindi essere archiviati nell'indice. Per integrare i dati non disponibili per query nei risultati della ricerca, occorre definire un campo non disponibile per la ricerca che archivia un riferimento URL alla risorsa.

## <a name="option-2-add-resources"></a>Opzione 2: Aggiungi risorse

I servizi di cui viene effettuato il provisioning a uno dei [piani tariffari Standard](search-sku-tier.md) hanno spesso una capacità sottoutilizzata sia per l'archiviazione che per i carichi di lavoro (query o indicizzazione), motivo per cui l'[aumento del numero di partizioni e repliche](search-capacity-planning.md) rappresenta la soluzione più ovvia per gestire set di dati di grandi dimensioni. Per ottenere risultati ottimali sono necessarie entrambe le risorse: le partizioni per l'archiviazione e le repliche per l'inserimento dati.

L'aumento delle repliche e delle partizioni è costituito da eventi fatturabili che aumentano il costo, ma a meno che non si inserisca continuamente l'indicizzazione in base al carico massimo, è possibile aggiungere la scalabilità per la durata del processo di indicizzazione e quindi regolare i livelli delle risorse in basso dopo termine.

## <a name="option-3-use-indexers"></a>Opzione 3: Usare gli indicizzatori

Gli [indicizzatori](search-indexer-overview.md) vengono usati per eseguire la ricerca per indicizzazione delle origini dati di Azure supportate. Sebbene non siano appositamente destinati all'indicizzazione su larga scala, diverse funzionalità degli indicizzatori sono particolarmente utili in presenza di set di dati di grandi dimensioni:

+ Le utilità di pianificazione consentono di suddividere l'indicizzazione in intervalli regolari in modo da distribuirne l'esecuzione nel tempo.
+ L'indicizzazione pianificata può ripartire dall'ultimo punto di arresto noto. Se una ricerca per indicizzazione su un'origine dati non viene completata in 24 ore, l'indicizzatore riprenderà l'indicizzazione il secondo giorno a partire dal punto di interruzione.
+ Il partizionamento dei dati in singole origini dati di dimensioni inferiori consente l'elaborazione parallela. È possibile suddividere i dati di origine in componenti più piccoli, ad esempio in più contenitori nell'archivio BLOB di Azure, e quindi creare più [oggetti origine dati](https://docs.microsoft.com/rest/api/searchservice/create-data-source) corrispondenti in ricerca di Azure che possono essere indicizzati in parallelo.

> [!NOTE]
> Gli indicizzatori sono specifici dell'origine dati, quindi l'uso di indicizzatori è possibile solo per origini dati selezionate in Azure: [database SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md), [archivio BLOB](search-howto-indexing-azure-blob-storage.md), [archivio tabelle](search-howto-indexing-azure-tables.md), [Cosmos DB](search-howto-index-cosmosdb.md).

### <a name="scheduled-indexing"></a>Indicizzazione pianificata

La pianificazione degli indicizzatori è un meccanismo importante per l'elaborazione di grandi set di dati, così come per i processi a esecuzione lenta come l'analisi delle immagini in una pipeline di ricerca cognitiva. L'elaborazione degli indicizzatori funziona entro una finestra di 24 ore. Se l'elaborazione non viene completata nell'arco di 24 ore, i comportamenti della pianificazione dell'indicizzatore possono essere usati a proprio vantaggio. 

Per impostazione predefinita, l'indicizzazione pianificata viene avviata a intervalli specifici, in cui un processo viene in genere completato prima di riprendere nell'intervallo pianificato successivo. Tuttavia, se l'elaborazione non viene completata entro l'intervallo, l'indicizzatore si arresta perché il tempo a disposizione è terminato. All'intervallo successivo l'elaborazione riprende da dove si è interrotta perché il sistema tiene traccia del punto in cui si è verificata l'interruzione. 

In pratica per i caricamenti di indici che richiedono più giorni, è possibile inserire l'indicizzatore in una pianificazione di 24 ore. Quando l'indicizzazione riprende per il ciclo di 24 ore successivo, riparte dall'ultimo documento valido noto. In questo modo un indicizzatore può continuare a funzionare tramite un backlog del documento per più giorni fino a quando non vengono elaborati tutti i documenti non elaborati. Per altre informazioni su questo approccio, vedere [Indicizzazione di set di dati di grandi dimensioni](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets). Per altre informazioni sull'impostazione delle pianificazioni in generale, vedere [creare un'API REST dell'indicizzatore](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer#request-syntax) o vedere [come pianificare gli indicizzatori per ricerca di Azure](search-howto-schedule-indexers.md).

<a name="parallel-indexing"></a>

### <a name="parallel-indexing"></a>Indicizzazione parallela

Una strategia di indicizzazione parallela si basa sull'indicizzazione di più origini dati in contemporanea, dove ogni definizione di origine dati specifica un subset dei dati. 

Per requisiti di indicizzazione non comuni e con elevati livelli di calcolo, ad esempio OCR su documenti digitalizzati in una pipeline di ricerca cognitiva, analisi delle immagini o elaborazione del linguaggio naturale, una strategia di indicizzazione parallela è spesso l'approccio giusto per completare un processo a esecuzione prolungata nel più breve tempo possibile. Se è possibile eliminare o ridurre le richieste di query, l'indicizzazione parallela in un servizio che non gestisce contemporaneamente le query rappresenta la strategia migliore per gestire grandi contenuti a elaborazione lenta. 

L'elaborazione parallela prevede le operazioni seguenti:

+ Suddividere i dati di origine in più contenitori o in più cartelle virtuali all'interno dello stesso contenitore. 
+ Eseguire il mapping di ogni mini set di dati alla propria [origine dati](https://docs.microsoft.com/rest/api/searchservice/create-data-source), abbinato al proprio [indicizzatore](https://docs.microsoft.com/rest/api/searchservice/create-indexer).
+ Per la ricerca cognitiva, fare riferimento allo stesso [set di competenze](https://docs.microsoft.com/rest/api/searchservice/create-skillset) in ogni definizione dell'indicizzatore.
+ Scrivere nello stesso indice di ricerca di destinazione. 
+ Pianificare l'esecuzione di tutti gli indicizzatori nello stesso momento.

> [!NOTE]
> Ricerca di Azure non consente di dedicare le repliche o le partizioni a carichi di lavoro specifici. Il rischio di indicizzazioni pesante simultanee sovraccarica il sistema a scapito delle prestazioni di query. In un ambiente di testing implementare prima l'indicizzazione parallela per comprendere i compromessi.

### <a name="how-to-configure-parallel-indexing"></a>Come configurare l'indicizzazione parallela

Per gli indicizzatori la capacità di elaborazione si basa vagamente su un sottosistema dell'indicizzatore per ogni unità di servizio usata dal servizio di ricerca. Nei servizi di Ricerca di Azure con provisioning nei livelli Basic o Standard aventi almeno due repliche sono possibili più indicizzatori simultanei. 

1. Nella pagina **Panoramica** della dashboard del servizio di ricerca del [portale di Azure](https://portal.azure.com) controllare il **Piano tariffario** per confermarne la possibilità di gestione dell'indicizzazione parallela. I livelli Basic e Standard offrono più repliche.

2. In **Impostazioni** > **Scalabilità** [aumentare le repliche](search-capacity-planning.md) per l'elaborazione parallela: una replica aggiuntiva per ogni carico di lavoro dell'indicizzatore. Indicare un numero sufficiente per il volume di query esistente. Sacrificare i carichi di lavoro delle query per l'indicizzazione non è un buon compromesso.

3. Distribuire i dati in più contenitori a un livello raggiungibile dagli indicizzatori di Ricerca di Azure. Potrebbe trattarsi di tabelle multiple nel database SQL di Azure, contenitori multipli nel servizio Archiviazione BLOB di Azure o di raccolte multiple. Definire un oggetto di origine dati per ogni tabella o contenitore.

4. Creare e pianificare indicizzatori multipli da eseguire in parallelo:

   + Si supponga di avere un servizio con sei repliche. Configurare i sei indicizzatori, ognuno dei quali mappato in un'origine dati contenente un sesto del set di dati per una divisione in sei parti dell'intero set di dati. 

   + Puntare ogni indicizzatore allo stesso indice. Per i carichi di lavoro di ricerca cognitivi puntare ogni indicizzatore allo stesso set competenze.

   + In ogni definizione dell'indicizzatore pianificare lo stesso modello della fase di esecuzione. Ad esempio, `"schedule" : { "interval" : "PT8H", "startTime" : "2018-05-15T00:00:00Z" }` crea una pianificazione il 15-05-2018 in tutti gli indicizzatori, con un'esecuzione a intervalli di otto ore.

All'ora pianificata tutti gli indicizzatori avviano l'esecuzione, caricando i dati, applicando gli arricchimenti, se è stata configurata una pipeline di ricerca cognitiva, e avviando operazioni di scrittura nell'indice. Ricerca di Azure non consente di bloccare l'indice per gli aggiornamenti. Vengono gestite le scritture simultanee, con un nuovo tentativo se un'operazione di scrittura specifica non riesce al primo tentativo.

> [!Note]
> Quando si aumentano le repliche, è opportuno aumentare il conteggio delle partizioni se le dimensioni dell'indice sono state pensate per un aumento significativo. Le partizioni archiviano sezioni di contenuto indicizzato: più sono le partizioni, più piccola è la sezione che ognuna deve archiviare.

## <a name="see-also"></a>Vedere anche

+ [Panoramica degli indicizzatori](search-indexer-overview.md)
+ [Indicizzazione nel portale](search-import-data-portal.md)
+ [Indicizzatore del database SQL di Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
+ [Indicizzatore di Azure Cosmos DB](search-howto-index-cosmosdb.md)
+ [Indicizzatore di archiviazione BLOB di Azure](search-howto-indexing-azure-blob-storage.md)
+ [Indicizzatore di archiviazione tabelle di Azure](search-howto-indexing-azure-tables.md)
+ [Sicurezza in Ricerca di Azure](search-security-overview.md)
