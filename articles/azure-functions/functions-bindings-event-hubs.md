---
title: Associazioni di Hub eventi di Azure per Funzioni di Azure
description: Informazioni su come usare le associazioni di Hub eventi di Azure in Funzioni di Azure.
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
keywords: Funzioni di Azure, Funzioni, elaborazione eventi, calcolo dinamico, architettura senza server
ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.service: azure-functions
ms.topic: reference
ms.date: 11/08/2017
ms.author: cshoe
ms.openlocfilehash: bc75ad08716a001ae0cfd934dbc8d5da668dc1c3
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2019
ms.locfileid: "70086663"
---
# <a name="azure-event-hubs-bindings-for-azure-functions"></a>Associazioni di Hub eventi di Azure per Funzioni di Azure

Questo articolo illustra come usare le associazioni di [Hub eventi di Azure](../event-hubs/event-hubs-what-is-event-hubs.md) in Funzioni di Azure. Funzioni di Azure supporta il trigger e le associazioni di output per Hub eventi.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Pacchetti: Funzioni 1.x

Per Funzioni di Azure versione 1.x, le associazioni di Hub eventi sono incluse nel pacchetto NuGet [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus) versione 2.x.
Il codice sorgente del pacchetto si trova nel repository GitHub [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs).


[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Pacchetti: Funzioni 2.x

Per Funzioni 2.x, usare il pacchetto [Microsoft.Azure.WebJobs.Extensions.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs) versione 3.x.
Il codice sorgente del pacchetto si trova nel repository GitHub [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/master/src/Microsoft.Azure.WebJobs.Extensions.EventHubs).

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

[!INCLUDE [functions-bindings-event-hubs](../../includes/functions-bindings-event-hubs.md)]

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Altre informazioni su trigger e associazioni di Funzioni di Azure](functions-triggers-bindings.md)
