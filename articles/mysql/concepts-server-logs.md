---
title: Log di query lente-database di Azure per MySQL
description: Descrive i log di query lente disponibili nel database di Azure per MySQL e i parametri disponibili per l'abilitazione di diversi livelli di registrazione.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 01/28/2020
ms.openlocfilehash: 9a3a58cab2d9673a4660967e3a11d7f88900e718
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2020
ms.locfileid: "79269432"
---
# <a name="slow-query-logs-in-azure-database-for-mysql"></a>Log di query lente nel database di Azure per MySQL
Nel Database di Azure per MySQL, il log delle query lente è disponibile per gli utenti. L'accesso al log delle transazioni non è supportato. Il log delle query lente può essere usato per identificare eventuali colli di bottiglia delle prestazioni e procedere alla risoluzione dei problemi.

Per altre informazioni sul log delle query lente MySQL, vedere la [sezione relativa ai log di query lente](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html) del manuale di riferimento per MySQL.

## <a name="access-slow-query-logs"></a>Accedere ai log di query lente
È possibile elencare e scaricare i log di query lente di database di Azure per MySQL usando il portale di Azure e l'interfaccia della riga di comando di Azure.

Nel portale di Azure selezionare il server del Database di Azure per MySQL. Nell'intestazione **Monitoraggio** selezionare la pagina **Log del server**.

Per altre informazioni sull'interfaccia della riga di comando di Azure, vedere [configurare e accedere ai log di query lente usando l'interfaccia](howto-configure-server-logs-in-cli.md)della riga

Analogamente, è possibile inviare tramite pipe i log a monitoraggio di Azure usando i log di diagnostica. Per ulteriori informazioni, vedere di [seguito](concepts-server-logs.md#diagnostic-logs) .

## <a name="log-retention"></a>Conservazione dei log
I log sono disponibili per un massimo di sette giorni dalla data di creazione. Se le dimensioni totali dei log disponibili superano 7 GB, i file meno recenti vengono eliminati fino a quando non è disponibile dello spazio. 

I log vengono ruotati ogni 24 ore o 7 GB, a seconda del valore raggiunto per primo.

## <a name="configure-slow-query-logging"></a>Configurare la registrazione lenta delle query 
Per impostazione predefinita il log delle query lente è disabilitato. Per abilitarlo, impostare slow_query_log su ON.

Altri parametri che è possibile modificare includono:

- **long_query_time**: se una query richiede più tempo del valore di long_query_time (in secondi), la query viene registrata. Il valore predefinito è 10 secondi.
- **log_slow_admin_statements**: se è ON include le istruzioni a livello amministrativo come ALTER_TABLE e ANALYZE_TABLE nelle istruzioni scritte in slow_query_log.
- **log_queries_not_using_indexes**: determina se le query che non usano gli indici vengono registrate in slow_query_log
- **log_throttle_queries_not_using_indexes**: questo parametro limita il numero di query non di indice che possono essere scritte nel log di query lente. Questo parametro ha effetto quando log_queries_not_using_indexes è impostato su ON.
- **log_output**: se "file", consente la scrittura del log di query lente sia nella risorsa di archiviazione del server locale che nei log di diagnostica di monitoraggio di Azure. Se "None", il log di query lente verrà scritto solo nei log di diagnostica di monitoraggio di Azure. 

> [!IMPORTANT]
> Se le tabelle non sono indicizzate, l'impostazione dei parametri `log_queries_not_using_indexes` e `log_throttle_queries_not_using_indexes` su ON può influire sulle prestazioni di MySQL, perché tutte le query in esecuzione in queste tabelle non indicizzate verranno scritte nel log di query lente.<br><br>
> Se si prevede di registrare query lente per un periodo di tempo prolungato, è consigliabile impostare `log_output` su "None". Se impostato su "file", questi log vengono scritti nell'archivio locale del server e possono influenzare le prestazioni di MySQL. 

Per una descrizione completa dei parametri per il log di query lente, vedere la [documentazione sul log di query lente](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html) per MySQL.

## <a name="diagnostic-logs"></a>Log di diagnostica
Database di Azure per MySQL è integrato con i log di diagnostica di Monitoraggio di Azure. Dopo aver abilitato i log di query lente nel server MySQL, è possibile scegliere di crearli in log di monitoraggio di Azure, Hub eventi o archiviazione di Azure. Per altre informazioni sull'abilitazione dei log di diagnostica, vedere la sezione sulle procedure della [documentazione sui log di diagnostica](../azure-monitor/platform/platform-logs-overview.md).

La tabella seguente descrive il contenuto di ogni log. A seconda del metodo di output, è possibile che i campi inclusi e il relativo ordine di visualizzazione siano differenti.

| **Proprietà** | **Descrizione** |
|---|---|
| `TenantId` | ID del tenant. |
| `SourceSystem` | `Azure` |
| `TimeGenerated` [UTC] | Timestamp in cui il log è stato registrato in formato UTC. |
| `Type` | Tipo di log. Sempre `AzureDiagnostics` |
| `SubscriptionId` | GUID per la sottoscrizione a cui appartiene il server. |
| `ResourceGroup` | Nome del gruppo di risorse a cui appartiene il server. |
| `ResourceProvider` | Nome del provider di risorse. Sempre `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | URI della risorsa |
| `Resource` | Nome del server |
| `Category` | `MySqlSlowLogs` |
| `OperationName` | `LogEvent` |
| `Logical_server_name_s` | Nome del server |
| `start_time_t` [UTC] | Ora di inizio della query |
| `query_time_s` | Tempo totale in secondi impiegato per l'esecuzione della query |
| `lock_time_s` | Tempo totale in secondi per cui la query è stata bloccata |
| `user_host_s` | Username |
| `rows_sent_s` | Numero di righe inviate |
| `rows_examined_s` | Numero di righe esaminate |
| `last_insert_id_s` | [last_insert_id](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_last-insert-id) |
| `insert_id_s` | Inserisci ID |
| `sql_text_s` | Query completa |
| `server_id_s` | ID del server |
| `thread_id_s` | ID del thread |
| `\_ResourceId` | URI della risorsa |

> [!Note]
> Per `sql_text`, il log verrà troncato se supera i 2048 caratteri.

## <a name="analyze-logs-in-azure-monitor-logs"></a>Analizzare i log nei log di monitoraggio di Azure

Quando i log di query lente vengono inviati tramite pipe ai log di monitoraggio di Azure tramite i log di diagnostica, è possibile eseguire un'ulteriore analisi delle query lente. Di seguito sono riportate alcune query di esempio utili per iniziare. Assicurarsi di aggiornare quanto segue con il nome del server.

- Query con più di 10 secondi in un determinato server

    ```Kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | where query_time_d > 10
    ```

- Elenca le prime cinque query più lunghe su un server specifico

    ```Kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | order by query_time_d desc
    | take 5
    ```

- Riepilogare query lente per tempo di query minimo, massimo, medio e di deviazione standard in un determinato server

    ```Kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | summarize count(), min(query_time_d), max(query_time_d), avg(query_time_d), stdev(query_time_d), percentile(query_time_d, 95) by LogicalServerName_s
    ```

- Grafico della distribuzione di query lente in un determinato server

    ```Kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | summarize count() by LogicalServerName_s, bin(TimeGenerated, 5m)
    | render timechart
    ```

- Visualizza query per più di 10 secondi in tutti i server MySQL con i log di diagnostica abilitati

    ```Kusto
    AzureDiagnostics
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | where query_time_d > 10
    ```    
    
## <a name="next-steps"></a>Passaggi successivi
- [Come configurare i log di query lente dal portale di Azure](howto-configure-server-logs-in-portal.md)
- [Come configurare i log di query lente dall'interfaccia della riga di comando di Azure](howto-configure-server-logs-in-cli.md).
