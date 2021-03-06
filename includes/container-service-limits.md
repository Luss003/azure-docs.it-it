---
title: File di inclusione
description: File di inclusione
services: container-service
author: dlepow
ms.service: container-service
ms.topic: include
ms.date: 11/22/2019
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: ac928d9087ba5db312540b8ec542d7a2a29e2a99
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2020
ms.locfileid: "77179059"
---
| Risorsa | Limite predefinito |
| --- | :--- |
| Numero massimo di cluster per sottoscrizione | 100 |
| Numero massimo di nodi per cluster con set di disponibilità delle macchine virtuali e SKU Basic Load Balancer  | 100 |
| Numero massimo di nodi per cluster con set di scalabilità di macchine virtuali e [SKU Load Balancer standard][standard-load-balancer] | 1000 (nodi 100 per ogni [pool][node-pool]di nodi) |
| Numero massimo di pod per nodo: [rete di base][basic-networking] con Kubenet | 110 |
| Numero massimo di pod per nodo: [rete avanzata][advanced-networking] con l'interfaccia di rete del contenitore di Azure | Distribuzione dell'interfaccia della riga di comando di Azure: 30<sup>1</sup><br />Modello di Azure Resource Manager: 30<sup>1</sup><br />Distribuzione portale: 30 |

<sup>1</sup> Quando si distribuisce un cluster Azure Kubernetes Service (AKS) con l'interfaccia della riga di comando di Azure o un modello di Gestione risorse, questo valore è configurabile fino a 250 pod per ogni nodo. Non è possibile configurare il numero massimo di pod per nodo dopo che è già stato distribuito un cluster AKS o se si distribuisce un cluster usando il portale di Azure.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/concepts-network.md#kubenet-basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#azure-cni-advanced-networking
[standard-load-balancer]: ../articles/load-balancer/load-balancer-standard-overview.md
[node-pool]: ../articles/aks/use-multiple-node-pools.md

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
