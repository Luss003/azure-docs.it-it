---
title: Installare un pacchetto in un notebook di Jupyter in Azure
description: Come installare un pacchetto Python, R o F# da un notebook di Jupyter in esecuzione in Azure.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 6f089c12-128b-4dbd-96e3-1320d37eeba4
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: b0881cb6dac9ec83d2126942c758508e760f9c83
ms.sourcegitcommit: 32242bf7144c98a7d357712e75b1aefcf93a40cc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/04/2019
ms.locfileid: "70274431"
---
# <a name="install-packages-from-within-a-notebook"></a>Installare pacchetti da un notebook

Sebbene sia possibile configurare l'[ambiente per il notebook a livello di progetto](configure-manage-azure-notebooks-projects.md#configure-the-project-environment), è possibile installare i pacchetti direttamente all'interno di un notebook singolo.

I pacchetti installati dal notebook si applicano solo alla sessione del server corrente. Le installazioni di un pacchetto non sono persistenti dopo che il server è stato arrestato.

## <a name="python"></a>Python

I pacchetti in Python possono essere installati tramite pip o conda usando i comandi all'interno delle celle di codice:

```bash
!pip install <package_name>

!conda install <package_name> -y
```

Se l'output del comando indica che il requisito è già soddisfatto, Azure Notebooks può includere il pacchetto per impostazione predefinita. È possibile installare il pacchetto anche tramite un [passaggio di installazione dell'ambiente del progetto](configure-manage-azure-notebooks-projects.md#configure-the-project-environment).

## <a name="r"></a>R

I pacchetti in R possono essere installati da CRAN o GitHub tramite la funzione `install.packages` in una cella di codice:

```r
install.packages("package_name")
```

È anche possibile installare altri pacchetti di sviluppo e versioni non definitive da GitHub tramite la raccolta devtools:

```r
options(unzip = 'internal')
library(devtools)
install_github('<user>/<repo>')
```

## <a name="f"></a>F#

I pacchetti in F# possono essere installati da [nuget.org](https://www.nuget.org) chiamando il manager delle dipendenze Paket dall'interno delle celle di codice. In primo luogo, caricare il manager Paket:

```fsharp
#load "Paket.fsx"
```

Installare quindi i pacchetti:

```fsharp
Paket.Package
  [ "MathNet.Numerics"
    "MathNet.Numerics.FSharp"
  ]
```

Caricare quindi il generatore Paket:
```fsharp
#load "Paket.Generated.Refs.fsx"
```

Aprire libray:
```fsharp
open MathNet.Numerics
```

## <a name="next-steps"></a>Passaggi successivi

- [Procedura: Configurare e gestire i progetti](configure-manage-azure-notebooks-projects.md)
- [Procedura: Eseguire una presentazione](present-jupyter-notebooks-slideshow.md)
