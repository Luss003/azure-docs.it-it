---
title: Connettività dei dispositivi in Azure IoT Central | Microsoft Docs
description: Questo articolo fornisce un riepilogo del modo in cui Azure IoT Central aiuta a garantire la connessione e l'integrità dei dispositivi.
author: aaronbjork
ms.author: abjork
ms.date: 12/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: ''
ms.openlocfilehash: 7c102e7ffde7faab5d864c1d9acaafe141664ebf
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2020
ms.locfileid: "76119551"
---
# <a name="stay-connected-with-azure-iot-central-preview-features"></a>Rimanere connessi con IoT Central di Azure (funzionalità di anteprima)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Questo articolo fornisce una panoramica del modo in cui Azure IoT Central aiuta a garantire la connessione e l'integrità dei dispositivi.

Con qualsiasi soluzione Internet per le cose progettata per operare su larga scala, è importante un approccio ben strutturato alla gestione dei dispositivi. Non è sufficiente che i dispositivi siano connessi al cloud, ma è necessario un approccio per proteggere i dispositivi in modo che la soluzione si evolva, cresca e invecchia. Azure IoT Central fornisce le funzionalità necessarie per garantire che i dispositivi nella soluzione Internet delle cose siano ben curati per tutto il ciclo di vita dell'applicazione.

## <a name="dashboards"></a>Dashboard 
I [Dashboard](howto-manage-devices.md#import-devices) predefiniti offrono una superficie di attacco personalizzabile per monitorare l'integrità e la telemetria dei dispositivi. È possibile iniziare con un dashboard predefinito da un [modello di applicazione](howto-use-app-templates.md) o creare dashboard personalizzati in base alle esigenze dell'applicazione. I dashboard possono essere condivisi con tutti gli utenti dell'applicazione o mantenuti privati.

## <a name="rules-and-actions"></a>Regole e azioni 
Identificare rapidamente i dispositivi che richiedono attenzione creando [regole personalizzate](tutorial-create-telemetry-rules.md) basate sullo stato del dispositivo e sulla telemetria. Configurare le azioni per notificare agli utenti appropriati le misure correttive eseguite in modo tempestivo.

## <a name="jobs"></a>Processi 
I [processi](../core/howto-run-a-job.md?toc=/azure/iot-central/preview/toc.json&bc=/azure/iot-central/preview/breadcrumb/toc.json) consentono di eseguire aggiornamenti singoli o in blocco per le proprietà, le impostazioni e i comandi del dispositivo. 

## <a name="user-roles-and-permissions"></a>Ruoli utente e autorizzazioni
I [ruoli e le autorizzazioni](howto-manage-users-roles.md) offrono la possibilità di personalizzare l'esperienza di ogni utente. Creare ruoli direttamente tramite l'interfaccia utente e assegnare facilmente le autorizzazioni appropriate. 




