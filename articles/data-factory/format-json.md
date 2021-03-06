---
title: Formato JSON in Azure Data Factory
description: Questo argomento descrive come gestire il formato JSON in Azure Data Factory.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 02/05/2020
ms.author: jingwang
ms.openlocfilehash: 7dac8d21e3b45307284ece15ca5ddbcc69db909b
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2020
ms.locfileid: "79260644"
---
# <a name="json-format-in-azure-data-factory"></a>Formato JSON in Azure Data Factory

Seguire questo articolo quando si vuole **analizzare i file JSON o scrivere i dati in formato JSON**. 

Il formato JSON è supportato per i connettori seguenti [: Amazon S3](connector-amazon-simple-storage-service.md), [BLOB di Azure](connector-azure-blob-storage.md), [Azure Data Lake storage Gen1](connector-azure-data-lake-store.md), [Azure Data Lake storage Gen2](connector-azure-data-lake-storage.md), [archiviazione file di Azure](connector-azure-file-storage.md), [file System](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [http](connector-http.md)e [SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Proprietà del set di dati

Per un elenco completo delle sezioni e delle proprietà disponibili per la definizione dei set di dati, vedere l'articolo [Set di dati](concepts-datasets-linked-services.md). Questa sezione presenta un elenco delle proprietà supportate dal set di dati JSON.

| Proprietà         | Descrizione                                                  | Obbligatoria |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | La proprietà Type del set di dati deve essere impostata su **JSON**. | Sì      |
| posizione         | Impostazioni del percorso dei file. Ogni connettore basato su file ha un tipo di percorso e proprietà supportate in `location`. **Per informazioni dettagliate, vedere l'articolo connettore-> sezione Proprietà set di dati**. | Sì      |
| encodingName     | Tipo di codifica utilizzato per leggere/scrivere file di test. <br>I valori consentiti sono i seguenti: "UTF-8", "UTF-16", "UTF-16BE", "UTF-32", "UTF-32BE", "US-ASCII", "UTF-7", "BIG5", "EUC-JP", "EUC-KR", "GB2312", "GB18030", "Johab", "SHIFT-JIS", "CP875", "CP866", "IBM00858", "IBM037", "IBM273", "IBM437", "IBM500", " IBM737", "IBM775", "IBM850", "IBM852", "IBM855", "IBM857", "IBM860", "IBM861", "IBM863", "IBM864", "IBM865", "IBM869", "IBM870", "IBM01140", "IBM01141", "IBM01142", "IBM01143", "IBM01144", "IBM01145", "IBM01146", "IBM01147", "IBM01148", "IBM01149" , "ISO-2022-JP", "ISO-2022-KR", "ISO-8859-1", "ISO-8859-2", "ISO-8859-3", "ISO-8859-4", "ISO-8859-5", "ISO-8859-6", "ISO-8859-7", "ISO-8859-8", "ISO-8859-9", "ISO-8859-13", "ISO-8859-15", "WINDOWS-874", "WINDOWS-1250", "WINDOWS-1251", " WINDOWS-1252 "," WINDOWS-1253 "," WINDOWS-1254 "," WINDOWS-1255 "," WINDOWS-1256 "," WINDOWS-1257 "," WINDOWS-1258 ".| No       |
| compressionCodec | Codec di compressione utilizzato per leggere/scrivere file di testo. <br>I valori consentiti sono **bzip2**, **gzip**, **deflate**, **ZipDeflate**, **Snapper**o **LZ4**. da usare quando si salva il file. <br>Nota Attualmente l'attività di copia non supporta "blocco" & "LZ4".<br>Nota Quando si usa l'attività di copia per decomprimere i file ZipDeflate e scrivere nell'archivio dati sink basato su file, i file vengono estratti nella cartella: `<path specified in dataset>/<folder named as source zip file>/`. | No       |
| compressionLevel | Rapporto di compressione. <br>I valori consentiti sono **ottimali** o più **veloci**.<br>- più **veloce:** l'operazione di compressione deve essere completata il più rapidamente possibile, anche se il file risultante non è compresso in modo ottimale.<br>- **ottimale**: l'operazione di compressione deve essere compressa in modo ottimale, anche se il completamento dell'operazione richiede più tempo. Per maggiori informazioni, vedere l'argomento relativo al [livello di compressione](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) . | No       |

Di seguito è riportato un esempio di set di dati JSON nell'archivio BLOB di Azure:

```json
{
    "name": "JSONDataset",
    "properties": {
        "type": "Json",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder",
            },
            "compression": {
                "type": "gzip"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Proprietà dell'attività di copia

Per un elenco completo delle sezioni e delle proprietà disponibili per la definizione delle attività, vedere l'articolo sulle [pipeline](concepts-pipelines-activities.md). Questa sezione presenta un elenco delle proprietà supportate dall'origine e dal sink JSON.

### <a name="json-as-source"></a>JSON come origine

Le proprietà seguenti sono supportate nella sezione ***\*origine\**** dell'attività di copia.

| Proprietà      | Descrizione                                                  | Obbligatoria |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | La proprietà Type dell'origine dell'attività di copia deve essere impostata su **JSONSource**. | Sì      |
| storeSettings | Un gruppo di proprietà su come leggere i dati da un archivio dati. Ogni connettore basato su file ha le proprie impostazioni di lettura supportate in `storeSettings`. **Per informazioni dettagliate, vedere l'articolo connettore > sezione proprietà dell'attività di copia**. | No       |

### <a name="json-as-sink"></a>JSON come sink

Le proprietà seguenti sono supportate nella sezione ***\*sink\**** dell'attività di copia.

| Proprietà      | Descrizione                                                  | Obbligatoria |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | La proprietà Type dell'origine dell'attività di copia deve essere impostata su **JSONSink**. | Sì      |
| formatSettings | Gruppo di proprietà. Vedere la tabella **delle impostazioni di scrittura JSON** riportata di seguito. | No       |
| storeSettings | Gruppo di proprietà su come scrivere dati in un archivio dati. Ogni connettore basato su file ha le proprie impostazioni di scrittura supportate in `storeSettings`. **Per informazioni dettagliate, vedere l'articolo connettore > sezione proprietà dell'attività di copia**. | No       |

**Impostazioni di scrittura JSON** supportate in `formatSettings`:

| Proprietà      | Descrizione                                                  | Obbligatoria                                              |
| ------------- | ------------------------------------------------------------ | ----------------------------------------------------- |
| type          | Il tipo di formatSettings deve essere impostato su **JsonWriteSettings**. | Sì                                                   |
| filePattern |Indicare il modello dei dati archiviati in ogni file JSON. I valori consentiti sono: **setOfObjects** e **arrayOfObjects**. Il valore **predefinito** è **setOfObjects**. Vedere la sezione [Modelli di file JSON](#json-file-patterns) per i dettagli su questi modelli. |No |

### <a name="json-file-patterns"></a>Modelli di file JSON

L'attività di copia può rilevare e analizzare automaticamente i modelli di file JSON seguenti. 

- **Tipo I: setOfObjects**

    Ogni file contiene un solo oggetto o più oggetti con delimitatori di riga/concatenati. 
    Quando questa opzione viene scelta in sink dell'attività di copia, l'attività di copia produce un singolo file JSON con ogni oggetto per riga (delimitato da riga).

    * **Esempio di JSON a oggetto singolo**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        ```

    * **Esempio di JSON con delimitatori di riga**

        ```json
        {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
        {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
        {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
        ```

    * **Esempio di JSON concatenati**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        }
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
        ```

- **Tipo II: arrayOfObjects**

    Ogni file contiene una matrice di oggetti.

    ```json
    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
    ]
    ```

## <a name="mapping-data-flow-properties"></a>Mapping delle proprietà del flusso di dati

I tipi di file JSON possono essere usati sia come sink che come origine nel flusso di dati di mapping.

### <a name="creating-json-structures-in-a-derived-column"></a>Creazione di strutture JSON in una colonna derivata

È possibile aggiungere una colonna complessa al flusso di dati tramite il generatore di espressioni della colonna derivata. Nella trasformazione colonna derivata aggiungere una nuova colonna e aprire il generatore di espressioni facendo clic sulla casella blu. Per rendere complessa una colonna, è possibile immettere la struttura JSON manualmente o usare l'esperienza utente per aggiungere le sottocolonne in modo interattivo.

#### <a name="using-the-expression-builder-ux"></a>Uso del generatore di espressioni UX

Nel riquadro lato schema di output posizionare il puntatore del mouse su una colonna e fare clic sull'icona con il segno più. Selezionare **Aggiungi sottocolonna** per rendere la colonna un tipo complesso.

![Aggiungi sottocolonna](media/data-flow/addsubcolumn.png "Aggiungi sottocolonna")

È possibile aggiungere colonne e sottocolonne aggiuntive nello stesso modo. Per ogni campo non complesso, è possibile aggiungere un'espressione nell'editor di espressioni a destra.

![Colonna complessa](media/data-flow/complexcolumn.png "Colonna complessa")

#### <a name="entering-the-json-structure-manually"></a>Immissione manuale della struttura JSON

Per aggiungere manualmente una struttura JSON, aggiungere una nuova colonna e immettere l'espressione nell'editor. L'espressione segue il formato generale seguente:

```
@(
    field1=0,
    field2=@(
        field1=0
    )
)
```

Se questa espressione è stata immessa per una colonna denominata "complexColumn", viene scritta nel sink come il JSON seguente:

```
{
    "complexColumn": {
        "field1": 0,
        "field2": {
            "field1": 0
        }
    }
}
```

#### <a name="sample-manual-script-for-complete-hierarchical-definition"></a>Script manuale di esempio per la definizione gerarchica completa
```
@(
    title=Title,
    firstName=FirstName,
    middleName=MiddleName,
    lastName=LastName,
    suffix=Suffix,
    contactDetails=@(
        email=EmailAddress,
        phone=Phone
    ),
    address=@(
        line1=AddressLine1,
        line2=AddressLine2,
        city=City,
        state=StateProvince,
        country=CountryRegion,
        postCode=PostalCode
    ),
    ids=[
        toString(CustomerID), toString(AddressID), rowguid
    ]
)
```

### <a name="source-format-options"></a>Opzioni del formato di origine

L'uso di un set di dati JSON come origine nel flusso di dati consente di impostare cinque impostazioni aggiuntive. Queste impostazioni sono disponibili nella scheda **Opzioni di origine** del file **JSON Settings** Accordion.  

![Impostazioni JSON](media/data-flow/json-settings.png "Impostazioni JSON")

#### <a name="default"></a>Default

Per impostazione predefinita, i dati JSON vengono letti nel formato seguente.

```
{ "json": "record 1" }
{ "json": "record 2" }
{ "json": "record 3" }
```

#### <a name="single-document"></a>Documento singolo

Se è selezionato un **singolo documento** , il mapping dei flussi di dati legge un documento JSON da ogni file. 

``` json
File1.json
{
    "json": "record 1"
}
File2.json
{
    "json": "record 2"
}
File3.json
{
    "json": "record 3"
}
```

#### <a name="unquoted-column-names"></a>Nomi di colonna non racchiusi tra virgolette

Se è selezionato **nomi di colonna non racchiusi tra virgolette** , il mapping dei flussi di dati legge le colonne JSON che non sono racchiuse tra virgolette 

```
{ json: "record 1" }
{ json: "record 2" }
{ json: "record 3" }
```

#### <a name="has-comments"></a>Con commenti

Select **ha commenti** se i dati JSON sono di tipo C++ C o con commenti di stile.

``` json
{ "json": /** comment **/ "record 1" }
{ "json": "record 2" }
{ /** comment **/ "json": "record 3" }
```

#### <a name="single-quoted"></a>Virgoletta singola

Selezionare **singolo racchiuso tra** virgolette se i campi e i valori JSON usano virgolette singole anziché virgolette doppie.

```
{ 'json': 'record 1' }
{ 'json': 'record 2' }
{ 'json': 'record 3' }
```

#### <a name="backslash-escaped"></a>Barra rovesciata con escape

Selezionare **singolo racchiuso tra virgolette** se le barre rovesciate vengono usate per usare caratteri di escape nei dati JSON.

```
{ "json": "record 1" }
{ "json": "\} \" \' \\ \n \\n record 2" }
{ "json": "record 3" }
```

## <a name="next-steps"></a>Passaggi successivi

- [Panoramica dell'attività di copia](copy-activity-overview.md)
- [Mapping del flusso di dati](concepts-data-flow-overview.md)
- [Attività Lookup](control-flow-lookup-activity.md)
- [Attività GetMetadata](control-flow-get-metadata-activity.md)
