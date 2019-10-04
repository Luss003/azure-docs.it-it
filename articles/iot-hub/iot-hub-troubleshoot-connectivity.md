---
title: Diagnosticare e risolvere i problemi di disconnessione per l'hub IoT di Azure
description: Informazioni su come diagnosticare e risolvere gli errori comuni di connettività dei dispositivi per l'hub IoT di Azure
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/19/2018
ms.author: jlian
ms.openlocfilehash: 3904c6390cfe8de197bae470c4ae32d22605ae6a
ms.sourcegitcommit: b7b0d9f25418b78e1ae562c525e7d7412fcc7ba0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/08/2019
ms.locfileid: "70801421"
---
# <a name="detect-and-troubleshoot-disconnects-with-azure-iot-hub"></a>Rilevare e risolvere i problemi di disconnessione per l'hub IoT di Azure

I problemi di connettività per i dispositivi IoT possono essere difficili da risolvere poiché sono presenti diversi possibili punti di errore. La logica dell'applicazione sul lato del dispositivo, le reti fisiche, i protocolli, l'hardware e l'hub IoT di Azure: tutti questi elementi possono causare problemi. Questo articolo fornisce suggerimenti su come rilevare e risolvere i problemi di connettività dei dispositivi sul lato cloud (anziché sul lato del dispositivo).

## <a name="get-alerts-and-error-logs"></a>Ottenere avvisi e log degli errori

Usare Monitoraggio di Azure per ricevere avvisi e scrivere i log in caso di interruzione delle connessioni dei dispositivi.

### <a name="turn-on-diagnostic-logs"></a>Attivare i log di diagnostica

Per registrare gli eventi e gli errori di connessione dei dispositivi, attivare la diagnostica per l'hub IoT.

1. Accedere al [portale di Azure](https://portal.azure.com).

2. Passare all'hub IoT.

3. Selezionare **Impostazioni di diagnostica**.

4. Selezionare **Attiva diagnostica**.

5. Abilitare la raccolta dei log **Connessioni**.

6. Per semplificare l'analisi, è necessario attivare **Invia a Log Analytics** ([vedere i prezzi](https://azure.microsoft.com/pricing/details/log-analytics/)). Vedere l'esempio sotto [Risolvere gli errori di connettività](#resolve-connectivity-errors).

   ![Impostazioni consigliate](./media/iot-hub-troubleshoot-connectivity/diagnostic-settings-recommendation.png)

Per altre informazioni, vedere [Monitorare l'integrità dell'hub IoT di Azure ed eseguire la diagnostica rapida dei problemi](iot-hub-monitor-resource-health.md).

### <a name="set-up-alerts-for-the-_connected-devices_-count-metric"></a>Impostare avvisi per la metrica di conteggio dei _dispositivi connessi_

Per ottenere gli avvisi quando i dispositivi si disconnettono, configurare gli avvisi per la metrica **dispositivi connessi (anteprima)** .

1. Accedere al [portale di Azure](https://portal.azure.com).

2. Passare all'hub IoT.

3. Selezionare **Avvisi**.

4. Selezionare **nuova regola di avviso**.

5. Selezionare **Aggiungi condizione**, quindi selezionare "dispositivi connessi (anteprima)".

6. Completare la configurazione delle soglie desiderate e delle opzioni di avviso seguendo le istruzioni.

Per altre informazioni, vedere [Cosa sono gli avvisi classici in Microsoft Azure?](../azure-monitor/platform/alerts-overview.md).

## <a name="resolve-connectivity-errors"></a>Risolvere gli errori di connettività

Quando si attivano i log di diagnostica e gli avvisi per i dispositivi connessi, si ricevono avvisi in caso di problemi. In questa sezione viene descritto come risolvere i problemi comuni quando si riceve un avviso. La procedura seguente presuppone che siano stati configurati i log di monitoraggio di Azure per i log di diagnostica.

1. Passare all'area di lavoro di **Log Analytics** nel portale di Azure.

2. Selezionare **Ricerca log**.

3. Per isolare i log degli errori di connettività per l'hub IoT, immettere la query seguente e selezionare **Esegui**:

    ```kusto
    search *
    | where ( Type == "AzureDiagnostics" and ResourceType == "IOTHUBS")
    | where ( Category == "Connections" and Level == "Error")
    ```

1. Se sono presenti risultati, cercare `OperationName`, `ResultType` (codice di errore) e `ResultDescription` (messaggio di errore) per ottenere altri dettagli sull'errore.

   ![Esempio di log degli errori](./media/iot-hub-troubleshoot-connectivity/diag-logs.png)

2. Usare questa tabella per comprendere e risolvere gli errori comuni.

    | Errore | Causa radice | Risoluzione |
    |-------|------------|------------|
    | 404104 DeviceConnectionClosedRemotely | La connessione è stata chiusa dal dispositivo, ma l'hub IoT non è in grado di determinare il motivo. Le cause più comuni includono il timeout MQTT/AMQP e la perdita di connettività Internet. | Assicurarsi che il dispositivo possa connettersi all'hub IoT [eseguendo il test della connessione](tutorial-connectivity.md). Se la connessione viene stabilita ma il dispositivo si disconnette in modo intermittente, assicurarsi di implementare per i dispositivi la logica keep-alive appropriata per il protocollo scelto (MQTT/AMPQ). |
    | 401003 IoTHubUnauthorized | L'hub IoT non è riuscito ad autenticare la connessione. | Assicurarsi che la firma di accesso condiviso o altri token di sicurezza in uso non siano scaduti. Gli [Azure IoT SDK](iot-hub-devguide-sdks.md) generano automaticamente i token senza richiedere una configurazione speciale. |
    | 409002 LinkCreationConflict | Un dispositivo ha più di una connessione. Quando arriva una nuova richiesta di connessione per un dispositivo, l'hub IoT chiude quella precedente con questo errore. | Nel caso più comune, un dispositivo rileva una disconnessione e tenta di ristabilire la connessione, ma per l'hub IoT non è ancora disconnesso. L'hub IoT chiude la connessione precedente e registra questo errore. In genere, questo errore si verifica come effetto collaterale di un altro problema temporaneo. Per risolvere il problema, cercare l'errore nei log. In alternativa, assicurarsi di generare una nuova richiesta di connessione solo se la connessione viene interrotta. |
    | 500001 ServerError | Si è verificato un problema sul lato server nell'hub IoT. Molto probabilmente, si tratta di un problema temporaneo. Anche se il team dell'hub IoT si impegna costantemente per il mantenimento del [contratto di servizio](https://azure.microsoft.com/support/legal/sla/iot-hub/), in piccoli subset di nodi dell'hub IoT possono occasionalmente verificarsi errori temporanei. Quando il dispositivo tenta di connettersi a un nodo in cui si sono verificati problemi, viene visualizzato questo errore. | Per attenuare il problema degli errori temporanei, eseguire un nuovo tentativo dal dispositivo. Per [gestire automaticamente i tentativi](iot-hub-reliability-features-in-sdks.md#connection-and-retry), assicurarsi di usare la versione più recente degli [Azure IoT SDK](iot-hub-devguide-sdks.md).<br><br>Per le procedure consigliate sulla gestione degli errori temporanei e la ripetizione dei tentativi, vedere [Gestione degli errori temporanei](/azure/architecture/best-practices/transient-faults).  <br><br>Se il problema persiste dopo la ripetizione dei tentativi, controllare [Integrità risorse](iot-hub-monitor-resource-health.md#use-azure-resource-health) e [Stato di Azure](https://azure.microsoft.com/status/history/) per verificare se nell'hub IoT si è verificato un problema noto. Se non sono presenti problemi noti e il problema persiste, [contattare il supporto tecnico](https://azure.microsoft.com/support/options/) per un'analisi più approfondita. |
    | 500008 GenericTimeout | L'hub IoT non è riuscito a completare la richiesta di connessione prima del timeout. Come 500001 ServerError, questo errore è probabilmente temporaneo. | Per individuare la causa radice e risolvere questo errore, attenersi alla procedura di risoluzione dei problemi per l'errore 500001 ServerError.|

## <a name="other-steps-to-try"></a>Altri passaggi da provare

Se i passaggi precedenti non consentono di risolvere il problema, è possibile provare le soluzioni seguenti:

* Se si ha accesso ai dispositivi che presentano problemi, fisicamente o in modalità remota (ad esempio, tramite SSH), seguire la [guida alla risoluzione dei problemi sul lato del dispositivo](https://github.com/Azure/azure-iot-sdk-node/wiki/Troubleshooting-Guide-Devices) per continuare la risoluzione dei problemi.

* Verificare che i dispositivi siano **abilitati** nel portale di Azure > Hub IoT > Dispositivi IoT.

* Ottenere assistenza dal [forum dedicato all'hub IoT di Azure](https://social.msdn.microsoft.com/Forums/azure/home?forum=azureiothub), da [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-iot-hub) o dal [supporto tecnico di Azure](https://azure.microsoft.com/support/options/).

Se questa guida non è stata utile, lasciare un commento nella sezione dei commenti seguente per contribuire a migliorare la documentazione per tutti gli utenti.

## <a name="next-steps"></a>Passaggi successivi

* Per informazioni sulla risoluzione dei problemi temporanei, vedere [Gestione degli errori temporanei](/azure/architecture/best-practices/transient-faults).

* Per informazioni su Azure IoT SDK e sulla gestione della ripetizione di tentativi, vedere [Come gestire le funzionalità di connettività e messaggistica affidabile con gli SDK per dispositivi dell'hub IoT di Azure](iot-hub-reliability-features-in-sdks.md#connection-and-retry).
