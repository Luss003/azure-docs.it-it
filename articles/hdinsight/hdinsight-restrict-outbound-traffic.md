---
title: Configurare la restrizione del traffico di rete in uscita-Azure HDInsight
description: Informazioni su come configurare la restrizione del traffico di rete in uscita per i cluster HDInsight di Azure.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/23/2019
ms.openlocfilehash: 6771cdb206920c8e3b746e28573de1742543b4c8
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/03/2020
ms.locfileid: "75646694"
---
# <a name="configure-outbound-network-traffic-for-azure-hdinsight-clusters-using-firewall"></a>Configurare il traffico di rete in uscita per i cluster HDInsight di Azure con il firewall

Questo articolo illustra la procedura per proteggere il traffico in uscita dal cluster HDInsight usando il firewall di Azure. I passaggi seguenti presuppongono che si stia configurando un firewall di Azure per un cluster esistente. Se si distribuisce un nuovo cluster e si è protetti da un firewall, creare prima il cluster HDInsight e la subnet, quindi seguire i passaggi descritti in questa guida.

## <a name="background"></a>Background

I cluster HDInsight di Azure vengono in genere distribuiti nella propria rete virtuale. Il cluster ha dipendenze da servizi esterni alla rete virtuale che richiedono l'accesso alla rete per funzionare correttamente.

Esistono diverse dipendenze che richiedono il traffico in ingresso. Il traffico di gestione in ingresso non può essere inviato tramite un dispositivo firewall. Gli indirizzi di origine per questo traffico sono noti e sono pubblicati [qui](hdinsight-management-ip-addresses.md). È anche possibile creare regole del gruppo di sicurezza di rete (NSG) con queste informazioni per proteggere il traffico in ingresso verso i cluster.

Le dipendenze del traffico in uscita HDInsight sono quasi completamente definite con FQDN, che non dispongono di indirizzi IP statici. La mancanza di indirizzi statici indica che i gruppi di sicurezza di rete (gruppi) non possono essere usati per bloccare il traffico in uscita da un cluster. Gli indirizzi cambiano abbastanza spesso che non è possibile configurare le regole in base alla risoluzione dei nomi corrente e usarle per configurare le regole NSG.

La soluzione per la protezione degli indirizzi in uscita consiste nell'usare un dispositivo firewall in grado di controllare il traffico in uscita in base ai nomi di dominio. Il firewall di Azure può limitare il traffico HTTP e HTTPS in uscita in base al nome di dominio completo dei tag di destinazione o [FQDN](https://docs.microsoft.com/azure/firewall/fqdn-tags).

## <a name="configuring-azure-firewall-with-hdinsight"></a>Configurazione del firewall di Azure con HDInsight

Un riepilogo dei passaggi per bloccare l'uscita dal HDInsight esistente con il firewall di Azure è:

1. Creazione di un firewall.
1. Aggiungere regole dell'applicazione al firewall
1. Aggiungere regole di rete al firewall.
1. Creare una tabella di routing.

### <a name="create-new-subnet"></a>Crea nuova subnet

Creare una subnet denominata **AzureFirewallSubnet** nella rete virtuale in cui è presente il cluster.

### <a name="create-a-new-firewall-for-your-cluster"></a>Creare un nuovo firewall per il cluster

Creare un firewall denominato **test-FW01** usando la procedura descritta in **distribuire il firewall** da [esercitazione: distribuire e configurare il firewall di Azure usando il portale di Azure](../firewall/tutorial-firewall-deploy-portal.md#deploy-the-firewall).

### <a name="configure-the-firewall-with-application-rules"></a>Configurare il firewall con le regole dell'applicazione

Creare una raccolta di regole dell'applicazione che consenta al cluster di inviare e ricevere comunicazioni importanti.

1. Selezionare il nuovo firewall **test-FW01** dalla portale di Azure.

1. Passare a **impostazioni** > **regole** > **raccolta regole applicazione** >  **+ Aggiungi raccolta regole applicazione**.

    ![Title: aggiungere una raccolta di regole dell'applicazione](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-app-rule-collection.png)

1. Nella schermata **Aggiungi raccolta regole applicazione** specificare le informazioni seguenti:

    **Sezione superiore**

    | Proprietà|  Valore|
    |---|---|
    |Nome| FwAppRule|
    |Priorità|200|
    |Azione|Allow|

    **Sezione Tag FQDN**

    | Nome | Indirizzo di origine | Tag FQDN | Note |
    | --- | --- | --- | --- |
    | Rule_1 | * | WindowsUpdate e HDInsight | Obbligatorio per i servizi HDI |

    **Sezione FQDN di destinazione**

    | Nome | Indirizzi di origine | Protocollo: porta | FQDN di destinazione | Note |
    | --- | --- | --- | --- | --- |
    | Rule_2 | * | https: 443 | login.windows.net | Consente l'attività di accesso di Windows |
    | Rule_3 | * | https: 443 | login.microsoftonline.com | Consente l'attività di accesso di Windows |
    | Rule_4 | * | https: 443, http: 80 | storage_account_name. blob. Core. Windows. NET | Sostituire `storage_account_name` con il nome dell'account di archiviazione effettivo. Se il cluster è supportato da WASB, aggiungere una regola per WASB. Per usare solo le connessioni HTTPS, verificare che l'account di archiviazione sia abilitato per il [trasferimento sicuro obbligatorio](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) . |

   ![Titolo: immettere i dettagli della raccolta regole dell'applicazione](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-app-rule-collection-details.png)

1. Selezionare **Aggiungi**.

### <a name="configure-the-firewall-with-network-rules"></a>Configurare il firewall con le regole di rete

Creare le regole di rete per configurare correttamente il cluster HDInsight.

1. Continuando dal passaggio precedente, passare alla **raccolta regole di rete** >  **+ Aggiungi raccolta regole di rete**.

1. Nella schermata **Aggiungi raccolta regole di rete** specificare le informazioni seguenti:

    **Sezione superiore**

    | Proprietà|  Valore|
    |---|---|
    |Nome| FwNetRule|
    |Priorità|200|
    |Azione|Allow|

    **Sezione indirizzi IP**

    | Nome | Protocollo | Indirizzi di origine | Indirizzi di destinazione | Porte di destinazione | Note |
    | --- | --- | --- | --- | --- | --- |
    | Rule_1 | UDP | * | * | 123 | Servizio Ora |
    | Rule_2 | Qualsiasi | * | DC_IP_Address_1, DC_IP_Address_2 | * | Se si usa Enterprise Security Package (ESP), aggiungere una regola di rete nella sezione indirizzi IP che consenta la comunicazione con AAD-DS per i cluster ESP. È possibile trovare gli indirizzi IP dei controller di dominio nella sezione AAD-DS nel portale |
    | Rule_3 | TCP | * | Indirizzo IP dell'account di Data Lake Storage | * | Se si usa Azure Data Lake Storage, è possibile aggiungere una regola di rete nella sezione indirizzi IP per risolvere un problema di SNI con ADLS Gen1 e Gen2. Questa opzione consente di instradare il traffico al firewall, il che potrebbe comportare costi più elevati per carichi di dati di grandi dimensioni, ma il traffico verrà registrato e controllabile nei log del firewall. Determinare l'indirizzo IP per l'account Data Lake Storage. È possibile usare un comando di PowerShell, ad esempio `[System.Net.DNS]::GetHostAddresses("STORAGEACCOUNTNAME.blob.core.windows.net")` per risolvere l'FQDN in un indirizzo IP.|
    | Rule_4 | TCP | * | * | 12000 | Opzionale Se si usa Log Analytics, creare una regola di rete nella sezione indirizzi IP per abilitare la comunicazione con l'area di lavoro di Log Analytics. |

    **Sezione Tag servizio**

    | Nome | Protocollo | Indirizzi di origine | Tag di servizio | Porte di destinazione | Note |
    | --- | --- | --- | --- | --- | --- |
    | Rule_7 | TCP | * | SQL | 1433 | Configurare una regola di rete nella sezione Tag del servizio per SQL che consentirà di registrare e controllare il traffico SQL, a meno che non siano stati configurati endpoint di servizio per SQL Server nella subnet HDInsight, che ignorerà il firewall. |

   ![Titolo: immettere la raccolta regole dell'applicazione](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-network-rule-collection.png)

1. Selezionare **Aggiungi**.

### <a name="create-and-configure-a-route-table"></a>Creare e configurare una tabella di route

Creare una tabella di route con le voci seguenti:

* Tutti gli indirizzi IP da [servizi di integrità e gestione: tutte le aree](../hdinsight/hdinsight-management-ip-addresses.md#health-and-management-services-all-regions) con un tipo di hop successivo di **Internet**.

* Due indirizzi IP per l'area in cui il cluster viene creato da [integrità e servizi di gestione: aree specifiche](../hdinsight/hdinsight-management-ip-addresses.md#health-and-management-services-specific-regions) con un tipo di hop successivo di **Internet**.

* Una route dell'appliance virtuale per l'indirizzo IP 0.0.0.0/0 con l'hop successivo che è l'indirizzo IP privato del firewall di Azure.

Ad esempio, per configurare la tabella di route per un cluster creato nell'area Stati Uniti di "Stati Uniti orientali", seguire questa procedura:

1. Selezionare il firewall **di Azure test-FW01**. Copiare l' **indirizzo IP privato** elencato nella pagina **Overview** . Per questo esempio, verrà usato un **indirizzo di esempio di 10.0.2.4**.

1. Passare quindi a **tutti i servizi** > **rete** > **tabelle di route** e **creare una tabella di route**.

1. Dalla nuova route passare a **impostazioni** > **Route** >  **+ Aggiungi**. Aggiungere le route seguenti:

| Nome route | Prefisso indirizzo | Tipo hop successivo | Indirizzo hop successivo |
|---|---|---|---|
| 168.61.49.99 | 168.61.49.99/32 | Internet | ND |
| 23.99.5.239 | 23.99.5.239/32 | Internet | ND |
| 168.61.48.131 | 168.61.48.131/32 | Internet | ND |
| 138.91.141.162 | 138.91.141.162/32 | Internet | ND |
| 13.82.225.233 | 13.82.225.233/32 | Internet | ND |
| 40.71.175.99 | 40.71.175.99/32 | Internet | ND |
| 0.0.0.0 | 0.0.0.0/0 | Appliance virtuale | 10.0.2.4 |

Completare la configurazione della tabella di route:

1. Assegnare la tabella di route creata alla subnet HDInsight selezionando **subnet** in **Impostazioni**.

1. Selezionare **+ associa**.

1. Nella schermata **associa subnet** selezionare la rete virtuale in cui è stato creato il cluster e la **Subnet** usata per il cluster HDInsight.

1. Selezionare **OK**.

## <a name="edge-node-or-custom-application-traffic"></a>Traffico di applicazioni di nodi perimetrali o personalizzate

I passaggi precedenti consentiranno al cluster di funzionare senza problemi. È comunque necessario configurare le dipendenze per supportare le applicazioni personalizzate in esecuzione nei nodi perimetrali, se applicabile.

Le dipendenze dell'applicazione devono essere identificate e aggiunte al firewall di Azure o alla tabella di route.

Per evitare problemi di routing asimmetrico, è necessario creare le route per il traffico dell'applicazione.

Se le applicazioni hanno altre dipendenze, è necessario aggiungerle al firewall di Azure. Creare regole per le applicazioni per consentire il traffico HTTP/HTTPS e regole di rete per tutto il resto.

## <a name="logging-and-scale"></a>Registrazione e scalabilità

Il firewall di Azure può inviare i log a diversi sistemi di archiviazione. Per istruzioni sulla configurazione della registrazione per il firewall, seguire la procedura descritta in [esercitazione: monitorare i log e le metriche del firewall di Azure](../firewall/tutorial-diagnostics.md).

Una volta completata la configurazione della registrazione, se si registrano dati in Log Analytics, è possibile visualizzare il traffico bloccato con una query come la seguente:

```Kusto
AzureDiagnostics | where msg_s contains "Deny" | where TimeGenerated >= ago(1h)
```

L'integrazione del firewall di Azure con i log di monitoraggio di Azure è utile per la prima volta che un'applicazione funziona quando non si è a conoscenza di tutte le dipendenze dell'applicazione. Per altre informazioni sui log di Monitoraggio di Azure, vedere [Analizzare i dati di log in Monitoraggio di Azure](../azure-monitor/log-query/log-query-overview.md)

Per informazioni sui limiti di scalabilità del firewall di Azure e sugli aumenti delle richieste, vedere [questo](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-firewall-limits) documento o consultare le [domande frequenti](../firewall/firewall-faq.md).

## <a name="access-to-the-cluster"></a>Accesso al cluster

Dopo aver configurato correttamente il firewall, è possibile usare l'endpoint interno (`https://CLUSTERNAME-int.azurehdinsight.net`) per accedere a Ambari dall'interno di VNET.

Per usare l'endpoint pubblico (`https://CLUSTERNAME.azurehdinsight.net`) o l'endpoint SSH (`CLUSTERNAME-ssh.azurehdinsight.net`), assicurarsi di avere le route corrette nella tabella di route e le regole NSG per evitare il problema di routing asimmetrico illustrato [qui](../firewall/integrate-lb.md). In particolare, in questo caso, è necessario consentire l'indirizzo IP del client nelle regole del NSG in ingresso e aggiungerlo anche alla tabella di route definita dall'utente con il set di hop successivo come `internet`. Se questa opzione non è configurata correttamente, verrà visualizzato un errore di timeout.

## <a name="configure-another-network-virtual-appliance"></a>Configurare un'altra appliance virtuale di rete

> [!Important]
> Le informazioni seguenti sono necessarie **solo** se si vuole configurare un'appliance virtuale di rete diversa dal firewall di Azure.

Le istruzioni precedenti consentono di configurare il firewall di Azure per la limitazione del traffico in uscita dal cluster HDInsight. Il firewall di Azure viene configurato automaticamente per consentire il traffico per molti degli scenari importanti comuni. Se si vuole usare un'altra appliance virtuale di rete, è necessario configurare manualmente una serie di funzionalità aggiuntive. Quando si configura l'appliance di rete virtuale, tenere presente quanto segue:

* Con gli endpoint di servizio devono essere configurati servizi che supportano endpoint di servizio.
* Le dipendenze degli indirizzi IP sono per il traffico non HTTP/S (traffico TCP e UDP).
* Gli endpoint HTTP/HTTPS FQDN possono essere inseriti nel dispositivo appliance virtuale di dispositivi.
* Gli endpoint HTTP/HTTPS con caratteri jolly sono dipendenze che possono variare in base a un numero di qualificatori.
* Assegnare la tabella di route creata alla subnet HDInsight.

### <a name="service-endpoint-capable-dependencies"></a>Dipendenze con supporto per endpoint di servizio

| **Endpoint** |
|---|
| Azure SQL |
| Archiviazione di Azure |
| Azure Active Directory |

#### <a name="ip-address-dependencies"></a>Dipendenze di indirizzi IP

| **Endpoint** | **Dettagli** |
|---|---|
| \*:123 | Controllo dell'orologio NTP. Il traffico viene verificato in più endpoint sulla porta 123. |
| Indirizzi IP pubblicati [qui](hdinsight-management-ip-addresses.md) | Si tratta del servizio HDInsight |
| Indirizzi IP privati AAD-DS per i cluster ESP |
| \*: 16800 per l'attivazione di Windows KMS |
| \*12000 per Log Analytics |

#### <a name="fqdn-httphttps-dependencies"></a>Dipendenze HTTP/HTTPS con nome di dominio completo

> [!Important]
> L'elenco seguente fornisce solo alcuni dei nomi di dominio completi più importanti. È possibile ottenere l'elenco completo dei nomi di dominio completi per la configurazione dell'appliance virtuale di sistema [in questo file](https://github.com/Azure-Samples/hdinsight-fqdn-lists/blob/master/HDInsightFQDNTags.json).

| **Endpoint**                                                          |
|---|
| azure.archive.ubuntu.com:80                                           |
| security.ubuntu.com:80                                                |
| ocsp.msocsp.com:80                                                    |
| ocsp.digicert.com:80                                                  |
| wawsinfraprodbay063.blob.core.windows.net:443                         |
| registry-1.docker.io:443                                              |
| auth.docker.io:443                                                    |
| production.cloudflare.docker.com:443                                  |
| download.docker.com:443                                               |
| us.archive.ubuntu.com:80                                              |
| download.mono-project.com:80                                          |
| packages.treasuredata.com:80                                          |
| security.ubuntu.com:80                                                |
| azure.archive.ubuntu.com:80                                           |
| ocsp.msocsp.com:80                                                    |
| ocsp.digicert.com:80                                                  |

## <a name="next-steps"></a>Passaggi successivi

* [Architettura della rete virtuale di Azure HDInsight](hdinsight-virtual-network-architecture.md)
