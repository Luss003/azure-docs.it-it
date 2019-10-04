---
title: Connettere i dati di Office 365 ad Azure Sentinel | Microsoft Docs
description: Informazioni su come connettere i dati di Office 365 ad Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: 481acfdccb54077f18c1f4bf0375c0dc66e66007
ms.sourcegitcommit: 992e070a9f10bf43333c66a608428fcf9bddc130
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2019
ms.locfileid: "71240658"
---
# <a name="connect-data-from-office-365-logs"></a>Connettere i dati dai log di Office 365



È possibile trasmettere i log di controllo da [Office 365](https://docs.microsoft.com/office365/admin/admin-home?view=o365-worldwide) ad Azure Sentinel con un solo clic. È possibile trasmettere i log di controllo da più tenant a una singola area di lavoro in Sentinel di Azure. Il connettore log attività di Office 365 fornisce informazioni approfondite sulle attività dell'utente in corso. Si otterranno informazioni su varie azioni utente, amministratore, sistema e criteri ed eventi di Office 365. Connettendo i log di Office 365 ad Azure Sentinel è possibile usare questi dati per visualizzare i dashboard, creare avvisi personalizzati e migliorare il processo di analisi.

> [!IMPORTANT]
> Se si dispone di una licenza E3, prima di poter accedere ai dati tramite l'API di gestione delle attività di Office 365, è necessario abilitare la registrazione di controllo unificata per l'organizzazione di Office 365. A tale scopo, accendere il registro di controllo di Office 365. Per istruzioni, vedere [attivare o disattivare la ricerca dei log di controllo di Office 365](https://docs.microsoft.com/office365/securitycompliance/turn-audit-log-search-on-or-off). Per ulteriori informazioni, vedere [riferimento all'API di gestione delle attività di Office 365](https://docs.microsoft.com/office/office-365-management-api/office-365-management-activity-api-reference).

## <a name="prerequisites"></a>Prerequisiti

- È necessario essere un amministratore globale o un amministratore della sicurezza nel tenant
- Nel computer da cui è stato eseguito l'accesso ad Azure Sentinel per creare la connessione, assicurarsi che la porta 4433 sia aperta per il traffico Web. Questa porta può essere chiusa nuovamente dopo che la connessione è stata eseguita correttamente.
- Se il tenant non ha una licenza Office 365 E3 o Office 365 E5, è necessario abilitare il controllo unificato nel tenant usando uno di questi processi:
    - [Usare il cmdlet Set-AdminAuditLogConfig](https://docs.microsoft.com/powershell/module/exchange/policy-and-compliance-audit/set-adminauditlogconfig?view=exchange-ps) e abilitare il parametro "UnifiedAuditLogIngestionEnabled").
    - [O usando l'interfaccia utente di Centro sicurezza e conformità](https://docs.microsoft.com/office365/securitycompliance/search-the-audit-log-in-security-and-compliance#before-you-begin).

## <a name="connect-to-office-365"></a>Connettersi a Office 365

1. In Sentinel di Azure selezionare **connettori dati** , quindi fare clic sul riquadro **Office 365** .

2. Se non è già stato abilitato, in **connessione** usare il pulsante **Abilita** per abilitare la soluzione Office 365. Se è già abilitato, verrà identificato nella schermata di connessione come già abilitato.
1. Office 365 consente di trasmettere i dati da più tenant ad Azure Sentinel. Per ogni tenant a cui ci si vuole connettere, aggiungere il tenant in **Connetti tenant a Sentinel di Azure**. 
1. Viene visualizzata una schermata Active Directory. Viene richiesto di eseguire l'autenticazione con un utente amministratore globale in ogni tenant a cui si vuole connettersi a Sentinel di Azure e di fornire le autorizzazioni per Azure Sentinel per leggere i log. 
5. In Stream Office 365 Activity logs fare clic su **Select** per scegliere i tipi di log da trasmettere in Azure Sentinel. Attualmente, Azure Sentinel supporta Exchange e SharePoint.

4. Fare clic su **Applica modifiche**.

3. Per usare lo schema pertinente in Log Analytics per i log di Office 365, cercare **OfficeActivity**.


## <a name="next-steps"></a>Passaggi successivi
In questo documento si è appreso come connettere Office 365 ad Azure Sentinel. Per altre informazioni su Azure Sentinel, vedere gli articoli seguenti:
- Scopri come [ottenere visibilità sui dati e potenziali minacce](quickstart-get-visibility.md).
- Iniziare a [rilevare le minacce con Azure Sentinel](tutorial-detect-threats-built-in.md).

