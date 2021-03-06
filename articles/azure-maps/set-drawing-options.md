---
title: Modulo strumenti di disegno | Mappe Microsoft Azure
description: In questo articolo si apprenderà come impostare i dati delle opzioni di disegno usando il Microsoft Azure Maps Web SDK
author: farah-alyasari
ms.author: v-faalya
ms.date: 01/29/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: f3634149b744b9a03f0ed89aafbc20932701bdbc
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2020
ms.locfileid: "77208183"
---
# <a name="use-the-drawing-tools-module"></a>Usare il modulo Strumenti di disegno

Azure Maps Web SDK fornisce un *modulo di strumenti di disegno*. Questo modulo consente di creare e modificare facilmente forme sulla mappa usando un dispositivo di input, ad esempio un mouse o un touchscreen. La classe principale di questo modulo è [gestione del disegno](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest#setoptions-drawingmanageroptions-). Il gestore di disegno offre tutte le funzionalità necessarie per disegnare e modificare forme sulla mappa. Può essere usato direttamente ed è integrato con un'interfaccia utente personalizzata della barra degli strumenti. È anche possibile usare la classe della [barra degli strumenti di disegno](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest) incorporata. 

## <a name="loading-the-drawing-tools-module-in-a-webpage"></a>Caricamento del modulo strumenti di disegno in una pagina Web

1. Creare un nuovo file HTML e [implementare la mappa come di consueto](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control).
2. Caricare il modulo Azure Maps Drawing Tools. È possibile caricarlo in uno dei due modi seguenti:
    - Usare la versione di rete per la distribuzione di contenuti di Azure ospitata a livello globale del modulo servizi di Azure maps. Aggiungere un riferimento al foglio di stile CSS e JavaScript nell'elemento `<head>` del file:

        ```html
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/drawing/0.1/atlas-drawing.min.css" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/javascript/drawing/0.1/atlas-drawing.min.js"></script>
        ```

    - In alternativa, è possibile caricare il modulo strumenti di disegno per il codice sorgente di Azure Maps Web SDK localmente usando il pacchetto NPM [Azure-Maps-Drawing-Tools](https://www.npmjs.com/package/azure-maps-drawing-tools) e quindi ospitarlo con l'app. Questo pacchetto include anche le definizioni TypeScript. Usare questo comando:
    
        > **NPM installare Azure-Maps-Drawing-Tools**
    
        Aggiungere quindi un riferimento al foglio di stile JavaScript e CSS nell'elemento `<head>` del file:

         ```html
        <link rel="stylesheet" href="node_modules/azure-maps-drawing-tools/dist/atlas-drawing.min.css" type="text/css" />
        <script src="node_modules/azure-maps-drawing-tools/dist/atlas-drawing.min.js"></script>
         ```

## <a name="use-the-drawing-manager-directly"></a>Usare direttamente gestione disegno

Una volta caricato il modulo strumenti di disegno nell'applicazione, è possibile abilitare le funzionalità di disegno e modifica usando [gestione disegno](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest#setoptions-drawingmanageroptions-). È possibile specificare le opzioni per il gestore di disegno durante la creazione di istanze o usare la funzione `drawingManager.setOptions()`.

### <a name="set-the-drawing-mode"></a>Impostare la modalità di disegno

Il codice seguente crea un'istanza del gestore di disegno e imposta l'opzione della **modalità** di disegno. 

```Javascript
//Create an instance of the drawing manager and set drawing mode.
drawingManager = new atlas.drawing.DrawingManager(map,{
    mode: "draw-polygon"
});
```

Il codice seguente è un esempio completo in cui viene illustrato come impostare una modalità di disegno di gestione del disegno. Fare clic sulla mappa per iniziare a disegnare un poligono.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Creare un poligono" src="//codepen.io/azuremaps/embed/YzKVKRa/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Vedere le informazioni su come <a href='https://codepen.io/azuremaps/pen/YzKVKRa/'>creare un poligono</a> per mappe di Azure (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) in <a href='https://codepen.io'>CodePen</a>.
</iframe>


### <a name="set-the-interaction-type"></a>Impostare il tipo di interazione

Il gestore del disegno supporta tre diverse modalità di interazione con la mappa per disegnare forme.

* `click` le coordinate vengono aggiunte quando si fa clic con il mouse o il tocco.
* `freehand ` le coordinate vengono aggiunte quando il mouse o il tocco viene trascinato sulla mappa. 
* `hybrid` le coordinate vengono aggiunte quando si fa clic o si trascina il mouse o il tocco.

Il codice seguente abilita la modalità di disegno Polygon e imposta il tipo di interazione del disegno che il gestore di disegno deve rispettare `freehand`. 

```Javascript
//Create an instance of the drawing manager and set drawing mode.
drawingManager = new atlas.drawing.DrawingManager(map,{
    mode: "draw-polygon",
    interactionType: "freehand"
});
```

 Questo esempio di codice implementa la funzionalità di disegno di un poligono sulla mappa. È sufficiente tener premuto il pulsante sinistro del mouse e trascinarlo liberamente.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Disegno a mano libera" src="//codepen.io/azuremaps/embed/ZEzKoaj/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Vedere il <a href='https://codepen.io/azuremaps/pen/ZEzKoaj/'>disegno a mano libera</a> di Pen di Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) in <a href='https://codepen.io'>CodePen</a>.
</iframe>


### <a name="customizing-drawing-options"></a>Personalizzazione delle opzioni di disegno

Negli esempi precedenti è stato illustrato come personalizzare le opzioni di disegno durante la creazione di un'istanza di gestione disegno. È inoltre possibile impostare le opzioni di gestione disegno utilizzando la funzione `drawingManager.setOptions()`. Di seguito è riportato uno strumento per testare la personalizzazione di tutte le opzioni per la gestione del disegno usando la funzione seoptions.

<br/>

<iframe height="685" title="Personalizzare gestione disegno" src="//codepen.io/azuremaps/embed/LYPyrxR/?height=600&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true" style='width: 100%;'>Vedere la pagina relativa ai <a href='https://codepen.io/azuremaps/pen/LYPyrxR/'>dati delle forme</a> per la penna Get di Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) in <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="next-steps"></a>Passaggi successivi

Informazioni su come usare le funzionalità aggiuntive del modulo strumenti di disegno:

> [!div class="nextstepaction"]
> [Aggiungere una barra degli strumenti di disegno](map-add-drawing-toolbar.md)

> [!div class="nextstepaction"]
> [Ottenere i dati di forma](map-get-shape-data.md)

> [!div class="nextstepaction"]
> [Reagire agli eventi di disegno](drawing-tools-events.md)

> [!div class="nextstepaction"]
> [Tipi di interazione e tasti di scelta rapida](drawing-tools-interactions-keyboard-shortcuts.md)

Per altre informazioni sulle classi e sui metodi usati in questo articolo, vedere:

> [!div class="nextstepaction"]
> [Mappa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Gestione disegno](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest)

> [!div class="nextstepaction"]
> [Barra degli strumenti disegno](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest)
