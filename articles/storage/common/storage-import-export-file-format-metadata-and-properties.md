---
title: Formato di file dei metadati e delle proprietà del servizio Importazione/Esportazione di Azure | Documentazione Microsoft
description: Informazioni su come specificare i metadati e le proprietà per uno o più BLOB nell'ambito di un processo di importazione o esportazione.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 2066d4a2ed6db97285d92d15e14dbd21629dbdfa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "61478556"
---
# <a name="azure-importexport-service-metadata-and-properties-file-format"></a>Formato di file dei metadati e delle proprietà di Importazione/Esportazione di Azure
È possibile specificare i metadati e le proprietà per uno o più BLOB nell'ambito di un processo di importazione o esportazione. Per impostare i metadati o le proprietà per i BLOB che vengono creati nell'ambito di un processo di importazione, occorre fornire un file di metadati o delle proprietà nel disco rigido contenente i dati da importare. In un processo di esportazione, i metadati e le proprietà vengono scritti in un file di metadati o delle proprietà che viene incluso nel disco rigido restituito all'utente.  
  
## <a name="metadata-file-format"></a>Formato del file di metadati  
Il formato del file di metadati è indicato di seguito:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
[<metadata-name-1>metadata-value-1</metadata-name-1>]  
[<metadata-name-2>metadata-value-2</metadata-name-2>]  
. . .  
</Metadata>  
```
  
|Elemento XML|Type|Descrizione|  
|-----------------|----------|-----------------|  
|`Metadata`|Elemento radice|L'elemento radice del file di metadati.|  
|`metadata-name`|String|facoltativo. L'elemento XML specifica il nome dei metadati per il BLOB e il relativo valore specifica il valore dell'impostazione dei metadati.|  
  
## <a name="properties-file-format"></a>Formato del file delle proprietà  
Il formato di un file delle proprietà è indicato di seguito:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
[<Last-Modified>date-time-value</Last-Modified>]  
[<Etag>etag</Etag>]  
[<Content-Length>size-in-bytes<Content-Length>]  
[<Content-Type>content-type</Content-Type>]  
[<Content-MD5>content-md5</Content-MD5>]  
[<Content-Encoding>content-encoding</Content-Encoding>]  
[<Content-Language>content-language</Content-Language>]  
[<Cache-Control>cache-control</Cache-Control>]  
</Properties>  
```
  
|Elemento XML|Type|Descrizione|  
|-----------------|----------|-----------------|  
|`Properties`|Elemento radice|L'elemento radice del file delle proprietà.|  
|`Last-Modified`|String|facoltativo. L'ora dell'ultima modifica per il BLOB. Solo per i processi di esportazione.|  
|`Etag`|String|facoltativo. Il valore ETag del BLOB. Solo per i processi di esportazione.|  
|`Content-Length`|String|facoltativo. La dimensione del BLOB in byte. Solo per i processi di esportazione.|  
|`Content-Type`|String|facoltativo. Il tipo di contenuto del BLOB.|  
|`Content-MD5`|String|facoltativo. L'hash MD5 del BLOB.|  
|`Content-Encoding`|String|facoltativo. La codifica del contenuto del BLOB.|  
|`Content-Language`|String|facoltativo. La lingua del contenuto del BLOB.|  
|`Cache-Control`|String|facoltativo. La stringa di controllo della cache per il BLOB.|  

## <a name="next-steps"></a>Passaggi successivi

Per le regole dettagliate sull'impostazione dei metadati e delle proprietà di un BLOB, vedere [Set blob properties](/rest/api/storageservices/set-blob-properties) (Impostare le proprietà di un BLOB), [Set Blob Metadata](/rest/api/storageservices/set-blob-metadata) (Impostare i metadati di un BLOB) e [Setting and retrieving properties and metadata for blob resources](/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources) (Impostazione e recupero di proprietà e metadati per le risorse BLOB).
