---
title: Importare o esportare dati con configurazione delle App di Azure | Microsoft Docs
description: Informazioni su come importare o esportare dati da o verso una configurazione di App di Azure
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 377c5088d39821e87412c517540b3190b0a14a00
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "66393278"
---
# <a name="import-or-export-configuration-data"></a>Importare o esportare dati di configurazione

Configurazione delle App sono supportati dati Azure importare ed esportare le operazioni. Usare queste operazioni per lavorare con i dati di configurazione in blocco e scambiare dati tra l'archivio di configurazione di app e il codice progetto. Ad esempio, è possibile impostare backup di archivio di configurazione di un'app per i test e per la produzione. È quindi possibile copiare le impostazioni dell'applicazione tra loro tramite un file in modo che non è necessario immettere due volte i dati.

Questo articolo fornisce una Guida per l'importazione ed esportazione dei dati con configurazione delle App.

## <a name="import-data"></a>Importa dati

Importazione porta configurazione archivio dati in una configurazione di App da un'origine esistente, anziché immetterlo manualmente. Usare la funzione di importazione per eseguire la migrazione dei dati in un archivio di configurazione app o i dati aggregati provenienti da più origini. Configurazione delle App supporta l'importazione da un file JSON, YAML o proprietà.

Importare dati usando il [portale di Azure](https://portal.azure.com) o il [Azure CLI](./scripts/cli-import.md). Seguire questa procedura nel portale di Azure:

1. Passare all'archivio di configurazione di app e selezionare **importazione/esportazione**.

2. Nel **importazione** scheda, seleziona **servizio di origine** > **File di configurazione**.

3. Selezionare **per il linguaggio** > **tipo di File**.

4. Selezionare il **cartella** icona e selezionare il file da importare.

    ![File di importazione](./media/import-file.png)

5. Selezionare una **separatore**e, facoltativamente, immettere una **prefisso** da utilizzare per i nomi delle chiavi importati.

6. Facoltativamente, selezionare una **etichetta**.

7. Selezionare **applica** a completare l'importazione.

    ![File di importazione completata](./media/import-file-complete.png)

## <a name="export-data"></a>Esportazione dei dati

Esportazione scrive i dati di configurazione archiviati in configurazione dell'App in un'altra destinazione. Utilizzare la funzione di esportazione, ad esempio, per salvare i dati in un archivio di configurazione di app in un file che viene incorporato con il codice dell'applicazione durante la distribuzione.

Esportare i dati usando il [portale di Azure](https://portal.azure.com) o nella [CLI di Azure](./scripts/cli-export.md). Seguire questa procedura nel portale di Azure:

1. Passare all'archivio di configurazione di app e selezionare **importazione/esportazione**.

2. Nel **esportare** scheda, seleziona **servizio di destinazione** > **File di configurazione**.

3. Immettere facoltativamente una **Prefix** e selezionare un **etichetta** e un punto nel tempo per l'esportazione delle chiavi.

4. Selezionare una **tipo di File** > **separatore**.

5. Selezionare **applica** per completare l'esportazione.

    ![File di esportazione completata](./media/export-file-complete.png)

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Creare un'app Web ASP.NET Core](./quickstart-aspnet-core-app.md)  
