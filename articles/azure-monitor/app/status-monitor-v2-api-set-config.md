---
title: "Informazioni di riferimento sull'API dell'agente applicazione Azure Insights: set config | Microsoft Docs"
description: Informazioni di riferimento sull'API dell'agente Application Insights. Set-ApplicationInsightsMonitoringConfig. Monitora le prestazioni del sito Web senza ridistribuire il sito Web. Funziona con le app Web ASP.NET ospitate in locale, in macchine virtuali o in Azure.
services: application-insights
documentationcenter: .net
author: TimothyMothra
manager: alexklim
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: tilee
ms.openlocfilehash: 2ab941b5587a8836f1e472fbce3966b12bfa1e11
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2019
ms.locfileid: "72388246"
---
# <a name="application-insights-agent-api-set-applicationinsightsmonitoringconfig"></a>API dell'agente di Application Insights: set-ApplicationInsightsMonitoringConfig

Questo documento descrive un cmdlet che fa parte del modulo di [PowerShell AZ. ApplicationMonitor](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

## <a name="description"></a>Description

Imposta il file di configurazione senza eseguire una reinstallazione completa.
Riavviare IIS per rendere effettive le modifiche.

> [!IMPORTANT] 
> Questo cmdlet richiede una sessione di PowerShell con autorizzazioni di amministratore.


## <a name="examples"></a>esempi

### <a name="example-with-a-single-instrumentation-key"></a>Esempio con una singola chiave di strumentazione
In questo esempio, a tutte le app nel computer corrente verrà assegnata una sola chiave di strumentazione.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

### <a name="example-with-an-instrumentation-key-map"></a>Esempio con una mappa delle chiavi di strumentazione
In questo esempio:
- `MachineFilter` corrisponde al computer corrente usando il carattere jolly `'.*'`.
- `AppFilter='WebAppExclude'` fornisce una chiave di strumentazione `null`. L'app specificata non verrà instrumentata.
- `AppFilter='WebAppOne'` assegna all'app specificata una chiave di strumentazione univoca.
- `AppFilter='WebAppTwo'` assegna all'app specificata una chiave di strumentazione univoca.
- Infine, `AppFilter` USA anche il carattere jolly `'.*'` per trovare la corrispondenza con tutte le app Web che non corrispondono alle regole precedenti e assegnare una chiave di strumentazione predefinita.
- Gli spazi vengono aggiunti per migliorare la leggibilità.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKeyMap 
    @(@{MachineFilter='.*';AppFilter='WebAppExclude'},
      @{MachineFilter='.*';AppFilter='WebAppOne';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx1'},
      @{MachineFilter='.*';AppFilter='WebAppTwo';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2'},
      @{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault'})

```


## <a name="parameters"></a>parameters

### <a name="-instrumentationkey"></a>-InstrumentationKey
**Obbligatorio.** Usare questo parametro per fornire una singola chiave di strumentazione da usare per tutte le app nel computer di destinazione.

### <a name="-instrumentationkeymap"></a>-InstrumentationKeyMap
**Obbligatorio.** Usare questo parametro per specificare più chiavi di strumentazione e un mapping delle chiavi di strumentazione usate da ogni app.
È possibile creare un singolo script di installazione per più computer impostando `MachineFilter`.

> [!IMPORTANT]
> Le app corrisponderanno alle regole nell'ordine in cui vengono fornite le regole. Pertanto, è necessario specificare prima le regole più specifiche e le regole più generiche.

#### <a name="schema"></a>SCHEMA
`@(@{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'})`

- **MachineFilter** è un'espressione C# regolare obbligatoria del nome del computer o della macchina virtuale.
    - '. *' corrisponderà a tutti
    - ' ComputerName ' corrisponderà solo ai computer con il nome specificato.
- **AppFilter** è un'espressione C# regolare obbligatoria del nome del computer o della macchina virtuale.
    - '. *' corrisponderà a tutti
    - ' ApplicationName ' corrisponderà solo alle app IIS con il nome specificato.
- **InstrumentationKey** è necessario per abilitare il monitoraggio delle app che corrispondono ai due filtri precedenti.
    - Lasciare questo valore null se si desidera definire regole per escludere il monitoraggio.


### <a name="-verbose"></a>-Verbose
**Parametro comune.** Usare questa opzione per visualizzare i log dettagliati.


## <a name="output"></a>Output

Per impostazione predefinita, non viene restituito alcun output.

#### <a name="example-verbose-output-from-setting-the-config-file-via--instrumentationkey"></a>Esempio di output dettagliato dall'impostazione del file di configurazione tramite-InstrumentationKey

```
VERBOSE: Operation: InstallWithIkey
VERBOSE: InstrumentationKeyMap parsed:
Filters:
0)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx AppFilter: .* MachineFilter: .*
VERBOSE: set config file
VERBOSE: Config File Path:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config
```

#### <a name="example-verbose-output-from-setting-the-config-file-via--instrumentationkeymap"></a>Esempio di output dettagliato dall'impostazione del file di configurazione tramite-InstrumentationKeyMap

```
VERBOSE: Operation: InstallWithIkeyMap
VERBOSE: InstrumentationKeyMap parsed:
Filters:
0)InstrumentationKey:  AppFilter: WebAppExclude MachineFilter: .*
1)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2 AppFilter: WebAppTwo MachineFilter: .*
2)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault AppFilter: .* MachineFilter: .*
VERBOSE: set config file
VERBOSE: Config File Path:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config
```

## <a name="next-steps"></a>Passaggi successivi

  Visualizzare i dati di telemetria:
 - [Esplora le metriche](../../azure-monitor/app/metrics-explorer.md) per monitorare le prestazioni e l'utilizzo.
- Eseguire [ricerche negli eventi e nei log](../../azure-monitor/app/diagnostic-search.md) per diagnosticare i problemi.
- [Usare Analytics](../../azure-monitor/app/analytics.md) per query più avanzate.
- [Creare dashboard](../../azure-monitor/app/overview-dashboard.md).
 
 Aggiungere altri dati di telemetria:
 - [Creare test Web](monitor-web-app-availability.md) per assicurarsi che il sito rimanga attivo.
- Aggiungere i dati di [telemetria del client Web](../../azure-monitor/app/javascript.md) per visualizzare le eccezioni dal codice della pagina Web e per abilitare le chiamate di traccia.
- [Aggiungere il Application Insights SDK al codice per](../../azure-monitor/app/asp-net.md) poter inserire le chiamate di traccia e log
 
 Eseguire altre operazioni con Application Insights Agent:
 - Usare la guida per [risolvere i problemi relativi](status-monitor-v2-troubleshoot.md) a Application Insights Agent.
 - [Ottenere la configurazione](status-monitor-v2-api-get-config.md) per verificare che le impostazioni siano state registrate correttamente.
 - [Ottenere lo stato](status-monitor-v2-api-get-status.md) per controllare il monitoraggio.
