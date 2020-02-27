---
title: Monitorare Azure Cosmos DB con monitoraggio di Azure per Cosmos DB (anteprima) | Microsoft Docs
description: Questo articolo descrive il monitoraggio di Azure per Cosmos DB funzionalità che fornisce ai proprietari Cosmos DB una rapida comprensione dei problemi di prestazioni e utilizzo con i propri account CosmosDB.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 10/27/2019
ms.openlocfilehash: dece5b0bb0508e2d83ee184e71ef0b4364d25ac8
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/26/2020
ms.locfileid: "77622934"
---
# <a name="explore-azure-monitor-for-azure-cosmos-db-preview"></a>Esplora monitoraggio di Azure per Azure Cosmos DB (anteprima)

Monitoraggio di Azure per Azure Cosmos DB (anteprima) offre una panoramica delle prestazioni, degli errori, della capacità e dello stato operativo generali di tutte le risorse di Azure Cosmos DB in un'esperienza interattiva unificata. Questo articolo consente di comprendere i vantaggi di questa nuova esperienza di monitoraggio e di come è possibile modificare e adattare l'esperienza per soddisfare le esigenze specifiche dell'organizzazione.   

## <a name="introduction"></a>Introduzione

Prima di approfondire l'esperienza, è necessario comprenderne il modo in cui presenta e visualizza le informazioni. 

Offre:

* In una singola posizione, il **punto di vista della scalabilità** delle risorse Azure Cosmos DB in tutte le sottoscrizioni, con la possibilità di definire l'ambito in modo selettivo solo per le sottoscrizioni e le risorse che si desidera valutare.

* Eseguire il **drill-down dell'analisi** di una particolare risorsa di Azure CosmosDB per diagnosticare i problemi o eseguire analisi dettagliate in base all'utilizzo delle categorie, agli errori, alla capacità e alle operazioni. Selezionando una di queste opzioni, viene fornita una visualizzazione approfondita delle metriche di Azure Cosmos DB pertinenti.  

* **Personalizzabile** : questa esperienza è basata sui modelli di cartella di lavoro di monitoraggio di Azure che consentono di modificare le metriche visualizzate, modificare o impostare le soglie che si allineano ai limiti e quindi salvarle in una cartella di lavoro personalizzata. I grafici nelle cartelle di lavoro possono quindi essere aggiunti ai dashboard di Azure.  

Per questa funzionalità non è necessario abilitare o configurare elementi, per impostazione predefinita vengono raccolte le metriche Azure Cosmos DB.

>[!NOTE]
>Non sono previsti addebiti per accedere a questa funzionalità e verranno addebitate solo le funzionalità essenziali di monitoraggio di Azure configurate o abilitate, come descritto nella pagina [Dettagli prezzi di monitoraggio di Azure](https://azure.microsoft.com/pricing/details/monitor/) .

## <a name="view-operation-level-metrics-for-azure-cosmos-db"></a>Visualizzare le metriche a livello di operazione per Azure Cosmos DB

1. Accedere al [portale di Azure](https://portal.azure.com/).

1. Selezionare **monitoraggio** nella barra di spostamento a sinistra e selezionare **metrica**.

   ![Riquadro metriche in monitoraggio di Azure](./media/cosmosdb-insights-overview/monitor-metrics-blade.png)

1. Dal riquadro **metriche** > **selezionare una risorsa** > scegliere la **sottoscrizione**e il **gruppo di risorse**richiesti. Per il **tipo di risorsa**selezionare **account Azure Cosmos DB**, scegliere uno degli account Azure Cosmos esistenti e selezionare **applica**.

   ![Scegliere un account Cosmos DB per visualizzare le metriche](./media/cosmosdb-insights-overview/select-cosmosdb-account.png)

1. È quindi possibile selezionare una metrica dall'elenco delle metriche disponibili. È possibile selezionare metriche specifiche per unità richiesta, archiviazione, latenza, disponibilità, Cassandra e altre. Per informazioni dettagliate su tutte le metriche disponibili in questo elenco, vedere l'articolo [metriche per categoria](../../cosmos-db/monitor-cosmos-db-reference.md) . In questo esempio, selezionare **unità richiesta** e **AVG** come valore di aggregazione.

   Oltre a questi dettagli, è anche possibile selezionare l' **intervallo di tempo** e la **granularità temporale** delle metriche. Al massimo, è possibile visualizzare le metriche negli ultimi 30 giorni.  Dopo aver applicato il filtro, viene visualizzato un grafico in base al filtro. È possibile visualizzare il numero medio di unità richiesta utilizzate al minuto per il periodo selezionato.  

   ![Scegliere una metrica dalla portale di Azure](./media/cosmosdb-insights-overview/metric-types.png)

### <a name="add-filters-to-metrics"></a>Aggiungere filtri alle metriche

È anche possibile filtrare le metriche e il grafico visualizzato da uno specifico **CollectionName**, **DatabaseName**, **OperationType**, **Region**e **statusCode**. Per filtrare le metriche, selezionare **Aggiungi filtro** e scegliere la proprietà obbligatoria, ad esempio **OperationType** , e selezionare un valore, ad esempio **query**. Il grafico Visualizza quindi le unità richiesta utilizzate per l'operazione di query per il periodo selezionato. Le operazioni eseguite tramite la stored procedure non vengono registrate in modo che non siano disponibili nella metrica OperationType.

![Aggiungere un filtro per selezionare la granularità della metrica](./media/cosmosdb-insights-overview/add-metrics-filter.png)

È possibile raggruppare le metriche usando l'opzione **applica suddivisione** . È ad esempio possibile raggruppare le unità richiesta per tipo di operazione e visualizzare il grafico per tutte le operazioni in una sola volta, come illustrato nell'immagine seguente:

![Aggiungi filtro di suddivisione applica](./media/cosmosdb-insights-overview/apply-metrics-splitting.png)

## <a name="view-utilization-and-performance-metrics-for-azure-cosmos-db"></a>Visualizzare le metriche di utilizzo e delle prestazioni per Azure Cosmos DB

Per visualizzare l'utilizzo e le prestazioni degli account di archiviazione in tutte le sottoscrizioni, seguire questa procedura.

1. Accedere al [portale di Azure](https://portal.azure.com).

2. Cercare **monitoraggio** e selezionare **monitoraggio**.

    ![Casella di ricerca con la parola "monitor" e un elenco a discesa che indica i servizi "monitoraggio" con un'immagine di stile tachimetro](./media/cosmosdb-insights-overview/search-monitor.png)

3. Selezionare **Cosmos DB (anteprima)** .

    ![Screenshot della cartella di lavoro di Cosmos DB Overview](./media/cosmosdb-insights-overview/cosmos-db.png)

### <a name="overview"></a>Panoramica

In **Panoramica**la tabella Visualizza le metriche di Azure Cosmos DB interattive. È possibile filtrare i risultati in base alle opzioni selezionate dagli elenchi a discesa seguenti:

* Sono elencate solo le **sottoscrizioni con** una risorsa Azure Cosmos DB.  

* **Cosmos DB** : è possibile selezionare tutti, un subset o una singola risorsa Azure Cosmos DB.

* **Intervallo di tempo** : per impostazione predefinita, Visualizza le ultime 4 ore di informazioni in base alle selezioni corrispondenti effettuate.

Il riquadro del contatore sotto gli elenchi a discesa esegue il rollup del numero totale di Azure Cosmos DB risorse incluse nelle sottoscrizioni selezionate. Per le colonne nella cartella di lavoro è disponibile una codifica dei colori condizionale o Heatmaps che segnala le metriche delle transazioni. Il colore più profondo ha il valore più alto e un colore più chiaro è basato sui valori più bassi. 

Selezionando una freccia a discesa accanto a una delle risorse Azure Cosmos DB verrà rilevata una suddivisione delle metriche delle prestazioni a livello di singolo contenitore di database:

![Elenco a discesa espanso che rivela singoli contenitori di database e ripartizione delle prestazioni associata](./media/cosmosdb-insights-overview/container-view.png)

Selezionando il nome della risorsa Azure Cosmos DB evidenziato in blu verrà visualizzata la **Panoramica** predefinita per l'account di Azure Cosmos DB associato. 

### <a name="failures"></a>Errori

Selezionare **errori** nella parte superiore della pagina e viene visualizzata la parte relativa agli **errori** del modello di cartella di lavoro. Vengono visualizzate le richieste totali con la distribuzione delle risposte che compongono tali richieste:

![Screenshot degli errori con suddivisione in base al tipo di richiesta HTTP](./media/cosmosdb-insights-overview/failures.png)

| Codice      |  Descrizione       | 
|-----------|:--------------------|
| `200 OK`  | Una delle operazioni REST seguenti è riuscita: </br>-OTTENERE su una risorsa. </br> -PUT su una risorsa. </br> -POST su una risorsa. </br> -POST su una risorsa stored procedure per eseguire l'stored procedure.|
| `201 Created` | Un'operazione POST per la creazione di una risorsa è riuscita. |
| `404 Not Found` | L'operazione tenta di agire su una risorsa che non esiste più. È ad esempio possibile che la risorsa sia già stata eliminata. |

Per un elenco completo dei codici di stato, vedere l' [articolo Azure Cosmos DB codice di stato http](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb).

### <a name="capacity"></a>Capacity

Selezionare **capacità** nella parte superiore della pagina per aprire la parte relativa alla **capacità** del modello di cartella di lavoro. Mostra il numero di documenti disponibili, la crescita del documento nel tempo, l'utilizzo dei dati e la quantità totale di spazio di archiviazione disponibile.  Questa operazione può essere usata per identificare i potenziali problemi di archiviazione e di utilizzo dei dati.

![Cartella di lavoro capacità](./media/cosmosdb-insights-overview/capacity.png) 

Come per la cartella di lavoro Panoramica, selezionando l'elenco a discesa accanto a una risorsa Azure Cosmos DB nella colonna **sottoscrizione** verrà rilevata una suddivisione dei singoli contenitori che costituiscono il database.

### <a name="operations"></a>Operazioni 

Selezionare **operazioni** nella parte superiore della pagina e verrà visualizzata la parte **operazioni** del modello di cartella di lavoro. Offre la possibilità di visualizzare le richieste suddivise in base al tipo di richieste effettuate. 

Nell'esempio seguente si noterà che `eastus-billingint` riceve principalmente le richieste di lettura, ma con un numero ridotto di richieste di Upsert e di creazione. Mentre `westeurope-billingint` è di sola lettura dal punto di vista della richiesta, almeno nelle ultime quattro ore in cui la cartella di lavoro è attualmente in ambito tramite il parametro intervallo di tempo.

![Cartella di lavoro operazioni](./media/cosmosdb-insights-overview/operation.png) 

## <a name="pin-export-and-expand"></a>Aggiungi, esporta ed Espandi

È possibile aggiungere una delle sezioni della metrica a un [dashboard di Azure](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards) selezionando l'icona a puntina da disegno in alto a destra nella sezione.

![Esempio di aggiunta della sezione metrica al dashboard](./media/cosmosdb-insights-overview/pin.png)

Per esportare i dati nel formato di Excel, fare clic sull'icona a forma di freccia rivolta verso il basso a sinistra dell'icona a forma di puntina da disegno.

![Icona Esporta cartella di lavoro](./media/cosmosdb-insights-overview/export.png)

Per espandere o comprimere tutte le visualizzazioni a discesa nella cartella di lavoro, selezionare l'icona di espansione a sinistra dell'icona Esporta:

![Icona Espandi cartella di lavoro](./media/cosmosdb-insights-overview/expand.png)

## <a name="customize-azure-monitor-for-azure-cosmos-db-preview"></a>Personalizzare monitoraggio di Azure per Azure Cosmos DB (anteprima)

Poiché questa esperienza è basata sui modelli di cartella di lavoro di monitoraggio di Azure, è possibile **personalizzare** > **modificare** e **salvare** una copia della versione modificata in una cartella di lavoro personalizzata. 

![Personalizza barra](./media/cosmosdb-insights-overview/customize.png)

Le cartelle di lavoro vengono salvate in un gruppo di risorse, nella sezione **report personali** riservata all'utente o nella sezione **report condivisi** accessibile a tutti gli utenti con accesso al gruppo di risorse. Dopo avere salvato la cartella di lavoro personalizzata, è necessario passare alla raccolta di cartelle di lavoro per avviarla.

![Avvia raccolta cartelle di lavoro dalla barra dei comandi](./media/cosmosdb-insights-overview/gallery.png)

## <a name="next-steps"></a>Passaggi successivi

* Configurare gli [avvisi delle metriche](../platform/alerts-metric.md) e le notifiche sull'integrità del [servizio](../../service-health/alerts-activity-log-service-notifications.md) per impostare avvisi automatici per facilitare il rilevamento dei problemi.

* Informazioni sulle cartelle di lavoro degli scenari sono progettate per supportare, come creare nuovi report esistenti e personalizzarli e altro ancora esaminando [creare report interattivi con le cartelle di lavoro di monitoraggio di Azure](../app/usage-workbooks.md).
