---
title: Esempio - Controllare l'impostazione di diagnostica
description: Questa definizione di criteri di esempio controlla se le impostazioni di diagnostica non sono abilitate per tipi di risorse specifici.
author: DCtheGeek
ms.service: azure-policy
ms.topic: sample
ms.date: 01/23/2019
ms.author: dacoulte
ms.openlocfilehash: b71876195ffd3993b49794835781e8ccbd1648d3
ms.sourcegitcommit: d7689ff43ef1395e61101b718501bab181aca1fa
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/06/2019
ms.locfileid: "71977467"
---
# <a name="sample---audit-diagnostic-setting"></a>Esempio - Controllare l'impostazione di diagnostica

Questo criterio predefinito controlla se le impostazioni di diagnostica non sono abilitate per i tipi di risorsa specificati. Si specifica una matrice di tipi di risorse per controllare se le impostazioni di diagnostica sono abilitate.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Modello di esempio

[!code-json[main](../../../../policy-templates/samples/Monitoring/audit-diagnostic-setting/azurepolicy.json "Audit diagnostic setting")]

È possibile distribuire questo modello usando il [portale di Azure](#deploy-with-the-portal), con [PowerShell](#deploy-with-powershell) o con l'[interfaccia della riga di comando di Azure](#deploy-with-azure-cli). Per ottenere i criteri predefiniti, usare l'ID `7f89b1eb-583c-429a-8828-af049802c1d9`.

## <a name="parameters"></a>Parametri

Per accettare il valore del parametro, usare il formato seguente:

```json
{"listOfResourceTypes":{"value":["Microsoft.Cache/Redis","Microsoft.Compute/virtualmachines"]}}
```

## <a name="deploy-with-the-portal"></a>Eseguire la distribuzione con il portale

Quando si assegna un criterio, selezionare **Controlla impostazione di diagnostica** dalle definizioni predefinite disponibili.

## <a name="deploy-with-powershell"></a>Distribuire con PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

```azurepowershell-interactive
$definition = Get-AzPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/7f89b1eb-583c-429a-8828-af049802c1d9

New-AzPolicyAssignment -name "Audit diagnostics" -PolicyDefinition $definition -PolicyParameter '{"listOfResourceTypes":{"value":["Microsoft.Cache/Redis","Microsoft.Compute/virtualmachines"]}}' -Scope <scope>
```

### <a name="clean-up-powershell-deployment"></a>Eliminare la distribuzione di PowerShell

Eseguire questo comando per rimuovere il gruppo di risorse, la macchina virtuale e tutte le risorse correlate.

```azurepowershell-interactive
Remove-AzPolicyAssignment -Name "Audit diagnostics" -Scope <scope>
```

## <a name="deploy-with-azure-cli"></a>Distribuire con l'interfaccia della riga di comando di Azure

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

```azurecli-interactive
az policy assignment create --scope <scope> --name "Audit diagnostics" --policy 7f89b1eb-583c-429a-8828-af049802c1d9 --params '{"listOfResourceTypes":{"value":["Microsoft.Cache/Redis","Microsoft.Compute/virtualmachines"]}}'
```

### <a name="clean-up-azure-cli-deployment"></a>Eliminare la distribuzione dell'interfaccia della riga di comando di Azure

Eseguire questo comando per rimuovere il gruppo di risorse, la macchina virtuale e tutte le risorse correlate.

```azurecli-interactive
az policy assignment delete --name "Audit diagnostics" --resource-group myResourceGroup
```

## <a name="next-steps"></a>Passaggi successivi

- Vedere altri esempi in [Esempi di Criteri di Azure](index.md)