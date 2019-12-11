---
title: Gestire il servizio Device provisioning in hub Internet usando l'interfaccia della riga di comando di Azure & estensione Internet
description: Informazioni su come usare l'interfaccia della riga di comando di Azure e l'estensione Internet per gestire il servizio Device provisioning in hub Internet (DPS)
author: chrissie926
ms.author: menchi
ms.date: 01/17/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: 0ba92279632a7283ea6ede423e808e3c7be82cff
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/10/2019
ms.locfileid: "74975160"
---
# <a name="how-to-use-azure-cli-and-the-iot-extension-to-manage-the-iot-hub-device-provisioning-service"></a>Come usare l'interfaccia della riga di comando di Azure e l'estensione IoT per gestire i servizi Device Provisioning in hub IoT

L'[interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) è uno strumento da riga di comando multipiattaforma e open source per la gestione di risorse di Azure come IoT Edge. L'interfaccia della riga di comando di Azure è disponibile su Windows, Linux e MacOS. L'interfaccia della riga di comando di Azure consente di gestire le risorse dell'hub IoT di Azure, le istanze del servizio Device Provisioning e gli hub collegati predefiniti.

L'estensione IoT arricchisce l'interfaccia della riga di comando di Azure con funzionalità quali la gestione dei dispositivi e le funzionalità complete di IoT Edge.

In questa esercitazione viene completata prima di tutto la procedura per la configurazione dell'interfaccia della riga di comando di Azure e dell'estensione IoT. Viene quindi illustrato come eseguire i comandi dell'interfaccia della riga di comando per eseguire operazioni di base del servizio Device Provisioning. 

## <a name="installation"></a>Installazione 

### <a name="step-1---install-python"></a>Passaggio 1 - Installare Python

È necessario [Python 2.7x o Python 3.x](https://www.python.org/downloads/).

### <a name="step-2---install-the-azure-cli"></a>Passaggio 2 - Installare l'interfaccia della riga di comando di Azure

Seguire le [istruzioni di installazione](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) per configurare l'interfaccia della riga di comando di Azure nell'ambiente. La versione dell'interfaccia della riga di comando di Azure deve essere 2.0.24 o successiva. Usare `az –version` per la convalida. Questa versione supporta i comandi dell'estensione az e introduce il framework dei comandi Knack. Un approccio semplice per l'installazione su Windows consiste nel scaricare e installare [MSI](https://aka.ms/InstallAzureCliWindows).

### <a name="step-3---install-iot-extension"></a>Passaggio 3 - Installare l'estensione IoT

Il [file Leggimi dell'estensione IoT](https://github.com/Azure/azure-iot-cli-extension) illustra molti modi per installare l'estensione. L'approccio più semplice consiste nell'eseguire `az extension add --name azure-cli-iot-ext`. Dopo l'installazione, è possibile usare `az extension list` per convalidare le estensioni attualmente installate o `az extension show --name azure-cli-iot-ext` per visualizzare informazioni dettagliate sull'estensione IoT. Per rimuovere l'estensione, è possibile usare `az extension remove --name azure-cli-iot-ext`.


## <a name="basic-device-provisioning-service-operations"></a>Operazioni di base del servizio Device Provisioning
Questo esempio illustra come accedere all'account Azure, creare un gruppo di risorse di Azure (un contenitore che include risorse correlate per una soluzione di Azure), creare un hub IoT, creare un servizio Device Provisioning, elencare i servizi Device Provisioning esistenti e creare un hub IoT collegato con i comandi dell'interfaccia della riga di comando. 

Prima di iniziare, completare la procedura di installazione illustrata in precedenza. Se non si ha un account Azure, è possibile [creare un account gratuito](https://azure.microsoft.com/free/?v=17.39a) subito. 


### <a name="1-log-in-to-the-azure-account"></a>1. accedere all'account Azure
  
    az login

![Accesso][1]

### <a name="2-create-a-resource-group-iothubblogdemo-in-eastus"></a>2. creare un gruppo di risorse IoTHubBlogDemo in eastus

    az group create -l eastus -n IoTHubBlogDemo

![Creare un gruppo di risorse][2]


### <a name="3-create-two-device-provisioning-services"></a>3. creare due servizi Device provisioning

    az iot dps create --resource-group IoTHubBlogDemo --name demodps

![Creare servizi Device Provisioning][3]

    az iot dps create --resource-group IoTHubBlogDemo --name demodps2

### <a name="4-list-all-the-existing-device-provisioning-services-under-this-resource-group"></a>4. elencare tutti i servizi Device provisioning esistenti in questo gruppo di risorse

    az iot dps list --resource-group IoTHubBlogDemo

![Elenco dei servizi Device Provisioning][4]


### <a name="5-create-an-iot-hub-blogdemohub-under-the-newly-created-resource-group"></a>5. creare un hub blogDemoHub nel gruppo di risorse appena creato

    az iot hub create --name blogDemoHub --resource-group IoTHubBlogDemo

![Creare un hub IoT][5]

### <a name="6-link-one-existing-iot-hub-to-a-device-provisioning-service"></a>6. Collegare un hub Internet esistente a un servizio Device provisioning

    az iot dps linked-hub create --resource-group IoTHubBlogDemo --dps-name demodps --connection-string <connection string> -l westus

![Collegare l'hub][5]

<!-- Images -->
[1]: ./media/how-to-manage-dps-with-cli/login.jpg
[2]: ./media/how-to-manage-dps-with-cli/create-resource-group.jpg
[3]: ./media/how-to-manage-dps-with-cli/create-dps.jpg
[4]: ./media/how-to-manage-dps-with-cli/list-dps.jpg
[5]: ./media/how-to-manage-dps-with-cli/create-hub.jpg
[6]: ./media/how-to-manage-dps-with-cli/link-hub.jpg


## <a name="next-steps"></a>Passaggi successivi
Questa esercitazione ha illustrato come:

> [!div class="checklist"]
> * Registrare il dispositivo
> * Avviare il dispositivo
> * Verificare che il dispositivo sia registrato

Passare all'esercitazione successiva per informazioni su come eseguire il provisioning di più dispositivi tra hub con bilanciamento del carico. 

> [!div class="nextstepaction"]
> [Eseguire il provisioning dei dispositivi in più hub IoT con bilanciamento del carico](./tutorial-provision-multiple-hubs.md)
