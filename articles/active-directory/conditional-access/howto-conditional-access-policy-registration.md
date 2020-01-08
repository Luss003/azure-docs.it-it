---
title: Accesso condizionale-informazioni di sicurezza combinate-Azure Active Directory
description: Creare un criterio di accesso condizionale personalizzato per richiedere un percorso attendibile per la registrazione delle informazioni di sicurezza
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 12/12/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4c9b01cc06b3d0ef8f47b34e9ef86bec9adac03f
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75424859"
---
# <a name="conditional-access-require-trusted-location-for-mfa-registration"></a>Accesso condizionale: Richiedi un percorso attendibile per la registrazione a più fattori

È ora possibile proteggere quando e come gli utenti si registrano per Azure Multi-Factor Authentication e la reimpostazione della password self-service con le azioni dell'utente nei criteri di accesso condizionale. Questa funzionalità di anteprima è disponibile per le organizzazioni che hanno abilitato l' [anteprima della registrazione combinata](../authentication/concept-registration-mfa-sspr-combined.md). Questa funzionalità può essere abilitata nelle organizzazioni in cui si vuole usare condizioni come il percorso di rete attendibile per limitare l'accesso per la registrazione per Multi-Factor Authentication di Azure e SSPR. Per ulteriori informazioni sulla creazione di percorsi attendibili nell'accesso condizionale, vedere l'articolo [Qual è la condizione di posizione in Azure Active Directory l'accesso condizionale?](../conditional-access/location-condition.md#named-locations)

## <a name="create-a-policy-to-require-registration-from-a-trusted-location"></a>Creare un criterio per richiedere la registrazione da un percorso attendibile

I criteri seguenti si applicano a tutti gli utenti selezionati, che tentano di eseguire la registrazione usando l'esperienza di registrazione combinata e bloccano l'accesso a meno che non si connettano da un percorso contrassegnato come rete attendibile.

1. Nella **portale di Azure**individuare **Azure Active Directory** > **sicurezza** > **accesso condizionale**.
1. Selezionare **Nuovi criteri**.
1. In nome immettere un nome per il criterio. Ad esempio, la **registrazione delle informazioni di sicurezza combinata su reti attendibili**.
1. In **assegnazioni**fare clic su **utenti e gruppi**e selezionare gli utenti e i gruppi a cui si desidera applicare questo criterio.

   > [!WARNING]
   > Gli utenti devono essere abilitati per l' [anteprima della registrazione combinata](../authentication/howto-registration-mfa-sspr-combined.md).

1. In **azioni o app Cloud**selezionare **azioni utente**, selezionare **registra informazioni di sicurezza (anteprima)** .
1. In **condizioni** > **località**.
   1. Configurare **Sì**.
   1. Includere **qualsiasi percorso**.
   1. Escludere **tutti i percorsi attendibili**.
   1. Fare clic su **fine** nel pannello percorsi.
   1. Fare clic su **fine** nel pannello condizioni.
1. In **Access controls** > **Grant**.
   1. Fare clic su **Blocca accesso**.
   1. Quindi fare clic su **Seleziona**.
1. Impostare **Abilita criterio** su **Sì**.
1. Fare quindi clic su **Salva**.

## <a name="next-steps"></a>Passaggi successivi

[Criteri comuni di accesso condizionale](concept-conditional-access-policy-common.md)

[Determinare l'effetto usando la modalità solo report di accesso condizionale](howto-conditional-access-report-only.md)

[Simulare il comportamento di accesso usando lo strumento di What If dell'accesso condizionale](troubleshoot-conditional-access-what-if.md)
