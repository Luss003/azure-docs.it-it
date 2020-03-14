---
title: Dettagli della struttura di assegnazione dei criteri
description: Descrive la definizione di assegnazione dei criteri usata da criteri di Azure per mettere in relazione le definizioni dei criteri e i parametri alle risorse per la valutazione.
ms.date: 09/23/2019
ms.topic: conceptual
ms.openlocfilehash: f03c654dfc4c8dfdf2bdc5103a5961b4d8ce1e64
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2020
ms.locfileid: "79265298"
---
# <a name="azure-policy-assignment-structure"></a>Struttura di assegnazione di Criteri di Azure

Le assegnazioni di criteri vengono usate dai criteri di Azure per definire a quali risorse sono assegnati i criteri o le iniziative. L'assegnazione di criteri può determinare i valori dei parametri per il gruppo di risorse in fase di assegnazione, rendendo possibile il riutilizzo delle definizioni dei criteri che indirizzano le stesse proprietà delle risorse con diverse esigenze di conformità.

Si usa JSON per creare un'assegnazione di criteri. L'assegnazione di criteri contiene elementi per:

- nome visualizzato
- description
- metadata
- modalità di imposizione
- definizione dei criteri
- parametri

Ad esempio, il codice JSON seguente mostra un'assegnazione di criteri in modalità _DoNotEnforce_ con i parametri dinamici:

```json
{
    "properties": {
        "displayName": "Enforce resource naming rules",
        "description": "Force resource names to begin with DeptA and end with -LC",
        "metadata": {
            "assignedBy": "Cloud Center of Excellence"
        },
        "enforcementMode": "DoNotEnforce",
        "policyDefinitionId": "/subscriptions/{mySubscriptionID}/providers/Microsoft.Authorization/policyDefinitions/ResourceNaming",
        "parameters": {
            "prefix": {
                "value": "DeptA"
            },
            "suffix": {
                "value": "-LC"
            }
        }
    }
}
```

Tutti gli esempi di criteri di Azure sono disponibili in [esempi di criteri di Azure](../samples/index.md).

## <a name="display-name-and-description"></a>Nome visualizzato e descrizione

Usare **DisplayName** e **Description** per identificare l'assegnazione dei criteri e fornire il contesto per l'uso con il set specifico di risorse. **displayName** ha una lunghezza massima di _128_ caratteri e **description** una lunghezza massima di _512_ caratteri.

## <a name="enforcement-mode"></a>Modalità di imposizione

La proprietà **enforcementMode** fornisce ai clienti la possibilità di testare il risultato di un criterio sulle risorse esistenti senza avviare l'effetto del criterio o attivare le voci nel [log attività di Azure](../../../azure-monitor/platform/platform-logs-overview.md). Questo scenario viene in genere definito "What If" e viene allineato a procedure di distribuzione sicure. **enforcementMode** è diverso dall'effetto [disabilitato](./effects.md#disabled) , perché questo effetto impedisce la valutazione delle risorse.

Questa proprietà presenta i valori seguenti:

|Modalità |Valore JSON |Type |Correzione manuale |Voce del log attività |Descrizione |
|-|-|-|-|-|-|
|Abilitato |Default |string |Sì |Sì |L'effetto dei criteri viene applicato durante la creazione o l'aggiornamento delle risorse. |
|Disabled |DoNotEnforce |string |Sì |No | L'effetto dei criteri non viene applicato durante la creazione o l'aggiornamento delle risorse. |

Se **enforcementMode** non è specificato nella definizione di un criterio o di un'iniziativa, viene usato il valore _predefinito_ . È possibile avviare le [attività di correzione](../how-to/remediate-resources.md) per i criteri [deployIfNotExists](./effects.md#deployifnotexists) , anche quando **enforcementMode** è impostato su _DoNotEnforce_.

## <a name="policy-definition-id"></a>ID definizione dei criteri

Questo campo deve essere il nome completo del percorso di una definizione di criteri o di una definizione di iniziativa.
`policyDefinitionId` è una stringa e non una matrice. Se più criteri vengono spesso assegnati insieme, è consigliabile usare invece un' [iniziativa](./definition-structure.md#initiatives) .

## <a name="parameters"></a>Parametri

Questo segmento dell'assegnazione di criteri fornisce i valori per i parametri definiti nella definizione dei [criteri o nella definizione di iniziativa](./definition-structure.md#parameters).
Questa progettazione rende possibile il riutilizzo di una definizione di criteri o di un'iniziativa con diverse risorse, ma verificare la presenza di valori o risultati aziendali diversi.

```json
"parameters": {
    "prefix": {
        "value": "DeptA"
    },
    "suffix": {
        "value": "-LC"
    }
}
```

In questo esempio, i parametri definiti in precedenza nella definizione dei criteri sono `prefix` e `suffix`. Questa particolare assegnazione di criteri imposta `prefix` a **depta** e `suffix` a **-LC**. La stessa definizione di criteri è riutilizzabile con un diverso set di parametri per un reparto diverso, riducendo la duplicazione e la complessità delle definizioni dei criteri, garantendo al tempo stesso flessibilità.

## <a name="next-steps"></a>Passaggi successivi

- Informazioni sulla [struttura della definizione dei criteri](./definition-structure.md).
- Informazioni su come [creare criteri a livello di codice](../how-to/programmatically-create.md).
- Informazioni su come [ottenere i dati di conformità](../how-to/get-compliance-data.md).
- Informazioni su come monitorare e [aggiornare le risorse non conformi](../how-to/remediate-resources.md).
- Rivedere le caratteristiche di un gruppo di gestione illustrate in [Organizzare le risorse con i gruppi di gestione di Azure](../../management-groups/overview.md).