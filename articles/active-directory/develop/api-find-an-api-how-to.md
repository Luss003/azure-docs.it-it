---
title: Trovare un'API per un'app personalizzata | Azure
description: Informazioni su come configurare le autorizzazioni necessarie per accedere a un'API specifica nell'applicazione Azure AD personalizzata
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: ryanwi
ms.openlocfilehash: bc50ec86866b7fe04c549c7fd463b6de4df3444b
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/23/2020
ms.locfileid: "76698391"
---
# <a name="how-to-find-a-specific-api-needed-for-a-custom-developed-application"></a>Come trovare un'API specifica necessaria per un'applicazione personalizzata

L'accesso alle API richiede la configurazione di ruoli e ambiti di accesso. Se si desidera esporre le API Web dell'applicazione della risorsa alle applicazioni client, è necessario configurare gli ambiti e i ruoli di accesso per l'API. Se si desidera che un'applicazione client acceda a un'API Web, è necessario configurare le autorizzazioni per accedere all'API nella registrazione dell'app.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Configurazione di un'applicazione della risorsa per esporre le API Web

Quando si espone l'API Web, l'API viene visualizzata nell'elenco **Selezionare un'API** quando si aggiungono le autorizzazioni alla registrazione di un'applicazione. Per aggiungere gli ambiti di accesso, seguire i passaggi descritti in [configurare un'applicazione per esporre le API Web](quickstart-configure-app-expose-web-apis.md).

## <a name="configuring-a-client-application-to-access-web-apis"></a>Configurazione di un'applicazione client per accedere alle API Web

Quando si aggiungono le autorizzazioni per la registrazione dell'app, è possibile **aggiungere l'accesso all'API** per esporre le API Web. Per accedere alle API Web, seguire la procedura illustrata in [configurare un'applicazione client per accedere alle API Web](quickstart-configure-app-access-web-apis.md).

## <a name="next-steps"></a>Passaggi successivi

- [Informazioni sul manifesto dell'applicazione in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)
