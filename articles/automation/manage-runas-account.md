---
title: Gestire account RunAs di Automazione di Azure
description: Questo articolo descrive come gestire account RunAs con PowerShell o dal portale.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: bobbytreed
ms.author: robreed
ms.date: 05/24/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: fd7e94261d8302224b0e31e5f4ac46978dfa812f
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690869"
---
# <a name="manage-azure-automation-run-as-accounts"></a>Gestire account RunAs di Automazione di Azure

Gli account RunAs in Automazione di Azure offrono l'autenticazione per la gestione delle risorse in Azure con i cmdlet di Azure.

Quando si crea un account RunAs, viene creato un nuovo utente dell'entità servizio in Azure Active Directory, a cui viene assegnato il ruolo Collaboratore a livello della sottoscrizione. Per i runbook che usano i ruoli di lavoro ibridi per runbook nelle macchine virtuali di Azure, è possibile usare [identità gestite per le risorse di Azure](automation-hrw-run-runbooks.md#managed-identities-for-azure-resources) invece degli account RunAs per l'autenticazione con le risorse di Azure.

Esistono due tipi di account RunAs:

* **Account RunAs di Azure** : questo account viene usato per gestire gestione risorse risorse del [modello di distribuzione](../azure-resource-manager/resource-manager-deployment-model.md) .
  * Crea un'applicazione Azure AD da esportare con un certificato autofirmato, crea un account dell'entità servizio per l'applicazione in Azure AD e assegna il ruolo Collaboratore per l'account nella sottoscrizione corrente. È possibile cambiare l'impostazione in Proprietario o in qualsiasi altro ruolo. Per altre informazioni, vedere [Controllo degli accessi in base al ruolo in Automazione di Azure](automation-role-based-access-control.md).
  * Crea un asset di certificato di Automazione denominato *AzureRunAsCertificate* nell'account di Automazione specificato. L'asset di certificato contiene la chiave privata del certificato usata dall'applicazione Azure AD.
  * Crea un asset di connessione di Automazione denominato *AzureRunAsConnection* nell'account di Automazione specificato. L'asset di connessione contiene l'ID applicazione, l'ID tenant, l'ID sottoscrizione e l'identificazione personale del certificato.

* **Account RunAs classico di Azure** : questo account viene usato per gestire le risorse del [modello di distribuzione classica](../azure-resource-manager/resource-manager-deployment-model.md) .
  * Crea un certificato di gestione nella sottoscrizione
  * Crea un asset di certificato di Automazione denominato *AzureClassicRunAsCertificate* nell'account di Automazione specificato. L'asset di certificato contiene la chiave privata del certificato usata dal certificato di gestione.
  * Crea un asset di connessione di Automazione denominato *AzureClassicRunAsConnection* nell'account di Automazione specificato. L'asset di connessione contiene il nome della sottoscrizione, l'ID sottoscrizione e il nome dell'asset di certificato.
  * Deve essere un coamministratore nella sottoscrizione per creare o rinnovare

  > [!NOTE]
  > Le sottoscrizioni Cloud Solution Provider di Azure (Azure CSP) supportano solo il modello Azure Resource Manager, i servizi non Azure Resource Manager non sono disponibili nel programma. Quando si usa una sottoscrizione CSP, l'Account RunAs di Azure non verrà creato. L'Account RunAs di Azure viene comunque creato. Per altre informazioni sulle sottoscrizioni CSP, vedere [Servizi disponibili nelle sottoscrizioni CSP](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services#comments).

  > [!NOTE]
  > L'entità servizio per un account RunAs non dispone delle autorizzazioni per leggere Azure Active Directory per impostazione predefinita. Se si vuole aggiungere autorizzazioni per la lettura o la gestione di Azure Active Directory, è necessario concedere tale autorizzazione nell'entità servizio in **autorizzazioni API**. Per altre informazioni, vedere [aggiungere autorizzazioni per accedere alle API Web](../active-directory/develop/quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

## <a name="permissions"></a>Autorizzazioni per la configurazione degli account RunAs

Per creare o aggiornare un account RunAs, è necessario avere autorizzazioni e privilegi specifici. Un amministratore dell'applicazione in Azure Active Directory e un proprietario in una sottoscrizione possono completare tutte le attività. Nel caso in cui i compiti siano separati, nella tabella seguente è disponibile un elenco delle attività, il relativo cmdlet e le autorizzazioni necessarie:

|Attività|Cmdlet  |Autorizzazioni minime  |Dove impostare le autorizzazioni|
|---|---------|---------|---|
|Creare un'applicazione Azure AD|[New-AzureRmADApplication](/powershell/module/azurerm.resources/new-azurermadapplication)     | Ruolo Sviluppatore di applicazioni<sup>1</sup>        |[Azure Active Directory](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>Home > Azure Active Directory > Registrazioni per l'app |
|Aggiungere una credenziale all'applicazione.|[New-AzureRmADAppCredential](/powershell/module/AzureRM.Resources/New-AzureRmADAppCredential)     | Amministratore dell'applicazione o AMMINISTRATORE GLOBALE<sup>1</sup>         |[Azure Active Directory](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>Home > Azure Active Directory > Registrazioni per l'app|
|Creare e ottenere un'entità servizio di Azure AD|[New-AzureRMADServicePrincipal](/powershell/module/AzureRM.Resources/New-AzureRmADServicePrincipal)</br>[Get-AzureRmADServicePrincipal](/powershell/module/AzureRM.Resources/Get-AzureRmADServicePrincipal)     | Amministratore dell'applicazione o AMMINISTRATORE GLOBALE<sup>1</sup>        |[Azure Active Directory](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>Home > Azure Active Directory > Registrazioni per l'app|
|Assegnare o ottenere il ruolo Controllo degli accessi in base al ruolo per l'entità specificata|[New-AzureRMRoleAssignment](/powershell/module/AzureRM.Resources/New-AzureRmRoleAssignment)</br>[Get-AzureRMRoleAssignment](/powershell/module/AzureRM.Resources/Get-AzureRmRoleAssignment)      | È necessario disporre delle autorizzazioni seguenti:</br></br><code>Microsoft.Authorization/Operations/read</br>Microsoft.Authorization/permissions/read</br>Microsoft.Authorization/roleDefinitions/read</br>Microsoft.Authorization/roleAssignments/write</br>Microsoft.Authorization/roleAssignments/read</br>Microsoft.Authorization/roleAssignments/delete</code></br></br>O essere:</br></br>Proprietario o Amministratore Accesso utenti        | [Sottoscrizione](../role-based-access-control/role-assignments-portal.md)</br>Home > Sottoscrizioni > \<nome della sottoscrizione\> - Controllo di accesso (IAM)|
|Creare o rimuovere un certificato di Automazione|[New-AzureRmAutomationCertificate](/powershell/module/AzureRM.Automation/New-AzureRmAutomationCertificate)</br>[Remove-AzureRmAutomationCertificate](/powershell/module/AzureRM.Automation/Remove-AzureRmAutomationCertificate)     | Collaboratore nel gruppo di risorse         |Gruppo di risorse account di Automazione|
|Creare o rimuovere una connessione di Automazione|[New-AzureRmAutomationConnection](/powershell/module/AzureRM.Automation/New-AzureRmAutomationConnection)</br>[Remove-AzureRmAutomationConnection](/powershell/module/AzureRM.Automation/Remove-AzureRmAutomationConnection)|Collaboratore nel gruppo di risorse |Gruppo di risorse account di Automazione|

<sup>1</sup> Gli utenti non amministratori nel tenant di Azure AD possono [registrare le applicazioni di AD](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions) se l'opzione **Gli utenti possono registrare applicazioni** del tenant di Azure AD nella pagina **Impostazioni utente** è impostata su **Sì**. Se l'impostazione registrazioni per l'app è impostata su **No**, l'utente che esegue questa azione deve essere definito nella tabella precedente.

Se non si è membri dell'istanza di Active Directory della sottoscrizione prima di essere aggiunti al ruolo di **amministratore globale** della sottoscrizione, si viene aggiunti come Guest. In questo caso si riceverà un avviso `You do not have permissions to create…` nella pagina **Aggiungi account di Automazione**. Gli utenti che sono stati aggiunti prima al ruolo di **amministratore globale** possono essere rimossi dall'istanza di Active Directory della sottoscrizione e aggiunti nuovamente per renderli un utente completo in Active Directory. Per verificare questa situazione dal riquadro **Azure Active Directory** nel portale di Azure selezionare **Utenti e gruppi**, **Tutti gli utenti** e, dopo avere selezionato l'utente specifico, selezionare **Profilo**. Il valore dell'attributo **Tipo utente** nel profilo utente non deve essere **Guest**.

## <a name="permissions-classic"></a>Autorizzazioni per configurare gli account RunAs classici

Per configurare o rinnovare gli account RunAs classici, è necessario avere il ruolo di **co-amministratore** a livello di sottoscrizione. Per altre informazioni sulle autorizzazioni classiche, vedere [amministratori della sottoscrizione classica di Azure](../role-based-access-control/classic-administrators.md#add-a-co-administrator).

## <a name="create-a-run-as-account-in-the-portal"></a>Creare un account RunAs nel portale

La procedura descritta in questa sezione consente di aggiornare un account di Automazione di Azure nel portale di Azure. Creare gli account RunAs e RunAs classico separatamente. Se non è necessario gestire le risorse classiche, è sufficiente creare l'account RunAs di Azure.

1. Accedere al Portale di Azure con un account membro del ruolo Amministratori della sottoscrizione e coamministratore della sottoscrizione.
2. Nel portale di Azure fare clic su **Tutti i servizi**. Nell'elenco delle risorse digitare **Automazione**. Non appena si inizia a digitare, l'elenco viene filtrato in base all'input. Selezionare **Account di automazione**.
3. Nella pagina **Account di automazione** selezionare l'account di Automazione dall'elenco.
4. Nel riquadro a sinistra selezionare **Account RunAs** nella sezione **Impostazioni account**.
5. A seconda del tipo di account necessario, selezionare **Account RunAs di Azure** o **Account RunAs classico di Azure**. Dopo aver selezionato **Account RunAs di Azure** o **Account RunAs classico di Azure**, viene visualizzato il riquadro corrispondente. Rivedere le informazioni generali e fare clic su **Crea** per procedere con la creazione dell'account RunAs.
6. Mentre Azure crea l'account RunAs, è possibile tenere traccia dello stato di avanzamento in **Notifiche** dal menu. Viene anche visualizzato un banner che indica che l'account è in fase di creazione. Il processo potrebbe richiedere alcuni minuti.

## <a name="create-run-as-account-using-powershell"></a>Creare un account RunAs tramite PowerShell

## <a name="prerequisites"></a>Prerequisiti

L'elenco seguente include i requisiti per creare un account RunAs in PowerShell:

* Windows 10 o Windows Server 2016 con i moduli di Azure Resource Manager 3.4.1 e versioni successive. Lo script di PowerShell non supporta versioni precedenti di Windows.
* Azure PowerShell 1.0 e versioni successive. Per informazioni su PowerShell 1.0, vedere [come installare e configurare Azure PowerShell](/powershell/azureps-cmdlets-docs).
* Un account di Automazione, a cui viene fatto riferimento come valore per i parametri *–AutomationAccountName* e *-ApplicationDisplayName*.
* Autorizzazioni equivalenti a quanto indicato in [Autorizzazioni per la configurazione degli account RunAs](#permissions)

Per ottenere i valori per i parametri *SubscriptionID*, *ResourceGroup* e *AutomationAccountName*, obbligatori per gli script, seguire questa procedura:

1. Nel portale di Azure fare clic su **Tutti i servizi**. Nell'elenco delle risorse digitare **Automazione**. Non appena si inizia a digitare, l'elenco viene filtrato in base all'input. Selezionare **Account di automazione**.
1. Nella pagina Account di Automazione selezionare l'account di Automazione e quindi in **Impostazioni account** selezionare **Proprietà**.
1. Prendere nota dei valori di **ID sottoscrizione**, **Nome** e **Gruppo di risorse** nella pagina **Proprietà**.

   ![Pagina "Proprietà" dell'account di Automazione](media/manage-runas-account/automation-account-properties.png)

Questo script di PowerShell include il supporto per le configurazioni seguenti:

* Creare un account RunAs usando un certificato autofirmato.
* Creare un account RunAs e un account RunAs classico usando un certificato autofirmato.
* Creare un account RunAs e un account RunAs classico usando un certificato rilasciato dall'autorità di certificazione globale (enterprise).
* Creare un account RunAs e un account RunAs classico usando un certificato autofirmato nel cloud di Azure per enti pubblici.

>[!NOTE]
> Se si seleziona una delle due opzioni per creare l'account RunAs classico, dopo l'esecuzione dello script è necessario caricare il certificato pubblico, con estensione cer, nell'archivio di gestione della sottoscrizione in cui è stato creato l'account di Automazione.

1. Salvare lo script seguente nel computer. Per questo esempio, salvare il file con il nome *New-RunAsAccount.ps1*.

   Lo script usa più cmdlet di Azure Resource Manager per creare le risorse. La tabella delle [autorizzazioni](#permissions) precedente Mostra i cmdlet e le relative autorizzazioni necessarie.

    ```powershell
    #Requires -RunAsAdministrator
    Param (
        [Parameter(Mandatory = $true)]
        [String] $ResourceGroup,

        [Parameter(Mandatory = $true)]
        [String] $AutomationAccountName,

        [Parameter(Mandatory = $true)]
        [String] $ApplicationDisplayName,

        [Parameter(Mandatory = $true)]
        [String] $SubscriptionId,

        [Parameter(Mandatory = $true)]
        [Boolean] $CreateClassicRunAsAccount,

        [Parameter(Mandatory = $true)]
        [String] $SelfSignedCertPlainPassword,

        [Parameter(Mandatory = $false)]
        [string] $EnterpriseCertPathForRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPlainPasswordForRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPathForClassicRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPlainPasswordForClassicRunAsAccount,

        [Parameter(Mandatory = $false)]
        [ValidateSet("AzureCloud", "AzureUSGovernment")]
        [string]$EnvironmentName = "AzureCloud",

        [Parameter(Mandatory = $false)]
        [int] $SelfSignedCertNoOfMonthsUntilExpired = 12
    )

    function CreateSelfSignedCertificate([string] $certificateName, [string] $selfSignedCertPlainPassword,
        [string] $certPath, [string] $certPathCer, [string] $selfSignedCertNoOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My `
            -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
            -NotAfter (Get-Date).AddMonths($selfSignedCertNoOfMonthsUntilExpired) -HashAlgorithm SHA256

        $CertPassword = ConvertTo-SecureString $selfSignedCertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPathCer -Type CERT | Write-Verbose
    }

    function CreateServicePrincipal([System.Security.Cryptography.X509Certificates.X509Certificate2] $PfxCert, [string] $applicationDisplayName) {
        $keyValue = [System.Convert]::ToBase64String($PfxCert.GetRawCertData())
        $keyId = (New-Guid).Guid

        # Create an Azure AD application, AD App Credential, AD ServicePrincipal

        # Requires Application Developer Role, but works with Application administrator or GLOBAL ADMIN
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $keyId)
        # Requires Application administrator or GLOBAL ADMIN
        $ApplicationCredential = New-AzureRmADAppCredential -ApplicationId $Application.ApplicationId -CertValue $keyValue -StartDate $PfxCert.NotBefore -EndDate $PfxCert.NotAfter
        # Requires Application administrator or GLOBAL ADMIN
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id

        # Sleep here for a few seconds to allow the service principal application to become active (ordinarily takes a few seconds)
        Sleep -s 15
        # Requires User Access Administrator or Owner.
        $NewRole = New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6) {
            Sleep -s 10
            New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
            $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
            $Retries++;
        }
        return $Application.ApplicationId.ToString();
    }

    function CreateAutomationCertificateAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $certifcateAssetName, [string] $certPath, [string] $certPlainPassword, [Boolean] $Exportable) {
        $CertPassword = ConvertTo-SecureString $certPlainPassword -AsPlainText -Force
        Remove-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $certifcateAssetName -ErrorAction SilentlyContinue
        New-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Path $certPath -Name $certifcateAssetName -Password $CertPassword -Exportable:$Exportable  | write-verbose
    }

    function CreateAutomationConnectionAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $connectionAssetName, [string] $connectionTypeName, [System.Collections.Hashtable] $connectionFieldValues ) {
        Remove-AzureRmAutomationConnection -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -Force -ErrorAction SilentlyContinue
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -ConnectionTypeName $connectionTypeName -ConnectionFieldValues $connectionFieldValues
    }

    Import-Module AzureRM.Profile
    Import-Module AzureRM.Resources

    $AzureRMProfileVersion = (Get-Module AzureRM.Profile).Version
    if (!(($AzureRMProfileVersion.Major -ge 3 -and $AzureRMProfileVersion.Minor -ge 4) -or ($AzureRMProfileVersion.Major -gt 3))) {
        Write-Error -Message "Please install the latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
        return
    }

    # To use the new Az modules to create your Run As accounts please uncomment the following lines and ensure you comment out the previous 8 lines that import the AzureRM modules to avoid any issues. To learn about about using Az modules in your Automation Account see https://docs.microsoft.com/azure/automation/az-modules

    # Import-Module Az.Automation
    # Enable-AzureRmAlias


    Connect-AzureRmAccount -Environment $EnvironmentName
    $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId

    # Create a Run As account by using a service principal
    $CertifcateAssetName = "AzureRunAsCertificate"
    $ConnectionAssetName = "AzureRunAsConnection"
    $ConnectionTypeName = "AzureServicePrincipal"

    if ($EnterpriseCertPathForRunAsAccount -and $EnterpriseCertPlainPasswordForRunAsAccount) {
        $PfxCertPathForRunAsAccount = $EnterpriseCertPathForRunAsAccount
        $PfxCertPlainPasswordForRunAsAccount = $EnterpriseCertPlainPasswordForRunAsAccount
    }
    else {
        $CertificateName = $AutomationAccountName + $CertifcateAssetName
        $PfxCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".pfx")
        $PfxCertPlainPasswordForRunAsAccount = $SelfSignedCertPlainPassword
        $CerCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".cer")
        CreateSelfSignedCertificate $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
    }

    # Create a service principal
    $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
    $ApplicationId = CreateServicePrincipal $PfxCert $ApplicationDisplayName

    # Create the Automation certificate asset
    CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

    # Populate the ConnectionFieldValues
    $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
    $TenantID = $SubscriptionInfo | Select TenantId -First 1
    $Thumbprint = $PfxCert.Thumbprint
    $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}

    # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
    CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

    if ($CreateClassicRunAsAccount) {
        # Create a Run As account by using a service principal
        $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
        $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
        $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
        $UploadMessage = "Please upload the .cer format of #CERT# to the Management store by following the steps below." + [Environment]::NewLine +
        "Log in to the Microsoft Azure portal (https://portal.azure.com) and select Subscriptions -> Management Certificates." + [Environment]::NewLine +
        "Then click Upload and upload the .cer format of #CERT#"

        if ($EnterpriseCertPathForClassicRunAsAccount -and $EnterpriseCertPlainPasswordForClassicRunAsAccount ) {
            $PfxCertPathForClassicRunAsAccount = $EnterpriseCertPathForClassicRunAsAccount
            $PfxCertPlainPasswordForClassicRunAsAccount = $EnterpriseCertPlainPasswordForClassicRunAsAccount
            $UploadMessage = $UploadMessage.Replace("#CERT#", $PfxCertPathForClassicRunAsAccount)
        }
        else {
            $ClassicRunAsAccountCertificateName = $AutomationAccountName + $ClassicRunAsAccountCertifcateAssetName
            $PfxCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".pfx")
            $PfxCertPlainPasswordForClassicRunAsAccount = $SelfSignedCertPlainPassword
            $CerCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".cer")
            $UploadMessage = $UploadMessage.Replace("#CERT#", $CerCertPathForClassicRunAsAccount)
            CreateSelfSignedCertificate $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate the ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.Name
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName   $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red       $UploadMessage
    }
    ```

    > [!IMPORTANT]
    > **Add-AzureRmAccount** è ora un alias per **Connect-AzureRMAccount**. Quando si esegue la ricerca tra gli elementi della libreria, se **Connect-AzureRMAccount** non viene visualizzato, è possibile usare **Add-AzureRmAccount** oppure [aggiornare i moduli](automation-update-azure-modules.md) nell'account di Automazione.

1. Avviare **Windows PowerShell** con diritti utente elevati nel computer dalla schermata **Start**.
1. Nella shell della riga di comando con privilegi elevati passare alla cartella contenente lo script creato nel passaggio 1.
1. Eseguire lo script usando i valori dei parametri per la configurazione richiesta.

    **Creare un account RunAs usando un certificato autofirmato**

    ```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false
    ```

    **Creare un account RunAs e un account RunAs classico usando un certificato autofirmato**

    ```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true
    ```

    **Creare un account RunAs e un account RunAs classico usando un certificato enterprise**

    ```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>
    ```

    **Creare un account RunAs e un account RunAs classico usando un certificato autofirmato nel cloud di Azure per enti pubblici**

    ```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment
    ```

    > [!NOTE]
    > Dopo l'esecuzione dello script, viene richiesto di autenticarsi con Azure. Accedere con un account membro del ruolo Amministratori della sottoscrizione e coamministratore della sottoscrizione.

Al termine dell'esecuzione dello script, tenere presente quanto segue:

* Se è stato creato un account RunAs classico con un certificato pubblico autofirmato, con estensione cer, lo script lo crea e lo salva nella cartella dei file temporanei del computer con il profilo utente *%USERPROFILE%\AppData\Local\Temp*, usato per eseguire la sessione di PowerShell.

* Se è stato creato un account RunAs classico con un certificato pubblico enterprise, con estensione cer, usare questo certificato. Seguire le istruzioni per il [caricamento di un certificato di gestione API nel portale di Azure](../azure-api-management-certs.md).

## <a name="delete-a-run-as-or-classic-run-as-account"></a>Eliminare un account RunAs o un account RunAs classico

Questa sezione illustra come eliminare e creare nuovamente un account RunAs o un account RunAs classico. Quando si esegue questa azione, l'account di Automazione viene mantenuto. Dopo aver eliminato l'account RunAs o l'account RunAs classico, è possibile ricrearlo nel portale di Azure.

1. Nel Portale di Azure aprire l'account di Automazione.

2. Nella pagina **Account di Automazione** selezionare **Account RunAs**.

3. Nella pagina delle proprietà **Account RunAs** selezionare l'account RunAs o l'account RunAs classico che si vuole eliminare. Nel riquadro **Proprietà** per l'account selezionato fare quindi clic su **Elimina**.

   ![Eliminare un account RunAs](media/manage-runas-account/automation-account-delete-runas.png)

1. Durante l'eliminazione dell'account, è possibile tenere traccia dello stato di avanzamento in **Notifiche** dal menu.

1. Dopo aver eliminato l'account, è possibile crearlo di nuovo nella pagina delle proprietà **Account RunAs** selezionando l'opzione di creazione **Account RunAs di Azure**.

   ![Ricreare l'account RunAs di Automazione](media/manage-runas-account/automation-account-create-runas.png)

## <a name="cert-renewal"></a>Rinnovo del certificato autofirmato

Prima della scadenza dell'account RunAs, è necessario rinnovare il certificato. Se si ritiene che l'account RunAs sia stato compromesso, è possibile eliminarlo e crearlo nuovamente. Questa sezione illustra come eseguire tali operazioni.

Il certificato autofirmato creato per l'account RunAs scade un anno dopo la data di creazione. È possibile rinnovarlo in qualsiasi momento prima della scadenza. Quando si procede al rinnovo, il certificato valido corrente viene mantenuto per evitare di influire negativamente su eventuali runbook messi in coda o in esecuzione, che eseguono l'autenticazione con l'account RunAs. Il certificato rimane valido fino alla relativa data di scadenza.

> [!NOTE]
> Se l'account RunAs di Automazione è stato configurato per l'uso di un certificato rilasciato dall'autorità di certificazione aziendale e si usa questa opzione, il certificato viene sostituito da un certificato autofirmato.

Per rinnovare il certificato, seguire questa procedura:

1. Nel Portale di Azure aprire l'account di Automazione.

1. In **Impostazioni account** selezionare **Account RunAs**.

    ![Riquadro delle proprietà dell'account di Automazione](media/manage-runas-account/automation-account-properties-pane.png)

1. Nella pagina delle proprietà **Account RunAs** selezionare l'account RunAs o l'account RunAs classico per cui si vuole rinnovare il certificato.

1. Nel riquadro **Proprietà** per l'account selezionato fare clic su **Rinnova certificato**.

    ![Rinnovare il certificato per l'account RunAs](media/manage-runas-account/automation-account-renew-runas-certificate.png)

1. Durante il rinnovamento del certificato, è possibile tenere traccia dello stato di avanzamento in **Notifiche** dal menu.

## <a name="auto-cert-renewal"></a>Configurare il rinnovo automatico del certificato con un Runbook di automazione

Per rinnovare automaticamente i certificati, è possibile usare un Runbook di automazione. Lo script seguente in [GitHub](https://github.com/ikanni/PowerShellScripts/blob/master/AzureAutomation/RunAsAccount/GrantPermissionToRunAsAccountAADApplication-ToRenewCertificateItself-CreateSchedule.ps1) Abilita questa funzionalità nell'account di automazione.

- Lo script `GrantPermissionToRunAsAccountAADApplication-ToRenewCertificateItself-CreateSchedule.ps1` crea una pianificazione settimanale per rinnovare i certificati dell'account RunAs.
- Lo script aggiunge un Runbook **Update-AutomationRunAsCredential** all'account di automazione.
  - È anche possibile visualizzare il codice Runbook in GitHub, nello script: [Update-AutomationRunAsCredential. ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AutomationRunAsCredential.ps1).
  - È anche possibile usare il codice PowerShell nel file per rinnovare manualmente i certificati in base alle esigenze.

Per testare immediatamente il processo di rinnovo, attenersi alla procedura seguente:

1. Modificare il Runbook **Update-AutomationRunAsCredential** e inserire un carattere di commento (`#`) alla riga 122, davanti al comando `Exit(1)`, come illustrato di seguito.

   ```powershell
   #Exit(1)
   ```

2. Pubblicare il Runbook.
3. Avviare il Runbook.
4. Verificare il rinnovo completato con il codice seguente:

   ```powershell
   (Get-AzAutomationCertificate -AutomationAccountName TestAA
                                -Name AzureRunAsCertificate
                                -ResourceGroupName TestAutomation).ExpiryTime.DateTime
   ```

   ```Output
   Thursday, November 7, 2019 7:00:00 PM
   ```

5. Dopo il test, modificare il Runbook e rimuovere il carattere di commento aggiunto nel **passaggio 1**.
6. **Pubblicare** il Runbook.

> [!NOTE]
> Per eseguire lo script, è necessario essere un **amministratore globale** o un **amministratore della società** in Azure Active Directory.

## <a name="limiting-run-as-account-permissions"></a>Limitazione delle autorizzazioni dell'account RunAs

Per controllare la destinazione dell'automazione sulle risorse in Azure, è possibile eseguire lo script [Update-AutomationRunAsAccountRoleAssignments. ps1](https://aka.ms/AA5hug8) in PowerShell Gallery per modificare l'entità servizio dell'account RunAs esistente per creare e usare un ruolo personalizzato definizione. Questo ruolo disporrà delle autorizzazioni per tutte le risorse eccetto [Key Vault](https://docs.microsoft.com/azure/key-vault/).

> [!IMPORTANT]
> Dopo aver eseguito lo script di `Update-AutomationRunAsAccountRoleAssignments.ps1`, manuali operativi che accedono all'insieme di credenziali delle credenziali tramite l'uso degli account RunAs non funzionerà più. È necessario esaminare manuali operativi nell'account per le chiamate all'insieme di credenziali delle credenziali di Azure.
>
> Per abilitare l'accesso a un insieme di credenziali delle credenziali da manuali operativi di automazione di Azure, è necessario [aggiungere l'account RunAs alle autorizzazioni dell'](#add-permissions-to-key-vault)insieme di credenziali.

Se è necessario limitare le operazioni che l'entità servizio RunAs può eseguire ulteriormente, è possibile aggiungere altri tipi di risorse al `NotActions` della definizione di ruolo personalizzata. Nell'esempio seguente viene limitato l'accesso ai `Microsoft.Compute`. Se si aggiunge questo oggetto ai **Notacts** della definizione di ruolo, questo ruolo non sarà in grado di accedere alle risorse di calcolo. Per altre informazioni sulle definizioni di ruolo, vedere informazioni sulle [definizioni di ruolo per le risorse di Azure](../role-based-access-control/role-definitions.md).

```powershell
$roleDefinition = Get-AzureRmRoleDefinition -Name 'Automation RunAs Contributor'
$roleDefinition.NotActions.Add("Microsoft.Compute/*")
$roleDefinition | Set-AzureRMRoleDefinition
```

Per determinare se l'entità servizio usata dall'account RunAs si trova nel **collaboratore** o in una definizione di ruolo personalizzata, passare all'account di automazione e in **Impostazioni account**selezionare account **RunAs**  > **account RunAs di Azure**. In **ruolo** è presente la definizione di ruolo utilizzata.

[![](media/manage-runas-account/verify-role.png "Verify the Run As Account role")](media/manage-runas-account/verify-role-expanded.png#lightbox)

Per determinare la definizione di ruolo utilizzata dagli account RunAs di automazione per più sottoscrizioni o account di automazione, è possibile utilizzare lo script [Check-AutomationRunAsAccountRoleAssignments. ps1](https://aka.ms/AA5hug5) nella PowerShell Gallery.

### <a name="add-permissions-to-key-vault"></a>Aggiungere autorizzazioni a Key Vault

Se si vuole consentire ad automazione di Azure di gestire Key Vault e l'entità servizio dell'account RunAs usa una definizione di ruolo personalizzata, è necessario eseguire passaggi aggiuntivi per consentire questo comportamento:

* Concedere le autorizzazioni all'Key Vault
* Impostare i criteri di accesso

È possibile usare lo script [extend-AutomationRunAsAccountRoleAssignmentToKeyVault. ps1](https://aka.ms/AA5hugb) nel PowerShell Gallery per assegnare le autorizzazioni dell'account RunAs all'insieme di credenziali delle chiavi oppure visitare [concedere alle applicazioni l'accesso a un](../key-vault/key-vault-group-permissions-for-apps.md) insieme di credenziali delle chiavi per ulteriori informazioni sulle impostazioni autorizzazioni per l'insieme di credenziali delle credenziali.

## <a name="misconfiguration"></a>Errore di configurazione

Alcuni elementi di configurazione necessari per il corretto funzionamento dell'account RunAs o dell'account RunAs classico potrebbero essere stati eliminati o non essere stati creati correttamente durante la configurazione iniziale. Tali elementi includono:

* Asset del certificato
* Asset di connessione
* Account RunAs rimosso dal ruolo di collaboratore
* Entità servizio o applicazione in Azure AD

In caso di errore di configurazione, l'account di Automazione rileva le modifiche e visualizza lo stato *Incompleto* nel riquadro delle proprietà **Account RunAs** per l'account.

![Stato di configurazione Incompleto dell'account RunAs](media/manage-runas-account/automation-account-runas-incomplete-config.png)

Quando si seleziona l'account RunAs, nel riquadro **Proprietà** dell'account viene visualizzato il messaggio di errore seguente:

```text
The Run As account is incomplete. Either one of these was deleted or not created - Azure Active Directory Application, Service Principal, Role, Automation Certificate asset, Automation Connect asset - or the Thumbprint is not identical between Certificate and Connection. Please delete and then re-create the Run As Account.
```

Per risolvere rapidamente questi problemi relativi all'account RunAs, è possibile eliminare e ricreare l'account.

## <a name="next-steps"></a>Passaggi successivi

* Per altre informazioni sulle entità servizio, vedere [Oggetti applicazione e oggetti entità servizio](../active-directory/develop/app-objects-and-service-principals.md).
* Per altre informazioni sui certificati e i servizi di Azure, vedere [Panoramica sui certificati per i servizi cloud di Azure](../cloud-services/cloud-services-certs-create.md).
