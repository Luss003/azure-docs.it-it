---
title: Considerazioni su Novell Android (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: Per informazioni su considerazioni specifiche, vedere l'articolo relativo all'uso di Novell Android con Microsoft Authentication Library per .NET (MSAL.NET).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/24/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: c916ac98774600c16eb26ed43b8ae4b273137865
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/23/2020
ms.locfileid: "76695008"
---
# <a name="xamarin-android-specific-considerations-with-msalnet"></a>Novell considerazioni specifiche per Android con MSAL.NET
Questo articolo illustra alcune considerazioni specifiche quando si usa Novell Android con Microsoft Authentication Library per .NET (MSAL.NET).

## <a name="set-the-parent-activity"></a>Impostare l'attività padre

In Novell. Android è necessario impostare l'attività padre in modo che il token venga restituito dopo che l'interazione è stata eseguita.

```csharp
var authResult = AcquireTokenInteractive(scopes)
 .WithParentActivityOrWindow(parentActivity)
 .ExecuteAsync();
```
È anche possibile impostare questa impostazione a livello di PublicClientApplication (in MSAL 4.2 +) tramite un callback.

```csharp
// Requires MSAL.NET 4.2 or above
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => parentActivity)
  .Build();
```

Si consiglia di usare CurrentActivityPlugin [qui](https://github.com/jamesmontemagno/CurrentActivityPlugin).  Il codice del generatore PublicClientApplication sarà simile al seguente:

```csharp
// Requires MSAL.NET 4.2 or above
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => CrossCurrentActivity.Current)
  .Build();
```


## <a name="ensuring-control-goes-back-to-msal-once-the-interactive-portion-of-the-authentication-flow-ends"></a>Assicurando che il controllo torni a MSAL una volta terminata la parte interattiva del flusso di autenticazione
In Android è necessario eseguire l'override del metodo `OnActivityResult` della `Activity` e chiamare il metodo SetAuthenticationContinuationEventArgs della classe MSAL di AuthenticationContinuationHelper.

```csharp
protected override void OnActivityResult(int requestCode, 
                                         Result resultCode, Intent data)
{
 base.OnActivityResult(requestCode, resultCode, data);
 AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode,
                                                                         resultCode,
                                                                         data);
}

```
Questa riga garantisce che il controllo torni a MSAL una volta terminata la parte interattiva del flusso di autenticazione.

## <a name="update-the-android-manifest"></a>Aggiornare il manifesto Android
Il `AndroidManifest.xml` deve contenere i valori seguenti:
```csharp
<activity android:name="microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="msal{client_id}" android:host="auth" />
         </intent-filter>
</activity>
```

In alternativa, è possibile [creare l'attività nel codice](https://docs.microsoft.com/xamarin/android/platform/android-manifest#the-basics) e non modificare manualmente `AndroidManifest.xml`. A tale proposito, è necessario creare una classe con l'attributo `Activity` e `IntentFilter`. Una classe che rappresenta gli stessi valori del codice XML precedente è la seguente:

```csharp
  [Activity]
  [IntentFilter(new[] { Intent.ActionView },
        Categories = new[] { Intent.CategoryBrowsable, Intent.CategoryDefault },
        DataHost = "auth",
        DataScheme = "msal{client_id}")]
  public class MsalActivity : BrowserTabActivity
  {
  }
```

### <a name="xamarinforms-43x-manifest"></a>Manifesto XamarinForms 4.3. X

Il codice generato da XamarinForms 4.3. x imposta l'attributo `package` su `com.companyname.{appName}` nel `AndroidManifest.xml`. Potrebbe essere necessario modificare il valore in modo che corrisponda allo spazio dei nomi `MainActivity.cs`, se si utilizza il `DataScheme` come `msal{client_id}`.

## <a name="use-the-embedded-web-view-optional"></a>Usare la visualizzazione Web incorporata (facoltativo)

Per impostazione predefinita, MSAL.NET usa il Web browser di sistema, che consente di ottenere SSO con le applicazioni Web e altre app. In alcuni casi rari, è possibile specificare che si desidera utilizzare la visualizzazione Web incorporata. Per altre informazioni, vedere [MSAL.NET usa un Web browser e un](msal-net-web-browsers.md) [browser di sistema Android](msal-net-system-browser-android-considerations.md).

```csharp
bool useEmbeddedWebView = !app.IsSystemWebViewAvailable;

var authResult = AcquireTokenInteractive(scopes)
 .WithParentActivityOrWindow(parentActivity)
 .WithEmbeddedWebView(useEmbeddedWebView)
 .ExecuteAsync();
```

## <a name="troubleshooting"></a>Risoluzione dei problemi
Se si crea una nuova applicazione Novell. Forms e si aggiunge un riferimento al pacchetto NuGet MSAL.Net, questo funzionerà semplicemente.
Tuttavia, se si desidera aggiornare un'applicazione Novell. Forms esistente a MSAL.NET Preview 1.1.2 o versione successiva, è possibile che si verifichino problemi di compilazione.

Per risolvere questi problemi, è necessario:
- Aggiornare il pacchetto NuGet MSAL.NET esistente a MSAL.NET Preview 1.1.2 o versione successiva
- Verificare che Novell. Forms venga aggiornato automaticamente alla versione 2.5.0.122203 (in caso contrario, eseguire l'aggiornamento a questa versione)
- Verificare che Novell. Android. support. v4 venga aggiornato automaticamente alla versione 25.4.0.2 (in caso contrario, eseguire l'aggiornamento a questa versione)
- Tutti i pacchetti Novell. Android. support dovrebbero avere come destinazione la versione 25.4.0.2
- Pulisci/ricompila
- Provare a impostare le compilazioni max Parallel Project su 1 in Visual Studio (Opzioni-> progetti e soluzioni-> compilare ed eseguire > numero massimo di compilazioni di progetti paralleli)
- In alternativa, se si esegue la compilazione dalla riga di comando, provare a rimuovere/m dal comando se lo si sta usando.


### <a name="error-the-name-authenticationcontinuationhelper-does-not-exist-in-the-current-context"></a>Errore: il nome ' AuthenticationContinuationHelper ' non esiste nel contesto corrente

Questa operazione è probabilmente dovuta al fatto che Visual Studio non ha aggiornato correttamente il file Android. csproj *. In alcuni casi il **\<HintPath >** FilePath contiene erroneamente netstandard13 anziché **monoandroid90**.

```xml
<Reference Include="Microsoft.Identity.Client, Version=3.0.4.0, Culture=neutral, PublicKeyToken=0a613f4dd989e8ae,
           processorArchitecture=MSIL">
  <HintPath>..\..\packages\Microsoft.Identity.Client.3.0.4-preview\lib\monoandroid90\Microsoft.Identity.Client.dll</HintPath>
</Reference>
```

## <a name="next-steps"></a>Passaggi successivi

Altre informazioni ed esempi sono disponibili nel paragrafo [considerazioni specifiche per Android](https://github.com/azure-samples/active-directory-xamarin-native-v2#android-specific-considerations) del file Readme.MD dell'esempio seguente:

| Esempio | Piattaforma | Description |
| ------ | -------- | ----------- |
|[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) | Xamarin iOS, Android, piattaforma UWP | Una semplice app Novell Forms che illustra come usare MSAL per autenticare MSA e Azure AD tramite l'endpoint aggiungere v 2.0 e accedere al Microsoft Graph con il token risultante. <br>![Topologia](media/msal-net-xamarin-android-considerations/topology.png) |
