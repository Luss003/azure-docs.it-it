---
title: Connect Windows computers to Azure Monitor | Microsoft Docs
description: This article describes how to connect Windows computers hosted in other clouds or on-premises to Azure Monitor with the Log Analytics agent for Windows.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: MGoedtel
ms.author: magoedte
ms.date: 10/07/2019
ms.openlocfilehash: 42183ca7b02ba75b241ee1a83b5a0dc936a8c1c8
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/23/2019
ms.locfileid: "74420428"
---
# <a name="connect-windows-computers-to-azure-monitor"></a>Connect Windows computers to Azure Monitor

In order to monitor and manage virtual machines or physical computers in your local datacenter or other cloud environment with Azure Monitor, you need to deploy the Log Analytics agent (also referred to as the Microsoft Monitoring Agent (MMA)) and configure it to report to one or more Log Analytics workspaces. L'agente supporta anche il ruolo di lavoro ibrido per runbook per Automazione di Azure.  

In un computer Windows monitorato, l'agente viene elencato come il servizio Microsoft Monitoring Agent. Il servizio Microsoft Monitoring Agent raccoglie gli eventi dal file di log e dal registro eventi di Windows, i dati sulle prestazioni e altri dati di telemetria. Even when the agent is unable to communicate with Azure Monitor it reports to, the agent continues to run and queues the collected data on the disk of the monitored computer. Quando la connessione viene ripristinata, il servizio Microsoft Monitoring Agent invia i dati raccolti al servizio.

L'agente può essere installato usando uno dei metodi seguenti. La maggior parte delle installazioni usa una combinazione di questi metodi per installare diversi set di computer, se necessario.  I dettagli sull'uso di ogni metodo sono disponibili più avanti nell'articolo.

* Installazione manuale. Il programma di installazione viene eseguito manualmente nel computer mediante la procedura di installazione guidata, dalla riga di comando o distribuito tramite uno strumento di distribuzione software esistente.
* Configurazione dello stato desiderato (DSC) di Automazione di Azure. Uso di DSC in Automazione di Azure con uno script per i computer Windows già distribuiti nell'ambiente in uso.  
* Script di PowerShell.
* Modello di Resource Manager per le macchine virtuali con Windows in esecuzione in locale in Azure Stack. 

>[!NOTE]
>Azure Security Center (ASC) depends on the Microsoft Monitoring Agent (also referred to as the Log Analytics Windows agent) and will install and configure it to report to a Log Analytics workspace as part of its deployment. ASC includes an automatic provisioning option which enables automatic installation of the Log Analytics Windows agent on all VMs in your subscription and configures it to report to a specific workspace. For more information about this option, see [Enable automatic provisioning of Log Analytics agent](../../security-center/security-center-enable-data-collection.md#auto-provision-mma).
>

If you need to configure the agent to report to more than one workspace, this cannot be performed during initial setup, only afterwards by updating the settings from Control Panel or PowerShell as described in [Adding or removing a workspace](agent-manage.md#adding-or-removing-a-workspace).  

Per comprendere la configurazione supportata, vedere i [sistemi operativi Windows supportati](log-analytics-agent.md#supported-windows-operating-systems) e la [configurazione del firewall di rete](log-analytics-agent.md#network-firewall-requirements).

## <a name="obtain-workspace-id-and-key"></a>Ottenere l'ID e la chiave dell'area di lavoro
Prima di installare l'agente di Log Analytics per Windows, sono necessari l'ID e la chiave dell'area di lavoro per l'area di lavoro Log Analytics.  This information is required during setup from each installation method to properly configure the agent and ensure it can successfully communicate with Azure Monitor in Azure commercial and US Government cloud. 

1. Nel portale di Azure fare clic su **Tutti i servizi**. Nell'elenco delle risorse digitare **Log Analytics**. Non appena si inizia a digitare, l'elenco viene filtrato in base all'input. Selezionare **Log Analytics**.
2. Nell'elenco delle aree di lavoro di Log Analytics selezionare quella che si intende configurare per l'uso come riferimento da parte dell'agente.
3. Selezionare **Impostazioni avanzate**.<br><br> ![Impostazioni avanzate di Log Analytics](media/agent-windows/log-analytics-advanced-settings-01.png)<br><br>  
4. Selezionare **Origini connesse** e quindi **Server Windows**.   
5. Copiare e incollare nell'editor preferito l'**ID area di lavoro** e la **chiave primaria**.    
   
## <a name="configure-agent-to-use-tls-12"></a>Configurare l'agente per usare TLS 1.2
Per configurare, usare il protocollo [TLS 1.2](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#tls-12) per la comunicazione tra l'agente di Windows e il servizio Log Analytics, è possibile seguire la procedura seguente per abilitare prima che l'agente venga installato nella macchina virtuale o in un secondo momento.

>[!NOTE]
>If you are configuring a VM running Windows Server 2008 SP2 x64 to use TLS 1.2, you first need to install the following [SHA-2 code signing support update](https://support.microsoft.com/help/4474419/sha-2-code-signing-support-update) before performing the steps below. 
>

1. Individuare la seguente sottochiave del registro di sistema: **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols**
2. Create a subkey under **Protocols** for TLS 1.2 **HKLM\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2**
3. Creare una sottochiave **Client** sotto la sottochiave della versione di protocollo TLS 1.2 creata in precedenza. For example, **HKLM\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client**.
4. Create the following DWORD values under **HKLM\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client**:

    * **Enabled** [Value = 1]
    * **DisabledByDefault** [Value = 0]  

Configurare .NET Framework 4.6 o versione successiva per supportare la crittografia sicura, dal momento che è disabilitata per impostazione predefinita. La [crittografia avanzata](https://docs.microsoft.com/dotnet/framework/network-programming/tls#schusestrongcrypto) usa protocolli di rete più sicuri, come TLS 1.2 che blocca i protocolli non sicuri. 

1. Individuare la sottochiave del registro di sistema seguente: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\\. NETFramework\v4.0.30319**.  
2. Creare il valore DWORD **SchUseStrongCrypto** sotto questa sottochiave con valore **1**.  
3. Individuare la sottochiave del registro di sistema seguente: **HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\\.NETFramework\v4.0.30319**.  
4. Creare il valore DWORD **SchUseStrongCrypto** sotto questa sottochiave con valore **1**. 
5. Riavviare il sistema per rendere effettive le impostazioni. 

## <a name="install-the-agent-using-setup-wizard"></a>Installare l'agente con l'installazione guidata
The following steps install and configure the Log Analytics agent in Azure and Azure Government cloud by using the setup wizard for the agent on your computer. Per altre informazioni su come configurare l'agente in modo che inoltri segnalazioni a un gruppo di gestione di System Center Operations Manager, vedere [distribuire l'agente di Operations Manager con l'installazione guidata dell'agente](https://docs.microsoft.com/system-center/scom/manage-deploy-windows-agent-manually#to-deploy-the-operations-manager-agent-with-the-agent-setup-wizard).

1. Nell'area di lavoro Log Analytics, dalla pagina **Server Windows** a cui si è passati in precedenza, selezionare la versione appropriata da scaricare in **Scarica agente Windows** a seconda dell'architettura del processore del sistema operativo Windows.   
2. Eseguire il programma di installazione per installare l'agente nel computer in uso.
2. Nella pagina di **benvenuto** fare clic su **Avanti**.
3. Nella pagina **Condizioni di licenza** leggere la licenza e quindi fare clic su **Accetto**.
4. Nella pagina **Cartella di destinazione** modificare o mantenere la cartella di installazione predefinita e quindi fare clic su **Avanti**.
5. Nella pagina **Opzioni di installazione dell'agente** scegliere di connettere l'agente ad Azure Log Analytics e quindi fare clic su **Avanti**.   
6. Nella pagina **Azure Log Analytics** eseguire le operazioni seguenti:
   1. Incollare **ID area di lavoro** e **Chiave dell'area di lavoro (Chiave primaria)** copiati in precedenza.  Se il computer deve fare riferimento a un'area di lavoro Log Analytics nel cloud di Azure per enti pubblici, selezionare **Azure Governo degli Stati Uniti** nell'elenco a discesa **Cloud di Azure**.  
   2. Se il computer deve comunicare tramite un server proxy con il servizio Log Analytics, fare clic su **Avanzate** e specificare l'URL e il numero di porta del server proxy.  Se il server proxy richiede l'autenticazione, digitare il nome utente e la password per l'autenticazione nel server proxy, quindi fare clic su **Avanti**.  
7. Fare clic su **Avanti** dopo aver specificato le impostazioni di configurazione necessarie.<br><br> ![incollare ID area di lavoro e chiave primaria](media/agent-windows/log-analytics-mma-setup-laworkspace.png)<br><br>
8. Nella pagina **Pronto per l'installazione** rivedere le scelte effettuate e quindi fare clic su **Installa**.
9. Nella pagina **Configurazione completata** fare clic su **Fine**.

Al termine, verrà visualizzato **Microsoft Monitoring Agent** nel **Pannello di controllo**. Per confermare che fa riferimento a Log Analytics, rivedere [Verificare la connettività dell'agente a Log Analytics](#verify-agent-connectivity-to-log-analytics). 

## <a name="install-the-agent-using-the-command-line"></a>Installare l'agente usando la riga di comando
Il file scaricato per l'agente è un pacchetto di installazione autonomo.  Il programma di installazione per l'agente e i file di supporto sono contenuti nel pacchetto e devono essere estratti per eseguire correttamente l'installazione tramite la riga di comando mostrata negli esempi seguenti.    

>[!NOTE]
>Per aggiornare un agente è necessario usare l'API di scripting di Log Analytics. Per altre informazioni, vedere l'argomento [Gestione e manutenzione dell'agente di Log Analytics per Windows e Linux](agent-manage.md).

La tabella seguente illustra i parametri specifici supportati dal programma di installazione per l'agente, inclusi quelli distribuiti tramite Automation DSC.

|Opzioni specifiche di MMA                   |Note         |
|---------------------------------------|--------------|
| NOAPM=1                               | Parametro facoltativo. Installa l'agente senza .NET Application Performance Monitoring.|   
|ADD_OPINSIGHTS_WORKSPACE               | 1 = Configura l'agente per fare riferimento a un'area di lavoro                |
|OPINSIGHTS_WORKSPACE_ID                | ID dell'area di lavoro (guid) per l'area di lavoro da aggiungere                    |
|OPINSIGHTS_WORKSPACE_KEY               | Chiave dell'area di lavoro usata per autenticarsi inizialmente nell'area di lavoro |
|OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE  | Specificare l'ambiente cloud in cui si trova l'area di lavoro <br> 0 = Cloud commerciale di Azure (impostazione predefinita) <br> 1 = Azure per enti pubblici |
|OPINSIGHTS_PROXY_URL               | URI per il proxy da usare |
|OPINSIGHTS_PROXY_USERNAME               | Nome utente per accedere a un proxy autenticato |
|OPINSIGHTS_PROXY_PASSWORD               | Password per accedere a un proxy autenticato |

1. Per estrarre i file di installazione dell'agente, da un prompt dei comandi con privilegi elevati eseguire `MMASetup-<platform>.exe /c` e verrà chiesto il percorso in cui estrarre i file.  In alternativa, è possibile specificare il percorso passando gli argomenti `MMASetup-<platform>.exe /c /t:<Full Path>`.  
2. Per installare in modo invisibile l'agente e configurarlo in modo da fare riferimento a un'area di lavoro nel cloud commerciale di Azure, dalla cartella da cui sono stati estratti i file di installazione, digitare: 
   
     ```dos
    setup.exe /qn NOAPM=1 ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=0 OPINSIGHTS_WORKSPACE_ID="<your workspace ID>" OPINSIGHTS_WORKSPACE_KEY="<your workspace key>" AcceptEndUserLicenseAgreement=1
    ```

   o per configurare l'uso come riferimento del cloud per il governo statunitense di Azure, digitare: 

     ```dos
    setup.exe /qn NOAPM=1 ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=1 OPINSIGHTS_WORKSPACE_ID="<your workspace ID>" OPINSIGHTS_WORKSPACE_KEY="<your workspace key>" AcceptEndUserLicenseAgreement=1
    ```
    >[!NOTE]
    >The string values for the parameters *OPINSIGHTS_WORKSPACE_ID* and *OPINSIGHTS_WORKSPACE_KEY* need to be encapsulated in double-quotes to instruct Windows Installer to interprit as valid options for the package. 

## <a name="install-the-agent-using-dsc-in-azure-automation"></a>Installare l'agente usando DSC in Automazione di Azure

È possibile usare lo script di esempio riportato di seguito per installare l'agente usando DSC in Automazione di Azure.   Se non si dispone di un account di Automazione, vedere [Guida introduttiva di automazione di Azure](/azure/automation/) per comprendere i requisiti e la procedura per la creazione di un account di Automazione richiesto per usare Automation DSC.  Se non si ha familiarità con Automation DSC, esaminare [Introduzione ad Automation DSC](../../automation/automation-dsc-getting-started.md).

L'esempio seguente illustra come installare l'agente a 64 bit, identificato dal valore `URI`. È possibile utilizzare anche la versione a 32 bit sostituendo il valore dell'URI. Gli URI per entrambe le versioni sono:

- Agente Windows a 64 bit: https://go.microsoft.com/fwlink/?LinkId=828603
- Agente Windows a 32 bit: https://go.microsoft.com/fwlink/?LinkId=828604


>[!NOTE]
>Questo esempio di procedura e script non supporta l'aggiornamento dell'agente già distribuito in un computer Windows.

Le versioni a 32 e 64 bit del pacchetto agente dispongono di un codice prodotto diverso e le nuove versioni rilasciate dispongono inoltre di un valore univoco.  Il codice prodotto è un GUID che rappresenta l'identificazione principale di un'applicazione o di un prodotto e viene rappresentato dalla proprietà **Codice prodotto** di Windows Installer.  Il valore `ProductId` nello script **MMAgent.ps1** deve corrispondere al codice prodotto dal pacchetto di installazione dell'agente a 32 bit o 64 bit.

Per recuperare direttamente il codice prodotto dal pacchetto di installazione dell'agente, è possibile usare Orca.exe dal [Componenti di Windows SDK per sviluppatori di Windows Installer](https://msdn.microsoft.com/library/windows/desktop/aa370834%28v=vs.85%29.aspx) che è un componente del Windows Software Development Kit o usare PowerShell seguendo uno [script di esempio](https://www.scconfigmgr.com/2014/08/22/how-to-get-msi-file-information-with-powershell/) scritto da un Microsoft Valuable Professional (MVP).  Per entrambi gli approcci, è necessario innanzitutto estrarre il file **MOMagent.msi** dal pacchetto di installazione MMASetup.  Questa operazione è illustrata in precedenza nel primo passaggio nella sezione [Installare l'agente usando la riga di comando](#install-the-agent-using-the-command-line).  

1. Importare il modulo DSC xPSDesiredStateConfiguration da [https://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](https://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) in Automazione di Azure.  
2.  Creare gli asset variabili di Automazione di Azure per *OPSINSIGHTS_WS_ID* e *OPSINSIGHTS_WS_KEY*. Impostare *OPSINSIGHTS_WS_ID* sull'ID dell'area di lavoro Log Analytics e impostare *OPSINSIGHTS_WS_KEY* sulla chiave primaria dell'area di lavoro.
3.  Copiare lo script e salvarlo come MMAgent.ps1.

    ```powershell
    Configuration MMAgent
    {
        $OIPackageLocalPath = "C:\Deploy\MMASetup-AMD64.exe"
        $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
        $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"

        Import-DscResource -ModuleName xPSDesiredStateConfiguration
        Import-DscResource -ModuleName PSDesiredStateConfiguration

        Node OMSnode {
            Service OIService
            {
                Name = "HealthService"
                State = "Running"
                DependsOn = "[Package]OI"
            }

            xRemoteFile OIPackage {
                Uri = "https://go.microsoft.com/fwlink/?LinkId=828603"
                DestinationPath = $OIPackageLocalPath
            }

            Package OI {
                Ensure = "Present"
                Path  = $OIPackageLocalPath
                Name = "Microsoft Monitoring Agent"
                ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
                Arguments = '/C:"setup.exe /qn NOAPM=1 ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
                DependsOn = "[xRemoteFile]OIPackage"
            }
        }
    }

    ```

4. Aggiornare il valore `ProductId` nello script con il codice prodotto estratto dalla versione più recente del pacchetto di installazione dell'agente usando i metodi consigliati in precedenza. 
5. [Importare lo script di configurazione MMAgent.ps1](../../automation/automation-dsc-getting-started.md#importing-a-configuration-into-azure-automation) nell'account di Automazione. 
5. [Assegnare un computer Windows o un nodo](../../automation/automation-dsc-getting-started.md#onboarding-an-azure-vm-for-management-with-azure-automation-state-configuration) alla configurazione. Entro 15 minuti il nodo controlla la configurazione e viene effettuato il push dell'agente al nodo.

## <a name="verify-agent-connectivity-to-log-analytics"></a>Verificare la connettività dell'agente a Log Analytics

Dopo aver completato l'installazione dell'agente, la verifica dell'avvenuta connessione e il riferimento possono essere eseguiti in due modi.  

Dal **Pannello di controllo** del computer, trovare l'elemento **Microsoft Monitoring Agent**.  Selezionarlo e, nella scheda **Azure Log Analytics** l'agente dovrebbe visualizzare un messaggio per segnalare che **Microsoft Monitoring Agent ha eseguito la connessione al servizio Microsoft Operations Management Suite.**<br><br> ![Stato della connessione di MMA a Log Analytics](media/agent-windows/log-analytics-mma-laworkspace-status.png)

You can also perform a simple log query in the Azure portal.  

1. Nel portale di Azure fare clic su **Tutti i servizi**. In the list of resources, type **Azure Monitor**. Non appena si inizia a digitare, l'elenco viene filtrato in base all'input. Select **Azure Monitor**.  
2. Select **Logs** in the menu. 
2. On the Logs pane, in the query field type:  

    ```
    Heartbeat 
    | where Category == "Direct Agent" 
    | where TimeGenerated > ago(30m)  
    ```

Nei risultati della ricerca restituiti, devono venire visualizzati i record di heartbeat per il computer indicanti che è connesso e che invia report al servizio.   

## <a name="next-steps"></a>Passaggi successivi

- Review [Managing and maintaining the Log Analytics agent for Windows and Linux](agent-manage.md) to learn about how to reconfigure, upgrade, or remove the agent from the virtual machine.

- Review [Troubleshooting the Windows agent](agent-windows-troubleshoot.md) if you encounter issues while installing or managing the agent.
