---
title: Esempio di script dell'interfaccia della riga di comando di Azure - Ruotare le chiavi di accesso dell'account di archiviazione | Microsoft Docs
description: Creare un account di Archiviazione di Azure, quindi recuperare e ruotare le relative chiavi di accesso.
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: ''
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 06/22/2017
ms.author: tamram
ms.openlocfilehash: ac58886225221677aa003833167ff58cd578255d
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/04/2019
ms.locfileid: "55693922"
---
# <a name="create-a-storage-account-and-rotate-its-account-access-keys"></a>Creare un account di archiviazione e ruotare le relative chiavi di accesso

Lo script crea un account di Archiviazione di Azure, consente di visualizzare le chiavi di accesso del nuovo account di archiviazione e rinnova, ovvero ruota, le chiavi.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script di esempio

[!code-azurecli-interactive[main](../../../cli_scripts/storage/rotate-storage-account-keys/rotate-storage-account-keys.sh "Rotate storage account keys")]

## <a name="clean-up-deployment"></a>Pulire la distribuzione 

Eseguire il comando seguente per rimuovere il gruppo di risorse, l'account di archiviazione e tutte le risorse correlate.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Spiegazione dello script

Lo script usa i comandi seguenti per creare l'account di archiviazione e recuperare e ruotare le relative chiavi di accesso. Ogni elemento della tabella include collegamenti alla documentazione specifica del comando.

| Comando | Note |
|---|---|
| [az group create](/cli/azure/group) | Consente di creare un gruppo di risorse in cui sono archiviate tutte le risorse. |
| [az storage account create](/cli/azure/storage/account) | Crea un account di Archiviazione di Azure nel gruppo di risorse specificato. |
| [az storage account keys list](/cli/azure/storage/account/keys) | Mostra le chiavi di accesso per l'account di archiviazione specificato. |
| [az storage account keys renew](/cli/azure/storage/account/keys) | Rigenera la chiave di accesso di account dell'account di archiviazione primario o secondario. |

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sull'interfaccia della riga di comando di Azure, vedere la [documentazione sull'interfaccia della riga di comando di Azure](/cli/azure).

Altri esempi di script dell'interfaccia della riga di comando sono disponibili in [Azure CLI samples for Azure Blob storage](../blobs/storage-samples-blobs-cli.md) (Esempi dell'interfaccia della riga di comando di Azure per l'Archiviazione BLOB di Azure).
