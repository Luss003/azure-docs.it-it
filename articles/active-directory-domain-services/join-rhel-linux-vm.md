---
title: Aggiungere una macchina virtuale RHEL a Azure AD Domain Services | Microsoft Docs
description: Informazioni su come configurare e aggiungere una macchina virtuale Red Hat Enterprise Linux a un dominio gestito da Azure AD Domain Services.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 16100caa-f209-4cb0-86d3-9e218aeb51c6
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: iainfou
ms.openlocfilehash: d12dd0c79f2e9c1d2b0cc17956a0bb8d8fa35865
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2020
ms.locfileid: "78299143"
---
# <a name="join-a-red-hat-enterprise-linux-virtual-machine-to-an-azure-ad-domain-services-managed-domain"></a>Aggiungere una macchina virtuale Red Hat Enterprise Linux a un dominio gestito Azure AD Domain Services

Per consentire agli utenti di accedere a macchine virtuali (VM) in Azure usando un unico set di credenziali, è possibile aggiungere macchine virtuali a un dominio gestito di Azure Active Directory Domain Services (AD DS). Quando si aggiunge una macchina virtuale a un dominio gestito di Azure AD DS, è possibile usare gli account utente e le credenziali del dominio per l'accesso e la gestione dei server. Vengono inoltre applicate le appartenenze ai gruppi dal dominio gestito Azure AD DS per consentire il controllo dell'accesso ai file o ai servizi nella macchina virtuale.

Questo articolo illustra come aggiungere una macchina virtuale Red Hat Enterprise Linux (RHEL) a un dominio gestito di Azure AD DS.

## <a name="prerequisites"></a>Prerequisites

Per completare l'esercitazione, sono necessari i privilegi e le risorse seguenti:

* Una sottoscrizione di Azure attiva.
    * Se non si ha una sottoscrizione di Azure, [creare un account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Un tenant di Azure Active Directory associato alla sottoscrizione, sincronizzato con una directory locale o con una directory solo cloud.
    * Se necessario, [creare un tenant di Azure Active Directory][create-azure-ad-tenant] o [associare una sottoscrizione di Azure al proprio account][associate-azure-ad-tenant].
* Un dominio gestito di Azure Active Directory Domain Services abilitato e configurato nel tenant di Azure AD.
    * Se necessario, seguire la prima esercitazione per [creare e configurare un'istanza di Azure Active Directory Domain Services][create-azure-ad-ds-instance].
* Un account utente che fa parte del dominio gestito Azure AD DS.

## <a name="create-and-connect-to-a-rhel-linux-vm"></a>Creare e connettersi a una macchina virtuale RHEL Linux

Se si dispone di una macchina virtuale RHEL Linux esistente in Azure, connettersi con SSH, quindi continuare con il passaggio successivo per [iniziare a configurare la macchina virtuale](#configure-the-hosts-file).

Se è necessario creare una macchina virtuale RHEL Linux o si vuole creare una macchina virtuale di test da usare con questo articolo, è possibile usare uno dei metodi seguenti:

* [Azure portal](../virtual-machines/linux/quick-create-portal.md)
* [Interfaccia della riga di comando di Azure](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

Quando si crea la VM, prestare attenzione alle impostazioni della rete virtuale per assicurarsi che la macchina virtuale sia in grado di comunicare con il dominio gestito di Azure AD DS:

* Distribuire la macchina virtuale nella stessa rete virtuale con peering, in cui è stata abilitata Azure AD Domain Services.
* Distribuire la macchina virtuale in una subnet diversa da quella dell'istanza di Azure AD Domain Services.

Dopo aver distribuito la macchina virtuale, seguire la procedura per connettersi alla macchina virtuale tramite SSH.

## <a name="configure-the-hosts-file"></a>Configurare il file hosts

Per assicurarsi che il nome host della macchina virtuale sia configurato correttamente per il dominio gestito, modificare *il file hosts e impostare il nome* host:

```console
sudo vi /etc/hosts
```

Nel file *host* aggiornare l'indirizzo *localhost* . Nell'esempio seguente:

* *aaddscontoso.com* è il nome di dominio DNS del dominio gestito di Azure AD DS.
* *RHEL* è il nome host della macchina virtuale RHEL che si sta aggiungendo al dominio gestito.

Aggiornare questi nomi con valori personalizzati:

```console
127.0.0.1 rhel rhel.aaddscontoso.com
```

Al termine, salvare e chiudere il file *host* usando il comando `:wq` dell'editor.

## <a name="install-required-packages"></a>Installare i pacchetti necessari

La VM necessita di alcuni pacchetti aggiuntivi per aggiungere la macchina virtuale al dominio gestito di Azure AD DS. Per installare e configurare questi pacchetti, aggiornare e installare gli strumenti di aggiunta al dominio usando `yum`. Esistono alcune differenze tra RHEL 7. x e RHEL 6. x, quindi usare i comandi appropriati per la versione della distribuzione nelle sezioni rimanenti di questo articolo.

**RHEL 7**

```console
sudo yum install realmd sssd krb5-workstation krb5-libs oddjob oddjob-mkhomedir samba-common-tools
```

**RHEL 6**

```console
sudo yum install adcli sssd authconfig krb5-workstation
```

## <a name="join-vm-to-the-managed-domain"></a>Aggiungere una macchina virtuale al dominio gestito

Ora che i pacchetti necessari sono installati nella macchina virtuale, aggiungere la macchina virtuale al dominio gestito di Azure AD DS. Anche in questo caso, usare la procedura appropriata per la versione di RHEL distro.

### <a name="rhel-7"></a>RHEL 7

1. Usare il comando `realm discover` per individuare il dominio gestito Azure AD DS. Nell'esempio seguente viene individuato il *AADDSCONTOSO.com*dell'area di autenticazione. Specificare il proprio nome di dominio gestito di Azure AD DS in tutte le lettere maiuscole:

    ```console
    sudo realm discover AADDSCONTOSO.COM
    ```

   Se il comando `realm discover` non riesce a trovare il dominio gestito Azure AD DS, esaminare i passaggi per la risoluzione dei problemi seguenti:

    * Verificare che il dominio sia raggiungibile dalla macchina virtuale. Provare `ping aaddscontoso.com` per verificare se viene restituita una risposta positiva.
    * Verificare che la macchina virtuale sia distribuita nello stesso o in una rete virtuale con peering in cui è disponibile il dominio gestito di Azure AD DS.
    * Verificare che le impostazioni del server DNS per la rete virtuale siano state aggiornate in modo che puntino ai controller di dominio del dominio gestito Azure AD DS.

1. A questo punto, inizializzare Kerberos usando il comando `kinit`. Specificare un utente che fa parte del dominio gestito di Azure AD DS. Se necessario, [aggiungere un account utente a un gruppo in Azure ad](../active-directory/fundamentals/active-directory-groups-members-azure-portal.md).

    Anche in questo caso, è necessario immettere il nome di dominio gestito Azure AD DS in tutti i caratteri maiuscoli. Nell'esempio seguente viene usato l'account denominato `contosoadmin@aaddscontoso.com` per inizializzare Kerberos. Immettere il proprio account utente che fa parte del dominio gestito di Azure AD DS:

    ```console
    kinit contosoadmin@AADDSCONTOSO.COM
    ```

1. Aggiungere infine il computer al dominio gestito Azure AD DS usando il comando `realm join`. Usare lo stesso account utente che fa parte del dominio gestito Azure AD DS specificato nel comando `kinit` precedente, ad esempio `contosoadmin@AADDSCONTOSO.COM`:

    ```console
    sudo realm join --verbose AADDSCONTOSO.COM -U 'contosoadmin@AADDSCONTOSO.COM'
    ```

Per aggiungere la macchina virtuale al dominio gestito di Azure AD DS sono necessari alcuni minuti. L'output di esempio seguente mostra che la macchina virtuale è stata aggiunta correttamente al dominio gestito di Azure AD DS:

```output
Successfully enrolled machine in realm
```

### <a name="rhel-6"></a>RHEL 6

1. Usare il comando `adcli info` per individuare il dominio gestito Azure AD DS. Nell'esempio seguente viene individuato il *ADDDSCONTOSO.com*dell'area di autenticazione. Specificare il proprio nome di dominio gestito di Azure AD DS in tutte le lettere maiuscole:

    ```console
    sudo adcli info aaddscontoso.com
    ```

   Se il comando `adcli info` non riesce a trovare il dominio gestito Azure AD DS, esaminare i passaggi per la risoluzione dei problemi seguenti:

    * Verificare che il dominio sia raggiungibile dalla macchina virtuale. Provare `ping aaddscontoso.com` per verificare se viene restituita una risposta positiva.
    * Verificare che la macchina virtuale sia distribuita nello stesso o in una rete virtuale con peering in cui è disponibile il dominio gestito di Azure AD DS.
    * Verificare che le impostazioni del server DNS per la rete virtuale siano state aggiornate in modo che puntino ai controller di dominio del dominio gestito Azure AD DS.

1. Per prima cosa, aggiungere il dominio usando il comando `adcli join`, questo comando creerà anche il keytab per autenticare il computer. Usare un account utente che fa parte del dominio gestito di Azure AD DS.

    ```console
    sudo adcli join aaddscontoso.com -U contosoadmin
    ```

1. Configurare ora il `/ect/krb5.conf` e creare i file di `/etc/sssd/sssd.conf` per usare il dominio di Active Directory `aaddscontoso.com`.
   Assicurarsi che `AADDSCONTOSO.COM` venga sostituito dal nome di dominio:

    Aprire il file di `/ect/krb5.conf` con un editor:

    ```console
    sudo vi /etc/krb5.conf
    ```

    Aggiornare il file di `krb5.conf` in modo che corrisponda all'esempio seguente:

    ```console
    [logging]
     default = FILE:/var/log/krb5libs.log
     kdc = FILE:/var/log/krb5kdc.log
     admin_server = FILE:/var/log/kadmind.log
    
    [libdefaults]
     default_realm = AADDSCONTOSO.COM
     dns_lookup_realm = true
     dns_lookup_kdc = true
     ticket_lifetime = 24h
     renew_lifetime = 7d
     forwardable = true
    
    [realms]
     AADDSCONTOSO.COM = {
     kdc = AADDSCONTOSO.COM
     admin_server = AADDSCONTOSO.COM
     }
    
    [domain_realm]
     .AADDSCONTOSO.COM = AADDSCONTOSO.COM
     AADDSCONTOSO.COM = AADDSCONTOSO.COM
    ```
    
   Creare il file di `/etc/sssd/sssd.conf`:
    
    ```console
    sudo vi /etc/sssd/sssd.conf
    ```

    Aggiornare il file di `sssd.conf` in modo che corrisponda all'esempio seguente:

    ```console
    [sssd]
     services = nss, pam, ssh, autofs
     config_file_version = 2
     domains = AADDSCONTOSO.COM
    
    [domain/AADDSCONTOSO.COM]
    
     id_provider = ad
    ```

1. Assicurarsi `/etc/sssd/sssd.conf` le autorizzazioni siano 600 ed è di proprietà dell'utente root:

    ```console
    sudo chmod 600 /etc/sssd/sssd.conf
    sudo chown root:root /etc/sssd/sssd.conf
    ```

1. Usare `authconfig` per indicare alla macchina virtuale l'integrazione di AD Linux:

    ```console
    sudo authconfig --enablesssd --enablesssdauth --update
    ```

1. Avviare e abilitare il servizio SSSD:

    ```console
    sudo service sssd start
    sudo chkconfig sssd on
    ```

Se la macchina virtuale non riesce a completare il processo di aggiunta al dominio, verificare che il gruppo di sicurezza di rete della macchina virtuale consenta il traffico Kerberos in uscita sulla porta TCP + UDP 464 alla subnet della rete virtuale per il dominio gestito di Azure AD DS.

A questo punto, controllare se è possibile eseguire query sulle informazioni relative AD un utente usando `getent`

```console
sudo getent passwd contosoadmin
```

## <a name="allow-password-authentication-for-ssh"></a>Consenti l'autenticazione tramite password per SSH

Per impostazione predefinita, gli utenti possono accedere a una VM solo usando l'autenticazione basata su chiave pubblica SSH. L'autenticazione basata su password ha esito negativo. Quando si aggiunge la macchina virtuale a un dominio gestito Azure AD DS, gli account di dominio devono usare l'autenticazione basata su password. Aggiornare la configurazione SSH per consentire l'autenticazione basata su password, come indicato di seguito.

1. Aprire il file di *sshd_conf* con un editor:

    ```console
    sudo vi /etc/ssh/sshd_config
    ```

1. Aggiornare la riga per *PasswordAuthentication* a *Sì*:

    ```console
    PasswordAuthentication yes
    ```

    Al termine, salvare e chiudere il file di *sshd_conf* usando il comando `:wq` dell'editor.

1. Per applicare le modifiche e consentire agli utenti di accedere con una password, riavviare il servizio SSH per la versione di RHEL distro:

   **RHEL 7**

    ```console
    sudo systemctl restart sshd
    ```

   **RHEL 6**

    ```console
    sudo service sshd restart
    ```

## <a name="grant-the-aad-dc-administrators-group-sudo-privileges"></a>Concedere i privilegi sudo al gruppo "Amministratori di AAD DC"

Per concedere ai membri degli *amministratori di AAD DC* i privilegi amministrativi per la VM RHEL, è necessario aggiungere una voce a */etc/sudoers*. Una volta aggiunti, i membri del gruppo *AAD DC Administrators* possono usare il comando `sudo` nella macchina virtuale RHEL.

1. Aprire il file *sudoers* per la modifica:

    ```console
    sudo visudo
    ```

1. Aggiungere la voce seguente alla fine del file */etc/sudoers* . Il gruppo *AAD DC Administrators* contiene uno spazio vuoto nel nome, quindi includere il carattere di escape barra rovesciata nel nome del gruppo. Aggiungere il proprio nome di dominio, ad esempio *aaddscontoso.com*:

    ```console
    # Add 'AAD DC Administrators' group members as admins.
    %AAD\ DC\ Administrators@aaddscontoso.com ALL=(ALL) NOPASSWD:ALL
    ```

    Al termine, salvare e chiudere l'editor utilizzando il `:wq` comando dell'editor.

## <a name="sign-in-to-the-vm-using-a-domain-account"></a>Accedere alla macchina virtuale usando un account di dominio

Per verificare che la macchina virtuale sia stata aggiunta correttamente al dominio gestito di Azure AD DS, avviare una nuova connessione SSH usando un account utente di dominio. Verificare che sia stata creata una home directory e che sia stata applicata l'appartenenza al gruppo dal dominio.

1. Creare una nuova connessione SSH dalla console di. Usare un account di dominio appartenente al dominio gestito usando il comando `ssh -l`, ad esempio `contosoadmin@aaddscontoso.com` e quindi immettere l'indirizzo della macchina virtuale, ad esempio *RHEL.aaddscontoso.com*. Se si usa la Azure Cloud Shell, usare l'indirizzo IP pubblico della macchina virtuale anziché il nome DNS interno.

    ```console
    ssh -l contosoadmin@AADDSCONTOSO.com rhel.aaddscontoso.com
    ```

1. Dopo aver eseguito la connessione alla macchina virtuale, verificare che la home directory sia stata inizializzata correttamente:

    ```console
    pwd
    ```

    Si dovrebbe trovarsi nella directory */Home* con la propria directory che corrisponde all'account utente.

1. Verificare ora che le appartenenze ai gruppi siano state risolte correttamente:

    ```console
    id
    ```

    Le appartenenze ai gruppi verranno visualizzate dal dominio gestito di Azure AD DS.

1. Se è stato effettuato l'accesso alla macchina virtuale come membro del gruppo di *amministratori di AAD DC* , verificare che sia possibile usare correttamente il comando `sudo`:

    ```console
    sudo yum update
    ```

## <a name="next-steps"></a>Passaggi successivi

Se si verificano problemi durante la connessione della macchina virtuale al dominio gestito di Azure AD DS o l'accesso con un account di dominio, vedere [risoluzione dei](join-windows-vm.md#troubleshoot-domain-join-issues)problemi di aggiunta a un dominio.

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
