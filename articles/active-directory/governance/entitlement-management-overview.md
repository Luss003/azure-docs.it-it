---
title: Cos'è Gestione entitlement di Azure AD? (Anteprima)-Azure Active Directory
description: Panoramica della gestione dei diritti Azure Active Directory e del modo in cui è possibile utilizzarla per gestire l'accesso a gruppi, applicazioni e siti di SharePoint Online per utenti interni ed esterni.
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
ms.date: 09/03/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 136a9994415b42c456ebdb0caa8ed6edcc7b4534
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/25/2019
ms.locfileid: "72934383"
---
# <a name="what-is-azure-ad-entitlement-management-preview"></a>Cos'è Gestione entitlement di Azure AD? (Anteprima)

> [!IMPORTANT]
> Gestione entitlement di Azure Active Directory (Azure AD) è attualmente in anteprima pubblica.
> Questa versione di anteprima viene messa a disposizione senza contratto di servizio e non è consigliata per i carichi di lavoro di produzione. Alcune funzionalità potrebbero non essere supportate o potrebbero presentare funzionalità limitate.
> Per altre informazioni, vedere [Condizioni supplementari per l'utilizzo delle anteprime di Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Per svolgere il proprio lavoro, i dipendenti delle organizzazioni devono accedere a diversi gruppi, applicazioni e siti. Gestire questo accesso è complesso. Nella maggior parte dei casi, non è disponibile un elenco organizzato di tutte le risorse necessarie per un progetto. Il project manager dispone di una conoscenza approfondita delle risorse necessarie, dei singoli utenti interessati e della durata del progetto. Tuttavia, il project manager in genere non dispone delle autorizzazioni per approvare o concedere l'accesso ad altri utenti. Questo scenario diventa più complicato quando si tenta di lavorare con le società o i singoli utenti esterni.

Azure Active Directory (Azure AD) la gestione dei diritti consente di gestire l'accesso a gruppi, applicazioni e siti di SharePoint Online per gli utenti interni e anche per gli utenti esterni all'organizzazione.

In questo video viene fornita una panoramica della gestione dei diritti e del relativo valore aziendale:

>[!VIDEO https://www.youtube.com/embed/_Lss6bFrnQ8]

## <a name="why-use-entitlement-management"></a>Perché usare la gestione dei diritti?

Le organizzazioni aziendali spesso affrontano problemi durante la gestione dell'accesso alle risorse, ad esempio:

- È possibile che gli utenti non conoscano l'accesso
- È possibile che gli utenti abbiano difficoltà nell'individuare gli individui giusti o le risorse giuste
- Una volta che gli utenti trovano e ricevono l'accesso a una risorsa, possono mantenere l'accesso più a lungo di quanto richiesto per scopi aziendali

Questi problemi sono composti per gli utenti che necessitano dell'accesso da un'altra directory, ad esempio gli utenti esterni che appartengono a organizzazioni di supply chain o altri partner commerciali. ad esempio:

- È possibile che le organizzazioni non conoscano tutti gli utenti specifici di altre directory per poterli invitare
- Anche se le organizzazioni erano in grado di invitare questi utenti, le organizzazioni potrebbero non ricordare di gestire tutti gli accessi degli utenti in modo coerente

Azure AD la gestione dei diritti può aiutare a risolvere questi problemi.

## <a name="what-can-i-do-with-entitlement-management"></a>Cosa posso fare con la gestione dei diritti?

Di seguito sono riportate alcune delle funzionalità di gestione dei diritti:

- Creare pacchetti di risorse correlate che gli utenti possono richiedere
- Definire le regole per la richiesta di risorse e la scadenza dell'accesso
- Governare il ciclo di vita dell'accesso per gli utenti interni ed esterni
- Delegare la gestione delle risorse
- Designare i responsabili approvazione per approvare le richieste
- Creare report per tenere traccia della cronologia

Per una panoramica della governance delle identità e della gestione dei diritti, guardare il video seguente dalla conferenza Ignite 2018:

>[!VIDEO https://www.youtube.com/embed/aY7A0Br8u5M]

## <a name="what-resources-can-i-manage"></a>Quali risorse è possibile gestire?

Di seguito sono riportati i tipi di risorse a cui è possibile gestire l'accesso con la gestione dei diritti:

- Gruppi di sicurezza Azure AD
- Gruppi di Office 365
- Azure AD applicazioni aziendali, tra cui applicazioni SaaS e applicazioni personalizzate che supportano la Federazione o il provisioning
- Siti e raccolte di siti di SharePoint Online

È anche possibile controllare l'accesso ad altre risorse che si basano su Azure AD gruppi di sicurezza o gruppi di Office 365.  ad esempio:

- È possibile concedere agli utenti le licenze per Microsoft Office 365 usando un gruppo di sicurezza Azure AD in un pacchetto di accesso e configurando le [licenze basate sui gruppi](../users-groups-roles/licensing-groups-assign.md) per quel gruppo
- È possibile concedere agli utenti l'accesso per gestire le risorse di Azure usando un gruppo di sicurezza Azure AD in un pacchetto di accesso e creando un' [assegnazione di ruolo di Azure](../../role-based-access-control/role-assignments-portal.md) per quel gruppo

## <a name="what-are-access-packages-and-policies"></a>Che cosa sono i pacchetti e i criteri di accesso?

La gestione dei diritti introduce il concetto di *pacchetto di accesso*. Un pacchetto di accesso è costituito da un bundle di tutte le risorse necessarie a un utente per lavorare a un progetto o per eseguire il proprio lavoro. Le risorse includono l'accesso a gruppi, applicazioni o siti. I pacchetti Access vengono usati per gestire l'accesso per i dipendenti interni e anche per gli utenti esterni all'organizzazione. I pacchetti di accesso sono definiti in contenitori denominati *cataloghi*.

I pacchetti Access includono inoltre uno o più *criteri*. Un criterio definisce le regole o Guardrails per accedere a un pacchetto di accesso. L'abilitazione di un criterio impone che solo agli utenti giusti venga concesso l'accesso, alle risorse corrette e per il periodo di tempo corretto.

![Accedere a pacchetti e criteri](./media/entitlement-management-overview/elm-overview-access-package.png)

Con un pacchetto di accesso e i relativi criteri, gestione pacchetti di Access definisce:

- resources
- Ruoli necessari agli utenti per le risorse
- Utenti interni e organizzazioni partner di utenti esterni idonei per richiedere l'accesso
- Processo di approvazione e utenti che possono approvare o negare l'accesso
- Durata dell'accesso dell'utente

Il diagramma seguente mostra un esempio dei diversi elementi della gestione dei diritti. Mostra due pacchetti di accesso di esempio.

- Il **pacchetto di accesso 1** include un singolo gruppo come risorsa. L'accesso viene definito con un criterio che consente a un set di utenti nella directory di richiedere l'accesso.
- Il **pacchetto di accesso 2** include un gruppo, un'applicazione e un sito di SharePoint Online come risorse. L'accesso viene definito con due criteri diversi. Il primo criterio consente a un set di utenti nella directory di richiedere l'accesso. Il secondo criterio consente agli utenti di una directory esterna di richiedere l'accesso.

![Panoramica sulla gestione dei diritti](./media/entitlement-management-overview/elm-overview.png)

## <a name="terminology"></a>Terminologia

Per comprendere meglio la gestione dei diritti e la relativa documentazione, è necessario esaminare i termini seguenti.

| Termine o concetto | Description |
| --- | --- |
| Gestione dei diritti | Servizio che assegna, revoca e amministra i pacchetti di accesso. |
| pacchetto di accesso | Un bundle di risorse necessarie a un team o a un progetto e viene regolato con i criteri. Un pacchetto di accesso è sempre contenuto in un catalogo. |
| richiesta di accesso | Richiesta di accesso alle risorse in un pacchetto di accesso. Una richiesta viene in genere attraversata da un flusso di lavoro. |
| policy | Set di regole che definisce il ciclo di vita dell'accesso, ad esempio il modo in cui gli utenti ottengono l'accesso, gli utenti che possono approvare e il tempo di accesso degli utenti. I criteri di esempio includono l'accesso dei dipendenti e l'accesso esterno. |
| catalog | Contenitore di risorse correlate e pacchetti di accesso. |
| Catalogo generale | Catalogo incorporato che è sempre disponibile. Per aggiungere risorse al catalogo generale, è necessario disporre di determinate autorizzazioni. |
| resource | Un asset o un servizio, ad esempio un gruppo di Office, un gruppo di sicurezza, un'applicazione o un sito di SharePoint Online, a cui un utente può concedere le autorizzazioni. |
| Tipo di risorsa | Tipo di risorsa, che include i gruppi, le applicazioni e i siti di SharePoint Online. |
| ruolo risorsa | Raccolta di autorizzazioni associate a una risorsa. |
| Directory delle risorse | Una directory che dispone di una o più risorse da condividere. |
| organizzazione connessa | Una directory o un dominio di Azure AD esterno con una relazione. |
| utenti assegnati | Assegnazione di un pacchetto di accesso a un utente, in modo che l'utente disponga di tutti i ruoli delle risorse del pacchetto di accesso. |
| enable | Il processo di creazione di un pacchetto di accesso disponibile per gli utenti da richiedere. |

## <a name="license-requirements"></a>Requisiti relativi alle licenze

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

I cloud specializzati, ad esempio Azure per enti pubblici, Azure Germania e Azure Cina 21Vianet, non sono attualmente disponibili per l'uso in questa versione di anteprima.

### <a name="which-users-must-have-licenses"></a>Quali utenti devono avere le licenze?

Il tenant deve avere almeno un numero di licenze Azure AD Premium P2 uguale a quello degli utenti membri attivi. Gli utenti membri attivi nella gestione dei diritti includono:

- Utente che avvia o approva una richiesta di un pacchetto di accesso.
- Utente a cui è stato assegnato un pacchetto di accesso. 
- Utente che gestisce i pacchetti di accesso.

Come parte delle licenze per gli utenti membri, è anche possibile consentire a diversi utenti guest di interagire con la gestione dei diritti. Per informazioni su come calcolare il numero di utenti guest che è possibile includere, vedere [Azure Active Directory linee guida sulle licenze di collaborazione B2B](../b2b/licensing-guidance.md).

Per informazioni su come assegnare le licenze agli utenti, vedere [assegnare o rimuovere licenze usando il portale di Azure Active Directory](../fundamentals/license-users-groups.md).

## <a name="next-steps"></a>Passaggi successivi

- [Esercitazione: creare il primo pacchetto di accesso](entitlement-management-access-package-first.md)
- [Scenari comuni](entitlement-management-scenarios.md)
