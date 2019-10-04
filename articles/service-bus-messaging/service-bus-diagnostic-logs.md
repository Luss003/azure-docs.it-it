---
title: Log di diagnostica del bus di servizio di Azure | Microsoft Docs
description: Informazioni su come configurare i log di diagnostica per il bus di servizio in Azure.
keywords: ''
documentationcenter: .net
services: service-bus-messaging
author: axisc
manager: timlt
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 6443cb727573645792a4e6c929b80c3406d72025
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/25/2019
ms.locfileid: "71261813"
---
# <a name="service-bus-diagnostic-logs"></a>Log di diagnostica del bus di servizio

È possibile visualizzare due tipi di log per il bus di servizio di Azure:
* **[Log attività](../azure-monitor/platform/activity-logs-overview.md)** . Questi log contengono informazioni sulle operazioni eseguite in un processo. I log sono sempre attivati.
* **[Log di diagnostica](../azure-monitor/platform/resource-logs-overview.md)** . È possibile configurare i log di diagnostica per informazioni più complete su tutto ciò che accade in un processo. I log di diagnostica coprono le attività che si verificano dal momento della creazione del processo fino alla sua eliminazione, inclusi gli aggiornamenti e le attività che si verificano durante l'esecuzione del processo.

## <a name="turn-on-diagnostic-logs"></a>Attivare i log di diagnostica

I log di diagnostica sono disabilitati per impostazione predefinita. Per abilitare i log di diagnostica, eseguire la procedura seguente:

1.  Nel [portale di Azure](https://portal.azure.com) in **Monitoraggio + Gestione** fare clic su **Log di diagnostica**.

    ![navigazione al pannello dei log di diagnostica](./media/service-bus-diagnostic-logs/image1.png)

2. Fare clic sulla risorsa da monitorare.  

3.  Fare clic su **Attiva diagnostica**.

    ![attivazione dei log di diagnostica](./media/service-bus-diagnostic-logs/image2.png)

4.  Per **Stato** fare clic su **Attivato**.

    ![modifica dello stato dei log di diagnostica](./media/service-bus-diagnostic-logs/image3.png)

5.  Impostare la destinazione dell'archivio desiderata; ad esempio, un account di archiviazione, un hub eventi o i log di monitoraggio di Azure.

6.  Salvare le nuove impostazioni di diagnostica.

Le nuove impostazioni diventano effettive in circa 10 minuti. Trascorso questo tempo, i log vengono visualizzati nella destinazione di archiviazione configurata, all'interno del pannello **Log di diagnostica**.

Per altre informazioni sulla configurazione della diagnostica, vedere la [panoramica dei log di diagnostica di Azure](../azure-monitor/platform/resource-logs-overview.md).

## <a name="diagnostic-logs-schema"></a>Schema dei log di diagnostica

Tutti i log vengono archiviati in formato JavaScript Object Notation (JSON). Ogni voce presenta campi stringa che usano il formato descritto nella sezione seguente.

## <a name="operational-logs-schema"></a>Schema di log operativi

I log nella categoria **OperationalLogs** acquisiscono i dati relativi al funzionamento del bus di servizio. In particolare, questi log acquisiscono il tipo di operazione, tra cui la creazione delle code, le risorse usate e lo stato dell'operazione.

Le stringhe JSON dei log operativi includono gli elementi elencati nella seguente tabella:

Attività | Descrizione
------- | -------
ActivityId | ID interno, usato a scopo di rilevamento
EventName | Nome operazione           
resourceId | ID della risorsa Azure Resource Manager
ID della sottoscrizione | ID sottoscrizione
EventTimeString | Durata dell'operazione
EventProperties | Proprietà dell'operazione
Stato | Stato dell'operazione
Caller | Chiamante dell'operazione (Portale di Azure o client di gestione)
category | OperationalLogs

Di seguito è riportato un esempio di stringa JSON di log operativo:

```json
{
  "ActivityId": "6aa994ac-b56e-4292-8448-0767a5657cc7",
  "EventName": "Create Queue",
  "resourceId": "/SUBSCRIPTIONS/1A2109E3-9DA0-455B-B937-E35E36C1163C/RESOURCEGROUPS/DEFAULT-SERVICEBUS-CENTRALUS/PROVIDERS/MICROSOFT.SERVICEBUS/NAMESPACES/SHOEBOXEHNS-CY4001",
  "SubscriptionId": "1a2109e3-9da0-455b-b937-e35e36c1163c",
  "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
  "EventProperties": "{\"SubscriptionId\":\"1a2109e3-9da0-455b-b937-e35e36c1163c\",\"Namespace\":\"shoeboxehns-cy4001\",\"Via\":\"https://shoeboxehns-cy4001.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
  "Status": "Succeeded",
  "Caller": "ServiceBus Client",
  "category": "OperationalLogs"
}
```

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sul bus di servizio, vedere i collegamenti seguenti:

* [Introduzione al bus di servizio](service-bus-messaging-overview.md)
* [Introduzione al bus di servizio](service-bus-dotnet-get-started-with-queues.md)
