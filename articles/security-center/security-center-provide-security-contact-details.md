---
title: Specificare i dettagli dei contatti di sicurezza nel Centro sicurezza di Azure | Documentazione Microsoft
description: Questo documento illustra come specificare i dettagli dei contatti di sicurezza nel Centro sicurezza di Azure.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 26b5dcb4-ce3f-4f22-8d56-d2bf743cfc90
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/09/2019
ms.author: memildin
ms.openlocfilehash: 1a66ea200082f60a3a763c6a4e2bdea62ec473d8
ms.sourcegitcommit: f34165bdfd27982bdae836d79b7290831a518f12
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2020
ms.locfileid: "75920991"
---
# <a name="provide-security-contact-details-in-azure-security-center"></a>Aggiunta dei dettagli dei contatti di sicurezza nel Centro sicurezza di Azure
Il Centro sicurezza di Azure consiglierà di specificare i dettagli dei contatti di sicurezza per la sottoscrizione di Azure, se non è già stato fatto. Queste informazioni verranno usate da Microsoft per contattare l'utente se il Microsoft Security Response Center (MSRC) rileva che un'entità illegale o non autorizzata ha effettuato l'accesso ai dati del cliente. Microsoft Security Response Center esegue il monitoraggio selettivo della sicurezza della rete e dell'infrastruttura di Azure e riceve informazioni sulle minacce e segnalazioni di violazioni da terzi.

Alla prima occorrenza giornaliera di un avviso e solo per gli avvisi di elevata gravità viene inviata una notifica tramite posta elettronica. Le preferenze di posta elettronica possono essere configurate solo per i criteri della sottoscrizione. I gruppi di risorse all'interno di una sottoscrizione ereditano queste impostazioni. Gli avvisi sono disponibili solo nel livello standard del Centro sicurezza di Azure.

Le notifiche di avviso tramite posta elettronica vengono inviate:
- Solo per avvisi con gravità alta
- A un singolo destinatario di posta elettronica per tipo di avviso al giorno  
- Non vengono inviati più di 3 messaggi di posta elettronica a un singolo destinatario in un solo giorno
- Ogni messaggio di posta elettronica contiene un unico avviso, non un'aggregazione di avvisi
 
Ad esempio, se è già stato inviato un messaggio di posta elettronica per avvisare l'utente di un attacco RDP, questo non riceverà nessun altro messaggio di posta elettronica relativo all'attacco RDP nello stesso giorno, anche se viene attivato un altro avviso. 

> [!NOTE]
> Il documento introduce il servizio usando una distribuzione di esempio.  Questa non è una guida dettagliata.

## Configurare le notifiche di posta elettronica per gli avvisi<a name="email"></a>

1. Dal portale selezionare **prezzi & impostazioni**.
1. Fare clic sulla sottoscrizione.
1. Fare clic su **Notifiche tramite posta elettronica**.

> [!NOTE]
> Se si sta implementando una raccomandazione, in **raccomandazioni**selezionare **specificare i dettagli dei contatti di sicurezza**e selezionare la sottoscrizione di Azure in cui inserire le informazioni di contatto. Si aprirà **Notifiche tramite posta elettronica**.

   ![Specificare dettagli del contatto per la sicurezza][2]

   * Immettere l'indirizzo o gli indirizzi di posta elettronica del contatto di sicurezza, separati da virgole. Non è previsto alcun limite per il numero di indirizzi di posta elettronica che è possibile immettere.
   * Immettere un numero di telefono internazionale per il contatto di sicurezza.
   * Per ricevere messaggi di posta elettronica relativi agli avvisi di elevata gravità, attivare l'opzione **Send me emails about alerts**(Invia messaggi di posta elettronica sugli avvisi).
   * È possibile inviare notifiche tramite posta elettronica ai proprietari della sottoscrizione (amministratore del servizio classico e coamministratori, oltre al ruolo di proprietario RBAC nell'ambito della sottoscrizione).
   * Selezionare **Salva** per applicare le informazioni di contatto di sicurezza alla sottoscrizione.

## <a name="see-also"></a>Vedi anche
Per altre informazioni sul Centro sicurezza, vedere gli argomenti seguenti:

* [Impostazione dei criteri di sicurezza nel Centro sicurezza di Azure](tutorial-security-policy.md) : informazioni su come configurare i criteri di sicurezza per le sottoscrizioni e i gruppi di risorse di Azure.
* [Gestione delle raccomandazioni di sicurezza nel Centro sicurezza di Azure](security-center-recommendations.md) : informazioni sul modo in cui le raccomandazioni semplificano la protezione delle risorse di Azure.
* [Monitoraggio dell'integrità della sicurezza nel Centro sicurezza di Azure](security-center-monitoring.md) : informazioni su come monitorare l'integrità delle risorse di Azure.
* [Gestione e risposta agli avvisi di sicurezza nel Centro sicurezza di Azure](security-center-managing-and-responding-alerts.md) : informazioni su come gestire e rispondere agli avvisi di sicurezza.
* [Monitoraggio delle soluzioni dei partner con il Centro sicurezza di Azure](security-center-partner-solutions.md) : informazioni su come monitorare lo stato integrità delle soluzioni dei partner.
* [Domande frequenti sul Centro sicurezza di Azure](security-center-faq.md) : domande frequenti sull'uso del servizio.
* [Blog sulla sicurezza di Azure](https://blogs.msdn.com/b/azuresecurity/) : informazioni e notizie aggiornate sulla sicurezza di Azure.

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png
