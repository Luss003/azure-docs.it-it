---
title: Evento di avvio attività di Azure Batch | Microsoft Docs
description: Riferimento per l'evento di avvio dell'attività batch.
services: batch
author: laurenhughes
manager: gwallace
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: lahugh
ms.openlocfilehash: ffad1696bc2c85a1a150ac87d90c2fb9c34e1519
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/04/2019
ms.locfileid: "70258534"
---
# <a name="task-start-event"></a>Evento di avvio attività

 Questo evento viene generato una volta che un'attività è stata pianificata per l'avvio su un nodo di calcolo dall'utilità di pianificazione. Se l'attività viene ritentata o reinserita nella coda, questo evento verrà generato nuovamente per la stessa attività, ma il numero di tentativi e la versione dell'attività di sistema verranno aggiornati di conseguenza.


 L'esempio seguente illustra il corpo di un evento di avvio attività.

```
{
    "jobId": "myJob",
    "id": "myTask",
    "taskType": "User",
    "systemTaskVersion": 220192842,
    "nodeInfo": {
        "poolId": "pool-001",
        "nodeId": "tvm-257509324_1-20160908t162728z"
    },
    "multiInstanceSettings": {
        "numberOfInstances": 1
    },
    "constraints": {
        "maxTaskRetryCount": 2
    },
    "executionInfo": {
        "retryCount": 0
    }
}
```

|Nome dell'elemento|Type|Note|
|------------------|----------|-----------|
|`jobId`|String|ID del processo che contiene l'attività.|
|`id`|String|ID dell'attività.|
|`taskType`|String|Tipo dell'attività. Il valore può essere "JobManager" per indicare che si tratta di un'attività del gestore di processi oppure 'User' per indicare che non si tratta di un'attività del gestore di processi.|
|`systemTaskVersion`|Int32|Contatore dei tentativi interni di esecuzione di un'attività. Il servizio Batch può ritentare internamente l'esecuzione di un'attività in funzione di problemi transitori. Questi problemi possono includere errori interni di pianificazione o tentativi di ripristino a seguito di nodi di calcolo in uno stato non valido.|
|[`nodeInfo`](#nodeInfo)|Tipo complesso|Contiene informazioni sul nodo di calcolo in cui è stata eseguita l'attività.|
|[`multiInstanceSettings`](#multiInstanceSettings)|Tipo complesso|Specifica che l'attività è un'attività con istanze multiple che richiede più nodi di calcolo.  Per informazioni dettagliate, vedere [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task).|
|[`constraints`](#constraints)|Tipo complesso|Vincoli di esecuzione che si applicano a questa attività.|
|[`executionInfo`](#executionInfo)|Tipo complesso|Contiene informazioni sull'esecuzione dell'attività.|

###  <a name="nodeInfo"></a> nodeInfo

|Nome dell'elemento|Type|Note|
|------------------|----------|-----------|
|`poolId`|String|ID del pool in cui è stata eseguita l'attività.|
|`nodeId`|String|ID del nodo in cui è stata eseguita l'attività.|

###  <a name="multiInstanceSettings"></a> multiInstanceSettings

|Nome dell'elemento|Type|Note|
|------------------|----------|-----------|
|`numberOfInstances`|Int|Numero di nodi di calcolo richiesti dall'attività.|

###  <a name="constraints"></a> constraints

|Nome dell'elemento|Type|Note|
|------------------|----------|-----------|
|`maxTaskRetryCount`|Int32|Numero massimo di tentativi consentiti per l'attività. Il servizio Batch ripete un'attività se il relativo codice di uscita è diverso da zero.<br /><br /> Si noti che questo valore controlla specificamente il numero di tentativi. Il servizio Batch eseguirà l'attività una volta e quindi ripeterà l'esecuzione fino al limite di tentativi specificato. Ad esempio, se il numero massimo di tentativi è 3, il servizio Batch eseguirà l'attività 4 volte, ovvero una iniziale e 3 ulteriori tentativi.<br /><br /> Se il numero massimo di tentativi è 0, il servizio Batch non eseguirà ulteriori tentativi.<br /><br /> Se il numero massimo di tentativi è -1, il servizio Batch continuerà a eseguire tentativi senza limiti.<br /><br /> Il valore predefinito è 0, ovvero nessun tentativo.|

###  <a name="executionInfo"></a> executionInfo

|Nome dell'elemento|Type|Note|
|------------------|----------|-----------|
|`retryCount`|Int32|Numero di tentativi di esecuzione dell'attività da parte del servizio Batch. L'attività viene ritentata se si conclude con un codice di uscita diverso da zero, fino al limite specificato in MaxTaskRetryCount.|
