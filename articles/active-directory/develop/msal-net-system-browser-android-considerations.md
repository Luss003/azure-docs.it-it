---
title: Considerazioni sul browser del sistema Novell Android (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: Informazioni sulle considerazioni sull'uso dei browser di sistema in Novell Android con Microsoft Authentication Library per .NET (MSAL.NET).
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: ad26a4d619a7984f08a8decc87f9339adae47cdd
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/11/2020
ms.locfileid: "77132606"
---
#  <a name="xamarin-android-system-browser-considerations-for-using-msalnet"></a>Novell Android System Browser considerazioni sull'uso di MSAL.NET

Questo articolo illustra le considerazioni da tenere presenti quando si usa il browser di sistema in Novell Android con Microsoft Authentication Library per .NET (MSAL.NET).

A partire da MSAL.NET 2.4.0 Preview, MSAL.NET supporta browser diversi da Chrome. Non è più necessario che Chrome sia installato nel dispositivo Android per l'autenticazione.

Si consiglia di utilizzare browser che supportano schede personalizzate. Di seguito sono riportati alcuni esempi di questi browser:

| Browser con supporto per schede personalizzate | Nome del pacchetto |
|------| ------- |
|Chrome | com.android.chrome|
|Microsoft Edge | com.microsoft.emmx|
|Firefox | org.mozilla.firefox|
|Ecosia | com.ecosia.android|
|Kiwi | com.kiwibrowser.browser|
|Incredibile | com. brave. browser|

Oltre a identificare i browser che offrono supporto per schede personalizzate, il test indica che alcuni browser che non supportano schede personalizzate funzionano anche per l'autenticazione. Questi browser includono opera, opera mini, inbrowser e Maxthon. 

## <a name="tested-devices-and-browsers"></a>Dispositivi e browser testati
Nella tabella seguente sono elencati i dispositivi e i browser testati per la compatibilità con l'autenticazione di.

| Dispositivo | Browser.     |  Risultato  | 
| ------------- |:-------------:|:-----:|
| Huawei/uno + | \* di Chrome | Test superato|
| Huawei/uno + | \* Edge | Test superato|
| Huawei/uno + | \* Firefox | Test superato|
| Huawei/uno + | \* coraggioso | Test superato|
| Uno + | \* Ecosia | Test superato|
| Uno + | Kiwi\* | Test superato|
| Huawei/uno + | Opera | Test superato|
| Huawei | OperaMini | Test superato|
| Huawei/uno + | InBrowser | Test superato|
| Uno + | Maxthon | Test superato|
| Huawei/uno + | DuckDuckGo | Autenticazione annullata dall'utente|
| Huawei/uno + | Browser UC | Autenticazione annullata dall'utente|
| Uno + | Delfino | Autenticazione annullata dall'utente|
| Uno + | Browser CM | Autenticazione annullata dall'utente|
| Huawei/uno + | Nessuno installato | Eccezione AndroidActivityNotFound|

\* supporta le schede personalizzate

## <a name="known-issues"></a>Problemi noti

Se per l'utente non è abilitato alcun browser sul dispositivo, MSAL.NET genererà un'eccezione `AndroidActivityNotFound`.  
  - **Mitigazione**: richiedere all'utente di abilitare un browser sul dispositivo. Consigliare un browser che supporti schede personalizzate.

Se l'autenticazione ha esito negativo, ad esempio se l'autenticazione viene avviata con DuckDuckGo, MSAL.NET restituirà `AuthenticationCanceled MsalClientException`. 
  - **Problema principale**: un browser che supporta le schede personalizzate non è stato abilitato nel dispositivo. Autenticazione avviata con un browser che non è stato in grado di completare l'autenticazione. 
  - **Mitigazione**: richiedere all'utente di abilitare un browser sul dispositivo. Consigliare un browser che supporti schede personalizzate.

## <a name="next-steps"></a>Passaggi successivi
Per altre informazioni ed esempi di codice, vedere [scelta tra un Web browser incorporato e un browser di sistema in Novell Android e l'](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/MSAL.NET-uses-web-browser#choosing-between-embedded-web-browser-or-system-browser-on-xamarinandroid) [interfaccia utente Web di sistema incorporata](msal-net-web-browsers.md#embedded-vs-system-web-ui).  