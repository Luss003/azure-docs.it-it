---
title: Configurare un servizio QnA Maker-QnA Maker
description: Prima di poter creare una Knowledge Base di QnA Maker, è necessario configurare un servizio QnA Maker in Azure. Qualsiasi utente autorizzato a creare nuove risorse in una sottoscrizione può configurare un servizio QnA Maker.
ms.topic: conceptual
ms.date: 01/28/2020
ms.openlocfilehash: 663cbce0e096c6189d97cf7872d466383d272f06
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2020
ms.locfileid: "79220589"
---
# <a name="manage-qna-maker-resources"></a>Gestisci risorse QnA Maker

Prima di poter creare una Knowledge Base di QnA Maker, è necessario configurare un servizio QnA Maker in Azure. Qualsiasi utente autorizzato a creare nuove risorse in una sottoscrizione può configurare un servizio QnA Maker.

Una conoscenza approfondita dei concetti seguenti è utile prima di creare la risorsa:

* [Risorse QnA Maker](../Concepts/azure-resources.md)
* [Creazione e pubblicazione di chiavi](../Concepts/azure-resources.md#keys-in-qna-maker)

## <a name="create-a-new-qna-maker-service"></a>Creare un nuovo servizio QnA Maker

Questa procedura consente di creare le risorse di Azure necessarie per gestire il contenuto della Knowledge base. Dopo aver completato questi passaggi, le chiavi della _sottoscrizione_ sono disponibili nella pagina **chiavi** della risorsa nel portale di Azure.

1. Accedere al portale di Azure e [creare una risorsa QnA Maker](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) .

1. Selezionare **Crea** dopo aver letto i termini e le condizioni:

    ![Creare un nuovo servizio QnA Maker](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

1. In **QnA Maker**selezionare i livelli e le aree appropriati:

    ![Creare un nuovo servizio QnA Maker - Piano tariffario e aree](../media/qnamaker-how-to-setup-service/enter-qnamaker-info.png)

    * Nel campo **nome** immettere un nome univoco per identificare questo servizio QnA Maker. Questo nome identifica anche l'endpoint QnA Maker a cui verranno associate le Knowledge base.
    * Scegliere la **sottoscrizione** in cui verrà distribuita la risorsa QnA Maker.
    * Selezionare il piano **tariffario** per i servizi di gestione QnA Maker (portale e API di gestione). Vedere [altri dettagli sui prezzi degli SKU](https://aka.ms/qnamaker-pricing).
    * Creare un nuovo **gruppo di risorse** (scelta consigliata) o utilizzarne uno esistente in cui distribuire questa risorsa QnA Maker. QnA Maker crea diverse risorse di Azure. Quando si crea un gruppo di risorse che contiene queste risorse, è possibile trovare, gestire ed eliminare facilmente queste risorse in base al nome del gruppo di risorse.
    * Selezionare una **località del gruppo di risorse**.
    * Scegliere il piano **tariffario di ricerca** del servizio ricerca cognitiva di Azure. Se l'opzione livello gratuito non è disponibile (visualizzata in grigio), significa che è già stato distribuito un servizio gratuito tramite la sottoscrizione. In tal caso, è necessario iniziare con il livello Basic. Vedi [i dettagli sui prezzi di Azure ricerca cognitiva](https://azure.microsoft.com/pricing/details/search/).
    * Scegliere il **percorso di ricerca** in cui si desidera distribuire gli indici ricerca cognitiva di Azure. Le restrizioni relative al percorso di archiviazione dei dati dei clienti contribuiranno a determinare il percorso scelto per ricerca cognitiva di Azure.
    * Nel campo **nome app** immettere un nome per l'istanza del servizio app Azure.
    * Per impostazione predefinita, il servizio app viene impostato sul livello standard (S1). È possibile modificare il piano dopo la creazione. Altre informazioni sui [prezzi del servizio app](https://azure.microsoft.com/pricing/details/app-service/).
    * Scegliere il **percorso del sito Web** in cui verrà distribuito il servizio app.

        > [!NOTE]
        > Il **percorso di ricerca** può essere diverso dal **percorso del sito Web**.

    * Specificare se si desidera abilitare o meno **Application Insights**. Se **Application Insights** è abilitato, QnA Maker raccoglierà dati di telemetria su traffico, log delle chat ed errori.
    * Scegliere il **percorso di App Insights** in cui verrà distribuita la risorsa Application Insights.
    * Per le misure di risparmio sui costi, è possibile [condividere](#configure-qna-maker-to-use-different-cognitive-search-resource) alcune ma non tutte le risorse di Azure create per QnA Maker.

1. Dopo la convalida di tutti i campi, selezionare **Crea**. Il completamento del processo può richiedere alcuni minuti.

1. Al termine della distribuzione, verranno visualizzate le risorse seguenti create nella sottoscrizione:

   ![Risorsa creata - Nuovo servizio QnA Maker](../media/qnamaker-how-to-setup-service/resources-created.png)

    La risorsa con il tipo di _Servizi cognitivi_ ha le chiavi di _sottoscrizione_ .

## <a name="find-subscription-keys-in-the-azure-portal"></a>Trovare le chiavi di sottoscrizione nel portale di Azure

È possibile visualizzare e reimpostare le chiavi della sottoscrizione dalla portale di Azure, in cui è stata creata la risorsa QnA Maker.

1. Passare alla risorsa QnA Maker nella portale di Azure e selezionare la risorsa con il tipo di _Servizi cognitivi_ :

    ![Elenco di risorse QnA Maker](../media/qnamaker-how-to-key-management/qnamaker-resource-list.png)

2. Vai alle **chiavi**:

    ![Chiave della sottoscrizione](../media/qnamaker-how-to-key-management/subscription-key.PNG)

## <a name="find-endpoint-keys-in-the-qna-maker-portal"></a>Trovare le chiavi degli endpoint nel portale di QnA Maker

L'endpoint si trova nella stessa area della risorsa perché le chiavi dell'endpoint vengono utilizzate per effettuare una chiamata alla Knowledge base.

Le chiavi endpoint possono essere gestite dal [portale di QnA Maker](https://qnamaker.ai).

1. Accedere al portale di [QnA Maker](https://qnamaker.ai), passare al profilo e quindi selezionare **impostazioni del servizio**:

    ![Chiave endpoint](../media/qnamaker-how-to-key-management/Endpoint-keys.png)

2. Visualizza o Reimposta le chiavi:

    > [!div class="mx-imgBorder"]
    > Gestione chiavi ![endpoint](../media/qnamaker-how-to-key-management/Endpoint-keys1.png)

    >[!NOTE]
    >Aggiornare le chiavi se si ritiene che siano state compromesse. Questa operazione può richiedere modifiche corrispondenti al codice del bot o dell'applicazione client.

### <a name="upgrade-qna-maker-sku"></a>Aggiornare QnA Maker SKU

Per ulteriori domande e risposte nella Knowledge base, oltre al livello corrente, aggiornare il piano tariffario del servizio QnA Maker.

Per aggiornare lo SKU di gestione di QnA Maker:

1. Passare alla risorsa QnA Maker nel portale di Azure e selezionare **Piano tariffario**.

    ![Risorsa QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource.png)

1. Scegliere lo SKU appropriato e fare clic su **Seleziona**.

    ![Prezzi di QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-pricing-page.png)

### <a name="upgrade-app-service"></a>Aggiornare il servizio app

 Quando la Knowledge base deve servire più richieste dall'app client, aggiornare il piano tariffario del servizio app.

È possibile [aumentare](https://docs.microsoft.com/azure/app-service/manage-scale-up) o ridurre il servizio app.

Passare alla risorsa del servizio app nel portale di Azure e selezionare l'opzione di **scalabilità verticale** o **orizzontale** secondo le esigenze.

![Scalabilità del servizio app QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-scale.png)

### <a name="upgrade-the-azure-cognitive-search-service"></a>Aggiornare il servizio Azure ricerca cognitiva

Se si prevede di avere molte Knowledge base, aggiornare il piano tariffario del servizio ricerca cognitiva di Azure.

Attualmente, non è possibile eseguire un aggiornamento sul posto dello SKU di ricerca di Azure. È tuttavia possibile creare una nuova risorsa di Ricerca di Azure con lo SKU desiderato, ripristinare i dati nella nuova risorsa e quindi collegarla allo stack di QnA Maker. A questo scopo, seguire questa procedura:

1. Creare una nuova risorsa di ricerca di Azure nella portale di Azure e selezionare lo SKU desiderato.

    ![Risorsa di Ricerca di Azure QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

1. Ripristinare gli indici dalla risorsa di Ricerca di Azure originale alla nuova risorsa. Vedere il [codice di esempio](https://github.com/pchoudhari/QnAMakerBackupRestore)per il ripristino del backup.

1. Dopo il ripristino dei dati, passare alla nuova risorsa ricerca di Azure, selezionare **chiavi**e annotare il **nome** e la **chiave amministratore**:

    ![Chiavi di Ricerca di Azure QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-keys.png)

1. Per collegare la nuova risorsa di ricerca di Azure allo stack di QnA Maker, passare all'istanza del servizio app QnA Maker.

    ![QnA Maker istanza del servizio app](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource-list-appservice.png)

1. Selezionare **Impostazioni applicazione** e modificare le impostazioni nei campi **AzureSearchName** e **AzureSearchAdminKey** del passaggio 3.

    ![Impostazione del servizio app QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-settings.png)

1. Riavviare l'istanza del servizio app.

    ![Riavvio dell'istanza del servizio app QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="get-the-latest-runtime-updates"></a>Ottenere gli aggiornamenti più recenti del runtime

Il runtime di QnAMaker fa parte dell'istanza del servizio app Azure distribuita quando si [Crea un servizio QnAMaker](./set-up-qnamaker-service-azure.md) nel portale di Azure. Vengono applicati aggiornamenti periodici al runtime. L'istanza del servizio app QnA Maker è in modalità di aggiornamento automatico dopo la versione dell'estensione del sito aprile 2019 (versione 5 +). Questo aggiornamento è stato progettato per gestire il tempo di inattività durante gli aggiornamenti.

È possibile controllare la versione corrente in https://www.qnamaker.ai/UserSettings. Se la versione è precedente alla versione 5. x, è necessario riavviare il servizio app per applicare gli aggiornamenti più recenti:

1. Passare al servizio QnAMaker (gruppo di risorse) nell' [portale di Azure](https://portal.azure.com).

    > [!div class="mx-imgBorder"]
    > ![gruppo di risorse di Azure di QnAMaker](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

1. Selezionare l'istanza del servizio app e aprire la sezione **Panoramica** .

    > [!div class="mx-imgBorder"]
    > ![istanza del servizio app di QnAMaker](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)


1. Riavviare il servizio app. Il processo di aggiornamento dovrebbe terminare tra qualche secondo. Eventuali applicazioni dipendenti o bot che usano questo servizio QnAMaker non saranno disponibili per gli utenti finali durante questo periodo di riavvio.

    ![Riavvio dell'istanza del servizio app QnAMaker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="configure-qna-maker-to-use-different-cognitive-search-resource"></a>Configurare QnA Maker per l'uso di risorse ricerca cognitiva diverse

Se si crea un servizio QnA e le relative dipendenze (ad esempio la ricerca) tramite il portale, viene creato un servizio di ricerca e collegato al servizio QnA Maker. Dopo aver creato queste risorse, è possibile aggiornare l'impostazione del servizio app per usare un servizio di ricerca precedentemente esistente e rimuovere quello appena creato.

La risorsa del **servizio app** di QnA Maker usa la risorsa ricerca cognitiva. Per modificare la risorsa ricerca cognitiva utilizzata da QnA Maker, è necessario modificare l'impostazione nella portale di Azure.

1. Ottenere la **chiave di amministrazione** e il **nome** della risorsa ricerca cognitiva che si desidera QnA Maker usare.

1. Accedere al [portale di Azure](https://portal.azure.com) e trovare il **servizio app** associato alla risorsa QnA Maker. Entrambe le con hanno lo stesso nome.

1. Selezionare **Settings (impostazioni**) e quindi **Configuration (configurazione**). Vengono visualizzate tutte le impostazioni esistenti per il servizio app del QnA Maker.

    > [!div class="mx-imgBorder"]
    > ![screenshot della portale di Azure che mostra le impostazioni di configurazione del servizio app](../media/qnamaker-how-to-upgrade-qnamaker/change-search-service-app-service-configuration.png)

1. Modificare i valori per le chiavi seguenti:

    * **AzureSearchAdminKey**
    * **AzureSearchName**

1. Per usare le nuove impostazioni, è necessario riavviare il servizio app. Selezionare **Panoramica**, quindi fare clic su **Riavvia**.

    > [!div class="mx-imgBorder"]
    > ![screenshot di portale di Azure il riavvio del servizio app dopo la modifica delle impostazioni di configurazione](../media/qnamaker-how-to-upgrade-qnamaker/screenshot-azure-portal-restart-app-service.png)

Se si crea un servizio QnA tramite modelli di Azure Resource Manager, è possibile creare tutte le risorse e controllare la creazione del servizio app per l'uso di un servizio di ricerca esistente.

## <a name="next-steps"></a>Passaggi successivi

Altre informazioni sul servizio [app](../../../app-service/index.yml) e il [servizio di ricerca](../../../search/index.yml).

> [!div class="nextstepaction"]
> [Creare e pubblicare una Knowledge Base](../Quickstarts/create-publish-knowledge-base.md)