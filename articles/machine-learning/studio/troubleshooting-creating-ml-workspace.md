---
title: Risoluzione dei problemi relativi a un'area di lavoro
titleSuffix: ML Studio (classic) - Azure
description: Questa guida fornisce soluzioni per alcune delle principali problemi riscontrati durante la configurazione delle aree di lavoro Azure Machine Learning Studio (classiche).
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/20/2017
ms.openlocfilehash: a4385d88912e7c4d391baa8f5bc50be1e07f4fa7
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2020
ms.locfileid: "77152789"
---
# <a name="troubleshooting-guide-create-and-connect-to-an-azure-machine-learning-studio-classic-workspace"></a>Guida alla risoluzione dei problemi: creare e connettersi a un'area di lavoro di Azure Machine Learning Studio (classica)
Questa guida fornisce soluzioni per alcune delle principali problemi riscontrati durante la configurazione delle aree di lavoro Azure Machine Learning Studio (classiche).



## <a name="workspace-owner"></a>Proprietario dell'area di lavoro
Per aprire un'area di lavoro in Machine Learning Studio (versione classica), è necessario aver eseguito l'accesso all'account Microsoft usato per creare l'area di lavoro oppure è necessario ricevere un invito dal proprietario per partecipare all'area di lavoro. Dal portale di Azure è possibile gestire l'area di lavoro e anche configurare l'accesso.

Per ulteriori informazioni sulla gestione di un'area di lavoro, vedere [gestire un'area di lavoro di Azure Machine Learning Studio (classica)].

[Gestire un'area di lavoro di Azure Machine Learning Studio (classica)]: manage-workspace.md

## <a name="allowed-regions"></a>Aree geografiche disponibili
Machine Learning è attualmente disponibile in un numero limitato di aree. Se la sottoscrizione non include una di queste aree, è possibile che venga visualizzato l'errore "Non sono presenti sottoscrizioni nelle aree consentite".

Per richiedere l'aggiunta di un'area alla sottoscrizione, creare una nuova richiesta al supporto tecnico Microsoft dal portale di Azure, scegliere **Fatturazione** come tipo di problema, quindi seguire le istruzioni per inviare la richiesta.

## <a name="storage-account"></a>Account di archiviazione
Il servizio Machine Learning necessita di un account di archiviazione per archiviare i dati. È possibile usare un account di archiviazione esistente oppure è possibile creare un nuovo account di archiviazione quando si crea la nuova area di lavoro Machine Learning Studio (classica) (se si dispone di una quota per creare un nuovo account di archiviazione).

Dopo aver creato la nuova area di lavoro Machine Learning Studio (classica), è possibile accedere a Machine Learning Studio (classica) usando il account Microsoft usato per creare l'area di lavoro. Se viene visualizzato il messaggio di errore relativo all'area di lavoro non trovata, analogo a quanto mostrato nella schermata sottostante, usare la procedura seguente per eliminare i cookie dal browser.

![Area di lavoro non trovata](media/troubleshooting-creating-ml-workspace/screen3.png)

**Per eliminare i cookie dal browser**

1. Se si usa Internet Explorer, fare clic sul pulsante **Strumenti** nell'angolo superiore destro, quindi selezionare **Opzioni Internet**.  

   ![Opzioni Internet](media/troubleshooting-creating-ml-workspace/screen4.png)

2. Nella scheda **Generale** fare clic su **Elimina**.

   ![Scheda Generale](media/troubleshooting-creating-ml-workspace/screen5.png)

3. Nella finestra di dialogo **Elimina cronologia esplorazioni** assicurarsi che l'opzione **Cookie e dati di siti Web** sia selezionata, quindi fare clic su **Elimina**.

   ![Eliminare i cookie](media/troubleshooting-creating-ml-workspace/screen6.png)

Dopo l'eliminazione dei cookie, riavviare il browser e passare alla pagina [Microsoft Azure Machine Learning Studio (classica)](https://studio.azureml.net) . Quando vengono richiesti nome utente e password, immettere lo stesso account Microsoft usato per creare l'area di lavoro.

## <a name="comments"></a>Comments

L'obiettivo consiste nel semplificare al massimo l'esperienza di Machine Learning. Inserire eventuali commenti e problemi nel [forum su Azure Machine Learning](https://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) per contribuire al miglioramento del prodotto.
