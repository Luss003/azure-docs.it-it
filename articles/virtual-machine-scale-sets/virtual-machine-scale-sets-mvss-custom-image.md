---
title: Fare riferimento a un'immagine personalizzata in un modello di set di scalabilità di Azure
description: Informazioni su come aggiungere un'immagine personalizzata in un modello di set di scalabilità di macchine virtuali di Azure esistente
author: mayanknayar
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 04/26/2018
ms.author: manayar
ms.openlocfilehash: fd1a567af1c35cf6b659995e998b11a61a526508
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2020
ms.locfileid: "76275586"
---
# <a name="add-a-custom-image-to-an-azure-scale-set-template"></a>Aggiungere un'immagine personalizzata in un modello di set di scalabilità di Azure

Questo articolo illustra come modificare il [modello di set di scalabilità di base](virtual-machine-scale-sets-mvss-start.md) per la distribuzione da un'immagine personalizzata.

## <a name="change-the-template-definition"></a>Modificare la definizione del modello
In un [articolo precedente](virtual-machine-scale-sets-mvss-start.md) è stato creato un modello di set di scalabilità di base. A questo punto si userà il modello precedente e lo si modificherà per creare un modello che distribuisce un set di scalabilità da un'immagine personalizzata.  

### <a name="creating-a-managed-disk-image"></a>Creazione dell'immagine di un disco gestito

Se si dispone già di un'immagine di disco gestito personalizzata (una risorsa di tipo `Microsoft.Compute/images`), è possibile ignorare questa sezione.

Prima di tutto aggiungere un parametro `sourceImageVhdUri`, ovvero l'URI al BLOB generalizzato in Archiviazione di Azure che contiene l'immagine personalizzata da cui avviare la distribuzione.


```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "sourceImageVhdUri": {
+      "type": "string",
+      "metadata": {
+        "description": "The source of the generalized blob containing the custom image"
+      }
     }
   },
   "variables": {},
```

Aggiungere poi una risorsa di tipo `Microsoft.Compute/images`, ovvero l'immagine del disco gestito basata sul BLOB generalizzato presente nell'URI `sourceImageVhdUri`. Questa immagine deve trovarsi nella stessa area del set di scalabilità da cui viene usata. Nelle proprietà dell'immagine specificare il tipo di sistema operativo, la posizione del BLOB ottenuta dal parametro `sourceImageVhdUri` e il tipo di account di archiviazione:

```diff
   "resources": [
     {
+      "type": "Microsoft.Compute/images",
+      "apiVersion": "2019-03-01",
+      "name": "myCustomImage",
+      "location": "[resourceGroup().location]",
+      "properties": {
+        "storageProfile": {
+          "osDisk": {
+            "osType": "Linux",
+            "osState": "Generalized",
+            "blobUri": "[parameters('sourceImageVhdUri')]",
+            "storageAccountType": "Standard_LRS"
+          }
+        }
+      }
+    },
+    {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "location": "[resourceGroup().location]",

```

Nella risorsa del set di scalabilità aggiungere una clausola `dependsOn` che fa riferimento all'immagine personalizzata, in modo da essere certi che l'immagine venga creata prima che il set di scalabilità tenti la distribuzione da tale immagine:

```diff
       "location": "[resourceGroup().location]",
       "apiVersion": "2019-03-01-preview",
       "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
+        "Microsoft.Network/virtualNetworks/myVnet",
+        "Microsoft.Compute/images/myCustomImage"
       ],
       "sku": {
         "name": "Standard_A1",

```

### <a name="changing-scale-set-properties-to-use-the-managed-disk-image"></a>Modifica delle proprietà del set di scalabilità per usare l'immagine del disco gestito

In `imageReference` del set di scalabilità `storageProfile` anziché specificare editore, offerta, SKU e versione di un'immagine della piattaforma, specificare `id` della risorsa `Microsoft.Compute/images`:

```json
         "virtualMachineProfile": {
           "storageProfile": {
             "imageReference": {
              "id": "[resourceId('Microsoft.Compute/images', 'myCustomImage')]"
             }
           },
           "osProfile": {
```

In questo esempio viene usata la funzione `resourceId` per ottenere l'ID della risorsa dell'immagine creata nello stesso modello. Se l'immagine del disco gestito è stata creata in precedenza, è necessario indicare invece l'ID di tale immagine. L'ID deve essere nel formato seguente: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Compute/images/<image-name>`.


## <a name="next-steps"></a>Fasi successive

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
