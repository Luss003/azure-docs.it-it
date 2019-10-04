---
title: Caratteristiche dell'interazione di tenant multipli - Azure Active Directory | Microsoft Docs
description: Gestire i tenant di Azure Active Directory considerando i tenant come risorse completamente indipendenti
services: active-tenant
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.subservice: users-groups-roles
ms.date: 01/31/2019
ms.author: curtand
ms.custom: it-pro
ms.reviewer: sumitp
ms.collection: M365-identity-device-management
ms.openlocfilehash: 45f48b6d8ef29d14606f18d4ccee77bd742a670a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "60470073"
---
# <a name="understand-how-multiple-azure-active-directory-tenants-interact"></a>Informazioni sull'interazione di più tenant di Azure Active Directory

In Azure Active Directory (Azure AD) ogni tenant è una risorsa totalmente indipendente, ovvero un peer logicamente indipendente dagli altri tenant gestiti. Tra i tenant non esiste alcuna relazione padre-figlio. Questa indipendenza tra i tenant include l'indipendenza delle risorse, l'indipendenza amministrativa e l'indipendenza della sincronizzazione.

## <a name="resource-independence"></a>Indipendenza delle risorse.
* Se si crea o si elimina una risorsa in un tenant, ciò non influisce sulle risorse contenute in un altro tenant, con l'eccezione parziale degli utenti esterni. 
* Se si usa uno dei nomi di dominio con un tenant, questo non potrà essere usato con altri tenant.

## <a name="administrative-independence"></a>Indipendenza amministrativa
Se un utente non amministratore del tenant "Contoso" crea un tenant di test denominato "Test", si verifica quanto segue:

* Per impostazione predefinita, l'utente che crea un tenant viene aggiunto come utente esterno nel nuovo tenant e gli viene assegnato il ruolo di amministratore globale in quel tenant.
* Gli amministratori del tenant "Contoso" non dispongono di privilegi amministrativi diretti per il tenant "Test", a meno che questi privilegi non vengano specificamente concessi da un amministratore di "Test". Gli amministratori di "Contoso" possono tuttavia controllare l'accesso al tenant "Test" se controllano l'account utente che ha creato "Test".
* Se si aggiunge o si rimuove un ruolo di amministratore per un utente in un tenant, la modifica non influisce sui ruoli di amministratore che l'utente può avere in un altro tenant.

## <a name="synchronization-independence"></a>Indipendenza della sincronizzazione
È possibile configurare ogni tenant di Azure AD in modo indipendente per sincronizzare i dati da una singola istanza di uno degli elementi seguenti:

* Strumento Azure AD Connect, per sincronizzare i dati con una singola foresta AD.
* Connettore dei tenant di Azure Active Directory per Forefront Identity Manager, per sincronizzare i dati con una o più foreste locali e/o origini dati non Azure AD.

## <a name="add-an-azure-ad-tenant"></a>Aggiungere un tenant di Azure AD
Per aggiungere un tenant di Azure AD nel portale di Azure, accedere al [portale di Azure](https://portal.azure.com) con un account amministratore globale per Azure AD e selezionare **Nuovo** a sinistra.

> [!NOTE]
> A differenza di altre risorse di Azure, i tenant non sono risorse figlio di una sottoscrizione di Azure. Se la sottoscrizione di Azure è stata annullata o scaduta, è comunque possibile accedere ai dati di tenant usando Azure PowerShell, API Graph di Azure o l'interfaccia di amministrazione di Microsoft 365. È anche possibile [associare un'altra sottoscrizione al tenant](../fundamentals/active-directory-how-subscriptions-associated-directory.md).
>

## <a name="next-steps"></a>Passaggi successivi
Per un'ampia panoramica dei problemi relativi alle licenze di Azure AD e le procedure consigliate, vedere [Informazioni sulle licenze dei tenant di Azure Active Directory](../fundamentals/active-directory-licensing-whatis-azure-portal.md).
