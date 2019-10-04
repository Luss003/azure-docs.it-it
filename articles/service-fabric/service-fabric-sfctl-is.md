---
title: Interfaccia della riga di comando Azure Service Fabric - sfctl is | Documentazione Microsoft
description: Descrive i comandi dell'interfaccia della riga di comando Service Fabric sfctl is.
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
ms.openlocfilehash: 998a94350250402d4face64f64e12f32cf5b2a36
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/15/2019
ms.locfileid: "69036499"
---
# <a name="sfctl-is"></a>sfctl is
Consente di eseguire query e inviare comandi al servizio di infrastruttura.

## <a name="commands"></a>Comandi:

|Comando|DESCRIZIONE|
| --- | --- |
| command | Richiama un comando amministrativo sull'istanza del servizio di infrastruttura specificato. |
| query | Richiama una query di sola lettura sull'istanza del servizio di infrastruttura specificato. |

## <a name="sfctl-is-command"></a>sfctl is command
Richiama un comando amministrativo sull'istanza del servizio di infrastruttura specificato.

Per i cluster che dispongono di una o più istanze del servizio di infrastruttura configurato, questo API fornisce un modo per inviare comandi specifici dell'infrastruttura a una particolare istanza del servizio di infrastruttura. I comandi disponibili e i relativi formati di risposta corrispondente variano a seconda dell'infrastruttura in esecuzione sul cluster. Questa API supporta la piattaforma Service Fabric. non deve essere utilizzata direttamente dal codice.

### <a name="arguments"></a>Argomenti

|Argomento|DESCRIZIONE|
| --- | --- |
| --command [Obbligatorio] | Il testo del comando da richiamare. Il contenuto del comando è specifico dell'infrastruttura. |
| --service-id | L'identità del servizio di infrastruttura. <br><br> Si tratta del nome completo del servizio di infrastruttura senza lo schema URI "fabric\:". Questo parametro è obbligatorio solo per il cluster con più di un'istanza del servizio di infrastruttura in esecuzione. |
| --timeout -t | Timeout del server in secondi.  Impostazione predefinita\: 60. |

### <a name="global-arguments"></a>Argomenti globali

|Argomento|Descrizione|
| --- | --- |
| --debug | Aumenta il livello di dettaglio di registrazione per mostrare tutti i log di debug. |
| --help -h | Mostra questo messaggio della Guida e l'uscita. |
| --output -o | Formato di output.  Valori consentiti\: json, jsonc, table, tsv.  Valore predefinito\: json. |
| --query | Stringa di query JMESPath. Per altre informazioni ed esempi, vedere http\://jmespath.org/. |
| --verbose | Aumenta il livello di dettaglio di registrazione. Usare --debug per i log di debug completi. |

## <a name="sfctl-is-query"></a>sfctl is query
Richiama una query di sola lettura sull'istanza del servizio di infrastruttura specificato.

Per i cluster che dispongono di una o più istanze del servizio di infrastruttura configurato, questo API fornisce un modo per inviare query specifiche dell'infrastruttura a una particolare istanza del servizio di infrastruttura. I comandi disponibili e i relativi formati di risposta corrispondente variano a seconda dell'infrastruttura in esecuzione sul cluster. Questa API supporta la piattaforma Service Fabric. non deve essere utilizzata direttamente dal codice.

### <a name="arguments"></a>Argomenti

|Argomento|DESCRIZIONE|
| --- | --- |
| --command [Obbligatorio] | Il testo del comando da richiamare. Il contenuto del comando è specifico dell'infrastruttura. |
| --service-id | L'identità del servizio di infrastruttura. <br><br> Si tratta del nome completo del servizio di infrastruttura senza lo schema URI "fabric\:". Questo parametro è obbligatorio solo per il cluster con più di un'istanza del servizio di infrastruttura in esecuzione. |
| --timeout -t | Timeout del server in secondi.  Impostazione predefinita\: 60. |

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