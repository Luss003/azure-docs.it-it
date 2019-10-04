---
title: Elencare le assegnazioni di rifiuto per le risorse di Azure usando l'API REST - Azure | Microsoft Docs
description: Informazioni su come elencare le assegnazioni per utenti, gruppi e applicazioni che usano il controllo di accesso basato sui ruoli (RBAC) per le risorse di Azure e l'API REST di negazione.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 0bc49456f5965846a2de542b4a063bab2d1838bf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "67118296"
---
# <a name="list-deny-assignments-for-azure-resources-using-the-rest-api"></a>Elencare le assegnazioni di rifiuto per le risorse di Azure usando l'API REST

Le [assegnazioni di rifiuto](deny-assignments.md) impediscono agli utenti di eseguire azioni specifiche sulle risorse di Azure, anche se un'assegnazione di ruolo concede loro l'accesso. Questo articolo descrive come elencare negare le assegnazioni con l'API REST.

> [!NOTE]
> È possibile creare direttamente il proprio negare le assegnazioni. Per informazioni su come negare le assegnazioni vengono create, vedere [negare le assegnazioni](deny-assignments.md).

## <a name="prerequisites"></a>Prerequisiti

Per ottenere informazioni su un'assegnazione di negazione, è necessario disporre di:

- `Microsoft.Authorization/denyAssignments/read` autorizzazione di accesso, che è incluso nella maggior parte [ruoli predefiniti per le risorse di Azure](built-in-roles.md).

## <a name="list-a-single-deny-assignment"></a>Elencare una singola assegnazione di rifiuto

1. Iniziare con la richiesta seguente:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments/{deny-assignment-id}?api-version=2018-07-01-preview
    ```

1. All'interno dell'URI sostituire *{scope}* con l'ambito per il quale elencare le assegnazioni di rifiuto.

    | `Scope` | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Sottoscrizione |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Gruppo di risorse |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Sostituire *{deny-assignment-id}* con l'identificatore dell'assegnazione di rifiuto da recuperare.

## <a name="list-multiple-deny-assignments"></a>Elencare più assegnazioni di rifiuto

1. Iniziare con una delle richieste seguenti:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview
    ```

    Parametri facoltativi:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. All'interno dell'URI sostituire *{scope}* con l'ambito per il quale elencare le assegnazioni di rifiuto.

    | `Scope` | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Sottoscrizione |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Gruppo di risorse |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Sostituire *{filter}* con la condizione da applicare per filtrare l'elenco delle assegnazioni di rifiuto.

    | Filtro | Descrizione |
    | --- | --- |
    | (nessun filtro) | Elenca tutte le assegnazioni di ruolo in corrispondenza, al di sopra e al di sotto dell'ambito specificato. |
    | `$filter=atScope()` | Elenca le assegnazioni di rifiuto per il solo ambito specificato e quanto sopra. Le assegnazioni di rifiuto nei sottoambiti non sono incluse. |
    | `$filter=denyAssignmentName%20eq%20'{deny-assignment-name}'` | Elenca le assegnazioni di rifiuto con il nome specificato. |

## <a name="list-deny-assignments-at-the-root-scope-"></a>Elenca le assegnazioni di rifiuto all'ambito radice (/)

1. Eseguire con privilegi elevati l'accesso come descritto in [Eseguire con privilegi elevati l'accesso per l'amministratore globale in Azure Active Directory](elevate-access-global-admin.md).

1. Usare la richiesta seguente:

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. Sostituire *{filter}* con la condizione da applicare per filtrare l'elenco delle assegnazioni di rifiuto. È obbligatorio applicare un filtro.

    | Filtro | Descrizione |
    | --- | --- |
    | `$filter=atScope()` | Elenca le assegnazioni di rifiuto per il solo ambito radice. Le assegnazioni di rifiuto nei sottoambiti non sono incluse. |
    | `$filter=denyAssignmentName%20eq%20'{deny-assignment-name}'` | Elenca le assegnazioni di rifiuto con il nome specificato. |

1. Rimuove l'accesso con privilegi elevati.

## <a name="next-steps"></a>Passaggi successivi

- [Informazioni sulle assegnazioni di rifiuto per le risorse di Azure](deny-assignments.md)
- [Eseguire con privilegi elevati l'accesso per l'amministratore globale in Azure Active Directory](elevate-access-global-admin.md)
- [Informazioni di riferimento sulle API REST](/rest/api/azure/)
