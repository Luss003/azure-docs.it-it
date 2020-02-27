---
title: Elimina Azure Active Directory Domain Services | Microsoft Docs
description: Informazioni su come disabilitare o eliminare un Azure Active Directory Domain Services dominio gestito usando il portale di Azure
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 89e407e1-e1e0-49d1-8b89-de11484eee46
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 11/26/2019
ms.author: iainfou
ms.openlocfilehash: e1836f91b8afc1bb4f5b7e141949f3724c57c857
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/26/2020
ms.locfileid: "77614045"
---
# <a name="delete-an-azure-active-directory-domain-services-managed-domain-using-the-azure-portal"></a>Eliminare un Azure Active Directory Domain Services dominio gestito utilizzando il portale di Azure

Se un dominio gestito non è più necessario, è possibile eliminare un'istanza di Azure Active Directory Domain Services (Azure AD DS). Non è possibile disattivare o disabilitare temporaneamente un dominio gestito di Azure AD DS. L'eliminazione del dominio gestito Azure AD DS non elimina o influisce negativamente sul tenant del Azure AD. Questo articolo illustra come usare la portale di Azure per eliminare un dominio gestito Azure AD DS.

> [!WARNING]
> **L'eliminazione è permanente e non può essere invertita.**
> Quando si elimina un dominio gestito Azure AD DS, si verificano i passaggi seguenti:
>   * Viene effettuato il deprovisioning dei controller di dominio per il dominio gestito e vengono rimossi dalla rete virtuale.
>   * I dati del dominio gestito vengono eliminati definitivamente, Questi dati includono unità organizzative personalizzate, oggetti Criteri di gruppo, record DNS personalizzati, entità servizio, servizi gestiti e così via, creati dall'utente.
>   * I computer aggiunti al dominio gestito perdono la relazione di trust con il dominio e devono essere separati dal dominio.
>       * Non è possibile accedere a questi computer usando le credenziali di Active Directory aziendali. Al contrario, è necessario utilizzare le credenziali di amministratore locale per il computer.

## <a name="delete-the-managed-domain"></a>Eliminare il dominio gestito

Per eliminare un dominio gestito di Azure AD DS, completare i passaggi seguenti:

1. Nella portale di Azure cercare e selezionare **Azure ad Domain Services**.
1. Selezionare il nome del dominio gestito di Azure AD DS, ad esempio *aaddscontoso.com*.
1. Nella pagina **Panoramica** selezionare **Elimina**. Per confermare l'eliminazione, digitare di nuovo il nome di dominio del dominio gestito e quindi selezionare **Elimina**.

L'eliminazione del dominio gestito di Azure AD DS può richiedere più di 15-20 minuti.

## <a name="next-steps"></a>Passaggi successivi

Valutare la possibilità di [condividere commenti e suggerimenti][feedback] per le funzionalità che si desidera visualizzare in Azure AD DS.

Per iniziare a usare nuovamente Azure AD DS, vedere [creare e configurare un'istanza di Azure Active Directory Domain Services][create-instance].

<!-- INTERNAL LINKS -->
[feedback]: contact-us.md
[create-instance]: tutorial-create-instance.md
