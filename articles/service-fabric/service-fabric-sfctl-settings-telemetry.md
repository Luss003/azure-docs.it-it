---
title: Interfaccia della riga di comando Azure Service Fabric - telemetria delle impostazioni sfctl | Microsoft Docs
description: Descrive i comandi della telemetria delle impostazioni sfctl dell'interfaccia della riga di comando Service Fabric.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: cf5ebbeb4d9b4757e0c55eeb1a9268065efb2c7c
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035192"
---
# <a name="sfctl-settings-telemetry"></a>sfctl settings telemetry
Configura le impostazioni di telemetria locali per questa istanza di sfctl.

La telemetria sfctl raccoglie il nome del comando senza i parametri forniti o i relativi valori, la versione di sfctl, il tipo di sistema operativo, la versione di python, l'esito positivo o negativo del comando, il messaggio di errore restituito.

## <a name="commands"></a>Comandi:

|Comando|DESCRIZIONE|
| --- | --- |
| set-telemetry | Attiva o disattiva la telemetria. |

## <a name="sfctl-settings-telemetry-set-telemetry"></a>sfctl settings telemetry set-telemetry
Attiva o disattiva la telemetria.

### <a name="arguments"></a>Argomenti

|Argomento|DESCRIZIONE|
| --- | --- |
| --off | Disattiva la telemetria. |
| --on | Attiva la telemetria. Si tratta del valore predefinito. |

### <a name="global-arguments"></a>Argomenti globali

|Argomento|Descrizione|
| --- | --- |
| --debug | Aumenta il livello di dettaglio di registrazione per mostrare tutti i log di debug. |
| --help -h | Mostra questo messaggio della Guida e l'uscita. |
| --output -o | Formato di output.  Valori consentiti\: json, jsonc, table, tsv.  Valore predefinito\: json. |
| --query | Stringa di query JMESPath. Per altre informazioni ed esempi, vedere http\://jmespath.org/. |
| --verbose | Aumenta il livello di dettaglio di registrazione. Usare --debug per i log di debug completi. |

### <a name="examples"></a>Esempi

Disattiva la telemetria.

```
sfctl settings telemetry set_telemetry --off
```

Attiva la telemetria.

```
sfctl settings telemetry set_telemetry --on
```


## <a name="next-steps"></a>Passaggi successivi
- [Configurare](service-fabric-cli.md) l'interfaccia della riga di comando di Service Fabric.
- Informazioni su come usare l'interfaccia della riga di comando Service Fabric usando gli [script di esempio](/azure/service-fabric/scripts/sfctl-upgrade-application).