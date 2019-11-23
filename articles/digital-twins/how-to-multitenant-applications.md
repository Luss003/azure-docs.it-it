---
title: Enable multitenant applications - Azure Digital Twins | Microsoft Docs
description: Come configurare applicazioni multi-tenant di Azure Active Directory per Gemelli digitali di Azure.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 11/21/2019
ms.openlocfilehash: 65e1fa3fe371766566eeeaaa2d33479ea0243d61
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/22/2019
ms.locfileid: "74383348"
---
# <a name="enable-multitenant-applications-with-azure-digital-twins"></a>Abilitare le applicazioni multi-tenant con Gemelli digitali di Azure

Gli sviluppatori di soluzioni che usano Gemelli digitali di Azure potrebbero aver bisogno di supportare più clienti con un singolo servizio o soluzione. Le applicazioni *multi-tenant* sono infatti tra le più comuni configurazioni di Gemelli digitali di Azure.

Questo documento descrive come configurare un'app di Gemelli digitali di Azure per supportare diversi clienti e tenant di Azure Active Directory.

## <a name="multitenancy"></a>Multi-tenancy

Una risorsa *multi-tenant* è una singola istanza sottoposta a provisioning che supporta più clienti. Ogni cliente dispone di privilegi e dati indipendenti propri. L'esperienza di ogni cliente è separata da quella degli altri e quindi la "visione" dell'applicazione è diversa per ognuno.

Per altre informazioni sulla multi-tenancy, vedere [Applicazioni multi-tenant in Azure](https://docs.microsoft.com/azure/dotnet-develop-multitenant-applications).

## <a name="problem-scenario"></a>Scenario del problema

In questo scenario sono presenti uno sviluppatore che compila una soluzione di Gemelli digitali di Azure (**SVILUPPATORE**) e un cliente che usa tale soluzione (**CLIENTE**):

- Lo **SVILUPPATORE** ha una sottoscrizione di Azure con un tenant di Azure Active Directory.
- Lo **SVILUPPATORE** distribuisce un'istanza di Gemelli digitali di Azure nella propria sottoscrizione di Azure. Azure Active Directory ha automaticamente creato un'entità servizio nel tenant di Azure Active Directory dello **SVILUPPATORE**.
- Gli utenti nel tenant di Azure Active Directory dello **SVILUPPATORE** possono quindi [acquisire i token OAuth 2.0](./security-authenticating-apis.md) dal servizio Gemelli digitali di Azure.
- Lo **SVILUPPATORE** crea ora un'app per dispositivi mobili che si integra direttamente con le API di gestione di Gemelli digitali di Azure.
- Lo **SVILUPPATORE** consente al **CLIENTE** di usare l'applicazione per dispositivi mobili.
- Il **CLIENTE** deve essere autorizzato a usare l'API di gestione di Gemelli digitali di Azure all'interno dell'applicazione dello **SVILUPPATORE**.

Problema:

- Quando il **CLIENTE** accede all'applicazione dello **SVILUPPATORE**, l'app non riesce ad acquisire i token perché gli utenti del **CLIENTE** possano eseguire l'autenticazione con le API di gestione di Gemelli digitali di Azure.
- In Azure Active Directory viene generata un'eccezione indicante che Gemelli digitali di Azure non è riconosciuto nella directory del **CLIENTE**.

## <a name="problem-solution"></a>Soluzione del problema

Per risolvere il problema dello scenario precedente, è necessario eseguire le azioni seguenti per creare un'entità servizio di Gemelli digitali di Azure all'interno del tenant di Azure Active Directory del **CLIENTE**:

- Se il **CLIENTE** non ha già una sottoscrizione di Azure con un tenant di Azure Active Directory:

  - L'amministratore del tenant di Azure Active Directory del **CLIENTE** deve acquistare una [sottoscrizione di Azure con pagamento in base al consumo](https://azure.microsoft.com/offers/ms-azr-0003p/).
  - L'amministratore del tenant di Azure Active Directory del **CLIENTE** deve quindi [collegare il tenant alla nuova sottoscrizione](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity).

- Nel [portale di Azure](https://portal.azure.com) l'amministratore del tenant di Azure Active Directory del **CLIENTE** deve quindi seguire questa procedura:

  1. Search for **Subscriptions** in the top Azure search field. Selezionare **Sottoscrizioni**.
  1. Selezionare la sottoscrizione con il tenant di Azure Active Directory da usare nell'applicazione dello **SVILUPPATORE**.

     [![Azure Active Directory subscriptions](media/multitenant/ad-subscriptions.png)](media/multitenant/ad-subscriptions.png#lightbox)

  1. Selezionare **Provider di risorse**.
  1. Cercare **Microsoft.IoTSpaces**.
  1. Selezionare **Registra**.

     [![Azure Active Directory resource providers](media/multitenant/ad-resource-providers.png)](media/multitenant/ad-resource-providers.png#lightbox)
  
## <a name="next-steps"></a>Passaggi successivi

- Per altre informazioni su come usare le funzioni definite dall'utente con Gemelli digitali di Azure, vedere [Come creare funzioni definite dall'utente in Gemelli digitali di Azure](./how-to-user-defined-functions.md).

- Per informazioni su come usare il controllo degli accessi in base al ruolo per proteggere ulteriormente l'applicazione con assegnazioni di ruolo, vedere [Come creare e gestire il controllo degli accessi in base al ruolo in Gemelli digitali di Azure](./security-create-manage-role-assignments.md).
