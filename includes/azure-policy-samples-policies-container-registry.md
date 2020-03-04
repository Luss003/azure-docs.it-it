---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/02/2020
ms.author: dacoulte
ms.openlocfilehash: 57cbd8d208422dfc124bed08109250e7bdd360d5
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/03/2020
ms.locfileid: "78262718"
---
|Nome |Descrizione |Effetto/i |Versione |GitHub |
|---|---|---|---|---|
|[I registri Container devono essere crittografati con una chiave gestita dal cliente](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F5b9159ae-1701-4a6f-9a7a-aa9c8ddd0580) |Controlla i registri Container per cui non è abilitata la crittografia con chiavi gestite dal cliente. Per altre informazioni sulla crittografia con chiavi gestite dal cliente, vedere: https://aka.ms/acr/CMK. |Audit, Disabled |1.0.0-preview |[Collegamento](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_CMKEncryptionEnabled_Audit.json)
|[I registri Container non devono consentire l'accesso alla rete senza restrizioni](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fd0793b48-0edc-4296-a390-4c75d1bdfd71) |Controlla i registri Container per cui non sono configurate regole di rete (IP o rete virtuale) e che consentono l'accesso a tutte le reti per impostazione predefinita. I registri Container con almeno una regola IP/firewall o una rete virtuale configurata saranno considerati conformi. Per altre informazioni sulle regole di rete del registro Container, vedere: https://aka.ms/acr/vnet. |Audit, Disabled |1.0.0-preview |[Collegamento](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_NetworkRulesExist_Audit.json)
