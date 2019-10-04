---
title: 'Servizio di sincronizzazione Azure AD Connect: estensioni della directory | Microsoft Docs'
description: Questo argomento illustra la funzionalità relativa alle estensioni della directory in Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 995ee876-4415-4bb0-a258-cca3cbb02193
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/05/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 88fdfce58bdd8e13637e77d01d4b6c0ab21f696a
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/29/2019
ms.locfileid: "68607646"
---
# <a name="azure-ad-connect-sync-directory-extensions"></a>Servizio di sincronizzazione Azure AD Connect: Estensioni della directory
È possibile usare le estensioni della directory per estendere lo schema in Azure Active Directory (Azure AD) con attributi personalizzati dall'istanza di Active Directory locale. Questa funzionalità consente di compilare app line-of-business che utilizzano attributi che continuano a essere gestiti in locale. Questi attributi possono essere utilizzati tramite le [estensioni della directory API Graph di Azure AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) o [Microsoft Graph](https://developer.microsoft.com/graph/). È possibile visualizzare gli attributi disponibili usando rispettivamente lo strumento [Graph Explorer di Azure AD](https://graphexplorer.azurewebsites.net/) e lo strumento [Microsoft Graph Explorer](https://developer.microsoft.com/graph/graph-explorer).

Attualmente nessun carico di lavoro di Office 365 utilizza questi attributi.

È possibile configurare gli attributi aggiuntivi da sincronizzare nel percorso delle impostazioni personalizzate nell'installazione guidata.

>[!NOTE]
>La casella Attributi disponibili fa distinzione tra maiuscole e minuscole.

![Procedura guidata per l'estensione dello schema](./media/how-to-connect-sync-feature-directory-extensions/extension2.png)  

L'installazione mostra gli attributi seguenti, che sono candidati validi:

* Tipi di oggetto utente e gruppo
* Attributi a valore singolo: String, Boolean, Integer, Binary
* Attributi multivalore: String, Binary


>[!NOTE]
> Anche se Azure AD Connect supporta la sincronizzazione di attributi Active Directory multivalore con Azure AD come estensioni di directory multivalore, attualmente non è possibile recuperare/usare i dati caricati negli attributi di estensioni di directory multivalore.

L'elenco di attributi viene letto dalla cache dello schema creata durante l'installazione di Azure AD Connect. Se è stato esteso lo schema di Active Directory con altri attributi, prima che questi nuovi attributi siano visibili è necessario [aggiornare lo schema](how-to-connect-installation-wizard.md#refresh-directory-schema).

Un oggetto in Azure AD può avere al massimo 100 attributi per le estensioni della directory. La lunghezza massima consentita è di 250 caratteri. Se il valore di un attributo è più lungo, viene troncato dal motore di sincronizzazione.

Durante l'installazione di Azure AD Connect viene registrata un'applicazione in cui sono disponibili questi attributi. È possibile visualizzare questa applicazione nel portale di Azure.

![App estensione dello schema](./media/how-to-connect-sync-feature-directory-extensions/extension3new.png)

Gli attributi sono preceduti dall'elemento \_{AppClientId}\_ dell'estensione. AppClientId ha lo stesso valore per tutti gli attributi nel tenant di Azure AD.

Questi attributi ora sono disponibili tramite l'API Graph di Azure AD. È possibile eseguire query su questi attributi usando lo strumento [Graph Explorer di Azure AD](https://graphexplorer.azurewebsites.net/).

![Graph Explorer di Azure AD](./media/how-to-connect-sync-feature-directory-extensions/extension4.png)

In alternativa è possibile eseguire una query sugli attributi tramite l'API Microsoft Graph, usando [Microsoft Graph Explorer](https://developer.microsoft.com/graph/graph-explorer#).

>[!NOTE]
> È necessario richiedere gli attributi da restituire. Selezionare in modo esplicito gli attributi seguenti:\:HTTPS//graph.microsoft.com/beta/users/abbie.spencer@fabrikamonline.com? $Select = extension_9d98ed114c4840d298fad781915f27e4_employeeID, extension_9d98ed114c4840d298fad781915f27e4_division. 
>
> Per altre informazioni, vedere [Microsoft Graph: Use query parameters](https://developer.microsoft.com/graph/docs/concepts/query_parameters#select-parameter) (Microsoft Graph: Usare parametri di query).

## <a name="next-steps"></a>Passaggi successivi
Ulteriori informazioni sulla configurazione della [sincronizzazione di Azure AD Connect](how-to-connect-sync-whatis.md).

Ulteriori informazioni su [Integrazione delle identità locali con Azure Active Directory](whatis-hybrid-identity.md).
