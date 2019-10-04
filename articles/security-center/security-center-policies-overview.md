---
title: Impostazioni del Centro sicurezza di Azure | Microsoft Docs
description: Configurare le impostazioni del Centro sicurezza di Azure.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: f24b1e4a-cc36-4542-b21e-041453cdfcd8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/03/2018
ms.author: memildin
ms.openlocfilehash: 4a7254d4ac67ee7d1bf203baf5741638dbc8f3dd
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71201619"
---
# <a name="security-center-settings"></a>Impostazioni di Centro sicurezza
Questo articolo offre una panoramica delle impostazioni del Centro sicurezza.

Le impostazioni seguenti sono disponibili in Criteri di sicurezza:

- **Raccolta dati**: determina il provisioning dell'agente e le impostazioni della [raccolta dati](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection).
- **Criteri di sicurezza**: determina i controlli monitorati e consigliati dal Centro sicurezza. È possibile modificare i [criteri di sicurezza](tutorial-security-policy.md) nel Centro sicurezza. È anche possibile usare [Criteri di Azure](tutorial-security-policy.md) per creare nuove definizioni, definire criteri aggiuntivi e assegnare criteri a livello di gruppi di gestione. 
- **Notifiche tramite posta elettronica**: determina i contatti di sicurezza e le impostazioni di [notifica tramite posta elettronica](security-center-provide-security-contact-details.md).
- **Piano tariffario**: definisce la selezione del livello Gratuito o Standard del [piano tariffario](security-center-pricing.md). Il piano scelto determina le funzionalità del Centro sicurezza disponibili per le risorse nell'ambito. È possibile specificare un livello per le sottoscrizioni e le aree di lavoro.

> [!NOTE]
> È possibile impostare tutti questi componenti per ogni sottoscrizione. Per le aree di lavoro, è possibile impostare solo Raccolta di dati e Piano tariffario.
>


## <a name="who-can-edit-security-policies"></a>Utenti che possono modificare i criteri di sicurezza
Il Centro sicurezza usa il controllo degli accessi in base al ruolo, che fornisce ruoli predefiniti assegnabili a utenti, gruppi e servizi in Azure. Quando un utente apre il Centro sicurezza, visualizza solo informazioni correlate alle risorse a cui ha accesso. Ciò significa che agli utenti viene assegnato il ruolo di *proprietario*, *collaboratore*o *lettore* alla sottoscrizione a cui appartiene la risorsa. Oltre a questi ruoli, esistono due ruoli specifici del Centro sicurezza:

- **Ruolo con autorizzazioni di lettura per la sicurezza**: ha i diritti di visualizzazione per il Centro sicurezza, inclusi avvisi, criteri, raccomandazioni e integrità, ma non può apportare modifiche.
- **Amministratore della sicurezza**: ha gli stessi diritti di visualizzazione del *Ruolo con autorizzazioni di lettura per la sicurezza*, ma può anche aggiornare i criteri di sicurezza e ignorare raccomandazioni e avvisi.


## <a name="next-steps"></a>Passaggi successivi
In questo articolo sono state fornite informazioni sui criteri di sicurezza nel Centro sicurezza di Azure. Per altre informazioni sul Centro sicurezza di Azure, vedere gli articoli seguenti:

* [Impostazione dei criteri di sicurezza nel Centro sicurezza di Azure](tutorial-security-policy.md): Informazioni su come configurare i criteri di sicurezza per le sottoscrizioni di Azure.
* [Gestione delle raccomandazioni di sicurezza nel Centro sicurezza di Azure](security-center-recommendations.md): informazioni sull'utilità delle raccomandazioni del Centro sicurezza per la protezione delle risorse di Azure.
* [Monitoraggio dell'integrità della sicurezza nel Centro sicurezza di Azure](security-center-monitoring.md): Informazioni su come monitorare l'integrità delle risorse di Azure.
* [Gestione e risposta agli avvisi di sicurezza nel Centro sicurezza di Azure](security-center-managing-and-responding-alerts.md): Informazioni su come gestire e rispondere agli avvisi di sicurezza.
* [Monitoraggio delle soluzioni dei partner con il Centro sicurezza di Azure](security-center-partner-solutions.md): Informazioni su come monitorare lo stato integrità delle soluzioni dei partner.
* [Sicurezza dei dati nel Centro sicurezza di Azure](security-center-data-security.md): informazioni su come il Centro sicurezza gestisce e protegge i dati.
* [Domande frequenti sul Centro sicurezza di Azure](security-center-faq.md): Risposte alle domande frequenti sull'uso del servizio.
* [Blog sulla sicurezza di Azure](https://blogs.msdn.com/b/azuresecurity/): informazioni e notizie aggiornate sulla sicurezza di Azure.
