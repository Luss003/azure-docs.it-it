---
title: Accesso alle reti virtuali di Azure
description: Panoramica su come gli ambienti di Integration Services (ISEs) consentono alle app per la logica di accedere alle reti virtuali di Azure (reti virtuali)
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 12/16/2019
ms.openlocfilehash: d6bb57c8163f7653f4b10142d7ec2b34f50456f1
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/28/2019
ms.locfileid: "75527859"
---
# <a name="access-to-azure-virtual-network-resources-from-azure-logic-apps-by-using-integration-service-environments-ises"></a>Accedere alle risorse di Rete virtuale di Azure da App per la logica di Azure usando ambienti del servizio di integrazione (ISE)

A volte, le app per la logica e gli account di integrazione devono accedere alle risorse protette, ad esempio macchine virtuali (VM) e altri sistemi o servizi, all'interno di una [rete virtuale di Azure](../virtual-network/virtual-networks-overview.md). Per configurare questo accesso, è possibile [creare un *ambiente del servizio di integrazione* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) in cui è possibile eseguire le app per la logica e creare gli account di integrazione.

Quando si crea un ISE, Azure *inserisce* tale ISE nella rete virtuale di Azure, che quindi distribuisce un'istanza privata e isolata del servizio app per la logica nella rete virtuale di Azure. Questa istanza privata usa risorse dedicate, ad esempio l'archiviazione, e viene eseguita separatamente dal servizio pubblico di app per la logica multi-tenant "globale". La separazione dell'istanza privata isolata e dell'istanza globale pubblica consente anche di ridurre l'impatto che altri tenant di Azure potrebbero avere sulle prestazioni delle app, nota anche come [effetto "vicini rumorosi"](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors). Un ISE fornisce anche indirizzi IP statici. Questi indirizzi IP sono distinti dagli indirizzi IP statici condivisi dalle app per la logica nel servizio pubblico multi-tenant.

Dopo aver creato l'ISE, quando si crea l'app per la logica o l'account di integrazione, è possibile selezionare il percorso di ISE come app per la logica o l'account di integrazione:

![Selezionare l'ambiente del servizio di integrazione](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

L'app per la logica ora può accedere direttamente ai sistemi interni o connessi alla rete virtuale usando uno di questi elementi:

* Un connettore con etichetta **ISE**per quel sistema
* Un trigger o un'azione incorporata con etichetta **Core**, ad esempio il trigger o l'azione http
* Un connettore personalizzato

Questa panoramica descrive più dettagliatamente il modo in cui un ISE fornisce alle app per la logica e agli account di integrazione l'accesso diretto alla rete virtuale di Azure e confronta le differenze tra un ISE e il servizio globale app per la logica.

> [!IMPORTANT]
> Le app per la logica, i trigger incorporati, le azioni predefinite e i connettori eseguiti in ISE usano un piano tariffario diverso dal piano tariffario in base al consumo. Per informazioni sul funzionamento dei prezzi e della fatturazione per ISEs, vedere il [modello di prezzi di app](../logic-apps/logic-apps-pricing.md#fixed-pricing)per la logica. Per informazioni sui prezzi, vedere [prezzi di app](../logic-apps/logic-apps-pricing.md)per la logica.
>
> ISE ha anche un aumento dei limiti sulla durata dell'esecuzione, sulla conservazione dell'archiviazione, sulla velocità effettiva, sui timeout di richiesta e risposta HTTP, sulle dimensioni dei messaggi e sulle richieste del connettore personalizzato. 
> Per altre informazioni, vedere [limiti e configurazione per app per la logica di Azure](logic-apps-limits-and-config.md).

<a name="difference"></a>

## <a name="isolated-versus-global"></a>Ambiente isolato e ambiente globale

Quando si crea un ambiente del servizio integrato (ISE) in Azure, è possibile selezionare la rete virtuale di Azure in cui si vuole *inserire* ISE. Azure inserisce, o distribuisce, un'istanza privata del servizio app per la logica nella rete virtuale. Questa azione crea un ambiente isolato in cui è possibile creare ed eseguire le app per la logica in risorse dedicate. Quando si crea l'app per la logica, si seleziona ISE come posizione dell'app, che consente all'app per la logica di accedere direttamente alla rete virtuale e alle risorse in tale rete.

Le app per la logica in un ambiente del servizio di integrazione offrono le stesse esperienze utente e funzionalità simili a quelle del servizio App per la logica globale. Non solo è possibile usare gli stessi trigger predefiniti, le azioni predefinite e i connettori del servizio app per la logica globale, ma è anche possibile usare connettori specifici di ISE. Ad esempio, di seguito sono riportati alcuni connettori standard che offrono versioni eseguite in ISE:

* Archiviazione BLOB, Archiviazione file e Archiviazione tabelle di Azure
* Code di Azure, bus di servizio di Azure, hub eventi di Azure e IBM MQ
* FTP e SFTP-SSH
* SQL Server, Azure SQL Data Warehouse, Azure Cosmos DB
* AS2, X12 ed EDIFACT

La differenza tra connettori dell'ambiente del servizio di integrazione e quelli non dell'ambiente del servizio di integrazione è data dalle posizioni in cui i trigger e le azioni vengono eseguiti:

* In ISE, i trigger e le azioni predefiniti, ad esempio HTTP, vengono sempre eseguiti nello stesso ISE dell'app per la logica e visualizzano l'etichetta **principale** .

  ![Selezionare i trigger e le azioni predefiniti "core"](./media/connect-virtual-network-vnet-isolated-environment-overview/select-core-built-in-actions-triggers.png)

* I connettori eseguiti in ISE hanno versioni ospitate pubblicamente disponibili nel servizio app per la logica globale. Per i connettori che offrono due versioni, i connettori con l'etichetta **ISE** vengono sempre eseguiti nello stesso ISE dell'app per la logica. I connettori senza l'etichetta **ISE** vengono eseguiti nel servizio App per la logica globale.

  ![Selezionare i connettori ISE](./media/connect-virtual-network-vnet-isolated-environment-overview/select-ise-connectors.png)

Un ISE fornisce anche maggiori limiti per durata dell'esecuzione, conservazione dell'archiviazione, velocità effettiva, timeout di richieste e risposte HTTP, dimensioni dei messaggi e richieste di connettori personalizzati. Per altre informazioni, vedere [limiti e configurazione per app per la logica di Azure](logic-apps-limits-and-config.md).

<a name="ise-level"></a>

## <a name="ise-skus"></a>SKU ISE

Quando si crea ISE, è possibile selezionare lo SKU per sviluppatori o lo SKU Premium. Ecco le differenze tra gli SKU seguenti:

* **Developer**

  Fornisce un costo di ISE più basso che è possibile usare per la sperimentazione, lo sviluppo e il test, ma non per i test di produzione e delle prestazioni. Lo SKU Developer include trigger e azioni predefiniti, connettori standard, connettori aziendali e un singolo account di integrazione del [livello gratuito](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits) per un prezzo mensile fisso. Tuttavia, questo SKU non include alcun contratto di servizio (SLA), opzioni per la scalabilità verticale o ridondanza durante il riciclo, il che significa che è possibile che si verifichino ritardi o tempi di inattività.

* **Premium**

  Fornisce un ISE che è possibile usare per la produzione e include supporto del contratto di servizio, trigger e azioni predefiniti, connettori standard, connettori aziendali, un unico account di integrazione del [livello standard](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits) , opzioni per la scalabilità verticale e ridondanza durante il riciclo per un prezzo mensile fisso.

> [!IMPORTANT]
> L'opzione SKU è disponibile solo alla creazione di ISE e non può essere modificata in un secondo momento.

Per informazioni sui prezzi, vedere [prezzi di app](https://azure.microsoft.com/pricing/details/logic-apps/)per la logica. Per informazioni sul funzionamento dei prezzi e della fatturazione per ISEs, vedere il [modello di prezzi di app](../logic-apps/logic-apps-pricing.md#fixed-pricing)per la logica.

<a name="endpoint-access"></a>

## <a name="ise-endpoint-access"></a>Accesso endpoint ISE

Quando si crea ISE, è possibile scegliere di usare endpoint di accesso interni o esterni. La selezione determina se i trigger di richiesta o webhook nelle app per la logica in ISE possono ricevere chiamate dall'esterno della rete virtuale.

Questi endpoint influiscono anche sul modo in cui è possibile accedere agli input e agli output nella cronologia di esecuzione delle app per la logica.

* **Interno**: endpoint privati che consentono chiamate a app per la logica in ISE, in cui è possibile visualizzare e accedere agli input e agli output delle app per la logica nella cronologia di esecuzione *solo dall'interno della rete virtuale*

* **External**: endpoint pubblici che consentono chiamate a app per la logica in ISE, in cui è possibile visualizzare e accedere agli input e agli output delle app per la logica nella cronologia di esecuzione *dall'esterno della rete virtuale*. Se si usano i gruppi di sicurezza di rete (gruppi), assicurarsi che siano configurati con regole in ingresso per consentire l'accesso agli input e agli output della cronologia di esecuzione. Per altre informazioni, vedere [abilitare l'accesso per ISE](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#enable-access).

> [!IMPORTANT]
> L'opzione endpoint di accesso è disponibile solo alla creazione di ISE e non può essere modificata in un secondo momento.

<a name="on-premises"></a>

## <a name="access-to-on-premises-data-sources"></a>Accesso alle origini dati locali

Per i sistemi locali connessi a una rete virtuale di Azure, inserire ISE in tale rete, in modo che le app per la logica possano accedere direttamente a tali sistemi usando uno di questi elementi:

* Azione HTTP

* Connettore con etichetta ISE per quel sistema

  > [!NOTE]
  > Per usare l'autenticazione di Windows con il connettore SQL Server in un [ambiente Integration Services (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md), usare la versione non ISE del connettore con il [gateway dati locale](../logic-apps/logic-apps-gateway-install.md). La versione con etichetta ISE non supporta l'autenticazione di Windows.

* Connettore personalizzato

  * Se sono presenti connettori personalizzati che richiedono il gateway dati locale e i connettori sono stati creati al di fuori di un ISE, le app per la logica in un ISE possono anche usare tali connettori.
  
  * I connettori personalizzati creati in un ISE non funzionano con il gateway dati locale. Tuttavia, questi connettori possono accedere direttamente alle origini dati locali connesse alla rete virtuale che ospita ISE. Quindi, le app per la logica in un ISE probabilmente non necessitano del gateway dati durante la comunicazione con tali risorse.

Per i sistemi locali che non sono connessi a una rete virtuale o non hanno connettori con etichetta ISE, è necessario prima [configurare il gateway dati locale](../logic-apps/logic-apps-gateway-install.md) prima che le app per la logica possano connettersi a tali sistemi.

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>Account di integrazione con ambiente del servizio di integrazione

È possibile usare gli account di integrazione con App per la logica all'interno di un ambiente del servizio di integrazione (ISE). È necessario tuttavia che gli account di integrazione usino lo *stesso ISE* delle app per la logica collegate. Le app per la logica in un ambiente del servizio di integrazione possono fare riferimento solo agli account di integrazione che sono nello stesso ambiente del servizio di integrazione. Quando si crea un account di integrazione, è possibile selezionare l'ambiente del servizio di integrazione come posizione per l'account di integrazione. Per informazioni sul funzionamento dei prezzi e della fatturazione per gli account di integrazione con ISE, vedere il [modello di prezzi di app](../logic-apps/logic-apps-pricing.md#fixed-pricing)per la logica. Per informazioni sui prezzi, vedere [prezzi di app](https://azure.microsoft.com/pricing/details/logic-apps/)per la logica.

## <a name="next-steps"></a>Passaggi successivi

* [Connettersi alle reti virtuali di Azure da app per la logica isolate](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)
* [Aggiungere elementi agli ambienti del servizio di integrazione](../logic-apps/add-artifacts-integration-service-environment-ise.md)
* [Gestire gli ambienti del servizio di integrazione](../logic-apps/ise-manage-integration-service-environment.md)
* Altre informazioni su [Rete virtuale di Azure](../virtual-network/virtual-networks-overview.md)
* Informazioni sull'[integrazione della rete virtuale per i servizi di Azure](../virtual-network/virtual-network-for-azure-services.md)
