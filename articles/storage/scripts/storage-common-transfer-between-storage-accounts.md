---
title: Eseguire la migrazione di BLOB tra account di archiviazione tramite AzCopy in Windows
description: Esempio di script di Azure PowerShell - Uso di AzCopy per copiare il contenuto dei BLOB da un account di archiviazione di Azure a un altro.
services: storage
documentationcenter: na
author: normesta
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 02/01/2018
ms.author: normesta
ms.openlocfilehash: 559b8b2875b789034ae07901f668f241505073b1
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75465055"
---
# <a name="migrate-blobs-across-storage-accounts-using-azcopy-on-windows"></a>Eseguire la migrazione di oggetti BLOB tra account di archiviazione tramite AzCopy in Windows

In questo esempio vengono copiati tutti gli oggetti BLOB da un account di archiviazione di origine fornito dall'utente a un account di archiviazione di destinazione fornito dall'utente. 

Questa operazione viene eseguita mediante il comando `Get-AzStorageContainer`, che elenca tutti i contenitori di un account di archiviazione. L'esempio genera quindi i comandi AzCopy, che eseguono la copia di ogni contenitore dall'account di archiviazione di origine all'account di archiviazione di destinazione. Se si verificano problemi, l'esempio esegue il numero di tentativi definito dal parametro $retryTimes (l'impostazione predefinita è 3 ma il valore può essere modificato mediante il parametro `-RetryTimes`). Se l'errore si verifica a ogni nuovo tentativo, l'utente può rieseguire lo script fornendo il codice di esempio con l'ultimo contenitore copiato correttamente tramite il parametro `-LastSuccessContainerName`. L'esempio continua quindi la copia dei contenitori da tale punto.

Per questo esempio è necessario il modulo di archiviazione di Azure PowerShell **0.7** o versioni successive. È possibile controllare la versione installata tramite `Get-Module -ListAvailable Az.storage`. Se è necessario eseguire l'installazione o l'aggiornamento, vedere come [installare il modulo Azure PowerShell](/powershell/azure/install-Az-ps). 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Questo esempio richiede inoltre la versione più recente di[AzCopy in Windows](https://aka.ms/downloadazcopy). La directory di installazione predefinita è `C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\`.

Questo esempio usa un nome e una chiave dell'account di archiviazione di origine, un nome e una chiave dell'account di archiviazione di destinazione e il percorso intero di AzCopy.exe (se non è installato nella directory predefinita).

Di seguito sono riportati alcuni esempi di input per questo esempio:

Se AzCopy è installato nella directory predefinita:
```powershell
srcStorageAccountName: ExampleSourceStorageAccountName
srcStorageAccountKey: ExampleSourceStorageAccountKey
DestStorageAccountName: ExampleTargetStorageAccountName
DestStorageAccountKey: ExampleTargetStorageAccountKey
```

Se AzCopy non è installato nella directory predefinita:

```Powershell
srcStorageAccountName: ExampleSourceStorageAccountName
srcStorageAccountKey: ExampleSourceStorageAccountKey
DestStorageAccountName: ExampleTargetStorageAccountName
DestStorageAccountKey: ExampleTargetStorageAccountKey
AzCopyPath: C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy.exe
```

Se si verifica un errore e se è necessario rieseguire l'esempio da un contenitore specifico: 

`.\copyScript.ps1 -LastSuccessContainerName myContainerName`

## <a name="sample-script"></a>Script di esempio

[!code-powershell[main](../../../powershell_scripts/storage/migrate-blobs-between-accounts/migrate-blobs-between-accounts.ps1 "Migrate blobs between storage accounts.")]

## <a name="script-explanation"></a>Spiegazione dello script

Questo script usa i comandi seguenti per copiare i dati da un account di archiviazione a un altro. Ogni elemento della tabella include collegamenti alla documentazione specifica del comando.

| Comando | Note |
|---|---|
| [Get-AzStorageContainer](/powershell/module/az.storage/Get-AzStorageContainer) | Restituisce i contenitori associati all'account di archiviazione corrente. |
| [New-AzStorageContext](/powershell/module/az.storage/New-AzStorageContext) | Crea un contesto di Archiviazione di Azure. |

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sul modulo Azure PowerShell, vedere la [documentazione di Azure PowerShell](/powershell/azure/overview).

Sono disponibili altri esempi di script di archiviazione di PowerShell in [PowerShell samples for Azure Blob storage](../blobs/storage-samples-blobs-powershell.md) (Esempi di PowerShell per l'Archiviazione BLOB di Azure).
