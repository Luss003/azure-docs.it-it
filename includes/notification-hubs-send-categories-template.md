---
title: File di inclusione
description: File di inclusione
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 03/30/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: f0ff729084d194ff2e05e89eadc45782f775b1c5
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/18/2019
ms.locfileid: "67180392"
---
In questa sezione si invieranno notizie aggiornate come notifiche modello con tag da un'app console .NET. 

1. In Visual Studio creare una nuova applicazione console in Visual C#: a. Nel menu selezionare **File** > **Nuovo** > **Progetto**.
    b. Espandere **Visual C#** e selezionare **Desktop Windows**. 
    c. Selezionare **App console (.NET Framework)** nell'elenco di modelli. 
    d. Immettere un **nome** per l'app. 
    e. Selezionare una **cartella** per l'app.
    f. Selezionare **OK** per creare il progetto. 
2. Nel menu principale di Visual Studio scegliere **Strumenti**,  > **Gestione pacchetti NuGet** > **Console di Gestione pacchetti** e quindi immettere la stringa seguente nella finestra della console:
   
    ```
    Install-Package Microsoft.Azure.NotificationHubs
    ```
   
3. Selezionare **Enter** (Invio).  
    Questa azione aggiunge un riferimento ad Azure Notification Hubs SDK usando il [pacchetto NuGet Microsoft.Azure.NotificationHubs].

4. Aprire il file Program.cs e aggiungere l'istruzione `using` seguente:
   
    ```csharp
    using Microsoft.Azure.NotificationHubs;
    ```

5. Nella classe `Program` aggiungere il metodo seguente oppure sostituirlo se esiste già:
   
    ```csharp
    private static async void SendTemplateNotificationAsync()
    {
        // Define the notification hub.
        NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");

        // Create an array of breaking news categories.
        var categories = new string[] { "World", "Politics", "Business", "Technology", "Science", "Sports"};

        // Send the notification as a template notification. All template registrations that contain
        // "messageParam" and the proper tags will receive the notifications.
        // This includes APNS, GCM, WNS, and MPNS template registrations.

        Dictionary<string, string> templateParams = new Dictionary<string, string>();

        foreach (var category in categories)
        {
            templateParams["messageParam"] = "Breaking " + category + " News!";
            await hub.SendTemplateNotificationAsync(templateParams, category);
        }
    }
    ```   
   
    Questo codice invia una notifica modello per ognuno dei sei tag nella matrice di stringhe. L'uso di tag garantisce che i dispositivi ricevano le notifiche solo per le categorie registrate.

5. Nel codice precedente sostituire i segnaposto `<hub name>` e `<connection string with full access>` con il nome dell'hub di notifica e la stringa di connessione per *DefaultFullSharedAccessSignature* dal dashboard dell'hub di notifica.

6. Nel metodo **Main** aggiungere le righe seguenti:
   
    ```csharp
    SendTemplateNotificationAsync();
    Console.ReadLine();
    ```

7. Creare l'app console.

<!-- Images. -->
[13]: ./media/notification-hubs-back-end/notification-hub-create-console-app.png

<!-- URLs. -->
[Get started with Notification Hubs]: ../articles/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Notification Hubs REST interface]: https://msdn.microsoft.com/library/windowsazure/dn223264.aspx
[Add push notifications for Mobile Apps]: ../articles/app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md
[How to use Notification Hubs from Java or PHP]: ../articles/notification-hubs/notification-hubs-java-push-notification-tutorial.md
[pacchetto NuGet Microsoft.Azure.NotificationHubs]: http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/
