---
title: Informazioni sul servizio riconoscimento vocale (Speech SDK)
titleSuffix: Azure Cognitive Services
description: Speech Software Development Kit (SDK) fornisce alle applicazioni l'accesso nativo alle funzioni del servizio Voce, rendendo più semplice lo sviluppo di software. Questo articolo fornisce dettagli aggiuntivi sull'SDK per Windows, Linux e Android.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: ee505cc78c16d85640c7e13541272f1bafe30061
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/02/2019
ms.locfileid: "71799997"
---
# <a name="about-the-speech-sdk"></a>Informazioni su Speech SDK

Speech Software Development Kit (SDK) consente alle applicazioni di accedere alle funzioni dei servizi di riconoscimento vocale, semplificando lo sviluppo di software abilitato per la sintesi vocale. Attualmente, gli SDK forniscono l'accesso a riconoscimento **vocale** **, sintesi vocale,** **traduzione vocale**, **riconoscimento preventivo**e **canale vocale diretto di bot Framework**. Una panoramica generale sulle funzionalità e sulle piattaforme supportate è disponibile nella [pagina voce](https://aka.ms/csspeech)della documentazione.

[!INCLUDE [Speech SDK Platforms](../../../includes/cognitive-services-speech-service-speech-sdk-platforms.md)]

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

## <a name="get-the-sdk"></a>Ottenere l'SDK

### <a name="windows"></a>Windows

Per Windows sono supportati le lingue seguenti:

* C# (UWP e .NET), C++: è possibile fare riferimento alla versione più recente del pacchetto Speech SDK NuGet e usare tale versione. Il pacchetto include librerie client a 32 e 64 bit e librerie gestite (.NET). Per installare l'SDK in Visual Studio, è possibile usare NuGet. Cercare **Microsoft.CognitiveServices.Speech**.

* Java: è possibile fare riferimento alla versione più recente del pacchetto Speech SDK Maven, che supporta solo Windows x64, e usare tale versione. Nel progetto Maven aggiungere `https://csspeechstorage.blob.core.windows.net/maven/` come repository aggiuntivo e fare riferimento a `com.microsoft.cognitiveservices.speech:client-sdk:1.7.0` come dipendenza.

### <a name="linux"></a>Linux

> [!NOTE]
> Attualmente sono supportati solo Ubuntu 16,04, Ubuntu 18,04 e Debian 9 nelle architetture di destinazione seguenti:
> - x86, x64 e ARM64 per C++ lo sviluppo
> - x64 e ARM64 per Java
> - x64 per .NET Core e Python

Verificare che siano installate le librerie necessarie eseguendo i comandi della shell seguenti:

In Ubuntu:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libasound2
```

In Debian 9:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.2 libasound2
```

* C#: è possibile fare riferimento alla versione più recente del pacchetto Speech SDK NuGet e usare tale versione. Per fare riferimento all'SDK, aggiungere al progetto il riferimento al pacchetto seguente:

  ```xml
  <PackageReference Include="Microsoft.CognitiveServices.Speech" Version="1.7.0" />
  ```

* Java: è possibile fare riferimento alla versione più recente del pacchetto Speech SDK Maven e usare tale versione. Nel progetto Maven aggiungere `https://csspeechstorage.blob.core.windows.net/maven/` come repository aggiuntivo e fare riferimento a `com.microsoft.cognitiveservices.speech:client-sdk:1.7.0` come dipendenza.

* C++: scaricare l'SDK come [pacchetto con estensione tar](https://aka.ms/csspeech/linuxbinary) e decomprimere i file nella directory desiderata. La tabella seguente illustra la struttura di cartelle dell'SDK:

  |`Path`|Descrizione|
  |-|-|
  |`license.md`|Licenza|
  |`ThirdPartyNotices.md`|Comunicazioni di terze parti|
  |`include`|File di intestazione per C e C++|
  |`lib/x64`|Libreria x64 nativa per il collegamento all'applicazione|
  |`lib/x86`|Libreria x86 nativa per il collegamento all'applicazione|

  Per creare un'applicazione, copiare o spostare i file binari e le librerie necessari nell'ambiente di sviluppo e includerli nel processo di compilazione come richiesto.

### <a name="android"></a>Android

Java SDK per Android viene inserito in un pacchetto come una [AAR (libreria Android)](https://developer.android.com/studio/projects/android-library), che include le librerie necessarie, nonché le autorizzazioni Android richieste. È ospitato in un repository Maven in `https://csspeechstorage.blob.core.windows.net/maven/` come pacchetto `com.microsoft.cognitiveservices.speech:client-sdk:1.7.0`.

Per usare il pacchetto dal progetto Android Studio apportare le modifiche seguenti:

* Nel file build.gradle a livello di progetto aggiungere quanto segue nella sezione `repository`:

  ```gradle
  maven { url 'https://csspeechstorage.blob.core.windows.net/maven/' }
  ```

* Nel file build.gradle a livello di modulo aggiungere quanto segue nella sezione `dependencies`:

  ```gradle
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:1.7.0'
  ```

Java SDK fa inoltre parte di [Speech Devices SDK](speech-devices-sdk.md).

[!INCLUDE [Get the samples](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Passaggi successivi

* [Ottenere una sottoscrizione di valutazione gratuita del Servizio di riconoscimento vocale](https://azure.microsoft.com/try/cognitive-services/)
* [Informazioni sul riconoscimento vocale in C#](quickstart-csharp-dotnet-windows.md)
