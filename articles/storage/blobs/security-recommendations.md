---
title: Raccomandazioni sulla sicurezza per l'archiviazione BLOB
titleSuffix: Azure Storage
description: Informazioni sulle raccomandazioni sulla sicurezza per l'archiviazione BLOB. L'implementazione di queste linee guida consente di soddisfare gli obblighi di sicurezza, come descritto nel modello di responsabilità condivisa.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: tamram
ms.custom: security-recommendations
ms.openlocfilehash: ee96ec85869cbde9d5fac540e9fbbaac88858daf
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75479384"
---
# <a name="security-recommendations-for-blob-storage"></a>Raccomandazioni sulla sicurezza per l'archiviazione BLOB

Questo articolo contiene raccomandazioni sulla sicurezza per l'archiviazione BLOB. L'implementazione di questi consigli consentirà di soddisfare gli obblighi di sicurezza come descritto nel modello di responsabilità condivisa. Per ulteriori informazioni sulle attività svolte da Microsoft per soddisfare le responsabilità del provider di servizi, vedere [responsabilità condivise per cloud computing](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/225366/1/Shared%20Responsibility%20for%20Cloud%20Computing-2019-10-25.pdf).

Alcuni consigli inclusi in questo articolo possono essere monitorati automaticamente dal centro sicurezza di Azure. Il Centro sicurezza di Azure è la prima linea di difesa per la protezione delle risorse in Azure. Per informazioni sul centro sicurezza di Azure, vedere informazioni sul [Centro sicurezza di Azure](../../security-center/security-center-intro.md).

Il Centro sicurezza di Azure analizza periodicamente lo stato di sicurezza delle risorse di Azure per identificare le potenziali vulnerabilità di sicurezza. Fornisce quindi suggerimenti su come risolverli. Per altre informazioni sulle raccomandazioni del Centro sicurezza di Azure, vedere [raccomandazioni per la sicurezza nel centro sicurezza di Azure](../../security-center/security-center-recommendations.md).

## <a name="data-protection"></a>Protezione dei dati

| Recommendation | Commenti | Centro sicurezza |
|-|----|--|
| Usare il modello di distribuzione Azure Resource Manager | È possibile creare nuovi account di archiviazione usando il modello di distribuzione Azure Resource Manager per importanti miglioramenti della sicurezza, tra cui controllo degli accessi (RBAC) e controllo (RBAC), gestione e distribuzione basati su Gestione risorse, accesso a identità gestite, accesso per Azure Key Vault per i segreti e l'autenticazione e l'autorizzazione basate su Azure AD per l'accesso ai dati e alle risorse di archiviazione di Azure. Se possibile, eseguire la migrazione degli account di archiviazione esistenti che usano il modello di distribuzione classica per usare Azure Resource Manager. Per ulteriori informazioni su Azure Resource Manager, vedere [Panoramica di Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview). | - |
| Abilitare l'opzione **trasferimento sicuro obbligatorio** in tutti gli account di archiviazione | Quando si Abilita l'opzione **trasferimento sicuro obbligatorio** , tutte le richieste effettuate nell'account di archiviazione devono essere eseguite su connessioni sicure. Eventuali richieste effettuate su HTTP avranno esito negativo. Per altre informazioni, vedere [richiedere il trasferimento sicuro in archiviazione di Azure](../common/storage-require-secure-transfer.md). | [Sì](../../security-center/security-center-sql-service-recommendations.md) |
| Abilitare Advanced Threat Protection per tutti gli account di archiviazione | Advanced Threat Protection per archiviazione di Azure offre un ulteriore livello di intelligence per la sicurezza che rileva tentativi insoliti e potenzialmente dannosi di accedere o sfruttare gli account di archiviazione. Gli avvisi di sicurezza vengono attivati nel centro sicurezza di Azure quando si verificano anomalie nelle attività e vengono inviati anche tramite posta elettronica agli amministratori della sottoscrizione, con dettagli relativi a attività sospette e consigli su come analizzare e correggere le minacce. Per altre informazioni, vedere [Advanced Threat Protection per archiviazione di Azure](../common/storage-advanced-threat-protection.md). | [Sì](../../security-center/security-center-sql-service-recommendations.md) |
| Attivare l'eliminazione temporanea per i dati BLOB | L'eliminazione temporanea consente di ripristinare i dati BLOB dopo l'eliminazione. Per altre informazioni sull'eliminazione temporanea, vedere [eliminazione temporanea per i BLOB di archiviazione di Azure](storage-blob-soft-delete.md). | - |
| Archiviare dati critici per l'azienda in BLOB non modificabili | Configurare i criteri di conservazione validi e temporali per archiviare i dati BLOB in un WORM (scrivere una sola volta, leggere molti) stato. I BLOB archiviati immutabilmente possono essere letti, ma non possono essere modificati o eliminati per la durata dell'intervallo di conservazione. Per altre informazioni, vedere [archiviare dati BLOB critici per l'azienda con archiviazione non modificabile](storage-blob-immutable-storage.md). | - |
| Limitare i token di firma di accesso condiviso solo alle connessioni HTTPS | Richiesta di HTTPS quando un client utilizza un token di firma di accesso condiviso per accedere ai dati BLOB consente di ridurre al minimo il rischio di intercettazione. Per altre informazioni, vedere [concedere l'accesso limitato alle risorse di archiviazione di Azure usando le firme di accesso condiviso (SAS)](../common/storage-sas-overview.md). | - |

## <a name="identity-and-access-management"></a>Gestione degli accessi e delle identità

| Recommendation | Commenti | Centro sicurezza |
|-|----|--|
| Usare Azure Active Directory (Azure AD) per autorizzare l'accesso ai dati BLOB | Azure AD offre sicurezza e facilità d'uso superiori rispetto alla chiave condivisa per l'autorizzazione delle richieste all'archiviazione BLOB. Per altre informazioni, vedere [autorizzare l'accesso a BLOB e code di Azure usando Azure Active Directory](../common/storage-auth-aad.md). | - |
| Tenere presente l'entità del privilegio minimo quando si assegnano autorizzazioni a un'entità di sicurezza Azure AD tramite RBAC | Quando si assegna un ruolo a un utente, a un gruppo o a un'applicazione, concedere all'entità di sicurezza solo le autorizzazioni necessarie per eseguire le attività. La limitazione dell'accesso alle risorse consente di evitare l'utilizzo non intenzionale e dannoso dei dati. | - |
| Usare una firma di accesso condiviso di delega utente per concedere l'accesso limitato ai dati BLOB ai client | Una firma di accesso condiviso di delega utente è protetta con credenziali Azure Active Directory (Azure AD) e anche dalle autorizzazioni specificate per la firma di accesso condiviso. Una firma di accesso condiviso dell'utente è analoga a una firma di accesso condiviso del servizio in termini di ambito e funzione, ma offre vantaggi di sicurezza sulla firma di accesso condiviso del servizio. Per altre informazioni, vedere [concedere l'accesso limitato alle risorse di archiviazione di Azure usando le firme di accesso condiviso (SAS)](../common/storage-sas-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json). | - |
| Proteggere le chiavi di accesso dell'account con Azure Key Vault | Microsoft consiglia di usare Azure AD per autorizzare le richieste ad archiviazione di Azure. Tuttavia, se è necessario usare l'autorizzazione della chiave condivisa, proteggere le chiavi dell'account con Azure Key Vault. È possibile recuperare le chiavi dall'insieme di credenziali delle chiavi in fase di esecuzione, anziché salvarle con l'applicazione. Per ulteriori informazioni su Azure Key Vault, vedere [Panoramica di Azure Key Vault](../../key-vault/key-vault-overview.md). | - |
| Rigenerare periodicamente le chiavi dell'account | La rotazione delle chiavi dell'account riduce periodicamente il rischio di esporre i dati a attori malintenzionati. | - |
| Tenere presente l'entità del privilegio minimo quando si assegnano le autorizzazioni a una firma di accesso condiviso | Quando si crea una firma di accesso condiviso, specificare solo le autorizzazioni richieste dal client per eseguire la relativa funzione. La limitazione dell'accesso alle risorse consente di evitare l'utilizzo non intenzionale e dannoso dei dati. | - |
| Disporre di un piano di revoca per qualsiasi firma di accesso condiviso che viene rilasciata ai client | Se una firma di accesso condiviso viene compromessa, è opportuno revocare la firma di accesso condiviso appena possibile. Per revocare la firma di accesso condiviso della delega utente, revocare la chiave di delega utente per invalidare rapidamente tutte le firme associate a tale chiave. Per revocare una firma di accesso condiviso del servizio associata a un criterio di accesso archiviato, è possibile eliminare i criteri di accesso archiviati, rinominare il criterio o modificare l'ora di scadenza in un'ora nel passato. Per altre informazioni, vedere [concedere l'accesso limitato alle risorse di archiviazione di Azure usando le firme di accesso condiviso (SAS)](../common/storage-sas-overview.md).  | - |
| Se una firma di accesso condiviso del servizio non è associata a criteri di accesso archiviati, impostare l'ora di scadenza su un'ora o su un valore inferiore | Non è possibile revocare una firma di accesso condiviso del servizio che non è associata a criteri di accesso archiviati. Per questo motivo, è consigliabile limitare l'ora di scadenza in modo che la firma di accesso condiviso sia valida per un'ora o meno. | - |
| Limitare l'accesso in lettura pubblico anonimo a contenitori e BLOB | L'accesso in lettura pubblico anonimo a un contenitore e ai relativi BLOB concede l'accesso in sola lettura a tali risorse a qualsiasi client. Evitare di abilitare l'accesso in lettura pubblico a meno che lo scenario non lo richieda. | - |

## <a name="networking"></a>Rete

| Recommendation | Commenti | Centro sicurezza |
|-|----|--|
| Abilitare le regole del firewall | Configurare le regole del firewall per limitare l'accesso all'account di archiviazione alle richieste che hanno origine da indirizzi o intervalli IP specificati oppure da un elenco di subnet in una rete virtuale di Azure (VNet). Per ulteriori informazioni sulla configurazione delle regole del firewall, vedere [sincronizzazione file di Azure impostazioni del proxy e del firewall](../files/storage-sync-files-firewall-and-proxy.md). | - |
| Consenti ai servizi Microsoft attendibili di accedere all'account di archiviazione | L'attivazione delle regole del firewall per l'account di archiviazione blocca le richieste in ingresso per i dati per impostazione predefinita, a meno che le richieste provengano da un servizio che opera all'interno di una rete virtuale di Azure (VNet) o da indirizzi IP pubblici consentiti. Le richieste che vengono bloccate sono quelle che provengono da altri servizi di Azure, dal portale di Azure, dai servizi di registrazione e metriche e così via. È possibile consentire le richieste da altri servizi di Azure aggiungendo un'eccezione per consentire ai servizi Microsoft attendibili di accedere all'account di archiviazione. Per ulteriori informazioni sull'aggiunta di un'eccezione per i servizi Microsoft attendibili, vedere [sincronizzazione file di Azure impostazioni del proxy e del firewall](../files/storage-sync-files-firewall-and-proxy.md).| - |
| Usare endpoint privati | Un endpoint privato assegna un indirizzo IP privato dalla rete virtuale di Azure (VNet) all'account di archiviazione. Protegge tutto il traffico tra il VNet e l'account di archiviazione tramite un collegamento privato. Per altre informazioni sugli endpoint privati, vedere [connettersi privatamente a un account di archiviazione usando un endpoint privato di Azure](../../private-link/create-private-endpoint-storage-portal.md). | - |
| Limitare l'accesso alla rete per reti specifiche | La limitazione dell'accesso di rete alle reti che ospitano i client che richiedono l'accesso riduce l'esposizione delle risorse agli attacchi di rete. | [Sì](../../security-center/security-center-sql-service-recommendations.md) |

## <a name="loggingmonitoring"></a>Registrazione/monitoraggio

| Recommendation | Commenti | Centro sicurezza |
|-|----|--|
| Tenere traccia del modo in cui le richieste sono autorizzate | Abilitare la registrazione di archiviazione di Azure per tenere traccia del modo in cui ogni richiesta effettuata nell'archiviazione di Azure è stata autorizza I log indicano se una richiesta è stata effettuata in modo anonimo, usando un token OAuth 2,0, usando la chiave condivisa o usando una firma di accesso condiviso (SAS). Per altre informazioni, vedere [registrazione di analisi archiviazione di Azure](../common/storage-analytics-logging.md). | - |

## <a name="next-steps"></a>Passaggi successivi

- [Documentazione sulla sicurezza di Azure](https://docs.microsoft.com//azure/security/)
- [Documentazione per lo sviluppo sicuro](https://docs.microsoft.com/azure/security/develop/).
