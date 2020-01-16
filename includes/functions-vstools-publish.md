---
title: File di inclusione
description: File di inclusione
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/02/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 689889588aba4da888a7d66f5e1d45dfde71d520
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/15/2020
ms.locfileid: "76020963"
---
1. In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto e scegliere **Pubblica**.

2. Nella finestra di dialogo **Selezionare una destinazione di pubblicazione** usare le opzioni di pubblicazione illustrate nella tabella sotto l'immagine: 

    ![Selezionare una destinazione di pubblicazione](./media/functions-vstools-publish/functions-visual-studio-publish-profile.png)

    | Opzione      | Descrizione                                |
    | ------------ |  -------------------------------------------------- |
    | **Piano di consumo di Funzioni di Azure** |   Quando si pubblica il progetto in un'app per le funzioni eseguita in un [piano a consumo](../articles/azure-functions/functions-scale.md#consumption-plan), vengono addebitati solo i costi relativi alle esecuzioni dell'app per le funzioni. Altri piani di hosting comportano costi più elevati. Per altre informazioni, vedere [Ridimensionamento e hosting di Funzioni di Azure](../articles/azure-functions/functions-scale.md). | 
    | **Crea nuovo** | Viene creata in Azure una nuova app per le funzioni, con le risorse correlate. Se si sceglie **Seleziona esistente**, tutti i file nell'app per le funzioni esistente in Azure vengono sovrascritti da file del progetto locale. Usare questa opzione solo quando si ripubblicano gli aggiornamenti a un'app per le funzioni esistente. |
    | **Esegui da file di pacchetto** | L'app per le funzioni viene distribuita usando la [distribuzione ZIP](../articles/azure-functions/functions-deployment-technologies.md#zip-deploy) con la modalità [esecuzione da pacchetto](../articles/azure-functions/run-functions-from-deployment-package.md) abilitata. È la modalità consigliata per eseguire le funzioni, perché garantisce prestazioni migliori. Quando non si usa questa opzione, assicurarsi di arrestare l'esecuzione in locale del progetto di app per le funzioni prima della pubblicazione in Azure. |


3. Selezionare **Pubblica**. Se non è già stato effettuato l'accesso all'account Azure da Visual Studio, selezionare **Accedi**. È anche possibile creare un account Azure gratuito.

4. Nella finestra di dialogo **Servizio app di Azure: Crea nuovo** usare le impostazioni per l'**hosting** specificate nella tabella sotto l'immagine:

    ![Finestra di dialogo Crea servizio app](./media/functions-vstools-publish/functions-visual-studio-publish.png)

    | Impostazione      | Valore consigliato  | Descrizione                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nome** | Nome globalmente univoco | Nome che identifica in modo univoco la nuova app per le funzioni. I caratteri validi sono `a-z`, `0-9` e `-`. |
    | **Sottoscrizione** | Scegliere la sottoscrizione | Sottoscrizione di Azure da usare. |
    | **[Gruppo di risorse](../articles/azure-resource-manager/management/overview.md)** | myResourceGroup |  Nome del gruppo di risorse in cui creare l'app per le funzioni. Per creare un nuovo gruppo di risorse scegliere **Nuovo**.|
    | **[Piano di hosting](../articles/azure-functions/functions-scale.md)** | Piano a consumo | Assicurarsi di scegliere **Consumo** in **Dimensione** dopo aver selezionato **Nuovo** per creare un piano serverless. Scegliere anche una **Località**  nelle [vicinanze](https://azure.microsoft.com/regions/) o vicino ad altri servizi a cui accedono le funzioni. Se si esegue un piano diverso da **Consumo**, è necessario gestire il [ridimensionamento dell'app per le funzioni](../articles/azure-functions/functions-scale.md).  |
    | **[Archiviazione di Azure](../articles/storage/common/storage-account-create.md)** | Account di archiviazione per utilizzo generico | L'account di archiviazione di Azure è necessario per il runtime di Funzioni. Selezionare **Nuovo** per creare un account di archiviazione per utilizzo generico. È anche possibile usare un account esistente che soddisfi i [requisiti dell'account di archiviazione](../articles/azure-functions/functions-scale.md#storage-account-requirements).  |

5. Selezionare **Crea** per creare un'app per le funzioni e le risorse correlate in Azure con queste impostazioni e distribuire il codice del progetto per le funzioni. 

6. Al termine della distribuzione prendere nota del valore di **URL sito**, ovvero l'indirizzo dell'app per le funzioni in Azure.

    ![Messaggio di esito positivo della pubblicazione](./media/functions-vstools-publish/functions-visual-studio-publish-complete.png)
