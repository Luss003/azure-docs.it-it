---
title: App client approvate con accesso condizionale-Azure Active Directory
description: Informazioni su come richiedere app client approvate per l'accesso alle app cloud con accesso condizionale in Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: spunukol
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1a8832234978a2c8b2db25d88b5dd6c211b634b7
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2020
ms.locfileid: "77186461"
---
# <a name="how-to-require-approved-client-apps-for-cloud-app-access-with-conditional-access"></a>Procedura: richiedere app client approvate per l'accesso alle app cloud con accesso condizionale 

I dipendenti usano dispositivi mobili sia per le attività personali che per quelle aziendali. È importante assicurarsi che i dipendenti siano produttivi e al contempo evitare la perdita di dati. Con l'accesso condizionale Azure Active Directory (Azure AD), è possibile limitare l'accesso alle app cloud alle app client approvate che possono proteggere i dati aziendali.  

Questo argomento illustra come configurare criteri di accesso che richiedono app client approvate.

## <a name="overview"></a>Panoramica

Con [l'accesso condizionale Azure ad](overview.md), è possibile ottimizzare il modo in cui gli utenti autorizzati possono accedere alle risorse. Si può, ad esempio, fare in modo che solo i dispositivi attendibili accedano alle app cloud.

È possibile usare i [criteri di protezione app di Intune](https://docs.microsoft.com/intune/app-protection-policy) per proteggere i dati aziendali. I criteri di protezione app di Intune non richiedono una soluzione di gestione dei dispositivi mobili (MDM), che consente di proteggere i dati aziendali con o senza la registrazione dei dispositivi in una soluzione di gestione di dispositivi.

Azure Active Directory l'accesso condizionale consente di limitare l'accesso alle app cloud alle app client che supportano i criteri di protezione delle app di Intune. È possibile, ad esempio, limitare l'accesso a Exchange Online all'app Outlook.

Nella terminologia relativa all'accesso condizionale queste app client sono note come **app client approvate**.  

![Accesso condizionale](./media/app-based-conditional-access/05.png)

Per un elenco di app client approvate, vedere [Requisito per le app client approvate](concept-conditional-access-grant.md).

È possibile combinare i criteri di accesso condizionale basato su app con altri criteri, ad esempio [criteri di accesso condizionale basato su dispositivo](require-managed-devices.md) , per fornire flessibilità nella protezione dei dati per i dispositivi personali e aziendali.

## <a name="before-you-begin"></a>Prima di iniziare

Questo argomento presuppone che l'utente abbia familiarità con:

- [Requisito dell'app client approvata](concept-conditional-access-grant.md).
- I concetti di base dell' [accesso condizionale in Azure Active Directory](overview.md).
- Come [configurare un criterio di accesso condizionale](app-based-mfa.md).
- [Migrazione dei criteri di accesso condizionale](best-practices.md#policy-migration).

## <a name="prerequisites"></a>Prerequisites

Per creare un criterio di accesso condizionale basato su app, è necessario avere un Enterprise Mobility + Security o una sottoscrizione Azure Active Directory Premium e gli utenti devono avere una licenza per EMS o Azure AD. 

## <a name="exchange-online-policy"></a>Criteri per Exchange Online 

Questo scenario è costituito da criteri di accesso condizionale basato su app per l'accesso a Exchange Online.

### <a name="scenario-playbook"></a>Istruzioni dello scenario

Questo scenario presuppone che un utente:

- Configuri la posta elettronica con un'applicazione di posta elettronica nativa in iOS o Android per la connessione a Exchange
- Riceva un messaggio di posta elettronica in cui è indicato che l'accesso è disponibile solo tramite l'app Outlook
- Esegua il download dell'applicazione con il collegamento
- Apra l'applicazione Outlook e acceda con le credenziali di Azure AD
- Riceva un messaggio in cui viene chiesto di installare Authenticator (iOS) o Portale aziendale (Android) per continuare
- Installi l'applicazione e possa tornare all'app Outlook per continuare
- Riceva un messaggio in cui viene chiesto di registrare un dispositivo
- Sia in grado di accedere alla posta elettronica

Tutti i criteri di protezione delle app di Intune vengono attivati al momento dell'accesso ai dati aziendali e possono richiedere all'utente di riavviare l'applicazione, usare un PIN aggiuntivo e così via (se configurato per l'applicazione e la piattaforma).

### <a name="configuration"></a>Configurazione 

**Passaggio 1: configurare un Azure AD criteri di accesso condizionale per Exchange Online**

Per i criteri di accesso condizionale in questo passaggio, è necessario configurare i componenti seguenti:

1. **Nome** dei criteri di accesso condizionale.
1. **Utenti e gruppi**: per ogni criterio di accesso condizionale deve essere selezionato almeno un utente o un gruppo.
1. **App cloud**: come app cloud è necessario selezionare **Office 365 Exchange Online**.
1. **Condizioni**: come **Condizioni** è necessario configurare **Piattaforme del dispositivo** e **App client**:
   1. Come **Piattaforme del dispositivo** selezionare **Android** e **iOS**.
   1. Come **App client (anteprima)** selezionare **App per dispositivi mobili e client desktop** e **Client di autenticazione moderna**.
1. Come **Controlli di accesso** è necessario che sia selezionata l'opzione **Richiedi app client approvata (anteprima)** .

   ![Accesso condizionale](./media/app-based-conditional-access/05.png)

**Passaggio 2: configurare un Azure AD criteri di accesso condizionale per Exchange Online con Active Sync (EAS)**

Per i criteri di accesso condizionale in questo passaggio, è necessario configurare i componenti seguenti:

1. **Nome** dei criteri di accesso condizionale.
1. **Utenti e gruppi**: per ogni criterio di accesso condizionale deve essere selezionato almeno un utente o un gruppo.
1. **App cloud**: come app cloud è necessario selezionare **Office 365 Exchange Online**.
1. **Condizioni**: come **Condizioni** è necessario configurare **App client (anteprima)** . 
   1. Come **App client (anteprima)** selezionare **App per dispositivi mobili e client desktop** e **Client Exchange ActiveSync**.
   1. Come **Controlli di accesso** è necessario che sia selezionata l'opzione **Richiedi app client approvata (anteprima)** .

      ![Accesso condizionale](./media/app-based-conditional-access/05.png)

**Passaggio 3: Configurare i criteri di protezione delle app di Intune per le applicazioni client iOS e Android**

Per altre informazioni, vedere [Proteggere app e dati con Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).

## <a name="exchange-online-and-sharepoint-online-policy"></a>Criteri per Exchange Online e SharePoint Online

Questo scenario è costituito da un accesso condizionale con criteri di gestione delle app mobili per l'accesso a Exchange Online e SharePoint Online con app approvate.

### <a name="scenario-playbook"></a>Istruzioni dello scenario

Questo scenario presuppone che un utente:

- Provi a usare l'app SharePoint per connettersi a siti aziendali e visualizzarli
- Tenta di accedere con le stesse credenziali delle credenziali dell'app Outlook
- Non debba ripetere la registrazione e riesca a ottenere l'accesso alle risorse

### <a name="configuration"></a>Configurazione

**Passaggio 1: configurare un Azure AD criteri di accesso condizionale per Exchange Online e SharePoint Online**

Per i criteri di accesso condizionale in questo passaggio, è necessario configurare i componenti seguenti:

1. **Nome** dei criteri di accesso condizionale.
1. **Utenti e gruppi**: per ogni criterio di accesso condizionale deve essere selezionato almeno un utente o un gruppo.
1. **App cloud:** come app cloud è necessario selezionare **Office 365 Exchange Online** e **Office 365 SharePoint Online**. 
1. **Condizioni**: come **Condizioni** è necessario configurare **Piattaforme del dispositivo** e **App client**:
   1. Come **Piattaforme del dispositivo** selezionare **Android** e **iOS**.
   1. Come **App client (anteprima)** selezionare **App per dispositivi mobili e client desktop** e **Client di autenticazione moderna**.
1. Come **Controlli di accesso** è necessario che sia selezionata l'opzione **Richiedi app client approvata (anteprima)** .

   ![Accesso condizionale](./media/app-based-conditional-access/05.png)

**Passaggio 2: configurare un Azure AD criteri di accesso condizionale per Exchange Online con Active Sync (EAS)**

Per i criteri di accesso condizionale in questo passaggio, è necessario configurare i componenti seguenti:

1. **Nome** dei criteri di accesso condizionale.
1. **Utenti e gruppi**: per ogni criterio di accesso condizionale deve essere selezionato almeno un utente o un gruppo.
1. **App cloud**: come app cloud è necessario selezionare **Office 365 Exchange Online**. Online 
1. **Condizioni**: come **Condizioni** è necessario configurare **App client**:
   1. Come **App client (anteprima)** selezionare **App per dispositivi mobili e client desktop** e **Client Exchange ActiveSync**.
   1. Come **Controlli di accesso** è necessario che sia selezionata l'opzione **Richiedi app client approvata (anteprima)** .

      ![Accesso condizionale](./media/app-based-conditional-access/05.png)

**Passaggio 3: Configurare i criteri di protezione delle app di Intune per le applicazioni client iOS e Android**

![Accesso condizionale](./media/app-based-conditional-access/09.png)

Per altre informazioni, vedere [Proteggere app e dati con Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).

## <a name="app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>Criteri di accesso basato su dispositivi conformi o basato su app per Exchange Online e SharePoint Online

Questo scenario è costituito da criteri di accesso condizionale basato su app o conformi per l'accesso a Exchange Online.

### <a name="scenario-playbook"></a>Istruzioni dello scenario

Questo scenario presuppone che:
 
- Alcuni utenti sono già registrati (con o senza dispositivi aziendali)
- Gli utenti che non hanno registrato il dispositivo e non hanno eseguito la registrazione ad Azure AD usando un'applicazione protetta tramite app debbano registrare un dispositivo per accedere alle risorse
- Gli utenti che hanno eseguito la registrazione usando l'applicazione protetta tramite app non debbano registrare nuovamente il dispositivo

### <a name="configuration"></a>Configurazione

**Passaggio 1: configurare un Azure AD criteri di accesso condizionale per Exchange Online e SharePoint Online**

Per i criteri di accesso condizionale in questo passaggio, è necessario configurare i componenti seguenti:

1. **Nome** dei criteri di accesso condizionale.
1. **Utenti e gruppi**: per ogni criterio di accesso condizionale deve essere selezionato almeno un utente o un gruppo.
1. **App cloud:** come app cloud è necessario selezionare **Office 365 Exchange Online** e **Office 365 SharePoint Online**. 
1. **Condizioni**: come **Condizioni** è necessario configurare **Piattaforme del dispositivo** e **App client**. 
   1. Come **Piattaforme del dispositivo** selezionare **Android** e **iOS**.
   1. Come **App client (anteprima)** selezionare **App per dispositivi mobili e client desktop** e **Client di autenticazione moderna**.
1. Come **Controlli di accesso** è necessario che siano selezionate le opzioni seguenti:
   - **Richiedi che i dispositivi siano contrassegnati come conformi**
   - **Richiedi app client approvata (anteprima)**
   - **Richiedi uno dei controlli selezionati**   
 
      ![Accesso condizionale](./media/app-based-conditional-access/11.png)

**Passaggio 2: configurare un Azure AD criteri di accesso condizionale per Exchange Online con Active Sync (EAS)**

Per i criteri di accesso condizionale in questo passaggio, è necessario configurare i componenti seguenti:

1. **Nome** dei criteri di accesso condizionale.
1. **Utenti e gruppi**: per ogni criterio di accesso condizionale deve essere selezionato almeno un utente o un gruppo.
1. **App cloud**: come app cloud è necessario selezionare **Office 365 Exchange Online**. 
1. **Condizioni**: come **Condizioni** è necessario configurare **App client**. 
   1. Come **App client (anteprima)** selezionare **App per dispositivi mobili e client desktop** e **Client Exchange ActiveSync**.
1. Come **Controlli di accesso** è necessario che sia selezionata l'opzione **Richiedi app client approvata (anteprima)** .
 
   ![Accesso condizionale](./media/app-based-conditional-access/11.png)

**Passaggio 3: Configurare i criteri di protezione delle app di Intune per le applicazioni client iOS e Android**

![Accesso condizionale](./media/app-based-conditional-access/09.png)

Per altre informazioni, vedere [Proteggere app e dati con Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).

## <a name="app-based-and-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>Criteri di accesso basato su dispositivi conformi e basato su app per Exchange Online e SharePoint Online

Questo scenario è costituito da un criterio di accesso condizionale basato su app e conforme per l'accesso a Exchange Online.

### <a name="scenario-playbook"></a>Istruzioni dello scenario

Questo scenario presuppone che un utente:
 
- Configuri la posta elettronica con un'applicazione di posta elettronica nativa in iOS o Android per la connessione a Exchange
- Riceva un messaggio di posta elettronica in cui è indicato che per accedere è necessario che il dispositivo sia registrato
- Esegua il download del portale aziendale e acceda a tale portale
- Controlli la posta elettronica e riceva un messaggio in cui viene chiesto di usare l'app Outlook
- Esegua il download dell'app Outlook
- Apra l'app Outlook e immetta le credenziali usate per la registrazione
- Sia in grado di accedere alla posta elettronica

I criteri di protezione delle app di Intune vengono attivati al momento dell'accesso ai dati aziendali e possono richiedere all'utente di riavviare l'applicazione, usare un PIN aggiuntivo e altro ancora, se configurato per l'applicazione e la piattaforma.

### <a name="configuration"></a>Configurazione

**Passaggio 1: configurare un Azure AD criteri di accesso condizionale per Exchange Online e SharePoint Online**

Per i criteri di accesso condizionale in questo passaggio, è necessario configurare i componenti seguenti:

1. **Nome** dei criteri di accesso condizionale.
1. **Utenti e gruppi**: per ogni criterio di accesso condizionale deve essere selezionato almeno un utente o un gruppo.
1. **App cloud:** come app cloud è necessario selezionare **Office 365 Exchange Online** e **Office 365 SharePoint Online**. 
1. **Condizioni**: come **Condizioni** è necessario configurare **Piattaforme del dispositivo** e **App client**. 
   1. Come **Piattaforme del dispositivo** selezionare **Android** e **iOS**.
   1. Come **App client (anteprima)** selezionare **App per dispositivi mobili e client desktop** e **Client di autenticazione moderna**.
1. Come **Controlli di accesso** è necessario che siano selezionate le opzioni seguenti:
   - **Richiedi che i dispositivi siano contrassegnati come conformi**
   - **Richiedi app client approvata (anteprima)**
   - **Richiedi tutti i controlli selezionati**   
 
      ![Accesso condizionale](./media/app-based-conditional-access/13.png)

**Passaggio 2: configurare un Azure AD criteri di accesso condizionale per Exchange Online con Active Sync (EAS)**

Per i criteri di accesso condizionale in questo passaggio, è necessario configurare i componenti seguenti:

1. **Nome** dei criteri di accesso condizionale.
1. **Utenti e gruppi**: per ogni criterio di accesso condizionale deve essere selezionato almeno un utente o un gruppo.
1. **App cloud**: come app cloud è necessario selezionare **Office 365 Exchange Online**. 
1. **Condizioni**: come **Condizioni** è necessario configurare **App client (anteprima)** . 
   1. Come **App client (anteprima)** selezionare **App per dispositivi mobili e client desktop** e **Client Exchange ActiveSync**.

   ![Accesso condizionale](./media/app-based-conditional-access/92.png)

1. Come **Controlli di accesso** è necessario che siano selezionate le opzioni seguenti:
   - **Richiedi che i dispositivi siano contrassegnati come conformi**
   - **Richiedi app client approvata (anteprima)**
   - **Richiedi tutti i controlli selezionati**   

**Passaggio 3: Configurare i criteri di protezione delle app di Intune per le applicazioni client iOS e Android**

![Accesso condizionale](./media/app-based-conditional-access/09.png)

Per altre informazioni, vedere [Proteggere app e dati con Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).

## <a name="next-steps"></a>Passaggi successivi

Per informazioni su come configurare criteri di accesso condizionale, vedere [Richiedere MFA per app specifiche con l'accesso condizionale di Azure Active Directory](app-based-mfa.md).

Se si è pronti per configurare i criteri di accesso condizionale per l'ambiente in uso, vedere le [procedure consigliate per l'accesso condizionale](best-practices.md) in Azure Active Directory. 
