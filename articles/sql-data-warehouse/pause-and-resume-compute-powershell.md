---
title: 'Guida introduttiva: Sospendere e riprendere il calcolo in Azure SQL Data Warehouse - PowerShell | Microsoft Docs'
description: PowerShell consente di sospendere il calcolo in un'istanza di Azure SQL Data Warehouse per ridurre i costi. Riprendere il calcolo quando si è pronti a usare il data warehouse.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: manage
ms.date: 03/20/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: da9c3d42919bba6ce04fc54bafc2fb5d245379f5
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/04/2019
ms.locfileid: "70306098"
---
# <a name="quickstart-pause-and-resume-compute-in-azure-sql-data-warehouse-with-azure-powershell"></a>Guida introduttiva: Sospendere e riprendere il calcolo in Azure SQL Data Warehouse con Azure PowerShell

PowerShell consente di sospendere il calcolo in un'istanza di Azure SQL Data Warehouse per ridurre i costi. [Riprendere il calcolo](sql-data-warehouse-manage-compute-overview.md) quando si è pronti a usare il data warehouse.

Se non si ha una sottoscrizione di Azure, creare un account [gratuito](https://azure.microsoft.com/free/) prima di iniziare.

## <a name="before-you-begin"></a>Prima di iniziare

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Questa guida di avvio rapido presuppone che l'utente abbia già un'istanza di SQL Data Warehouse di cui è possibile sospendere e riprendere l'esecuzione. Se è necessario crearne uno, è possibile usare [Creare e connettere - portale](create-data-warehouse-portal.md) per creare un data warehouse denominato **mySampleDataWarehouse**.

## <a name="log-in-to-azure"></a>Accedere ad Azure

Accedere alla sottoscrizione di Azure con il comando [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) e seguire le indicazioni visualizzate.

```powershell
Connect-AzAccount
```

Per vedere quale sottoscrizione si sta usando, eseguire [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription).

```powershell
Get-AzSubscription
```

Se è necessario usare una sottoscrizione diversa da quella predefinita, eseguire [Set-AzContext](/powershell/module/az.accounts/set-azcontext).

```powershell
Set-AzContext -SubscriptionName "MySubscription"
```

## <a name="look-up-data-warehouse-information"></a>Cercare informazioni sul data warehouse

Individuare il nome del database, il nome del server e il gruppo di risorse del data warehouse di cui si prevede di sospendere e riprendere l'esecuzione.

Seguire questa procedura per trovare le informazioni sulla posizione del data warehouse.

1. Accedere al [portale di Azure](https://portal.azure.com/).
2. Nella pagina di sinistra del portale di Azure fare clic su **Database SQL**.
3. Selezionare **mySampleDataWarehouse** nella pagina **Database SQL**. Verrà aperto il data warehouse.

    ![Nome del server e gruppo di risorse](media/pause-and-resume-compute-powershell/locate-data-warehouse-information.png)

4. Annotare il nome del data warehouse, che corrisponde al nome del database. Annotare anche il nome del server e il gruppo di risorse.
6. Se il server è foo.database.windows.net, usare solo la prima parte come nome del server nei cmdlet PowerShell. Nella figura precedente il nome completo del server è newserver-20171113.database.windows.net. Eliminare il suffisso e usare **newserver-20171113** come nome del server nel cmdlet PowerShell.

## <a name="pause-compute"></a>Sospendere le risorse di calcolo

Per ridurre i costi, è possibile sospendere e riprendere le risorse di calcolo su richiesta. Ad esempio, se non si usa il database durante la notte e nei fine settimana, è possibile sospenderlo in questi intervalli di tempo e riprenderne l'esecuzione durante il giorno. Mentre il database è sospeso non vengono addebitati costi per le risorse di calcolo. Continuano tuttavia a essere applicati addebiti per l'archiviazione.

Per sospendere l'esecuzione di un database, usare il cmdlet [Suspend-AzSqlDatabase](/powershell/module/az.sql/suspend-azsqldatabase). Nell'esempio seguente viene sospeso un data warehouse denominato **mySampleDataWarehouse** ospitato in un server denominato **newserver 20171113**. Il server appartiene a un gruppo di risorse di Azure denominato **myResourceGroup**.


```Powershell
Suspend-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" –DatabaseName "mySampleDataWarehouse"
```

Come variazione, il database dell'esempio seguente viene recuperato nell'oggetto $database. L'oggetto viene quindi inviato tramite pipe a [Suspend-AzSqlDatabase](/powershell/module/az.sql/suspend-azsqldatabase). I risultati vengono archiviati nell'oggetto resultDatabase. Il comando finale mostra i risultati.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" –DatabaseName "mySampleDataWarehouse"
$resultDatabase = $database | Suspend-AzSqlDatabase
$resultDatabase
```


## <a name="resume-compute"></a>Riavviare le risorse di calcolo

Per avviare un database, usare il cmdlet [Resume-AzSqlDatabase](/powershell/module/az.sql/resume-azsqldatabase). Nell'esempio seguente viene avviato un database denominato mySampleDataWarehouse ospitato in un server denominato newserver 20171113. Il server appartiene a un gruppo di risorse di Azure denominato myResourceGroup.

```Powershell
Resume-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" -DatabaseName "mySampleDataWarehouse"
```

Come variazione, il database dell'esempio seguente viene recuperato nell'oggetto $database. Quindi l'oggetto viene inviato tramite pipe a [Resume-AzSqlDatabase](/powershell/module/az.sql/resume-azsqldatabase) e i risultati vengono archiviati in $resultDatabase. Il comando finale mostra i risultati.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzSqlDatabase
$resultDatabase
```

## <a name="check-status-of-your-data-warehouse-operation"></a>Controllare lo stato dell'operazione di data warehouse

Per controllare lo stato del data warehouse, usare il cmdlet [Get-AzSqlDatabaseActivity](https://docs.microsoft.com/powershell/module/az.sql/Get-AzSqlDatabaseActivity#description).

```
Get-AzSqlDatabaseActivity -ResourceGroupName "ResourceGroup01" -ServerName "Server01" -DatabaseName "Database02"
```

## <a name="clean-up-resources"></a>Pulire le risorse

Per le unità del data warehouse e i dati archiviati vengono addebitati costi. Le risorse di calcolo e archiviazione vengono fatturate separatamente.

- Se si vogliono mantenere i dati nelle risorse di archiviazione, sospendere il calcolo.
- Per evitare di ricevere addebiti in futuro, è possibile eliminare il data warehouse.

Seguire questa procedura per pulire le risorse nel modo desiderato.

1. Accedere al [portale di Azure](https://portal.azure.com) e fare clic sul data warehouse.

    ![Pulire le risorse](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

2. Per sospendere il calcolo, fare clic sul pulsante **Pausa**. Quando si sospende il data warehouse, viene visualizzato il pulsante **Avvia**.  Per riprendere il calcolo, fare clic su **Avvia**.

3. Per rimuovere il data warehouse in modo da non ricevere addebiti per operazioni di calcolo o archiviazione, fare clic su **Elimina**.

4. Per rimuovere il server SQL creato, fare clic su **mynewserver-20171113.database.windows.net** e quindi fare clic su **Elimina**.  Fare attenzione quando si esegue questa operazione perché l'eliminazione del server comporta anche quella di tutti i database assegnati al server.

5. Per rimuovere il gruppo di risorse, fare clic su **myResourceGroup** e quindi su **Elimina gruppo di risorse**.


## <a name="next-steps"></a>Passaggi successivi

È stato sospeso e ripreso il calcolo per il data warehouse. Per altre informazioni su Azure SQL Data Warehouse, continuare con l'esercitazione per il caricamento dei dati.

> [!div class="nextstepaction"]
> [Caricare i dati in SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md)
