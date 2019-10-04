---
title: Risoluzione dei problemi - Crittografia dischi di Azure per le macchine virtuali IaaS | Microsoft Docs
description: Questo articolo offre suggerimenti per la risoluzione dei problemi di Crittografia dischi di Microsoft Azure per VM IaaS Windows e Linux.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/12/2019
ms.custom: seodec18
ms.openlocfilehash: e2464332727b0ef1e616c04a975df5ac475a7b19
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/29/2019
ms.locfileid: "68610278"
---
# <a name="azure-disk-encryption-troubleshooting-guide"></a>Guida alla risoluzione dei problemi di Crittografia dischi di Azure

Questa guida è destinata ai professionisti IT, agli analisti della sicurezza delle informazioni e agli amministratori cloud le cui organizzazioni usano Crittografia dischi di Azure. Questo articolo contiene indicazioni per la risoluzione dei problemi relativi alla crittografia dei dischi.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="troubleshooting-linux-os-disk-encryption"></a>Risoluzione dei problemi relativi alla crittografia del disco del sistema operativo Linux

Per la crittografia del disco del sistema operativo Linux, prima di affrontare l'intero processo è necessario smontare l'unità del sistema operativo. Se non è possibile smontare l'unità, è probabile che si verifichi un errore del tipo "Impossibile smontare dopo...".

Questo errore può verificarsi quando viene eseguito un tentativo di crittografia del disco del sistema operativo in un ambiente di macchina virtuale di destinazione modificato dall'immagine della raccolta supportata. Eventuali deviazioni dall'immagine supportata possono impedire all'estensione di smontare l'unità del sistema operativo. Di seguito sono riportati alcuni esempi di deviazioni:
- Immagini personalizzate che non corrispondono più al file system o allo schema di partizionamento supportato.
- Le applicazioni di grandi dimensioni come SAP, MongoDB, Apache Cassandra e Docker non sono supportate quando sono installate e in esecuzione nel sistema operativo prima della crittografia. Crittografia dischi di Azure non è in grado di arrestare questi processi in modo sicuro come necessario durante la preparazione dell'unità del sistema operativo per la crittografia dei dischi. Se sono presenti processi ancora attivi con handle di file aperti nell'unità del sistema operativo, tale unità non può essere smontata e ciò genera un errore di crittografia dell'unità del sistema operativo. 
- Esecuzione di script personalizzati a breve distanza di tempo dall'abilitazione della crittografia o esecuzione di qualsiasi altra modifica nella VM durante il processo di crittografia. Questo conflitto può verificarsi quando un modello di Azure Resource Manager definisce l'esecuzione simultanea di più estensioni o quando un'estensione di script personalizzati o un'altra azione viene eseguita contemporaneamente alla crittografia del disco. Il problema potrebbe essere risolto serializzando e isolando tali passaggi.
- Security Enhanced Linux (SELinux) non è stato disabilitato prima dell'abilitazione della crittografia, con conseguente esito negativo dello smontaggio. È possibile riabilitare SELinux al termine della crittografia.
- Il disco del sistema operativo usa uno schema di gestione dei volumi logici. Anche se è disponibile un supporto limitato per dischi dati LVM, non sono supportati dischi del sistema operativo LVM.
- I requisiti minimi di memoria non sono soddisfatti. Per la crittografia del disco del sistema operativo si consigliano 7 GB.
- Montaggio ricorsivo delle unità dati nella directory /mnt/ o l'una nell'altra (ad esempio, /mnt/data1, /mnt/data2, /data3 + /data3/data4).
- Altri [prerequisiti](azure-security-disk-encryption-prerequisites.md) di Crittografia dischi di Azure per Linux non sono soddisfatti.

## <a name="bkmk_Ubuntu14"></a> Aggiornare il kernel predefinito per Ubuntu 14.04 LTS

L'immagine di Ubuntu 14.04 LTS viene fornita con la versione del kernel predefinito 4.4. Questa versione del kernel presenta un problema noto che consiste in un errore di memoria insufficiente killer, che termina in modo errato il comando dd durante il processo di crittografia del sistema operativo. Questo bug è stato risolto nel più recente kernel Linux ottimizzato di Azure. Per evitare tale errore, prima di abilitare la crittografia sull'immagine, aggiornare il [kernel ottimizzato 4.15 di Azure](https://packages.ubuntu.com/trusty/linux-azure) oppure usare i comandi seguenti in un secondo momento:

```
sudo apt-get update
sudo apt-get install linux-azure
sudo reboot
```

Dopo aver riavviato la macchina virtuale nel nuovo kernel, la nuova versione del kernel può essere confermata tramite:

```
uname -a
```

## <a name="update-the-azure-virtual-machine-agent-and-extension-versions"></a>Aggiornare l'agente di macchine virtuali di Azure e le versioni dell'estensione

Le operazioni di crittografia dischi di Azure possono avere esito negativo sulle immagini di macchine virtuali usando versioni non supportate dell'agente di macchine virtuali di Azure. Prima di abilitare la crittografia, è necessario aggiornare le immagini Linux con le versioni di Agent precedenti a 2.2.38. Per altre informazioni, vedere [come aggiornare l'agente Linux di Azure in una](../virtual-machines/extensions/update-linux-agent.md) macchina virtuale e il [supporto della versione minima per gli agenti di macchine virtuali in Azure](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support).

È necessaria anche la versione corretta dell'estensione agente guest Microsoft. Azure. Security. AzureDiskEncryption o Microsoft. Azure. Security. AzureDiskEncryptionForLinux. Le versioni delle estensioni vengono gestite e aggiornate automaticamente dalla piattaforma quando sono soddisfatti i prerequisiti dell'agente di macchine virtuali di Azure e viene usata una versione supportata dell'agente di macchine virtuali.

L'estensione Microsoft. OSTCExtensions. AzureDiskEncryptionForLinux è stata deprecata e non è più supportata.  

## <a name="unable-to-encrypt-linux-disks"></a>Impossibile crittografare i dischi Linux

In alcuni casi, la crittografia del disco Linux sembra bloccata nella fase "OS disk encryption started" e SSH è disabilitato. Il processo di crittografia in un'immagine di galleria della raccolta può richiedere da 3 a 16 ore. Se vengono aggiunti dischi dati di dimensioni da più TB, il processo può richiedere giorni.

La sequenza di crittografia del disco del sistema operativo Linux smonta temporaneamente l'unità del sistema operativo, per poi eseguire la crittografia blocco per blocco dell'intero disco del sistema operativo e riportarlo allo stato crittografato. A differenza di Crittografia dischi di Azure in Windows, la crittografia dei dischi Linux non consente l'uso simultaneo della macchina virtuale mentre è in corso la crittografia. Le caratteristiche delle prestazioni della macchina virtuale possono determinare una differenza significativa nel tempo necessario per completare la crittografia. Tali caratteristiche includono le dimensioni del disco e l'archiviazione standard o premium (SSD) dell'account di archiviazione.

Per controllare lo stato della crittografia, eseguire il polling del campo **ProgressMessage** restituito dal comando [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) . Mentre viene eseguita la crittografia dell'unità del sistema operativo, la macchina virtuale passa in stato di manutenzione e SSH viene disabilitato per impedire interruzioni del processo in corso. Mentre la crittografia è in corso, di norma viene visualizzato il messaggio **EncryptionInProgress**. Diverse ore più tardi, il messaggio **VMRestartPending** chiederà di riavviare la macchina virtuale. Ad esempio:


```azurepowershell
PS > Get-AzVMDiskEncryptionStatus -ResourceGroupName "MyVirtualMachineResourceGroup" -VMName "VirtualMachineName"
OsVolumeEncrypted          : EncryptionInProgress
DataVolumesEncrypted       : EncryptionInProgress
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk encryption started

PS > Get-AzVMDiskEncryptionStatus -ResourceGroupName "MyVirtualMachineResourceGroup" -VMName "VirtualMachineName"
OsVolumeEncrypted          : VMRestartPending
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk successfully encrypted, please reboot the VM
```

Dopo la richiesta di riavviare la macchina virtuale e il riavvio della stessa, è necessario attendere 2-3 minuti perché nella destinazione siano completati il riavvio e i passaggi finali. Una volta completata la crittografia, il messaggio di stato cambia. Quando viene visualizzato tale messaggio, l'unità del sistema operativo crittografata sarà pronta per l'uso e la macchina virtuale sarà nuovamente utilizzabile.

Nei casi seguenti, è consigliabile ripristinare la macchina virtuale alla copia shadow o al backup eseguito immediatamente prima della crittografia:
   - Se la sequenza di riavvio descritta in precedenza non viene completata.
   - Se le informazioni di riavvio, il messaggio di stato di avanzamento o altri indicatori di errore indicano che la crittografia del sistema operativo all'interno di questo processo non è riuscita. Un esempio di messaggio è l'errore "Impossibile smontare" descritto in questa Guida.

Prima del tentativo successivo, valutare nuovamente le caratteristiche della macchina virtuale e verificare che siano soddisfatti tutti i prerequisiti.

## <a name="troubleshooting-azure-disk-encryption-behind-a-firewall"></a>Risoluzione dei problemi di Crittografia dischi di Azure dietro un firewall
Quando la connettività è limitata da un firewall, da un requisito del proxy o dalle impostazioni di gruppi di sicurezza di rete (NSG), l'estensione potrebbe non essere più in grado di eseguire le attività necessarie. È quindi possibile che vengano visualizzati messaggi del tipo "Stato estensione non disponibile nella macchina virtuale". In scenari previsti, la crittografia non viene completata. Le sezioni che seguono illustrano alcuni problemi comuni relativi ai firewall che è possibile esaminare.

### <a name="network-security-groups"></a>Gruppi di sicurezza di rete
Tutte le impostazioni dei gruppi di sicurezza di rete devono consentire all'endpoint di soddisfare i [prerequisiti](azure-security-disk-encryption-prerequisites.md#bkmk_GPO) di configurazione di rete documentati per la crittografia dei dischi.

### <a name="azure-key-vault-behind-a-firewall"></a>Azure Key Vault protetto da firewall

Quando la crittografia è abilitata con le [credenziali di Azure AD](azure-security-disk-encryption-prerequisites-aad.md), la macchina virtuale di destinazione deve consentire la connettività sia sugli endpoint di Azure Active Directory sia sugli endpoint di Key Vault. Gli endpoint di autenticazione di Azure Active Directory correnti sono mantenuti nelle sezioni 56 e 59 della documentazione [URL e intervalli di indirizzi IP per Office 365](https://docs.microsoft.com/office365/enterprise/urls-and-ip-address-ranges). Le istruzioni di Key Vault sono fornite nella documentazione [Accedere ad Azure Key Vault protetto da firewall](../key-vault/key-vault-access-behind-firewall.md).

### <a name="azure-instance-metadata-service"></a>Servizio metadati dell'istanza di Azure 
La macchina virtuale deve essere in grado di accedere all'endpoint del [servizio metadati dell'istanza di Azure](../virtual-machines/windows/instance-metadata-service.md) che usa un indirizzo IP noto e non instradabile (`169.254.169.254`) accessibile solo dall'interno della macchina virtuale.  Le configurazioni proxy che modificano il traffico HTTP locale a questo indirizzo, ad esempio l'aggiunta di un'intestazione X-inoltro-for, non sono supportate.

### <a name="linux-package-management-behind-a-firewall"></a>Gestione pacchetti Linux protetto da firewall

In fase di esecuzione, Crittografia dischi di Azure per Linux usa il sistema di gestione pacchetti della distribuzione di destinazione per installare i componenti prerequisiti prima di abilitare la crittografia. Se le impostazioni del firewall impediscono alla VM di scaricare e installare tali componenti, si verificheranno errori successivi. I passaggi per configurare questo sistema di gestione pacchetti possono variare in base alla distribuzione. In Red Hat, quando è necessario un proxy, è essenziale verificare che subscription-manager e yum siano configurati correttamente. Per altre informazioni, vedere [Risoluzione dei problemi relativi a subscription-manager e yum](https://access.redhat.com/solutions/189533).  


## <a name="troubleshooting-windows-server-2016-server-core"></a>Risoluzione dei problemi di Server Core di Windows Server 2016

In Server Core di Windows Server 2016, la componente bdehdcfg non è disponibile per impostazione predefinita. Questa componente è necessaria per la Crittografia dischi di Azure. e viene usata per dividere il volume di sistema dal volume del sistema operativo, operazione eseguita solo una volta per la durata della macchina virtuale. Tali file binari non sono necessari durante le operazioni di crittografia successive.

Per risolvere il problema, copiare i quattro file seguenti da una macchina virtuale Windows Server 2016 Data Center nello stesso percorso in Server Core:

   ```
   \windows\system32\bdehdcfg.exe
   \windows\system32\bdehdcfglib.dll
   \windows\system32\en-US\bdehdcfglib.dll.mui
   \windows\system32\en-US\bdehdcfg.exe.mui
   ```

1. Immettere il comando seguente:

   ```
   bdehdcfg.exe -target default
   ```

1. Questo comando crea una partizione di sistema da 550 MB. Riavviare il sistema.

1. Usare DiskPart per verificare i volumi, quindi procedere.  

Ad esempio:

```
DISKPART> list vol

  Volume ###  Ltr  Label        Fs     Type        Size     Status     Info
  ----------  ---  -----------  -----  ----------  -------  ---------  --------
  Volume 0     C                NTFS   Partition    126 GB  Healthy    Boot
  Volume 1                      NTFS   Partition    550 MB  Healthy    System
  Volume 2     D   Temporary S  NTFS   Partition     13 GB  Healthy    Pagefile
```
<!-- ## Troubleshooting encryption status

If the expected encryption state does not match what is being reported in the portal, see the following support article:
[Encryption status is displayed incorrectly on the Azure Management Portal](https://support.microsoft.com/en-us/help/4058377/encryption-status-is-displayed-incorrectly-on-the-azure-management-por) --> 

## <a name="troubleshooting-encryption-status"></a>Risoluzione dei problemi relativi allo stato della crittografia 

Il portale può visualizzare un disco come crittografato anche dopo che è stato decrittografato all'interno della macchina virtuale.  Questo problema può verificarsi quando si usano comandi di basso livello per decrittografare direttamente il disco dall'interno della VM, anziché usare i comandi di gestione di crittografia dischi di Azure di livello superiore.  I comandi di livello superiore non solo decrittografano il disco dalla macchina virtuale, ma all'esterno della macchina virtuale aggiornano anche le impostazioni di crittografia e le impostazioni di estensione di livello piattaforma associate alla VM.  Se questi non vengono mantenuti nell'allineamento, la piattaforma non sarà in grado di segnalare lo stato della crittografia o di effettuare il provisioning della macchina virtuale correttamente.   

Per disabilitare crittografia dischi di Azure con PowerShell, usare [Disable-AzVMDiskEncryption](/powershell/module/az.compute/disable-azvmdiskencryption) seguito da [Remove-AzVMDiskEncryptionExtension](/powershell/module/az.compute/remove-azvmdiskencryptionextension). L'esecuzione di Remove-AzVMDiskEncryptionExtension prima della disabilitazione della crittografia avrà esito negativo.

Per disabilitare crittografia dischi di Azure con l'interfaccia della riga di comando, usare il comando [AZ VM Encryption Disable](/cli/azure/vm/encryption). 

## <a name="next-steps"></a>Passaggi successivi

In questo documento sono stati esaminati alcuni problemi comuni di Crittografia dischi di Azure ed è stato illustrato come risolverli. Per altre informazioni su questo servizio e sulle relative funzionalità, vedere gli articoli seguenti:

- [Applicare la crittografia dei dischi nel Centro sicurezza di Azure](../security-center/security-center-apply-disk-encryption.md)
- [Crittografia dei dati inattivi in Azure](fundamentals/encryption-atrest.md)
