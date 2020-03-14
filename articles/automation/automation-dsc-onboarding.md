---
title: Onboarding di computer per la gestione tramite Configurazione stato di Automazione di Azure
description: Come configurare i computer per la gestione con la configurazione dello stato di automazione di Azure
services: automation
ms.service: automation
ms.subservice: dsc
author: mgoedtel
ms.author: magoedte
ms.topic: conceptual
ms.date: 12/10/2019
manager: carmonm
ms.openlocfilehash: 89e86a6702be7314b99975cac90818252eb07df7
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2020
ms.locfileid: "79278961"
---
# <a name="onboarding-machines-for-management-by-azure-automation-state-configuration"></a>Onboarding di computer per la gestione tramite Configurazione stato di Automazione di Azure

## <a name="why-manage-machines-with-azure-automation-state-configuration"></a>Perché gestire computer tramite Configurazione stato di Automazione di Azure?

La configurazione dello stato di automazione di Azure è un servizio di gestione della configurazione per i nodi DSC (Desired state Configuration) in qualsiasi Data Center nel cloud o in locale.
Consente la scalabilità tra migliaia di computer in modo rapido e facile da una posizione centrale e sicura.
È possibile caricare facilmente i computer, assegnare agli stessi configurazioni dichiarative e visualizzare report che mostrano la conformità di ogni computer con lo stato desiderato specificato.
Il servizio di configurazione dello stato di automazione di Azure è DSC quali sono i manuali operativi di automazione di Azure per gli script di PowerShell.
In altre parole, Automazione di Azure consente di gestire gli script di PowerShell e le configurazioni DSC.
Per altre informazioni sui vantaggi dell'uso di Configurazione stato di Automazione di Azure, vedere [Panoramica di Configurazione stato di Automazione di Azure](automation-dsc-overview.md).

È possibile usare Configurazione stato di Automazione di Azure per gestire macchine virtuali di diversi tipi:

- Macchine virtuali di Azure
- Macchine virtuali di Azure (classica)
- Computer fisici/virtuali Windows locali o in un cloud diverso da Azure (incluse le istanze di AWS EC2)
- Computer fisici/macchine virtuali Linux locali, in Azure o in un cloud diverso da Azure

Inoltre, se non si è pronti per gestire la configurazione dei computer dal cloud, Configurazione stato di Automazione di Azure può essere usato anche come endpoint solo per i report.
In questo modo è possibile impostare le configurazioni (push) tramite DSC e visualizzare i dettagli dei report in automazione di Azure.

> [!NOTE]
> La gestione di macchine virtuali di Azure con Configurazione stato è inclusa senza costi aggiuntivi se l'estensione DSC per le macchine virtuali installata è successiva alla versione 2.70. Per ulteriori informazioni, vedere la [**pagina dei prezzi di automazione**](https://azure.microsoft.com/pricing/details/automation/).

Le sezioni seguenti descrivono come caricare ogni tipo di computer in Configurazione stato di Automazione di Azure.

> [!NOTE]
>La distribuzione di DSC in un nodo Linux usa la cartella `/tmp` e i moduli come **nxAutomation** vengono temporaneamente scaricati per la verifica prima di installarli nel percorso appropriato. Per assicurarsi che i moduli vengano installati correttamente, l'agente di Log Analytics per Linux necessita dell'autorizzazione di lettura/scrittura per `/tmp` cartella. L'agente di Log Analytics per Linux viene eseguito come utente `omsagent`. 
>
>Per concedere l'autorizzazione di scrittura a `omsagent` utente, eseguire i comandi seguenti: `setfacl -m u:omsagent:rwx /tmp`
>

## <a name="azure-virtual-machines"></a>Macchine virtuali di Azure

Configurazione stato di Automazione di Azure permette di caricare macchine virtuali di Azure in tutta semplicità per la gestione della configurazione, usando il portale di Azure, modelli di Azure Resource Manager o PowerShell. In background e senza che l'amministratore debba connettersi in remoto alla macchina virtuale, l'estensione DSC (Desired State Configuration) per le macchine virtuali di Azure registra la macchina virtuale per Configurazione stato di Automazione di Azure.
Poiché l'estensione DSC per le VM di Azure viene eseguita in modalità asincrona, nella sezione seguente [**Risoluzione dei problemi di caricamento di macchine virtuali di Azure**](#troubleshooting-azure-virtual-machine-onboarding) è disponibile la procedura per tenere traccia dell'avanzamento o risolverne i problemi.

### <a name="azure-portal"></a>Portale di Azure

Nel [portale di Azure](https://portal.azure.com/)passare all'account di Automazione di Azure in cui caricare le macchine virtuali. Nella pagina Configurazione stato della scheda **Nodi** fare clic su **+ Aggiungi**.

Selezionare una macchina virtuale di Azure da caricare.

Se nel computer non è installata l'estensione di stato PowerShell desiderata e tramite il PowerShell e lo stato di alimentazione è "in esecuzione", fare clic su **Connetti**.

In **Registrazione** immettere i [valori di Gestione configurazione locale per PowerShell DSC](/powershell/scripting/dsc/managing-nodes/metaConfig) necessari per il proprio caso d'uso e facoltativamente una configurazione del nodo da assegnare alla VM.

![onboarding](./media/automation-dsc-onboarding/DSC_Onboarding_6.png)

### <a name="azure-resource-manager-templates"></a>Modelli di Gestione risorse di Azure

Le macchine virtuali di Azure possono essere distribuite e caricate in Configurazione stato di Automazione di Azure tramite modelli di Azure Resource Manager. Per un modello di esempio che carica una macchina virtuale esistente nella configurazione dello stato di automazione di Azure, vedere [server gestito dal servizio di configurazione dello stato desiderato](https://azure.microsoft.com/resources/templates/101-automation-configuration/) .
Se si gestisce un set di scalabilità di macchine virtuali, vedere il modello di esempio [configurazione del set di scalabilità di macchine virtuali gestito da automazione di Azure](https://azure.microsoft.com/resources/templates/201-vmss-automation-dsc/).

### <a name="powershell"></a>PowerShell

Il cmdlet [Register-AzAutomationDscNode](/powershell/module/az.automation/register-azautomationdscnode) può essere usato per l'onboarding delle macchine virtuali in Azure tramite PowerShell.
Tuttavia, attualmente viene implementato solo per i computer che eseguono Windows (il cmdlet attiva solo l'estensione Windows).

### <a name="registering-virtual-machines-across-azure-subscriptions"></a>Registrazione di macchine virtuali tra sottoscrizioni di Azure

Il modo migliore per registrare le macchine virtuali da altre sottoscrizioni di Azure consiste nell'usare l'estensione DSC in un modello di distribuzione Azure Resource Manager.
Gli esempi sono disponibili nell'estensione DSC ( [desired state Configuration) con i modelli Azure Resource Manager](https://docs.microsoft.com/azure/virtual-machines/extensions/dsc-template).
Per trovare la chiave di registrazione e l'URL di registrazione da usare come parametri nel modello, vedere la sezione seguente relativa alla [**registrazione sicura**](#secure-registration) .

## <a name="physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azure-including-aws-ec2-instances"></a>Computer fisici/virtuali Windows locali o in un cloud diverso da Azure (incluse le istanze di AWS EC2)

I server Windows in esecuzione in locale o in altri ambienti cloud possono anche essere caricati nella configurazione dello stato di automazione di Azure, purché dispongano dell' [accesso in uscita ad Azure](automation-dsc-overview.md#network-planning):

1. Assicurarsi che nei computer da caricare in Configurazione stato di Automazione di Azure sia installata la versione più recente di [WMF 5](https://aka.ms/wmf5latest).
1. Seguire le istruzioni riportate nella sezione [**Generazione di metaconfigurazioni DSC**](#generating-dsc-metaconfigurations) di seguito per generare una cartella contenente le metaconfigurazioni DSC necessarie.
1. Applicare in remoto la metaconfigurazione di PowerShell DSC ai computer da caricare. **Nel computer dal quale viene eseguito questo comando deve essere installata la versione più recente di [WMF 5](https://aka.ms/wmf5latest)** :

   ```powershell
   Set-DscLocalConfigurationManager -Path C:\Users\joe\Desktop\DscMetaConfigs -ComputerName MyServer1, MyServer2
   ```

1. Se non è possibile applicare le metaconfigurazioni di PowerShell DSC in remoto, copiare la cartella delle metaconfigurazioni dal passaggio 2 in ogni computer da caricare. Chiamare quindi **Set-DscLocalConfigurationManager** localmente in ogni computer da caricare.
1. Usando il portale di Azure o i cmdlet, verificare che i computer da caricare vengano visualizzati come nodi di configurazione dello stato registrati nell'account di automazione di Azure.

## <a name="physicalvirtual-linux-machines-on-premises-or-in-a-cloud-other-than-azure"></a>Computer fisici/virtuali Linux in locale o in un cloud diverso da Azure

I server Linux in esecuzione in locale o in altri ambienti cloud possono anche essere caricati nella configurazione dello stato di automazione di Azure, purché dispongano dell' [accesso in uscita ad Azure](automation-dsc-overview.md#network-planning):

1. Assicurarsi che nei computer da caricare in Configurazione stato di Automazione di Azure sia installata la versione più recente di [PowerShell DSC (Desired State Configuration) per Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux).
2. Se i [valori predefiniti di Gestione configurazione locale per PowerShell DSC](/powershell/scripting/dsc/managing-nodes/metaConfig4) corrispondono al caso d'uso corrente e si vuole caricare computer in modo che **entrambi** eseguano il pull e inviino informazioni a Configurazione stato di Automazione di Azure:

   - In ogni computer Linux da caricare in Configurazione stato di Automazione di Azure usare `Register.py` per l'onboarding tramite i valori predefiniti di Gestione configurazione locale per PowerShell DSC:

     `/opt/microsoft/dsc/Scripts/Register.py <Automation account registration key> <Automation account registration URL>`

   - Per trovare la chiave e l'URL di registrazione per l'account di automazione, vedere la sezione seguente [**Registrazione sicura**](#secure-registration).

     Se i valori predefiniti Configuration Manager locali di PowerShell DSC **non** corrispondono al caso d'uso o si vuole caricare i computer in modo che riportino solo la configurazione dello stato di automazione di Azure, seguire i passaggi 3-6. In caso contrario, andare direttamente al passaggio 6.

3. Seguire le istruzioni riportate di seguito nella sezione [**Generazione di metaconfigurazioni DSC**](#generating-dsc-metaconfigurations) per generare una cartella contenente le metaconfigurazioni DSC necessarie.
4. Applicare in remoto la metaconfigurazione di PowerShell DSC ai computer da caricare:

    ```powershell
    $SecurePass = ConvertTo-SecureString -String '<root password>' -AsPlainText -Force
    $Cred = New-Object System.Management.Automation.PSCredential 'root', $SecurePass
    $Opt = New-CimSessionOption -UseSsl -SkipCACheck -SkipCNCheck -SkipRevocationCheck

    # need a CimSession for each Linux machine to onboard
    $Session = New-CimSession -Credential $Cred -ComputerName <your Linux machine> -Port 5986 -Authentication basic -SessionOption $Opt

    Set-DscLocalConfigurationManager -CimSession $Session -Path C:\Users\joe\Desktop\DscMetaConfigs
    ```

Nel computer dal quale viene eseguito questo comando deve essere installata la versione più recente di [WMF 5](https://aka.ms/wmf5latest) .

1. Se non è possibile applicare le metaconfigurazioni di PowerShell DSC in modalità remota, copiare la metaconfigurazione corrispondente a tale computer dalla cartella nel passaggio 5 nel computer Linux. Chiamare quindi `SetDscLocalConfigurationManager.py` in locale in ogni computer Linux da caricare in Configurazione stato di Automazione di Azure:

   `/opt/microsoft/dsc/Scripts/SetDscLocalConfigurationManager.py -configurationmof <path to metaconfiguration file>`

2. Tramite il portale di Azure o i cmdlet verificare che ora i computer da caricare siano visualizzati come nodi DSC registrati nell'account di Automazione di Azure.

## <a name="generating-dsc-metaconfigurations"></a>Generazione di metaconfigurazioni DSC

Per caricare in modo generico qualsiasi computer nella configurazione dello stato di automazione di Azure, è possibile generare una [metaconfigurazione DSC](/powershell/scripting/dsc/managing-nodes/metaConfig) che indichi all'agente DSC di effettuare il pull da e/o creare report nella configurazione dello stato di automazione di Azure. Le metaconfigurazioni DSC per Configurazione stato di Automazione di Azure possono essere generate tramite una configurazione DSC di PowerShell o i cmdlet di PowerShell per Automazione di Azure.

> [!NOTE]
> Le metaconfigurazioni DSC contengono i segreti necessari per caricare un computer in un account di automazione per la relativa gestione. Assicurarsi di proteggere adeguatamente le metaconfigurazioni DSC create oppure eliminarle dopo l'uso.

### <a name="using-a-dsc-configuration"></a>Uso di una configurazione DSC

1. Aprire VSCode (o l'editor preferito) come amministratore in un computer nell'ambiente locale. Nel computer deve essere installata la versione più recente di [WMF 5](https://aka.ms/wmf5latest) .
1. Copiare localmente lo script seguente. Questo script contiene una configurazione DSC di PowerShell per la creazione delle metaconfigurazioni e un comando per avviare la creazione delle metaconfigurazioni.

> [!NOTE]
> I nomi di configurazione dei nodi di Configurazione stato fanno distinzione tra maiuscole e minuscole nel portale. Se le maiuscole e le minuscole non corrispondono, il nodo non verrà visualizzato nella scheda **Nodi**.

   ```powershell
   # The DSC configuration that will generate metaconfigurations
   [DscLocalConfigurationManager()]
   Configuration DscMetaConfigs
   {
        param
        (
            [Parameter(Mandatory=$True)]
            [String]$RegistrationUrl,

            [Parameter(Mandatory=$True)]
            [String]$RegistrationKey,

            [Parameter(Mandatory=$True)]
            [String[]]$ComputerName,

            [Int]$RefreshFrequencyMins = 30,

            [Int]$ConfigurationModeFrequencyMins = 15,

            [String]$ConfigurationMode = 'ApplyAndMonitor',

            [String]$NodeConfigurationName,

            [Boolean]$RebootNodeIfNeeded= $False,

            [String]$ActionAfterReboot = 'ContinueConfiguration',

            [Boolean]$AllowModuleOverwrite = $False,

            [Boolean]$ReportOnly
        )

        if(!$NodeConfigurationName -or $NodeConfigurationName -eq '')
        {
            $ConfigurationNames = $null
        }
        else
        {
            $ConfigurationNames = @($NodeConfigurationName)
        }

        if($ReportOnly)
        {
            $RefreshMode = 'PUSH'
        }
        else
        {
            $RefreshMode = 'PULL'
        }

        Node $ComputerName
        {
            Settings
            {
                RefreshFrequencyMins           = $RefreshFrequencyMins
                RefreshMode                    = $RefreshMode
                ConfigurationMode              = $ConfigurationMode
                AllowModuleOverwrite           = $AllowModuleOverwrite
                RebootNodeIfNeeded             = $RebootNodeIfNeeded
                ActionAfterReboot              = $ActionAfterReboot
                ConfigurationModeFrequencyMins = $ConfigurationModeFrequencyMins
            }

            if(!$ReportOnly)
            {
            ConfigurationRepositoryWeb AzureAutomationStateConfiguration
                {
                    ServerUrl          = $RegistrationUrl
                    RegistrationKey    = $RegistrationKey
                    ConfigurationNames = $ConfigurationNames
                }

                ResourceRepositoryWeb AzureAutomationStateConfiguration
                {
                ServerUrl       = $RegistrationUrl
                RegistrationKey = $RegistrationKey
                }
            }

            ReportServerWeb AzureAutomationStateConfiguration
            {
                ServerUrl       = $RegistrationUrl
                RegistrationKey = $RegistrationKey
            }
        }
   }

    # Create the metaconfigurations
    # NOTE: DSC Node Configuration names are case sensitive in the portal.
    # TODO: edit the below as needed for your use case
   $Params = @{
        RegistrationUrl = '<fill me in>';
        RegistrationKey = '<fill me in>';
        ComputerName = @('<some VM to onboard>', '<some other VM to onboard>');
        NodeConfigurationName = 'SimpleConfig.webserver';
        RefreshFrequencyMins = 30;
        ConfigurationModeFrequencyMins = 15;
        RebootNodeIfNeeded = $False;
        AllowModuleOverwrite = $False;
        ConfigurationMode = 'ApplyAndMonitor';
        ActionAfterReboot = 'ContinueConfiguration';
        ReportOnly = $False;  # Set to $True to have machines only report to AA DSC but not pull from it
   }

   # Use PowerShell splatting to pass parameters to the DSC configuration being invoked
   # For more info about splatting, run: Get-Help -Name about_Splatting
   DscMetaConfigs @Params
   ```

1. Specificare il codice di registrazione e l'URL per l'account di automazione, nonché i nomi dei computer da caricare. Tutti gli altri parametri sono facoltativi. Per trovare la chiave e l'URL di registrazione per l'account di automazione, vedere la sezione seguente [**Registrazione sicura**](#secure-registration).
1. Se si vuole che i computer inviino informazioni sullo stato DSC a Configurazione stato di Automazione di Azure, ma senza eseguire il pull della configurazione o dei moduli di PowerShell, impostare il parametro **ReportOnly** su true.
1. Eseguire lo script. A questo punto è disponibile una cartella denominata **DscMetaConfigs** nella directory di lavoro, contenente le metaconfigurazioni di PowerShell DSC per i computer da caricare (come amministratore):

    ```powershell
    Set-DscLocalConfigurationManager -Path ./DscMetaConfigs
    ```

### <a name="using-the-azure-automation-cmdlets"></a>Uso dei cmdlet di Automazione di Azure

Se i valori predefiniti di Gestione configurazione locale per PowerShell DSC corrispondono al caso d'uso corrente e si vuole caricare i computer in modo che eseguano il pull e inviino informazioni a Configurazione stato di Automazione di Azure, i cmdlet di Automazione di Azure offrono un metodo semplificato per generare le metaconfigurazioni DSC necessarie:

1. Aprire la console di PowerShell o VSCode come amministratore in un computer nell'ambiente locale.
2. Connettersi ad Azure Resource Manager tramite `Connect-AzAccount`
3. Dall'account di automazione in cui si caricheranno i nodi, scaricare le metaconfigurazioni di PowerShell DSC per i computer da caricare:

   ```powershell
   # Define the parameters for Get-AzAutomationDscOnboardingMetaconfig using PowerShell Splatting
   $Params = @{
       ResourceGroupName = 'ContosoResources'; # The name of the Resource Group that contains your Azure Automation Account
       AutomationAccountName = 'ContosoAutomation'; # The name of the Azure Automation Account where you want a node on-boarded to
       ComputerName = @('web01', 'web02', 'sql01'); # The names of the computers that the meta configuration will be generated for
       OutputFolder = "$env:UserProfile\Desktop\";
   }
   # Use PowerShell splatting to pass parameters to the Azure Automation cmdlet being invoked
   # For more info about splatting, run: Get-Help -Name about_Splatting
   Get-AzAutomationDscOnboardingMetaconfig @Params
   ```

1. A questo punto è disponibile una cartella denominata ***DscMetaConfigs***, contenente le metaconfigurazioni di PowerShell DSC per i computer da caricare (come amministratore):

    ```powershell
    Set-DscLocalConfigurationManager -Path $env:UserProfile\Desktop\DscMetaConfigs
    ```

## <a name="secure-registration"></a>Registrazione sicura

I computer possono essere caricati in modo sicuro in un account di Automazione di Azure tramite il protocollo di registrazione DSC WMF 5, che permette a un nodo DSC di autenticarsi in un server di pull o di report di PowerShell DSC, incluso Configurazione stato di Automazione di Azure. Il nodo si registra al server con un **URL di registrazione**, autenticandosi con una **Chiave di registrazione**. Durante la registrazione, il nodo DSC e il server di pull/report DSC negoziano un certificato univoco per questo nodo da usare per l'autenticazione con il server successivamente alla registrazione. Questo processo impedisce ai nodi caricati di rappresentarsi reciprocamente, ad esempio nel caso di un nodo compromesso e che presenta un comportamento dannoso. Dopo la registrazione, la chiave di registrazione non viene usata di nuovo per l'autenticazione e viene eliminata dal nodo.

È possibile ottenere le informazioni necessarie per il protocollo di registrazione di Configurazione stato da **Chiavi** in **Impostazioni account** nel portale di Azure. Aprire questo pannello facendo clic sull'icona della chiave nel pannello **Informazioni di base** dell'account di automazione.

![Chiavi e URL di Automazione di Azure](./media/automation-dsc-onboarding/DSC_Onboarding_4.png)

- L'URL di registrazione si trova nel campo URL del pannello Gestisci chiavi.
- La chiave di registrazione si trova nel campo Chiave di accesso primaria o Chiave di accesso secondaria del pannello Gestisci chiavi. È possibile usare una delle due chiavi.

Per maggiore sicurezza, le chiavi di accesso primaria e secondaria di un account di Automazione di Azure possono essere rigenerate in qualsiasi momento, nella pagina **Gestisci chiavi**, per evitare l'esecuzione di registrazioni di nodi successive con chiavi precedenti.

## <a name="certificate-expiration-and-re-registration"></a>Scadenza e ripetizione della registrazione del certificato

Dopo la registrazione di un computer come nodo DSC nella configurazione dello stato di automazione di Azure, esistono diversi motivi per cui potrebbe essere necessario ripetere la registrazione del nodo in futuro:

- Per le versioni di Windows Server precedenti a Windows Server 2019, ogni nodo negozia automaticamente un certificato univoco per l'autenticazione che scade dopo un anno. Attualmente, il protocollo di registrazione di PowerShell DSC non può rinnovare automaticamente i certificati quando si avvicina la scadenza, quindi è necessario registrare di nuovo i nodi dopo un anno. Prima di ripetere la registrazione, assicurarsi che ogni nodo esegua Windows Management Framework 5,0 RTM. Se il certificato di autenticazione di un nodo scade e il nodo non viene registrato nuovamente, il nodo non è in grado di comunicare con automazione di Azure ed è contrassegnato "non risponde". La ripetizione della registrazione eseguita 90 giorni o meno dall'ora di scadenza del certificato, o in qualsiasi momento dopo l'ora di scadenza del certificato, comporterà la generazione e l'utilizzo di un nuovo certificato.  Una soluzione a questo problema è inclusa in Windows Server 2019 e versioni successive.
- Per modificare qualsiasi [valore di Gestione configurazione locale per PowerShell DS](/powershell/scripting/dsc/managing-nodes/metaConfig4) impostato durante la registrazione iniziale del nodo, ad esempio ConfigurationMode. Attualmente, i valori degli agenti DSC possono essere modificati solo tramite la ripetizione della registrazione. L'unica eccezione è il valore di Configurazione del nodo assegnato al nodo che può essere modificato direttamente in Automation DSC per Azure.

La ripetizione della registrazione può essere eseguita nello stesso modo in cui è stato registrato inizialmente il nodo, usando uno dei metodi di onboarding descritti in questo documento. Non è necessario annullare la registrazione di un nodo dalla configurazione dello stato di automazione di Azure prima di registrarlo di nuovo.

## <a name="troubleshooting-azure-virtual-machine-onboarding"></a>Risoluzione dei problemi di caricamento di macchine virtuali di Azure

Configurazione stato di Automazione di Azure permette di caricare macchine virtuali Windows di Azure in tutta semplicità per la gestione della configurazione. In background viene usata l'estensione DSC (Desired State Configuration) per le macchine virtuali di Azure per registrare la macchina virtuale con Configurazione stato di Automazione di Azure. Poiché l'estensione DSC (Desired State Configuration) per le VM di Azure viene eseguita in modalità asincrona, può essere importante tenere traccia dell'avanzamento e risolvere i problemi di esecuzione.

> [!NOTE]
> Qualsiasi metodo di onboarding di una macchina virtuale Windows di Azure in Configurazione stato di Automazione di Azure che usa l'estensione DSC (Desired State Configuration) per le macchine virtuali di Azure può richiedere fino a un'ora prima che il nodo appaia come registrato in Automazione di Azure. Questo è dovuto all'installazione di Windows Management Framework 5.0 nella macchina virtuale, necessario per l'onboarding della macchina virtuale in Configurazione stato di Automazione di Azure.

Per risolvere i problemi o visualizzare lo stato dell'estensione DSC per le macchine virtuali di Azure, nel portale di Azure passare alla macchina virtuali da caricare, quindi fare clic su **Estensioni** in **Impostazioni**. Fare quindi clic su **DSC** o **DSCForLinux** a seconda del sistema operativo. Per altri dettagli, è possibile fare clic su **Visualizza stato dettagliato**.

Per altre informazioni sulla risoluzione dei problemi, vedere [risoluzione dei problemi di automazione DSC (Desired state Configuration) di automazione di Azure](./troubleshoot/desired-state-configuration.md).

## <a name="next-steps"></a>Passaggi successivi

- Per iniziare, vedere [Introduzione a Configurazione stato di Automazione di Azure](automation-dsc-getting-started.md)
- Per informazioni sulla compilazione di configurazioni DSC da assegnare ai nodi di destinazione, vedere [Compilazione di configurazioni in Configurazione stato di Automazione di Azure](automation-dsc-compile.md)
- Per informazioni di riferimento sui cmdlet di PowerShell, vedere [Azure Automation State Configuration cmdlets](/powershell/module/az.automation#automation) (Cmdlet per Configurazione stato di Automazione di Azure)
- Per informazioni sui prezzi, vedere [Prezzi di Configurazione stato di Automazione di Azure](https://azure.microsoft.com/pricing/details/automation/)
- Per un esempio dell'uso di Configurazione stato di Automazione di Azure in una pipeline di distribuzione continua, vedere [Continuous Deployment Using Azure Automation State Configuration and Chocolatey](automation-dsc-cd-chocolatey.md) (Distribuzione continua tramite Configurazione stato di Automazione di Azure e Chocolatey)
