---
title: Impostazioni delle porte di Inoltro di Azure | Documentazione Microsoft
description: Informazioni dettagliate sui valori di porta di Inoltro di Azure.
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/26/2018
ms.author: spelluru
ms.openlocfilehash: 9d11179a8518ebf48f68f8607f94e0253d4edb80
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "60789927"
---
# <a name="azure-relay-port-settings"></a>Impostazioni delle porte di inoltro di Azure

La tabella seguente descrive la configurazione necessaria per i valori della porta del servizio Inoltro di Azure.

## <a name="hybrid-connections"></a>connessioni ibride

Connessioni ibride si avvale di WebSocket sulla porta 443 come meccanismo di trasporto di base che usa solo **HTTPS**. 

## <a name="wcf-relays"></a>Inoltri WCF
  
|Associazione|Sicurezza trasporto|Port|  
|-------------|------------------------|----------|  
|[Classe BasicHttpRelayBinding](/dotnet/api/microsoft.servicebus.basichttprelaybinding) (client)|Yes|HTTPS| 
|" |No|HTTP|  
|[Classe BasicHttpRelayBinding](/dotnet/api/microsoft.servicebus.basichttprelaybinding) (servizio)|È possibile usare il|9351/HTTP|  
|[Classe NetEventRelayBinding](/dotnet/api/microsoft.servicebus.neteventrelaybinding) (client)|Yes|9351/HTTPS|  
|" |No|9350/HTTP|  
|[Classe NetEventRelayBinding](/dotnet/api/microsoft.servicebus.neteventrelaybinding) (servizio)|È possibile usare il|9351/HTTP|  
|[Classe NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) (client/servizio)|È possibile usare il|5671/9352/HTTP (9352/9353 in caso di implementazione ibrida)|  
|[Classe NetOnewayRelayBinding](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) (client)|Yes|9351/HTTPS|  
|" |No|9350/HTTP|  
|[Classe NetOnewayRelayBinding](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) (servizio)|È possibile usare il|9351/HTTP|  
|[Classe WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) (client)|Yes|HTTPS|  
|" |No|HTTP|  
|[Classe WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) (servizio)|È possibile usare il|9351/HTTP|  
|[Classe WS2007HttpRelayBinding](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding) (client)|Yes|HTTPS|  
|" |No|HTTP|  
|[Classe WS2007HttpRelayBinding](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding) (servizio)|È possibile usare il|9351/HTTP|

## <a name="next-steps"></a>Passaggi successivi
Per altre informazioni sul servizio di inoltro di Azure, vedere i collegamenti seguenti:
* [Che cos'è il servizio di inoltro di Azure?](relay-what-is-it.md)
* [Domande frequenti sul servizio di inoltro](relay-faq.md)