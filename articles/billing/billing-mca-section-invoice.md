---
title: Creare sezioni della fattura per organizzare i costi - Azure
description: Informazioni su come organizzare i costi con le sezioni della fattura.
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.workload: na
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: ff8b2da353d623cd9f05c8d0b0317587d7093ce3
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75389282"
---
# <a name="create-sections-on-your-invoice-to-organize-your-costs"></a>Creare sezioni sulla fattura per organizzare i costi

È possibile creare sezioni della fattura per organizzare i costi in base a un reparto, un ambiente di sviluppo o in base alle esigenze dell'organizzazione. È quindi possibile concedere ad altri utenti l'autorizzazione a creare sottoscrizioni di Azure fatturate nella sezione. Eventuali addebiti per l'utilizzo e acquisti per le sottoscrizioni vengono quindi fatturati nella sezione. È possibile visualizzare gli addebiti totali per la sezione sulla fattura, nel portale di Azure o esaminarli nell'analisi dei costi di Azure. Per altre informazioni, vedere [Visualizzare le transazioni in base alle sezioni della fattura](billing-mca-understand-your-bill.md#view-transactions-by-invoice-sections).

Questo articolo di applica a un account di fatturazione per un Contratto del cliente Microsoft. [Verificare di avere accesso a un Contratto del cliente Microsoft](#check-access-to-a-microsoft-customer-agreement).

## <a name="create-an-invoice-section-in-the-azure-portal"></a>Creare una sezione della fattura nel portale di Azure

Per creare una sezione della fattura, è necessario essere un **proprietario del profilo di fatturazione** o un **collaboratore per il profilo di fatturazione**. Per altre informazioni, vedere [Gestire le sezioni della fattura per il profilo di fatturazione](billing-understand-mca-roles.md#manage-invoice-sections-for-billing-profile).

1. Accedere al [portale di Azure](https://portal.azure.com).

2. Cercare **Gestione dei costi e fatturazione**.

   ![Screenshot che mostra una ricerca nel portale di Azure](./media/billing-mca-section-invoice/billing-search-cost-management-billing.png)

3. Selezionare **Sezioni della fattura** dal riquadro a sinistra. A seconda dell'accesso può essere necessario selezionare un account di fatturazione o un profilo di fatturazione, quindi selezionare **Sezioni della fattura**.

   ![Screenshot che mostra l'elenco delle sezioni della fattura](./media/billing-mca-section-invoice/mca-select-invoice-sections.png)

4. Nella parte superiore della pagina selezionare **Aggiungi**.

5. Immettere un nome per la sezione della fattura e selezionare un profilo di fatturazione. Questa sezione verrà visualizzata nella fattura di questo profilo di fatturazione e rispecchierà l'utilizzo di ogni sottoscrizione e gli acquisti assegnati alla sezione.

   ![Screenshot che mostra la pagina di creazione della sezione della fattura](./media/billing-mca-section-invoice/mca-create-invoice-section.png)

6. Selezionare **Create** (Crea).

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Verificare l'accesso a un Contratto del cliente Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Richiesta di assistenza Contattare il supporto tecnico

Se si necessita assistenza, [contattare il supporto tecnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) per ottenere una rapida risoluzione del problema.

## <a name="next-steps"></a>Passaggi successivi

- [Creare un'altra sottoscrizione di Azure per il Contratto del cliente Microsoft](billing-mca-create-subscription.md)
- [Gestire i ruoli di fatturazione nel portale di Azure](billing-understand-mca-roles.md#manage-billing-roles-in-the-azure-portal)
- [Ottenere la proprietà della fatturazione delle sottoscrizioni di Azure dagli utenti in altri account di fatturazione](billing-mca-request-billing-ownership.md)
