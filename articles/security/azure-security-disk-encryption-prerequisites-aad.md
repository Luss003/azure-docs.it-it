---
title: Abilitare Crittografia dischi di Azure con prerequisiti di app Azure AD (versione precedente)
description: Questo articolo illustra i prerequisiti per l'uso di Crittografia dischi di Azure per le macchine virtuali IaaS.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 09/30/2019
ms.custom: seodec18
ms.openlocfilehash: 07db6c5cecf2b8dec803e73d5775edef3f8906fa
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/02/2019
ms.locfileid: "71828620"
---
# <a name="azure-disk-encryption-prerequisites-previous-release"></a>Prerequisiti di Crittografia dischi di Azure (versione precedente)

**La nuova versione di Crittografia dischi di Azure elimina la necessità di specificare un parametro dell'applicazione Azure AD per abilitare la crittografia dei dischi per le macchine virtuali. Con la nuova versione, non è più necessario specificare le credenziali di Azure AD durante il passaggio di abilitazione della crittografia. Tutte le nuove macchine virtuali devono essere crittografate senza i parametri dell'applicazione Azure AD. Per istruzioni su come abilitare la crittografia dei dischi di macchine virtuali usando la nuova versione, vedere [Prerequisiti di Crittografia dischi di Azure](azure-security-disk-encryption-prerequisites.md). Le macchine virtuali che sono già state crittografate con i parametri dell'applicazione Azure AD sono ancora supportate e dovrebbero continuare a essere gestite con la sintassi di AAD.**

 Questo articolo illustra i prerequisiti per l'uso di Crittografia dischi di Azure. Oltre ai prerequisiti generali, Crittografia dischi di Azure è integrato con [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/) e usa un'applicazione Azure AD per fornire l'autenticazione al fine di gestire le chiavi di crittografia nell'insieme di credenziali delle chiavi. Può anche essere utile usare [Azure PowerShell](/powershell/azure/overview) o l'[interfaccia della riga di comando di Azure](/cli/azure/) per impostare o configurare Key Vault e l'applicazione Azure AD.

Prima di abilitare Crittografia dischi di Azure nelle macchine virtuali IaaS di Azure per gli scenari supportati illustrati nella sezione "Panoramica" dell'articolo [Crittografia dischi di Azure per le macchine virtuali IaaS](azure-security-disk-encryption-overview.md), assicurarsi che i prerequisiti siano soddisfatti. 

> [!WARNING]
> - Alcune indicazioni possono comportare un maggior utilizzo delle risorse di calcolo, rete o dati con un conseguente aumento dei costi di licenza o sottoscrizione. Per creare le risorse in Azure nella aree geografiche supportate, è necessario avere una sottoscrizione di Azure attiva e valida.
> - Se in precedenza è stata usata la [Crittografia dischi di Azure con l'app Azure AD](azure-security-disk-encryption-prerequisites-aad.md) per crittografare la macchina virtuale, sarà necessario continuare a usare questa opzione per crittografare la macchina virtuale. Non è possibile usare [Crittografia dischi di Azure](azure-security-disk-encryption-prerequisites.md) in questa macchina virtuale crittografata perché non è uno scenario supportato, ovvero non è ancora supportato il trasferimento dall'applicazione AAD per questa macchina virtuale. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="supported-operating-systems"></a>Sistemi operativi supportati

### <a name="windows"></a>Windows

- Client Windows: Windows 8 e versioni successive.
- Server Windows: Windows Server 2008 R2 e versioni successive.  
 
> [!NOTE] 
> Windows Server 2008 R2 richiede l'installazione di .NET Framework 4,5 per la crittografia. installarlo da Windows Update con l'aggiornamento facoltativo Microsoft .NET Framework 4.5.2 per sistemi basati su Windows Server 2008 R2 x64 ([KB2901983](https://www.catalog.update.microsoft.com/Search.aspx?q=KB2901983)).  
>  
> Windows Server 2012 R2 Core e Windows Server 2016 Core richiedono l'installazione del componente BdeHdCfg nella macchina virtuale per la crittografia.

### <a name="linux"></a>Linux 

Crittografia dischi di Azure è supportata in un subset di [distribuzioni Linux approvate per Azure](../virtual-machines/linux/endorsed-distros.md), che è a sua volta un subset di tutte le distribuzioni di server Linux possibili.

![Diagramma di Venn di distribuzioni di server Linux che supportano crittografia dischi di Azure](./media/azure-security-disk-encryption-faq/ade-supported-distros.png)

Le distribuzioni di server Linux che non sono approvate da Azure non supportano crittografia dischi di Azure e, di quelle approvate, solo le distribuzioni e le versioni seguenti supportano crittografia dischi di Azure:

| Distribuzione Linux | Versione | Tipo di volume supportato per la crittografia|
| --- | --- |--- |
| Ubuntu | 18,04| Disco del sistema operativo e dati |
| Ubuntu | 16.04| Disco del sistema operativo e dati |
| Ubuntu | 14.04.5</br>[con il kernel ottimizzato per Azure aggiornato alla versione 4.15 o successiva](azure-security-disk-encryption-tsg.md#bkmk_Ubuntu14) | Disco del sistema operativo e dati |
| RHEL | 7,7 | Sistema operativo e disco dati (vedere la nota di seguito) |
| RHEL | 7.6 | Sistema operativo e disco dati (vedere la nota di seguito) |
| RHEL | 7.5 | Sistema operativo e disco dati (vedere la nota di seguito) |
| RHEL | 7.4 | Sistema operativo e disco dati (vedere la nota di seguito) |
| RHEL | 7.3 | Sistema operativo e disco dati (vedere la nota di seguito) |
| RHEL | 7.2 | Sistema operativo e disco dati (vedere la nota di seguito) |
| RHEL | 6.8 | Disco dati (vedere la nota di seguito) |
| RHEL | 6.7 | Disco dati (vedere la nota di seguito) |
| CentOS | 7,7 | Disco del sistema operativo e dati |
| CentOS | 7.6 | Disco del sistema operativo e dati |
| CentOS | 7.5 | Disco del sistema operativo e dati |
| CentOS | 7.4 | Disco del sistema operativo e dati |
| CentOS | 7.3 | Disco del sistema operativo e dati |
| CentOS | 7.2n | Disco del sistema operativo e dati |
| CentOS | 6.8 | Disco dati |
| openSUSE | 42.3 | Disco dati |
| SLES | 12-SP4 | Disco dati |
| SLES | 12-SP3 | Disco dati |

> [!NOTE]
> La nuova implementazione di ADE è supportata per il sistema operativo RHEL e il disco dati per le immagini con pagamento in base al consumo di RHEL7. Crittografia dischi di Azure non è attualmente supportata per le immagini RHEL di tipo BYOS (Bring-Your-Own-Subscription). Per altre informazioni, vedere [crittografia dischi di Azure per Linux](azure-security-disk-encryption-linux.md) .

- Crittografia dischi di Azure richiede che l'insieme di credenziali delle chiavi e le macchine virtuali si trovino nella stessa area e sottoscrizione di Azure. La configurazione delle risorse in aree separate causa un errore nell'attivazione della funzionalità Crittografia dischi di Azure.

#### <a name="additional-prerequisites-for-linux-iaas-vms"></a>Prerequisiti aggiuntivi per le macchine virtuali IaaS Linux 

- Crittografia dischi di Azure richiede che i moduli dm-crypt e VFAT siano presenti nel sistema. La rimozione o la disabilitazione di VFAT dall'immagine predefinita impedisce al sistema di leggere il volume della chiave e ottenere la chiave necessaria per sbloccare i dischi durante i successivi riavvii. I passaggi per la protezione avanzata del sistema che rimuovono il modulo VFAT dal sistema non sono compatibili con crittografia dischi di Azure. 
- Prima di abilitare la crittografia, i dischi dati da crittografare devono essere elencati correttamente in /etc/fstab. Usare un nome del dispositivo a blocchi permanente per questa voce, in quanto i nomi di dispositivo nel formato "dev/sdX" non possono essere considerati affidabili per l'associazione con lo stesso disco tra un riavvio e l'altro, in particolare dopo l'applicazione della crittografia. Per altre informazioni su questo comportamento, vedere: [Risolvere il problema dei nomi di dispositivo nelle macchine virtuali Linux](../virtual-machines/linux/troubleshoot-device-names-problems.md)
- Verificare che le impostazioni /etc/fstab siano configurate correttamente per il montaggio. Per configurare queste impostazioni, eseguire il comando mount -a o riavviare la macchina virtuale e attivare il rimontaggio in questo modo. Al termine, controllare l'output del comando lsblk per verificare che l'unità desiderata sia ancora montata. 
  - Se il file /etc/fstab non monta l'unità in modo corretto prima di abilitare la crittografia; Crittografia dischi di Azure non sarà in grado di montarla correttamente.
  - Il processo di Crittografia dischi di Azure sposterà le informazioni di montaggio da /etc/fstab e le inserirà in un proprio file di configurazione come parte del processo di crittografia. Non allarmarsi per la mancanza della voce da /etc/fstab dopo il completamento della crittografia dell'unità dati.
  - Prima di avviare la crittografia, assicurarsi di arrestare tutti i servizi e i processi che potrebbero scrivere sui dischi dati montati e disabilitarli, in modo che non vengano riavviati automaticamente dopo un riavvio. È possibile che i file vengano aperti in queste partizioni evitando che la procedura di crittografia li rimonta, causando un errore di crittografia. 
  - Dopo il riavvio, il processo di Crittografia dischi di Azure avrà bisogno di tempo per montare i dischi appena crittografati. Non saranno disponibili immediatamente dopo un riavvio. Il processo richiede tempo per iniziare, sbloccare e quindi montare le unità crittografate prima che siano disponibili per l'accesso da parte di altri processi. Questo processo potrebbe richiedere più di un minuto dopo il riavvio, a seconda delle caratteristiche di sistema.

Un esempio dei comandi che è possibile usare per montare i dischi dati e creare le voci /etc/fstab necessarie è disponibile alle [righe 244-248 di questo file di script](https://github.com/ejarvi/ade-cli-getting-started/blob/master/validate.sh#L244-L248). 

## <a name="bkmk_GPO"></a> Rete e Criteri di gruppo

**Per abilitare la funzionalità Crittografia dischi di Azure usando la sintassi precedente dei parametri AAD, le macchine virtuali IaaS devono soddisfare i requisiti di configurazione degli endpoint di rete seguenti:** 
  - Per ottenere un token per la connessione all'insieme di credenziali delle chiavi, è necessario che la macchina virtuale IaaS possa connettersi a un endpoint di Azure Active Directory, \[login.microsoftonline.com\].
  - Per scrivere le chiavi di crittografia nell'insieme di credenziali delle chiavi, è necessario che la macchina virtuale IaaS possa connettersi all'endpoint dell'insieme di credenziali delle chiavi.
  - La VM IaaS deve potersi connettere a un endpoint di archiviazione di Azure che ospita il repository delle estensioni di Azure e a un account di archiviazione di Azure che ospita i file del disco rigido virtuale.
  -  Se i criteri di sicurezza limitano l'accesso dalle macchine virtuali di Azure a Internet, è possibile risolvere l'URI precedente e configurare una regola specifica per consentire la connettività in uscita agli indirizzi IP. Per altre informazioni, vedere [Azure Key Vault protetto da firewall](../key-vault/key-vault-access-behind-firewall.md).
  - In Windows, se TLS 1.0 è stato disabilitato esplicitamente e .NET non è stato aggiornato almeno alla versione 4.6, la seguente modifica del Registro di sistema consentirà a ADE di selezionare una versione TLS più recente:
    
        [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
        "SystemDefaultTlsVersions"=dword:00000001
        "SchUseStrongCrypto"=dword:00000001

        [HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319]
        "SystemDefaultTlsVersions"=dword:00000001
        "SchUseStrongCrypto"=dword:00000001` 
     

 


**Criteri di gruppo:**
 - La soluzione Crittografia dischi di Azure usa la protezione con chiave esterna BitLocker per macchine virtuali IaaS Windows. Per le macchine virtuali aggiunte a un dominio, non eseguire il push di criteri di gruppo che applichino protezioni TPM. Per informazioni sui Criteri di gruppo per consentire BitLocker senza un TPM compatibile, vedere [BitLocker Group Policy Reference](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings#bkmk-unlockpol1) (Informazioni di riferimento sui Criteri di gruppo BitLocker).

-  I criteri di BitLocker nelle macchine virtuali aggiunte a un dominio con criteri di gruppo personalizzati devono includere l'impostazione seguente: [Configurare l'archiviazione utente delle informazioni di ripristino di BitLocker: > consentire la chiave di ripristino a 256 bit](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings). Crittografia dischi di Azure avrà esito negativo quando le impostazioni di Criteri di gruppo personalizzate per BitLocker sono incompatibili. Sulle macchine sprovviste delle corrette impostazioni di criteri, applicare i nuovi criteri, forzare l'aggiornamento dei criteri (gpupdate.exe /force) e, dopodiché, potrebbe essere necessario riavviare.  


## <a name="bkmk_PSH"></a> Azure PowerShell
[Azure PowerShell](/powershell/azure/overview) offre un set di cmdlet che usano il modello [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) per la gestione delle risorse di Azure. È possibile usarlo nel browser con [Azure Cloud Shell](../cloud-shell/overview.md) oppure installarlo nel computer locale seguendo le istruzioni di seguito per usarlo in una sessione di PowerShell. Se è già installato localmente, assicurarsi di usare la versione più recente di Azure PowerShell per configurare crittografia dischi di Azure.

### <a name="install-azure-powershell-for-use-on-your-local-machine-optional"></a>Installare Azure PowerShell per l'uso nel computer locale (facoltativo):  
1. [Installare e configurare Azure PowerShell](/powershell/azure/install-az-ps). 

2. Installare il [modulo Azure Active Directory PowerShell](/powershell/azure/active-directory/install-adv2#installing-the-azure-ad-module). 

     ```powershell
     Install-Module AzureAD
     ```

3. Verificare le versioni installate dei moduli.
      ```powershell
      Get-Module Az -ListAvailable | Select-Object -Property Name,Version,Path
      Get-Module AzureAD -ListAvailable | Select-Object -Property Name,Version,Path
      ```
4. Accedere ad Azure usando il cmdlet [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) .
     
     ```powershell
     Connect-AzAccount
     # For specific instances of Azure, use the -Environment parameter.
     Connect-AzAccount –Environment (Get-AzEnvironment –Name AzureUSGovernment)
    
     <# If you have multiple subscriptions and want to specify a specific one, 
     get your subscription list with Get-AzSubscription and 
     specify it with Set-AzContext.  #>
     Get-AzSubscription
     Set-AzContext -SubscriptionId "xxxx-xxxx-xxxx-xxxx"
     ```

5.  Connettersi ad Azure AD con [Connect-AzureAD](/powershell/module/azuread/connect-azuread).
     
     ```powershell
     Connect-AzureAD
     ```

6. Se necessario, vedere [Introduzione ad Azure PowerShell](/powershell/azure/get-started-azureps) e [AzureAD](/powershell/module/azuread).

## <a name="bkmk_CLI"></a> Interfaccia della riga di comando di Azure

L'[interfaccia della riga di comando di Azure 2.0](/cli/azure) è uno strumento da riga di comando per la gestione delle risorse di Azure. L'interfaccia della riga di comando è progettata per eseguire query sui dati in modo flessibile, supportare operazioni a esecuzione prolungata come processi non bloccanti e semplificare la creazione di script. È possibile usarlo nel browser con [Azure Cloud Shell](../cloud-shell/overview.md) oppure installarlo nel computer locale e usarlo in una sessione di PowerShell.

1. [Installare l'interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli) per l'uso nel computer locale (facoltativo):

2. Verificare la versione installata.
     
     ```azurecli-interactive
     az --version
     ``` 

3. Accedere ad Azure tramite [az login](/cli/azure/authenticate-azure-cli).
     
     ```azurecli
     az login
     
     # If you would like to select a tenant, use: 
     az login --tenant "<tenant>"

     # If you have multiple subscriptions, get your subscription list with az account list and specify with az account set.
     az account list
     az account set --subscription "<subscription name or ID>"
     ```

4. Se necessario, vedere [Introduzione all'interfaccia della riga di comando di Azure 2.0](/cli/azure/get-started-with-azure-cli). 


## <a name="prerequisite-workflow-for-key-vault-and-the-azure-ad-app"></a>Flusso di lavoro dei prerequisiti per Key Vault e l'app Azure AD

Se si ha già familiarità con i prerequisiti di Key Vault e Azure AD per Crittografia dischi di Azure, è possibile usare lo [script di PowerShell per i prerequisiti di Crittografia dischi di Azure](https://raw.githubusercontent.com/Azure/azure-powershell/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1 ). Per altre informazioni sull'uso dello script dei prerequisiti, vedere l'appendice relativa a [crittografia dischi di Azure](azure-security-disk-encryption-appendix.md#bkmk_prereq-script). 

1. Creare un insieme di credenziali delle chiavi. 
2. Configurare un'applicazione Azure AD e un'entità servizio.
3. Configurare i criteri di accesso per l'insieme di credenziali delle chiavi per l'app Azure AD.
4. Impostare i criteri di accesso avanzati per l'insieme di credenziali delle chiavi.
 
## <a name="bkmk_KeyVault"></a> Creare un insieme di credenziali delle chiavi 
La soluzione Crittografia dischi di Azure è integrata con [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) per semplificare il controllo e la gestione delle chiavi di crittografia dei dischi e dei segreti nella sottoscrizione dell'insieme di credenziali delle chiavi. È possibile creare un insieme di credenziali delle chiavi o usarne uno esistente per Crittografia dischi di Azure. Per altre informazioni sugli insiemi di credenziali delle chiavi, vedere [Introduzione all'insieme di credenziali delle chiavi di Azure](../key-vault/key-vault-get-started.md) e [Proteggere l'insieme di credenziali delle chiavi](../key-vault/key-vault-secure-your-key-vault.md). Per creare un insieme di credenziali delle chiavi è possibile usare un modello di Resource Manager, Azure PowerShell o l'interfaccia della riga di comando di Azure. 


>[!WARNING]
>Per assicurarsi che i segreti di crittografia non superino i confini a livello di area, Crittografia dischi di Azure richiede che l'insieme di credenziali delle chiavi e le macchine virtuali si trovino nella stessa area. Creare e usare un insieme di credenziali delle chiavi nella stessa area della macchina virtuale da crittografare. 


### <a name="bkmk_KVPSH"></a> Creare un insieme di credenziali delle chiavi con PowerShell

È possibile creare un insieme di credenziali delle chiavi con Azure PowerShell usando il cmdlet [New-AzKeyVault](/powershell/module/az.keyvault/New-azKeyVault) . Per ulteriori cmdlet per Key Vault, vedere [AZ.](/powershell/module/az.keyvault/)insieme di credenziali. 

1. Se necessario, [connettersi alla sottoscrizione di Azure](azure-security-disk-encryption-appendix.md#bkmk_ConnectPSH). 
2. Creare un nuovo gruppo di risorse, se necessario, con [New-AzResourceGroup](/powershell/module/az.Resources/New-azResourceGroup).  Per elencare data center percorsi, usare [Get-AzLocation](/powershell/module/az.resources/get-azlocation). 
     
     ```azurepowershell-interactive
     # Get-AzLocation 
     New-AzResourceGroup –Name 'MyKeyVaultResourceGroup' –Location 'East US'
     ```

3. Creare un nuovo insieme di credenziali delle chiavi con [New-AzKeyVault](/powershell/module/az.keyvault/New-azKeyVault)
    
      ```azurepowershell-interactive
     New-AzKeyVault -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -Location 'East US'
     ```

4. Prendere nota dei valori restituiti per **Nome dell'insieme di credenziali**, **Nome del gruppo di risorse**, **ID risorsa**, **URI dell'insieme di credenziali**e **ID oggetto** per l'uso successivo, quando si crittograferanno i dischi. 


### <a name="bkmk_KVCLI"></a> Creare un insieme di credenziali delle chiavi con l'interfaccia della riga di comando di Azure
È possibile gestire l'insieme di credenziali delle chiavi tramite l'interfaccia della riga di comando di Azure usando i comandi [az keyvault](/cli/azure/keyvault#commands). Per creare un insieme di credenziali delle chiavi, usare [az keyvault create](/cli/azure/keyvault#az-keyvault-create).

1. Se necessario, [connettersi alla sottoscrizione di Azure](azure-security-disk-encryption-appendix.md#bkmk_ConnectCLI).
2. Se necessario, creare un nuovo gruppo di risorse usando [az group create](/cli/azure/group#az-group-create). Per un elenco delle posizioni, usare [az account list-locations](/cli/azure/account#az-account-list) 
     
     ```azurecli-interactive
     # To list locations: az account list-locations --output table
     az group create -n "MyKeyVaultResourceGroup" -l "East US"
     ```

3. Creare un nuovo insieme di credenziali delle chiavi usando [az keyvault create](/cli/azure/keyvault#az-keyvault-create).
    
     ```azurecli-interactive
     az keyvault create --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --location "East US"
     ```

4. Prendere nota dei valori restituiti per **Nome dell'insieme di credenziali** (nome), **Nome del gruppo di risorse**, **ID risorsa**, (ID), **URI dell'insieme di credenziali** e **ID oggetto** per l'uso successivo. 

### <a name="bkmk_KVRM"></a> Creare un insieme di credenziali delle chiavi con un modello di Resource Manager

È possibile creare un insieme di credenziali delle chiavi usando il [modello di Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create).

1. Nel modello di avvio rapido di Azure fare clic su **Distribuisci in Azure**.
2. Selezionare la sottoscrizione, il gruppo di risorse, la posizione del gruppo di risorse, il nome dell'insieme di credenziali delle chiavi, l'ID oggetto, le note legali e il contratto, quindi fare clic su **Acquista**. 


## <a name="bkmk_ADapp"></a> Configurare un'app Azure AD e un'entità servizio 
Quando è necessario abilitare la crittografia in una macchina virtuale in esecuzione in Azure, Crittografia dischi di Azure genera e scrive le chiavi di crittografia nell'insieme di credenziali delle chiavi. La gestione delle chiavi di crittografia nell'insieme di credenziali delle chiavi richiede l'autenticazione di Azure AD. Creare un'applicazione Azure AD per questo scopo. Ai fini dell'autenticazione, è possibile usare l'autenticazione basata sul segreto client o l'[autenticazione di Azure AD basata sul certificato client](../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md).


### <a name="bkmk_ADappPSH"></a> Configurare un'app Azure AD e un'entità servizio con Azure PowerShell 
Per eseguire i comandi seguenti, recuperare e usare il [modulo di Azure AD per PowerShell](/powershell/azure/active-directory/install-adv2). 

1. Se necessario, [connettersi alla sottoscrizione di Azure](azure-security-disk-encryption-appendix.md#bkmk_ConnectPSH).
2. Usare il cmdlet di PowerShell [New-AzADApplication](/powershell/module/az.resources/new-azadapplication) per creare un'applicazione Azure ad. MyApplicationHomePage e MyApplicationUri possono essere qualsiasi valore desiderato.

     ```azurepowershell
     $aadClientSecret = "My AAD client secret"
     $aadClientSecretSec = ConvertTo-SecureString -String $aadClientSecret -AsPlainText -Force
     $azureAdApplication = New-AzADApplication -DisplayName "My Application Display Name" -HomePage "https://MyApplicationHomePage" -IdentifierUris "https://MyApplicationUri" -Password $aadClientSecretSec
     $servicePrincipal = New-AzADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId
     ```

3. $azureAdApplication.ApplicationId è l'ID client di Azure AD e $aadClientSecret è il segreto client da usare in seguito per abilitare Crittografia dischi di Azure. Proteggere correttamente il segreto client di Azure AD. Eseguendo `$azureAdApplication.ApplicationId` verrà visualizzato l'ID applicazione.


### <a name="bkmk_ADappCLI"></a> Configurare un'app Azure AD e un'entità servizio con l'interfaccia della riga di comando di Azure

È possibile gestire le entità servizio con l'interfaccia della riga di comando di Azure usando i comandi [az ad sp](/cli/azure/ad/sp). Per altre informazioni, vedere [creare un'entità servizio di Azure](/cli/azure/create-an-azure-service-principal-azure-cli).

1. Se necessario, [connettersi alla sottoscrizione di Azure](azure-security-disk-encryption-appendix.md#bkmk_ConnectCLI).
2. Creare una nuova entità servizio.
     
     ```azurecli-interactive
     az ad sp create-for-rbac --name "ServicePrincipalName" --password "My-AAD-client-secret" --skip-assignment 
     ```
3.  Il valore appId restituito è l'ID client di Azure AD usato in altri comandi. È anche il nome SPN che verrà usato per az keyvault set-policy. La password è il segreto client da usare in seguito per abilitare Crittografia dischi di Azure. Proteggere correttamente il segreto client di Azure AD.
 
### <a name="bkmk_ADappRM"></a> Configurare un'app Azure AD e un'entità servizio tramite il portale di Azure
Seguire i passaggi illustrati nell'articolo [Usare il portale per creare un'applicazione Azure Active Directory e un'entità servizio che possano accedere alle risorse](../active-directory/develop/howto-create-service-principal-portal.md) per creare un'applicazione Azure AD. Ognuno dei passaggi elencati di seguito conduce direttamente alla sezione dell'articolo da completare. 

1. [Verificare le autorizzazioni necessarie](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)
2. [Creare un'applicazione Azure Active Directory](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application) 
     - Quando si crea l'applicazione è possibile usare qualsiasi nome e URL di accesso desiderato.
3. [Ottenere l'ID applicazione e la chiave di autenticazione](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in). 
     - La chiave di autenticazione è il segreto client e viene usato come AadClientSecret per set-AzVMDiskEncryptionExtension. 
        - La chiave di autenticazione viene usata dall'applicazione come credenziale per accedere ad Azure AD. Nel portale di Azure il segreto è denominato chiave, ma non ha alcuna relazione con gli insiemi di credenziali delle chiavi. Proteggere il segreto in modo appropriato. 
     - L'ID applicazione verrà usato in un secondo momento come AadClientId per set-AzVMDiskEncryptionExtension e come ServicePrincipalName per set-AzKeyVaultAccessPolicy. 

## <a name="bkmk_KVAP"></a> Configurare i criteri di accesso per l'insieme di credenziali delle chiavi per l'app Azure AD
Per scrivere segreti di crittografia in un insieme di credenziali delle chiavi specificato, Crittografia dischi di Azure deve avere l'ID client e il Segreto client dell'applicazione Azure Active Directory che dispone delle autorizzazioni per scrivere segreti nell'insieme di credenziali delle chiavi. 

> [!NOTE]
> Crittografia dischi di Azure richiede la configurazione di criteri di accesso all'applicazione client di Azure AD, ovvero le autorizzazioni _WrapKey_ e _Set_.

### <a name="bkmk_KVAPPSH"></a> Configurare i criteri di accesso per l'insieme di credenziali delle chiavi per l'app Azure AD con Azure PowerShell
L'applicazione Azure AD deve avere i diritti di accesso alle chiavi o ai segreti nell'insieme di credenziali. Usare il cmdlet [set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) per concedere le autorizzazioni all'applicazione, usando l'ID client, generato al momento della registrazione dell'applicazione, come valore del parametro _– servicePrincipalName_ . Per altre informazioni, vedere il post di blog [Azure Key Vault - Step by Step](https://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx) (Procedura dettagliata per Azure Key Vault). 

1. Se necessario, [connettersi alla sottoscrizione di Azure](azure-security-disk-encryption-appendix.md#bkmk_ConnectPSH).
2. Configurare i criteri di accesso dell'insieme di credenziali delle chiavi per l'applicazione AD con PowerShell.

     ```azurepowershell
     $keyVaultName = 'MySecureVault'
     $aadClientID = 'MyAadAppClientID'
     $KVRGname = 'MyKeyVaultResourceGroup'
     Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $KVRGname
     ```

### <a name="bkmk_KVAPCLI"></a> Configurare i criteri di accesso dell'insieme di credenziali delle chiavi per l'app Azure AD con l'interfaccia della riga di comando di Azure
Usare [az keyvault set-policy](/cli/azure/keyvault#az-keyvault-set-policy) per impostare i criteri di accesso. Per altre informazioni, vedere [Gestire Key Vault tramite l'interfaccia della riga di comando 2.0](../key-vault/key-vault-manage-with-cli2.md#authorizing-an-application-to-use-a-key-or-secret).

1. Se necessario, [connettersi alla sottoscrizione di Azure](azure-security-disk-encryption-appendix.md#bkmk_ConnectCLI).
2. Assegnare all'entità servizio creata tramite l'interfaccia della riga di comando di Azure l'accesso per ottenere i segreti ed eseguire il wrapping delle chiavi con il comando seguente:
 
     ```azurecli-interactive
     az keyvault set-policy --name "MySecureVault" --spn "<spn created with CLI/the Azure AD ClientID>" --key-permissions wrapKey --secret-permissions set
     ```

### <a name="bkmk_KVAPRM"></a> Configurare i criteri di accesso dell'insieme di credenziali delle chiavi per l'app Azure AD con il portale

1. Aprire il gruppo di risorse con l'insieme di credenziali delle chiavi.
2. Selezionare l'insieme di credenziali delle chiavi, passare a **Criteri di accesso**, quindi fare clic su **Aggiungi nuovo**.
3. In **Selezionare un'entità** cercare l'applicazione Azure AD creata e selezionarla. 
4. Per **Autorizzazioni chiave** selezionare **Esegui il wrapping della chiave** in **Operazioni crittografiche**.
5. Per **Autorizzazioni segrete** selezionare **Imposta** in **Operazioni di gestione dei segreti**.
6. Fare clic su **OK** per salvare il criterio di accesso. 

![Operazioni crittografiche di Azure Key Vault - Esegui il wrapping della chiave](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![Autorizzazioni segrete di Azure Key Vault - Imposta](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

## <a name="bkmk_KVper"></a> Impostare i criteri di accesso avanzati per l'insieme di credenziali delle chiavi
La piattaforma Azure deve avere accesso alle chiavi di crittografia o i segreti nell'insieme di credenziali delle chiavi per renderli disponibili alla macchina virtuale per l'avvio e la decrittografia dei volumi. Abilitare la crittografia del disco nell'insieme di credenziali delle chiavi o le distribuzioni avranno esito negativo.  

### <a name="bkmk_KVperPSH"></a> Impostare i criteri di accesso avanzati per l'insieme di credenziali delle chiavi con Azure PowerShell
 Usare il cmdlet PowerShell di Key Vault [set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) per abilitare la crittografia del disco per l'insieme di credenziali delle chiavi.

  - **Abilitare Key Vault per la crittografia dischi:** EnabledForDiskEncryption è obbligatorio per Crittografia dischi di Azure.
      
     ```azurepowershell-interactive 
     Set-AzKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -EnabledForDiskEncryption
     ```

  - **Abilitare Key Vault per la distribuzione, se necessario:** in questo modo si consente al provider di risorse Microsoft.Compute di recuperare segreti da questo insieme di credenziali delle chiavi quando vi viene fatto riferimento durante la creazione di risorse, ad esempio quando si crea una macchina virtuale.

     ```azurepowershell-interactive
      Set-AzKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -EnabledForDeployment
     ```

  - **Abilitare Key Vault per la distribuzione di modelli, se necessario:** in questo modo si consente a Azure Resource Manager di recuperare segreti da questo insieme di credenziali delle chiavi quando vi viene fatto riferimento durante la distribuzione di un modello.

     ```azurepowershell-interactive             
     Set-AzKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -EnabledForTemplateDeployment
     ```

### <a name="bkmk_KVperCLI"></a> Impostare i criteri di accesso avanzati per l'insieme di credenziali delle chiavi con l'interfaccia della riga di comando di Azure
Usare [az keyvault update](/cli/azure/keyvault#az-keyvault-update) per abilitare la crittografia del disco per l'insieme di credenziali delle chiavi. 

 - **Abilitare Key Vault per la crittografia dischi:** È richiesto Enabled-for-disk-encryption. 

     ```azurecli-interactive
     az keyvault update --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --enabled-for-disk-encryption "true"
     ```  

 - **Abilitare Key Vault per la distribuzione, se necessario:** consente alle macchine virtuali di recuperare i certificati archiviati come segreti dall'insieme di credenziali.
     ```azurecli-interactive
     az keyvault update --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --enabled-for-deployment "true"
     ``` 

 - **Abilitare Key Vault per la distribuzione di modelli, se necessario:** consente a Resource Manager di recuperare i segreti dall'insieme di credenziali.
     ```azurecli-interactive  
     az keyvault update --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --enabled-for-template-deployment "true"
     ```


### <a name="bkmk_KVperrm"></a> Impostare i criteri di accesso avanzati per l'insieme di credenziali delle chiavi tramite il portale di Azure

1. Selezionare l'insieme di credenziali delle chiavi, passare a **Criteri di accesso** e **Fare clic per visualizzare i criteri di accesso avanzati**.
2. Selezionare la casella **Abilita l'accesso a Crittografia dischi di Azure per la crittografia dei volumi**.
3. Selezionare **Abilita l'accesso alle macchine virtuali di Azure per la distribuzione** e/o **Abilita l'accesso ad Azure Resource Manager per la distribuzione dei modelli**, se necessario. 
4. Fare clic su **Salva**.

![Criteri di accesso avanzati per l'insieme di credenziali delle chiavi di Azure](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)


## <a name="bkmk_KEK"></a> Configurare una chiave di crittografia della chiave (facoltativo)
Se si vuole usare una chiave di crittografia della chiave (KEK) per un livello aggiuntivo di sicurezza per le chiavi di crittografia, aggiungere una KEK all'insieme di credenziali delle chiavi. Usare il cmdlet [Add-AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey) per creare una chiave di crittografia della chiave nell'insieme di credenziali delle chiavi. È anche possibile importare una chiave di crittografia della chiave dal modulo di protezione hardware di gestione delle chiavi locale. Per altre informazioni, vedere la [documentazione di Key Vault](../key-vault/key-vault-hsm-protected-keys.md). Quando viene specificata una chiave di crittografia della chiave, Crittografia dischi di Azure la usa per eseguire il wrapping dei segreti di crittografia prima di scrivere nell'insieme di credenziali delle chiavi. 

* Quando si generano chiavi, usare un tipo di chiave RSA. Crittografia dischi di Azure non supporta ancora l'uso di chiavi a curva ellittica.

* È necessario applicare il controllo delle versioni agli URL del segreto dell'insieme di credenziali delle chiavi e della chiave di crittografia della chiave. Azure applica questa restrizione relativa al controllo delle versioni. Per informazioni sugli URL del segreto e della chiave di crittografia della chiave validi, vedere gli esempi seguenti:

  * Esempio di URL del segreto valido:  *https://contosovault.vault.azure.net/secrets/EncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Esempio di URL della chiave di crittografia della chiave valido:  *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* Crittografia dischi di Azure non supporta la possibilità di specificare i numeri di porta come parte dei segreti dell'insieme di credenziali delle chiavi e degli URL della chiave di crittografia della chiave. Ecco alcuni esempi di URL di insiemi di credenziali delle chiavi non supportati e supportati:

  * URL dell'insieme di credenziali delle chiavi non accettabile:  *https://contosovault.vault.azure.net:443/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * URL dell'insieme di credenziali delle chiavi accettabile:  *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

### <a name="bkmk_KEKPSH"></a> Configurare una chiave di crittografia della chiave con Azure PowerShell 
Prima di usare lo script di PowerShell, è necessario conoscere i prerequisiti di Crittografia dischi di Azure per comprendere i passaggi nello script. Lo script di esempio potrebbe richiedere modifiche per l'ambiente in uso. Questo script crea tutti i prerequisiti di Crittografia dischi di Azure e crittografa una macchina virtuale IaaS esistente, eseguendo il wrapping della chiave di crittografia del disco tramite una chiave di crittografia della chiave. 

 ```powershell
 # Step 1: Create a new resource group and key vault in the same location.
     # Fill in 'MyLocation', 'MyKeyVaultResourceGroup', and 'MySecureVault' with your values.
     # Use Get-AzLocation to get available locations and use the DisplayName.
     # To use an existing resource group, comment out the line for New-AzResourceGroup
     
     $Loc = 'MyLocation';
     $KVRGname = 'MyKeyVaultResourceGroup';
     $KeyVaultName = 'MySecureVault'; 
     New-AzResourceGroup –Name  $KVRGname –Location $Loc;
     New-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname -Location $Loc;
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName  $KVRGname;
     $KeyVaultResourceId = (Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName  $KVRGname).ResourceId;
     $diskEncryptionKeyVaultUrl = (Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName  $KVRGname).VaultUri;
     
 # Step 2: Create the AD application and service principal.
     # Fill in 'MyAADClientSecret', "<My Application Display Name>", "<https://MyApplicationHomePage>", and "<https://MyApplicationUri>" with your values.
     # MyApplicationHomePage and the MyApplicationUri can be any values you wish.
     
     $aadClientSecret =  'MyAADClientSecret';
     $aadClientSecretSec = ConvertTo-SecureString -String $aadClientSecret -AsPlainText -Force;
     $azureAdApplication = New-AzADApplication -DisplayName "<My Application Display Name>" -HomePage "<https://MyApplicationHomePage>" -IdentifierUris "<https://MyApplicationUri>" -Password $aadClientSecretSec
     $servicePrincipal = New-AzADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId;
     $aadClientID = $azureAdApplication.ApplicationId;
     
 #Step 3: Enable the vault for disk encryption and set the access policy for the Azure AD application.
     
     Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $KVRGname -EnabledForDiskEncryption;
     Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName  $KVRGname;
     
 #Step 4: Create a new key in the key vault with the Add-AzKeyVaultKey cmdlet.
     # Fill in 'MyKeyEncryptionKey' with your value.
     
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     Add-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName -Destination 'Software';
     $keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
     
 #Step 5: Encrypt the disks of an existing IaaS VM
     # Fill in 'MySecureVM' and 'MyVirtualMachineResourceGroup' with your values. 
     
     $VMName = 'MySecureVM';
      $VMRGName = 'MyVirtualMachineResourceGroup';
     Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
```

## <a name="bkmk_Cert"></a> Autenticazione basata su certificati (facoltativo)
Se si vuole usare l'autenticazione basata su certificati, è possibile caricarne uno nell'insieme di credenziali delle chiavi e distribuirlo al client. Prima di usare lo script di PowerShell, è necessario conoscere i prerequisiti di Crittografia dischi di Azure per comprendere i passaggi nello script. Lo script di esempio potrebbe richiedere modifiche per l'ambiente in uso.

     
 ```powershell

 # Fill in "MyKeyVaultResourceGroup", "MySecureVault", and 'MyLocation' ('My location' only if needed)

   $KVRGname = 'MyKeyVaultResourceGroup'
   $KeyVaultName= 'MySecureVault'

   # Create a key vault and set enabledForDiskEncryption property on it. 
   # Comment out the next three lines if you already have an existing key vault enabled for encryption. No need to set 'My location' in this case.

   $Loc = 'MyLocation'
   New-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname -Location $Loc
   Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $KVRGname -EnabledForDiskEncryption

   #Setting some variables with the key vault information 
   $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname
   $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
   $KeyVaultResourceId = $KeyVault.ResourceId

   # Create the Azure AD application and associate the certificate with it. 
   # Fill in "C:\certificates\mycert.pfx", "Password", "<My Application Display Name>", "<https://MyApplicationHomePage>", and "<https://MyApplicationUri>" with your values.
   # MyApplicationHomePage and the MyApplicationUri can be any values you wish

   $CertPath = "C:\certificates\mycert.pfx"
   $CertPassword = "Password"
   $Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($CertPath, $CertPassword)
   $CertValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

   $AzureAdApplication = New-AzADApplication -DisplayName "<My Application Display Name>" -HomePage "<https://MyApplicationHomePage>" -IdentifierUris "<https://MyApplicationUri>" -CertValue $CertValue 
   $ServicePrincipal = New-AzADServicePrincipal -ApplicationId $AzureAdApplication.ApplicationId

   $AADClientID = $AzureAdApplication.ApplicationId
   $aadClientCertThumbprint= $cert.Thumbprint

   Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $KVRGname
   
   # Upload the pfx file to the key vault. 
   # Fill in "MyAADCert".  

   $KeyVaultSecretName = "MyAADCert"
   $FileContentBytes = get-content $CertPath -Encoding Byte
   $FileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
           $JSONObject = @"
           { 
               "data" : "$filecontentencoded", 
               "dataType" : "pfx", 
               "password" : "$CertPassword" 
           } 
"@

   $JSONObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
   $JSONEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

   #Set the secret and set the key vault policy for -EnabledForDeployment

   $Secret = ConvertTo-SecureString -String $JSONEncoded -AsPlainText -Force
   Set-AzKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName -SecretValue $Secret
   Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $KVRGname -EnabledForDeployment

   # Deploy the certificate to the VM
   # Fill in 'MySecureVM' and 'MyVirtualMachineResourceGroup' with your values.

   $VMName = 'MySecureVM'
   $VMRGName = 'MyVirtualMachineResourceGroup'
   $CertUrl = (Get-AzKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName).Id
   $SourceVaultId = (Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGName).ResourceId
   $VM = Get-AzVM -ResourceGroupName $VMRGName -Name $VMName 
   $VM = Add-AzVMSecret -VM $VM -SourceVaultId $SourceVaultId -CertificateStore "My" -CertificateUrl $CertUrl
   Update-AzVM -VM $VM -ResourceGroupName $VMRGName 

   #Enable encryption on the VM using Azure AD client ID and the client certificate thumbprint

   Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $VMName -AadClientID $AADClientID -AadClientCertThumbprint $AADClientCertThumbprint -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId
 ```

## <a name="bkmk_CertKEK"></a> Autenticazione basata su certificati e KEK (facoltativo)

Se si vuole usare l'autenticazione basata su certificati ed eseguire il wrapping della chiave di crittografia con una KEK, si può usare lo script seguente come esempio. Prima di usare lo script di PowerShell, è necessario conoscere tutti i prerequisiti di Crittografia dischi di Azure precedenti per comprendere i passaggi nello script. Lo script di esempio potrebbe richiedere modifiche per l'ambiente in uso.

> [!IMPORTANT]
> L'autenticazione basata su certificato per Azure AD non è attualmente supportata nelle VM Linux.



     
 ```powershell
# Fill in 'MyKeyVaultResourceGroup', 'MySecureVault', and 'MyLocation' (if needed)

   $KVRGname = 'MyKeyVaultResourceGroup'
   $KeyVaultName= 'MySecureVault'

   # Create a key vault and set enabledForDiskEncryption property on it. 
   # Comment out the next three lines if you already have an existing key vault enabled for encryption.

   $Loc = 'MyLocation'
   New-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname -Location $Loc
   Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $KVRGname -EnabledForDiskEncryption

   # Create the Azure AD application and associate the certificate with it.  
   # Fill in "C:\certificates\mycert.pfx", "Password", "<My Application Display Name>", "<https://MyApplicationHomePage>", and "<https://MyApplicationUri>" with your values.
   # MyApplicationHomePage and the MyApplicationUri can be any values you wish

   $CertPath = "C:\certificates\mycert.pfx"
   $CertPassword = "Password"
   $Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($CertPath, $CertPassword)
   $CertValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

   $AzureAdApplication = New-AzADApplication -DisplayName "<My Application Display Name>" -HomePage "<https://MyApplicationHomePage>" -IdentifierUris "<https://MyApplicationUri>" -CertValue $CertValue 
   $ServicePrincipal = New-AzADServicePrincipal -ApplicationId $AzureAdApplication.ApplicationId

   $AADClientID = $AzureAdApplication.ApplicationId
   $aadClientCertThumbprint= $cert.Thumbprint

   ## Give access for setting secrets and wraping keys
   Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $KVRGname

   # Upload the pfx file to the key vault. 
   # Fill in "MyAADCert". 

   $KeyVaultSecretName = "MyAADCert"
   $FileContentBytes = get-content $CertPath -Encoding Byte
   $FileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
           $JSONObject = @"
           { 
               "data" : "$filecontentencoded", 
               "dataType" : "pfx", 
               "password" : "$CertPassword" 
           } 
"@

   $JSONObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
   $JSONEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

   #Set the secret and set the key vault policy for deployment

   $Secret = ConvertTo-SecureString -String $JSONEncoded -AsPlainText -Force
   Set-AzKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName -SecretValue $Secret
   Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $KVRGname -EnabledForDeployment

   #Setting some variables with the key vault information and generating a KEK 
   # FIll in 'KEKName'
   
   $KEKName ='KEKName'
   $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname
   $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
   $KeyVaultResourceId = $KeyVault.ResourceId
   $KEK = Add-AzKeyVaultKey -VaultName $KeyVaultName -Name $KEKName -Destination "Software"
   $KeyEncryptionKeyUrl = $KEK.Key.kid



   # Deploy the certificate to the VM
   # Fill in 'MySecureVM' and 'MyVirtualMachineResourceGroup' with your values.

   $VMName = 'MySecureVM';
   $VMRGName = 'MyVirtualMachineResourceGroup';
   $CertUrl = (Get-AzKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName).Id
   $SourceVaultId = (Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGName).ResourceId
   $VM = Get-AzVM -ResourceGroupName $VMRGName -Name $VMName 
   $VM = Add-AzVMSecret -VM $VM -SourceVaultId $SourceVaultId -CertificateStore "My" -CertificateUrl $CertUrl
   Update-AzVM -VM $VM -ResourceGroupName $VMRGName

   #Enable encryption on the VM using Azure AD client ID and the client certificate thumbprint

   Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $VMName -AadClientID $AADClientID -AadClientCertThumbprint $AADClientCertThumbprint -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId
```

 
## <a name="next-steps"></a>Passaggi successivi
> [!div class="nextstepaction"]
> [Abilitare Crittografia dischi di Azure per Windows](azure-security-disk-encryption-windows-aad.md)

> [!div class="nextstepaction"]
> [Abilitare Crittografia dischi di Azure per Linux](azure-security-disk-encryption-linux-aad.md)
