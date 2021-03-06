---
title: Modificare la modalità di replica di un account di archiviazione
titleSuffix: Azure Storage
description: Informazioni su come modificare la modalità di replica dei dati in un account di archiviazione esistente.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 02/10/2020
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: 14ad6dbf139b34f501e0b0ea8c16d8570b2ace5b
ms.sourcegitcommit: 0eb0673e7dd9ca21525001a1cab6ad1c54f2e929
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2020
ms.locfileid: "77212577"
---
# <a name="change-how-a-storage-account-is-replicated"></a>Modificare la modalità di replica di un account di archiviazione

Archiviazione di Azure archivia sempre più copie dei dati in modo che siano protette da eventi pianificati e non pianificati, inclusi errori hardware temporanei, interruzioni della rete o dell'alimentazione e calamità naturali di grandi dimensioni. La ridondanza garantisce che l'account di archiviazione soddisfi il [contratto di servizio (SLA) per archiviazione di Azure](https://azure.microsoft.com/support/legal/sla/storage/) anche in caso di errori.

Archiviazione di Azure offre i tipi di replica seguenti:

- Archiviazione con ridondanza locale (LRS)
- Archiviazione con ridondanza della zona (ZRS).
- Archiviazione con ridondanza geografica (GRS) o archiviazione con ridondanza geografica e accesso in lettura (RA-GRS)
- Archiviazione con ridondanza della zona geografica (GZRS) o archiviazione con ridondanza geografica e accesso in lettura (RA-GZRS) (anteprima)

Per una panoramica di ciascuna di queste opzioni, vedere [ridondanza di archiviazione di Azure](storage-redundancy.md).

## <a name="switch-between-types-of-replication"></a>Passare da un tipo di replica all'altra

È possibile passare a un account di archiviazione da un tipo di replica a un altro tipo, ma alcuni scenari sono più semplici rispetto ad altri. Se si vuole aggiungere o rimuovere la replica geografica o l'accesso in lettura all'area secondaria, è possibile usare l'interfaccia della riga di comando portale di Azure, PowerShell o l'interfaccia della riga di comando di Azure per aggiornare l'impostazione di replica. Tuttavia, se si vuole modificare il modo in cui i dati vengono replicati nell'area primaria, passando da con ridondanza locale a ZRS o viceversa, è necessario eseguire una migrazione manuale.

Nella tabella seguente viene fornita una panoramica su come passare da un tipo di replica a un altro:

| Commutazione | ... a con ridondanza locale | ... al GRS/RA-GRS | ... a ZRS | ... a GZRS/RA-GZRS |
|--------------------|----------------------------------------------------|---------------------------------------------------------------------|----------------------------------------------------|---------------------------------------------------------------------|
| <b>... da con ridondanza locale</b> | N/D | Usare portale di Azure, PowerShell o l'interfaccia della riga di comando per modificare l'impostazione di replica<sup>1</sup> | Eseguire una migrazione manuale <br /><br />Richiedi una migrazione in tempo reale | Eseguire una migrazione manuale <br /><br /> OPPURE <br /><br /> Passa prima a GRS/RA-GRS e quindi Richiedi una migrazione in tempo reale<sup>1</sup> |
| <b>... da GRS/RA-GRS</b> | Usare portale di Azure, PowerShell o l'interfaccia della riga di comando per modificare l'impostazione di replica | N/D | Eseguire una migrazione manuale <br /><br /> OPPURE <br /><br /> Passa prima a con ridondanza locale e quindi Richiedi una migrazione in tempo reale | Eseguire una migrazione manuale <br /><br /> Richiedi una migrazione in tempo reale |
| <b>... da ZRS</b> | Eseguire una migrazione manuale | Eseguire una migrazione manuale | N/D | Usare portale di Azure, PowerShell o l'interfaccia della riga di comando per modificare l'impostazione di replica<sup>1</sup> |
| <b>... da GZRS/RA-GZRS</b> | Eseguire una migrazione manuale | Eseguire una migrazione manuale | Usare portale di Azure, PowerShell o l'interfaccia della riga di comando per modificare l'impostazione di replica | N/D |

<sup>1</sup> comporta un addebito di uscita una sola volta.

## <a name="change-the-replication-setting"></a>Modificare l'impostazione di replica

È possibile usare la portale di Azure, PowerShell o l'interfaccia della riga di comando di Azure per modificare l'impostazione di replica per un account di archiviazione, purché non si stia modificando il modo in cui i dati vengono replicati nell'area primaria. Se si esegue la migrazione da con ridondanza locale nell'area primaria a ZRS nell'area primaria o viceversa, è necessario eseguire una [migrazione manuale](#perform-a-manual-migration-to-zrs) o una [migrazione in tempo reale](#request-a-live-migration-to-zrs).

La modifica della modalità di replica dell'account di archiviazione non comporta tempi di inattività per le applicazioni.

# <a name="portaltabportal"></a>[Portale](#tab/portal)

Per modificare l'opzione di ridondanza per l'account di archiviazione nell'portale di Azure, attenersi alla procedura seguente:

1. Passare all'account di archiviazione nel portale di Azure.
1. Selezionare l'impostazione di **configurazione** .
1. Aggiornare l'impostazione di **replica** .

![Screenshot che illustra come modificare l'opzione di replica nel portale](media/redundancy-migration/change-replication-option.png)

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

Per modificare l'opzione di ridondanza per l'account di archiviazione con PowerShell, chiamare il comando [set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) e specificare il parametro `-SkuName`:

```powershell
Set-AzStorageAccount -ResourceGroupName <resource_group> `
    -AccountName <storage_account> `
    -SkuName <sku>
```

# <a name="azure-clitabazure-cli"></a>[Interfaccia della riga di comando di Azure](#tab/azure-cli)

Per modificare l'opzione di ridondanza per l'account di archiviazione con l'interfaccia della riga di comando di Azure, chiamare il comando [AZ storage account Update](/cli/azure/storage/account#az-storage-account-update) e specificare il parametro `--sku`:

```azurecli-interactive
az storage account update \
    --name <storage-account>
    --resource-group <resource_group> \
    --sku <sku>
```

---

## <a name="perform-a-manual-migration-to-zrs"></a>Eseguire una migrazione manuale a ZRS

Se si vuole modificare la modalità di replica dei dati nell'account di archiviazione nell'area primaria, passando da con ridondanza locale a ZRS o viceversa, è possibile scegliere di eseguire una migrazione manuale. Una migrazione manuale offre maggiore flessibilità rispetto a una migrazione in tempo reale. È possibile controllare l'intervallo di una migrazione manuale, quindi usare questa opzione se è necessario completare la migrazione entro una determinata data.

Quando si esegue una migrazione manuale da con ridondanza locale a ZRS nell'area primaria o viceversa, l'account di archiviazione di destinazione può essere con ridondanza geografica e può anche essere configurato per l'accesso in lettura all'area secondaria. Ad esempio, è possibile eseguire la migrazione di un account con ridondanza locale a un account GZRS o RA-GZRS in un unico passaggio.

Una migrazione manuale può comportare tempi di inattività dell'applicazione. Se l'applicazione richiede disponibilità elevata, Microsoft offre anche un'opzione di migrazione in tempo reale. Una migrazione in tempo reale è una migrazione sul posto senza tempi di inattività.

Con una migrazione manuale, i dati vengono copiati dall'account di archiviazione esistente a un nuovo account di archiviazione che usa ZRS nell'area primaria. Per eseguire una migrazione manuale, è possibile usare una delle opzioni seguenti:

- Copiare i dati usando uno strumento esistente, ad esempio AzCopy, una delle librerie client di archiviazione di Azure o uno strumento di terze parti affidabile.
- Se si ha familiarità con Hadoop o HDInsight, è possibile associare l'account di archiviazione di origine e l'account di archiviazione di destinazione al cluster. Quindi, parallelizzare il processo di copia dei dati con uno strumento, ad esempio DistCp.

## <a name="request-a-live-migration-to-zrs"></a>Richiedere una migrazione in tempo reale a ZRS

Se è necessario eseguire la migrazione dell'account di archiviazione da con ridondanza locale o GRS a ZRS nell'area primaria senza tempi di inattività dell'applicazione, è possibile richiedere una migrazione in tempo reale da Microsoft. Durante una migrazione in tempo reale, è possibile accedere ai dati nell'account di archiviazione e senza perdita di durabilità o disponibilità. Il contratto di contratto di archiviazione di Azure viene mantenuto durante il processo di migrazione. Non si verifica alcuna perdita di dati associata a una migrazione in tempo reale. Gli endpoint di servizio, le chiavi di accesso, le firme di accesso condiviso e altre opzioni dell'account rimangono invariati dopo la migrazione.

ZRS supporta solo account di uso generico V2. Assicurarsi quindi di aggiornare l'account di archiviazione prima di inviare una richiesta di migrazione in tempo reale a ZRS. Per altre informazioni, vedere [eseguire l'aggiornamento a un account di archiviazione per utilizzo generico V2](storage-account-upgrade.md). Un account di archiviazione deve contenere dati di cui eseguire la migrazione tramite Live Migration.

La migrazione in tempo reale è supportata solo per gli account di archiviazione che usano la replica con l'archiviazione con ridondanza locale o l'archiviazione con ridondanza geografica. Se l'account usa l'archiviazione con ridondanza geografica e accesso in lettura, è necessario innanzitutto modificare il tipo di replica dell'account in archiviazione con ridondanza locale o archiviazione con ridondanza geografica prima di procedere. Questo passaggio intermedio rimuove, prima della migrazione, l'endpoint secondario di sola lettura fornito dall'archiviazione con ridondanza geografica e accesso in lettura.

Sebbene Microsoft gestisca tempestivamente la richiesta di migrazione in tempo reale, non c'è alcuna garanzia per quanto riguarda il tempo necessario per il completamento di questo tipo di migrazione. Se è necessario eseguire la migrazione dei dati in un'archiviazione con ridondanza della zona entro una certa data, Microsoft consiglia di eseguire una migrazione manuale. In genere, maggiore è la quantità di dati nell'account, più tempo richiederà la migrazione dei dati.

È necessario eseguire una migrazione manuale se:

- Si vuole eseguire la migrazione dei dati in un account di archiviazione ZRS che si trova in un'area diversa da quella dell'account di origine.
- L'account di archiviazione è un BLOB di pagine Premium o un account BLOB in blocchi.
- Si vuole eseguire la migrazione dei dati da ZRS a con ridondanza locale, GRS o RA-GRS.
- L'account di archiviazione include i dati nel livello archivio.

È possibile richiedere la migrazione in tempo reale tramite il [portale del supporto tecnico di Azure](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview). Nel portale selezionare l'account di archiviazione da convertire in archiviazione con ridondanza della zona.

1. Selezionare **Nuova richiesta di supporto**
2. Completare le **nozioni di base** con le informazioni relative all'account. Nella sezione **Assistenza** selezionare **Gestione Account di archiviazione** e la risorsa che si vuole convertire in archiviazione con ridondanza della zona.
3. Fare clic su **Avanti**.
4. Specificare i valori seguenti nella sezione **Problema**:
    - **Gravità**: lasciare il valore predefinito.
    - **Tipo di problema**: selezionare **Migrazione dei dati**.
    - **Category**: selezionare **migrate to ZRS**.
    - **Titolo**: digitare un titolo descrittivo, ad esempio **migrazione di account con archiviazione con ridondanza della zona**.
    - **Dettagli**: digitare ulteriori dettagli nella casella **Dettagli** , ad esempio, si desidera eseguire la migrazione a ZRS da [con ridondanza locale, GRS] nell'area \_\_.
5. Fare clic su **Avanti**.
6. Verificare che le informazioni di contatto nel pannello **Informazioni contatto** siano corrette.
7. Selezionare **Crea**.

Un addetto del supporto tecnico contatterà l'utente e fornirà l'assistenza necessaria.

> [!NOTE]
> La migrazione in tempo reale non è attualmente supportata per le condivisioni file Premium. Attualmente sono supportati solo i dati copiati o spostati manualmente.
>
> Gli account di archiviazione GZRS attualmente non supportano il livello archivio. Per altri dettagli [, vedere Archiviazione BLOB di Azure: livelli di accesso ad accesso frequente, ad accesso sporadico e archivio](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers) .
>
> I dischi gestiti sono disponibili solo per con ridondanza locale e non è possibile eseguirne la migrazione a ZRS. È possibile archiviare snapshot e immagini per i dischi gestiti da unità SSD standard nell'archiviazione HDD standard e [scegliere tra le opzioni con ridondanza locale e ZRS](https://azure.microsoft.com/pricing/details/managed-disks/). Per informazioni sull'integrazione con i set di disponibilità, vedere [Introduzione a Managed Disks di Azure](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview#integration-with-availability-sets).

## <a name="switch-from-zrs-classic"></a>Passa da ZRS classico

> [!IMPORTANT]
> Microsoft imposterà come deprecati gli account della versione classica dell'archiviazione con ridondanza della zona e ne eseguirà la migrazione il 31 marzo 2021. Altri dettagli verranno inviati ai clienti della versione classica dell'archiviazione con ridondanza della zona prima di essere deprecati.
>
> Quando ZRS diventa disponibile a livello generale in una determinata area, i clienti non saranno più in grado di creare account ZRS classici dal portale di Azure in tale area. L'uso di Microsoft PowerShell e dell'interfaccia della riga di comando di Azure per creare gli account della versione classica dell'archiviazione con ridondanza della zona sarà disponibile finché la versione classica dell'archiviazione con ridondanza della zona viene deprecata. Per informazioni sul punto in cui è disponibile ZRS, vedere [ridondanza di archiviazione di Azure](storage-redundancy.md).

La versione classica dell'archiviazione con ridondanza della zona replica i dati in modo asincrono tra data center in una o due aree. I dati replicati potrebbero non essere disponibili a meno che Microsoft non avvii il failover all'area secondaria. Gli account della versione classica dell'archiviazione con ridondanza della zona non possono essere convertiti in o da archiviazione con ridondanza locale, archiviazione con ridondanza geografica o archiviazione con ridondanza geografica e accesso in lettura. Gli account della versione classica dell'archiviazione con ridondanza della zona non supportano inoltre le metriche o la registrazione.

La versione classica dell'archiviazione con ridondanza della zona è disponibile solo per i **BLOB in blocchi** negli account di archiviazione per utilizzo generico V1 (GPv1). Per altre informazioni sugli account di archiviazione, vedere [Panoramica dell'account di archiviazione di Azure](storage-account-overview.md).

Per eseguire manualmente la migrazione dei dati dell'account ZRS a o da un account con ridondanza locale, GRS, RA-GRS o ZRS classico, usare uno degli strumenti seguenti: AzCopy, Azure Storage Explorer, PowerShell o l'interfaccia della riga di comando di Azure. È anche possibile creare una soluzione di migrazione personalizzata con una delle librerie client di Archiviazione di Azure.

È anche possibile aggiornare l'account di archiviazione ZRS classico a ZRS usando il portale di Azure, PowerShell o l'interfaccia della riga di comando di Azure nelle aree in cui è disponibile ZRS.

# <a name="portaltabportal"></a>[Portale](#tab/portal)

Per eseguire l'aggiornamento a ZRS nel portale di Azure, passare alle impostazioni di **configurazione** dell'account e scegliere **Aggiorna**:

![Aggiornare ZRS classico a ZRS nel portale](media/redundancy-migration/portal-zrs-classic-upgrade.png)

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

Per eseguire l'aggiornamento a ZRS tramite PowerShell, chiamare il comando seguente:

```powershell
Set-AzStorageAccount -ResourceGroupName <resource_group> -AccountName <storage_account> -UpgradeToStorageV2
```

# <a name="azure-clitabazure-cli"></a>[Interfaccia della riga di comando di Azure](#tab/azure-cli)

Per eseguire l'aggiornamento a ZRS tramite l'interfaccia della riga di comando di Azure, chiamare il comando seguente:

```cli
az storage account update -g <resource_group> -n <storage_account> --set kind=StorageV2
```

---

## <a name="costs-associated-with-changing-how-data-is-replicated"></a>Costi associati alla modifica della modalità di replica dei dati

I costi associati alla modifica del modo in cui i dati vengono replicati dipendono dal percorso di conversione. L'ordinamento tra le offerte di ridondanza di archiviazione di Azure più costose, tra cui con ridondanza locale, ZRS, GRS, RA-GRS, GZRS e RA-GZRS.

Ad esempio, se si passa *da* con ridondanza locale a un altro tipo di replica, verranno addebitati costi aggiuntivi perché si passa a un livello di ridondanza più sofisticato. La migrazione *a* GRS o RA-GRS comporta un addebito per la larghezza di banda in uscita perché i dati dell'area primaria vengono replicati nell'area secondaria remota. Questo addebito è un costo monouso alla configurazione iniziale. Dopo la copia dei dati, non sono previsti ulteriori addebiti per la migrazione. Per informazioni dettagliate sui costi addebitati per la larghezza di banda, vedere la [pagina dei prezzi di Archiviazione di Azure](https://azure.microsoft.com/pricing/details/storage/blobs/).

Se si esegue la migrazione dell'account di archiviazione da GRS a con ridondanza locale, non sono previsti costi aggiuntivi, ma i dati replicati vengono eliminati dalla posizione secondaria.

> [!IMPORTANT]
> Se si esegue la migrazione dell'account di archiviazione da RA-GRS a GRS o con ridondanza locale, tale account viene fatturato come RA-GRS per altri 30 giorni oltre la data in cui è stata convertita.

## <a name="see-also"></a>Vedere anche

- [Azure Storage redundancy](storage-redundancy.md) (Ridondanza di Archiviazione di Azure)
- [Controllare la proprietà ora ultima sincronizzazione per un account di archiviazione](last-sync-time-get.md)
- [Progettazione di applicazioni a disponibilità elevata tramite l'archiviazione con ridondanza geografica e accesso in lettura](storage-designing-ha-apps-with-ragrs.md)
