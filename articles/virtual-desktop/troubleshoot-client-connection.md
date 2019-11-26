---
title: Risolvere i problemi di Desktop remoto desktop virtuale Windows-Azure
description: Come risolvere i problemi quando si configurano le connessioni client in un ambiente tenant di desktop virtuali Windows.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 04/08/2019
ms.author: helohr
ms.openlocfilehash: 9fcc65768db3029461a5823034336bc883379292
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/20/2019
ms.locfileid: "74227678"
---
# <a name="remote-desktop-client-connections"></a>Connessioni client di Desktop remoto

Usare questo articolo per risolvere i problemi relativi alle connessioni client di desktop virtuali Windows.

## <a name="provide-feedback"></a>Fornire commenti e suggerimenti

Visitare la pagina [Windows Virtual Desktop Tech Community](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) per discutere del servizio Desktop virtuale Windows con il team del prodotto e i membri attivi della community.

## <a name="you-cant-open-a-web-client"></a>Non è possibile aprire un client Web

Verificare che sia presente la connettività Internet aprendo un altro sito Web. ad esempio, [www.Bing.com](https://www.bing.com).

Usare **nslookup** per confermare che DNS può risolvere il nome di dominio completo:

```cmd
nslookup rdweb.wvd.microsoft.com
```

Provare a connettersi con un altro client, ad esempio Desktop remoto client per Windows 7 o Windows 10, e verificare se è possibile aprire il client Web.

### <a name="error-opening-another-site-fails"></a>Errore: l'apertura di un altro sito non riesce

**Motivo:** Problemi di rete e/o interruzioni.

**Correzione:** Contattare il supporto di rete.

### <a name="error-nslookup-cannot-resolve-the-name"></a>Errore: Impossibile risolvere il nome

**Motivo:** Problemi di rete e/o interruzioni.

**Correzione:** Contattare il supporto di rete

### <a name="error-you-cant-connect-but-other-clients-can-connect"></a>Errore: non è possibile connettersi, ma gli altri client possono connettersi

**Motivo:** Il browser non funziona come previsto e ha smesso di funzionare.

**Correzione:** Seguire queste istruzioni per risolvere i problemi del browser.

1. Riavviare il browser.
2. Cancella i cookie del browser. Vedere [come eliminare i file dei cookie in Internet Explorer](https://support.microsoft.com/help/278835/how-to-delete-cookie-files-in-internet-explorer).
3. Cancellare la cache del browser. Vedere [Cancella cache del browser per il browser](https://binged.it/2RKyfdU).
4. Aprire il browser in modalità privata.

## <a name="web-client-stops-responding-or-disconnects"></a>Il client Web smette di rispondere o si disconnette

Provare a connettersi usando un altro browser o client.

### <a name="error-other-browsers-and-clients-also-malfunction-or-fail-to-open"></a>Errore: anche altri browser e client malfunzionanti o non vengono aperti

**Motivo:** Problemi o interruzioni del sistema operativo di rete e/o

**Correzione:** Contattare il team di supporto.

## <a name="web-client-keeps-prompting-for-credentials"></a>Il client Web continua a richiedere le credenziali

Se il client Web continua a richiedere le credenziali, seguire queste istruzioni.

1. Verificare che l'URL del client Web sia corretto.
2. Verificare che le credenziali siano per l'ambiente di desktop virtuale Windows associato all'URL.
3. Cancella i cookie del browser. Vedere [come eliminare i file dei cookie in Internet Explorer](https://support.microsoft.com/help/278835/how-to-delete-cookie-files-in-internet-explorer).
4. Cancellare la cache del browser. Vedere [Cancella cache del browser per il browser](https://binged.it/2RKyfdU).
5. Aprire il browser in modalità privata.

## <a name="remote-desktop-client-for-windows-7-or-windows-10-stops-responding-or-cannot-be-opened"></a>Desktop remoto client per Windows 7 o Windows 10 smette di rispondere o non può essere aperto

Usare i cmdlet di PowerShell seguenti per pulire i registri client fuori banda (OOB).

```PowerShell
Remove-ItemProperty 'HKCU:\Software\Microsoft\Terminal Server Client\Default' - Name FeedURLs

#Remove RdClientRadc registry key
Remove-Item 'HKCU:\Software\Microsoft\RdClientRadc' -Recurse

#Remove all files under %appdata%\RdClientRadc
Remove-Item C:\Users\pavithir\AppData\Roaming\RdClientRadc\* -Recurse
```

Passare a **%AppData%\RdClientRadc** ed eliminare tutto il contenuto.

Disinstallare e reinstallare Desktop remoto client per Windows 7 e Windows 10.

## <a name="troubleshooting-end-user-connectivity"></a>Risoluzione dei problemi relativi alla connettività degli utenti finali

A volte gli utenti possono accedere al feed e alle risorse locali, ma hanno comunque problemi di configurazione, disponibilità o prestazioni che ne impediscono l'accesso alle risorse remote. In questi casi, l'utente riceve messaggi simili ai seguenti:

![Messaggio di errore Connessione Desktop remoto.](media/eb76b666808bddb611448dfb621152ce.png)

![Non è possibile connettersi al messaggio di errore del gateway.](media/a8fbb9910d4672147335550affe58481.png)

Seguire queste istruzioni generali per la risoluzione dei problemi relativi ai codici di errore della connessione client.

1. Verificare il nome utente e l'ora in cui si è verificato il problema.
2. Aprire **PowerShell** e stabilire una connessione al tenant di desktop virtuale di Windows in cui è stato segnalato il problema.
3. Confermare la connessione al tenant corretto con **Get-RdsTenant.**
4. Usando i cmdlet **Get-RdsHostPool** e **Get-RdsSessionHost** , verificare che la risoluzione dei problemi venga eseguita nel pool di host corretto.
5. Eseguire il comando seguente per ottenere un elenco di tutte le attività non riuscite di tipo Connection per l'intervallo di tempo specificato:

    ```PowerShell
     Get-RdsDiagnosticActivities -TenantName <TenantName> -username <UPN> -StartTime
     "11/21/2018 1:07:03 PM" -EndTime "11/21/2018 1:27:03 PM" -Outcome Failure -ActivityType Connection
    ```

6. Utilizzando **ActivityID** dall'output del cmdlet precedente, eseguire il comando seguente:

    ```PowerShell
    (Get-RdsDiagnosticActivities -TenantName <TenantName> -ActivityId <ActivityId> -Detailed).Errors
    ```

7. Il comando genera un output simile all'output riportato di seguito. Usare **ErrorCodeSymbolic** e **ErrorMessage** per risolvere i problemi relativi alla causa principale.

    ```PowerShell
    ErrorSource       : <Source>
    ErrorOperation    : <Operation>
    ErrorCode         : <Error code>
    ErrorCodeSymbolic : <Error code string>
    ErrorMessage      : <Error code message>
    ErrorInternal     : <Internal for the OS>
    ReportedBy        : <Reported by component>
    Time              : <Timestampt>
    ```

### <a name="error-o_add_user_to_group_failed--failed-to-add-user--username-to-group--remote-desktop-users-reason-win32error_no_such_member"></a>Errore: O_ADD_USER_TO_GROUP_FAILED/non è stato possibile aggiungere l'utente = ≤ nomeutente ≥ al gruppo = Desktop remoto utenti. Motivo: Win32. ERROR_NO_SUCH_MEMBER

**Motivo:** La macchina virtuale non è stata aggiunta al dominio in cui l'oggetto utente è.

**Correzione:** Aggiungere la macchina virtuale al dominio corretto. Vedere [aggiungere una macchina virtuale Windows Server a un dominio gestito](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-admin-guide-join-windows-vm-portal).

### <a name="error-nslookup-cannot-resolve-the-name"></a>Errore: Impossibile risolvere il nome

**Motivo:** Problemi di rete o interruzioni.

**Correzione:** Contattare il supporto di rete

### <a name="error-connectionfailedclientprotocolerror"></a>Errore: ConnectionFailedClientProtocolError

**Motivo:** Le macchine virtuali a cui un utente sta tentando di connettersi non sono unite a un dominio.

**Correzione:** Aggiungere al controller di dominio tutte le macchine virtuali che fanno parte di un pool host.

### <a name="error-connectionfailedusersidinformationmismatch"></a>Errore: ConnectionFailedUserSIDInformationMismatch
**Motivo:** Il SID del token Azure Active Directory (AD) dell'utente non corrisponde al SID restituito dal controller di dominio durante il tentativo di abilitare l'utente per l'accesso remoto. Questo errore si verifica in genere quando si tenta di accedere a un ambiente di Azure Active Directory Domain Services (Azure AD DS) con un utente originariamente originato da un Active Directory di Windows Server.

**Correzione:** Questo scenario non è supportato in questo momento. Solo gli utenti originati da Azure Active Directory possono accedere a VM desktop virtuali Windows connesse a Azure AD DS.

## <a name="user-connects-but-nothing-is-displayed-no-feed"></a>L'utente si connette ma non viene visualizzato nulla (nessun feed)

Un utente può avviare Desktop remoto client ed è in grado di eseguire l'autenticazione, tuttavia l'utente non visualizza alcuna icona nel feed di individuazione Web.

Verificare che l'utente che ha segnalato i problemi sia stato assegnato ai gruppi di applicazioni utilizzando la riga di comando seguente:

```PowerShell
Get-RdsAppGroupUser <tenantname> <hostpoolname> <appgroupname>
```

Verificare che l'utente abbia effettuato l'accesso con le credenziali corrette.

Se il client Web viene utilizzato, verificare che non siano presenti problemi di credenziali memorizzate nella cache.

## <a name="next-steps"></a>Passaggi successivi

- Per una panoramica sulla risoluzione dei problemi relativi a desktop virtuale Windows e alle tracce di escalation, vedere [panoramica sulla risoluzione dei problemi, commenti e suggerimenti e supporto](troubleshoot-set-up-overview.md).
- Per risolvere i problemi durante la creazione di un tenant e di un pool host in un ambiente desktop virtuale Windows, vedere [creazione di tenant e pool host](troubleshoot-set-up-issues.md).
- Per risolvere i problemi durante la configurazione di una macchina virtuale (VM) in desktop virtuale di Windows, vedere [configurazione della macchina virtuale host sessione](troubleshoot-vm-configuration.md).
- Per risolvere i problemi relativi all'uso di PowerShell con desktop virtuale di Windows, vedere [PowerShell per desktop virtuale di Windows](troubleshoot-powershell.md).
- Per un'esercitazione per la risoluzione dei problemi, vedere [esercitazione: risolvere i problemi relativi alle distribuzioni di modelli gestione risorse](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-tutorial-troubleshoot).
