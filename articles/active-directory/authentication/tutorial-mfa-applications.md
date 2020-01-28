---
title: Abilitazione di un gruppo pilota per Azure Multi-Factor Authentication
description: In questa esercitazione si abiliterà Azure AD Multi-Factor Authentication per un gruppo di utenti pilota
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: tutorial
ms.date: 07/11/2018
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 68474738aabde1b14752aa33789d7e40c3831908
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2020
ms.locfileid: "76154840"
---
# <a name="tutorial-complete-an-azure-multi-factor-authentication-pilot-roll-out"></a>Esercitazione: Completare l'implementazione di un gruppo pilota per Azure Multi-Factor Authentication

In questa esercitazione vengono descritte le procedure per la configurazione di criteri di accesso condizionale per l'abilitazione di Azure Multi-Factor Authentication (Azure MFA) quando si accede al portale di Azure. I criteri vengono distribuiti e testati per un gruppo specifico di utenti pilota. La distribuzione di Azure MFA con l'accesso condizionale offre una notevole flessibilità alle organizzazioni e agli amministratori rispetto al tradizionale metodo applicato.

> [!div class="checklist"]
> * Abilitare Azure Multi-Factor Authentication
> * Testare Azure Multi-Factor Authentication

## <a name="prerequisites"></a>Prerequisites

* Un tenant di Azure AD funzionante, con almeno una licenza di valutazione abilitata.
* Un account con privilegi di amministratore globale.
* Un utente di prova non amministratore con una password nota per il test. Se è necessario creare un utente, vedere l'articolo [Guida introduttiva: Aggiungere nuovi utenti ad Azure Active Directory](../add-users-azure-active-directory.md).
* Un gruppo pilota per il test di cui fare parte l'utente non amministratore. Se è necessario creare un gruppo, vedere l'articolo [Creare un gruppo e aggiungere membri in Azure Active Directory](../active-directory-groups-create-azure-portal.md).

## <a name="enable-azure-multi-factor-authentication"></a>Abilitare Azure Multi-Factor Authentication

1. Accedere al [portale di Azure](https://portal.azure.com) con un account amministratore globale.
1. Passare ad **Azure Active Directory** > **Sicurezza** > **Accesso condizionale**
1. Selezionare **Nuovi criteri**
1. Assegnare ai criteri il nome **MFA Pilot**
1. In **Utenti e gruppi** selezionare il pulsante di opzione **Seleziona utenti e gruppi**
    * Selezionare il gruppo pilota creato nella sezione dei prerequisiti di questo articolo.
    * Fare clic su **Fine**
1. In **App cloud** selezionare il pulsante di opzione **Selezionare le app**
    * L'app cloud per il portale di Azure è **Gestione di Microsoft Azure**
    * Fare clic su **Seleziona**
    * Fare clic su **Fine**
1. Ignorare la sezione **Condizioni**
1. In **Concedi** assicurarsi che il pulsante di opzione **Concedi accesso** sia selezionato
    * Selezionare la casella **Richiedi autenticazione a più fattori**
    * Fare clic su **Seleziona**
1. Ignorare la sezione **Sessione**
1. Impostare l'interruttore **Abilita criterio** su **Sì**
1. Fare clic su **Crea**

## <a name="test-azure-multi-factor-authentication"></a>Testare Azure Multi-Factor Authentication

Per dimostrare che i criteri di accesso condizionale funzionano, testare l'accesso a una risorsa che non richiede l'autenticazione MFA e quindi al portale di Azure che richiede l'autenticazione MFA.

1. Aprire una nuova finestra del browser in modalità InPrivate o in incognito e passare a [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com).
   * Eseguire l'accesso con l'utente di prova creato nella sezione dei prerequisiti di questo articolo e notare che non deve essere richiesto di completare l'autenticazione MFA.
   * Chiudere la finestra del browser.
2. Aprire una nuova finestra del browser in modalità InPrivate o in incognito e passare a [https://portal.azure.com](https://portal.azure.com).
   * Accedere con l'utente di prova creato nella sezione dei prerequisiti di questo articolo, notare che ora deve essere richiesto di eseguire la registrazione per l'autenticazione MFA e usare tale autenticazione.
   * Chiudere la finestra del browser.

## <a name="clean-up-resources"></a>Pulire le risorse

Se si decide di non volere più usare le funzionalità configurate nell'ambito di questa esercitazione, apportare la modifica seguente.

1. Accedere al [portale di Azure](https://portal.azure.com).
1. Passare ad **Azure Active Directory** > **Sicurezza** > **Accesso condizionale**.
1. Selezionare i criteri di accesso condizionale creati.
1. Fare clic su **Elimina**.

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione è stato abilitato Azure Multi-Factor Authentication. Continuare con l'esercitazione successiva per informazioni su come è possibile integrare Azure Identity Protection nelle esperienze di reimpostazione della password self-service e Multi-Factor Authentication.

> [!div class="nextstepaction"]
> [Valutare i rischi all'accesso](tutorial-risk-based-sspr-mfa.md)
