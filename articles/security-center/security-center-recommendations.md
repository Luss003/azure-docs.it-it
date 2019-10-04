---
title: Raccomandazioni per la sicurezza nel centro sicurezza di Azure | Microsoft Docs
description: Questo documento dimostra come le raccomandazioni presenti nel Centro sicurezza di Azure facilitino la protezione delle risorse di Azure e garantiscano la conformità ai criteri di sicurezza.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/29/2019
ms.author: memildin
ms.openlocfilehash: 32b7f1d699c0d620d70614c441a8c18520c1b2d5
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71201051"
---
# <a name="security-recommendations-in-azure-security-center"></a>Raccomandazioni di sicurezza nel Centro sicurezza di Azure 
Questo argomento illustra come visualizzare e comprendere le raccomandazioni nel centro sicurezza di Azure per proteggere le risorse di Azure.

> [!NOTE]
> Il documento introduce il servizio usando una distribuzione di esempio.  Questo argomento non costituisce una guida dettagliata.
>

## <a name="what-are-security-recommendations"></a>Informazioni sulle raccomandazioni di sicurezza

Le raccomandazioni sono azioni da eseguire per proteggere le risorse.

Centro sicurezza che analizza periodicamente lo stato di sicurezza delle risorse di Azure per identificare le potenziali vulnerabilità di sicurezza. Vengono quindi fornite indicazioni su come rimuoverle.

Ogni raccomandazione fornisce:

- Descrizione breve degli elementi consigliati.
- Procedura di correzione da eseguire per implementare la raccomandazione. <!-- In some cases, one-click remediation is available. -->
- Quali risorse sono necessarie per eseguire l'azione consigliata su di essi.
- L' **effetto del Punteggio sicuro**, ovvero la quantità di tempo per cui il Punteggio sicuro aumenterà se si implementa questa raccomandazione.

## Monitorare le raccomandazioni<a name="monitor-recommendations"></a>

Il Centro sicurezza analizza lo stato di sicurezza delle risorse per identificare le potenziali vulnerabilità. Il riquadro **raccomandazioni** in **Panoramica** Mostra il numero totale di raccomandazioni identificate dal centro sicurezza.

![Panoramica del Centro sicurezza](./media/security-center-recommendations/asc-overview.png)

1. Selezionare il **riquadro raccomandazioni** in **Panoramica**. Verrà visualizzato l'elenco **raccomandazioni** .

      ![Visualizza i consigli](./media/security-center-recommendations/view-recommendations.png)

    È possibile filtrare le raccomandazioni. Per filtrare le raccomandazioni, selezionare **Filtro** nel pannello **Raccomandazioni**. Viene visualizzato il pannello **Filtro** in cui è possibile selezionare i valori relativi a gravità e stato da visualizzare.

   * **SUGGERIMENTI**: Indicazione.
   * **EFFETTO DEL PUNTEGGIO SICURO**: Punteggio generato dal centro sicurezza usando le raccomandazioni sulla sicurezza e applicazione di algoritmi avanzati per determinare il livello di importanza di ogni raccomandazione. Per altre informazioni, vedere [calcolo del Punteggio sicuro](security-center-secure-score.md#secure-score-calculation).
   * **RISORSA**: elenca le risorse a cui si applica la raccomandazione.
   * **BARRE DI STATO**:  descrive il livello di gravità della raccomandazione:
       * **Alto (rosso)** : è presente una vulnerabilità associata a una risorsa significativa, ad esempio un'applicazione, una macchina virtuale o un gruppo di sicurezza di rete, che richiede attenzione.
       * **Media (arancione)** : è presente una vulnerabilità e sono necessari passaggi aggiuntivi, o non critici, per eliminarla o completare un processo.
       * **Bassa (blu)** : è presente una vulnerabilità che deve essere risolta ma non richiede attenzione immediata. Per impostazione predefinita, le raccomandazioni con gravità bassa non appaiono, ma è possibile visualizzarle applicando il filtro corrispondente. 
       * **Integro (verde)** :
       * **Non disponibile (grigio)** :

1. Per visualizzare i dettagli di ogni raccomandazione, fare clic sull'indicazione.

    ![Dettagli raccomandazione](./media/security-center-recommendations/recommendation-details.png)

>[!NOTE] 
> Vedere [modelli di distribuzione classica e gestione risorse](../azure-classic-rm.md) per le risorse di Azure.
 
## <a name="next-steps"></a>Passaggi successivi

Questo documento ha introdotto le raccomandazioni relative alla sicurezza nel Centro sicurezza. Per informazioni su come correggere le raccomandazioni:

* [Correggere le raccomandazioni](security-center-remediate-recommendations.md) : informazioni su come configurare i criteri di sicurezza per le sottoscrizioni e i gruppi di risorse di Azure.
