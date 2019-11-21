---
title: Deploy Azure Blockchain Workbench Preview
description: How to deploy Azure Blockchain Workbench Preview
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 11/19/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: brendal
manager: femila
ms.openlocfilehash: 61d47709df2d1bc41bfbf61a7f71f1d73fe27389
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/20/2019
ms.locfileid: "74222907"
---
# <a name="deploy-azure-blockchain-workbench-preview"></a>Deploy Azure Blockchain Workbench Preview

Azure Blockchain Workbench Preview is deployed using a solution template in the Azure Marketplace. Il modello semplifica la distribuzione dei componenti necessari per creare applicazioni blockchain. Una volta distribuito, Blockchain Workbench fornisce l'accesso alle app client per creare e gestire utenti e applicazioni blockchain.

Per altre informazioni sui componenti di Blockchain Workbench, vedere [Architettura di Azure Blockchain Workbench](architecture.md).

[!INCLUDE [Preview note](./includes/preview.md)]

## <a name="prepare-for-deployment"></a>Preparare la distribuzione

Blockchain Workbench consente di distribuire un libro mastro blockchain insieme a un set di servizi di Azure pertinenti usati frequentemente per compilare un'applicazione basata su blockchain. La distribuzione di Blockchain Workbench comporta il provisioning dei seguenti servizi Azure all'interno di un gruppo di risorse nella sottoscrizione Azure.

* App Service Plan (Standard)
* Application Insights
* Griglia di eventi
* Azure Key Vault
* Bus di servizio
* SQL Database (Standard S0) + SQL Logical Server
* Azure Storage account (Standard LRS)
* Virtual machine scale set with capacity of 1
* Virtual Network resource group (with Load Balancer, Network Security Group, Public IP Address, Virtual Network)
* Azure Blockchain Service. If you are using a previous Blockchain Workbench deployment, consider redeploying Azure Blockchain Workbench to use Azure Blockchain Service.

Di seguito è riportato un esempio di distribuzione creata nel gruppo di risorse **myblockchain**.

![Distribuzione di esempio](media/deploy/example-deployment.png)

Il costo di Blockchain Workbench è un'aggregazione del costo dei servizi Azure sottostanti. Per calcolare i prezzi per i servizi di Azure, usare il [Calcolatore dei prezzi](https://azure.microsoft.com/pricing/calculator/).

## <a name="prerequisites"></a>Prerequisiti

Azure Blockchain Workbench richiede le registrazioni per l'applicazione e la configurazione di Azure AD. È possibile scegliere di eseguire la [configurazione di Azure AD manualmente](#azure-ad-configuration) prima della distribuzione o eseguire uno script successivamente alla distribuzione. Se si sta ridistribuendo Blockchain Workbench, vedere [Configurazione di Azure AD](#azure-ad-configuration) per verificare la configurazione di Azure AD.

> [!IMPORTANT]
> Non è necessario distribuire Workbench nello stesso tenant di quello usato per registrare un'applicazione Azure AD. Workbench deve essere distribuito in un tenant in cui sono disponibili autorizzazioni sufficienti per distribuire le risorse. Per altre informazioni sui tenant di Azure AD, vedere [Come ottenere un tenant di Active Directory](../../active-directory/develop/quickstart-create-new-tenant.md) e [Integrazione di applicazioni con Azure Active Directory](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md).

## <a name="deploy-blockchain-workbench"></a>Distribuire Blockchain Workbench

Dopo avere completato i passaggi preliminari necessari, è possibile distribuire Blockchain Workbench. Le sezioni seguenti descrivono come distribuire il framework.

1. Accedere al [portale di Azure](https://portal.azure.com).
1. Selezionare l'account nell'angolo superiore destro e passare al tenant di Azure AD in cui si vuole distribuire Azure Blockchain Workbench.
1. Selezionare **Crea risorsa** nell'angolo superiore sinistro del portale di Azure.
1. Select **Blockchain** > **Azure Blockchain Workbench (preview)** .

    ![Creare Azure Blockchain Workbench](media/deploy/blockchain-workbench-settings-basic.png)

    | Impostazione | Description  |
    |---------|--------------|
    | Prefisso della risorsa | Identificatore univoco breve della distribuzione. Questo valore viene usato come base per la denominazione delle risorse. |
    | Nome utente macchina virtuale | Nome utente usato come amministratore per tutte le macchine virtuali (VM). |
    | Tipo di autenticazione | Selezionare questa opzione se si vuole usare una password o una chiave per la connessione alle macchine virtuali. |
    | Password | Password usata per la connessione alle macchine virtuali. |
    | SSH | Usare una chiave pubblica RSA in formato a una riga che inizi con **ssh-rsa** oppure usare il formato PEM a più righe. È possibile generare le chiavi SSH tramite `ssh-keygen` in Linux e OS X oppure usando PuTTYGen in Windows. Per altre informazioni sulle chiavi SSH, vedere [Come usare SSH con Windows in Azure](../../virtual-machines/linux/ssh-from-windows.md). |
    | Database and Blockchain password | Specificare la password da usare per accedere al database creato nell'ambito della distribuzione. The password must meet three of the following four requirements: length needs to be between 12 & 72 characters, 1 lower case character, 1 upper case character, 1 number, and 1 special character that is not number sign(#), percent(%), comma(,), star(*), back quote(\`), double quote("), single quote('), dash(-) and semicolumn(;) |
    | Area di distribuzione | Specificare dove distribuire le risorse di Blockchain Workbench. Per ottimizzare la disponibilità, questa impostazione deve corrispondere alla **Località**. |
    | Sottoscrizione | Specificare la sottoscrizione di Azure che si vuole usare per la distribuzione. |
    | Gruppi di risorse | Creare un nuovo gruppo di risorse selezionando **Crea nuovo** e quindi specificare un nome di gruppo di risorse univoco. |
    | Località | Specificare l'area in cui si vuole distribuire il framework. |

1. Selezionare **OK** per completare la sezione di configurazione delle impostazione di base.

1. In **Advanced Settings** (Impostazioni avanzate) scegliere se si vuole creare una nuova rete blockchain oppure usare una rete blockchain proof-of-authority esistente.

    Per **Crea nuovo**:

    The *create new* option deploys an Azure Blockchain Service Quorum ledger with the default basic sku.

    ![Impostazioni avanzate per la nuova rete blockchain](media/deploy/advanced-blockchain-settings-new.png)

    | Impostazione | Description  |
    |---------|--------------|
    | Azure Blockchain Service pricing tier | Choose **Basic** or **Standard** Azure Blockchain Service tier that is used for Blockchain Workbench |
    | Impostazioni di Azure Active Directory | Scegliere **Add Later** (Aggiungi in seguito).</br>Nota: se si sceglie di [pre-configurare Azure AD](#azure-ad-configuration) o si sta eseguendo una ridistribuzione, scegliere *Add Now* (Aggiungi adesso). |
    | Seleziona macchina virtuale | Select preferred storage performance and VM size for your blockchain network. Scegliere dimensioni di macchina virtuali inferiori, come *DS1 Standard v2* se si usa una sottoscrizione con limiti di servizio ridotto, ad esempio il livello gratuito di Azure. |

    Per **Usa esistente**:

    L'opzione *Usa esistente* consente di specificare una rete blockchain di tipo Ethereum Proof-of-Authority (PoA). Gli endpoint presentano i requisiti seguenti.

   * L'endpoint deve essere una rete blockchain di tipo Ethereum Proof-of-Authority (PoA).
   * L'endpoint deve essere pubblicamente accessibile in rete.
   * La rete blockchain PoA deve essere configurata con il prezzo del gas impostato su zero.

     > [!NOTE]
     > Gli account di Blockchain Workbench non sono stati finanziati. Se sono richiesti finanziamenti, le transazioni hanno esito negativo.

     ![Impostazioni avanzate per la rete blockchain esistente](media/deploy/advanced-blockchain-settings-existing.png)

     | Impostazione | Description  |
     |---------|--------------|
     | Endpoint RPC Ethereum | Fornire l'endpoint RPC di una rete blockchain PoA esistente. L'endpoint inizia con https:// o http:// e termina con un numero di porta. Ad esempio: `http<s>://<network-url>:<port>` |
     | Impostazioni di Azure Active Directory | Scegliere **Add Later** (Aggiungi in seguito).</br>Nota: se si sceglie di [pre-configurare Azure AD](#azure-ad-configuration) o si sta eseguendo una ridistribuzione, scegliere *Add Now* (Aggiungi adesso). |
     | Seleziona macchina virtuale | Select preferred storage performance and VM size for your blockchain network. Scegliere dimensioni di macchina virtuali inferiori, come *DS1 Standard v2* se si usa una sottoscrizione con limiti di servizio ridotto, ad esempio il livello gratuito di Azure. |

1. Selezionare **OK** per completare le impostazioni avanzate.

1. Esaminare il riepilogo per verificare che i parametri siano corretti.

    ![Summary](media/deploy/blockchain-workbench-summary.png)

1. Selezionare **Crea** per accettare le condizioni e distribuire Azure Blockchain Workbench.

La distribuzione può richiedere fino a 90 minuti. È possibile usare il portale di Azure per monitorare lo stato di avanzamento. Nel nuovo gruppo di risorse creato selezionare **Distribuzioni > Panoramica** per visualizzare lo stato degli elementi distribuiti.

> [!IMPORTANT]
> Successivamente alla distribuzione, è necessario completare le impostazioni di Active Directory. Se si è scelto **Add Later** (Aggiungi in seguito), è necessario eseguire lo [script di configurazione di Azure AD](#azure-ad-configuration-script).  Se si è scelto **Add Now** (Aggiungi adesso), è necessario [configurare l'URL di risposta](#configuring-the-reply-url).

## <a name="blockchain-workbench-web-url"></a>Blockchain Workbench web URL

Al termine della distribuzione di Blockchain Workbench, un nuovo gruppo di risorse contiene le risorse di Blockchain Workbench. È possibile accedere ai servizi Blockchain Workbench tramite un URL Web. I passaggi seguenti mostrano come recuperare l'URL Web del framework distribuito.

1. Accedere al [portale di Azure](https://portal.azure.com).
1. In the left-hand navigation pane, select **Resource groups**.
1. Scegliere il nome del gruppo di risorse specificato al momento della distribuzione di Blockchain Workbench.
1. Selezionare l'intestazione di colonna **TIPO** per ordinare l'elenco in ordine alfabetico in base al tipo.
1. Sono presenti due risorse di tipo **Servizio app**. Selezionare la risorsa di tipo **Servizio app** *senza* il suffisso "-api".

    ![Elenco di servizi app](media/deploy/resource-group-list.png)

1. In the App Service **Overview**, copy the **URL** value, which represents the web URL to your deployed Blockchain Workbench.

    ![Sezione Essentials del servizio app](media/deploy/app-service.png)

Per associare un nome di dominio personalizzato a Blockchain Workbench, vedere [Configurazione di un nome di dominio personalizzato per un'app Web nel servizio app di Azure con Gestione traffico](../../app-service/web-sites-traffic-manager-custom-domain-name.md).

## <a name="azure-ad-configuration-script"></a>Script di configurazione di Azure AD

Azure AD deve essere configurato per completare la distribuzione di Blockchain Workbench. Per eseguire la configurazione, si userà uno script di PowerShell.

1. In un browser passare all'[indirizzo Web di Blockchain Workbench](#blockchain-workbench-web-url).
1. Verranno visualizzate le istruzioni per configurare Azure AD con Cloud Shell. Copiare il comando e avviare Cloud Shell.

    ![Avviare lo script di AAD](media/deploy/launch-aad-script.png)

1. Scegliere il tenant di Azure AD in cui è stato distribuito Blockchain Workbench.
1. In Cloud Shell, incollare ed eseguire il comando.
1. Quando richiesto, immettere il tenant di Azure AD che si vuole usare per Blockchain Workbench. Questo sarà il tenant che contiene gli utenti per Blockchain Workbench.

    > [!IMPORTANT]
    > L'utente autenticato richiede le autorizzazioni per creare registrazioni per l'applicazione Azure AD e concedere le autorizzazioni per l'applicazione delegata nel tenant. Può essere necessario chiedere all'amministratore del tenant di eseguire lo script di configurazione di Azure AD o creare un nuovo tenant.

    ![Immettere il tenant di Azure AD](media/deploy/choose-tenant.png)

1. Viene richiesto di eseguire l'autenticazione al tenant di Azure AD usando un browser. Aprire l'indirizzo Web in un browser, immettere il codice ed eseguire l'autenticazione.

    ![Eseguire l'autenticazione con il codice](media/deploy/authenticate.png)

1. Lo script restituisce alcuni messaggi di stato. Si ottiene un messaggio di stato **OPERAZIONE COMPLETATA**, se è stato correttamente eseguito il provisioning del tenant.
1. Passare all'indirizzo Web di Blockchain Workbench. Viene chiesto di fornire il consenso per concedere le autorizzazioni di lettura alla directory. In questo modo, l'app Web Blockchain Workbench può accedere agli utenti nel tenant. Se si hanno autorizzazioni di amministratore del tenant, è possibile fornire il consenso per l'intera organizzazione. Questa opzione può accettare il consenso per tutti gli utenti nel tenant. In caso contrario, ogni utente deve fornire il consenso al primo utilizzo dell'applicazione Web Blockchain Workbench.
1. Selezionare **Accetto** per fornire il consenso.

     ![Consenso per leggere i profili degli utenti](media/deploy/graph-permission-consent.png)

1. Dopo aver ottenuto il consenso, è possibile usare l'app Web Blockchain Workbench.

## <a name="azure-ad-configuration"></a>Configurazione di Azure AD

Se si sceglie di configurare manualmente o verificare le impostazioni di Azure AD prima della distribuzione, completare tutti i passaggi in questa sezione. Se si preferisce configurare automaticamente le impostazioni di Azure AD, usare lo [script di configurazione di Azure AD](#azure-ad-configuration-script) dopo aver distribuito Blockchain Workbench.

### <a name="blockchain-workbench-api-app-registration"></a>Registrazione dell'app per le API per Blockchain Workbench

Per la distribuzione di Blockchain Workbench, è necessaria la registrazione di un'applicazione Azure AD. Per registrare l'app, è necessario un tenant di Azure Active Directory (Azure AD). È possibile usare un tenant esistente oppure crearne uno nuovo. Se si usa un tenant di Azure AD esistente, sono necessarie autorizzazioni sufficienti per registrare le applicazioni, concedere le autorizzazioni per l'API Graph e consentire accesso guest all'interno di un tenant di Azure AD. Se non sono disponibili autorizzazioni sufficienti in un tenant di Azure AD esistente, creare un nuovo tenant.


1. Accedere al [portale di Azure](https://portal.azure.com).
1. Selezionare l'account nell'angolo superiore destro e passare al tenant di Azure AD desiderato. The tenant should be the subscription admin's tenant of the subscription where Azure Blockchain Workbench is deployed and you have sufficient permissions to register applications.
1. Nel riquadro di spostamento a sinistra selezionare il servizio **Azure Active Directory**. Select **App registrations** > **New registration**.

    ![Registrazione delle app](media/deploy/app-registration.png)

1. Provide a display **Name** and choose **Accounts in this organizational directory only**.

    ![Creare la registrazione dell'app](media/deploy/app-registration-create.png)

1. Select **Register** to register the Azure AD application.

### <a name="modify-manifest"></a>Modificare il manifesto

Successivamente, è necessario modificare il manifesto per l'uso di ruoli applicazione in Azure AD, in modo da specificare gli amministratori di Blockchain Workbench.  Per altre informazioni sui manifesti delle applicazioni, vedere [Manifesto dell'applicazione Azure Active Directory](../../active-directory/develop/reference-app-manifest.md).


1. You need to generate a GUID for the manifest. You can generate a GUID using the PowerShell command `[guid]::NewGuid()` or `New-GUID` cmdlet. Un'altra opzione consiste nell'usare un sito Web di generatori di GUID.
1. For the application you registered, select **Manifest** in the **Manage** section.
1. Next, update the **appRoles** section of the manifest. Replace `"appRoles": []` with the provided JSON. Assicurarsi di sostituire il valore per il campo **id** con il GUID generato. 

    ![Modificare il manifesto](media/deploy/edit-manifest.png)

    ``` json
    "appRoles": [
         {
           "allowedMemberTypes": [
             "User",
             "Application"
           ],
           "displayName": "Administrator",
           "id": "<A unique GUID>",
           "isEnabled": true,
           "description": "Blockchain Workbench administrator role allows creation of applications, user to role assignments, etc.",
           "value": "Administrator"
         }
       ],
    ```

    > [!IMPORTANT]
    > Il valore **Amministratore** è necessario solo per identificare gli amministratori di Blockchain Workbench.

1. Nel manifesto modificare anche il valore **Oauth2AllowImplicitFlow** impostandolo su **true**.

    ``` json
    "oauth2AllowImplicitFlow": true,
    ```

1. Selezionare **Salva** per salvare le modifiche apportate al manifesto.

### <a name="add-graph-api-required-permissions"></a>Aggiungere le autorizzazioni dell'API Graph necessarie

L'applicazione API deve richiedere l'autorizzazione dell'utente per accedere alla directory. Impostare l'autorizzazione seguente per l'applicazione API:

1. In the *Blockchain API* app registration, select **API permissions**. By default, the Graph API **User.Read** permission is added.

1. In **Grant consent**, select **Grant admin consent** for the domain then select **Yes** for the verification prompt.

   ![Concedere le autorizzazioni](media/deploy/client-app-grant-permissions.png)

   Concedendo l'autorizzazione, si consente a Blockchain Workbench di accedere agli utenti nella directory. L'autorizzazione di lettura è necessaria per cercare e aggiungere membri a Blockchain Workbench.

### <a name="get-application-id"></a>Ottenere l'ID applicazione

Le informazioni su ID e tenant dell'applicazione sono necessarie per la distribuzione. Raccogliere e archiviare le informazioni per usarle durante la distribuzione.

1. For the application you registered, select **Overview**.
1. Copy and store the **Application ID** value for later use during deployment.

    ![Proprietà dell'app per le API](media/deploy/app-properties.png)

    | Impostazione da archiviare  | Da usare nella distribuzione |
    |------------------|-------------------|
    | Application (client) ID | Azure Active Directory setup (Configurazione di Azure Active Directory) > ID applicazione |

### <a name="get-tenant-domain-name"></a>Ottenere il nome di dominio del tenant

Raccogliere e archiviare il nome di dominio del tenant di Active Directory in cui vengono registrate le applicazioni. 

Nel riquadro di spostamento a sinistra selezionare il servizio **Azure Active Directory**. Selezionare **Nomi di dominio personalizzati**. Copiare e archiviare il nome di dominio.

![Nome di dominio](media/deploy/domain-name.png)

### <a name="guest-user-settings"></a>Impostazioni degli utenti guest

Se sono presenti utenti guest nel tenant di Azure AD, seguire i passaggi aggiuntivi per assicurarsi che la gestione e l'assegnazione degli utenti di Blockchain Workbench funzioni correttamente.

1. Passare al tenant di Azure AD e selezionare **Azure Active Directory > Impostazioni utente > Gestisci le impostazioni di collaborazione esterna**.
1. Impostare **Le autorizzazioni degli utenti guest sono limitate** su **No**.
    ![Impostazioni di collaborazione esterna](media/deploy/user-collaboration-settings.png)

## <a name="configuring-the-reply-url"></a>Configuring the reply URL

Dopo la distribuzione di Azure Blockchain Workbench, è necessario configurare l'**URL di risposta** dell'applicazione client Azure Active Directory (Azure AD) dell'indirizzo Web di Blockchain Workbench distribuito.

1. Accedere al [portale di Azure](https://portal.azure.com).
1. Verificare di trovarsi all'interno del tenant in cui è stata registrata l'applicazione client Azure AD.
1. Nel riquadro di spostamento a sinistra selezionare il servizio **Azure Active Directory**. Selezionare **Registrazioni per l'app**.
1. Selezionare l'applicazione client Azure AD registrata nella sezione delle operazioni preliminari.
1. Select **Authentication**.
1. Specify the main web URL of the Azure Blockchain Workbench deployment you retrieved in the [Blockchain Workbench web URL](#blockchain-workbench-web-url) section. L'URL di risposta è preceduto dal prefisso `https://`. Ad esempio: `https://myblockchain2-7v75.azurewebsites.net`

    ![Authentication reply URLs](media/deploy/configure-reply-url.png)

1. In the **Advanced setting** section, check **Access tokens** and **ID tokens**.

    ![Authentication advanced settings](media/deploy/authentication-advanced-settings.png)

1. Selezionare **Salva** per aggiornare la registrazione del client.

## <a name="remove-a-deployment"></a>Rimuovere una distribuzione

È possibile rimuovere una distribuzione non più necessaria eliminando il gruppo di risorse di Blockchain Workbench.

1. Nel portale di Azure passare a **Gruppo di risorse** nel riquadro di spostamento a sinistra e selezionare il gruppo di risorse da eliminare. 
1. Selezionare **Elimina gruppo di risorse**. Confermare l'eliminazione immettendo il nome del gruppo di risorse e selezionare **Elimina**.

    ![Eliminare un gruppo di risorse](media/deploy/delete-resource-group.png)

## <a name="next-steps"></a>Passaggi successivi

In questa procedura dettagliata si è eseguita la distribuzione di Azure Blockchain Workbench. Per informazioni su come creare un'applicazione blockchain, proseguire con la procedura dettagliata successiva.

> [!div class="nextstepaction"]
> [Creare un'applicazione blockchain in Azure Blockchain Workbench](create-app.md)
