---
title: Copiare dati da un'origine HTTP tramite Azure Data Factory | Microsoft Docs
description: Informazioni su come copiare dati da un'origine HTTP locale o cloud in archivi dati sink supportati usando un'attività di copia in una pipeline di Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/09/2019
ms.author: jingwang
ms.openlocfilehash: 6dd40527cdb073c76872c5768a7bea44b74155b7
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2019
ms.locfileid: "71092054"
---
# <a name="copy-data-from-an-http-endpoint-by-using-azure-data-factory"></a>Copiare dati da un endpoint HTTP tramite Azure Data Factory

> [!div class="op_single_selector" title1="Selezionare uSelezionare la versione del servizio di Azure Data Factory in uso:"]
> * [Versione 1](v1/data-factory-http-connector.md)
> * [Versione corrente](connector-http.md)

Questo articolo descrive come usare l'attività di copia in Azure Data Factory per copiare dati da un endpoint HTTP. L'articolo è basato su [Attività di copia in Azure Data Factory](copy-activity-overview.md), dove viene presentata una panoramica generale dell'attività di copia.

La differenza tra questo connettore HTTP, il [connettore REST](connector-rest.md) e il [connettore Tabella Web](connector-web-table.md) è la seguente:

- Il **connettore REST** supporta in modo specifico la copia dei dati dalle API RESTful. 
- Il **connettore HTTP** è un connettore generico per recuperare i dati da qualsiasi endpoint HTTP, ad esempio per scaricare file. Prima che il connettore REST diventi disponibile, può capitare di usare il connettore HTTP per copiare dati dall'API RESTful, che è supportata ma meno funzionale rispetto al connettore REST.
- Il **connettore Tabella Web** estrae il contenuto della tabella da una pagina Web HTML.

## <a name="supported-capabilities"></a>Funzionalità supportate

Questo connettore HTTP è supportato per le attività seguenti:

- [Attività di copia](copy-activity-overview.md) con [matrice di origine/sink supportata](copy-activity-overview.md)
- [Attività Lookup](control-flow-lookup-activity.md)

È possibile copiare dati da un'origine HTTP in qualsiasi archivio dati sink supportato. Per un elenco degli archivi dati supportati dall'attività di copia come origini e sink, vedere [Archivi dati e formati supportati](copy-activity-overview.md#supported-data-stores-and-formats).

È possibile usare questo Connettore HTTP per:

- Recuperare dati da un endpoint HTTP/S tramite il metodo HTTP **GET** o **POST**.
- Recuperare dati usando una delle autenticazioni seguenti: **Anonymous**, **Basic**, **Digest**, **Windows** o **ClientCertificate**.
- Copiare la risposta HTTP così com'è o analizzarla usando i [formati di file e i codec di compressione supportati](supported-file-formats-and-compression-codecs.md).

> [!TIP]
> Per testare una richiesta HTTP per il recupero dei dati prima di configurare il connettore HTTP in Data Factory, fare riferimento alla specifica dell'API per i requisiti relativi a intestazione e corpo. È possibile usare strumenti come Postman o un Web browser per la convalida.

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="get-started"></a>Attività iniziali

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Le sezioni seguenti presentano informazioni dettagliate sulle proprietà che è possibile usare per definire entità di Data Factory specifiche per il connettore HTTP.

## <a name="linked-service-properties"></a>Proprietà del servizio collegato

Per il servizio collegato HTTP sono supportate le proprietà seguenti:

| Proprietà | Descrizione | Obbligatoria |
|:--- |:--- |:--- |
| type | La proprietà **type** deve essere impostata su **HttpServer**. | Sì |
| url | URL di base del server Web. | Sì |
| enableServerCertificateValidation | Specificare se abilitare la convalida del certificato SSL del server quando ci si connette a un endpoint HTTP. Se il server HTTPS usa un certificato autofirmato, impostare questa proprietà su **false**. | N.<br /> (il valore predefinito è **true**) |
| authenticationType | Specifica il tipo di autenticazione. I valori consentiti sono **Anonymous**, **Basic**, **Digest**, **Windows** e **ClientCertificate**. <br><br> Vedere le sezioni seguenti per altre proprietà e altri esempi JSON per questi tipi di autenticazione. | Sì |
| connectVia | [Runtime di integrazione](concepts-integration-runtime.md) da usare per la connessione all'archivio dati. Ulteriori informazioni sono disponibili nella sezione [prerequisiti](#prerequisites) . Se questa proprietà non è specificata, viene usato il tipo Azure Integration Runtime predefinito. |No |

### <a name="using-basic-digest-or-windows-authentication"></a>Usando l'autenticazione Basic, Digest o Windows

Impostare la proprietà **authenticationType** su **Basic**, **Digest** o **Windows**. Oltre alle proprietà generiche descritte nella sezione precedente, specificare le proprietà seguenti:

| Proprietà | Descrizione | Obbligatorio |
|:--- |:--- |:--- |
| userName | Nome utente da usare per accedere all'endpoint HTTP. | Sì |
| password | Password per l'utente (valore di **userName**). Contrassegnare questo campo come tipo **SecureString** per archiviare la password in modo sicuro in Data Factory. È anche possibile [fare riferimento a un segreto archiviato in Azure Key Vault](store-credentials-in-key-vault.md). | Sì |

**Esempio**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "Basic",
            "url" : "<HTTP endpoint>",
            "userName": "<user name>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="using-clientcertificate-authentication"></a>Usando l'autenticazione ClientCertificate

Per usare l'autenticazione ClientCertificate, impostare la proprietà **authenticationType** su **ClientCertificate**. Oltre alle proprietà generiche descritte nella sezione precedente, specificare le proprietà seguenti:

| Proprietà | Descrizione | Obbligatorio |
|:--- |:--- |:--- |
| embeddedCertData | Dati del certificato con codifica Base64. | Specificare **embeddedCertData** o **certThumbprint**. |
| certThumbprint | Identificazione personale del certificato installato nell'archivio certificati del computer per il runtime di integrazione self-hosted. Si applica solo quando nella proprietà **connectVia** è specificato il tipo self-hosted del runtime di integrazione. | Specificare **embeddedCertData** o **certThumbprint**. |
| password | Password associata al certificato. Contrassegnare questo campo come tipo **SecureString** per archiviare la password in modo sicuro in Data Factory. È anche possibile [fare riferimento a un segreto archiviato in Azure Key Vault](store-credentials-in-key-vault.md). | N. |

Se si usa **certThumbprint** per l'autenticazione e il certificato è installato nell'archivio personale del computer locale, concedere l'autorizzazione di lettura al runtime di integrazione self-hosted:

1. Aprire Microsoft Management Console (MMC). Aggiungere lo snap-in **Certificati** con **Computer locale** come destinazione.
2. Espandere **Certificati** > **Personale** e quindi selezionare **Certificati**.
3. Fare clic con il tasto destro del mouse sul certificato dall'archivio personale e quindi scegliere **Tutte le attività** > **Gestisci chiavi private**.
3. Nella scheda **Sicurezza** aggiungere l'account utente in cui è in esecuzione il servizio host di Integration Runtime (DIAHostService), con l'accesso in lettura al certificato.

**Esempio 1: Uso di certThumbprint**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "<HTTP endpoint>",
            "certThumbprint": "<thumbprint of certificate>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Esempio 2: Uso di embeddedCertData**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "<HTTP endpoint>",
            "embeddedCertData": "<Base64-encoded cert data>",
            "password": {
                "type": "SecureString",
                "value": "password of cert"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Proprietà del set di dati

Per un elenco completo delle sezioni e delle proprietà disponibili per la definizione dei set di dati, vedere l'articolo [Set di dati](concepts-datasets-linked-services.md). 

- Per **parquet, delimitato testo, JSON, avro e formato binario**, vedere la sezione [parquet, delimitato testo, JSON, avro e formato binario set di dati](#format-based-dataset) .
- Per altri formati come il **formato ORC**, vedere la sezione [altro set di dati del formato](#other-format-dataset) .

### <a name="format-based-dataset"></a>Set di dati parquet, delimitato di testo, JSON, avro e Binary Format

Per copiare dati da e verso **parquet, testo delimitato, JSON, avro e formato binario**, vedere l'articolo formato [parquet](format-parquet.md), [formato testo delimitato](format-delimited-text.md), formato [avro](format-avro.md) e [formato binario](format-binary.md) nel set di dati basato su formato e impostazioni supportate . Le proprietà seguenti sono supportate per http `location` in impostazioni nel set di dati basato sul formato:

| Proprietà    | Descrizione                                                  | Obbligatoria |
| ----------- | ------------------------------------------------------------ | -------- |
| type        | La proprietà `location` Type nel set di dati deve essere impostata su **HttpServerLocation**. | Sì      |
| relativeUrl | URL relativo della risorsa che contiene i dati.       | N.       |

> [!NOTE]
> Le dimensioni del payload della richiesta HTTP supportate sono circa 500 KB. Se le dimensioni del payload da passare all'endpoint Web sono maggiori di 500 KB, provare a inviare in batch il payload in blocchi più piccoli.

> [!NOTE]
> Il set di dati di tipo **HttpFile** con formato parquet/testo indicato nella sezione successiva è ancora supportato così com'è per l'attività di copia/ricerca per la compatibilità con le versioni precedenti. Si consiglia di usare questo nuovo modello in futuro e l'interfaccia utente di creazione di ADF ha cambiato la generazione di questi nuovi tipi.

**Esempio:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "HttpServerLocation",
                "relativeUrl": "<relative url>"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

### <a name="other-format-dataset"></a>Set di dati di altri formati

Per copiare dati da HTTP in **formato ORC**, sono supportate le proprietà seguenti:

| Proprietà | Descrizione | Obbligatoria |
|:--- |:--- |:--- |
| type | La proprietà **type** del set di dati deve essere impostata su **HttpFile**. | Sì |
| relativeUrl | URL relativo della risorsa che contiene i dati. Quando questa proprietà non è specificata, viene usato solo l'URL indicato nella definizione del servizio collegato. | N. |
| requestMethod | Metodo HTTP. I valori consentiti sono **Get** (predefinito) e **Post**. | N. |
| additionalHeaders | Intestazioni richiesta HTTP aggiuntive. | N. |
| requestBody | Corpo della richiesta HTTP. | No |
| format | Se si vuole recuperare dati dall'endpoint HTTP così come sono, senza analizzarli, e quindi copiarli in un archivio basato su file, ignorare la sezione **format** nelle definizioni del set di dati di input e di output.<br/><br/>Se si vuole analizzare il contenuto della risposta HTTP durante la copia, sono supportati i tipi di formato file seguenti: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat** e **ParquetFormat**. In **format** impostare la proprietà **type** su uno di questi valori. Per altre informazioni, vedere le sezioni relative ai formati [JSON](supported-file-formats-and-compression-codecs.md#json-format), [testo](supported-file-formats-and-compression-codecs.md#text-format), [Avro](supported-file-formats-and-compression-codecs.md#avro-format), [Orc](supported-file-formats-and-compression-codecs.md#orc-format) e [Parquet](supported-file-formats-and-compression-codecs.md#parquet-format). |N. |
| compression | Specificare il tipo e il livello di compressione dei dati. Per altre informazioni, vedere l'articolo sui [formati di file supportati e i codec di compressione](supported-file-formats-and-compression-codecs.md#compression-support).<br/><br/>Tipi supportati: **GZip**, **Deflate**, **BZip2** e **ZipDeflate**.<br/>Livelli supportati:  **Optimal** (Ottimale) e **Fastest** (Più veloce). |N. |

> [!NOTE]
> Le dimensioni del payload della richiesta HTTP supportate sono circa 500 KB. Se le dimensioni del payload da passare all'endpoint Web sono maggiori di 500 KB, provare a inviare in batch il payload in blocchi più piccoli.

**Esempio 1: Uso del metodo Get (predefinito)**

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "HttpFile",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "relativeUrl": "<relative url>",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        }
    }
}
```

**Esempio 2: Uso del metodo Post**

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "HttpFile",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "relativeUrl": "<relative url>",
            "requestMethod": "Post",
            "requestBody": "<body for POST HTTP request>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Proprietà dell'attività di copia

Questa sezione presenta un elenco delle proprietà supportate dall'origine HTTP.

Per un elenco completo delle sezioni e delle proprietà disponibili per la definizione delle attività, vedere [Pipeline](concepts-pipelines-activities.md). 

### <a name="http-as-source"></a>HTTP come origine

- Per eseguire la copia da **parquet, testo delimitato, JSON, avro e formato binario**, vedere la sezione [parquet, delimitato testo, JSON, avro e formato binario](#format-based-source) .
- Per eseguire la copia da altri formati come il **formato ORC**, vedere la sezione [altra origine del formato](#other-format-source) .

#### <a name="format-based-source"></a>Parquet, delimitato testo, JSON, avro e origine del formato binario

Per copiare dati da **parquet, testo delimitato, JSON, avro e formato binario**, vedere l'articolo formato [parquet](format-parquet.md), [formato testo delimitato](format-delimited-text.md), formato [avro](format-avro.md) e [formato binario](format-binary.md) in origine dell'attività di copia basata su formato e supportato Impostazioni. Le proprietà seguenti sono supportate per http `storeSettings` in impostazioni in origine copia basata sul formato:

| Proprietà                 | Descrizione                                                  | Obbligatoria |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | La proprietà Type in `storeSettings` deve essere impostata su **HttpReadSetting**. | Sì      |
| requestMethod            | Metodo HTTP. <br>I valori consentiti sono **Get** (predefinito) e **Post**. | N.       |
| addtionalHeaders         | Intestazioni richiesta HTTP aggiuntive.                             | N.       |
| requestBody              | Corpo della richiesta HTTP.                               | N.       |
| requestTimeout           | Timeout (valore di **TimeSpan**) durante il quale la richiesta HTTP attende una risposta. Si tratta del timeout per ottenere una risposta, non per leggere i dati della risposta. Il valore predefinito è **00:01:40**. | N.       |
| maxConcurrentConnections | Numero di connessioni simultanee per la connessione all'archivio di archiviazione. Specificare solo quando si desidera limitare la connessione simultanea all'archivio dati. | N.       |

> [!NOTE]
> Per il formato di testo parquet/delimitato, l'origine dell'attività di copia di tipo **HttpSource** citata nella sezione successiva è ancora supportata così com'è per la compatibilità con le versioni precedenti. Si consiglia di usare questo nuovo modello in futuro e l'interfaccia utente di creazione di ADF ha cambiato la generazione di questi nuovi tipi.

**Esempio:**

```json
"activities":[
    {
        "name": "CopyFromHTTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
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
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSetting",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "HttpReadSetting",
                    "requestMethod": "Post",
                    "additionalHeaders": "<header key: header value>\n<header key: header value>\n",
                    "requestBody": "<body for POST HTTP request>"
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="other-format-source"></a>Altra origine del formato

Per copiare dati da HTTP in **formato ORC**, nella sezione **origine** dell'attività di copia sono supportate le proprietà seguenti:

| Proprietà | Descrizione | Obbligatoria |
|:--- |:--- |:--- |
| type | La proprietà **type** dell'origine dell'attività di copia deve essere impostata su **HttpSource**. | Sì |
| httpRequestTimeout | Timeout (valore di **TimeSpan**) durante il quale la richiesta HTTP attende una risposta. Si tratta del timeout per ottenere una risposta, non per leggere i dati della risposta. Il valore predefinito è **00:01:40**.  | N. |

**Esempio**

```json
"activities":[
    {
        "name": "CopyFromHTTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<HTTP input dataset name>",
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
                "type": "HttpSource",
                "httpRequestTimeout": "00:01:00"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="lookup-activity-properties"></a>Proprietà attività di ricerca

Per informazioni dettagliate sulle proprietà, controllare l' [attività di ricerca](control-flow-lookup-activity.md).


## <a name="next-steps"></a>Passaggi successivi

Per un elenco degli archivi dati supportati come origini o sink dall'attività di copia in Azure Data Factory, vedere [Archivi dati e formati supportati](copy-activity-overview.md#supported-data-stores-and-formats).
