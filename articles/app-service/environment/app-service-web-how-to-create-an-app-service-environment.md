---
title: Come creare un ambiente del servizio app (versione 1) - Azure
description: Descrizione del flusso di creazione di un ambiente del servizio app (versione 1)
services: app-service
documentationcenter: ''
author: ccompy
manager: stefsch
editor: ''
ms.assetid: 81bd32cf-7ae5-454b-a0d2-23b57b51af47
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 017c79ec1341c85f3bd08393dd5553f90a2f6cef
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2019
ms.locfileid: "70069749"
---
# <a name="how-to-create-an-app-service-environment-v1"></a>Come creare un ambiente del servizio app (versione 1) 

> [!NOTE]
> Questo articolo riguarda l'ambiente del servizio app v1. È disponibile una versione più recente dell'ambiente del servizio app più facile da usare ed eseguita in un'infrastruttura più potente. Per altre informazioni su questa nuova versione, vedere [Introduzione all'ambiente del servizio app](intro.md).
> 

### <a name="overview"></a>Panoramica
L'ambiente del servizio app è un'opzione di servizio Premium del servizio app di Azure che offre una funzionalità avanzata non disponibile negli indicatori di multi-tenant. In breve, la funzionalità Ambiente del servizio app distribuisce il servizio app di Azure nella rete virtuale del cliente. Per comprendere meglio le funzionalità offerte dagli ambienti del servizio app, vedere la documentazione relativa a [ambiente del servizio app][WhatisASE] .

### <a name="before-you-create-your-ase"></a>Prima di creare l'ambiente del servizio app
È importante notare che alcuni elementi non possono essere modificati. Dopo la creazione dell'ambiente del servizio app non è possibile modificare questi elementi:

* Location
* Sottoscrizione
* Gruppo di risorse
* Rete virtuale usata
* Subnet usata 
* Dimensioni della subnet

Quando si sceglie una rete virtuale e si specifica una subnet, assicurarsi che le dimensioni siano sufficienti per supportare la crescita futura. 

### <a name="creating-an-app-service-environment-v1"></a>Creazione di un ambiente del servizio app (versione 1)
Per creare un ambiente del servizio app (versione 1), è possibile cercare ***App Service Environment v1*** (Ambiente del servizio app - versione 1) in Azure Marketplace oppure passare a **Crea una risorsa** -> **Web e dispositivi mobili** -> **Ambiente del servizio app**. Per creare un ambiente del servizio app (versione 1):

1. Specificare il nome dell'ambiente del servizio app. Il nome specificato per l'ambiente del servizio app verrà usato per le app create nell'ambiente stesso. Se il nome dell'ambiente del servizio app è appsvcenvdemo, il nome del sottodominio sarà *appsvcenvdemo.p.azurewebsites.net*. Se è stata creata un'app denominata *mytestapp*, questa sarà disponibile all'indirizzo *mytestapp.appsvcenvdemo.p.azurewebsites.net*. Il nome dell'ambiente del servizio app non può contenere spazi. Anche se nel nome vengono usati caratteri maiuscoli, il nome di dominio corrisponderà alla versione in caratteri minuscoli del nome. Se si usa un servizio di bilanciamento del carico interno, il nome dell'ambiente del servizio app non viene usato nel sottodominio, ma viene dichiarato in modo esplicito durante la creazione dell'ambiente.
   
    ![][1]
2. Selezionare la propria sottoscrizione. La sottoscrizione usata per l'ambiente del servizio app sarà valida anche per tutte le app create in tale ambiente. Non è possibile posizionare l'ambiente del servizio app in una rete virtuale che si trova in un'altra sottoscrizione.
3. Selezionare o specificare un nuovo gruppo di risorse. Il gruppo di risorse per l'ambiente del servizio app deve essere uguale a quello usato per la rete virtuale. Se si seleziona una rete virtuale già esistente, la selezione del gruppo di risorse per l'ambiente del servizio app verrà aggiornata per riflettere il valore della rete virtuale.
   
    ![][2]
4. Selezionare la rete virtuale e la località. È possibile scegliere di creare una nuova rete virtuale o selezionare una rete virtuale già esistente. Se si seleziona una nuova rete virtuale, è possibile specificare un nome e una località. Alla nuova rete virtuale verrà assegnato l'intervallo di indirizzi 192.168.250.0/23 e una subnet denominata **predefinita**, definita come 192.168.250.0/24. È anche possibile selezionare semplicemente una rete virtuale classica o di Resource Manager già esistente. La selezione del tipo di indirizzo VIP indica se è possibile accedere all'ambiente del servizio app direttamente da Internet (Esterno) o se si usa un Servizio di bilanciamento del carico interno. Per altre informazioni, vedere [uso di un Load Balancer interno con una ambiente del servizio app][ILBASE]. Se si seleziona un indirizzo VIP di tipo Esterno, è possibile selezionare il numero di indirizzi IP esterni con cui viene creato il sistema per finalità IPSSL. Se si seleziona Interno, è necessario specificare il sottodominio che verrà usato dall'ambiente del servizio app. Gli ambienti del servizio app possono essere distribuiti nelle reti virtuali che usano *gli* intervalli di indirizzi pubblici *o gli* spazi di indirizzi RFC1918, ovvero gli indirizzi privati. Per usare una rete virtuale con un intervallo di indirizzi pubblici, è necessario creare in anticipo la rete virtuale. Quando si seleziona una rete virtuale già esistente, sarà necessario creare una nuova subnet durante la creazione dell'ambiente del servizio app. **Non è possibile usare una subnet creata in precedenza nel portale. È possibile creare un ambiente del servizio app con una subnet già esistente se si crea l'ambiente del servizio app usando un modello di Resource Manager.** Per creare un ambiente del servizio app da un modello, usare le informazioni riportate qui, [creando un ambiente del servizio app da un modello][ILBAseTemplate] e qui, [creando un ambiente del servizio app ILB dal modello][ASEfromTemplate].

### <a name="details"></a>Dettagli
Un ambiente del servizio app viene creato con due front-end e due ruoli di lavoro. Le risorse front-end fungono da endpoint HTTP/HTTPS e inviano il traffico ai ruoli di lavoro, ovvero i ruoli che ospitano le applicazioni. È possibile modificare la quantità dopo la creazione dell'ambiente del servizio app e configurare regole di scalabilità automatica per questi pool di risorse. Per altri dettagli sul ridimensionamento manuale, la gestione e il monitoraggio di un ambiente del servizio app, vedere [Come configurare un ambiente del servizio app][ASEConfig] 

Nella subnet usata dall'ambiente del servizio app può essere presente solo un ambiente del servizi app. La subnet non può essere usata per scopi diversi dall'ambiente del servizio app.

### <a name="after-app-service-environment-v1-creation"></a>Dopo la creazione dell'ambiente del servizio app (versione 1)
Dopo la creazione dell'ambiente del servizio app è possibile modificare:

* Quantità di server front-end (minimo: 2)
* Quantità di ruoli di lavoro (minimo: 2)
* Quantità di indirizzi IP disponibili per IP SSL
* Dimensioni delle risorse di calcolo usate dai server front-end o dai processi di lavoro (dimensioni minime per i front-end: P2)

Per altri dettagli sul ridimensionamento manuale, la gestione e il monitoraggio degli ambienti del servizio app, vedere [Come configurare un ambiente del servizio app][ASEConfig] 

Per informazioni sul ridimensionamento automatico, vedere [Come configurare la scalabilità automatica per un ambiente del servizio app][ASEAutoscale]

Sono inoltre presenti altre dipendenze non disponibili per la personalizzazione, ad esempio il database e l'archiviazione. Questi sono gestiti da Azure e sono inclusi nel sistema. Il servizio di archiviazione del sistema supporta fino a 500 GB per l'intero ambiente del servizio app e il database viene rettificato da Azure in base alle esigenze, tramite il ridimensionamento del sistema.

## <a name="getting-started"></a>Introduzione
Per iniziare a usare ambiente del servizio app V1, vedere [Introduzione al ambiente del servizio app V1][WhatisASE]

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-basecreateblade.png
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-vnetcreation.png

<!--Links-->
[WhatisASE]: app-service-app-service-environment-intro.md
[ASEConfig]: app-service-web-configure-an-app-service-environment.md
[AppServicePricing]: https://azure.microsoft.com/pricing/details/app-service/ 
[ASEAutoscale]: app-service-environment-auto-scale.md
[ILBASE]: app-service-environment-with-internal-load-balancer.md
[ILBAseTemplate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/
[ASEfromTemplate]: app-service-app-service-environment-create-ilb-ase-resourcemanager.md
