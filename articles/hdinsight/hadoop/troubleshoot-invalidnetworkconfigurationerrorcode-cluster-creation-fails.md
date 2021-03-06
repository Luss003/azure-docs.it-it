---
title: Errore InvalidNetworkConfigurationErrorCode-Azure HDInsight
description: Diversi motivi per le creazioni di cluster non riuscite con InvalidNetworkConfigurationErrorCode in Azure HDInsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/22/2020
ms.openlocfilehash: 6dd4db999cb130c9816ad023888a4333e968c224
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2020
ms.locfileid: "76720385"
---
# <a name="cluster-creation-fails-with-invalidnetworkconfigurationerrorcode-in-azure-hdinsight"></a>La creazione del cluster non riesce con InvalidNetworkConfigurationErrorCode in Azure HDInsight

Questo articolo descrive le procedure di risoluzione dei problemi e le possibili soluzioni per i problemi durante l'interazione con i cluster HDInsight di Azure.

Se viene visualizzato il codice di errore `InvalidNetworkConfigurationErrorCode` con la descrizione "la configurazione della rete virtuale non è compatibile con il requisito di HDInsight", in genere indica un problema con la [configurazione della rete virtuale](../hdinsight-plan-virtual-network-deployment.md) per il cluster. In base al resto della descrizione dell'errore, seguire le sezioni seguenti per risolvere il problema.

## <a name="hostname-resolution-failed"></a>"Risoluzione nome host non riuscita"

### <a name="issue"></a>Problema

La descrizione dell'errore contiene "risoluzione nome host non riuscita".

### <a name="cause"></a>Causa

Questo errore indica un problema con la configurazione DNS personalizzata. I server DNS all'interno di una rete virtuale possono inviare query DNS ai resolver ricorsivi di Azure per risolvere i nomi host all'interno della rete virtuale. per informazioni dettagliate, vedere [risoluzione dei nomi nelle reti virtuali](../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) . L'accesso ai resolver ricorsivi di Azure viene fornito tramite l'indirizzo IP virtuale 168.63.129.16. Questo indirizzo IP è accessibile solo dalle macchine virtuali di Azure. Quindi non funzionerà se si usa un server DNS locale o se il server DNS è una macchina virtuale di Azure, che non fa parte della rete virtuale del cluster.

### <a name="resolution"></a>Risoluzione

1. Connettersi tramite SSH alla macchina virtuale che fa parte del cluster ed eseguire il comando `hostname -f`. Verrà restituito il nome di dominio completo dell'host (denominato `<host_fqdn>` nelle istruzioni seguenti).

1. Eseguire quindi il comando `nslookup <host_fqdn>`, ad esempio `nslookup hn1-hditest.5h6lujo4xvoe1kprq3azvzmwsd.hx.internal.cloudapp.net`. Se questo comando risolve il nome in un indirizzo IP, significa che il server DNS funziona correttamente. In questo caso, viene generato un caso di supporto con HDInsight e si analizzerà il problema. Nel caso di supporto, includere i passaggi per la risoluzione dei problemi eseguiti. Questo consentirà di risolvere il problema più velocemente.

1. Se il comando precedente non restituisce un indirizzo IP, eseguire `nslookup <host_fqdn> 168.63.129.16`, ad esempio `nslookup hn1-hditest.5h6lujo4xvoe1kprq3azvzmwsd.hx.internal.cloudapp.net 168.63.129.16`. Se questo comando è in grado di risolvere l'indirizzo IP, significa che il server DNS non sta inviando la query al DNS di Azure o non è una macchina virtuale che fa parte della stessa rete virtuale del cluster.

1. Se non si dispone di una macchina virtuale di Azure che può fungere da server DNS personalizzato nella rete virtuale del cluster, è necessario prima aggiungere questa. Creare una VM nella rete virtuale, che verrà configurata come server di trasmissione DNS.

1. Una volta distribuita una macchina virtuale nella rete virtuale, configurare le regole di invio DNS in questa macchina virtuale. Inviare tutte le richieste di risoluzione dei nomi di IDN a 168.63.129.16 e il resto al server DNS. Di [seguito](../hdinsight-plan-virtual-network-deployment.md) è riportato un esempio di questa configurazione per un server DNS personalizzato.

1. Aggiungere l'indirizzo IP della macchina virtuale come prima voce DNS per la configurazione DNS della rete virtuale.

---

## <a name="failed-to-connect-to-azure-storage-account"></a>"Non è stato possibile connettersi all'account di archiviazione di Azure"

### <a name="issue"></a>Problema

La descrizione dell'errore contiene "Impossibile connettersi all'account di archiviazione di Azure" o "Impossibile connettersi a SQL Azure".

### <a name="cause"></a>Causa

Archiviazione di Azure e SQL non hanno indirizzi IP fissi, quindi è necessario consentire le connessioni in uscita a tutti gli IP per consentire l'accesso a questi servizi. La procedura di risoluzione esatta varia a seconda che sia stato configurato un gruppo di sicurezza di rete (NSG) o regole definite dall'utente (UDR). Per informazioni dettagliate su queste configurazioni, vedere la sezione [controllo del traffico di rete con HDInsight con gruppi di sicurezza di rete e route definite dall'utente](../hdinsight-plan-virtual-network-deployment.md#hdinsight-ip) .

### <a name="resolution"></a>Risoluzione

* Se il cluster usa un [gruppo di sicurezza di rete (NSG)](../../virtual-network/virtual-network-vnet-plan-design-arm.md).

    Passare alla portale di Azure e identificare il NSG associato alla subnet in cui viene distribuito il cluster. Nella sezione **regole di sicurezza in uscita** , consentire l'accesso in uscita a Internet senza limitazioni (si noti che un numero di **priorità** più basso qui indica una priorità più elevata). Inoltre, nella sezione **subnet** verificare se questo NSG viene applicato alla subnet del cluster.

* Se il cluster USA [route definite dall'utente (UDR)](../../virtual-network/virtual-networks-udr-overview.md).

    Passare alla portale di Azure e identificare la tabella di route associata alla subnet in cui viene distribuito il cluster. Una volta trovata la tabella di route per la subnet, controllare la sezione **Route** .

    Se sono presenti route definite, assicurarsi che siano presenti route per gli indirizzi IP per l'area in cui è stato distribuito il cluster e che il **NextHopType** per ogni route sia **Internet**. Deve essere definita una route per ogni indirizzo IP richiesto documentato nell'articolo citato sopra.

---

## <a name="virtual-network-configuration-is-not-compatible-with-hdinsight-requirement"></a>"La configurazione della rete virtuale non è compatibile con il requisito di HDInsight"

### <a name="issue"></a>Problema

Le descrizioni degli errori contengono messaggi simili ai seguenti:

```
ErrorCode: InvalidNetworkConfigurationErrorCode
ErrorDescription: Virtual Network configuration is not compatible with HDInsight Requirement. Error: 'Failed to connect to Azure Storage Account; Failed to connect to Azure SQL; HostName Resolution failed', Please follow https://go.microsoft.com/fwlink/?linkid=853974 to fix it.
```

### <a name="cause"></a>Causa

Probabilmente si è riscontrato un problema con la configurazione del DNS personalizzato.

### <a name="resolution"></a>Risoluzione

Verificare che 168.63.129.16 sia nella catena DNS personalizzata. I server DNS all'interno di una rete virtuale possono inoltrare query DNS ai resolver ricorsivi di Azure per risolvere i nomi host all'interno di quella rete virtuale. Per altre informazioni, vedere [risoluzione dei nomi nelle reti virtuali](../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server). L'accesso ai resolver ricorsivi di Azure viene fornito tramite l'indirizzo IP virtuale 168.63.129.16.

1. Usare il [comando ssh](../hdinsight-hadoop-linux-use-ssh-unix.md) per connettersi al cluster. Modificare il comando seguente sostituendo CLUSTERname con il nome del cluster e quindi immettere il comando:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Eseguire il seguente comando:

    ```bash
    cat /etc/resolv.conf | grep nameserver*
    ```

    Verrà visualizzata una schermata analoga alla seguente:

    ```output
    nameserver 168.63.129.16
    nameserver 10.21.34.43
    nameserver 10.21.34.44
    ```

    In base al risultato, scegliere uno dei seguenti passaggi da seguire:

#### <a name="1686312916-is-not-in-this-list"></a>168.63.129.16 non è presente nell'elenco

**Opzione 1**  
Aggiungere 168.63.129.16 come primo DNS personalizzato per la rete virtuale usando la procedura descritta in [pianificare una rete virtuale per Azure HDInsight](../hdinsight-plan-virtual-network-deployment.md). Questi passaggi sono applicabili solo se il server DNS personalizzato viene eseguito in Linux.

**Opzione 2**  
Distribuire una VM del server DNS per la rete virtuale. Questo include i passaggi seguenti:

* Creare una VM nella rete virtuale, che verrà configurata come server di trasmissione DNS (può essere una macchina virtuale Linux o Windows).
* Configurare le regole di invio DNS in questa macchina virtuale (inviare tutte le richieste di risoluzione dei nomi di IDN a 168.63.129.16 e il resto al server DNS).
* Aggiungere l'indirizzo IP della macchina virtuale come prima voce DNS per la configurazione DNS della rete virtuale.

#### <a name="1686312916-is-in-the-list"></a>168.63.129.16 è nell'elenco

In questo caso, creare un caso di supporto con HDInsight e si analizzerà il problema. Includere il risultato dei comandi seguenti nel caso di supporto. Questo consentirà di analizzare e risolvere il problema più rapidamente.

Da una sessione SSH nel nodo Head, modificare e quindi eseguire quanto segue:

```bash
hostname -f
nslookup <headnode_fqdn> (e.g.nslookup hn1-hditest.5h6lujo4xvoe1kprq3azvzmwsd.hx.internal.cloudapp.net)
dig @168.63.129.16 <headnode_fqdn> (e.g. dig @168.63.129.16 hn0-hditest.5h6lujo4xvoe1kprq3azvzmwsd.hx.internal.cloudapp.net)
```

---

### <a name="next-steps"></a>Passaggi successivi

Se il problema riscontrato non è presente in questo elenco o se non si riesce a risolverlo, visitare uno dei canali seguenti per ottenere ulteriore assistenza:

* Ottieni risposte dagli esperti di Azure tramite il [supporto della community di Azure](https://azure.microsoft.com/support/community/).

* È possibile connettersi con [@AzureSupport](https://twitter.com/azuresupport) , l'account ufficiale Microsoft Azure per migliorare l'esperienza del cliente connettendo la community di Azure alle risorse appropriate: risposte, supporto ed esperti.

* Se è necessaria ulteriore assistenza, è possibile inviare una richiesta di supporto dal [portale di Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Selezionare **supporto** dalla barra dei menu o aprire l'hub **Guida e supporto** . Per informazioni più dettagliate, vedere [come creare una richiesta di supporto di Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). L'accesso alla gestione delle sottoscrizioni e al supporto per la fatturazione è incluso nella sottoscrizione di Microsoft Azure e il supporto tecnico viene fornito tramite uno dei [piani di supporto di Azure](https://azure.microsoft.com/support/plans/).
