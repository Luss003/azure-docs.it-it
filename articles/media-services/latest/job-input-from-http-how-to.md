---
title: Creare un input del processo di Servizi multimediali di Azure da un URL HTTPS | Microsoft Docs
description: Questo argomento illustra come creare un input del processo da un URL HTTP(s).
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/13/2019
ms.author: juliako
ms.openlocfilehash: f6eee912bb3bba112bd13969f1a8d9cb5748e387
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "65413821"
---
# <a name="create-a-job-input-from-an-https-url"></a>Creare un input del processo da un URL HTTPS

In Servizi multimediali v3, quando si inviano processi per elaborare i video, è necessario indicare a Servizi multimediali dove trovare il video di input. Una delle opzioni consiste nello specificare un URL HTTPS come input del processo, come illustrato in questo esempio. Attualmente AMS v3 non supporta la codifica di trasferimenti in blocchi su URL HTTPS. Per un esempio completo, vedere questo [esempio di GitHub](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

> [!TIP]
> Prima di iniziare lo sviluppo, esaminare [lo sviluppo con le API di servizi multimediali v3](media-services-apis-overview.md) (include informazioni sull'accesso alle API, le convenzioni di denominazione e così via).

## <a name="net-sample"></a>Esempio .NET

Il codice seguente mostra come creare un processo da un input URL HTTP(s).

[!code-csharp[Main](../../../media-services-v3-dotnet-quickstarts/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="job-error-codes"></a>Codici di errore dei processi

Vedere i [codici di errore](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode).

## <a name="next-steps"></a>Passaggi successivi

[Creare un input del processo da un file locale](job-input-from-local-file-how-to.md).
