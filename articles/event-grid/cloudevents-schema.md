---
title: Usare Griglia di eventi di Azure con gli eventi nello schema CloudEvents
description: Viene descritto come usare lo schema CloudEvents per gli eventi in griglia di eventi di Azure. Il servizio supporta gli eventi nell'implementazione JSON degli eventi cloud.
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: babanisa
ms.openlocfilehash: f903a358ea493cd01238339ede10b4b16f98c7c5
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2020
ms.locfileid: "76514596"
---
# <a name="use-cloudevents-v10-schema-with-event-grid"></a>Usare lo schema CloudEvents v 1.0 con griglia di eventi

Oltre allo schema di [eventi predefinito](event-schema.md), griglia di eventi di Azure supporta in modo nativo gli eventi nell' [implementazione JSON di CloudEvents v 1.0 e del binding del](https://github.com/cloudevents/spec/blob/v1.0/json-format.md) [protocollo http](https://github.com/cloudevents/spec/blob/v1.0/http-protocol-binding.md). [CloudEvents](https://cloudevents.io/) è una [specifica aperta](https://github.com/cloudevents/spec/blob/v1.0/spec.md) per la descrizione dei dati degli eventi.

CloudEvents semplifica l'interoperabilità fornendo uno schema di eventi comune per la pubblicazione e l'utilizzo degli eventi basati sul cloud. Questo schema consente strumenti uniformi, modi standard per il routing e la gestione degli eventi e modi universali per la deserializzazione dello schema di eventi esterni. Con uno schema comune, è possibile integrare più facilmente il lavoro tra le piattaforme.

CloudEvents viene compilato da diversi [collaboratori](https://github.com/cloudevents/spec/blob/master/community/contributors.md), tra cui Microsoft, tramite [Cloud Native Computing Foundation](https://www.cncf.io/). È attualmente disponibile come versione 1,0.

Questo articolo descrive come usare lo schema CloudEvents con Griglia di eventi.

[!INCLUDE [requires-azurerm](../../includes/requires-azurerm.md)]

## <a name="install-preview-feature"></a>Installare la funzionalità di anteprima

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="cloudevent-schema"></a>Schema CloudEvent

Di seguito è riportato un esempio di un evento di archiviazione BLOB di Azure nel formato CloudEvents:

``` JSON
{
    "specversion": "1.0",
    "type": "Microsoft.Storage.BlobCreated",  
    "source": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-account}",
    "id": "9aeb0fdf-c01e-0131-0922-9eb54906e209",
    "time": "2019-11-18T15:13:39.4589254Z",
    "subject": "blobServices/default/containers/{storage-container}/blobs/{new-file}",
    "dataschema": "#",
    "data": {
        "api": "PutBlockList",
        "clientRequestId": "4c5dd7fb-2c48-4a27-bb30-5361b5de920a",
        "requestId": "9aeb0fdf-c01e-0131-0922-9eb549000000",
        "eTag": "0x8D76C39E4407333",
        "contentType": "image/png",
        "contentLength": 30699,
        "blobType": "BlockBlob",
        "url": "https://gridtesting.blob.core.windows.net/testcontainer/{new-file}",
        "sequencer": "000000000000000000000000000099240000000000c41c18",
        "storageDiagnostics": {
            "batchId": "681fe319-3006-00a8-0022-9e7cde000000"
        }
    }
}
```

Una descrizione dettagliata dei campi, dei tipi e delle definizioni disponibili in CloudEvents v 1.0 è [disponibile qui](https://github.com/cloudevents/spec/blob/v1.0/spec.md#required-attributes).

I valori delle intestazioni per gli eventi recapitati nello schema CloudEvents e nello schema Griglia di eventi sono gli stessi, ad eccezione di `content-type`. Per lo schema CloudEvents, tale valore intestazione è `"content-type":"application/cloudevents+json; charset=utf-8"`. Per lo schema Griglia di eventi, tale valore intestazione è `"content-type":"application/json; charset=utf-8"`.

## <a name="configure-event-grid-for-cloudevents"></a>Configurare Griglia di eventi per CloudEvents

È possibile usare Griglia di eventi sia per l'input che per l'output degli eventi nello schema CloudEvents. È possibile usare CloudEvents per gli eventi di sistema, ad esempio gli eventi di archiviazione BLOB, gli eventi dell'hub IoT e gli eventi personalizzati. Può anche trasformare tali eventi in transito in entrambe le direzioni.


| Schema di input       | Schema di output.
|--------------------|---------------------
| Formato CloudEvents | Formato CloudEvents
| Formato di Griglia di eventi  | Formato CloudEvents
| Formato CloudEvents | Formato di Griglia di eventi
| Formato di Griglia di eventi  | Formato di Griglia di eventi

Per tutti gli schemi di eventi, Griglia di eventi richiede la convalida quando si esegue la pubblicazione in un argomento di Griglia di eventi e quando si crea una sottoscrizione di eventi. Per altre informazioni, vedere [Event Grid security and authentication](security-authentication.md) (Sicurezza e autenticazione di Griglia di eventi).

### <a name="input-schema"></a>Schema di input

Si imposta lo schema di input per un argomento personalizzato quando si crea l'argomento personalizzato.

Per l'interfaccia della riga di comando di Azure usare:

```azurecli-interactive
# If you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic create \
  --name <topic_name> \
  -l westcentralus \
  -g gridResourceGroup \
  --input-schema cloudeventschemav1_0
```

Per PowerShell, usare:

```azurepowershell-interactive
# If you have not already installed the module, do it now.
# This module is required for preview features.
Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery

New-AzureRmEventGridTopic `
  -ResourceGroupName gridResourceGroup `
  -Location westcentralus `
  -Name <topic_name> `
  -InputSchema CloudEventSchemaV1_0
```

La versione corrente di CloudEvents non supporta l'invio in batch di eventi. Per pubblicare eventi con lo schema CloudEvent in un argomento, pubblicare singolarmente ogni evento.

### <a name="output-schema"></a>Schema di output.

Si imposta lo schema di output quando si crea la sottoscrizione dell'evento.

Per l'interfaccia della riga di comando di Azure usare:

```azurecli-interactive
topicID=$(az eventgrid topic show --name <topic-name> -g gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --name <event_subscription_name> \
  --source-resource-id $topicID \
  --endpoint <endpoint_URL> \
  --event-delivery-schema cloudeventschemav1_0
```

Per PowerShell, usare:
```azurepowershell-interactive
$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name <topic-name>).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -DeliverySchema CloudEventSchemaV1_0
```

 Attualmente non è possibile usare un trigger di Griglia di eventi per un'app di Funzioni di Azure quando l'evento viene recapitato nello schema CloudEvents. Usare un trigger HTTP. Per esempi di implementazione di un trigger HTTP che riceve gli eventi nello schema CloudEvents, vedere [Usare un trigger HTTP come trigger Griglia di eventi](../azure-functions/functions-bindings-event-grid.md#use-an-http-trigger-as-an-event-grid-trigger).

 ## <a name="endpoint-validation-with-cloudevents-v10"></a>Convalida degli endpoint con CloudEvents v 1.0

Se si ha già familiarità con griglia di eventi, è possibile che l'handshake di convalida degli endpoint della griglia di eventi non venga usato per impedire abusi. CloudEvents v 1.0 implementa la propria [semantica di protezione da abusi](security-authentication.md#webhook-event-delivery) usando il metodo delle opzioni http. Per altre informazioni, leggere [qui](https://github.com/cloudevents/spec/blob/v1.0/http-webhook.md#4-abuse-protection). Quando si usa lo schema CloudEvents per l'output, griglia di eventi USA con la protezione dagli abusi di CloudEvents v 1.0 al posto del meccanismo di convalida degli eventi di griglia di eventi.

## <a name="next-steps"></a>Passaggi successivi

* Per informazioni sul monitoraggio dei recapiti degli eventi, vedere [Monitorare il recapito dei messaggi di Griglia di eventi di Azure](monitor-event-delivery.md).
* Microsoft invita gli utenti a testare, commentare e [contribuire](https://github.com/cloudevents/spec/blob/master/CONTRIBUTING.md) a CloudEvents.
* Per altre informazioni sulla creazione di una sottoscrizione di Griglia di eventi di Azure, vedere [Schema di sottoscrizione per Griglia di eventi](subscription-creation-schema.md).
