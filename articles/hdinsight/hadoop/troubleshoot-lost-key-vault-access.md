---
title: I cluster HDInsight di Azure con crittografia del disco perdono l'accesso Key Vault
description: Procedure di risoluzione dei problemi e possibili soluzioni per i problemi durante l'interazione con i cluster HDInsight di Azure.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.openlocfilehash: 2ae389be25cd8633a53a49cf000796c1510733a1
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2020
ms.locfileid: "76965164"
---
# <a name="scenario-azure-hdinsight-clusters-with-disk-encryption-lose-key-vault-access"></a>Scenario: i cluster HDInsight di Azure con crittografia del disco perdono l'accesso Key Vault

Questo articolo descrive le procedure di risoluzione dei problemi e le possibili soluzioni per i problemi durante l'interazione con i cluster HDInsight di Azure.

## <a name="issue"></a>Problema

Viene visualizzato l'avviso Integrità risorse Center (RHC), `The HDInsight cluster is unable to access the key for BYOK encryption at rest`, per i cluster Bring Your Own Key (BYOK) in cui i nodi del cluster hanno perso l'accesso ai clienti Key Vault (KV). Avvisi simili possono essere visualizzati anche sull'interfaccia utente di Apache Ambari.

## <a name="cause"></a>Causa

L'avviso garantisce che KV sia accessibile dai nodi del cluster, garantendo in tal modo la connessione di rete, l'integrità KV e i criteri di accesso per l'identità gestita assegnata dall'utente. Questo avviso è solo un avviso di imminente arresto di Service Broker nei successivi riavvii del nodo, il cluster continua a funzionare fino al riavvio dei nodi.

Passare all'interfaccia utente di Apache Ambari per trovare altre informazioni sull'avviso dalla **crittografia del disco Key Vault stato**. Questo avviso include informazioni dettagliate sul motivo dell'errore di verifica.

## <a name="resolution"></a>Risoluzione

### <a name="kvaad-outage"></a>Interruzione di KV/AAD

Per altri dettagli, vedere [Azure Key Vault disponibilità e ridondanza](../../key-vault/key-vault-disaster-recovery-guidance.md) e la pagina relativa allo stato di Azure https://status.azure.com/

### <a name="kv-accidental-deletion"></a>Eliminazione accidentale KV

* Ripristinare la chiave eliminata in KV per il ripristino automatico. Per ulteriori informazioni, vedere la pagina relativa al [ripristino della chiave eliminata](https://docs.microsoft.com/rest/api/keyvault/recoverdeletedkey).
* Contattare il team KV per eseguire il ripristino da eliminazioni accidentali.

### <a name="kv-access-policy-changed"></a>Criteri di accesso KV modificati

Ripristinare i criteri di accesso per l'identità gestita assegnata dall'utente assegnata al cluster HDI per accedere al KV.

### <a name="key-permitted-operations"></a>Operazioni chiave consentite

Per ogni chiave in KV è possibile scegliere il set di operazioni consentite. Verificare che siano state abilitate le operazioni wrap e unwrap per la chiave BYOK

### <a name="expired-key"></a>Chiave scaduta

Se la scadenza è stata superata e la chiave non è ruotata, ripristinare la chiave dal modulo di protezione hardware di backup o contattare il team KV per cancellare la data di scadenza.

### <a name="kv-firewall-blocking-access"></a>Accesso bloccante del firewall KV

Correggere le impostazioni del firewall KV per consentire ai nodi del cluster BYOK di accedere al KV.

### <a name="nsg-rules-on-virtual-network-blocking-access"></a>Regole NSG sulla rete virtuale che bloccano l'accesso

Controllare le regole di NSG associate alla rete virtuale collegata al cluster.

## <a name="mitigation-and-prevention-steps"></a>Procedure di mitigazione e prevenzione

### <a name="kv-accidental-deletion"></a>Eliminazione accidentale KV

* Configurare Key Vault con [set di blocchi di risorse](../../azure-resource-manager/management/lock-resources.md).
* Eseguire il backup delle chiavi nel modulo di protezione hardware.

### <a name="key-deletion"></a>Eliminazione della chiave

Il cluster deve essere eliminato prima dell'eliminazione della chiave.

### <a name="kv-access-policy-changed"></a>Criteri di accesso KV modificati

Controllare e testare regolarmente i criteri di accesso.

### <a name="expired-key"></a>Chiave scaduta

* Eseguire il backup delle chiavi nel modulo di protezione hardware.
* Usare una chiave senza set di scadenza.
* Se è necessario impostare la scadenza, ruotare le chiavi prima della data di scadenza.

## <a name="next-steps"></a>Passaggi successivi

Se il problema riscontrato non è presente in questo elenco o se non si riesce a risolverlo, visitare uno dei canali seguenti per ottenere ulteriore assistenza:

* Ottieni risposte dagli esperti di Azure tramite il [supporto della community di Azure](https://azure.microsoft.com/support/community/).

* Connettersi con [@AzureSupport](https://twitter.com/azuresupport) : l'account ufficiale Microsoft Azure per migliorare l'esperienza del cliente. Connessione della community di Azure alle risorse appropriate: risposte, supporto ed esperti.

* Se è necessaria ulteriore assistenza, è possibile inviare una richiesta di supporto dal [portale di Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Selezionare **supporto** dalla barra dei menu o aprire l'hub **Guida e supporto** . Per informazioni più dettagliate, vedere [come creare una richiesta di supporto di Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). L'accesso alla gestione delle sottoscrizioni e al supporto per la fatturazione è incluso nella sottoscrizione di Microsoft Azure e il supporto tecnico viene fornito tramite uno dei [piani di supporto di Azure](https://azure.microsoft.com/support/plans/).
