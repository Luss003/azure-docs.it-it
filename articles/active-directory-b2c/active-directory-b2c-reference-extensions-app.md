---
title: App Extensions in Azure Active Directory B2C | Microsoft Docs
description: Ripristino di b2c-extensions-app.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/06/2017
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: e5d38d8d791c6b932d8a28a898f2e2b80caff7ac
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2019
ms.locfileid: "67654064"
---
# <a name="azure-ad-b2c-extensions-app"></a>Azure AD B2C: App Extensions

Quando viene creata una directory di Azure AD B2C, un'app denominata `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` viene creata automaticamente nella nuova directory. Questa app, definita **b2c-extensions-app**, è visibile in *Registrazioni per l'app*. Viene usata dal servizio Azure AD B2C per archiviare informazioni sugli utenti e sugli attributi personalizzati. Se l'app viene eliminata, Azure AD B2C non funzionerà correttamente e questo problema influirà sull'ambiente di produzione.

> [!IMPORTANT]
> Eliminare l'app b2c-extensions-app solo se si prevede l'eliminazione immediata del tenant. Se l'app rimane eliminata per più di 30 giorni, le informazioni utente andranno perse definitivamente.

## <a name="verifying-that-the-extensions-app-is-present"></a>Verifica della presenza dell'app Estensioni

Per verificare che l'app b2c-extensions-app sia presente:

1. Nel tenant di Azure AD B2C fare clic su **Tutti i servizi** nel menu di spostamento di sinistra.
1. Cercare e aprire **Registrazioni per l'app**.
1. Cercare un'app che inizia con **b2c-extensions-app**

## <a name="recover-the-extensions-app"></a>Recuperare l'app Estensioni

Se l'app b2c-extensions-app è stata eliminata accidentalmente, sono disponibili 30 giorni per il recupero. È possibile ripristinare l'app usando l'API Graph:

1. Passare a [https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/).
1. Accedere al sito come amministratore globale per la directory di Azure AD B2C per cui si vuole ripristinare l'app eliminata. L'amministratore globale deve avere un indirizzo di posta elettronica simile al seguente: `username@{yourTenant}.onmicrosoft.com`.
1. Emettere una richiesta HTTP GET per l'URL `https://graph.windows.net/myorganization/deletedApplications` con api-version=1.6. Questa operazione consente di elencare tutte le applicazioni eliminate negli ultimi 30 giorni.
1. Trovare nell'elenco l'applicazione il cui nome inizia con 'b2c-extension-app' e copiare il valore della rispettiva proprietà `objectid`.
1. Emettere una richiesta HTTP POST per l'URL `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`. Sostituire la parte `{OBJECTID}` dell'URL con il valore `objectid` del passaggio precedente.

Dovrebbe essere possibile [visualizzare l'app ripristinata](#verifying-that-the-extensions-app-is-present) nel portale di Azure.

> [!NOTE]
> Un'applicazione può essere ripristinata solo se è stata eliminata negli ultimi 30 giorni. Se sono trascorsi più di 30 giorni, i dati andranno persi definitivamente. Per ottenere assistenza, inviare un ticket al supporto tecnico.
