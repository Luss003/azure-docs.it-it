---
title: Domande frequenti su Monitoraggio di Azure per le macchine virtuali (GA) | Microsoft Docs
description: Monitoraggio di Azure per le macchine virtuali è una soluzione di Azure che combina il monitoraggio dell'integrità e delle prestazioni del sistema operativo delle macchine virtuali di Azure, nonché l'individuazione automatica dei componenti e delle dipendenze delle applicazioni con altre risorse e mappa la comunicazione tra questi elementi. Questo articolo risponde alle domande più comuni sulla versione GA.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/24/2020
ms.openlocfilehash: 3877632565c1ca2c9a16681e03f8931a94af0599
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/28/2020
ms.locfileid: "76765755"
---
# <a name="azure-monitor-for-vms-generally-available-ga-frequently-asked-questions"></a>Monitoraggio di Azure per le macchine virtuali domande frequenti disponibili a livello generale (GA)

Queste domande frequenti sulla disponibilità generale riguardano le modifiche che si verificano in Monitoraggio di Azure per le macchine virtuali durante la preparazione per la versione GA. 

## <a name="updates-for-azure-monitor-for-vms"></a>Aggiornamenti per Monitoraggio di Azure per le macchine virtuali

È stata rilasciata una nuova versione di Monitoraggio di Azure per le macchine virtuali. I clienti che abilitano i monitoraggi di Azure per le macchine virtuali riceveranno ora la nuova versione, ma verrà richiesto di aggiornare i clienti esistenti che usano già Monitoraggio di Azure per le macchine virtuali. Queste domande frequenti e la nostra documentazione offrono indicazioni per eseguire un aggiornamento su larga scala se si dispone di distribuzioni di grandi dimensioni in più aree di lavoro.

Con questo aggiornamento, Monitoraggio di Azure per le macchine virtuali i dati sulle prestazioni vengono archiviati nella stessa tabella *InsightsMetrics* di [monitoraggio di Azure per i contenitori](container-insights-overview.md), semplificando l'esecuzione di query sui due set di dati. Inoltre, è possibile archiviare set di dati più diversi che non è stato possibile archiviare nella tabella usata in precedenza. 

Nella prossima settimana o due verranno aggiornate anche le visualizzazioni delle prestazioni per l'uso della nuova tabella.

Ci rendiamo conto che la richiesta di aggiornamento da parte dei clienti esistenti causa un'alterazione del flusso di lavoro, motivo per cui abbiamo scelto di eseguire questa operazione ora in anteprima pubblica anziché in un secondo momento dopo GA.


## <a name="what-is-changing"></a>Cosa cambierà

È stata rilasciata una nuova soluzione, denominata VMInsights, che include funzionalità aggiuntive per la raccolta di dati insieme a una nuova posizione per archiviare questi dati nell'area di lavoro Log Analytics. 

In passato, la soluzione ServiceMap è stata abilitata nell'area di lavoro e i contatori delle prestazioni sono stati impostati nell'area di lavoro Log Analytics per inviare i dati alla tabella *Perf* . Questa nuova soluzione invia i dati a una tabella denominata *InsightsMetrics* usata anche da monitoraggio di Azure per i contenitori. Questo schema di tabella consente di archiviare metriche e set di dati del servizio aggiuntivi che non sono compatibili con il formato di tabella delle *prestazioni* .


## <a name="how-do-i-upgrade"></a>Ricerca per categorie l'aggiornamento?
Ogni macchina virtuale che richiede l'aggiornamento verrà identificata nella scheda attività **iniziali** Monitoraggio di Azure per le macchine virtuali nel portale di Azure. È possibile aggiornare una singola macchina virtuale o selezionare multiplo per eseguire l'aggiornamento insieme. Usare il comando seguente per eseguire l'aggiornamento usando PowerShell:

```PowerShell
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName <resource-group-name> -WorkspaceName <workspace-name> -IntelligencePackName "VMInsights" -Enabled $True
```

## <a name="what-should-i-do-about-the-performance-counters-in-my-workspace-if-i-install-the-vminsights-solution"></a>Cosa devo fare per i contatori delle prestazioni nell'area di lavoro se si installa la soluzione VMInsights?

Il metodo corrente di abilitazione di Monitoraggio di Azure per le macchine virtuali usa i contatori delle prestazioni nell'area di lavoro. Il nuovo metodo archivia questi dati in una nuova tabella denominata `InsightsMetrics`.

Una volta aggiornata l'interfaccia utente per l'uso dei dati nella tabella `InsightsMetrics`, verrà aggiornata la documentazione e verrà comunicato questo annuncio tramite più canali, inclusa la visualizzazione di un banner nel portale di Azure. A questo punto, è possibile scegliere di disabilitare questi [contatori delle prestazioni](vminsights-enable-overview.md#performance-counters-enabled) nell'area di lavoro se non è più necessario usarli. 

>[!NOTE]
>Se sono presenti regole di avviso che fanno riferimento a questi contatori nella tabella `Perf`, è necessario aggiornarli in modo che facciano riferimento a nuovi dati archiviati nella tabella `InsightsMetrics`. Vedere la documentazione per le query di log di esempio che è possibile usare per fare riferimento a questa tabella.
>

Se si decide di tenere i contatori delle prestazioni abilitati, verranno addebitati i dati inseriti e archiviati nella tabella `Perf` in base a [prezzo Log Analytics [(https://azure.microsoft.com/pricing/details/monitor/).

## <a name="how-will-this-change-affect-my-alert-rules"></a>In che modo questa modifica influirà sulle regole di avviso?

Se sono stati creati [avvisi del log](../platform/alerts-unified-log.md) che eseguono una query sulla tabella `Perf` destinata ai contatori delle prestazioni abilitati nell'area di lavoro, è necessario aggiornare queste regole per fare riferimento alla tabella `InsightsMetrics`. Queste linee guida si applicano anche a tutte le regole di ricerca nei log che usano `ServiceMapComputer_CL` e `ServiceMapProcess_CL`, perché tali set di dati vengono spostati nelle tabelle `VMComputer` e `VMProcess`.

Le domande frequenti e la documentazione per includere le regole di avviso di ricerca log di esempio per i set di dati raccolti vengono aggiornate.

## <a name="how-will-this-affect-my-bill"></a>Come influirà la fattura?

La fatturazione è ancora basata sui dati inseriti e conservati nell'area di lavoro Log Analytics.

I dati sulle prestazioni a livello di computer raccolti sono gli stessi, con dimensioni simili ai dati archiviati nella tabella `Perf` e che costeranno approssimativamente la stessa quantità.

## <a name="what-if-i-only-want-to-use-service-map"></a>Cosa accade se si vuole usare solo Mapping dei servizi?

Questo è un problema. Quando si visualizzano Monitoraggio di Azure per le macchine virtuali sull'aggiornamento imminente, vengono visualizzate richieste nella portale di Azure. Una volta rilasciato, verrà visualizzato un messaggio che richiede di eseguire l'aggiornamento alla nuova versione. Se si preferisce usare solo la funzionalità [Maps](vminsights-maps.md) , è possibile scegliere di non eseguire l'aggiornamento e continuare a usare la funzionalità maps in monitoraggio di Azure per le macchine virtuali e la soluzione mapping dei servizi a cui si accede dall'area di lavoro o dal riquadro del dashboard.

Se si sceglie di abilitare manualmente i contatori delle prestazioni nell'area di lavoro, potrebbe essere possibile visualizzare i dati in alcuni dei grafici delle prestazioni visualizzati da monitoraggio di Azure. Una volta rilasciata la nuova soluzione, i grafici delle prestazioni vengono aggiornati per eseguire query sui dati archiviati nella tabella `InsightsMetrics`. Se si desidera visualizzare i dati di tale tabella in questi grafici, sarà necessario eseguire l'aggiornamento alla nuova versione di Monitoraggio di Azure per le macchine virtuali.

Le modifiche per spostare i dati da `ServiceMapComputer_CL` e `ServiceMapProcess_CL` avranno effetto sia Mapping dei servizi che Monitoraggio di Azure per le macchine virtuali, quindi è comunque necessario pianificare l'aggiornamento.

Se si sceglie di non eseguire l'aggiornamento alla soluzione **VMInsights** , si continuerà a fornire le versioni legacy delle cartelle di lavoro delle prestazioni che fanno riferimento ai dati nella tabella `Perf`.  

## <a name="will-the-service-map-data-sets-also-be-stored-in-insightsmetrics"></a>I set di dati Mapping dei servizi verranno archiviati anche in InsightsMetrics?

I set di dati non verranno duplicati se si utilizzano entrambe le soluzioni. Entrambe le offerte condividono i set di dati che verranno archiviati nelle tabelle `VMComputer` (in precedenza ServiceMapComputer_CL), `VMProcess` (in precedenza ServiceMapProcess_CL), `VMConnection`e `VMBoundPort` per archiviare i set di dati della mappa raccolti.  

Nella tabella `InsightsMetrics` verranno archiviati i set di dati di VM, processi e servizi raccolti e verranno popolati solo se si usano Monitoraggio di Azure per le macchine virtuali e la soluzione VM Insights. La soluzione Mapping dei servizi non raccoglie né o archivia i dati nella tabella `InsightsMetrics`.

## <a name="will-i-be-double-charged-if-i-have-the-service-map-and-vminsights-solutions-in-my-workspace"></a>Viene addebitato un costo doppio se si hanno le soluzioni Mapping dei servizi e VMInsights nell'area di lavoro?

No, le due soluzioni condividono i set di dati della mappa archiviati in `VMComputer` (in precedenza ServiceMapComputer_CL), `VMProcess` (in precedenza ServiceMapProcess_CL), `VMConnection`e `VMBoundPort`. I dati non verranno addebitati a doppio se si dispone di entrambe le soluzioni nell'area di lavoro.

## <a name="if-i-remove-either-the-service-map-or-vminsights-solution-will-it-remove-my-data"></a>Se si rimuove la soluzione Mapping dei servizi o VMInsights, i dati vengono rimossi?

No, le due soluzioni condividono i set di dati della mappa archiviati in `VMComputer` (in precedenza ServiceMapComputer_CL), `VMProcess` (in precedenza ServiceMapProcess_CL), `VMConnection`e `VMBoundPort`. Se si rimuove una delle soluzioni, questi set di dati si notano che è ancora disponibile una soluzione che utilizza i dati e rimane nell'area di lavoro Log Analytics. È necessario rimuovere entrambe le soluzioni dall'area di lavoro per consentire la rimozione dei dati.

## <a name="when-will-this-update-be-released"></a>Quando verrà rilasciato questo aggiornamento?

Si prevede di rilasciare l'aggiornamento per Monitoraggio di Azure per le macchine virtuali all'inizio del gennaio 2020. Man mano che ci avviciniamo alla data di rilascio nel gennaio, pubblichiamo gli aggiornamenti e presentiamo le notifiche nella portale di Azure quando si apre Monitoraggio di Azure.

## <a name="health-feature-is-in-limited-public-preview"></a>La funzionalità di integrità è in anteprima pubblica limitata

Abbiamo ricevuto numerosi commenti e suggerimenti dai clienti sul set di funzionalità per l'integrità delle macchine virtuali. Questa funzionalità ha suscitato molto interesse e molte aspettative relative alla possibilità di supportare i flussi di lavoro di monitoraggio. Stiamo pianificando una serie di modifiche per aggiungere funzionalità e rispondere al feedback ricevuto. 

Per ridurre al minimo l'effetto di queste modifiche apportate ai nuovi clienti, questa funzionalità è stata spostata in un' **anteprima pubblica limitata**. Questo aggiornamento è stato eseguito nel 2019 ottobre.

Si prevede di riavviare questa funzionalità di integrità in 2020, dopo che Monitoraggio di Azure per le macchine virtuali è disponibile in GA.

## <a name="how-do-existing-customers-access-the-health-feature"></a>In che modo i clienti esistenti accedono alla funzionalità di integrità?

I clienti esistenti che usano la funzionalità di integrità continueranno ad avere accesso a tale funzionalità, ma non verranno offerti ai nuovi clienti.  

Per accedere alla funzionalità, è possibile aggiungere il flag di funzionalità seguente `feature.vmhealth=true` all'URL del portale di Azure [https://portal.azure.com](https://portal.azure.com). Esempio `https://portal.azure.com/?feature.vmhealth=true`.

È anche possibile usare questo breve URL, che imposta automaticamente il flag di funzionalità: [https://aka.ms/vmhealthpreview](https://aka.ms/vmhealthpreview).

Come cliente esistente, è possibile continuare a usare la funzionalità di integrità nelle VM connesse a una configurazione dell'area di lavoro esistente con la funzionalità di integrità.  

## <a name="i-use-vm-health-now-with-one-environment-and-would-like-to-deploy-it-to-a-new-one"></a>Si usa ora l'integrità VM con un ambiente e si vuole distribuirla in una nuova

Se si è un cliente esistente che utilizza la funzionalità di integrità e si desidera utilizzarlo per un nuovo Rollup, contattare Microsoft all'indirizzo vminsights@microsoft.com per richiedere istruzioni.

## <a name="next-steps"></a>Passaggi successivi

Per informazioni sui requisiti e i metodi per il monitoraggio delle macchine virtuali, vedere [Distribuire Monitoraggio di Azure per le macchine virtuali](vminsights-enable-overview.md).
