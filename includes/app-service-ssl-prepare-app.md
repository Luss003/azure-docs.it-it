---
title: File di inclusione
description: File di inclusione
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 10/15/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: ecdd419331c88e712644851f9213861f882cf0f6
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/18/2019
ms.locfileid: "67180307"
---
## <a name="prepare-your-web-app"></a>Preparare l'app Web

Per associare un certificato SSL personalizzato (un certificato di terze parti o un certificato di servizio app) all'app Web, il [piano di servizio app](https://azure.microsoft.com/pricing/details/app-service/) in uso deve essere di livello **Basic**, **Standard**, **Premium** o **Isolato**. In questo passaggio si verifica che l'app Web sia supportata dal piano tariffario adeguato.

### <a name="log-in-to-azure"></a>Accedere ad Azure

Aprire il [portale di Azure](https://portal.azure.com).

### <a name="navigate-to-your-web-app"></a>Accedere all'app Web

Nel menu a sinistra fare clic su **Servizi app** e quindi sul nome dell'app Web.

![Selezionare l'app Web](./media/app-service-ssl-prepare-app/select-app.png)

Viene visualizzata la pagina di gestione dell'app Web.  

### <a name="check-the-pricing-tier"></a>Scegliere il piano tariffario

Nel riquadro di spostamento a sinistra della pagina dell'app Web scorrere fino alla sezione **Impostazioni** e selezionare **Scala verticalmente (piano di servizio app)** .

![Menu di scalabilità verticale](./media/app-service-ssl-prepare-app/scale-up-menu.png)

Verificare che l'app Web non sia inclusa nel livello **F1** o **D1**. Il livello corrente dell'app Web è evidenziato da una casella blu scuro.

![Controllare il piano tariffario](./media/app-service-ssl-prepare-app/check-pricing-tier.png)

Il certificato SSL personalizzato non è supportato nel livello **F1** o **D1**. Se è necessario passare a un livello superiore, seguire la procedura della sezione successiva. Altrimenti, chiudere la pagina **Aumenta** e ignorare la sezione [Passare a un piano di servizio app superiore](#scale-up-your-app-service-plan) sezione.

### <a name="scale-up-your-app-service-plan"></a>Passare a un piano di servizio app superiore

Selezionare uno dei livelli non gratuiti (**B1**, **B2**, **B3** o uno qualsiasi dei livelli della categoria **Produzione**). Per altre opzioni, fare clic su **Visualizza opzioni aggiuntive**.

Fare clic su **Apply**.

![Scegliere un piano tariffario](./media/app-service-ssl-prepare-app/choose-pricing-tier.png)

La visualizzazione della notifica seguente indica che l'operazione di passaggio al livello superiore è stata completata.

![Notifica di passaggio al livello superiore](./media/app-service-ssl-prepare-app/scale-notification.png)

