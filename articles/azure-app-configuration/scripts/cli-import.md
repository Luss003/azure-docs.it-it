---
title: Esempio di script dell'interfaccia della riga di comando di Azure - Eseguire l'importazione in un archivio di Configurazione app
titleSuffix: Azure App Configuration
description: Questo articolo fornisce informazioni e script di esempio per l'importazione in un archivio di Configurazione app di Azure
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: azure-app-configuration
ms.date: 02/24/2019
ms.author: lcozzens
ms.custom: mvc
ms.openlocfilehash: bb661e6d4497a85cf2ef445fc39774e93a04cc99
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/31/2020
ms.locfileid: "76899411"
---
# <a name="import-to-an-azure-app-configuration-store"></a>Importare in un archivio di Configurazione app di Azure

Questo script di esempio importa le impostazioni di coppie chiave-valore in un archivio di Configurazione app di Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se si sceglie di installare e usare l'interfaccia della riga di comando di Azure in locale, per questo articolo è necessario eseguire la versione 2.0 o successiva dell'interfaccia della riga di comando di Azure. Eseguire `az --version` per trovare la versione. Per eseguire l'installazione o l'aggiornamento, vedere [Installare l'interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli).

È prima necessario installare l'estensione dell'interfaccia della riga di comando di Configurazione app di Azure eseguendo il comando seguente:

        az extension add -n appconfig

## <a name="sample-script"></a>Script di esempio

```azurecli-interactive
#!/bin/bash

# Import key-values from a file
az appconfig kv import --name myTestAppConfigStore --source file --path ~/Import.json
```

[!INCLUDE [cli-script-cleanup](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Spiegazione dello script

Questo script usa i comandi seguenti per importare un archivio di Configurazione app. Ogni comando della tabella include collegamenti alla documentazione specifica del comando.

| Comando | Note |
|---|---|
| [az appconfig kv import](/cli/azure/ext/appconfig/appconfig/kv#ext-appconfig-az-appconfig-kv-import) | Importa una risorsa archivio di Configurazione app. |

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sull'interfaccia della riga di comando di Azure, vedere la [documentazione sull'interfaccia della riga di comando di Azure](/cli/azure).

Altri esempi di script sono disponibili negli [esempi dell'interfaccia della riga di comando di Configurazione app di Azure](../cli-samples.md).
