---
title: Scenari supportati SAP HANA in Azure (istanze Large) | Microsoft Docs
description: Scenari supportati e relativi dettagli di architettura per SAP HANA in Azure (istanze Large)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/26/2019
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7ed63f5caa6b1f1c0072a92f6a60ad43c5431af0
ms.sourcegitcommit: 36eb583994af0f25a04df29573ee44fbe13bd06e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/26/2019
ms.locfileid: "74538337"
---
# <a name="supported-scenarios-for-hana-large-instances"></a>Scenari supportati nelle istanze Large di HANA
Questo documento descrive gli scenari supportati e i dettagli delle relative architettura per istanze Large di HANA (HLI).

>[!NOTE]
>Se lo scenario richiesto non è indicato di seguito, contattare il team Microsoft Service Management per valutare i requisiti.
Prima di procedere con il provisioning dell'unità HLI, verificare il progetto con SAP o il partner che si occupa dell'implementazione del servizio.

## <a name="terms-and-definitions"></a>Termini e definizioni
È utile comprendere i termini e le definizioni utilizzate nel documento.

- SID: identificatore del sistema HANA.
- HLI: istanze Large di HANA.
- Ripristino di emergenza: sito di ripristino di emergenza.
- Ripristino di emergenza normale: installazione di sistema con una risorsa dedicata utilizzata solo per di ripristino di emergenza.
- Ripristino di emergenza multifunzione: sistema nel sito di ripristino di emergenza configurato per l'utilizzo di un ambiente non di produzione insieme all'istanza di produzione configurata per l'utilizzo dell'evento di ripristino di emergenza. 
- SID singolo: sistema con una sola istanza installata.
- SID multiplo: sistema con più istanze configurate. Definito anche ambiente MCOS.
- HSR: SAP HANA replica di sistema.

## <a name="overview"></a>Panoramica
Le istanze Large di HANA supportano una gamma di architetture per soddisfare svariati requisiti aziendali. Nell'elenco seguente vengono illustrati gli scenari e i relativi dettagli di configurazione. 

Il progetto dell'architettura derivata rispetta unicamente il punto di vista dell'infrastruttura. Per la distribuzione HANA è necessario consultare SAP o i partner di implementazione. Se i propri scenari non sono elencati, contattare il team degli account Microsoft per esaminare l'architettura e derivare una soluzione su misura.

**Queste architetture sono pienamente compatibili con la progettazione TDI (integrazione di dati personalizzati) e supportate da SAP.**

In questo documento vengono descritti i dettagli dei due componenti in ognuna delle architetture supportate:

- Ethernet
- Archiviazione

### <a name="ethernet"></a>Ethernet

Ogni server fornito è preconfigurato con i set di interfacce Ethernet. Di seguito i dettagli delle interfacce Ethernet configurate in ogni unità HLI.

- **A**: interfaccia usata per/dall'accesso al client.
- **B**: interfaccia usata per la comunicazione da nodo a nodo. Questa interfaccia è configurata in tutti i server, indipendentemente dalla topologia richiesta, ma è utilizzata solo per gli 
- scenari scale-out.
- **C**: interfaccia usata per il nodo di connettività dell'archiviazione.
- **D**: interfaccia usata per il nodo di connessione del dispositivo ISCSI per l'installazione STONITH. Questa interfaccia viene configurata solo quando è richiesta l'installazione HSR.  

| INTERFACCE LOGICHE NIC | TIPO SKU | Nome con sistema operativo SUSE | Nome con sistema operativo RHEL | Caso d'uso|
| --- | --- | --- | --- | --- |
| A | TIPO I | eth0.tenant | eno1.tenant | Da client a HLI |
| b | TIPO I | eth2.tenant | eno3.tenant | Da nodo a nodo |
| C | TIPO I | eth1.tenant | eno2.tenant | Da nodo ad archiviazione |
| D | TIPO I | eth4.tenant | eno4.tenant | STONITH |
| A | TIPO II | VLAN\<tenantNo > | team0.tenant | Da client a HLI |
| b | TIPO II | VLAN\<tenantNo + 2 > | team0.tenant+2 | Da nodo a nodo |
| C | TIPO II | VLAN\<tenantNo + 1 > | team0.tenant+1 | Da nodo ad archiviazione |
| D | TIPO II | VLAN\<tenantNo + 3 > | team0.tenant+3 | STONITH |

Utilizzare le interfacce sulla base della topologia configurata nell'unità HLI. L'interfaccia "B", ad esempio, è impostata per la comunicazione da nodo a nodo, che risulta utile quando è stata configurata una topologia con scalabilità orizzontale. Per le configurazioni a scalabilità verticale con un singolo nodo, questa interfaccia non viene usata. Esaminare gli scenari richiesti, indicati più avanti in questo documento, per ottenere ulteriori informazioni sull'utilizzo dell'interfaccia. 

Se necessario, l'utente può definire altre schede di rete. Tuttavia, la configurazione delle schede di rete esistenti non può essere modificata.

>[!NOTE]
>È possibile comunque trovare interfacce aggiuntive di tipo fisico o associativo. Prendere in considerazione le interfacce sopra indicate per il proprio caso d'uso; le restanti possono essere ignorate o non devono essere modificate.

La distribuzione per le unità con due indirizzi IP assegnati dovrebbe appare come segue:

- A Ethernet "A" deve essere assegnato un indirizzo IP non compreso nell'intervallo di indirizzi del pool di indirizzi IP del server inviati a Microsoft. Questo indirizzo IP deve essere usato per la gestione in /etc/hosts del sistema operativo.

- A Ethernet "C" deve essere assegnato un indirizzo IP utilizzato per la comunicazione con NFS. Pertanto, tali indirizzi **NON** devono essere gestiti in etc/hosts per consentire il traffico da istanza a istanza nel tenant.

Una configurazione del pannello con due indirizzi IP assegnati non è appropriata per i casi di distribuzione di tipo replica del sistema HANA o di HANA con scalabilità orizzontale. Se sono stati assegnati solo due indirizzi IP e si vuole distribuire una configurazione di questo tipo, contattare SAP HANA in Azure Service Management per ottenere un terzo indirizzo IP in una terza VLAN assegnata. Per le unità di istanza Large di HANA con tre indirizzi IP assegnati alle tre porte NIC, si applicano le regole di utilizzo seguenti:

- A Ethernet "A" deve essere assegnato un indirizzo IP non compreso nell'intervallo di indirizzi del pool di indirizzi IP del server inviati a Microsoft. Questo indirizzo, pertanto, IP deve non essere usato per la gestione in /etc/hosts del sistema operativo.

- Ethernet "B" deve essere utilizzata esclusivamente in etc/hosts per la comunicazione tra istanze diverse. Questi indirizzi corrispondono agli indirizzi IP da mantenere nelle configurazioni HANA con scalabilità orizzontale come indirizzi IP usati da HANA per la configurazione tra nodi.

- A Ethernet "C" deve essere assegnato un indirizzo IP utilizzato per la comunicazione con l'archiviazione NFS. Questo tipo di indirizzi, quindi, non deve essere conservato in etc/host.

- Ethernet "D" deve essere usata esclusivamente per accedere al dispositivo STONITH per pacemaker. Questa interfaccia è necessaria per la configurazione di HSR (HANA System Replication) e per realizzare il failover automatico sul sistema operativo usando un dispositivo basato su SBD.


### <a name="storage"></a>Archiviazione
L'archiviazione è preconfigurata e si basa sulla topologia richiesta. Le dimensioni del volume e il punto di montaggio variano in base alla topologia e al numero di server e SKU configurati. Esaminare gli scenari richiesti, indicati più avanti in questo documento, per ottenere altre informazioni. Se è necessario disporre di ulteriore spazio di archiviazione, è possibile acquistare incrementi di un TB.

>[!NOTE]
>Il mountpoint/usr/SAP/\<SID > è un collegamento simbolico a/Hana/Shared mountpoint.


## <a name="supported-scenarios"></a>Scenari Supportati

Nei diagrammi dell'architettura, per la grafica vengono utilizzate le notazioni seguenti:

[![Legends. PNG](media/hana-supported-scenario/Legends.png)](media/hana-supported-scenario/Legends.png#lightbox)

L'elenco seguente mostra gli scenari supportati:

1. Nodo singolo con un SID
2. Nodo singolo MCOS
3. Nodo singolo con ripristino di emergenza (normale)
4. Nodo singolo con ripristino di emergenza (multifunzione)
5. HSR con STONITH
6. HSR con ripristino di emergenza (normale/multifunzione) 
7. Failover automatico dell'host (1+1) 
8. Scale-out con standby
9. Scale-out senza standby
10. Scale-out con ripristino di emergenza



## <a name="1-single-node-with-one-sid"></a>1. singolo nodo con un SID

Questa topologia supporta un nodo in una configurazione a scalabilità verticale con un SID.

### <a name="architecture-diagram"></a>Diagramma dell'architettura  

![Single-node-with-one-SID.png](media/hana-supported-scenario/Single-node-with-one-SID.png)

### <a name="ethernet"></a>Ethernet
Sono preconfigurate le interfacce di rete seguenti:

| INTERFACCE LOGICHE NIC | TIPO SKU | Nome con sistema operativo SUSE | Nome con sistema operativo RHEL | Caso d'uso|
| --- | --- | --- | --- | --- |
| A | TIPO I | eth0.tenant | eno1.tenant | Da client a HLI |
| b | TIPO I | eth2.tenant | eno3.tenant | Configurato ma non in uso |
| C | TIPO I | eth1.tenant | eno2.tenant | Da nodo ad archiviazione |
| D | TIPO I | eth4.tenant | eno4.tenant | Configurato ma non in uso |
| A | TIPO II | VLAN\<tenantNo > | team0.tenant | Da client a HLI |
| b | TIPO II | VLAN\<tenantNo + 2 > | team0.tenant+2 | Configurato ma non in uso |
| C | TIPO II | VLAN\<tenantNo + 1 > | team0.tenant+1 | Da nodo ad archiviazione |
| D | TIPO II | VLAN\<tenantNo + 3 > | team0.tenant+3 | Configurato ma non in uso |

### <a name="storage"></a>Archiviazione
I punti di montaggio seguenti sono preconfigurati:

| Punto di montaggio | Caso d'uso | 
| --- | --- |
|/hana/shared/SID | Installazione HANA | 
|/hana/data/SID/mnt00001 | Installazione di file di dati | 
|/hana/log/SID/mnt00001 | Installazione di file di log | 
|/hana/logbackups/SID | Log di rollforward |

### <a name="key-considerations"></a>Considerazioni sulle chiavi
- /usr/SAP/SID è un collegamento simbolico a /hana/shared/SID.

## <a name="2-single-node-mcos"></a>2. MCOS a nodo singolo

Questa topologia supporta un nodo in una configurazione a scalabilità verticale con vari SID.

### <a name="architecture-diagram"></a>Diagramma dell'architettura  

![single-node-mcos.png](media/hana-supported-scenario/single-node-mcos.png)

### <a name="ethernet"></a>Ethernet
Sono preconfigurate le interfacce di rete seguenti:

| INTERFACCE LOGICHE NIC | TIPO SKU | Nome con sistema operativo SUSE | Nome con sistema operativo RHEL | Caso d'uso|
| --- | --- | --- | --- | --- |
| A | TIPO I | eth0.tenant | eno1.tenant | Da client a HLI |
| b | TIPO I | eth2.tenant | eno3.tenant | Configurato ma non in uso |
| C | TIPO I | eth1.tenant | eno2.tenant | Da nodo ad archiviazione |
| D | TIPO I | eth4.tenant | eno4.tenant | Configurato ma non in uso |
| A | TIPO II | VLAN\<tenantNo > | team0.tenant | Da client a HLI |
| b | TIPO II | VLAN\<tenantNo + 2 > | team0.tenant+2 | Configurato ma non in uso |
| C | TIPO II | VLAN\<tenantNo + 1 > | team0.tenant+1 | Da nodo ad archiviazione |
| D | TIPO II | VLAN\<tenantNo + 3 > | team0.tenant+3 | Configurato ma non in uso |

### <a name="storage"></a>Archiviazione
I punti di montaggio seguenti sono preconfigurati:

| Punto di montaggio | Caso d'uso | 
| --- | --- |
|/hana/shared/SID1 | Installazione HANA per SID1 | 
|/hana/data/SID1/mnt00001 | Installazione di file di dati per SID1 | 
|/hana/log/SID1/mnt00001 | Installazione di file di log per SID1 | 
|/hana/logbackups/SID1 | Log di rollforward per SID1 |
|/hana/shared/SID2 | Installazione HANA per SID2 | 
|/hana/data/SID2/mnt00001 | Installazione di file di dati per SID2 | 
|/hana/log/SID2/mnt00001 | Installazione di file di log per SID2 | 
|/hana/logbackups/SID2 | Log di rollforward per SID2 |

### <a name="key-considerations"></a>Considerazioni sulle chiavi
- /usr/SAP/SID è un collegamento simbolico a /hana/shared/SID.
- La distribuzione delle dimensioni del volume è basata sulle dimensioni del database in memoria. Consultare la sezione [Panoramica e architettura](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) per conoscere quali dimensioni del database in memoria sono supportate in un ambiente multisid.

## <a name="3-single-node-with-dr-using-storage-replication"></a>3. singolo nodo con ripristino di emergenza tramite replica di archiviazione
 
Questa topologia supporta un nodo in una configurazione a scalabilità verticale con uno o più SID e la replica basata sull'archiviazione nel sito di ripristino di emergenza per un SID primario. Nel diagramma, in corrispondenza del sito primario, viene rappresentato solo il SID singolo, ma è supportato anche multisid (MCOS).

### <a name="architecture-diagram"></a>Diagramma dell'architettura  

![Single-node-with-dr.png](media/hana-supported-scenario/Single-node-with-dr.png)

### <a name="ethernet"></a>Ethernet
Sono preconfigurate le interfacce di rete seguenti:

| INTERFACCE LOGICHE NIC | TIPO SKU | Nome con sistema operativo SUSE | Nome con sistema operativo RHEL | Caso d'uso|
| --- | --- | --- | --- | --- |
| A | TIPO I | eth0.tenant | eno1.tenant | Da client a HLI |
| b | TIPO I | eth2.tenant | eno3.tenant | Configurato ma non in uso |
| C | TIPO I | eth1.tenant | eno2.tenant | Da nodo ad archiviazione |
| D | TIPO I | eth4.tenant | eno4.tenant | Configurato ma non in uso |
| A | TIPO II | VLAN\<tenantNo > | team0.tenant | Da client a HLI |
| b | TIPO II | VLAN\<tenantNo + 2 > | team0.tenant+2 | Configurato ma non in uso |
| C | TIPO II | VLAN\<tenantNo + 1 > | team0.tenant+1 | Da nodo ad archiviazione |
| D | TIPO II | VLAN\<tenantNo + 3 > | team0.tenant+3 | Configurato ma non in uso |

### <a name="storage"></a>Archiviazione
I punti di montaggio seguenti sono preconfigurati:

| Punto di montaggio | Caso d'uso | 
| --- | --- |
|/hana/shared/SID | Installazione HANA per SID | 
|/hana/data/SID/mnt00001 | Installazione di file di dati per SID | 
|/hana/log/SID/mnt00001 | Installazione di file di log per SID | 
|/hana/logbackups/SID | Log di rollforward per SID |


### <a name="key-considerations"></a>Considerazioni sulle chiavi
- /usr/SAP/SID è un collegamento simbolico a /hana/shared/SID.
- Per MCOS: la distribuzione delle dimensioni del volume è basata sulle dimensioni del database in memoria. Consultare la sezione [Panoramica e architettura](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) per conoscere quali dimensioni del database in memoria sono supportate in un ambiente multisid.
- Nel ripristino di emergenza: i volumi e i punti di montaggio sono configurati, vale a dire contrassegnati come "Required for HANA installation" (Obbligatori per l'installazione HANA) per l'installazione dell'istanza HANA di produzione nell'unità HLI di ripristino di emergenza. 
- Nel ripristino di emergenza: i dati, i backup dei file di log e i volumi condivisi, contrassegnati come "Replica di archiviazione", vengono replicati tramite snapshot dal sito di produzione. Questi volumi sono montati solo durante il tempo di failover. Per altre informazioni, leggere il documento [Procedura di failover di ripristino di emergenza](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery).
- Il volume di avvio della **classe SKU di Tipo I** è replicato nel nodo ripristino di emergenza.


## <a name="4-single-node-with-dr-multipurpose-using-storage-replication"></a>4. singolo nodo con ripristino di emergenza (Multipurpose) con replica di archiviazione
 
Questa topologia supporta un nodo in una configurazione a scalabilità verticale con uno o più SID e la replica basata sull'archiviazione nel sito di ripristino di emergenza per un SID primario. Nel diagramma, in corrispondenza del sito primario, viene rappresentato solo il SID singolo, ma è supportato anche multisid (MCOS). Nel sito di ripristino di emergenza l'unità HLI viene utilizzata per l'istanza QA mentre le operazioni di produzione vengono eseguite nel sito primario. Al momento del failover del ripristino di emergenza (o test di failover), l'istanza QA del sito di ripristino di emergenza viene disattivata.

### <a name="architecture-diagram"></a>Diagramma dell'architettura  

![single-node-with-dr-multipurpose.png](media/hana-supported-scenario/single-node-with-dr-multipurpose.png)

### <a name="ethernet"></a>Ethernet
Sono preconfigurate le interfacce di rete seguenti:

| INTERFACCE LOGICHE NIC | TIPO SKU | Nome con sistema operativo SUSE | Nome con sistema operativo RHEL | Caso d'uso|
| --- | --- | --- | --- | --- |
| A | TIPO I | eth0.tenant | eno1.tenant | Da client a HLI |
| b | TIPO I | eth2.tenant | eno3.tenant | Configurato ma non in uso |
| C | TIPO I | eth1.tenant | eno2.tenant | Da nodo ad archiviazione |
| D | TIPO I | eth4.tenant | eno4.tenant | Configurato ma non in uso |
| A | TIPO II | VLAN\<tenantNo > | team0.tenant | Da client a HLI |
| b | TIPO II | VLAN\<tenantNo + 2 > | team0.tenant+2 | Configurato ma non in uso |
| C | TIPO II | VLAN\<tenantNo + 1 > | team0.tenant+1 | Da nodo ad archiviazione |
| D | TIPO II | VLAN\<tenantNo + 3 > | team0.tenant+3 | Configurato ma non in uso |

### <a name="storage"></a>Archiviazione
I punti di montaggio seguenti sono preconfigurati:

| Punto di montaggio | Caso d'uso | 
| --- | --- |
|**Nel sito primario**|
|/hana/shared/SID | Installazione HANA per SID di produzione | 
|/hana/data/SID/mnt00001 | Installazione di file di dati per SID di produzione | 
|/hana/log/SID/mnt00001 | Installazione di file di log per SID di produzione | 
|/hana/logbackups/SID | Log di rollforward per SID di produzione |
|**Nel sito di ripristino di emergenza**|
|/hana/shared/SID | Installazione HANA per SID di produzione | 
|/hana/data/SID/mnt00001 | Installazione di file di dati per SID di produzione | 
|/hana/log/SID/mnt00001 | Installazione di file di log per SID di produzione | 
|/hana/shared/QA-SID | Installazione HANA per SID QA | 
|/hana/data/QA-SID/mnt00001 | Installazione di file di dati per SID QA | 
|/hana/log/QA-SID/mnt00001 | Installazione di file di log per SID QA |
|/hana/logbackups/QA-SID | Log di rollforward per SID QA |

### <a name="key-considerations"></a>Considerazioni sulle chiavi
- /usr/SAP/SID è un collegamento simbolico a /hana/shared/SID.
- Per MCOS: la distribuzione delle dimensioni del volume è basata sulle dimensioni del database in memoria. Consultare la sezione [Panoramica e architettura](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) per conoscere quali dimensioni del database in memoria sono supportate in un ambiente multisid.
- Nel ripristino di emergenza: i volumi e i punti di montaggio sono configurati, vale a dire contrassegnati come "Required for HANA installation" (Obbligatori per l'installazione HANA) per l'installazione dell'istanza HANA di produzione nell'unità HLI di ripristino di emergenza. 
- Nel ripristino di emergenza: i dati, i backup dei file di log e i volumi condivisi, contrassegnati come "Replica di archiviazione", vengono replicati tramite snapshot dal sito di produzione. Questi volumi sono montati solo durante il tempo di failover. Per altre informazioni, leggere il documento [Procedura di failover di ripristino di emergenza](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery). 
- Nel ripristino di emergenza: i dati, i backup dei file di log, i log e i volumi condivisi per QA, contrassegnati come “QA Instance installation” (Installazione dell'istanza QA) sono configurati per l'installazione dell'istanza QA.
- Il volume di avvio della **classe SKU di Tipo I** è replicato nel nodo ripristino di emergenza.

## <a name="5-hsr-with-stonith-for-high-availability"></a>5. HSR con STONITH per la disponibilità elevata
 
Questa topologia supporta due nodi per la configurazione HSR (HANA System Replication). Questa configurazione è supportata solo per singole istanze HANA in un nodo. Gli scenari MCOS NON sono supportati.

**A partire da ora, questa architettura è supportata solo per il sistema operativo SUSE.**


### <a name="architecture-diagram"></a>Diagramma dell'architettura  

![HSR-with-STONITH.png](media/hana-supported-scenario/HSR-with-STONITH.png)



### <a name="ethernet"></a>Ethernet
Sono preconfigurate le interfacce di rete seguenti:

| INTERFACCE LOGICHE NIC | TIPO SKU | Nome con sistema operativo SUSE | Nome con sistema operativo RHEL | Caso d'uso|
| --- | --- | --- | --- | --- |
| A | TIPO I | eth0.tenant | eno1.tenant | Da client a HLI |
| b | TIPO I | eth2.tenant | eno3.tenant | Configurato ma non in uso |
| C | TIPO I | eth1.tenant | eno2.tenant | Da nodo ad archiviazione |
| D | TIPO I | eth4.tenant | eno4.tenant | Usato per STONITH |
| A | TIPO II | VLAN\<tenantNo > | team0.tenant | Da client a HLI |
| b | TIPO II | VLAN\<tenantNo + 2 > | team0.tenant+2 | Configurato ma non in uso |
| C | TIPO II | VLAN\<tenantNo + 1 > | team0.tenant+1 | Da nodo ad archiviazione |
| D | TIPO II | VLAN\<tenantNo + 3 > | team0.tenant+3 | Usato per STONITH |

### <a name="storage"></a>Archiviazione
I punti di montaggio seguenti sono preconfigurati:

| Punto di montaggio | Caso d'uso | 
| --- | --- |
|**Nel nodo primario**|
|/hana/shared/SID | Installazione HANA per SID di produzione | 
|/hana/data/SID/mnt00001 | Installazione di file di dati per SID di produzione | 
|/hana/log/SID/mnt00001 | Installazione di file di log per SID di produzione | 
|/hana/logbackups/SID | Log di rollforward per SID di produzione |
|**Nel nodo secondario**|
|/hana/shared/SID | Installazione HANA per SID secondario | 
|/hana/data/SID/mnt00001 | Installazione di file di dati per SID secondario | 
|/hana/log/SID/mnt00001 | Installazione di file di log per SID secondario | 
|/hana/logbackups/SID | Log di rollforward per SID secondario |

### <a name="key-considerations"></a>Considerazioni sulle chiavi
- /usr/SAP/SID è un collegamento simbolico a /hana/shared/SID.
- Per MCOS: la distribuzione delle dimensioni del volume è basata sulle dimensioni del database in memoria. Consultare la sezione [Panoramica e architettura](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) per conoscere quali dimensioni del database in memoria sono supportate in un ambiente multisid.
- STONITH: per l'installazione STONITH è configurato un SBD. Tuttavia, l'uso di STONITH è facoltativo.


## <a name="6-high-availability-with-hsr-and-dr-with-storage-replication"></a>6. disponibilità elevata con HSR e ripristino di emergenza con replica di archiviazione
 
Questa topologia supporta due nodi per la configurazione HSR (HANA System Replication). È supportato sia il ripristino di emergenza normale che multifunzione. Queste configurazioni sono supportate solo per singole istanze HANA in un nodo. Pertanto gli scenari MCOS non sono supportati con queste configurazioni.

Nel diagramma è illustrato lo scenario multifunzione nel sito di ripristino di emergenza dove l'unità HLI viene utilizzata per l'istanza QA mentre le operazioni di produzione vengono eseguite nel sito primario. Al momento del failover del ripristino di emergenza (o test di failover), l'istanza QA del sito di ripristino di emergenza viene disattivata. 



### <a name="architecture-diagram"></a>Diagramma dell'architettura  

![HSR-with-DR.png](media/hana-supported-scenario/HSR-with-DR.png)

### <a name="ethernet"></a>Ethernet
Sono preconfigurate le interfacce di rete seguenti:

| INTERFACCE LOGICHE NIC | TIPO SKU | Nome con sistema operativo SUSE | Nome con sistema operativo RHEL | Caso d'uso|
| --- | --- | --- | --- | --- |
| A | TIPO I | eth0.tenant | eno1.tenant | Da client a HLI |
| b | TIPO I | eth2.tenant | eno3.tenant | Configurato ma non in uso |
| C | TIPO I | eth1.tenant | eno2.tenant | Da nodo ad archiviazione |
| D | TIPO I | eth4.tenant | eno4.tenant | Usato per STONITH |
| A | TIPO II | VLAN\<tenantNo > | team0.tenant | Da client a HLI |
| b | TIPO II | VLAN\<tenantNo + 2 > | team0.tenant+2 | Configurato ma non in uso |
| C | TIPO II | VLAN\<tenantNo + 1 > | team0.tenant+1 | Da nodo ad archiviazione |
| D | TIPO II | VLAN\<tenantNo + 3 > | team0.tenant+3 | Usato per STONITH |

### <a name="storage"></a>Archiviazione
I punti di montaggio seguenti sono preconfigurati:

| Punto di montaggio | Caso d'uso | 
| --- | --- |
|**Nel nodo primario del sito primario**|
|/hana/shared/SID | Installazione HANA per SID di produzione | 
|/hana/data/SID/mnt00001 | Installazione di file di dati per SID di produzione | 
|/hana/log/SID/mnt00001 | Installazione di file di log per SID di produzione | 
|/hana/logbackups/SID | Log di rollforward per SID di produzione |
|**Nel nodo secondario del sito primario**|
|/hana/shared/SID | Installazione HANA per SID secondario | 
|/hana/data/SID/mnt00001 | Installazione di file di dati per SID secondario | 
|/hana/log/SID/mnt00001 | Installazione di file di log per SID secondario | 
|/hana/logbackups/SID | Log di rollforward per SID secondario |
|**Nel sito di ripristino di emergenza**|
|/hana/shared/SID | Installazione HANA per SID di produzione | 
|/hana/data/SID/mnt00001 | Installazione di file di dati per SID di produzione | 
|/hana/log/SID/mnt00001 | Installazione di file di log per SID di produzione | 
|/hana/shared/QA-SID | Installazione HANA per SID QA | 
|/hana/data/QA-SID/mnt00001 | Installazione di file di dati per SID QA | 
|/hana/log/QA-SID/mnt00001 | Installazione di file di log per SID QA |
|/hana/logbackups/QA-SID | Log di rollforward per SID QA |

### <a name="key-considerations"></a>Considerazioni sulle chiavi
- /usr/SAP/SID è un collegamento simbolico a /hana/shared/SID.
- Per MCOS: la distribuzione delle dimensioni del volume è basata sulle dimensioni del database in memoria. Consultare la sezione [Panoramica e architettura](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) per conoscere quali dimensioni del database in memoria sono supportate in un ambiente multisid.
- STONITH: per l'installazione STONITH è configurato un SBD. Tuttavia, l'uso di STONITH è facoltativo.
- Nel di ripristino di emergenza: **sono necessari due gruppi di volumi di archiviazione** per la replica del nodo primario e secondario.
- Nel ripristino di emergenza: i volumi e i punti di montaggio sono configurati, vale a dire contrassegnati come "Required for HANA installation" (Obbligatori per l'installazione HANA) per l'installazione dell'istanza HANA di produzione nell'unità HLI di ripristino di emergenza. 
- Nel ripristino di emergenza: i dati, i backup dei file di log e i volumi condivisi, contrassegnati come "Replica di archiviazione", vengono replicati tramite snapshot dal sito di produzione. Questi volumi sono montati solo durante il tempo di failover. Per altre informazioni, leggere il documento [Procedura di failover di ripristino di emergenza](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery). 
- Nel ripristino di emergenza: i dati, i backup dei file di log, i log e i volumi condivisi per QA, contrassegnati come “QA Instance installation” (Installazione dell'istanza QA) sono configurati per l'installazione dell'istanza QA.
- Il volume di avvio della **classe SKU di Tipo I** è replicato nel nodo ripristino di emergenza.


## <a name="7-host-auto-failover-11"></a>7. failover automatico dell'host (1 + 1)
 
Questa topologia supporta due nodi in una configurazione di failover automatico dell'host. È presente un nodo con il ruolo di master/di lavoro e un altro di standby. **SAP supporta questo scenario solo per HANA S/4.** Fare riferimento alla nota sui sistemi operativi "[2408419 - SAP S/4HANA - Multi-Node Support](https://launchpad.support.sap.com/#/notes/2408419)" per ulteriori dettagli.



### <a name="architecture-diagram"></a>Diagramma dell'architettura  

![sca](media/hana-supported-scenario/scaleup-with-standby.png)

### <a name="ethernet"></a>Ethernet
Sono preconfigurate le interfacce di rete seguenti:

| INTERFACCE LOGICHE NIC | TIPO SKU | Nome con sistema operativo SUSE | Nome con sistema operativo RHEL | Caso d'uso|
| --- | --- | --- | --- | --- |
| A | TIPO I | eth0.tenant | eno1.tenant | Da client a HLI |
| b | TIPO I | eth2.tenant | eno3.tenant | Comunicazione da nodo a nodo |
| C | TIPO I | eth1.tenant | eno2.tenant | Da nodo ad archiviazione |
| D | TIPO I | eth4.tenant | eno4.tenant | Configurato ma non in uso |
| A | TIPO II | VLAN\<tenantNo > | team0.tenant | Da client a HLI |
| b | TIPO II | VLAN\<tenantNo + 2 > | team0.tenant+2 | Comunicazione da nodo a nodo |
| C | TIPO II | VLAN\<tenantNo + 1 > | team0.tenant+1 | Da nodo ad archiviazione |
| D | TIPO II | VLAN\<tenantNo + 3 > | team0.tenant+3 | Configurato ma non in uso |

### <a name="storage"></a>Archiviazione
I punti di montaggio seguenti sono preconfigurati:

| Punto di montaggio | Caso d'uso | 
| --- | --- |
|**Nei nodi master e standby**|
|/hana/shared | Installazione HANA per SID di produzione | 
|/hana/data/SID/mnt00001 | Installazione di file di dati per SID di produzione | 
|/hana/log/SID/mnt00001 | Installazione di file di log per SID di produzione | 
|/hana/logbackups/SID | Log di rollforward per SID di produzione |



### <a name="key-considerations"></a>Considerazioni sulle chiavi
- /usr/SAP/SID è un collegamento simbolico a /hana/shared/SID.
- In standby: i volumi e i punti di montaggio sono configurati, vale a dire contrassegnati come "Required for HANA installation" (Obbligatori per l'installazione HANA) per l'installazione dell'istanza HANA nell'unità standby.
 

## <a name="8-scale-out-with-standby"></a>8. scalabilità orizzontale con standby
 
Questa topologia supporta più nodi in una configurazione scale-out. È presente un nodo con ruolo master, uno o più nodi con ruolo di lavoro e uno o più nodi di standby. In ogni dato momento è tuttavia ammesso un solo nodo master.


### <a name="architecture-diagram"></a>Diagramma dell'architettura  

![scaleout-nm-standby.png](media/hana-supported-scenario/scaleout-nm-standby.png)

### <a name="ethernet"></a>Ethernet
Sono preconfigurate le interfacce di rete seguenti:

| INTERFACCE LOGICHE NIC | TIPO SKU | Nome con sistema operativo SUSE | Nome con sistema operativo RHEL | Caso d'uso|
| --- | --- | --- | --- | --- |
| A | TIPO I | eth0.tenant | eno1.tenant | Da client a HLI |
| b | TIPO I | eth2.tenant | eno3.tenant | Comunicazione da nodo a nodo |
| C | TIPO I | eth1.tenant | eno2.tenant | Da nodo ad archiviazione |
| D | TIPO I | eth4.tenant | eno4.tenant | Configurato ma non in uso |
| A | TIPO II | VLAN\<tenantNo > | team0.tenant | Da client a HLI |
| b | TIPO II | VLAN\<tenantNo + 2 > | team0.tenant+2 | Comunicazione da nodo a nodo |
| C | TIPO II | VLAN\<tenantNo + 1 > | team0.tenant+1 | Da nodo ad archiviazione |
| D | TIPO II | VLAN\<tenantNo + 3 > | team0.tenant+3 | Configurato ma non in uso |

### <a name="storage"></a>Archiviazione
I punti di montaggio seguenti sono preconfigurati:

| Punto di montaggio | Caso d'uso | 
| --- | --- |
|**Nei nodi master, di lavoro e standby**|
|/hana/shared | Installazione HANA per SID di produzione | 
|/hana/data/SID/mnt00001 | Installazione di file di dati per SID di produzione | 
|/hana/log/SID/mnt00001 | Installazione di file di log per SID di produzione | 
|/hana/logbackups/SID | Log di rollforward per SID di produzione |


## <a name="9-scale-out-without-standby"></a>9. scalabilità orizzontale senza standby
 
Questa topologia supporta più nodi in una configurazione scale-out. È presente un nodo con ruolo di master e uno o più nodi con ruolo di lavoro. In ogni dato momento è tuttavia ammesso un solo nodo master.


### <a name="architecture-diagram"></a>Diagramma dell'architettura  

![scaleout-nm.png](media/hana-supported-scenario/scaleout-nm.png)


### <a name="ethernet"></a>Ethernet
Sono preconfigurate le interfacce di rete seguenti:

| INTERFACCE LOGICHE NIC | TIPO SKU | Nome con sistema operativo SUSE | Nome con sistema operativo RHEL | Caso d'uso|
| --- | --- | --- | --- | --- |
| A | TIPO I | eth0.tenant | eno1.tenant | Da client a HLI |
| b | TIPO I | eth2.tenant | eno3.tenant | Comunicazione da nodo a nodo |
| C | TIPO I | eth1.tenant | eno2.tenant | Da nodo ad archiviazione |
| D | TIPO I | eth4.tenant | eno4.tenant | Configurato ma non in uso |
| A | TIPO II | VLAN\<tenantNo > | team0.tenant | Da client a HLI |
| b | TIPO II | VLAN\<tenantNo + 2 > | team0.tenant+2 | Comunicazione da nodo a nodo |
| C | TIPO II | VLAN\<tenantNo + 1 > | team0.tenant+1 | Da nodo ad archiviazione |
| D | TIPO II | VLAN\<tenantNo + 3 > | team0.tenant+3 | Configurato ma non in uso |

### <a name="storage"></a>Archiviazione
I punti di montaggio seguenti sono preconfigurati:

| Punto di montaggio | Caso d'uso | 
| --- | --- |
|**Nei nodi master e di lavoro**|
|/hana/shared | Installazione HANA per SID di produzione | 
|/hana/data/SID/mnt00001 | Installazione di file di dati per SID di produzione | 
|/hana/log/SID/mnt00001 | Installazione di file di log per SID di produzione | 
|/hana/logbackups/SID | Log di rollforward per SID di produzione |


### <a name="key-considerations"></a>Considerazioni sulle chiavi
- /usr/SAP/SID è un collegamento simbolico a /hana/shared/SID.

## <a name="10-scale-out-with-dr-using-storage-replication"></a>10. scalabilità orizzontale con ripristino di emergenza tramite la replica di archiviazione
 
Questa topologia supporta più nodi in una configurazione scale-out con ripristino di emergenza. È supportato sia il ripristino di emergenza normale che multifunzione. Nel diagramma è illustrato solo il ripristino di emergenza normale. Questa topologia può essere richiesta con o senza il nodo standby.


### <a name="architecture-diagram"></a>Diagramma dell'architettura  

![scaleout-with-dr.png](media/hana-supported-scenario/scaleout-with-dr.png)


### <a name="ethernet"></a>Ethernet
Sono preconfigurate le interfacce di rete seguenti:

| INTERFACCE LOGICHE NIC | TIPO SKU | Nome con sistema operativo SUSE | Nome con sistema operativo RHEL | Caso d'uso|
| --- | --- | --- | --- | --- |
| A | TIPO I | eth0.tenant | eno1.tenant | Da client a HLI |
| b | TIPO I | eth2.tenant | eno3.tenant | Comunicazione da nodo a nodo |
| C | TIPO I | eth1.tenant | eno2.tenant | Da nodo ad archiviazione |
| D | TIPO I | eth4.tenant | eno4.tenant | Configurato ma non in uso |
| A | TIPO II | VLAN\<tenantNo > | team0.tenant | Da client a HLI |
| b | TIPO II | VLAN\<tenantNo + 2 > | team0.tenant+2 | Comunicazione da nodo a nodo |
| C | TIPO II | VLAN\<tenantNo + 1 > | team0.tenant+1 | Da nodo ad archiviazione |
| D | TIPO II | VLAN\<tenantNo + 3 > | team0.tenant+3 | Configurato ma non in uso |

### <a name="storage"></a>Archiviazione
I punti di montaggio seguenti sono preconfigurati:

| Punto di montaggio | Caso d'uso | 
| --- | --- |
|**Nel nodo primario**|
|/hana/shared | Installazione HANA per SID di produzione | 
|/hana/data/SID/mnt00001 | Installazione di file di dati per SID di produzione | 
|/hana/log/SID/mnt00001 | Installazione di file di log per SID di produzione | 
|/hana/logbackups/SID | Log di rollforward per SID di produzione |
|**Nel nodo ripristino di emergenza**|
|/hana/shared | Installazione HANA per SID di produzione | 
|/hana/data/SID/mnt00001 | Installazione di file di dati per SID di produzione | 
|/hana/log/SID/mnt00001 | Installazione di file di log per SID di produzione | 


### <a name="key-considerations"></a>Considerazioni sulle chiavi
- /usr/SAP/SID è un collegamento simbolico a /hana/shared/SID.
-  Nel ripristino di emergenza: i volumi e i punti di montaggio sono configurati, vale a dire contrassegnati come "Required for HANA installation" (Obbligatori per l'installazione HANA) per l'installazione dell'istanza HANA di produzione nell'unità HLI di ripristino di emergenza. 
- Nel ripristino di emergenza: i dati, i backup dei file di log e i volumi condivisi, contrassegnati come "Replica di archiviazione", vengono replicati tramite snapshot dal sito di produzione. Questi volumi sono montati solo durante il tempo di failover. Per altre informazioni, leggere il documento [Procedura di failover di ripristino di emergenza](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery). 
- Il volume di avvio della **classe SKU di Tipo I** è replicato nel nodo ripristino di emergenza.


## <a name="11-single-node-with-dr-using-hsr"></a>11. singolo nodo con ripristino di emergenza con HSR
 
Questa topologia supporta un nodo in una configurazione con scalabilità verticale con un SID con la replica di sistema HANA nel sito di ripristino di emergenza per un SID primario. Nel diagramma, in corrispondenza del sito primario, viene rappresentato solo il SID singolo, ma è supportato anche multisid (MCOS).

### <a name="architecture-diagram"></a>Diagramma dell'architettura  

![Single-node-HSR-Dr-111. png](media/hana-supported-scenario/single-node-hsr-dr-111.png)

### <a name="ethernet"></a>Ethernet
Sono preconfigurate le interfacce di rete seguenti:

| INTERFACCE LOGICHE NIC | TIPO SKU | Nome con sistema operativo SUSE | Nome con sistema operativo RHEL | Caso d'uso|
| --- | --- | --- | --- | --- |
| A | TIPO I | eth0.tenant | eno1.tenant | Da client a HLI/HSR |
| b | TIPO I | eth2.tenant | eno3.tenant | Configurato ma non in uso |
| C | TIPO I | eth1.tenant | eno2.tenant | Da nodo ad archiviazione |
| D | TIPO I | eth4.tenant | eno4.tenant | Configurato ma non in uso |
| A | TIPO II | VLAN\<tenantNo > | team0.tenant | Da client a HLI/HSR |
| b | TIPO II | VLAN\<tenantNo + 2 > | team0.tenant+2 | Configurato ma non in uso |
| C | TIPO II | VLAN\<tenantNo + 1 > | team0.tenant+1 | Da nodo ad archiviazione |
| D | TIPO II | VLAN\<tenantNo + 3 > | team0.tenant+3 | Configurato ma non in uso |

### <a name="storage"></a>Archiviazione
I seguenti montaggio sono preconfigurati in entrambe le unità HLI (primario e ripristino di emergenza):

| Punto di montaggio | Caso d'uso | 
| --- | --- |
|/hana/shared/SID | Installazione HANA per SID | 
|/hana/data/SID/mnt00001 | Installazione di file di dati per SID | 
|/hana/log/SID/mnt00001 | Installazione di file di log per SID | 
|/hana/logbackups/SID | Log di rollforward per SID |


### <a name="key-considerations"></a>Considerazioni sulle chiavi
- /usr/SAP/SID è un collegamento simbolico a /hana/shared/SID.
- Per MCOS: la distribuzione delle dimensioni del volume è basata sulle dimensioni del database in memoria. Consultare la sezione [Panoramica e architettura](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) per conoscere quali dimensioni del database in memoria sono supportate in un ambiente multisid.
- Nodo primario per la sincronizzazione del nodo di ripristino di emergenza tramite la replica di sistema HANA. 
- [Copertura globale](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach) viene usato per collegare insieme i circuiti ExpressRoute per creare una rete privata tra la rete delle aree geografiche.



## <a name="12-single-node-hsr-to-dr-cost-optimized"></a>12. HSR a nodo singolo al ripristino di emergenza (costo ottimizzato) 
 
 Questa topologia supporta un nodo in una configurazione con scalabilità verticale con un SID con la replica di sistema HANA nel sito di ripristino di emergenza per un SID primario. Nel diagramma, in corrispondenza del sito primario, viene rappresentato solo il SID singolo, ma è supportato anche multisid (MCOS). Nel sito di ripristino di emergenza l'unità HLI viene utilizzata per l'istanza QA mentre le operazioni di produzione vengono eseguite nel sito primario. Al momento del failover del ripristino di emergenza (o test di failover), l'istanza QA del sito di ripristino di emergenza viene disattivata.

### <a name="architecture-diagram"></a>Diagramma dell'architettura  

![Single-node-HSR-Dr-cost-optimized-121. png](media/hana-supported-scenario/single-node-hsr-dr-cost-optimized-121.png)

### <a name="ethernet"></a>Ethernet
Sono preconfigurate le interfacce di rete seguenti:

| INTERFACCE LOGICHE NIC | TIPO SKU | Nome con sistema operativo SUSE | Nome con sistema operativo RHEL | Caso d'uso|
| --- | --- | --- | --- | --- |
| A | TIPO I | eth0.tenant | eno1.tenant | Da client a HLI/HSR |
| b | TIPO I | eth2.tenant | eno3.tenant | Configurato ma non in uso |
| C | TIPO I | eth1.tenant | eno2.tenant | Da nodo ad archiviazione |
| D | TIPO I | eth4.tenant | eno4.tenant | Configurato ma non in uso |
| A | TIPO II | VLAN\<tenantNo > | team0.tenant | Da client a HLI/HSR |
| b | TIPO II | VLAN\<tenantNo + 2 > | team0.tenant+2 | Configurato ma non in uso |
| C | TIPO II | VLAN\<tenantNo + 1 > | team0.tenant+1 | Da nodo ad archiviazione |
| D | TIPO II | VLAN\<tenantNo + 3 > | team0.tenant+3 | Configurato ma non in uso |

### <a name="storage"></a>Archiviazione
I punti di montaggio seguenti sono preconfigurati:

| Punto di montaggio | Caso d'uso | 
| --- | --- |
|**Nel sito primario**|
|/hana/shared/SID | Installazione HANA per SID di produzione | 
|/hana/data/SID/mnt00001 | Installazione di file di dati per SID di produzione | 
|/hana/log/SID/mnt00001 | Installazione di file di log per SID di produzione | 
|/hana/logbackups/SID | Log di rollforward per SID di produzione |
|**Nel sito di ripristino di emergenza**|
|/hana/shared/SID | Installazione HANA per SID di produzione | 
|/hana/data/SID/mnt00001 | Installazione di file di dati per SID di produzione | 
|/hana/log/SID/mnt00001 | Installazione di file di log per SID di produzione | 
|/hana/logbackups/SID | Log di rollforward per SID di produzione |
|/hana/shared/QA-SID | Installazione HANA per SID QA | 
|/hana/data/QA-SID/mnt00001 | Installazione di file di dati per SID QA | 
|/hana/log/QA-SID/mnt00001 | Installazione di file di log per SID QA |
|/hana/logbackups/QA-SID | Log di rollforward per SID QA |

### <a name="key-considerations"></a>Considerazioni sulle chiavi
- /usr/SAP/SID è un collegamento simbolico a /hana/shared/SID.
- Per MCOS: la distribuzione delle dimensioni del volume è basata sulle dimensioni del database in memoria. Consultare la sezione [Panoramica e architettura](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) per conoscere quali dimensioni del database in memoria sono supportate in un ambiente multisid.
- Per l'installazione dell'istanza di HANA di produzione nell'unità di ripristino di emergenza, i volumi e i montaggio di ripristino di emergenza sono configurati, contrassegnati come "istanza di PROD nel sito di ripristino di emergenza". 
- Nel ripristino di emergenza: i dati, i backup dei file di log, i log e i volumi condivisi per QA, contrassegnati come “QA Instance installation” (Installazione dell'istanza QA) sono configurati per l'installazione dell'istanza QA.
- Nodo primario per la sincronizzazione del nodo di ripristino di emergenza tramite la replica di sistema HANA. 
- [Copertura globale](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach) viene usato per collegare insieme i circuiti ExpressRoute per creare una rete privata tra la rete delle aree geografiche.

## <a name="13-high-availability-and-disaster-recovery-with-hsr"></a>13. disponibilità elevata e ripristino di emergenza con HSR 
 
 Questa topologia supporta due nodi per la configurazione della replica di sistema HANA (HSR) per la disponibilità elevata delle aree locali. Per il ripristino di emergenza, il terzo nodo nell'area di ripristino di emergenza riceve la sincronizzazione dal sito primario usando HSR (modalità asincrona). 

### <a name="architecture-diagram"></a>Diagramma dell'architettura  

![Hana-System-Replication-Dr-131. png](media/hana-supported-scenario/hana-system-replication-dr-131.png)

### <a name="ethernet"></a>Ethernet
Sono preconfigurate le interfacce di rete seguenti:

| INTERFACCE LOGICHE NIC | TIPO SKU | Nome con sistema operativo SUSE | Nome con sistema operativo RHEL | Caso d'uso|
| --- | --- | --- | --- | --- |
| A | TIPO I | eth0.tenant | eno1.tenant | Da client a HLI/HSR |
| b | TIPO I | eth2.tenant | eno3.tenant | Configurato ma non in uso |
| C | TIPO I | eth1.tenant | eno2.tenant | Da nodo ad archiviazione |
| D | TIPO I | eth4.tenant | eno4.tenant | Configurato ma non in uso |
| A | TIPO II | VLAN\<tenantNo > | team0.tenant | Da client a HLI/HSR |
| b | TIPO II | VLAN\<tenantNo + 2 > | team0.tenant+2 | Configurato ma non in uso |
| C | TIPO II | VLAN\<tenantNo + 1 > | team0.tenant+1 | Da nodo ad archiviazione |
| D | TIPO II | VLAN\<tenantNo + 3 > | team0.tenant+3 | Configurato ma non in uso |

### <a name="storage"></a>Archiviazione
I punti di montaggio seguenti sono preconfigurati:

| Punto di montaggio | Caso d'uso | 
| --- | --- |
|**Nel sito primario**|
|/hana/shared/SID | Installazione HANA per SID di produzione | 
|/hana/data/SID/mnt00001 | Installazione di file di dati per SID di produzione | 
|/hana/log/SID/mnt00001 | Installazione di file di log per SID di produzione | 
|/hana/logbackups/SID | Log di rollforward per SID di produzione |
|**Nel sito di ripristino di emergenza**|
|/hana/shared/SID | Installazione HANA per SID di produzione | 
|/hana/data/SID/mnt00001 | Installazione di file di dati per SID di produzione | 
|/hana/log/SID/mnt00001 | Installazione di file di log per SID di produzione | 
|/hana/logbackups/SID | Log di rollforward per SID di produzione |


### <a name="key-considerations"></a>Considerazioni sulle chiavi
- /usr/SAP/SID è un collegamento simbolico a /hana/shared/SID.
- Al ripristino di emergenza: i volumi e montaggio sono configurati (contrassegnati come "istanza di ripristino di emergenza") per l'installazione dell'istanza di HANA di produzione nell'unità di ripristino di emergenza HLI. 
- Nodo del sito primario ottenere la sincronizzazione al nodo di ripristino di emergenza tramite la replica di sistema HANA. 
- [Copertura globale](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach) viene usato per collegare insieme i circuiti ExpressRoute per creare una rete privata tra la rete delle aree geografiche.

## <a name="14-high-availability-and-disaster-recovery-with-hsr-cost-optimized"></a>14. disponibilità elevata e ripristino di emergenza con HSR (costo ottimizzato)
 
 Questa topologia supporta due nodi per la configurazione della replica di sistema HANA (HSR) per la disponibilità elevata delle aree locali. Per il ripristino di emergenza, il terzo nodo nell'area di ripristino di emergenza riceve la sincronizzazione dal sito primario usando HSR (modalità asincrona), mentre un'altra istanza (ad esempio, Domande e risposte già in uscita dal nodo di ripristino di emergenza. 

### <a name="architecture-diagram"></a>Diagramma dell'architettura  

![Hana-System-Replication-Dr-cost-optimized-141. png](media/hana-supported-scenario/hana-system-replication-dr-cost-optimized-141.png)

### <a name="ethernet"></a>Ethernet
Sono preconfigurate le interfacce di rete seguenti:

| INTERFACCE LOGICHE NIC | TIPO SKU | Nome con sistema operativo SUSE | Nome con sistema operativo RHEL | Caso d'uso|
| --- | --- | --- | --- | --- |
| A | TIPO I | eth0.tenant | eno1.tenant | Da client a HLI/HSR |
| b | TIPO I | eth2.tenant | eno3.tenant | Configurato ma non in uso |
| C | TIPO I | eth1.tenant | eno2.tenant | Da nodo ad archiviazione |
| D | TIPO I | eth4.tenant | eno4.tenant | Configurato ma non in uso |
| A | TIPO II | VLAN\<tenantNo > | team0.tenant | Da client a HLI/HSR |
| b | TIPO II | VLAN\<tenantNo + 2 > | team0.tenant+2 | Configurato ma non in uso |
| C | TIPO II | VLAN\<tenantNo + 1 > | team0.tenant+1 | Da nodo ad archiviazione |
| D | TIPO II | VLAN\<tenantNo + 3 > | team0.tenant+3 | Configurato ma non in uso |

### <a name="storage"></a>Archiviazione
I punti di montaggio seguenti sono preconfigurati:

| Punto di montaggio | Caso d'uso | 
| --- | --- |
|**Nel sito primario**|
|/hana/shared/SID | Installazione HANA per SID di produzione | 
|/hana/data/SID/mnt00001 | Installazione di file di dati per SID di produzione | 
|/hana/log/SID/mnt00001 | Installazione di file di log per SID di produzione | 
|/hana/logbackups/SID | Log di rollforward per SID di produzione |
|**Nel sito di ripristino di emergenza**|
|/hana/shared/SID | Installazione HANA per SID di produzione | 
|/hana/data/SID/mnt00001 | Installazione di file di dati per SID di produzione | 
|/hana/log/SID/mnt00001 | Installazione di file di log per SID di produzione | 
|/hana/logbackups/SID | Log di rollforward per SID di produzione |
|/hana/shared/QA-SID | Installazione HANA per SID QA | 
|/hana/data/QA-SID/mnt00001 | Installazione di file di dati per SID QA | 
|/hana/log/QA-SID/mnt00001 | Installazione di file di log per SID QA |
|/hana/logbackups/QA-SID | Log di rollforward per SID QA |

### <a name="key-considerations"></a>Considerazioni sulle chiavi
- /usr/SAP/SID è un collegamento simbolico a /hana/shared/SID.
- Al ripristino di emergenza: i volumi e montaggio sono configurati (contrassegnati come "istanza di ripristino di emergenza") per l'installazione dell'istanza di HANA di produzione nell'unità di ripristino di emergenza HLI. 
- Nel ripristino di emergenza: i dati, i backup dei file di log, i log e i volumi condivisi per QA, contrassegnati come “QA Instance installation” (Installazione dell'istanza QA) sono configurati per l'installazione dell'istanza QA.
- Nodo del sito primario ottenere la sincronizzazione al nodo di ripristino di emergenza tramite la replica di sistema HANA. 
- [Copertura globale](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach) viene usato per collegare insieme i circuiti ExpressRoute per creare una rete privata tra la rete delle aree geografiche.

## <a name="15-scale-out-with-dr-using-hsr"></a>15. scalabilità orizzontale con il ripristino di emergenza tramite HSR
 
Questa topologia supporta più nodi in una configurazione scale-out con ripristino di emergenza. Questa topologia può essere richiesta con o senza il nodo standby. I nodi del sito primario ottengono la sincronizzazione con i nodi del sito di ripristino di emergenza con la replica di sistema HANA (modalità asincrona).


### <a name="architecture-diagram"></a>Diagramma dell'architettura  

[![scale-out-Dr-HSR-151. png](media/hana-supported-scenario/scale-out-dr-hsr-151.png)](media/hana-supported-scenario/scale-out-dr-hsr-151.png#lightbox)


### <a name="ethernet"></a>Ethernet
Sono preconfigurate le interfacce di rete seguenti:

| INTERFACCE LOGICHE NIC | TIPO SKU | Nome con sistema operativo SUSE | Nome con sistema operativo RHEL | Caso d'uso|
| --- | --- | --- | --- | --- |
| A | TIPO I | eth0.tenant | eno1.tenant | Da client a HLI/HSR |
| b | TIPO I | eth2.tenant | eno3.tenant | Comunicazione da nodo a nodo |
| C | TIPO I | eth1.tenant | eno2.tenant | Da nodo ad archiviazione |
| D | TIPO I | eth4.tenant | eno4.tenant | Configurato ma non in uso |
| A | TIPO II | VLAN\<tenantNo > | team0.tenant | Da client a HLI/HSR |
| b | TIPO II | VLAN\<tenantNo + 2 > | team0.tenant+2 | Comunicazione da nodo a nodo |
| C | TIPO II | VLAN\<tenantNo + 1 > | team0.tenant+1 | Da nodo ad archiviazione |
| D | TIPO II | VLAN\<tenantNo + 3 > | team0.tenant+3 | Configurato ma non in uso |

### <a name="storage"></a>Archiviazione
I punti di montaggio seguenti sono preconfigurati:

| Punto di montaggio | Caso d'uso | 
| --- | --- |
|**Nel nodo primario**|
|/hana/shared | Installazione HANA per SID di produzione | 
|/hana/data/SID/mnt00001 | Installazione di file di dati per SID di produzione | 
|/hana/log/SID/mnt00001 | Installazione di file di log per SID di produzione | 
|/hana/logbackups/SID | Log di rollforward per SID di produzione |
|**Nel nodo ripristino di emergenza**|
|/hana/shared | Installazione HANA per SID di produzione | 
|/hana/data/SID/mnt00001 | Installazione di file di dati per SID di produzione | 
|/hana/log/SID/mnt00001 | Installazione di file di log per SID di produzione | 
|/hana/logbackups/SID | Log di rollforward per SID di produzione |


### <a name="key-considerations"></a>Considerazioni sulle chiavi
- /usr/SAP/SID è un collegamento simbolico a /hana/shared/SID.
- Al ripristino di emergenza: i volumi e montaggio sono configurati per l'installazione dell'istanza di HANA di produzione nell'unità di ripristino di emergenza HLI. 
- I nodi del sito primario ottengono la sincronizzazione ai nodi di ripristino di emergenza tramite la replica di sistema HANA. 
- [Copertura globale](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach) viene usato per collegare insieme i circuiti ExpressRoute per creare una rete privata tra la rete delle aree geografiche.


## <a name="next-steps"></a>Passaggi successivi
- Fare riferimento a [Infrastruttura e connettività](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-infrastructure-connectivity) per HLI
- Fare riferimento a [Disponibilità elevata e ripristino di emergenza](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery) per HLI
