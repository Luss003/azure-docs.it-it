---
title: Immagini Red Hat Enterprise Linux in Azure | Microsoft Docs
description: Informazioni sulle immagini Red Hat Enterprise Linux in Microsoft Azure
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: BorisB2015
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/10/2020
ms.author: alsin
ms.openlocfilehash: e6109a87750e588b12bfc9836c5db3db55420ec2
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/11/2020
ms.locfileid: "77133793"
---
# <a name="red-hat-enterprise-linux-rhel-images-available-in-azure"></a>Immagini Red Hat Enterprise Linux (RHEL) disponibili in Azure
Azure offre un'ampia gamma di immagini RHEL per diversi casi d'uso.

> [!NOTE]
> Tutte le immagini RHEL sono disponibili nei cloud pubblici di Azure e Azure per enti pubblici. Non sono disponibili nei cloud di Azure Cina.

## <a name="list-of-rhel-images"></a>Elenco di immagini RHEL
Questo è un elenco di immagini RHEL disponibili in Azure. Se non diversamente specificato, tutte le immagini sono partizionate in LVM e collegate a normali repository RHEL (non EUS, non E4S). Le immagini seguenti sono attualmente disponibili per l'uso generale:

Offerta| SKU | Partizionamento | Provisioning | Note
:----|:----|:-------------|:-------------|:-----
RHEL          | 6.7      | RAW    | Agente Linux |
|             | 6.8      | RAW    | Agente Linux |
|             | 6.9      | RAW    | Agente Linux |
|             | 6.10     | RAW    | Agente Linux |
|             | 7-RAW    | RAW    | Agente Linux | Famiglia di immagini RHEL 7. x. <br> Collegato a repository regolari per impostazione predefinita (non EUS).
|             | 7-LVM    | LVM    | Agente Linux | Famiglia di immagini RHEL 7. x. <br> Collegato a repository regolari per impostazione predefinita (non EUS). Se si sta cercando un'immagine RHEL standard da distribuire, usare questo set di immagini e/o la relativa controparte di seconda generazione.
|             | 7lvm-Gen2| LVM    | Agente Linux | Famiglia di immagini di generazione 2, RHEL 7. x. <br> Collegato a repository regolari per impostazione predefinita (non EUS). Se si sta cercando un'immagine RHEL standard da distribuire, usare questo set di immagini e/o la relativa controparte di generazione 1.
|             | 7-RAW-CI | RAW-CI | Cloud-init  | Famiglia di immagini RHEL 7. x. <br> Collegato a repository regolari per impostazione predefinita (non EUS).
|             | 7.2      | RAW    | Agente Linux |
|             | 7.3      | RAW    | Agente Linux |
|             | 7.4      | RAW    | Agente Linux | Allegato ai repository EUS per impostazione predefinita, a partire dal 2019 aprile.
|             | 74-Gen2  | RAW    | Agente Linux | Allegato ai repository EUS per impostazione predefinita.
|             | 7.5      | RAW    | Agente Linux | Allegato ai repository EUS per impostazione predefinita a partire dal 2019 giugno.
|             | 75-Gen2  | RAW    | Agente Linux | Allegato ai repository EUS per impostazione predefinita.
|             | 7.6      | RAW    | Agente Linux | Collegato ai repository EUS per impostazione predefinita a partire dal 2019 maggio.
|             | 76-Gen2  | RAW    | Agente Linux | Allegato ai repository EUS per impostazione predefinita.
|             | 7,7      | LVM    | Agente Linux | Allegato ai repository EUS per impostazione predefinita.
|             | 8        | LVM    | Agente Linux | Famiglia di immagini RHEL 8. x
|             | 8-Gen2   | LVM    | Agente Linux | Famiglia di immagini Hyper-V Generation 2-RHEL 8. x.
RHEL-SAP      | 7.4      | LVM    | Agente Linux | RHEL 7,4 per app aziendali e SAP HANA. Collegato ai repository E4S, addebiterà un Premium per SAP e RHEL, oltre alla tariffa di calcolo di base.
|             | 74sap-Gen2| LVM    | Agente Linux | RHEL 7,4 per app aziendali e SAP HANA. Immagine di generazione 2. Collegato ai repository E4S, addebiterà un Premium per SAP e RHEL, oltre alla tariffa di calcolo di base.
|             | 7.5       | LVM    | Agente Linux | RHEL 7,5 per app aziendali e SAP HANA. Collegato ai repository E4S, addebiterà un Premium per SAP e RHEL, oltre alla tariffa di calcolo di base.
|             | 75sap-Gen2| LVM    | Agente Linux | RHEL 7,5 per app aziendali e SAP HANA. Immagine di generazione 2. Collegato ai repository E4S, addebiterà un Premium per SAP e RHEL, oltre alla tariffa di calcolo di base.
|             | 7.6       | LVM    | Agente Linux | RHEL 7,6 per app aziendali e SAP HANA. Collegato ai repository E4S, addebiterà un Premium per SAP e RHEL, oltre alla tariffa di calcolo di base.
|             | 76sap-Gen2| LVM    | Agente Linux | RHEL 7,6 per app aziendali e SAP HANA. Immagine di generazione 2. Collegato ai repository E4S, addebiterà un Premium per SAP e RHEL, oltre alla tariffa di calcolo di base.
|             | 7,7       | LVM    | Agente Linux | RHEL 7,7 per app aziendali e SAP HANA. Collegato ai repository E4S, addebiterà un Premium per SAP e RHEL, oltre alla tariffa di calcolo di base.
RHEL-SAP-HANA | 6.7       | RAW    | Agente Linux | RHEL 6,7 per SAP HANA. Obsoleto a favore delle immagini RHEL-SAP.
|             | 7.2       | LVM    | Agente Linux | RHEL 7,2 per SAP HANA. Obsoleto a favore delle immagini RHEL-SAP.
|             | 7.3       | LVM    | Agente Linux | RHEL 7,3 per SAP HANA. Obsoleto a favore delle immagini RHEL-SAP.
RHEL-SAP-APPS | 6.8       | RAW    | Agente Linux | RHEL 6,8 per SAP Business Applications. Obsoleto a favore delle immagini RHEL-SAP.
|             | 7.3       | LVM    | Agente Linux | RHEL 7,3 per SAP Business Applications. Obsoleto a favore delle immagini RHEL-SAP.
RHEL-HA       | 7.4       | LVM    | Agente Linux | RHEL 7,4 con componente aggiuntivo a disponibilità elevata. In viene addebitata una tariffa Premium per la disponibilità elevata e RHEL oltre alla tariffa di calcolo di base.
|             | 7.5       | LVM    | Agente Linux | RHEL 7,5 con componente aggiuntivo a disponibilità elevata. In viene addebitata una tariffa Premium per la disponibilità elevata e RHEL oltre alla tariffa di calcolo di base.
|             | 7.6       | LVM    | Agente Linux | RHEL 7,6 con componente aggiuntivo a disponibilità elevata. In viene addebitata una tariffa Premium per la disponibilità elevata e RHEL oltre alla tariffa di calcolo di base.
RHEL-SAP-HA   | 7.4          | LVM    | Agente Linux | RHEL 7,4 per SAP con disponibilità elevata e servizi di aggiornamento. Allegato ai repository E4S. Viene addebitata una tariffa Premium per i repository SAP e a disponibilità elevata, oltre a RHEL, oltre ai costi di calcolo di base.
|             | 74sapha-Gen2 | LVM    | Agente Linux | RHEL 7,4 per SAP con disponibilità elevata e servizi di aggiornamento. Immagine di generazione 2. Allegato ai repository E4S. Viene addebitata una tariffa Premium per i repository SAP e a disponibilità elevata, oltre a RHEL, oltre ai costi di calcolo di base.
|             | 7.5          | LVM    | Agente Linux | RHEL 7,5 per SAP con disponibilità elevata e servizi di aggiornamento. Allegato ai repository E4S. Viene addebitata una tariffa Premium per i repository SAP e a disponibilità elevata, oltre a RHEL, oltre ai costi di calcolo di base.
|             | 7.6          | LVM    | Agente Linux | RHEL 7,6 per SAP con disponibilità elevata e servizi di aggiornamento. Allegato ai repository E4S. Viene addebitata una tariffa Premium per i repository SAP e a disponibilità elevata, oltre a RHEL, oltre ai costi di calcolo di base.
|             | 76sapha-Gen2 | LVM    | Agente Linux | RHEL 7,6 per SAP con disponibilità elevata e servizi di aggiornamento. Immagine di generazione 2. Allegato ai repository E4S. Viene addebitata una tariffa Premium per i repository SAP e a disponibilità elevata, oltre a RHEL, oltre ai costi di calcolo di base.
|             | 7,7          | LVM    | Agente Linux | RHEL 7,7 per SAP con disponibilità elevata e servizi di aggiornamento. Allegato ai repository E4S. Viene addebitata una tariffa Premium per i repository SAP e a disponibilità elevata, oltre a RHEL, oltre ai costi di calcolo di base.
|             | 77sapha-Gen2 | LVM    | Agente Linux | RHEL 7,7 per SAP con disponibilità elevata e servizi di aggiornamento. Immagine di generazione 2. Allegato ai repository E4S. Viene addebitata una tariffa Premium per i repository SAP e a disponibilità elevata, oltre a RHEL, oltre ai costi di calcolo di base.
RHEL-BYOS     |RHEL-lvm74| LVM    | Agente Linux | Le immagini RHEL 7,4 BYOS, non associate ad alcuna origine di aggiornamenti, non addebiteranno un Premium RHEL.
|             |RHEL-lvm75| LVM    | Agente Linux | Le immagini RHEL 7,5 BYOS, non associate ad alcuna origine di aggiornamenti, non addebiteranno un Premium RHEL.
|             |RHEL-lvm76| LVM    | Agente Linux | Le immagini RHEL 7,6 BYOS, non associate ad alcuna origine di aggiornamenti, non addebiteranno un Premium RHEL.
|             |RHEL-lvm77| LVM    | Agente Linux | Le immagini RHEL 7,7 BYOS, non associate ad alcuna origine di aggiornamenti, non addebiteranno un Premium RHEL.
|             |RHEL-lvm8 | LVM    | Agente Linux | Le immagini RHEL 8 BYOS (la versione secondaria di RHEL è indicata nel valore della versione dell'immagine), non collegata ad alcuna origine degli aggiornamenti, non addebiterà un Premium RHEL.

## <a name="next-steps"></a>Passaggi successivi
* Scopri di più sulle [Immagini Red Hat in Azure](./redhat-images.md).
* Altre informazioni su [Red Hat Update Infrastructure](./redhat-rhui.md).
* Scopri di più sull' [offerta RHEL BYOS](./byos.md).
* Informazioni sui criteri di supporto di Red Hat per tutte le versioni di RHEL sono reperibili alla pagina [Red Hat Enterprise Linux Life Cycle (Ciclo di vita di Red Hat Enterprise Linux)](https://access.redhat.com/support/policy/updates/errata).
