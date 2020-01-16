---
title: Pagare le sottoscrizioni di Azure tramite fattura
description: Questo articolo descrive come pagare le sottoscrizioni di Azure tramite fattura.
author: bandersmsft
manager: jureid
tags: billing
ms.service: cost-management-billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/23/2019
ms.author: banders
ms.openlocfilehash: a0f012145788d2d1d4935e10691859e5aaf71255
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/15/2020
ms.locfileid: "75994337"
---
# <a name="pay-for-your-azure-subscription-by-invoice"></a>Pagare la sottoscrizione di Azure tramite fattura

Con il metodo di pagamento tramite fattura la fattura viene pagata entro 30 giorni dalla data di emissione, con assegno o bonifico bancario. Per diventare idonei al pagamento della sottoscrizione di Azure tramite fattura, inviare una richiesta al supporto tecnico di Azure. Dopo che la richiesta è stata approvata, è possibile passare al pagamento con fattura (assegno/bonifico bancario) nel [portale di Azure](https://portal.azure.com).

> [!IMPORTANT]
> * Il pagamento con fattura (assegno/bonifico bancario) è disponibile solo per gli account aziendali.
> * Prima di passare al pagamento tramite fattura, è necessario corrispondere tutti gli addebiti insoluti.
> * Attualmente, il pagamento con fattura non è supportato per Azure globale in Cina.

## <a name="request-to-pay-by-invoice"></a>Richiedere il passaggio al pagamento con fattura

1. Passare alla [portale di Azure](https://portal.azure.com) per inviare una richiesta di supporto. Cercare e selezionare **Guida e supporto**.

    ![Cerca assistenza e supporto tecnico, portale di Microsoft Azure](./media/pay-by-invoice/search-for-help-and-support.png)

2. Selezionare **Nuova richiesta di supporto**.

    ![Nuovo collegamento della richiesta di supporto, schermata di guida e supporto tecnico, portale di Microsoft Azure](./media/pay-by-invoice/help-and-support.png)

2. Selezionare **Fatturazione** per **Tipo di problema**. *Tipo di problema* è la categoria della richiesta di supporto. Selezionare la sottoscrizione che si vuole pagare tramite fattura, selezionare un piano di supporto e quindi scegliere **Avanti**.

3. Selezionare **Pagamento** per **Tipo di problema**. *Tipo di problema* è la categoria della richiesta di supporto.

4. Selezionare l' **opzione per il pagamento tramite fattura** come **sottotipo di problema**.

5. Immettere le informazioni seguenti nella casella **Dettagli** e quindi selezionare **Avanti**.

         New or existing customer:
         If existing, current payment method:
         Order ID (requesting for invoice option):
         Account Admins Live ID (or Org ID) (should be company domain):
         Commerce Account ID:
         Company Name (as registered under VAT or Government Website):
         Company Address (as registered under VAT or Government Website):
         Company Website:
         Country:
         TAX ID/ VAT ID:
         Company Established on (Year):
         Any prior business with Microsoft:
         Contact Name:
         Contact Phone:
         Contact Email:
         Justification on why you prefer Invoice option over credit card:

         For cores increase, provide the following additional information:

         (Old quota) Existing Cores:
         (New quota) Requested cores:
         Specific region & series of Subscription:

    - Le voci in **Nome società** e **Indirizzo società** devono corrispondere alle informazioni fornite per l'account Azure. Per visualizzare o aggiornare le informazioni, vedere [Modificare le informazioni del profilo dell'account Azure](change-azure-account-profile.md).
    - Aggiungere le informazioni di contatto per la fatturazione nel portale di Azure per consentire l'approvazione del limite di credito. Le informazioni di contatto devono essere correlate al reparto di contabilità fornitori o finanziario dell'azienda. Per aggiornare le informazioni di contatto per la fatturazione, passare al [Centro account di Azure](https://account.azure.com/Profile).

6. Verificare le informazioni di contatto e il metodo di contatto preferito, quindi selezionare **Crea**.

Se occorre eseguire una verifica del credito per la quantità di credito necessario, l'utente riceverà una richiesta di verifica del credito.

## <a name="switch-to-invoice-pay-checkwire-transfer"></a>Passare al pagamento tramite fattura (assegno/bonifico)

Dopo che la richiesta di pagamento tramite fattura è stata approvata, è possibile passare al metodo di pagamento con fattura (assegno/bonifico bancario) nel portale di Azure.

Se si dispone di un account del programma Microsoft Online Services, è possibile passare a una sottoscrizione di Azure con pagamento tramite assegno/bonifico. Con un contratto del cliente Microsoft è possibile impostare il pagamento con assegno/bonifico nel profilo di fatturazione. Leggere le informazioni su [come controllare il tipo di account](#check-access-to-a-microsoft-customer-agreement).

### <a name="switch-azure-subscription-to-checkwire-transfer"></a>Impostare il pagamento con assegno/bonifico per la sottoscrizione di Azure

Seguire questa procedura per impostare il pagamento tramite fattura (assegno/bonifico) per la sottoscrizione di Azure. *Dopo il passaggio al pagamento con fattura (assegno/bonifico), non è possibile tornare al pagamento con carta di credito.*

1. Passare alla [portale di Azure](https://portal.azure.com) per accedere come amministratore account. Cercare e selezionare **Gestione costi e fatturazione**.

    ![Cerca gestione costi e fatturazione, portale di Microsoft Azure](./media/pay-by-invoice/search.png)

1. Selezionare la sottoscrizione per cui si vuole passare al pagamento tramite fattura.
1. Selezionare **Metodi di pagamento**.
1. Nella barra dei comandi selezionare il pulsante **paga per fattura** .

    ![Pulsante con pagamento in fattura, metodi di pagamento, portale di Microsoft Azure](./media/pay-by-invoice/pay-by-invoice.png)

### <a name="switch-billing-profile-to-checkwire-transfer"></a>Impostare il pagamento con assegno/bonifico nel profilo di fatturazione

Seguire questa procedura per impostare il pagamento con assegno/bonifico nel profilo di fatturazione. Il metodo di pagamento predefinito per un profilo di fatturazione può essere cambiato solo dalla persona che ha effettuato l'iscrizione ad Azure.

1. Passare alla [portale di Azure](https://portal.azure.com) visualizzare le informazioni di fatturazione. Cercare e selezionare **Gestione costi e fatturazione**.
1. Nel menu scegliere profili di **fatturazione**.

    ![Voce di menu profili di fatturazione, gestione costi e fatturazione, portale di Microsoft Azure](./media/pay-by-invoice/billing-profile.png)

1. Selezionare un profilo di fatturazione.
1. Nel menu **profilo di fatturazione** selezionare **metodi di pagamento**.

   ![Voce di menu metodi di pagamento, profili di fatturazione, gestione costi, portale di Microsoft Azure](./media/pay-by-invoice/billing-profile-payment-methods.png)

1. Selezionare il banner che indica che l'utente è idoneo per il pagamento tramite check/Wire Transfer.

    ![Banner per passare a check/Wire, metodi di pagamento, portale di Microsoft Azure](./media/pay-by-invoice/customer-led-switch-to-invoice.png)

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Verificare l'accesso a un Contratto del cliente Microsoft
[!INCLUDE [billing-check-mca](../../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Opzioni per Contattaci.

In caso di domande o per assistenza, [creare una richiesta di supporto](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Passaggi successivi

- Se necessario, aggiornare le informazioni di contatto per la fatturazione nel [Centro account di Azure](https://account.azure.com/Profile).
