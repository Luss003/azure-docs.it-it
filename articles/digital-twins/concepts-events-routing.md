---
title: Routing di eventi e messaggi-dispositivi gemelli digitali di Azure | Microsoft Docs
description: Panoramica del routing di eventi e messaggi agli endpoint di servizio con Gemelli digitali di Azure
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/03/2020
ms.openlocfilehash: 094a3a838269921339dcd6c2c3b551720f394251
ms.sourcegitcommit: 51ed913864f11e78a4a98599b55bbb036550d8a5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/04/2020
ms.locfileid: "75660325"
---
# <a name="routing-iot-events-and-messages"></a>Routing di eventi e messaggi

Internet delle cose soluzioni spesso uniscono diversi servizi avanzati che includono archiviazione, analisi e altro ancora. Questo articolo descrive come connettere app di Gemelli digitali di Azure a servizi di analisi, intelligenza artificiale e archiviazione di Azure per arricchirle con informazioni e funzionalità più avanzate.

## <a name="route-types"></a>Tipi di routing  

I dispositivi gemelli digitali di Azure offrono due modi per connettere gli eventi Internet con altri servizi di Azure o applicazioni aziendali:

* **Routing di eventi di Gemelli digitali di Azure**: un oggetto nel grafico spaziale che cambia, dati di telemetria che vengono ricevuti o una funzione definita dall'utente che crea una notifica in base a condizioni predefinite possono attivare eventi di Gemelli digitali di Azure. Gli utenti possono inviare questi eventi a [Hub eventi di Azure](https://azure.microsoft.com/services/event-hubs/), [argomenti del bus di servizio di Azure](https://azure.microsoft.com/services/service-bus/) o [Griglia di eventi di Azure](https://azure.microsoft.com/services/event-grid/) per l'ulteriore elaborazione.

* **Routing dei dati di telemetria dei dispositivi**: oltre al routing di eventi, Gemelli digitali di Azure può instradare anche messaggi di telemetria dei dispositivi non elaborati a hub eventi per ulteriori analisi e recupero di informazioni. Questi tipi di messaggi non vengono elaborati da Gemelli digitali di Azure. Vengono solo inoltrati all'hub eventi.

Gli utenti possono specificare uno o più endpoint in uscita per inviare eventi o inoltrare messaggi. Eventi e messaggi verranno inviati agli endpoint in base a queste preferenze di routing predefinite. In altre parole, gli utenti possono specificare un determinato endpoint per ricevere gli eventi delle operazioni del grafico, un altro per ricevere gli eventi di telemetria dei dispositivi e così via.

[![il routing degli eventi di Azure Digital Twins](media/concepts/digital-twins-events-routing.png)](media/concepts/digital-twins-events-routing.png#lightbox)

Il routing a Hub eventi mantiene l'ordine di invio dei messaggi di telemetria. I messaggi arrivano quindi nell'endpoint nello stesso ordine con cui sono stati originariamente ricevuti. 

Griglia di eventi e il bus di servizio non garantiscono che gli endpoint ricevano gli eventi nello stesso ordine in cui si sono verificati. Lo schema degli eventi include tuttavia un timestamp che è possibile usare per identificare l'ordine dopo che gli eventi raggiungono l'endpoint.

## <a name="route-implementation"></a>Implementazione del routing

Attualmente il servizio Gemelli digitali di Azure supporta gli elementi **EndpointType** seguenti:

* **EventHub**, l'endpoint della stringa di connessione di Hub eventi.
* **ServiceBus**, l'endpoint della stringa di connessione del bus di servizio.
* **EventGrid**, l'endpoint della stringa di connessione di Griglia di eventi.

Attualmente Gemelli digitali di Azure supporta gli elementi **EventType** seguenti da inviare all'endpoint scelto:

* **DeviceMessages**, i messaggi di telemetria inviati dai dispositivi degli utenti e inoltrati dal sistema.
* **TopologyOperation**, un'operazione che modifica il grafico o i metadati del grafico. Ad esempio, l'aggiunta o l'eliminazione di un'entità, come uno spazio.
* **SpaceChange**, una modifica in un valore calcolato di uno spazio come risultato di un messaggio di telemetria di un dispositivo.
* **SensorChange**, una modifica in un valore calcolato di un sensore come risultato di un messaggio di telemetria di un dispositivo.
* **UdfCustom**, una notifica personalizzata proveniente da una funzione definita dall'utente.

> [!IMPORTANT]  
> Non tutti gli elementi **EndpointType** supportano tutti gli **EventType**.
> Vedere la tabella seguente per gli elementi **EventTypes** consentiti per ogni elemento **EndpointType**.

|             | DeviceMessages | TopologyOperation | SpaceChange | SensorChange | UdfCustom |
| ----------- | -------------- | ----------------- | ----------- | ------------ | --------- |
| Hub eventi|     X          |         X         |     X       |      X       |   X       |
| ServiceBus|              |         X         |     X       |      X       |   X       |
| EventGrid|               |         X         |     X       |      X       |   X       |

>[!NOTE]  
>Per altre informazioni su come creare endpoint e per esempi di schema degli eventi, vedere [Traffico in uscita ed endpoint](how-to-egress-endpoints.md).

## <a name="next-steps"></a>Passaggi successivi

- Per informazioni sui limiti dell'anteprima di Gemelli digitali di Azure, vedere [Limiti del servizio durante l'anteprima pubblica](concepts-service-limits.md).

- Per provare un esempio di Gemelli digitali di Azure, vedere [Guida introduttiva: Trovare le stanze disponibili ](quickstart-view-occupancy-dotnet.md).