---
title: Risolvere gli errori con Gestione aggiornamenti
description: Informazioni su come risolvere i problemi con Gestione aggiornamenti
services: automation
author: bobbytreed
ms.author: robreed
ms.date: 05/31/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 48d2463eee2caeaae36118bf736d00eed84c897a
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/30/2019
ms.locfileid: "70186220"
---
# <a name="troubleshooting-issues-with-update-management"></a>Risoluzione dei problemi con Gestione aggiornamenti

Questo articolo illustra alcune procedure per la risoluzione dei problemi che si possono riscontrare quando si usa Gestione aggiornamenti.

È disponibile uno strumento di risoluzione dei problemi dell'agente che consente all'agente del ruolo di lavoro ibrido di determinare il problema sottostante. Per altre informazioni sullo strumento di risoluzione dei problemi, vedere [Risolvere i problemi dell'agente di aggiornamento](update-agent-issues.md). Per tutti gli altri problemi, vedere le informazioni dettagliate seguenti sui possibili problemi.

## <a name="general"></a>Generale

### <a name="rp-register"></a>Scenario: Non è possibile registrare il provider di risorse di automazione per le sottoscrizioni

#### <a name="issue"></a>Problema

È possibile che venga visualizzato l'errore seguente quando si utilizzano soluzioni nell'account di automazione.

```error
Error details: Unable to register Automation Resource Provider for subscriptions:
```

#### <a name="cause"></a>Causa

Il provider di risorse di automazione non è registrato nella sottoscrizione.

#### <a name="resolution"></a>Risoluzione

È possibile registrare i provider di risorse di automazione completando i seguenti passaggi nel portale di Azure:

1. Fare clic su **Tutti i servizi** in fondo all'elenco dei servizi di Azure e quindi selezionare **Sottoscrizioni** nel gruppo di servizi _Generale_.
2. Selezionare la propria sottoscrizione.
3. Fare clic su **provider di risorse** in _Impostazioni_.
4. Dall'elenco dei provider di risorse, verificare che il provider di risorse **Microsoft. Automation** sia registrato.
5. Se il provider non è elencato, registrare il provider **Microsoft. Automation** con i passaggi elencati in [](/azure/azure-resource-manager/resource-manager-register-provider-errors).

### <a name="mw-exceeded"></a>Scenario: Gestione aggiornamenti pianificata non riuscita con l'errore MaintenanceWindowExceeded

#### <a name="issue"></a>Problema

La finestra di manutenzione predefinita per gli aggiornamenti è di 120 minuti. È possibile aumentare la finestra di manutenzione fino a un massimo di sei (6) ore o 360 minuti.

#### <a name="resolution"></a>Risoluzione

Modificare le distribuzioni degli aggiornamenti pianificati con errori e aumentare la finestra di manutenzione.

Per ulteriori informazioni sulle finestre di manutenzione, vedere [Install Updates](../automation-update-management.md#install-updates).

### <a name="components-enabled-not-working"></a>Scenario: i componenti per la soluzione di Gestione aggiornamenti sono stati abilitati ed è in corso la configurazione della macchina virtuale

#### <a name="issue"></a>Problema

Nella macchina virtuale continua a essere visualizzato il messaggio seguente 15 minuti dopo l'onboarding:

```error
The components for the 'Update Management' solution have been enabled, and now this virtual machine is being configured. Please be patient, as this can sometimes take up to 15 minutes.
```

#### <a name="cause"></a>Causa

Questo errore può dipendere dalle cause seguenti:

1. Le comunicazioni verso l'account di Automazione sono bloccate.
2. La macchina virtuale di cui si esegue l'onboarding può provenire da una macchina clonata che non è stata preparata tramite Sysprep con Microsoft Monitoring Agent installato.

#### <a name="resolution"></a>Risoluzione

1. Vedere [Configurare la rete](../automation-hybrid-runbook-worker.md#network-planning) per informazioni sugli indirizzi e sulle porte da abilitare per il funzionamento di Gestione aggiornamenti.
2. Se si usa un'immagine clonata:
   1. Nell'area di lavoro log Analytics rimuovere la macchina virtuale dalla ricerca salvata per la configurazione `MicrosoftDefaultScopeConfig-Updates` dell'ambito, se visualizzata. Le ricerche salvate sono disponibili in **Generale** nell'area di lavoro.
   2. Eseguire `Remove-Item -Path "HKLM:\software\microsoft\hybridrunbookworker" -Recurse -Force`
   3. Eseguire `Restart-Service HealthService` per riavviare `HealthService`. In questo modo viene ricreata la chiave e viene creato un nuovo UUID.
   4. Se questa operazione non funziona, eseguire prima Sysprep l'immagine e installare l'agente MMA dopo il fatto.

### <a name="multi-tenant"></a>Scenario: viene visualizzato un errore di sottoscrizione collegata durante la creazione di una distribuzione degli aggiornamenti per i computer in un altro tenant di Azure.

#### <a name="issue"></a>Problema

viene visualizzato il seguente durante la creazione di una distribuzione degli aggiornamenti per i computer in un altro tenant di Azure:

```error
The client has permission to perform action 'Microsoft.Compute/virtualMachines/write' on scope '/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Automation/automationAccounts/automationAccountName/softwareUpdateConfigurations/updateDeploymentName', however the current tenant '00000000-0000-0000-0000-000000000000' is not authorized to access linked subscription '00000000-0000-0000-0000-000000000000'.
```

#### <a name="cause"></a>Causa

Questo errore si verifica quando si crea una distribuzione degli aggiornamenti con macchine virtuali di Azure in un altro tenant incluso in una distribuzione degli aggiornamenti.

#### <a name="resolution"></a>Risoluzione

Occorrerà usare la seguente soluzione alternativa per la pianificazione. È possibile usare il cmdlet [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) con l'opzione `-ForUpdate` per creare una pianificazione, usare il cmdlet [New-AzureRmAutomationSoftwareUpdateConfiguration](/powershell/module/azurerm.automation/new-azurermautomationsoftwareupdateconfiguration
) e passare i computer nell'altro tenant al parametro `-NonAzureComputer`. Segue un esempio di come effettuare questa operazione:

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$s = New-AzureRmAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdate

New-AzureRmAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName $aa -Schedule $s -Windows -AzureVMResourceId $azureVMIdsW -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

### <a name="updates-nodeployment"></a>Scenario: Aggiornamenti installazione senza distribuzione

### <a name="issue"></a>Problema

Quando si registra un computer Windows in Gestione aggiornamenti, è possibile visualizzare gli aggiornamenti installa senza una distribuzione.

### <a name="cause"></a>Causa

In Windows, gli aggiornamenti vengono installati automaticamente non appena sono disponibili. Ciò può causare confusione se non è stato pianificato un aggiornamento da distribuire nel computer.

### <a name="resolution"></a>Risoluzione

La chiave del registro di `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU` sistema di Windows, il valore predefinito è "4"- **download automatico e installazione**.

Per Gestione aggiornamenti client, è consigliabile impostare questa chiave su "3"- **download automatico ma non installazione automatica**.

Per ulteriori informazioni, vedere [configurazione di aggiornamenti automatici](https://docs.microsoft.com/en-us/windows/deployment/update/waas-wu-settings#configure-automatic-updates).

### <a name="nologs"></a>Scenario: I computer non vengono visualizzati nel portale in Gestione aggiornamenti

#### <a name="issue"></a>Problema

È possibile che si verifichino negli scenari seguenti:

* Il computer visualizzato **non è configurato** dalla visualizzazione Gestione aggiornamenti di una macchina virtuale

* Il computer non è presente nella visualizzazione Gestione aggiornamenti dell'account di automazione

* Sono presenti computer che non sono **valutati** in **conformità**, ma i dati heartbeat nei log di monitoraggio di Azure per il ruolo di lavoro ibrido per Runbook, ma non gestione aggiornamenti.

#### <a name="cause"></a>Causa

Questa situazione può essere causata da problemi di configurazione locali potenziali o da una configurazione non corretta dell'ambito.

È possibile che il ruolo di lavoro ibrido per runbook debba essere registrato nuovamente e reinstallato.

È possibile che sia stata definita una quota nell'area di lavoro raggiunta e l'interruzione dell'archiviazione dei dati.

#### <a name="resolution"></a>Risoluzione

* Verificare che il computer stia segnalando l'area di lavoro corretta. Verificare l'area di lavoro a cui viene segnalato il computer. Per istruzioni su come verificare questo problema, vedere [verificare la connettività dell'agente a log Analytics](../../azure-monitor/platform/agent-windows.md#verify-agent-connectivity-to-log-analytics). Assicurarsi quindi che questa sia l'area di lavoro collegata all'account di automazione di Azure. Per confermarlo, passare all'account di automazione e fare clic su **area di lavoro collegata** in **risorse correlate**.

* Verificare che le macchine virtuali vengano visualizzate nell'area di lavoro Log Analytics. Eseguire la query seguente nell'area di lavoro di Log Analytics collegata all'account di automazione. Se il computer non viene visualizzato nei risultati della query, il computer non è in stato di heartbeat, il che significa che è probabile che si verifichi un problema di configurazione locale. È possibile eseguire lo strumento di risoluzione dei problemi per [Windows](update-agent-issues.md#troubleshoot-offline) o [Linux](update-agent-issues-linux.md#troubleshoot-offline) a seconda del sistema operativo oppure [reinstallare l'agente](../../azure-monitor/learn/quick-collect-windows-computer.md#install-the-agent-for-windows). Se il computer viene visualizzato nei risultati della query, è necessario specificare la configurazione dell'ambito specificata nel seguente elenco.

  ```loganalytics
  Heartbeat
  | summarize by Computer, Solutions
  ```

* Verificare la presenza di problemi di configurazione dell'ambito. La [configurazione dell'ambito](../automation-onboard-solutions-from-automation-account.md#scope-configuration) determina quali computer vengono configurati per la soluzione. Se il computer viene visualizzato nell'area di lavoro ma non viene visualizzato, sarà necessario configurare la configurazione dell'ambito per le macchine virtuali. Per informazioni su come eseguire questa operazione, vedere l'articolo relativo all' [onboarding dei computer nell'area di lavoro](../automation-onboard-solutions-from-automation-account.md#onboard-machines-in-the-workspace).

* Se i passaggi precedenti non risolvono il problema, seguire la procedura descritta in [distribuire un ruolo di lavoro ibrido per Runbook Windows](../automation-windows-hrw-install.md) per reinstallare il ruolo di lavoro ibrido per Windows o [distribuire un ruolo di lavoro ibrido per Runbook Linux](../automation-linux-hrw-install.md) per Linux.

* Nell'area di lavoro eseguire la query seguente. Se viene visualizzato il risultato `Data collection stopped due to daily limit of free data reached. Ingestion status = OverQuota` , è stata definita una quota nell'area di lavoro che è stata raggiunta ed è stato interrotto il salvataggio dei dati. Nell'area di lavoro passare > a **utilizzo e costi stimati** **gestione del volume di dati** e verificare la quota o rimuovere la quota.

  ```loganalytics
  Operation
  | where OperationCategory == 'Data Collection Status'
  | sort by TimeGenerated desc
  ```

## <a name="windows"></a>Windows

Se si riscontrano problemi durante il tentativo di caricare la soluzione o una macchina virtuale, nel computer locale cercare nel log eventi di **Operations Manager**, in **Registri applicazioni e servizi**, gli eventi con ID **4502** e il messaggio di evento contenente **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**.

La sezione seguente evidenzia i messaggi di errore specifici e una possibile risoluzione per ognuno. Per altri problemi di onboarding, vedere la sezione relativa alla [risoluzione dei problemi di onboarding delle soluzioni](onboarding.md).

### <a name="machine-already-registered"></a>Scenario: il computer è già registrato per un altro account

#### <a name="issue"></a>Problema

Viene visualizzato il messaggio di errore seguente:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.InvalidOperationException: {"Message":"Machine is already registered to a different account."}
```

#### <a name="cause"></a>Causa

Il computer è già caricato in un'altra area di lavoro per Gestione aggiornamenti.

#### <a name="resolution"></a>Risoluzione

Eseguire la pulizia degli elementi obsoleti nel computer [eliminando il gruppo di runbook ibridi](../automation-hybrid-runbook-worker.md#remove-a-hybrid-worker-group) e riprovare.

### <a name="machine-unable-to-communicate"></a>Scenario: il computer non è in grado di comunicare con il servizio

#### <a name="issue"></a>Problema

Viene visualizzato uno dei seguenti messaggi di errore:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.Net.Http.HttpRequestException: An error occurred while sending the request. ---> System.Net.WebException: The underlying connection was closed: An unexpected error occurred on a receive. ---> System.ComponentModel.Win32Exception: The client and server can't communicate, because they do not possess a common algorithm
```

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception Newtonsoft.Json.JsonReaderException: Error parsing positive infinity value.
```

```error
The certificate presented by the service <wsid>.oms.opinsights.azure.com was not issued by a certificate authority used for Microsoft services. Contact your network administrator to see if they are running a proxy that intercepts TLS/SSL communication.
```

#### <a name="cause"></a>Causa

La comunicazione di rete potrebbe essere bloccata da un proxy, un gateway o un firewall.

#### <a name="resolution"></a>Risoluzione

Esaminare le funzionalità di rete e verificare che le porte e gli indirizzi appropriati siano consentiti. Vedere i [requisiti di rete](../automation-hybrid-runbook-worker.md#network-planning) per un elenco di porte e indirizzi richiesti da Gestione aggiornamenti e i ruoli di lavoro ibrido per runbook.

### <a name="unable-to-create-selfsigned-cert"></a>Scenario: impossibile creare un certificato autofirmato

#### <a name="issue"></a>Problema

Viene visualizzato uno dei seguenti messaggi di errore:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception AgentService.HybridRegistration. PowerShell.Certificates.CertificateCreationException: Failed to create a self-signed certificate. ---> System.UnauthorizedAccessException: Access is denied.
```

#### <a name="cause"></a>Causa

Il ruolo di lavoro ibrido per runbook non è stato in grado di generare un certificato autofirmato.

#### <a name="resolution"></a>Risoluzione

Verificare che l'account di sistema abbia accesso in lettura alla cartella **C:\ProgramData\Microsoft\Crypto\RSA** e riprovare.

### <a name="failed-to-start"></a>Scenario: Non è stato possibile avviare un computer in una distribuzione di aggiornamenti

#### <a name="issue"></a>Problema

Lo stato di un computer **non** è stato avviato per un computer. Quando si visualizzano i dettagli specifici per il computer, viene visualizzato l'errore seguente:

```error
Failed to start the runbook. Check the parameters passed. RunbookName Patch-MicrosoftOMSComputer. Exception You have requested to create a runbook job on a hybrid worker group that does not exist.
```

#### <a name="cause"></a>Causa

Questo errore può verificarsi a causa di uno dei motivi seguenti:

* Il computer non esiste più.
* Il computer è disattivato e non è raggiungibile.
* Il computer ha un problema di connettività di rete e il ruolo di lavoro ibrido nel computer non è raggiungibile.
* Si è verificato un aggiornamento per la Microsoft Monitoring Agent che ha modificato il SourceComputerId
* L'esecuzione dell'aggiornamento potrebbe essere stata limitata se si raggiunge il limite di 2.000 di processi simultanei in un account di automazione. Ogni distribuzione viene considerata un processo e ogni computer in una distribuzione di aggiornamenti viene conteggiato come un processo. Qualsiasi altro processo di automazione o distribuzione di aggiornamenti attualmente in esecuzione nell'account di automazione viene conteggiato per il limite di processi simultanei.

#### <a name="resolution"></a>Risoluzione

Quando applicabile, usare i [gruppi dinamici](../automation-update-management.md#using-dynamic-groups) per le distribuzioni degli aggiornamenti.

* Verificare che il computer esista ancora e che sia raggiungibile. Se non esiste, modificare la distribuzione e rimuovere il computer.
* Vedere la sezione relativa alla [pianificazione della rete](../automation-update-management.md#ports) per un elenco di porte e indirizzi necessari per gestione aggiornamenti e verificare che il computer soddisfi questi requisiti.
* Eseguire la query seguente in log Analytics per trovare i computer nell'ambiente in `SourceComputerId` cui sono stati modificati. Cercare i computer con lo stesso `Computer` valore, ma con un valore diverso. `SourceComputerId` Una volta individuati i computer interessati, è necessario modificare le distribuzioni degli aggiornamenti destinate a tali computer, quindi rimuovere e aggiungere nuovamente i computer `SourceComputerId` in modo che riflettano il valore corretto.

   ```loganalytics
   Heartbeat | where TimeGenerated > ago(30d) | distinct SourceComputerId, Computer, ComputerIP
   ```

### <a name="hresult"></a>Scenario: il computer viene indicato come non valutato e genera a un'eccezione HResult

#### <a name="issue"></a>Problema

Sono presenti computer che risultano **non valutati** in **Conformità** con un messaggio di eccezione sottostante.

#### <a name="cause"></a>Causa

Windows Update o Windows Server Update Services non sono configurati correttamente nel computer. Gestione aggiornamenti si basa su Windows Update o Windows Server Update Services per fornire gli aggiornamenti necessari, lo stato delle patch e i risultati delle patch distribuite. Senza queste informazioni Gestione aggiornamenti non può segnalare correttamente segnalare le patch necessarie o installate.

#### <a name="resolution"></a>Risoluzione

Fare doppio clic sull'eccezione in rosso per visualizzare il messaggio completo. Nella tabella seguente individuare possibili soluzioni o azioni da intraprendere:

|Eccezione  |Risoluzione o azione  |
|---------|---------|
|`Exception from HRESULT: 0x……C`     | Per maggiori dettagli sulla causa dell'eccezione, cercare il codice di errore pertinente nell'[elenco codici di errore relativi a Windows Update](https://support.microsoft.com/help/938205/windows-update-error-code-list).        |
|`0x8024402C`</br>`0x8024401C`</br>`0x8024402F`      | Sono problemi di connettività di rete. Assicurarsi che il computer disponga della connettività di rete appropriata per Gestione aggiornamenti. Per un elenco di porte e indirizzi necessari, vedere la sezione sulla [pianificazione della rete](../automation-update-management.md#ports).        |
|`0x8024001E`| L'operazione di aggiornamento non è stata completata perché il servizio o il sistema è stato arrestato.|
|`0x8024002E`| Il servizio Windows Update è disabilitato.|
|`0x8024402C`     | Se si usa un server WSUS, assicurarsi che i valori del Registro di sistema per `WUServer` e `WUStatusServer` nella chiave del Registro di sistema `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate` contengano il server WSUS corretto.        |
|`0x80072EE2`|Problemi di connettività di rete o problemi di comunicazione con un server WSUS configurato. Controllare le impostazioni di WSUS e verificare che sia accessibile dal client.|
|`The service cannot be started, either because it is disabled or because it has no enabled devices associated with it. (Exception from HRESULT: 0x80070422)`     | Assicurarsi che il servizio Windows Update (wuauserv) sia in esecuzione e non sia disattivato.        |
|Qualsiasi altra eccezione generica     | Ricercare possibili soluzioni in Internet e collaborare con il supporto tecnico IT locale.         |

La `windowsupdate.log` revisione di può aiutare a determinare anche la possibile cause. Per ulteriori informazioni su come leggere il log, vedere [come leggere il file windowsupdate. log](https://support.microsoft.com/en-ca/help/902093/how-to-read-the-windowsupdate-log-file).

È inoltre possibile scaricare ed eseguire lo [strumento di risoluzione dei problemi di Windows Update](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) per verificare se sono presenti problemi con Windows Update nel computer.

> [!NOTE]
> Lo [strumento di risoluzione dei problemi di Windows Update](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) è destinato all’uso con i client Windows, ma funziona anche su Windows Server.

## <a name="linux"></a>Linux

### <a name="scenario-update-run-fails-to-start"></a>Scenario: impossibile avviare le operazioni di aggiornamento

#### <a name="issue"></a>Problema

Le operazioni di aggiornamento non vengono avviate in un computer Linux.

#### <a name="cause"></a>Causa

Il ruolo di lavoro ibrido Linux non è integro.

#### <a name="resolution"></a>Risoluzione

Creare una copia del file di log seguente e conservarlo per la risoluzione dei problemi:

```bash
/var/opt/microsoft/omsagent/run/automationworker/worker.log
```

### <a name="scenario-update-run-starts-but-encounters-errors"></a>Scenario: l'operazione di aggiornamento viene avviata, ma si verificano errori

#### <a name="issue"></a>Problema

Un'operazione di aggiornamento viene avviata, ma rileva errori durante l'esecuzione.

#### <a name="cause"></a>Causa

Le possibili cause sono:

* La gestione pacchetti non è integra
* Pacchetti specifici possono interferire con l'applicazione di patch basata su cloud
* Altri motivi

#### <a name="resolution"></a>Risoluzione

Se si verificano errori durante un'operazione di aggiornamento dopo un normale avvio in Linux, controllare l'output del processo del computer interessato. È possibile trovare messaggi di errore specifici generati dalla gestione pacchetti del computer che possono essere esaminati per intervenire di conseguenza. Gestione aggiornamenti richiede che la gestione pacchetti sia integra affinché le distribuzioni degli aggiornamenti abbiano esito positivo.

In alcuni casi gli aggiornamenti dei pacchetti possono interferire con Gestione aggiornamenti e impedire il completamento della distribuzione di un aggiornamento. Se si riscontra questo problema, è necessario escludere questi pacchetti dalle future operazioni di aggiornamento oppure installarli manualmente.

Se non è possibile risolvere un problema di patch, creare una copia del seguente file di log e conservarla **prima** dell'inizio della successiva distribuzione degli aggiornamenti ai fini della risoluzione dei problemi:

```bash
/var/opt/microsoft/omsagent/run/automationworker/omsupdatemgmt.log
```

## <a name="patches-are-not-installed"></a>Le patch non sono installate

### <a name="machines-do-not-install-updates"></a>Computer che non installano gli aggiornamenti

* Provare a eseguire gli aggiornamenti direttamente nel computer. Se non è possibile eseguire gli aggiornamenti nel computer, vedere l'[elenco degli errori potenziali nella guida alla risoluzione dei problemi](https://docs.microsoft.com/azure/automation/troubleshoot/update-management#hresult).
* Se gli aggiornamenti sono eseguiti localmente, provare a rimuovere e reinstallare l'agente nel computer seguendo le istruzioni riportate in ["Rimuovere una macchina virtuale da Gestione aggiornamenti"](https://docs.microsoft.com/azure/automation/automation-update-management#remove-a-vm-from-update-management).

### <a name="i-know-updates-are-available-but-they-dont-show-as-needed-on-my-machines"></a>So che gli aggiornamenti sono disponibili, ma non vengono visualizzati in base alle esigenze nei miei computer

* Ciò si verifica spesso se i computer sono configurati per ottenere aggiornamenti da WSUS/SCCM, che tuttavia non li ha approvati.
* È possibile controllare se i computer sono configurati per WSUS/SCCM eseguendo un [riferimento incrociato tra la chiave del Registro di sistema "UseWUServer" e le chiavi del Registro di sistema nella sezione "Configurazione degli aggiornamenti automatici mediante modifica del Registro di sistema" del presente documento](https://support.microsoft.com/help/328010/how-to-configure-automatic-updates-by-using-group-policy-or-registry-s)

### <a name="updates-show-as-installed-but-i-cant-find-them-on-my-machine"></a>**Gli aggiornamenti vengono visualizzati come installati, ma non è possibile trovarli sul computer**

* Spesso gli aggiornamenti vengono sostituiti da altri aggiornamenti. Per altre informazioni, vedere ["Aggiornamento sostituito" nella Guida alla risoluzione dei problemi di Windows Update](https://docs.microsoft.com/windows/deployment/update/windows-update-troubleshooting#the-update-is-not-applicable-to-your-computer)

### <a name="installing-updates-by-classification-on-linux"></a>**Installazione degli aggiornamenti in base alla classificazione in Linux**

* La distribuzione degli aggiornamenti in Linux in base alla classificazione ("Aggiornamenti critici e della sicurezza") presenta avvertimenti importanti, in particolare per CentOS. Queste [limitazioni sono documentate nella pagina di panoramica di Gestione aggiornamenti](https://docs.microsoft.com/azure/automation/automation-update-management#linux-2)

### <a name="kb2267602-is-consistently--missing"></a>**KB2267602 manca in modo coerente**

* KB2267602 è l'[aggiornamento delle definizioni di Windows Defender](https://www.microsoft.com/wdsi/definitions). Viene aggiornato quotidianamente.

## <a name="other"></a>Scenario: il problema non è incluso in questo elenco

### <a name="issue"></a>Problema

Si è riscontrato un problema che non viene risolto dagli altri scenari elencati.

### <a name="cause"></a>Causa

Le chiavi del registro di sistema non configurate correttamente o mancanti possono causare problemi con Gestione aggiornamenti.

### <a name="resolution"></a>Risoluzione

Eliminare la chiave `HKLM:\SOFTWARE\Microsoft\HybridRunbookWorker` del registro di sistema e riavviare il **HealthService**.

È anche possibile usare i comandi di PowerShell seguenti.

```powershell
Remove-Item -Path "HKLM:\software\microsoft\hybridrunbookworker" -Recurse -Force
Restart-Service healthservice
```

## <a name="next-steps"></a>Passaggi successivi

Se il problema riscontrato non è presente in questo elenco o se non si riesce a risolverlo, visitare uno dei canali seguenti per ottenere ulteriore assistenza:

* Ottieni risposte dagli esperti di Azure tramite i [forum di Azure](https://azure.microsoft.com/support/forums/)
* Collegarsi a [@AzureSupport](https://twitter.com/azuresupport), l'account Microsoft Azure ufficiale per il miglioramento dell'esperienza dei clienti che mette in contatto la community di Azure con le risorse corrette: risposte, supporto ed esperti.
* Se è necessaria un'assistenza maggiore, è possibile inviare una richiesta al supporto tecnico di Azure. Accedere al [sito del supporto di Azure](https://azure.microsoft.com/support/options/) e selezionare **Ottenere supporto**.
