---
title: Copy data from Dynamics AX
description: Informazioni su come copiare dati da Dynamics AX in archivi dati di sink supportati usando un'attività di copia in una pipeline di Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/01/2019
ms.author: jingwang
ms.openlocfilehash: 555293492a37b9c635cb11087c08d52aa4b4d8c4
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/20/2019
ms.locfileid: "74218619"
---
# <a name="copy-data-from-dynamics-ax-by-using-azure-data-factory"></a>Copy data from Dynamics AX by using Azure Data Factory

Questo articolo descrive come usare l'attività di copia in Azure Data Factory per copiare dati da un'origine Dynamics AX. L'articolo è basato su [Attività di copia in Azure Data Factory](copy-activity-overview.md), dove viene presentata una panoramica generale dell'attività di copia.

## <a name="supported-capabilities"></a>Funzionalità supportate

This Dynamics AX connector is supported for the following activities:

- [Copy activity](copy-activity-overview.md) with [supported source/sink matrix](copy-activity-overview.md)
- [Attività Lookup](control-flow-lookup-activity.md)

È possibile copiare dati da Dynamics AX in qualsiasi archivio dati di sink supportato. Per un elenco degli archivi dati supportati dall'attività di copia come origini e sink, vedere [Archivi dati e formati supportati](copy-activity-overview.md#supported-data-stores-and-formats).

In particolare, questo connettore Dynamics AX supporta la copia dei dati da Dynamics AX utilizzando il protocollo **OData**  con l’**autenticazione basata su entità servizio**.

>[!TIP]
>Per copiare dati da **Dynamics 365 for Finance and Operations**, è possibile usare questo connettore. Fare riferimento al [supporto per OData](https://docs.microsoft.com/dynamics365/unified-operations/dev-itpro/data-entities/odata) di Dynamics 365 e al [metodo di autenticazione](https://docs.microsoft.com/dynamics365/unified-operations/dev-itpro/data-entities/services-home-page#authentication).

## <a name="get-started"></a>Inizia oggi stesso

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Le sezioni seguenti presentano informazioni dettagliate sulle proprietà che è possibile usare per definire entità di Data Factory specifiche per il connettore Dynamics AX.

## <a name="prerequisites"></a>Prerequisiti

Per usare l'autenticazione basata su entità servizio, eseguire la procedura seguente:

1. Registrare un'entità applicazione in Azure Active Directory (Azure AD) seguendo le indicazioni fornite in [Registrare l'applicazione con un tenant di Azure AD](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant). Prendere nota dei valori seguenti che si usano per definire il servizio collegato:

    - ID applicazione
    - Chiave applicazione
    - ID tenant

2. Passare a Dynamics AX e concedere questa autorizzazione appropriata dell'entità servizio per accedere a Dynamics AX.

## <a name="linked-service-properties"></a>Proprietà del servizio collegato

Per il servizio collegato di Dynamics AX sono supportate le proprietà seguenti:

| Proprietà | Description | Obbligatoria |
|:--- |:--- |:--- |
| type | La proprietà **type** deve essere impostata su: **DynamicsAX**. |SÌ |
| url | L'istanza dell'endpoint OData di Dynamics AX (o Dynamics 365 Finance and Operations). |SÌ |
| servicePrincipalId | Specificare l'ID client dell'applicazione. | SÌ |
| servicePrincipalKey | Specificare la chiave dell'applicazione. Contrassegnare questo campo come **SecureString** per archiviarlo in modo sicuro in Data Factory oppure [fare riferimento a un segreto archiviato in Azure Key Vault](store-credentials-in-key-vault.md). | SÌ |
| tenant | Specificare le informazioni sul tenant (nome di dominio o ID tenant) in cui si trova l'applicazione. Recuperarle passando il cursore del mouse sull'angolo superiore destro del portale di Azure. | SÌ |
| aadResourceId | Specificare la risorsa AAD per cui si sta richiedendo l'autorizzazione. Ad esempio, se è l'URL di Dynamics è `https://sampledynamics.sandbox.operations.dynamics.com/data/`, la risorsa AAD corrispondente è in genere `https://sampledynamics.sandbox.operations.dynamics.com`. | SÌ |
| connectVia | [Runtime di integrazione](concepts-integration-runtime.md) da usare per la connessione all'archivio dati. È possibile scegliere tra Azure Integration Runtime e un runtime di integrazione self-hosted (se l'archivio dati si trova in una rete privata). Se questa proprietà non è specificata, viene usato il tipo Azure Integration Runtime predefinito. |No |

**Esempio**

```json
{
    "name": "DynamicsAXLinkedService",
    "properties": {
        "type": "DynamicsAX",
        "typeProperties": {
            "url": "<Dynamics AX instance OData endpoint>",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "aadResourceId": "<AAD resource, e.g. https://sampledynamics.sandbox.operations.dynamics.com>"
        }
    },
    "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
    }
}

```

## <a name="dataset-properties"></a>Proprietà del set di dati

Questa sezione presenta un elenco delle proprietà supportate dal set di dati Dynamics AX.

Per un elenco completo delle sezioni e delle proprietà disponibili per la definizione dei set di dati, vedere [Set di dati e servizi collegati](concepts-datasets-linked-services.md). 

Per copiare dati da Dynamics AX, impostare la proprietà **type** del set di dati su **DynamicsAXResource**. Sono supportate le proprietà seguenti:

| Proprietà | Description | Obbligatoria |
|:--- |:--- |:--- |
| type | La proprietà **type** del set di dati deve essere impostata su: **DynamicsAXResource**. | SÌ |
| path | Percorso di entità OData Dynamics AX. | SÌ |

**Esempio**

```json
{
    "name": "DynamicsAXResourceDataset",
    "properties": {
        "type": "DynamicsAXResource",
        "typeProperties": {
            "path": "<entity path e.g. dd04tentitySet>"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Dynamics AX linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Proprietà dell'attività di copia

Questa sezione presenta un elenco delle proprietà supportate dall'origine Dynamics AX.

Per un elenco completo delle sezioni e delle proprietà disponibili per la definizione delle attività, vedere [Pipeline](concepts-pipelines-activities.md). 

### <a name="dynamics-ax-as-source"></a>Dynamics AX come origine

Per copiare dati da Dynamics AX, impostare il tipo di **origine** nell'attività di copia su **DynamicsAXSource**. Nella sezione **source** dell'attività di copia sono supportate le proprietà seguenti:

| Proprietà | Description | Obbligatoria |
|:--- |:--- |:--- |
| type | La proprietà **type** dell'origine dell'attività di copia deve essere impostata su **DynamicsAXSource**. | SÌ |
| query | Opzioni di query OData per filtrare i dati. Esempio: `"?$select=Name,Description&$top=5"`.<br/><br/>**Nota**: il connettore copia dati dall'URL combinato: `[URL specified in linked service]/[path specified in dataset][query specified in copy activity source]`. Per altre informazioni, vedere [OData URL components](https://www.odata.org/documentation/odata-version-3-0/url-conventions/) (Componenti dell'URL di OData). | No |

**Esempio**

```json
"activities":[
    {
        "name": "CopyFromDynamicsAX",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Dynamics AX input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DynamicsAXSource",
                "query": "$top=10"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```


## <a name="lookup-activity-properties"></a>Lookup activity properties

To learn details about the properties, check [Lookup activity](control-flow-lookup-activity.md).

## <a name="next-steps"></a>Passaggi successivi

Per un elenco degli archivi dati supportati come origini o sink dall'attività di copia in Azure Data Factory, vedere [Archivi dati e formati supportati](copy-activity-overview.md##supported-data-stores-and-formats).
