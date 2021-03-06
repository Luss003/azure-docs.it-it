---
title: Eseguire l'autenticazione dal cluster Kubernetes
description: Informazioni su come fornire un cluster Kubernetes con accesso alle immagini nel registro contenitori di Azure creando un segreto di pull usando un'entità servizio
ms.topic: article
author: karolz-ms
ms.author: karolz
ms.reviewer: danlep
ms.date: 02/10/2020
ms.openlocfilehash: 0608ca0e0e53acf2f19910a7f1107dacf67d4e61
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2020
ms.locfileid: "77154894"
---
# <a name="pull-images-from-an-azure-container-registry-to-a-kubernetes-cluster"></a>Eseguire il pull delle immagini da un registro contenitori di Azure a un cluster Kubernetes

È possibile usare un registro contenitori di Azure come origine di immagini del contenitore con qualsiasi cluster Kubernetes, inclusi i cluster Kubernetes "locali", ad esempio [minikube](https://minikube.sigs.k8s.io/) e [Kind](https://kind.sigs.k8s.io/). Questo articolo illustra come creare un segreto pull di Kubernetes basato su un'entità servizio Azure Active Directory. Usare quindi il segreto per estrarre le immagini da un registro contenitori di Azure in una distribuzione di Kubernetes.

> [!TIP]
> Se si usa il servizio gestito di [Azure Kubernetes](../aks/intro-kubernetes.md), è anche possibile [integrare il cluster](../aks/cluster-container-registry-integration.md?toc=/azure/container-registry/toc.json&bc=/azure/container-registry/breadcrumb/toc.json) con un registro contenitori di Azure di destinazione per i pull di immagini. 

Questo articolo presuppone che sia già stato creato un registro contenitori di Azure privato. È anche necessario avere un cluster Kubernetes in esecuzione e accessibile tramite lo strumento da riga di comando `kubectl`.

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

Se non si salva o si ricorda la password dell'entità servizio, è possibile reimpostarla con il comando [AZ ad SP Credential Reset][az-ad-sp-credential-reset] :

```azurecli
az ad sp credential reset  --name http://<service-principal-name> --query password --output tsv
```

Questo comando restituisce una nuova password valida per l'entità servizio.

## <a name="create-an-image-pull-secret"></a>Creare un segreto di pull immagine

Kubernetes usa un *segreto di pull immagine* per archiviare le informazioni necessarie per l'autenticazione nel registro. Per creare il segreto di pull per un registro contenitori di Azure, è necessario specificare l'ID dell'entità servizio, la password e l'URL del registro di sistema. 

Creare un segreto di pull dell'immagine con il comando di `kubectl` seguente:

```console
kubectl create secret docker-registry <secret-name> \
  --namespace <namespace> \
  --docker-server=https://<container-registry-name>.azurecr.io \
  --docker-username=<service-principal-ID> \
  --docker-password=<service-principal-password>
```
dove:

| Valore | Descrizione |
| :--- | :--- |
| `secret-name` | Nome del segreto di pull dell'immagine, ad esempio *ACR-Secret* |
| `namespace` | Spazio dei nomi Kubernetes in cui inserire il segreto <br/> Necessaria solo se si desidera inserire il segreto in uno spazio dei nomi diverso dallo spazio dei nomi predefinito |
| `container-registry-name` | Nome del registro contenitori di Azure |
| `service-principal-ID` | ID dell'entità servizio che verrà usata da Kubernetes per accedere al registro |
| `service-principal-password` | Password dell'entità servizio |

## <a name="use-the-image-pull-secret"></a>Usa il segreto di pull dell'immagine

Una volta creato il segreto di pull dell'immagine, è possibile usarlo per creare pod e distribuzioni Kubernetes. Consente di specificare il nome del segreto in `imagePullSecrets` nel file di distribuzione. Ad esempio,

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: your-awesome-app-pod
  namespace: awesomeapps
spec:
  containers:
    - name: main-app-container
      image: your-awesome-app:v1
      imagePullPolicy: IfNotPresent
  imagePullSecrets:
    - name: acr-secret
```

Nell'esempio precedente, `your-awesome-app:v1` è il nome dell'immagine da estrarre dal registro contenitori di Azure e `acr-secret` è il nome del segreto di pull creato per accedere al registro di sistema. Quando si distribuisce il Pod, Kubernetes esegue automaticamente il pull dell'immagine dal registro di sistema, se non è già presente nel cluster.


## <a name="next-steps"></a>Passaggi successivi

* Per altre informazioni sull'uso delle entità servizio e di Azure Container Registry, vedere [autenticazione container Registry di Azure con entità servizio](container-registry-auth-service-principal.md)
* Scopri di più sui segreti di pull delle immagini nella [documentazione di Kubernetes](https://kubernetes.io/docs/concepts/containers/images/#specifying-imagepullsecrets-on-a-pod)


<!-- IMAGES -->

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
[az-ad-sp-credential-reset]: /cli/azure/ad/sp/credential#az-ad-sp-credential-reset