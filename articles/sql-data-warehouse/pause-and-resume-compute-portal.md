---
title: 'Guida introduttiva: Sospendere e riprendere il calcolo - Portale di Azure '
description: Usare il portale di Azure per sospendere il calcolo in Azure SQL Data Warehouse per ridurre i costi. Riprendere il calcolo quando si è pronti a usare il data warehouse.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: manage
ms.date: 04/18/2018
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 14f66f71948f75a723c9fdbed7490d54c2c3e2b2
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73692992"
---
# <a name="quickstart-pause-and-resume-compute-for-an-azure-sql-data-warehouse-in-the-azure-portal"></a>Guida introduttiva: Sospendere e riprendere il calcolo per un'istanza di Azure SQL Data Warehouse nel portale di Azure

Usare il portale di Azure per sospendere il calcolo in Azure SQL Data Warehouse per ridurre i costi. [Riprendere il calcolo](sql-data-warehouse-manage-compute-overview.md) quando si è pronti a usare il data warehouse.

Se non si ha una sottoscrizione di Azure, creare un account [gratuito](https://azure.microsoft.com/free/) prima di iniziare.

## <a name="sign-in-to-the-azure-portal"></a>Accedere al portale di Azure

Accedere al [portale di Azure](https://portal.azure.com/).

## <a name="before-you-begin"></a>Prima di iniziare

Usare [Creare e connettere - portale](create-data-warehouse-portal.md) per creare un data warehouse denominato **mySampleDataWarehouse**. 

## <a name="pause-compute"></a>Sospendere le risorse di calcolo

Per ridurre i costi, è possibile sospendere e riprendere le risorse di calcolo su richiesta. Ad esempio, se non si usa il database durante la notte e nei fine settimana, è possibile sospenderlo in questi intervalli di tempo e riprenderne l'esecuzione durante il giorno. Mentre il database è sospeso, non verranno addebitati costi per le risorse di calcolo. Continueranno tuttavia a essere applicati addebiti per l'archiviazione. 

Per sospendere un'istanza di SQL Data Warehouse, seguire questa procedura.

1. Nella pagina di sinistra del portale di Azure fare clic su **Database SQL**.
2. Selezionare **mySampleDataWarehouse** nella pagina **Database SQL**. Verrà aperto il data warehouse. 
3. Nella pagina **mySampleDataWarehouse** verificare che **Stato** sia **Online**.

    ![Calcolo delle risorse online](media/pause-and-resume-compute-portal/compute-online.png)

4. Per sospendere il data warehouse, fare clic sul pulsante **Pausa**. 
5. Viene visualizzato un messaggio in cui viene chiesto se si vuole continuare. Fare clic su **Sì**.
6. Attendere qualche minuto e quindi verificare che lo **Stato** sia **Sospensione**.

    ![Sospensione](media/pause-and-resume-compute-portal/pausing.png)

7. Al termine dell'operazione di sospensione, lo stato diventa **Sospeso** e il pulsante di opzione è **Avvia**.
8. Le risorse di calcolo per il data warehouse sono offline. Non verranno addebitate le risorse di calcolo fino al ripristino del servizio.

    ![Calcolo delle risorse offline](media/pause-and-resume-compute-portal/compute-offline.png)


## <a name="resume-compute"></a>Riavviare le risorse di calcolo

Per riprendere un'istanza di SQL Data Warehouse, seguire questa procedura.

1. Nella pagina di sinistra del portale di Azure fare clic su **Database SQL**.
2. Selezionare **mySampleDataWarehouse** nella pagina **Database SQL**. Verrà aperto il data warehouse. 
3. Nella pagina **mySampleDataWarehouse** verificare che **Stato** sia **Sospeso**.

    ![Calcolo delle risorse offline](media/pause-and-resume-compute-portal/compute-offline.png)

4. Per riprendere il data warehouse, fare clic sul pulsante **Avvia**. 
5. Viene visualizzato un messaggio in cui viene chiesto se si vuole avviare. Fare clic su **Sì**.
6. Si noti che lo **Stato** è **Ripresa**.

    ![Resuming](media/pause-and-resume-compute-portal/resuming.png)

7. Quando il data warehouse è di nuovo online, lo stato diventa **Online** e il pulsante di opzione è **Pausa**.
8. Le risorse di calcolo per il data warehouse sono online ed è possibile usare il servizio. Verranno ripresi gli addebiti per il calcolo.

    ![Calcolo delle risorse online](media/pause-and-resume-compute-portal/compute-online.png)

## <a name="clean-up-resources"></a>Pulire le risorse

Verranno addebitati le unità del data warehouse e i dati archiviati nel data warehouse. Le risorse di calcolo e archiviazione vengono fatturate separatamente. 

- Se si vogliono mantenere i dati nelle risorse di archiviazione, sospendere il calcolo.
- Per evitare di ricevere addebiti in futuro, è possibile eliminare il data warehouse. 

Seguire questa procedura per pulire le risorse nel modo desiderato.

1. Accedere al [portale di Azure](https://portal.azure.com) e fare clic sul data warehouse.

    ![Pulire le risorse](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

1. Per sospendere il calcolo, fare clic sul pulsante **Pausa**. Quando si sospende il data warehouse, viene visualizzato il pulsante **Avvia**.  Per riprendere il calcolo, fare clic su **Avvia**.

2. Per rimuovere il data warehouse in modo da non ricevere addebiti per operazioni di calcolo o archiviazione, fare clic su **Elimina**.

3. Per rimuovere il server SQL creato, fare clic su **mynewserver-20171113.database.windows.net** e quindi fare clic su **Elimina**.  Fare attenzione quando si esegue questa operazione perché l'eliminazione del server comporta anche quella di tutti i database assegnati al server.

4. Per rimuovere il gruppo di risorse, fare clic su **myResourceGroup** e quindi su **Elimina gruppo di risorse**.


## <a name="next-steps"></a>Passaggi successivi

È stato sospeso e ripreso il calcolo per il data warehouse. Per altre informazioni su Azure SQL Data Warehouse, continuare con l'esercitazione per il caricamento dei dati.

> [!div class="nextstepaction"]
> [Caricare i dati in SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md)
