---
title: Azure Application Insights | Microsoft Docs
description: ''
ms.topic: conceptual
author: eternovsky
ms.author: evternov
ms.date: 08/08/2018
ms.reviewer: mbullwin
ms.openlocfilehash: 75c5bd5bd6a7ded8679c30446a45809a1ea4406a
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/27/2020
ms.locfileid: "77672005"
---
# <a name="correlating-application-insights-data-with-custom-data-sources"></a>Correlazione dei dati di Application Insights con origini dati personalizzate

Application Insights raccoglie diversi tipi di dati: eccezioni, tracce, visualizzazioni pagina e altro ancora. Anche se ciò è spesso sufficiente per analizzare le prestazioni, l'affidabilità e l'utilizzo di un'applicazione, esistono casi in cui è utile correlare i dati archiviati in Application Insights ad altri set di dati completamente personalizzati.

Alcune situazioni in cui è preferibile disporre di dati personalizzati includono:

- Tabelle di arricchimento dei dati o ricerca: integrare, ad esempio, un nome del server con il proprietario del server e il percorso lab in cui è disponibile 
- Correlazione con origini dati non di Application Insights: ad esempio, correlare i dati relativi a un acquisto in un archivio Web con informazioni provenienti dal servizio evasione degli acquisti per determinare l'accuratezza dei tempi di spedizione stimati 
- Dati completamente personalizzati: molti clienti Microsoft apprezzano il linguaggio di query e le prestazioni della piattaforma di log di Monitoraggio di Azure che esegue il backup di Application Insights e vogliono usarli per eseguire query su dati non correlati ad Application Insights. Ad esempio, per tenere traccia delle prestazioni di un pannello solare nel quadro di un'installazione domestica intelligente, come delineato [qui](https://www.catapultsystems.com/blogs/using-log-analytics-and-a-special-guest-to-forecast-electricity-generation/).

## <a name="how-to-correlate-custom-data-with-application-insights-data"></a>Procedura di correlazione di dati personalizzati con dati di Application Insights 

Poiché Application Insights è supportato dalla potente piattaforma di log di Monitoraggio di Azure, è possibile usare tutte le potenzialità di Monitoraggio di Azure per inserire i dati. Verranno quindi scritte query che usano l'operatore "join" per mettere in correlazione i dati personalizzati con i dati disponibili nei log di Monitoraggio di Azure. 

## <a name="ingesting-data"></a>Inserimento di dati

In questa sezione viene esaminata anche la procedura per ottenere i dati nei log di Monitoraggio di Azure.

Se non se ne possiede già una, effettuare il provisioning di una nuova area di lavoro Log Analytics seguendo [queste istruzioni](../learn/quick-collect-azurevm.md) fino al passaggio "creare un'area di lavoro" incluso.

Per iniziare a inviare i dati di log in Monitoraggio di Azure. Sono disponibili diverse opzioni:

- Per un meccanismo sincrono, è possibile chiamare direttamente l'[API di raccolta dati](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-collector-api) o usare il connettore di App per la logica. È sufficiente cercare "Azure Log Analytics" e selezionare l'opzione "Invia dati":

  ![Screenshot di scelta e azione](./media/custom-data-correlation/01-logic-app-connector.png)  

- Per un'opzione asincrona, usare l'API di raccolta dati per creare una pipeline di elaborazione. Per informazioni dettagliate, vedere [questo articolo](https://docs.microsoft.com/azure/log-analytics/log-analytics-create-pipeline-datacollector-api).

## <a name="correlating-data"></a>Correlazione dei dati

Application Insights si basa sulla piattaforma di log di Monitoraggio di Azure. Di conseguenza verranno usati [join tra risorse](https://docs.microsoft.com/azure/log-analytics/log-analytics-cross-workspace-search) per correlare i dati inseriti in Monitoraggio di Azure con i dati di Application Insights.

Ad esempio, è possibile inserire l'inventario e i percorsi lab in una tabella denominata "LabLocations_CL" in un'area di lavoro Log Analytics denominata "myLA". Per esaminare le richieste registrate nell'app di Application Insights denominata "myAI" e correlare i nomi di computer che hanno servito le richieste ai percorsi di tali computer archiviati nella tabella personalizzata menzionata in precedenza, è quindi possibile eseguire la query seguente dal contesto di Application Insights o di Monitoraggio di Azure:

```
app('myAI').requests
| join kind= leftouter (
    workspace('myLA').LabLocations_CL
    | project Computer_S, Owner_S, Lab_S
) on $left.cloud_RoleInstance == $right.Computer
```

## <a name="next-steps"></a>Passaggi successivi

- Consultare il riferimento all'[API di raccolta dati](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-collector-api).
- Altre informazioni sul [join tra risorse](https://docs.microsoft.com/azure/log-analytics/log-analytics-cross-workspace-search).
