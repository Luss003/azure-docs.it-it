---
title: "Esercitazione: Usare eventi dell'hub IoT per attivare App per la logica di Azure"
description: "Esercitazione: Tramite il servizio di routing di eventi di Griglia di eventi di Azure, creare processi automatizzati per eseguire azioni di App per la logica di Azure in base ad eventi dell'hub IoT."
services: iot-hub
documentationcenter: ''
author: kgremban
manager: philmea
editor: ''
ms.service: iot-hub
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2019
ms.author: kgremban
ms.openlocfilehash: e003cb650b0589ab43c984850838c56cbbf1ff2f
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2019
ms.locfileid: "74106770"
---
# <a name="tutorial-send-email-notifications-about-azure-iot-hub-events-using-logic-apps"></a>Esercitazione: Inviare notifiche di posta elettronica sugli eventi dell'hub IoT di Azure usando App per la logica

Griglia di eventi di Azure consente di rispondere agli eventi dell'hub IoT attivando azioni nelle applicazioni aziendali downstream.

Questo articolo illustra una configurazione di esempio che usa l'hub IoT e Griglia di eventi. Al termine, si otterrà un'app per la logica di Azure configurata per l'invio di un messaggio di posta elettronica di notifica ogni volta che viene aggiunto un dispositivo all'hub IoT. 

## <a name="prerequisites"></a>Prerequisiti

* Un account di un provider di posta elettronica supportato da App per la logica di Azure, ad esempio Office 365 Outlook, Outlook.com o Gmail. Questo account di posta elettronica viene usato per inviare le notifiche degli eventi. Per un elenco completo dei connettori per App per la logica supportati, vedere [Panoramica dei connettori](https://docs.microsoft.com/connectors/).
* Un account Azure attivo. Se non si ha un account, è possibile [crearne uno gratuito](https://azure.microsoft.com/pricing/free-trial/).
* Un hub IoT in Azure. Se l'hub non è stato ancora creato, vedere [Introduzione all'hub IoT](../iot-hub/iot-hub-csharp-csharp-getstarted.md) per la procedura dettagliata. 

## <a name="create-a-logic-app"></a>Creare un'app per la logica

Per prima cosa, creare un'app per la logica e aggiungere un trigger di Griglia di eventi che monitori il gruppo di risorse per la macchina virtuale. 

### <a name="create-a-logic-app-resource"></a>Creare una risorsa di App per la logica

1. Nel [portale di Azure](https://portal.azure.com) selezionare **Crea una risorsa** > **Integrazione** > **App per la logica**.

   ![Creare l'app per la logica](./media/publish-iot-hub-events-to-logic-apps/select-logic-app.png)

2. Assegnare all'app per la logica un nome che sia univoco nella sottoscrizione e quindi selezionare la stessa sottoscrizione, lo stesso gruppo di risorse e la stessa posizione dell'hub IoT. 
3. Selezionare **Create** (Crea).

4. Dopo aver creato la risorsa, passare all'app per la logica. 

5. Progettazione app per la logica mostra i modelli comuni disponibili per poter iniziare più rapidamente. In Progettazione app per la logica, nella sezione **Modelli**, scegliere **App per la logica vuota** per creare un'app per la logica completamente nuova.

### <a name="select-a-trigger"></a>Selezionare un trigger

Un trigger è un evento specifico che avvia l'app per la logica. Per questa esercitazione, il trigger che attiva il flusso di lavoro riceve una richiesta tramite HTTP.  

1. Nella barra di ricerca dei trigger e dei connettori digitare **HTTP**.
2. Selezionare **Richiesta - Alla ricezione di una richiesta HTTP** come trigger. 

   ![Selezionare il trigger di richiesta HTTP](./media/publish-iot-hub-events-to-logic-apps/http-request-trigger.png)

3. Selezionare **Usare il payload di esempio per generare lo schema**. 

   ![Selezionare il trigger di richiesta HTTP](./media/publish-iot-hub-events-to-logic-apps/sample-payload.png)

4. Incollare l'esempio di codice JSON seguente nella casella di testo e quindi selezionare **Fine**:

   ```json
   [{
     "id": "56afc886-767b-d359-d59e-0da7877166b2",
     "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>",
     "subject": "devices/LogicAppTestDevice",
     "eventType": "Microsoft.Devices.DeviceCreated",
     "eventTime": "2018-01-02T19:17:44.4383997Z",
     "data": {
       "twin": {
         "deviceId": "LogicAppTestDevice",
         "etag": "AAAAAAAAAAE=",
         "deviceEtag": "null",
         "status": "enabled",
         "statusUpdateTime": "0001-01-01T00:00:00",
         "connectionState": "Disconnected",
         "lastActivityTime": "0001-01-01T00:00:00",
         "cloudToDeviceMessageCount": 0,
         "authenticationType": "sas",
         "x509Thumbprint": {
           "primaryThumbprint": null,
           "secondaryThumbprint": null
         },
         "version": 2,
         "properties": {
           "desired": {
             "$metadata": {
               "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
             },
             "$version": 1
           },
           "reported": {
             "$metadata": {
               "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
             },
             "$version": 1
           }
         }
       },
       "hubName": "egtesthub1",
       "deviceId": "LogicAppTestDevice"
     },
     "dataVersion": "1",
     "metadataVersion": "1"
   }]
   ```

5. È possibile che venga inviata una notifica popup con la dicitura **Ricordare di includere nella richiesta un'intestazione Content-Type impostata su application/json**. Questo suggerimento può essere tranquillamente ignorato ed è possibile passare alla sezione successiva. 

### <a name="create-an-action"></a>Creare un'azione

Le azioni sono i passaggi eseguiti dopo che il trigger ha avviato il flusso di lavoro dell'app per la logica. Per questa esercitazione, l'azione è costituita dall'invio di una notifica dal provider di posta elettronica. 

1. Selezionare **Nuovo passaggio**. Verrà visualizzata una finestra **Scegliere un'azione**.

2. Cercare **Posta elettronica**.

3. Selezionare il connettore corrispondente al provider di posta elettronica in uso. Questa esercitazione usa **Office 365 Outlook**. I passaggi per altri provider di posta elettronica sono del tutto simili. 

   ![Selezionare il connettore del provider di posta elettronica](./media/publish-iot-hub-events-to-logic-apps/o365-outlook.png)

4. Selezionare l'azione **Invia un messaggio di posta elettronica**. 

5. Se richiesto, accedere all'account di posta elettronica. 

6. Creare il modello di messaggio di posta elettronica. 
   * **A**: immettere l'indirizzo di posta elettronica per ricevere i messaggi di notifica. Per questa esercitazione, usare un account di posta elettronica a cui è possibile accedere per il test. 
   * **Oggetto** e **Corpo**: scrivere il testo del messaggio di posta elettronica. Selezionare le proprietà JSON tramite lo strumento di selezione per includere il contenuto dinamico in base ai dati degli eventi.  

   Il modello di messaggio di posta elettronica creato sarà simile all'esempio seguente:

   ![Immettere le informazioni del messaggio di posta elettronica](./media/publish-iot-hub-events-to-logic-apps/email-content.png)

7. Salvare l'app per la logica. 

### <a name="copy-the-http-url"></a>Copiare l'URL HTTP

Prima di uscire da Progettazione app per la logica, copiare l'URL su cui è in ascolto l'app per la logica per un trigger. Questo URL viene usato per configurare Griglia di eventi. 

1. Espandere la casella di configurazione del trigger **Alla ricezione di una richiesta HTTP** facendo clic sull'opzione. 
2. Copiare il valore di **URL POST HTTP** facendo clic sul pulsante di copia che si trova accanto. 

   ![Copiare l'URL POST HTTP](./media/publish-iot-hub-events-to-logic-apps/copy-url.png)

3. Salvare l'URL per poterlo usare nella sezione successiva. 

## <a name="configure-subscription-for-iot-hub-events"></a>Configurare la sottoscrizione degli eventi dell'hub IoT

In questa sezione viene configurato l'hub IoT per la pubblicazione degli eventi che si verificano. 

1. Nel portale di Azure passare all'hub IoT. 
2. Selezionare **Eventi**.

   ![Aprire i dettagli di Griglia di eventi](./media/publish-iot-hub-events-to-logic-apps/event-grid.png)

3. Selezionare **Sottoscrizione di eventi**. 

   ![Creare una nuova sottoscrizione di eventi](./media/publish-iot-hub-events-to-logic-apps/event-subscription.png)

4. Creare la sottoscrizione di eventi con i valori seguenti: 
   * **Tipo di evento**: deselezionare Esegui la sottoscrizione di tutti i tipi di eventi e selezionare **Creati da dispositivo** dal menu.
   * **Dettagli endpoint**: come Tipo di endpoint selezionare **Webhook**, fare clic su Selezione endpoint, incollare l'URL copiato dall'app per la logica e confermare la selezione.

     ![URL di Selezione endpoint](./media/publish-iot-hub-events-to-logic-apps/endpoint-url.png)

   * **Dettagli sottoscrizione evento**: fornire un nome descrittivo e selezionare **Schema Griglia di eventi**

   Al termine, verrà visualizzato un modulo simile all'esempio seguente: 

    ![Modulo sottoscrizione di eventi di esempio](./media/publish-iot-hub-events-to-logic-apps/subscription-form.png)

5. È possibile salvare la sottoscrizione di eventi e ricevere notifiche per ogni dispositivo creato nell'hub IoT. Per questa esercitazione, tuttavia, si vogliono usare i campi facoltativi per filtrare dispositivi specifici. Selezionare **Funzionalità aggiuntive** nella parte superiore del modulo. 

6. Creare i filtri seguenti:

   * **L'oggetto inizia con**: immettere `devices/Building1_` per filtrare gli eventi del dispositivo nell'edificio 1.
   * **L'oggetto termina con**: immettere `_Temperature` per filtrare gli eventi del dispositivo correlati alla temperatura.

5. Selezionare **Crea** per salvare la sottoscrizione di eventi.

## <a name="create-a-new-device"></a>Creare un nuovo dispositivo

Testare l'app per la logica creando un nuovo dispositivo per l'attivazione di un messaggio di posta elettronica di notifica degli eventi. 

1. Nell'hub IoT selezionare **IoT Devices** (Dispositivi IoT). 
2. Selezionare **Aggiungi**.
3. In **ID dispositivo** immettere `Building1_Floor1_Room1_Temperature`.
4. Selezionare **Salva**. 
5. È possibile aggiungere più dispositivi con ID diversi per testare i filtri della sottoscrizione di eventi. Provare con questi esempi: 
   * Building1_Floor1_Room1_Light
   * Building1_Floor2_Room2_Temperature
   * Building2_Floor1_Room1_Temperature
   * Building2_Floor1_Room1_Light

Dopo aver aggiunto alcuni dispositivi all'hub IoT, controllare la posta elettronica per vedere quali sono quelli che hanno attivato l'app per la logica. 

## <a name="use-the-azure-cli"></a>Utilizzare l’interfaccia della riga di comando di Azure

Anziché tramite il portale di Azure, è possibile eseguire le procedure relative all'hub IoT usando l'interfaccia della riga di comando di Azure. Per informazioni dettagliate, vedere le pagine relative all'interfaccia della riga di comando di Azure per la [creazione di una sottoscrizione di eventi](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription) e la [creazione di un dispositivo IoT](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity).

## <a name="clean-up-resources"></a>Pulire le risorse

Questa esercitazione ha usato risorse che generano addebiti sulla sottoscrizione di Azure. Al termine dell'esercitazione e dopo aver testato i risultati ottenuti, disabilitare o eliminare le risorse che non si vogliono mantenere. 

Per non perdere il lavoro fatto, è possibile disabilitare l'app per la logica anziché eliminarla. 

1. Passare all'app per la logica.
2. Nel pannello **Panoramica** selezionare **Elimina** o **Disabilita**. 

Ogni sottoscrizione può avere un unico hub IoT gratuito. Se per questa esercitazione è stato creato un hub gratuito, non è necessario eliminarlo per impedire eventuali addebiti.

1. Passare all'hub IoT. 
2. Nel pannello **Panoramica** selezionare **Elimina**. 

Anche se si mantiene l'hub IoT, può essere opportuno eliminare la sottoscrizione di eventi creata. 

1. Nell'hub IoT selezionare **Griglia di eventi**.
2. Selezionare la sottoscrizione di eventi da rimuovere. 
3. Selezionare **Elimina**. 

## <a name="next-steps"></a>Passaggi successivi

* Altre informazioni su come [rispondere agli eventi dell'hub IoT usando Griglia di eventi per attivare le azioni](../iot-hub/iot-hub-event-grid.md).
* [Informazioni sull'ordinamento di eventi di connessione e disconnessione dispositivi](../iot-hub/iot-hub-how-to-order-connection-state-events.md)
* Informazioni sulle altre operazioni che è possibile eseguire con [Griglia di eventi](overview.md).


