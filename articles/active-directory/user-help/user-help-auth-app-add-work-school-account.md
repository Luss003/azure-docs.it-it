---
title: Aggiungere un account aziendale o dell'Istituto di istruzione all'app Microsoft Authenticator-Azure AD
description: Aggiungere l'account aziendale o dell'Istituto di istruzione all'app Microsoft Authenticator per verificare la propria identità durante l'uso della verifica a due fattori.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 01/24/2019
ms.author: curtand
ms.reviewer: olhaun
ms.openlocfilehash: f0cc14a53f7ead7f0a496728d477d7d30857a0fb
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/07/2020
ms.locfileid: "77063918"
---
# <a name="add-your-work-or-school-account-to-the-microsoft-authenticator-app"></a>Aggiungere l'account aziendale o dell'Istituto di istruzione all'app Microsoft Authenticator

Se l'organizzazione usa la verifica a due fattori, è possibile impostare l'account aziendale o dell'istituto di istruzione in modo da usare l'app Microsoft Authenticator come uno dei metodi di verifica.

>[!Important]
>Prima di poter aggiungere l'account, è necessario scaricare e installare l'app Microsoft Authenticator. Se non è ancora stato fatto, seguire i passaggi descritti nell'articolo [Scaricare e installare l'app](user-help-auth-app-download-install.md).

## <a name="add-your-work-or-school-account"></a>Aggiungere l'account aziendale o dell'istituto di istruzione

1. Nel computer in uso andare alla pagina [Verifica aggiuntiva di sicurezza](https://account.activedirectory.windowsazure.com/proofup.aspx?proofup=1).

    >[!Note]
    >Se non viene visualizzata la pagina **Verifica aggiuntiva di sicurezza**, è possibile che l'amministratore abbia attivato l'esperienza informazioni di sicurezza (anteprima). In tal caso è necessario seguire le istruzioni descritte nella sezione [Configurare le informazioni di sicurezza per l'uso di un'app di autenticazione](security-info-setup-auth-app.md). Se non è questo il caso, è necessario contattare l'help desk dell'organizzazione per ricevere assistenza. Per altre informazioni sulle informazioni di sicurezza, vedere [Panoramica delle informazioni di sicurezza (anteprima)](user-help-security-info-overview.md).

2. Selezionare la casella accanto ad **App Authenticator** e quindi selezionare **Configura**.

    Viene visualizzata la pagina **Configurare l'app per dispositivi mobili**.

    ![Schermata contenente il codice a matrice](./media/user-help-auth-app-download-install/auth-app-barcode.png)

3. Aprire l'app Microsoft Authenticator, selezionare **Aggiungi account** dall'icona **Customize and control** (Personalizza e controlla) nell'angolo in alto a destra e quindi selezionare **Account aziendale o dell'istituto di istruzione**.

    >[!Note]
    >Se si configura l'app Microsoft Authenticator per la prima volta, è possibile che venga visualizzato un messaggio in cui viene richiesto se consentire all'app di accedere alla fotocamera (iOS) o se consentire all'app di scattare foto e registrare video (Android). Selezionare **Consenti** per consentire all'app Authenticator di accedere alla fotocamera per scattare una foto del codice a matrice nel passaggio successivo. Se non si consente l'accesso alla fotocamera, è comunque possibile configurare l'app Authenticator, ma sarà necessario aggiungere le informazioni sul codice manualmente. Per informazioni su come aggiungere manualmente il codice, vedere [aggiungere manualmente un account all'app](user-help-auth-app-add-account-manual.md).

4. Usare la fotocamera del dispositivo per eseguire la scansione del codice a matrice dalla schermata **Configura l'app per dispositivi mobili** del computer e quindi scegliere **Fine**.

    >[!Note]
    >Se la fotocamera non è in grado di acquisire il codice a matrice, è possibile aggiungere manualmente le informazioni dell'account all'app Microsoft Authenticator per la verifica a due fattori. Per altre informazioni e istruzioni su come eseguire questa operazione, consultare [Manually add your account](user-help-auth-app-add-account-manual.md) (Aggiungere manualmente l'account).

5. Rivedere la schermata **Account** dell'app nel dispositivo per assicurarsi che l'account sia corretto e che sia associato a un codice di verifica a sei cifre. Per una maggiore sicurezza, il codice di verifica cambia ogni 30 secondi per impedire che venga usato più volte.

    ![Schermata Account](./media/user-help-auth-app-download-install/auth-app-accounts.png)

## <a name="next-steps"></a>Passaggi successivi

- Dopo aver aggiunto gli account all'app, è possibile accedere usando l'app Authenticator nel dispositivo. Per altre informazioni, consultare [Accedere con l'app](user-help-auth-app-sign-in.md).

- Per i dispositivi che eseguono iOS, è anche possibile eseguire il backup nel cloud delle credenziali dell'account e delle relative impostazioni dell'app, ad esempio l'ordine degli account. Per altre informazioni, consultare [Eseguire il backup e il ripristino con l'app Microsoft Authenticator](user-help-auth-app-backup-recovery.md).
