---
title: Risoluzione dei problemi relativi ad Azure SQL Data Warehouse | Microsoft Docs
description: Risoluzione dei problemi relativi a SQL Data Warehouse di Azure.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 7/29/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: a6a6fdf6e63bf8c063f8dd6f23ae380e9ce7b98d
ms.sourcegitcommit: 5ded08785546f4a687c2f76b2b871bbe802e7dae
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/19/2019
ms.locfileid: "69575519"
---
# <a name="troubleshooting-azure-sql-data-warehouse"></a>Risoluzione dei problemi relativi a SQL Data Warehouse di Azure
In questo articolo sono elencate le domande frequenti relative alla risoluzione dei problemi.

## <a name="connecting"></a>Connessione in corso
| Problema                                                        | Risoluzione                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Accesso non riuscito per l'utente 'NT AUTHORITY\ANONYMOUS LOGON'. (Microsoft SQL Server, Errore: 18456) | Questo errore si verifica quando un utente AAD tenta di connettersi al database master, ma non dispone di un utente nel database master.  Per risolvere questo problema specificare l'istanza di SQL Data Warehouse a cui si desidera connettersi al momento della connessione o aggiungere l'utente al database master.  Per ulteriori dettagli, vedere l'articolo [Panoramica della sicurezza][Security overview] . |
| L'entità server "MyUserName" non può accedere al database "master" nel contesto di sicurezza corrente. Impossibile aprire il database predefinito dell'utente. Accesso non riuscito. Accesso non riuscito per l'utente 'MyUserName'. (Microsoft SQL Server, Errore: 916) | Questo errore si verifica quando un utente AAD tenta di connettersi al database master, ma non dispone di un utente nel database master.  Per risolvere questo problema specificare l'istanza di SQL Data Warehouse a cui si desidera connettersi al momento della connessione o aggiungere l'utente al database master.  Per ulteriori dettagli, vedere l'articolo [Panoramica della sicurezza][Security overview] . |
| Errore CTAIP                                                  | Questo errore può verificarsi quando è stato creato un account di accesso nel database master di SQL Server, ma non nel database di SQL Data Warehouse.  Se si verifica questo errore, vedere l'articolo [Panoramica della sicurezza][Security overview] .  Questo articolo illustra come creare un account di accesso e un utente in un database master e come creare un utente nel database di SQL Data Warehouse. |
| Blocco da parte del firewall                                          | I database SQL di Azure sono protetti da firewall a livello di server e di database per garantire che solo gli indirizzi IP noti accedano a un database. I firewall sono protetti per impostazione predefinita, il che significa che è necessario abilitare in modo esplicito un indirizzo IP o un intervallo di indirizzi prima di potersi connettere.  Per configurare il firewall per l'accesso, seguire la procedura descritta in [configurare l'accesso al firewall del server per l'indirizzo IP client][Configure server firewall access for your client IP] nelle [istruzioni][Provisioning instructions]di provisioning. |
| Impossibile connettersi con lo strumento o il driver                           | SQL Data Warehouse consiglia di usare [SSMS][SSMS], [SSDT per Visual Studio][SSDT for Visual Studio]o [SQLCMD][sqlcmd] per eseguire query sui dati. Per ulteriori informazioni sui driver e sulla connessione a SQL Data Warehouse, vedere [driver per Azure SQL data warehouse][Drivers for Azure SQL Data Warehouse] e [connettersi agli articoli di Azure SQL data warehouse][Connect to Azure SQL Data Warehouse] . |

## <a name="tools"></a>Strumenti
| Problema                                                        | Risoluzione                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Esplora oggetti di Visual Studio non riconosce gli utenti di AAD           | Si tratta di un problema noto.  Come soluzione alternativa è possibile visualizzare gli utenti in [sys.database_principals][sys.database_principals].  Per maggiori informazioni sull'uso di Azure Active Directory con SQL Data Warehouse, vedere [Autenticazione in Azure SQL Data Warehouse][Authentication to Azure SQL Data Warehouse] . |
| La creazione manuale di script, l'utilizzo di script o la connessione tramite SSMS è lenta, non risponde o produce errori | Assicurarsi che gli utenti siano stati creati nel database master. Nelle opzioni di scripting, assicurarsi inoltre che l'edizione del motore sia impostata come "Edizione Microsoft Azure SQL Data Warehouse" e che il tipo di motore sia "Database SQL di Microsoft Azure". |
| Errore della generazione di script in SSMS                               | La generazione di uno script per SQL Data Warehouse ha esito negativo se l'opzione "genera script per oggetti dipendenti" è impostata su "true". Per risolvere il problema, gli utenti devono passare manualmente a Strumenti -> Opzioni -> Esplora oggetti di SQL Server -> Genera script per oggetti dipendenti e impostare l'opzione su false |

## <a name="performance"></a>Prestazioni
| Problema                                                        | Risoluzione                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Risoluzione dei problemi di prestazioni delle query                            | Se si sta cercando di risolvere i problemi relativi a una determinata query, un ottimo punto di partenza è l'articolo su come [imparare a monitorare le query][Learning how to monitor your queries]. |
| Piani e prestazioni delle query di scarsa qualità sono spesso causati dalla mancanza di statistiche | La causa più comune di prestazioni di scarsa qualità è la mancanza di statistiche per le tabelle.  Per informazioni dettagliate su come creare statistiche e sul motivo per cui le prestazioni sono fondamentali, vedere [gestione delle statistiche delle tabelle][Statistics] . |
| Concorrenza bassa/query in coda                             | Comprendere la [gestione del carico di lavoro][Workload management] è importante per capire come bilanciare l'allocazione di memoria con la concorrenza. |
| Come implementare le procedure consigliate                              | L'articolo [Procedure consigliate per SQL Data Warehouse][SQL Data Warehouse best practices] è un ottimo punto di partenza per informazioni su come migliorare le prestazioni delle query. |
| Come migliorare le prestazioni con la scalabilità                      | La soluzione per aumentare le prestazioni consiste a volte nell'aggiungere semplicemente maggiore potenza di calcolo alle query [ridimensionando SQL Data Warehouse][Scaling your SQL Data Warehouse]. |
| Scarse prestazioni delle query a causa di scarsa qualità degli indici     | Alcune volte le query possono rallentare a causa della [scarsa qualità][Poor columnstore index quality]degli indici columnstore.  Vedere questo articolo per ulteriori informazioni e per capire come [ricompilare gli indici per migliorare la qualità dei segmenti][Rebuild indexes to improve segment quality]. |

## <a name="system-management"></a>Gestione del sistema
| Problema                                                        | Risoluzione                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Messaggio 40847: Non è stato possibile eseguire l'operazione perché il server avrebbe superato la quota di DTU consentita di 45000. | Ridurre il valore di [DWU][DWU] del database che si sta tentando di creare oppure [richiedere un aumento della quota][request a quota increase]. |
| Analisi dell'uso dello spazio                              | Per comprendere l'uso dello spazio nel sistema, vedere la [tabella sulle dimensioni][Table sizes] . |
| Aiuto nella gestione delle tabelle                                    | Per informazioni sulla gestione delle tabelle, vedere l'articolo [Panoramica della tabella][Overview] .  Questo articolo include inoltre collegamenti ad argomenti più dettagliati, ad esempio [tipi di dati di tabella][Data types], [distribuzione di una][Distribute]tabella, indicizzazione [di una tabella][Index], [partizionamento di una tabella][Partition], [gestione delle statistiche][Statistics] delle tabelle e [tabelle temporanee][Temporary]. |
| L'indicatore di stato Transparent Data Encryption (Transparent Data Encryption) non viene aggiornato nella portale di Azure | È possibile visualizzare lo stato di TDE tramite [powershell](/powershell/module/az.sql/get-azsqldatabasetransparentdataencryption). |


## <a name="differences-from-sql-database"></a>Differenze rispetto al database SQL
| Problema                                 | Risoluzione                                                   |
| :------------------------------------ | :----------------------------------------------------------- |
| Funzionalità non supportate del database SQL     | Vedere [Funzionalità non supportate delle tabelle][Unsupported table features]. |
| Tipi di dati non supportati del database SQL   | Vedere [Tipi di dati non supportati][Unsupported data types].        |
| Limitazioni DELETE e UPDATE         | Vedere le [soluzioni alternative][UPDATE workarounds]per gli aggiornamenti, [eliminare le soluzioni alternative][DELETE workarounds] e [usare CTAs per aggirare la sintassi di aggiornamento e eliminazione non supportata][Using CTAS to work around unsupported UPDATE and DELETE syntax]. |
| Istruzione MERGE non supportata      | Vedere [Soluzioni alternative MERGE][MERGE workarounds].                  |
| Limitazioni delle stored procedure          | Per capire alcune limitazioni delle stored procedure, vedere [Limitazioni delle stored procedure][Stored procedure limitations] . |
| Le UDF non supportano istruzioni SELECT | Si tratta di una limitazione corrente delle UDF.  Per conoscere la sintassi supportata, vedere [CREATE FUNCTION][CREATE FUNCTION] . |

## <a name="next-steps"></a>Passaggi successivi
Se non si riesce a trovare una soluzione al problema, ecco alcune altre risorse che è possibile provare.

* [Blog]
* [Richieste di funzionalità]
* [Video]
* [Blog del team CAT]
* [Creare un ticket di supporto]
* [Forum MSDN]
* [Forum su Stack Overflow]
* [Twitter]

<!--Image references-->

<!--Article references-->
[Security overview]: sql-data-warehouse-overview-manage-security.md
[SSMS]: /sql/ssms/download-sql-server-management-studio-ssms
[SSDT for Visual Studio]: sql-data-warehouse-install-visual-studio.md
[Drivers for Azure SQL Data Warehouse]: sql-data-warehouse-connection-strings.md
[Connect to Azure SQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Creare un ticket di supporto]: sql-data-warehouse-get-started-create-support-ticket.md
[Scaling your SQL Data Warehouse]: sql-data-warehouse-manage-compute-overview.md
[DWU]: sql-data-warehouse-overview-what-is.md
[request a quota increase]: sql-data-warehouse-get-started-create-support-ticket.md
[Learning how to monitor your queries]: sql-data-warehouse-manage-monitor.md
[Provisioning instructions]: sql-data-warehouse-get-started-provision.md
[Configure server firewall access for your client IP]: sql-data-warehouse-get-started-provision.md
[SQL Data Warehouse best practices]: sql-data-warehouse-best-practices.md
[Table sizes]: sql-data-warehouse-tables-overview.md#table-size-queries
[Unsupported table features]: sql-data-warehouse-tables-overview.md#unsupported-table-features
[Unsupported data types]: sql-data-warehouse-tables-data-types.md#unsupported-data-types
[Overview]: sql-data-warehouse-tables-overview.md
[Data types]: sql-data-warehouse-tables-data-types.md
[Distribute]: sql-data-warehouse-tables-distribute.md
[Index]: sql-data-warehouse-tables-index.md
[Partition]: sql-data-warehouse-tables-partition.md
[Statistics]: sql-data-warehouse-tables-statistics.md
[Temporary]: sql-data-warehouse-tables-temporary.md
[Poor columnstore index quality]: sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Rebuild indexes to improve segment quality]: sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Workload management]: resource-classes-for-workload-management.md
[Using CTAS to work around unsupported UPDATE and DELETE syntax]: sql-data-warehouse-develop-ctas.md
[UPDATE workarounds]: sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[DELETE workarounds]: sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[MERGE workarounds]: sql-data-warehouse-develop-ctas.md#replace-merge-statements
[Stored procedure limitations]: sql-data-warehouse-develop-stored-procedures.md#limitations
[Authentication to Azure SQL Data Warehouse]: sql-data-warehouse-authentication.md


<!--MSDN references-->
[sys.database_principals]: /sql/relational-databases/system-catalog-views/sys-database-principals-transact-sql
[CREATE FUNCTION]: /sql/t-sql/statements/create-function-sql-data-warehouse
[sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Blog]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Blog del team CAT]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Richieste di funzionalità]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Forum MSDN]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse
[Forum su Stack Overflow]: https://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Video]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[Databricks]: https://docs.microsoft.com/azure/azure-databricks/databricks-extract-load-sql-data-warehouse#load-data-into-azure-sql-data-warehouse
