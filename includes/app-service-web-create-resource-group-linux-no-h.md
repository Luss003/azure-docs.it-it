---
title: File di inclusione
description: File di inclusione
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: c70e2c166bee14ac58ee88bd18baf0cc61702767
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/18/2019
ms.locfileid: "67180621"
---
[!INCLUDE [resource group intro text](resource-group.md)]

In Cloud Shell creare un gruppo di risorse con il comando [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create). L'esempio seguente crea un gruppo di risorse denominato *myResourceGroup* nella località *West Europe*. Per visualizzare tutte le località supportate per il servizio app in Linux nel livello **Basic**, eseguire il comando [`az appservice list-locations --sku B1 --linux-workers-enabled`](/cli/azure/appservice?view=azure-cli-latest#az-appservice-list-locations).

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

In genere, il gruppo di risorse e le risorse vengono create in un'area vicina alla località dell'utente. 

Al termine del comando, un output JSON visualizza le proprietà del gruppo di risorse.