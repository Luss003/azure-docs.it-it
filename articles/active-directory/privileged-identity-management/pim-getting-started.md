---
title: Iniziare a usare PIM-Azure Active Directory | Microsoft Docs
description: Informazioni su come iniziare a usare Azure AD Privileged Identity Management (PIM) nel portale di Azure.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: pim
ms.topic: conceptual
ms.workload: identity
ms.date: 04/09/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 018cc54a770a0d18fa9af3d93fec2f5f720ce17b
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804523"
---
# <a name="start-using-pim"></a>Iniziare a usare PIM

Con Azure Active Directory Privileged Identity Management (PIM) è possibile gestire, controllare e monitorare l'accesso nell'organizzazione. Questo ambito include l'accesso alle risorse di Azure, Azure AD e altri servizi Microsoft online, ad esempio Office 365 o Microsoft Intune.

Questo articolo descrive come abilitare e iniziare a usare PIM.

## <a name="prerequisites"></a>Prerequisiti

Per usare PIM, è necessario avere una delle licenze seguenti:

- Azure AD P2 Premium
- Enterprise Mobility + Security (EMS) E5

Per altre informazioni, vedere [Requisiti della licenza per usare PIM](subscription-requirements.md).

## <a name="first-person-to-use-pim"></a>Prima persona a usare PIM

Alla prima persona che usa PIM nella directory vengono assegnati automaticamente i ruoli di [Amministratore della sicurezza](../users-groups-roles/directory-assign-admin-roles.md#security-administrator) e [Amministratore dei ruoli con privilegi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) nella directory. Solo gli amministratori dei ruoli con privilegi possono gestire Azure AD assegnazioni di ruolo degli utenti. È inoltre possibile scegliere di eseguire la [procedura guidata relativa alla sicurezza](pim-security-wizard.md) che guida nei passaggi inziali di individuazione e assegnazione.

## <a name="enable-pim"></a>Abilitare PIM

Per iniziare a usare PIM nella directory, è prima di tutto necessario abilitare PIM.

1. Accedere al [portale di Azure](https://portal.azure.com/) come amministratore globale della directory.

    Per abilitare PIM per una directory, è necessario essere un amministratore globale con un account aziendale, ad esempio @yourdomain.com, e non un account Microsoft, ad esempio @outlook.com.

1. Fare clic su **Tutti i servizi** e trovare il servizio **Azure AD Privileged Identity Management**.

    ![Azure AD Privileged Identity Management in Tutti i servizi](./media/pim-getting-started/pim-all-services-find.png)

1. Fare clic per aprire il riquadro di avvio rapido di PIM.

1. Nell'elenco fare clic su **Concedi consenso per PIM**.

    ![Consenso a PIM per abilitare PIM](./media/pim-getting-started/consent-pim.png)

1. Fare clic su **Verifica l'identità dell'utente** per verificare l'identità con Azure MFA. Verrà chiesto di scegliere un account.

    ![Selezionare una finestra dell'account per verificare la propria identità](./media/pim-getting-started/pick-account.png)

1. Se sono necessarie altre informazioni per la verifica, si verrà guidati nel processo. Per altre informazioni, vedere [Informazioni sulla verifica in due passaggi](https://go.microsoft.com/fwlink/p/?LinkId=708614).

    ![Altre informazioni richiedono la finestra se l'organizzazione necessita di ulteriori informazioni](./media/pim-getting-started/more-information-required.png)

    Ad esempio, potrebbe venire chiesto di fornire la verifica telefonica.

    ![Pagina di verifica aggiuntiva di sicurezza che chiede come contattare l'utente](./media/pim-getting-started/additional-security-verification.png)

1. Dopo aver completato il processo di verifica, fare clic sul pulsante **Consenso**.

1. Nel messaggio visualizzato fare clic su **Sì** per fornire il consenso per il servizio PIM.

    ![Consenso al messaggio PIM per completare il processo di consenso](./media/pim-getting-started/consent-pim-message.png)

## <a name="sign-up-pim-for-azure-ad-roles"></a>Iscrivere PIM ai ruoli di Azure AD

Dopo aver abilitato PIM per la directory, è necessario effettuare l'iscrizione di PIM per la gestione dei ruoli di Azure AD.

1. Aprire **Azure AD Privileged Identity Management**.

1. Fare clic su **Ruoli di Azure AD**.

    ![Iscrivere PIM ai ruoli di Azure AD](./media/pim-getting-started/sign-up-pim-azure-ad-roles.png)

1. Fare clic su **Iscrizione**.

1. Nel messaggio visualizzato fare clic su **Sì** per effettuare l'iscrizione di PIM per la gestione dei ruoli di Azure AD.

    ![Messaggio di iscrizione di PIM ai ruoli di Azure AD](./media/pim-getting-started/sign-up-pim-message.png)

    Al termine dell'iscrizione, verranno abilitate le opzioni di Azure AD. Potrebbe essere necessario aggiornare il portale.

    Per informazioni su come individuare e selezionare le risorse di Azure per proteggerle con PIM, vedere [Individuare le risorse di Azure per la gestione in PIM](pim-resource-roles-discover-resources.md).

## <a name="navigate-to-your-tasks"></a>Accedere alle attività

Dopo aver configurato PIM, è possibile eseguire le attività di gestione delle identità.

![Finestra di navigazione in PIM che mostra le attività e Gestisci le opzioni](./media/pim-getting-started/pim-quickstart-tasks.png)

| Attività e gestione | DESCRIZIONE |
| --- | --- |
| **Ruoli personali**  | Visualizza un elenco di ruoli idonei e attivi assegnati all'utente. Qui è possibile attivare tutti i ruoli idonei assegnati. |
| **Richieste personali** | Visualizza le richieste in sospeso per attivare le assegnazioni di ruolo idonee. |
| **Approvare le richieste** | Visualizza un elenco di richieste per attivare i ruoli idonei per gli utenti della directory che possono essere approvati. |
| **Verificare l'accesso** | Elenca le verifiche di accesso attive assegnate all'utente per essere completate, sia per se stesso che per altri utenti. |
| **Ruoli di Azure AD** | Visualizza un dashboard e le impostazioni per gli amministratori dei ruoli con privilegi per gestire le assegnazioni di ruolo Azure AD. Per gli utenti che non sono amministratori dei ruoli con privilegi, questo dashboard è disabilitato. Questi utenti possono accedere a un dashboard speciale denominato My view (Visualizzazione personalizzata). Il dashboard My view (Visualizzazione personalizzata) visualizza solo informazioni relative all'accesso dell'utente al dashboard, non l'intero tenant. |
| **Risorse di Azure** | Visualizza un dashboard e le impostazioni per gli amministratori dei ruoli con privilegi per gestire le assegnazioni dei ruoli della risorsa di Azure AD. Per gli utenti che non sono amministratori dei ruoli con privilegi, questo dashboard è disabilitato. Questi utenti possono accedere a un dashboard speciale denominato My view (Visualizzazione personalizzata). Il dashboard My view (Visualizzazione personalizzata) visualizza solo informazioni relative all'accesso dell'utente al dashboard, non l'intero tenant. |

## <a name="add-a-pim-tile-to-the-dashboard"></a>Aggiungere un riquadro PIM al dashboard

Per semplificare l'apertura di PIM, è necessario aggiungere un riquadro PIM al dashboard del portale di Azure.

1. Accedere al [portale di Azure](https://portal.azure.com/).

1. Fare clic su **Tutti i servizi** e trovare il servizio **Azure AD Privileged Identity Management**.

    ![Azure AD Privileged Identity Management in Tutti i servizi](./media/pim-getting-started/pim-all-services-find.png)

1. Fare clic per aprire il riquadro di avvio rapido di PIM.

1. Selezionare **Pin blade to dashboard** (Aggiungi pannello al dashboard) per aggiungere il pannello di avvio rapido di PIM al dashboard.

    ![Icona a puntina da disegno per aggiungere il pannello PIM al dashboard](./media/pim-getting-started/pim-quickstart-pin-to-dashboard.png)

    Nel dashboard di Azure viene visualizzato un riquadro simile al seguente:

    ![Riquadro avvio rapido PIM nel dashboard](./media/pim-getting-started/pim-quickstart-dashboard-tile.png)

## <a name="next-steps"></a>Passaggi successivi

- [Assegnare ruoli Azure AD in PIM](pim-how-to-add-role-to-user.md)
- [Individuare le risorse di Azure per la gestione in PIM](pim-resource-roles-discover-resources.md)
