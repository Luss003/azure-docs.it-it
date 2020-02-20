---
title: Creare un database singolo
description: Informazioni su come creare un database singolo ed eseguire query nel database SQL di Azure usando il portale di Azure, PowerShell e l'interfaccia della riga di comando di Azure.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: sachinpMSFT
ms.author: ninarn
ms.reviewer: carlrab, sstein, vanto
ms.date: 02/14/2020
ms.openlocfilehash: 2dacdfaa5443707ab82ae53922ac439319375276
ms.sourcegitcommit: 79cbd20a86cd6f516acc3912d973aef7bf8c66e4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2020
ms.locfileid: "77252137"
---
# <a name="quickstart-create-a-single-database-in-azure-sql-database-using-the-azure-portal-powershell-and-azure-cli"></a>Avvio rapido: Creare un database singolo nel database SQL di Azure usando il portale di Azure, PowerShell e l'interfaccia della riga di comando di Azure

La creazione di un [database singolo](sql-database-single-database.md) è l'opzione di distribuzione più semplice e rapida per la creazione di database nel database SQL di Azure. Questa guida introduttiva mostra come creare un database singolo e quindi eseguire query usando il portale di Azure.

Se non si ha una sottoscrizione di Azure, [creare un account gratuito](https://azure.microsoft.com/free/). 

Per tutti i passaggi di questa guida introduttiva, accedere al [portale di Azure](https://portal.azure.com/).

## <a name="create-a-single-database"></a>Creare un database singolo

Un database singolo può essere creato nel livello di calcolo con provisioning o serverless.

- A un database singolo nel livello di elaborazione con provisioning viene preassegnata una quantità fissa di risorse di elaborazione, incluse CPU e memoria, usando uno dei due [modelli di acquisto](sql-database-purchase-models.md).
- Un database singolo nel livello di elaborazione serverless dispone di una gamma di risorse di elaborazione, comprendenti la CPU e la memoria che vengono ridimensionate automaticamente, ed è disponibile solo nei [modelli di acquisto basati su vCore](sql-database-service-tiers-vcore.md).

Quando si crea un database singolo, si definisce anche un [server di database SQL](sql-database-servers.md) per gestirlo e lo si inserisce all'interno di un [gruppo di risorse di Azure](../azure-resource-manager/management/overview.md) in un'area geografica specificata.

> [!NOTE]
> Questo argomento di avvio rapido usa il [modello di acquisto basato su vCore](sql-database-service-tiers-vcore.md), ma è disponibile anche il [modello di acquisto basato su DTU](sql-database-service-tiers-DTU.md).

Per creare un database singolo contenente i dati di esempio di AdventureWorksLT:

[!INCLUDE [sql-database-create-single-database](includes/sql-database-create-single-database.md)]

## <a name="query-the-database"></a>Eseguire query sul database

Dopo aver creato il database, usare lo strumento di query predefinito nel portale di Azure per connettersi ed eseguire query sui dati.

1. Nella pagina **Database SQL** del database selezionare **Editor di query (anteprima)** nel menu a sinistra.

   ![Accedere all'editor di query](./media/sql-database-get-started-portal/query-editor-login.png)

2. Immettere le informazioni di accesso e selezionare **OK**.
3. Immettere la query seguente nel riquadro **Editor di query**.

   ```sql
   SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

4. Selezionare **Esegui** e quindi esaminare i risultati della query nel riquadro **Risultati**.

   ![Risultati nell'editor di query](./media/sql-database-get-started-portal/query-editor-results.png)

5. Chiudere la pagina **Editor di query** e selezionare **OK** quando richiesto per rimuovere le modifiche non salvate.

## <a name="clean-up-resources"></a>Pulire le risorse

Se si vuole procedere con i [Passaggi successivi](#next-steps), conservare il gruppo di risorse, il server di database e il database singolo. I passaggi successivi illustrano come connettersi al database ed eseguire query con diversi metodi.

Al termine, sarà possibile eliminare queste risorse come segue:

1. Nel menu a sinistra nel portale di Azure selezionare **Gruppi di risorse** e quindi **myResourceGroup**.
2. Nella pagina del gruppo di risorse selezionare **Elimina gruppo di risorse**.
3. Immettere *myResourceGroup* nel campo e quindi selezionare **Elimina**.

## <a name="next-steps"></a>Passaggi successivi

- Creare una regola del firewall a livello di server per connettersi al database singolo da strumenti locali o remoti. Per altre informazioni, vedere [Creare una regola di firewall a livello di server](sql-database-server-level-firewall-rule.md).
- Dopo aver creato una regola del firewall a livello di server, [connettersi al database ed eseguire query](sql-database-connect-query.md) usando diversi strumenti e linguaggi.
  - [Connettersi ed eseguire query usando SQL Server Management Studio](sql-database-connect-query-ssms.md)
  - [Connettersi ed eseguire query usando Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- Per creare un database singolo nel livello di elaborazione con provisioning usando l'interfaccia della riga di comando di Azure, vedere [Esempi di interfaccia della riga di comando di Azure](sql-database-cli-samples.md).
- Per creare un database singolo nel livello di elaborazione con provisioning usando Azure PowerShell, vedere [Esempi di Azure PowerShell](sql-database-powershell-samples.md).
- Per creare un database singolo nel livello di elaborazione serverless con Azure PowerShell, vedere [Creare database serverless](sql-database-serverless.md#create-new-database-in-serverless-compute-tier).
