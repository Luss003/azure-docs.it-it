---
title: File di inclusione
description: File di inclusione
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/05/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 3d93d3aa3e4e646f8e054f96f17bbe4a011d422d
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2020
ms.locfileid: "77211451"
---
Il modello di progetto Funzioni di Azure in Visual Studio crea un progetto che può essere pubblicato in un'app per le funzioni in Azure. È possibile usare un'app per le funzioni per raggruppare le funzioni in un'unità logica e semplificare la gestione, la distribuzione, il ridimensionamento e la condivisione delle risorse.

1. Nel menu **File** in Visual Studio selezionare **Nuovo** > **Progetto**.

1. Nella finestra di dialogo **Crea un nuovo progetto** cercare `functions`, scegliere il modello **Funzioni di Azure** e selezionare **Avanti**.

1. Immettere un nome per il progetto e selezionare **Crea**. Il nome dell'app per le funzioni deve essere valido come spazio dei nomi C#, quindi non usare caratteri di sottolineatura, trattini o altri caratteri non alfanumerici.

1. In **Crea una nuova applicazione Funzioni di Azure** usare le opzioni seguenti:

    + **Funzioni di Azure v2 (.NET Core)**
    + **Trigger HTTP**
    + **Account di archiviazione**: **Emulatore di archiviazione**
    + **Livello di autorizzazione**: **Anonimo** 

    | Opzione      | Valore consigliato  | Descrizione                      |
    | ------------ |  ------- |----------------------------------------- |
    | **Runtime di Funzioni** | **Funzioni di Azure 2.x <br />(.NET Core)** | Questa impostazione crea un progetto per le funzioni che usa la versione 2.x del runtime di Funzioni di Azure e supporta .NET Core. Funzioni di Azure 1.x supporta .NET Framework. Per altre informazioni, vedere [Come specificare le versioni del runtime per Funzioni di Azure](../articles/azure-functions/functions-versions.md).   |
    | **Modello di funzione** | **Trigger HTTP** | Questa impostazione crea una funzione attivata da una richiesta HTTP. |
    | **Storage Account**  | **Emulatore di archiviazione** | Un trigger HTTP non usa la connessione dell'account di archiviazione di Azure. Tutti gli altri tipi di trigger richiedono una stringa di connessione dell'account di archiviazione valida. Dato che Funzioni richiede un account di archiviazione, ne viene assegnato o creato uno quando si pubblica il progetto in Azure. |
    | **Livello di autorizzazione** | **Anonimo** | Viene creata una funzione che può essere attivata da qualsiasi client senza dover fornire una chiave. Questa impostazione di autorizzazione consente di testare più facilmente la nuova funzione. Per altre informazioni sulle chiavi e l'autorizzazione, vedere [Chiavi autorizzazione](../articles/azure-functions/functions-bindings-http-webhook-trigger.md#authorization-keys) in [Associazioni HTTP e webhook](../articles/azure-functions/functions-bindings-http-webhook.md). |
    
    > [!NOTE]
    > Assicurarsi di impostare **Livello di autorizzazione** su `Anonymous`. Se si sceglie il livello predefinito di `Function`, è necessario fornire la [chiave di funzione](../articles/azure-functions/functions-bindings-http-webhook-trigger.md#authorization-keys) nelle richieste di accesso all'endpoint della funzione.
    
4. Selezionare **Crea** per creare il progetto per le funzioni e la funzione attivata da HTTP.
