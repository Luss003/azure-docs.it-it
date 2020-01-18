---
title: Introduzione a SAP in macchine virtuali di Azure | Microsoft Docs
description: Informazioni sulle soluzioni SAP eseguite in macchine virtuali (VM) in Microsoft Azure
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: ad8e5c75-0cf6-4564-ae62-ea1246b4e5f2
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 01/16/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3712e67e95c8957d2537660add14d6cdb09d609f
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/17/2020
ms.locfileid: "76264815"
---
# <a name="use-azure-to-host-and-run-sap-workload-scenarios"></a>Usare Azure per ospitare ed eseguire scenari di carico di lavoro SAP

Quando si usa Microsoft Azure, è possibile eseguire in modo affidabile i carichi di lavoro e gli scenari SAP cruciali in una piattaforma scalabile, conforme e collaudata per le aziende. Ottieni la scalabilità, la flessibilità e il risparmio sui costi di Azure. Con la partnership estesa tra Microsoft e SAP, è possibile eseguire applicazioni SAP in scenari di sviluppo e test e di produzione in Azure ed essere completamente supportati. Da SAP NetWeaver a SAP S/4HANA, da SAP BI in Linux a Windows e da SAP HANA a SQL, abbiamo una copertura.

Oltre a ospitare scenari di SAP NetWeaver con i diversi sistemi DBMS in Azure, è possibile ospitare altri scenari di carico di lavoro SAP, ad esempio SAP BI in Azure. 

L'univocità di Azure per SAP HANA è un'offerta che consente di impostare Azure separatamente. Per consentire l'hosting di più risorse di memoria e CPU che richiedono scenari SAP che coinvolgono SAP HANA, Azure offre l'uso di hardware bare metal dedicato ai clienti. Usare questa soluzione per eseguire distribuzioni SAP HANA che richiedono fino a 24 TB (scalabilità orizzontale 120 TB) per S/4HANA o un altro carico di lavoro SAP HANA. 

L'hosting di scenari di carico di lavoro SAP in Azure può anche creare requisiti per l'integrazione di identità e Single Sign-On. Questa situazione può verificarsi quando si usa Azure Active Directory (Azure AD) per connettere componenti SAP diversi e offerte SaaS (software-as-a-Service) di SAP o piattaforma distribuita come servizio (PaaS). Un elenco di questi scenari di integrazione e Single Sign-On con le entità Azure AD e SAP viene descritto e documentato nella sezione "integrazione delle identità e Single Sign-On di AAD SAP".

## <a name="changes-to-the-sap-workload-section"></a>Modifiche alla sezione del carico di lavoro SAP
Alla fine di questo articolo sono elencate le modifiche apportate ai documenti nella sezione del carico di lavoro SAP in Azure.


## <a name="sap-hana-on-azure-large-instances"></a>SAP HANA in Azure (istanze Large)

Una serie di documenti consente di SAP HANA in Azure (istanze large) o in caso di istanze large di HANA. Per informazioni sulle seguenti aree di istanze large di HANA, vedere:

- [Panoramica di SAP HANA in Azure (istanze Large)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- [Architettura di SAP HANA in Azure (istanze Large)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-architecture)
- [Infrastruttura e connettività a SAP HANA in Azure (istanze large)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-infrastructure-connectivity)
- [Installare SAP HANA in Azure (istanze large)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-installation)
- [Disponibilità elevata e ripristino di emergenza di SAP HANA in Azure (istanze large)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery)
- [Risolvere i problemi e monitorare SAP HANA in Azure (istanze large)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/troubleshooting-monitoring)

Passaggi successivi:

- Leggi [la panoramica e l'architettura dei SAP Hana in Azure (istanze large)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)



## <a name="sap-hana-on-azure-virtual-machines"></a>SAP HANA in macchine virtuali di Azure
Questa sezione della documentazione presenta diversi aspetti relativi a SAP HANA. Come prerequisito, è necessario avere familiarità con i servizi principali di Azure che forniscono servizi elementari di Azure IaaS. È quindi necessario conoscere il calcolo, l'archiviazione e la rete di Azure. Molti di questi argomenti sono gestiti nella [Guida alla pianificazione di Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide)correlata a SAP NetWeaver. 

Per informazioni su HANA in Azure, vedere gli articoli seguenti e i relativi SottoArticoli:

- [Guida introduttiva: Installazione manuale di SAP HANA a istanza singola nelle macchine virtuali di Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-get-started)
- [Distribuire SAP S/4HANA o BW/4HANA in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h)
- [Configurazioni e operazioni dell'infrastruttura SAP HANA in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations)
- [Disponibilità elevata di SAP HANA per macchine virtuali di Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview)
- [Disponibilità di SAP HANA in un'area di Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-one-region)
- [Disponibilità di SAP HANA tra aree di Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions)
- [Disponibilità elevata di SAP HANA in macchine virtuali di Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability)
- [Guida al backup per SAP HANA in macchine virtuali di Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [Backup di SAP HANA di Azure a livello di file](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [Backup di SAP HANA basato su snapshot di archiviazione](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)

 

## <a name="sap-netweaver-deployed-on-azure-virtual-machines"></a>SAP NetWeaver distribuito in macchine virtuali di Azure
Questa sezione elenca la documentazione di pianificazione e distribuzione per SAP NetWeaver e Business One in Azure. La documentazione è incentrata sulle nozioni di base e sull'uso di database non HANA con un carico di lavoro SAP in Azure. I documenti e gli articoli per la disponibilità elevata sono anche le fondamenta per la disponibilità elevata di HANA in Azure, ad esempio:

- [SAP Business One in macchine virtuali di Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/business-one-azure)
- [Distribuire SAP IDES EHP7 SP3 per SAP ERP 6.0 in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-ides-erp6-erp7-sp3-sql)
- [Eseguire SAP NetWeaver in Microsoft Azure macchine virtuali SUSE Linux](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/suse-quickstart)
- [Guida alla pianificazione e all'implementazione di Macchine virtuali di Azure per SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide)
- [Distribuzione di Macchine virtuali di Azure per SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide)
- [Proteggere una distribuzione di applicazioni SAP NetWeaver multilivello usando Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-sap)
- [Connettore SAP LaMa per Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/lama-installation)

Per informazioni sui database non HANA in un carico di lavoro SAP in Azure, vedere:

- [Considerazioni sulla distribuzione DBMS di macchine virtuali di Azure per un carico di lavoro SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)
- [Distribuzione DBMS per SQL Server di macchine virtuali di Azure per SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sqlserver)
- [Distribuzione DBMS per Oracle di Macchine virtuali di Azure per un carico di lavoro SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_oracle)
- [Distribuzione DBMS per IBM DB2 di Macchine virtuali di Azure per un carico di lavoro SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_ibm)
- [Distribuzione DBMS per SAP ASE di Macchine virtuali di Azure per un carico di lavoro SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sapase)
- [Distribuzione di SAP MaxDB, Live cache e Content Server in macchine virtuali di Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_maxdb)

Per informazioni sui database SAP HANA in Azure, vedere la sezione "SAP HANA in macchine virtuali di Azure".

Per informazioni sulla disponibilità elevata di un carico di lavoro SAP in Azure, vedere:

- [Disponibilità elevata in Macchine virtuali di Azure per SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-start)

Questo documento fa riferimento a diversi altri documenti sull'architettura e sullo scenario. Nei documenti degli scenari successivi sono disponibili collegamenti a documenti tecnici dettagliati che illustrano la distribuzione e la configurazione dei diversi metodi di disponibilità elevata. I diversi documenti che illustrano come stabilire e configurare la disponibilità elevata per un carico di lavoro di SAP NetWeaver coprono i sistemi operativi Linux e Windows.


Per informazioni sull'integrazione tra Azure Active Directory (Azure AD) e i servizi SAP e Single Sign-On, vedere:

- [Esercitazione: Integrazione di Azure Active Directory con SAP Cloud for Customer](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-customer-cloud-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Esercitazione: Integrazione di Azure Active Directory con SAP Cloud Platform Identity Authentication](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-identity-authentication-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Esercitazione: Integrazione di Azure Active Directory con SAP Cloud Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Esercitazione: Integrazione di Azure Active Directory con SAP NetWeaver](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-netweaver-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Esercitazione: Integrazione di Azure Active Directory con SAP Business ByDesign](https://docs.microsoft.com/azure/active-directory/saas-apps/sapbusinessbydesign-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Esercitazione: Integrazione di Azure Active Directory con SAP HANA](https://docs.microsoft.com/azure/active-directory/saas-apps/saphana-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Ambiente S/4HANA: fiori Launchpad per SAML Single Sign-On con Azure AD](https://blogs.sap.com/2017/02/20/your-s4hana-environment-part-7-fiori-launchpad-saml-single-sing-on-with-azure-ad/)

Per informazioni sull'integrazione dei servizi di Azure nei componenti SAP, vedere:

- [Usare SAP HANA in Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-sap-hana)
- [DirectQuery e SAP HANA](https://docs.microsoft.com/power-bi/desktop-directquery-sap-hana)
- [Usare il connettore SAP BW in Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-sap-bw-connector) 
- [Azure Data Factory offre l'integrazione di dati SAP HANA e Business Warehouse](https://azure.microsoft.com/blog/azure-data-factory-offer-sap-hana-and-business-warehouse-data-integration)


## <a name="change-log"></a>Registro modifiche
- 01/16/2020: modificare [le modalità di installazione e configurazione di SAP Hana (istanze large) in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-installation) per adattare le versioni del sistema operativo alla directory hardware Hana IaaS
- 01/16/2020: modifiche alla [disponibilità elevata per SAP NetWeaver in macchine virtuali di Azure nella Guida a più SID SLES](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid) per aggiungere istruzioni per i sistemi SAP, usando l'architettura di Accodamento server 2 (ENSA2)
- 01/10/2020: le modifiche apportate all' [SAP Hana con scalabilità orizzontale con il nodo standby in macchine virtuali di Azure con Azure NetApp files in SLES](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse) e in [SAP Hana con scalabilità orizzontale con nodo standby in macchine virtuali di Azure con Azure NetApp files su RHEL](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-rhel) per aggiungere istruzioni su come rendere permanenti le modifiche `nfs4_disable_idmapping`.
- 01/10/2020: modifiche alla [disponibilità elevata per SAP NetWeaver in macchine virtuali di Azure in SLES con Azure NetApp files per le applicazioni SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files) e in [macchine virtuali di Azure disponibilità elevata per SAP NetWeaver in RHEL con Azure NetApp files per le applicazioni SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files) per aggiungere istruzioni su come montare Azure NetApp files volumi NFSv4.
- 12/23/2019: rilascio della [disponibilità elevata per SAP NetWeaver in macchine virtuali di Azure nella Guida a più SID di SLES](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid)
- 12/18/2019: rilascio di [SAP Hana con scalabilità orizzontale con nodo standby in macchine virtuali di Azure con Azure NetApp files in RHEL](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-rhel)
- 11/21/2019: le modifiche apportate all' [SAP Hana con scalabilità orizzontale con il nodo standby in macchine virtuali di Azure con Azure NetApp files in SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse) per semplificare la configurazione del mapping degli ID NFS e modificare l'interfaccia di rete primaria consigliata per semplificare il routing.
- 11/15/2019: modifiche minime apportate alla [disponibilità elevata per SAP NetWeaver in SUSE Linux Enterprise Server con Azure NetApp files per le applicazioni SAP](high-availability-guide-suse-netapp-files.md) e [disponibilità elevata per sap netweaver in Red Hat Enterprise Linux con Azure NetApp files per le applicazioni SAP](high-availability-guide-rhel-netapp-files.md) per chiarire le restrizioni delle dimensioni del pool di capacità e l'istruzione Remove che è supportata solo la versione NFSv3.
- 11/12/2019: versione di [disponibilità elevata per SAP NetWeaver in Windows con Azure NetApp Files (SMB)](high-availability-guide-windows-netapp-files-smb.md)
- 11/08/2019: modifiche alla [disponibilità elevata di SAP Hana nelle VM di Azure in SUSE Linux Enterprise Server](sap-hana-high-availability.md), [configurare SAP Hana la replica di sistema in macchine virtuali (VM)](sap-hana-high-availability-rhel.md)di Azure, [disponibilità elevata di macchine virtuali di azure per SAP NETWEAVER su SUSE Linux Enterprise Server per applicazioni SAP](high-availability-guide-suse.md), [disponibilità elevata di macchine virtuali di azure per SAP NetWeaver in SUSE Linux Enterprise Server con Azure NetApp files](high-availability-guide-suse-netapp-files.md), [disponibilità elevata di macchine virtuali di Azure per SAP NetWeaver in Red Hat Enterprise Linux ](high-availability-guide-rhel.md), [Disponibilità elevata di macchine virtuali di Azure per SAP netweaver in Red Hat Enterprise Linux con Azure NetApp files](high-availability-guide-rhel-netapp-files.md), [disponibilità elevata per NFS in macchine virtuali di Azure in SUSE Linux Enterprise Server](high-availability-guide-suse-nfs.md), GlusterFS in macchine virtuali di [Azure in Red Hat Enterprise Linux per SAP NetWeaver](high-availability-guide-rhel-glusterfs.md) per consigliare il servizio di bilanciamento del carico standard di Azure  
- 11/08/2019: modifiche nell' [elenco di controllo della pianificazione e distribuzione del carico di lavoro SAP](sap-deployment-checklist.md) per chiarire la raccomandazione  
- 11/04/2019: modifiche apportate alla configurazione di [pacemaker in SUSE Linux Enterprise Server in Azure](high-availability-guide-suse-pacemaker.md) per creare il cluster direttamente con la configurazione unicast  
- 10/29/2019: rilascio della [connettività degli endpoint pubblici per le macchine virtuali con Load Balancer standard di Azure in scenari a disponibilità elevata di SAP](high-availability-guide-standard-load-balancer-outbound-connections.md)
- 10/25/2019: modifiche in [SAP Hana configurazioni di archiviazione delle macchine virtuali di Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) e [SAP Hana con scalabilità orizzontale con nodo standby in macchine virtuali di Azure con Azure NetApp files su SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse) per chiarire il protocollo NFS per il volume/Hana/Shared
- 10/22/2019: modifica della [disponibilità elevata per SAP NetWeaver in macchine virtuali di Azure in SUSE Linux Enterprise Server per le applicazioni SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse), [disponibilità elevata per SAP NetWeaver in macchine virtuali di Azure in SUSE Linux Enterprise Server con Azure NetApp files per le applicazioni SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files), [disponibilità elevata per NFS in macchine virtuali di Azure in SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs), [configurazione di pacemaker in SUSE Linux Enterprise Server in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker), [disponibilità elevata di IBM DB2 LUW in macchine virtuali di Azure in SUSE Linux Enterprise Server con pacemaker](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms-guide-ha-ibm) e [disponibilità elevata di SAP Hana in macchine virtuali di Azure in SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability) per la protezione avanzata del rilevamento del servizio di bilanciamento del carico di Azure
- Modifica la sezione e l'intestazione di e in [SAP Hana configurazioni di archiviazione delle macchine virtuali di Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage)
- 10/21/2019: rilascio di [SAP Hana con scalabilità orizzontale con nodo standby in macchine virtuali di Azure con Azure NetApp files su SLES](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse)
- 10/16/2019: correzione dei collegamenti interrotti nel [backup e nel ripristino](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-backup-restore)
- 10/16/2019: modificare il sistema operativo consigliato minimo da SLES 12 SP3 a SLES 12 SP4 in [disponibilità elevata di IBM DB2 LUW in macchine virtuali di Azure in SUSE Linux Enterprise Server con pacemaker](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms-guide-ha-ibm)
- 10/11/2019: modifiche alle configurazioni di archiviazione su disco Ultra e introduzione di e in [SAP Hana configurazioni di archiviazione delle macchine virtuali di Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage)
- 10/01/2019: modificare la grafica dei [gruppi di posizionamento di prossimità di Azure per la latenza di rete ottimale con le applicazioni SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-proximity-placement-scenarios) per ottenere maggiore chiarezza
- 10/01/2019: modificare [le configurazioni e le operazioni di SAP Hana infrastruttura in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations) per correggere le istruzioni sulla condivisione NFS a disponibilità elevata per/Hana/Shared. 
- 09/28/2019: modificare la [configurazione di pacemaker in Red Hat Enterprise Linux in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker) per chiarire SBD come meccanismo di schermatura non supportato nei cluster RHEL  
- 09/17/2019: modificare la guida alla pianificazione e distribuzione di NetWeaver per unificare i termini relativi all'estensione della macchina virtuale per SAP  
- 08/22/2019: modifiche apportate alla [configurazione di pacemaker in SUSE Linux Enterprise Server in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker) per aggiornare gli URL per la creazione di un ruolo personalizzato  
- 08/16/2019: modifiche apportate alla [configurazione di pacemaker in Red Hat Enterprise Linux in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker) per ricordare ai clienti di aggiornare le azioni nel ruolo personalizzato, se aggiornate alla nuova versione dell'agente di recinzione di Azure  
- 08/15/2019: modifiche in [SAP Hana configurazioni di archiviazione delle macchine virtuali di Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) per riflettere la disponibilità generale del disco Ultra (in precedenza ultra SSD)
- 08/01/2019: modifiche apportate alla [configurazione di pacemaker in SUSE Linux Enterprise Server in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker) per integrare le modifiche in modo specifico per SLES 15 
- 07/23/2019: modifiche nel [cluster di un'istanza di SAP ASC/SCS in un cluster di failover Windows tramite una condivisione file in Azure](sap-high-availability-guide-wsfc-file-share.md) per riflettere il supporto dello spazio di archiviazione diretto da Azure Site Recovery Services
- 07/14/2019: rilascio dei [gruppi di posizionamento di prossimità di Azure per la latenza di rete ottimale con le applicazioni SAP](sap-proximity-placement-scenarios.md)
- 07/11/2019: modifiche nei diversi documenti che coprono istanze large di HANA per coprire la revisione 4 delle istanze large di HANA
- 07/09/2019: rilascio di una nuova guida per [IBM DB2 HADR in Red Hat Enterprise Server](high-availability-guide-rhel-ibm-db2-luw.md)
- 06/13/2019: rilascio della [disponibilità elevata per SAP NetWeaver in Red Hat Enterprise Linux con Azure NetApp files per le applicazioni SAP](high-availability-guide-rhel-netapp-files.md)


