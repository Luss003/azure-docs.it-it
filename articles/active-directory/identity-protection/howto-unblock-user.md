---
title: Come sbloccare gli utenti con Azure Active Directory Identity Protection | Microsoft Docs
description: Informazioni su come sbloccare gli utenti bloccati da un criterio di Azure Active Directory Identity Protection.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4e3756435703c4e8c887a4e7b9d4f75a6701d840
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2019
ms.locfileid: "70126259"
---
# <a name="how-to-unblock-users"></a>Procedura: Sbloccare gli utenti

Azure Active Directory Identity Protection consente di configurare criteri per bloccare gli utenti se vengono rispettate le condizioni configurate. Un utente bloccato contatta in genere l'help desk per essere sbloccato. Questo articolo illustra la procedura da eseguire per sbloccare un utente bloccato.

## <a name="determine-the-reason-for-blocking"></a>Determinare il motivo del blocco

Il primo passaggio per sbloccare un utente consiste nel determinare il tipo di criterio che ha bloccato l'utente, perché i passaggi successivi dipendono da questa informazione.
Azure Active Directory Identity Protection consente di bloccare un utente mediante un criterio di rischio di accesso o un criterio di rischio.

È possibile ottenere il tipo di criterio che ha bloccato un utente basandosi sull'intestazione della finestra di dialogo visualizzata all'utente durante il tentativo di accesso:

| Criteri | Finestra di dialogo utente |
| --- | --- |
| Rischio di accesso |![Accesso bloccato](./media/howto-unblock-user/02.png) |
| Rischio utente |![Account bloccato](./media/howto-unblock-user/104.png) |

Un utente bloccato da:

* Un criterio di rischio di accesso è noto anche come accesso sospetto
* Un criterio di rischio utente è noto anche come account a rischio

## <a name="unblocking-suspicious-sign-ins"></a>Sblocco di accessi sospetti

Per sbloccare un accesso sospetto, sono disponibili le opzioni seguenti:

1. **Accesso da una località o un dispositivo familiare**: un motivo comune del blocco di accessi sospetti è costituito da tentativi di accesso da località o dispositivi non familiari. Gli utenti possono determinare rapidamente se questo è il motivo del blocco provando ad accedere da una località o un dispositivo familiare.
2. **Esclusione dal criterio** : se si ritiene che la configurazione corrente del criterio di accesso provochi problemi agli utenti, è possibile escludere gli utenti dal criterio. Per altre informazioni, vedere [Azure Active Directory Identity Protection](../active-directory-identityprotection.md).
3. **Disabilitazione del criterio** : se si ritiene che la configurazione del criterio provochi problemi per tutti gli utenti, è possibile disabilitare il criterio. Per altre informazioni, vedere [Azure Active Directory Identity Protection](../active-directory-identityprotection.md).

## <a name="unblocking-accounts-at-risk"></a>Sblocco di account a rischio

Per sbloccare un account a rischio, sono disponibili le opzioni seguenti:

1. **Reimpostazione della password** : è possibile reimpostare la password dell'utente. 
2. **Ignora tutti i** rilevamenti dei rischi: il criterio di rischio utente blocca un utente se è stato raggiunto il livello di rischio utente configurato per il blocco dell'accesso. È possibile ridurre il livello di rischio di un utente chiudendo manualmente i rilevamenti dei rischi segnalati. 
3. **Esclusione dal criterio** : se si ritiene che la configurazione corrente del criterio di accesso provochi problemi agli utenti, è possibile escludere gli utenti dal criterio. Per altre informazioni, vedere [Azure Active Directory Identity Protection](../active-directory-identityprotection.md).
4. **Disabilitazione del criterio** : se si ritiene che la configurazione del criterio provochi problemi per tutti gli utenti, è possibile disabilitare il criterio. Per altre informazioni, vedere [Azure Active Directory Identity Protection](../active-directory-identityprotection.md).

## <a name="next-steps"></a>Passaggi successivi
 
Per altre informazioni su Azure AD Identity Protection, vedere [Azure Active Directory Identity Protection](../active-directory-identityprotection.md).
