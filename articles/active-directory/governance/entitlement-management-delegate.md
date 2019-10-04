---
title: Delegare attività in Azure AD gestione dei diritti (anteprima)-Azure Active Directory
description: Informazioni sui ruoli che è possibile assegnare alle attività delegate in Azure Active Directory gestione dei diritti.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 07/10/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6857697423e494c515bd052cb42af3ad1d9fe188
ms.sourcegitcommit: ca359c0c2dd7a0229f73ba11a690e3384d198f40
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/17/2019
ms.locfileid: "71057786"
---
# <a name="delegate-tasks-in-azure-ad-entitlement-management-preview"></a>Delegare attività in Azure AD gestione dei diritti (anteprima)

> [!IMPORTANT]
> Gestione entitlement di Azure Active Directory (Azure AD) è attualmente in anteprima pubblica.
> Questa versione di anteprima viene messa a disposizione senza contratto di servizio e non è consigliata per i carichi di lavoro di produzione. Alcune funzionalità potrebbero non essere supportate o potrebbero presentare funzionalità limitate.
> Per altre informazioni, vedere [Condizioni supplementari per l'utilizzo delle anteprime di Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Per impostazione predefinita, gli amministratori globali e gli amministratori utenti possono creare e gestire tutti gli aspetti della gestione dei diritti Azure AD. Tuttavia, gli utenti di questi ruoli potrebbero non essere a conoscenza di tutti gli scenari in cui sono necessari i pacchetti di accesso. In genere si tratta di utenti all'interno di reparti che sanno chi deve collaborare. 

Anziché concedere autorizzazioni senza restrizioni a utenti non amministratori, è possibile concedere agli utenti le autorizzazioni minime necessarie per eseguire il proprio lavoro ed evitare la creazione di diritti di accesso in conflitto o non appropriati. Questo articolo descrive i ruoli che è possibile assegnare per delegare diverse attività nella gestione dei diritti. 

## <a name="delegate-example-for-departmental-adoption"></a>Esempio di delegato per l'adozione del reparto

Per comprendere come è possibile delegare le attività nella gestione dei diritti, è utile prendere in considerazione un esempio. 

Si supponga che l'organizzazione disponga dei seguenti cinque utenti:

| Utente | department | Note |
| --- | --- | --- |
| Alice | IT | Amministratore globale |
| Bob | Research | Bob è anche proprietario di un gruppo di ricerca |
| Carole | Research |  |
| Dave | Marketing |  |
| Elisa | Marketing | Elisa è anche proprietario di un'applicazione marketing |

I reparti di ricerca e marketing desiderano utilizzare la gestione dei diritti per gli utenti. Alice non è ancora pronta per l'uso della gestione dei diritti da altri reparti. Ecco un modo in cui Alice può delegare le attività ai reparti di ricerca e marketing.

1. Alice crea un nuovo gruppo di sicurezza Azure AD per gli autori del catalogo e aggiunge Bob, Carol, Dave ed Elisa come membri del gruppo.

1. Alice usa le impostazioni di gestione dei diritti per aggiungere tale gruppo al ruolo creatori del catalogo.

1. Carol crea un catalogo di **ricerca** e aggiunge Bob come co-proprietario di tale catalogo. Bob aggiunge il gruppo di ricerca che possiede al catalogo come risorsa, in modo che possa essere usato in un pacchetto di accesso per la collaborazione alla ricerca.

1. Dave crea un catalogo **Marketing** e aggiunge Elisa come co-proprietario di tale catalogo. Elisa aggiunge l'applicazione di marketing che possiede al catalogo come risorsa, in modo che possa essere usata in un pacchetto di accesso per la collaborazione di marketing.

Ora i reparti di ricerca e marketing possono utilizzare la gestione dei diritti. Bob, Carol, Dave ed Elisa possono creare e gestire i pacchetti di accesso nei rispettivi cataloghi.

![Esempio di delegati di gestione dei diritti](./media/entitlement-management-delegate/elm-delegate.png)


## <a name="entitlement-management-roles"></a>Ruoli di gestione dei diritti

La gestione dei diritti ha i seguenti ruoli specifici per la gestione dei diritti.

| Role | DESCRIZIONE |
| --- | --- |
| Creatore del catalogo | Creare e gestire cataloghi. In genere un amministratore IT che non è un amministratore globale o un proprietario di risorse per una raccolta di risorse. La persona che crea un catalogo diventa automaticamente il primo proprietario del catalogo e può aggiungere altri proprietari del catalogo. Un autore del catalogo non può gestire o vedere cataloghi di cui non è proprietario e non può aggiungere risorse di cui non è proprietario per un catalogo. Se l'autore del catalogo deve gestire un altro catalogo o aggiungere risorse di cui non è proprietario, può richiedere di essere un comproprietario di tale catalogo o risorsa. |
| Proprietario Catalogo | Modificare e gestire cataloghi esistenti. In genere un amministratore IT o proprietari di risorse oppure un utente designato dal proprietario del catalogo. |
| Gestione pacchetti di Access | Modificare e gestire tutti i pacchetti di accesso esistenti all'interno di un catalogo. |

Inoltre, un responsabile approvazione designato e un richiedente di un pacchetto di accesso dispongono anche di diritti, anche se non sono ruoli.
 
* Approvatore Autorizzazione eseguita da un criterio per approvare o negare le richieste di accesso ai pacchetti, sebbene non possano modificare le definizioni dei pacchetti di accesso.
* Richiedente Autorizzazione eseguita da un criterio di un pacchetto di accesso per richiedere il pacchetto di accesso.

Nella tabella seguente sono elencate le attività che possono essere eseguite da questi ruoli.

| Attività | Creatore del catalogo | Proprietario Catalogo | Gestione pacchetti di Access | Responsabile approvazione |
| --- | :---: | :---: | :---: | :---: |
| [Crea un nuovo catalogo](entitlement-management-catalog-create.md) | :heavy_check_mark: |  |  |  |
| [Aggiungere una risorsa a un catalogo](entitlement-management-catalog-create.md#add-resources-to-a-catalog) | | :heavy_check_mark: | | |
| [Modificare un catalogo](entitlement-management-catalog-create.md#edit-a-catalog) |  | :heavy_check_mark: |  |  |
| [Eliminare un catalogo](entitlement-management-catalog-create.md#delete-a-catalog) |  | :heavy_check_mark: |  |  |
| [Aggiungere un proprietario del catalogo o un gestore di pacchetti di accesso a un catalogo](#add-a-catalog-owner-or-an-access-package-manager) |  | :heavy_check_mark: |  |  |
| [Creare un nuovo pacchetto di accesso in un catalogo](entitlement-management-access-package-create.md) |  | :heavy_check_mark:  | :heavy_check_mark:  |  |
| [Gestire i ruoli delle risorse in un pacchetto di accesso](entitlement-management-access-package-edit.md) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Creazione e modifica di criteri](entitlement-management-access-package-edit.md#add-a-new-policy) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Assegnare direttamente un utente a un pacchetto di accesso](entitlement-management-access-package-edit.md#directly-assign-a-user) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Visualizza chi ha un'assegnazione a un pacchetto di accesso](entitlement-management-access-package-edit.md#view-who-has-an-assignment) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Visualizzare le richieste di un pacchetto di accesso](entitlement-management-access-package-edit.md#view-requests) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Visualizzare gli errori di recapito di una richiesta](entitlement-management-access-package-edit.md#view-a-requests-delivery-errors) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Annulla una richiesta in sospeso](entitlement-management-access-package-edit.md#cancel-a-pending-request) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Nascondi un pacchetto di accesso](entitlement-management-access-package-edit.md#change-the-hidden-setting) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Eliminare un pacchetto di accesso](entitlement-management-access-package-edit.md#delete) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Approva una richiesta di accesso](entitlement-management-request-approve.md) |  |  |  | :heavy_check_mark: |

## <a name="required-roles-to-add-resources-to-a-catalog"></a>Ruoli necessari per aggiungere risorse a un catalogo

Un amministratore globale può aggiungere o rimuovere qualsiasi gruppo (gruppi di sicurezza creati dal cloud o gruppi di Office 365 creati dal cloud), un'applicazione o un sito di SharePoint online in un catalogo. Un amministratore utente può aggiungere o rimuovere qualsiasi gruppo o applicazione in un catalogo.

Per un utente che non è un amministratore globale o un amministratore di utenti, per aggiungere gruppi, applicazioni o siti di SharePoint Online a un catalogo, tale utente deve avere *sia* il ruolo della directory Azure ad necessario che il ruolo di gestione dei diritti del proprietario del catalogo. Nella tabella seguente sono elencate le combinazioni di ruoli necessarie per aggiungere risorse a un catalogo. Per rimuovere le risorse da un catalogo, è necessario avere gli stessi ruoli.

| Ruolo della directory di Azure AD | Ruolo di gestione dei diritti | È possibile aggiungere un gruppo di sicurezza | È possibile aggiungere il gruppo Office 365 | È possibile aggiungere l'app | È possibile aggiungere il sito di SharePoint Online |
| --- | :---: | :---: | :---: | :---: | :---: |
| [Amministratore globale](../users-groups-roles/directory-assign-admin-roles.md) | n/d |  :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Amministratore utenti](../users-groups-roles/directory-assign-admin-roles.md) | n/d |  :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Amministratore di Intune](../users-groups-roles/directory-assign-admin-roles.md) | Proprietario Catalogo | :heavy_check_mark: | :heavy_check_mark: |  |  |
| [Amministratore di Exchange](../users-groups-roles/directory-assign-admin-roles.md) | Proprietario Catalogo |  | :heavy_check_mark: |  |  |
| [Amministratore del servizio Teams](../users-groups-roles/directory-assign-admin-roles.md) | Proprietario Catalogo |  | :heavy_check_mark: |  |  |
| [Amministratore di SharePoint](../users-groups-roles/directory-assign-admin-roles.md) | Proprietario Catalogo |  | :heavy_check_mark: |  | :heavy_check_mark: |
| [Amministratore dell'applicazione](../users-groups-roles/directory-assign-admin-roles.md) | Proprietario Catalogo |  |  | :heavy_check_mark: |  |
| [Amministratore applicazione cloud](../users-groups-roles/directory-assign-admin-roles.md) | Proprietario Catalogo |  |  | :heavy_check_mark: |  |
| Utente | Proprietario Catalogo | Solo se il proprietario del gruppo | Solo se il proprietario del gruppo | Solo se il proprietario dell'app |  |

Per determinare il ruolo con privilegi minimi per un'attività, è anche possibile fare riferimento ai [ruoli di amministratore per attività di amministrazione in Azure Active Directory](../users-groups-roles/roles-delegate-by-task.md#entitlement-management).

## <a name="add-a-catalog-creator"></a>Aggiungere un creatore del catalogo

Se si desidera delegare la creazione del catalogo, aggiungere gli utenti al ruolo di autore del catalogo.  È possibile aggiungere singoli utenti o per praticità aggiungere un gruppo, i cui membri sono quindi in grado di creare cataloghi. Attenersi alla procedura seguente per assegnare un utente al ruolo di autore del catalogo.

**Ruolo prerequisito:** Amministratore globale o Amministratore utenti

1. Nel portale di Azure fare clic su **Azure Active Directory** e quindi su **Identity Governance**.

1. Nel menu a sinistra, nella sezione **gestione dei diritti** , fare clic su **Impostazioni**.

1. Fare clic su **Modifica**.

1. Nella sezione **delega diritti di gestione** , fare clic su **Aggiungi autori del catalogo** per selezionare gli utenti o i gruppi che saranno membri di questo ruolo di gestione dei diritti.

1. Fare clic su **Seleziona**.

1. Fare clic su **Save**.

## <a name="add-a-catalog-owner-or-an-access-package-manager"></a>Aggiungere un proprietario del catalogo o una gestione pacchetti di accesso

Per delegare la gestione di un catalogo o di accedere ai pacchetti nel catalogo, è necessario aggiungere gli utenti al proprietario del catalogo o accedere ai ruoli di gestione pacchetti. Chi crea un catalogo diventa il primo proprietario del catalogo. 

Il proprietario del catalogo assegnato o gestione pacchetti di accesso deve avere familiarità con il progetto. Il creatore del catalogo deve creare il pacchetto di accesso se è incluso nelle operazioni quotidiane del progetto e conosce le informazioni seguenti:
- quali risorse sono necessarie
- utenti che dovranno accedere
- utenti che devono approvare l'accesso
- per quanto tempo il progetto durerà

Il creatore del catalogo deve delegare l'attività al responsabile del progetto, che creerà e gestirà il pacchetto di accesso, se non è incluso nelle operazioni quotidiane del progetto. Attenersi alla procedura seguente per assegnare un utente al ruolo di proprietario del catalogo o accedere a gestione pacchetti:

**Ruolo prerequisito:** Amministratore globale, amministratore utente o proprietario del catalogo

1. Nel portale di Azure fare clic su **Azure Active Directory** e quindi su **Identity Governance**.

1. Nel menu a sinistra fare clic su **cataloghi** , quindi aprire il catalogo al quale si desidera aggiungere gli amministratori.

1. Nel menu a sinistra fare clic su **ruoli e amministratori**.

1. Fare clic su **Aggiungi proprietari** o **Aggiungi gestione pacchetti di accesso** per selezionare i membri per questi ruoli.

1. Fare clic su **Seleziona** per aggiungere questi membri.

## <a name="next-steps"></a>Passaggi successivi

- [Aggiungi responsabili approvazione](entitlement-management-access-package-edit.md#policy-request)
- [Aggiungere risorse a un catalogo](entitlement-management-catalog-create.md#add-resources-to-a-catalog)
