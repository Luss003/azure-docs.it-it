---
title: Domande frequenti - Crittografia dischi di Azure per le macchine virtuali IaaS | Microsoft Docs
description: Questo articolo fornisce le risposte alle domande frequenti su Crittografia dischi di Microsoft Azure per le macchine virtuali IaaS Windows e Linux.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 06/05/2019
ms.custom: seodec18
ms.openlocfilehash: 4f2a34e63a870814c8d2a3ffe24c60083c9d7bb2
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/05/2019
ms.locfileid: "68781097"
---
# <a name="azure-disk-encryption-for-iaas-vms-faq"></a>Domande frequenti su Crittografia dischi di Azure per macchine virtuali IaaS

Questo articolo fornisce le risposte alle domande frequenti (FAQ) su Crittografia dischi di Azure per le macchine virtuali IaaS Windows e Linux. Per altre informazioni su questo servizio, vedere [Crittografia dischi di Azure per macchine virtuali IaaS Windows e Linux](azure-security-disk-encryption-overview.md).

## <a name="where-is-azure-disk-encryption-in-general-availability-ga"></a>Dove è presente Crittografia dischi di Azure con disponibilità generale (GA)?

Crittografia dischi di Azure per VM IaaS Windows e Linux è presente con disponibilità generale in tutte le aree pubbliche di Azure.

## <a name="what-user-experiences-are-available-with-azure-disk-encryption"></a>Quali esperienze utente sono disponibili con Crittografia dischi di Azure?

La versione GA di Crittografia dischi di Azure supporta i modelli di Azure Resource Manager, Azure PowerShell e l'interfaccia della riga di comando di Azure. La presenza di diverse esperienze utente consente una maggiore flessibilità. Sono disponibili tre diverse opzioni per l'abilitazione della crittografia del disco per le macchine virtuali IaaS. Per altre informazioni sull'esperienza utente e sulle indicazioni dettagliate disponibili in Crittografia dischi di Azure, vedere [Abilitare Crittografia dischi di Azure per Windows](azure-security-disk-encryption-windows.md) e [Enable Azure Disk Encryption for Linux](azure-security-disk-encryption-linux.md) (Abilitare Crittografia dischi di Azure per Linux).

## <a name="how-much-does-azure-disk-encryption-cost"></a>Quanto costa Crittografia dischi di Azure?

Non è previsto alcun addebito per la crittografia dei dischi delle macchine virtuali con Crittografia dischi di Azure, ma sono previsti addebiti associati all'uso di Azure Key Vault. Per altre informazioni sui costi associati ad Azure Key Vault, vedere la pagina [Prezzi di Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="how-can-i-start-using-azure-disk-encryption"></a>Come si può iniziare a usare Crittografia dischi di Azure?

Per iniziare, leggere l'articolo [Azure Disk Encryption overview](azure-security-disk-encryption-overview.md) (Panoramica di Crittografia dischi di Azure).

## <a name="what-vm-sizes-and-operating-systems-support-azure-disk-encryption"></a>Quali sono le dimensioni e i sistemi operativi delle macchine virtuali che supportano crittografia dischi di Azure?

L'articolo sui [prerequisiti di crittografia dischi di Azure](azure-security-disk-encryption-prerequisites.md) elenca le [dimensioni delle VM](azure-security-disk-encryption-prerequisites.md#supported-vm-sizes) e i [sistemi operativi delle macchine virtuali](azure-security-disk-encryption-prerequisites.md#supported-operating-systems) che supportano crittografia dischi di Azure.

## <a name="can-i-encrypt-both-boot-and-data-volumes-with-azure-disk-encryption"></a>È possibile crittografare sia i volumi di avvio che i volumi di dati con Crittografia dischi di Azure?

Sì, è possibile crittografare volumi di avvio e volumi di dati per le macchine virtuali IaaS Windows e Linux. Per le macchine virtuali Windows, non è possibile crittografare i dati senza prima crittografare il volume del sistema operativo. Per le macchine virtuali Linux, è possibile crittografare il volume dei dati senza dover prima crittografare il volume del sistema operativo. Dopo che è stato crittografato il volume del sistema operativo per Linux, la disabilitazione della crittografia su un volume del sistema operativo per le macchine virtuali IaaS Linux non è supportata. Per le VM Linux in un set di scalabilità, è possibile crittografare solo il volume di dati.

## <a name="can-i-encrypt-an-unmounted-volume-with-azure-disk-encryption"></a>È possibile crittografare un volume smontato con crittografia dischi di Azure?

No, crittografia dischi di Azure Crittografa solo i volumi montati.

## <a name="how-do-i-rotate-secrets-or-encryption-keys"></a>Ricerca per categorie ruotare i segreti o le chiavi di crittografia?

Per ruotare i segreti, è sufficiente chiamare lo stesso comando usato originariamente per abilitare la crittografia del disco, specificando un Key Vault diverso. Per ruotare la chiave di crittografia della chiave, chiamare lo stesso comando usato originariamente per abilitare la crittografia del disco, specificando la nuova crittografia della chiave. 

>[!WARNING]
> - Se in precedenza è stato usato [crittografia dischi di Azure con Azure ad app](azure-security-disk-encryption-prerequisites-aad.md) specificando Azure ad credenziali per crittografare questa macchina virtuale, sarà necessario continuare a usare questa opzione per crittografare la macchina virtuale. Non è possibile usare [Crittografia dischi di Azure](azure-security-disk-encryption-prerequisites.md) in questa macchina virtuale crittografata perché non è uno scenario supportato, ovvero non è ancora supportato il trasferimento dall'applicazione AAD per questa macchina virtuale.

## <a name="how-do-i-add-or-remove-a-key-encryption-key-if-i-didnt-originally-use-one"></a>Ricerca per categorie aggiungere o rimuovere una chiave di crittografia della chiave se non ne è stata originariamente usata una?

Per aggiungere una chiave di crittografia della chiave, chiamare di nuovo il comando di abilitazione passando il parametro della chiave di crittografia della chiave. Per rimuovere una chiave di crittografia della chiave, chiamare di nuovo il comando di abilitazione senza il parametro chiave di crittografia chiave.

## <a name="does-azure-disk-encryption-allow-you-to-bring-your-own-key-byok"></a>Con Crittografia dischi di Azure sono disponibili BYOK (Bring Your Own Key)?

Sì, è possibile fornire le proprie chiavi di crittografia della chiave. Queste chiavi sono protette in Azure Key Vault, l'archivio delle chiavi per Crittografia dischi di Azure. Per altre informazioni sugli scenari di supporto delle chiavi di crittografia delle chiavi, vedere [Azure Disk Encryption prerequisites](azure-security-disk-encryption-prerequisites.md) (Prerequisiti di Crittografia dischi di Azure).

## <a name="can-i-use-an-azure-created-key-encryption-key"></a>È possibile usare una chiave di crittografia della chiave creata in Azure?

Sì, è possibile usare Azure Key Vault per generare una chiave di crittografia della chiave per l'uso con Crittografia dischi di Azure. Queste chiavi sono protette in Azure Key Vault, l'archivio delle chiavi per Crittografia dischi di Azure. Per altre informazioni sulla chiave di crittografia della chiave, vedere [Azure Disk Encryption prerequisites](azure-security-disk-encryption-prerequisites.md) (Prerequisiti di Crittografia dischi di Azure).

## <a name="can-i-use-an-on-premises-key-management-service-or-hsm-to-safeguard-the-encryption-keys"></a>È possibile usare un servizio di gestione delle chiavi/modulo di protezione hardware locale per proteggere le chiavi di crittografia?

Non è possibile usare il servizio di gestione delle chiavi/modulo di protezione hardware locale per proteggere le chiavi di crittografia con Crittografia dischi di Azure. Per proteggere le chiavi di crittografia, è possibile usare solo il servizio Azure Key Vault. Per altre informazioni sugli scenari di supporto della chiave di crittografia della chiave, vedere [Azure Disk Encryption prerequisites](azure-security-disk-encryption-prerequisites.md) (Prerequisiti di Crittografia dischi di Azure).

## <a name="what-are-the-prerequisites-to-configure-azure-disk-encryption"></a>Quali sono i prerequisiti per la configurazione di Crittografia dischi di Azure?

Per usare Crittografia dischi di Azure, è necessario rispettare alcuni prerequisiti. Per creare un nuovo insieme di credenziali delle chiavi o per configurare un insieme di credenziali delle chiavi esistente per l'accesso alla crittografia dei dischi in modo da abilitare la crittografia e proteggere segreti e chiavi, vedere [Prerequisiti di Crittografia dischi di Azure](azure-security-disk-encryption-prerequisites.md). Per altre informazioni sugli scenari di supporto della chiave di crittografia della chiave, vedere [Azure Disk Encryption prerequisites](azure-security-disk-encryption-overview.md) (Panoramica di Crittografia dischi di Azure).

## <a name="what-are-the-prerequisites-to-configure-azure-disk-encryption-with-an-azure-ad-app-previous-release"></a>Quali sono i prerequisiti per la configurazione di Crittografia dischi di Azure con un'app Azure AD (versione precedente)?

Per usare Crittografia dischi di Azure, è necessario rispettare alcuni prerequisiti. Per creare un'applicazione di Azure Active Directory o un nuovo insieme di credenziali delle chiavi o per configurare un insieme di credenziali delle chiavi esistente per l'accesso alla crittografia dischi per abilitare la crittografia e proteggere segreti e chiavi, vedere [Azure Disk Encryption prerequisites](azure-security-disk-encryption-prerequisites-aad.md) (Prerequisiti di Crittografia dischi di Azure). Per altre informazioni sugli scenari di supporto della chiave di crittografia della chiave, vedere [Azure Disk Encryption prerequisites](azure-security-disk-encryption-overview.md) (Panoramica di Crittografia dischi di Azure).

## <a name="is-azure-disk-encryption-using-an-azure-ad-app-previous-release-still-supported"></a>L'esecuzione di Crittografia dischi di Azure con un'app Azure AD (versione precedente) è ancora supportata?
Sì. L'esecuzione di Crittografia dischi con un'app Azure AD è ancora supportata. Tuttavia, quando si esegue la crittografia di nuove macchine virtuali, è consigliabile usare il nuovo metodo anziché un'app Azure AD. 

## <a name="can-i-migrate-vms-that-were-encrypted-with-an-azure-ad-app-to-encryption-without-an-azure-ad-app"></a>È possibile eseguire la migrazione di macchine virtuali crittografate con un'app Azure AD alla crittografia senza tale app?
  Non esiste attualmente un percorso di migrazione diretto per le macchine crittografate con un'app Azure AD alla crittografia senza tale app. Non esiste nemmeno un percorso diretto dalla crittografia senza un'app Azure AD alla crittografia con tale app. 

## <a name="what-version-of-azure-powershell-does-azure-disk-encryption-support"></a>Quale versione di Azure PowerShell è supportata da Crittografia dischi di Azure?

Usare la versione più recente di Azure PowerShell SDK per configurare Crittografia dischi di Azure. Scaricare la versione più recente di [Azure PowerShell](https://github.com/Azure/azure-powershell/releases). Crittografia dischi di Azure *non* è supportato da Azure SDK versione 1.1.0.

> [!NOTE]
> L'estensione di anteprima "Microsoft. OSTCExtension. AzureDiskEncryptionForLinux" di Linux Azure Disk Encryption è deprecata. Questa estensione è stata pubblicata per la versione di anteprima di crittografia dischi di Azure. Non usare la versione di anteprima dell'estensione nella distribuzione di test o produzione.

> Per gli scenari di distribuzione come Azure Resource Manager (ARM), in cui è necessario distribuire l'estensione di crittografia dischi di Azure per la macchina virtuale Linux per abilitare la crittografia nella VM IaaS Linux, è necessario usare l'estensione supportata per la produzione di crittografia dischi di Azure. Microsoft. Azure. Security. AzureDiskEncryptionForLinux ".

## <a name="can-i-apply-azure-disk-encryption-on-my-custom-linux-image"></a>È possibile applicare Crittografia dischi di Azure a un'immagine Linux personalizzata?

Non è possibile applicare Crittografia dischi di Azure a un'immagine Linux personalizzata. Sono consentite solo le immagini Linux per le distribuzioni supportate indicate in precedenza. Le immagini Linux personalizzate non sono attualmente supportate.

## <a name="can-i-apply-updates-to-a-linux-red-hat-vm-that-uses-the-yum-update"></a>È possibile applicare gli aggiornamenti a una VM Red Hat Linux che usa l'aggiornamento Yum?

Sì, è possibile eseguire un aggiornamento yum in una VM Red Hat Linux.  Per altre informazioni, vedere [gestione dei pacchetti Linux dietro a un firewall](azure-security-disk-encryption-tsg.md#linux-package-management-behind-a-firewall).

## <a name="what-is-the-recommended-azure-disk-encryption-workflow-for-linux"></a>Qual è il flusso di lavoro di crittografia dischi di Azure consigliato per Linux?

Di seguito è riportato il flusso di lavoro consigliato per ottenere risultati ottimali in Linux:
* Iniziare dall'immagine della raccolta non modificata corrispondente alla distribuzione e alla versione del sistema operativo desiderate
* Eseguire il backup di tutte le unità montate che verranno crittografate.  In questo modo è possibile eseguire il ripristino in caso di errore, ad esempio se la macchina virtuale viene riavviata prima del completamento della crittografia.
* Eseguire la crittografia (può richiedere più ore o addirittura giorni a seconda delle caratteristiche della macchina virtuale e delle dimensioni di qualsiasi disco dati collegato)
* Personalizzare e aggiungere il software all'immagine in base alle esigenze.

Se questo flusso di lavoro non è possibile, l'uso della [crittografia del servizio di archiviazione](../storage/common/storage-service-encryption.md) a livello di account di archiviazione della piattaforma può essere un'alternativa alla crittografia completa del disco tramite dm-crypt.

## <a name="what-is-the-disk-bek-volume-or-mntazure_bek_disk"></a>Che cos'è il disco "volume BEK" o "/mnt/azure_bek_disk"?

Il disco "volume BEK" per Windows o "/mnt/azure_bek_disk" per Linux è un volume dati locale in cui vengono archiviate in modo sicuro le chiavi di crittografia per le macchine virtuali IaaS di Azure crittografate.
> [!NOTE]
> Non eliminare né modificare il contenuto del disco. Non smontare il disco, perché la presenza delle chiavi di crittografia è necessaria per qualsiasi operazione di crittografia sulla VM IaaS.


## <a name="what-encryption-method-does-azure-disk-encryption-use"></a>Qual è il metodo di crittografia usato da Crittografia dischi di Azure?

In Windows, Crittografia dischi di Azure usa il metodo di crittografia BitLocker AES256 (AES256WithDiffuser nelle versioni precedenti a Windows Server 2012). In Linux, ADE usa la decrittografia predefinita di AES-XTS-plain64 con una chiave master del volume a 256 bit.

## <a name="if-i-use-encryptformatall-and-specify-all-volume-types-will-it-erase-the-data-on-the-data-drives-that-we-already-encrypted"></a>Se si utilizzano EncryptFormatAll e vengono specificati tutti i tipi di volume, verranno cancellati i dati sulle unità dati già crittografati?
No, i dati non verranno cancellati da unità dati che sono già crittografate usando Crittografia dischi di Azure. Analogamente al modo in cui EncryptFormatAll non crittografa nuovamente l'unità del sistema operativo, non riapplica la crittografia all'unità dati già crittografata. Per altre informazioni, vedere [Criteri EncryptFormatAll ](azure-security-disk-encryption-linux.md#bkmk_EFACriteria).        

## <a name="is-xfs-filesystem-supported"></a>Il file system XFS è supportato?
I volumi XFS sono supportati per la crittografia del disco dati solo con EncryptFormatAll. Il volume verrà riformattato, cancellando tutti i dati precedentemente presenti. Per altre informazioni, vedere [Criteri EncryptFormatAll ](azure-security-disk-encryption-linux.md#bkmk_EFACriteria).

## <a name="can-i-backup-and-restore-an-encrypted-vm"></a>È possibile eseguire il backup e il ripristino di una VM crittografata? 

Backup di Azure fornisce un meccanismo per il backup e il ripristino di VM crittografate all'interno della stessa sottoscrizione e area.  Per istruzioni, vedere backup [e ripristino di macchine virtuali crittografate con backup di Azure](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption).  Il ripristino di una macchina virtuale crittografata in un'area diversa non è attualmente supportato.  

## <a name="where-can-i-go-to-ask-questions-or-provide-feedback"></a>Quali risorse sono disponibili per porre domande o inviare commenti?

È possibile porre domande o inviare commenti e suggerimenti sul [forum di Crittografia dischi di Azure disponibile](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureDiskEncryption).

## <a name="next-steps"></a>Passaggi successivi
In questo documento sono state fornite le risposte alle domande più frequenti relative a Crittografia dischi di Azure. Per altre informazioni sul servizio, vedere gli articoli seguenti:

- [Informazioni su Crittografia dischi di Azure](azure-security-disk-encryption-overview.md)
- [Applicare la crittografia dei dischi nel Centro sicurezza di Azure](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Crittografia dei dati inattivi in Azure](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest)
