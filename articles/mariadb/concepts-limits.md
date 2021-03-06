---
title: Limitazioni-database di Azure per MariaDB
description: Questo articolo descrive i limiti di Database di Azure per MariaDB, ad esempio il numero di connessioni e le opzioni del motore di archiviazione.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/9/2020
ms.openlocfilehash: c982181dee34a7eb0715d5e1271ef5ed794f3809
ms.sourcegitcommit: c29b7870f1d478cec6ada67afa0233d483db1181
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2020
ms.locfileid: "79296745"
---
# <a name="limitations-in-azure-database-for-mariadb"></a>Limiti di Database di Azure per MariaDB
Le sezioni seguenti illustrano la capacità, il supporto del motore di archiviazione, dei privilegi e delle istruzioni di gestione dei dati e i limiti funzionali del servizio di database.

## <a name="server-parameters"></a>Parametri del server

I valori minimo e massimo di diversi parametri server comuni sono determinati dal piano tariffario e da vcore. Per i limiti, vedere le tabelle seguenti.

### <a name="max_connections"></a>max_connections

|**Piano tariffario**|**vCore**|**Valore predefinito**|**Valore minimo**|**Valore massimo**|
|---|---|---|---|---|
|Basic|1|50|10|50|
|Basic|2|100|10|100|
|Utilizzo generico|2|300|10|600|
|Utilizzo generico|4|625|10|1250|
|Utilizzo generico|8|1250|10|2500|
|Utilizzo generico|16|2500|10|5000|
|Utilizzo generico|32|5000|10|10000|
|Utilizzo generico|64|10000|10|20000|
|Con ottimizzazione per la memoria|2|600|10|800|
|Con ottimizzazione per la memoria|4|1250|10|2500|
|Con ottimizzazione per la memoria|8|2500|10|5000|
|Con ottimizzazione per la memoria|16|5000|10|10000|
|Con ottimizzazione per la memoria|32|10000|10|20000|

Quando le connessioni superano il limite, è possibile che venga visualizzato l'errore seguente:
> ERROR 1040 (08004): Too many connections (ERRORE 1040 (08004): numero eccessivo di connessioni)

> [!IMPORTANT]
> Per un'esperienza ottimale, è consigliabile usare una connessione pool come ProxySQL per gestire in modo efficiente le connessioni.

La creazione di nuove connessioni client a MariaDB richiede tempo e una volta stabilite, queste connessioni occupano le risorse del database, anche in caso di inattività. La maggior parte delle applicazioni richiede molte connessioni di breve durata, che comunicano questa situazione. Il risultato è un minor numero di risorse disponibili per il carico di lavoro effettivo, causando una riduzione delle prestazioni. Un pool di connessione che riduce le connessioni inattive e riutilizza le connessioni esistenti consente di evitare questo problema. Per informazioni sulla configurazione di ProxySQL, visitare il [post di Blog](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/load-balance-read-replicas-using-proxysql-in-azure-database-for/ba-p/880042).

## <a name="query_cache_size"></a>query_cache_size

Per impostazione predefinita, la cache delle query è disattivata. Per abilitare la cache delle query, configurare il parametro `query_cache_type`. 

Per ulteriori informazioni su questo parametro, vedere la [documentazione di MariaDB](https://mariadb.com/kb/en/server-system-variables/#query_cache_size) .

|**Piano tariffario**|**vCore**|**Valore predefinito**|**Valore minimo**|**Valore massimo**|
|---|---|---|---|---|
|Basic|1|Non configurabile nel livello Basic|N/D|N/D|
|Basic|2|Non configurabile nel livello Basic|N/D|N/D|
|Utilizzo generico|2|0|0|16777216|
|Utilizzo generico|4|0|0|33554432|
|Utilizzo generico|8|0|0|67108864|
|Utilizzo generico|16|0|0|134217728|
|Utilizzo generico|32|0|0|134217728|
|Utilizzo generico|64|0|0|134217728|
|Con ottimizzazione per la memoria|2|0|0|33554432|
|Con ottimizzazione per la memoria|4|0|0|67108864|
|Con ottimizzazione per la memoria|8|0|0|134217728|
|Con ottimizzazione per la memoria|16|0|0|134217728|
|Con ottimizzazione per la memoria|32|0|0|134217728|

## <a name="sort_buffer_size"></a>sort_buffer_size

Per ulteriori informazioni su questo parametro, vedere la [documentazione di MariaDB](https://mariadb.com/kb/en/server-system-variables/#sort_buffer_size) .

|**Piano tariffario**|**vCore**|**Valore predefinito**|**Valore minimo**|**Valore massimo**|
|---|---|---|---|---|
|Basic|1|Non configurabile nel livello Basic|N/D|N/D|
|Basic|2|Non configurabile nel livello Basic|N/D|N/D|
|Utilizzo generico|2|524288|32768|4194304|
|Utilizzo generico|4|524288|32768|8388608|
|Utilizzo generico|8|524288|32768|16777216|
|Utilizzo generico|16|524288|32768|33554432|
|Utilizzo generico|32|524288|32768|33554432|
|Utilizzo generico|64|524288|32768|33554432|
|Con ottimizzazione per la memoria|2|524288|32768|8388608|
|Con ottimizzazione per la memoria|4|524288|32768|16777216|
|Con ottimizzazione per la memoria|8|524288|32768|33554432|
|Con ottimizzazione per la memoria|16|524288|32768|33554432|
|Con ottimizzazione per la memoria|32|524288|32768|33554432|

## <a name="join_buffer_size"></a>join_buffer_size

Per ulteriori informazioni su questo parametro, vedere la [documentazione di MariaDB](https://mariadb.com/kb/en/server-system-variables/#join_buffer_size) .

|**Piano tariffario**|**vCore**|**Valore predefinito**|**Valore minimo**|**Valore massimo**|
|---|---|---|---|---|
|Basic|1|Non configurabile nel livello Basic|N/D|N/D|
|Basic|2|Non configurabile nel livello Basic|N/D|N/D|
|Utilizzo generico|2|262144|128|268435455|
|Utilizzo generico|4|262144|128|536870912|
|Utilizzo generico|8|262144|128|1073741824|
|Utilizzo generico|16|262144|128|2147483648|
|Utilizzo generico|32|262144|128|4294967295|
|Utilizzo generico|64|262144|128|4294967295|
|Con ottimizzazione per la memoria|2|262144|128|536870912|
|Con ottimizzazione per la memoria|4|262144|128|1073741824|
|Con ottimizzazione per la memoria|8|262144|128|2147483648|
|Con ottimizzazione per la memoria|16|262144|128|4294967295|
|Con ottimizzazione per la memoria|32|262144|128|4294967295|

## <a name="max_heap_table_size"></a>max_heap_table_size

Per ulteriori informazioni su questo parametro, vedere la [documentazione di MariaDB](https://mariadb.com/kb/en/server-system-variables/#max_heap_table_size) .

|**Piano tariffario**|**vCore**|**Valore predefinito**|**Valore minimo**|**Valore massimo**|
|---|---|---|---|---|
|Basic|1|Non configurabile nel livello Basic|N/D|N/D|
|Basic|2|Non configurabile nel livello Basic|N/D|N/D|
|Utilizzo generico|2|16777216|16384|268435455|
|Utilizzo generico|4|16777216|16384|536870912|
|Utilizzo generico|8|16777216|16384|1073741824|
|Utilizzo generico|16|16777216|16384|2147483648|
|Utilizzo generico|32|16777216|16384|4294967295|
|Utilizzo generico|64|16777216|16384|4294967295|
|Con ottimizzazione per la memoria|2|16777216|16384|536870912|
|Con ottimizzazione per la memoria|4|16777216|16384|1073741824|
|Con ottimizzazione per la memoria|8|16777216|16384|2147483648|
|Con ottimizzazione per la memoria|16|16777216|16384|4294967295|
|Con ottimizzazione per la memoria|32|16777216|16384|4294967295|

## <a name="tmp_table_size"></a>tmp_table_size

Per ulteriori informazioni su questo parametro, vedere la [documentazione di MariaDB](https://mariadb.com/kb/en/server-system-variables/#tmp_table_size) .

|**Piano tariffario**|**vCore**|**Valore predefinito**|**Valore minimo**|**Valore massimo**|
|---|---|---|---|---|
|Basic|1|Non configurabile nel livello Basic|N/D|N/D|
|Basic|2|Non configurabile nel livello Basic|N/D|N/D|
|Utilizzo generico|2|16777216|1024|67108864|
|Utilizzo generico|4|16777216|1024|134217728|
|Utilizzo generico|8|16777216|1024|268435456|
|Utilizzo generico|16|16777216|1024|536870912|
|Utilizzo generico|32|16777216|1024|1073741824|
|Utilizzo generico|64|16777216|1024|1073741824|
|Con ottimizzazione per la memoria|2|16777216|1024|134217728|
|Con ottimizzazione per la memoria|4|16777216|1024|268435456|
|Con ottimizzazione per la memoria|8|16777216|1024|536870912|
|Con ottimizzazione per la memoria|16|16777216|1024|1073741824|
|Con ottimizzazione per la memoria|32|16777216|1024|1073741824|

## <a name="storage-engine-support"></a>Supporto del motore di archiviazione

### <a name="supported"></a>Supportato
- [InnoDB](https://mariadb.com/kb/en/library/xtradb-and-innodb/)
- [MEMORY](https://mariadb.com/kb/en/library/memory-storage-engine/)

### <a name="unsupported"></a>Unsupported
- [MyISAM](https://mariadb.com/kb/en/library/myisam-storage-engine/)
- [BLACKHOLE](https://mariadb.com/kb/en/library/blackhole/)
- [ARCHIVE](https://mariadb.com/kb/en/library/archive/)

## <a name="privilege-support"></a>Supporto dei privilegi

### <a name="unsupported"></a>Unsupported
- Ruolo DBA: molti parametri e impostazioni server possono accidentalmente influire in modo negativo sulle prestazioni del server o negare le proprietà ACID del sistema DBMS. Per mantenere quindi l'integrità del servizio e un contratto di servizio a livello di prodotto, il ruolo DBA non è esposto. L'account utente predefinito, costruito quando viene creata una nuova istanza di database, consente agli utenti di eseguire la maggior parte delle istruzioni DDL e DML nell'istanza di database gestita.
- Privilegi SUPER: in modo analogo, anche i [privilegi SUPER](https://mariadb.com/kb/en/library/grant/#global-privileges) presentano limitazioni.
- Definir: richiede privilegi Super per creare ed è limitato. Se vengono importati dati tramite backup, rimuovere i comandi `CREATE DEFINER` manualmente o tramite il comando `--skip-definer` quando si esegue mysqldump.

## <a name="data-manipulation-statement-support"></a>Supporto delle istruzioni di gestione dei dati

### <a name="supported"></a>Supportato
- L'istruzione `LOAD DATA INFILE` è supportata ma è necessario specificare il parametro `[LOCAL]` che deve essere indirizzato a un percorso UNC (archiviazione di Azure montata tramite SMB).

### <a name="unsupported"></a>Unsupported
- `SELECT ... INTO OUTFILE`

## <a name="functional-limitations"></a>Limitazioni funzionali

### <a name="scale-operations"></a>Operazioni di scalabilità
- Non è attualmente supportata la scalabilità dinamica tra i piani tariffari.
- La riduzione delle dimensioni di archiviazione del server non è supportato.

### <a name="server-version-upgrades"></a>Aggiornamenti della versione dei server
- La migrazione automatica tra le versioni del motore del database principale non è attualmente supportata.

### <a name="point-in-time-restore"></a>Ripristino temporizzato
- Quando si usa la funzionalità di ripristino temporizzato, il nuovo server viene creato con le stesse configurazioni del server su cui si basa.
- Il ripristino di un server eliminato non è supportato.

### <a name="subscription-management"></a>Gestione sottoscrizioni
- Lo spostamento dinamico di server creati in precedenza tra le sottoscrizioni e il gruppo di risorse non è attualmente supportato.

### <a name="vnet-service-endpoints"></a>Endpoint del servizio di rete virtuale
- Gli endpoint di servizio di rete virtuale sono supportati solo per i server per utilizzo generico e ottimizzati per la memoria.

### <a name="storage-size"></a>Dimensioni dello spazio di archiviazione
- Per i limiti di dimensioni di archiviazione per ogni piano tariffario, fare riferimento ai [piani tariffari](concepts-pricing-tiers.md) .

## <a name="current-known-issues"></a>Problemi attualmente noti
- Quando viene stabilita la connessione, l'istanza del server MariaDB visualizza una versione di server non corretta. Per ottenere la versione corretta del motore dell'istanza del server, usare il comando `select version();`.

## <a name="next-steps"></a>Passaggi successivi
- [Funzionalità disponibili in ogni livello di servizio](concepts-pricing-tiers.md)
- [Versioni supportate del database MariaDB](concepts-supported-versions.md)
