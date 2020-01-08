---
title: 'Problemi noti: migrazioni online da PostgreSQL a database di Azure per PostgreSQL'
titleSuffix: Azure Database Migration Service
description: Informazioni sui problemi noti e sulle limitazioni della migrazione con migrazioni online da PostgreSQL a database di Azure per PostgreSQL-server singolo con il servizio migrazione del database di Azure.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom:
- seo-lt-2019
- seo-dt-2019
ms.topic: article
ms.date: 10/27/2019
ms.openlocfilehash: c5c0015c5034dd3b30b716264fd97e9881b3fe67
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75437863"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-from-postgresql-to-azure-db-for-postgresql-single-server"></a>Problemi noti/limitazioni della migrazione con migrazioni online da PostgreSQL a database di Azure per PostgreSQL-server singolo

Le sezioni seguenti illustrano i problemi noti e le limitazioni associate alle migrazioni online da PostgreSQL al database di Azure per PostgreSQL: un server singolo.

## <a name="online-migration-configuration"></a>Configurazione della migrazione online

- Il server PostgreSQL di origine deve eseguire la versione 9.5.11, 9.6.7 o 10,3 o successiva. Per altre informazioni, vedere l'articolo [Versioni supportate del database PostgreSQL](../postgresql/concepts-supported-versions.md).
- Sono supportate solo le migrazioni della stessa versione. Ad esempio, la migrazione di PostgreSQL 9.5.11 in Database di Azure per PostgreSQL 9.6.7 non è supportata.

    > [!NOTE]
    > Per PostgreSQL versione 10, il Servizio Migrazione del database supporta attualmente solo la migrazione della versione 10.3 a Database di Azure per PostgreSQL. Si prevede di supportare le versioni più recenti di PostgreSQL molto presto.

- Per abilitare la replica logica nel file **postgresql.config del server PostgreSQL di origine**, impostare i parametri seguenti:
  - **wal_level** = logical
  - **max_replication_slots** = [numero massimo di database per la migrazione]; Se si desidera eseguire la migrazione di quattro database, impostare il valore su 4.
  - **max_wal_senders** = [numero di database eseguiti contemporaneamente]; il valore consigliato è 10
- Aggiungere l'indirizzo IP dell'agente DMS al pg_hba PostgreSQL di origine. conf
  1. Prendere nota dell'indirizzo IP del Servizio Migrazione del database dopo aver completato il provisioning di un'istanza nel Servizio Migrazione del database.
  2. Aggiungere l'indirizzo IP al file pg_hba.conf come illustrato:

        host all 172.16.136.18/10 MD5 host Replication Postgres 172.16.136.18/10 MD5

- L'utente deve avere l'autorizzazione utente con privilegi avanzati per il server che ospita il database di origine
- A parte la presenza di ENUM nello schema del database di origine, gli schemi dei database di origine e di destinazione devono corrispondere.
- Lo schema nel database di Azure di destinazione per PostgreSQL-singolo server non deve avere chiavi esterne. Per rilasciare le chiavi esterne usare la query seguente:

    ```
                                SELECT Queries.tablename
           ,concat('alter table ', Queries.tablename, ' ', STRING_AGG(concat('DROP CONSTRAINT ', Queries.foreignkey), ',')) as DropQuery
                ,concat('alter table ', Queries.tablename, ' ', 
                                                STRING_AGG(concat('ADD CONSTRAINT ', Queries.foreignkey, ' FOREIGN KEY (', column_name, ')', 'REFERENCES ', foreign_table_name, '(', foreign_column_name, ')' ), ',')) as AddQuery
        FROM
        (SELECT
        tc.table_schema, 
        tc.constraint_name as foreignkey, 
        tc.table_name as tableName, 
        kcu.column_name, 
        ccu.table_schema AS foreign_table_schema,
        ccu.table_name AS foreign_table_name,
        ccu.column_name AS foreign_column_name 
    FROM 
        information_schema.table_constraints AS tc 
        JOIN information_schema.key_column_usage AS kcu
          ON tc.constraint_name = kcu.constraint_name
          AND tc.table_schema = kcu.table_schema
        JOIN information_schema.constraint_column_usage AS ccu
          ON ccu.constraint_name = tc.constraint_name
          AND ccu.table_schema = tc.table_schema
    WHERE constraint_type = 'FOREIGN KEY') Queries
      GROUP BY Queries.tablename;
    
    ```

    Eseguire drop foreign key, ovvero la seconda colonna, sul risultato della query.

- Lo schema nel database di Azure di destinazione per PostgreSQL-server singolo non deve avere alcun trigger. Per disabilitare i trigger nel database di destinazione:

     ```
    SELECT Concat('DROP TRIGGER ', Trigger_Name, ';') FROM  information_schema.TRIGGERS WHERE TRIGGER_SCHEMA = 'your_schema';
     ```

## <a name="datatype-limitations"></a>Limitazioni relative ai tipi di dati

- **Limitazione**: se è presente un tipo di dati ENUM nel database PostgreSQL di origine, la migrazione ha esito negativo durante la sincronizzazione continua.

    **Soluzione alternativa**: modificare il tipo di dati ENUM nella variante carattere nell'istanza di Database di Azure per PostgreSQL.

- **Limitazione**: se non è presente alcuna chiave primaria nelle tabelle, la sincronizzazione continua ha esito negativo.

    **Soluzione alternativa**: impostare temporaneamente una chiave primaria per la tabella per consentire alla migrazione di continuare. Al termine della migrazione dei dati, è possibile rimuovere la chiave primaria.

- **Limitazione**: il tipo di dati JSONB non è supportato per la migrazione.

## <a name="lob-limitations"></a>Limitazioni relative ai tipi di dati LOB

Le colonne LOB (Large Object) sono colonne che possono raggiungere dimensioni elevate. Per PostgreSQL, gli esempi di tipi di dati LOB includono XML, JSON, IMAGE, TEXT e così via.

- **Limitazione**: se come chiavi primarie vengono usati tipi di dati LOB, la migrazione ha esito negativo.

    **Soluzione alternativa**: sostituire la chiave primaria con altri tipi di dati o colonne non LOB.

- **Limitazione**: se la lunghezza della colonna LOB è maggiore di 32 kB, è possibile che nella destinazione i dati vengano troncati. È possibile controllare la lunghezza della colonna LOB usando questa query:

    ```
    SELECT max(length(cast(body as text))) as body FROM customer_mail
    ```

    **Soluzione alternativa**: se si dispone di un oggetto LOB di dimensioni maggiori di 32 KB, contattare il team di progettazione di per [chiedere alle migrazioni del database di Azure](mailto:AskAzureDatabaseMigrations@service.microsoft.com).

- **Limitazione**: se nella tabella sono presenti colonne LOB e non viene impostata una chiave primaria per la tabella, è possibile che la migrazione dei dati non venga eseguita per questa tabella.

    **Soluzione alternativa**: impostare temporaneamente una chiave primaria per la tabella per consentire alla migrazione di continuare. Al termine della migrazione dei dati, è possibile rimuovere la chiave primaria.

## <a name="postgresql10-workaround"></a>Soluzione alternativa per PostgreSQL10

PostgreSQL 10.x apporta diverse modifiche ai nomi delle cartelle pg_xlog e per questo la migrazione non viene eseguita come previsto. Se si esegue la migrazione da PostgreSQL 10.x al Database di Azure per PostgreSQL 10.3, eseguire lo script seguente nel database PostgreSQL di origine per creare una funzione wrapper intorno alle funzioni pg_xlog.

```
BEGIN;
CREATE SCHEMA IF NOT EXISTS fnRenames;
CREATE OR REPLACE FUNCTION fnRenames.pg_switch_xlog() RETURNS pg_lsn AS $$ 
   SELECT pg_switch_wal(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_xlog_replay_pause() RETURNS VOID AS $$ 
   SELECT pg_wal_replay_pause(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_xlog_replay_resume() RETURNS VOID AS $$ 
   SELECT pg_wal_replay_resume(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_current_xlog_location() RETURNS pg_lsn AS $$ 
   SELECT pg_current_wal_lsn(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_is_xlog_replay_paused() RETURNS boolean AS $$ 
   SELECT pg_is_wal_replay_paused(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_xlogfile_name(lsn pg_lsn) RETURNS TEXT AS $$ 
   SELECT pg_walfile_name(lsn); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_last_xlog_replay_location() RETURNS pg_lsn AS $$ 
   SELECT pg_last_wal_replay_lsn(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_last_xlog_receive_location() RETURNS pg_lsn AS $$ 
   SELECT pg_last_wal_receive_lsn(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_current_xlog_flush_location() RETURNS pg_lsn AS $$ 
   SELECT pg_current_wal_flush_lsn(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_current_xlog_insert_location() RETURNS pg_lsn AS $$ 
   SELECT pg_current_wal_insert_lsn(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_xlog_location_diff(lsn1 pg_lsn, lsn2 pg_lsn) RETURNS NUMERIC AS $$ 
   SELECT pg_wal_lsn_diff(lsn1, lsn2); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_xlogfile_name_offset(lsn pg_lsn, OUT TEXT, OUT INTEGER) AS $$ 
   SELECT pg_walfile_name_offset(lsn); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_create_logical_replication_slot(slot_name name, plugin name, 
   temporary BOOLEAN DEFAULT FALSE, OUT slot_name name, OUT xlog_position pg_lsn) RETURNS RECORD AS $$ 
   SELECT slot_name::NAME, lsn::pg_lsn FROM pg_catalog.pg_create_logical_replication_slot(slot_name, plugin, 
   temporary); $$ LANGUAGE SQL;
ALTER USER PG_User SET search_path = fnRenames, pg_catalog, "$user", public;

-- DROP SCHEMA fnRenames CASCADE;
-- ALTER USER PG_User SET search_path TO DEFAULT;
COMMIT;
```

  > [!NOTE]
  > Nello script precedente, "PG_User" si riferisce al nome utente usato per la connessione all'origine della migrazione.

## <a name="limitations-when-migrating-online-from-aws-rds-postgresql"></a>Limitazioni per la migrazione in linea da AWS Servizi Desktop remoto PostgreSQL

Quando si tenta di eseguire una migrazione in linea da AWS Servizi Desktop remoto da AWS a database di Azure per PostgreSQL, è possibile che si verifichino i seguenti errori.

- **Errore**: il valore predefinito della colonna ' {column}' nella tabella ' {Table}' nel database ' {database}' è diverso nei server di origine e di destinazione. È "{value on source}" nell'origine e "{value on target}" nella destinazione.

  **Limitazione**: questo errore si verifica quando il valore predefinito in uno schema di colonna è diverso tra i database di origine e di destinazione.
  **Soluzione temporanea**: assicurarsi che lo schema nella destinazione corrisponda allo schema nell'origine. Per informazioni dettagliate sulla migrazione dello schema, vedere la [documentazione relativa alla migrazione in linea di Azure PostgreSQL](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online#migrate-the-sample-schema).

- **Errore**: il database di destinazione ' {database}' contiene ' {Number of tables}' tabelle in cui il database di origine ' {database}' contiene ' {Number of tables}' tabelle. Il numero di tabelle nei database di origine e di destinazione deve corrispondere.

  **Limitazione**: questo errore si verifica quando il numero di tabelle è diverso tra i database di origine e di destinazione.
  **Soluzione temporanea**: assicurarsi che lo schema nella destinazione corrisponda allo schema nell'origine. Per informazioni dettagliate sulla migrazione dello schema, vedere la [documentazione relativa alla migrazione in linea di Azure PostgreSQL](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online#migrate-the-sample-schema).

- **Errore:** Il database di origine {database} è vuoto.

  **Limitazione**: questo errore si verifica quando il database di origine è vuoto. Ciò è probabilmente dovuto al fatto che è stato selezionato il database errato come origine.
  **Soluzione alternativa**: controllare il database di origine selezionato per la migrazione, quindi riprovare.

- **Errore:** Il database di destinazione {database} è vuoto. Eseguire la migrazione dello schema.

  **Limitazione**: questo errore si verifica quando non è presente alcuno schema nel database di destinazione. Verificare che lo schema nella destinazione corrisponda allo schema nell'origine.
  **Soluzione temporanea**: assicurarsi che lo schema nella destinazione corrisponda allo schema nell'origine. Per informazioni dettagliate sulla migrazione dello schema, vedere la [documentazione relativa alla migrazione in linea di Azure PostgreSQL](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online#migrate-the-sample-schema).

## <a name="other-limitations"></a>Altre limitazioni

- Il nome del database non può includere un punto e virgola (;).
- La stringa della password con parentesi graffe di apertura e chiusura { } non è supportata. Questa limitazione si applica a entrambe le connessioni, sia al server PostgreSQL di origine sia all'istanza di Database di Azure per PostgreSQL.
- Una tabella acquisita deve avere una chiave primaria. Se una tabella non contiene una chiave primaria, il risultato delle operazioni DELETE e UPDATE sui record sarà imprevedibile.
- L'aggiornamento di un segmento di chiave primaria viene ignorato. In questi casi, se applicato l'aggiornamento verrà identificato dalla destinazione come un aggiornamento che non ha aggiornato alcuna riga e verrà scritto un record nella tabella delle eccezioni.
- La migrazione di più tabelle con lo stesso nome ma un diverso uso di maiuscole e minuscole (ad esempio, table1, TABLE1 e Table1) può causare un comportamento imprevedibile e quindi non è supportata.
- L'elaborazione delle modifiche per le DDL della tabella [CREATE | ALTER | DROP] è supportata a meno che le DDL non siano incluse in un blocco di corpo della funzione o routine interna o in altri costrutti annidati. Ad esempio, la modifica seguente non verrà acquisita:

    ```
    CREATE OR REPLACE FUNCTION pg.create_distributors1() RETURNS void
    LANGUAGE plpgsql
    AS $$
    BEGIN
    create table pg.distributors1(did serial PRIMARY KEY,name varchar(40)
    NOT NULL);
    END;
    $$;
    ```

- L'elaborazione delle modifiche (sincronizzazione continua) delle operazioni di TRONCAmento non è supportata. La migrazione delle tabelle partizionate non è supportata. Quando viene rilevata una tabella partizionata, si verifica quanto segue:

  - Il database segnala un elenco delle tabelle padre e figlio.
  - La tabella viene creata nella destinazione come una normale tabella con le stesse proprietà delle tabelle selezionate.
  - Se la tabella padre nel database di origine ha lo stesso valore di chiave primaria delle tabelle figlio, verrà generato un errore di "chiave duplicata".

- Nel Servizio Migrazione del database il limite di database per eseguire la migrazione con una singola attività è quattro.
