---
title: Metodi di autenticazione | Mappe Microsoft Azure
description: In questo articolo si apprenderà come Azure Active Directory (Azure AD) o l'autenticazione con chiave condivisa per l'uso di Microsoft Azure Maps Services. Informazioni su come ottenere la chiave di sottoscrizione di Azure maps.
author: walsehgal
ms.author: v-musehg
ms.date: 12/30/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 006adae99b2430f4c08ce5fc692598e48f45c239
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/12/2020
ms.locfileid: "75911832"
---
# <a name="authentication-with-azure-maps"></a>Autenticazione con Mappe di Azure

Azure Maps supporta due modi per autenticare le richieste: chiave condivisa e Azure Active Directory (Azure AD). Questo articolo illustra questi metodi di autenticazione per guidare il processo di implementazione.

## <a name="shared-key-authentication"></a>Autenticazione con chiave condivisa

L'autenticazione con chiave condivisa passa le chiavi generate da un account Azure Maps a ogni richiesta a Maps di Azure. Per ogni richiesta ai servizi di Azure Maps, la *chiave di sottoscrizione* deve essere aggiunta come parametro all'URL. Le chiavi primarie e secondarie vengono generate dopo la creazione dell'account Azure maps. Si consiglia di usare la chiave primaria come chiave di sottoscrizione quando si chiama Azure Maps usando l'autenticazione con chiave condivisa. La chiave secondaria può essere usata in scenari come le modifiche delle chiavi in sequenza.  

Per informazioni sulla visualizzazione delle chiavi nell'portale di Azure, vedere [Manage Authentication](https://aka.ms/amauthdetails).

> [!Tip]
> È consigliabile rigenerare le chiavi regolarmente. Vengono fornite due chiavi per mantenere attive le connessioni con una chiave durante la rigenerazione dell'altra. Quando si rigenerano le chiavi è necessario aggiornare tutte le applicazioni che accedono all'account per usare le nuove chiavi.



## <a name="authentication-with-azure-active-directory-preview"></a>Autenticazione con Azure Active Directory (anteprima)

Mappe di Azure offre ora l'integrazione di [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) per l'autenticazione delle richieste per i servizi di Mappe di Azure. Azure AD fornisce l'autenticazione basata sull'identità, incluso il [controllo degli accessi in base al ruolo](https://docs.microsoft.com/azure/role-based-access-control/overview), per concedere l'accesso a livello di utente, a livello di gruppo e a livello di applicazione alle risorse di Azure maps. Le sezioni seguenti illustrano i concetti e i componenti dell'integrazione di Mappe di Azure con Azure AD.

## <a name="authentication-with-oauth-access-tokens"></a>Autenticazione con token di accesso OAuth

Mappe di Azure accetta token di accesso **OAuth 2.0** per i tenant di Azure AD associati a una sottoscrizione di Azure contenente un account di Mappe di Azure. Mappe di Azure accetta token per:

* Utenti di Azure AD. 
* Applicazioni partner che usano autorizzazioni delegate dagli utenti.
* Identità gestite per le risorse di Azure.

Mappe di Azure genera un *identificatore univoco (ID client)* per ogni account Mappe di Azure. Quando si combina questo ID client con parametri aggiuntivi, è possibile richiedere i token da Azure AD specificando i valori nella tabella seguente, a seconda dell'ambiente di Azure.

| Ambiente Azure   | Endpoint token Azure AD |
| --------------------|-------------------------|
| Azure Public        | https://login.microsoftonline.com |
| Azure per enti pubblici    | https://login.microsoftonline.us |


Per altre informazioni su come configurare Azure AD e richiedere i token per Mappe di Azure, vedere [Gestire l'autenticazione in Mappe di Azure](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

Per informazioni generali su come richiedere token da Azure AD, vedere [Informazioni sull'autenticazione](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).

## <a name="request-azure-map-resources-with-oauth-tokens"></a>Richiedere risorse di Mappe di Azure con token OAuth

Una volta ricevuto un token da Azure AD, è possibile inviare una richiesta a Mappe di Azure impostando le due seguenti intestazioni di richiesta necessarie:

| Intestazione della richiesta    |    Valore    |
|:------------------|:------------|
| x-ms-client-id    | 30d7cc….9f55|
| Autorizzazione     | Bearer eyJ0e….HNIVN |

> [!Note]
> `x-ms-client-id` è il GUID basato sull'account di Mappe di Azure che compare nella pagina di autenticazione di Mappe di Azure.

Ecco un esempio di instradamento delle richieste di Mappe di Azure che usa un token OAuth:

```
GET /route/directions/json?api-version=1.0&query=52.50931,13.42936:52.50274,13.43872 
Host: atlas.microsoft.com 
x-ms-client-id: 30d7cc….9f55 
Authorization: Bearer eyJ0e….HNIVN 
```

Per informazioni sulla visualizzazione dell'ID client, vedere [Visualizzare i dettagli di autenticazione](https://aka.ms/amauthdetails).

## <a name="control-access-with-rbac"></a>Controllare l'accesso con il controllo degli accessi in base al ruolo

Azure AD consente di controllare l'accesso alle risorse protette utilizzando il controllo degli accessi in base al ruolo. Dopo aver creato l'account Azure Maps e aver registrato l'applicazione Azure Maps Azure AD all'interno del tenant Azure AD, è possibile configurare il controllo degli accessi in base al ruolo per un utente, un gruppo, un'applicazione o una risorsa di Azure nella pagina del portale per gli account di Azure maps.

Azure Maps supporta il controllo di accesso in lettura per singoli utenti, gruppi, applicazioni e servizi di Azure di Azure AD tramite identità gestite per le risorse di Azure.

![Lettore di dati per Mappe di Azure (anteprima)](./media/azure-maps-authentication/concept.png)

Per informazioni su come visualizzare le impostazioni del controllo degli accessi in base al ruolo, vedere [Come configurare il controllo degli accessi in base al ruolo per Mappe di Azure](https://aka.ms/amrbac).

## <a name="managed-identities-for-azure-resources-and-azure-maps"></a>Identità gestite per risorse di Azure e Mappe di Azure

Le [identità gestite per le risorse di Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) forniscono ai servizi di Azure (Servizio app di Azure, Funzioni di Azure, Macchine virtuali di Microsoft Azure e così via) un'identità gestita automaticamente che può essere autorizzata per l'accesso ai servizi di Mappe di Azure.  

## <a name="next-steps"></a>Passaggi successivi

* Per altre informazioni sull'autenticazione di un'applicazione con Azure AD e Mappe di Azure, vedere [Gestire l'autenticazione in Mappe di Azure](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

* Per altre informazioni sull'autenticazione del controllo mappa di Mappe di Azure e di Azure AD, vedere [Usare il controllo mappa di Mappe di Azure](https://aka.ms/amaadmc).
