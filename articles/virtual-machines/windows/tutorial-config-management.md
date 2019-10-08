---
title: Esercitazione - Gestire la configurazione delle macchine virtuali Windows in Azure | Microsoft Docs
description: Questa esercitazione illustra come identificare le modifiche e gestire gli aggiornamenti dei pacchetti in una macchina virtuale Windows
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/05/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 013938f37258b5aa8c4e9751bdc8cf1e7b826ef1
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/30/2019
ms.locfileid: "71678382"
---
# <a name="tutorial-monitor-changes-and-update-a-windows-virtual-machine-in-azure"></a>Esercitazione: Monitorare le modifiche e aggiornare una macchina virtuale Windows in Azure

[Rilevamento modifiche](../../automation/change-tracking.md) di Azure consente di identificare facilmente le modifiche, mentre [Gestione aggiornamenti](../../automation/automation-update-management.md) permette di gestire gli aggiornamenti del sistema operativo per le macchine virtuali Windows di Azure.

In questa esercitazione si apprenderà come:

> [!div class="checklist"]
> * Gestire gli aggiornamenti di Windows
> * Monitorare le modifiche e l'inventario

## <a name="launch-azure-cloud-shell"></a>Avviare Azure Cloud Shell

Azure Cloud Shell è una shell interattiva gratuita che può essere usata per eseguire la procedura di questo articolo. Include strumenti comuni di Azure preinstallati e configurati per l'uso con l'account. 

Per aprire Cloud Shell, basta selezionare **Prova** nell'angolo superiore destro di un blocco di codice. È anche possibile avviare Cloud Shell in una scheda separata del browser visitando [https://shell.azure.com/powershell](https://shell.azure.com/powershell). Selezionare **Copia** per copiare i blocchi di codice, incollarli in Cloud Shell e premere INVIO per eseguirli.

## <a name="create-virtual-machine"></a>Crea macchina virtuale

Per configurare il monitoraggio di Azure e la gestione degli aggiornamenti in questa esercitazione, è necessario disporre di una macchina virtuale Windows in Azure. Impostare prima di tutto nome utente e password dell'amministratore della macchina virtuale con il comando [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

Creare ora la VM con [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm). L'esempio seguente crea una macchina virtuale denominata *myVM* nell'area *EastUS*. Se non esistono già, vengono creati il gruppo di risorse *myResourceGroupMonitorMonitor* e le rispettive risorse di rete di supporto:

```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroupMonitor" `
    -Name "myVM" `
    -Location "East US" `
    -Credential $cred
```

Per creare le risorse e la macchina virtuale sono necessari alcuni minuti.

## <a name="manage-windows-updates"></a>Gestire gli aggiornamenti di Windows

Gestione aggiornamenti consente di gestire gli aggiornamenti e le patch per le macchine virtuali Windows di Azure. Direttamente dalla macchina virtuale è possibile valutare in modo rapido lo stato degli aggiornamenti disponibili, pianificare l'installazione degli aggiornamenti richiesti ed esaminare i risultati della distribuzione per verificare che gli aggiornamenti siano stati applicati correttamente alla macchina virtuale.

Per informazioni sui prezzi, vedere [Prezzi di Automazione per Gestione aggiornamenti](https://azure.microsoft.com/pricing/details/automation/).

### <a name="enable-update-management"></a>Abilitare Gestione aggiornamenti

Abilitare Gestione aggiornamenti per la macchina virtuale:

1. Sul lato sinistro della schermata selezionare **Macchine virtuali**.
2. Selezionare una macchina virtuale dall'elenco.
3. Nella schermata della macchina virtuale nella sezione **Operazioni** fare clic su **Gestione aggiornamenti**. Viene visualizzata la schermata **Abilita Gestione aggiornamenti**.

Viene eseguita una convalida per determinare se Gestione aggiornamenti è abilitata per la macchina virtuale.
La convalida include controlli per un'area di lavoro Log Analytics e un account di Automazione collegato e verifica se la soluzione è presente nell'area di lavoro.

L'area di lavoro di [Log Analytics](../../log-analytics/log-analytics-overview.md) consente di raccogliere i dati generati da funzionalità e servizi, ad esempio Gestione aggiornamenti.
L'area di lavoro offre un'unica posizione per esaminare e analizzare i dati di più origini.
Per eseguire altre azioni nelle macchine virtuali che richiedono gli aggiornamenti, Automazione di Azure consente di eseguire runbook nelle macchine virtuali, ad esempio il download e l'applicazione degli aggiornamenti.

Il processo di convalida controlla anche se nella macchina virtuale è presente Microsoft Monitoring Agent (MMA) e un ruolo di lavoro ibrido per runbook di Automazione.
L'agente consente di comunicare con la macchina virtuale e ottenere informazioni sullo stato dell'aggiornamento.

Scegliere l'area di lavoro di Log Analytics e l'account di automazione da usare e fare clic su **Abilita** per abilitare la soluzione. Per l'abilitazione della soluzione sono necessari fino a 15 minuti.

Se risultano mancanti durante l'onboarding, i prerequisiti seguenti vengono aggiunti automaticamente:

* Area di lavoro di [Log Analytics](../../log-analytics/log-analytics-overview.md)
* [Automazione](../../automation/automation-offering-get-started.md)
* [Ruolo di lavoro ibrido per runbook](../../automation/automation-hybrid-runbook-worker.md) abilitato nella macchina virtuale

Viene visualizzata la schermata **Gestione aggiornamenti**. Configurare la località, l'area di lavoro di Log Analytics e l'account di automazione da usare e fare clic su **Abilita**. Se i campi sono inattivi, significa che un'altra soluzione di automazione è abilitata per la VM e devono essere usati la stessa area di lavoro e lo stesso account di Automazione.

![Abilitare la soluzione Gestione aggiornamenti](./media/tutorial-monitoring/manageupdates-update-enable.png)

L'abilitazione della soluzione può richiedere fino a 15 minuti. Durante questo intervallo di tempo, non chiudere la finestra del browser. Dopo l'abilitazione della soluzione, le informazioni sugli aggiornamenti mancanti nella macchina virtuale passano ai log di Monitoraggio di Azure. Affinché i dati diventino disponibili per l'analisi, sarà necessario attendere da 30 minuti a 6 ore.

### <a name="view-update-assessment"></a>Visualizzare la valutazione degli aggiornamenti

Dopo aver abilitato **Gestione aggiornamenti**, viene visualizzata la schermata **Gestione aggiornamenti**. Dopo la valutazione degli aggiornamenti, viene visualizzato un elenco di aggiornamenti mancanti nella scheda **Aggiornamenti mancanti**.

 ![Visualizzare lo stato degli aggiornamenti](./media/tutorial-monitoring/manageupdates-view-status-win.png)

### <a name="schedule-an-update-deployment"></a>Pianificare la distribuzione degli aggiornamenti

Per installare gli aggiornamenti, pianificare una distribuzione che rispetti la pianificazione dei rilasci e l'intervallo di servizio. È possibile scegliere i tipi di aggiornamento da includere nella distribuzione. È possibile ad esempio includere gli aggiornamenti critici o della sicurezza ed escludere gli aggiornamenti cumulativi.

Pianificare una nuova distribuzione di aggiornamenti per la macchina virtuale facendo clic su **Pianifica la distribuzione di aggiornamenti** nella parte superiore della schermata **Gestione aggiornamenti**. Nella schermata **Nuova distribuzione di aggiornamenti** specificare le informazioni seguenti:

Per creare una nuova distribuzione di aggiornamenti, selezionare **Pianifica la distribuzione di aggiornamenti**. Si apre la pagina **Nuova distribuzione di aggiornamenti**. Specificare i valori per le proprietà descritte nella tabella seguente e quindi fare clic su **Crea**:

| Proprietà | DESCRIZIONE |
| --- | --- |
| NOME |Nome univoco che identifica la distribuzione degli aggiornamenti. |
|Sistema operativo| Linux o Windows|
| Gruppi da aggiornare |Per i computer di Azure, definire una query basata su una combinazione di sottoscrizione, gruppi di risorse, posizioni e tag per creare un gruppo dinamico di macchine virtuali di Azure da includere nella distribuzione. </br></br>Per i computer non di Azure, selezionare una ricerca esistente salvata per selezionare un gruppo di computer non di Azure da includere nella distribuzione. </br></br>Per altre informazioni, vedere [Gruppi dinamici](../../automation/automation-update-management.md#using-dynamic-groups)|
| Computer da aggiornare |Selezionare una ricerca salvata o un gruppo importato, oppure scegliere Computer dall'elenco a discesa e selezionare i singoli computer. Se si sceglie**Computer**, l'idoneità del computer è indicata nella colonna **AGGIORNA IDONEITÀ AGENTE**.</br> Per altre informazioni sui diversi metodi di creazione di gruppi di computer nei log di Monitoraggio di Azure, vedere [Gruppi di computer nei log di Monitoraggio di Azure](../../azure-monitor/platform/computer-groups.md) |
|Classificazioni degli aggiornamenti|Selezionare tutte le classificazioni degli aggiornamenti necessarie|
|Includi/Escludi aggiornamenti|Apre la pagina **Includi/Escludi**. Gli aggiornamenti da includere o escludere si trovano in schede separate. Per altre informazioni sulla modalità di gestione dell'inclusione, vedere il [comportamento dell'inclusione](../../automation/automation-update-management.md#inclusion-behavior) |
|Impostazioni di pianificazione|Selezionare l'ora di inizio e selezionare Una sola volta o Ricorrente per la ricorrenza|
| Pre-script e post-script|Selezionare gli script da eseguire prima e dopo la distribuzione|
| Finestra di manutenzione |Numero di minuti impostato per gli aggiornamenti. Il valore non può essere inferiore a 30 minuti e superiore a 6 ore |
| Controllo riavvio| Determina come vengono gestiti i riavvii. Le opzioni disponibili sono:</br>Riavvia se necessario (opzione predefinita)</br>Riavvia sempre</br>Non riavviare mai</br>Riavvia solamente: gli aggiornamenti non verranno installati|

Le distribuzioni di aggiornamenti possono essere create anche a livello di codice. Per informazioni su come creare una distribuzione di aggiornamenti con l'API REST, vedere [Software Update Configurations - Create](/rest/api/automation/softwareupdateconfigurations/create) (Configurazioni degli aggiornamenti software - Creazione). È anche disponibile un runbook di esempio che può essere usato per creare una distribuzione di aggiornamenti settimanale. Per altre informazioni su questo runbook, vedere [Create a weekly update deployment for one or more VMs in a resource group](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1) (Creare una distribuzione di aggiornamenti settimanale per una o più macchine virtuali in un gruppo di risorse).

Dopo avere configurato la pianificazione, fare clic sul pulsante **Crea**. Viene nuovamente visualizzato il dashboard di stato.
Si noti che la tabella **Pianificata** mostra la pianificazione della distribuzione creata.

### <a name="view-results-of-an-update-deployment"></a>Visualizzare i risultati di una distribuzione di aggiornamenti

Dopo avere avviato la distribuzione pianificata, è possibile visualizzare lo stato della distribuzione nella scheda **Distribuzioni di aggiornamenti** nella schermata **Gestione aggiornamenti**.
Se la distribuzione è in corso, viene visualizzato lo stato **In corso**. Quando la distribuzione viene completata correttamente, lo stato diventa **Completato**.
Se si verifica un errore in uno o più aggiornamenti della distribuzione, lo stato sarà **Parzialmente non riuscito**.
Fare clic sulla distribuzione di aggiornamenti completata per visualizzare il dashboard della distribuzione.

![Dashboard di stato di una distribuzione di aggiornamenti specifica](./media/tutorial-monitoring/manageupdates-view-results.png)

Il riquadro **Risultati aggiornamento** include un riepilogo del numero totale di aggiornamenti e dei risultati della distribuzione nella macchina virtuale.
La tabella di destra visualizza una descrizione dettagliata di ogni aggiornamento e i risultati dell'installazione che possono corrispondere a uno dei valori seguenti:

* **Tentativo non eseguito**: l'aggiornamento non è stato installato a causa di tempo disponibile non sufficiente basato sulla durata della finestra di manutenzione specificata.
* **Completato**: l'aggiornamento è stato completato
* **Non riuscito**: l'aggiornamento non è riuscito

Fare clic su **Tutti i log** per visualizzare tutte le voci di log create dalla distribuzione.

Fare clic sul riquadro **Output** per visualizzare il flusso del processo del runbook responsabile della gestione della distribuzione di aggiornamenti nella macchina virtuale di destinazione.

Fare clic su **Errori** per visualizzare informazioni dettagliate sugli errori della distribuzione.

## <a name="monitor-changes-and-inventory"></a>Monitorare le modifiche e l'inventario

È possibile raccogliere e visualizzare l'inventario per il software, i file, i daemon Linux, i servizi di Windows e le chiavi del Registro di sistema di Windows presenti nei computer. Tenendo traccia delle configurazioni dei computer, è possibile individuare i problemi operativi dell'ambiente e conoscere meglio lo stato dei computer.

### <a name="enable-change-and-inventory-management"></a>Abilitare la gestione delle modifiche e dell'inventario

Abilitare la gestione delle modifiche e dell'inventario per la macchina virtuale:

1. Sul lato sinistro della schermata selezionare **Macchine virtuali**.
2. Selezionare una macchina virtuale dall'elenco.
3. Nella sezione **Operazioni** della schermata della macchina virtuale fare clic su **Inventario** o **Rilevamento modifiche**. Verrà visualizzata la schermata **Enable Change Tracking and Inventory** (Abilita rilevamento modifiche e inventario).

Configurare la località, l'area di lavoro di Log Analytics e l'account di automazione da usare e fare clic su **Abilita**. Se i campi sono inattivi, significa che un'altra soluzione di automazione è abilitata per la VM e devono essere usati la stessa area di lavoro e lo stesso account di Automazione. Sebbene le soluzioni siano presentate separatamente sul menu, rappresentano la stessa soluzione. L'abilitazione di una di queste le abilita entrambe per la macchina virtuale.

![Abilitare il rilevamento delle modifiche e dell'inventario](./media/tutorial-monitoring/manage-inventory-enable.png)

Dopo aver abilitato la soluzione, la raccolta dell'inventario sulla macchina virtuale potrebbe richiedere alcuni minuti prima che i dati vengono visualizzati.

### <a name="track-changes"></a>Rilevare le modifiche

Nella macchina virtuale selezionare **Rilevamento modifiche** in **OPERAZIONI**. Fare clic su **Modifica impostazioni**, viene visualizzata la pagina **Rilevamento modifiche**. Selezionare il tipo di impostazione che si desidera rilevare e quindi fare clic su **+ Aggiungi** per configurare le impostazioni. Le opzioni disponibili per Windows sono:

* Registro di sistema di Windows
* File Windows

Per informazioni dettagliate sul rilevamento delle modifiche, vedere [Risolvere i problemi delle modifiche in una macchina virtuale](../../automation/automation-tutorial-troubleshoot-changes.md)

### <a name="view-inventory"></a>Visualizzare l'inventario

Nella macchina virtuale selezionare **Inventario** in **OPERAZIONI**. Nella scheda **Software** è presente una tabella con l'elenco del software trovato. I dettagli generali di ogni record software sono visibili nella tabella. Questi dettagli includono il nome del software, la versione, l'autore, l'ora dell'ultimo aggiornamento.

![Visualizzare l'inventario](./media/tutorial-monitoring/inventory-view-results.png)

### <a name="monitor-activity-logs-and-changes"></a>Monitorare i log attività e le modifiche

Dalla pagina **Rilevamento modifiche** della VM, selezionare **Gestisci connessione al log attività**. Si aprirà la pagina **Log attività di Azure**. Selezionare **Connetti** per connettere Rilevamento modifiche al log attività di Azure per la VM.

Con questa impostazione abilitata, passare alla pagina **Panoramica** della VM e selezionare **Arresta** per arrestare la macchina virtuale. Quando richiesto, selezionare **Sì** per arrestare la VM. Quando la VM è deallocata, selezionare **Avvia** per riavviarla.

Con l'arresto e l'avvio di una macchina virtuale viene registrato un evento nel log attività. Tornare alla pagina **Rilevamento modifiche**. Selezionare la scheda **Eventi** nella parte inferiore della pagina. Dopo poco, gli eventi verranno visualizzati nel grafico e nella tabella. È possibile selezionare ogni evento per visualizzare le relative informazioni dettagliate.

![Visualizzare le modifiche nel log attività](./media/tutorial-monitoring/manage-activitylog-view-results.png)

Il grafico mostra le modifiche nel tempo. Dopo aver aggiunto una connessione al log attività, il grafico a linee nella parte superiore visualizza gli eventi del log attività di Azure. Ogni riga dei grafici a barre rappresenta un tipo di modifica tracciabile diverso. I tipi sono daemon Linux, file, chiavi del Registro di sistema di Windows, software e servizi di Windows. La scheda delle modifiche visualizza i dettagli delle modifiche in ordine decrescente in base alla data della modifica, a partire dalla più recente.

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione sono state configurate e verificate le funzionalità Rilevamento modifiche e Gestione aggiornamenti per la macchina virtuale. Si è appreso come:

> [!div class="checklist"]
> * Creare un gruppo di risorse e una macchina virtuale
> * Gestire gli aggiornamenti di Windows
> * Monitorare le modifiche e l'inventario

Passare all'esercitazione successiva per informazioni sul monitoraggio delle macchine virtuali.

> [!div class="nextstepaction"]
> [Monitorare le macchine virtuali](tutorial-monitor.md)
