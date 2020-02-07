---
title: Monitorare l'utilizzo delle risorse e le metriche delle query
titleSuffix: Azure Cognitive Search
description: Abilitare la registrazione, ottenere le metriche dell'attività di query, l'utilizzo delle risorse e altri dati di sistema da un servizio ricerca cognitiva di Azure.
manager: nitinme
author: HeidiSteen
ms.author: heidist
tags: azure-portal
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 7ef868f156ac537cb066f293872f69135c4df25f
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/07/2020
ms.locfileid: "77059650"
---
# <a name="monitor-resource-consumption-and-query-activity-in-azure-cognitive-search"></a>Monitorare l'utilizzo delle risorse e le attività di query in Azure ricerca cognitiva

Nella pagina Panoramica del servizio ricerca cognitiva di Azure è possibile visualizzare i dati di sistema sull'utilizzo delle risorse, le metriche di query e la quantità di quota disponibile per creare più indici, indicizzatori e origini dati. È anche possibile usare il portale per configurare Log Analytics o un'altra risorsa per la raccolta di dati persistenti. 

La configurazione di log è utile per attività di autodiagnostica e per conservare la cronologia operativa. Internamente, i log esistono nel back-end per un breve periodo di tempo, sufficiente per indagini e analisi se si crea un ticket di supporto. Se si vuole avere il controllo delle informazioni dei log e potervi accedere, è necessario configurare una delle soluzioni descritte in questo articolo.

In questo articolo sono disponibili informazioni sulle opzioni per il monitoraggio, su come abilitare la registrazione e l'archiviazione dei log e su come visualizzare il contenuto dei log.

## <a name="metrics-at-a-glance"></a>Metriche subito disponibili

Le sezioni **Utilizzo** e **Monitoraggio** disponibili per impostazione predefinita nella pagina Panoramica includono le metriche per l'utilizzo delle risorse e l'esecuzione di query. Queste informazioni diventano disponibili non appena si inizia a usare il servizio, senza che sia necessaria alcuna configurazione. Questa pagina viene aggiornata ogni pochi minuti. Se è necessario prendere decisioni in merito al [livello da usare per i carichi di lavoro di produzione](search-sku-tier.md) o per stabilire se occorre [modificare il numero di repliche e partizioni attive](search-capacity-planning.md), queste metriche sono utili perché indicano con quale velocità vengono utilizzate le risorse e se la configurazione corrente è adeguata per gestire il carico esistente.

La scheda **Utilizzo** mostra la disponibilità delle risorse rispetto ai [limiti](search-limits-quotas-capacity.md) correnti. La figura seguente è riferita al servizio gratuito, che prevede un limite di 3 oggetti di ogni tipo e 50 MB di spazio di archiviazione. Un servizio Basic o Standard ha limiti più elevati e se si aumenta il numero delle partizioni, lo spazio di archiviazione massimo aumenta in proporzione.

![Stato di utilizzo relativo ai limiti effettivi](./media/search-monitor-usage/usage-tab.png
 "Stato di utilizzo relativo ai limiti effettivi")

## <a name="queries-per-second-qps-and-other-metrics"></a>Query al secondo e altre metriche

La scheda **Monitoraggio** mostra le medie mobili per metriche quali le *query di ricerca al secondo*, aggregate per minuto. 
*Latenza ricerca* è il tempo necessario al servizio di ricerca per elaborare le query di ricerca, aggregate per minuto. *Percentuale query di ricerca limitate* (non visualizzata) è la percentuale di query di ricerca che sono state limitate, aggregate per minuto.

Questi numeri sono approssimativi e hanno lo scopo di offrire un quadro generale dell'efficienza del sistema per la gestione delle richieste. Le query al secondo effettive potrebbero essere superiori o inferiori rispetto al numero indicato nel portale.

![Attività query al secondo](./media/search-monitor-usage/monitoring-tab.png "Attività query al secondo")

## <a name="activity-logs"></a>Log attività

Il **log attività** raccoglie informazioni da Azure Resource Manager. Sono esempi di informazioni presenti nel log attività la creazione o l'eliminazione di un servizio, l'aggiornamento di un gruppo di risorse, il controllo della disponibilità del nome o il recupero di una chiave di accesso del servizio per gestire una richiesta. 

È possibile accedere al **log attività** nel riquadro di spostamento a sinistra o dalle notifiche nella barra dei comandi nella finestra superiore oppure dalla pagina **Diagnostica e risolvi i problemi**.

Per le attività interne al servizio, come la creazione di un indice o l'eliminazione di un'origine dati, verranno visualizzate notifiche generiche, ad esempio "Get Admin Key" (Recupera chiave amministratore) per ogni richiesta, ma non l'azione specifica stessa. Per questo livello di informazioni, è necessario abilitare una soluzione di monitoraggio aggiuntiva.

## <a name="add-on-monitoring-solutions"></a>Soluzioni di monitoraggio aggiuntive

Azure ricerca cognitiva non archivia i dati oltre gli oggetti gestiti, il che significa che i dati di log devono essere archiviati esternamente. Per salvare in modo permanente i dati dei log, è possibile configurare le risorse seguenti. 

La tabella seguente confronta le opzioni per l'archiviazione dei log e l'aggiunta di funzionalità di monitoraggio approfondite per le operazioni del servizio e i carichi di lavoro di query tramite Application Insights.

| Risorsa | Utilizzo |
|----------|----------|
| [Log di Monitoraggio di Azure](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview) | Eventi registrati e metriche di query in base agli schemi seguenti. Gli eventi vengono registrati in un'area di lavoro Log Analytics. È possibile eseguire query su un'area di lavoro per restituire informazioni dettagliate dal log. Per altre informazioni, vedere [Introduzione ai log di monitoraggio di Azure](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-viewdata) |
| [Archiviazione BLOB](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) | Eventi registrati e metriche di query in base agli schemi seguenti. Gli eventi vengono registrati in un contenitore BLOB e archiviati in file JSON. Usare un editor JSON per visualizzare il contenuto dei file.|
| [Hub eventi](https://docs.microsoft.com/azure/event-hubs/) | Eventi registrati e metriche di query, basati sugli schemi documentati in questo articolo. Scegliere questa opzione come servizio di raccolta dati alternativo per i log di dimensioni molto grandi. |

I log di monitoraggio di Azure e l'archiviazione BLOB sono disponibili come servizio gratuito, in modo che sia possibile provarli senza costi aggiuntivi per la durata della sottoscrizione di Azure. L'iscrizione e l'uso di Application Insights sono gratuiti purché le dimensioni dei dati dell'applicazione non superino determinati limiti (vedere la [pagina dei prezzi](https://azure.microsoft.com/pricing/details/monitor/) per informazioni dettagliate).

La sezione successiva illustra i passaggi per l'abilitazione e l'uso dell'archiviazione BLOB di Azure per la raccolta e l'accesso ai dati di log creati dalle operazioni di ricerca cognitiva di Azure.

## <a name="enable-logging"></a>Abilitazione della registrazione

La registrazione per i carichi di lavoro di indicizzazione e query è disattivata per impostazione predefinita e dipende da soluzioni aggiuntive sia per l'infrastruttura di registrazione che per l'archiviazione esterna a lungo termine. Di per sé, gli unici dati persistenti in Azure ricerca cognitiva sono gli oggetti che crea e gestisce, quindi i log devono essere archiviati altrove.

In questa sezione si apprenderà come usare l'archiviazione BLOB per archiviare gli eventi registrati e i dati di metrica.

1. Se non ne è già disponibile uno, [creare un account di archiviazione](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account). È possibile inserirlo nello stesso gruppo di risorse di Azure ricerca cognitiva per semplificare la pulizia in un secondo momento se si desidera eliminare tutte le risorse usate in questo esercizio.

   L'account di archiviazione deve esistere nella stessa area del ricerca cognitiva di Azure.

2. Aprire la pagina Panoramica del servizio di ricerca. Nel riquadro di spostamento a sinistra scorrere verso il basso fino a **monitoraggio** e fare clic su **impostazioni di diagnostica**.

   ![Impostazioni di diagnostica](./media/search-monitor-usage/diagnostic-settings.png "Impostazioni di diagnostica")

3. Selezionare **Aggiungi impostazioni di diagnostica**

4. Scegliere i dati da esportare: log, metriche o entrambi. È possibile copiarlo in un account di archiviazione, inviarlo a un hub eventi o esportarlo nei log di monitoraggio di Azure.

   Per l'opzione di archiviazione BLOB, deve esistere solo l'account di archiviazione. I contenitori e i BLOB verranno creati in base alle esigenze quando si esportano i dati di log.

   ![Configurare l'archivio di archiviazione BLOB](./media/search-monitor-usage/configure-blob-storage-archive.png "Configurare l'archivio di archiviazione BLOB")

5. Salva il profilo

6. Testare la registrazione mediante la creazione o eliminazione di oggetti (creazione di eventi nel log) e con l'invio di query (generazione di metriche). 

La registrazione viene abilitata dopo il salvataggio del profilo. I contenitori vengono creati solo quando è presente un'attività da registrare o misurare. I dati che vengono copiati in un account di archiviazione vengono formattati come JSON e inseriti in due contenitori:

* insights-logs-operationlogs: per i log relativi al traffico ricerca
* insights-metrics-pt1m: per le metriche

**Richiede un'ora prima che i contenitori vengano visualizzati nell'archivio BLOB. È presente un BLOB, all'ora, per ogni contenitore.**

È possibile usare [Visual Studio Code](#download-and-open-in-visual-studio-code) o un altro editor JSON per visualizzare i file. 

### <a name="example-path"></a>Percorso di esempio

```
resourceId=/subscriptions/<subscriptionID>/resourcegroups/<resourceGroupName>/providers/microsoft.search/searchservices/<searchServiceName>/y=2018/m=12/d=25/h=01/m=00/name=PT1H.json
```

## <a name="log-schema"></a>Schema del log
I BLOB che contengono i log del traffico del servizio di ricerca sono strutturati come descritto in questa sezione. Ogni BLOB ha un oggetto radice denominato **record** che contiene una matrice di oggetti di log. Ogni BLOB contiene record su tutte le operazioni eseguite nell'arco della stessa ora.

| Nome | Type | Esempio | Note |
| --- | --- | --- | --- |
| time |Datetime |"2018-12-07T00:00:43.6872559Z" |Timestamp dell'operazione |
| resourceId |string |"/SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111/<br/>RESOURCEGROUPS/DEFAULT/PROVIDERS/<br/> MICROSOFT.SEARCH/SEARCHSERVICES/SEARCHSERVICE" |resourceId in uso |
| operationName |string |"Query.Search" |Nome dell'operazione |
| operationVersion |string |"2019-05-06" |api-version usata |
| category |string |"OperationLogs" |constant |
| resultType |string |"Esito positivo" |Valori possibili: esito positivo o negativo |
| resultSignature |INT |200 |Codice risultato HTTP |
| durationMS |INT |50 |Durata dell'operazione in millisecondi |
| properties |object |Vedere la tabella seguente |Oggetto contenente dati specifici dell'operazione |

**Schema delle proprietà**

| Nome | Type | Esempio | Note |
| --- | --- | --- | --- |
| Descrizione |string |"GET /indexes('content')/docs" |Endpoint dell'operazione |
| Query |string |"?search=AzureSearch&$count=true&api-version=2019-05-06" |Parametri della query |
| Documenti |INT |42 |Numero di documenti elaborati |
| IndexName |string |"testindex" |Nome dell'indice associato all'operazione |

## <a name="metrics-schema"></a>Schema delle metriche

Vengono acquisite metriche per le richieste di query.

| Nome | Type | Esempio | Note |
| --- | --- | --- | --- |
| resourceId |string |"/SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111/<br/>RESOURCEGROUPS/DEFAULT/PROVIDERS/<br/>MICROSOFT.SEARCH/SEARCHSERVICES/SEARCHSERVICE" |ID risorsa |
| metricName |string |"Latenza" |Nome della metrica |
| time |Datetime |"2018-12-07T00:00:43.6872559Z" |Timestamp dell'operazione |
| average |INT |64 |Valore medio degli esempi non elaborati nell'intervallo di tempo della metrica |
| minimum |INT |37 |Valore minimo degli esempi non elaborati nell'intervallo di tempo della metrica |
| maximum |INT |78 |Valore massimo degli esempi non elaborati nell'intervallo di tempo della metrica |
| total |INT |258 |Valore totale degli esempi non elaborati nell'intervallo di tempo della metrica |
| count |INT |4 |Numero degli esempi non elaborati usati per generare la metrica |
| timegrain |string |"PT1M" |Intervallo di tempo della metrica nel formato ISO 8601 |

Tutte le metriche vengono segnalate in intervalli di un minuto. Ogni metrica espone i valori minimi, massimi e medi al minuto.

Per la metrica SearchQueriesPerSecond, il valore minimo è quello più basso per le query di ricerca al secondo registrato durante questo minuto. Lo stesso vale anche per il valore massimo. Il valore medio è l'aggregato nel corso dell'intero minuto.
Considerare questo scenario nell'arco di un minuto: un secondo di carico elevato, che è il valore massimo per SearchQueriesPerSecond, seguito da 58 secondi di carico medio e infine da un secondo con una sola query, che è il valore minimo.

Per ThrottledSearchQueriesPercentage, i valori minimo, massimo, medio e totale corrispondono tutti allo stesso valore: la percentuale di query di ricerca che sono state limitate, dal numero totale di query di ricerca nell'arco di un minuto.

## <a name="download-and-open-in-visual-studio-code"></a>Scaricare e aprire in Visual Studio Code

È possibile usare qualsiasi editor JSON per visualizzare il file di log. Se non ne è già disponibile uno, è consigliabile usare [Visual Studio Code](https://code.visualstudio.com/download).

1. Nel portale di Azure aprire l'account di archiviazione. 

2. Nel riquadro di spostamento a sinistra fare clic su **BLOB**. Dovrebbero essere disponibili i contenitori **insights-logs-operationlogs** e **insights-metrics-pt1m**. Questi contenitori vengono creati da Azure ricerca cognitiva quando i dati di log vengono esportati nell'archivio BLOB.

3. Scorrere la gerarchia di cartelle verso il basso fino a raggiungere il file con estensione JSON.  Usare il menu di scelta rapida per scaricare il file.

Dopo aver scaricato il file, aprirlo in un editor JSON per visualizzare il contenuto.

## <a name="use-system-apis"></a>Usare le API del sistema
Sia l'API REST di Azure ricerca cognitiva che .NET SDK forniscono l'accesso a livello di codice alle metriche dei servizi, alle informazioni sugli indici e agli indicizzatori e ai conteggi dei documenti.

* [Ottenere le statistiche del servizio](/rest/api/searchservice/get-service-statistics)
* [Ottenere le statistiche di un indice](/rest/api/searchservice/get-index-statistics)
* [Conteggio documenti](/rest/api/searchservice/count-documents)
* [Ottenere lo stato dell'indicizzatore](/rest/api/searchservice/get-indexer-status)

Per abilitare il monitoraggio tramite PowerShell o l'interfaccia della riga di comando di Azure, vedere la documentazione [qui](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-overview).

## <a name="next-steps"></a>Passaggi successivi

Vedere [Amministrazione del servizio per Ricerca di Azure nel portale di Azure](search-manage.md) per altre informazioni sull'amministrazione del servizio e [Prestazioni e ottimizzazione](search-performance-optimization.md) per indicazioni sull'ottimizzazione.
