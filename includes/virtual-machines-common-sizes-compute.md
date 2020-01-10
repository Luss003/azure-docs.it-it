---
title: File di inclusione
description: File di inclusione
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 11/04/2019
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: dd94f29317e703a68ba1b4a78639f635034d4492
ms.sourcegitcommit: f2149861c41eba7558649807bd662669574e9ce3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/07/2020
ms.locfileid: "75751956"
---
<!-- F-series, Fs-series* -->

Le dimensioni delle macchine virtuali ottimizzate per il calcolo hanno un rapporto elevato tra CPU e memoria. Queste dimensioni sono valide per server Web con traffico medio, appliance di rete, processi batch e server applicazioni. Questo articolo fornisce informazioni sul numero di vCPU, dischi dati e schede di rete. Sono inoltre incluse informazioni sulla velocità effettiva di archiviazione e sulla larghezza di banda di rete per ogni dimensione di questo raggruppamento.

La serie Fsv2 è basata sul processore Intel® Xeon® Platinum 8168. Include una velocità di clock di base di livello principale di 3,4 GHz e una frequenza Turbo Single Core massima di 3,7 GHz. Le istruzioni Intel® AVX-512 sono nuove per i processori Intel scalabili. Queste istruzioni forniscono fino a un incremento delle prestazioni di 2X ai carichi di lavoro di elaborazione vettoriali nelle operazioni a virgola mobile a precisione singola e doppia. In altre parole, sono molto veloci per qualsiasi carico di lavoro di calcolo.

Con un prezzo di listino orario più basso, la serie Fsv2 presenta il migliore rapporto prezzo-prestazioni nel portfolio Azure basato sull'unità di calcolo di Azure (ACU, Azure Compute Unit) per ogni vCPU.

## <a name="fsv2-series-sup1sup"></a>Serie Fsv2 <sup>1</sup>

ACU: 195 - 210

Archiviazione Premium: supportata

Caching archiviazione Premium: supportato

| Dimensioni             | vCPU | Memoria: GiB | GiB di archiviazione temporanea (unità SSD) | Numero massimo di dischi dati | Velocità effettiva massima di archiviazione temporanea e nella cache: IOPS/MBps (dimensioni della cache in GiB) | Max velocità effettiva del disco non memorizzato nella cache: IOPS/MBps | Schede di interfaccia di rete max/larghezza di banda della rete prevista (Mbps) |
|------------------|--------|-------------|----------------|----------------|--------------------------|--------------------------|-------------------------|
| Standard_F2s_v2  | 2      | 4           | 16             | 4              | 4000/31 (32)           | 3200/47                | 2 / 875                 |
| Standard_F4s_v2  | 4      | 8           | 32             | 8              | 8000/63 (64)           | 6400/95                | 2 / 1750               |
| Standard_F8s_v2  | 8      | 16          | 64             | 16             | 16000/127 (128)        | 12800/190              | 4 / 3500               |
| Standard_F16s_v2 | 16     | 32          | 128            | 32             | 32000/255 (256)        | 25600/380              | 4 / 7000               |
| Standard_F32s_v2 | 32     | 64          | 256            | 32             | 64000/512 (512)        | 51200/750              | 8 / 14000              |
| Standard_F48s_v2 | 48     | 96          | 384            | 32             | 96000/768 (768)        | 76800/1100             | 8 / 21000              |
| Standard_F64s_v2 | 64     | 128         | 512            | 32             | 128000/1024 (1024)     | 80000/1100             | 8 / 28000              |
| Standard_F72s_v2<sup>2,&nbsp;3</sup> | 72 | 144 | 576         | 32             | 144000/1152 (1520)     | 80000/1100             | 8 / 30000              |

<sup>1</sup> le macchine virtuali serie Fsv2 includono tecnologia Intel® Hyper-Threading.

<sup>2</sup> l'uso di più di 64 vCPU richiede uno di questi sistemi operativi guest supportati:
- Windows Server 2016 o versione successiva
- Ubuntu 16,04 LTS o versioni successive, con kernel ottimizzato per Azure (kernel 4,15 o versione successiva)
- SLES 12 SP2 o versione successiva
- RHEL o CentOS versione 6,7 alla 6,10, con il pacchetto LIS fornito da Microsoft (o versioni successive) installato
- RHEL o CentOS versione 7,3, con il pacchetto LIS fornito da Microsoft (o versioni successive) installato
- RHEL o CentOS versione 7,6 o successiva
- Oracle Linux con UEK4 o versione successiva
- Debian 9 con il kernel di backports, Debian 10 o versione successiva
- CoreOS con un kernel 4,14 o versione successiva

<sup>3</sup> L'istanza è isolata e prevede hardware dedicato per un singolo cliente.
