---
title: Distribuire le risorse di Azure tra sottoscrizioni & gruppo di risorse
description: Illustra come specificare come destinazione più gruppi di sottoscrizioni e risorse di Azure durante la distribuzione.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 06/02/2018
ms.author: tomfitz
ms.openlocfilehash: c90096043f54eb8db5834fbe83ed1d6ae710d371
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2019
ms.locfileid: "72528319"
---
# <a name="deploy-azure-resources-to-more-than-one-subscription-or-resource-group"></a>Distribuire le risorse di Azure in più gruppi di sottoscrizioni e risorse

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

In genere si distribuiscono tutte le risorse del modello in un unico [gruppo di risorse](resource-group-overview.md). ma in alcuni scenari può essere preferibile distribuire insieme un set di risorse, inserendole tuttavia in gruppi di sottoscrizioni e risorse diversi. Potrebbe essere necessario, ad esempio, distribuire la macchina virtuale di backup per Azure Site Recovery in un gruppo di risorse e in una posizione separati. Resource Manager consente di usare modelli annidati per specificare come destinazione gruppi di sottoscrizioni e risorse diversi da quello usato per il modello padre.

> [!NOTE]
> Un'unica distribuzione può interessare solo cinque gruppi di risorse. Questa limitazione significa in genere che è possibile eseguire la distribuzione in un solo gruppo di risorse specificato per il modello padre e in un massimo di quattro gruppi di risorse nelle distribuzioni annidate o collegate. Tuttavia, se il modello padre contiene solo modelli annidati o collegati e non distribuisce risorse, è possibile includere fino a cinque gruppi di risorse nelle distribuzioni annidate o collegate.

## <a name="specify-a-subscription-and-resource-group"></a>Specificare un gruppo di sottoscrizioni e risorse

Per specificare come destinazione una risorsa diversa, usare un modello annidato o collegato. Il tipo di risorsa `Microsoft.Resources/deployments` fornisce parametri per `subscriptionId` e `resourceGroup`. Queste proprietà consentono di specificare un gruppo di sottoscrizioni e risorse diverso per la distribuzione nidificata. Tutti i gruppi di risorse devono esistere prima di eseguire la distribuzione. Se non si specifica un ID sottoscrizione o un gruppo di risorse, viene usato il gruppo di sottoscrizioni e risorse del modello padre.

L'account usato per distribuire il modello deve disporre delle autorizzazioni per la distribuzione per l'ID sottoscrizione specificato. Se la sottoscrizione specificata è presente in un tenant di Azure Active Directory diverso, è necessario [aggiungere gli utenti guest da un'altra directory](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md).

Per specificare un gruppo di risorse e una sottoscrizione differenti, usare:

```json
"resources": [
    {
        "apiVersion": "2017-05-10",
        "name": "nestedTemplate",
        "type": "Microsoft.Resources/deployments",
        "resourceGroup": "[parameters('secondResourceGroup')]",
        "subscriptionId": "[parameters('secondSubscriptionID')]",
        ...
    }
]
```

Se i gruppi di risorse si trovano nella stessa sottoscrizione, è possibile rimuovere il valore **subscriptionId**.

L'esempio seguente distribuisce due account di archiviazione, uno nel gruppo di risorse specificato durante la distribuzione e uno in un gruppo di risorse specificato nel parametro `secondResourceGroup`:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storagePrefix": {
            "type": "string",
            "maxLength": 11
        },
        "secondResourceGroup": {
            "type": "string"
        },
        "secondSubscriptionID": {
            "type": "string",
            "defaultValue": ""
        },
        "secondStorageLocation": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "variables": {
        "firstStorageName": "[concat(parameters('storagePrefix'), uniqueString(resourceGroup().id))]",
        "secondStorageName": "[concat(parameters('storagePrefix'), uniqueString(parameters('secondSubscriptionID'), parameters('secondResourceGroup')))]"
    },
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "nestedTemplate",
            "type": "Microsoft.Resources/deployments",
            "resourceGroup": "[parameters('secondResourceGroup')]",
            "subscriptionId": "[parameters('secondSubscriptionID')]",
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Storage/storageAccounts",
                            "name": "[variables('secondStorageName')]",
                            "apiVersion": "2017-06-01",
                            "location": "[parameters('secondStorageLocation')]",
                            "sku":{
                                "name": "Standard_LRS"
                            },
                            "kind": "Storage",
                            "properties": {
                            }
                        }
                    ]
                },
                "parameters": {}
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('firstStorageName')]",
            "apiVersion": "2017-06-01",
            "location": "[resourceGroup().location]",
            "sku":{
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {
            }
        }
    ]
}
```

Se si imposta `resourceGroup` sul nome di un gruppo di risorse che non esiste, la distribuzione ha esito negativo.

## <a name="use-the-resourcegroup-and-subscription-functions"></a>Usare le funzioni resourceGroup() e subscription()

Per le distribuzioni tra gruppi di risorse, le funzioni [resourceGroup()](resource-group-template-functions-resource.md#resourcegroup) e [subscription()](resource-group-template-functions-resource.md#subscription) vengono risolte in modo diverso in base al modo in cui si specifica il modello annidato. 

Se si incorpora un modello in un altro modello, le funzioni nel modello annidato si risolvono nel gruppo di risorse e nella sottoscrizione padre. Un modello incorporato usa il formato seguente:

```json
"apiVersion": "2017-05-10",
"name": "embeddedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "template": {
        ...
        resourceGroup() and subscription() refer to parent resource group/subscription
    }
}
```

Se si crea un collegamento a un modello separato, le funzioni nel modello collegato si risolvono nel gruppo di risorse e nella sottoscrizione annidati. Un modello collegato usa il formato seguente:

```json
"apiVersion": "2017-05-10",
"name": "linkedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "templateLink": {
        ...
        resourceGroup() and subscription() in linked template refer to linked resource group/subscription
    }
}
```

## <a name="example-templates"></a>Modelli di esempio

I modelli seguenti mostrano distribuzioni tra più gruppi di risorse. Dopo la tabella sono mostrati script per distribuire i modelli.

|Modello  |Description  |
|---------|---------|
|[Modello tra più sottoscrizioni](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crosssubscription.json) |Distribuisce un account di archiviazione in un gruppo di risorse e un account di archiviazione in un secondo gruppo di risorse. Includere un valore per l'ID sottoscrizione quando il secondo gruppo di risorse è in una sottoscrizione diversa. |
|[Modello di proprietà a più gruppi di risorse](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crossresourcegroupproperties.json) |Mostra in che modo si risolve la funzione `resourceGroup()`. Non distribuisce alcuna risorsa. |

### <a name="powershell"></a>PowerShell

Per PowerShell, per distribuire due account di archiviazione in due gruppi di risorse nella **stessa sottoscrizione**, usare:

```azurepowershell-interactive
$firstRG = "primarygroup"
$secondRG = "secondarygroup"

New-AzResourceGroup -Name $firstRG -Location southcentralus
New-AzResourceGroup -Name $secondRG -Location eastus

New-AzResourceGroupDeployment `
  -ResourceGroupName $firstRG `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json `
  -storagePrefix storage `
  -secondResourceGroup $secondRG `
  -secondStorageLocation eastus
```

Per PowerShell, per distribuire due account di archiviazione in **due sottoscrizioni**, usare:

```azurepowershell-interactive
$firstRG = "primarygroup"
$secondRG = "secondarygroup"

$firstSub = "<first-subscription-id>"
$secondSub = "<second-subscription-id>"

Select-AzSubscription -Subscription $secondSub
New-AzResourceGroup -Name $secondRG -Location eastus

Select-AzSubscription -Subscription $firstSub
New-AzResourceGroup -Name $firstRG -Location southcentralus

New-AzResourceGroupDeployment `
  -ResourceGroupName $firstRG `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json `
  -storagePrefix storage `
  -secondResourceGroup $secondRG `
  -secondStorageLocation eastus `
  -secondSubscriptionID $secondSub
```

Per PowerShell, per testare in che modo l'**oggetto gruppo di risorse** si risolve per il modello padre, modello inline e modello collegato, usare:

```azurepowershell-interactive
New-AzResourceGroup -Name parentGroup -Location southcentralus
New-AzResourceGroup -Name inlineGroup -Location southcentralus
New-AzResourceGroup -Name linkedGroup -Location southcentralus

New-AzResourceGroupDeployment `
  -ResourceGroupName parentGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json
```

Nell'esempio precedente, sia **parentRG** che **inlineRG** vengono risolti in **parentGroup**. **linkedRG** viene risolto in **linkedGroup**. L'output dell'esempio precedente è:

```powershell
 Name             Type                       Value
 ===============  =========================  ==========
 parentRG         Object                     {
                                               "id": "/subscriptions/<subscription-id>/resourceGroups/parentGroup",
                                               "name": "parentGroup",
                                               "location": "southcentralus",
                                               "properties": {
                                                 "provisioningState": "Succeeded"
                                               }
                                             }
 inlineRG         Object                     {
                                               "id": "/subscriptions/<subscription-id>/resourceGroups/parentGroup",
                                               "name": "parentGroup",
                                               "location": "southcentralus",
                                               "properties": {
                                                 "provisioningState": "Succeeded"
                                               }
                                             }
 linkedRG         Object                     {
                                               "id": "/subscriptions/<subscription-id>/resourceGroups/linkedGroup",
                                               "name": "linkedGroup",
                                               "location": "southcentralus",
                                               "properties": {
                                                 "provisioningState": "Succeeded"
                                               }
                                             }
```

### <a name="azure-cli"></a>Interfaccia della riga di comando di Azure

Per l'interfaccia della riga di comando di Azure, per distribuire due account di archiviazione in due gruppi di risorse nella **stessa sottoscrizione**, usare:

```azurecli-interactive
firstRG="primarygroup"
secondRG="secondarygroup"

az group create --name $firstRG --location southcentralus
az group create --name $secondRG --location eastus
az group deployment create \
  --name ExampleDeployment \
  --resource-group $firstRG \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json \
  --parameters storagePrefix=tfstorage secondResourceGroup=$secondRG secondStorageLocation=eastus
```

Per l'interfaccia della riga di comando di Azure, per distribuire due account di archiviazione in **due sottoscrizioni**, usare:

```azurecli-interactive
firstRG="primarygroup"
secondRG="secondarygroup"

firstSub="<first-subscription-id>"
secondSub="<second-subscription-id>"

az account set --subscription $secondSub
az group create --name $secondRG --location eastus

az account set --subscription $firstSub
az group create --name $firstRG --location southcentralus

az group deployment create \
  --name ExampleDeployment \
  --resource-group $firstRG \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json \
  --parameters storagePrefix=storage secondResourceGroup=$secondRG secondStorageLocation=eastus secondSubscriptionID=$secondSub
```

Per l'interfaccia della riga di comando di Azure, per testare in che modo l'**oggetto gruppo di risorse** si risolve per il modello padre, modello inline e modello collegato, usare:

```azurecli-interactive
az group create --name parentGroup --location southcentralus
az group create --name inlineGroup --location southcentralus
az group create --name linkedGroup --location southcentralus

az group deployment create \
  --name ExampleDeployment \
  --resource-group parentGroup \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json 
```

Nell'esempio precedente, sia **parentRG** che **inlineRG** vengono risolti in **parentGroup**. **linkedRG** viene risolto in **linkedGroup**. L'output dell'esempio precedente è:

```azurecli
...
"outputs": {
  "inlineRG": {
    "type": "Object",
    "value": {
      "id": "/subscriptions/<subscription-id>/resourceGroups/parentGroup",
      "location": "southcentralus",
      "name": "parentGroup",
      "properties": {
        "provisioningState": "Succeeded"
      }
    }
  },
  "linkedRG": {
    "type": "Object",
    "value": {
      "id": "/subscriptions/<subscription-id>/resourceGroups/linkedGroup",
      "location": "southcentralus",
      "name": "linkedGroup",
      "properties": {
        "provisioningState": "Succeeded"
      }
    }
  },
  "parentRG": {
    "type": "Object",
    "value": {
      "id": "/subscriptions/<subscription-id>/resourceGroups/parentGroup",
      "location": "southcentralus",
      "name": "parentGroup",
      "properties": {
        "provisioningState": "Succeeded"
      }
    }
  }
},
...
```

## <a name="next-steps"></a>Passaggi successivi

* Per informazioni su come definire i parametri nel modello, vedere [Comprendere la struttura e la sintassi dei modelli di Azure Resource Manager](resource-group-authoring-templates.md).
* Per suggerimenti su come risolvere i comuni errori di distribuzione, vedere [Risolvere errori comuni durante la distribuzione di risorse in Azure con Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Per informazioni sulla distribuzione di un modello che richiede un token di firma di accesso condiviso, vedere [Distribuire un modello privato con un token di firma di accesso condiviso](resource-manager-powershell-sas-token.md).
