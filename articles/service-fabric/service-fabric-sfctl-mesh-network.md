---
title: Interfaccia della riga di comando di Azure Service Fabric - sfctl mesh network | Microsoft Docs
description: Descrive i comandi sfctl mesh network dell'interfaccia della riga di comando di Service Fabric.
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
ms.openlocfilehash: e92fbc3856bba69db11aba6ad5d85e814bf30a81
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035050"
---
# <a name="sfctl-mesh-network"></a>sfctl mesh network
Consente di ottenere ed eliminare le risorse di rete mesh.

## <a name="commands"></a>Comandi:

|Comando|DESCRIZIONE|
| --- | --- |
| delete | Elimina la risorsa di rete. |
| list | Elimina tutte le risorse di rete. |
| show | Ottiene la risorsa di rete con il nome specificato. |

## <a name="sfctl-mesh-network-delete"></a>sfctl mesh network delete
Elimina la risorsa di rete.

Elimina la risorsa di rete identificata dal nome.

### <a name="arguments"></a>Argomenti

|Argomento|DESCRIZIONE|
| --- | --- |
| --name -n [Obbligatorio] | Nome della rete. |

### <a name="global-arguments"></a>Argomenti globali

|Argomento|Descrizione|
| --- | --- |
| --debug | Aumenta il livello di dettaglio di registrazione per mostrare tutti i log di debug. |
| --help -h | Mostra questo messaggio della Guida e l'uscita. |
| --output -o | Formato di output.  Valori consentiti\: json, jsonc, table, tsv.  Valore predefinito\: json. |
| --query | Stringa di query JMESPath. Per altre informazioni ed esempi, vedere http\://jmespath.org/. |
| --verbose | Aumenta il livello di dettaglio di registrazione. Usare --debug per i log di debug completi. |

## <a name="sfctl-mesh-network-list"></a>sfctl mesh network list
Elimina tutte le risorse di rete.

Ottiene le informazioni su tutte le risorse di rete in un determinato gruppo di risorse. Le informazioni includono la descrizione e altre proprietà della rete.

### <a name="global-arguments"></a>Argomenti globali

|Argomento|Descrizione|
| --- | --- |
| --debug | Aumenta il livello di dettaglio di registrazione per mostrare tutti i log di debug. |
| --help -h | Mostra questo messaggio della Guida e l'uscita. |
| --output -o | Formato di output.  Valori consentiti\: json, jsonc, table, tsv.  Valore predefinito\: json. |
| --query | Stringa di query JMESPath. Per altre informazioni ed esempi, vedere http\://jmespath.org/. |
| --verbose | Aumenta il livello di dettaglio di registrazione. Usare --debug per i log di debug completi. |

## <a name="sfctl-mesh-network-show"></a>sfctl mesh network show
Ottiene la risorsa di rete con il nome specificato.

Ottiene le informazioni sulla risorsa di rete con il nome specificato. Le informazioni includono la descrizione e altre proprietà della rete.

### <a name="arguments"></a>Argomenti

|Argomento|DESCRIZIONE|
| --- | --- |
| --name -n [Obbligatorio] | Nome della rete. |

### <a name="global-arguments"></a>Argomenti globali

|Argomento|Descrizione|
| --- | --- |
| --debug | Aumenta il livello di dettaglio di registrazione per mostrare tutti i log di debug. |
| --help -h | Mostra questo messaggio della Guida e l'uscita. |
| --output -o | Formato di output.  Valori consentiti\: json, jsonc, table, tsv.  Valore predefinito\: json. |
| --query | Stringa di query JMESPath. Per altre informazioni ed esempi, vedere http\://jmespath.org/. |
| --verbose | Aumenta il livello di dettaglio di registrazione. Usare --debug per i log di debug completi. |


## <a name="next-steps"></a>Passaggi successivi
- [Configurare](service-fabric-cli.md) l'interfaccia della riga di comando di Service Fabric.
- Informazioni su come usare l'interfaccia della riga di comando Service Fabric usando gli [script di esempio](/azure/service-fabric/scripts/sfctl-upgrade-application).