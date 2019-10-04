---
title: Configurare Apache Kafka in HDInsight usando il portale di Azure - Avvio rapido
description: Questa guida di avvio rapido illustra come creare un cluster Apache Kafka in Azure HDInsight tramite il portale di Azure. Illustra inoltre gli argomenti, i sottoscrittori e i consumer di Kafka.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: mvc
ms.topic: quickstart
ms.date: 06/12/2019
ms.openlocfilehash: b66306de6b2afa1e39a91ba3b3981aec4b440e1a
ms.sourcegitcommit: fad368d47a83dadc85523d86126941c1250b14e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/19/2019
ms.locfileid: "71123576"
---
# <a name="quickstart-create-apache-kafka-cluster-in-azure-hdinsight-using-azure-portal"></a>Guida introduttiva: Creare cluster Apache Kafka in HDInsight di Azure usando il portale di Azure

Apache Kafka è una piattaforma di streaming open source distribuita. Viene spesso usata come broker di messaggi perché offre funzionalità simili a una coda messaggi di pubblicazione/sottoscrizione. 

Questa guida introduttiva illustra come creare un cluster[Apache Kafka](https://kafka.apache.org) tramite il portale di Azure. Illustra anche come usare le utilità incluse per inviare e ricevere messaggi tramite Apache Kafka.

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

Possono accedere all'API Apache Kafka solo risorse interne alla stessa rete virtuale. In questa guida di avvio rapido si accede al cluster direttamente usando SSH. Per connettere altri servizi, reti o macchine virtuali ad Apache Kafka, è necessario prima di tutto creare una rete virtuale e quindi creare le risorse all'interno della rete. Per altre informazioni, vedere il documento [Connettersi ad Apache Kafka da una rete locale](apache-kafka-connect-vpn-gateway.md).

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.

## <a name="prerequisites"></a>Prerequisiti

Un client SSH. Per altre informazioni, vedere [Connettersi a HDInsight (Apache Hadoop) con SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="create-an-apache-kafka-cluster"></a>Creare un cluster Apache Kafka

Per creare un cluster Apache Kafka in HDInsight seguire questa procedura:

1. Accedere al [portale di Azure](https://portal.azure.com).

2. Dal menu a sinistra, passare a **+ Crea una risorsa** > **Analisi** > **HDInsight**.

    ![Creazione di una risorsa HDInsight nel portale di Azure](./media/apache-kafka-get-started/create-hdinsight-cluster.png)

3. In **Informazioni di base** immettere o selezionare le informazioni seguenti:

    | Impostazione | Valore |
    | --- | --- |
    | Cluster Name | Un nome univoco per il cluster HDInsight. |
    | Subscription | Selezionare la propria sottoscrizione. |

   Selezionare __Tipo di cluster__ per visualizzare **Configurazione cluster**.

   ![Configurazione di base del cluster Apache Kafka in HDInsight](./media/apache-kafka-get-started/custom-basics-kafka1.png)

4. In __Configurazione cluster__ selezionare i valori seguenti:

    | Impostazione | Valore |
    | --- | --- |
    | Tipo di cluster | Kafka |
    | Versione | Kafka 1.1.0 (HDI 3.6) |

    Scegliere **Seleziona** per salvare le impostazioni del tipo di cluster e tornare a __Informazioni di base__.

    ![Tipo di cluster Apache Kafka di HDInsight](./media/apache-kafka-get-started/apache-kafka-cluster-type.png)

5. In __Informazioni di base__ immettere o selezionare le informazioni seguenti:

    | Impostazione | Valore |
    | --- | --- |
    | Nome utente dell'account di accesso del cluster | Il nome account di accesso quando si accede ai servizi Web o alle API REST ospitate nel cluster. Mantenere il valore predefinito (admin). |
    | Password di accesso al cluster | La password dell'account di accesso quando si accede ai servizi Web o alle API REST ospitate nel cluster. |
    | Nome utente Secure Shell (SSH) | L'account di accesso usato quando si accede al cluster su SSH. Per impostazione predefinita, la password corrisponde alla password di accesso al cluster. |
    | Gruppo di risorse | Il gruppo di risorse in cui creare il cluster. |
    | Location | Area di Azure in cui creare il cluster. |

    Ogni area (posizione) di Azure fornisce _domini di errore_. Un dominio di errore è un raggruppamento logico dell'hardware sottostante in un data center di Azure. Ogni dominio di errore condivide una fonte di alimentazione e un commutatore di rete comuni. Le macchine virtuali e i dischi gestiti che implementano i nodi in un cluster HDInsight sono distribuiti tra i domini di errore. Questa architettura limita il potenziale impatto dei guasti dell'hardware fisico.

    Per la disponibilità elevata dei dati, selezionare una posizione (area) contenente __tre domini di errore__. Per informazioni sul numero di domini di errore in un'area, vedere il documento [Disponibilità delle macchine virtuali Linux](../../virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set).

   ![Informazioni di base per la creazione del cluster nel portale di Azure](./media/apache-kafka-get-started/hdinsight-basic-configuration-2.png)

    Scegliere __Avanti__ per completare la configurazione di base.

6. Per questa guida introduttiva non modificare le impostazioni di sicurezza predefinite. Per altre informazioni su Enterprise Security Package, vedere [Configurare un cluster HDInsight con Enterprise Security Package usando Azure Active Directory Domain Services](../domain-joined/apache-domain-joined-configure-using-azure-adds.md). Per informazioni su come usare la propria chiave per la crittografia disco di Apache Kafka, vedere [Bring Your Own Key per Apache Kafka in Azure HDInsight](apache-kafka-byok.md)

   Se si vuole connettere il cluster a una rete virtuale, selezionare una rete virtuale nell'elenco a discesa **Rete virtuale**.

   ![Aggiungere il cluster a una rete virtuale](./media/apache-kafka-get-started/kafka-security-config.png)

7. In **Archiviazione** selezionare o creare un account di archiviazione. Per la procedura illustrata in questo documento, negli altri campi mantenere i valori predefiniti. Usare il pulsante __Avanti__ per salvare la configurazione della risorsa di archiviazione. Per altre informazioni sull'uso di Data Lake Storage Gen2, vedi [Avvio rapido: Impostazione dei cluster in HDInsight](../../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md)

   ![Configurare le impostazioni dell'account di archiviazione per HDInsight](./media/apache-kafka-get-started/storage-configuration.png)

8. In __Applicazioni (facoltativo)__ selezionare __Avanti__ per continuare con le impostazioni predefinite.

9. In __Dimensioni del cluster__ selezionare __Avanti__ per continuare con le impostazioni predefinite.

    Per garantire la disponibilità di Apache Kafka in HDInsight, la voce relativa al __numero di nodi di ruolo di lavoro__ deve essere impostata almeno su 3. Il valore predefinito è 4.

    La voce relativa ai **dischi per nodo di lavoro** consente di configurare la scalabilità di Apache Kafka in HDInsight. Per archiviare i dati, Apache Kafka in HDInsight usa il disco locale delle macchine virtuali nel cluster. Dal momento che in Apache Kafka i processi I/O sono intensivi, viene usato il servizio [Azure Managed Disks](../../virtual-machines/windows/managed-disks-overview.md) per assicurare una velocità effettiva elevata e fornire maggiore spazio di archiviazione per ogni nodo. Il tipo di disco gestito può essere __Standard__ (HDD) o __Premium__ (SSD). Il tipo di disco dipende dalle dimensioni della macchina virtuale usate dai nodi del ruolo di lavoro (broker Apache Kafka). I dischi Premium vengono usati con le macchine virtuali serie DS e GS. Tutti gli altri tipi di macchine virtuali usano dischi Standard.

   ![Impostare le dimensioni del cluster Apache Kafka](./media/apache-kafka-get-started/apace-kafka-cluster-size.png)

10. In __Impostazioni avanzate__ selezionare __Avanti__ per continuare con le impostazioni predefinite.

11. Nel **riepilogo** esaminare la configurazione del cluster. Usare i collegamenti __Modifica__ per cambiare eventuali impostazioni non corrette. Infine, scegliere **Crea** per creare il cluster.

    ![Riepilogo della configurazione del cluster Kafka](./media/apache-kafka-get-started/kafka-configuration-summary.png)

    La creazione del cluster può richiedere fino a 20 minuti.

## <a name="connect-to-the-cluster"></a>Connettersi al cluster

1. Per connettersi al nodo head primario del cluster Apache Kafka, usare il comando seguente. Sostituire `sshuser` con il nome utente SSH. Sostituire `mykafka` con il nome del cluster Apache Kafka.

    ```bash
    ssh sshuser@mykafka-ssh.azurehdinsight.net
    ```

2. Alla prima connessione al cluster, è possibile che il client SSH mostri un avviso relativo all'impossibilità di confermare l'autenticità dell'host. Al prompt digitare __yes__e quindi premere __INVIO__ per aggiungere l'host all'elenco dei server attendibili del client SSH.

3. Quando richiesto, immettere la password per l'utente SSH.

    Dopo avere eseguito la connessione, vengono visualizzate informazioni simili al testo seguente:
    
    ```output
    Authorized uses only. All activity may be monitored and reported.
    Welcome to Ubuntu 16.04.4 LTS (GNU/Linux 4.13.0-1011-azure x86_64)
    
     * Documentation:  https://help.ubuntu.com
     * Management:     https://landscape.canonical.com
     * Support:        https://ubuntu.com/advantage
    
      Get cloud support with Ubuntu Advantage Cloud Guest:
        https://www.ubuntu.com/business/services/cloud
    
    83 packages can be updated.
    37 updates are security updates.


    Welcome to Apache Kafka on HDInsight.
    
    Last login: Thu Mar 29 13:25:27 2018 from 108.252.109.241
    ```

## <a id="getkafkainfo"></a>Ottenere le informazioni sugli host Apache Zookeeper e broker

Quando si usa Kafka, è necessario conoscere gli host *Apache Zookeeper* e *broker*. Questi host vengono usati con l'API Apache Kafka e molte delle utilità offerte con Kafka.

In questa sezione si ottengono le informazioni sull'host dall'API REST Apache Ambari nel cluster.

1. Installare [jq](https://stedolan.github.io/jq/), un processore JSON da riga di comando. Questa utilità consente di analizzare i documenti JSON ed è utile nell'analisi delle informazioni sull'host. Dalla connessione SSH aperta, immettere il comando seguente per installare `jq`:
   
    ```bash
    sudo apt -y install jq
    ```

2. Impostare le variabili di ambiente. Sostituire `PASSWORD` e `CLUSTERNAME` rispettivamente con la password di accesso al cluster e il nome del cluster, quindi immettere il comando:

    ```bash
    export password='PASSWORD'
    export clusterNameA='CLUSTERNAME'
    ```

3. Estrarre il nome del cluster con l'uso corretto di maiuscole e minuscole. L'uso effettivo di maiuscole e minuscole nel nome del cluster può differire dal previsto, a seconda della modalità di creazione del cluster. Questo comando ottiene le maiuscole e minuscole effettive, le archivia in una variabile e quindi visualizza il nome con le maiuscole e minuscole corrette e il nome specificato in precedenza. Immettere il comando seguente:

    ```bash
    export clusterName=$(curl -u admin:$password -sS -G "https://$clusterNameA.azurehdinsight.net/api/v1/clusters" | jq -r '.items[].Clusters.cluster_name')
    echo $clusterName, $clusterNameA
    ```

4. Per impostare una variabile di ambiente con le informazioni degli host Zookeeper, usare il comando seguente. Il comando recupera tutti gli host Zookeeper, quindi restituisce solo le prime due voci. per mantenere un certo livello di ridondanza nel caso in cui un host fosse irraggiungibile.

    ```bash
    export KAFKAZKHOSTS=`curl -sS -u admin:$password -G http://headnodehost:8080/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    ```

    Questo comando esegue direttamente una query sul servizio Ambari nel nodo head del cluster. È possibile accedere ad Ambari anche usando l'indirizzo pubblico di `https://$CLUSTERNAME.azurehdinsight.net:80/`. Alcune configurazioni di rete possono impedire l'accesso all'indirizzo pubblico. È il caso ad esempio dell'uso di gruppi di sicurezza di rete (NSG) per limitare l'accesso a HDInsight in una rete virtuale.

5. Usare il comando seguente per verificare che la variabile di ambiente sia impostata correttamente:

    ```bash
    echo $KAFKAZKHOSTS
    ```

    Questo comando restituisce informazioni simili al testo seguente:

    `zk0-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk2-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181`

6. Per impostare una variabile di ambiente con le informazioni degli host broker Apache Kafka, usare il comando seguente:

    ```bash
    export KAFKABROKERS=`curl -sS -u admin:$password -G http://headnodehost:8080/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    ```

7. Usare il comando seguente per verificare che la variabile di ambiente sia impostata correttamente:

    ```bash   
    echo $KAFKABROKERS
    ```

    Questo comando restituisce informazioni simili al testo seguente:
   
    `wn1-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092,wn0-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092`

## <a name="manage-apache-kafka-topics"></a>Gestire gli argomenti di Apache Kafka

Kafka archivia i flussi di dati in *argomenti*. Per gestire gli argomenti è possibile usare l'utilità `kafka-topics.sh`.

* **Per creare un argomento** usare il comando seguente nella connessione SSH:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
    ```

    Questo comando si connette a Zookeeper usando le informazioni sull'host archiviate in `$KAFKAZKHOSTS` e quindi crea un argomento di Apache Kafka denominato **test**. 

    * I dati archiviati in questo argomento vengono divisi in otto partizioni.

    * Ogni partizione è replicata in tre nodi di ruolo di lavoro nel cluster.

        Se il cluster è stato creato in un'area di Azure con tre domini di errore, usare il fattore di replica 3. In caso contrario usare il fattore di replica 4.
        
        Nelle aree con tre domini di errore, il fattore di replica 3 consente di distribuire le repliche tra i domini di errore. Nelle aree con due domini di errore, il fattore di replica 4 distribuisce le repliche uniformemente tra i domini.
        
        Per informazioni sul numero di domini di errore in un'area, vedere il documento [Disponibilità delle macchine virtuali Linux](../../virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set).

        Apache Kafka non rileva i domini di errore di Azure. Quando si creano le repliche di partizione per gli argomenti, è possibile che le repliche non vengano distribuite in modo corretto per la disponibilità elevata.

        Per garantire la disponibilità elevata, usare lo [strumento per il ribilanciamento delle partizioni Apache Kafka](https://github.com/hdinsight/hdinsight-kafka-tools). Questo strumento deve essere eseguito da una connessione SSH al nodo head del cluster Apache Kafka.

        Per garantire la massima disponibilità dei dati Apache Kafka, è consigliabile ribilanciare le repliche di partizione per l'argomento nelle situazioni seguenti:

        * Quando si crea un nuovo argomento o una nuova partizione

        * Quando si aumentano le prestazioni di un cluster

* **Per elencare gli argomenti** usare il comando seguente:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
    ```

    Questo comando consente di elencare gli argomenti disponibili nel cluster Apache Kafka.

* **Per eliminare un argomento** usare il comando seguente:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --delete --topic topicname --zookeeper $KAFKAZKHOSTS
    ```

    Questo comando elimina l'argomento denominato `topicname`.

    > [!WARNING]  
    > Se si elimina l'argomento `test` creato in precedenza, è necessario crearlo di nuovo. Questo argomento si userà in passaggi indicati più avanti in questo documento.

Per altre informazioni sui comandi disponibili con l'utilità `kafka-topics.sh`, usare il comando seguente:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh
```

## <a name="produce-and-consume-records"></a>Produrre e utilizzare record

Kafka archivia i *record* negli argomenti. I record vengono prodotti da *producer* e usati da *consumer*. I producer e i consumer comunicano con il servizio *broker Kafka*. Ogni nodo del ruolo di lavoro nel cluster HDInsight è un host del broker Apache Kafka.

Seguire questa procedura per archiviare i record nell'argomento test creato in precedenza e quindi leggerli usando un consumer:

1. Per scrivere i record nell'argomento usare l'utilità `kafka-console-producer.sh` dalla connessione SSH:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test
    ```
   
    Dopo questo comando viene visualizzata una riga vuota.

2. Digitare un messaggio di testo nella riga vuota e premere INVIO. Digitare invece alcuni messaggi in questo modo e quindi usare **Ctrl + C** per tornare al prompt normale. Ogni riga viene inviata come record distinto all'argomento Apache Kafka.

3. Per scrivere i record dall'argomento usare l'utilità `kafka-console-consumer.sh` dalla connessione SSH:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic test --from-beginning
    ```

    Questo comando recupera i record dall'argomento e li visualizza. L'uso di `--from-beginning` indica al consumer di partire dall'inizio del flusso, quindi verranno recuperati tutti i record.

    Se si usa una versione meno recente di Kafka, sostituire `--bootstrap-server $KAFKABROKERS` con `--zookeeper $KAFKAZKHOSTS`.

4. Usare __Ctrl + C__ per arrestare il consumer.

È possibile creare producer e consumer anche a livello di codice. Per un esempio dell'uso di questa API, vedere il documento relativo alle [API Producer e Consumer Apache Kafka con HDInsight](apache-kafka-producer-consumer-api.md).

## <a name="clean-up-resources"></a>Pulire le risorse

Per pulire le risorse create da questa guida introduttiva, è possibile eliminare il gruppo di risorse. Se si elimina il gruppo di risorse, vengono eliminati anche il cluster HDInsight associato e tutte le altre risorse correlate al gruppo di risorse.

Per rimuovere il gruppo di risorse usando il portale di Azure:

1. Nel portale di Azure espandere il menu a sinistra per aprire il menu dei servizi e quindi scegliere __Gruppi di risorse__ per visualizzare l'elenco dei gruppi di risorse.
2. Individuare il gruppo di risorse da eliminare e quindi fare clic con il pulsante destro del mouse su __Altro__ (...) a destra dell'elenco.
3. Scegliere __Elimina gruppo di risorse__ e quindi confermare.

> [!WARNING]  
> La fatturazione del cluster HDInsight inizia dopo la creazione del cluster e si interrompe solo quando questo viene eliminato. La fatturazione avviene con tariffa oraria, perciò si deve sempre eliminare il cluster in uso quando non lo si usa più.
> 
> Se si elimina un cluster Apache Kafka in HDInsight, vengono eliminati anche eventuali dati archiviati in Kafka.

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Usare Apache Spark con Apache Kafka](../hdinsight-apache-kafka-spark-structured-streaming.md)
