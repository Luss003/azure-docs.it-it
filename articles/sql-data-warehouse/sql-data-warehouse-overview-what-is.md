---
title: Che cos'è SQL Data Warehouse di Azure? | Microsoft Docs
description: Database distribuito di livello aziendale in grado di elaborare volumi di dati relazionali e non relazionali anche nell'ordine di petabyte. Si tratta del primo data warehouse basato sul cloud del settore con capacità di aumento, riduzione e sospensione in pochi secondi.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: overview
ms.subservice: design
ms.date: 05/30/2019
ms.author: martinle
ms.reviewer: igorstan
mscustom: sqlfreshmay19
ms.openlocfilehash: a9126e9023091dd8c3df71f2aa2558a01227a8be
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66428026"
---
# <a name="what-is-azure-sql-data-warehouse"></a>Che cos'è SQL Data Warehouse di Azure?

SQL Data Warehouse è un data warehouse aziendale basato sul cloud che usa l'elaborazione MPP (Massively Parallel Processing) per eseguire rapidamente query complesse su petabyte di dati. Usare SQL Data Warehouse come componente chiave di una soluzione per Big Data. Importare i Big Data in SQL Data Warehouse con semplici query T-SQL [PolyBase](/sql/relational-databases/polybase/polybase-guide?view=sql-server-2017&viewFallbackFrom=azure-sqldw-latest) e quindi sfruttare la potenza dell'elaborazione MPP per eseguire analisi ad alte prestazioni. Man mano che si procede con l'integrazione e l'analisi, il data warehouse diventerà il punto di riferimento dell'azienda da cui ricavare informazioni dettagliate.  

## <a name="key-component-of-big-data-solution"></a>Componente chiave della soluzione per Big Data

SQL Data Warehouse è un componente chiave di una soluzione per Big Data end-to-end nel cloud.

![Soluzione di data warehouse](media/sql-data-warehouse-overview-what-is/data-warehouse-solution.png) 

In una soluzione cloud per i dati, i dati vengono inseriti in archivi di Big Data da numerose origini. Una volta inseriti i dati in un archivio di Big Data, Hadoop, Spark e gli algoritmi di apprendimento automatico preparano i dati e ne eseguono il training. Quando i dati sono pronti per le analisi complesse, SQL Data Warehouse usa PolyBase per eseguire query sugli archivi di Big Data. PolyBase usa query T-SQL standard per inserire i dati in SQL Data Warehouse.
 
SQL Data Warehouse archivia i dati in tabelle relazionali con archiviazione a colonne. Questo formato consente di ridurre notevolmente i costi di archiviazione dei dati e di migliorare le prestazioni di query. Una volta archiviati i dati in SQL Data Warehouse, è possibile eseguire analisi su larga scala. Rispetto ai sistemi di database tradizionali, le query di analisi vengono completate in secondi invece che in minuti oppure in ore invece che in giorni. 

I risultati delle analisi possono essere passati ad applicazioni o database di report in tutto il mondo. Gli analisti aziendali possono quindi ottenere informazioni dettagliate per prendere decisioni ben informate.

## <a name="next-steps"></a>Passaggi successivi

- Esplorare l'[architettura di Azure SQL Data Warehouse](/azure/sql-data-warehouse/massively-parallel-processing-mpp-architecture)
- [Creare un SQL Data Warehouse][create a SQL Data Warehouse] rapidamente
- [Caricare dati di esempio][load sample data]
- Esplorare i [video](/azure/sql-data-warehouse/sql-data-warehouse-videos)

Oppure vedere alcune delle altre risorse disponibili per SQL Data Warehouse.  
* Eseguire ricerche nei [blog]
* Inviare [richieste di funzionalità]
* Eseguire ricerche nei [Blog Customer Advisory Team]
* [Creare un ticket di supporto]
* Eseguire ricerche nei [forum MSDN]
* Eseguire ricerche nel [Forum su Stack Overflow]


<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Creare un ticket di supporto]: ./sql-data-warehouse-get-started-create-support-ticket.md
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md
[Migration documentation]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[Integrated tools overview]: ./sql-data-warehouse-overview-integrate.md
[Backup and restore overview]: ./sql-data-warehouse-restore-database-overview.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Blog]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Blog Customer Advisory Team]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Richieste di funzionalità]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Forum MSDN]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Forum su Stack Overflow]: https://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA for SQL Data Warehouse]: https://azure.microsoft.com/support/legal/sla/sql-data-warehouse/v1_0/
[Volume Licensing]: https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Service Level Agreements]: https://azure.microsoft.com/support/legal/sla/
