---
title: Creare Dashboard personali Azure IoT Central | Microsoft Docs
description: Un utente di informazioni su come creare e gestire i dashboard personali.
author: dominicbetts
ms.author: dobett
ms.date: 06/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: c048ae8c0daba0e467a9243f4dd83f8d95921e10
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/02/2019
ms.locfileid: "67502641"
---
# <a name="create-and-manage-personal-dashboards"></a>Creare e gestire i dashboard personali

Il **Dashboard** è la pagina che viene caricato quando si passa prima di tutto per l'applicazione. Oggetto **generatore** definisce i dashboard dell'applicazione predefinita per tutti gli utenti nell'applicazione. È possibile sostituire questo dashboard predefinito con il proprio, dashboard dell'applicazione personalizzata. È possibile avere diversi dashboard che visualizzano dati diversi e spostarsi tra le due.

## <a name="create-dashboard"></a>Creare un dashboard

Lo screenshot seguente mostra il dashboard in un'applicazione creata mediante il **Contoso di esempio** modello. È possibile sostituire il dashboard dell'applicazione predefinito con un dashboard personale. A tale scopo, selezionare **+ nuovo** nella parte superiore destra della pagina.

![Dashboard per le applicazioni basate sul modello di "Contoso di esempio"](media/howto-personalize-dashboard/defaultdashboard.png)

Selezionando **+ nuovo**, viene aperto l'editor del dashboard. Nell'editor, è possibile assegnare un nome al dashboard e sceglie gli elementi dalla raccolta. La libreria contiene i riquadri e le primitive di dashboard che è possibile usare per personalizzare il dashboard.

![Libreria dashboard](media/howto-personalize-dashboard/dashboardeditor.png)

Ad esempio, è possibile aggiungere un **le impostazioni del dispositivo e proprietà** riquadro per visualizzare i valori di proprietà e le impostazioni per uno dei dispositivi gestiti. A tale scopo, selezionare prima di tutto un **Device Template** (Modello di dispositivo) quindi selezionare una **Device Instance** (Istanza del dispositivo). Assegnare quindi il riquadro, un titolo e selezionare una **impostazione** o una **proprietà** da visualizzare. La schermata seguente mostra le **effettuare il fan-velocità** impostazione selezionata da aggiungere al riquadro. Selezionare **Done** per salvare la modifica al dashboard.

![Modulo "Configure Device Details" (Configura dettagli dispositivo) con i dettagli per impostazioni e proprietà](media/howto-personalize-dashboard/dashboardsetting.png)

A questo punto quando si visualizza il dashboard persona, viene visualizzato il nuovo riquadro con il **effettuare il fan-velocità** impostazione per il dispositivo:

![Scheda "Dashboard" con visualizzate le impostazioni e le proprietà per il riquadro](media/howto-personalize-dashboard/personaldashboard.png)

È possibile esplorare altri tipi di riquadro nella libreria per informazioni su come personalizzare ulteriormente i dashboard personali.

Per altre informazioni su come usare i riquadri in Azure IoT Central, vedere [usare i riquadri del dashboard](howto-use-tiles.md).

## <a name="manage-dashboards"></a>Gestire i dashboard

È possibile avere diversi Dashboard personali e passare tra loro o selezionare il dashboard dell'applicazione predefinita:

![Dashboard commutatore](media/howto-personalize-dashboard/switchdashboards.png)

È possibile modificare il Dashboard personale ed eliminare quelli che non è più necessario:

![Elimina dashboard](media/howto-personalize-dashboard/managedashboards.png)

## <a name="next-steps"></a>Passaggi successivi

Ora che si è appreso come creare e gestire i dashboard personali, è possibile:

> [!div class="nextstepaction"]
> [Informazioni su come gestire le preferenze dell'applicazione](howto-manage-preferences.md)
