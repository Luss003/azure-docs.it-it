---
title: Azure AD guidata sicurezza ruoli in PIM-Azure Active Directory | Microsoft Docs
description: Descrive la procedura guidata relativa alla sicurezza che è possibile usare per convertire le assegnazioni di ruolo di Azure AD con privilegi permanenti in assegnazioni idonee usando Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: curtand
ms.custom: pim ; H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0d6d94df29ba16ecee06d70f5edac15a96a4299d
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804023"
---
# <a name="azure-ad-roles-security-wizard-in-pim"></a>Procedura guidata relativa alla sicurezza per i ruoli di Azure AD in PIM

Se si è la prima persona a eseguire Azure Active Directory (Azure AD) Privileged Identity Management (PIM) per l'organizzazione, verrà visualizzata una procedura guidata. La procedura guidata offre informazioni sui rischi di sicurezza delle identità con privilegi e su come usare PIM per ridurre tali rischi. Se si vuole farlo in seguito, non è necessario apportare modifiche alle assegnazioni dei ruoli esistenti nella procedura guidata.

## <a name="wizard-overview"></a>Panoramica della procedura guidata

Prima che l'organizzazione inizi a usare PIM, tutte le assegnazioni dei ruoli sono permanenti, ovvero gli utenti sono sempre presenti nei ruoli anche se in quel momento non necessitano dei privilegi del ruolo. Il primo passaggio della procedura guidata visualizza un elenco dei ruoli con privilegi elevati e il numero di utenti attualmente presenti in tali ruoli. È possibile visualizzare i dettagli di un ruolo particolare per visualizzare altre informazioni sugli utenti nel caso in cui uno o più utenti non siano noti.

Il secondo passaggio della procedura guidata offre la possibilità di modificare le assegnazioni dei ruoli di amministratore.  

> [!WARNING]
> È importante che siano presenti almeno un amministratore globale e più amministratori dei ruoli con privilegi con un account aziendale (non un account Microsoft). Se è presente un solo amministratore dei ruoli con privilegi, l'organizzazione non potrà gestire PIM se tale account viene eliminato.
> Mantenere inoltre assegnazioni permanenti di ruoli se l'utente dispone di un account Microsoft (usato per accedere a servizi Microsoft come Skype e Outlook.com). Se si prevede di richiedere l'autenticazione MFA per l'attivazione di questo ruolo, l'utente verrà bloccato.

## <a name="run-the-wizard"></a>Eseguire la procedura guidata

1. Accedere al [portale di Azure](https://portal.azure.com/).

1. Aprire **Azure AD Privileged Identity Management**.

1. Fare clic su **Ruoli di Azure AD** e quindi su **Procedura guidata**.

    ![Azure AD ruoli-pagina della procedura guidata che mostra i 3 passaggi per eseguire la procedura guidata](./media/pim-security-wizard/wizard-start.png)

1. Fare clic su **1 Individua i ruoli con privilegi**.

1. Esaminare l'elenco di ruoli con privilegi per verificare quali utenti sono permanenti o idonei.

    ![Individuazione dei ruoli con privilegi-riquadro del ruolo che Mostra membri permanenti e idonei](./media/pim-security-wizard/discover-privileged-roles-users.png)

1. Fare clic su **Avanti** per selezionare i membri da rendere idonei.

    ![Convertire i membri in una pagina idonea con le opzioni per selezionare i membri che si desidera rendere idonei per i ruoli](./media/pim-security-wizard/convert-members-eligible.png)

1. Dopo avere selezionato i membri, fare clic su **Avanti**.

    ![Pagina verifica modifiche che mostra i membri con assegnazioni di ruolo permanenti che verranno convertite](./media/pim-security-wizard/review-changes.png)

1. Fare clic su **OK** per convertire le assegnazioni permanenti in idonee.

    Al termine della conversione, verrà visualizzata una notifica.

    ![Notifica che mostra lo stato di una conversione](./media/pim-security-wizard/notification-completion.png)

Se è necessario convertire altre assegnazioni di ruolo con privilegi in idonee, è possibile eseguire nuovamente la procedura guidata. Se si vuole usare l'interfaccia PIM invece della procedura guidata, vedere [assegnare Azure ad ruoli in PIM](pim-how-to-add-role-to-user.md).

## <a name="next-steps"></a>Passaggi successivi

- [Assegnare ruoli Azure AD in PIM](pim-how-to-add-role-to-user.md)
- [Concedere l'accesso ad altri amministratori per gestire PIM](pim-how-to-give-access-to-pim.md)
