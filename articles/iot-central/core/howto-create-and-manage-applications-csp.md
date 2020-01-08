---
title: Creare e gestire applicazioni Azure IoT Central dal portale CSP | Microsoft Docs
description: Come creare, in qualità di CSP, un'applicazione IoT Central di Azure per conto del cliente.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 08/23/2019
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: 0e49a5c8edd074c71d5972ee8d9c2e81f9c512ea
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75453953"
---
# <a name="create-and-manage-an-azure-iot-central-application-from-the-csp-portal"></a>Creare e gestire un'applicazione IoT Central di Azure dal portale CSP

Il programma Microsoft Cloud Solution Provider (CSP) è un programma Microsoft Reseller. La finalità è quella di fornire ai partner di canale un programma centralizzato per rivendere tutti i servizi Online commerciali di Microsoft. Altre informazioni sul [programma Cloud Solution Provider](https://partner.microsoft.com/cloud-solution-provider).

In qualità di CSP, è possibile creare e gestire le applicazioni IoT Central si Microsoft Azure per conto dei clienti tramite il [Centro per i partner Microsoft](https://partnercenter.microsoft.com/partner/home). Quando le applicazioni IoT Central di Azure vengono create per conto dei clienti dai CSP, proprio come con altri servizi di Azure gestiti dai CSP, questi gestiscono la fatturazione per i clienti. Un addebito per IoT Central di Azure verrà inserito nella fattura totale nel Centro per i partner Microsoft.

Per iniziare, accedere al proprio account nel portale dei partner Microsoft e selezionare un cliente per il quale si desidera creare un'applicazione IoT Central di Azure. Passare a gestione servizi per il cliente dal NAV di sinistra.

![Centro per i partner Microsoft, vista clienti](media/howto-create-application-csp/image1.png)

IoT Central di Azure è elencato come un servizio disponibile per l'amministrazione. Selezionare il collegamento IoT Central di Azure nella pagina per creare nuove applicazioni o gestire le applicazioni esistenti per questo cliente.

![Applicazioni IoT Central di Azure disponibili per la gestione](media/howto-create-application-csp/image2.png)

Passare alla pagina Application Manager di Azure IoT Central. Azure IoT Central mantiene il contesto di provenienza dal Centro per i partner Microsoft e l'informazione che l'utente sta gestendo quel cliente specifico. Si noterà questa informazione nell'intestazione della pagina Application Manager. Da qui è possibile passare a un'applicazione esistente già creata per il cliente per gestirla o creare una nuova applicazione.

![Create Manager (Crea manager) per CSP](media/howto-create-application-csp/image3.png)

Per creare un'applicazione IoT Central di Azure, selezionare **Compila** nel menu a sinistra. Scegliere uno dei modelli di settore o scegliere **applicazione legacy** per creare un'applicazione da zero. Verrà caricata la pagina Application Creation (Creazione applicazione). Completare tutti i campi in questa pagina e quindi scegliere **Create** (Crea). Vengono fornite maggiori informazioni su ognuno dei campi qui di seguito.

![La pagina Create Application (Crea applicazione) per CSP](media/howto-create-application-csp/image4.png)

![La pagina Create Application (Crea applicazione) per CSP](media/howto-create-application-csp/image4-1.png)

## <a name="payment-plan"></a>Piano di pagamento

In qualità di CSP è possibile creare solo applicazioni con pagamento in base al consumo. Per presentare IoT Central di Azure al cliente è possibile creare separatamente un'applicazione di prova. Altre informazioni sulle applicazioni in versione di prova e con pagamento in base al consumo sono disponibili nella [pagina Prezzi di IoT Central](https://azure.microsoft.com/pricing/details/iot-central/).

## <a name="application-name"></a>Nome dell'applicazione

Il nome dell'applicazione è mostrato sulla pagina **Application Manager** e in ogni applicazione Azure IoT Central. È possibile scegliere qualsiasi nome per l'applicazione Azure IoT Central. Scegliere un nome significativo per sé e per tutte le persone dell'organizzazione.

## <a name="application-url"></a>URL applicazione

L'URL dell'applicazione è il collegamento all'applicazione. È possibile salvare un segnalibro nel browser o condividerlo con altri utenti.

Quando si immette il nome dell'applicazione, l'URL dell'applicazione viene generato automaticamente. Se si preferisce, è possibile scegliere un URL diverso per l'applicazione. Ogni URL di Azure IoT Central deve essere univoco all'interno di Azure IoT Central. Se l'URL scelto è già in uso, compare un messaggio di errore.

## <a name="directory"></a>Directory

Poiché Azure IoT Central dispone del contesto da cui l'utente proviene per gestire il cliente selezionato nel portale dei Partner Microsoft, nel campo Directory viene visualizzato solo il tenant di Azure Active Directory per quel cliente. 

Un tenant di Azure Active Directory contiene le identità degli utenti, le credenziali e altre informazioni sull'organizzazione. Più sottoscrizioni di Azure possono essere associate a un singolo tenant di Azure Active Directory.

Per altre informazioni, vedere [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/).

## <a name="azure-subscription"></a>Sottoscrizione di Azure

Una sottoscrizione di Azure consente di creare istanze dei servizi di Azure. Azure IoT Central rileva automaticamente tutte le sottoscrizioni di Azure del cliente a cui è possibile accedere e le mostra in un elenco a discesa nella pagina **Create Application** (Crea applicazione). Scegliere una sottoscrizione di Azure per creare una nuova applicazione Azure IoT Central.

Se non si ha una sottoscrizione di Azure, è possibile crearne una nel Centro per i partner Microsoft. Dopo aver creato la sottoscrizione di Azure, passare alla pagina **Create Application** (Crea applicazione). Selezionare la sottoscrizione nella casella di riepilogo a discesa **Azure Subscription** (Sottoscrizione di Azure).

Per altre informazioni, vedere la documentazione sulle [sottoscrizioni di Azure](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing).

## <a name="region"></a>Area

Scegliere la regione o l'area [geografica](https://azure.microsoft.com/global-infrastructure/geographies/) in cui si vuole creare l'applicazione IoT Central di Azure. In genere, è consigliabile scegliere l'area più vicina fisicamente ai dispositivi per ottenere prestazioni ottimali.

> [!NOTE]
> I modelli di applicazione di anteprima sono attualmente disponibili solo nelle località **Europa** e **Stati Uniti** .

Per altre informazioni, vedere [aree di Azure](https://azure.microsoft.com/global-infrastructure/regions/) e [geografie di Azure](https://azure.microsoft.com/global-infrastructure/geographies/).

È possibile vedere le aree in cui Azure IoT Central è disponibile nella pagina [Prodotti disponibili in base all'area](https://azure.microsoft.com/global-infrastructure/services/?products=iot-central).

> [!Note]
> Dopo aver scelto un'area, è possibile spostare l'applicazione in un'altra area geografica in un secondo momento.

## <a name="application-template"></a>Modello di applicazione

È possibile scegliere il modello di applicazione seguente per la nuova applicazione IoT Central di Azure.

| Modello di applicazione | Description |
| -------------------- | ----------- |
| Applicazione legacy   | Crea un'applicazione vuota per l'utente da popolare con i propri modelli di dispositivi e dispositivi. |


## <a name="next-steps"></a>Passaggi successivi

Ora che si conosce la procedura per creare un'applicazione Azure IoT Central in qualità di CSP, il prossimo passo suggerito è:

> [!div class="nextstepaction"]
> [Amministrare l'applicazione](howto-administer.md)
