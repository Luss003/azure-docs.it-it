---
title: Abilitare Desktop remoto per Linux in Azure Lab Services | Microsoft Docs
description: Informazioni su come abilitare Desktop remoto per le macchine virtuali Linux in un Lab in Azure Lab Services.
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
ms.date: 08/20/2019
ms.author: spelluru
ms.openlocfilehash: c67ca111bf87c9dbfa69c93149d29dbd32767fbd
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71350763"
---
# <a name="enable-remote-desktop-for-linux-virtual-machines-in-a-lab-in-azure-lab-services"></a>Abilitare Desktop remoto per le macchine virtuali Linux in un Lab in Azure Lab Services
Questo articolo illustra come eseguire le attività seguenti:

- Abilitare Desktop remoto per la macchina virtuale Linux
- Il modo in cui l'insegnante può connettersi alla macchina virtuale modello tramite Connessione Desktop remoto (RDP).

## <a name="enable-remote-desktop-for-linux-vm"></a>Abilitare Desktop remoto per la macchina virtuale Linux
Durante la creazione del Lab, gli insegnanti possono abilitare la **Connessione desktop remoto** per le immagini **Linux** . L'opzione **abilita connessione Desktop remoto** viene visualizzata quando si seleziona un'immagine Linux per il modello. Quando questa opzione è abilitata, gli insegnanti possono connettersi alle VM del modello e alle VM degli studenti tramite RDP (Desktop remoto). 

![Abilitare la connessione Desktop remoto per un'immagine Linux](../media/how-to-enable-remote-desktop-linux/enable-rdp-option.png)

Nella finestra di messaggio **Abilitazione di connessione Desktop remoto** selezionare **continua con desktop remoto**. 

![Abilitare la connessione Desktop remoto per un'immagine Linux](../media/how-to-enable-remote-desktop-linux/enabling-remote-desktop-connection-dialog.png)

> [!IMPORTANT] 
> L'abilitazione di **Connessione desktop remoto** apre solo la porta **RDP** nei computer Linux. Se RDP è già installato e configurato nell'immagine della macchina virtuale, ad esempio: Ubuntu Data Science Virtual Machine Image), gli studenti possono connettersi alle macchine virtuali tramite RDP senza seguire ulteriori passaggi.
> 
> Se l'immagine di macchina virtuale non dispone di RDP installato e configurato, è necessario connettersi al computer Linux usando SSH per la prima volta e installare i pacchetti RDP e GUI in modo che gli studenti possano connettersi al computer Linux usando RDP in un secondo momento. Per altre informazioni, vedere [installare e configurare Desktop remoto per connettersi a una macchina virtuale Linux in Azure](../../virtual-machines/linux/use-remote-desktop.md). Quindi, si pubblica l'immagine in modo che gli studenti possano usare il protocollo RDP per le macchine virtuali Linux per studenti. 

## <a name="supported-operating-systems"></a>Sistemi operativi supportati
Attualmente, la connessione Desktop remoto è supportata per i sistemi operativi seguenti:

- openSUSE Leap 42.3
- Basato su CentOS 7,5
- Debian 9 "Stretch"
- Ubuntu Server 16.04 LTS

## <a name="teachers-connecting-to-the-template-vm-using-rdp"></a>Insegnanti che si connettono alla VM modello usando RDP
Gli insegnanti devono connettersi prima alla macchina virtuale del modello usando SSH e installare i pacchetti RDP e GUI. Quindi, gli insegnanti possono usare la procedura seguente per connettersi alle macchine virtuali Linux tramite RDP: 

Viene visualizzata l'opzione **Desktop remoto** per connettersi alla macchina virtuale del modello al momento della creazione del Lab. 

![Connettersi al modello tramite RDP al momento della creazione](../media/how-to-enable-remote-desktop-linux/connect-at-creation.png)

Viene visualizzata l'opzione **Desktop remoto** sul Home page del Lab dopo che il Lab è stato creato e la macchina virtuale del modello è stata avviata. Avviare la macchina virtuale del modello, se non è già stata avviata. 

![Connettersi al modello tramite RDP dopo la creazione del Lab](../media/how-to-enable-remote-desktop-linux/rdp-after-lab-creation.png) 

Per ulteriori informazioni sulla connessione alla macchina virtuale tramite SSH o RDP, vedere la pagina relativa alla connessione tramite SSH o RDP ((#connect-using-SSH-o-RDP). 

## <a name="teachers-connecting-to-a-student-vm-using-rdp"></a>Insegnanti che si connettono a una macchina virtuale per studenti tramite RDP
Un insegnante/professore può connettersi a una macchina virtuale per studenti passando alla visualizzazione **macchine virtuali** e selezionando l'icona **Connetti** . Prima di questo, gli insegnanti devono **pubblicare** l'immagine modello con i pacchetti RDP e GUI installati. 

![Insegnanti che si connettono alla macchina virtuale per studenti](../media/how-to-enable-remote-desktop-linux/teacher-connect-to-student-vm.png)

Per ulteriori informazioni sulla connessione alla macchina virtuale tramite SSH o RDP, vedere la pagina relativa alla connessione tramite SSH o RDP ((#connect-using-SSH-o-RDP). 

## <a name="connect-using-ssh-or-rdp"></a>Connettersi tramite SSH o RDP
Se si seleziona l'opzione **SSH** , viene visualizzata la finestra **di dialogo Connetti alla macchina virtuale** seguente:  

![Stringa di connessione SSH](../media/how-to-enable-remote-desktop-linux/ssh-connection-string.png)

Selezionare il pulsante **copia** accanto alla casella di testo per copiarlo negli Appunti. Salvare la stringa di connessione SSH. Usare questa stringa di connessione da un terminale SSH, ad esempio [Putty](https://www.putty.org/), per connettersi alla macchina virtuale.

Se si seleziona l'opzione **RDP** , nel computer viene scaricato un file RDP. Salvarlo e aprirlo per connettersi al computer. 

## <a name="next-steps"></a>Passaggi successivi
Dopo che un insegnante ha abilitato la funzionalità di connessione Desktop remoto, gli studenti possono connettersi alle macchine virtuali tramite RDP/SSH. Per altre informazioni, vedere [usare desktop remoto per macchine virtuali Linux in un Lab della classe](how-to-use-remote-desktop-linux-student.md). 