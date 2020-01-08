---
title: INTERFACCIA della riga di comando di Azure Service Fabric-sfctl rpm
description: Informazioni su sfctl, l'interfaccia della riga di comando di Azure Service Fabric. Include un elenco di comandi per il servizio di gestione della riparazione.
author: jeffj6123
ms.topic: reference
ms.date: 9/17/2019
ms.author: jejarry
ms.openlocfilehash: 674970276046034d13801db7c1bb4ab5175385fb
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/03/2020
ms.locfileid: "75639089"
---
# <a name="sfctl-rpm"></a>sfctl rpm
Consente di eseguire query e inviare comandi al servizio di gestione di ripristino.

## <a name="commands"></a>Comandi:

|Comando|Description|
| --- | --- |
| approve-force | Forza l'approvazione dell'attività di ripristino specificata. |
| delete | Elimina un'attività di ripristino completata. |
| list | Recupera un elenco di attività di ripristino corrispondenti ai filtri specificati. |

## <a name="sfctl-rpm-approve-force"></a>sfctl rpm approve-force
Forza l'approvazione dell'attività di ripristino specificata.

Questa API supporta la piattaforma Service Fabric. non deve essere utilizzata direttamente dal codice.

### <a name="arguments"></a>Argomenti

|Argomento|Description|
| --- | --- |
| --task-id [obbligatorio] | ID dell'attività di ripristino. |
| --version | Il numero di versione corrente dell'attività di ripristino. Se è diverso da zero, la richiesta verrà completata solo se questo valore corrisponde alla versione corrente effettiva dell'attività di ripristino. Se è zero, non viene eseguito alcun controllo di versione. |

### <a name="global-arguments"></a>Argomenti globali

|Argomento|Description|
| --- | --- |
| --debug | Aumenta il livello di dettaglio della registrazione per mostrare tutti i log di debug. |
| --help -h | Visualizza questo messaggio della guida ed esce. |
| --output -o | Formato di output.  Valori consentiti\: json, jsonc, table, tsv.  Valore predefinito\: json. |
| --query | Stringa di query JMESPath. Per altre informazioni ed esempi, vedere http\://jmespath.org/. |
| --verbose | Aumenta il livello di dettaglio della registrazione. Usare --debug per log di debug completi. |

## <a name="sfctl-rpm-delete"></a>sfctl rpm delete
Elimina un'attività di ripristino completata.

Questa API supporta la piattaforma Service Fabric. non deve essere utilizzata direttamente dal codice.

### <a name="arguments"></a>Argomenti

|Argomento|Description|
| --- | --- |
| --task-id [obbligatorio] | L'ID dell'attività di ripristino completata da eliminare. |
| --version | Il numero di versione corrente dell'attività di ripristino. Se è diverso da zero, la richiesta verrà completata solo se questo valore corrisponde alla versione corrente effettiva dell'attività di ripristino. Se è zero, non viene eseguito alcun controllo di versione. |

### <a name="global-arguments"></a>Argomenti globali

|Argomento|Description|
| --- | --- |
| --debug | Aumenta il livello di dettaglio della registrazione per mostrare tutti i log di debug. |
| --help -h | Visualizza questo messaggio della guida ed esce. |
| --output -o | Formato di output.  Valori consentiti\: json, jsonc, table, tsv.  Valore predefinito\: json. |
| --query | Stringa di query JMESPath. Per altre informazioni ed esempi, vedere http\://jmespath.org/. |
| --verbose | Aumenta il livello di dettaglio della registrazione. Usare --debug per log di debug completi. |

## <a name="sfctl-rpm-list"></a>sfctl rpm list
Recupera un elenco di attività di ripristino corrispondenti ai filtri specificati.

Questa API supporta la piattaforma Service Fabric. non deve essere utilizzata direttamente dal codice.

### <a name="arguments"></a>Argomenti

|Argomento|Description|
| --- | --- |
| --executor-filter | Il nome dell'executor di ripristino le cui attività richieste devono essere incluse nell'elenco. |
| --state-filter | Un operatore OR bit per bit dei valori seguenti, che specifica gli stati delle attività che devono essere inclusi nell'elenco dei risultati. <br> 1 - Creato <br>2 - Richiesto  <br>4 - Preparazione in corso  <br>8 - Approvato  <br>16 - Esecuzione in corso  <br>32 - Ripristino in corso  <br>64 - Completato |
| --task-id-filter | Il prefisso dell'ID dell'attività di ripristino da ricercare. |

### <a name="global-arguments"></a>Argomenti globali

|Argomento|Description|
| --- | --- |
| --debug | Aumenta il livello di dettaglio della registrazione per mostrare tutti i log di debug. |
| --help -h | Visualizza questo messaggio della guida ed esce. |
| --output -o | Formato di output.  Valori consentiti\: json, jsonc, table, tsv.  Valore predefinito\: json. |
| --query | Stringa di query JMESPath. Per altre informazioni ed esempi, vedere http\://jmespath.org/. |
| --verbose | Aumenta il livello di dettaglio della registrazione. Usare --debug per log di debug completi. |


## <a name="next-steps"></a>Passaggi successivi
- [Configurare](service-fabric-cli.md) l'interfaccia della riga di comando di Service Fabric.
- Informazioni su come usare l'interfaccia della riga di comando Service Fabric usando gli [script di esempio](/azure/service-fabric/scripts/sfctl-upgrade-application).