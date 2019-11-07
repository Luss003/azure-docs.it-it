---
title: Gestire account lab in Azure Lab Services | Microsoft Docs
description: Informazioni su come creare un account lab, visualizzare tutti gli account lab o eliminare un account lab in una sottoscrizione di Azure.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2019
ms.author: spelluru
ms.openlocfilehash: ee64780bca13bf497793dc90ad22d3eaf91949a6
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2019
ms.locfileid: "73606337"
---
# <a name="manage-lab-accounts-in-azure-lab-services"></a>Gestire account lab in Azure Lab Services 
In Azure Lab Services, un account Lab è un contenitore per i tipi Lab gestiti, ad esempio Lab in aula. Un amministratore configura un account lab con Azure Lab Services e fornisce l'accesso ai proprietari del lab autorizzati a creare lab nell'account. Questo articolo descrive come creare un account lab, visualizzare tutti gli account lab o eliminare un account lab.

## <a name="create-a-lab-account"></a>Creare un account lab
La procedura seguente illustra come usare il portale di Azure per creare un account lab con Azure Lab Services. 

1. Accedere al [portale di Azure](https://portal.azure.com).
2. Scegliere **Tutti i servizi** dal menu a sinistra. Selezionare **account Lab** nella sezione **DevOps** . Se si seleziona l'asterisco (`*`) accanto ad **Account Lab**, l'opzione verrà aggiunta alla sezione **PREFERITI** nel menu a sinistra. Da questo momento in poi, selezionare **Account Lab** in **PREFERITI**.

    ![Tutti i servizi -> Account Lab](../media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. Nella pagina **Account Lab** selezionare **Aggiungi** sulla barra degli strumenti. 

    ![Selezionare Aggiungi nella pagina Account Lab](../media/tutorial-setup-lab-account/add-lab-account-button.png)
4. Nella pagina **Account Lab** seguire questa procedura: 
    1. In **Lab account name** (Nome account lab) immettere un nome. 
    2. Selezionare la **sottoscrizione di Azure** in cui creare l'account lab.
    3. In **Gruppo di risorse** selezionare **Crea nuovo** e immettere un nome per il gruppo di risorse.
    4. In **Posizione** selezionare una posizione o un'area in cui si vuole creare l'account lab. 
    5. Selezionare una **raccolta immagini condivisa** esistente o crearne una. È possibile salvare la macchina virtuale modello nella raccolta immagini condivisa per poter essere riutilizzata da altri utenti. Per informazioni dettagliate sulle raccolte di immagini condivisa, vedere [Use a shared image gallery in Azure Lab Services](how-to-use-shared-image-gallery.md) (Usare una raccolta immagini condivisa in Azure Lab Services).
    6. Per **Rete virtuale peer** selezionare una rete virtuale peer per la rete lab. I lab creati in questo account sono connessi alla rete virtuale selezionata e hanno accesso alle risorse nella rete virtuale selezionata. 
    7. Specificare un **intervallo di indirizzi** per le macchine virtuali nel lab. L'intervallo di indirizzi deve essere nella notazione CIDR (Inter-Domain Routing) di classe (ad esempio: 10.20.0.0/23). Le macchine virtuali nel lab verranno create in questo intervallo di indirizzi. Per altre informazioni, vedere [Specificare un intervallo di indirizzi per le macchine virtuali nel lab](how-to-configure-lab-accounts.md#specify-an-address-range-for-vms-in-the-lab).    
    8. Per il campo **Consenti all'autore del lab di selezionare la località del lab**, specificare se si vuole consentire agli autori di selezionare una località per il lab. L'opzione è disabilitata per impostazione predefinita. Quando l'opzione è disattivata, gli autori del lab non possono specificare una località per il lab che si sta creando. I lab vengono creati nella località geografica più vicina all'account lab. Quando l'opzione è abilitata, un autore può selezionare una località al momento della creazione di un lab.      
    9. Selezionare **Crea**. 

        ![Finestra Create a lab account (Crea un account lab)](../media/tutorial-setup-lab-account/lab-account-settings.png)
5. Selezionare l'**icona del campanello** sulla barra degli strumenti (**Notifiche**), verificare se la distribuzione è riuscita e quindi selezionare **Vai alla risorsa**. 

    In alternativa, selezionare **Aggiorna** nella pagina **Account Lab** e selezionare l'account lab creato. 

    ![Finestra Create a lab account (Crea un account lab)](../media/tutorial-setup-lab-account/go-to-lab-account.png)    
6. Verrà visualizzata la pagina dell'**account lab** seguente:

    ![Pagina dell'account lab](../media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="view-lab-accounts"></a>Visualizzare gli account lab
1. Accedere al [portale di Azure](https://portal.azure.com).
2. Selezionare **Tutte le risorse** dal menu. 
3. Selezionare **Account Lab** per il **tipo**. 
    È anche possibile filtrare per sottoscrizione, gruppo di risorse, posizioni e tag. 

    ![Tutte le risorse -> Account Lab](../media/how-to-manage-lab-accounts/all-resources-lab-accounts.png)

## <a name="view-and-manage-labs-in-the-lab-account"></a>Visualizzare e gestire i lab nell'account lab

1. Nella pagina **Account lab** selezionare **Lab** nel menu a sinistra.

    ![Lab nell'account](../media/how-to-manage-lab-accounts/labs-in-account.png)
1. Viene visualizzato un **elenco di lab** nell'account con le informazioni seguenti: 
    1. Nome del lab.
    2. Data di creazione del lab. 
    3. Indirizzo di posta elettronica dell'utente che ha creato il lab. 
    4. Numero massimo di utenti consentiti nel lab. 
    5. Stato del lab. 

## <a name="delete-a-lab-in-the-lab-account"></a>Eliminare un lab nell'account lab
Per visualizzare un elenco dei lab nell'account lab, seguire le istruzioni nella sezione precedente.

1. Selezionare **...** (puntini di sospensione) e quindi **Elimina**. 

    ![Pulsante Elimina un lab](../media/how-to-manage-lab-accounts/delete-lab-button.png)
2. Selezionare **Sì** nel messaggio di avviso. 

    ![Conferma dell'eliminazione del lab](../media/how-to-manage-lab-accounts/confirm-lab-delete.png)

## <a name="delete-a-lab-account"></a>Eliminare un account lab
Seguire le istruzioni della sezione precedente che visualizza gli account lab in un elenco. Per eliminare un account lab seguire questa procedura: 

1. Selezionare l'**account lab** da eliminare. 
2. Selezionare **Elimina** dalla barra degli strumenti. 

    ![Account Lab -> Pulsante Elimina](../media/how-to-manage-lab-accounts/delete-button.png)
1. Digitare **Sì** per confermare.
1. Selezionare **Elimina**. 

    ![Eliminazione dell'account lab - Conferma](../media/how-to-manage-lab-accounts/delete-lab-account-confirmation.png)

> [!NOTE]
> È anche possibile usare il modulo di PowerShell AZ. LabServices (anteprima) per gestire gli account Lab. Per ulteriori informazioni, vedere [AZ. LabServices Home page su GitHub](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Modules/Library).

## <a name="next-steps"></a>Passaggi successivi
Vedere l'articolo seguente: [come configurare gli account Lab](how-to-configure-lab-accounts.md).