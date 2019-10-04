---
title: 'Esercitazione: Integrazione di Azure Active Directory con BlueJeans | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e BlueJeans.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0308fbb103fb06bd2dffe0a442346a3fc4f7db62
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "67106163"
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a>Esercitazione: Integrazione di Azure Active Directory con BlueJeans

Questa esercitazione descrive come integrare BlueJeans con Azure Active Directory (Azure AD).
L'integrazione di BlueJeans con Azure AD offre i vantaggi seguenti:

* È possibile controllare in Azure AD chi può accedere a BlueJeans.
* È possibile abilitare gli utenti per l'accesso automatico (Single Sign-On) a BlueJeans con gli account Azure AD personali.
* È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Se non si ha una sottoscrizione di Azure, [creare un account gratuito](https://azure.microsoft.com/free/) prima di iniziare.

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con BlueJeans, sono necessari gli elementi seguenti:

* Una sottoscrizione di Azure AD. Se non si dispone di un ambiente di Azure AD, è possibile ottenere un [account gratuito](https://azure.microsoft.com/free/).
* Sottoscrizione di BlueJeans abilitata per l'accesso Single Sign-On

## <a name="scenario-description"></a>Descrizione dello scenario

In questa esercitazione vengono eseguiti la configurazione e il test dell'accesso Single Sign-On di Azure AD in un ambiente di test.

* BlueJeans supporta l'accesso SSO avviato da **SP**

* BlueJeans supporta il [**provisioning utenti automatico**](bluejeans-provisioning-tutorial.md)

## <a name="adding-bluejeans-from-the-gallery"></a>Aggiunta di BlueJeans dalla raccolta

Per configurare l'integrazione di BlueJeans in Azure AD, è necessario aggiungere BlueJeans dalla raccolta all'elenco di app SaaS gestite.

**Per aggiungere BlueJeans dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Pulsante Azure Active Directory](common/select-azuread.png)

2. Passare ad **Applicazioni aziendali** e quindi selezionare l'opzione **Tutte le applicazioni**.

    ![Pannello Applicazioni aziendali](common/enterprise-applications.png)

3. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![Pulsante Nuova applicazione](common/add-new-app.png)

4. Nella casella di ricerca digitare **BlueJeans**, selezionare **BlueJeans** nel pannello dei risultati e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

     ![BlueJeans nell'elenco risultati](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurare e testare l'accesso Single Sign-On di Azure AD

In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con BlueJeans usando un utente di test di nome **Britta Simon**.
Per il corretto funzionamento dell'accesso Single Sign-On, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in BlueJeans.

Per configurare e testare l'accesso Single Sign-On di Azure AD con BlueJeans, è necessario completare le procedure di base seguenti:

1. **[Configurare l'accesso Single Sign-On di Azure AD](#configure-azure-ad-single-sign-on)** : per consentire agli utenti di usare questa funzionalità.
2. **[Configurare l'accesso Single Sign-On di BlueJeans](#configure-bluejeans-single-sign-on)** : per configurare le impostazioni di Single Sign-On sul lato applicazione.
3. **[Creare un utente di test di Azure AD](#create-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
4. **[Assegnare l'utente test di Azure AD](#assign-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
5. **[Creare un utente di test di BlueJeans](#create-bluejeans-test-user)** : per avere una controparte di Britta Simon in BlueJeans collegata alla rappresentazione dell'utente in Azure AD.
6. **[Testare l'accesso Single Sign-On](#test-single-sign-on)** per verificare se la configurazione funziona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurare l'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure.

Per configurare l'accesso Single Sign-On di Azure AD con BlueJeans, seguire questa procedura:

1. Nella pagina di integrazione dell'applicazione **BlueJeans** del [portale di Azure](https://portal.azure.com/) selezionare **Single Sign-On**.

    ![Collegamento Configura accesso Single Sign-On](common/select-sso.png)

2. Nella finestra di dialogo **Selezionare un metodo di accesso Single Sign-On** selezionare la modalità **SAML/WS-Fed** per abilitare il Single Sign-On.

    ![Selezione della modalità Single Sign-On](common/select-saml-option.png)

3. Nella pagina **Configura l'accesso Single Sign-On con SAML** fare clic sull'icona **Modifica** per aprire la finestra di dialogo **Configurazione SAML di base**.

    ![Modificare la configurazione SAML di base](media/bluejeans-tutorial/edit-urls-bluejeans.png)

4. Nella finestra di dialogo **Configurazione SAML di base** immettere i valori seguenti:

    ![Informazioni sull'accesso Single Sign-On per URL e dominio di BlueJeans](media/bluejeans-tutorial/tutorial_bluejeans-basic-configuration.png)

   - Nella casella di testo **Identificatore** digitare il valore seguente: `http://samlsp.bluejeans.com`
    
   - Nella casella di testo **URL accesso** digitare l'URL della pagina di destinazione fornita da BlueJeans: `https://<companyname>.bluejeans.com`. Per ottenere questo valore, contattare il [team di supporto clienti di BlueJeans team](https://support.bluejeans.com/contact).
    
   - Fare clic su **Save**.

5. Nella pagina **Configura l'accesso Single Sign-On con SAML**, nella sezione **Certificato di firma SAML**, fare clic su **Scarica** per scaricare il **Certificato (Base64)** definito dalle opzioni specificate in base ai propri requisiti e salvarlo in questo computer.

    ![Collegamento di download del certificato](common/certificatebase64.png)

6. Nella sezione **Configura BlueJeans** copiare gli URL appropriati in base alle proprie esigenze.

    ![Copiare gli URL di configurazione](common/copy-configuration-urls.png)

    a. URL di accesso

    b. Identificatore di Azure AD

    c. URL di chiusura sessione

### <a name="configure-bluejeans-single-sign-on"></a>Configurare l'accesso Single Sign-On di BlueJeans

1. In un'altra finestra del Web browser accedere al sito aziendale di **BlueJeans** come amministratore.

2. Passare ad **ADMIN \> GROUP SETTINGS \> SECURITY**.

    ![Amministratore](./media/bluejeans-tutorial/ic785868.png "Amministratore")

3. Nella sezione **SECURITY** seguire questa procedura:

    ![Single Sign-On SAML](./media/bluejeans-tutorial/ic785869.png "Single Sign-On SAML")

    a. Selezionare **SAML Single Sign On**.

    b. Selezionare **Enable automatic provisioning**.

4. Continuare e seguire questa procedura:

    ![Percorso certificato](./media/bluejeans-tutorial/ic785870.png "Percorso certificato")

    a. Fare clic su **Choose a File** (Scegli un file) per caricare il certificato codificato in base 64 scaricato dal portale di Azure.

    b. Nella casella di testo **URL di accesso** incollare il valore di **URL di accesso** copiato dal portale di Azure.

    c. Nella casella di testo **Modifica URL password** incollare il valore dell'**URL di modifica password** copiato dal portale di Azure.

    d. Nella casella di testo **URL disconnessione** incollare il valore dell'**URL di disconnessione** copiato dal portale di Azure.

5. Continuare e seguire questa procedura:

    ![Salvare le modifiche](./media/bluejeans-tutorial/ic785874.png "Salvare le modifiche")

    a. Nella casella di testo **User Id** (ID utente) digitare `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    b. Nella casella di testo **Email** (Posta elettronica) digitare `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    c. Fare clic su **SAVE CHANGES** (Salva modifiche).

### <a name="create-an-azure-ad-test-user"></a>Creare un utente test di Azure AD

Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

1. Nel riquadro sinistro del portale di Azure, selezionare **Azure Active Directory**, **Utenti** e quindi **Tutti gli utenti**.

    ![Collegamenti "Utenti e gruppi" e "Tutti gli utenti"](common/users.png)

2. Selezionare **Nuovo utente** in alto nella schermata.

    ![Pulsante Nuovo utente](common/new-user.png)

3. In Proprietà utente seguire questa procedura.

    ![Finestra di dialogo Utente](common/user-properties.png)

    a. Nel campo **Nome** immettere **BrittaSimon**.
  
    b. Nel campo **Nome utente** digitare `brittasimon\@yourcompanydomain.extension`. Ad esempio: BrittaSimon@contoso.com.

    c. Selezionare la casella di controllo **Mostra password** e quindi prendere nota del valore visualizzato nella casella Password.

    d. Fare clic su **Create**(Crea).

### <a name="assign-the-azure-ad-test-user"></a>Assegnare l'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a BlueJeans.

1. Nel portale di Azure selezionare **Applicazioni aziendali**, quindi **Tutte le applicazioni** e infine **BlueJeans**.

    ![Pannello delle applicazioni aziendali](common/enterprise-applications.png)

2. Nell'elenco delle applicazioni selezionare **BlueJeans**.

    ![Collegamento a BlueJeans nell'elenco delle applicazioni](common/all-applications.png)

3. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Collegamento "Utenti e gruppi"](common/users-groups-blade.png)

4. Fare clic sul pulsante **Aggiungi utente** e quindi selezionare **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Riquadro Aggiungi assegnazione](common/add-assign-user.png)

5. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti e quindi fare clic sul pulsante **Seleziona** in basso nella schermata.

6. Se si prevede un valore di ruolo nell'asserzione SAML, nella finestra di dialogo **Selezionare un ruolo** selezionare il ruolo appropriato per l'utente dall'elenco, quindi fare clic sul pulsante **Seleziona** nella parte inferiore della schermata.

7. Nella finestra di dialogo **Aggiungi assegnazione** fare clic sul pulsante **Assegna**.

### <a name="create-bluejeans-test-user"></a>Creare un utente di test di BlueJeans

L'obiettivo di questa sezione consiste nel creare un utente chiamato Britta Simon in BlueJeans. BlueJeans supporta il provisioning utenti automatico, che è abilitato per impostazione predefinita. È possibile scoprire più dettagli [qui](bluejeans-provisioning-tutorial.md) su come configurare il provisioning utenti automatico.

**Per creare un utente manualmente, seguire questa procedura:**

1. Accedere al sito aziendale di **BlueJeans** come amministratore.

2. Passare ad **AMMINISTRATORE \> GESTISCI UTENTI \> AGGIUNGI UTENTE**.

    ![Amministratore](./media/bluejeans-tutorial/ic785877.png "Amministratore")

    > [!IMPORTANT]
    > La scheda **AGGIUNGI UTENTE** è disponibile solo se nella scheda **SICUREZZA** l'opzione **Enable automatic provisioning** è deselezionata.

3. Nella sezione **AGGIUNGI UTENTE** seguire questa procedura:

    ![Aggiungere un utente](./media/bluejeans-tutorial/ic785886.png "Aggiungere un utente")

    a. Nella casella di testo **First Name** (Nome) immettere il nome dell'utente, ad esempio **Britta**.

    b. Nella casella di testo **Last Name** (Nome) immettere il cognome dell'utente, ad esempio **Simon**.

    c. Nella casella di testo **Pick a BlueJeans Username** (Scegli un nome utente BlueJeans) immettere il nome utente dell'utente, ad esempio **Brittasimon**

    d. Nella casella di testo **Crea una password** immettere la password.

    e. Nella casella di testo **Società** immettere il nome della società.

    f. Nella casella di testo **Email address** (Indirizzo e-mail) immettere l'indirizzo di posta elettronica dell'utente, ad esempio `brittasimon\@contoso.com`.

    g. Nella casella di testo **Create a BlueJeans Meeting I.D** (Crea un ID riunione BlueJeans) immettere l'ID riunione.

    h. Nella casella di testo **Pick a Moderator Passcode** (Seleziona passcode moderatore) immettere il passcode.

    i. Fare clic su **CONTINUE** (Continua).

    ![Aggiungere un utente](./media/bluejeans-tutorial/ic785887.png "Aggiungere un utente")

    J. Fare clic su **Aggiungi utente**.

> [!NOTE]
> È possibile usare qualsiasi altro strumento o API di creazione di account utente fornita da BlueJeans per eseguire il provisioning degli account utente di Azure AD.

### <a name="test-single-sign-on"></a>Testare l'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro di BlueJeans nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione BlueJeans per cui si è configurato l'accesso SSO. Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Risorse aggiuntive

- [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Che cos'è l'accesso condizionale in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
