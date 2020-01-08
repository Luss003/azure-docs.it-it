---
title: Gestire gli aggiornamenti e le patch per le macchine virtuali di Azure
description: Questo articolo offre una panoramica dell'uso di Gestione aggiornamenti di Automazione di Azure per gestire gli aggiornamenti e le patch per le VM di Azure e non di Azure.
services: automation
ms.subservice: update-management
ms.topic: tutorial
ms.date: 12/03/2019
ms.custom: mvc
ms.openlocfilehash: 0fd25863d26c38608b6f64f22782422b844fdec8
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75420650"
---
# <a name="manage-updates-and-patches-for-your-azure-vms"></a>Gestire gli aggiornamenti e le patch per le macchine virtuali di Azure

È possibile usare la soluzione Gestione aggiornamenti per gestire gli aggiornamenti e le patch per le macchine virtuali. In questa esercitazione viene illustrato come valutare in modo rapido lo stato degli aggiornamenti disponibili, pianificare l'installazione degli aggiornamenti richiesti, esaminare i risultati della distribuzione e creare un avviso per verificare che gli aggiornamenti vengano applicati correttamente.

Per informazioni sui prezzi, vedere [Prezzi di Automazione per la gestione degli aggiornamenti](https://azure.microsoft.com/pricing/details/automation/).

In questa esercitazione verranno illustrate le procedure per:

> [!div class="checklist"]
> * Caricare una VM per Gestione aggiornamenti
> * Visualizzare una valutazione degli aggiornamenti
> * Configurare gli avvisi
> * Pianificare la distribuzione degli aggiornamenti
> * Visualizzare i risultati di una distribuzione

## <a name="prerequisites"></a>Prerequisites

Per completare questa esercitazione, sono necessari:

* Una sottoscrizione di Azure. Se non si ha ancora una sottoscrizione, è possibile [attivare il credito Azure mensile per i sottoscrittori di Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oppure iscriversi per ottenere un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Un [account di Automazione di Azure](automation-offering-get-started.md) per contenere i runbook watcher e azione e l'attività watcher.
* Una [macchina virtuale](../virtual-machines/windows/quick-create-portal.md) da caricare.

## <a name="sign-in-to-azure"></a>Accedere ad Azure

Accedere al portale di Azure all'indirizzo https://portal.azure.com.

## <a name="enable-update-management"></a>Abilitare la gestione degli aggiornamenti

Prima di tutto, abilitare Gestione aggiornamenti nella macchina virtuale per questa esercitazione:

1. Nel menu del [portale di Azure](https://portal.azure.com) selezionare **Macchine virtuali** oppure cercare e selezionare **Macchine virtuali** nella pagina **Home**.
1. Selezionare la macchina virtuale per la quale si vuole abilitare Gestione aggiornamenti.
1. Nella pagina della macchina virtuale, in **OPERAZIONI**, selezionare **Gestione aggiornamenti**. Verrà aperto il riquadro **Abilita Gestione aggiornamenti**.

Viene eseguita una convalida per determinare se Gestione aggiornamenti è abilitato per la macchina virtuale. La convalida include controlli per un'area di lavoro Log Analytics di Azure e un account di Automazione collegato e verifica se la soluzione Gestione aggiornamenti è presente nell'area di lavoro.

L'area di lavoro di [Log Analytics](../azure-monitor/platform/data-platform-logs.md) consente di raccogliere i dati generati da funzionalità e servizi come Gestione aggiornamenti. L'area di lavoro offre un'unica posizione per esaminare e analizzare i dati di più origini.

Il processo di convalida controlla anche se nella macchina virtuale sono presenti l'agente Log Analytics e un ruolo di lavoro ibrido per runbook di Automazione. L'agente consente di comunicare con Automazione di Azure e ottenere informazioni sullo stato dell'aggiornamento. L'agente richiede che la porta 443 sia aperta per comunicare con il servizio Automazione di Azure e scaricare gli aggiornamenti.

Se risultano mancanti durante l'onboarding, i prerequisiti seguenti vengono aggiunti automaticamente:

* Area di lavoro di [Log Analytics](../azure-monitor/platform/data-platform-logs.md)
* [Account di Automazione](./automation-offering-get-started.md)
* [Ruolo di lavoro ibrido per runbook](./automation-hybrid-runbook-worker.md) (abilitato nella macchina virtuale)

In **Gestione aggiornamenti** configurare la posizione, l'area di lavoro Log Analytics e l'account di Automazione da usare. Selezionare quindi **Abilita**. Se queste opzioni non sono disponibili, un'altra soluzione di automazione è abilitata per la macchina virtuale. In tal caso, è necessario usare lo stesso account di Automazione e la stessa area di lavoro.

![Finestra di abilitazione della soluzione Gestione aggiornamenti](./media/automation-tutorial-update-management/manageupdates-update-enable.png)

L'abilitazione della soluzione può richiedere alcuni minuti. Durante questo intervallo di tempo, non chiudere la finestra del browser. Dopo l'abilitazione della soluzione, le informazioni sugli aggiornamenti mancanti nella macchina virtuale passano ai log di Monitoraggio di Azure. Affinché i dati diventino disponibili per l'analisi, sarà necessario attendere da 30 minuti a 6 ore.

## <a name="view-update-assessment"></a>Visualizzare la valutazione degli aggiornamenti

Dopo aver abilitato Gestione aggiornamenti, verrà visualizzata la schermata **Gestione aggiornamenti**. Se alcuni aggiornamenti vengono identificati come mancanti, il relativo elenco viene visualizzato nella scheda **Aggiornamenti mancanti**.

In **COLLEGAMENTO ALLE INFORMAZIONI** selezionare il collegamento per aprire l'articolo di supporto per l'aggiornamento. È possibile acquisire informazioni importanti sull'aggiornamento.

![Visualizzare lo stato degli aggiornamenti](./media/automation-tutorial-update-management/manageupdates-view-status-win.png)

Facendo clic in altro punto dell'area dell'aggiornamento, viene aperto il riquadro **Ricerca log** per l'aggiornamento selezionato. La query per la ricerca log è predefinita per tale specifico aggiornamento. È possibile modificare questa query o crearne una nuova per visualizzare informazioni dettagliate sugli aggiornamenti distribuiti o mancanti nell'ambiente.

![Visualizzare lo stato degli aggiornamenti](./media/automation-tutorial-update-management/logsearch.png)

## <a name="configure-alerts"></a>Configurare gli avvisi

In questo passaggio si apprenderà come configurare un avviso per conoscere lo stato della distribuzione di un aggiornamento.

### <a name="alert-conditions"></a>Condizioni di avviso

Nell'account di Automazione, sotto **Monitoraggio**, passare ad **Avvisi** e fare clic su **+ Nuova regola di avviso**.

L'account di Automazione è già selezionato come risorsa. Se si desidera modificarlo, fare clic su **Seleziona** e scegliere dalla pagina **Seleziona una risorsa** gli **account di Automazione** nell'elenco a discesa **Filtra per tipo di risorsa**. Selezionare l'account di automazione e quindi selezionare **Fine**.

Fare clic su **Aggiungi condizione** e selezionare il segnale appropriato per la distribuzione degli aggiornamenti. Nella tabella di seguito vengono illustrati i dettagli dei due segnali disponibili per le distribuzioni degli aggiornamenti:

|Nome segnale|Dimensioni|Descrizione|
|---|---|---|
|**Esecuzioni totali della distribuzione di aggiornamenti**|- Nome della distribuzione di aggiornamenti</br>- Stato|Questo segnale viene usato per inviare avvisi sullo stato generale della distribuzione di un aggiornamento.|
|**Esecuzioni totali della distribuzione di aggiornamenti del computer**|- Nome della distribuzione di aggiornamenti</br>- Stato</br>- Computer di destinazione</br>- ID di esecuzione della distribuzione di aggiornamenti|Questo segnale viene usato per inviare avvisi sullo stato di una distribuzione di aggiornamenti destinata a computer specifici|

Per i valori di dimensione, selezionare un valore valido dall'elenco. Se il valore desiderato non è presente nell'elenco, fare clic sul segno **\+** accanto alla dimensione e digitare il nome personalizzato. È quindi possibile selezionare il valore desiderato. Se si desidera selezionare tutti i valori da una dimensione, fare clic sul pulsante **Seleziona\*** . Se non si sceglie un valore per la dimensione, durante la valutazione questa verrà ignorata.

![Configurare la logica dei segnali](./media/automation-tutorial-update-management/signal-logic.png)

In **Logica avvisi**, per **Soglia**, immettere **1**. Al termine, fare clic su **Fine**.

### <a name="alert-details"></a>Dettagli dell'avviso

In **2. Definire i dettagli dell'avviso** assegnare un nome e una descrizione per l'avviso. Impostare **Gravità** su **Informational(Sev 2)** per un'esecuzione riuscita o su **Informational(Sev 1)** per un'esecuzione non riuscita.

![Configurare la logica dei segnali](./media/automation-tutorial-update-management/define-alert-details.png)

Da **Gruppi di azioni**, selezionare **Crea nuovo**. Un gruppo di azioni è un insieme di azioni che è possibile usare in più avvisi. È ad esempio possibile usare notifiche tramite posta elettronica, runbook, webhook e molto altro ancora. Per altre informazioni sui gruppi di azioni, vedere [Creare e gestire gruppi di azioni](../azure-monitor/platform/action-groups.md).

Nella casella **Nome gruppo di azioni** immettere un nome per l'avviso e un nome breve. Il nome breve viene usato al posto del nome completo di un gruppo di azioni quando le notifiche vengono inviate usando questo gruppo.

In **Azioni** immettere un nome per l'azione, ad esempio **Notifiche tramite posta elettronica**. In **TIPO DI AZIONE** selezionare **Email/SMS/Push/Voice** (Posta elettronica/SMS/Push/Voce). In **DETTAGLI** selezionare **Modifica dettagli**.

Nel riquadro **Email/SMS/Push/Voice** (Posta elettronica/SMS/Push/Voce) immettere un nome. Selezionare la casella di controllo **Posta elettronica** e quindi immettere un indirizzo di posta elettronica valido.

![Configurare un gruppo di azioni di posta elettronica](./media/automation-tutorial-update-management/configure-email-action-group.png)

Nel riquadro **Email/SMS/Push/Voice** (Posta elettronica/SMS/Push/Voce) selezionare **OK**. Nel riquadro **Aggiungi gruppo di azioni** selezionare **OK**.

Per personalizzare l'oggetto del messaggio di posta elettronica di avviso, in **Crea regola**, in **Personalizza azioni**, selezionare **Oggetto messaggio di posta elettronica**. Al termine, selezionare **Crea regola di avviso**. L'avviso segnala quando una distribuzione di un aggiornamento ha esito positivo e indica i computer inclusi nella distribuzione.

## <a name="schedule-an-update-deployment"></a>Pianificare la distribuzione degli aggiornamenti

Pianificare quindi una distribuzione che rispetti la pianificazione dei rilasci e l'intervallo di servizio per installare gli aggiornamenti. È possibile scegliere i tipi di aggiornamento da includere nella distribuzione. È possibile ad esempio includere gli aggiornamenti critici o della sicurezza ed escludere gli aggiornamenti cumulativi.

>[!NOTE]
>Quando si pianifica una distribuzione degli aggiornamenti, viene creata una risorsa di tipo [pianificazione](shared-resources/schedules.md) collegata al runbook **Patch-MicrosoftOMSComputers** che gestisce la distribuzione degli aggiornamenti nei computer di destinazione. Se si elimina la risorsa di tipo pianificazione dal portale di Azure o si usa PowerShell dopo aver creato la distribuzione, la distribuzione degli aggiornamenti pianificati viene interrotta e viene visualizzato un errore quando si prova a riconfigurarla dal portale. È possibile eliminare la risorsa di tipo pianificazione solo eliminando la pianificazione di distribuzione corrispondente.  
>

Per pianificare una nuova distribuzione di aggiornamenti per la macchina virtuale, passare a **Gestione aggiornamenti** e quindi selezionare **Pianifica la distribuzione di aggiornamenti**.

In **Nuova distribuzione di aggiornamenti** specificare le informazioni seguenti:

* **Name**: immettere un nome univoco per la distribuzione degli aggiornamenti.

* **Sistema operativo**: selezionare il sistema operativo di destinazione per la distribuzione degli aggiornamenti.

* **Gruppi da aggiornare (anteprima)** : Definire una query basata su una combinazione di sottoscrizione, gruppi di risorse, posizioni e tag per creare un gruppo dinamico di macchine virtuali di Azure da includere nella distribuzione. Per altre informazioni, vedere [Gruppi dinamici](automation-update-management-groups.md)

* **Computer da aggiornare**: Selezionare una ricerca salvata o un gruppo importato, oppure scegliere Computer dall'elenco a discesa e selezionare i singoli computer. Se si sceglie**Computer**, l'idoneità del computer è indicata nella colonna **AGGIORNA IDONEITÀ AGENTE**. Per altre informazioni sui diversi metodi di creazione di gruppi di computer nei log di Monitoraggio di Azure, vedere [Gruppi di computer nei log di Monitoraggio di Azure](../azure-monitor/platform/computer-groups.md)

* **Classificazione aggiornamenti**: selezionare i tipi di software inclusi nella distribuzione degli aggiornamenti. Per questa esercitazione, lasciare tutti i tipi selezionati.

  I tipi di classificazione sono:

   |OS  |Type  |
   |---------|---------|
   |Windows     | Aggiornamenti critici</br>Aggiornamenti per la sicurezza</br>Aggiornamenti cumulativi</br>Feature Pack</br>Service Pack</br>Aggiornamenti della definizione</br>Strumenti</br>Aggiornamenti        |
   |Linux     | Aggiornamenti critici e della sicurezza</br>Altri aggiornamenti       |

   Per una descrizione dei tipi di classificazione, vedere le [classificazioni degli aggiornamenti](automation-view-update-assessments.md#update-classifications).

* **Includi/Escludi aggiornamenti**: apre la pagina **Includi/Escludi**. Gli aggiornamenti da includere o escludere si trovano in schede separate.

> [!NOTE]
> Tenere presente che le esclusioni eseguono l'override delle inclusioni. Se ad esempio si definisce una regola di esclusione `*`, non vengono installate patch o pacchetti perché vengono tutti esclusi. Le patch escluse vengono ancora visualizzate come mancanti dal computer. Per i computer Linux, se un pacchetto è incluso, ma ha un pacchetto dipendente che è stato escluso, il pacchetto non viene installato.

* **Impostazioni di pianificazione**: apre il riquadro **Impostazioni di pianificazione**. L'ora di inizio predefinita è 30 minuti dopo il momento corrente. È possibile impostare l'ora di inizio su qualsiasi orario a partire da 10 minuti dal momento corrente.

   È anche possibile specificare se eseguire la distribuzione una sola volta o impostare una pianificazione ricorrente. In **Ricorrenza** selezionare **Una sola volta**. Mantenere l'impostazione predefinita di 1 giorno e selezionare **OK**. Viene configurata una pianificazione ricorrente.

* **Pre-script e post-script**: selezionare gli script da eseguire prima e dopo la distribuzione. Per altre informazioni, vedere [Gestire i pre-script e i post-script](pre-post-scripts.md).

* **Finestra di manutenzione (minuti)** : Lasciare il valore predefinito. Le finestre di manutenzione controllano la quantità di tempo consentito per l'installazione di aggiornamenti. Quando si specifica una finestra di manutenzione, tenere presenti i dettagli seguenti.

  * Le finestre di manutenzione controllano la quantità di aggiornamenti che si tenta di installare.
  * Gestione aggiornamenti non interrompe l'installazione di nuovi aggiornamenti se si avvicina la fine di una finestra di manutenzione.
  * Gestione aggiornamenti non termina gli aggiornamenti in corso se viene superata la finestra di manutenzione.
  * Se la finestra di manutenzione viene superata in Windows, il motivo è in genere dovuto all'aggiornamento di un service pack la cui installazione impiega molto tempo.

  > [!NOTE]
  > Per evitare che gli aggiornamenti vengano applicati al di fuori di una finestra di manutenzione in Ubuntu, riconfigurare il pacchetto Unattended-Upgrade per disabilitare gli aggiornamenti automatici. Per informazioni sulla configurazione del pacchetto, vedere l'[argomento Aggiornamenti automatici nella Guida a Ubuntu Server](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

* **Opzioni di riavvio**: questa impostazione determina le modalità di gestione dei riavvii. Le opzioni disponibili sono:
  * Riavvia se necessario (opzione predefinita)
  * Riavvia sempre
  * Non riavviare mai
  * Riavvia solamente: gli aggiornamenti non verranno installati

> [!NOTE]
> Le chiavi del Registro di sistema elencate in [Chiavi del Registro di sistema usate per gestire il riavvio](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) possono causare un evento di riavvio se l'opzione **Reboot Control** (Controllo del riavvio) è impostata su **Non riavviare mai**.

Al termine della configurazione della pianificazione, selezionare **Crea**.

![Riquadro di impostazioni della pianificazione di aggiornamenti](./media/automation-tutorial-update-management/manageupdates-schedule-win.png)

Verrà nuovamente visualizzato il dashboard dello stato. Selezionare **Distribuzioni di aggiornamenti pianificate** per visualizzare la pianificazione della distribuzione creata.

> [!NOTE]
> Gestione aggiornamenti supporta la distribuzione degli aggiornamenti proprietari e il pre-download di patch. Ciò richiede modifiche dei sistemi sui quali sono applicate le patch. Vedere la sezione sul [supporto del pre-download di aggiornamenti proprietari](automation-configure-windows-update.md) per vedere come configurare queste impostazioni nei propri sistemi.

Le **distribuzioni di aggiornamenti** possono essere create anche a livello di codice. Per informazioni su come creare una **distribuzione di aggiornamenti** con l'API REST, vedere [Configurazione degli aggiornamenti software - Creazione](/rest/api/automation/softwareupdateconfigurations/create). È anche disponibile un runbook di esempio che può essere usato per creare una **distribuzione di aggiornamenti** settimanale. Per altre informazioni su questo runbook, vedere [Create a weekly update deployment for one or more VMs in a resource group](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1) (Creare una distribuzione di aggiornamenti settimanale per una o più macchine virtuali in un gruppo di risorse).

## <a name="view-results-of-an-update-deployment"></a>Visualizzare i risultati di una distribuzione di aggiornamenti

Dopo avere avviato la distribuzione pianificata, è possibile visualizzare lo stato della distribuzione nella scheda **Distribuzioni di aggiornamenti** in **Gestione aggiornamenti**. Se la distribuzione è attualmente in esecuzione, lo stato sarà **In corso**. Al termine della distribuzione, se questa ha esito positivo, lo stato diventa **Completato**. Quando si verificano errori in uno o più aggiornamenti della distribuzione, lo stato sarà **Parzialmente non riuscito**.

Selezionare la distribuzione di aggiornamenti completata per visualizzare il dashboard della distribuzione.

![Dashboard di stato di una distribuzione di aggiornamenti specifica](./media/automation-tutorial-update-management/manageupdates-view-results.png)

In **Risultati aggiornamento** è disponibile un riepilogo che fornisce il numero totale di aggiornamenti e i risultati della distribuzione nella macchina virtuale. La tabella a destra offre una suddivisione dettagliata di ogni aggiornamento e dei risultati dell'installazione.

L'elenco seguente mostra i valori disponibili:

* **Tentativo non eseguito**: l'aggiornamento non è stato installato poiché, in base alla durata definita per la finestra di manutenzione, il tempo disponibile non era sufficiente.
* **Riuscito**: aggiornamento completato.
* **Non riuscito**: aggiornamento non riuscito.

Selezionare **Tutti i log** per visualizzare tutte le voci di log create dalla distribuzione.

Selezionare **Output** per visualizzare il flusso del processo del runbook responsabile della gestione della distribuzione di aggiornamenti nella macchina virtuale di destinazione.

Per visualizzare informazioni dettagliate sugli errori della distribuzione, selezionare **Errori**.

Dopo il completamento della distribuzione dell'aggiornamento, viene inviato un messaggio simile a quello nell'esempio seguente per indicare la riuscita della distribuzione:

![Configurare il gruppo di azioni di posta elettronica](./media/automation-tutorial-update-management/email-notification.png)

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione sono state illustrate le procedure per:

> [!div class="checklist"]
> * Caricare una VM per Gestione aggiornamenti
> * Visualizzare una valutazione degli aggiornamenti
> * Configurare gli avvisi
> * Pianificare la distribuzione degli aggiornamenti
> * Visualizzare i risultati di una distribuzione

Passare alla panoramica della soluzione Gestione aggiornamenti.

> [!div class="nextstepaction"]
> [Soluzione Gestione aggiornamenti](../operations-management-suite/oms-solution-update-management.md?toc=%2fazure%2fautomation%2ftoc.json)

