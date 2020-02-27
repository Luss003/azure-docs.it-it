---
title: Integrazione con i servizi gestiti di Azure usando Open Service Broker for Azure (OSBA)
description: Integrazione con i servizi gestiti di Azure usando Open Service Broker for Azure (OSBA)
author: zr-msft
ms.topic: overview
ms.date: 12/05/2017
ms.author: zarhoads
ms.openlocfilehash: 8d727256afbe152a4f7022d0fd2454c4677b023c
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/25/2020
ms.locfileid: "77595604"
---
# <a name="integrate-with-azure-managed-services-using-open-service-broker-for-azure-osba"></a>Integrazione con i servizi gestiti di Azure usando Open Service Broker for Azure (OSBA)

Insieme al [catalogo di servizi di Kubernetes][kubernetes-service-catalog], Open Service Broker for Azure (OSBA) consente agli sviluppatori di usare i servizi gestiti di Azure in Kubernetes. Questa guida descrive la distribuzione del catalogo di servizi di Kubernetes, di Open Service Broker for Azure (OSBA) e delle applicazioni che usano i servizi gestiti di Azure con Kubernetes.

## <a name="prerequisites"></a>Prerequisites
* Una sottoscrizione di Azure.

* Interfaccia della riga di comando di Azure: [installarla localmente][azure-cli-install] o usarla in [Azure Cloud Shell][azure-cloud-shell].

* Interfaccia della riga di comando di Helm 2.7+: [installarla localmente][helm-cli-install] o usarla in [Azure Cloud Shell][azure-cloud-shell].

* Autorizzazioni per creare un'entità servizio con il ruolo Collaboratore nella sottoscrizione di Azure

* Cluster del servizio Azure Kubernetes esistente. Se è necessario un cluster del servizio Azure Container consultare la guida introduttiva [Creare un cluster del servizio Azure Container][create-aks-cluster].

## <a name="install-service-catalog"></a>Installare il catalogo di servizi

Il primo passaggio consiste nell'installare il catalogo di servizi nel cluster di Kubernetes usando un grafico Helm. Aggiornare l'installazione di Tiller (server Helm) nel cluster con:

```azurecli-interactive
helm init --upgrade
```

A questo punto aggiungere il grafico del catalogo di servizi al repository Helm:

```azurecli-interactive
helm repo add svc-cat https://svc-catalog-charts.storage.googleapis.com
```

Infine installare il catalogo di servizi con il grafico Helm. Se il cluster dispone dell'abilitazione RBAC, eseguire questo comando.

```azurecli-interactive
helm install svc-cat/catalog --name catalog --namespace catalog --set apiserver.storage.etcd.persistence.enabled=true --set apiserver.healthcheck.enabled=false --set controllerManager.healthcheck.enabled=false --set apiserver.verbosity=2 --set controllerManager.verbosity=2
```

Se il cluster non dispone dell'abilitazione RBAC, eseguire questo comando.

```azurecli-interactive
helm install svc-cat/catalog --name catalog --namespace catalog --set rbacEnable=false --set apiserver.storage.etcd.persistence.enabled=true --set apiserver.healthcheck.enabled=false --set controllerManager.healthcheck.enabled=false --set apiserver.verbosity=2 --set controllerManager.verbosity=2
```

Dopo l'esecuzione del grafico Helm verificare che `servicecatalog` venga visualizzato nell'output del comando seguente:

```azurecli-interactive
kubectl get apiservice
```

Ad esempio, l'output dovrebbe essere simile al seguente (troncato):

```
NAME                                 AGE
v1.                                  10m
v1.authentication.k8s.io             10m
...
v1beta1.servicecatalog.k8s.io        34s
v1beta1.storage.k8s.io               10
```

## <a name="install-open-service-broker-for-azure"></a>Installare Open Service Broker for Azure

Il passaggio successivo consiste nell'installare [Open Service Broker for Azure][open-service-broker-azure], che include il catalogo dei servizi gestiti di Azure. Esempi di servizi di Azure disponibili sono Database di Azure per PostgreSQL, Database di Azure per MySQL e Database SQL di Azure.

Iniziare aggiungendo Open Service Broker per il repository Helm di Azure:

```azurecli-interactive
helm repo add azure https://kubernetescharts.blob.core.windows.net/azure
```

Creare un'[Entità servizio][create-service-principal] con il comando dell'interfaccia della riga di comando di Azure seguente:

```azurecli-interactive
az ad sp create-for-rbac
```

L'output dovrebbe essere simile al seguente: Annotare i valori `appId`, `password` e `tenant` che verranno usati nel passaggio successivo.

```JSON
{
  "appId": "7248f250-0000-0000-0000-dbdeb8400d85",
  "displayName": "azure-cli-2017-10-15-02-20-15",
  "name": "http://azure-cli-2017-10-15-02-20-15",
  "password": "77851d2c-0000-0000-0000-cb3ebc97975a",
  "tenant": "72f988bf-0000-0000-0000-2d7cd011db47"
}
```

Impostare le variabili di ambiente seguenti con i valori precedenti:

```azurecli-interactive
AZURE_CLIENT_ID=<appId>
AZURE_CLIENT_SECRET=<password>
AZURE_TENANT_ID=<tenant>
```

A questo punto ottenere l'ID sottoscrizione di Azure:

```azurecli-interactive
az account show --query id --output tsv
```

Impostare nuovamente la variabile di ambiente seguente con il valore precedente:

```azurecli-interactive
AZURE_SUBSCRIPTION_ID=[your Azure subscription ID from above]
```

Ora che queste variabili di ambiente sono state popolate, eseguire il comando seguente per installare Open Service Broker for Azure usando il grafico Helm:

```azurecli-interactive
helm install azure/open-service-broker-azure --name osba --namespace osba \
    --set azure.subscriptionId=$AZURE_SUBSCRIPTION_ID \
    --set azure.tenantId=$AZURE_TENANT_ID \
    --set azure.clientId=$AZURE_CLIENT_ID \
    --set azure.clientSecret=$AZURE_CLIENT_SECRET
```

Una volta completata la distribuzione di Open Service Broker for Azure installare l'[interfaccia della riga di comando del catalogo di servizi][service-catalog-cli], un'interfaccia della riga di comando di facile utilizzo per l'interrogazione di service broker, classi di servizio, piani di servizio e molto altro.

Eseguire i comandi seguenti per installare il binario dell'interfaccia della riga di comando del catalogo di servizi:

```azurecli-interactive
curl -sLO https://servicecatalogcli.blob.core.windows.net/cli/latest/$(uname -s)/$(uname -m)/svcat
chmod +x ./svcat
```

A questo punto elencare i Service Broker installati:

```azurecli-interactive
./svcat get brokers
```

L'output dovrebbe essere simile al seguente:

```
  NAME                               URL                                STATUS
+------+--------------------------------------------------------------+--------+
  osba   http://osba-open-service-broker-azure.osba.svc.cluster.local   Ready
```

Successivamente elencare le classi di servizio disponibili. Le classi di servizio visualizzate sono i servizi gestiti di Azure disponibili che possono essere sottoposti a provisioning tramite Open Service Broker for Azure.

```azurecli-interactive
./svcat get classes
```

Infine elencare tutti i piani di servizio disponibili. I piani di servizio sono i livelli di servizio dei servizi gestiti di Azure. Ad esempio, per il Database di Azure per MySQL, i piani vanno da `basic50` per il livello Basic con 50 unità di transazione database (DTU), a `standard800` per il livello Standard con 800 DTU.

```azurecli-interactive
./svcat get plans
```

## <a name="install-wordpress-from-helm-chart-using-azure-database-for-mysql"></a>Installare WordPress dal grafico Helm usando il Database di Azure per MySQL

In questo passaggio usare Helm per installare un grafico Helm aggiornato per WordPress. Il grafico esegue il provisioning di un'istanza del Database di Azure per MySQL che WordPress può usare. Il processo potrebbe richiedere alcuni minuti.

```azurecli-interactive
helm install azure/wordpress --name wordpress --namespace wordpress --set resources.requests.cpu=0 --set replicaCount=1
```

Per verificare che l'installazione abbia eseguito il provisioning delle risorse corrette elencare le istanze e i collegamenti del servizio installato:

```azurecli-interactive
./svcat get instances -n wordpress
./svcat get bindings -n wordpress
```

Elencare i segreti installati:

```azurecli-interactive
kubectl get secrets -n wordpress -o yaml
```

## <a name="next-steps"></a>Passaggi successivi

L'articolo ha descritto la distribuzione di un catalogo di servizi in un cluster del servizio Azure Kubernetes. Con Open Service Broker for Azure è stato possibile distribuire un'installazione di WordPress che usa i servizi gestiti di Azure, in questo caso il Database di Azure per MySQL.

Consultare il repository [Azure/helm-charts][helm-charts] per accedere ad altri grafici Helm basati su OSBA aggiornati. Se si desidera creare grafici che funzionino con OSBA consultare [Creating a New Chart][helm-create-new-chart] (Creazione di un nuovo grafico).

<!-- LINKS - external -->
[helm-charts]: https://github.com/Azure/helm-charts
[helm-cli-install]: https://docs.helm.sh/helm/#helm-install
[helm-create-new-chart]: https://github.com/Azure/helm-charts#creating-a-new-chart
[kubernetes-service-catalog]: https://github.com/kubernetes-incubator/service-catalog
[open-service-broker-azure]: https://github.com/Azure/open-service-broker-azure
[service-catalog-cli]: https://github.com/Azure/service-catalog-cli

<!-- LINKS - internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
[azure-cloud-shell]: ../cloud-shell/overview.md
[create-aks-cluster]: ./kubernetes-walkthrough.md
[create-service-principal]: ./kubernetes-service-principal.md
