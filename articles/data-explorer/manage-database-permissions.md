---
title: Gestire le autorizzazioni del database in Azure Esplora dati
description: Questo articolo descrive i controlli degli accessi in base al ruolo per i database e le tabelle in Esplora dati di Azure.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: b4d5e56e990c0353f44209c6b19ae2d1727de27a
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/15/2020
ms.locfileid: "76030099"
---
# <a name="manage-azure-data-explorer-database-permissions"></a>Gestire le autorizzazioni per database di Esplora dati di Azure

Esplora dati di Azure consente di controllare l'accesso ai database e alle tabelle, usando il modello *controllo degli accessi in base al ruolo*. In questo modello viene eseguito il mapping ai *ruoli* delle*entità di sicurezza* (utenti, gruppi e app). Le entità di sicurezza possono accedere alle risorse in base ai ruoli che vengono loro assegnati.

Questo articolo descrive i ruoli disponibili e come assegnare le entità di sicurezza a tali ruoli usando i comandi di gestione del portale di Azure e di Esplora dati di Azure.

## <a name="roles-and-permissions"></a>Ruoli e autorizzazioni

Esplora dati di Azure presenta i seguenti ruoli:

|Ruolo                       |Autorizzazioni                                                                        |
|---------------------------|-----------------------------------------------------------------------------------|
|Amministratore database             |Può eseguire qualsiasi operazione nell'ambito di un determinato database.|
|Utente del database              |Può leggere tutti i dati e metadati nel database. Inoltre, può creare tabelle (diventando l'amministratore di tabella per tale tabella) e funzioni nel database.|
|Visualizzatore database            |Può leggere tutti i dati e metadati nel database.|
|Inseritore database          |Può inserire dati per tutte le tabelle esistenti nel database, ma non una query sui dati.|
|Monitoraggio del database           |Può eseguire comandi ".show..." nel contesto del database e le relative entità figlio.|
|Amministratore tabella                |Può eseguire qualsiasi operazione nell'ambito di una determinata tabella. |
|Inseritore tabella             |Può inserire dati nell'ambito di una determinata tabella, ma non una query sui dati.|

## <a name="manage-permissions-in-the-azure-portal"></a>Gestire le autorizzazioni nel portale di Azure

1. Accedere al [portale di Azure](https://portal.azure.com/).

1. Passare al cluster Esplora dati di Azure.

1. Nella sezione **Panoramica**, selezionare il database in cui si desidera gestire le autorizzazioni.

    ![Seleziona database](media/manage-database-permissions/select-database.png)

1. Selezionare **Autorizzazioni** quindi**Aggiungi**.

    ![Autorizzazioni per il database](media/manage-database-permissions/database-permissions.png)

1. In **Aggiungere autorizzazioni per il database**, selezionare il ruolo che si desidera assegnare all'entità di sicurezza, quindi **Selezionare entità di sicurezza**.

    ![Aggiungere autorizzazioni database](media/manage-database-permissions/add-permission.png)

1. Cercare l'entità di sicurezza, selezionarla, quindi fare clic su **Seleziona**.

    ![Gestire le autorizzazioni nel portale di Azure](media/manage-database-permissions/new-principals.png)

1. Selezionare **Salva**.

    ![Gestire le autorizzazioni nel portale di Azure](media/manage-database-permissions/save-permission.png)

## <a name="manage-permissions-with-management-commands"></a>Gestire le autorizzazioni con i comandi di gestione

1. Accedere a [https://dataexplorer.azure.com](https://dataexplorer.azure.com) e aggiungere il cluster, se non è già disponibile.

1. Nel riquadro sinistro, selezionare il database appropriato.

1. Usare il comando `.add` per assegnare le entità di sicurezza ai ruoli: `.add database databasename rolename ('aaduser | aadgroup=user@domain.com')`. Per aggiungere un utente al ruolo di Utente database, eseguire il comando seguente, sostituendo il nome del database e l'utente.

    ```Kusto
    .add database <TestDatabase> users ('aaduser=<user@contoso.com>')
    ```

    L'output del comando mostra l'elenco di utenti esistenti e i ruoli che vengono loro assegnati nel database.
    
    Per esempi relativi a Azure Active Directory e al modello di autorizzazione kusto, vedere [principi e provider di identità](https://docs.microsoft.com/azure/kusto/management/access-control/principals-and-identity-providers)

## <a name="next-steps"></a>Passaggi successivi

[Scrivere query](write-queries.md)
