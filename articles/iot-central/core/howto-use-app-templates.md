---
title: Usare i modelli di applicazione in Azure IoT Central | Microsoft Docs
description: Come usare i set di dispositivi nell'applicazione Azure IoT Central in qualità di operatore.
author: dominicbetts
ms.author: dobett
ms.date: 05/30/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 3cc6f82676f426240fba4cc4910246073aa9a556
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/04/2020
ms.locfileid: "76982461"
---
# <a name="use-application-templates"></a>Usare i modelli di applicazione

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

Questo articolo descrive come Gestione soluzioni per creare e usare i modelli di applicazione.

Quando si crea un'applicazione IoT Central di Azure, è possibile scegliere tra modelli di esempio predefiniti. È anche possibile creare modelli di applicazione personalizzati da applicazioni IoT Central esistenti. È quindi possibile usare i modelli di applicazione personalizzati quando si creano nuove applicazioni.

Quando si crea un modello di applicazione, include gli elementi seguenti dell'applicazione esistente:

- Il dashboard dell'applicazione predefinito, inclusi il layout del dashboard e tutti i riquadri definiti.
- Modelli di dispositivo, incluse misure, impostazioni, proprietà, comandi e dashboard.
- Regole. Sono incluse tutte le definizioni delle regole. Tuttavia, le azioni, ad eccezione delle azioni di posta elettronica, non sono incluse.
- Set di dispositivi, incluse le condizioni e i dashboard.

> [!WARNING]
> Se un dashboard include riquadri che visualizzano informazioni su dispositivi specifici, i riquadri mostrano che **la risorsa richiesta non è stata trovata** nella nuova applicazione. È necessario riconfigurare questi riquadri per visualizzare le informazioni sui dispositivi nella nuova applicazione.

Quando si crea un modello di applicazione, non include gli elementi seguenti:

- Dispositivi
- Utenti
- Definizioni processi
- Definizioni di esportazione dei dati continua

Aggiungere manualmente questi elementi a qualsiasi applicazione creata da un modello di applicazione.

## <a name="create-an-application-template"></a>Creare un modello di applicazione

Per creare un modello di applicazione da un'applicazione IoT Central esistente:

1. Passare alla sezione **Amministrazione** nell'applicazione.
1. Selezionare **Esporta modello di applicazione**.
1. Nella pagina **esportazione modello di applicazione** immettere un nome e una descrizione per il modello.
1. Selezionare il pulsante **Esporta** per creare il modello di applicazione. È ora possibile copiare il **collegamento condivisibile** che consente a un utente di creare una nuova applicazione dal modello:

![Creare un modello di applicazione](media/howto-use-app-templates/create-template.png)

## <a name="use-an-application-template"></a>Usare un modello di applicazione

Per usare un modello di applicazione per creare una nuova applicazione IoT Central, è necessario un **collegamento condiviso**creato in precedenza. Incollare il **collegamento condivisibile** nella barra degli indirizzi del browser. Viene visualizzata la pagina **Crea un'applicazione** con il modello di applicazione personalizzato selezionato:

![Creare un'applicazione da un modello](media/howto-use-app-templates/create-app.png)

Selezionare il piano tariffario e compilare gli altri campi nel form. Selezionare quindi **Crea** per creare una nuova applicazione IoT Central dal modello di applicazione.

## <a name="manage-application-templates"></a>Gestire i modelli di applicazione

Nella pagina **esportazione modello di applicazione** è possibile eliminare o aggiornare il modello di applicazione.

Se si elimina un modello di applicazione, non è più possibile utilizzare il collegamento condivisibile generato in precedenza per creare nuove applicazioni.

Per aggiornare il modello di applicazione, modificare il nome o la descrizione del modello nella pagina **esportazione modello di applicazione** . Quindi selezionare di nuovo il pulsante **Esporta** . Questa azione genera un nuovo **collegamento condivisibile** e invalida eventuali URL di **collegamento condivisibili** precedenti.

## <a name="next-steps"></a>Passaggi successivi

Ora che si è appreso come usare i modelli di applicazione, il passaggio successivo suggerito consiste nell'apprendere come [gestire IOT Central dalla portale di Azure](howto-manage-iot-central-from-portal.md)
