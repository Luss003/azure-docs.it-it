---
title: Snapshot temporizzato di Configurazione app di Azure | Microsoft Docs
description: Panoramica del funzionamento dello snapshot temporizzato in Configurazione app di Azure
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: 83770e8c5f415670855b5cf2502d02c4d6919440
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2019
ms.locfileid: "59998074"
---
# <a name="point-in-time-snapshot"></a>Snapshot temporizzato

Configurazione app di Azure conserva i record dei momenti precisi in cui una nuova coppia chiave-valore viene creata e quindi modificata. Questi record formano una sequenza temporale completa relativa alle modifiche delle coppie chiave-valore. Un archivio di configurazione app consente di ricostruire la cronologia di qualsiasi coppia chiave-valore e di risalire a un valore precedente in un momento qualsiasi, fino a quello attuale. Questa funzionalità consente di "viaggiare indietro nel tempo" e di recuperare una coppia chiave-valore precedente. È ad esempio possibile ottenere le impostazioni di configurazione del giorno precedente, subito prima della distribuzione più recente, in modo da poter recuperare una configurazione precedente ed eseguire il rollback dell'applicazione.

## <a name="key-value-retrieval"></a>Recupero di coppie chiave-valore

Per recuperare coppie chiave-valore precedenti, specificare il momento di acquisizione dello snapshot delle coppie chiave-valore nell'intestazione HTTP di una chiamata all'API REST. Ad esempio: 

        GET /kv HTTP/1.1
        Accept-Datetime: Sat, 1 Jan 2019 02:10:00 GMT

Attualmente, il servizio Configurazione app conserva sette giorni di cronologia delle modifiche.

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Creare un'app Web ASP.NET Core](./quickstart-aspnet-core-app.md)  
