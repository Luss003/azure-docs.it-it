---
title: Create a resilient access control management strategy - Azure AD
description: Questo documento fornisce materiale sussidiario sulle strategie che un'organizzazione deve adottare per garantire la resilienza e ridurre il rischio di blocco durante interruzioni impreviste
services: active-directory
author: martincoetzer
manager: daveba
tags: azuread
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 12/19/2018
ms.author: martinco
ms.collection: M365-identity-device-management
ms.openlocfilehash: 478cccb3a8235291a4c4f0566cd130b4b75dbe6b
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/20/2019
ms.locfileid: "74208563"
---
# <a name="create-a-resilient-access-control-management-strategy-with-azure-active-directory"></a>Creare una strategia di gestione di controllo di accesso resiliente con Azure Active Directory

>[!NOTE]
> Le informazioni contenute in questo documento rappresentano la posizione attuale di Microsoft Corporation sulle problematiche trattate alla data di pubblicazione. Poiché Microsoft deve rispondere alle mutevoli condizioni del mercato, questo non deve essere interpretato come un impegno da parte di Microsoft, che non garantisce l'accuratezza delle informazioni presentate dopo la data di pubblicazione.

Le organizzazioni che proteggono i loro sistemi IT basandosi su un controllo di accesso singolo, ad esempio l'autenticazione a più fattori (MFA) o un unico percorso di rete, sono soggette a errori di accesso alle loro app e risorse se tale controllo di accesso singolo non è disponibile o è non è configurato correttamente. Una calamità naturale, ad esempio, può causare l'indisponibilità di segmenti di grandi dimensioni di un'infrastruttura di telecomunicazioni o reti aziendali. Un'interruzione di questo tipo potrebbe togliere la possibilità di accedere agli utenti finali e agli amministratori.

Questo documento fornisce materiale sussidiario sulle strategie che un'organizzazione deve adottare per garantire la resilienza e ridurre il rischio di blocco durante interruzioni impreviste con i seguenti scenari:

 1. Le organizzazioni possono aumentare la resilienza per ridurre il rischio di blocco **prima di un'interruzione** implementando le strategie di mitigazione o piani di emergenza.
 2. Grazie alle strategie di mitigazione e ai piani di emergenza, le organizzazioni possono continuare ad accedere alle app e risorse scelte **durante un'interruzione**.
 3. Le organizzazioni devono assicurarsi di conservare le informazioni, come ad esempio i log, **dopo un'interruzione** e prima di eseguire il rollback di eventuali contingenze implementate.
 4. Le organizzazioni che non hanno implementato strategie di prevenzione o piani alternativi potrebbero implementare **opzioni di emergenza** per affrontare l'interruzione.

## <a name="key-guidance"></a>Materiale sussidiario chiave

Da questo documento si traggono quattro punti chiave:

* Evitare il blocco dell'amministratore usando account di accesso di emergenza.
* Implement MFA using Conditional Access (CA) rather than per-user MFA.
* Mitigate user lockout by using multiple Conditional Access (CA) controls.
* Ridurre i blocchi dell'utente effettuando il provisioning di più metodi o equivalenti di autenticazione per ciascun utente.

## <a name="before-a-disruption"></a>Prima di un'interruzione

La riduzione di un'interruzione reale deve essere l'obiettivo principale di un'organizzazione nella gestione dei problemi di controllo di accesso che potrebbero verificarsi. La riduzione del rischio include la pianificazione di un evento reale e l'implementazione di strategie per assicurarsi che i controlli e le operazioni di accesso non siano coinvolti durante le interruzioni.

### <a name="why-do-you-need-resilient-access-control"></a>Perché è necessario il controllo di accesso resiliente?

 L'identità è il piano di controllo degli utenti che accedono alle app e risorse. Il sistema di identità consente di controllare quali utenti ottengono accesso alle applicazioni e in quali condizioni, ad esempio tramite controlli di accesso o requisiti di autenticazione. Quando uno o più requisiti di autenticazione o controllo di accesso non sono disponibili per l'autenticazione degli utenti a causa di circostanze impreviste, le organizzazioni possono ritrovarsi in una o entrambe le situazioni seguenti:

* **Administrator lockout:** Administrators can’t manage the tenant or services.
* **User lockout:** Users can’t access apps or resources.

### <a name="administrator-lockout-contingency"></a>Piano di emergenza per il blocco dell'amministratore

Per sbloccare l'accesso dell'amministratore al tenant, è consigliabile creare account di accesso di emergenza. Questi account, noti anche come account *break glass*, consentono l'accesso per gestire la configurazione di Azure AD quando non sono disponibili le normali procedure di accesso con privilegi. Devono essere creati almeno due account di accesso di emergenza seguendo le [indicazioni relative agli account di accesso di emergenza]( https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access).

### <a name="mitigating-user-lockout"></a>Riduzione del blocco dell'utente

 To mitigate the risk of user lockout, use Conditional Access policies with multiple controls to give users a choice of how they will access apps and resources. Se si permette a un utente di scegliere tra, ad esempio, accedere con autenticazione a più fattori **o** da un dispositivo gestito **o** dalla rete aziendale, se uno dei controlli di accesso non è disponibile l'utente ha altre opzioni per continuare a lavorare.

#### <a name="microsoft-recommendations"></a>Elementi consigliati di Microsoft

Incorporate the following access controls in your existing Conditional Access policies for organization:

1. Effettuare il provisioning di più metodi di autenticazione per ogni utente che si basano su canali di comunicazione diversi, ad esempio l'app Microsoft Authenticator (basata su internet), il token OATH (generato sul dispositivo) e l'autenticazione via SMS (telefonica).
2. Distribuire Windows Hello for Business nei dispositivi Windows 10 per soddisfare i requisiti di autenticazione a più fattori direttamente dall'accesso del dispositivo.
3. Usare dispositivi attendibili tramite [Aggiunta ad Azure AD ibrido](https://docs.microsoft.com/azure/active-directory/devices/overview) o [ dispositivi gestiti di Microsoft Intune](https://docs.microsoft.com/intune/planning-guide). Un dispositivo attendibile migliorerà l'esperienza dell'utente, in quanto il dispositivo stesso è in grado di soddisfare i requisiti avanzati di autenticazione dei criteri senza una richiesta di autenticazione a più fattori per l'utente. L'autenticazione a più fattori sarà poi necessaria durante la registrazione di un nuovo dispositivo e durante l'accesso alle app o risorse da dispositivi non attendibili.
4. Usare criteri di protezione dell'identità di Azure AD basati sul rischio, i quali impediscono l'accesso quando l'utente o l'accesso è a rischio, piuttosto che criteri di autenticazione a più fattori fissi.

>[!NOTE]
> I criteri basati sul rischio richiedono licenze di [Azure AD Premium P2](https://azure.microsoft.com/pricing/details/active-directory/).

L'esempio seguente descrive i criteri da creare per fornire un controllo di accesso resiliente all'utente che vuole accedere alle app e risorse. In questo esempio, saranno necessari un gruppo di sicurezza **AppUsers** con gli utenti di destinazione ai quali si vuole consentire l'accesso, uno denominato **CoreAdmins** con gli amministratori di core e uno denominato  **EmergencyAccess** con gli account di accesso di emergenza.
Questo set di criteri di esempio concederà, agli utenti selezionati in **AppUsers**, l'accesso alle app selezionate se sono connessi da un dispositivo attendibile OPPURE se forniscono un'autenticazione avanzata, come l'autenticazione a più fattori. Il criterio esclude gli account di emergenza e gli amministratori di core.

**Set di criteri di mitigazione dell'accesso condizionale:**

* Policy 1: Block access to people outside target groups
  * Users and Groups: Include all users. Escludi AppUsers CoreAdmins ed EmergencyAccess
  * Cloud Apps: Include all apps
  * Conditions: (None)
  * Grant Control: Block
* Policy 2: Grant access to AppUsers requiring MFA OR trusted device.
  * Users and Groups: Include AppUsers. Escludi CoreAdmins ed EmergencyAccess
  * Cloud Apps: Include all apps
  * Conditions: (None)
  * Grant Control: Grant access, require multi-factor authentication, require device to be compliant. For multiple controls: Require one of the selected controls.

### <a name="contingencies-for-user-lockout"></a>Contingenze per blocco dell'utente

In alternativa, l'organizzazione può anche creare dei criteri di emergenza. Per creare i criteri di emergenza, è necessario definire i criteri di compromesso tra continuità aziendale, costo operativo, costo finanziario e rischi relativi alla sicurezza. Ad esempio, è possibile attivare un criterio di emergenza solo in un subset di utenti, per un subset di app, per un subset di client o da un subset di percorsi. I criteri di emergenza forniranno l'accesso ad app e risorse agli amministratori e agli utenti finali durante un'interruzione se non è stato implementato alcun metodo di mitigazione dei rischi.
Comprendere l'esposizione durante un'interruzione aiuta a ridurre i rischi ed è una parte essenziale del processo di pianificazione. Per creare il piano di emergenza, determinare innanzitutto i seguenti requisiti aziendali dell'organizzazione:

1. Determine your mission critical apps ahead of time: What are the apps that you must give access to, even with a lower risk/security posture? Compilare un elenco di queste app e assicurarsi che gli altri stakeholder (business, sicurezza, legali, leadership) concordino sul fatto che se tutto il controllo di accesso è indisponibile, l'esecuzione di queste app deve comunque continuare. È probabile che si vadano a formare le seguenti categorie:
   * **Categoria 1: app di importanza strategica fondamentale** che non possono essere indisponibili per più di pochi minuti, ad esempio le app che hanno un effetto diretto sui ricavi dell'organizzazione.
   * **Categoria 2: app importanti** che devono tornare accessibili per l'azienda entro poche ore.
   * **Categoria 3: app di priorità bassa** che possono sopportare un'interruzione di alcuni giorni.
2. Per le app nella categoria 1 e 2, Microsoft consiglia di pianificare in anticipo quale tipo di livello di accesso si desidera consentire:
   * Si desidera consentire l'accesso completo o una sessione con restrizioni, come la limitazione dei download?
   * Si desidera consentire l'accesso a parte dell'app, ma non all'intera app?
   * Si desidera consentire l'accesso all'information worker e bloccare l'accesso dell'amministratore fino al ripristino del controllo di accesso?
3. Per tali app, è consigliato inoltre pianificare quali vie di accesso saranno aperte deliberatamente e quali invece saranno chiuse:
   * Si desidera consentire esclusivamente l'accesso dal browser e bloccare i rich client in grado di salvare i dati offline?
   * Si desidera consentire l'accesso solo agli utenti all'interno della rete aziendale e mantenere il blocco degli utenti esterni?
   * Si desidera consentire l'accesso esclusivamente da determinati paesi o aree geografiche durante l'interruzione?
   * Si desidera che i criteri di emergenza, in particolare nel casi di app di importanza strategica fondamentale, abbiano esito positivo o negativo se un controllo di accesso alternativo non è disponibile?

#### <a name="microsoft-recommendations"></a>Elementi consigliati di Microsoft

A contingency Conditional Access policy is a **disabled policy** that omits Azure MFA, third-party MFA, risk-based or device-based controls. Quindi, quando l'organizzazione decide di attivare il piano di emergenza, gli amministratori possono abilitare il criterio e disabilitare i normali criteri basati sui controlli.

>[!IMPORTANT]
> La disabilitazione dei criteri che applicano la sicurezza sugli utenti, anche solo temporaneamente, ridurrà il livello di sicurezza mentre il piano di emergenza è in funzione.

* Configurare un set di criteri di fallback se un'interruzione in un tipo di credenziali o in un meccanismo di controllo di accesso ha effetti sull'accesso alle app. Configurare un criterio disattivato che richiede il controllo di aggiunta a un dominio come backup per un criterio attivo che richiede un provider di autenticazione a più fattori di terze parti.
* Ridurre il rischio di password indovinate da malintenzionati, quando l'autenticazione a più fattori non è necessaria, seguendo le procedure consigliate nel white paper relativo alle [indicazioni sulle password](https://aka.ms/passwordguidance).
* Distribuire [Reimpostazione self-service delle password di Azure AD (SSPR)](https://docs.microsoft.com/azure/active-directory/authentication/quickstart-sspr) e [Protezione della password di Azure AD](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-ban-bad-on-premises-deploy) per assicurarsi che gli utenti non usino password comuni e termini esclusi.
* Usare criteri che limitano l'accesso all'interno delle app se non si raggiunge un determinato livello di autenticazione, anziché eseguire semplicemente il fallback all'accesso completo. ad esempio:
  * Configurare un criterio di backup che invia l'attestazione di sessione con restrizioni a Exchange e SharePoint.
  * Se l'organizzazione usa Microsoft Cloud App Security, è consigliabile eseguire il fallback a un criterio che coinvolga MCAS, quindi che MCAS consenta l'accesso di sola lettura, ma non il caricamento.
* Assegnare un nome ai criteri per esseri sicuri di trovarli facilmente durante un'interruzione. Includere gli elementi seguenti nel nome dei criteri:
  * Un *numero di etichetta* per i criteri.
  * Il testo da visualizzare. Questo criterio è destinato solo ai casi di emergenza. For example: **ENABLE IN EMERGENCY**
  * L'*interruzione* a cui si applica il criterio. For example: **During MFA Disruption**
  * Un *numero di sequenza* per mostrare l'ordine in cui è necessario attivare i criteri.
  * Le *app* a cui si applica il criterio.
  * I *controlli* a cui si applicherà il criterio.
  * Le *condizioni* richieste.
  
Lo standard di denominazione per i criteri di emergenza avrà il formato seguente: 

```
EMnnn - ENABLE IN EMERGENCY: [Disruption][i/n] - [Apps] - [Controls] [Conditions]
```

The following example: **Example A - Contingency CA policy to restore Access to mission-critical Collaboration Apps**, is a typical corporate contingency. In questo scenario, l'organizzazione in genere richiede l'autenticazione a più fattori per tutti gli accessi a Exchange Online e SharePoint Online. In questo caso, l'interruzione coinvolge il servizio del provider di autenticazione a più fattori per il cliente (Azure, provider locale o terze parti). Questo criterio riduce l'interruzione permettendo a utenti di destinazione specifici di accedere a tali app da dispositivi Windows attendibili esclusivamente quando l'accesso all'app si verifica tramite la rete aziendale attendibile. Anche gli account di emergenza e degli amministratori di core saranno esclusi da queste restrizioni. Gli utenti di destinazione otterranno quindi l'accesso a Exchange Online e SharePoint Online, mentre gli altri utenti continueranno a non poter accedere alle app a causa dell'interruzione del servizio. In questo esempio, saranno necessari un percorso di rete denominato **CorpNetwork**, un gruppo di sicurezza **ContingencyAccess** con gli utenti di destinazione, uno denominato **CoreAdmins** con gli amministratori di core e uno denominato **EmergencyAccess** con gli account di accesso di emergenza. L'emergenza richiede quattro criteri per garantire l'accesso desiderato. 

**Esempio A: criteri di accesso condizionale di emergenza per il ripristino dell'accesso alle app di collaborazione di importanza strategica fondamentale:**

* Policy 1: Require Domain Joined devices for Exchange and SharePoint
  * Name: EM001 - ENABLE IN EMERGENCY: MFA Disruption[1/4] - Exchange SharePoint - Require Hybrid Azure AD Join
  * Users and Groups: Include ContingencyAccess. Escludi CoreAdmins ed EmergencyAccess
  * Cloud Apps: Exchange Online and SharePoint Online
  * Conditions: Any
  * Grant Control: Require Domain Joined
  * State: Disabled
* Policy 2: Block platforms other than Windows
  * Name: EM002 - ENABLE IN EMERGENCY: MFA Disruption[2/4] - Exchange SharePoint - Block access except Windows
  * Users and Groups: Include all users. Escludi CoreAdmins ed EmergencyAccess
  * Cloud Apps: Exchange Online and SharePoint Online
  * Conditions: Device Platform Include All Platforms, exclude Windows
  * Grant Control: Block
  * State: Disabled
* Policy 3: Block networks other than CorpNetwork
  * Name: EM003 - ENABLE IN EMERGENCY: MFA Disruption[3/4] - Exchange SharePoint - Block access except Corporate Network
  * Users and Groups: Include all users. Escludi CoreAdmins ed EmergencyAccess
  * Cloud Apps: Exchange Online and SharePoint Online
  * Conditions: Locations Include any location, exclude CorpNetwork
  * Grant Control: Block
  * State: Disabled
* Policy 4: Block EAS Explicitly
  * Name: EM004 - ENABLE IN EMERGENCY: MFA Disruption[4/4] - Exchange - Block EAS for all users
  * Users and Groups: Include all users
  * Cloud Apps: Include Exchange Online
  * Conditions: Client apps: Exchange Active Sync
  * Grant Control: Block
  * State: Disabled

Ordine di attivazione:

1. Escludere ContingencyAccess, CoreAdmins ed EmergencyAccess dai criteri di autenticazione a più fattori esistenti. Verificare che un utente in ContingencyAccess possa accedere a SharePoint Online ed Exchange Online.
2. Enable Policy 1: Verify users on Domain Joined devices who are not in the exclude groups are able to access Exchange Online and SharePoint Online. Verificare che gli utenti nel gruppo Exclude possano accedere a SharePoint Online ed Exchange da qualsiasi dispositivo.
3. Enable Policy 2: Verify users who are not in the exclude group cannot get to SharePoint Online and Exchange Online from their mobile devices. Verificare che gli utenti nel gruppo Exclude possano accedere a SharePoint ed Exchange da qualsiasi dispositivo (Windows/iOS/Android).
4. Enable Policy 3: Verify users who are not in the exclude groups cannot access SharePoint and Exchange off the corporate network, even with a domain joined machine. Verificare che gli utenti nel gruppo di esclusione possano accedere a SharePoint ed Exchange da qualsiasi rete.
5. Enable Policy 4: Verify all users cannot get Exchange Online from the native mail applications on mobile devices.
6. Disabilitare i criteri di autenticazione a più fattori esistenti per SharePoint Online ed Exchange Online.

In questo esempio successivo, **Esempio B: criteri di accesso condizionale di emergenza per consentire l'accesso a Salesforce da dispositivi mobili**, viene ripristinata l'accesso a un app aziendale. In questo scenario, in genere, il cliente richiede che l'accesso a Salesforce (configurato per l'accesso singolo con Azure AD) da parte degli addetti alla vendita con dispositivi mobili sia consentito solo se tramite dispositivi conformi. L'interruzione, in questo caso, consiste in un problema nella valutazione della conformità dei dispositivi. Si è verificata un'interruzione a un orario delicato, dove il team vendite deve accedere a Salesforce per chiudere delle operazioni commerciali. Questi criteri di emergenza concederanno l'accesso a Salesforce da un dispositivo mobile agli utenti fondamentali, in modo da poter continuare a chiudere operazioni commerciali senza interrompere le attività. In questo esempio, **SalesforceContingency** contiene tutti gli addetti alle vendite che hanno bisogno di mantenere l'accesso, mentre **SalesAdmins** include gli amministratori di Salesforce necessari.

**Esempio B: criteri di accesso condizionale di emergenza:**

* Policy 1: Block everyone not in the SalesContingency team
  * Name: EM001 - ENABLE IN EMERGENCY: Device Compliance Disruption[1/2] - Salesforce - Block All users except SalesforceContingency
  * Users and Groups: Include all users. Escludere SalesAdmins e SalesforceContingency
  * Cloud Apps: Salesforce.
  * Conditions: None
  * Grant Control: Block
  * State: Disabled
* Policy 2: Block the Sales team from any platform other than mobile (to reduce surface area of attack)
  * Name: EM002 - ENABLE IN EMERGENCY: Device Compliance Disruption[2/2] - Salesforce - Block All platforms except iOS and Android
  * Users and Groups: Include SalesforceContingency. Escludere SalesAdmins
  * Cloud Apps: Salesforce
  * Conditions: Device Platform Include All Platforms, exclude iOS and Android
  * Grant Control: Block
  * State: Disabled

Ordine di attivazione:

1. Escludere SalesAdmins e SalesforceContingency dai criteri di conformità del dispositivo esistenti per Salesforce. Verificare che un utente nel gruppo SalesforceContingency possa accedere a Salesforce.
2. Enable Policy 1: Verify users outside of SalesContingency cannot access Salesforce. Verificare che gli utenti in SalesAdmins e SalesforceContingency possano accedere a Salesforce.
3. Enable Policy 2: Verify users in the SalesContingency group cannot access Salesforce from their Windows/Mac laptops but can still access from their mobile devices. Verificare che SalesAdmin possa comunque accedere a Salesforce da qualsiasi dispositivo.
4. Disabilitare i criteri di conformità del dispositivo esistenti per Salesforce.

### <a name="deploy-password-hash-sync-even-if-you-are-federated-or-use-pass-through-authentication"></a>Distribuire la sincronizzazione dell'hash delle password, anche in caso di federazione o uso dell'autenticazione pass-through

Il blocco dell'utente può inoltre verificarsi se sussistono le condizioni seguenti:

- L'organizzazione usa una soluzione a identità ibrida con autenticazione pass-through o federazione.
- I sistemi di identità in locale (ad esempio Active Directory, AD FS o un componente dipendente) non sono disponibili. 
 
Per essere più resiliente, l'organizzazione dovrebbe [abilitare la sincronizzazione dell'hash delle password](https://docs.microsoft.com/azure/security/fundamentals/choose-ad-authn), in quanto consente di [passare all'uso della sincronizzazione dell'hash delle password](https://docs.microsoft.com/azure/active-directory/hybrid/plan-connect-user-signin) se i sistemi di identità in locale sono inattivi.

#### <a name="microsoft-recommendations"></a>Elementi consigliati di Microsoft
 Abilitare la sincronizzazione dell'hash delle password usando la procedura guidata di Azure AD Connect, indipendentemente dal fatto che l'organizzazione usi l'autenticazione pass-through o la federazione.

>[!IMPORTANT]
> Per usare la sincronizzazione dell'hash delle password, non è necessario convertire gli utenti dalla federazione all'autenticazione gestita.

## <a name="during-a-disruption"></a>Durante un'interruzione

Se si è scelto di implementare il piano di mitigazione, sarà possibile sopravvivere automaticamente a un'interruzione del controllo di accesso singolo. Tuttavia, se si è scelto di creare un piano di emergenza, sarà possibile attivare i criteri di emergenza durante l'interruzione del controllo di accesso:

1. Abilitare i criteri di emergenza che concedono l'accesso ad app specifiche, da reti specifiche, agli utenti di destinazione.
2. Disabilitare i normali criteri basati sui controlli.

### <a name="microsoft-recommendations"></a>Elementi consigliati di Microsoft

A seconda delle mitigazioni o emergenze usate durante un'interruzione, l'organizzazione potrebbe garantire l'accesso mediante le sole password. Non adottare nessuna misura di protezione è un rischio per la sicurezza notevole da valutare con attenzione. Le organizzazioni devono:

1. Come parte della strategia di controllo modifiche, documentare tutte le modifiche e lo stato precedente per poter eseguire il rollback di qualsiasi emergenza implementata non appena i controlli di accesso sono completamente operativi.
2. Presupporre che i malintenzionati tenteranno di rubare password tramite attacchi di tipo password spray o phishing quando si disabilita l'autenticazione a più fattori. Inoltre, i malintenzionati potrebbero avere già password che, se in precedenza non consentivano l'accesso a nessuna risorsa, possono essere tentate durante questo intervallo. Per gli utenti fondamentali, come i dirigenti, è possibile ridurre parzialmente questo rischio reimpostando le loro password prima di disabilitare l'autenticazione a più fattori.
3. Archiviare tutte le attività di accesso per identificare chi accede a cosa durante la fase in cui l'autenticazione a più fattori è disabilitata.
4. [Triage all risk detections reported](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-sign-ins) during this window.

## <a name="after-a-disruption"></a>Dopo un'interruzione

Annullare le modifiche apportate nell'ambito del piano di emergenza attivato una volta ripristinato il servizio che ha causato l'interruzione. 

1. Abilitare i criteri normali
2. Disabilitare i criteri di emergenza. 
3. Eseguire il rollback delle modifiche apportate e documentate durante l'interruzione.
4. Se si usa un account di accesso di emergenza, ricordarsi di rigenerare le credenziali e proteggere fisicamente i dettagli delle nuove credenziali come parte delle procedure di account di accesso di emergenza.
5. Continue to [triage all risk detections reported](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-sign-ins) after the disruption for suspicious activity.
6. Revocare tutti i token di aggiornamento rilasciati [usando PowerShell](https://docs.microsoft.com/powershell/module/azuread/revoke-azureaduserallrefreshtoken?view=azureadps-2.0) e destinati a un set di utenti. La revoca di tutti i token di aggiornamento è importante per gli account con privilegi usati durante l'interruzione. Questa operazione costringerà gli utenti a ripetere l'autenticazione e rispettare il controllo dei criteri ripristinati.

## <a name="emergency-options"></a>Opzioni di emergenza

 In case of an emergency and your organization did not previously implement a mitigation or contingency plan, then follow the recommendations in the [Contingencies for user lockout](#contingencies-for-user-lockout) section if they already use Conditional Access policies to enforce MFA.
Se l'organizzazione usa criteri di autenticazione a più fattori obsoleti per l'utente, è possibile prendere in considerazione l'alternativa seguente:

1. Se si dispone dell'indirizzo IP in uscita della rete aziendale, è possibile aggiungerli come indirizzi IP attendibili per abilitare l'autenticazione esclusivamente sulla rete aziendale.
   1. Se non si ha l'inventario degli indirizzi IP in uscita, o se è necessario abilitare l'accesso all'interno e all'esterno della rete aziendale, è possibile aggiungere l'intero spazio indirizzi IPv4 come indirizzi IP attendibili specificando 0.0.0.0/1 e 128.0.0.0/1.

>[!IMPORTANT]
 > If you broaden the trusted IP addresses to unblock access, risk detections associated with IP addresses (for example, impossible travel or unfamiliar locations) will not be generated.

>[!NOTE]
 > La configurazione di [indirizzi IP attendibili](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-mfasettings) per l'autenticazione a più fattori di Azure è disponibile solo con [licenze Azure AD Premium](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-licensing).

## <a name="learn-more"></a>Altre informazioni.

* [Documentazione di Autenticazione di Azure AD](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-iis)
* [Gestire gli account amministrativi di accesso di emergenza in Azure AD](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access)
* [Configurare località denominate in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)
  * [Set-MsolDomainFederationSettings](https://docs.microsoft.com/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0)
* [Come configurare dispositivi aggiunti all'identità ibrida di Azure Active Directory](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-plan)
* [Windows Hello for Business Deployment Guide](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-deployment-guide) (Guida alla distribuzione di Windows Hello fo Business)
  * [Materiale sussidiario sulle password - Microsoft Research](https://research.microsoft.com/pubs/265143/microsoft_password_guidance.pdf)
* [What are conditions in Azure Active Directory Conditional Access?](https://docs.microsoft.com/azure/active-directory/conditional-access/conditions)
* [What are access controls in Azure Active Directory Conditional Access?](https://docs.microsoft.com/azure/active-directory/conditional-access/controls)
