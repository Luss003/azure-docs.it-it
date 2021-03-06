---
title: Annunci di nuove funzionalità
titleSuffix: Azure Cognitive Search
description: Annunci di funzionalità nuove e migliorate, tra cui il servizio Ricerca di Azure rinominato in Ricerca cognitiva di Azure.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: overview
ms.date: 03/03/2020
ms.openlocfilehash: 27dae07328af125c25512ab9f1eb81d0f4eda99b
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2020
ms.locfileid: "78271314"
---
# <a name="whats-new-in-azure-cognitive-search"></a>Novità di Ricerca cognitiva di Azure

Ecco cosa c'è di nuovo nel servizio. Aggiungere un segnalibro a questa pagina per rimanere sempre aggiornati sul servizio.

<a name="new-service-name"></a>

## <a name="new-service-name"></a>Nuovo nome del servizio

Il servizio Ricerca di Azure è stato rinominato in **Ricerca cognitiva di Azure** per riflettere l'uso più ampio (ancora facoltativo) delle competenze cognitive e dell'elaborazione di intelligenza artificiale nelle operazioni principali. Le versioni delle API, i pacchetti NuGet, gli spazi dei nomi e gli endpoint sono rimasti invariati. Le soluzioni di ricerca nuove ed esistenti non sono interessate dalla modifica del nome del servizio.

## <a name="feature-announcements"></a>Annunci di funzionalità

### <a name="february-2020"></a>Febbraio 2020

+ Il [rilevamento di informazioni personali (anteprima)](cognitive-search-skill-pii-detection.md) è una competenza cognitiva usata durante l'indicizzazione che estrae informazioni personali da un testo di input e offre diverse opzioni per mascherarle.

+ La [ricerca di entità personalizzate (anteprima)](cognitive-search-skill-custom-entity-lookup.md ) cerca il testo in un elenco personalizzato di parole e frasi definito dall'utente. Con questo elenco vengono etichettati tutti i documenti con entità corrispondenti. La competenza supporta anche un grado di corrispondenza fuzzy che può essere applicato per trovare corrispondenze simili ma non proprio esatte. 

### <a name="january-2020"></a>Gennaio 2020

+ Le [chiavi di crittografia gestite dal cliente](search-security-manage-encryption-keys.md) sono ora disponibili a livello generale. Se si usa REST, è possibile accedere alla funzionalità tramite `api-version=2019-05-06`. Per il codice gestito, il pacchetto corretto è ancora [.NET SDK versione 8.0-preview](search-dotnet-sdk-migration-version-9.md) anche se la funzionalità non è più in anteprima. 

+ L'accesso privato a un servizio di ricerca è disponibile tramite due meccanismi, attualmente in anteprima:

  + È possibile limitare l'accesso a specifici indirizzi IP usando l'API REST di gestione `api-version=2019-10-01-Preview` per creare il servizio. L'API di anteprima include le nuove proprietà **IpRule** e **NetworkRuleSet** nell'[API CreateOrUpdate ](https://docs.microsoft.com/rest/api/searchmanagement/2019-10-01-preview/createorupdate-service). Questa funzionalità di anteprima è disponibile in specifiche aree. Per altre informazioni, vedere [Come usare l'API REST di gestione](https://docs.microsoft.com/rest/api/searchmanagement/search-howto-management-rest-api).

  + Con una funzionalità attualmente disponibile tramite un'anteprima con accesso limitato, è possibile effettuare il provisioning di un servizio di ricerca di Azure che supporta l'endpoint privato di Azure per le connessioni dei client nella stessa rete virtuale. Per altre informazioni, vedere [Creare un endpoint privato per una connessione sicura](service-create-private-endpoint.md).

### <a name="december-2019"></a>Dicembre 2019

+ [Crea un'app (anteprima)](search-create-app-portal.md) è una nuova procedura guidata del portale che genera un file HTML scaricabile. Il file include uno script incorporato che esegue il rendering di un'app Web operativa di tipo "localhost", associata a un indice nel servizio di ricerca. Le pagine sono configurabili nella procedura guidata e possono contenere una barra di ricerca, l'area dei risultati, una barra laterale di spostamento e il supporto per le query con completamento automatico. È possibile modificare il codice HTML offline per estendere o personalizzare il flusso di lavoro o l'aspetto.

+ [Creare un endpoint privato per connessioni sicure (anteprima)](service-create-private-endpoint.md) spiega come configurare un collegamento privato per proteggere le connessioni al servizio di ricerca. Questa funzionalità di anteprima è disponibile su richiesta e usa [Collegamento privato di Azure](../private-link/private-link-overview.md) e la [rete virtuale di Azure](../virtual-network/virtual-networks-overview.md) come parte della soluzione.

### <a name="november-2019---ignite-conference"></a>Novembre 2019 - Conferenza Ignite

+ L'[arricchimento incrementale (anteprima)](cognitive-search-incremental-indexing-conceptual.md) aggiunge la memorizzazione nella cache e il supporto di più stati a una pipeline di arricchimento, consentendo di eseguire specifici passaggi o fasi senza perdere il contenuto già elaborato. In precedenza, tutte le modifiche apportate a una pipeline di arricchimento richiedevano una ricompilazione completa. Con l'arricchimento incrementale, l'output di analisi costose viene mantenuto, in particolare l'analisi delle immagini.

<!-- 
+ Custom Entity Lookup is a cognitive skill used during indexing that allows you to provide a list of custom entities (such as part numbers, diseases, or names of locations you care about) that should be found within the text. It supports fuzzy matching, case-insensitive matching, and entity synonyms. -->

+ L'[estrazione di documenti (anteprima)](cognitive-search-skill-document-extraction.md) è una competenza cognitiva usata durante l'indicizzazione che consente di estrarre contenuto da un file all'interno di un set di competenze. In precedenza, il cracking di documenti si verificava solo prima dell'esecuzione del set di competenze. Con l'aggiunta di questa competenza, è anche possibile eseguire questa operazione all'interno dell'esecuzione del set di competenze.

+ [Traduzione testuale](cognitive-search-skill-text-translation.md) è una competenza cognitiva usata durante l'indicizzazione che valuta il testo e, per ogni record, lo restituisce convertito nella lingua di destinazione specificata.

+ I [modelli di Power BI](https://github.com/Azure-Samples/cognitive-search-templates/blob/master/README.md) possono velocizzare le visualizzazioni e le analisi di contenuto arricchito in un archivio conoscenze in Power BI Desktop. Questo modello è progettato per le proiezioni di tabelle di Azure create tramite l'[Importazione guidata dati](knowledge-store-create-portal.md).

+ Gli indicizzatori ora supportano [Azure Data Lake Storage Gen2 (anteprima)](search-howto-index-azure-data-lake-storage.md), l'[API Gremlin di Cosmos DB (anteprima)](search-howto-index-cosmosdb.md) e l'[API Cassandra di Cosmos DB (anteprima)](search-howto-index-cosmosdb.md). È possibile iscriversi usando [questo modulo](https://aka.ms/azure-cognitive-search/indexer-preview). Quando si verrà accettati nel programma di anteprima, si riceverà un messaggio di posta elettronica di conferma.

### <a name="july-2019"></a>Luglio 2019

+ Disponibile a livello generale in [Azure per enti pubblici](../azure-government/documentation-government-services-webandmobile.md#azure-cognitive-search).

## <a name="service-updates"></a>Aggiornamenti del servizio

Gli [annunci sugli aggiornamenti del servizio](https://azure.microsoft.com/updates/?product=search&status=all) per Ricerca cognitiva di Azure sono reperibili nel sito Web di Azure.