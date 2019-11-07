---
title: Connettersi con SSMS
description: Usare SQL Server Management Studio (SSMS) per connettersi ed eseguire query in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: d5c903a24ea47cb152555330688dd0bc515c625b
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73692589"
---
# <a name="connect-to-sql-data-warehouse-with-sql-server-management-studio-ssms"></a>Connettersi a SQL Data Warehouse con SQL Server Management Studio (SSMS)
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Usare SQL Server Management Studio (SSMS) per connettersi ed eseguire query in Azure SQL Data Warehouse. 

## <a name="prerequisites"></a>Prerequisiti
Per eseguire questa esercitazione, è necessario:

* SQL Data Warehouse esistente. Per crearne una, vedere [Creare un Azure SQL Data Warehouse][Create a SQL Data Warehouse].
* SQL Server Management Studio (SSMS) installato. [Installare SSMS][Install SSMS] gratuitamente se non è già disponibile.
* Il nome completo dell'istanza di SQL Server. Per trovarlo, vedere [Connect to Azure SQL Data Warehouse][Connect to SQL Data Warehouse](Connettersi ad Azure SQL Data Warehouse).

## <a name="1-connect-to-your-sql-data-warehouse"></a>1. connettersi alla SQL Data Warehouse
1. Aprire SQL Server Management Studio.
2. Aprire Esplora oggetti. A questo scopo, selezionare **File** > **Connetti Esplora oggetti**.
   
    ![Esplora oggetti di SQL Server][1]
3. Compilare i campi nella finestra Connetti al server.
   
    ![Connetti al server][2]
   
   * **Nome server**. Immettere il **nome server** identificato in precedenza.
   * **Autenticazione**. Selezionare **Autenticazione di SQL Server** o **Autenticazione integrata di Active Directory**.
   * **Nome utente** e **Password**. Se è stata selezionata l'autenticazione di SQL Server, immettere il nome utente e la password.
   * Fare clic su **Connetti**.
4. Per l'esplorazione, espandere il server SQL Azure. È possibile visualizzare i database associati al server. Espandere AdventureWorksDW per visualizzare le tabelle nel database di esempio.
   
    ![Esplorare AdventureWorksDW][3]

## <a name="2-run-a-sample-query"></a>2. eseguire una query di esempio
Ora che è stata stabilita una connessione al database, è possibile scrivere una query.

1. Fare clic con il pulsante destro del mouse sul database in Esplora oggetti di SQL Server.
2. Scegliere **Nuova query**. Verrà visualizzata una nuova finestra di query.
   
    ![Nuova Query][4]
3. Copiare la query TSQL seguente nella finestra di query:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Eseguire la query. A questo scopo, fare clic su `Execute` oppure usare la combinazione di tasti seguente: `F5`.
   
    ![Esegui query][5]
5. Osservare i risultati della query. In questo esempio la tabella FactInternetSales include 60398 righe.
   
    ![Risultati query][6]

## <a name="next-steps"></a>Passaggi successivi
Ora che è possibile connettersi ed eseguire una query, provare a [visualizzare i dati con PowerBI][visualizing the data with PowerBI].

Per configurare l'ambiente per l'autenticazione di Azure Active Directory, vedere [Authentication to Azure SQL Data Warehouse][Authenticate to SQL Data Warehouse](Autenticazione in Azure SQL Data Warehouse).

<!--Arcticles-->
[Connect to SQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Authenticate to SQL Data Warehouse]: sql-data-warehouse-authentication.md
[visualizing the data with PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md 

<!--Other-->
[Azure portal]: https://portal.azure.com
[Install SSMS]: https://msdn.microsoft.com/library/hh213248.aspx


<!--Image references-->

[1]: media/sql-data-warehouse-query-ssms/connect-object-explorer.png
[2]: media/sql-data-warehouse-query-ssms/connect-object-explorer1.png
[3]: media/sql-data-warehouse-query-ssms/explore-tables.png
[4]: media/sql-data-warehouse-query-ssms/new-query.png
[5]: media/sql-data-warehouse-query-ssms/execute-query.png
[6]: media/sql-data-warehouse-query-ssms/results.png
