---
title: 'Esercitazione: Integrazione di Azure Active Directory con Coralogix | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Coralogix.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: ba79bfc1-992e-4924-b76a-8eb0dfb97724
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 1/2/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 721e0c40ec2e02dabee0681e01fea4182b906183
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "67104655"
---
# <a name="tutorial-azure-active-directory-integration-with-coralogix"></a>Esercitazione: Integrazione di Azure Active Directory con Coralogix

Questa esercitazione descrive come integrare Coralogix con Azure Active Directory (Azure AD).
L'integrazione di Coralogix con Azure AD offre i vantaggi seguenti:

* È possibile controllare in Azure AD chi può accedere a Coralogix.
* È possibile abilitare gli utenti per l'accesso automatico (Single Sign-On) a Coralogix con gli account Azure AD personali.
* È possibile gestire gli account da una posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Se non si ha una sottoscrizione di Azure, [creare un account gratuito](https://azure.microsoft.com/free/) prima di iniziare.

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con Coralogix, sono necessari gli elementi seguenti:

- Una sottoscrizione di Azure AD. Se non si dispone di un ambiente di Azure AD, è possibile ottenere una [versione di valutazione di un mese](https://azure.microsoft.com/pricing/free-trial/).
- Una sottoscrizione di Coralogix abilitata per l'accesso Single Sign-On. 

## <a name="scenario-description"></a>Descrizione dello scenario

In questa esercitazione vengono eseguiti la configurazione e il test dell'accesso Single Sign-On di Azure AD in un ambiente di test.

* Coralogix supporta l'accesso SSO avviato da SP.

## <a name="add-coralogix-from-the-gallery"></a>Aggiungere Coralogix dalla raccolta

Per configurare l'integrazione di Coralogix in Azure AD, è necessario prima aggiungere Coralogix dalla raccolta al proprio elenco di app SaaS gestite.

Per aggiungere Coralogix dalla raccolta, seguire questa procedura:

1. Nel [portale di Azure](https://portal.azure.com) fare clic sull'icona **Azure Active Directory** nel riquadro sinistro.

    ![Pulsante Azure Active Directory](common/select-azuread.png)

2. Passare ad **Applicazioni aziendali** e quindi selezionare **Tutte le applicazioni**.

    ![Pannello Applicazioni aziendali](common/enterprise-applications.png)

3. Per aggiungere una nuova applicazione, fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo.

    ![Pulsante Nuova applicazione](common/add-new-app.png)

4. Nella casella di ricerca immettere **Coralogix**. Selezionare **Coralogix** nel riquadro dei risultati e quindi selezionare il pulsante **Aggiungi** per aggiungere l'applicazione.

     ![Coralogix nell'elenco dei risultati](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurare e testare l'accesso Single Sign-On di Azure AD

In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Coralogix usando un utente di test di nome Britta Simon.
Per il funzionamento dell'accesso Single Sign-On, deve essere stabilito un collegamento tra un utente di Azure AD e l'utente correlato in Coralogix.

Per configurare e testare l'accesso Single Sign-On di Azure AD con Coralogix, è necessario prima completare le procedure di base seguenti:

1. [Configurare l'accesso Single Sign-On di Azure AD](#configure-azure-ad-single-sign-on) per consentire agli utenti di usare questa funzionalità.
2. [Configurare l'accesso Single Sign-On di Coralogix](#configure-coralogix-single-sign-on) per configurare le impostazioni di Single Sign-On sul lato applicazione.
3. [Creare un utente di test di Azure AD](#create-an-azure-ad-test-user) per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
4. [Assegnare l'utente di test di Azure AD](#assign-the-azure-ad-test-user) per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
5. [Creare un utente di test di Coralogix](#create-a-coralogix-test-user) per avere una controparte di Britta Simon in Coralogix collegata alla rappresentazione dell'utente in Azure AD.
6. [Testare l'accesso Single Sign-On](#test-single-sign-on) per verificare se la configurazione funziona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurare l'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure.

Per configurare l'accesso Single Sign-On di Azure AD con Coralogix, seguire questa procedura:

1. Nella pagina di integrazione dell'applicazione **Coralogix** del [portale di Azure](https://portal.azure.com/) selezionare **Single Sign-On**.

    ![Collegamento Configura accesso Single Sign-On](common/select-sso.png)

2. Nella finestra di dialogo **Selezionare un metodo di accesso Single Sign-On** selezionare **SAML** per abilitare l'accesso Single Sign-On.

    ![Selezione della modalità Single Sign-On](common/select-saml-option.png)

3. Nella pagina **Configura l'accesso Single Sign-On con SAML** selezionare l'icona **Modifica** per aprire la finestra di dialogo **Configurazione SAML di base**.

    ![Modificare la configurazione SAML di base](common/edit-urls.png)

4. Nella finestra di dialogo **Configurazione SAML di base** seguire questa procedura:

    ![Informazioni su URL e dominio per l'accesso Single Sign-On di Coralogix](common/sp-identifier.png)

    a. Nella casella **URL di accesso** immettere un URL usando il modello seguente: `https://<SUBDOMAIN>.coralogix.com`

    b. Nella casella di testo **Identificatore (ID entità)** immettere un URL, ad esempio:
    
    `https://api.coralogix.com/saml/metadata.xml`

    oppure

    `https://aws-client-prod.coralogix.com/saml/metadata.xml` 

    > [!NOTE]
    > Poiché il valore dell'URL di accesso non è reale, è necessario aggiornare questo valore con l'URL di accesso effettivo. Per ottenere questo valore, contattare il [team di supporto clienti di Coralogix](mailto:info@coralogix.com). È anche possibile fare riferimento ai modelli nella sezione **Configurazione SAML di base** del portale di Azure.

5. L'applicazione Coralogix prevede un formato specifico per le asserzioni SAML. Configurare le attestazioni seguenti per questa applicazione. È possibile gestire i valori di questi attributi dalla sezione **Attributi utente** nella pagina di integrazione dell'applicazione. Nella pagina **Configura l'accesso Single Sign-On con SAML** selezionare il pulsante **Modifica** per aprire la finestra di dialogo **Attributi utente**.

    ![image](common/edit-attribute.png)

6. Nella sezione **Attestazioni utente** della finestra di dialogo **Attributi utente** modificare le attestazioni tramite l'icona **Modifica**. È anche possibile aggiungere le attestazioni usando **Aggiungi nuova attestazione** per configurare l'attributo del token SAML come illustrato nell'immagine precedente. Seguire quindi questa procedura:
    
    a. Selezionare l'**icona Modifica** per aprire la finestra di dialogo **Gestisci attestazioni utente**.

    ![image](./media/coralogix-tutorial/tutorial_usermail.png) ![image](./media/coralogix-tutorial/tutorial_usermailedit.png)

    b. Nell'elenco **Scegliere il formato per l'identificatore del nome** selezionare **Indirizzo di posta elettronica**.

    c. Nell'elenco **Attributo di origine** selezionare **user.mail**.

    d. Selezionare **Salva**.

7. Nella pagina **Configura l'accesso Single Sign-On con SAML**, nella sezione **Certificato di firma SAML**, selezionare **Scarica** per scaricare il file **XML metadati federazione** definito dalle opzioni specificate in base ai propri requisiti. Salvare quindi il file nel computer.

    ![Collegamento di download del certificato](common/metadataxml.png)

8. Nella sezione **Configura Coralogix** copiare gli URL appropriati.

    ![Copiare gli URL di configurazione](common/copy-configuration-urls.png)

    a. URL di accesso

    b. Identificatore di Azure AD

    c. URL di chiusura sessione

### <a name="configure-coralogix-single-sign-on"></a>Configurare l'accesso Single Sign-On di Coralogix

Per configurare l'accesso Single Sign-On sul lato **Coralogix**, inviare il file **XML metadati federazione** scaricato e gli URL copiati dal portale di Azure al [team di supporto di Coralogix](mailto:info@coralogix.com). Questi dati garantiscono che la connessione Single Sign-On SAML sia impostata correttamente su entrambi i lati.

### <a name="create-an-azure-ad-test-user"></a>Creare un utente test di Azure AD 

Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

1. Nel riquadro sinistro del portale di Azure, selezionare **Azure Active Directory**, **Utenti** e quindi **Tutti gli utenti**.

    ![Collegamenti "Utenti e gruppi" e "Tutti gli utenti"](common/users.png)

2. Selezionare **Nuovo utente** in alto nella schermata.

    ![Pulsante Nuovo utente](common/new-user.png)

3. Nella finestra di dialogo **Utente** seguire questa procedura.

    ![Finestra di dialogo Utente](common/user-properties.png)

    a. Nel campo **Nome** immettere **BrittaSimon**.
  
    b. Nel campo **Nome utente** immettere "brittasimon@yourcompanydomain.extension". In questo caso, ad esempio, è possibile immettere "brittasimon@contoso.com".

    c. Selezionare la casella di controllo **Mostra password** e quindi prendere nota del valore visualizzato nella casella **Password**.

    d. Selezionare **Create** (Crea).

### <a name="assign-the-azure-ad-test-user"></a>Assegnare l'utente test di Azure AD

In questa sezione si abilita Britta Simon all'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a Coralogix.

1. Nel portale di Azure selezionare **Applicazioni aziendali**, quindi **Tutte le applicazioni** e infine **Coralogix**.

    ![Pannello delle applicazioni aziendali](common/enterprise-applications.png)

2. Nell'elenco delle applicazioni selezionare **Coralogix**.

    ![Collegamento di Coralogix nell'elenco delle applicazioni](common/all-applications.png)

3. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Collegamento "Utenti e gruppi"](common/users-groups-blade.png)

4. Selezionare il pulsante **Aggiungi utente**. Nella finestra di dialogo **Aggiungi assegnazione** selezionare quindi **Utenti e gruppi**.

    ![Riquadro Aggiungi assegnazione](common/add-assign-user.png)

5. Nella finestra di dialogo **Users and groups** (Utenti e gruppi) selezionare **Britta Simon** nell'elenco di utenti. Fare quindi clic sul pulsante **Seleziona** nella parte inferiore della schermata.

6. Se si prevede un valore di ruolo nell'asserzione SAML, nella finestra di dialogo **Selezionare un ruolo** selezionare il ruolo appropriato per l'utente dall'elenco. Fare quindi clic sul pulsante **Seleziona** nella parte inferiore della schermata.

7. Nella finestra di dialogo **Aggiungi assegnazione** selezionare il pulsante **Assegna**.

### <a name="create-a-coralogix-test-user"></a>Creare un utente di test di Coralogix

In questa sezione si crea un utente di nome Britta Simon in Coralogix. Collaborare con il  [team di supporto di Coralogix](mailto:info@coralogix.com) per aggiungere gli utenti alla piattaforma Coralogix. È necessario creare e attivare gli utenti prima di usare l'accesso Single Sign-On.

### <a name="test-single-sign-on"></a>Testare l'accesso Single Sign-On 

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD tramite il portale App personali.

Quando si seleziona il riquadro di Coralogix nel portale App personali, si dovrebbe accedere automaticamente a Coralogix. Per altre informazioni sul portale App personali, vedere [Accedere e usare le app nel portale App personali](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Risorse aggiuntive

- [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Che cos'è l'accesso condizionale in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

