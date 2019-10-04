---
title: Pubblicare un'offerta di macchina virtuale in Azure Marketplace
description: Elenca i passaggi necessari per pubblicare un'offerta di macchina virtuale esistente in Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: article
ms.date: 08/17/2018
ms.author: pabutler
ms.openlocfilehash: 6796c2871cf8a5928beed2ab557cefdfe8ecaae9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "64938604"
---
# <a name="publish-a-virtual-machine-offer"></a>Pubblicare un'offerta di macchina virtuale

 L'ultimo passaggio, dopo aver definito l'offerta nel portale e aver creato le risorse tecniche associate, consiste nell'inviare l'offerta per la pubblicazione. Il diagramma seguente illustra i passaggi principali del processo di pubblicazione che rende un'offerta disponibile in Azure Marketplace:

![Procedura di pubblicazione di un'offerta di macchina virtuale](./media/publishvm_013.png)

La tabella seguente descrive questi passaggi e indica una stima del tempo massimo necessario per il completamento:
<!-- we need to tell them that if an offer seems stuck in a step, to know that they should file a support ticket (link to support ticket doc) -->


|  **Passaggio per la pubblicazione**           | **Ora**    | **Descrizione**                                                            |
|  -------------------           | --------    | ---------------                                                            |
| Convalida dei prerequisiti         | 15 min   | Vengono convalidate le informazioni e le impostazioni dell'offerta.                        |
| Convalida del test drive (facoltativa) | 2 ore | Se si è scelto di abilitare i test drive, Microsoft ne convalida la configurazione, la distribuzione e la replica nelle aree selezionate. |
| Certificazione                  | 3 giorni | L'offerta viene analizzata dal team delle certificazioni di Azure. Questo passaggio esegue le analisi di virus, malware, conformità con le normative sulla sicurezza e problemi di sicurezza. Se viene rilevato un problema, viene fornito un feedback. |
| Provisioning                   | 4 giorni   | L'offerta di macchina virtuale viene replicata nei sistemi di produzione del marketplace.               |
| Creazione di pacchetti e registrazione per la generazione di clienti potenziali | \< 1 ora  | Le risorse tecniche dell'offerta vengono incluse in un pacchetto per uso dei clienti e i sistemi di registrazione di clienti potenziali vengono configurati e impostati. |
|  Approvazione dell'editore             |  -        | Revisione e conferma finali dell'editore prima che l'offerta diventi disponibile in Azure Marketplace. È possibile distribuire l'offerta nelle sottoscrizioni selezionate (nei passaggi relativi alle informazioni sull'offerta) per verificare che soddisfi tutti i requisiti.  |
| Provisioning                   | 4 giorni | L'offerta di macchina virtuale finalizzata viene replicata nelle aree e nei sistemi di produzione del marketplace. | 
| Live                           | 4 giorni | L'offerta di macchina virtuale viene rilasciata, replicata nelle aree necessarie e resa disponibile al pubblico. |
|  |  |

Il completamento di questo processo può richiedere fino a 16 giorni.  Dopo l'esecuzione di questa procedura di pubblicazione, l'offerta di macchina virtuale verrà pubblicata in [Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/). 

