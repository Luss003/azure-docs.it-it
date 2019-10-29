---
title: 'Guida introduttiva: Controllare un dispositivo da Azure IoT (Node.js)'
description: In questa guida introduttiva vengono eseguite due applicazioni Node.js di esempio. Una è un'applicazione back-end che può controllare in remoto i dispositivi connessi all'hub. L'altra applicazione simula un dispositivo connesso all'hub che può essere controllato in remoto.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: quickstart
ms.custom: mvc, seo-javascript-september2019, seo-javascript-october2019
ms.date: 06/21/2019
ms.openlocfilehash: ed3946c40d8e3eff665e4454b81f408c2033c09a
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2019
ms.locfileid: "72529594"
---
# <a name="quickstart-use-nodejs-to-control-a-device-connected-to-an-azure-iot-hub"></a>Guida introduttiva: Usare Node.js per controllare un dispositivo connesso a un hub IoT di Azure

[!INCLUDE [iot-hub-quickstarts-2-selector](../../includes/iot-hub-quickstarts-2-selector.md)]

Hub IoT è un servizio di Azure che consente di gestire i dispositivi IoT dal cloud e di acquisire volumi elevati di dati di telemetria dai dispositivi nel cloud per l'archiviazione o l'elaborazione. In questa guida introduttiva viene usato un *metodo diretto* per controllare un dispositivo simulato connesso all'hub IoT. È possibile usare metodi diretti per modificare in remoto il comportamento di un dispositivo connesso all'hub IoT.

La guida introduttiva usa due applicazioni Node.js già scritte.

* Un'applicazione del dispositivo simulato che risponde ai metodi diretti chiamati da un'applicazione back-end. Per ricevere le chiamate dei metodi diretti, l'applicazione si connette a un endpoint specifico del dispositivo nell'hub IoT.

* Un'applicazione back-end che chiama i metodi diretti sul dispositivo simulato. Per chiamare un metodo diretto su un dispositivo l'applicazione si connette a un endpoint sul lato servizio dell'hub IoT.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.

## <a name="prerequisites"></a>Prerequisiti

Le due applicazioni di esempio eseguite in questa guida introduttiva sono scritte in Node.js. È necessario disporre di Node.js v10.x.x o versioni successive nel computer di sviluppo.

È possibile scaricare Node.js per più piattaforme da [nodejs.org](https://nodejs.org).

È possibile verificare la versione corrente di Node.js installata nel computer di sviluppo tramite il comando seguente:

```cmd/sh
node --version
```

Eseguire il comando seguente per aggiungere l'estensione Microsoft Azure IoT per l'interfaccia della riga di comando di Azure all'istanza di Cloud Shell. L'estensione IoT aggiunge i comandi specifici di hub IoT, IoT Edge e servizio Device Provisioning in hub IoT all'interfaccia della riga di comando di Azure.

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Se non è già stato fatto, scaricare il progetto Node.js di esempio da https://github.com/Azure-Samples/azure-iot-samples-node/archive/master.zip ed estrarre l'archivio ZIP.

## <a name="create-an-iot-hub"></a>Creare un hub IoT

Se è stata completata la precedente [Guida introduttiva: Inviare dati di telemetria da un dispositivo a un hub IoT](quickstart-send-telemetry-node.md), è possibile ignorare questo passaggio.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Registrare un dispositivo

Se è stata completata la precedente [Guida introduttiva: Inviare dati di telemetria da un dispositivo a un hub IoT](quickstart-send-telemetry-node.md), è possibile ignorare questo passaggio.

È necessario registrare un dispositivo con l'hub IoT perché questo possa connettersi. In questa guida introduttiva si usa Azure Cloud Shell per registrare un dispositivo simulato.

1. Eseguire il comando seguente in Azure Cloud Shell per creare l'identità del dispositivo.

   **YourIoTHubName**: sostituire il segnaposto in basso con il nome scelto per l'hub IoT.

   **MyNodeDevice**: nome del dispositivo da registrare. È consigliabile usare **MyNodeDevice**, come illustrato. Se si sceglie un altro nome per il dispositivo, è necessario usare tale nome anche nell'ambito di questo articolo e aggiornare il nome del dispositivo nelle applicazioni di esempio prima di eseguirle.

    ```azurecli-interactive
    az iot hub device-identity create \
      --hub-name {YourIoTHubName} --device-id MyNodeDevice
    ```

2. Eseguire il comando seguente in Azure Cloud Shell per ottenere la _stringa di connessione del dispositivo_ per il dispositivo appena registrato.

    **YourIoTHubName**: sostituire il segnaposto in basso con il nome scelto per l'hub IoT.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string \
      --hub-name {YourIoTHubName} \
      --device-id MyNodeDevice \
      --output table
    ```

    Annotare la stringa di connessione del dispositivo, che avrà questo aspetto:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Il valore verrà usato più avanti in questa guida introduttiva.

3. È necessaria anche una _stringa di connessione del servizio_ per consentire all'applicazione back-end di connettersi all'hub IoT dell'utente e recuperare i messaggi. Il comando seguente recupera la stringa di connessione del servizio per l'hub IoT:

    **YourIoTHubName**: sostituire il segnaposto in basso con il nome scelto per l'hub IoT.

    ```azurecli-interactive
    az iot hub show-connection-string \
      --policy-name service --name {YourIoTHubName} --output table

    ```

    Annotare la stringa di connessione del servizio, che avrà questo aspetto:

   `HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}`

    Il valore verrà usato più avanti in questa guida introduttiva. Questa stringa di connessione del servizio è diversa dalla stringa di connessione del dispositivo annotata nel passaggio precedente.

## <a name="listen-for-direct-method-calls"></a>Ascoltare le chiamate dei metodi diretti

L'applicazione del dispositivo simulato si connette a un endpoint specifico del dispositivo nell'hub IoT, invia dati di telemetria simulati e ascolta le chiamate dei metodi diretti dall'hub. In questa guida introduttiva la chiamata dei metodi diretti dall'hub indica al dispositivo di modificare l'intervallo di invio dei dati di telemetria. Il dispositivo simulato invia un acknowledgement all'hub dopo l'esecuzione del metodo diretto.

1. In una finestra del terminale locale passare alla cartella radice del progetto Node.js di esempio. Passare quindi alla cartella **iot-hub\Quickstarts\simulated-device-2**.

2. Aprire il file **SimulatedDevice.js** in un editor di testo di propria scelta.

    Sostituire il valore della variabile `connectionString` con la stringa di connessione del dispositivo annotata in precedenza. Salvare quindi le modifiche nel file **SimulatedDevice.js**.

3. Nella finestra del terminale locale eseguire i comandi seguenti per installare le librerie necessarie ed eseguire l'applicazione del dispositivo simulato:

    ```cmd/sh
    npm install
    node SimulatedDevice.js
    ```

    La schermata seguente mostra l'output mentre l'applicazione del dispositivo simulato invia i dati di telemetria all'hub IoT:

    ![Eseguire il dispositivo simulato](./media/quickstart-control-device-node/simulated-device-telemetry-iot-hub.png)

## <a name="call-the-direct-method"></a>Chiamare il metodo diretto

L'applicazione back-end si connette a un endpoint sul lato servizio nell'IoT Hub dell'utente. L'applicazione effettua chiamate dei metodi diretti a un dispositivo tramite l'hub IoT e rimane in ascolto degli acknowledgement. Un'applicazione back-end dell'hub IoT in genere viene eseguita nel cloud.

1. In un'altra finestra del terminale locale passare alla cartella radice del progetto Node.js di esempio. Passare quindi alla cartella **iot-hub\Quickstarts\back-end-application**.

2. Aprire il file **BackEndApplication.js** in un editor di testo di propria scelta.

    Sostituire il valore della variabile `connectionString` con la stringa di connessione del servizio annotata in precedenza. Salvare quindi le modifiche nel file **BackEndApplication.js**.

3. Nella finestra del terminale locale eseguire i comandi seguenti per installare le librerie necessarie ed eseguire l'applicazione back-end:

    ```cmd/sh
    npm install
    node BackEndApplication.js
    ```

    La schermata seguente mostra l'output visualizzato quando l'applicazione effettua una chiamata del metodo diretto al dispositivo e riceve un acknowledgement:

    ![Output quando l'applicazione effettua una chiamata del metodo diretto al dispositivo](./media/quickstart-control-device-node/direct-method-device-call.png)

    Dopo l'esecuzione dell'applicazione back-end, viene visualizzato un messaggio nella finestra della console che esegue il dispositivo simulato e cambia la frequenza di invio dei messaggi:

    ![Output in caso di modifica nel client simulato](./media/quickstart-control-device-node/simulated-device-message-change.png)

## <a name="clean-up-resources"></a>Pulire le risorse

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Passaggi successivi

In questa guida introduttiva è stato chiamato un metodo diretto su un dispositivo da un'applicazione back-end ed è stata inviata una risposta alla chiamata del metodo diretto in un'applicazione del dispositivo simulato.

Per informazioni su come indirizzare messaggi da dispositivo a cloud a diverse destinazioni nel cloud, continuare con l'esercitazione successiva.

> [!div class="nextstepaction"]
> [Esercitazione: indirizzare i dati di telemetria a endpoint diversi per l'elaborazione](tutorial-routing.md)
