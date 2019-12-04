---
title: Esempio - Applicare un tag e un valore a gruppi di risorse
description: Questa definizione di criteri di esempio richiede un tag e un valore definiti in un parametro in un gruppo di risorse.
ms.date: 01/31/2019
ms.topic: sample
ms.openlocfilehash: d04c48e2633e1a23990723c91a66cf8ec219b160
ms.sourcegitcommit: 95931aa19a9a2f208dedc9733b22c4cdff38addc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2019
ms.locfileid: "74463629"
---
# <a name="sample---enforce-tag-and-its-value-on-resource-groups"></a>Esempio - Applicare un tag e il relativo valore a gruppi di risorse

Questo criterio richiede un tag e un valore in un gruppo di risorse. Si specificano il nome del tag e il valore obbligatori.

È possibile distribuire questo criterio di esempio tramite:

- Il[portale di Azure](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [Interfaccia della riga di comando di Azure](#azure-cli)
- [API REST](#rest-api)

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-policy"></a>Criterio di esempio

### <a name="policy-definition"></a>Definizione di criteri

Definizione JSON completa del criterio, usata dall'API REST, dai pulsanti "Distribuisci in Azure" e manualmente nel portale.

[!code-json[main](../../../../policy-templates/samples/ResourceGroup/enforce-resourceGroup-tags/azurepolicy.json "Enforce tag and its value on resource groups")]

> [!NOTE]
> Se si crea manualmente un criterio nel portale, usare le sezioni **properties.parameters**  e  **properties.policyRule** dell'esempio precedente. Eseguire il wrapping delle due sezioni con le parentesi graffe `{}` per renderle di formato JSON valido.

### <a name="policy-rules"></a>Regole del criterio

Il codice JSON che definisce le regole del criterio, usato dall'interfaccia della riga di comando di Azure e Azure PowerShell.

[!code-json[rule](../../../../policy-templates/samples/ResourceGroup/enforce-resourceGroup-tags/azurepolicy.rules.json "Policy rules (JSON)")]

### <a name="policy-parameters"></a>Parametri dei criteri

Il codice JSON che definisce i parametri del criterio, usato dall'interfaccia della riga di comando di Azure e Azure PowerShell.

[!code-json[parameters](../../../../policy-templates/samples/ResourceGroup/enforce-resourceGroup-tags/azurepolicy.parameters.json "Policy parameters (JSON)")]

|NOME |Type |Campo |DESCRIZIONE |
|---|---|---|---|
|tagName |string |tags |Nome del tag, ad esempio costCenter|
|tagValue |string |tags |Valore del tag, ad esempio headquarter|

Quando si crea un'assegnazione tramite PowerShell o l'interfaccia della riga di comando di Azure, i valori dei parametri possono essere passati in formato JSON in una stringa o tramite un file usando `-PolicyParameter` (PowerShell) o `--params` (interfaccia della riga di comando di Azure).
PowerShell supporta anche `-PolicyParameterObject`, che richiede di passare al cmdlet una tabella hash nome/valore il cmdlet in cui il **nome** è il nome del parametro e il **valore** è il valore singolo o la matrice di valori passati durante assegnazione.

In questo parametro di esempio vengono definiti un tag _tagName_ **costCenter** e un tag _tagValue_ **headquarter**.

```json
{
    "tagName": {
        "value": "costCenter"
    },
    "tagValue": {
        "value": "headquarter"
    }
}
```

## <a name="azure-portal"></a>Portale di Azure

[![Distribuire l'esempio in Azure](../media/deploy/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FResourceGroup%2Fenforce-resourceGroup-tags%2Fazurepolicy.json)
[![Distribuire l'esempio in Azure Gov](../media/deploy/deployGovbutton.png)](https://portal.azure.us/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FResourceGroup%2Fenforce-resourceGroup-tags%2Fazurepolicy.json)

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

### <a name="deploy-with-azure-powershell"></a>Distribuire con Azure PowerShell

```azurepowershell-interactive
# Create the Policy Definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'enforce-resourceGroup-tags' -DisplayName 'Enforce tag and its value on resource groups' -description 'Enforces a required tag and its value on resource groups.' -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/ResourceGroup/enforce-resourceGroup-tags/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/ResourceGroup/enforce-resourceGroup-tags/azurepolicy.parameters.json' -Mode All

# Set the scope to a resource group; may also be a resource, subscription, or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Set the Policy Parameter (JSON format)
$policyParam = '{ "tagName": { "value": "costCenter" }, "tagValue": { "value": "headquarter" } }'

# Create the Policy Assignment
$assignment = New-AzPolicyAssignment -Name 'enforce-resourceGroup-tags-assignment' -Scope $scope.ResourceId -PolicyDefinition $definition -PolicyParameter $policyParam
```

### <a name="remove-with-azure-powershell"></a>Rimuovere con Azure PowerShell

Eseguire questi comandi per rimuovere l'assegnazione e la definizione precedenti:

```azurepowershell-interactive
# Remove the Policy Assignment
Remove-AzPolicyAssignment -Id $assignment.ResourceId

# Remove the Policy Definition
Remove-AzPolicyDefinition -Id $definition.ResourceId
```

### <a name="azure-powershell-explanation"></a>Spiegazione di Azure PowerShell

Gli script di distribuzione e rimozione usano i comandi seguenti. Ogni comando della tabella seguente include collegamenti alla documentazione specifica del comando:

| Comando | Note |
|---|---|
| [New-AzPolicyDefinition](/powershell/module/az.resources/New-Azpolicydefinition) | Crea una nuova definizione di Criteri di Azure. |
| [Get-AzResourceGroup](/powershell/module/az.resources/Get-Azresourcegroup) | Ottiene un singolo gruppo di risorse. |
| [New-AzPolicyAssignment](/powershell/module/az.resources/New-Azpolicyassignment) | Crea una nuova assegnazione di Criteri di Azure. In questo esempio si specifica una definizione, ma può assumere un'iniziativa. |
| [Remove-AzPolicyAssignment](/powershell/module/az.resources/Remove-Azpolicyassignment) | Rimuove un'assegnazione di Criteri di Azure esistente. |
| [Remove-AzPolicyDefinition](/powershell/module/az.resources/Remove-Azpolicydefinition) | Rimuove una definizione di Criteri di Azure esistente. |

## <a name="azure-cli"></a>Interfaccia della riga di comando di Azure

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

### <a name="deploy-with-azure-cli"></a>Distribuire con l'interfaccia della riga di comando di Azure

```azurecli-interactive
# Create the Policy Definition (Subscription scope)
definition=$(az policy definition create --name 'enforce-resourceGroup-tags' --display-name 'Enforce tag and its value on resource groups' --description 'Enforces a required tag and its value on resource groups.' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/ResourceGroup/enforce-resourceGroup-tags/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/ResourceGroup/enforce-resourceGroup-tags/azurepolicy.parameters.json' --mode All)

# Set the scope to a resource group; may also be a resource, subscription, or management group
scope=$(az group show --name 'YourResourceGroup')

# Set the Policy Parameter (JSON format)
policyParam='{ "tagName": { "value": "costCenter" }, "tagValue": { "value": "headquarter" } }'

# Create the Policy Assignment
assignment=$(
az policy assignment create --name 'enforce-resourceGroup-tags-assignment' --display-name 'Enforce tag and its value on resource groups'  --scope `echo $scope | jq '.id' -r` --policy `echo $definition | jq '.name' -r` --params "$policyparam")
```

### <a name="remove-with-azure-cli"></a>Rimuovere con l'interfaccia della riga di comando di Azure

Eseguire questi comandi per rimuovere l'assegnazione e la definizione precedenti:

```azurecli-interactive
# Remove the Policy Assignment
az policy assignment delete --name `echo $assignment | jq '.name' -r`

# Remove the Policy Definition
az policy definition delete --name `echo $definition | jq '.name' -r`
```

### <a name="azure-cli-explanation"></a>Spiegazione dell'interfaccia della riga di comando di Azure

| Comando | Note |
|---|---|
| [az policy definition create](/cli/azure/policy/definition?view=azure-cli-latest#az-policy-definition-create) | Crea una nuova definizione di Criteri di Azure. |
| [az group show](/cli/azure/group?view=azure-cli-latest#az-group-show) | Ottiene un singolo gruppo di risorse. |
| [az policy assignment create](/cli/azure/policy/assignment?view=azure-cli-latest#az-policy-assignment-create) | Crea una nuova assegnazione di Criteri di Azure. In questo esempio si specifica una definizione, ma può assumere un'iniziativa. |
| [az policy assignment delete](/cli/azure/policy/assignment?view=azure-cli-latest#az-policy-assignment-delete) | Rimuove un'assegnazione di Criteri di Azure esistente. |
| [az policy definition delete](/cli/azure/policy/definition?view=azure-cli-latest#az-policy-definition-delete) | Rimuove una definizione di Criteri di Azure esistente. |

Sono disponibili diversi strumenti che possono essere usati per interagire con l'API REST di Resource Manager, ad esempio [ARMClient](https://github.com/projectkudu/ARMClient) o PowerShell. Un esempio di chiamata all'API REST da PowerShell è disponibile nella sezione **alias** della [struttura della definizione del criterio](../concepts/definition-structure.md#aliases).

## <a name="rest-api"></a>API REST

### <a name="deploy-with-rest-api"></a>Distribuire con l'API REST

- Creare la definizione del criterio (ambito della sottoscrizione). Usare il codice JSON di [definizione del criterio](#policy-definition) per il corpo della richiesta.

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/enforce-resourceGroup-tags?api-version=2016-12-01
  ```

- Creare l'assegnazione del criterio (ambito del gruppo di risorse)

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/YourResourceGroup/providers/Microsoft.Authorization/policyAssignments/enforce-resourceGroup-tags-assignment?api-version=2017-06-01-preview
  ```

  Usare questo codice JSON di esempio per il corpo della richiesta:

```json
  {
      "properties": {
          "displayName": "Enforce tag and its value Assignment",
          "policyDefinitionId": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/enforce-resourceGroup-tags",
          "parameters": {
              "tagName": {
                  "value": "costCenter"
              },
              "tagValue": {
                  "value": "headquarter"
              }
          }
      }
  }
  ```

### <a name="remove-with-rest-api"></a>Rimuovere con l'API REST

- Rimuovere l'assegnazione del criterio

  ```http
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/enforce-resourceGroup-tags-assignment?api-version=2017-06-01-preview
  ```

- Rimuovere la definizione del criterio

  ```http
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/enforce-resourceGroup-tags?api-version=2016-12-01
  ```

### <a name="rest-api-explanation"></a>Spiegazione dell'API REST

| Service | Gruppo | Operazione | Note |
|---|---|---|---|
| Gestione delle risorse | Definizioni dei criteri | [Creare](/rest/api/resources/policydefinitions/createorupdate) | Crea una nuova definizione di Criteri di Azure a livello della sottoscrizione. Alternativa: [Creare a livello di gruppo di gestione](/rest/api/resources/policydefinitions/createorupdateatmanagementgroup) |
| Gestione delle risorse | Assegnazioni di criteri | [Creare](/rest/api/resources/policyassignments/create) | Crea una nuova assegnazione di Criteri di Azure. In questo esempio si specifica una definizione, ma può assumere un'iniziativa. |
| Gestione delle risorse | Assegnazioni di criteri | [Elimina](/rest/api/resources/policyassignments/delete) | Rimuove un'assegnazione di Criteri di Azure esistente. |
| Gestione delle risorse | Definizioni dei criteri | [Elimina](/rest/api/resources/policydefinitions/delete) | Rimuove una definizione di Criteri di Azure esistente. Alternativa: [Eliminare a livello di gruppo di gestione](/rest/api/resources/policydefinitions/deleteatmanagementgroup) |

## <a name="next-steps"></a>Passaggi successivi

- Vedere altri [esempi di Criteri di Azure](index.md)
- Vedere [Struttura delle definizioni di Criteri di Azure](../concepts/definition-structure.md)