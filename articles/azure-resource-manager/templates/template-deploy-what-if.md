---
title: Distribuzione modelli simulazione (anteprima)
description: Determinare quali modifiche si verificheranno nelle risorse prima di distribuire un modello di Azure Resource Manager.
author: mumian
ms.topic: conceptual
ms.date: 03/05/2020
ms.author: jgao
ms.openlocfilehash: b9d4150779842614a5dc284a2b3a489593fabfe1
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2020
ms.locfileid: "78388550"
---
# <a name="resource-manager-template-deployment-what-if-operation-preview"></a>Gestione risorse l'operazione di simulazione della distribuzione del modello (anteprima)

Prima di distribuire un modello, è possibile visualizzare l'anteprima delle modifiche che si verificheranno. Azure Resource Manager fornisce l'operazione di simulazione per visualizzare il modo in cui le risorse vengono modificate se si distribuisce il modello. L'operazione di simulazione non consente di apportare modifiche alle risorse esistenti. Vengono invece stimate le modifiche se il modello specificato viene distribuito.

> [!NOTE]
> L'operazione di simulazione è attualmente in anteprima. Per usarlo, è necessario [iscriversi per l'anteprima](https://aka.ms/armtemplatepreviews). Come versione di anteprima, i risultati possono a volte indicare che una risorsa cambierà quando in realtà non si verifica alcuna modifica. Ci stiamo impegnando per ridurre questi problemi, ma è necessario aiutarti. Segnalare questi problemi in [https://aka.ms/whatifissues](https://aka.ms/whatifissues).

È possibile usare l'operazione di simulazione con i comandi di PowerShell o le operazioni dell'API REST.

In PowerShell l'output include risultati codificati a colori che consentono di visualizzare i diversi tipi di modifiche.

![Gestione risorse l'operazione di simulazione della distribuzione del modello fullresourcepayload e i tipi di modifica](./media/template-deploy-what-if/resource-manager-deployment-whatif-change-types.png)

Il testo ouptput è:

```powershell
Resource and property changes are indicated with these symbols:
  - Delete
  + Create
  ~ Modify

The deployment will update the following scope:

Scope: /subscriptions/./resourceGroups/ExampleGroup

  ~ Microsoft.Network/virtualNetworks/vnet-001 [2018-10-01]
    - tags.Owner: "Team A"
    ~ properties.addressSpace.addressPrefixes: [
      - 0: "10.0.0.0/16"
      + 0: "10.0.0.0/15"
      ]
    ~ properties.subnets: [
      - 0:

          name:                     "subnet001"
          properties.addressPrefix: "10.0.0.0/24"

      ]

Resource changes: 1 to modify.
```

## <a name="what-if-commands"></a>Comandi di simulazione

È possibile usare Azure PowerShell o l'API REST di Azure per l'operazione di simulazione.

### <a name="azure-powershell"></a>Azure PowerShell

Per visualizzare un'anteprima delle modifiche prima di distribuire un modello, aggiungere il parametro `-Whatif` switch al comando Deployment.

* `New-AzResourceGroupDeployment -Whatif` per le distribuzioni di gruppi di risorse
* `New-AzSubscriptionDeployment -Whatif` e `New-AzDeployment -Whatif` per le distribuzioni a livello di sottoscrizione

In alternativa, è possibile usare il parametro switch `-Confirm` per visualizzare l'anteprima delle modifiche e ricevere la richiesta di continuare con la distribuzione.

* `New-AzResourceGroupDeployment -Confirm` per le distribuzioni di gruppi di risorse
* `New-AzSubscriptionDeployment -Confirm` e `New-AzDeployment -Confirm` per le distribuzioni a livello di sottoscrizione

I comandi precedenti restituiscono un riepilogo di testo che è possibile ispezionare manualmente. Per ottenere un oggetto a cui è possibile controllare a livello di codice le modifiche, usare:

* `$results = Get-AzResourceGroupDeploymentWhatIf` per le distribuzioni di gruppi di risorse
* `$results = Get-AzSubscriptionDeploymentWhatIf` o `$results = Get-AzDeploymentWhatIf` per le distribuzioni a livello di sottoscrizione

> [!NOTE]
> Prima del rilascio della versione 2.0.1-alpha5, è stato usato il comando `New-AzDeploymentWhatIf`. Questo comando è stato sostituito dai comandi `Get-AzDeploymentWhatIf`, `Get-AzResourceGroupDeploymentWhatIf`e `Get-AzSubscriptionDeploymentWhatIf`. Se è stata usata una versione precedente, è necessario aggiornare tale sintassi. Il parametro `-ScopeType` è stato rimosso.

### <a name="azure-rest-api"></a>API REST di Azure

Per l'API REST, usare:

* [Distribuzioni-What If](/rest/api/resources/deployments/whatif) per le distribuzioni di gruppi di risorse
* [Distribuzioni-What If nell'ambito della sottoscrizione](/rest/api/resources/deployments/whatifatsubscriptionscope) per le distribuzioni a livello di sottoscrizione

## <a name="change-types"></a>Tipi di modifiche

L'operazione di simulazione elenca sei tipi diversi di modifiche:

- **Create**: la risorsa attualmente non esiste ma è definita nel modello. La risorsa verrà creata.

- **Elimina**: questo tipo di modifica si applica solo quando si usa la [modalità completa](deployment-modes.md) per la distribuzione. La risorsa esiste, ma non è definita nel modello. Con la modalità completa, la risorsa verrà eliminata. Solo le risorse che [supportano l'eliminazione in modalità completa](complete-mode-deletion.md) sono incluse in questo tipo di modifica.

- **Ignore**: la risorsa esiste, ma non è definita nel modello. La risorsa non verrà distribuita o modificata.

- **NoChange**: la risorsa esiste e viene definita nel modello. La risorsa verrà ridistribuita, ma le proprietà della risorsa non verranno modificate. Questo tipo di modifica viene restituito quando [ResultFormat](#result-format) è impostato su `FullResourcePayloads`, che corrisponde al valore predefinito.

- **Modifica**: la risorsa esiste e viene definita nel modello. La risorsa verrà ridistribuita e le proprietà della risorsa verranno modificate. Questo tipo di modifica viene restituito quando [ResultFormat](#result-format) è impostato su `FullResourcePayloads`, che corrisponde al valore predefinito.

- **Deploy**: la risorsa esiste e viene definita nel modello. La risorsa verrà ridistribuita. Le proprietà della risorsa possono essere modificate o meno. Tramite l'operazione viene restituito questo tipo di modifica quando non sono disponibili informazioni sufficienti per determinare se le proprietà cambiano. Questa condizione viene visualizzata solo quando [ResultFormat](#result-format) è impostato su `ResourceIdOnly`.

## <a name="result-format"></a>Formato risultato

È possibile controllare il livello di dettaglio restituito sulle modifiche previste. Nei comandi di distribuzione (`New-Az*Deployment`) usare il parametro **-WhatIfResultFormat** . Nei comandi dell'oggetto programmatico (`Get-Az*DeploymentWhatIf`), usare il parametro **ResultFormat** .

Impostare il parametro format su **FullResourcePayloads** per ottenere un elenco di risorse che cambieranno e dettagli sulle proprietà che cambieranno. Impostare il parametro format su **ResourceIdOnly** per ottenere un elenco di risorse che cambieranno. Il valore predefinito è **FullResourcePayloads**.  

I risultati seguenti mostrano i due diversi formati di output:

- Payload di risorse completi

  ```powershell
  Resource and property changes are indicated with these symbols:
    - Delete
    + Create
    ~ Modify

  The deployment will update the following scope:

  Scope: /subscriptions/./resourceGroups/ExampleGroup

    ~ Microsoft.Network/virtualNetworks/vnet-001 [2018-10-01]
      - tags.Owner: "Team A"
      ~ properties.addressSpace.addressPrefixes: [
        - 0: "10.0.0.0/16"
        + 0: "10.0.0.0/15"
        ]
      ~ properties.subnets: [
        - 0:

          name:                     "subnet001"
          properties.addressPrefix: "10.0.0.0/24"

        ]

  Resource changes: 1 to modify.
  ```

- Solo ID risorsa

  ```powershell
  Resource and property changes are indicated with this symbol:
    ! Deploy

  The deployment will update the following scope:

  Scope: /subscriptions/./resourceGroups/ExampleGroup

    ! Microsoft.Network/virtualNetworks/vnet-001

  Resource changes: 1 to deploy.
  ```

## <a name="run-what-if-operation"></a>Eseguire un'operazione di simulazione

### <a name="set-up-environment"></a>Configurare l'ambiente

Per verificarne il funzionamento, è possibile eseguire alcuni test. Per prima cosa, distribuire un modello per la [creazione di una rete virtuale](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/what-if/what-if-before.json). Questa rete virtuale verrà usata per verificare il modo in cui vengono segnalate le modifiche da simulazione.

```azurepowershell
New-AzResourceGroup `
  -Name ExampleGroup `
  -Location centralus
New-AzResourceGroupDeployment `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-before.json"
```

### <a name="test-modification"></a>Modifica test

Al termine della distribuzione, si è pronti per testare l'operazione di simulazione. Questa volta si distribuisce un [modello che modifica la rete virtuale](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/what-if/what-if-after.json). Mancano uno dei tag originali, una subnet è stata rimossa e il prefisso dell'indirizzo è stato modificato.

```azurepowershell
New-AzResourceGroupDeployment `
  -Whatif `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-after.json"
```

L'output di simulazione appare simile al seguente:

![Gestione risorse l'output dell'operazione di simulazione della distribuzione del modello](./media/template-deploy-what-if/resource-manager-deployment-whatif-change-types.png)

L'output di testo è:

```powershell
Resource and property changes are indicated with these symbols:
  - Delete
  + Create
  ~ Modify

The deployment will update the following scope:

Scope: /subscriptions/./resourceGroups/ExampleGroup

  ~ Microsoft.Network/virtualNetworks/vnet-001 [2018-10-01]
    - tags.Owner: "Team A"
    ~ properties.addressSpace.addressPrefixes: [
      - 0: "10.0.0.0/16"
      + 0: "10.0.0.0/15"
      ]
    ~ properties.subnets: [
      - 0:

        name:                     "subnet001"
        properties.addressPrefix: "10.0.0.0/24"

      ]

Resource changes: 1 to modify.
```

Si noti che nella parte superiore dell'output vengono definiti i colori per indicare il tipo di modifiche.

Nella parte inferiore dell'output è indicato che il proprietario del tag è stato eliminato. Il prefisso dell'indirizzo è stato modificato da 10.0.0.0/16 a 10.0.0.0/15. La subnet denominata subnet001 è stata eliminata. Tenere presente che le modifiche non sono state effettivamente distribuite. Viene visualizzata un'anteprima delle modifiche che si verificheranno se si distribuisce il modello.

Alcune delle proprietà elencate come eliminate non cambiano effettivamente. Le proprietà possono essere segnalate erroneamente come eliminate quando non sono incluse nel modello, ma vengono impostate automaticamente durante la distribuzione come valori predefiniti. Questo risultato viene considerato "Noise" nella risposta di simulazione. La risorsa finale distribuita avrà i valori impostati per le proprietà. Quando l'operazione di simulazione è matura, queste proprietà verranno filtrate fuori dal risultato.

## <a name="programmatically-evaluate-what-if-results"></a>Valutare i risultati di simulazione a livello di codice

A questo punto, è possibile valutare i risultati di simulazione a livello di codice impostando il comando su una variabile.

```azurepowershell
$results = Get-AzResourceGroupDeploymentWhatIf `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-after.json"
```

È possibile visualizzare un riepilogo di ogni modifica.

```azurepowershell
foreach ($change in $results.Changes)
{
  $change.Delta
}
```

## <a name="confirm-deletion"></a>Confermare l'eliminazione

L'operazione di simulazione supporta l'uso della [modalità di distribuzione](deployment-modes.md). Quando viene impostata la modalità completa, le risorse non presenti nel modello vengono eliminate. Nell'esempio seguente viene distribuito un [modello privo di risorse definite](https://github.com/Azure/azure-docs-json-samples/blob/master/empty-template/azuredeploy.json) in modalità completa.

Per visualizzare in anteprima le modifiche prima di distribuire un modello, usare il parametro `-Confirm` switch con il comando Deployment. Se le modifiche sono quelle previste, confermare che si desidera completare la distribuzione.

```azurepowershell
New-AzResourceGroupDeployment `
  -Confirm `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/empty-template/azuredeploy.json" `
  -Mode Complete
```

Poiché nel modello non sono definite risorse e la modalità di distribuzione è impostata su completa, la rete virtuale verrà eliminata.

![Modalità di distribuzione dell'output dell'operazione di distribuzione del modello di Gestione risorse completamento](./media/template-deploy-what-if/resource-manager-deployment-whatif-output-mode-complete.png)

L'output di testo è:

```powershell
Resource and property changes are indicated with this symbol:
  - Delete

The deployment will update the following scope:

Scope: /subscriptions/./resourceGroups/ExampleGroup

  - Microsoft.Network/virtualNetworks/vnet-001

      id:
"/subscriptions/./resourceGroups/ExampleGroup/providers/Microsoft.Network/virtualNet
works/vnet-001"
      location:        "centralus"
      name:            "vnet-001"
      tags.CostCenter: "12345"
      tags.Owner:      "Team A"
      type:            "Microsoft.Network/virtualNetworks"

Resource changes: 1 to delete.

Are you sure you want to execute the deployment?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
```

Vengono visualizzate le modifiche previste e è possibile confermare che si desidera eseguire la distribuzione.

## <a name="next-steps"></a>Passaggi successivi

- Se si notano risultati non corretti dalla versione di anteprima di simulazione, segnalare i problemi in [https://aka.ms/whatifissues](https://aka.ms/whatifissues).
- Per distribuire i modelli con Azure PowerShell, vedere [distribuire le risorse con i modelli e Azure PowerShell di gestione risorse](deploy-powershell.md).
- Per distribuire i modelli con REST, vedere [distribuire le risorse con gestione risorse modelli e gestione risorse API REST](deploy-rest.md).
