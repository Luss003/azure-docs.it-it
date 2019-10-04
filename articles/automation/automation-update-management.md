---
title: Soluzione Gestione aggiornamenti in Azure
description: Questo articolo illustra l'uso della soluzione Gestione aggiornamenti di Azure per gestire gli aggiornamenti per i computer Windows e Linux.
services: automation
ms.service: automation
ms.subservice: update-management
author: bobbytreed
ms.author: robreed
ms.date: 05/22/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 253fc940cfb42aa9bf7e93dd631d2ca596f7db6f
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/30/2019
ms.locfileid: "71677858"
---
# <a name="update-management-solution-in-azure"></a>Soluzione Gestione aggiornamenti in Azure

È possibile usare la soluzione Gestione aggiornamenti in automazione di Azure per gestire gli aggiornamenti del sistema operativo per i computer Windows e Linux in Azure, in ambienti locali o in altri provider di servizi cloud. È possibile valutare rapidamente lo stato degli aggiornamenti disponibili in tutti i computer agente e gestire il processo di installazione degli aggiornamenti necessari per i server.

È possibile abilitare Gestione aggiornamenti per le macchine virtuali direttamente dall'account di Automazione di Azure. Per informazioni su come abilitare Gestione aggiornamenti per le macchine virtuali dall'account di Automazione, vedere [Gestire gli aggiornamenti per più macchine virtuali](manage-update-multi.md). È anche possibile abilitare Gestione aggiornamenti per una macchina virtuale dalla pagina della macchina virtuale nel portale di Azure. Questo scenario è disponibile per macchine virtuali [Linux](../virtual-machines/linux/tutorial-config-management.md#enable-update-management) e [Windows](../virtual-machines/windows/tutorial-config-management.md#enable-update-management).

> [!NOTE]
> La soluzione Gestione aggiornamenti richiede il collegamento di un'area di lavoro Log Analytics all'account di automazione. Per un elenco definitivo delle aree supportate, vedere [mapping dell'area di lavoro di Azure](./how-to/region-mappings.md). I mapping dell'area non influiscono sulla possibilità di gestire le macchine virtuali in un'area separata rispetto all'account di automazione.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="solution-overview"></a>Panoramica della soluzione

I computer gestiti da Gestione aggiornamenti usano le configurazioni seguenti per le valutazioni e le distribuzioni degli aggiornamenti:

* Microsoft Monitoring Agent (MMA) per Windows o Linux
* PowerShell DSC (Desired State Configuration) per Linux
* Ruolo di lavoro ibrido per runbook di Automazione
* Microsoft Update o Windows Server Update Services (WSUS) per computer Windows

Il diagramma seguente offre una visualizzazione concettuale del comportamento e del flusso dei dati e indica in che modo la soluzione valuta e applica gli aggiornamenti della sicurezza a tutti i computer Windows Server e Linux connessi in un'area di lavoro:

![Flusso del processo di Gestione aggiornamenti](./media/automation-update-management/update-mgmt-updateworkflow.png)

Gestione aggiornamenti può essere usato per l'onboarding nativo di computer in più sottoscrizioni nello stesso tenant.

Una volta rilasciato un pacchetto, sono necessarie 2-3 ore prima che la patch venga visualizzata per i computer Linux per la valutazione. Per i computer Windows, la visualizzazione della patch per la valutazione dopo il rilascio richiede 12-15 ore.

Quando un computer completa un'analisi per la conformità degli aggiornamenti, l'agente trasmette le informazioni in blocco ai log di monitoraggio di Azure. In un computer Windows l'analisi della conformità viene eseguita ogni 12 ore per impostazione predefinita.

Oltre all'analisi pianificata, l'analisi della conformità degli aggiornamenti viene avviata entro 15 minuti dal momento del riavvio di MMA e prima e dopo l'installazione degli aggiornamenti.

Per un computer Linux, l'analisi di conformità viene eseguita ogni ora per impostazione predefinita. Se l'agente MMA viene riavviato, viene avviata un'analisi della conformità entro 15 minuti.

La soluzione genera report sullo stato di aggiornamento del computer in base all'origine configurata per la sincronizzazione. Se il computer Windows è configurato per l'invio di report a WSUS, in base a quando WSUS ha eseguito l'ultima sincronizzazione con Microsoft Update i risultati possono differire da quanto visualizzato da Microsoft Update. Questo comportamento è lo stesso per i computer Linux configurati per l'invio di report a un repository locale anziché a un repository pubblico.

> [!NOTE]
> Per inviare correttamente un report al servizio, Gestione aggiornamenti richiede l'abilitazione di determinati URL e porte. Per altre informazioni su questi requisiti, vedere l'articolo sulla [pianificazione della rete per i ruoli di lavoro ibridi](automation-hybrid-runbook-worker.md#network-planning).

È possibile distribuire e installare gli aggiornamenti software nei computer che richiedono gli aggiornamenti creando una distribuzione pianificata. Gli aggiornamenti classificati come *facoltativi* non sono inclusi nell'ambito della distribuzione per i computer Windows. Nell'ambito della distribuzione vengono inclusi solo gli aggiornamenti obbligatori.

La distribuzione pianificata definisce quali computer di destinazione ricevono gli aggiornamenti applicabili, specificando in modo esplicito i computer o selezionando un [gruppo di computer](../azure-monitor/platform/computer-groups.md) basato sulle ricerche log di un determinato set di computer o su una [query di Azure](#azure-machines) che seleziona dinamicamente le VM di Azure in base ai criteri specificati. Questi gruppi sono diversi dalla [configurazione dell'ambito](../azure-monitor/insights/solution-targeting.md), che viene usata solo per determinare quali computer ottengono i Management Pack che abilitano la soluzione.

Si specifica anche una pianificazione per approvare e impostare un periodo di tempo durante il quale è possibile installare gli aggiornamenti. Questo periodo di tempo viene chiamato finestra di manutenzione. Dieci minuti della finestra di manutenzione sono riservati ai riavvii se è necessario un riavvio ed è stata selezionata l'opzione di riavvio appropriato. Se l'applicazione di patch richiede più tempo del previsto e la finestra di manutenzione è inferiore a dieci minuti, non si verificherà un riavvio.

Gli aggiornamenti vengono installati da runbook in Automazione di Azure. Questi runbook non richiedono alcuna configurazione e non possono essere visualizzati. Quando si crea una distribuzione degli aggiornamenti, nella distribuzione viene creata una pianificazione che avvia un runbook di aggiornamento master alla data e ora specificate per i computer inclusi. Il runbook master avvia un runbook figlio in ogni agente per installare gli aggiornamenti necessari.

Alla data e ora specificate nella distribuzione degli aggiornamenti, i computer di destinazione eseguono la distribuzione in parallelo. Prima dell'installazione viene eseguita un'analisi per verificare che gli aggiornamenti siano ancora necessari. Per i computer client WSUS, la distribuzione degli aggiornamenti ha esito negativo se gli aggiornamenti non sono approvati in WSUS.

Il computer registrato per Gestione aggiornamenti in più di un'area di lavoro di Log Analytics (multihoming) non è supportato.

## <a name="clients"></a>Client

### <a name="supported-client-types"></a>Tipi di client supportati

La tabella seguente mostra un elenco dei sistemi operativi supportati per le valutazioni degli aggiornamenti. L'applicazione di patch richiede un ruolo di lavoro ibrido per Runbook. Per informazioni sui requisiti del ruolo di lavoro ibrido per Runbook, vedere le guide all'installazione per [Windows lavoro ibrido](automation-windows-hrw-install.md#installing-the-windows-hybrid-runbook-worker) e [Linux lavoro ibrido](automation-linux-hrw-install.md#installing-a-linux-hybrid-runbook-worker).

|Sistema operativo  |Note  |
|---------|---------|
|Windows Server 2019 (Datacenter/datacenter core/standard)<br><br>Windows Server 2016 (Datacenter/datacenter core/standard)<br><br>Windows Server 2012 R2 (Datacenter/standard)<br><br>Windows Server 2012<br><br>Windows Server 2008 R2 (RTM e SP1 Standard)||
|CentOS 6 (x86/x64) e 7 (x64)      | Gli agenti Linux devono avere accesso a un repository degli aggiornamenti. L'applicazione di patch basata sulla classificazione richiede "yum" per restituire i dati sulla sicurezza che non sono predefiniti in CentOS. Per altre informazioni sull'applicazione di patch basata sulla classificazione su CentOS, vedere classificazioni degli [aggiornamenti in Linux](#linux-2)          |
|Red Hat Enterprise 6 (x86/x64) e 7 (x64)     | Gli agenti Linux devono avere accesso a un repository degli aggiornamenti.        |
|SUSE Linux Enterprise Server 11 (x86/x64) e 12 (x64)     | Gli agenti Linux devono avere accesso a un repository degli aggiornamenti.        |
|Ubuntu 14.04 LTS, 16.04 LTS e 18.04 LTS (x86/x64)      |Gli agenti Linux devono avere accesso a un repository degli aggiornamenti.         |

> [!NOTE]
> I set di scalabilità di macchine virtuali di Azure possono essere gestiti con Gestione aggiornamenti. Gestione aggiornamenti funziona sulle istanze stesse e non sull'immagine di base. È necessario pianificare gli aggiornamenti in modo incrementale, perché non aggiornare tutte le istanze di macchina virtuale in una sola volta.
> I nodi VMSS possono essere aggiunti attenendosi alla procedura descritta in [onboarding a computer non Azure](automation-tutorial-installed-software.md#onboard-a-non-azure-machine).

### <a name="unsupported-client-types"></a>Tipi di client non supportati

Nella tabella seguente sono elencati i sistemi operativi non supportati:

|Sistema operativo  |Note  |
|---------|---------|
|Client Windows     | I sistemi operativi client, ad esempio Windows 7 e Windows 10, non sono supportati.        |
|Windows Server 2016 Nano Server     | Non supportati.       |
|Nodi del servizio Azure Kubernetes | Non supportati. Usare il processo di applicazione delle patch dettagliato in [applicare gli aggiornamenti di sicurezza e kernel ai nodi Linux in Azure Kubernetes Service (AKS)](../aks/node-updates-kured.md)|

### <a name="client-requirements"></a>Requisiti per i client

#### <a name="windows"></a>Windows

Gli agenti Windows devono essere configurati per comunicare con un server WSUS o devono avere accesso a Microsoft Update. Gestione aggiornamenti può essere usato con System Center Configuration Manager. Per altre informazioni sugli scenari di integrazione, vedere [Integrare System Center Configuration Manager con Gestione aggiornamenti](oms-solution-updatemgmt-sccmintegration.md#configuration). L'[agente Windows](../azure-monitor/platform/agent-windows.md) è obbligatorio. L'agente viene installato automaticamente in caso di onboarding di una macchina virtuale di Azure.

> [!NOTE]
> È possibile che un utente modifichi Criteri di gruppo in modo che il riavvio del computer possa essere eseguito solo dall'utente, non dal sistema. I computer gestiti potrebbero rimanere bloccati, se Gestione aggiornamenti non dispone dei diritti necessari per riavviare il computer senza interazione manuale da parte dell'utente.
>
> Per ulteriori informazioni, vedere [configurare le impostazioni di criteri di gruppo per aggiornamenti automatici](https://docs.microsoft.com/en-us/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates).

#### <a name="linux"></a>Linux

Per Linux, il computer deve avere accesso a un repository degli aggiornamenti. Il repository degli aggiornamenti può essere privato o pubblico. Per interagire con Gestione aggiornamenti è necessario TLS 1.1 o TLS 1.2. Un agente di Log Analytics per Linux configurato per l'invio di report a più di un'area di lavoro di Log Analytics non è supportato per questa soluzione.  Nel computer deve essere installato anche Python 2. x.

Per informazioni su come installare l'agente di Log Analytics per Linux e scaricare la versione più recente, vedere [log Analytics Agent per Linux](https://github.com/microsoft/oms-agent-for-linux). Per informazioni su come installare l'agente di Log Analytics per Windows, vedere [Microsoft Monitoring Agent per Windows](../log-analytics/log-analytics-windows-agent.md).

## <a name="permissions"></a>Autorizzazioni

Per creare e gestire le distribuzioni degli aggiornamenti, sono necessarie autorizzazioni specifiche. Per informazioni su queste autorizzazioni, vedere [Controllo degli accessi in base al ruolo - Gestione aggiornamenti](automation-role-based-access-control.md#update-management).

## <a name="solution-components"></a>Componenti della soluzione

La soluzione è costituita dalle risorse seguenti. Le risorse vengono aggiunte all'account di Automazione. Sono agenti con connessione diretta o fanno parte di un gruppo di gestione connesso a Operations Manager.

### <a name="hybrid-worker-groups"></a>Gruppi di ruoli di lavoro ibridi

Dopo aver abilitato questa soluzione, qualsiasi computer Windows direttamente connesso all'area di lavoro Log Analytics viene configurato automaticamente come ruolo di lavoro ibrido per runbook per supportare i runbook che fanno parte di questa soluzione.

Ogni computer Windows gestito dalla soluzione è elencato nella pagina dei **gruppi di ruoli di lavoro ibridi** come **gruppo di ruoli di lavoro ibridi per il sistema** per l'account di Automazione. Le soluzioni usano la convenzione di denominazione *Hostname FQDN_GUID*. Non è possibile applicare runbook a questi gruppi nell'account. In caso contrario avranno esito negativo. Questi gruppi sono destinati solo al supporto della soluzione di gestione.

È possibile aggiungere i computer Windows a un gruppo di ruoli di lavoro ibridi per runbook nell'account di Automazione per supportare i runbook di Automazione, se si usa lo stesso account sia per la soluzione che per l'appartenenza al gruppo di ruoli di lavoro ibridi per runbook. Questa funzionalità è stata aggiunta alla versione 7.2.12024.0 del ruolo di lavoro ibrido per runbook.

### <a name="management-packs"></a>Management Pack

Se il gruppo di gestione di System Center Operations Manager è connesso all'area di lavoro Log Analytics, in Operations Manager verranno installati i Management Pack seguenti. Questi Management Pack vengono installati anche su computer Windows direttamente connessi dopo l'aggiunta della soluzione. Non è necessario configurare o gestire questi Management Pack.

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Update Deployment MP

> [!NOTE]
> Se si dispone di un gruppo di gestione Operations Manager 1807 o 2019 con gli agenti configurati a livello di gruppo di gestione da associare a un'area di lavoro, la soluzione alternativa corrente per visualizzarli è eseguire l'override di **IsAutoRegistrationEnabled** su **true** in regola **Microsoft. IntelligencePacks. AzureAutomation. HybridAgent. init** .

Per altre informazioni sulla modalità di aggiornamento dei Management Pack per le soluzioni, vedere [connettere Operations Manager ai log di monitoraggio di Azure](../azure-monitor/platform/om-agents.md).

> [!NOTE]
> Nei sistemi con l'agente di Operations Manager, quest'ultimo deve essere aggiornato a Microsoft Monitoring Agent per essere gestito completamente da Gestione aggiornamenti. Per informazioni su come aggiornare l'agente, vedere [How to upgrade an Operations Manager agent](https://docs.microsoft.com/system-center/scom/deploy-upgrade-agents) (Come aggiornare un agente di Operations Manager). Per gli ambienti che usano Operations Manager, è necessario eseguire System Center Operations Manager 2012 R2 UR 14 o versione successiva.

## <a name="onboard"></a>Abilitare Gestione aggiornamenti

Per avviare l'applicazione di patch ai sistemi, è necessario abilitare la soluzione Gestione aggiornamenti. Esistono molti modi per eseguire l'onboarding dei computer in Gestione aggiornamenti. Per eseguire l'onboarding della soluzione si consigliano i metodi supportati seguenti:

* [Da una macchina virtuale](automation-onboard-solutions-from-vm.md)
* [Dall'esplorazione di più computer](automation-onboard-solutions-from-browse.md)
* [Dall'account di Automazione](automation-onboard-solutions-from-automation-account.md)
* [Con un runbook di Automazione di Azure](automation-onboard-solutions.md)

### <a name="confirm-that-non-azure-machines-are-onboarded"></a>Verificare l'onboarding di computer non di Azure

Per verificare che i computer connessi direttamente comunicano con i log di monitoraggio di Azure, dopo alcuni minuti è possibile eseguire una delle ricerche nei log seguenti.

#### <a name="linux"></a>Linux

```loganalytics
Heartbeat
| where OSType == "Linux" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

#### <a name="windows"></a>Windows

```loganalytics
Heartbeat
| where OSType == "Windows" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

In un computer Windows, è possibile esaminare le informazioni seguenti per verificare la connettività degli agenti con i log di monitoraggio di Azure:

1. Aprire **Microsoft Monitoring Agent** nel Pannello di controllo. Nella scheda **Log Analytics di Azure**, l'agente visualizza il messaggio per indicare che: **Microsoft Monitoring Agent ha eseguito la connessione a Log Analytics**.
2. Aprire il registro eventi di Windows. Passare a **Registri applicazioni e servizi\Operations Manager** e cercare gli ID evento 3000 e 5002 del **connettore del servizio** di origine. Questi eventi indicano che il computer ha eseguito la registrazione all'area di lavoro Log Analytics e sta ricevendo la configurazione.

Se l'agente non è in grado di comunicare con i log di monitoraggio di Azure e l'agente è configurato per la comunicazione con Internet tramite un firewall o un server proxy, verificare che il firewall o il server proxy sia configurato correttamente. Per sapere come verificare se il firewall o il server proxy è configurato correttamente, vedere [Connettere computer Windows al servizio Log Analytics in Azure](../azure-monitor/platform/agent-windows.md) oppure [Raccogliere dati dal computer Linux ospitato nell'ambiente in uso](../log-analytics/log-analytics-agent-linux.md).

> [!NOTE]
> Se i sistemi Linux sono configurati per la comunicazione con un proxy o il gateway di Log Analytics e si sta eseguendo l'onboarding di questa soluzione, aggiornare le autorizzazioni *proxy.conf* per concedere al gruppo omiuser le necessarie autorizzazioni di lettura per il file usando i comandi seguenti:
>
> `sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf`
> `sudo chmod 644 /etc/opt/microsoft/omsagent/proxy.conf`

Gli agenti Linux appena aggiunti visualizzano lo stato **Aggiornato** dopo l'esecuzione di una valutazione. Il processo può richiedere fino a 6 ore.

Per verificare che un gruppo di gestione di Operations Manager comunichi con i log di monitoraggio di Azure, vedere convalidare [Operations Manager integrazione con i log di monitoraggio di Azure](../azure-monitor/platform/om-agents.md#validate-operations-manager-integration-with-azure-monitor).

## <a name="data-collection"></a>Raccolta dei dati

### <a name="supported-agents"></a>Agenti supportati

La tabella seguente descrive le origini connesse che sono supportate da questa soluzione:

| Origine connessa | Supportato | DESCRIZIONE |
| --- | --- | --- |
| Agenti di Windows |Sì |La soluzione raccoglie informazioni sugli aggiornamenti del sistema dagli agenti Windows e quindi avvia l'installazione degli aggiornamenti necessari. |
| Agenti Linux |Sì |La soluzione raccoglie informazioni sugli aggiornamenti del sistema dagli agenti Linux e quindi avvia l'installazione degli aggiornamenti necessari nelle distribuzioni supportate. |
| Gruppo di gestione di Operations Manager |Sì |La soluzione raccoglie informazioni sugli aggiornamenti del sistema dagli agenti in un gruppo di gestione connesso.<br/>Non è necessaria una connessione diretta dall'agente Operations Manager ai log di monitoraggio di Azure. I dati vengono inoltrati dal gruppo di gestione all'area di lavoro Log Analytics. |

### <a name="collection-frequency"></a>Frequenza della raccolta

Per ogni computer Windows gestito viene eseguita un'analisi due volte al giorno. Ogni 15 minuti viene chiamata l'API Windows per eseguire una query su data/ora dell'ultimo aggiornamento e determinare se lo stato è cambiato. Se lo stato è cambiato, viene avviata un'analisi della conformità.

Un'analisi viene eseguita ogni ora per ogni computer Linux gestito.

La visualizzazione nel dashboard dei dati aggiornati dei computer gestiti può richiedere da 30 minuti a 6 ore.

Il monitoraggio di Azure medio registra l'utilizzo dei dati per un computer che usa Gestione aggiornamenti è approssimativamente 25MB al mese. Questo valore è solo un'approssimazione ed è soggetto a modifiche, in base all'ambiente in uso. È consigliabile monitorare l'ambiente per visualizzare l'esatta quantità di dati usati.

## <a name="viewing-update-assessments"></a>Visualizzare la valutazione degli aggiornamenti

Selezionare **Gestione aggiornamenti** nell'account di Automazione per visualizzare lo stato dei computer.

Questa visualizzazione contiene informazioni sui computer, sugli aggiornamenti mancanti, sulle distribuzioni degli aggiornamenti e sulle distribuzioni degli aggiornamenti pianificate. Nella colonna **CONFORMITÀ** è possibile vedere quando è stata eseguita l'ultima valutazione del computer. Nella colonna **UPDATE AGENT READINESS** (Idoneità agente di aggiornamento) è possibile verificare l'integrità dell'agente di aggiornamento. Se si verifica un problema, selezionare il collegamento alla documentazione sulla risoluzione dei problemi, che contiene informazioni utili sulle operazioni da eseguire per risolverlo.

Per eseguire una ricerca log che restituisce informazioni sul computer, l'aggiornamento o la distribuzione, selezionare l'elemento nell'elenco. Si apre il riquadro **Ricerca log** con una query per l'elemento selezionato:

![Visualizzazione predefinita di Gestione aggiornamenti](media/automation-update-management/update-management-view.png)

## <a name="install-updates"></a>Installare gli aggiornamenti

Dopo aver valutato gli aggiornamenti per tutti i computer Linux e Windows nell'area di lavoro, è possibile installare gli aggiornamenti necessari creando una *distribuzione degli aggiornamenti*. Per creare una distribuzione degli aggiornamenti, è necessario avere accesso in scrittura all'account di automazione e l'accesso in scrittura alle macchine virtuali di Azure di destinazione della distribuzione. Una distribuzione degli aggiornamenti è un'installazione pianificata di aggiornamenti necessari per uno o più computer. Specificare la data e l'ora della distribuzione e un computer o gruppo di computer da includere nell'ambito della distribuzione. Per altre informazioni sui gruppi di computer, vedere [gruppi di computer nei log di monitoraggio di Azure](../azure-monitor/platform/computer-groups.md).

Quando si includono gruppi di computer nella distribuzione degli aggiornamenti, l'appartenenza ai gruppi viene valutata una sola volta al momento della creazione della pianificazione. Le modifiche successive a un gruppo non vengono riflesse. Per aggirare questo problema, usare i [gruppi dinamici](#using-dynamic-groups). questi gruppi vengono risolti in fase di distribuzione e sono definiti da una query per le macchine virtuali di Azure o da una ricerca salvata per le macchine virtuali non di Azure.

> [!NOTE]
> Per impostazione predefinita, le macchine virtuali di Windows distribuite da Azure Marketplace ricevono aggiornamenti automatici dal servizio Windows Update. Questo comportamento non cambia quando si aggiunge questa soluzione o si aggiungono macchine virtuali di Windows all'area di lavoro. Se gli aggiornamenti non vengono gestiti attivamente con questa soluzione, è applicabile il comportamento predefinito, ovvero gli aggiornamenti vengono applicati automaticamente.

Per evitare che gli aggiornamenti vengano applicati al di fuori di una finestra di manutenzione in Ubuntu, riconfigurare il pacchetto Unattended-Upgrade per disabilitare gli aggiornamenti automatici. Per informazioni sulla configurazione del pacchetto, vedere l'[argomento Aggiornamenti automatici nella Guida a Ubuntu Server](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

Le macchine virtuali create dalle immagini di Red Hat Enterprise Linux (RHEL) su richiesta e disponibili in Azure Marketplace vengono registrate per accedere al servizio [Red Hat Update Infrastructure (RHUI)](../virtual-machines/virtual-machines-linux-update-infrastructure-redhat.md) distribuito in Azure. Altre distribuzioni di Linux devono essere aggiornate dal repository di file online della distribuzione seguendo i metodi supportati della distribuzione.

Per creare una nuova distribuzione di aggiornamenti, selezionare **Pianifica la distribuzione di aggiornamenti**. Verrà visualizzata la pagina **nuova distribuzione aggiornamenti** . Specificare i valori per le proprietà descritte nella tabella seguente e quindi fare clic su **Crea**:

| Proprietà | DESCRIZIONE |
| --- | --- |
| NOME |Nome univoco che identifica la distribuzione degli aggiornamenti. |
|Sistema operativo| Linux o Windows|
| Gruppi da aggiornare |Per i computer di Azure, definire una query basata su una combinazione di sottoscrizione, gruppi di risorse, posizioni e tag per creare un gruppo dinamico di macchine virtuali di Azure da includere nella distribuzione. </br></br>Per i computer non di Azure, selezionare una ricerca esistente salvata per selezionare un gruppo di computer non di Azure da includere nella distribuzione. </br></br>Per altre informazioni, vedere [Gruppi dinamici](automation-update-management.md#using-dynamic-groups)|
| Computer da aggiornare |Selezionare una ricerca salvata o un gruppo importato, oppure scegliere Computer dall'elenco a discesa e selezionare i singoli computer. Se si sceglie**Computer**, l'idoneità del computer è indicata nella colonna **AGGIORNA IDONEITÀ AGENTE**.</br> Per altre informazioni sui diversi metodi di creazione di gruppi di computer nei log di Monitoraggio di Azure, vedere [Gruppi di computer nei log di Monitoraggio di Azure](../azure-monitor/platform/computer-groups.md) |
|Classificazioni degli aggiornamenti|Selezionare tutte le classificazioni degli aggiornamenti necessarie|
|Includi/Escludi aggiornamenti|Apre la pagina **Includi/Escludi**. Gli aggiornamenti da includere o escludere si trovano in schede separate. Per altre informazioni sulla modalità di gestione dell'inclusione, vedere il [comportamento dell'inclusione](automation-update-management.md#inclusion-behavior) |
|Impostazioni di pianificazione|Selezionare l'ora di inizio e selezionare Una sola volta o Ricorrente per la ricorrenza|
| Pre-script e post-script|Selezionare gli script da eseguire prima e dopo la distribuzione|
| Finestra di manutenzione |Numero di minuti impostato per gli aggiornamenti. Il valore non può essere inferiore a 30 minuti e superiore a 6 ore |
| Controllo riavvio| Determina come vengono gestiti i riavvii. Le opzioni disponibili sono:</br>Riavvia se necessario (opzione predefinita)</br>Riavvia sempre</br>Non riavviare mai</br>Riavvia solamente: gli aggiornamenti non verranno installati|

Le distribuzioni di aggiornamenti possono essere create anche a livello di codice. Per informazioni su come creare una distribuzione di aggiornamenti con l'API REST, vedere [Software Update Configurations - Create](/rest/api/automation/softwareupdateconfigurations/create) (Configurazioni degli aggiornamenti software - Creazione). È anche disponibile un runbook di esempio che può essere usato per creare una distribuzione di aggiornamenti settimanale. Per altre informazioni su questo runbook, vedere [Create a weekly update deployment for one or more VMs in a resource group](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1) (Creare una distribuzione di aggiornamenti settimanale per una o più macchine virtuali in un gruppo di risorse).

> [!NOTE]
> Le chiavi del registro di sistema elencate in [chiavi del registro di sistema usate per gestire il riavvio](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) possono causare un evento di riavvio se il **controllo riavvio** è impostato su **non riavviare mai**.

### <a name="maintenance-windows"></a>Finestre di manutenzione

Le finestre di manutenzione controllano la quantità di tempo consentita per l'installazione degli aggiornamenti. Quando si specifica una finestra di manutenzione, tenere presenti i dettagli seguenti.

* Finestre di manutenzione consente di controllare il numero di aggiornamenti che si tenta di installare.
* Gestione aggiornamenti non interrompe l'installazione di nuovi aggiornamenti se si avvicina la fine di una finestra di manutenzione.
* Gestione aggiornamenti non termina gli aggiornamenti in corso se quando viene superata la finestra di manutenzione.
* Se la finestra di manutenzione viene superata in Windows, spesso è dovuto a un Service Pack aggiornamento che richiede molto tempo per l'installazione.

### <a name="multi-tenant"></a>Distribuzioni di aggiornamenti tra tenant

Se si hanno computer in un altro tenant di Azure che segnala a Gestione aggiornamenti che necessitano di una patch, occorrerà usare la seguente soluzione alternativa per la pianificazione. È possibile usare il cmdlet [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) con l'opzione `-ForUpdate` per creare una pianificazione, usare il cmdlet [New-AzureRmAutomationSoftwareUpdateConfiguration](/powershell/module/azurerm.automation/new-azurermautomationsoftwareupdateconfiguration
) e passare i computer nell'altro tenant al parametro `-NonAzureComputer`. Segue un esempio di come effettuare questa operazione:

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$sched = New-AzureRmAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdate

New-AzureRmAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName <automationAccountName> -Schedule $sched -Windows -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

## <a name="view-missing-updates"></a>Visualizzare gli aggiornamenti mancanti

Selezionare **Aggiornamenti mancanti** per visualizzare l'elenco di aggiornamenti mancanti nei computer. Ogni aggiornamento viene inserito nell'elenco e può essere selezionato. Sono disponibili informazioni relative al numero di computer che richiedono l'aggiornamento e al sistema operativo, oltre a un collegamento per accedere ad altre informazioni. Il riquadro **Ricerca log** visualizza altri dettagli sugli aggiornamenti.

## <a name="view-update-deployments"></a>Visualizzare le distribuzioni di aggiornamenti

Selezionare la scheda **Distribuzioni di aggiornamenti** per visualizzare l'elenco delle distribuzioni esistenti. Selezionare una delle distribuzioni di aggiornamenti nella tabella per aprire la pagina **Esecuzione di distribuzione aggiornamenti** per quella specifica distribuzione. I log dei processi vengono archiviati per un massimo di 30 giorni.

![Panoramica dei risultati della distribuzione di aggiornamenti](./media/automation-update-management/update-deployment-run.png)

Per visualizzare una distribuzione di aggiornamenti dall'API REST, vedere [Software Update Configuration Runs](/rest/api/automation/softwareupdateconfigurationruns) (Esecuzioni delle configurazioni degli aggiornamenti software).

## <a name="update-classifications"></a>Classificazioni degli aggiornamenti

Nelle tabelle che seguono sono riportate le classificazioni degli aggiornamenti in Gestione aggiornamenti, con una definizione per ogni classificazione.

### <a name="windows"></a>Windows

|classificazione  |DESCRIZIONE  |
|---------|---------|
|Aggiornamenti critici     | Un aggiornamento per un problema specifico che risolve un bug critico non correlato alla sicurezza.        |
|Aggiornamenti della sicurezza     | Un aggiornamento per un problema specifico del prodotto correlato alla sicurezza.        |
|Aggiornamenti cumulativi     | Un set cumulativo di aggiornamenti rapidi, contenuti nello stesso pacchetto per facilitarne la distribuzione.        |
|Feature Pack     | Nuove funzionalità del prodotto distribuite di fuori di una versione del prodotto.        |
|Service Pack     | Un set cumulativo di aggiornamenti rapidi applicati a un'applicazione.        |
|Aggiornamenti della definizione     | Un aggiornamento per un virus o altri file di definizione.        |
|Strumenti     | Utilità o funzionalità che consente di completare una o più attività.        |
|Aggiornamenti     | Un aggiornamento di un'applicazione o un file attualmente installati.        |

### <a name="linux-2"></a>Linux

|classificazione  |DESCRIZIONE  |
|---------|---------|
|Aggiornamenti critici e della sicurezza     | Aggiornamenti per un problema specifico o specifico del prodotto, correlato alla sicurezza.         |
|Altri aggiornamenti     | Tutti gli altri aggiornamenti non critici per loro natura o che non sono aggiornamenti della sicurezza.        |

Per Linux, Gestione aggiornamenti è in grado di distinguere tra gli aggiornamenti critici e quelli della sicurezza nel cloud, visualizzando i dati di valutazione dovuti all'arricchimento dei dati nel cloud. Per l'applicazione di patch, Gestione aggiornamenti si affida ai dati di classificazione disponibili nel computer. A differenza di altre distribuzioni, CentOS non mette a disposizione queste informazioni per impostazione predefinita. Se i computer CentOS sono configurati in modo da restituire dati sulla sicurezza per il comando seguente, Gestione aggiornamenti sarà in grado di applicare patch in base alle classificazioni.

```bash
sudo yum -q --security check-update
```

Attualmente non è supportato alcun metodo per abilitare i dati di classificazione nativi in CentOS. In questa fase viene offerto il miglior supporto possibile ai clienti che abilitano autonomamente questa funzionalità.

## <a name="firstparty-predownload"></a>Impostazioni avanzate

Gestione aggiornamenti si basa su Windows Update per scaricare e installare gli aggiornamenti di Windows. Di conseguenza, vengono applicate molte delle impostazioni usate da Windows Update. Se si usano le impostazioni per abilitare gli aggiornamenti non Windows, Gestione aggiornamenti gestisce anche questi. Se si abilita il download degli aggiornamenti prima che venga eseguita una distribuzione degli aggiornamenti, le distribuzioni degli aggiornamenti saranno più veloci e avranno meno probabilità di superare la finestra di manutenzione.

### <a name="pre-download-updates"></a>Eseguire il pre-download degli aggiornamenti

Per configurare automaticamente il download degli aggiornamenti in Criteri di gruppo, impostare [Configura Aggiornamenti automatici](/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates##configure-automatic-updates) su **3**. In questo modo gli aggiornamenti necessari vengono scaricati in background senza essere installati. Gestione aggiornamenti mantiene il controllo delle pianificazioni, ma consente di scaricare gli aggiornamenti al di fuori della finestra di manutenzione. Ciò contribuisce a evitare errori di **superamento della finestra di manutenzione** di Gestione aggiornamenti.

Questa configurazione è possibile anche con PowerShell. A tal fine, eseguire il comando PowerShell seguente nel sistema in cui si intende scaricare automaticamente gli aggiornamenti.

```powershell
$WUSettings = (New-Object -com "Microsoft.Update.AutoUpdate").Settings
$WUSettings.NotificationLevel = 3
$WUSettings.Save()
```

### <a name="disable-automatic-installation"></a>Disabilita installazione automatica

Per le macchine virtuali di Azure l'installazione automatica degli aggiornamenti è abilitata per impostazione predefinita. Questa operazione può causare l'installazione degli aggiornamenti prima di pianificarne l'installazione da Gestione aggiornamenti. È possibile disabilitare questo comportamento impostando la `NoAutoUpdate` chiave del registro `1`di sistema su. Il seguente frammento di codice di PowerShell Mostra un modo per eseguire questa operazione.

```powershell
$AutoUpdatePath = "HKLM:SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU"
Set-ItemProperty -Path $AutoUpdatePath -Name NoAutoUpdate -Value 1
```

### <a name="enable-updates-for-other-microsoft-products"></a>Abilitare gli aggiornamenti per altri prodotti Microsoft

Per impostazione predefinita, Windows Update fornisce soltanto gli aggiornamenti per Windows. Se si abilitano **gli aggiornamenti per altri prodotti Microsoft durante l'aggiornamento di Windows**, vengono forniti aggiornamenti per altri prodotti, incluse le patch di sicurezza per SQL Server o altro software di terze parti. Non è possibile configurare questa opzione tramite Criteri di gruppo. Eseguire il comando PowerShell seguente nei sistemi sui quali si intende abilitare gli aggiornamenti proprietari per applicare l'impostazione a Gestione aggiornamenti.

```powershell
$ServiceManager = (New-Object -com "Microsoft.Update.ServiceManager")
$ServiceManager.Services
$ServiceID = "7971f918-a847-4430-9279-4a52d1efe18d"
$ServiceManager.AddService2($ServiceId,7,"")
```

## <a name="third-party"></a> Patch di terze parti in Windows

Gestione aggiornamenti si basa sul repository di aggiornamenti configurato localmente per applicare patch ai sistemi Windows supportati. Si tratta di WSUS o Windows Update. Strumenti come [System Center Updates Publisher](/sccm/sum/tools/updates-publisher
) (Updates Publisher) consentono di pubblicare gli aggiornamenti personalizzati in Windows Server Update Services. Questo scenario consente Gestione aggiornamenti di applicare patch ai computer che usano System Center Configuration Manager come repository di aggiornamento con software di terze parti. Per informazioni su come configurare Updates Publisher, vedere [Installare Updates Publisher](/sccm/sum/tools/install-updates-publisher).

## <a name="ports"></a>Pianificazione della rete

I seguenti indirizzi sono necessari e specifici per Gestione aggiornamenti. La comunicazione verso questi indirizzi avviene sulla porta 443.

|Azure Public  |Azure Government  |
|---------|---------|
|*.ods.opinsights.azure.com     |*.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net|*.blob.core.usgovcloudapi.net|
|*.azure-automation.net|*.azure-automation.us|

Per i computer Windows, è necessario consentire il traffico anche a tutti gli endpoint necessari per Windows Update.  È possibile trovare un elenco aggiornato degli endpoint necessari nei [problemi relativi a http/proxy](/windows/deployment/update/windows-update-troubleshooting#issues-related-to-httpproxy). Se si dispone di un [server di Windows Update](/windows-server/administration/windows-server-update-services/plan/plan-your-wsus-deployment)locale, è necessario consentire anche il traffico al server specificato nella [chiave WSUS](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry).

Per i computer Red Hat Linux, vedere [gli indirizzi IP per i server per la distribuzione di contenuti di RHUI per gli](../virtual-machines/linux/update-infrastructure-redhat.md#the-ips-for-the-rhui-content-delivery-servers) endpoint richiesti. Per altre distribuzioni Linux, vedere la documentazione del provider.

Per altre informazioni sulle porte richieste dal ruolo di lavoro ibrido per runbook, vedere [Porte del ruolo di lavoro ibrido](automation-hybrid-runbook-worker.md#hybrid-worker-role).

È consigliabile usare gli indirizzi elencati quando si definiscono eccezioni. Per gli indirizzi IP è possibile scaricare gli [intervalli di indirizzi IP dei data center di Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653). Questo file viene aggiornato ogni settimana con gli intervalli attualmente distribuiti e le eventuali modifiche imminenti agli intervalli IP.

Seguire le istruzioni riportate in [connettere i computer senza accesso a Internet](../azure-monitor/platform/gateway.md) per configurare i computer che non hanno accesso a Internet.

## <a name="search-logs"></a>Eseguire ricerche nei log

Oltre ai dettagli presenti sul portale di Azure, è possibile eseguire ricerche nei log. Selezionare **Log Analytics** nelle pagine delle soluzioni. Viene aperto il riquadro **Ricerca log**.

Per sapere come personalizzare le query o usarle da client diversi e altro ancora, visitare:  [Documentazione dell'API di ricerca di Log Analytics](
https://dev.loganalytics.io/).

### <a name="sample-queries"></a>Query di esempio

Le sezioni seguenti contengono esempi di query di ricerca per l'aggiornamento dei record raccolti da questa soluzione:

#### <a name="single-azure-vm-assessment-queries-windows"></a>Singole query di valutazione delle macchine virtuali di Azure (Windows)

Sostituire il valore VMUUID con il GUID VM della macchina virtuale per cui si esegue la query. È possibile trovare il VMUUID che deve essere usato eseguendo la query seguente nei log di monitoraggio di Azure:`Update | where Computer == "<machine name>" | summarize by Computer, VMUUID`

##### <a name="missing-updates-summary"></a>Riepilogo degli aggiornamenti mancanti

```loganalytics
Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and VMUUID=~"b08d5afa-1471-4b52-bd95-a44fea6e4ca8"
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| summarize by UpdateID, Classification
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security")
```

##### <a name="missing-updates-list"></a>Elenco degli aggiornamenti mancanti

```loganalytics
Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and VMUUID=~"8bf1ccc6-b6d3-4a0b-a643-23f346dfdf82"
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, KBID, PublishedDate, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| project-away UpdateState, Approved, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), displayName=any(Title), publishedDate=min(PublishedDate), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(UpdateID, "_", KBID), classification=Classification, InformationId=strcat("KB", KBID), InformationUrl=iff(isnotempty(KBID), strcat("https://support.microsoft.com/kb/", KBID), ""), osType=2
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl
```

#### <a name="single-azure-vm-assessment-queries-linux"></a>Singole query di valutazione delle macchine virtuali di Azure (Linux)

Per alcune distribuzioni di Linux, si verifica una [mancata corrispondenza](https://en.wikipedia.org/wiki/Endianness) tra l'elemento e il valore di VMUUID che deriva da Azure Resource Manager e ciò che viene archiviato nei log di monitoraggio di Azure. La query seguente verifica la presenza di una corrispondenza in uno degli ordini di byte. Sostituire i valori VMUUID con il formato big-endian e little-endian del GUID in modo che i risultati vengano restituiti correttamente. È possibile trovare il VMUUID che deve essere usato eseguendo la query seguente nei log di monitoraggio di Azure:`Update | where Computer == "<machine name>"
| summarize by Computer, VMUUID`

##### <a name="missing-updates-summary"></a>Riepilogo degli aggiornamenti mancanti

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and (VMUUID=~"625686a0-6d08-4810-aae9-a089e68d4911" or VMUUID=~"a0865662-086d-1048-aae9-a089e68d4911")
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| summarize by Product, ProductArch, Classification
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security")
```

##### <a name="missing-updates-list"></a>Elenco degli aggiornamenti mancanti

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and (VMUUID=~"625686a0-6d08-4810-aae9-a089e68d4911" or VMUUID=~"a0865662-086d-1048-aae9-a089e68d4911")
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, BulletinUrl, BulletinID) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| project-away UpdateState, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(Product, "_", ProductArch), displayName=Product, productArch=ProductArch, classification=Classification, InformationId=BulletinID, InformationUrl=tostring(split(BulletinUrl, ";", 0)[0]), osType=1
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl

```

#### <a name="multi-vm-assessment-queries"></a>Query di valutazione per più macchine virtuali

##### <a name="computers-summary"></a>Riepilogo dei computer

```loganalytics
Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(14h) and OSType!="Linux"
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Approved, Optional, Classification) by SourceComputerId, UpdateID
    | distinct SourceComputerId, Classification, UpdateState, Approved, Optional
    | summarize WorstMissingUpdateSeverity=max(iff(UpdateState=~"Needed" and (Optional==false or Classification has "Critical" or Classification has "Security") and Approved!=false, iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1)), 0)) by SourceComputerId
)
on SourceComputerId
| extend WorstMissingUpdateSeverity=coalesce(WorstMissingUpdateSeverity, -1)
| summarize computersBySeverity=count() by WorstMissingUpdateSeverity
| union (Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(5h) and OSType=="Linux"
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by SourceComputerId, Product, ProductArch
    | distinct SourceComputerId, Classification, UpdateState
    | summarize WorstMissingUpdateSeverity=max(iff(UpdateState=~"Needed", iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1)), 0)) by SourceComputerId
)
on SourceComputerId
| extend WorstMissingUpdateSeverity=coalesce(WorstMissingUpdateSeverity, -1)
| summarize computersBySeverity=count() by WorstMissingUpdateSeverity)
| summarize assessedComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity>-1), notAssessedComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==-1), computersNeedCriticalUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==4), computersNeedSecurityUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==2), computersNeedOtherUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==1), upToDateComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==0)
| summarize assessedComputersCount=sum(assessedComputersCount), computersNeedCriticalUpdatesCount=sum(computersNeedCriticalUpdatesCount),  computersNeedSecurityUpdatesCount=sum(computersNeedSecurityUpdatesCount), computersNeedOtherUpdatesCount=sum(computersNeedOtherUpdatesCount), upToDateComputersCount=sum(upToDateComputersCount), notAssessedComputersCount=sum(notAssessedComputersCount)
| extend allComputersCount=assessedComputersCount+notAssessedComputersCount


```

##### <a name="missing-updates-summary"></a>Riepilogo degli aggiornamenti mancanti

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| summarize by Product, ProductArch, Classification
| union (Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| summarize by UpdateID, Classification )
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security")
```

##### <a name="computers-list"></a>Elenco dei computer

```loganalytics
Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions, Computer, ResourceId, ComputerEnvironment, VMUUID) by SourceComputerId
| where Solutions has "updates"
| extend vmuuId=VMUUID, azureResourceId=ResourceId, osType=1, environment=iff(ComputerEnvironment=~"Azure", 1, 2), scopedToUpdatesSolution=true, lastUpdateAgentSeenTime=""
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
    | where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
    | summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
    | where Solutions has "updates"
    | distinct SourceComputerId))
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Product, Computer, ComputerEnvironment) by SourceComputerId, Product, ProductArch
    | summarize Computer=any(Computer), ComputerEnvironment=any(ComputerEnvironment), missingCriticalUpdatesCount=countif(Classification has "Critical" and UpdateState=~"Needed"), missingSecurityUpdatesCount=countif(Classification has "Security" and UpdateState=~"Needed"), missingOtherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security" and UpdateState=~"Needed"), lastAssessedTime=max(TimeGenerated), lastUpdateAgentSeenTime="" by SourceComputerId
    | extend compliance=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0, 2, 1)
    | extend ComplianceOrder=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0 or missingOtherUpdatesCount > 0, 1, 3)
)
on SourceComputerId
| project id=SourceComputerId, displayName=Computer, sourceComputerId=SourceComputerId, scopedToUpdatesSolution=true, missingCriticalUpdatesCount=coalesce(missingCriticalUpdatesCount, -1), missingSecurityUpdatesCount=coalesce(missingSecurityUpdatesCount, -1), missingOtherUpdatesCount=coalesce(missingOtherUpdatesCount, -1), compliance=coalesce(compliance, 4), lastAssessedTime, lastUpdateAgentSeenTime, osType=1, environment=iff(ComputerEnvironment=~"Azure", 1, 2), ComplianceOrder=coalesce(ComplianceOrder, 2)
| union(Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions, Computer, ResourceId, ComputerEnvironment, VMUUID) by SourceComputerId
| where Solutions has "updates"
| extend vmuuId=VMUUID, azureResourceId=ResourceId, osType=2, environment=iff(ComputerEnvironment=~"Azure", 1, 2), scopedToUpdatesSolution=true, lastUpdateAgentSeenTime=""
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(14h) and OSType!="Linux" and SourceComputerId in ((Heartbeat
    | where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
    | summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
    | where Solutions has "updates"
    | distinct SourceComputerId))
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, Optional, Approved, Computer, ComputerEnvironment) by Computer, SourceComputerId, UpdateID
    | summarize Computer=any(Computer), ComputerEnvironment=any(ComputerEnvironment), missingCriticalUpdatesCount=countif(Classification has "Critical" and UpdateState=~"Needed" and Approved!=false), missingSecurityUpdatesCount=countif(Classification has "Security" and UpdateState=~"Needed" and Approved!=false), missingOtherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security" and UpdateState=~"Needed" and Optional==false and Approved!=false), lastAssessedTime=max(TimeGenerated), lastUpdateAgentSeenTime="" by SourceComputerId
    | extend compliance=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0, 2, 1)
    | extend ComplianceOrder=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0 or missingOtherUpdatesCount > 0, 1, 3)
)
on SourceComputerId
| project id=SourceComputerId, displayName=Computer, sourceComputerId=SourceComputerId, scopedToUpdatesSolution=true, missingCriticalUpdatesCount=coalesce(missingCriticalUpdatesCount, -1), missingSecurityUpdatesCount=coalesce(missingSecurityUpdatesCount, -1), missingOtherUpdatesCount=coalesce(missingOtherUpdatesCount, -1), compliance=coalesce(compliance, 4), lastAssessedTime, lastUpdateAgentSeenTime, osType=2, environment=iff(ComputerEnvironment=~"Azure", 1, 2), ComplianceOrder=coalesce(ComplianceOrder, 2) )
| order by ComplianceOrder asc, missingCriticalUpdatesCount desc, missingSecurityUpdatesCount desc, missingOtherUpdatesCount desc, displayName asc
| project-away ComplianceOrder
```

##### <a name="missing-updates-list"></a>Elenco degli aggiornamenti mancanti

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, BulletinUrl, BulletinID) by SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| project-away UpdateState, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(Product, "_", ProductArch), displayName=Product, productArch=ProductArch, classification=Classification, InformationId=BulletinID, InformationUrl=tostring(split(BulletinUrl, ";", 0)[0]), osType=1
| union(Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, KBID, PublishedDate, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| project-away UpdateState, Approved, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), displayName=any(Title), publishedDate=min(PublishedDate), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(UpdateID, "_", KBID), classification=Classification, InformationId=strcat("KB", KBID), InformationUrl=iff(isnotempty(KBID), strcat("https://support.microsoft.com/kb/", KBID), ""), osType=2)
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl
```

## <a name="using-dynamic-groups"></a>Uso di gruppi dinamici

Gestione aggiornamenti offre la possibilità di specificare come destinazione un gruppo dinamico di macchine virtuali di Azure o non Azure per le distribuzioni di aggiornamenti. Questi gruppi vengono valutati in fase di distribuzione, pertanto non è necessario modificare la distribuzione per aggiungere computer.

> [!NOTE]
> Quando si crea una distribuzione degli aggiornamenti, è necessario disporre delle autorizzazioni appropriate. Per altre informazioni, vedere [installare gli aggiornamenti](#install-updates).

### <a name="azure-machines"></a>Macchine virtuali di Azure

Questi gruppi vengono definiti da una query. Quando inizia una distribuzione di aggiornamenti, i membri di tale gruppo vengono valutati. I gruppi dinamici non funzionano con le VM classiche. Quando si definisce la query, gli elementi seguenti possono essere usati per popolare il gruppo dinamico

* Sottoscrizione
* Gruppi di risorse
* Località
* Tag

![Selezionare i gruppi](./media/automation-update-management/select-groups.png)

Per visualizzare in anteprima i risultati di un gruppo dinamico, fare clic sul pulsante **Anteprima**. Questa anteprima mostra l'appartenenza al gruppo in quel momento. In questo esempio si esegue una ricerca dei computer con il tag **Ruolo** uguale a **BackendServer**. Se questo tag è stato aggiunto a più computer, tali computer verranno aggiunti alle distribuzioni future in quel gruppo.

![Visualizzare in anteprima i gruppi](./media/automation-update-management/preview-groups.png)

### <a name="non-azure-machines"></a>Computer non Azure

Per i computer non Azure, le ricerche salvate definite anche come gruppi di computer vengono usate per creare il gruppo dinamico. Per informazioni su come creare una ricerca salvata, vedere [creazione di un gruppo di computer](../azure-monitor/platform/computer-groups.md#creating-a-computer-group). Una volta creato il gruppo, è possibile selezionarlo dall'elenco di ricerche salvate. Fare clic su **Anteprima** per visualizzare in anteprima i computer nella ricerca salvata in quel momento.

![Selezionare i gruppi](./media/automation-update-management/select-groups-2.png)

## <a name="integrate-with-system-center-configuration-manager"></a>Integrazione con System Center Configuration Manager

I clienti che hanno investito in System Center Configuration Manager per gestire PC, server e dispositivi mobili si affidano alle caratteristiche potenti e avanzate di questa soluzione anche per gestire gli aggiornamenti software. Configuration Manager fa parte del ciclo di gestione degli aggiornamenti software (SUM).

Per informazioni su come integrare la soluzione di gestione con System Center Configuration Manager, vedere [Integrare System Center Configuration Manager con Gestione aggiornamenti](oms-solution-updatemgmt-sccmintegration.md).

## <a name="inclusion-behavior"></a>Comportamento di inclusione

L'inclusione di aggiornamenti consente di specificare gli aggiornamenti da applicare. Vengono installati le patch o i pacchetti inclusi. Quando vengono incluse patch o pacchetti e viene anche selezionata una classificazione, vengono installati sia gli elementi inclusi che gli elementi che soddisfano la classificazione.

Tenere presente che le esclusioni eseguono l'override delle inclusioni. Se ad esempio si definisce una regola di esclusione `*`, non vengono installate patch o pacchetti perché vengono tutti esclusi. Le patch escluse vengono ancora visualizzate come mancanti dal computer. Per i computer Linux, se un pacchetto è incluso, ma ha un pacchetto dipendente che è stato escluso, il pacchetto non viene installato.

## <a name="patch-linux-machines"></a>Applicazione di patch ai computer Linux

Le sezioni seguenti illustrano i potenziali problemi relativi all'applicazione di patch a computer Linux.

### <a name="unexpected-os-level-upgrades"></a>Aggiornamenti imprevisti a livello di sistema operativo

In alcune varianti di Linux, ad esempio Red Hat Enterprise Linux, è possibile che gli aggiornamenti a livello di sistema operativo vengano eseguiti con i pacchetti. Questo può causare esecuzioni di Gestione aggiornamenti in cui viene modificato il numero di versione del sistema operativo. Poiché Gestione aggiornamenti usa gli stessi metodi di aggiornamento dei pacchetti che userebbe un amministratore in locale nel computer Linux, questo comportamento è intenzionale.

Per evitare l'aggiornamento della versione del sistema operativo tramite esecuzioni di Gestione aggiornamenti, usare la funzionalità **Esclusione**.

In Red Hat Enterprise Linux il nome del pacchetto da escludere è redhat-release-server.x86_64.

![Pacchetti da escludere per Linux](./media/automation-update-management/linuxpatches.png)

### <a name="critical--security-patches-arent-applied"></a>Le patch critiche o di sicurezza non vengono applicate

Durante la distribuzione degli aggiornamenti in un computer Linux è possibile selezionare le classificazioni degli aggiornamenti. In questo modo è possibile filtrare gli aggiornamenti applicati al computer che soddisfano i criteri specificati. Questo filtro viene applicato in locale nel computer in cui viene distribuito l'aggiornamento.

Poiché Gestione aggiornamenti esegue l'arricchimento degli aggiornamenti nel cloud, è possibile contrassegnare alcuni aggiornamenti in Gestione aggiornamenti con un effetto di sicurezza, anche se il computer locale non dispone di tali informazioni. Di conseguenza, se si applicano aggiornamenti critici in un computer Linux, è possibile che alcuni aggiornamenti non siano contrassegnati come elementi che influiscono sulla protezione del computer e gli aggiornamenti non vengono applicati.

Tuttavia, Gestione aggiornamenti potrebbe continuare a segnalare tale computer come non conforme dal momento che contiene informazioni aggiuntive sull'aggiornamento pertinente.

La distribuzione degli aggiornamenti in base alla classificazione di aggiornamento non funziona in CentOS per impostazione predefinita. Per distribuire correttamente gli aggiornamenti per CentOS, selezionare tutte le classificazioni per assicurarsi che gli aggiornamenti vengano applicati. Per SUSE, se si seleziona *solo* "Altri aggiornamenti" come classificazione, è possibile che vengano installati anche alcuni aggiornamenti della sicurezza se per prima cosa sono richiesti aggiornamenti della sicurezza correlati a zypper (gestione pacchetti) o alle relative dipendenze. Si tratta di una limitazione di zypper. In alcuni casi può essere necessario eseguire di nuovo la distribuzione degli aggiornamenti. Per verificare, controllare il log di aggiornamento.

## <a name="remove-a-vm-from-update-management"></a>Rimuovere una VM da Gestione aggiornamenti

Per rimuovere una macchina virtuale per Gestione aggiornamenti:

* Nell'area di lavoro Log Analytics rimuovere la macchina virtuale dalla ricerca salvata per la configurazione dell'ambito `MicrosoftDefaultScopeConfig-Updates`. Le ricerche salvate sono disponibili in **Generale** nell'area di lavoro.
* Rimuovere [Microsoft Monitoring Agent](../azure-monitor/learn/quick-collect-windows-computer.md#clean-up-resources) o l'[agente di Log Analytics per Linux](../azure-monitor/learn/quick-collect-linux-computer.md#clean-up-resources).

## <a name="next-steps"></a>Passaggi successivi

Continuare con l'esercitazione sulla gestione degli aggiornamenti per le macchine virtuali Windows.

> [!div class="nextstepaction"]
> [Gestire gli aggiornamenti e le patch per le macchine virtuali Windows di Azure](automation-tutorial-update-management.md)

* Usare le ricerche log nei [log di monitoraggio di Azure](../log-analytics/log-analytics-log-searches.md) per visualizzare i dati dettagliati sugli aggiornamenti.
* [Creare avvisi](automation-tutorial-update-management.md#configure-alerts) per lo stato di distribuzione degli aggiornamenti.

* Per informazioni su come interagire con Gestione aggiornamenti tramite l'API REST, vedere [Configurazioni degli aggiornamenti software](/rest/api/automation/softwareupdateconfigurations)
* Sono disponibili informazioni sulla [risoluzione dei problemi di Gestione aggiornamenti](troubleshoot/update-management.md)
