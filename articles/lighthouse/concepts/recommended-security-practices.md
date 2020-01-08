---
title: Procedure di sicurezza consigliate
description: Quando si usa la gestione delle risorse delegata di Azure, è importante considerare sicurezza e controllo di accesso.
ms.date: 07/11/2019
ms.topic: conceptual
ms.openlocfilehash: 8972d6548eccb1006d90bfcbb4dba8c01b05a981
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75456875"
---
# <a name="recommended-security-practices"></a>Procedure di sicurezza consigliate

Quando si usa la gestione delle risorse delegata di Azure, è importante considerare sicurezza e controllo di accesso. Gli utenti del tenant avranno accesso diretto alle sottoscrizioni dei clienti e ai gruppi di risorse, quindi è opportuno eseguire le operazioni necessarie per gestire la sicurezza del tenant. Sarà anche necessario assicurarsi di consentire solo l'accesso necessario per gestire efficacemente le risorse dei clienti. Questo argomento fornisce indicazioni utili a tale scopo.

## <a name="require-azure-multi-factor-authentication"></a>Richiedere Azure Multi-Factor Authentication

[Azure multi-factor authentication](../../active-directory/authentication/concept-mfa-howitworks.md) (anche noto come verifica in due passaggi) aiuta a impedire agli utenti malintenzionati di accedere a un account richiedendo più passaggi di autenticazione. È consigliabile richiedere Multi-Factor Authentication per tutti gli utenti nel tenant del provider di servizi, inclusi gli utenti che avranno accesso alle risorse dei clienti.

Si consiglia di chiedere ai clienti di implementare Azure Multi-Factor Authentication anche nei propri tenant.

## <a name="assign-permissions-to-groups-using-the-principle-of-least-privilege"></a>Assegnare le autorizzazioni ai gruppi, usando il principio dei privilegi minimi

Per semplificare la gestione, è consigliabile usare gruppi di utenti di Azure AD per ogni ruolo necessario per gestire le risorse dei clienti. Ciò consente di aggiungere o rimuovere singoli utenti al gruppo in base alle esigenze, invece di assegnare le autorizzazioni direttamente a tale utente.

Quando si crea la struttura di autorizzazioni, assicurarsi di seguire il principio dei privilegi minimi, in modo che gli utenti abbiano solo le autorizzazioni necessarie per completare il proprio lavoro, riducendo la possibilità di errori accidentali.

È ad esempio possibile usare una struttura simile alla seguente:

|Nome gruppo  |Tipo  |principalId  |Definizione di ruolo  |ID di definizione del ruolo  |
|---------|---------|---------|---------|---------|
|Architetti     |Gruppo utenti         |\<principalId\>         |Collaboratore         |b24988ac-6180-42a0-ab88-20f7382dd24c  |
|Valutazione     |Gruppo utenti         |\<principalId\>         |Reader         |acdd72a7-3385-48ef-bd42-f606fba81ae7  |
|Specialisti di macchine virtuali     |Gruppo utenti         |\<principalId\>         |Collaboratore macchina virtuale         |9980e02c-c2be-4d73-94e8-173b1dc7cf3c  |
|Automazione     |Nome dell'entità servizio (SPN)         |\<principalId\>         |Collaboratore         |b24988ac-6180-42a0-ab88-20f7382dd24c  |

Dopo aver creato questi gruppi, è possibile assegnare gli utenti in base alle esigenze. Aggiungere solo gli utenti che devono avere effettivamente accesso. Controllare regolarmente l'appartenenza ai gruppi e rimuovere gli utenti che non è più appropriato o necessario includere.

Tenere presente che, quando si [esegue l'onboarding di clienti tramite un'offerta di servizio gestito pubblica](../how-to/publish-managed-services-offers.md), qualsiasi gruppo (o utente o entità servizio) incluso avrà le stesse autorizzazioni per ogni cliente che acquista il piano. Per assegnare gruppi diversi per lavorare con ogni cliente, è necessario pubblicare un piano privato separato che sia esclusivo per ogni cliente o eseguire l'onboarding dei singoli clienti usando modelli di Azure Resource Manager. È ad esempio possibile pubblicare un piano pubblico con accesso molto limitato, quindi collaborare direttamente con il cliente per eseguire l'onboarding delle risorse per l'accesso aggiuntivo usando un modello di risorsa di Azure personalizzato che concede l'accesso aggiuntivo in base alle esigenze.


## <a name="next-steps"></a>Passaggi successivi

- [Distribuire Azure Multi-Factor Authentication](../../active-directory/authentication/howto-mfa-getstarted.md).
- Informazioni sulle [esperienze di gestione tra tenant](cross-tenant-management-experience.md).
