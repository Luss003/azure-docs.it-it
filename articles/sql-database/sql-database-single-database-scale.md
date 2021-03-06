---
title: Ridimensionare le risorse di database singolo
description: Questo articolo illustra come ridimensionare le risorse di calcolo e di archiviazione disponibili per un database singolo nel database SQL di Azure.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 03/10/2020
ms.openlocfilehash: 92d6dccec3ce6483072a81c8739b65e81ce2c7fe
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2020
ms.locfileid: "79268574"
---
# <a name="scale-single-database-resources-in-azure-sql-database"></a>Ridimensionare le risorse di database singoli nel database SQL di Azure

Questo articolo descrive come ridimensionare le risorse di calcolo e di archiviazione disponibili per un database SQL di Azure nel livello di calcolo di cui è stato effettuato il provisioning. In alternativa, il [livello di calcolo senza server](sql-database-serverless.md) fornisce scalabilità automatica di calcolo e fatture al secondo per il calcolo usato.

Dopo aver selezionato inizialmente il numero di Vcore o DTU, è possibile ridimensionare un singolo database in modo dinamico in base all'esperienza effettiva usando il [portale di Azure](sql-database-single-databases-manage.md#manage-an-existing-sql-database-server), [Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1), [PowerShell](/powershell/module/az.sql/set-azsqldatabase), l'interfaccia della riga di comando di [Azure](/cli/azure/sql/db#az-sql-db-update)o l' [API REST](https://docs.microsoft.com/rest/api/sql/databases/update).

Il video seguente mostra come modificare in modo dinamico il livello di servizio e le dimensioni di calcolo per aumentare le DTU disponibili per un singolo database.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]

> [!IMPORTANT]
> In alcune circostanze, può essere necessario compattare un database per recuperare spazio inutilizzato. Per altre informazioni, vedere [Gestire lo spazio file nel database SQL di Azure](sql-database-file-space-management.md).

## <a name="impact"></a>Impatto

La modifica del livello di servizio o delle dimensioni di calcolo di comporta principalmente il servizio che esegue i passaggi seguenti:

1. Crea nuova istanza di calcolo per il database  

    Viene creata una nuova istanza di calcolo con il livello di servizio e le dimensioni di calcolo richiesti. Per alcune combinazioni di modifiche del livello di servizio e delle dimensioni di calcolo, è necessario creare una replica del database nella nuova istanza di calcolo, che prevede la copia dei dati e può influenzare fortemente la latenza complessiva. Indipendentemente dal fatto che il database rimanga online durante questo passaggio, le connessioni continueranno a essere indirizzate al database nell'istanza di calcolo originale.

2. Passare il routing delle connessioni a una nuova istanza di calcolo

    Le connessioni esistenti al database nell'istanza di calcolo originale verranno eliminate. Tutte le nuove connessioni vengono stabilite nel database nella nuova istanza di calcolo. Per alcune combinazioni di modifiche del livello di servizio e delle dimensioni di calcolo, i file di database vengono scollegati e ricollegati durante l'opzione.  Indipendentemente dall'opzione, l'opzione può causare una breve interruzione del servizio quando il database non è disponibile in genere per meno di 30 secondi e spesso solo per pochi secondi. Se sono in esecuzione transazioni con esecuzione prolungata quando si eliminano le connessioni, la durata di questo passaggio potrebbe richiedere più tempo per recuperare le transazioni interrotte. Il [recupero accelerato del database](sql-database-accelerated-database-recovery.md) può ridurre l'effetto di un'interruzione delle transazioni a esecuzione prolungata.

> [!IMPORTANT]
> Durante qualsiasi passaggio del flusso di lavoro non viene perso alcun dato. Assicurarsi di aver implementato una logica di [ripetizione dei tentativi](sql-database-connectivity-issues.md) nelle applicazioni e nei componenti che usano il database SQL di Azure durante la modifica del livello di servizio.

## <a name="latency"></a>Latenza 

La latenza stimata per modificare il livello di servizio o ridimensionare le dimensioni di calcolo di un singolo database o di un pool elastico è parametrizzata come indicato di seguito:

|Livello di servizio|Database singolo di base,</br>Standard (S0-S1)|Pool elastico di base,</br>Standard (S2-S12), </br>Con iperscalabilità </br>per utilizzo generico database singolo o il pool elastico|Premium o business critical database singolo o pool elastico|
|:---|:---|:---|:---|
|**Database singolo Basic,</br> standard (S0-S1)**|&bull; &nbsp;latenza temporale costante indipendentemente dallo spazio utilizzato</br>&bull; &nbsp;in genere, meno di 5 minuti|&bull; &nbsp;latenza proporzionale allo spazio del database usato a causa della copia dei dati</br>&bull; &nbsp;in genere, meno di 1 minuto per GB di spazio usato|&bull; &nbsp;latenza proporzionale allo spazio del database usato a causa della copia dei dati</br>&bull; &nbsp;in genere, meno di 1 minuto per GB di spazio usato|
|**Pool elastico Basic, </br>standard (S2-S12), </br>iperscalabilità, </br>per utilizzo generico database singolo o pool elastico**|&bull; &nbsp;latenza proporzionale allo spazio del database usato a causa della copia dei dati</br>&bull; &nbsp;in genere, meno di 1 minuto per GB di spazio usato|&bull; &nbsp;latenza temporale costante indipendentemente dallo spazio utilizzato</br>&bull; &nbsp;in genere, meno di 5 minuti|&bull; &nbsp;latenza proporzionale allo spazio del database usato a causa della copia dei dati</br>&bull; &nbsp;in genere, meno di 1 minuto per GB di spazio usato|
|**Premium o business critical database singolo o pool elastico**|&bull; &nbsp;latenza proporzionale allo spazio del database usato a causa della copia dei dati</br>&bull; &nbsp;in genere, meno di 1 minuto per GB di spazio usato|&bull; &nbsp;latenza proporzionale allo spazio del database usato a causa della copia dei dati</br>&bull; &nbsp;in genere, meno di 1 minuto per GB di spazio usato|&bull; &nbsp;latenza proporzionale allo spazio del database usato a causa della copia dei dati</br>&bull; &nbsp;in genere, meno di 1 minuto per GB di spazio usato|

> [!TIP]
> Per monitorare le operazioni in corso, vedere: [gestire le operazioni tramite l'API REST di SQL](https://docs.microsoft.com/rest/api/sql/operations/list), [gestire le operazioni tramite l'interfaccia](/cli/azure/sql/db/op)della riga di comando, [monitorare le operazioni usando T-SQL](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) e questi due comandi di PowerShell: [Get-AzSqlDatabaseActivity](/powershell/module/az.sql/get-azsqldatabaseactivity) e [Stop-AzSqlDatabaseActivity](/powershell/module/az.sql/stop-azsqldatabaseactivity).

## <a name="cancelling-changes"></a>Annullamento delle modifiche

Una modifica del livello di servizio o un'operazione di ridimensionamento del calcolo può essere annullata.

#### <a name="azure-portal"></a>Portale di Azure

Nel pannello Panoramica database passare a **notifiche** e fare clic sul riquadro che indica che è in corso un'operazione:

![Operazione in corso](media/sql-database-single-database-scale/ongoing-operations.png)

Quindi, fare clic sul pulsante con l'etichetta **Annulla questa operazione**.

![Annulla l'operazione in corso](media/sql-database-single-database-scale/cancel-ongoing-operation.png)

#### <a name="powershell"></a>PowerShell

Da un prompt dei comandi di PowerShell, impostare il `$resourceGroupName`, `$serverName`e `$databaseName`, quindi eseguire il comando seguente:

```powershell
$operationName = (az sql db op list --resource-group $resourceGroupName --server $serverName --database $databaseName --query "[?state=='InProgress'].name" --out tsv)
if (-not [string]::IsNullOrEmpty($operationName)) {
    (az sql db op cancel --resource-group $resourceGroupName --server $serverName --database $databaseName --name $operationName)
        "Operation " + $operationName + " has been canceled"
}
else {
    "No service tier change or compute rescaling operation found"
}
```

## <a name="additional-considerations"></a>Altre considerazioni

- Se si esegue l'aggiornamento a un livello di servizio o dimensioni di calcolo superiori, le dimensioni massime del database non aumentano a meno che non si specifichino esplicitamente dimensioni più elevate (massime).
- Per effettuare il downgrade di un database, la relativa quantità di spazio usato deve essere inferiore alle dimensioni massime consentite per il livello di servizio e le dimensioni di calcolo di destinazione.
- Quando si effettua il downgrade dal livello **Premium** al livello **Standard**, viene applicato un costo per le risorse di archiviazione extra se (1) le dimensioni massime del database sono supportate nelle dimensioni di calcolo di destinazione e (2) le dimensioni massime superano lo spazio di archiviazione incluso delle dimensioni di calcolo di destinazione. Ad esempio, se un database P1 con una dimensione massima di 500 GB viene ridimensionato a S3, viene applicato un costo di archiviazione aggiuntivo poiché S3 supporta una dimensione massima di 1 TB e la quantità di spazio di archiviazione inclusa è solo 250 GB. Lo spazio di archiviazione extra è quindi 500 GB - 250 GB = 250 GB. Per i prezzi delle risorse di archiviazione extra, vedere [Prezzi di Database SQL](https://azure.microsoft.com/pricing/details/sql-database/). Se la quantità effettiva di spazio usato è inferiore allo spazio di archiviazione incluso, questo costo aggiuntivo può essere evitato riducendo le dimensioni massime del database fino allo spazio incluso.
- Quando si aggiorna un database con la [replica geografica](sql-database-geo-replication-portal.md) abilitata, aggiornare i database secondari al livello di servizio e alle dimensioni di calcolo desiderati prima di aggiornare il database primario (indicazione generale per ottenere prestazioni ottimali). Quando si esegue l'aggiornamento a un'edizione diversa, è necessario che il database secondario venga aggiornato per primo.
- Quando si effettua il downgrade di un database con la [replica geografica](sql-database-geo-replication-portal.md) abilitata, eseguire il downgrade dei database primari al livello di servizio e alle dimensioni di calcolo desiderati prima del downgrade del database secondario (indicazione generale per ottenere prestazioni ottimali). Quando si effettua il downgrade a un'edizione diversa, è necessario eseguire prima il downgrade del database primario.
- Le offerte per il ripristino del servizio sono diverse per i vari livelli di servizio. In caso di downgrade al livello **Basic**, il periodo di conservazione dei backup sarà inferiore. Vedere l'articolo relativo ai [backup del database SQL di Azure](sql-database-automated-backups.md).
- Le nuove proprietà del database non vengono applicate finché non sono state completate le modifiche.

## <a name="billing"></a>Fatturazione 

Viene fatturata ogni ora per cui un database esiste usando il livello di servizio più elevato e le dimensioni di calcolo applicati in quell'ora, indipendentemente dall'uso o dal fatto che il database sia stato attivo per meno di un'ora. Ad esempio, se si crea un database singolo che viene eliminato cinque minuti dopo, in fattura viene riportato l'addebito relativo a un'ora di database.

## <a name="change-storage-size"></a>modifica delle dimensioni di archiviazione

### <a name="vcore-based-purchasing-model"></a>Modello di acquisto basato su vCore

- È possibile eseguire il provisioning dell'archiviazione fino al limite massimo di dimensioni di archiviazione dati con incrementi di 1 GB. L'archiviazione dei dati minima configurabile è di 1 GB. Vedere le pagine relative al limite delle risorse per [database singoli](sql-database-vcore-resource-limits-single-databases.md) e [pool elastici](sql-database-vcore-resource-limits-elastic-pools.md) per i limiti di dimensioni massime per l'archiviazione dei dati in ogni obiettivo di servizio.
- È possibile eseguire il provisioning dell'archiviazione dei dati per un singolo database aumentando o diminuendo le dimensioni massime usando il [portale di Azure](https://portal.azure.com), [Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1), [PowerShell](/powershell/module/az.sql/set-azsqldatabase), l'interfaccia della riga di comando di [Azure](/cli/azure/sql/db#az-sql-db-update)o l' [API REST](https://docs.microsoft.com/rest/api/sql/databases/update). Se il valore delle dimensioni massime viene specificato in byte, deve essere un multiplo di 1 GB (1073741824 byte).
- La quantità di dati che può essere archiviata nei file di dati di un database è limitata dalle dimensioni massime di archiviazione dati configurate. Oltre a tale archiviazione, il database SQL alloca automaticamente il 30% di spazio di archiviazione da utilizzare per il log delle transazioni.
- Il database SQL alloca automaticamente 32 GB per vCore per il database `tempdb`. `tempdb` si trova nell'archivio SSD locale in tutti i livelli di servizio.
- Il prezzo di archiviazione per un singolo database o un pool elastico è la somma degli importi di archiviazione dei dati e dei log delle transazioni moltiplicati per il prezzo unitario di archiviazione del livello di servizio. Il costo di `tempdb` è incluso nel prezzo. Per informazioni dettagliate sui prezzi di archiviazione, vedere [prezzi del database SQL](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> In alcune circostanze, può essere necessario compattare un database per recuperare spazio inutilizzato. Per altre informazioni, vedere [Gestire lo spazio file nel database SQL di Azure](sql-database-file-space-management.md).

### <a name="dtu-based-purchasing-model"></a>modello di acquisto basato su DTU

- Il prezzo DTU per un singolo database include una determinata quantità di risorse di archiviazione senza costi aggiuntivi. Le risorse di archiviazione extra rispetto alla quantità inclusa possono essere sottoposte a provisioning per un costo aggiuntivo fino alla quantità massima in incrementi di 250 GB fino a 1 TB e quindi in incrementi di 256 GB oltre 1 TB. Per gli spazi di archiviazione inclusi e i limiti di dimensioni massime, vedere [Database singolo: dimensioni di archiviazione e dimensioni di calcolo](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes).
- Il provisioning delle risorse di archiviazione extra per un database singolo può essere effettuato aumentandone le dimensioni massime tramite il portale di Azure, [Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1), [PowerShell](/powershell/module/az.sql/set-azsqldatabase), l'[interfaccia della riga di comando di Azure](/cli/azure/sql/db#az-sql-db-update) o l'[API REST](https://docs.microsoft.com/rest/api/sql/databases/update).
- Il prezzo delle risorse di archiviazione extra per un singolo database corrisponde allo spazio di archiviazione extra moltiplicato per il prezzo unitario del livello di servizio. Per informazioni dettagliate sul prezzo delle risorse di archiviazione extra, vedere [Prezzi di Database SQL](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> In alcune circostanze, può essere necessario compattare un database per recuperare spazio inutilizzato. Per altre informazioni, vedere [Gestire lo spazio file nel database SQL di Azure](sql-database-file-space-management.md).

### <a name="geo-replicated-database"></a>Database con replica geografica

Per modificare le dimensioni del database di un database secondario replicato, modificare le dimensioni del database primario. Questa modifica verrà quindi replicata e implementata anche nel database secondario. 

## <a name="p11-and-p15-constraints-when-max-size-greater-than-1-tb"></a>Vincoli P11 e P15 quando la dimensione massima è maggiore di 1 TB

Più di 1 TB di spazio di archiviazione nel livello Premium è attualmente disponibile in tutte le aree, ad eccezione di: Cina orientale, Cina settentrionale, Germania centrale, Germania nord-orientale, Stati Uniti centro-occidentali, US DoD aree e Stati Uniti centrali. In queste aree la quantità massima di spazio di archiviazione nel livello Premium è limitata a 1 TB. Ai database P11 e P15 con dimensioni massime maggiori di 1 TB vengono applicate le considerazioni e le limitazioni seguenti:

- Se le dimensioni massime per un database P11 o P15 sono state impostate su un valore maggiore di 1 TB, è possibile che vengano ripristinate o copiate solo in un database P11 o P15.  Successivamente, il database può essere ridimensionato a una dimensione di calcolo diversa, a condizione che la quantità di spazio allocata al momento dell'operazione di ridimensionamento non superi i limiti di dimensioni massime delle nuove dimensioni di calcolo.
- Per gli scenari di replica geografica attiva:
  - Impostazione di una relazione di replica geografica: se il database primario è P11 o P15, devono esserlo anche i database secondari. Le dimensioni di calcolo inferiori non vengono accettate come database secondari in quanto non supportano l'opzione superiore a 1 TB.
  - Aggiornamento del database primario in una relazione di replica geografica: portando a oltre 1 TB le dimensioni massime di un database primario, viene attivata la stessa modifica nel database secondario. Entrambi gli aggiornamenti devono avere esito positivo per applicare la modifica al database primario. Vengono applicate limitazioni per l'opzione da oltre 1 TB. Se il database secondario si trova in un'area che non supporta l'opzione da oltre 1 TB, il database primario non viene aggiornato.
- Il servizio di importazione/esportazione per caricare i database P11 o P15 con oltre 1 TB non è supportato. Usare SqlPackage.exe per [importare](sql-database-import.md) ed [esportare](sql-database-export.md) i dati.

## <a name="next-steps"></a>Passaggi successivi

Per i limiti generali delle risorse, vedere [Limiti delle risorse basate su vCore del database SQL - database singoli](sql-database-vcore-resource-limits-single-databases.md), [Limiti delle risorse basate su DTU del database SQL - pool elastici](sql-database-dtu-resource-limits-single-databases.md).
