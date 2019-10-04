---
title: Come collegare una sottoscrizione di Azure - Azure Active Directory B2C | Microsoft Docs
description: Guida dettagliata all'abilitazione della fatturazione per tenant Azure AD B2C in una sottoscrizione di Azure.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 01/24/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 892f47b6acf22c62ce2290e2ede9d0bcd21eefc8
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/17/2019
ms.locfileid: "71065903"
---
# <a name="link-an-azure-subscription-to-an-azure-active-directory-b2c-tenant"></a>Collegare una sottoscrizione di Azure a un tenant di Azure Active Directory B2C

> [!IMPORTANT]
> Per informazioni aggiornate sulla fatturazione e sui prezzi di utilizzo per Azure Active Directory B2C (Azure AD B2C), vedere [prezzi di Azure ad B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/).

I costi per l'utilizzo di Azure AD B2C vengono addebitati a una sottoscrizione di Azure. Quando viene creato un tenant di Azure AD B2C, l'amministratore del tenant deve collegarlo in modo esplicito a una sottoscrizione di Azure. Questo articolo illustra i passaggi da eseguire.

> [!NOTE]
> È possibile usare una sottoscrizione collegata a un tenant di Azure AD B2C solo per la fatturazione dei costi di utilizzo di Azure AD B2C o di altre risorse di Azure, tra cui le risorse di Azure AD B2C.  La sottoscrizione non può essere usata per aggiungere altri servizi basati su licenza di Azure o licenze di Office 365 nel tenant di Azure AD B2C.

Questo collegamento alla sottoscrizione viene realizzato mediante la creazione di una "risorsa" di Azure AD B2C nella sottoscrizione di Azure di destinazione. È possibile creare molte "risorse" di Azure AD B2C all'interno di una singola sottoscrizione, insieme ad altre risorse di Azure, ad esempio macchine virtuali, archivi dati, app per la logica e così via. È possibile visualizzare tutte le risorse nella sottoscrizione passando al tenant di Azure AD cui è associata la sottoscrizione.

Le sottoscrizioni di Azure Cloud Solution Provider (CSP) sono supportate in Azure AD B2C. La funzionalità è disponibile tramite le API o il portale di Azure per Azure AD B2C e per tutte le risorse di Azure. Gli amministratori delle sottoscrizioni CSP possono collegare, spostare ed eliminare le relazioni con Azure AD B2C esattamente come per tutte le risorse di Azure. La gestione di Azure AD B2C tramite il controllo degli accessi in base al ruolo non è influenzata dall'associazione tra il tenant di Azure AD B2C e una sottoscrizione di Azure CSP. Per ottenere il controllo degli accessi in base al ruolo, usare i ruoli di base del tenant, non i ruoli basati su sottoscrizioni.

Per continuare, è necessaria una sottoscrizione di Azure valida.

## <a name="create-an-azure-ad-b2c-tenant"></a>Creare un tenant di Azure AD B2C

Prima di tutto, è necessario [creare il tenant di Azure AD B2C](active-directory-b2c-get-started.md) cui si vuole collegare una sottoscrizione. Ignorare questo passaggio se è già stato creato un tenant di Azure AD B2C.

## <a name="open-azure-portal-in-the-azure-ad-tenant-that-shows-your-azure-subscription"></a>Aprire il portale di Azure nel tenant di Azure AD che mostra la sottoscrizione di Azure

Passare al tenant di Azure AD che mostra la sottoscrizione di Azure. Aprire il [portale di Azure](https://portal.azure.com) e passare al tenant di Azure AD che mostra la sottoscrizione di Azure che si vuole usare.

![Passaggio al tenant di Azure AD](./media/active-directory-b2c-how-to-enable-billing/SelectAzureADTenant.png)

## <a name="find-azure-ad-b2c-in-the-azure-marketplace"></a>Trovare Azure Active Directory B2C in Azure Marketplace

Fare clic sul pulsante **Crea una risorsa**. Nel campo **Cerca nel Marketplace** immettere `Active Directory B2C`.

![Screenshot del portale con ' Active Directory B2C ' nella ricerca nel Marketplace](../../includes/media/active-directory-b2c-create-tenant/find-azure-ad-b2c.png)

Nell'elenco dei risultati selezionare **Azure Active Directory B2C**.

![Azure Active Directory B2C selezionato nell'elenco dei risultati](../../includes/media/active-directory-b2c-create-tenant/find-azure-ad-b2c-result.png)

Vengono visualizzate informazioni dettagliate su Azure AD B2C. Per iniziare la configurazione del nuovo tenant di Azure Active Directory B2C, fare clic sul pulsante **Crea**.

Nella schermata di creazione delle risorse selezionare **Collega un tenant Azure AD B2C esistente alla sottoscrizione di Azure**.

## <a name="create-an-azure-ad-b2c-resource-within-the-azure-subscription"></a>Creare una risorsa di Azure AD B2C nella sottoscrizione di Azure

Nella finestra di dialogo di creazione delle risorse selezionare un tenant di Azure AD B2C nell'elenco a discesa. Verranno visualizzati tutti i tenant di cui si è l'amministratore globale e quelli che non sono già collegati a una sottoscrizione.

Il nome della risorsa di Azure AD B2C sarà preselezionato in modo da corrispondere al nome di dominio del tenant di Azure AD B2C.

Per Sottoscrizione, selezionare una sottoscrizione attiva di Azure di cui si è l'amministratore.

Selezionare un gruppo di risorse e la località del gruppo di risorse. La selezione non influisce su località, prestazioni o stato di fatturazione del tenant di Azure AD B2C.

![Pagina di creazione della risorsa Azure AD B2C in portale di Azure](./media/active-directory-b2c-how-to-enable-billing/createresourceb2c.png)

## <a name="manage-your-azure-ad-b2c-tenant-resources"></a>Gestire le risorse del tenant di Azure AD B2C

Dopo aver creato una risorsa di Azure AD B2C nella sottoscrizione di Azure, verrà visualizzata una nuova risorsa di tipo "Tenant B2C" aggiunta insieme alle altre risorse di Azure.

È possibile usare questa risorsa per:

- Passare alla sottoscrizione per esaminare le informazioni di fatturazione.
- Passare al tenant di Azure AD B2C.
- Inviare una richiesta di supporto.
- Spostare la risorsa del tenant di Azure AD B2C in un'altra sottoscrizione di Azure o in un altro gruppo di risorse.

![Pagina Impostazioni risorse B2C nella portale di Azure](./media/active-directory-b2c-how-to-enable-billing/b2cresourcesettings.PNG)

## <a name="change-the-azure-ad-b2c-tenant-billing-subscription"></a>Modificare la sottoscrizione per la fatturazione di Azure AD B2C tenant

Azure AD B2C tenant possono essere spostati in un'altra sottoscrizione se le sottoscrizioni di origine e di destinazione sono presenti nello stesso tenant di Azure Active Directory.

Per informazioni su come spostare risorse di Azure come il tenant di Azure AD B2C in un'altra sottoscrizione, vedere [spostare le risorse in un gruppo di risorse o una sottoscrizione nuovi](../azure-resource-manager/resource-group-move-resources.md).

Prima di iniziare lo spostamento, assicurarsi di leggere l'intero articolo per comprendere completamente le limitazioni e i requisiti per tale spostamento. Oltre alle istruzioni per lo spostamento delle risorse, include informazioni critiche come un elenco di controllo di pre-spostamento e come convalidare l'operazione di spostamento.

## <a name="known-issues"></a>Problemi noti

### <a name="self-imposed-restrictions"></a>Restrizioni imposte personalmente

Un utente potrebbe aver stabilito una restrizione regionale per la creazione di risorse di Azure. Questa restrizione può impedire la creazione di una risorsa di Azure AD B2C. Per attenuate il problema, ridurre questa restrizione.

## <a name="next-steps"></a>Passaggi successivi

Dopo aver completato questa procedura per ogni tenant di Azure AD B2C, i costi della sottoscrizione di Azure vengono addebitati in base ai dettagli relativi al contratto Enterprise Agreement o ad Azure Direct.

È possibile esaminare le informazioni su utilizzo e fatturazione all'interno della sottoscrizione di Azure selezionata. È anche possibile esaminare report dettagliati sull'utilizzo giornaliero con l'[API di segnalazione dell'utilizzo](active-directory-b2c-reference-usage-reporting-api.md).
