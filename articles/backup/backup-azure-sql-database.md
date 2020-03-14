---
title: Eseguire un backup dei database SQL Server in Azure
description: Questo articolo illustra come eseguire il backup di SQL Server in Azure. L'articolo spiega inoltre il recupero di SQL Server.
ms.topic: conceptual
ms.date: 06/18/2019
ms.openlocfilehash: 39f2348a95be95a03dada45d48952dce99ec4ec7
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2020
ms.locfileid: "79273241"
---
# <a name="about-sql-server-backup-in-azure-vms"></a>Informazioni sul backup di SQL Server in macchine virtuali di Azure

I database di SQL Server sono carichi di lavoro critici che richiedono un obiettivo del punto di ripristino (RPO) basso e la conservazione a lungo termine. È possibile eseguire il backup di database SQL Server in esecuzione nelle macchine virtuali di Azure usando [Backup di Azure](backup-overview.md).

## <a name="backup-process"></a>Processo di backup

Questa soluzione sfrutta le API native SQL per eseguire i backup dei database SQL.

* Dopo aver specificato la macchina virtuale di SQL Server da proteggere e aver eseguito la query per trovare i database al suo interno, il servizio Backup di Azure installerà un'estensione di backup di carichi di lavoro nella macchina virtuale denominata `AzureBackupWindowsWorkload`.
* Questa estensione è costituita da un coordinatore e da un plug-in SQL. Mentre il coordinatore è responsabile di attivare i flussi di lavoro per varie operazioni, come la configurazione del backup, il backup e il ripristino, il plug-in gestisce il flusso di dati effettivo.
* Per individuare i database in questa VM, Backup di Azure crea l'account `NT SERVICE\AzureWLBackupPluginSvc`. Questo account viene usato per il backup e il ripristino e richiede le autorizzazioni sysadmin SQL. L'account `NT SERVICE\AzureWLBackupPluginSvc` è un [account del servizio virtuale](https://docs.microsoft.com/windows/security/identity-protection/access-control/service-accounts#virtual-accounts) e pertanto non richiede alcuna gestione delle password. Backup di Azure sfrutta l'account `NT AUTHORITY\SYSTEM` per l'individuazione o l'interrogazione dei database, di conseguenza questo account deve essere un account di accesso pubblico in SQL. Se la VM SQL Server non è stata creata da Azure Marketplace, è possibile ricevere un errore **UserErrorSQLNoSysadminMembership**. In tal caso [seguire queste istruzioni](#set-vm-permissions).
* Dopo l'attivazione della configurazione della protezione nei database selezionati, il servizio di backup configura il coordinatore con le pianificazioni di backup e altri dettagli sui criteri, che l'estensione memorizza nella cache locale della VM.
* Nell'orario pianificato il coordinatore comunica con il plug-in, che avvia lo streaming dei dati di backup dal server SQL tramite VDI.  
* Il plug-in invia i dati direttamente all'insieme di credenziali dei servizi di ripristino, eliminando così la necessità di una posizione per la gestione temporanea. I dati vengono crittografati e archiviati dal servizio Backup di Azure negli account di archiviazione.
* Al termine del trasferimento dei dati, il coordinatore conferma il commit con il servizio di backup.

  ![Architettura del backup SQL](./media/backup-azure-sql-database/backup-sql-overview.png)

## <a name="before-you-start"></a>Prima di iniziare

Prima di iniziare, verificare quanto segue:

1. Assicurarsi che sia in esecuzione un'istanza di SQL Server in Azure. È possibile [creare rapidamente un'istanza di SQL Server](../virtual-machines/windows/sql/quickstart-sql-vm-create-portal.md) nel Marketplace.
2. Vedere le [considerazioni sulla funzionalità](#feature-consideration-and-limitations) e il [supporto degli scenari](#scenario-support).
3. [Esaminare le domande comuni](faq-backup-sql-server.md) su questo scenario.

## <a name="scenario-support"></a>Supporto degli scenari

**Supporto** | **Dettagli**
--- | ---
**Distribuzioni supportate** | Sono supportate VM di Azure del Marketplace SQL e non del Marketplace (SQL Server installato manualmente).
**Aree geografiche supportate** | Australia sud-orientale (ASE), Australia orientale (AE), Australia centrale (AC), Australia centrale 2 (AC) <br> Brasile meridionale (BRS)<br> Canada centrale (CNC), Canada orientale (CE)<br> Asia sud-orientale (SEA), Asia orientale (EA) <br> Stati Uniti orientali (EUS), Stati Uniti orientali 2 (EUS2), Stati Uniti centro-occidentali (WCUS), Stati Uniti occidentali (WUS), Stati Uniti occidentali 2 (WUS 2), Stati Uniti centro-settentrionali (NCUS), Stati Uniti centrali (CUS), Stati Uniti centro-meridionali (SCUS) <br> India centrale (INC), India meridionale (INS), India occidentale <br> Giappone orientale (JPE), Giappone occidentale (JPW) <br> Corea centrale (KRC), Corea meridionale (KRS) <br> Europa settentrionale (NE), Europa occidentale <br> Regno Unito meridionale (UKS), Regno Unito occidentale (UKW) <br> US Gov Arizona, US Gov Virginia, US Gov Texas, US DoD (area centrale), US DoD (area orientale) <br> Germania settentrionale, Germania centro-occidentale <br> Svizzera settentrionale, Svizzera occidentale <br> Francia centrale <br> Cina orientale, Cina orientale 2, Cina settentrionale, Cina settentrionale 2
**Sistemi operativi supportati** | Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2008 R2 SP1 <br/><br/> Linux non è attualmente supportato.
**Versioni di SQL Server supportate** | SQL Server 2019, SQL Server 2017 come indicato nella [pagina Ricerca nel ciclo di vita del supporto](https://support.microsoft.com/lifecycle/search?alpha=SQL%20server%202017), SQL Server 2016 e SP come indicato nella [pagina Ricerca nel ciclo di vita del supporto](https://support.microsoft.com/lifecycle/search?alpha=SQL%20server%202016%20service%20pack), SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008 <br/><br/> Enterprise, Standard, Web, Developer, Express.
**Versioni di .NET supportate** | .NET Framework 4.5.2 o versioni successive installato nella macchina virtuale

## <a name="feature-consideration-and-limitations"></a>Considerazioni e limitazioni della funzionalità

* Il backup di SQL Server può essere configurato nel portale di Azure o in **PowerShell**. L'interfaccia della riga di comando non è supportata.
* La soluzione è supportata in entrambe le tipologie di [distribuzione](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model): macchine virtuali di Azure Resource Manager e macchine virtuali classiche.
* La macchina virtuale che esegue SQL Server richiede la connettività Internet per accedere agli indirizzi IP pubblici di Azure.
* SQL Server **istanza del cluster di failover (FCI)** non è supportata.
* Le operazioni di backup e ripristino per i database mirror e gli snapshot di database non sono supportate.
* L'uso di più soluzioni per eseguire il backup dell'istanza di SQL Server autonoma o del gruppo di disponibilità AlwaysOn di SQL potrebbe generare un errore di backup. Evitare questo approccio.
* Anche il backup di due nodi di un gruppo di disponibilità singolarmente con soluzioni uguali o differenti potrebbe generare errori.
* Backup di Azure supporta solo i tipi di backup completo e completo solo copia per i database di **sola lettura**
* Non è possibile proteggere i database con un numero elevato di file. Il numero massimo di file supportato è **~1000**.  
* È possibile eseguire il backup di un totale di **~2000** database SQL Server in un insieme di credenziali. Nel caso di un numero elevato di database, è possibile creare più insiemi di credenziali.
* È possibile configurare il backup per un totale di **50** database alla volta. Questa restrizione contribuisce a ottimizzare i carichi di backup.
* Sono supportati database di dimensioni fino a **2 TB** ; per le dimensioni maggiori di questi problemi di prestazioni possono essere rilevati.
* Per avere un'idea di come il numero di database che è possibile proteggere per ogni server, è necessario prendere in considerazione fattori quali larghezza di banda, dimensioni della macchina virtuale, frequenza di backup, dimensioni del database e così via. [scaricare](https://download.microsoft.com/download/A/B/5/AB5D86F0-DCB7-4DC3-9872-6155C96DE500/SQL%20Server%20in%20Azure%20VM%20Backup%20Scale%20Calculator.xlsx) il Resource Planner che fornisce il numero approssimativo di database che è possibile avere per ogni server in base alle risorse della macchina virtuale e ai criteri di backup.
* Nel caso dei gruppi di disponibilità, i backup vengono eseguiti da diversi nodi in base ad alcuni fattori. Il comportamento di backup per un gruppo di disponibilità è riepilogato di seguito.

### <a name="back-up-behavior-in-case-of-always-on-availability-groups"></a>Comportamento di backup nel caso di gruppi di disponibilità AlwaysOn

È consigliabile configurare il backup solo su un nodo di un gruppo di disponibilità. Il backup deve sempre essere configurato nella stessa area del nodo primario. In altre parole, il nodo primario deve essere sempre presente nell'area in cui si sta configurando il backup. Se tutti i nodi del gruppo di disponibilità risiedono nella stessa area in cui è configurato il backup, non c'è motivo di preoccuparsi.

#### <a name="for-cross-region-ag"></a>Per i gruppi di disponibilità tra aree

* Indipendentemente dalle preferenze per il backup, non verrà eseguito alcun backup dai nodi che non risiedono nella stessa area in cui è configurato il backup, perché i backup tra più aree non sono supportati. Se sono presenti solo due nodi e il nodo secondario risiede nell'altra area, verranno eseguiti solo i backup dal nodo primario, a meno che la preferenza scelta per il backup non sia "solo secondario".
* Se viene eseguito il failover in un'area diversa da quella in cui è configurato il backup, il backup sui nodi dell'area di failover non verrà eseguito.

In base alle preferenze e ai tipi di backup (completo/differenziale/log/completo solo copia), i backup vengono eseguiti da uno specifico nodo (primario o secondario).

* **Preferenza di backup: primario**

**Tipo di backup** | **Node**
    --- | ---
    Full | Primaria
    Differenziale | Primaria
    File di log |  Primaria
    Completo solo copia |  Primaria

* **Preferenza di backup: solo secondario**

**Tipo di backup** | **Node**
--- | ---
Full | Primaria
Differenziale | Primaria
File di log |  Secondari
Completo solo copia |  Secondari

* **Preferenza di backup: secondaria**

**Tipo di backup** | **Node**
--- | ---
Full | Primaria
Differenziale | Primaria
File di log |  Secondari
Completo solo copia |  Secondari

* **Nessuna preferenza di backup**

**Tipo di backup** | **Node**
--- | ---
Full | Primaria
Differenziale | Primaria
File di log |  Secondari
Completo solo copia |  Secondari

## <a name="set-vm-permissions"></a>Impostare le autorizzazioni della VM

  Quando si esegue l'individuazione in SQL Server, Backup di Azure esegue le operazioni seguenti:

* Aggiunge l'estensione AzureBackupWindowsWorkload.
* Crea un account NT SERVICE\AzureWLBackupPluginSvc per individuare i database nella macchina virtuale. Questo account viene usato per il backup e il ripristino e richiede le autorizzazioni di amministratore di sistema SQL.
* Individua i database in esecuzione in una macchina virtuale. Backup di Azure usa l'account NT AUTHORITY\SYSTEM. Questo account deve essere un account di accesso pubblico in SQL.

Se la VM di SQL Server non è stata creata in Azure Marketplace o se si usa SQL 2008 o 2008 R2, potrebbe venire generato un errore **UserErrorSQLNoSysadminMembership**.

Per concedere le autorizzazioni in caso di **SQL 2008** e **2008 R2** in esecuzione in Windows 2008 R2, vedere [qui](#give-sql-sysadmin-permissions-for-sql-2008-and-sql-2008-r2).

Per tutte le altre versioni, correggere le autorizzazioni con i passaggi seguenti:

  1. Usare un account con le autorizzazioni sysadmin SQL Server per accedere a SQL Server Management Studio (SSMS). A meno che non siano necessarie autorizzazioni speciali, l'autenticazione di Windows dovrebbe funzionare.
  2. In SQL Server aprire la cartella **Sicurezza/Account di accesso**.

      ![Aprire la cartella Sicurezza/Account di accesso per vedere gli account](./media/backup-azure-sql-database/security-login-list.png)

  3. Fare clic con il pulsante destro del mouse sulla cartella **Account di accesso** e scegliere **Nuovo account di accesso**. In **Account di accesso - Nuovo** selezionare **Cerca**.

      ![Nella finestra di dialogo Account di accesso - Nuovo selezionare Cerca](./media/backup-azure-sql-database/new-login-search.png)

  4. L'account di servizio virtuale di Windows **NT SERVICE\AzureWLBackupPluginSvc** è stato creato durante la fase di registrazione della macchina virtuale e di individuazione SQL. Immettere il nome dell'account come mostrato nella casella **Immettere il nome dell'oggetto da selezionare**. Selezionare **Controlla nomi** per risolvere il nome. Fare clic su **OK**.

      ![Selezionare Controlla nomi per risolvere il nome del servizio sconosciuto](./media/backup-azure-sql-database/check-name.png)

  5. In **Ruoli server** assicurarsi che sia selezionato il ruolo **sysadmin**. Fare clic su **OK**. Le autorizzazioni necessarie a questo punto devono essere presenti.

      ![Verificare che sia selezionato il ruolo del server sysadmin](./media/backup-azure-sql-database/sysadmin-server-role.png)

  6. Associare ora il database all'insieme di credenziali di Servizi di ripristino. Nell'elenco dei **server protetti** del portale di Azure fare clic con il pulsante destro del mouse sul server in stato di errore e scegliere **Individua di nuovo i database**.

      ![Verificare che il server abbia le autorizzazioni appropriate](./media/backup-azure-sql-database/check-erroneous-server.png)

  7. Verificare l'avanzamento nell'area **Notifiche**. Quando i database selezionati sono stati individuati, viene visualizzato un messaggio di conferma.

      ![Messaggio di distribuzione riuscita](./media/backup-azure-sql-database/notifications-db-discovered.png)

> [!NOTE]
> Se sono installate più istanze di SQL Server, è necessario aggiungere l'autorizzazione sysadmin per l'account **NT Service\AzureWLBackupPluginSvc** a tutte le istanze di SQL.

### <a name="give-sql-sysadmin-permissions-for-sql-2008-and-sql-2008-r2"></a>Concedere le autorizzazioni di amministratore di sistema SQL per SQL 2008 e SQL 2008 R2

Aggiungere gli account di accesso **NT AUTHORITY\SYSTEM** e **NT Service\AzureWLBackupPluginSvc** all'istanza di SQL Server:

1. Passare all'istanza di SQL Server in Esplora oggetti.
2. Passare a Sicurezza -> Account di accesso
3. Fare clic con il pulsante destro del mouse su Account di accesso e scegliere *Nuovo account di accesso*.

    ![Nuovo account di accesso con SSMS](media/backup-azure-sql-database/sql-2k8-new-login-ssms.png)

4. Passare alla scheda Generale e immettere **NT AUTHORITY\SYSTEM** come ID di accesso.

    ![ID di accesso per SSMS](media/backup-azure-sql-database/sql-2k8-nt-authority-ssms.png)

5. Passare a *Ruoli del server* e scegliere i ruoli *public* e *sysadmin*.

    ![Scelta dei ruoli in SSMS](media/backup-azure-sql-database/sql-2k8-server-roles-ssms.png)

6. Passare a *Stato*. Selezionare *Concedi* per Autorizzazione per la connessione al motore di database e selezionare *Abilitato* per Account di accesso.

    ![Concedere le autorizzazioni in SSMS](media/backup-azure-sql-database/sql-2k8-grant-permission-ssms.png)

7. Fare clic su OK.
8. Ripetere la stessa procedura (passaggi da 1 a 7 precedenti) per aggiungere l'account di accesso NT Service\AzureWLBackupPluginSvc all'istanza di SQL Server. Se l'account di accesso esiste già, assicurarsi che abbia il ruolo del server sysadmin e che in Stato siano selezionate l'opzione Concedi per Autorizzazione per la connessione al motore di database e l'opzione Abilitato per Account di accesso.
9. Dopo aver concesso l'autorizzazione, **riindividuare** i database nel portale: insieme di credenziali **->** infrastruttura di backup **->** carico di lavoro in una macchina virtuale di Azure:

    ![Opzione Individua di nuovo i database nel portale di Azure](media/backup-azure-sql-database/sql-rediscover-dbs.png)

In alternativa, è possibile automatizzare la concessione delle autorizzazioni eseguendo i comandi di PowerShell seguenti in modalità di amministrazione. Per impostazione predefinita, il nome dell'istanza è impostato su MSSQLSERVER. Modificare l'argomento relativo al nome dell'istanza nello script, se necessario:

```powershell
param(
    [Parameter(Mandatory=$false)]
    [string] $InstanceName = "MSSQLSERVER"
)
if ($InstanceName -eq "MSSQLSERVER")
{
    $fullInstance = $env:COMPUTERNAME   # In case it is the default SQL Server Instance
}
else
{
    $fullInstance = $env:COMPUTERNAME + "\" + $InstanceName   # In case of named instance
}
try
{
    sqlcmd.exe -S $fullInstance -Q "sp_addsrvrolemember 'NT Service\AzureWLBackupPluginSvc', 'sysadmin'" # Adds login with sysadmin permission if already not available
}
catch
{
    Write-Host "An error occurred:"
    Write-Host $_.Exception|format-list -force
}
try
{
    sqlcmd.exe -S $fullInstance -Q "sp_addsrvrolemember 'NT AUTHORITY\SYSTEM', 'sysadmin'" # Adds login with sysadmin permission if already not available
}
catch
{
    Write-Host "An error occurred:"
    Write-Host $_.Exception|format-list -force
}
```

## <a name="next-steps"></a>Passaggi successivi

* [Informazioni](backup-sql-server-database-azure-vms.md) sul backup dei database di SQL Server.
* [Informazioni](restore-sql-database-azure-vm.md) sul ripristino dei database SQL Server di cui è stato eseguito il backup.
* [Informazioni](manage-monitor-sql-database-backup.md) sulla gestione dei database SQL Server di cui è stato eseguito il backup.
