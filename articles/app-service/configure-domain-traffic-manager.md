---
title: Configurare i nomi DNS con gestione traffico
description: Informazioni su come configurare un dominio personalizzato per un'app di servizio app Azure che si integra con gestione traffico per il bilanciamento del carico.
ms.assetid: 0f96c0e7-0901-489b-a95a-e3b66ca0a1c2
ms.topic: article
ms.date: 03/05/2020
ms.custom: seodec18
ms.openlocfilehash: f8322c12669e41fc7c9aa88e99f95cf1b26ea87d
ms.sourcegitcommit: 8f4d54218f9b3dccc2a701ffcacf608bbcd393a6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/09/2020
ms.locfileid: "78944154"
---
# <a name="configure-a-custom-domain-name-in-azure-app-service-with-traffic-manager-integration"></a>Configurare un nome di dominio personalizzato in app Azure servizio con l'integrazione di gestione traffico

[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

> [!NOTE]
> Per i servizi cloud, vedere [Configurazione di un nome di dominio personalizzato per un servizio cloud di Azure](../cloud-services/cloud-services-custom-domain-name.md).

Quando si usa [Gestione traffico di Azure](/azure/traffic-manager/) per bilanciare il carico del traffico verso il [servizio app Azure](overview.md), è possibile accedere all'app del servizio app usando **\<Traffic-Manager-endpoint >. trafficmanager. NET**. È possibile assegnare un nome di dominio personalizzato, ad esempio www\.contoso.com, con l'app del servizio app in modo da fornire un nome di dominio più riconoscibile per gli utenti.

Questo articolo illustra come configurare un nome di dominio personalizzato con un'app del servizio app integrata con [Gestione traffico](../traffic-manager/traffic-manager-overview.md).

> [!NOTE]
> Quando si configura un nome di dominio tramite l'endpoint di gestione traffico, sono supportati solo i record [CNAME](https://en.wikipedia.org/wiki/CNAME_record) . Poiché i record non sono supportati, anche un mapping del dominio radice, ad esempio contoso.com, non è supportato.
> 

## <a name="prepare-the-app"></a>Preparare l'app

Per eseguire il mapping di un nome DNS personalizzato a un'app integrata con gestione traffico di Azure, il [piano di servizio app](https://azure.microsoft.com/pricing/details/app-service/) dell'app Web deve essere nel livello **standard** o superiore. In questo passaggio si verifica che l'app del servizio app si trovi nel piano tariffario supportato.

### <a name="check-the-pricing-tier"></a>Scegliere il piano tariffario

Nella [portale di Azure](https://portal.azure.com)cercare e selezionare **Servizi app**.

Nella pagina **Servizi app** selezionare il nome dell'app di Azure.

![Passaggio all'app di Azure nel portale](./media/app-service-web-tutorial-custom-domain/select-app.png)

Nella finestra di spostamento a sinistra della pagina dell'app fare clic su **scalabilità verticale (piano di servizio app)** .

![Menu di scalabilità verticale](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

Il livello corrente dell'app è evidenziato da un bordo blu. Verificare che l'app sia nel livello **standard** o superiore (qualsiasi livello nella categoria di **produzione** o **isolata** ). In caso affermativo, chiudere la pagina **scalabilità verticale** e saltare per [creare il mapping CNAME](#create-the-cname-mapping).

![Controllare il piano tariffario](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

### <a name="scale-up-the-app-service-plan"></a>Passare a un piano di servizio app di livello superiore

Se è necessario aumentare le prestazioni dell'app, selezionare uno dei piani tariffari nella categoria **produzione** . Per altre opzioni, fare clic su **Visualizza opzioni aggiuntive**.

Fare clic su **Apply**.

## <a name="create-traffic-manager-endpoint"></a>Crea endpoint di gestione traffico

Seguendo i passaggi descritti in [aggiungere o eliminare endpoint](../traffic-manager/traffic-manager-endpoints.md), aggiungere l'app del servizio app come endpoint nel profilo di gestione traffico.

Quando l'app del servizio app si trova in un piano tariffario supportato, viene visualizzata nell'elenco delle destinazioni del servizio app disponibili quando si aggiunge l'endpoint. Se l'app non è elencata, [verificare il piano tariffario dell'app](#prepare-the-app).

## <a name="create-the-cname-mapping"></a>Creare il mapping CNAME
> [!NOTE]
> Per configurare un [dominio del servizio app acquistato](manage-custom-dns-buy-domain.md), ignorare questa sezione e passare a [Abilita dominio personalizzato](#enable-custom-domain).
> 

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records-no-h.md)]

Sebbene le specifiche di ogni provider di dominio variano, è possibile eseguire il mapping *dal* nome di dominio personalizzato (ad esempio **contoso.com**) *al* nome di dominio di Traffic Manager (**contoso.trafficmanager.NET**) integrato con l'app.

> [!NOTE]
> Se un record è già in uso ed è necessario associare le app in modalità preemptive, è possibile creare un altro record CNAME. Ad esempio, per associare preventivamente **www\.contoso.com** all'app, creare un record CNAME da **awverify. www** a **contoso.trafficmanager.NET**. È quindi possibile aggiungere "www\.contoso.com" all'app senza la necessità di modificare il record CNAME "www". Per altre informazioni, vedere [eseguire la migrazione di un nome DNS attivo al servizio app Azure](manage-custom-dns-migrate-domain.md).

Dopo aver completato l'aggiunta o la modifica di record DNS presso il provider di dominio, salvare le modifiche.

## <a name="enable-custom-domain"></a>Abilita dominio personalizzato
Dopo la propagazione dei record per il nome di dominio, usare il browser per verificare che il nome di dominio personalizzato venga risolto nell'app del servizio app.

> [!NOTE]
> La propagazione del record CNAME in tutto il sistema DNS può richiedere tempo. È possibile usare un servizio come <a href="https://www.digwebinterface.com/">https://www.digwebinterface.com/</a> per verificare se il record CNAME è disponibile.
> 
> 

1. Una volta completata la risoluzione del dominio, tornare alla pagina dell'app nel [portale di Azure](https://portal.azure.com)
2. Nel percorso di spostamento a sinistra selezionare **domini personalizzati** > **Aggiungi nome host**.
4. Digitare il nome di dominio personalizzato di cui è stato eseguito il mapping in precedenza e selezionare **convalida**.
5. Assicurarsi che **Tipo di record del nome host** sia impostato su **CNAME (www\.example.com o qualsiasi sottodominio**).

6. Poiché l'app del servizio app è ora integrata con un endpoint di gestione traffico, in **configurazione CNAME**dovrebbe essere visualizzato il nome di dominio di gestione traffico. Selezionarlo e fare clic su **Aggiungi dominio personalizzato**.

    ![Aggiunta del nome DNS all'app](./media/configure-domain-traffic-manager/enable-traffic-manager-domain.png)

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Proteggere un nome DNS personalizzato con un binding SSL nel Servizio app di Azure](configure-ssl-bindings.md)
