---
title: Gestire e monitorare i backup degli agenti MARS
description: Informazioni su come gestire e monitorare i backup degli agenti Servizi di ripristino di Microsoft Azure (MARS) usando il servizio backup di Azure.
ms.reviewer: srinathv
ms.topic: conceptual
ms.date: 10/07/2019
ms.openlocfilehash: c11d73edd32c197aac2cec58eeb1cc20e5c6a339
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78673256"
---
# <a name="manage-microsoft-azure-recovery-services-mars-agent-backups-by-using-the-azure-backup-service"></a>Gestire i backup degli agenti Servizi di ripristino di Microsoft Azure (MARS) con il servizio backup di Azure

Questo articolo descrive come gestire i file e le cartelle di cui viene eseguito il backup con l'agente di Servizi di ripristino di Microsoft Azure.

## <a name="modify-a-backup-policy"></a>Modificare un criterio di backup

Quando si modificano i criteri di backup, è possibile aggiungere nuovi elementi, rimuovere gli elementi esistenti dal backup o escludere i file dal backup usando le impostazioni di esclusione.

- **Aggiungi elementi** usare questa opzione solo per aggiungere nuovi elementi per il backup. Per rimuovere gli elementi esistenti, utilizzare l'opzione **Rimuovi elementi** o **impostazioni di esclusione** .  
- **Rimuovi elementi** usare questa opzione per rimuovere gli elementi dal backup.
  - Usare **le impostazioni di esclusione** per rimuovere tutti gli elementi all'interno di un volume anziché **rimuovere gli elementi**.
  - La cancellazione di tutte le selezioni in un volume comporta la conservazione dei backup obsoleti degli elementi in base alle impostazioni di conservazione al momento dell'ultimo backup, senza ambito per la modifica.
  - Riselezionando questi elementi, viene effettuato un primo backup completo e le nuove modifiche ai criteri non vengono applicate ai backup precedenti.
  - La deselezione di un intero volume mantiene il backup passato senza alcun ambito per la modifica dei criteri di conservazione.
- **Impostazioni di esclusione** usare questa opzione per escludere elementi specifici dal backup.

### <a name="add-new-items-to-existing-policy"></a>Aggiungi nuovi elementi a criteri esistenti

1. In **azioni**fare clic su **Pianifica backup**.

    ![Pianificare un backup di Windows Server](./media/backup-configure-vault/schedule-first-backup.png)

2. Nella scheda **Seleziona elemento criteri** selezionare **Modifica pianificazione backup per file e cartelle** e fare clic su **Avanti**.

    ![Seleziona elementi criterio](./media/backup-azure-manage-mars/select-policy-items.png)

3. Nella scheda **modifica o Interrompi pianificazione backup** selezionare **apportare modifiche a elementi o orari di backup** e fare clic su **Avanti**.

    ![Modificare o pianificare il backup](./media/backup-azure-manage-mars/modify-schedule-backup.png)

4. Nella scheda **selezionare gli elementi da** sottoporre a backup fare clic su **Aggiungi elementi** per aggiungere gli elementi di cui si desidera eseguire il backup.

    ![Modificare o pianificare l'aggiunta di elementi di backup](./media/backup-azure-manage-mars/modify-schedule-backup-add-items.png)

5. Nella finestra **Seleziona elementi** selezionare le mosche o le cartelle che si desidera aggiungere e fare clic su **OK**.

    ![Selezionare gli elementi](./media/backup-azure-manage-mars/select-item.png)

6. Completare i passaggi successivi e fare clic su **fine** per completare l'operazione.

### <a name="add-exclusion-rules-to-existing-policy"></a>Aggiungere regole di esclusione a criteri esistenti

È possibile aggiungere regole di esclusione per ignorare i file e le cartelle di cui non si vuole eseguire il backup. Questa operazione può essere eseguita durante la definizione di un nuovo criterio o la modifica di un criterio esistente.

1. Dal riquadro azioni fare clic su **Pianifica backup**. Passare a **selezionare gli elementi di cui eseguire il backup** e fare clic su **impostazioni di esclusione**.

    ![Selezionare gli elementi](./media/backup-azure-manage-mars/select-exclusion-settings.png)

2. In **impostazioni di esclusione**fare clic su **Aggiungi esclusione**.

    ![Selezionare gli elementi](./media/backup-azure-manage-mars/add-exclusion.png)

3. Da **selezionare gli elementi da escludere**, sfogliare i file e le cartelle e selezionare gli elementi che si desidera escludere, quindi fare clic su **OK**.

    ![Selezionare gli elementi](./media/backup-azure-manage-mars/select-items-exclude.png)

4. Per impostazione predefinita, tutte le **sottocartelle** all'interno delle cartelle selezionate sono escluse. È possibile modificare questa impostazione selezionando **Sì** o **No**. È possibile modificare e specificare i tipi di file da escludere come illustrato di seguito:

    ![Selezionare gli elementi](./media/backup-azure-manage-mars/subfolders-type.png)

5. Completare i passaggi successivi e fare clic su **fine** per completare l'operazione.

### <a name="remove-items-from-existing-policy"></a>Rimuovi elementi dal criterio esistente

1. Dal riquadro azioni fare clic su **Pianifica backup**. Passare a **selezionare gli elementi di cui eseguire il backup**. Nell'elenco selezionare i file e le cartelle che si desidera rimuovere dalla pianificazione del backup e fare clic su **Rimuovi elementi**.

    ![Selezionare gli elementi](./media/backup-azure-manage-mars/select-items-remove.png)

> [!NOTE]
> Quando si rimuove completamente un volume dal criterio, procedere con cautela.  Se è necessario aggiungerlo di nuovo, verrà considerato come un nuovo volume. Il backup pianificato successivo eseguirà un backup iniziale (backup completo) invece del backup incrementale. Se è necessario rimuovere e aggiungere temporaneamente elementi in un secondo momento, è consigliabile usare **le impostazioni di esclusione** anziché **rimuovere gli elementi** per assicurare il backup incrementale anziché il backup completo.

2. Completare i passaggi successivi e fare clic su **fine** per completare l'operazione.

## <a name="stop-protecting-files-and-folder-backup"></a>Arrestare la protezione dei file e del backup delle cartelle

Esistono due modi per arrestare la protezione del backup di file e cartelle:

- **Arrestare la protezione e conservare i dati di backup**.
  - Questa opzione consente di arrestare tutti i processi di backup futuri dalla protezione dati.
  - Il servizio backup di Azure manterrà i punti di ripristino di cui è stato eseguito il backup in base ai criteri di conservazione.
  - Sarà possibile ripristinare i dati di cui è stato eseguito il backup per i punti di ripristino non scaduti.
  - Se si decide di riprendere la protezione, è possibile usare l'opzione *riabilita pianificazione backup* . Successivamente, i dati verrebbero mantenuti in base ai nuovi criteri di conservazione.
- **Arrestare la protezione ed eliminare i dati di backup**.
  - Questa opzione consente di arrestare tutti i processi di backup futuri per proteggere i dati ed eliminare tutti i punti di ripristino.
  - Si riceverà un messaggio di posta elettronica di avviso per l'eliminazione dei dati di backup con un messaggio che *i dati per questo elemento di backup sono stati eliminati. Questi dati saranno temporaneamente disponibili per 14 giorni, dopo i quali verranno eliminati definitivamente* e l'azione consigliata *riproteggere l'elemento di backup entro 14 giorni per ripristinare i dati.*
  - Per riprendere la protezione, riproteggere entro 14 giorni dall'operazione di eliminazione.

### <a name="stop-protection-and-retain-backup-data"></a>Arrestare la protezione e conservare i dati di backup

1. Aprire la console di gestione di MARS, passare al **riquadro azioni**e **selezionare Pianifica backup**.

    ![Modificare o arrestare un backup pianificato.](./media/backup-azure-manage-mars/mars-actions.png)
1. Nella pagina **Selezione elemento criteri** selezionare **Modifica pianificazione backup per file e cartelle** e fare clic su **Avanti**.

    ![Modificare o arrestare un backup pianificato.](./media/backup-azure-manage-mars/select-policy-item-retain-data.png)
1. Nella pagina **modificare o arrestare un backup pianificato** selezionare **Interrompi utilizzando questa pianificazione del backup, ma Mantieni i backup archiviati fino a quando non viene attivata nuovamente una pianificazione**. Quindi selezionare **Avanti**.

    ![Modificare o arrestare un backup pianificato.](./media/backup-azure-manage-mars/stop-schedule-backup.png)
1. In **Sospendi backup pianificato** esaminare le informazioni e fare clic su **fine**.

    ![Modificare o arrestare un backup pianificato.](./media/backup-azure-manage-mars/pause-schedule-backup.png)
1. In **Modifica processo di backup** controllare che la pianificazione della sospensione del backup sia in stato di esito positivo e fare clic su **Chiudi** per terminare.

### <a name="stop-protection-and-delete-backup-data"></a>Arrestare la protezione ed eliminare i dati di backup

1. Aprire la console di gestione di MARS, passare al riquadro **azioni** e selezionare **Pianifica backup**.
2. Nella pagina **modificare o arrestare un backup pianificato** selezionare **Interrompi utilizzo di questa pianificazione del backup ed eliminare tutti i backup archiviati**. Quindi selezionare **Avanti**.

    ![Modificare o arrestare un backup pianificato.](./media/backup-azure-delete-vault/modify-schedule-backup.png)

3. Nella pagina **Arresta un backup pianificato** selezionare **fine**.

    ![Arrestare un backup pianificato.](./media/backup-azure-delete-vault/stop-schedule-backup.png)
4. Viene richiesto di immettere un PIN di sicurezza (Personal Identification Number), che è necessario generare manualmente. A tale scopo, accedere prima al portale di Azure.
5. Passare a insieme di credenziali di **servizi di ripristino** > **Impostazioni** > **proprietà**.
6. In **pin di sicurezza**selezionare **genera**. Copiare questo PIN. Il PIN è valido solo per cinque minuti.
7. Nella console di gestione incollare il PIN e quindi fare clic su **OK**.

    ![Generare un PIN di sicurezza.](./media/backup-azure-delete-vault/security-pin.png)

8. Nella pagina **modifica stato backup** viene visualizzato il messaggio seguente: i *dati di backup eliminati verranno conservati per 14 giorni. Dopo tale periodo, i dati di backup verranno eliminati definitivamente.*  

    ![Eliminare l'infrastruttura di backup.](./media/backup-azure-delete-vault/deleted-backup-data.png)

Dopo aver eliminato gli elementi di backup locali, seguire i passaggi successivi dal portale.

## <a name="re-enable-protection"></a>Abilitare di nuovo la protezione

Se la protezione è stata interrotta durante la conservazione dei dati e si è deciso di riprendere la protezione, è possibile riattivare la pianificazione del backup utilizzando modifica criteri di backup.

1. In **azioni** selezionare **Pianifica backup**.
1. Selezionare **riabilita pianificazione backup. È anche possibile modificare gli elementi o le ore di backup** e fare clic su **Avanti**.<br>

    ![Eliminare l'infrastruttura di backup.](./media/backup-azure-manage-mars/re-enable-policy-next.png)
1. In **Seleziona elementi di cui eseguire il backup**fare clic su **Avanti**.

    ![Eliminare l'infrastruttura di backup.](./media/backup-azure-manage-mars/re-enable-next.png)
1. In **specificare la pianificazione del backup**specificare la pianificazione del backup e fare clic su **Avanti**.
1. In **Seleziona criteri di conservazione**specificare durata conservazione e fare clic su **Avanti**.
1. Infine, nella schermata di **conferma** esaminare i dettagli dei criteri e fare clic su **fine**.

## <a name="re-generate-passphrase"></a>Genera di nuovo passphrase

Viene usata una passphrase per crittografare e decrittografare i dati durante il backup o il ripristino del computer locale o locale usando l'agente MARS da o verso Azure. Se la passphrase è stata persa o dimenticata, è possibile rigenerare la passphrase (purché il computer sia ancora registrato con l'insieme di credenziali di servizi di ripristino e il backup sia configurato) attenendosi alla procedura seguente:

- Dalla console dell'agente MARS andare al **riquadro azioni** > **modificare le proprietà** >. Passare quindi alla **scheda crittografia**.<br>
- Selezionare la casella di controllo **Cambia passphrase** .<br>
- Immettere una nuova passphrase o fare clic su **genera passphrase**.
- Fare clic su **Sfoglia** per salvare la nuova passphrase.

    ![Genera passphrase.](./media/backup-azure-manage-mars/passphrase.png)
- Fare clic su **OK** per applicare le modifiche.  Se la [funzionalità di sicurezza](https://docs.microsoft.com/azure/backup/backup-azure-security-feature#enable-security-features) è abilitata nella portale di Azure per l'insieme di credenziali di servizi di ripristino, verrà richiesto di immettere il pin di sicurezza. Per ricevere il PIN, seguire i passaggi elencati in questo [articolo](https://docs.microsoft.com/azure/backup/backup-azure-security-feature#authentication-to-perform-critical-operations).<br>
- Incollare il PIN di sicurezza dal portale e fare clic su **OK** per applicare le modifiche.<br>

    ![Genera passphrase.](./media/backup-azure-manage-mars/passphrase2.png)
- Verificare che la passphrase venga salvata in modo sicuro in un percorso alternativo (diverso dalla macchina di origine), preferibilmente nella Azure Key Vault. Tenere traccia di tutte le passphrase se è stato eseguito il backup di più computer con gli agenti MARS.


## <a name="next-steps"></a>Passaggi successivi

- Per informazioni sugli scenari e le limitazioni supportati, vedere la [matrice di supporto per l'agente Mars](https://docs.microsoft.com/azure/backup/backup-support-matrix-mars-agent).
- Altre informazioni sul [comportamento di conservazione dei criteri di backup su richiesta](backup-windows-with-mars-agent.md#set-up-on-demand-backup-policy-retention-behavior).
