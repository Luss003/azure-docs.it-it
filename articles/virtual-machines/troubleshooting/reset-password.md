---
title: Come reimpostare la password di Linux locale nelle VM di Azure | Microsoft Docs
description: Viene illustrata la procedura per reimpostare la password di Linux locale in una VM di Azure
services: virtual-machines-linux
documentationcenter: ''
author: Deland-Han
manager: dcscontentpm
editor: ''
tags: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: troubleshooting
ms.date: 08/20/2019
ms.author: delhan
ms.openlocfilehash: 83751538efe4f3d3af5928caa04b265b6c867442
ms.sourcegitcommit: 116bc6a75e501b7bba85e750b336f2af4ad29f5a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2019
ms.locfileid: "71153563"
---
# <a name="how-to-reset-local-linux-password-on-azure-vms"></a>Come reimpostare la password di Linux locale nelle VM di Azure

Questo articolo illustra diversi metodi per reimpostare le password delle macchine virtuali (VM) di Linux locali. Se l'account utente è scaduto o si vuole creare un nuovo account, è possibile usare i metodi seguenti per creare un nuovo account amministratore locale e riottenere l'accesso alla VM.

## <a name="symptoms"></a>Sintomi

Non è possibile accedere alla VM e viene visualizzato un messaggio indicante che la password usata non è corretta. Inoltre non è possibile usare VMAgent per reimpostare la password nel portale di Azure.

## <a name="manual-password-reset-procedure"></a>Procedura di reimpostazione manuale della password

> [!NOTE]
> I passaggi seguenti non si applicano alla macchina virtuale con disco non gestito.

1. Creare uno snapshot per il disco del sistema operativo della macchina virtuale interessata, creare un disco dallo snapshot e quindi alporre il disco a una VM di risoluzione dei problemi. Per altre informazioni, vedere [risolvere i problemi di una VM Windows connettendo il disco del sistema operativo a una VM di ripristino usando il portale di Azure](troubleshoot-recovery-disks-portal-linux.md).

2. Connettersi alla macchina virtuale per la risoluzione dei problemi usando Desktop remoto.

3.  Eseguire il comando SSH seguente nella macchina virtuale per la risoluzione dei problemi per diventare un utente con privilegi avanzati.

    ```bash
    sudo su
    ```

4.  Eseguire **fdisk -l** o esaminare i log di sistema per trovare il nuovo disco collegato. Trovare il nome dell'unità da montare, quindi nella VM temporale esaminare il file di log pertinente.

    ```bash
    grep SCSI /var/log/kern.log (ubuntu)
    grep SCSI /var/log/messages (centos, suse, oracle)
    ```

    Il seguente è l'output di esempio del comando grep:

    ```bash
    kernel: [ 9707.100572] sd 3:0:0:0: [sdc] Attached SCSI disk
    ```

5.  Creare un punto di montaggio denominato **tempmount**.

    ```bash
    mkdir /tempmount
    ```

6.  Montare il disco del sistema operativo nel punto di montaggio. È in genere necessario montare *sdc1* o *sdc2*, a seconda della partizione di hosting nella directory */etc* del disco del computer danneggiato.

    ```bash
    mount /dev/sdc1 /tempmount
    ```

7.  Creare copie dei file di credenziali principali prima di apportare qualsiasi modifica:

    ```bash
    cp /etc/passwd /etc/passwd_orig    
    cp /etc/shadow /etc/shadow_orig    
    cp /tempmount/etc/passwd /etc/passwd
    cp /tempmount/etc/shadow /etc/shadow 
    cp /tempmount/etc/passwd /tempmount/etc/passwd_orig
    cp /tempmount/etc/shadow /tempmount/etc/shadow_orig
    ```

8.  Reimpostare la password dell'utente necessaria:

    ```bash
    passwd <<USER>> 
    ```

9.  Spostare i file modificati nella posizione corretta sul disco del computer danneggiato.

    ```bash
    cp /etc/passwd /tempmount/etc/passwd
    cp /etc/shadow /tempmount/etc/shadow
    cp /etc/passwd_orig /etc/passwd
    cp /etc/shadow_orig /etc/shadow
    ```

10. Tornare alla radice e smontare il disco.

    ```bash
    cd /
    umount /tempmount
    ```

11. In portale di Azure scollegare il disco dalla macchina virtuale per la risoluzione dei problemi.

12. [Modificare il disco del sistema operativo per la macchina virtuale interessata](troubleshoot-recovery-disks-portal-linux.md#swap-the-os-disk-for-the-vm).

## <a name="next-steps"></a>Passaggi successivi

* [Risolvere i problemi della macchina virtuale di Azure collegando il disco del sistema operativo a un'altra macchina virtuale di Azure](https://social.technet.microsoft.com/wiki/contents/articles/18710.troubleshoot-azure-vm-by-attaching-os-disk-to-another-azure-vm.aspx)

* [INTERFACCIA della riga di comando di Azure: Come eliminare e ridistribuire una macchina virtuale da un disco rigido virtuale](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/azure-cli-how-to-delete-and-re-deploy-a-vm-from-vhd/)
