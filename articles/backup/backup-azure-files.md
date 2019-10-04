---
title: Eseguire il backup e il ripristino di condivisioni file di Azure
description: Questo articolo descrive come eseguire il backup e il ripristino delle condivisioni file di Azure e spiega le attività di gestione.
author: dcurwin
ms.author: dacurwin
ms.date: 07/29/2019
ms.topic: tutorial
ms.service: backup
manager: carmonm
ms.openlocfilehash: 44a2b0feab19d042de58359a7ea13814415e6c9e
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/19/2019
ms.locfileid: "71129559"
---
# <a name="back-up-and-restore-azure-file-shares"></a>Eseguire il backup e il ripristino di condivisioni file di Azure
Questo articolo illustra come usare il portale di Azure per eseguire il backup e il ripristino delle [condivisioni file di Azure](../storage/files/storage-files-introduction.md).

Questa guida illustra come eseguire queste operazioni:
> [!div class="checklist"]
> * Configurare un insieme di credenziali di Servizi di ripristino per eseguire il backup di file di Azure
> * Eseguire un processo di backup su richiesta per creare un punto di ripristino
> * Ripristinare uno o più file da un punto di ripristino
> * Gestire i processi di backup
> * Interrompere la protezione in file di Azure
> * Eliminare i dati di backup

## <a name="prerequisites"></a>Prerequisiti
Prima di eseguire il backup di una condivisione file di Azure, assicurarsi che la condivisione si trovi in uno dei [tipi di account di archiviazione supportati](backup-azure-files.md#limitations-for-azure-file-share-backup-during-preview). Dopo questa verifica è possibile proteggere le condivisioni file.

## <a name="limitations-for-azure-file-share-backup-during-preview"></a>Limitazioni per il backup delle condivisioni file di Azure durante l'anteprima
Il backup per le condivisioni file di Azure è disponibile in anteprima. Le condivisioni file di Azure sono supportate negli account di archiviazione per utilizzo generico sia v1 che v2. Gli scenari di backup seguenti non sono supportati nelle condivisioni file di Azure:
- Il supporto per il backup delle condivisioni file di Azure negli account di archiviazione con replica di [archiviazione con ridondanza della zona](../storage/common/storage-redundancy-zrs.md) (ZRS) è attualmente limitato a [queste aree](backup-azure-files-faq.md#in-which-geos-can-i-back-up-azure-file-shares-).
- Per la protezione di File di Azure con Backup di Azure non è disponibile l'interfaccia della riga di comando.
- Backup di Azure supporta attualmente la configurazione di backup pianificati una volta al giorno per le condivisioni file di Azure.
- Il numero massimo di backup pianificati al giorno è uno.
- Il numero massimo di backup su richiesta al giorno è quattro.
- Usare i [blocchi delle risorse](https://docs.microsoft.com/cli/azure/resource/lock?view=azure-cli-latest) nell'account di archiviazione per impedire l'eliminazione accidentale dei backup nell'insieme di credenziali di Servizi di ripristino.
- Non eliminare gli snapshot creati da Backup di Azure. L'eliminazione degli snapshot può comportare la perdita di punti di ripristino e/o errori di ripristino.
- Non eliminare le condivisioni file protette mediante Backup di Azure. La soluzione corrente implica l'eliminazione di tutti gli snapshot creati da Backup di Azure dopo l'eliminazione della condivisione file, causando la perdita di tutti i punti di ripristino.



## <a name="configuring-backup-for-an-azure-file-share"></a>Configurazione del backup per una condivisione file di Azure
In questa esercitazione si presuppone che sia già disponibile una condivisione file di Azure. Per eseguire il backup della condivisione file di Azure:

1. Creare un insieme di credenziali di Servizi di ripristino nella stessa area della condivisione file. Se è già disponibile un insieme di credenziali, aprire la pagina Panoramica correlata e fare clic su **Backup**.

    ![Scegliere Condivisione file di Azure come obiettivo di backup](./media/backup-file-shares/overview-backup-page.png)

2. Nel menu **Obiettivo di backup** scegliere Condivisione file di Azure da **Elementi di cui eseguire il backup**.

    ![Scegliere Condivisione file di Azure come obiettivo di backup](./media/backup-file-shares/choose-azure-fileshare-from-backup-goal.png)

3. Fare clic su **Backup** per configurare la condivisione file di Azure nell'insieme di credenziali di Servizi di ripristino.

   ![Fare clic su Backup per associare la condivisione file di Azure all'insieme di credenziali](./media/backup-file-shares/set-backup-goal.png)

    Dopo aver associato l'insieme di credenziali alla condivisione file di Azure, si aprirà il menu Backup che chiede selezionare un account di archiviazione. Il menu visualizza tutti gli account di archiviazione supportati presenti nell'area in cui si trova l'insieme di credenziali e non già associati a un insieme di credenziali di Servizi di ripristino.

   ![Fare clic su Backup per associare la condivisione file di Azure all'insieme di credenziali](./media/backup-file-shares/list-of-storage-accounts.png)

4. Nell'elenco degli account di archiviazione selezionare un account e fare clic su **OK**. Azure cerca nell'account di archiviazione le condivisioni file di cui è possibile eseguire il backup. Se sono state recentemente aggiunte condivisioni file e queste non vengono visualizzate nell'elenco, attendere qualche istante.

   ![Fare clic su Backup per associare la condivisione file di Azure all'insieme di credenziali](./media/backup-file-shares/discover-file-shares.png)

5. Dall'elenco **Condivisioni file** selezionare una o più condivisioni file delle quali eseguire il backup e fare clic su **OK**.

6. Dopo aver scelto le condivisioni file, il menu Backup passa a **Criteri di backup**. In questo menu selezionare un criterio di backup esistente oppure crearne uno nuovo, quindi fare clic su **Abilita backup**.

   ![Fare clic su Backup per associare la condivisione file di Azure all'insieme di credenziali](./media/backup-file-shares/apply-backup-policy.png)

    Dopo aver stabilito un criterio di backup verrà creato uno snapshot delle condivisioni file all'orario pianificato e il punto di ripristino verrà conservato per il periodo scelto.

## <a name="create-an-on-demand-backup"></a>Creare un backup su richiesta
A volte si vuole generare uno snapshot di backup, o punto di ripristino, fuori dall'orario pianificato nel criterio di backup. Un momento comune per la generazione di un backup su richiesta è subito dopo aver configurato il criterio di backup. A seconda della pianificazione indicata nel criterio di backup, possono volerci ore o giorni prima che venga creato uno snapshot. Per proteggere i dati fino a quando non si attiva il criterio di backup, avviare un backup su richiesta. La creazione di un backup su richiesta è spesso necessaria prima di apportare modifiche pianificate alle condivisioni file.

### <a name="to-create-an-on-demand-backup"></a>Per creare un backup su richiesta

1. Aprire l'insieme di credenziali di Servizi di ripristino che contiene i punti di ripristino della condivisione file, quindi fare clic su **Elementi di backup**. Verrà visualizzato l'elenco degli elementi di backup.

   ![Fare clic su Backup per associare la condivisione file di Azure all'insieme di credenziali](./media/backup-file-shares/list-of-backup-items.png)

2. Dall'elenco selezionare **Archiviazione di Azure (File di Azure)** . Verrà visualizzato l'elenco delle condivisioni file di Azure.

   ![Fare clic su Backup per associare la condivisione file di Azure all'insieme di credenziali](./media/backup-file-shares/list-of-azure-files-backup-items.png)

3. Dall'elenco delle condivisioni file di Azure selezionare la condivisione file desiderata. Verranno visualizzati i dettagli di **Elemento di backup**. Nel menu **Elemento di backup** fare clic su **Esegui backup ora**. Trattandosi di un processo di backup su richiesta, non sono presenti criteri di conservazione associati al punto di ripristino.

   ![Fare clic su Backup per associare la condivisione file di Azure all'insieme di credenziali](./media/backup-file-shares/backup-item-menu.png)

4. Verrà visualizzata la finestra di dialogo **Esegui backup ora**. Specificare l'ultimo giorno di conservazione del punto di ripristino.

   ![Fare clic su Backup per associare la condivisione file di Azure all'insieme di credenziali](./media/backup-file-shares/backup-now-menu.png)

5. Fare clic su **OK** per confermare il processo di backup su richiesta.

## <a name="restore-from-backup-of-azure-file-share"></a>Eseguire il ripristino da un backup di condivisioni file di Azure
Se è necessario ripristinare un'intera condivisione file oppure singoli file o cartelle da un punto di ripristino, passare all'elemento di backup come descritto dettagliatamente nella sezione precedente. Scegliere **Ripristina condivisione** per ripristinare un'intera condivisione file da un punto di ripristino. Dall'elenco dei punti di ripristino visualizzati, selezionarne uno per sovrascrivere la condivisione file corrente oppure ripristinarlo in un'altra condivisione file della stessa area.

   ![Fare clic su Backup per associare la condivisione file di Azure all'insieme di credenziali](./media/backup-file-shares/select-restore-location.png)

## <a name="restore-individual-files-or-folders-from-backup-of-azure-file-shares"></a>Ripristinare singoli file o cartelle dal backup di condivisioni file di Azure
Backup di Azure consente di individuare un punto di ripristino nel portale di Azure. Per ripristinare un file o una cartella, fare clic su Recupero file nella pagina Elemento di backup e scegliere un punto di ripristino dall'elenco. Selezionare la destinazione del ripristino e quindi fare clic su **Seleziona file** per individuare il punto di ripristino. Selezionare il file o la cartella e fare clic su **Ripristina**.

   ![Fare clic su Backup per associare la condivisione file di Azure all'insieme di credenziali](./media/backup-file-shares/restore-individual-files-folders.png)

## <a name="manage-azure-file-share-backups"></a>Gestire i backup di condivisioni file di Azure

Nella pagina **Processi di backup** è possibile eseguire diverse attività di gestione per i backup di condivisioni file di Azure, tra cui:
- [Monitorare i processi](backup-azure-files.md#monitor-jobs)
- [Creare nuovi criteri](backup-azure-files.md#create-a-new-policy)
- [Interrompere la protezione in una condivisione file](backup-azure-files.md#stop-protecting-an-azure-file-share)
- [Riprendere la protezione in una condivisione file](backup-azure-files.md#resume-protection-for-azure-file-share)
- [Eliminare i dati di backup](backup-azure-files.md#delete-backup-data)

### <a name="monitor-jobs"></a>Monitorare i processi

È possibile monitorare lo stato di avanzamento di tutti i processi nella pagina **Processi di backup**.

Per aprire la pagina **Processi di backup**:

- Aprire l'insieme di credenziali di Servizi di ripristino che si vuole monitorare e nel relativo menu fare clic su **Processi** e quindi su **Processi di backup**.

   ![Selezionare il processo da monitorare](./media/backup-file-shares/open-backup-jobs.png)

    Verrà visualizzato l'elenco dei processi di backup con il relativo stato.

    ![Selezionare il processo da monitorare](./media/backup-file-shares/backup-jobs-progress-list.png)

### <a name="create-a-new-policy"></a>Creare nuovi criteri

È possibile creare un nuovo criterio per il backup di condivisioni file di Azure dai **criteri di backup** dell'insieme di credenziali di Servizi di ripristino. Tutti i criteri creati quando si configura il backup delle condivisioni file vengono visualizzati con il tipo di criteri "Condivisione file di Azure".

Per visualizzare i criteri di backup esistenti:

- Aprire l'insieme di credenziali di Servizi di ripristino desiderato e nel relativo menu fare clic su **Criteri di backup**. Verranno elencati tutti i criteri di backup.

   ![Selezionare il processo da monitorare](./media/backup-file-shares/list-of-backup-policies.png)

Per creare un nuovo criterio di backup:

1. Nel menu Insieme di credenziali di Servizi di ripristino fare clic su **Criteri di backup**.
2. Nell'elenco dei criteri di backup fare clic su **Aggiungi**.

   ![Selezionare il processo da monitorare](./media/backup-file-shares/new-backup-policy.png)

3. Nel menu **Aggiungi** selezionare **Condivisione file di Azure**. Verrà visualizzato il menu Criteri di backup per la condivisione file di Azure. Specificare il nome del criterio, la frequenza del backup e l'intervallo di conservazione dei punti di ripristino. Al termine della definizione del criterio, fare clic su OK.

   ![Selezionare il processo da monitorare](./media/backup-file-shares/create-new-policy.png)

### <a name="stop-protecting-an-azure-file-share"></a>Interrompere la protezione di una condivisione file di Azure

Se si sceglie di interrompere la protezione di una condivisione file di Azure, viene chiesto se si vogliono conservare i punti di ripristino. Per interrompere la protezione di condivisioni file di Azure è possibile procedere in due modi:

- Arrestare tutti i processi di backup futuri ed eliminare tutti i punti di ripristino oppure
- Interrompere tutti i processi di backup futuri mantenendo però i punti di ripristino

Può essere associato un costo alla conservazione dei punti di ripristino nella risorsa di archiviazione, dato che verranno conservati gli snapshot sottostanti creati da Backup di Azure. Il vantaggio offerto dalla conservazione dei punti di ripristino è tuttavia la possibilità di ripristinare eventualmente la condivisione file in un secondo momento. Per informazioni sul costo associato alla conservazione dei punti di ripristino, vedere i dettagli dei prezzi. Se si sceglie di eliminare tutti i punti di ripristino, non sarà possibile ripristinare la condivisione file.

Per interrompere la protezione di una condivisione file di Azure:

1. Aprire l'insieme di credenziali di Servizi di ripristino che contiene i punti di ripristino della condivisione file, quindi fare clic su **Elementi di backup**. Verrà visualizzato l'elenco degli elementi di backup.

   ![Fare clic su Backup per associare la condivisione file di Azure all'insieme di credenziali](./media/backup-file-shares/list-of-backup-items.png)

2. Nell'elenco **Tipo di gestione di backup** selezionare **Archiviazione di Azure (File di Azure)** . Verrà visualizzato l'elenco Elementi di backup (Archiviazione di Azure (File di Azure)).

   ![Fare clic su un elemento per aprire un altro menu](./media/backup-file-shares/azure-file-share-backup-items.png)

3. Nell'elenco Elementi di backup (Archiviazione di Azure (File di Azure)) selezionare l'elemento di backup da interrompere.

4. Negli elementi della condivisione file di Azure fare clic sul menu **Altro** e selezionare **Interrompi backup**.

   ![Fare clic su un elemento per aprire un altro menu](./media/backup-file-shares/stop-backup.png)

5. Nel menu Interrompi backup scegliere se **conservare** o **eliminare i dati di backup** e quindi fare clic su **Interrompi backup**.

   ![Fare clic su un elemento per aprire un altro menu](./media/backup-file-shares/retain-data.png)

### <a name="resume-protection-for-azure-file-share"></a>Riprendere la protezione di una condivisione file di Azure

Se è stata scelta l'opzione Conserva i dati di backup quando è stata interrotta la protezione della condivisione file, è possibile riprendere la protezione. Se è stata scelta l'opzione **Elimina dati di backup**, la protezione della condivisione file non può essere ripresa.

Per riprendere la protezione della condivisione file, passare all'elemento di backup e fare clic su Riprendi backup. Si aprirà la finestra Criteri di backup e sarà possibile scegliere un criterio per riprendere il backup.

   ![Selezionare il processo da monitorare](./media/backup-file-shares/resume-backup-job.png)

### <a name="delete-backup-data"></a>Eliminare i dati di backup

È possibile eliminare il backup di una condivisione file durante il processo di interruzione del backup oppure in qualsiasi momento dopo aver interrotto la protezione. Attendere settimane o mesi prima di eliminare i punti di ripristino potrebbe anche essere utile. A differenza del recupero dei punti di ripristino, quando si eliminano i dati di backup non è possibile scegliere di eliminare punti di ripristino specifici. Se si sceglie di eliminare i dati di backup, vengono eliminati tutti i punti di ripristino associati all'elemento.

Nella procedura seguente si presuppone che il processo di backup per la condivisione file sia stato interrotto. Dopo aver interrotto il processo di backup, nel dashboard dell'elemento di backup sono disponibili le opzioni Riprendi backup ed Elimina dati di backup. Fare clic su Elimina dati di backup e immettere il nome della condivisione file per confermare l'eliminazione. L'aggiunta di un motivo o commento per l'eliminazione è facoltativa.

## <a name="see-also"></a>Vedere anche
Per altre informazioni sulle condivisioni file di Azure, vedere
- [Domande frequenti sul backup di condivisioni file di Azure](backup-azure-files-faq.md)
- [Risolvere i problemi del backup delle condivisioni file di Azure](troubleshoot-azure-files.md)
