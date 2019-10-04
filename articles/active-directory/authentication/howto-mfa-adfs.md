---
title: Proteggere le risorse cloud con Azure MFA e AD FS - Azure Active Directory
description: Questa è la pagina su Multi-Factor Authentication di Azure in cui viene descritto come iniziare a utilizzare questa tipologia di autenticazione di Azure nel cloud.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: a5b1838007e1be7fc1d9872516ede14c208b1f57
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "67113469"
---
# <a name="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs"></a>Protezione delle risorse cloud con Azure Multi-Factor Authentication e AD FS

Se l'organizzazione è federata con Azure Active Directory, usare Azure Multi-Factor Authentication o Active Directory Federation Services (AD FS) per proteggere le risorse accessibili da Azure AD. Usare le procedure seguenti per proteggere le risorse di Azure Active Directory con Azure Multi-Factor Authentication o Active Directory Federation Services.

## <a name="secure-azure-ad-resources-using-ad-fs"></a>Proteggere le risorse Azure AD con ADFS

Per proteggere le risorse cloud, configurare una regola attestazioni in modo che Active Directory Federation Services generi l'attestazione multipleauthn quando un utente esegue correttamente la verifica in due passaggi. Questa attestazione viene passata ad Azure AD. Seguire questa procedura per eseguire i passaggi:

1. Aprire il componente di gestione di ADFS.
2. A sinistra selezionare **Attendibilità componente**.
3. Fare clic con il pulsante destro del mouse su **Piattaforma delle identità di Microsoft Office 365** e selezionare **Modifica regole attestazione**.

   ![Console di ad FS - trust Relying Party](./media/howto-mfa-adfs/trustedip1.png)

4. In Regole di trasformazione rilascio fare clic su **Aggiungi regola**.

   ![Modifica di regole di trasformazione rilascio](./media/howto-mfa-adfs/trustedip2.png)

5. Nell'Aggiunta guidata regole attestazione di trasformazione selezionare **Applicare la funzione di pass-through o di filtro a un'attestazione in ingresso** dall'elenco a discesa e fare clic su **Avanti**.

   ![Aggiunta guidata regole attestazione di trasformazione](./media/howto-mfa-adfs/trustedip3.png)

6. Assegnare un nome alla regola. 
7. Selezionare **Riferimenti dei metodi di autenticazione** come Tipo di attestazione in ingresso.
8. Selezionare **Pass-through di tutti i valori attestazione**.
    ![Aggiunta guidata regole attestazione di trasformazione](./media/howto-mfa-adfs/configurewizard.png)
9. Fare clic su **Finish**. Chiudere la console di gestione di ADFS.

## <a name="trusted-ips-for-federated-users"></a>Indirizzi IP attendibili per utenti federati

Gli indirizzi IP attendibili consentono agli amministratori di ignorare la verifica in due passaggi per specifici indirizzi IP o utenti federati con richieste provenienti dalla propria rete Intranet. Le sezioni seguenti descrivono come configurare gli indirizzi IP attendibili di Azure Multi-Factor Authentication con utenti federati e ignorare la verifica in due passaggi quando una richiesta proviene da una Intranet di utenti federati. Questo avviene configurando ADFS in modo da applicare la funzione di pass-through o filtro a un modello di attestazione in ingresso con il tipo di attestazione All'interno della rete aziendale.

Questo esempio usa Office 365 per l'attendibilità del componente.

### <a name="configure-the-ad-fs-claims-rules"></a>Configurare le regole attestazioni di ADFS

Per prima cosa è necessario configurare le attestazioni ADFS. Creare due regole attestazioni, una per il tipo di attestazione All'interno della rete aziendale e un'altra per mantenere gli utenti connessi.

1. Aprire il componente di gestione di ADFS.
2. A sinistra selezionare **Attendibilità componente**.
3. Fare clic su **piattaforma delle identità di Microsoft Office 365** e selezionare **Modifica regole attestazione...** 
   ![Console ADFS - Modifica regole attestazione](./media/howto-mfa-adfs/trustedip1.png)
4. In regole di trasformazione rilascio fare clic su **Aggiungi regola.** 
    ![Aggiunta di una regola di attestazione](./media/howto-mfa-adfs/trustedip2.png)
5. Nell'Aggiunta guidata regole attestazione di trasformazione selezionare **Applicare la funzione di pass-through o di filtro a un'attestazione in ingresso** dall'elenco a discesa e fare clic su **Avanti**.
   ![Aggiunta guidata regole attestazione di trasformazione](./media/howto-mfa-adfs/trustedip3.png)
6. Nella casella Nome regola attestazione assegnare un nome alla regola. Ad esempio:  InsideCorpNet.
7. Nell'elenco a discesa accanto a Tipo di attestazione in ingresso selezionare **All'interno della rete aziendale**.
   ![Attestazione aggiunta all'interno di rete aziendale](./media/howto-mfa-adfs/trustedip4.png)
8. Fare clic su **Finish**.
9. In Regole di trasformazione rilascio fare clic su **Aggiungi regola**.
10. Nell'Aggiunta guidata regole attestazione di trasformazione selezionare **Inviare attestazioni mediante una regola personalizzata** dall'elenco a discesa e fare clic su **Avanti**.
11. Nella casella sotto a Nome regola attestazione: specificare di *mantenere gli utenti connessi*.
12. Nella casella Regola personalizzata immettere:

        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
    ![Creare l'attestazione personalizzata per mantenere gli utenti connessi](./media/howto-mfa-adfs/trustedip5.png)
13. Fare clic su **Finish**.
14. Fare clic su **Apply**.
15. Fare clic su **OK**.
16. Chiudere Gestione ADFS.

### <a name="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users"></a>Configurare gli indirizzi IP attendibili di Azure Multi-Factor Authentication con utenti federati

Ora che le attestazioni sono configurate, è possibile procedere alla configurazione degli indirizzi IP attendibili.

1. Accedere al [portale di Azure](https://portal.azure.com).
2. Selezionare **Azure Active Directory** > **accesso condizionale** > **località denominate**.
3. Dal **accesso condizionale - posizioni specifiche** pannello seleziona **indirizzi IP attendibili MFA configurare**

   ![Località denominate di accesso condizionale di Azure AD MFA configurare gli indirizzi IP attendibili](./media/howto-mfa-adfs/trustedip6.png)

4. In **Indirizzi IP attendibili** nella pagina Impostazioni servizio selezionare **Ignora l'autenticazione a più fattori per le richieste provenienti da utenti federati nella Intranet**.  
5. Fare clic su **save**.

La procedura è terminata. A questo punto, gli utenti federati di Office 365 dovrebbero usare solo MFA quando una richiesta proviene dall'esterno della Intranet aziendale.
