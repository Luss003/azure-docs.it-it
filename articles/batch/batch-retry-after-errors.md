---
title: Nuovo tentativo di attività dopo un errore
description: Verificare la presenza di errori e riprovare.
services: batch
documentationcenter: .net
author: LauraBrenner
manager: evansma
editor: ''
tags: azure-batch
ms.assetid: 16279b23-60ff-4b16-b308-5de000e4c028
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 02/15/2020
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: 94ed936e619461a2dbf7ec837c2d80e21c01c88e
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/28/2020
ms.locfileid: "77919992"
---
# <a name="detecting-and-handling-batch-service-errors"></a>Rilevamento e gestione degli errori del servizio batch

È importante ricordare di verificare la presenza di errori quando si lavora con un'API del servizio REST. Non è insolito che si verifichino errori durante l'esecuzione di processi batch.

## <a name="common-errors"></a>Errori comuni 

- Errori di rete: si tratta di richieste che non hanno mai raggiunto il batch o che la risposta batch non ha raggiunto il client nel tempo.
- Errori interni del server: risposta HTTP del codice di stato 5xx standard.
- La limitazione può causare errori come le risposte HTTP del codice di stato 429 o 503 con l'intestazione Retry-After.
- errori 4xx che includono errori quali esiste già e InvalidOperation. Ciò significa che la risorsa non è nello stato corretto per la transizione di stato.

Per informazioni dettagliate sui vari tipi di codici di errore e codici di errore specifici, vedere [codici di stato e](https://docs.microsoft.com/rest/api/batchservice/batch-status-and-error-codes)di errore di batch.

## <a name="when-to-retry"></a>Quando riprovare

Le API batch invieranno una notifica in caso di errore. Tutti possono essere ripetuti e tutti includono un gestore tentativi globale a tale scopo. È preferibile utilizzare questo meccanismo incorporato.

In seguito a un errore, prima di riprovare è necessario attendere alcuni secondi tra i tentativi. Se si riprova troppo spesso o troppo rapidamente, il gestore tentativi limiterà la limitazione.

### <a name="for-more-information"></a>Per ulteriori informazioni  

Le [API e gli strumenti di batch](batch-apis-tools.md) sono collegamenti a informazioni di riferimento sulle API. L'API .NET, ad esempio, ha una [classe RetryPolicyProvider]( https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.retrypolicyprovider?view=azure-dotnet) in cui devono essere specificati i criteri di ripetizione richiesti. 

Per informazioni dettagliate su ogni API e sui criteri di ripetizione dei tentativi predefiniti, vedere [codici di stato e di errore di batch](https://docs.microsoft.com/rest/api/batchservice/batch-status-and-error-codes).
