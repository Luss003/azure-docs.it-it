---
title: Parametri e intestazioni comuni
description: Parametri e intestazioni comuni a tutte le operazioni eseguibili con le risorse di Key Vault.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: 7ec8e1df71e6513b13e9c37174a3363471be01d9
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/11/2019
ms.locfileid: "70879276"
---
# <a name="common-parameters-and-headers"></a>Parametri e intestazioni comuni

Le seguenti informazioni sono comuni a tutte le operazioni che è possibile eseguire in relazione alle risorse di Key Vault:

- Sostituire `{api-version}` con la versione dell'API nell'URI.
- Sostituire `{subscription-id}` con l'identificatore della sottoscrizione nell'URI
- Sostituire `{resource-group-name}` con il gruppo di risorse. Per altre informazioni, vedere Uso di gruppi di risorse per gestire le risorse di Azure.
- Sostituire `{vault-name}` con il nome dell'insieme di credenziali delle chiavi nell'URI.
- Impostare l'intestazione Content-Type su application/json.
- Impostare l'intestazione dell'autorizzazione su un token JSON Web ottenuto da Azure Active Directory. Per altre informazioni, vedere [Autenticazione delle richieste di Azure Resource Manager](authentication-requests-and-responses.md).

## <a name="common-error-response"></a>Risposta di errore comune
Il servizio userà i codici di stato HTTP per indicare l'esito positivo o negativo. Gli errori contengono inoltre una risposta nel formato seguente:

```
   {  
     "error": {  
     "code": "BadRequest",  
     "message": "The key vault sku is invalid."  
     }  
   }  
```

|Nome dell'elemento | Type | DESCRIZIONE |
|---|---|---|
| code | string | Il tipo di errore che si è verificato.|
| message | string | Descrizione della causa dell'errore. |



## <a name="see-also"></a>Vedere anche
 [Informazioni di riferimento sull'API REST di Azure Key Vault](/rest/api/keyvault/)
