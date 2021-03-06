---
title: Domande frequenti sull'individuazione, la valutazione e l'analisi delle dipendenze
description: Risposte alle domande più comuni sull'individuazione, la valutazione e l'analisi delle dipendenze in Azure Migrate.
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: e46d1e6ee1dd404e6e040eb394e89dd86a3d4d8e
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2020
ms.locfileid: "79269874"
---
# <a name="discovery-assessment-and-dependency-analysis---common-questions"></a>Individuazione, valutazione e analisi delle dipendenze-domande comuni

Questo articolo risponde alle domande più comuni sull'individuazione, la valutazione e l'analisi delle dipendenze in Azure Migrate. Per altre domande, vedere le risorse seguenti:

- [Domande generali](resources-faq.md) su Azure migrate
- Domande sull' [appliance Azure migrate](common-questions-appliance.md)
- Domande sulla [migrazione del server](common-questions-server-migration.md)
- Risposte alle domande nel [forum Azure migrate](https://aka.ms/AzureMigrateForum)

## <a name="how-many-vms-can-i-discover-with-an-appliance"></a>Quante VM è possibile individuare con un'appliance?

È possibile individuare fino a 10.000 VM VMware, fino a 5.000 macchine virtuali Hyper-V e fino a 250 server fisici usando una singola appliance. Se sono presenti più computer, vedere la pagina relativa alla [scalabilità di una valutazione di Hyper-V](scale-hyper-v-assessment.md), alla [scalabilità di una valutazione VMware](scale-vmware-assessment.md)o alla [scalabilità di una valutazione del server fisico](scale-physical-assessment.md)

## <a name="the-size-of-my-vm-changed-can-i-run-an-assessment-again"></a>La dimensione della macchina virtuale è cambiata. È possibile eseguire nuovamente una valutazione?

L'appliance Azure Migrate raccoglie continuamente informazioni sull'ambiente locale.  Una valutazione è uno snapshot temporizzato delle macchine virtuali locali. Se si modificano le impostazioni in una macchina virtuale che si desidera valutare, utilizzare l'opzione Ricalcola per aggiornare la valutazione con le modifiche più recenti.

## <a name="how-do-i-discover-vms-in-a-multitenant-environment"></a>Ricerca per categorie individuare le macchine virtuali in un ambiente multi-tenant?

- **VMware**: se un ambiente viene condiviso tra i tenant e non si vuole individuare le macchine virtuali di un tenant nella sottoscrizione di un altro tenant, creare server VMware vCenter credenziali che possono accedere solo alle macchine virtuali che si desidera individuare. Quindi, usare queste credenziali quando si avvia l'individuazione nell'appliance Azure Migrate.
- **Hyper-v**: l'individuazione usa le credenziali dell'host Hyper-v. Se le VM condividono lo stesso host Hyper-V, attualmente non è possibile separare l'individuazione.  

## <a name="do-i-need-vcenter-server"></a>È necessario server vCenter?

Sì, Azure Migrate richiede server vCenter in un ambiente VMware per eseguire l'individuazione. Azure Migrate non supporta l'individuazione di host ESXi che non sono gestiti da server vCenter.

## <a name="what-are-the-sizing-options"></a>Quali sono le opzioni di ridimensionamento?

Con il ridimensionamento locale, Azure Migrate non considera i dati sulle prestazioni della macchina virtuale per la valutazione. Azure Migrate valuta le dimensioni delle macchine virtuali in base alla configurazione locale. Con il dimensionamento basato sulle prestazioni, il dimensionamento è basato sui dati di utilizzo.

Ad esempio, se una macchina virtuale locale ha quattro core e 8 GB di memoria al 50% di utilizzo della CPU e il 50% di memoria:
- Il dimensionamento locale consiglierà uno SKU di VM di Azure con quattro core e 8 GB di memoria.
- Il dimensionamento basato sulle prestazioni consiglierà uno SKU di VM con due core e 4 GB di memoria perché la percentuale di utilizzo è considerata.

Analogamente, il ridimensionamento del disco dipende dai criteri di ridimensionamento e dal tipo di archiviazione:
- Se i criteri di ridimensionamento sono basati sulle prestazioni e il tipo di archiviazione è automatico, Azure Migrate prende in considerazione i valori di IOPS e velocità effettiva del disco quando identifica il tipo di disco di destinazione (standard o Premium).
- Se i criteri di ridimensionamento sono basati sulle prestazioni e il tipo di archiviazione è Premium, Azure Migrate consiglia uno SKU del disco Premium in base alle dimensioni del disco locale. La stessa logica viene applicata al dimensionamento del disco quando il dimensionamento è locale e il tipo di archiviazione è standard o Premium.

## <a name="does-performance-history-and-utilization-affect-sizing"></a>La cronologia delle prestazioni e l'utilizzo influiscono sul dimensionamento?

Sì, la cronologia delle prestazioni e l'utilizzo influiscono sul dimensionamento in Azure Migrate.

### <a name="performance-history"></a>Cronologia delle prestazioni

Solo per il dimensionamento basato sulle prestazioni, Azure Migrate raccoglie la cronologia delle prestazioni dei computer locali e quindi la usa per consigliare le dimensioni della macchina virtuale e il tipo di disco in Azure:

1. L'appliance continua a profilare l'ambiente locale per raccogliere i dati di utilizzo in tempo reale ogni 20 secondi.
1. Il dispositivo esegue il rollup degli esempi raccolti di 20 secondi e li usa per creare un singolo punto dati ogni 15 minuti.
1. Per creare il punto dati, l'appliance seleziona il valore massimo da tutti gli esempi di 20 secondi.
1. Il dispositivo invia il punto dati ad Azure.

### <a name="utilization"></a>Utilizzo

Quando si crea una valutazione in Azure, a seconda della durata delle prestazioni e del valore percentile cronologia prestazioni impostato, Azure Migrate calcola il valore di utilizzo effettivo e quindi lo usa per il ridimensionamento.

Se ad esempio si imposta la durata delle prestazioni su un giorno e il valore percentile sul 95 ° percentile, Azure Migrate Ordina i punti di campionamento di 15 minuti inviati dall'agente di raccolta per il giorno precedente in ordine crescente. Il valore del 95 ° percentile viene scelto come utilizzo effettivo.

L'utilizzo del valore 95 ° percentile garantisce che gli outlier vengano ignorati. Gli outlier potrebbero essere inclusi se il Azure Migrate usa il 99 ° percentile. Per selezionare l'utilizzo di picco per il periodo senza gli outlier, impostare Azure Migrate per usare il 99 ° percentile.

## <a name="how-are-import-based-assessments-different-from-assessments-with-discovery-source-as-appliance"></a>In che modo le valutazioni basate sull'importazione sono diverse dalle valutazioni con l'origine di individuazione come appliance?

Le valutazioni basate sull'importazione sono valutazioni create con computer importati in Azure Migrate usando un file CSV. Solo quattro campi sono obbligatori per l'importazione: nome del server, Core, memoria e sistema operativo. Ecco alcuni aspetti da considerare: 
 - I criteri di conformità sono meno rigorosi nelle valutazioni basate sull'importazione sul parametro di tipo di avvio. Se il tipo di avvio non viene specificato, si presuppone che il computer disponga di un tipo di avvio BIOS e che il computer non sia contrassegnato come pronto in modo **condizionale**. Nelle valutazioni con l'origine di individuazione come appliance, la conformità è contrassegnata come **predisposta in modo condizionale** se il tipo di avvio è mancante. Questa differenza nel calcolo della conformità è dovuta al fatto che gli utenti potrebbero non disporre di tutte le informazioni sui computer nelle prime fasi della pianificazione della migrazione quando vengono eseguite le valutazioni basate sull'importazione. 
 - Per le valutazioni di importazione basate sulle prestazioni viene utilizzato il valore di utilizzo fornito dall'utente per il dimensionamento corretto dei calcoli. Poiché il valore di utilizzo viene fornito dall'utente, le opzioni **Cronologia prestazioni** e **utilizzo percentile** sono disabilitate nelle proprietà di valutazione. Nelle valutazioni con l'origine di individuazione come appliance, il valore percentile scelto viene selezionato dai dati sulle prestazioni raccolti dal dispositivo.

## <a name="what-is-dependency-visualization"></a>Informazioni sulla visualizzazione delle dipendenze

La visualizzazione delle dipendenze consente di valutare i gruppi di macchine virtuali per la migrazione con maggiore sicurezza. La visualizzazione delle dipendenze controlla le dipendenze tra computer prima di eseguire una valutazione. Consente di evitare interruzioni impreviste quando si esegue la migrazione ad Azure. Azure Migrate usa la soluzione Mapping dei servizi in Monitoraggio di Azure per abilitare la visualizzazione delle dipendenze. [Altre informazioni](concepts-dependency-visualization.md).

> [!NOTE]
> La visualizzazione delle dipendenze non è disponibile in Azure per enti pubblici.

## <a name="whats-the-difference-between-agent-based-and-agentless"></a>Qual è la differenza tra l'agente e l'agente?

Le differenze tra la visualizzazione senza agente e la visualizzazione basata su agenti sono riepilogate nella tabella.

**Requisito** | **Senza agenti** | **Basata su agenti**
--- | --- | ---
Supporto | Questa opzione è attualmente in anteprima ed è disponibile solo per le macchine virtuali VMware. [Esaminare](migrate-support-matrix-vmware.md#agentless-dependency-visualization) i sistemi operativi supportati. | In disponibilità generale (GA).
Agente di | Non è necessario installare gli agenti nei computer che si vuole controllare in modo incrociato. | Agenti da installare in ogni computer locale che si vuole analizzare: [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows)e [Dependency Agent](https://docs.microsoft.com/azure/azure-monitor/platform/agents-overview#dependency-agent). 
Prerequisiti | [Esaminare](concepts-dependency-visualization.md#agentless-visualization) i prerequisiti e i requisiti di distribuzione. | [Esaminare](concepts-dependency-visualization.md#agent-based-visualization) i prerequisiti e i requisiti di distribuzione.
Log Analytics | Non obbligatorio | Azure Migrate usa la soluzione [mapping dei servizi](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) nei [log di monitoraggio di Azure](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) per la visualizzazione delle dipendenze. [Altre informazioni](concepts-dependency-visualization.md#agent-based-visualization).
Funzionamento | Acquisisce i dati di connessione TCP nei computer abilitati per la visualizzazione delle dipendenze. Dopo l'individuazione, raccoglie i dati a intervalli di cinque minuti. | Mapping dei servizi agenti installati in un computer raccolgono i dati relativi ai processi TCP e alle connessioni in ingresso/in uscita per ogni processo.
Data | Nome del server del computer di origine, processo, nome dell'applicazione.<br/><br/> Nome del server del computer di destinazione, processo, nome dell'applicazione e porta. | Nome del server del computer di origine, processo, nome dell'applicazione.<br/><br/> Nome del server del computer di destinazione, processo, nome dell'applicazione e porta.<br/><br/> Il numero di connessioni, la latenza e le informazioni sul trasferimento dei dati sono raccolte e disponibili per Log Analytics query. 
Visualizzazione | La mappa delle dipendenze di un singolo server può essere visualizzata per una durata di un'ora a 30 giorni. | Mappa delle dipendenze di un singolo server.<br/><br/> La mappa può essere visualizzata solo in un'ora.<br/><br/> Mappa delle dipendenze di un gruppo di server.<br/><br/> Aggiungere e rimuovere i server in un gruppo dalla vista mappa.
Esportazione dati | Attualmente non è possibile scaricare il formato tabulare. | È possibile eseguire query sui dati con Log Analytics.

## <a name="do-i-pay-for-dependency-visualization"></a>Si paga per la visualizzazione delle dipendenze?

No. Scopri di più sui [prezzi Azure migrate](https://azure.microsoft.com/pricing/details/azure-migrate/).

## <a name="what-do-i-install-for-agent-based-dependency-visualization"></a>Cosa si installa per la visualizzazione delle dipendenze basate su agenti?

Per usare la visualizzazione delle dipendenze basata su agente, scaricare e installare gli agenti in ogni computer locale che si vuole valutare:

- [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows)
- [Dependency Agent](../azure-monitor/platform/agents-overview.md#dependency-agent)
- Se sono presenti computer che non dispongono di connettività Internet, scaricare e installare il gateway Log Analytics.

Questi agenti sono necessari solo se si usa la visualizzazione di dipendenza basata su agenti.

## <a name="can-i-use-an-existing-workspace"></a>È possibile usare un'area di lavoro esistente?

Sì, per la visualizzazione delle dipendenze basate su agenti, è possibile allegare un'area di lavoro esistente al progetto di migrazione e usarla per la visualizzazione delle dipendenze. 

## <a name="can-i-export-the-dependency-visualization-report"></a>È possibile esportare il report di visualizzazione delle dipendenze?

No, il report di visualizzazione delle dipendenze nella visualizzazione basata su agente non può essere esportato. Tuttavia, Azure Migrate USA Mapping dei servizi ed è possibile usare l' [API REST mapping dei servizi](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) per recuperare le dipendenze in formato JSON.

## <a name="can-i-automate-agent-installation"></a>È possibile automatizzare l'installazione dell'agente?

Per la visualizzazione delle dipendenze basate su agente:

- Usare uno [script per installare Dependency Agent](../azure-monitor/insights/vminsights-enable-hybrid-cloud.md#installation-script-examples).
- Per MMA, [usare la riga di comando o l'automazione](../azure-monitor/platform/log-analytics-agent.md#installation-and-configuration)oppure usare uno [script](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab).
- Oltre agli script, è possibile utilizzare strumenti di distribuzione come Microsoft endpoint Configuration Manager e [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration) per distribuire gli agenti.

## <a name="what-operating-systems-does-mma-support"></a>Quali sistemi operativi sono supportati da MMA?

- Visualizzare l'elenco dei [sistemi operativi Windows supportati da MMA](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems).
- Visualizzare l'elenco dei [sistemi operativi Linux supportati da MMA](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

## <a name="can-i-visualize-dependencies-for-more-than-one-hour"></a>È possibile visualizzare le dipendenze per più di un'ora?

Per la visualizzazione basata su agenti, è possibile visualizzare le dipendenze per un massimo di un'ora. È possibile tornare indietro fino a un mese fino a una data specifica nella cronologia, ma la durata massima per la visualizzazione è di un'ora. Ad esempio, è possibile usare la durata dell'ora nella mappa delle dipendenze per visualizzare le dipendenze per ieri, ma è possibile visualizzare le dipendenze solo per una finestra di un'ora. Tuttavia, è possibile usare i log di monitoraggio di Azure per [eseguire query sui dati di dipendenza](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) per un periodo di tempo più lungo.

Per la visualizzazione senza agenti, è possibile visualizzare la mappa delle dipendenze di un singolo server da una durata compresa tra un'ora e 30 giorni.

## <a name="can-i-visualize-dependencies-for-groups-of-more-than-10-vms"></a>È possibile visualizzare le dipendenze per gruppi di più di 10 VM?

È possibile [visualizzare le dipendenze](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) per i gruppi che dispongono di un massimo di 10 macchine virtuali. Se si dispone di un gruppo con più di 10 macchine virtuali, è consigliabile suddividere il gruppo in gruppi più piccoli, quindi visualizzare le dipendenze.

## <a name="next-steps"></a>Passaggi successivi

Leggere la [Panoramica di Azure migrate](migrate-services-overview.md).
