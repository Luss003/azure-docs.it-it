---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 9d9790c9b3dbe3b130be999dd76092ae64f7b52c
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/18/2019
ms.locfileid: "67179853"
---
|Nome parametro| Type | Descrizione| Valori possibili|
|-|-|-|-|
| /Modalità server|Mandatory|Specifica se devono essere installati i server di configurazione e di elaborazione o solo il server di elaborazione|CS<br>PS|
|/InstallLocation|Mandatory|Cartella in cui sono installati i componenti| Qualsiasi cartella del computer|
|/MySQLCredsFilePath|Mandatory|Percorso del file in cui sono archiviate le credenziali del server MySQL|Il file deve essere nel formato specificato di seguito|
|/VaultCredsFilePath|Mandatory|Percorso del file di credenziali dell'insieme di credenziali|Percorso del file valido|
|/EnvType|Mandatory|Tipo di ambiente che si vuole proteggere |VMware<br>NonVMware|
|/PSIP|Mandatory|Indirizzo IP della scheda di interfaccia di rete da utilizzare per il trasferimento di dati di replica| Qualsiasi indirizzo IP valido|
|/CSIP|Mandatory|Indirizzo IP della scheda di interfaccia di rete su cui il server di configurazione è in ascolto| Qualsiasi indirizzo IP valido|
|/PassphraseFilePath|Mandatory|Percorso completo del file della passphrase|Percorso del file valido|
|/BypassProxy|Facoltativo|Specifica che il server di configurazione si connette ad Azure senza un proxy|Per ottenere questo valore da Venu|
|/ProxySettingsFilePath|Facoltativo|Impostazioni proxy, il proxy predefinito richiede l'autenticazione o un proxy personalizzato|Il file deve essere nel formato specificato di seguito|
|DataTransferSecurePort|Facoltativo|Numero di porta su PSIP da usare per i dati di replica| Numero di porta valido (il valore predefinito è 9433)|
|/SkipSpaceCheck|Facoltativo|Ignora la verifica dello spazio per il disco della cache| |
|/AcceptThirdpartyEULA|Mandatory|Il flag implica l'accettazione dell'EULA di terze parti| |
|/ShowThirdpartyEULA|Facoltativo|Visualizza le condizioni di licenza di terze parti. Se specificato come input, tutti gli altri parametri vengono ignorati| |
