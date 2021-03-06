---
title: Che cos'è applicazione Azure controller di ingresso gateway?
description: Questo articolo fornisce un'introduzione al controller di ingresso del gateway applicazione.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 838145f8573e11deff8566c932a9c73c6f59f03b
ms.sourcegitcommit: 5925df3bcc362c8463b76af3f57c254148ac63e3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/31/2019
ms.locfileid: "75561661"
---
# <a name="what-is-application-gateway-ingress-controller"></a>Che cos'è il controller di ingresso del gateway applicazione?
Il controller di ingresso del gateway applicazione (AGIC) è un'applicazione Kubernetes, che consente ai clienti di [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/services/kubernetes-service/) di sfruttare il servizio di bilanciamento del carico L7 del [gateway applicazione](https://azure.microsoft.com/services/application-gateway/) nativo di Azure per esporre il software cloud a Internet. AGIC monitora il cluster Kubernetes in cui è ospitato e aggiorna continuamente un gateway applicazione, in modo che i servizi selezionati siano esposti a Internet.

Il controller di ingresso viene eseguito nel proprio pod sul AKS del cliente. AGIC monitora un subset di risorse Kubernetes per le modifiche. Lo stato del cluster AKS viene convertito in configurazione specifica del gateway applicazione e applicato al [Azure Resource Manager (ARM)](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

## <a name="benefits-of-application-gateway-ingress-controller"></a>Vantaggi del controller di ingresso del gateway applicazione
AGIC consente alla distribuzione di controllare più cluster AKS con un singolo controller di ingresso del gateway applicazione. AGIC consente inoltre di eliminare la necessità di un altro servizio di bilanciamento del carico/IP pubblico davanti al cluster AKS ed evita più hop nel percorso di DataPath prima che le richieste raggiungano il cluster AKS. Il gateway applicazione comunica con i pod usando direttamente il proprio indirizzo IP privato e non richiede i servizi Deport o KubeProxy. Offre inoltre prestazioni migliori per le distribuzioni.

Il controller di ingresso è supportato esclusivamente da SKU Standard_v2 e WAF_v2, che offre anche vantaggi per la scalabilità automatica. Il gateway applicazione può reagire in risposta a un aumento o una riduzione del carico di traffico e della scalabilità di conseguenza, senza l'utilizzo di risorse dal cluster AKS.

L'uso del gateway applicazione oltre a AGIC consente anche di proteggere il cluster AKS fornendo i criteri TLS e la funzionalità Web Application Firewall (WAF).

![Gateway applicazione Azure + AKS](./media/application-gateway-ingress-controller-overview/architecture.png)

AGIC viene configurato tramite la [risorsa di ingresso](https://kubernetes.io/docs/user-guide/ingress/)Kubernetes, insieme al servizio e alle distribuzioni/pod. Offre una serie di funzionalità, sfruttando il servizio di bilanciamento del carico L7 del gateway applicazione nativo di Azure. Ecco alcuni esempi:
  - Routing basato su URL
  - Affinità basata sui cookie
  - Terminazione SSL
  - SSL end-to-end
  - Supporto per siti Web pubblici, privati e ibridi
  - Web application firewall integrato

AGIC è in grado di gestire più spazi dei nomi e ha ProhibitedTargets, il che significa che AGIC può configurare il gateway applicazione in modo specifico per i cluster AKS senza influire su altri backend esistenti. 

## <a name="next-steps"></a>Fasi successive

- [**Distribuzione di Greenfield**](ingress-controller-install-new.md): istruzioni sull'installazione di AGIC, AKS e gateway applicazione in un'infrastruttura Slate vuota.
- [**Distribuzione di Brownfield**](ingress-controller-install-existing.md): installare AGIC in un gateway applicazione e un servizio contenitore di app esistente.

