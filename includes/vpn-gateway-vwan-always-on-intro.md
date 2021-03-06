---
title: File di inclusione
description: File di inclusione
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/12/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 2783c828f96eeb3539e2266eaf1d6d590a6af4c4
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/14/2020
ms.locfileid: "79370984"
---
Una nuova funzionalità del client VPN di Windows 10, Always On, è la possibilità di mantenere una connessione VPN. Con Always On, il profilo VPN attivo può connettersi automaticamente e rimanere connesso in base ai trigger, ad esempio l'accesso utente, la modifica dello stato della rete o lo schermo del dispositivo attivo.

È possibile usare i gateway di rete virtuale di Azure con Windows 10 Always On per stabilire tunnel utente persistenti e tunnel di dispositivo in Azure. Questo articolo consente di configurare un tunnel utente Always On VPN.

Always On connessioni VPN includono uno dei due tipi di tunnel:

* **Tunnel del dispositivo**: consente di connettersi ai server VPN specificati prima di accedere al dispositivo. Gli scenari di connettività di pre-accesso e la gestione dei dispositivi usano un tunnel del dispositivo.

* **Tunnel utente**: si connette solo dopo che gli utenti hanno eseguito l'accesso al dispositivo. Usando i tunnel utente è possibile accedere alle risorse dell'organizzazione tramite i server VPN.

I tunnel e i tunnel utente del dispositivo operano indipendentemente dai profili VPN. Possono essere connesse contemporaneamente e possono usare metodi di autenticazione diversi e altre impostazioni di configurazione VPN, in base alle esigenze.
