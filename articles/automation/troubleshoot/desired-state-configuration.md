---
title: Risolvere i problemi di automazione DSC (Desired state Configuration) di Azure
description: Questo articolo contiene informazioni sulla risoluzione dei problemi di configurazione dello stato desiderato (DSC, Desired State Configuration)
services: automation
ms.service: automation
ms.subservice: ''
author: mgoedtel
ms.author: magoedte
ms.date: 04/16/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: dcd0371d275c3a46fe9bf07c96516a2d0820abb7
ms.sourcegitcommit: dfa543fad47cb2df5a574931ba57d40d6a47daef
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/18/2020
ms.locfileid: "77430534"
---
# <a name="troubleshoot-issues-with-azure-automation-desired-state-configuration-dsc"></a>Risolvere i problemi di automazione DSC (Desired state Configuration) di automazione di Azure

Questo articolo contiene informazioni sulla risoluzione dei problemi della configurazione dello stato desiderato (DSC, Desired State Configuration).

## <a name="diagnosing-an-issue"></a>Diagnosi di un problema

Quando si verificano errori durante la compilazione o la distribuzione di configurazioni in Azure state Configuration, di seguito sono riportati alcuni passaggi per diagnosticare il problema.

### <a name="1-ensure-that-your-configuration-compiles-successfully-on-the-local-machine"></a>1. Assicurarsi che la configurazione venga compilata correttamente nel computer locale

La configurazione dello stato di Azure si basa su PowerShell DSC. È possibile trovare la documentazione relativa alla sintassi e al linguaggio DSC in [PowerShell DSC docs](https://docs.microsoft.com/powershell/scripting/overview).

Compilando la configurazione DSC nel computer locale, è possibile individuare e risolvere gli errori comuni, ad esempio:

   - Moduli mancanti
   - Errori di sintassi
   - Errori di logica

### <a name="2-view-dsc-logs-on-your-node"></a>2. visualizzare i log DSC nel nodo

Se la configurazione viene compilata correttamente, ma ha esito negativo quando viene applicata a un nodo, è possibile trovare informazioni dettagliate nei log DSC. Per informazioni su dove trovare questi log, vedere [dove sono i registri eventi DSC](/powershell/scripting/dsc/troubleshooting/troubleshooting#where-are-dsc-event-logs).

Il modulo [xDscDiagnostics](https://github.com/PowerShell/xDscDiagnostics) può risultare utile per l'analisi di informazioni dettagliate dai log DSC. Se si contatta il supporto tecnico, questi registri sono necessari per diagnosticare il problema.

È possibile installare il modulo xDscDiagnostics nel computer locale usando le istruzioni disponibili in [installare il modulo versione stabile](https://github.com/PowerShell/xDscDiagnostics#install-the-stable-version-module).

Per installare il modulo xDscDiagnostics nel computer Azure, usare [Invoke-AzVMRunCommand](/powershell/module/azurerm.compute/invoke-azurermvmruncommand). È anche possibile usare l'opzione **Esegui comando** dal portale, seguendo i passaggi descritti in [eseguire script di PowerShell nella macchina virtuale Windows con il comando Esegui](../../virtual-machines/windows/run-command.md).

Per informazioni sull'uso di xDscDiagnostics, vedere [uso di xDscDiagnostics per analizzare i log DSC](/powershell/scripting/dsc/troubleshooting/troubleshooting#using-xdscdiagnostics-to-analyze-dsc-logs). Vedere anche [cmdlet xDscDiagnostics](https://github.com/PowerShell/xDscDiagnostics#cmdlets).

### <a name="3-ensure-that-nodes-and-the-automation-workspace-have-required-modules"></a>3. Assicurarsi che i nodi e l'area di lavoro di automazione dispongano dei moduli necessari

DSC dipende dai moduli installati nel nodo. Quando si usa la configurazione dello stato di automazione di Azure, importare tutti i moduli richiesti nell'account di automazione usando i passaggi elencati in [importare i moduli](../shared-resources/modules.md#import-modules). Le configurazioni possono anche avere una dipendenza da versioni specifiche dei moduli. Per altre informazioni, vedere [risolvere i problemi relativi ai moduli](shared-resources.md#modules).

## <a name="common-errors-when-working-with-dsc"></a>Errori comuni durante l'uso di DSC

### <a name="unsupported-characters"></a>Scenario: una configurazione con caratteri speciali non può essere eliminata dal portale

#### <a name="issue"></a>Problema

Quando si tenta di eliminare una configurazione DSC dal portale, viene visualizzato l'errore seguente:

```error
An error occurred while deleting the DSC configuration '<name>'.  Error-details: The argument configurationName with the value <name> is not valid.  Valid configuration names can contain only letters,  numbers, and underscores.  The name must start with a letter.  The length of the name must be between 1 and 64 characters.
```

#### <a name="cause"></a>Causa

Questo errore è un problema temporaneo che è pianificato per essere risolto.

#### <a name="resolution"></a>Risoluzione

* Usare il comando AZ cmdlet "Remove-AzAutomationDscConfiguration" per eliminare la configurazione.
* La documentazione per questo cmdlet non è stata ancora aggiornata.  Fino ad allora, fare riferimento alla documentazione per il modulo AzureRM.
  * [Remove-AzureRmAutomationDSCConfiguration](/powershell/module/azurerm.automation/Remove-AzureRmAutomationDscConfiguration)

### <a name="failed-to-register-agent"></a>Scenario: non è stato possibile registrare l'agente DSC

#### <a name="issue"></a>Problema

Quando si tenta di eseguire `Set-DscLocalConfigurationManager` o un altro cmdlet DSC, viene visualizzato l'errore:

```error
Registration of the Dsc Agent with the server
https://<location>-agentservice-prod-1.azure-automation.net/accounts/00000000-0000-0000-0000-000000000000 failed. The
underlying error is: Failed to register Dsc Agent with AgentId 00000000-0000-0000-0000-000000000000 with the server htt
ps://<location>-agentservice-prod-1.azure-automation.net/accounts/00000000-0000-0000-0000-000000000000/Nodes(AgentId='00000000-0000-0000-0000-000000000000'). .
    + CategoryInfo          : InvalidResult: (root/Microsoft/...gurationManager:String) [], CimException
    + FullyQualifiedErrorId : RegisterDscAgentCommandFailed,Microsoft.PowerShell.DesiredStateConfiguration.Commands.Re
   gisterDscAgentCommand
    + PSComputerName        : <computerName>
```

#### <a name="cause"></a>Causa

Questo errore è in genere causato da un firewall, dal computer che si trova dietro un server proxy o da altri errori di rete.

#### <a name="resolution"></a>Risoluzione

Verificare che il computer abbia accesso agli endpoint appropriati per Automation DSC di Azure e riprovare. Per un elenco di porte e indirizzi necessari, vedere [pianificazione della rete](../automation-dsc-overview.md#network-planning)

### <a name="a-nameunauthorizedascenario-status-reports-return-response-code-unauthorized"></a>Scenario di <a/><a name="unauthorized">: i rapporti di stato restituiscono il codice di risposta "non autorizzato"

#### <a name="issue"></a>Problema

Quando si registra un nodo con configurazione dello stato (DSC), viene visualizzato uno dei messaggi di errore seguenti:

```error
The attempt to send status report to the server https://{your Automation account URL}/accounts/xxxxxxxxxxxxxxxxxxxxxx/Nodes(AgentId='xxxxxxxxxxxxxxxxxxxxxxxxx')/SendReport returned unexpected response code Unauthorized.
```

```error
VM has reported a failure when processing extension 'Microsoft.Powershell.DSC / Registration of the Dsc Agent with the server failed.
```

### <a name="cause"></a>Causa

Questo problema è causato da un certificato non valido o scaduto.  Per ulteriori informazioni, vedere [scadenza e registrazione dei certificati](../automation-dsc-onboarding.md#certificate-expiration-and-re-registration).

### <a name="resolution"></a>Risoluzione

Attenersi alla procedura riportata di seguito per registrare nuovamente il nodo DSC in errore.

Per prima cosa, annullare la registrazione del nodo attenendosi alla procedura seguente.

1. Dal portale di Azure, in **Home** -> **account di automazione**-> {Your automation account}-> **state Configuration (DSC)**
2. Fare clic su "nodi" e fare clic sul nodo in cui si sono verificati problemi.
3. Fare clic su "Annulla registrazione" per annullare la registrazione del nodo.

In secondo luogo, disinstallare l'estensione DSC dal nodo.

1. Dal portale di Azure, in **Home** -> **macchina virtuale** -> {nodo in errore}-> **Extensions**
2. Fare clic su "Microsoft. PowerShell. DSC".
3. Fare clic su "Disinstalla" per disinstallare l'estensione DSC di PowerShell.

Terzo, rimuovere tutti i certificati non validi o scaduti dal nodo.

Nel nodo che ha avuto esito negativo da un prompt di PowerShell con privilegi elevati, eseguire il comando seguente:

```powershell
$certs = @()
$certs += dir cert:\localmachine\my | ?{$_.FriendlyName -like "DSC"}
$certs += dir cert:\localmachine\my | ?{$_.FriendlyName -like "DSC-OaaS Client Authentication"}
$certs += dir cert:\localmachine\CA | ?{$_.subject -like "CN=AzureDSCExtension*"}
"";"== DSC Certificates found: " + $certs.Count
$certs | FL ThumbPrint,FriendlyName,Subject
If (($certs.Count) -gt 0)
{ 
    ForEach ($Cert in $certs) 
    {
        RD -LiteralPath ($Cert.Pspath) 
    }
}
```

Infine, registrare di nuovo il nodo che ha avuto esito negativo attenendosi alla procedura seguente.

1. Dal portale di Azure, in **Home** -> **account di automazione** -> {Your automation account}-> **state Configuration (DSC)**
2. Fare clic su "Nodes".
3. Fare clic sul pulsante "Aggiungi".
4. Selezionare il nodo in errore.
5. Fare clic su "Connetti" e selezionare le opzioni desiderate.

### <a name="failed-not-found"></a>Scenario: Lo stato del nodo indica che non è riuscito con errore "Non trovato"

#### <a name="issue"></a>Problema

Un report del nodo indica lo stato **non riuscito** e contiene l'errore seguente:

```error
The attempt to get the action from server https://<url>//accounts/<account-id>/Nodes(AgentId=<agent-id>)/GetDscAction failed because a valid configuration <guid> cannot be found.
```

#### <a name="cause"></a>Causa

Questo errore si verifica in genere quando al nodo viene assegnato un nome di configurazione, ad esempio ABC, anziché un nome di configurazione nodo, ad esempio ABC.WebServer.

#### <a name="resolution"></a>Risoluzione

* Assicurarsi di assegnare il nodo "nome configurazione nodo" e non il "nome configurazione".
* È possibile assegnare a un nodo una configurazione di nodo usando il portale di Azure o un cmdlet di PowerShell.

  * Per assegnare una configurazione del nodo a un nodo usando portale di Azure, aprire la pagina **nodi DSC** , selezionare un nodo e fare clic sul pulsante **Assegna configurazione nodo** .
  * Per assegnare una configurazione del nodo a un nodo usando il cmdlet di PowerShell, usare il cmdlet **set-AzureRmAutomationDscNode**

### <a name="no-mof-files"></a>Scenario: Non sono state prodotte configurazioni di nodo (file MOF) durante la compilazione di una configurazione

#### <a name="issue"></a>Problema

Il processo di compilazione DSC viene sospeso con l'errore seguente:

```error
Compilation completed successfully, but no node configuration.mofs were generated.
```

#### <a name="cause"></a>Causa

Quando l'espressione che segue la parola chiave **Node** nella configurazione DSC restituisce `$null`, non vengono prodotte configurazioni di nodo.

#### <a name="resolution"></a>Risoluzione

una qualsiasi delle soluzioni seguenti consente di correggere il problema:

* Verificare che l'espressione accanto alla parola chiave **node** nella definizione di configurazione non stia valutando $null.
* Se durante la compilazione della configurazione si passano dei dati di configurazione, verificare di specificare i valori previsti necessari per la configurazione da [ConfigurationData](../automation-dsc-compile.md).

### <a name="dsc-in-progress"></a>Scenario: Il report relativo al nodo DSC rimane bloccato nello stato "in corso"

#### <a name="issue"></a>Problema

L'output dell'agente DSC è il seguente:

```error
No instance found with given property values
```

#### <a name="cause"></a>Causa

È stato eseguito l'aggiornamento della versione di WMF e WMI è stato danneggiato.

#### <a name="resolution"></a>Risoluzione

Per risolvere il problema, seguire le istruzioni riportate nell'articolo [limitazioni e problemi noti di DSC](https://docs.microsoft.com/powershell/scripting/wmf/known-issues/known-issues-dsc) .

### <a name="issue-using-credential"></a>Scenario: Non è possibile usare le credenziali in una configurazione DSC

#### <a name="issue"></a>Problema

Il processo di compilazione DSC è stato sospeso con l'errore seguente:

```error
System.InvalidOperationException error processing property 'Credential' of type <some resource name>: Converting and storing an encrypted password as plaintext is allowed only if PSDscAllowPlainTextPassword is set to true.
```

#### <a name="cause"></a>Causa

Sono state usate le credenziali in una configurazione ma non è stato fornito un **configurationData** appropriato per impostare **PSDscAllowPlainTextPassword** su true per ogni configurazione del nodo.

#### <a name="resolution"></a>Risoluzione

* Assicurarsi di passare il **configurationData** appropriato per impostare **PSDscAllowPlainTextPassword** su true per ogni configurazione di nodo citata nella configurazione. Per altre informazioni, vedere [compilazione di configurazioni DSC in configurazione dello stato di automazione di Azure](../automation-dsc-compile.md).

### <a name="failure-processing-extension"></a>Scenario: onboarding dall'estensione DSC, errore dell'estensione per l'elaborazione degli errori

#### <a name="issue"></a>Problema

Quando si esegue l'onboarding con l'estensione DSC, si verifica un errore che contiene l'errore:

```error
VM has reported a failure when processing extension 'Microsoft.Powershell.DSC'. Error message: \"DSC COnfiguration 'RegistrationMetaConfigV2' completed with error(s). Following are the first few: Registration of the Dsc Agent with the server <url> failed. The underlying error is: The attempt to register Dsc Agent with Agent Id <ID> with the server <url> return unexpected response code BadRequest. .\".
```

#### <a name="cause"></a>Causa

Questo errore si verifica in genere quando al nodo viene assegnato un nome di configurazione di nodo che non esiste nel servizio.

#### <a name="resolution"></a>Risoluzione

* Assicurarsi di assegnare il nodo con un nome di configurazione nodo che corrisponda esattamente al nome del servizio.
* È possibile scegliere di non includere il nome della configurazione del nodo, che comporterà l'onboarding del nodo, ma non l'assegnazione di una configurazione del nodo

### <a name="cross-subscription"></a>Scenario: la registrazione di un nodo con PowerShell restituisce l'errore "si sono verificati uno o più errori"

#### <a name="issue"></a>Problema

Quando si registra un nodo usando `Register-AzAutomationDSCNode` o `Register-AzureRMAutomationDSCNode`, viene visualizzato l'errore seguente.

```error
One or more errors occurred.
```

#### <a name="cause"></a>Causa

Questo errore si verifica quando si tenta di registrare un nodo che risiede in una sottoscrizione distinta rispetto all'account di automazione.

#### <a name="resolution"></a>Risoluzione

Considerare il nodo tra sottoscrizioni come se si trovasse in un cloud separato o in locale.

Attenersi alla procedura seguente per registrare il nodo.

* Windows: [computer fisici/virtuali Windows locali o in un cloud diverso da Azure/AWS](../automation-dsc-onboarding.md#physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azure-including-aws-ec2-instances).
* Linux: [macchine virtuali Linux in locale o in un cloud diverso da Azure](../automation-dsc-onboarding.md#physicalvirtual-linux-machines-on-premises-or-in-a-cloud-other-than-azure).

### <a name="agent-has-a-problem"></a>Scenario: messaggio di errore-"provisioning non riuscito"

#### <a name="issue"></a>Problema

Quando si registra un nodo, viene visualizzato l'errore:

```error
Provisioning has failed
```

#### <a name="cause"></a>Causa

Questo messaggio viene visualizzato quando si verifica un problema di connettività tra il nodo e Azure.

#### <a name="resolution"></a>Risoluzione

Determinare se il nodo si trova in una rete virtuale privata o se si sono verificati altri problemi di connessione ad Azure.

Per altre informazioni, vedere [risolvere gli errori durante l'onboarding delle soluzioni](onboarding.md).

### <a name="failure-linux-temp-noexec"></a>Scenario: applicazione di una configurazione in Linux. si verifica un errore con un errore generale

#### <a name="issue"></a>Problema

Quando si applica una configurazione in Linux, si verifica un errore che contiene l'errore:

```error
This event indicates that failure happens when LCM is processing the configuration. ErrorId is 1. ErrorDetail is The SendConfigurationApply function did not succeed.. ResourceId is [resource]name and SourceInfo is ::nnn::n::resource. ErrorMessage is A general error occurred, not covered by a more specific error code..
```

#### <a name="cause"></a>Causa

I clienti hanno identificato che se il percorso del `/tmp` è impostato su `noexec`, la versione corrente di DSC non sarà in grado di applicare le configurazioni.

#### <a name="resolution"></a>Risoluzione

* Rimuovere l'opzione `noexec` dal percorso `/tmp`.

### <a name="compilation-node-name-overlap"></a>Scenario: i nomi di configurazione del nodo che si sovrappongono possono causare una versione non valida

#### <a name="issue"></a>Problema

Se viene utilizzato un singolo script di configurazione per generare più configurazioni di nodo e alcune delle configurazioni del nodo hanno un nome che è un subset di altri, un problema nel servizio di compilazione può comportare l'assegnazione della configurazione errata.  Questo si verifica solo quando si usa un singolo script per generare configurazioni con i dati di configurazione per nodo e solo quando si verifica una sovrapposizione del nome all'inizio della stringa.

Esempio, se viene utilizzato un singolo script di configurazione per generare configurazioni basate sui dati del nodo passati come Hashtable mediante i cmdlet e i dati del nodo includono un server denominato "Server" e "1Server".

#### <a name="cause"></a>Causa

Problema noto con il servizio di compilazione.

#### <a name="resolution"></a>Risoluzione

La soluzione migliore consiste nel compilare localmente o in una pipeline CI/CD e caricare i file MOF direttamente nel servizio.  Se la compilazione nel servizio è un requisito, la soluzione migliore successiva consiste nel suddividere i processi di compilazione in modo che non vi siano sovrapposizioni nei nomi.

## <a name="next-steps"></a>Passaggi successivi

Se il problema riscontrato non è presente in questo elenco o se non si riesce a risolverlo, visitare uno dei canali seguenti per ottenere ulteriore assistenza:

* Ottieni risposte dagli esperti di Azure tramite i [Forum di Azure](https://azure.microsoft.com/support/forums/).
* Collegarsi a [@AzureSupport](https://twitter.com/azuresupport), l'account Microsoft Azure ufficiale per il miglioramento dell'esperienza dei clienti che mette in contatto la community di Azure con le risorse corrette: risposte, supporto ed esperti.
* Se è necessaria un'assistenza maggiore, è possibile inviare una richiesta al supporto tecnico di Azure. Accedere al [sito del supporto di Azure](https://azure.microsoft.com/support/options/) e selezionare **Ottenere supporto**.
