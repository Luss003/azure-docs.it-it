---
title: Problemi noti del browser Safari (MSAL. js) | Azure
titleSuffix: Microsoft identity platform
description: Informazioni sui problemi che si verificano quando si usa Microsoft Authentication Library per JavaScript (MSAL. js) con Safari browser.
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: troubleshooting
ms.workload: identity
ms.date: 05/16/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: edb995e31c2872c1541e29fee09dd66aafc8f9e2
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/23/2020
ms.locfileid: "76696113"
---
# <a name="known-issues-on-safari-browser-with-msaljs"></a>Problemi noti del browser Safari con MSAL. js 

## <a name="silent-token-renewal-on-safari-12-and-itp-20"></a>Rinnovo del token invisibile in Safari 12 e ITP 2,0

I sistemi operativi Apple iOS 12 e MacOS 10,14 includono una versione del [browser Safari 12](https://developer.apple.com/safari/whats-new/). Per la sicurezza e la privacy, Safari 12 include la [prevenzione del rilevamento intelligente 2,0](https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/). In questo modo il browser Elimina i cookie di terze parti impostati. ITP 2,0 Considera anche i cookie impostati dai provider di identità come cookie di terze parti.

### <a name="impact-on-msaljs"></a>Effetto su MSAL. js

MSAL. js usa un iframe nascosto per eseguire l'acquisizione e il rinnovo del token invisibile all'utente come parte delle chiamate `acquireTokenSilent`. Le richieste di token invisibile di sistema si basano sull'iframe che ha accesso alla sessione utente autenticata rappresentata dai cookie impostati da Azure AD. Con ITP 2,0 che impedisce l'accesso a questi cookie, MSAL. js non è in grado di acquisire e rinnovare automaticamente i token, causando `acquireTokenSilent` errori.

A questo punto non esiste alcuna soluzione per questo problema e si stanno valutando le opzioni con la community degli standard.

### <a name="work-around"></a>Aggirare

Per impostazione predefinita, l'impostazione ITP è abilitata nel browser Safari. È possibile disabilitare questa impostazione passando a **preferenze** -> **privacy** e deselezionando l'opzione **Impedisci rilevamento tra siti** .

![impostazione Safari](./media/msal-js-known-issue-safari-browser/safari.png)

È necessario gestire gli errori di `acquireTokenSilent` con una chiamata a un token di acquisizione interattiva, che richiede all'utente di eseguire l'accesso.
Per evitare gli accessi ripetuti, un approccio che è possibile implementare consiste nel gestire l'errore `acquireTokenSilent` e fornire all'utente un'opzione per disabilitare l'impostazione di ITP in Safari prima di procedere con la chiamata interattiva. Dopo che l'impostazione è stata disabilitata, i rinnovi dei token Silent successivi dovrebbero avere esito positivo.
