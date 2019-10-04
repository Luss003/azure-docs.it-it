---
title: Informazioni sulle versioni dei modelli di dispositivo per le app Azure IoT Central | Microsoft Docs
description: Eseguire iterazioni con i modelli di dispositivo creando nuove versioni, senza determinare conseguenze per i dispositivi collegati
author: sandeeppujar
ms.author: sandeepu
ms.date: 07/08/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 155f392410c5722a28ba09acafc1480e72586773
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2019
ms.locfileid: "70100896"
---
# <a name="create-a-new-device-template-version"></a>Creare una nuova versione di un modello di dispositivo

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

Azure IoT Central consente lo sviluppo rapido di applicazioni IoT. È possibile eseguire velocemente iterazioni con i progetti dei modelli di dispositivo per aggiungere, modificare o eliminare misure, impostazioni o proprietà. Alcune di queste modifiche potrebbe compromettere i dispositivi attualmente connessi. Azure IoT Central identifica queste modifiche intrusive e fornisce un modo per distribuire in sicurezza gli aggiornamenti ai dispositivi.

Un modello di dispositivo ha un numero di versione quando viene creato. Per impostazione predefinita, il numero di versione è 1.0.0. Se si modifica un modello di dispositivo e tale modifica potrebbe influire sui dispositivi attualmente connessi, Azure IoT Central chiederà di creare una nuova versione del modello di dispositivo.

> [!NOTE]
> Per altre informazioni su come creare un modello di dispositivo, vedere [Configurare un modello di dispositivo](howto-set-up-template.md)

## <a name="changes-that-prompt-a-version-change"></a>Modifiche che richiedono un cambio di versione

In generale le modifiche alle impostazioni o proprietà del modello di dispositivo richiedono un cambio di versione.

> [!NOTE]
> Quando si modifica il modello di dispositivo in genere non viene richiesto di creare una nuova versione se non ci sono dispositivi connessi o al massimo c'è un solo dispositivo connesso.

L'elenco seguente descrive le azioni dell'utente che potrebbero richiedere una nuova versione:

* Proprietà (obbligatorie)
    * Aggiunta o eliminazione di una proprietà obbligatoria
    * Modifica del nome di campo di una proprietà, nome di campo che viene usato dai dispositivi per inviare messaggi.
*  Proprietà (facoltative)
    * Eliminazione di una proprietà facoltativa
    * Modifica del nome di campo di una proprietà, nome di campo che viene usato dai dispositivi per inviare messaggi.
    * Modifica di una proprietà facoltativa in obbligatoria
*  Impostazioni
    * Aggiunta o eliminazione di un'impostazione
    * Modifica del nome di campo di un'impostazione, nome di campo utilizzato per inviare e ricevere messaggi dai dispositivi.

## <a name="what-happens-on-version-change"></a>Cosa accade con il cambio di versione?

Cosa accade alle regole e ai dashboard di dispositivo quando viene eseguito un cambio di versione?

Le **regole** della versione precedente del modello di dispositivo continuano a funzionare senza modifiche. Non viene automaticamente eseguita la migrazione delle regole alla nuova versione del modello di dispositivo. È possibile creare regole per la nuova versione del modello come di consueto. Per altre informazioni, vedere l'articolo procedure per la [creazione di una regola di telemetria e la configurazione delle notifiche nell'applicazione Azure IOT Central](howto-create-telemetry-rules.md) .

I **dashboard di dispositivo** possono contenere diversi tipi di riquadri. Alcuni riquadri possono contenere impostazioni e proprietà. Quando viene rimossa un'impostazione o una proprietà utilizzata in un riquadro, il riquadro perde l'integrità totalmente o parzialmente. È possibile accedere al riquadro e risolvere il problema eliminando il riquadro o aggiornando il contenuto del riquadro.

## <a name="migrate-a-device-across-device-template-versions"></a>Eseguire la migrazione di un dispositivo fra diverse versioni del modello di dispositivo

È possibile creare più versioni del modello di dispositivo. Nel corso del tempo si avranno più dispositivi connessi che usano questi modelli di dispositivo. È possibile eseguire la migrazione dei dispositivi da una versione del modello di dispositivo a un'altra. I passaggi seguenti descrivono come eseguire la migrazione di un dispositivo:

1. Passare alla pagina **Device Explorer** .
1. Selezionare il dispositivo di cui eseguire la migrazione a un'altra versione.
1. Scegliere **Migrate Device** (Esegui la migrazione del dispositivo).
1. Selezionare il numero di versione a cui portare il dispositivo e scegliere **Migrate** (Esegui migrazione).

![Come eseguire la migrazione di un dispositivo](media/howto-version-device-template/pick-version.png)

## <a name="next-steps"></a>Passaggi successivi

Ora che si conosce la procedura per usare le versioni dei modelli di dispositivo nell'applicazione Azure IoT Central, il prossimo passaggio suggerito è:

> [!div class="nextstepaction"]
> [Come creare regole dei dati di telemetria](howto-create-telemetry-rules.md)