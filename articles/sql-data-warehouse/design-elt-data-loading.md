---
title: Progettare un processo ELT anziché ETL per Azure SQL Data Warehouse | Microsoft Docs
description: Invece di ETL, progettare un processo di estrazione, caricamento e trasformazione (ELT, Extract, Load, Transform) per il caricamento di dati in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: load-data
ms.date: 07/28/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: c90deefba75cd8bbeda126c9da8a05e1069831d4
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/29/2019
ms.locfileid: "68597462"
---
# <a name="designing-a-polybase-data-loading-strategy-for-azure-sql-data-warehouse"></a>Progettazione di un strategia di caricamento dei dati di PolyBase per Azure SQL Data Warehouse

I data warehouse SMP tradizionali usano un processo di estrazione, trasformazione e caricamento (ETL, Extract, Transform, Load) per il caricamento dei dati. Azure SQL Data Warehouse è un'architettura di elaborazione parallela massiva (MPP, Massively Parallel Processing) che sfrutta la scalabilità e la flessibilità delle risorse di calcolo e archiviazione. Usando un processo di estrazione, caricamento e trasformazione (ELT, Extract, Load, Transform), è possibile sfruttare i vantaggi dell'elaborazione MPP ed eliminare le risorse necessarie per trasformare i dati prima del caricamento. Anche se SQL Data Warehouse supporta molti metodi di caricamento, incluse opzioni non PolyBase come BCP e l'API SQLBulkCopy, il modo più veloce e scalabile per caricare i dati consiste nell'uso di PolyBase,  una tecnologia che accede ai dati archiviati esterni in Archiviazione BLOB di Azure o Azure Data Lake Store tramite il linguaggio T-SQL.

> [!VIDEO https://www.youtube.com/embed/l9-wP7OdhDk]


## <a name="what-is-elt"></a>Definizione di ELT

ELT è un processo mediante il quale i dati vengono estratti da un sistema di origine, caricati in un data warehouse e quindi trasformati. 

Per l'implementazione di un processo ELT PolyBase per SQL Data Warehouse è necessario eseguire questi passaggi:

1. Estrarre i dati di origine in file di testo.
2. Trasferire i dati nell'archivio BLOB di Azure o in Azure Data Lake Store.
3. Preparare i dati per il caricamento.
4. Caricare i dati in tabelle di staging di SQL Data Warehouse usando PolyBase. 
5. Trasformare i dati.
6. Inserire i dati in tabelle di produzione.


Per un'esercitazione sul caricamento, vedere [Usare PolyBase per caricare dati dall'archivio BLOB di Azure ad Azure SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md).

Per altre informazioni, vedere il [blog sui modelli di caricamento](https://blogs.msdn.microsoft.com/sqlcat/20../../azure-sql-data-warehouse-loading-patterns-and-strategies/). 


## <a name="1-extract-the-source-data-into-text-files"></a>1. Estrarre i dati di origine in file di testo

La modalità di recupero dei dati dal sistema di origine dipende dalla posizione di archiviazione.  L'obiettivo è spostare i dati in file di testo delimitati supportati da PolyBase. 

### <a name="polybase-external-file-formats"></a>Formati di file esterni PolyBase

PolyBase carica i dati da file di testo delimitati con codifica UTF-8 e UTF-16. Oltre ai file di testo delimitati, supporta il caricamento da formati di file Hadoop, ovvero RC, ORC e Parquet. PolyBase può anche caricare dati da file compressi Gzip e Snappy. PolyBase non supporta attualmente i formati ASCII esteso, a larghezza fissa e annidati, come WinZip, JSON e XML. Se si esegue l'esportazione da SQL Server, è possibile usare lo [strumento da riga di comando bcp](/sql/tools/bcp-utility) per esportare i dati in file di testo delimitati. Il mapping del tipo di dati parquet a SQL DW è il seguente:

| **Tipo di dati parquet** |                      **Tipo di dati SQL**                       |
| :-------------------: | :----------------------------------------------------------: |
|        tinyint        |                           tinyint                            |
|       smallint        |                           smallint                           |
|          int          |                             int                              |
|        bigint         |                            bigint                            |
|        boolean        |                             bit                              |
|        Double         |                            float                             |
|         float         |                             real                             |
|        Double         |                            money                             |
|        Double         |                          smallmoney                          |
|        string         |                            nchar                             |
|        string         |                           nvarchar                           |
|        string         |                             char                             |
|        string         |                           varchar                            |
|        binary         |                            binary                            |
|        binary         |                          varbinary                           |
|       timestamp       |                             date                             |
|       timestamp       |                        smalldatetime                         |
|       timestamp       |                          datetime2                           |
|       timestamp       |                           Datetime                           |
|       timestamp       |                             time                             |
|       date            |                             date                             |
|        decimal        |                            decimal                           |

## <a name="2-land-the-data-into-azure-blob-storage-or-azure-data-lake-store"></a>2. Trasferire i dati in Archiviazione BLOB di Azure o in Azure Data Lake Store

Per trasferire i dati in Archiviazione di Azure, è possibile spostarli nell'[archivio BLOB di Azure](../storage/blobs/storage-blobs-introduction.md) o in [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md). In entrambe le posizioni, i dati devono essere archiviati in file di testo. PolyBase può eseguire il caricamento da entrambe le posizioni.

Strumenti e servizi che è possibile usare per spostare i dati in Archiviazione di Azure:

- Il servizio [Azure ExpressRoute](../expressroute/expressroute-introduction.md) migliora la velocità effettiva della rete, le prestazioni e la prevedibilità. ExpressRoute è un servizio che instrada i dati tramite una connessione privata dedicata ad Azure. Le connessioni ExpressRoute non instradano i dati attraverso la rete Internet pubblica. Queste connessioni offrono maggiore affidabilità, velocità più elevate, latenze minori e sicurezza superiore rispetto alle tipiche connessioni tramite la rete Internet pubblica.
- L'[utilità AZCopy](../storage/common/storage-moving-data.md) sposta i dati in Archiviazione di Azure tramite la rete Internet pubblica. Si tratta di un'opzione appropriata se le dimensioni dei dati sono inferiori a 10 TB. Per eseguire regolarmente caricamenti con AZCopy, assicurasi che la velocità di rete sia accettabile. 
- [Azure Data Factory (ADF)](../data-factory/introduction.md) include un gateway che è possibile installare nel server locale. È quindi possibile creare una pipeline per spostare i dati dal server locale ad Archiviazione di Azure. Per l'uso di Data Factory con SQL Data Warehouse, vedere [Caricare dati in Azure SQL Data Warehouse tramite Azure Data Factory](/azure/data-factory/load-azure-sql-data-warehouse).


## <a name="3-prepare-the-data-for-loading"></a>3. Preparare i dati per il caricamento

Potrebbe essere necessario preparare e pulire i dati nell'account di archiviazione prima di caricarli in SQL Data Warehouse. La preparazione dei dati può essere eseguita nella posizione di origine dei dati, mentre si esportano i dati in file di testo o quando i dati raggiungono Archiviazione di Azure.  È più facile lavorare con i dati il prima possibile nel processo.  

### <a name="define-external-tables"></a>Definire tabelle esterne

Prima di caricare i dati, è necessario definire le tabelle esterne nel data warehouse. PolyBase usa le tabelle esterne per definire i dati e accedervi in Archiviazione di Azure. Una tabella esterna è simile a una vista di database. La tabella esterna contiene lo schema di tabella e punta a dati archiviati all'esterno del data warehouse. 

La definizione di tabelle esterne include la specifica dell'origine dati, del formato dei file di testo e delle definizioni delle tabelle. Questi sono gli argomenti della sintassi T-SQL a cui fare riferimento:
- [CREATE EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql)
- [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql)
- [CREATE EXTERNAL TABLE](/sql/t-sql/statements/create-external-table-transact-sql)

Per un esempio di creazione di oggetti esterni, vedere il passaggio [Creare una tabella esterna](load-data-from-azure-blob-storage-using-polybase.md#create-external-tables-for-the-sample-data) nell'esercitazione per il caricamento.

### <a name="format-text-files"></a>Formattare i file di testo

Dopo aver definito gli oggetti esterni, è necessario allineare le righe dei file di testo alla definizione della tabella esterna e del formato del file. I dati in ogni riga del file di testo devono essere allineati alla definizione della tabella.
Per formattare i file di testo:

- Se i dati provengono da un'origine non relazionale, è necessario trasformarli in righe e colonne. Sia che i dati provengano da un'origine relazionale o non relazionale, devono essere trasformati per allinearli alle definizioni di colonna per la tabella in cui si prevede di caricare i dati. 
- Formattare i dati nel file di testo per allinearli alle colonne e ai tipi di dati nella tabella di destinazione di SQL Data Warehouse. In caso di non allineamento dei tipi di dati nei file di testo esterni e nella tabella del data warehouse, le righe verranno rifiutate durante il caricamento.
- Separare i campi nel file di testo con un carattere di terminazione.  Assicurarsi di usare un carattere o una sequenza di caratteri non inclusi nei dati di origine. Usare il carattere di terminazione specificato con [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql).


## <a name="4-load-the-data-into-sql-data-warehouse-staging-tables-using-polybase"></a>4. Caricare i dati in tabelle di staging di SQL Data Warehouse usando PolyBase

È consigliabile caricare i dati in una tabella di staging. Le tabelle di staging consentono di gestire gli errori senza interferire con le tabelle di produzione. Una tabella di staging offre anche l'opportunità di usare l'elaborazione MPP di SQL Data Warehouse per eseguire trasformazioni di dati prima di inserirli nelle tabelle di produzione.

### <a name="options-for-loading-with-polybase"></a>Opzioni per il caricamento con PolyBase

Per caricare i dati con PolyBase, è possibile usare una di queste opzioni di caricamento:

- [PolyBase con T-SQL](load-data-from-azure-blob-storage-using-polybase.md): ideale quando i dati sono nell'archivio BLOB di Azure o in Azure Data Lake Store. Questa opzione offre il massimo controllo sul processo di caricamento, ma richiede anche di definire oggetti dati esterni. Gli altri metodi definiscono questi oggetti dietro le quinte, man mano che si esegue il mapping di tabelle di origine e tabelle di destinazione.  Per orchestrare i caricamenti con T-SQL, è possibile usare Azure Data Factory, SSIS o funzioni di Azure. 
- [PolyBase con SSIS](/sql/integration-services/load-data-to-sql-data-warehouse): ideale quando i dati di origine sono in SQL Server, in locale o nel cloud. SSIS definisce i mapping delle tabelle di origine e di destinazione, oltre a orchestrare il caricamento. Se sono già disponibili pacchetti SSIS, è possibile modificarli per utilizzare la nuova destinazione di data warehouse. 
- [PolyBase con Azure Data Factory (ADF)](sql-data-warehouse-load-with-data-factory.md) è un altro strumento di orchestrazione,  che definisce una pipeline e pianifica i processi. 
- La [polibase con Azure](../azure-databricks/databricks-extract-load-sql-data-warehouse.md) databricks trasferisce i dati da una tabella SQL data warehouse a un frame di dati databricks e/o scrive i dati da un dataframe di databricks in una tabella SQL data warehouse usando la polibase.

### <a name="non-polybase-loading-options"></a>Opzioni di caricamento non PolyBase

Se i dati non sono compatibili con PolyBase, è possibile usare [bcp](/sql/tools/bcp-utility) o l'[API SQLBulkCopy](https://msdn.microsoft.com/library/system.data.sqlclient.sqlbulkcopy.aspx). bcp carica direttamente i dati in SQL Data Warehouse senza dover passare attraverso l'archivio BLOB di Azure ed è destinato esclusivamente a piccoli caricamenti. Si noti che le prestazioni di caricamento di queste opzioni sono notevolmente inferiori rispetto a PolyBase. 


## <a name="5-transform-the-data"></a>5. Trasformare i dati

Mentre i dati sono nella tabella di staging, eseguire le trasformazioni richieste dal carico di lavoro, quindi spostare i dati in una tabella di produzione.


## <a name="6-insert-the-data-into-production-tables"></a>6. Inserire i dati in tabelle di produzione

L'istruzione INSERT INTO... SELECT sposta i dati dalla tabella di staging alla tabella permanente. 

Quando si progetta un processo ETL, provare a eseguire il processo su un campione di test di piccole dimensioni. Provare a estrarre 1000 righe dalla tabella in un file, spostarlo in Azure e quindi provare a caricarlo in una tabella di staging. 


## <a name="partner-loading-solutions"></a>Soluzioni di caricamento dei partner

Molti partner Microsoft dispongono di soluzioni di caricamento. Per altre informazioni, vedere l'elenco dei [partner che offrono soluzioni](sql-data-warehouse-partner-business-intelligence.md). 


## <a name="next-steps"></a>Passaggi successivi

Per linee guida relative al caricamento, vedere [Linee guida per il caricamento di dati](guidance-for-loading-data.md).


