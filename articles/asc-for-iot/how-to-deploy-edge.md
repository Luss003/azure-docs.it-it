---
title: Distribuire il Centro sicurezza di Azure per il modulo IoT Edge | Microsoft Docs
description: Informazioni su come distribuire un centro sicurezza di Azure per l'agente di sicurezza Internet in IoT Edge.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 32a9564d-16fd-4b0d-9618-7d78d614ce76
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/30/2020
ms.author: mlottner
ms.openlocfilehash: b2af392dc4dc848a099b8297bb58e7d4a7104fa6
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2020
ms.locfileid: "76964040"
---
# <a name="deploy-a-security-module-on-your-iot-edge-device"></a>Distribuire un modulo di sicurezza nel dispositivo IoT Edge


Il modulo **Centro sicurezza di Azure** offre una soluzione di sicurezza completa per i dispositivi IOT Edge.
Il modulo Security raccoglie, aggrega e analizza i dati di sicurezza non elaborati dal sistema operativo e dal sistema del contenitore in consigli e avvisi di sicurezza di utilità pratica.
Per altre informazioni, vedere [modulo di sicurezza per IOT Edge](security-edge-architecture.md).

In questo articolo si apprenderà come distribuire un modulo di sicurezza sul dispositivo IoT Edge.

## <a name="deploy-security-module"></a>Distribuisci modulo di sicurezza

Usare la procedura seguente per distribuire un centro sicurezza di Azure per il modulo di sicurezza Internet per IoT Edge.

### <a name="prerequisites"></a>Prerequisiti

1. Nell'hub Internet delle cose assicurarsi che il dispositivo sia [registrato come dispositivo IOT Edge](https://docs.microsoft.com/azure/iot-edge/how-to-register-device-portal).

1. Il Centro sicurezza di Azure per il modulo IoT Edge richiede che il [Framework controllato](https://linux.die.net/man/8/auditd) sia installato nel dispositivo IOT Edge.

    - Installare il Framework eseguendo il comando seguente sul dispositivo IoT Edge:
   
    `sudo apt-get install auditd audispd-plugins`

    - Verificare che il controllo sia attivo eseguendo il comando seguente: 
   
    `sudo systemctl status auditd`<br>
    - La risposta prevista è: `active (running)` 
        

### <a name="deployment-using-azure-portal"></a>Distribuzione con portale di Azure

1. Dal portale di Azure aprire **Marketplace**.

1. Selezionare **Internet delle cose**, quindi cercare il **Centro sicurezza di Azure** e selezionarlo.

   ![Selezionare il Centro sicurezza di Azure per le cose](media/howto/edge-onboarding-8.png)

1. Fare clic su **Crea** per configurare la distribuzione. 

1. Scegliere la **sottoscrizione** di Azure dell'hub Internet delle cose, quindi selezionare l' **Hub**Internet.<br>Selezionare **Distribuisci in un dispositivo** per fare riferimento a un singolo dispositivo o selezionare **Distribuisci su scala** per fare riferimento a più dispositivi, quindi fare clic su **Crea**. Per ulteriori informazioni sulla distribuzione su larga scala, vedere [How to deploy](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-monitor). 

    >[!Note] 
    >Se è stata selezionata l'opzione **Distribuisci su scala**, aggiungere il nome e i dettagli del dispositivo prima di continuare con la scheda **Aggiungi moduli** nelle istruzioni seguenti.     

Completare ogni passaggio per completare la distribuzione di IoT Edge per il Centro sicurezza di Azure. 

#### <a name="step-1-modules"></a>Passaggio 1: moduli

1. Selezionare il modulo **AzureSecurityCenterforIoT** .
1. Nella scheda **Impostazioni modulo** modificare il **nome** in **azureiotsecurity**.
1. Nella scheda **variabili ambiente** aggiungere una variabile, se necessario (ad esempio, livello di debug).
1. Nella scheda **Crea opzioni del contenitore** aggiungere la configurazione seguente:

    ``` json
    {
        "NetworkingConfig": {
            "EndpointsConfig": {
                "host": {}
            }
        },
        "HostConfig": {
            "Privileged": true,
            "NetworkMode": "host",
            "PidMode": "host",
            "Binds": [
                "/:/host"
            ]
        }
    }    
    ```
    
1. Nella scheda **Impostazioni gemelli del modulo** aggiungere la configurazione seguente:
      
    ``` json
      "ms_iotn:urn_azureiot_Security_SecurityAgentConfiguration":{}
    ```

1. Selezionare **Aggiorna**.

#### <a name="step-2-runtime-settings"></a>Passaggio 2: impostazioni di runtime

1. Selezionare **le impostazioni di runtime**.
1. In **Hub Edge**modificare l' **immagine** in **MCR.Microsoft.com/azureiotedge-Hub:1.0.8.3**.
1. Verificare che **Crea opzioni** sia impostato sulla configurazione seguente: 
         
    ``` json
    { 
       "HostConfig":{ 
          "PortBindings":{ 
             "8883/tcp":[ 
                { 
                   "HostPort":"8883"
                }
             ],
             "443/tcp":[ 
                { 
                   "HostPort":"443"
                }
             ],
             "5671/tcp":[ 
                { 
                   "HostPort":"5671"
                }
             ]
          }
       }
    }
    ```
    
1. Selezionare **Salva**.
   
1. Selezionare **Avanti**.

#### <a name="step-3-specify-routes"></a>Passaggio 3: specificare le route 

1. Nella scheda **specificare le route** verificare che sia presente una route (esplicita o implicita) che inoltra i messaggi dal modulo **azureiotsecurity** a **$upstream** in base agli esempi seguenti. Selezionare **Avanti**solo quando è attiva la route.

   Route di esempio:

    ~~~Default implicit route
    "route": "FROM /messages/* INTO $upstream" 
    ~~~

    ~~~Explicit route
    "ASCForIoTRoute": "FROM /messages/modules/azureiotsecurity/* INTO $upstream"
    ~~~

1. Selezionare **Avanti**.

#### <a name="step-4-review-deployment"></a>Passaggio 4: esaminare la distribuzione

- Nella scheda **Verifica distribuzione** esaminare le informazioni di distribuzione e quindi selezionare **Crea** per completare la distribuzione.

## <a name="diagnostic-steps"></a>Procedure di diagnostica

Se si verifica un problema, i log del contenitore rappresentano il modo migliore per ottenere informazioni sullo stato di un dispositivo IoT Edge Security Module. Usare i comandi e gli strumenti illustrati in questa sezione per raccogliere informazioni.

### <a name="verify-the-required-containers-are-installed-and-functioning-as-expected"></a>Verificare che i contenitori necessari siano installati e funzionino come previsto

1. Eseguire il comando seguente sul dispositivo IoT Edge:
    
    `sudo docker ps`
   
1. Verificare che siano in esecuzione i seguenti contenitori:
   
   | Nome | IMAGE |
   | --- | --- |
   | azureiotsecurity | mcr.microsoft.com/ascforiot/azureiotsecurity:1.0.2 |
   | edgeHub | mcr.microsoft.com/azureiotedge-hub:1.0.8.3 |
   | edgeAgent | mcr.microsoft.com/azureiotedge-agent:1.0.1 |
   
   Se non sono presenti i contenitori minimi necessari, controllare se il manifesto di distribuzione IoT Edge è allineato con le impostazioni consigliate. Per altre informazioni, vedere [Deploy IOT Edge Module](#deployment-using-azure-portal).

### <a name="inspect-the-module-logs-for-errors"></a>Esaminare i registri dei moduli per individuare eventuali errori
   
1. Eseguire il comando seguente sul dispositivo IoT Edge:

   `sudo docker logs azureiotsecurity`
   
1. Per log più dettagliati, aggiungere la variabile di ambiente seguente alla distribuzione del modulo **azureiotsecurity** : `logLevel=Debug`.

## <a name="next-steps"></a>Passaggi successivi

Per ulteriori informazioni sulle opzioni di configurazione, passare alla Guida alle procedure per la configurazione del modulo. 
> [!div class="nextstepaction"]
> [Guida alle procedure di configurazione del modulo](./how-to-agent-configuration.md)
