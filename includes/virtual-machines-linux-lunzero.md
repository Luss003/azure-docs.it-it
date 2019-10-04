---
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 87dd3680aae3e87f78ab2dbe70c44b2008706747
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/18/2019
ms.locfileid: "67180168"
---
Quando si aggiungono dischi dati a una VM Linux, potrebbero verificarsi errori in caso di disco inesistente nel LUN 0. Se si aggiunge un disco manualmente usando il comando `azure vm disk attach-new` e si specifica un LUN (`--lun`) anziché consentire alla piattaforma Azure di determinare il LUN appropriato, fare attenzione che nel LUN 0 sia/sarà già presente un disco. 

L'esempio seguente mostra un frammento di codice dell'output di `lsscsi`:

```bash
[5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc 
[5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd 
```

I due dischi dati sono disponibili nel LUN 0 e nel LUN 1 (la prima colonna dell'output `lsscsi` indica `[host:channel:target:lun]`). Entrambi i dischi devono essere accessibili dalla macchina virtuale. Se è stata specificata manualmente l'aggiunta del primo e del secondo disco al LUN 1 e al LUN 2 rispettivamente, è possibile che i dischi non siano visualizzati correttamente all'interno della VM.

> [!NOTE]
> In questi esempi il valore `host` di Azure è 5, ma può variare a seconda del tipo di archiviazione selezionato.
> 
> 

Questo comportamento del disco non è un problema di Azure, ma dipende dal modo in cui il kernel Linux segue le specifiche SCSI. Quando il kernel Linux analizza il bus SCSI per individuare i dispositivi collegati, affinché il sistema continui ad analizzare la presenza di altri dispositivi è necessario che ne rilevi uno nel LUN 0. Di conseguenza:

* Dopo aver aggiunto un disco dati, esaminare l'output di `lsscsi` per verificare la presenza di un disco nel LUN 0.
* Se il disco non viene visualizzato correttamente nella VM, verificare che sia presente un disco nel LUN 0.

