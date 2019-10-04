---
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 11/03/2016
ms.author: estfan
ms.openlocfilehash: 752c43604349a2361a8f5b26cd6d0bce7b516bc0
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/18/2019
ms.locfileid: "67180408"
---
### <a name="prerequisites"></a>Prerequisiti
* Un account [MailChimp](https://www.MailChimp.com/) 

Prima di poter usare l'account MailChimp in un'app per la logica, è necessario autorizzare l'app per la logica a connettersi all'account MailChimp. Fortunatamente, è possibile eseguire questa operazione in modo facile dalla propria app per la logica nel Portale di Azure. 

Ecco i passaggi per autorizzare l'app per la logica a connettersi all'account MailChimp:

1. Per creare una connessione a MailChimp, nella finestra di progettazione dell'app per la logica selezionare **Mostra API gestite da Microsoft** nell'elenco a discesa, quindi immettere *MailChimp* nella casella di ricerca. Selezionare il trigger o l'azione che si desidera usare:  
   ![Passaggio 1 di MailChimp](./media/connectors-create-api-mailchimp/mailchimp-1.png)
2. Se non sono mai state create connessioni a MailChimp prima d'ora, verrà chiesto di fornire le credenziali di MailChimp. Queste credenziali verranno usate per autorizzare l'app per la logica a connettersi e ad accedere ai dati dell'account MailChimp:  
   ![Passaggio 2 MailChimp](./media/connectors-create-api-mailchimp/mailchimp-2.png)
3. Fornire il nome utente e la password di MailChimp per autorizzare l'app per la logica:  
   ![Passaggio 3 MailChimp](./media/connectors-create-api-mailchimp/mailchimp-3.png)   
4. Si noti che la connessione è stata creata ed è ora possibile procedere con gli altri passaggi nell'app per la logica:  
   ![Passaggio 4 MailChimp](./media/connectors-create-api-mailchimp/mailchimp-4.png)

