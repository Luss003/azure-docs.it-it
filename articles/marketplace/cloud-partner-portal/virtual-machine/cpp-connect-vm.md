---
title: Connettersi alla macchina virtuale basata su Azure Microsoft | Azure Marketplace
description: L'articolo descrive come connettersi alla nuova macchina virtuale creata in Azure.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: article
ms.date: 10/19/2018
ms.author: pabutler
ms.openlocfilehash: ca002a77fd7252466f604b995af949b54694bc8c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "64938589"
---
# <a name="connect-to-your-azure-based-virtual-machine"></a>Connettersi alla macchina virtuale basata su Azure

Questo articolo illustra come connettersi ed eseguire l'accesso alle macchine virtuali create in Azure.  Dopo aver eseguito correttamente la connessione, è possibile usare la macchina virtuale come se si fosse connessi in locale al server host. 

## <a name="connect-to-a-windows-based-vm"></a>Connettersi a una macchina virtuale basata su Windows

Verrà usato il client Desktop remoto per connettersi alla macchina virtuale basata su Windows ospitata in Azure.  La maggior parte delle versioni di Windows in modalità nativa dispone del supporto per il protocollo RDP (Remote Desktop Protocol).  Per altre macchine, altre informazioni sui client sono disponibili in [Client Desktop remoto](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients).  

L'articolo seguente illustra nel dettaglio come usare il supporto predefinito Windows RDP per connettersi alla VM: [Come connettersi e accedere a una macchina virtuale di Azure che esegue Windows](../../../virtual-machines/windows/connect-logon.md).  

>[!TIP]
> È possibile ricevere avvisi di sicurezza durante il processo, che indicano ad esempio che il file con estensione rdp proviene da un autore sconosciuto o che le credenziali utente non possono essere verificate.  È opportuno ignorare questi avvisi.


## <a name="connect-to-a-linux-based-vm"></a>Connettersi a una macchina virtuale basata su Linux

Per connettersi a una macchina virtuale basata su Linux, è necessario disporre di un client SSH (Secure Shell Protocol).  In questa discussione verrà usato il terminale SHH [PuTTY](https://www.ssh.com/ssh/putty/) gratuito.

1. Nel pannello **Macchine virtuali** del [portale di Azure](https://ms.portal.azure.com) selezionare la macchina virtuale a cui si intende accedere.  
2. **Avviare** la macchina virtuale se è già in esecuzione.
3. Fare clic sul nome della macchina virtuale da aprire nella pagina **Panoramica**.
4. Annotare l'indirizzo IP pubblico e il nome DNS della macchina virtuale.  Se questi valori non sono impostati, è necessario [creare un'interfaccia di rete](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface#create-a-network-interface).

   ![Impostazioni di panoramica della macchina virtuale](./media/publishvm_019.png)
 
5. Aprire l'applicazione PuTTY.  
6. Nella finestra di dialogo per la configurazione di PuTTY immettere l'indirizzo IP e il nome DNS della macchina virtuale. 

   ![Impostazioni di terminale PuTTY](./media/publishvm_020.png)
 
7. Fare clic su **Apri** per aprire un terminale PuTTY.  
8. Quando richiesto, immettere il nome account e la password dell'account di macchina virtuale di Linux. 

   Se si sono verificati problemi di connessione, vedere la documentazione per il client SSH, ad esempio [capitolo 10: Messaggi di errore comuni](https://www.ssh.com/ssh/putty/putty-manuals/0.68/Chapter10.html#errors).

Per altre informazioni, ad esempio sul modo in cui aggiungere un desktop a una macchina virtuale Linux di cui è stato eseguito il provisioning, vedere [Installare e configurare Desktop remoto per connettersi a una macchina virtuale Linux di Azure](../../../virtual-machines/linux/use-remote-desktop.md).


## <a name="stop-unused-vms"></a>Arrestare macchine virtuali non usate
Azure emette le fatture per l'hosting di macchine virtuali quando una macchina virtuale è in esecuzione o *inattiva*.  È consigliabile pertanto arrestare le macchine virtuali attualmente non in uso.  Le macchine virtuali di test e di backup e ritirate sono candidate per l'arresto. Per arrestare una macchina virtuale, eseguire queste passaggi:

1. Nel pannello **Macchine virtuali** selezionare la macchina virtuale da arrestare. 
2. Nella barra degli strumenti accanto alla parte superiore della pagina fare clic sul pulsante **Arresta**.

   ![Arrestare una macchina virtuale](./media/publishvm_018.png)

Azure arresta rapidamente la macchina virtuale in un processo denominato *deallocazione*, che non solo arresta il sistema operativo nella macchina virtuale, ma consente anche di liberare le risorse hardware e di rete di cui è stato eseguito il provisioning in precedenza.

Se si intende riattivare in seguito una macchina virtuale arrestata, selezionarla e quindi fare clic sul pulsante **Avvia**.


## <a name="next-steps"></a>Passaggi successivi

Dopo aver eseguito la connessione in modalità remota, è possibile [configurare la macchina virtuale](./cpp-configure-vm.md).
