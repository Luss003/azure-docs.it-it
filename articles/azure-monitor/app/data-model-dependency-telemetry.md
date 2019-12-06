---
title: Modello di dati di Application Insights dipendenza di monitoraggio di Azure
description: Modello di dati di Application Insights per la telemetria delle dipendenze
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 04/17/2017
ms.reviewer: sergkanz
ms.openlocfilehash: 5021d3b34816159fc78590a5947ddd3a790303ee
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/05/2019
ms.locfileid: "74872639"
---
# <a name="dependency-telemetry-application-insights-data-model"></a>Telemetria delle dipendenze: modello di dati di Application Insights

In [Application Insights](../../azure-monitor/app/app-insights-overview.md), la telemetria delle dipendenze rappresenta un'interazione del componente monitorato con un componente remoto, ad esempio SQL o un endpoint HTTP.

## <a name="name"></a>name

Nome del comando avviato con questa chiamata delle dipendenze. Valore di cardinalità basso. Esempi sono il nome della stored procedure e il modello di percorso URL.

## <a name="id"></a>ID

Identificatore dell'istanza di una chiamata delle dipendenze. Usato per la correlazione con l'elemento di telemetria delle richieste corrispondente a questa chiamata delle dipendenze. Per altre informazioni vedere la pagina relativa alla [correlazione](../../azure-monitor/app/correlation.md).

## <a name="data"></a>Dati

Comando avviato con questa chiamata delle dipendenze. Esempi sono l'istruzione SQL e l'URL HTTP con tutti i parametri di query.

## <a name="type"></a>Type

Nome del tipo di dipendenza. Valore di cardinalità basso per un raggruppamento logico delle dipendenze e l'interpretazione di altri campi come commandName e resultCode. Esempi sono SQL, tabelle di Azure e HTTP.

## <a name="target"></a>Obiettivo

Sito di destinazione di una chiamata delle dipendenze. Esempi sono nome del server, indirizzo host. Per altre informazioni vedere la pagina relativa alla [correlazione](../../azure-monitor/app/correlation.md).

## <a name="duration"></a>Duration

Durata della richiesta in formato: `DD.HH:MM:SS.MMMMMM`. Deve essere inferiore a `1000` giorni.

## <a name="result-code"></a>Codice risultato

Codice risultato di una chiamata delle dipendenze. Esempi sono il codice di errore SQL e il codice di stato HTTP.

## <a name="success"></a>Success

Indicazione di chiamata con esito positivo o con esito negativo.

## <a name="custom-properties"></a>Proprietà personalizzate

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Misure personalizzate

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]


## <a name="next-steps"></a>Passaggi successivi

- Impostare il rilevamento delle dipendenze per [.NET](../../azure-monitor/app/asp-net-dependencies.md).
- Impostare il rilevamento delle dipendenze per [Java](../../azure-monitor/app/java-agent.md).
- [Scrivere dati di telemetria delle dipendenze personalizzate](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency)
- Per informazioni sul modello di dati e sui tipi di Application Insights, vedere il [modello di dati](data-model.md).
- Verificare quali [piattaforme](../../azure-monitor/app/platforms.md) supportano Application Insights.
