---
title: Explore data using the explorer - Azure Time Series Insights  | Microsoft Docs
description: Learn how to use the Azure Time Series Insights explorer to view your IoT data.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/20/2019
ms.custom: seodec18
ms.openlocfilehash: f6fafb2d4d363ee0d01d5bd9f18a1294ae8110b7
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/22/2019
ms.locfileid: "74327845"
---
# <a name="azure-time-series-insights-explorer"></a>Strumento di esplorazione di Azure Time Series Insights

This article describes the features and options in general availability for the Azure Time Series Insights [explorer web app](https://insights.timeseries.azure.com/). The Time Series Insights explorer demonstrates the powerful data visualization capabilities provided by the service and can be accessed within your own environment.

Azure Time Series Insights è un servizio completamente gestito per analisi, archiviazione e visualizzazione, che semplifica l'esplorazione e l'analisi di miliardi di eventi IoT contemporaneamente. Offre una visualizzazione globale dei dati e consente di convalidare rapidamente le soluzioni IoT e di evitare così costosi periodi di inattività per i dispositivi di importanza strategica. È possibile individuare tendenze nascoste e anomalie, nonché eseguire analisi della causa radice. Lo strumento di esplorazione di Time Series Insights è attualmente disponibile nella versione di anteprima pubblica.

> [!TIP]
> For a guided tour through the demonstration environment, read the [Azure Time Series Insights quickstart](time-series-quickstart.md).

## <a name="video"></a>Video

### <a name="learn-about-querying-data-by-using-the-time-series-insights-explorer-br"></a>Learn about querying data by using the Time Series Insights explorer. </br>

> [!VIDEO https://www.youtube.com/embed/SHFPZvrR71s]

>[!NOTE]
>See the preceding video <a href="https://www.youtube.com/watch?v=6ehNf6AJkFo">"Getting started with Time Series Insights by using an Azure IoT Solution Accelerator."</a>

## <a name="prerequisites"></a>Prerequisiti

Prima di poter usare lo strumento di esplorazione di Time Series Insights, è necessario:

- Creare un ambiente Time Series Insights For more information, see [How to get started with Time Series Insights](./time-series-insights-get-started.md).
- [Provide access](time-series-insights-data-access.md) to your account in the environment.
- Add an [IoT hub](time-series-insights-how-to-add-an-event-source-iothub.md) or [event hub](time-series-insights-how-to-add-an-event-source-eventhub.md) event source to it.

## <a name="explore-and-query-data"></a>Esplorare ed eseguire query sui dati

Dopo aver connesso l'origine evento all'ambiente Time Series Insights, è possibile esplorare ed eseguire query sui dati di serie temporali.

1. To start, open the [Time Series Insights explorer](https://insights.timeseries.azure.com/) in your web browser. On the left side of the window, select an environment. Tutti gli ambiente a cui l'utente può accedere sono elencati in ordine alfabetico.

1. After you select an environment, either use the **From** and **To** configurations at the top, or click and drag over the timespan you want. Select the magnifying glass in the upper-right corner, or right-click on the selected timespan and select **Search**.

1. You also can refresh availability automatically every minute by selecting the **Auto On** button. The **Auto On** button only applies to the availability chart, not the content of the main visualization.

1. The Azure cloud icon takes you to your environment in the Azure portal.

   [![Time Series Insights environment selection](media/time-series-insights-explorer/tsi-ga-explorer-environments.png)](media/time-series-insights-explorer/tsi-ga-explorer-environments.png#lightbox)

1. Nel grafico visualizzato è riportato il conteggio di tutti gli eventi durante l'intervallo di tempo selezionato. Sono disponibili i controlli seguenti:

    - **Terms Editor panel**: The term space is where you query your environment. It’s found on the left side of the screen:
      - **MEASURE**: This drop-down list shows all numeric columns (**Doubles**).
      - **SPLIT BY**: This drop-down list shows categorical columns (**Strings**).
      - You can enable step interpolation, show minimum and maximum, and adjust the y-axis from the control panel next to **MEASURE**. You also can adjust whether data shown is a count, average, or sum of the data.
      - You can add up to five terms to view on the same x-axis. Select **Add** to add a fresh term or use the **Clone this term** button to add a copy of an existing term.

        [![Terms selection, filtering, and query panel](media/time-series-insights-explorer/tsi-ga-explorer-add-or-clone.png)](media/time-series-insights-explorer/tsi-ga-explorer-add-or-clone.png#lightbox)

      - **Predicate**: Use the predicate to quickly filter your events by using the set of operands listed in the following table. If you conduct a search by selecting or clicking, the predicate automatically updates based on that search. I tipi di operandi supportati includono:

         |Operazione  |Tipi supportati  |Note  |
         |---------|---------|---------|
         |**<** , **>** , **<=** , **>=**    |  **Double**, **DateTime**, **TimeSpan**       |         |
         |**=** , **!=** , **<>**     | **String**, **Bool**, **Double**, **DateTime**, **TimeSpan**, **NULL**        |         |
         |**IN**     | **String**, **Bool**, **Double**, **DateTime**, **TimeSpan**, **NULL**        |  All operands should be of the same type or be **NULL** constant.        |
         |**HAS**     | **Stringa**        |  Only constant string literals are allowed at the right side. Empty string and **NULL** aren't allowed.       |

      - **Example queries**

         [![Example GA queries](media/time-series-insights-explorer/tsi-ga-example-queries.png)](media/time-series-insights-explorer/tsi-ga-example-queries.png#lightbox)

1. You can use the **Interval Size** slider tool to zoom in and out of intervals over the same timespan. The slider provides more precise control of movement between large slices of time that show smooth trends down to slices as small as the millisecond, which allow you to see granular, high-resolution cuts of your data. The slider’s default starting point is set as the most optimal view of the data from your selection to balance resolution, query speed, and granularity.

1. The **Time brush** tool makes it easy to navigate from one timespan to another.

1. Select the **Save** icon to save your current query and share it with other users of the environment. When you select the **Open** icon, you can see all of your saved queries and any shared queries of other users in environments you have access to.

   [![Queries](media/time-series-insights-explorer/tsi-ga-explorer-saved-queries.png)](media/time-series-insights-explorer/tsi-ga-explorer-saved-queries.png#lightbox)

## <a name="visualize-data"></a>Visualizzare i dati

1. Use the **Perspective View** tool for a simultaneous view of up to four unique queries. The **Perspective View** button is in the upper-right corner of the chart.

   [![Select queries to add to the perspective pane](media/time-series-insights-explorer/tsi-ga-explorer-perspective-panes.png)](media/time-series-insights-explorer/tsi-ga-explorer-perspective-panes.png#lightbox)

1. View a chart to visually explore your data, and use the **Chart** tools:

    - **Select** or **click** a specific timespan or a single data series.
    - Within a timespan selection, you can zoom or explore events.
    - Selezione all'interno di una serie di dati: è possibile suddividere la serie in base a una colonna diversa, aggiungere la serie sotto forma di nuovo termine, visualizzare solo la serie selezionata, escludere la serie selezionata, bloccare tale serie o esplorare gli eventi nella serie selezionata.
    - In the filter area to the left of the chart, you can see all displayed data series and reorder by value or name. You also can view all data series or any pinned or unpinned series. You can select a single data series and split the series by another column, add the series as a new term, show only the selected series, exclude the selected series, pin that series, or explore events from the selected series.
    - When you view multiple terms simultaneously, you can stack, unstack, see additional data about a data series, and use the same y-axis across all terms. Use the buttons in the upper-right corner of the chart.

    [![Chart tool upper right corner option settings](media/time-series-insights-explorer/tsi-ga-example-chart-options.png)](media/time-series-insights-explorer/tsi-ga-example-chart-options.png#lightbox)

1. Use the **heatmap** to quickly spot unique or anomalous data series in a given query. È possibile visualizzare come mappa termica solo un termine di ricerca.

    [![GA explorer heatmap charting ](media/time-series-insights-explorer/tsi-ga-example-heatmap-charting.png)](media/time-series-insights-explorer/tsi-ga-example-heatmap-charting.png#lightbox)

1. When you explore events by selecting or right-clicking, the **EVENTS** panel is made available. In questa area è possibile visualizzare tutti gli eventi non elaborati ed esportare gli eventi come file con estensione json o csv. Time Series Insights stores all raw data.

    [![Events](media/time-series-insights-explorer/tsi-ga-explorer-events-panel.png)](media/time-series-insights-explorer/tsi-ga-explorer-events-panel.png#lightbox)

1. Select the **STATS** tab after you explore events to expose patterns and column stats.

    - **Patterns**: This feature proactively surfaces the most statistically significant patterns in a selected data region. You don't have to look at thousands of events to understand what patterns require the most time and energy. With Time Series Insights, you can jump directly into these statistically significant patterns to continue conducting an analysis. Questa funzionalità risulta utile anche per le analisi retrospettive di dati storici.
    - **Column Stats**: Column stats provide charts and tables that break down data from each column of the selected data series over the selected timespan.

      [![STATS column charting and options](media/time-series-insights-explorer/tsi-ga-explorer-stat-column.png)](media/time-series-insights-explorer/tsi-ga-explorer-stat-column.png#lightbox)

Now you've seen the various features and options that are available within the Time Series Insights explorer web app.

## <a name="next-steps"></a>Passaggi successivi

- Learn how to [diagnose and solve problems](time-series-insights-diagnose-and-solve-problems.md) in your Time Series Insights environment.

- Take the guided [Azure Time Series Insights quickstart](time-series-quickstart.md) tour.
