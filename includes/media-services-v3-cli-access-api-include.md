---
title: File di inclusione
description: File di inclusione
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 05/01/2019
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: 58e9156091702718dccd75eb4a57e5b6d8c1f073
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/31/2020
ms.locfileid: "76896320"
---
## <a name="access-the-media-services-api"></a>Accedere all'API di Servizi multimediali

Per connettersi alle API di Servizi multimediali di Azure, si usa l'autenticazione dell'entità servizio di Azure AD. Il comando seguente consente di creare un'applicazione Azure AD e di collegare un'entità servizio all'account. È consigliabile usare i valori restituiti per configurare l'applicazione.

Prima di eseguire lo script, è necessario sostituire `amsaccount` e `amsResourceGroup` con i nomi scelti durante la creazione di queste risorse. `amsaccount` è il nome dell'account di Servizi multimediali di Azure al quale collegare l'entità servizio.

Se si ha accesso a più sottoscrizioni, impostare prima di tutto come sottoscrizione attiva la sottoscrizione in cui è stato creato l'account Servizi multimediali.

```azurecli
az account set --subscription subscriptionId
```

Il comando seguente restituisce un output `json`:

```azurecli
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup
```

Con questo comando viene generata una risposta simile alla seguente:

```json
{
  "AadClientId": "00000000-0000-0000-0000-000000000000",
  "AadEndpoint": "https://login.microsoftonline.com",
  "AadSecret": "00000000-0000-0000-0000-000000000000",
  "AadTenantId": "00000000-0000-0000-0000-000000000000",
  "AccountName": "amsaccount",
  "ArmAadAudience": "https://management.core.windows.net/",
  "ArmEndpoint": "https://management.azure.com/",
  "Region": "West US 2",
  "ResourceGroup": "amsResourceGroup",
  "SubscriptionId": "00000000-0000-0000-0000-000000000000"
}
```

Se si vuole ottenere un output `xml` nella risposta, usare il comando seguente:

```azurecli
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup --xml
```
