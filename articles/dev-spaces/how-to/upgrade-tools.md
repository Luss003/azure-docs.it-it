---
title: Aggiornare gli strumenti di Azure Dev Spaces
services: azure-dev-spaces
ms.date: 07/03/2018
ms.topic: conceptual
description: Informazioni su come eseguire l'aggiornamento degli strumenti da riga di comando Azure Dev Spaces, l'estensione di codice Visual Studs e l'estensione di Visual Studio
keywords: Docker, Kubernetes, Azure, servizio Azure Kubernetes, servizio Azure Container, contenitori
ms.openlocfilehash: 07d55689ac94a865527f4b595765d67b28ddb97a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75438412"
---
# <a name="how-to-upgrade-azure-dev-spaces-tools"></a>Aggiornare gli strumenti di Azure Dev Spaces

Se è presente una nuova versione e si sta già usando Azure Dev Spaces, potrebbe essere necessario aggiornare gli strumenti client di Azure Dev Spaces.

## <a name="update-the-azure-cli"></a>Aggiornare l'interfaccia della riga di comando di Azure

Quando si aggiorna l'interfaccia della riga di comando di Azure più recente, è anche possibile ottenere la versione più recente dell'estensione dell'interfaccia della riga di comando per Dev Spaces.

Non è necessario disinstallare la versione precedente: basta individuare solo il download all'indirizzo dell'[interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli?view=azure-cli-latest).


## <a name="update-the-dev-spaces-cli-extension-and-command-line-tools"></a>Aggiornare l'estensione dell'interfaccia della riga di comando di Dev Spaces e gli strumenti da riga di comando

Eseguire il comando seguente:

```cmd
az aks use-dev-spaces -n <your-aks-cluster> -g <your-aks-cluster-resource-group> --update
```

## <a name="update-the-vs-code-extension"></a>Aggiornare l'estensione di Visual Studio Code

Una volta installata, l'estensione viene aggiornata automaticamente. Per usare delle nuove funzionalità potrebbe essere necessario ricaricare l'estensione. In Visual Studio Code, aprire il riquadro **Estensioni**, scegliere le estensioni **Azure Dev Spaces**, quindi scegliere **Ricarica**.

## <a name="update-the-visual-studio-extension"></a>Aggiornare l'estensione di Visual Studio

Come con altre estensioni e aggiornamenti, Visual Studio informerà quando un aggiornamento è disponibile per Visual Studio Tools per Kubernetes, che include Azure Dev Spaces. Cercare un'icona di contrassegno nella parte superiore destra della schermata.

Per aggiornare gli strumenti di Visual Studio, scegliere la voce di menu **Strumenti > Estensioni e aggiornamenti** e, a sinistra, scegliere **Aggiornamenti**. Individuare **Visual Studio Tools per Kubernetes** e scegliere il pulsante **Aggiorna**.

## <a name="next-steps"></a>Passaggi successivi

Testare i nuovi strumenti mediante la creazione di un nuovo cluster. Provare le guide introduttive e le esercitazioni in [Azure Dev Spaces](/azure/dev-spaces).