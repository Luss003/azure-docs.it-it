---
title: Abilitare servizi di dominio Azure DS usando un modello | Microsoft Docs
description: Informazioni su come configurare e abilitare Azure Active Directory Domain Services usando un modello di Azure Resource Manager
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 01/14/2020
ms.author: iainfou
ms.openlocfilehash: 2daadb539bc08df37f15c187866b735e45309288
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/26/2020
ms.locfileid: "77612786"
---
# <a name="create-an-azure-active-directory-domain-services-managed-domain-using-an-azure-resource-manager-template"></a>Creare un Azure Active Directory Domain Services dominio gestito usando un modello di Azure Resource Manager

Azure Active Directory Domain Services (Azure AD DS) offre servizi di dominio gestiti, come l'aggiunta a un dominio, Criteri di gruppo, LDAP e l'autenticazione Kerberos/NTLM, completamente compatibili con Windows Server Active Directory. È possibile utilizzare questi servizi di dominio senza distribuire, gestire e applicare patch manualmente ai controller di dominio. Azure AD DS si integra con il tenant di Azure AD esistente. Questa integrazione consente agli utenti di accedere con le proprie credenziali aziendali ed è possibile usare i gruppi e gli account utente esistenti per proteggere l'accesso alle risorse.

Questo articolo illustra come abilitare Azure AD DS usando un modello di Azure Resource Manager. Le risorse di supporto vengono create utilizzando Azure PowerShell.

## <a name="prerequisites"></a>Prerequisiti

Per completare questo articolo, sono necessarie le risorse seguenti:

* Installare e configurare Azure PowerShell.
    * Se necessario, seguire le istruzioni per [installare il modulo Azure PowerShell e connettersi alla sottoscrizione di Azure](/powershell/azure/install-az-ps).
    * Assicurarsi di accedere alla sottoscrizione di Azure usando il cmdlet [Connect-AzAccount][Connect-AzAccount] .
* Installare e configurare Azure AD PowerShell.
    * Se necessario, seguire le istruzioni per [installare il modulo Azure ad PowerShell e connettersi al Azure ad](/powershell/azure/active-directory/install-adv2).
    * Assicurarsi di accedere al tenant di Azure AD usando il cmdlet [Connect-AzureAD][Connect-AzureAD] .
* Per abilitare Azure AD DS, sono necessari privilegi di *amministratore globale* nel tenant di Azure AD.
* Per creare le risorse di Azure AD DS richieste, sono necessari privilegi di *collaboratore* nella sottoscrizione di Azure.

## <a name="dns-naming-requirements"></a>Requisiti per i nomi DNS

Quando si crea un'istanza di Azure AD DS, si specifica un nome DNS. Di seguito sono riportate alcune considerazioni per la scelta di questo nome DNS:

* **Nome di dominio predefinito:** Per impostazione predefinita, viene usato il nome di dominio predefinito della directory (suffisso *. onmicrosoft.com* ). Se si vuole abilitare l'accesso LDAP sicuro al dominio gestito tramite Internet, non è possibile creare un certificato digitale per proteggere la connessione con il dominio predefinito. Microsoft è proprietaria del dominio *.onmicrosoft.com*, quindi un'autorità di certificazione (CA) pubblica non emetterà un certificato.
* **Nomi di dominio personalizzati:** L'approccio più comune è quello di specificare un nome di dominio personalizzato, in genere uno di cui si è già proprietari ed è instradabile. Se si usa un dominio personalizzato instradabile, il traffico può fluire correttamente in base alle esigenze per supportare le applicazioni.
* **Suffissi di dominio non instradabili:** È in genere consigliabile evitare un suffisso del nome di dominio non instradabile, ad esempio *contoso. local*. Il suffisso *.local* non è instradabile e può causare problemi con la risoluzione DNS.

> [!TIP]
> Se si crea un nome di dominio personalizzato, prestare attenzione agli spazi dei nomi DNS esistenti. È consigliabile usare un nome di dominio separato da uno spazio dei nomi DNS locale o di Azure esistente.
>
> Se, ad esempio, si dispone di uno spazio dei nomi DNS esistente di *contoso.com*, creare un dominio gestito di Azure AD DS con il nome di dominio personalizzato *aaddscontoso.com*. Se è necessario usare il protocollo LDAP sicuro, è necessario registrarsi e denominare il nome di dominio personalizzato per generare i certificati necessari.
>
> Potrebbe essere necessario creare alcuni record DNS aggiuntivi per altri servizi nell'ambiente in uso o i server d'inoltri DNS condizionali tra gli spazi dei nomi DNS esistenti nell'ambiente in uso. Ad esempio, se si esegue un server Web che ospita un sito con il nome DNS radice, possono essere presenti conflitti di denominazione che richiedono voci DNS aggiuntive.
>
> In queste esercitazioni e articoli sulle procedure viene usato come breve esempio il dominio personalizzato di *aaddscontoso.com* . In tutti i comandi specificare il proprio nome di dominio.

Si applicano anche le seguenti restrizioni relative ai nomi DNS:

* **Restrizioni del prefisso di dominio:** Non è possibile creare un dominio gestito con un prefisso più lungo di 15 caratteri. Il prefisso del nome di dominio specificato, ad esempio *aaddscontoso* nel nome di dominio *aaddscontoso.com* , deve contenere un massimo di 15 caratteri.
* **Conflitti di nomi di rete:** Il nome di dominio DNS per il dominio gestito non dovrebbe essere già presente nella rete virtuale. In particolare, verificare i seguenti scenari che potrebbero causare un conflitto di nomi:
    * È già presente un dominio di Active Directory con lo stesso nome di dominio DNS nella rete virtuale.
    * La rete virtuale in cui si intende abilitare il dominio gestito ha una connessione VPN alla rete locale. In questo caso, verificare che non sia presente un dominio con lo stesso nome di dominio DNS nella rete locale.
    * Esiste un servizio cloud di Azure con lo stesso nome della rete virtuale di Azure.

## <a name="create-required-azure-ad-resources"></a>Creare risorse di Azure AD richieste

Azure AD DS richiede un'entità servizio e un gruppo di Azure AD. Queste risorse consentono al dominio gestito di Azure AD DS di sincronizzare i dati e di definire quali utenti dispongono di autorizzazioni amministrative nel dominio gestito.

Per prima cosa, registrare il provider di risorse Azure AD Domain Services usando il cmdlet [Register-AzResourceProvider][Register-AzResourceProvider] :

```powershell
Register-AzResourceProvider -ProviderNamespace Microsoft.AAD
```

Creare un'entità servizio Azure AD usando il cmdlet [New-AzureADServicePrincipal][New-AzureADServicePrincipal] per Azure AD DS per comunicare ed eseguire l'autenticazione. Un ID applicazione specifico viene usato denominato *domain controller Services* con ID *2565bd9d-DA50-47d4-8B85-4c97f669dc36*. Non modificare questo ID applicazione.

```powershell
New-AzureADServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
```

A questo punto, creare un gruppo di Azure AD denominato *AAD DC Administrators* usando il cmdlet [New-AzureADGroup][New-AzureADGroup] . Agli utenti aggiunti a questo gruppo vengono concesse le autorizzazioni per eseguire attività amministrative nel dominio gestito Azure AD DS.

```powershell
New-AzureADGroup -DisplayName "AAD DC Administrators" `
  -Description "Delegated group to administer Azure AD Domain Services" `
  -SecurityEnabled $true -MailEnabled $false `
  -MailNickName "AADDCAdministrators"
```

Con il gruppo di *amministratori di AAD DC* creato, aggiungere un utente al gruppo usando il cmdlet [Add-AzureADGroupMember][Add-AzureADGroupMember] . Per prima cosa, è necessario ottenere l'ID oggetto gruppo *amministratori di AAD DC* usando il cmdlet [Get-AzureADGroup][Get-AzureADGroup] , quindi l'ID oggetto dell'utente desiderato usando il cmdlet [Get-AzureADUser][Get-AzureADUser] .

Nell'esempio seguente, l'ID oggetto utente per l'account con UPN `admin@aaddscontoso.onmicrosoft.com`. Sostituire questo account utente con l'UPN dell'utente che si vuole aggiungere al gruppo di *amministratori di AAD DC* :

```powershell
# First, retrieve the object ID of the newly created 'AAD DC Administrators' group.
$GroupObjectId = Get-AzureADGroup `
  -Filter "DisplayName eq 'AAD DC Administrators'" | `
  Select-Object ObjectId

# Now, retrieve the object ID of the user you'd like to add to the group.
$UserObjectId = Get-AzureADUser `
  -Filter "UserPrincipalName eq 'admin@aaddscontoso.onmicrosoft.com'" | `
  Select-Object ObjectId

# Add the user to the 'AAD DC Administrators' group.
Add-AzureADGroupMember -ObjectId $GroupObjectId.ObjectId -RefObjectId $UserObjectId.ObjectId
```

Infine, creare un gruppo di risorse usando il cmdlet [New-AzResourceGroup][New-AzResourceGroup] . Nell'esempio seguente, il gruppo di risorse è denominato *myResourceGroup* e viene creato nell'area *westus* . Usare il proprio nome e l'area desiderata:

```powershell
New-AzResourceGroup `
  -Name "myResourceGroup" `
  -Location "WestUS"
```

Se si sceglie un'area che supporta le zone di disponibilità, le risorse di Azure AD DS vengono distribuite in più zone per garantire maggiore ridondanza. Le zone di disponibilità sono località fisiche esclusive all'interno di un'area di Azure. Ogni zona è costituita da uno o più data center dotati di impianti indipendenti per l'alimentazione, il raffreddamento e la connettività di rete. Per garantire la resilienza, sono presenti almeno tre zone separate in tutte le aree abilitate.

Non è necessario eseguire alcuna operazione di configurazione per distribuire Azure AD DS in più zone. La piattaforma Azure gestisce automaticamente la distribuzione delle risorse nelle zone. Per altre informazioni e per vedere la pagina relativa alla disponibilità delle aree, vedere informazioni sulle [zone di disponibilità in Azure][availability-zones].

## <a name="resource-definition-for-azure-ad-ds"></a>Definizione di risorsa per Azure AD DS

Come parte della definizione della risorsa Gestione risorse, sono necessari i seguenti parametri di configurazione:

| Parametro               | Valore |
|-------------------------|---------|
| domainName              | Nome di dominio DNS per il dominio gestito, prendendo in considerazione i punti precedenti sui prefissi e i conflitti di denominazione. |
| filteredSync            | Azure AD DS consente di sincronizzare *tutti* gli utenti e i gruppi disponibili in Azure AD oppure di eseguire una sincronizzazione *con ambito* solo di gruppi specifici. Se si sceglie di sincronizzare tutti gli utenti e i gruppi, non sarà più possibile scegliere di eseguire solo una sincronizzazione con ambito.<br /> Per altre informazioni sulla sincronizzazione con ambito, vedere [Sincronizzazione con ambito in Azure AD Domain Services][scoped-sync].|
| notificationSettings    | Se sono presenti avvisi generati nel dominio gestito di Azure AD DS, è possibile inviare notifiche tramite posta elettronica. <br />Gli *amministratori globali* del tenant di Azure e i membri del gruppo *AAD DC Administrators* possono essere *abilitati* per queste notifiche.<br /> Se lo si desidera, è possibile aggiungere altri destinatari per le notifiche quando sono presenti avvisi che richiedono attenzione.|
| domainConfigurationType | Per impostazione predefinita, viene creato un dominio gestito di Azure AD DS come foresta *Utente*. Questo tipo di foresta sincronizza tutti gli oggetti di Azure AD, inclusi tutti gli account utente creati in un ambiente AD DS locale. Non è necessario specificare un valore *domainConfiguration* per creare una foresta utente.<br /> Una foresta *Risorsa* sincronizza solo gli utenti e i gruppi creati direttamente in Azure AD. Le foreste Risorsa sono attualmente disponibili in anteprima. Impostare il valore su *ResourceTrusting* per creare una foresta di risorse.<br />Per altre informazioni sulle foreste *Risorsa*, inclusi i motivi per cui usarle e come creare trust tra foreste con domini di AD DS locali, vedere [Panoramica delle foreste di risorse di Azure AD DS][resource-forests].|

La definizione dei parametri condensati seguente mostra come vengono dichiarati questi valori. Una foresta di utenti denominata *aaddscontoso.com* viene creata con tutti gli utenti di Azure ad sincronizzati con il dominio gestito Azure AD DS:

```json
"parameters": {
    "domainName": {
        "value": "aaddscontoso.com"
    },
    "filteredSync": {
        "value": "Disabled"
    },
    "notificationSettings": {
        "value": {
            "notifyGlobalAdmins": "Enabled",
            "notifyDcAdmins": "Enabled",
            "additionalRecipients": []
        }
    },
    [...]
}
```

Il seguente tipo di risorsa modello di Gestione risorse condensato viene quindi utilizzato per definire e creare il dominio gestito Azure AD DS. È necessario che esista già una rete virtuale e una subnet di Azure o che sia stata creata come parte del modello Gestione risorse. Il dominio gestito di Azure AD DS è connesso a questa subnet.

```json
"resources": [
    {
        "apiVersion": "2017-06-01",
        "type": "Microsoft.AAD/DomainServices",
        "name": "[parameters('domainName')]",
        "location": "[parameters('location')]",
        "dependsOn": [
            "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
        ],
        "properties": {
            "domainName": "[parameters('domainName')]",
            "subnetId": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', parameters('vnetName'), '/subnets/', parameters('subnetName'))]",
            "filteredSync": "[parameters('filteredSync')]",
            "notificationSettings": "[parameters('notificationSettings')]"
        }
    },
    [...]
]
```

Questi parametri e tipo di risorsa possono essere usati come parte di un modello di Gestione risorse più ampio per distribuire un dominio gestito, come illustrato nella sezione seguente.

## <a name="create-a-managed-domain-using-sample-template"></a>Creare un dominio gestito usando un modello di esempio

Il modello di esempio completo Gestione risorse seguente consente di creare un dominio gestito Azure AD DS e le regole del gruppo di sicurezza di rete, subnet e rete virtuale di supporto. Le regole del gruppo di sicurezza di rete sono necessarie per proteggere il dominio gestito e verificare che il traffico possa fluire correttamente. Viene creata una foresta di utenti con il nome DNS *aaddscontoso.com* con tutti gli utenti sincronizzati da Azure ad:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "apiVersion": {
            "value": "2017-06-01"
        },
        "domainConfigurationType": {
            "value": "FullySynced"
        },
        "domainName": {
            "value": "aaddscontoso.com"
        },
        "filteredSync": {
            "value": "Disabled"
        },
        "location": {
            "value": "westus"
        },
        "notificationSettings": {
            "value": {
                "notifyGlobalAdmins": "Enabled",
                "notifyDcAdmins": "Enabled",
                "additionalRecipients": []
            }
        },
        "subnetName": {
            "value": "aadds-subnet"
        },
        "vnetName": {
            "value": "aadds-vnet"
        },
        "vnetAddressPrefixes": {
            "value": [
                "10.1.0.0/24"
            ]
        },
        "subnetAddressPrefix": {
            "value": "10.1.0.0/24"
        },
        "nsgName": {
            "value": "aadds-nsg"
        }
    },
    "resources": [
        {
            "apiVersion": "2017-06-01",
            "type": "Microsoft.AAD/DomainServices",
            "name": "[parameters('domainName')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
            ],
            "properties": {
                "domainName": "[parameters('domainName')]",
                "subnetId": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', parameters('vnetName'), '/subnets/', parameters('subnetName'))]",
                "filteredSync": "[parameters('filteredSync')]",
                "domainConfigurationType": "[parameters('domainConfigurationType')]",
                "notificationSettings": "[parameters('notificationSettings')]"
            }
        },
        {
            "type": "Microsoft.Network/NetworkSecurityGroups",
            "name": "[parameters('nsgName')]",
            "location": "[parameters('location')]",
            "properties": {
                "securityRules": [
                    {
                        "name": "AllowSyncWithAzureAD",
                        "properties": {
                            "access": "Allow",
                            "priority": 101,
                            "direction": "Inbound",
                            "protocol": "Tcp",
                            "sourceAddressPrefix": "AzureActiveDirectoryDomainServices",
                            "sourcePortRange": "*",
                            "destinationAddressPrefix": "*",
                            "destinationPortRange": "443"
                        }
                    },
                    {
                        "name": "AllowPSRemoting",
                        "properties": {
                            "access": "Allow",
                            "priority": 301,
                            "direction": "Inbound",
                            "protocol": "Tcp",
                            "sourceAddressPrefix": "AzureActiveDirectoryDomainServices",
                            "sourcePortRange": "*",
                            "destinationAddressPrefix": "*",
                            "destinationPortRange": "5986"
                        }
                    },
                    {
                        "name": "AllowRD",
                        "properties": {
                            "access": "Allow",
                            "priority": 201,
                            "direction": "Inbound",
                            "protocol": "Tcp",
                            "sourceAddressPrefix": "CorpNetSaw",
                            "sourcePortRange": "*",
                            "destinationAddressPrefix": "*",
                            "destinationPortRange": "3389"
                        }
                    }
                ]
            },
            "apiVersion": "2018-04-01"
        },
        {
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('vnetName')]",
            "location": "[parameters('location')]",
            "apiVersion": "2018-04-01",
            "dependsOn": [
                "[concat('Microsoft.Network/NetworkSecurityGroups/', parameters('nsgName'))]"
            ],
            "properties": {
                "addressSpace": {
                    "addressPrefixes": "[parameters('vnetAddressPrefixes')]"
                },
                "subnets": [
                    {
                        "name": "[parameters('subnetName')]",
                        "properties": {
                            "addressPrefix": "[parameters('subnetAddressPrefix')]",
                            "networkSecurityGroup": {
                                "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/NetworkSecurityGroups/', parameters('nsgName'))]"
                            }
                        }
                    }
                ]
            }
        }
    ],
    "outputs": {}
}
```

Questo modello può essere distribuito usando il metodo di distribuzione preferito, ad esempio [portale di Azure][portal-deploy], [Azure PowerShell][powershell-deploy]o una pipeline ci/CD. Nell'esempio seguente viene usato il cmdlet [New-AzResourceGroupDeployment][New-AzResourceGroupDeployment] . Specificare il nome del gruppo di risorse e il nome file del modello:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "myResourceGroup" -TemplateFile <path-to-template>
```

Sono necessari alcuni minuti per creare la risorsa e restituire il controllo al prompt di PowerShell. Il provisioning del dominio gestito di Azure AD DS continua a essere eseguito in background e può richiedere fino a un'ora per completare la distribuzione. Nella portale di Azure la pagina **Panoramica** per il dominio gestito di Azure AD DS Mostra lo stato corrente in questa fase di distribuzione.

Quando il portale di Azure indica che il dominio gestito da Azure AD DS ha completato il provisioning, è necessario completare le attività seguenti:

* Aggiornare le impostazioni DNS per la rete virtuale, in modo che le macchine virtuali possano trovare il dominio gestito per l'autenticazione o l'aggiunta al dominio.
    * Per configurare DNS, selezionare il dominio gestito di Azure AD DS nel portale. Nella finestra **Panoramica** viene richiesto di configurare automaticamente queste impostazioni DNS.
* [Abilitare la sincronizzazione password per Azure ad Domain Services](tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds) in modo che gli utenti finali possano accedere al dominio gestito usando le credenziali aziendali.

## <a name="next-steps"></a>Passaggi successivi

Per visualizzare il dominio gestito di Azure AD DS in azione, è possibile [aggiungere un dominio a una VM Windows][windows-join], [configurare LDAP sicuro][tutorial-ldaps]e [configurare la sincronizzazione dell'hash delle password][tutorial-phs].

<!-- INTERNAL LINKS -->
[windows-join]: join-windows-vm.md
[tutorial-ldaps]: tutorial-configure-ldaps.md
[tutorial-phs]: tutorial-configure-password-hash-sync.md
[availability-zones]: ../availability-zones/az-overview.md
[portal-deploy]: ../azure-resource-manager/templates/deploy-portal.md
[powershell-deploy]: ../azure-resource-manager/templates/deploy-powershell.md
[scoped-sync]: scoped-synchronization.md
[resource-forests]: concepts-resource-forest.md

<!-- EXTERNAL LINKS -->
[Connect-AzAccount]: /powershell/module/Az.Accounts/Connect-AzAccount
[Connect-AzureAD]: /powershell/module/AzureAD/Connect-AzureAD
[New-AzureADServicePrincipal]: /powershell/module/AzureAD/New-AzureADServicePrincipal
[New-AzureADGroup]: /powershell/module/AzureAD/New-AzureADGroup
[Add-AzureADGroupMember]: /powershell/module/AzureAD/Add-AzureADGroupMember
[Get-AzureADGroup]: /powershell/module/AzureAD/Get-AzureADGroup
[Get-AzureADUser]: /powershell/module/AzureAD/Get-AzureADUser
[Register-AzResourceProvider]: /powershell/module/Az.Resources/Register-AzResourceProvider
[New-AzResourceGroup]: /powershell/module/Az.Resources/New-AzResourceGroup
[Get-AzSubscription]: /powershell/module/Az.Accounts/Get-AzSubscription
[cloud-shell]: /azure/cloud-shell/cloud-shell-windows-users
[naming-prefix]: /windows-server/identity/ad-ds/plan/selecting-the-forest-root-domain
[New-AzResourceGroupDeployment]: /powershell/module/Az.Resources/New-AzResourceGroupDeployment
