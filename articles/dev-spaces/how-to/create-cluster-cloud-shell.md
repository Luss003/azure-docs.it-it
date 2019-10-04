---
title: Come creare un cluster Kubernetes abilitata per gli spazi di sviluppo di Azure usando Azure Cloud Shell
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 10/04/2018
ms.topic: conceptual
description: Informazioni su come creare in modo rapido un cluster Kubernetes abilitato per Azure Dev Spaces direttamente dal browser senza installare alcun componente.
keywords: Docker, Kubernetes, Azure, AKS, servizio Azure Kubernetes, contenitori, Helm, rete mesh di servizi, routing rete mesh di servizi, kubectl, k8s
ms.openlocfilehash: cd0c8c3c26feefe3448ada1cf1575706cd17e525
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "66808683"
---
# <a name="create-a-kubernetes-cluster-using-azure-cloud-shell"></a>Creare un cluster Kubernetes usando Azure Cloud Shell

È possibile usare [Azure Cloud Shell](/azure/cloud-shell) per creare un cluster Azure Kubernetes Service tramite il **prova** pulsante da questa pagina. Se non si è connessi, seguire le istruzioni per accedere con un account Azure, quindi digitare i comandi al prompt di Azure Cloud Shell, quando viene visualizzato.

## <a name="create-the-cluster"></a>Creare il cluster

In primo luogo, creare il gruppo di risorse in un [area che supporta Azure Dev spazi][supported-regions].

```azurecli-interactive
az group create --name MyResourceGroup --location <region>
```

Usare il comando seguente per creare un cluster Kubernetes:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyAKS --location <region> --disable-rbac --generate-ssh-keys
```

La creazione del cluster richiede alcuni minuti.  Al termine dell'esercitazione, viene visualizzato l'output in formato JSON. Cercare `provisioningState` e verificare il `Succeeded`.

## <a name="next-steps"></a>Passaggi successivi

Vedere [Azure Dev Spaces](/azure/dev-spaces/) per i collegamenti alle esercitazioni complete.

> [!IMPORTANT]
> Molte delle esercitazioni e guide introduttive di spazi di sviluppo di Azure usare la CLI di spazi di sviluppo di Azure per eseguire operazioni. È possibile installare CLI Azure Dev spazi in Azure Cloud Shell.


[supported-regions]: ../about.md#supported-regions-and-configurations