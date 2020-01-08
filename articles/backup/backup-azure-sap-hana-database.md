---
title: Eseguire il backup di un database di SAP HANA in Azure con backup di Azure
description: Questo articolo illustra come eseguire il backup di un database di SAP HANA in macchine virtuali di Azure con il servizio backup di Azure.
ms.topic: conceptual
ms.date: 11/12/2019
ms.openlocfilehash: 3246f6cf8046e0a0c5795059ad3448b70130e7e1
ms.sourcegitcommit: f0dfcdd6e9de64d5513adf3dd4fe62b26db15e8b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/26/2019
ms.locfileid: "75496967"
---
# <a name="back-up-sap-hana-databases-in-azure-vms"></a>Eseguire il backup di database SAP HANA nelle VM di Azure

SAP HANA database sono carichi di lavoro di importanza critica che richiedono un obiettivo del punto di ripristino (RPO) basso e la conservazione a lungo termine. È possibile eseguire il backup di SAP HANA database in esecuzione in macchine virtuali di Azure con [backup di Azure](backup-overview.md).

Questo articolo illustra come eseguire il backup di database SAP HANA in esecuzione in macchine virtuali di Azure in un insieme di credenziali di servizi di ripristino di backup di Azure.

In questo articolo verrà spiegato come:
> [!div class="checklist"]
>
> * Creare e configurare un insieme di credenziali
> * Individuare i database
> * Configurare i backup
> * Eseguire un processo di backup su richiesta

## <a name="prerequisites"></a>Prerequisiti

Per configurare il database per il backup, vedere le sezioni [prerequisiti](tutorial-backup-sap-hana-db.md#prerequisites) e [impostazione delle autorizzazioni](tutorial-backup-sap-hana-db.md#setting-up-permissions) .

### <a name="set-up-network-connectivity"></a>Configurare la connettività di rete

Per tutte le operazioni, la macchina virtuale SAP HANA richiede la connettività agli indirizzi IP pubblici di Azure. Le operazioni della macchina virtuale, ad esempio individuazione dei database, configurazione e pianificazione dei backup, ripristino dei punti di ripristino e così via, non funzionano in assenza di connettività. Stabilire la connettività consentendo l'accesso agli intervalli IP dei data center Azure:

* È possibile scaricare gli [intervalli di indirizzi IP](https://www.microsoft.com/download/details.aspx?id=41653) dei data center Azure e quindi consentirvi l'accesso.
* Se si usano i gruppi di sicurezza di rete, è possibile usare il [tag del servizio](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) AzureCloud per consentire tutti gli indirizzi IP pubblici di Azure. Per modificare le regole dei gruppi di sicurezza di rete, è possibile usare il cmdlet [Set-AzureNetworkSecurityRule](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azurenetworksecurityrule?view=azuresmps-4.0.0).
* La porta 443 deve essere aggiunta all'elenco di elementi consentiti, perché il trasporto avviene tramite HTTPS.

## <a name="onboard-to-the-public-preview"></a>Eseguire l'onboarding nell'anteprima pubblica

Eseguire l'onboarding nell'anteprima pubblica come segue:

* Nel portale registrare l'ID sottoscrizione nel provider di Servizi di ripristino seguendo le istruzioni di [questo articolo](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-register-provider-errors#solution-3---azure-portal).
* Per il modulo ' AZ ' in PowerShell, eseguire questo cmdlet. Dovrebbe essere completato come "Registrato".

    ```powershell
    Register-AzProviderFeature -FeatureName "HanaBackup" –ProviderNamespace Microsoft.RecoveryServices
    ```
* Se si usa il modulo ' AzureRM ' in PowerShell, eseguire questo cmdlet. Dovrebbe essere completato come "Registrato".

    ```powershell
    Register-AzureRmProviderFeature -FeatureName "HanaBackup" –ProviderNamespace Microsoft.RecoveryServices
    ```
    

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-the-databases"></a>Individuare i database

1. Nell'insieme di credenziali, in **Attività iniziali**, fare clic su **Backup**. In **Posizione di esecuzione del carico di lavoro** selezionare **SAP HANA in una macchina virtuale di Azure**.
2. Fare clic su **Avvia individuazione**. Viene avviata l'individuazione delle macchine virtuali Linux non protette nell'area dell'insieme di credenziali.

   * Al termine dell'individuazione, le macchine virtuali non protette vengono visualizzate nel portale, elencate in base al nome e al gruppo di risorse.
   * Se una macchina virtuale non è elencata come previsto, controllare se è già stato eseguito il backup in un insieme di credenziali.
   * Più macchine virtuali possono avere lo stesso nome ma appartengono a gruppi di risorse diversi.

3. In **Seleziona macchine virtuali** fare clic sul collegamento per scaricare lo script che fornisce le autorizzazioni per il servizio Backup di Azure per accedere alle macchine virtuali SAP HANA per l'individuazione del database.
4. Eseguire lo script in ogni macchina virtuale che ospita SAP HANA database di cui si vuole eseguire il backup.
5. Dopo aver eseguito lo script nelle VM, selezionare le VM in **Seleziona macchine virtuali**. Quindi fare clic su **Individua database**.
6. Backup di Azure individua tutti i database SAP HANA presenti nella macchina virtuale. Durante l'individuazione, Backup di Azure registra la VM con l'insieme di credenziali e installa l'estensione. Nel database non vengono installati agenti.

    ![Individuazione di database SAP HANA](./media/backup-azure-sap-hana-database/hana-discover.png)

## <a name="configure-backup"></a>Configurare il backup  

A questo punto, abilitare il backup.

1. Nel passaggio 2 fare clic su **Configura backup**.

    ![Configurare il servizio Backup](./media/backup-azure-sap-hana-database/configure-backup.png)
2. In **Seleziona elementi di cui eseguire il backup**selezionare tutti i database che si desidera proteggere > **OK**.

    ![Seleziona elementi per backup](./media/backup-azure-sap-hana-database/select-items.png)
3. In **criteri di backup** > **scegliere criteri**di backup, creare un nuovo criterio di backup per i database, in base alle istruzioni riportate di seguito.

    ![Scegliere i criteri di backup](./media/backup-azure-sap-hana-database/backup-policy.png)
4. Dopo aver creato il criterio, scegliere **Abilita backup**dal menu **backup** .

    ![Abilita backup](./media/backup-azure-sap-hana-database/enable-backup.png)
5. Per tenere traccia dello stato di avanzamento della configurazione di backup, vedere l'area **Notifiche** del portale.

### <a name="create-a-backup-policy"></a>Creare un criterio di backup

Un criterio di backup definisce quando vengono acquisiti i backup e per quanto tempo vengono conservati.

* Un criterio viene creato a livello di insieme di credenziali.
* Più insiemi di credenziali possono usare gli stessi criteri di backup, ma è necessario applicare i criteri di backup a ogni insieme di credenziali.

Specificare le impostazioni del criterio come segue:

1. In **Nome criterio** immettere un nome per il nuovo criterio.

   ![Immettere il nome del criterio](./media/backup-azure-sap-hana-database/policy-name.png)
2. Nel criterio **Backup completo** selezionare una **frequenza di backup** scegliendo **Giornaliero** o **Settimanale**.
   * **Giornaliero**: selezionare l'ora e il fuso orario in cui inizia il processo di backup.
       * È necessario eseguire un backup completo. Non è possibile disattivare questa opzione.
       * Fare clic su **Backup completo** per visualizzare il criterio.
       * Se si sceglie di eseguire backup completi giornalieri, non è possibile creare backup differenziali.
   * **Settimanale**: selezionare il giorno della settimana, l'ora e il fuso orario in cui viene eseguito il processo di backup.

   ![Selezionare la frequenza di backup](./media/backup-azure-sap-hana-database/backup-frequency.png)

3. In **Intervallo conservazione** configurare le impostazioni di conservazione per il backup completo.
    * Per impostazione predefinita, tutte le opzioni sono selezionate. Cancellare i limiti del periodo di mantenimento dati che non si vuole usare e impostarli.
    * Il periodo di conservazione minimo di qualsiasi tipo di backup (completo/differenziale/del log) è di sette giorni.
    * I punti di recupero vengono contrassegnati per la conservazione, in base al relativo intervallo. Ad esempio, se si seleziona un backup completo giornaliero, viene attivato solo un backup completo ogni giorno.
    * Il backup di un giorno specifico viene contrassegnato e conservato in base all'intervallo e all'impostazione di conservazione settimanale.
    * L'intervallo di conservazione mensile e annuale si comporta allo stesso modo.

4. Nel menu **Criteri di backup completo** fare clic su **OK** per accettare le impostazioni.
5. Selezionare **backup differenziale** per aggiungere un criterio differenziale.
6. Nel **criterio Backup differenziale** selezionare **Abilita** per accedere alle opzioni di frequenza e conservazione.
    * Al massimo, è possibile attivare un backup differenziale al giorno.
    * I backup differenziali possono essere conservati al massimo per 180 giorni. Se è necessario conservarli più a lungo, usare i backup completi.

    ![Criteri di backup differenziale](./media/backup-azure-sap-hana-database/differential-backup-policy.png)

    > [!NOTE]
    > I backup incrementali non sono attualmente supportati.

7. Fare clic su **OK** per salvare il criterio e tornare nel menu principale **Criteri di backup**.
8. Selezionare **Backup del log** per aggiungere un criterio per i backup del log delle transazioni.
    * In **backup del log**selezionare **Abilita**.  Questa opzione non può essere disabilitata perché SAP HANA gestisce tutti i backup del log.
    * Impostare la frequenza e i controlli di conservazione.

    > [!NOTE]
    > Il flusso dei backup del log inizia solo dopo il completamento di un backup completo.

9. Fare clic su **OK** per salvare il criterio e tornare nel menu principale **Criteri di backup**.
10. Dopo aver completato la definizione dei criteri di backup, fare clic su **OK**.

> [!NOTE]
> Ogni backup del log viene concatenato al backup completo precedente per formare una catena di recupero. Questo backup completo verrà mantenuto fino alla scadenza della conservazione dell'ultimo backup del log. Questo potrebbe significare che il backup completo viene mantenuto per un periodo aggiuntivo per assicurarsi che tutti i log possano essere ripristinati. Si supponga che l'utente disponga di un backup completo settimanale, dei registri differenziali giornalieri e di 2 ore. Tutti vengono conservati per 30 giorni. Tuttavia, l'intero settimanale può essere effettivamente pulito/eliminato solo dopo che è disponibile il backup completo successivo, ad esempio dopo 30 + 7 giorni. Supponiamo che un backup completo settimanale avvenga il 16 novembre. In base ai criteri di conservazione, deve essere mantenuto fino al 16 dicembre. L'ultimo backup del log per questa versione completa si verifica prima del successivo programma completo, il 22 novembre. Fino a quando questo log non sarà disponibile fino al 22 dicembre, non sarà possibile eliminare il 16 novembre completo. Il 16 novembre, quindi, viene mantenuto fino al 22 dicembre.

## <a name="run-an-on-demand-backup"></a>Eseguire un backup su richiesta

I backup vengono eseguiti in base alla pianificazione dei criteri. È possibile eseguire un backup su richiesta nel modo seguente:

1. Selezionare **Elementi di backup** dal menu dell'insieme di credenziali.
2. In **elementi di backup**selezionare la macchina virtuale che esegue il database di SAP Hana, quindi fare clic su **Esegui backup ora**.
3. In **backup ora**usare il controllo calendario per selezionare l'ultimo giorno in cui deve essere mantenuto il punto di ripristino. Fare quindi clic su **OK**.
4. Monitorare le notifiche del portale. È possibile monitorare l'avanzamento del processo nel dashboard dell'insieme di credenziali > **Processi di Backup** > **In corso**. A seconda delle dimensioni del database, la creazione del backup iniziale potrebbe richiedere qualche minuto.

## <a name="run-sap-hana-studio-backup-on-a-database-with-azure-backup-enabled"></a>Eseguire il backup di SAP HANA studio su un database con backup di Azure abilitato

Se si vuole eseguire un backup locale (usando HANA Studio) di un database di cui viene eseguito il backup con backup di Azure, eseguire le operazioni seguenti:

1. Attendere il completamento di tutti i backup completi o del log per il database. Verificare lo stato in SAP HANA Studio/Cockpit.
2. Disabilitare i backup del log e impostare il catalogo di backup sul file system per il database pertinente.
3. A tale scopo, fare doppio clic su **systemdb** > **Configuration** > **selezionare database** > **Filter (log)** .
4. Impostare **enable_auto_log_backup** su **No**.
5. Impostare **log_backup_using_backint** su **false**.
6. Eseguire un backup completo su richiesta del database.
7. Attendere il completamento del backup completo e del catalogo.
8. Ripristinare le impostazioni precedenti a quelle per Azure:
    * Impostare **enable_auto_log_backup** su **Sì**.
    * Impostare **log_backup_using_backint** su **true**.

## <a name="next-steps"></a>Passaggi successivi

* Informazioni su come [ripristinare i database SAP HANA in esecuzione nelle VM di Azure](https://docs.microsoft.com/azure/backup/sap-hana-db-restore)
* Informazioni su come [gestire SAP Hana database di cui viene eseguito il backup con backup di Azure](https://docs.microsoft.com/azure/backup/sap-hana-db-manage)
