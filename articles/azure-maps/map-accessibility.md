---
title: Creazione di un'applicazione accessibile con Mappe di Azure | Microsoft Docs
description: Come compilare un'applicazione accessibile con Mappe di Azure
services: azure-maps
author: chgennar
ms.author: chgennar
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
manager: timlt
ms.openlocfilehash: 8865027c895be09150797608e43184f1fdefb267
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638778"
---
# <a name="building-an-accessible-application"></a>Compilazione di un'applicazione accessibile

Questo articolo illustra come compilare un'applicazione per le mappe che può essere usata da un'utilità per la lettura dello schermo.

## <a name="understand-the-code"></a>Informazioni sul codice

<iframe height='500' scrolling='no' title='Creare un applicazione accessibile' src='//codepen.io/azuremaps/embed/ZoVyZQ/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Vedere l'elemento Pen <a href='https://codepen.io/azuremaps/pen/ZoVyZQ/'>Creare un applicazione accessibile</a> di Mappe di Azure (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) in <a href='https://codepen.io'>CodePen</a>.
</iframe>

La mappa è precompilata con funzionalità di accessibilità. Gli utenti possono spostarsi all'interno della mappa usando la tastiera. Se è in esecuzione un'utilità per la lettura dello schermo, la mappa notificherà all'utente le modifiche dello stato.
Ad esempio, agli utenti vengono segnalate le modifiche apportate alla mappa quando si visualizza una panoramica o si fa zoom avanti nella mappa. A eventuali informazioni aggiuntive inserite nella mappa di base devono corrispondere informazioni testuali per gli utenti dell'utilità di lettura dello schermo.

A questo scopo, è possibile usare [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest). Nell'esempio di ricerca precedente viene aggiunto alla mappa un popup con informazioni testuali per ogni segnaposto inserito nella mappa. Il metodo [attach](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) di [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#attach) consente al popup di essere individuato da un'utilità di lettura dello schermo senza visualizzare il popup nella mappa.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulla classe Popup e sui metodi usati in questo articolo, vedere:

> [!div class="nextstepaction"]
> [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

Vedere altri esempi di codice:

> [!div class="nextstepaction"]
> [Pagina dell'esempio di codice](https://aka.ms/AzureMapsSamples)
