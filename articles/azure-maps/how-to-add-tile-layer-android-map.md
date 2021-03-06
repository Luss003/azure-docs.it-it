---
title: Aggiungere un livello sezione a Maps Android | Mappe Microsoft Azure
description: In questo articolo si apprenderà come eseguire il rendering di un livello sezione su una mappa usando le mappe di Microsoft Azure Android SDK.
author: farah-alyasari
ms.author: v-faalya
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 8e1a77ae83783b2841a2600654a9775e9ceb6ada
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2020
ms.locfileid: "77209937"
---
# <a name="add-a-tile-layer-to-a-map-using-the-azure-maps-android-sdk"></a>Aggiungere un livello sezione a una mappa usando le mappe di Azure Android SDK

Questo articolo illustra come eseguire il rendering di un livello sezione su una mappa usando le mappe di Azure Android SDK. I livelli riquadro consentono di sovrapporre immagini sopra i riquadri mappa di base in Mappe di Azure. Altre informazioni sul sistema di riquadri di Mappe di Azure sono reperibili nella documentazione [Livelli di Zoom e griglia riquadri](zoom-levels-and-tile-grid.md).

Un livello sezione viene caricato in riquadri da un server. Queste immagini possono essere pre-renderizzate e archiviate come qualsiasi altra immagine in un server, usando una convenzione di denominazione che il livello sezione riconosce. In alternativa, è possibile eseguire il rendering di queste immagini con un servizio dinamico che genera le immagini quasi in tempo reale. Sono supportate tre diverse convenzioni di denominazione del servizio affiancate dalla classe TileLayer di Azure Maps:

* Notazione zoom di X, Y: in base al livello di zoom, x è la posizione nella colonna e y è la posizione nella riga del riquadro nella griglia dei riquadri.
* Notazione Quadkey: combinazione delle informazioni x, y e zoom in un singolo valore stringa che sia un identificatore univoco per un riquadro.
* Rettangolo di selezione: le coordinate del rettangolo di selezione possono essere utilizzate per specificare un'immagine nel formato `{west},{south},{east},{north}` comunemente utilizzata dai [ servizi Web di Mapping (WMS)](https://www.opengeospatial.org/standards/wms).

> [!TIP]
> Un TileLayer è un ottimo modo per visualizzare set di dati di grandi dimensioni sulla mappa. Non solo è possibile generare un livello riquadro da un'immagine, ma anche i dati vettoriali possono essere sottoposti a rendering come livello riquadro. Sottoponendo a rendering i dati vettoriali come livello riquadro, il controllo della mappa deve solo caricare i riquadri che possono essere molto più piccoli in termini di dimensione del file rispetto ai dati vettoriali che rappresentano. Molti utenti usano questa tecnica per eseguire il rendering di milioni di righe di dati sulla mappa.

L'URL di riquadro passato in un livello riquadro deve essere un URL http/https indirizzato a una risorsa TileJSON o un modello di URL di riquadro che usa i parametri seguenti: 

* `{x}` - posizione X del riquadro. Necessita inoltre di `{y}` e `{z}`.
* `{y}` - posizione Y del riquadro. Necessita inoltre di `{x}` e `{z}`.
* `{z}` - Livello di zoom del riquadro. Necessita inoltre di `{x}` e `{y}`.
* `{quadkey}` - Identificatore del riquadro quadkey basato sulla convenzione di denominazione del sistema di riquadri di Mappe di Bing.
* `{bbox-epsg-3857}` - Una stringa  del rettangolo delimitatore nel formato `{west},{south},{east},{north}` nel sistema di riferimento spaziale EPSG 3857.
* `{subdomain}`: segnaposto per i valori del sottodominio, se viene specificato il valore del sottodominio.

## <a name="prerequisites"></a>Prerequisiti

Per completare il processo in questo articolo, è necessario installare [Azure Maps Android SDK](https://docs.microsoft.com/azure/azure-maps/how-to-use-android-map-control-library) per caricare una mappa.


## <a name="add-a-tile-layer-to-the-map"></a>Aggiungere un livello sezione alla mappa

 In questo esempio viene illustrato come creare un livello sezione che punta a un set di riquadri. Questi riquadri usano il sistema di affiancamento "x, y, zoom". L'origine di questo livello riquadro è una sovrapposizione di radar meteo dall'[Iowa Environmental Mesonet dell'Iowa State University](https://mesonet.agron.iastate.edu/ogc/). 

È possibile aggiungere un livello sezione alla mappa seguendo questa procedura.

1. Modificare il **layout res > > activity_main. XML** in modo che abbia un aspetto simile al seguente:

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <FrameLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        >
    
        <com.microsoft.azure.maps.mapcontrol.MapControl
            android:id="@+id/mapcontrol"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:mapcontrol_centerLat="40.75"
            app:mapcontrol_centerLng="-99.47"
            app:mapcontrol_zoom="3"
            />
    
    </FrameLayout>
    ```

2. Copiare il seguente frammento di codice nel metodo **OnCreate ()** della classe `MainActivity.java`.

    ```Java
    mapControl.onReady(map -> {
        //Add a tile layer to the map, below the map labels.
        map.layers.add(new TileLayer(
            tileUrl("https://mesonet.agron.iastate.edu/cache/tile.py/1.0.0/nexrad-n0q-900913/{z}/{x}/{y}.png"),
            opacity(0.8f),
            tileSize(256)
        ), "labels");
    });
    ```
    
    Il frammento di codice precedente ottiene innanzitutto un'istanza del controllo mappa di Azure Maps usando il metodo di callback **onReady ()** . Viene quindi creato un oggetto `TileLayer` e viene passato un URL del riquadro **XYZ** formattato nell'opzione `tileUrl`. L'opacità del livello è impostata su `0.8` e poiché i riquadri del servizio affiancato sono riquadri di 256 pixel, queste informazioni vengono passate nell'opzione `tileSize`. Il livello sezione viene quindi passato a Maps Layer Manager.

    Dopo aver aggiunto il frammento di codice precedente, il `MainActivity.java` dovrebbe essere simile a quello riportato di seguito:
    
    ```Java
    package com.example.myapplication;

    import android.app.Activity;
    import android.os.Bundle;
    import android.support.v7.app.AppCompatActivity;
    import com.microsoft.azure.maps.mapcontrol.layer.TileLayer;
    import java.util.Arrays;
    import java.util.List;
    import com.microsoft.azure.maps.mapcontrol.AzureMaps;
    import com.microsoft.azure.maps.mapcontrol.MapControl;
    import static com.microsoft.azure.maps.mapcontrol.options.TileLayerOptions.tileSize;
    import static com.microsoft.azure.maps.mapcontrol.options.TileLayerOptions.tileUrl;
        
    public class MainActivity extends AppCompatActivity {
    
        static{
            AzureMaps.setSubscriptionKey("<Your Azure Maps subscription key>");
        }
    
        MapControl mapControl;
        @Override
        protected void onCreate(Bundle savedInstanceState) {
    
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            mapControl = findViewById(R.id.mapcontrol);
    
            mapControl.onCreate(savedInstanceState);
    
            mapControl.onReady(map -> {

                //Add a tile layer to the map, below the map labels.
                map.layers.add(new TileLayer(
                    tileUrl("https://mesonet.agron.iastate.edu/cache/tile.py/1.0.0/nexrad-n0q-900913/{z}/{x}/{y}.png"),
                    opacity(0.8f),
                    tileSize(256)
                ), "labels");
            });    
        }
    
        @Override
        public void onResume() {
            super.onResume();
            mapControl.onResume();
        }
    
        @Override
        public void onPause() {
            super.onPause();
            mapControl.onPause();
        }
    
        @Override
        public void onStop() {
            super.onStop();
            mapControl.onStop();
        }
    
        @Override
        public void onLowMemory() {
            super.onLowMemory();
            mapControl.onLowMemory();
        }
    
        @Override
        protected void onDestroy() {
            super.onDestroy();
            mapControl.onDestroy();
        }
    
        @Override
        protected void onSaveInstanceState(Bundle outState) {
            super.onSaveInstanceState(outState);
            mapControl.onSaveInstanceState(outState);
        }    
    }
    ```

Se si esegue ora l'applicazione, viene visualizzata una riga sulla mappa come illustrato di seguito:

<center>

![linea della mappa Android](./media/how-to-add-tile-layer-android-map/xyz-tile-layer-android.png)</center>

## <a name="next-steps"></a>Passaggi successivi

Vedere l'articolo seguente per altre informazioni sui modi per impostare gli stili della mappa

> [!div class="nextstepaction"]
> [Modificare gli stili della mappa nelle mappe Android](https://docs.microsoft.com/azure/azure-maps/set-android-map-styles)