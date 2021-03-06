---
title: Valutare i server usando i dati importati con Valutazione server di Azure Migrate
description: Viene descritto come valutare i server locali per la migrazione ad Azure con Valutazione server di Azure Migrate con dati importati.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 10/23/2019
ms.author: raynew
ms.openlocfilehash: 91b9c71e7c735fca08f71ca37ed28734c8d634a1
ms.sourcegitcommit: 72c2da0def8aa7ebe0691612a89bb70cd0c5a436
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/10/2020
ms.locfileid: "79079859"
---
# <a name="assess-servers-by-using-imported-data"></a>Valutare i server con i dati importati

Questo articolo descrive come valutare i server locali con lo strumento [Azure Migrate: Valutazione server](migrate-services-overview.md#azure-migrate-server-assessment-tool) tramite l'importazione di metadati in formato CSV con valori delimitati da virgole. Con questo metodo, non è necessario configurare l'appliance di Azure Migrate per creare una valutazione. Risulta utile se:

- Si vuole creare una rapida valutazione iniziale prima di distribuire l'appliance.
- Non è possibile distribuire l'appliance di Azure Migrate nell'organizzazione.
- Non è possibile condividere le credenziali che consentono l'accesso ai server locali.
- I vincoli di sicurezza impediscono la raccolta e l'invio di dati raccolti dall'appliance ad Azure. È possibile controllare i dati condivisi in un file importato. Inoltre, è facoltativo specificare la maggior parte dei dati, ad esempio gli indirizzi IP.

## <a name="before-you-start"></a>Prima di iniziare

Tenere presente quanto segue:

- È possibile aggiungere fino a un massimo di 20.000 server in un singolo file CSV.
- È possibile aggiungere fino a 20.000 server in un progetto di Azure Migrate tramite CSV.
- È possibile caricare più volte le informazioni sul server in Valutazione server tramite file CSV.
- La raccolta di informazioni sulle applicazioni è utile per valutare l'ambiente locale per la migrazione. Tuttavia, Valutazione server non esegue attualmente la valutazione a livello di applicazione e non prende in considerazione le applicazioni durante la creazione di una valutazione.

In questa esercitazione verranno illustrate le procedure per:
> [!div class="checklist"]
> * Configurare un progetto di Azure Migrate.
> * Compilare un file con estensione csv con le informazioni sul server.
> * Importare il file per aggiungere informazioni sul server in Valutazione server.
> * Creare ed esaminare una valutazione.

> [!NOTE]
> Le esercitazioni illustrano il percorso di distribuzione più semplice per uno scenario, per consentire di configurare rapidamente un modello di verifica. Quando possibile vengono usate le opzioni predefinite e non sono riportati tutti i percorsi e le impostazioni possibili. Per istruzioni dettagliate, vedere le guide pratiche.

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/pricing/free-trial/) prima di iniziare.

## <a name="set-azure-permissions-for-azure-migrate"></a>Impostare le autorizzazioni di Azure per Azure Migrate

l'account Azure deve avere le autorizzazioni per creare un progetto di Azure Migrate.

1. Nel portale di Azure aprire la sottoscrizione e selezionare **Controllo di accesso (IAM)** .
2. In **Verifica l'accesso** trovare l'account pertinente e selezionarlo per visualizzare le autorizzazioni.
3. Assicurarsi di avere le autorizzazioni di **Collaboratore** o **Proprietario**.
    - Se è appena stato creato un account Azure gratuito, si è proprietari della propria sottoscrizione.
    - Se non si ha il ruolo di proprietario della sottoscrizione, collaborare con il proprietario per assegnare il ruolo.

## <a name="set-up-an-azure-migrate-project"></a>Configurare un progetto di Azure Migrate

Per configurare un nuovo progetto di Azure Migrate:

1. Nel portale di Azure cercare **Azure Migrate** in **Tutti i servizi**.
2. In **Servizi** selezionare **Azure Migrate**.
3. In **Individuare, valutare ed eseguire la migrazione dei server** della pagina **Panoramica** selezionare **Valutare ed eseguire la migrazione dei server**.

    ![Individuare e valutare i server](./media/tutorial-assess-import/assess-migrate.png)

4. In **Attività iniziali** selezionare **Aggiungi strumenti**.
5. In **Progetto di migrazione** selezionare la sottoscrizione di Azure e creare un gruppo di risorse, se non se ne ha già uno.
6. In **DETTAGLI DEL PROGETTO** specificare il nome del progetto e l'area geografica in cui lo si vuole creare. Per altre informazioni:

    - Vedere le [aree geografiche supportate](migrate-support-matrix.md#supported-geographies). L'area geografica del progetto viene usata solo per archiviare i metadati raccolti dalle macchine virtuali locali.
    - Per la migrazione è possibile selezionare qualsiasi area di destinazione.

    ![Creare un progetto di Azure Migrate](./media/tutorial-assess-import/migrate-project.png)

7. Selezionare **Avanti**.
8. In **Selezionare lo strumento di valutazione** selezionare **Azure Migrate: Valutazione server** > **Avanti**.

    ![Creare una valutazione di Azure Migrate](./media/tutorial-assess-import/assessment-tool.png)

9. In **Selezione strumento di migrazione** selezionare **Ignora l'aggiunta di uno strumento di migrazione per adesso** > **Avanti**.
10. In **Rivedi e aggiungi strumenti** rivedere le impostazioni e selezionare **Aggiungi strumenti**.
11. Attendere alcuni minuti durante la distribuzione del progetto di Azure Migrate. Verrà quindi visualizzata la pagina del progetto. Se il progetto non viene visualizzato, è possibile accedervi da **Server** nel dashboard di Azure Migrate.

## <a name="prepare-the-csv"></a>Preparare il CSV

Scaricare il modello CSV e aggiungervi le informazioni sul server.

### <a name="download-the-template"></a>Scaricare il modello

1. In **Obiettivi della migrazione** > **Server** > **Azure Migrate: Valutazione server** selezionare **Individua**.
2. In **Individua macchine virtuali**selezionare **Importa tramite file CSV**.
3. Selezionare **Scarica** per scaricare il modello CSV. In alternativa, è possibile [scaricarlo direttamente](https://go.microsoft.com/fwlink/?linkid=2109031).

    ![Scaricare il modello CSV](./media/tutorial-assess-import/download-template.png)

### <a name="add-server-information"></a>Aggiungere informazioni sul server

Raccogliere i dati sul server e aggiungerli al file CSV.

- Per raccogliere dati, è possibile esportarli dagli strumenti usati per la gestione di server locali, ad esempio VMware vSphere o il database di gestione della configurazione (CMDB).
- Per esaminare i dati di esempio, scaricare il [file di esempio](https://go.microsoft.com/fwlink/?linkid=2108405).

Nella tabella seguente vengono riepilogati i campi del file da compilare:

**Nome campo** | **Obbligatorio** | **Dettagli**
--- | --- | ---
**Nome server** | Sì | È consigliabile specificare il nome di dominio completo (FQDN).
**Indirizzo IP** | No | Indirizzo del server.
**Core** | Sì | Numero di core del processore allocati al server.
**Memoria** | Sì | RAM totale, in MB, allocata al server.
**Nome sistema operativo** | Sì | Sistema operativo server. <br/> I nomi dei sistemi operativi che corrispondono o che contengono i nomi in [questo](#supported-operating-system-names) elenco sono riconosciuti dalla valutazione.
**Versione del sistema operativo** | No | Versione del sistema operativo server.
**Numero di dischi** | No | Non necessario se sono presenti i dettagli dei singoli dischi.
**Dimensioni disco 1**  | No | Dimensioni massime del disco, in GB.<br/>È possibile aggiungere dettagli per altri dischi tramite[aggiunta di colonne](#add-multiple-disks) nel modello. È possibile aggiungere fino a otto versioni del disco.
**Operazioni di lettura disco 1** | No | Operazioni di lettura da disco al secondo.
**Operazioni di scrittura disco 1** | No | Operazioni di scrittura su disco al secondo.
**Velocità effettiva lettura da disco 1** | No | Dati letti dal disco al secondo, in MB al secondo.
**Velocità effettiva scrittura da disco 1** | No | Dati scritti sul disco al secondo, in MB al secondo.
**Percentuale utilizzo CPU** | No | Percentuale di CPU usata.
**Percentuale utilizzo memoria** | No | Percentuale di RAM usata.
**Totale operazioni di lettura dischi** | No | Operazioni di lettura da disco al secondo.
**Totale operazioni di scrittura dischi** | No | Operazioni di scrittura su disco al secondo.
**Totale velocità effettiva lettura da dischi** | No | Dati letti dal disco, in MB al secondo.
**Totale velocità effettiva scrittura da dischi** | No | Dati scritti sul disco, in MB al secondo.
**Velocità effettiva della rete in ingresso** | No | Dati ricevuti dal server, in MB al secondo.
**Velocità effettiva della rete in uscita** | No | Dati trasmessi dal server, in MB al secondo.
**Tipo di firmware** | No | Firmware del server. I valori possono essere "BIOS" o "UEFI".
**Tipo di server** | No | I valori possono essere "Fisico" o "Virtuale".
**Hypervisor** | No | Hypervisor in cui un computer è in esecuzione. <br/> I valori possono essere "VMware", "Hyper-V", "Xen", "AWS", "GCP" o "Altro".
**Numero di versione Hypervisor** | No | Versione Hypervisor.
**ID macchina virtuale** | No | Identificatore VM. Questo è il valore di **InstanceUUid** per la VM VMware vCenter o l'**ID della VM Hyper-V** per Hyper-V.
**ID Virtual Machine Manager** | No | Questo è il valore di **InstanceUUid** per VMWare vCenter. Non necessario per Hyper-V.
**Indirizzo MAC**| No | Indirizzo MAC server.
**BIOS ID** | No | Server BIOS ID.
**ID server personalizzato** | No | ID univoco locale del server locale. <br/> Utile per il rilevamento del server importato in base all'ID locale.
**Nome applicazione 1** | No | Nome del carico di lavoro in esecuzione nel server.<br/>È possibile aggiungere dettagli per altre app tramite[aggiunta di colonne](#add-multiple-applications) nel modello. È possibile aggiungere fino a cinque applicazioni.
**Tipo applicazione 1** | No | Tipo di carico di lavoro in esecuzione nel server
**Versione applicazione 1** | No | Versione del carico di lavoro in esecuzione nel server.
**Scadenza licenza applicazione 1** | No | Scadenza della licenza per il carico di lavoro (se applicabile).
**Business unit** | No | Business unit a cui appartiene il server.
**Titolare dell'azienda** | No | Titolare business unit.
**Nome applicazione aziendale** | No | Nome dell'applicazione cui appartiene l'app.
**Posizione** | No | Data center in cui si trova il server.
**Data di rimozione del server** | No | Data di rimozione del server fisico o del server fisico sottostante il server virtuale.

### <a name="add-operating-systems"></a>Aggiungere sistemi operativi

La valutazione riconosce i nomi specifici del sistema operativo. Qualsiasi nome del sistema operativo specificato deve corrispondere a una delle stringhe incluse nell'elenco di [nomi supportati](#supported-operating-system-names).

### <a name="add-multiple-disks"></a>Aggiungere più dischi

Il modello fornisce campi predefiniti per il primo disco. È possibile aggiungere colonne simili per un totale di otto dischi.

Ad esempio, per specificare tutti i campi per un secondo disco, aggiungere queste colonne:

- Dimensioni disco 2
- Operazioni di lettura disco 2
- Operazioni di scrittura disco 2
- Velocità effettiva lettura disco 2
- Velocità effettiva scrittura disco 2

### <a name="add-multiple-applications"></a>Aggiungere più applicazioni

Il modello fornisce campi per una singola applicazione. È possibile aggiungere colonne simili fino a cinque app.  

Ad esempio, per specificare tutti i campi per una seconda app, aggiungere queste colonne:

- Nome applicazione 2
- Tipo applicazione 2
- Versione applicazione 2
- Scadenza licenza applicazione 2

> [!NOTE]
> Le informazioni sulle app sono utili per valutare l'ambiente locale per la migrazione. Tuttavia, Valutazione server di Azure Migrate non esegue attualmente la valutazione a livello di app e non prende in considerazione le app durante la creazione di una valutazione.

## <a name="import-the-server-information"></a>Importare le informazioni sul server

Dopo aver aggiunto le informazioni nel modello CSV, importare i server in Valutazione server.

1. In Azure Migrate, in **Individua macchine virtuali**, passare al modello completato.
2. Selezionare **Importa**.
3. Viene visualizzato lo stato dell'importazione.
    - Se nello stato vengono visualizzati avvisi, è possibile correggerli o continuare senza risolverli.
    - Per una maggiore accuratezza della valutazione, migliorare le informazioni sul server come suggerito negli avvisi.
    - Per visualizzare e correggere gli avvisi, selezionare **Download warning details .CSV** (Scarica i dettagli degli avvisi in formato CSV). Questa operazione scarica il file CSV con gli avvisi inclusi. Esaminare gli avvisi e correggere i problemi se necessario.
    - Se vengono visualizzati errori per cui lo stato dell'importazione è **Non riuscito**, è necessario correggerli prima di continuare:
        1. Scaricare il file CSV, che ora include i dettagli degli errori.
        1. Esaminare e risolvere gli errori se necessario. 
        1. Caricare di nuovo il file modificato.
4. Quando lo stato dell'importazione è **Completato**, vengono importate le informazioni sul server.

## <a name="update-server-information"></a>Aggiornare le informazioni sul server

È possibile aggiornare le informazioni su un server importando nuovamente i dati per il server con lo stesso **Nome del server**. Non è possibile modificare il campo **Nome del server**. L'eliminazione di server è attualmente supportata.

## <a name="verify-servers-in-the-portal"></a>Verificare i server nel portale

Per verificare se i server vengono visualizzati nel portale di Azure dopo l'individuazione:

1. Aprire il dashboard di Azure Migrate.
2. Nella pagina **Azure Migrate - Server** > **Azure Migrate: Valutazione server** selezionare l'icona che mostra il numero di **Server individuati**.
3. Selezionare la scheda **Basati sull'importazione**.

## <a name="set-up-and-run-an-assessment"></a>Configurare ed eseguire una valutazione

È possibile creare due tipi di valutazioni con Valutazione server.

**Tipo di valutazione** | **Dettagli** | **Dati**
--- | --- | ---
**Basata sulle prestazioni** | Valutazioni basate sui valori dei dati sulle prestazioni specificati. | **Dimensioni VM consigliate**: in base ai dati sull'utilizzo di CPU e memoria.<br/><br/> **Tipo di disco consigliato (disco gestito Standard o Premium)** : in base alle operazioni di input/output al secondo e alla velocità effettiva dei dischi locali.
**Come in locale** | Valutazioni basate sul dimensionamento locale. | **Dimensioni VM consigliate**: in base alle dimensioni del server specificate.<br/><br> **Tipo di disco consigliato**: in base all'impostazione del tipo di archiviazione selezionata per la valutazione.

Per eseguire una valutazione:

1. Rivedere le [procedure consigliate](best-practices-assessment.md) per la creazione di valutazioni.
2. Nella scheda **Server**, nel riquadro **Azure Migrate: Valutazione server** selezionare **Valuta**.

    ![Valutare](./media/tutorial-assess-physical/assess.png)

3. In **Valuta server** specificare un nome per la valutazione.
4. In **Origine individuazione** selezionare **Machines added via import to Azure Migrate** (Computer aggiunti tramite importazione in Azure Migrate).
5. Fare clic su **Visualizza tutto** per rivedere le proprietà della valutazione.

    ![Proprietà valutazione](./media/tutorial-assess-physical/view-all.png)

6. In **Selezionare o creare un gruppo** selezionare **Crea nuovo** e specificare un nome per il gruppo. Un gruppo raccoglie una o più VM per la valutazione.
7. In **Aggiungere le macchine virtuali al gruppo** selezionare i server da aggiungere al gruppo.
8. Selezionare **Crea valutazione** per creare il gruppo e quindi eseguire la valutazione.

    ![Creare una valutazione](./media/tutorial-assess-physical/assessment-create.png)

9. Dopo aver creato la valutazione, visualizzarla in **Server** > **Azure Migrate: Valutazione server** > **Valutazioni**.
10. Selezionare **Esporta valutazione** per scaricarla come file di Microsoft Excel.

## <a name="review-an-assessment"></a>Esaminare una valutazione

Una valutazione descrive:

- **Idoneità per Azure**: se i server sono idonei per la migrazione ad Azure.
- **Stima dei costi mensili**: i costi mensili di calcolo e archiviazione stimati per l'esecuzione dei server in Azure.
- **Stima costo di archiviazione mensile**: i costi stimati per l'archiviazione su disco dopo la migrazione.

### <a name="view-an-assessment"></a>Visualizzare una valutazione

1. In **Obiettivi della migrazione** > **Server** selezionare **Valutazioni** in **Azure Migrate: Valutazione server**.
2. In **Valutazioni** selezionare una valutazione per aprirla.

    ![Riepilogo della valutazione](./media/tutorial-assess-physical/assessment-summary.png)

### <a name="review-azure-readiness"></a>Esaminare l'idoneità per Azure

1. In **Idoneità per Azure** verificare se i server sono pronti per la migrazione ad Azure.
2. Verificare lo stato:
    - **Idonea per Azure**: Azure Migrate consiglia le dimensioni e le stime dei costi per le VM nella valutazione.
    - **Idonea con condizioni**: mostra i problemi e le correzioni consigliate.
    - **Non idonea per Azure**: mostra i problemi e le correzioni consigliate.
    - **Idoneità sconosciuta**: Azure Migrate non è in grado di valutare l'idoneità a causa di problemi di disponibilità dei dati.

3. Selezionare uno stato di **Idoneità per Azure**. È possibile visualizzare i dettagli sull'idoneità dei server ed eseguire il drill-down per visualizzare i dettagli dei server, incluse le impostazioni di calcolo, archiviazione e rete.

### <a name="review-cost-details"></a>Esaminare i dettagli dei costi

Questa visualizzazione mostra il costo stimato di calcolo e archiviazione associato all'esecuzione delle macchine virtuali in Azure. È possibile:

- Esaminare i costi mensili di calcolo e archiviazione. I costi sono aggregati per tutti i server nel gruppo valutato.

    - Le stime dei costi sono basate sulle dimensioni consigliate per una VM e sui relativi dischi e proprietà.
    - Vengono visualizzati i costi mensili stimati per il calcolo e l'archiviazione.
    - La stima dei costi si riferisce all'esecuzione dei server locali come VM dell'infrastruttura distribuita come servizio (IaaS). Valutazione server non considera i costi relativi alla piattaforma distribuita come servizio (PaaS, Platform as a Service) o al software come un servizio (SaaS, Software as a Service).

- Esaminare le stime dei costi di archiviazione mensili. Questa visualizzazione mostra i costi di archiviazione aggregati per il gruppo valutato, suddivisi in base ai diversi tipi di dischi di archiviazione.
- Eseguire il drill-down per visualizzare i dettagli relativi a VM specifiche.

> [!NOTE]
> Le classificazioni di attendibilità non vengono assegnate alle valutazioni dei server importati in Valutazione server tramite CSV.

## <a name="supported-operating-system-names"></a>Nomi dei sistemi operativi supportati

<!-- BEGIN A - H -->

:::row:::
   :::column span="2":::
      **A - H**
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Apple Mac OS X 10
   :::column-end:::
   :::column span="":::
      Asianux 3<br/>
      Asianux 4<br/>
      Asianux 5
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      CentOS<br/>
      CentOS 4/5
   :::column-end:::
   :::column span="":::
      CoreOS Linux
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Debian GNU/Linux 4<br/>
      Debian GNU/Linux 5<br/>
      Debian GNU/Linux 6<br/>
      Debian GNU/Linux 7<br/>
      Debian GNU/Linux 8
   :::column-end:::
   :::column span="":::
      FreeBSD
   :::column-end:::
:::row-end:::

<!-- BEGIN I - R -->

:::row:::
   :::column span="2":::
      **I - R**
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      IBM OS/2
   :::column-end:::
   :::column span="":::
      MS-DOS
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Novell NetWare 5<br/>
      Novell NetWare 6
   :::column-end:::
   :::column span="":::
      Oracle Linux<br/>
       Oracle Linux 4/5<br/>
      Oracle Solaris 10<br/>
       Oracle Solaris 11
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Red Hat Enterprise Linux 2<br/>
      Red Hat Enterprise Linux 3<br/>
      Red Hat Enterprise Linux 4<br/>
      Red Hat Enterprise Linux 5<br/>
      Red Hat Enterprise Linux 6<br/>
      Red Hat Enterprise Linux 7<br/>
      Red Hat Fedora
   :::column-end:::
:::row-end:::

<!-- BEGIN S - T -->

:::row:::
   :::column span="2":::
      **S - T**
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      SCO OpenServer 5<br/>
      SCO OpenServer 6<br/>
      SCO UnixWare 7
   :::column-end:::
   :::column span="":::
      Serenity Systems eComStation 1<br/>
      Serenity Systems eComStation 2
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Sun Microsystems Solaris 8<br/>
      Sun Microsystems Solaris 9
   :::column-end:::
   :::column span="":::
      SUSE Linux Enterprise 10<br/>
      SUSE Linux Enterprise 11<br/>
      SUSE Linux Enterprise 12<br/>
      SUSE Linux Enterprise 8/9<br/>
      SUSE Linux Enterprise 11<br/>
      SUSE openSUSE
   :::column-end:::
:::row-end:::

<!-- BEGIN U - Z -->
:::row:::
   :::column span="2":::
      **U - Z**
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Ubuntu Linux
   :::column-end:::
   :::column span="":::
      VMware ESXi 4<br/>
      VMware ESXi 5<br/>
      VMware ESXi 6
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Windows 10<br/>
      Windows 2000<br/>
      Windows 3<br/>
      Windows 7<br/>
      Windows 8<br/>
      Windows 95<br/>
      Windows 98<br/>
      Windows NT<br/>
      Windows Server (R) 2008<br/>
      Windows Server 2003
   :::column-end:::
   :::column span="":::
      Windows Server 2008<br/>
      Windows Server 2008 R2<br/>
      Windows Server 2012<br/>
      Windows Server 2012 R2<br/>
      Windows Server 2016<br/>
      Windows Server 2019<br/>
      Windows Server Threshold<br/>
      Windows Vista<br/>
      Windows Web Server 2008 R2<br/>
      Windows XP Professional
   :::column-end:::
:::row-end:::

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione:

> [!div class="checklist"]
> * Sono stati importati i server in Azure Migrate: Valutazione server tramite CSV.
> * È stata creata ed esaminata una valutazione.

A questo punto, [distribuire un'appliance](./migrate-appliance.md) per valutazioni più accurate e riunire i server in gruppi per una valutazione più approfondita tramite [analisi delle dipendenze](./concepts-dependency-visualization.md).
