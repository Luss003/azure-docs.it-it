---
title: 'Esercitazione: Configurare regole e azioni in Azure IoT Central'
description: Questa esercitazione illustra come i creatori possono configurare regole e azioni basate su dati di telemetria nell'applicazione Azure IoT Central.
author: ankitscribbles
ms.author: ankitgup
ms.date: 11/13/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 9140114e7d31f24770bdcce9aae849b01aae9996
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/17/2020
ms.locfileid: "76263637"
---
# <a name="tutorial-configure-rules-and-actions-for-your-device-in-azure-iot-central"></a>Esercitazione: Configurare regole e azioni per il dispositivo in Azure IoT Central

*Questo articolo è rivolto a operatori, autori e amministratori.*

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

In questa esercitazione si crea una regola che invia un messaggio di posta elettronica quando la temperatura di un condizionatore connesso supera 70&deg; F (circa 32° C).

In questa esercitazione verranno illustrate le procedure per:

> [!div class="checklist"]
>
> * Creare una regola basata su dati di telemetria
> * Aggiungere un'azione

## <a name="prerequisites"></a>Prerequisites

Prima di iniziare, si consiglia di completare l'esercitazione [Definire un nuovo tipo di dispositivo nell'applicazione](tutorial-define-device-type.md) per creare il modello di dispositivo **Condizionatore connesso** con cui lavorare.

## <a name="create-a-telemetry-based-rule"></a>Creare una regola basata su dati di telemetria

1. Per aggiungere una nuova regola basata sui dati di telemetria all'applicazione, nel riquadro sinistro selezionare **Modelli di dispositivo**.

    ![Pagina Modelli di dispositivo](media/tutorial-configure-rules/templatespage1.png)

    Viene visualizzato il modello di dispositivo **Condizionatore connesso (1.0.0)** creato nell'esercitazione precedente.

2. Per personalizzare il modello di dispositivo, selezionare il modello **Connected Air Conditioner** creato nell'esercitazione precedente.

3. Per aggiungere una regola basata su dati di telemetria, nella visualizzazione **Regole** selezionare **Regole**, quindi **+ Nuova regola** e infine **Telemetria**:

    ![Visualizzazione Regole](media/tutorial-configure-rules/newrule.png)

4. Usare le informazioni nella tabella seguente per definire la regola:

    | Impostazione                                      | valore                             |
    | -------------------------------------------- | ------------------------------    |
    | Nome                                         | Avviso temperatura condizionatore |
    | Abilita la regola per tutti i dispositivi di questo modello | Attivato                                |
    | Condizione                                    | La temperatura è maggiore di 70    |
    | Aggregazione                                  | nessuno                              |

    ![Condizione della regola di temperatura](media/tutorial-configure-rules/temperaturerule.png)

    Selezionare quindi **Salva**.

## <a name="add-an-action"></a>Aggiungere un'azione

Quando si definisce una regola è anche possibile definire un'azione da eseguire quando vengono soddisfatte le condizioni della regola. In questa esercitazione viene creata una regola con un'azione che invia una notifica tramite posta elettronica.

1. Per aggiungere un'**azione**, **salvare** prima la regola e quindi scorrere verso il basso nel pannello **Configure Telemetry Rule** (Configura regola di telemetria). Scegliere il segno **+** accanto ad **Azioni** e quindi scegliere **Posta elettronica**:

    ![Azione della regola di temperatura](media/tutorial-configure-rules/addaction.png)

2. Usare le informazioni nella tabella seguente per definire l'azione:

    | Impostazione      | valore                                               |
    | ------------ | --------------------------------------------------- |
    | Nome visualizzato | Avviso di temperatura tramite posta elettronica                             |
    | A           | Indirizzo di posta elettronica                                  |
    | Note        | La temperatura del condizionatore ha superato la soglia. |

    > [!NOTE]
    > Per ricevere una notifica, l'indirizzo di posta elettronica deve essere un [ID utente nell'applicazione](howto-administer.md) e tale utente deve aver eseguito almeno una volta l'accesso all'applicazione.

    ![Azione della temperatura](media/tutorial-configure-rules/temperatureaction.png)

3. Selezionare **Salva**. La regola è elencata nella pagina **Regole**.

## <a name="test-the-rule"></a>Testare la regola

Subito dopo aver salvato la regola, questa viene applicata. Quando vengono soddisfatte le condizioni definite nella regola, l'applicazione invia un messaggio all'indirizzo di posta elettronica specificato nell'azione.

> [!NOTE]
> Al termine del test, disattivare la regola per smettere di ricevere gli avvisi nella posta in arrivo.

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione sono state illustrate le procedure per:

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
>
> * Creare una regola basata su dati di telemetria
> * Aggiungere un'azione

Dopo aver definito la regola basata su una soglia, il passaggio successivo consigliato consiste nel [Personalizzare le visualizzazioni dell'operatore](tutorial-customize-operator.md).

Per altre informazioni sui diversi tipi di regole in Azure IoT Central e su come aggiungere parametri alla definizione della regola, vedere:

* [Creare una regola di telemetria e impostare le notifiche](howto-create-telemetry-rules.md).
* [Creare una regola di evento e impostare le notifiche](howto-create-event-rules.md).

<!-- Next tutorials in the sequence -->