---
title: Esempio di script dell'interfaccia della riga di comando di Azure - Creare un'app Web che usa l'autenticazione del servizio SignalR e GitHub
description: Esempio di script dell'interfaccia della riga di comando di Azure - Creare un'app Web che usa l'autenticazione del servizio SignalR e GitHub
author: sffamily
ms.service: signalr
ms.devlang: azurecli
ms.topic: sample
ms.date: 04/22/2018
ms.author: zhshang
ms.custom: mvc
ms.openlocfilehash: 16fa44c5fa0b674fe27e2ec8e2dc8e640742ec63
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565786"
---
# <a name="create-a-web-app-that-uses-signalr-service-and-github-authentication"></a>Creare un'app Web che usa l'autenticazione del servizio SignalR e GitHub

Questo script di esempio crea una nuova risorsa servizio Azure SignalR che viene usata per eseguire il push in tempo reale degli aggiornamenti del contenuto verso i client. Questo script aggiunge anche una nuova app Web e un nuovo piano di servizio app per ospitare l'app Web ASP.NET Core che usa il servizio SignalR. L'app Web è configurata con le impostazioni app per connettersi alla nuova risorsa servizio SignalR ed eseguire l'autenticazione con l'[autenticazione di GitHub](https://developer.github.com/v3/guides/basics-of-authentication/). L'app Web viene anche configurata per l'uso di un'origine di distribuzione del repository Git locale.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se si sceglie di installare e usare l'interfaccia della riga di comando in locale, per questo articolo è necessario eseguire la versione 2.0 o successiva dell'interfaccia della riga di comando di Azure. Eseguire `az --version` per trovare la versione. Se è necessario eseguire l'installazione o l'aggiornamento, vedere [Installare l'interfaccia della riga di comando di Azure]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Script di esempio

Questo script usa l'estensione *signalr* per l'interfaccia della riga di comando di Azure. Eseguire il comando seguente per installare l'estensione *signalr* per l'interfaccia della riga di comando di Azure prima di usare questo script di esempio:

```azurecli-interactive
az extension add -n signalr
```

[!code-azurecli-interactive[main](../../../cli_scripts/azure-signalr/create-signalr-with-app-service-github-oauth/create-signalr-with-app-service-github-oauth.sh "Create a new SignalR Service and Web App configured to use SignalR, GitHub OAuth, and local Git repository deployment source.")]

Annotare il nome effettivo generato per il nuovo gruppo di risorse. Sarà visualizzato nell'output. Il nome del gruppo di risorse viene usato quando si vuole eliminare tutte le risorse di gruppo.

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Spiegazione dello script

Ogni comando della tabella include collegamenti alla documentazione specifica del comando. Questo script usa i comandi seguenti:

| Comando | Note |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Consente di creare un gruppo di risorse in cui sono archiviate tutte le risorse. |
| [az signalr create](/cli/azure/signalr#az-signalr-create) | Crea una risorsa servizio Azure SignalR. |
| [az signalr key list](/cli/azure/signalr/key#az-signalr-key-list) | Elencare le chiavi che saranno usate dall'applicazione per il push in tempo reale degli aggiornamenti del contenuto con SignalR. |
| [az appservice plan create](/cli/azure/appservice/plan#az-appservice-plan-create) | Crea un piano del servizio app di Azure per l'hosting di app Web. |
| [az webapp create](/cli/azure/webapp#az-webapp-create) | Crea un'app Web di Azure usando il piano di servizio app host. |
| [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#az-webapp-config-appsettings-set) | Aggiunge nuove impostazioni app per l'app Web. Queste impostazioni app vengono usate per archiviare la stringa di connessione SignalR e i segreti dell'app GitHub OAuth. |
| [az webapp deployment user set](/cli/azure/webapp/deployment/user#az-webapp-deployment-user-set) | Aggiornare le credenziali di distribuzione. |
| [az webapp deployment source config-local-git](/cli/azure/webapp/deployment/source#az-webapp-deployment-source-config-local-git) | Ottenere un URL per un endpoint del repository git in cui clonare ed eseguire il push della distribuzione dell'app Web. |

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sull'interfaccia della riga di comando di Azure, vedere la [documentazione sull'interfaccia della riga di comando di Azure](/cli/azure).

Altri esempi di script dell'interfaccia della riga di comando del servizio Azure SignalR sono disponibili nella [documentazione del servizio Azure SignalR](../signalr-reference-cli.md).
