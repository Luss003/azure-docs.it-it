---
title: Introduzione a Xamarin per Azure AD | Microsoft Docs
description: Compilare un'applicazione Xamarin che si integra con Azure AD per l'accesso e chiama le API protette da Azure AD usando OAuth.
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: 198cd2c3-f7c8-4ec2-b59d-dfdea9fe7d95
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 07/17/2019
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 26eff9456ba3ee1e2ff5ea41a552b4ec90a4fd1f
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/23/2020
ms.locfileid: "76703695"
---
# <a name="quickstart-build-a-xamarin-app-that-integrates-microsoft-sign-in"></a>Avvio rapido: Creare un'app Xamarin che si integra con il sistema di accesso Microsoft

[Microsoft Identity Platform](v2-overview.md) è un'evoluzione della piattaforma per sviluppatori di Azure Active Directory (Azure AD). Consente agli sviluppatori di creare applicazioni che supportano l'accesso per tutte le identità Microsoft e il recupero di token per chiamare API Microsoft, come Microsoft Graph o API create dagli sviluppatori.

[Microsoft Authentication Library (MSAL)](msal-overview.md) consente agli sviluppatori di acquisire token dall'endpoint di Microsoft Identity Platform per accedere ad API Web protette. Active Directory Authentication Library (ADAL) si integra con l'endpoint di Azure AD per sviluppatori (v1.0), dove MSAL si integra con l'endpoint di Microsoft Identity Platform (v2.0).

## <a name="next-steps"></a>Passaggi successivi

Per le nuove applicazioni Xamarin, è consigliabile usare Microsoft Identity Platform (v2.0) e MSAL per acquisire i token e accedere alle API Web protette. Per iniziare, vedere [Integrare Microsoft Platform Identity e Microsoft Graph in un'app di moduli Xamarin usando MSAL](https://github.com/azure-samples/active-directory-xamarin-native-v2#integrate-microsoft-identity-and-the-microsoft-graph-into-a-xamarin-forms-app-using-msal) (senza i passaggi facoltativi).
