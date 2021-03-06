---
title: Ridimensionare Azure Cosmos DB in base a una pianificazione tramite il timer di funzioni di Azure
description: Informazioni su come ridimensionare le modifiche nella velocità effettiva in Azure Cosmos DB usando PowerShell e funzioni di Azure.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 01/13/2020
ms.author: mjbrown
ms.openlocfilehash: 68ba40ea212c061fa5c8bbddc47ea0dfc6d8caa4
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75935168"
---
# <a name="scale-azure-cosmos-db-throughput-by-using-azure-functions-timer-trigger"></a>Ridimensionare la velocità effettiva Azure Cosmos DB usando il trigger timer di funzioni di Azure

Le prestazioni di un account Azure Cosmos si basano sulla quantità di velocità effettiva con provisioning espressa in unità richiesta al secondo (UR/sec). Il provisioning è a una seconda granularità e viene fatturato in base alle UR/sec più alte all'ora. Questo modello di capacità con provisioning consente al servizio fornire una velocità effettiva prevedibile e coerente, con bassa latenza e disponibilità elevata garantite. La maggior parte dei carichi di lavoro di produzione queste funzionalità. Tuttavia, negli ambienti di sviluppo e test in cui Azure Cosmos DB viene usato solo durante le ore lavorative, è possibile aumentare la velocità effettiva al mattino e ridurla di nuovo nella sera dopo le ore lavorative.

È possibile impostare la velocità effettiva tramite [Azure Resource Manager modelli](resource-manager-samples.md), l'interfaccia della riga di comando di [Azure](cli-samples.md)e [PowerShell](powershell-samples-sql.md), per gli account API di base (SQL) o usando gli SDK Azure Cosmos DB specifici del linguaggio. Il vantaggio di usare modelli di Gestione risorse, l'interfaccia della riga di comando di Azure o PowerShell è che supportano tutte le API del modello di Azure Cosmos DB.

## <a name="throughput-scheduler-sample-project"></a>Progetto di esempio di utilità di pianificazione della velocità effettiva

Per semplificare il processo di scalabilità Azure Cosmos DB in base a una pianificazione, è stato creato un progetto di esempio denominato [utilità di pianificazione della velocità effettiva di Azure Cosmos](https://github.com/Azure-Samples/azure-cosmos-throughput-scheduler). Questo progetto è un'app funzioni di Azure con due trigger timer, ovvero "ScaleUpTrigger" e "ScaleDownTrigger". I trigger eseguono uno script di PowerShell che imposta la velocità effettiva per ogni risorsa come definito nel file di `resources.json` in ogni trigger. ScaleUpTrigger è configurato per l'esecuzione alle 8.00 UTC e il ScaleDownTrigger è configurato per l'esecuzione alle 18.00 UTC e tali orari possono essere facilmente aggiornati all'interno del file `function.json` per ogni trigger.

È possibile clonare questo progetto in locale, modificarlo per specificare le risorse di Azure Cosmos DB per la scalabilità verticale e orizzontale e la pianificazione per l'esecuzione. In seguito sarà possibile distribuirlo in una sottoscrizione di Azure e proteggerlo usando l'identità del servizio gestito con le autorizzazioni di [controllo degli accessi in base al ruolo](role-based-access-control.md) (RBAC) con il ruolo "operatore Azure Cosmos DB" per impostare la velocità effettiva negli account Azure Cosmos.

## <a name="next-steps"></a>Fasi successive

- Scopri di più e Scarica l'esempio da [Azure Cosmos DB utilità di pianificazione della velocità effettiva](https://github.com/Azure-Samples/azure-cosmos-throughput-scheduler).
