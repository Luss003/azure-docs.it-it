---
title: Asset in servizi multimediali di Azure | Microsoft Docs
description: Questo articolo descrive le caratteristiche degli asset e come vengono usati da Servizi multimediali di Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 08/29/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 3dc1866a3c0339bca0c27fb53894a14581e88490
ms.sourcegitcommit: 88ae4396fec7ea56011f896a7c7c79af867c90a1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/06/2019
ms.locfileid: "70390487"
---
# <a name="assets"></a>Asset

In servizi multimediali di Azure, un [Asset](https://docs.microsoft.com/rest/api/media/assets) contiene informazioni sui file digitali archiviati in archiviazione di Azure (inclusi video, audio, immagini, raccolte di anteprime, tracce di testo e file di didascalia chiusi). 

Un asset viene mappato a un contenitore BLOB nell'[account di archiviazione di Azure](storage-account-concept.md) e i file contenuti nell'asset vengono archiviati come BLOB in blocchi in tale contenitore. Servizi multimediali supporta i livelli BLOB quando l'account usa l'archiviazione Utilizzo generico v2 (GPv2). Con GPv2, è possibile spostare i file in uno [spazio di archiviazione ad accesso sporadico o archivio](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers). L'archiviazione **archivio** è adatta all'archiviazione di file di origine quando non sono più necessari (ad esempio, dopo la codifica).

Il livello di archiviazione **archivio** è consigliato solo per file di origine di dimensioni molto estese già codificati e con l'output del processo di codifica inserito in un contenitore BLOB di output. I BLOB nel contenitore di output che si vuole associare a un asset e usare per trasmettere o analizzare il contenuto, devono esistere in un livello di archiviazione ad **accesso frequente** o ad **accesso sporadico**.

### <a name="naming-blobs"></a>Denominazione di BLOB

I nomi di file/BLOB all'interno di un asset devono rispettare i requisiti del [nome del BLOB](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) e i [requisiti del nome NTFS](https://docs.microsoft.com/windows/win32/fileio/naming-a-file). Il motivo di questi requisiti è che i file possono essere copiati dall'archiviazione BLOB a un disco NTFS locale per l'elaborazione.

## <a name="upload-digital-files-into-assets"></a>Caricare i file digitali negli asset

Dopo che i file digitali sono stati caricati nell'archiviazione e associati a un asset, possono essere usati nella codifica, nello streaming e nell'analisi dei flussi di lavoro del contenuto di servizi multimediali. Uno dei flussi di lavoro comuni di Servizi multimediali consiste nel caricare, codificare e trasmettere un file. Questa sezione descrive i passaggi generali.

> [!TIP]
> Prima di iniziare lo sviluppo, vedere [sviluppo con le API di servizi multimediali V3](media-services-apis-overview.md) (include informazioni sull'accesso alle API, alle convenzioni di denominazione e così via).

1. Usare l'API Servizi multimediali v3 per creare un nuovo asset "input". Questa operazione crea un contenitore nell'account di archiviazione associato all'account di Servizi multimediali. L'API restituisce il nome del contenitore (ad esempio, `"container": "asset-b8d8b68a-2d7f-4d8c-81bb-8c7bbbe67ee4"`).
   
    Se si ha già un contenitore BLOB che si vuole associare a un asset, è possibile specificare il nome del contenitore durante la creazione dell'asset. Servizi multimediali supporta attualmente solo i BLOB nella radice del contenitore e non quelli con i percorsi nel nome del file. Un contenitore con il nome file "input.mp4" andrà quindi bene. Un contenitore con il nome file "videos/inputs/input.mp4" non andrà invece bene.

    È possibile usare l'interfaccia della riga di comando di Azure per eseguire il caricamento direttamente in qualsiasi account di archiviazione e contenitore della sottoscrizione, per cui si hanno i diritti. <br/>Il nome del contenitore deve essere univoco e seguire le linee guida sulla denominazione delle risorse di archiviazione. Il nome non deve necessariamente seguire il formato dei nomi di contenitore di asset di Servizi multimediali (Asset-GUID). 
    
    ```azurecli
    az storage blob upload -f /path/to/file -c MyContainer -n MyBlob
    ```
2. Ottenere un URL di firma di accesso condiviso con autorizzazioni di lettura/scrittura che verranno usate per caricare i file digitali nel contenitore di asset. È possibile usare l'API Servizi multimediali per [elencare gli URL dei contenitori di asset](https://docs.microsoft.com/rest/api/media/assets/listcontainersas).
3. Usare le API di archiviazione di Azure o gli SDK (ad esempio, l' [API REST di archiviazione](../../storage/common/storage-rest-api-auth.md) o [.NET SDK](../../storage/blobs/storage-quickstart-blobs-dotnet.md)) per caricare i file nel contenitore di asset. 
4. Usare le API di Servizi multimediali v3 per creare una trasformazione e un processo per elaborare l'asset "input". Per altre informazioni, vedere [Trasformazioni e processi](transform-concept.md).
5. Trasmettere il contenuto dall'asset "output".

Per un esempio .NET completo che illustra come creare l'asset, ottenere un URL di firma di accesso condiviso scrivibile del contenitore dell'asset nella risorsa di archiviazione e caricare il file nel contenitore nella risorsa di archiviazione usando l'URL di firma di accesso condiviso, vedere [Creare un input del processo da un file locale](job-input-from-local-file-how-to.md).

### <a name="create-a-new-asset"></a>Creare un nuovo asset

> [!NOTE]
> Le proprietà dell'asset di tipo Datetime sono sempre in formato UTC.

#### <a name="rest"></a>REST

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{amsAccountName}/assets/{assetName}?api-version=2018-07-01
```

Per un esempio REST, vedere l'esempio [Create an Asset with REST](https://docs.microsoft.com/rest/api/media/assets/createorupdate#examples) (Creare un asset con REST).

L'esempio illustra come creare il **corpo della richiesta** in cui è possibile specificare utili informazioni come la descrizione, il nome del contenitore, l'account di archiviazione e altre informazioni.

#### <a name="curl"></a>cURL

```cURL
curl -X PUT \
  'https://management.azure.com/subscriptions/00000000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Media/mediaServices/amsAccountName/assets/myOutputAsset?api-version=2018-07-01' \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "properties": {
    "description": "",
  }
}'
```

#### <a name="net"></a>.NET

```csharp
 Asset asset = await client.Assets.CreateOrUpdateAsync(resourceGroupName, accountName, assetName, new Asset());
```

Per un esempio completo, vedere [Creare un input del processo da un file locale](job-input-from-local-file-how-to.md). In Servizi multimediali v3 l'input di un processo può essere creato anche da URL HTTPS. Vedere [Creare un input del processo da un URL HTTPS](job-input-from-http-how-to.md).

## <a name="map-v3-asset-properties-to-v2"></a>Mappare le proprietà Asset V3 alla versione V2

La tabella seguente illustra in che modo le proprietà dell' [Asset](https://docs.microsoft.com/rest/api/media/assets/createorupdate#asset)in V3 sono mappate alle proprietà dell'asset nella versione V2.

|Proprietà V3|V2 (proprietà)|
|---|---|
|ID-(univoco) il percorso di Azure Resource Manager completo, vedere esempi nell' [Asset](https://docs.microsoft.com/rest/api/media/assets/createorupdate)||
|nome-(univoco) vedere [convenzioni di denominazione](media-services-apis-overview.md#naming-conventions) ||
|alternateId|AlternateId|
|assetId|Il valore ID-(Unique) inizia con `nb:cid:UUID:` il prefisso.|
|creato|Data creazione|
|description|NOME|
|LastModified|LastModified|
|storageAccountName|StorageAccountName|
|storageEncryptionFormat| Opzioni (opzioni di creazione)|
|type||

## <a name="storage-side-encryption"></a>Crittografia lato archiviazione

Per proteggere gli asset inattivi, è necessario crittografarli tramite crittografia lato archiviazione. La tabella seguente illustra il funzionamento della crittografia lato archiviazione in Servizi multimediali:

|Opzione di crittografia|Descrizione|Servizi multimediali v2|Servizi multimediali v3|
|---|---|---|---|
|Crittografia di archiviazione di Servizi multimediali|Crittografia AES-256, chiave gestita da Servizi multimediali|Supportata<sup>(1)</sup>|Non supportata<sup>(2)</sup>|
|[Crittografia del servizio di archiviazione per dati inattivi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Crittografia lato server offerta da Archiviazione di Azure, chiave gestita da Azure o dal cliente|Supportato|Supportato|
|[Crittografia lato client di archiviazione](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Crittografia lato client offerta da Archiviazione di Azure, chiave gestita dal cliente in Key Vault|Non supportate|Non supportate|

<sup>1</sup> Servizi multimediali supporta la gestione di contenuto non crittografato/senza alcuna forma di crittografia, ma tale modalità non è consigliata.

<sup>2</sup> In Servizi multimediali versione 3, la crittografia di archiviazione (crittografia AES-256) è supportata per la compatibilità con le versioni precedenti solo se gli asset sono stati creati con Servizi multimediali versione 2. In altre parole, la versione 3 funziona con asset con crittografia di archiviazione esistenti, ma non consente la creazione di nuovi asset di questo tipo.

## <a name="filtering-ordering-paging"></a>Filtro, ordinamento, paging

Vedere [Applicazione di filtri, ordinamento e restituzione di più pagine delle entità di Servizi multimediali](entities-overview.md).

## <a name="next-steps"></a>Passaggi successivi

* [Eseguire lo streaming di un file](stream-files-dotnet-quickstart.md)
* [Utilizzo di un DVR cloud](live-event-cloud-dvr.md)
* [Differenze tra Servizi multimediali v2 e v3](migrate-from-v2-to-v3.md)
