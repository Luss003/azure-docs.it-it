---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 08/19/2019
ms.author: tomfitz
ms.openlocfilehash: 25928ef35da1ce4b3824303a5d46749c32aa701f
ms.sourcegitcommit: cd70273f0845cd39b435bd5978ca0df4ac4d7b2c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2019
ms.locfileid: "69626353"
---
| Risorsa | Limite predefinito | Limite massimo |
| --- | --- | --- |
| Risorse per [gruppo di risorse](../articles/azure-resource-manager/resource-group-overview.md#resource-groups), per tipo di risorsa |800 |Alcuni tipi di risorse possono superare il limite di 800. Vedere [risorse non limitate a 800 di istanze per gruppo di risorse](../articles/azure-resource-manager/resources-without-rg-limit.md). |
| Distribuzioni per gruppo di risorse nella cronologia della distribuzione |800<sup>1</sup> |800 |
| Risorse per distribuzione |800 |800 |
| Blocchi di gestione per ambito univoco |20 |20 |
| Numero di tag per risorsa o gruppo di risorse |50 |50 |
| Lunghezza della chiave dei tag |512 |512 |
| Lunghezza del valore dei tag |256 |256 |

<sup>1</sup> Se si raggiunge il limite di 800 distribuzioni per gruppo di risorse, eliminare le distribuzioni dalla cronologia che non sono più necessarie. L'eliminazione di una voce dalla cronologia di distribuzione non influisce sulle risorse distribuite. È possibile eliminare le voci dalla cronologia usando [az group deployment delete](/cli/azure/group/deployment) nell'interfaccia della riga di comando di Azure o [Remove-AzResourceGroupDeployment](/powershell/module/az.resources/remove-azresourcegroupdeployment) in PowerShell.  Per uno script di PowerShell che automatizza l'eliminazione delle distribuzioni in uno scenario di integrazione continua e recapito continuo (CI/CD), vedere [Remove-Deployments. ps1](https://gist.github.com/bmoore-msft/ed33fb940dafb09380174b7fca57651f).

#### <a name="template-limits"></a>Limiti del modello

| Value | Limite predefinito | Limite massimo |
| --- | --- | --- |
| Parametri |256 |256 |
| Variabili |256 |256 |
| Risorse (incluso il numero di copie) |800 |800 |
| Output |64 |64 |
| Espressione del modello |24.576 caratteri |24.576 caratteri |
| Risorse in modelli esportati |200 |200 | 
| Dimensione del modello |4 MB |4 MB |
| Dimensione del file di parametri |64 KB |64 KB |

È possibile superare alcuni limiti del modello usando un modello annidato. Per altre informazioni, vedere [usare i modelli collegati quando si distribuiscono risorse di Azure](../articles/azure-resource-manager/resource-group-linked-templates.md). Per ridurre il numero di parametri, variabili o output, è possibile combinare più valori in un oggetto. Per altre informazioni, vedere [Oggetti come parametri](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).
