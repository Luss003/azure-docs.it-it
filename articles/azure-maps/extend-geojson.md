---
title: Geometrie estese GeoJSON | Mappe Microsoft Azure
description: In questo articolo si apprenderà come Microsoft Azure Maps estende le specifiche GeoJSON per rappresentare determinate geometrie.
author: sataneja
ms.author: sataneja
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 98db10f0fc7a417f39d4bb00e77af6bdea034a03
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2020
ms.locfileid: "79276400"
---
# <a name="extended-geojson-geometries"></a>Geometrie estese GeoJSON

Azure Maps fornisce un elenco di potenti API per la ricerca all'interno e nelle funzionalità geografiche. Queste API rispettano la [specifica GeoJSON][1] standard di che rappresenta le funzionalità geografiche.  

La [specifica GeoJSON][1] supporta solo le geometrie seguenti:

* GeometryCollection
* LineString
* MultiLineString
* MultiPoint
* MultiPolygon
* Point
* Polygon

Alcune API di Azure Maps accettano geometrie che non fanno parte della [specifica GeoJSON][1]. Ad esempio, la [ricerca all'interno](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry) dell'API Geometry accetta il cerchio e i poligoni.

Questo articolo fornisce una spiegazione dettagliata su come Azure Maps estende le [specifiche GeoJSON][1] per rappresentare determinate geometrie.

## <a name="circle"></a>Circle

La geometria `Circle` non è supportata dalla [specifica GeoJSON][1]. Viene usato un oggetto `GeoJSON Point Feature` per rappresentare un cerchio.

Una `Circle` Geometry rappresentata utilizzando l'oggetto `GeoJSON Feature` __deve__ contenere le coordinate e le proprietà seguenti:

- Center

    Il centro del cerchio viene rappresentato usando un oggetto `GeoJSON Point`.

- Radius

    L'elemento `radius` del cerchio viene rappresentato usando le proprietà di `GeoJSON Feature`. Il valore del raggio è espresso in _metri_ e deve essere di tipo `double`.

- Sottotipo

    La geometria circle deve contenere anche la proprietà `subType`. Questa proprietà deve essere una parte delle proprietà del `GeoJSON Feature`e il relativo valore deve essere _Circle_

#### <a name="example"></a>Esempio

Di seguito viene illustrato come rappresentare un cerchio usando un oggetto `GeoJSON Feature`. Si concentrerà il cerchio in Latitudine: 47,639754 e Longitudine:-122,126986 e si assegnerà un raggio uguale a 100 metri:

```json            
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.126986, 47.639754]
    },
    "properties": {
        "subType": "Circle",
        "radius": 100
    }
}          
```

## <a name="rectangle"></a>Rectangle

La geometria `Rectangle` non è supportata dalla [specifica GeoJSON][1]. Viene usato un oggetto `GeoJSON Polygon Feature` per rappresentare un rettangolo. L'estensione Rectangle viene utilizzata principalmente dal modulo degli strumenti di disegno di Web SDK.

Una `Rectangle` Geometry rappresentata utilizzando l'oggetto `GeoJSON Polygon Feature` __deve__ contenere le coordinate e le proprietà seguenti:

- Angoli

    Gli angoli del rettangolo sono rappresentati usando le coordinate di un oggetto `GeoJSON Polygon`. Devono essere presenti cinque coordinate, una per ogni angolo. E, una quinta coordinata uguale alla prima coordinata, per chiudere l'anello del poligono. Si presuppone che queste coordinate siano allineate e che lo sviluppatore possa ruotarle come desiderato.

- Sottotipo

    La geometria del rettangolo deve contenere anche la proprietà `subType`. Questa proprietà deve essere una parte delle proprietà del `GeoJSON Feature`e il relativo valore deve essere _Rectangle_

### <a name="example"></a>Esempio

```json
{
    "type": "Feature",
    "geometry": {
        "type": "Polygon",
        "coordinates": [[[5,25],[14,25],[14,29],[5,29],[5,25]]]
    },
    "properties": {
        "subType": "Rectangle"
    }
}

```
## <a name="next-steps"></a>Passaggi successivi

Altre informazioni sui dati GeoJSON in mappe di Azure:

> [!div class="nextstepaction"]
> [Formato GeoJSON Geofence](geofence-geojson.md)

Esaminare il Glossario dei termini tecnici comuni associati a mappe di Azure e alle applicazioni di Business Intelligence per la posizione:

> [!div class="nextstepaction"]
> [Glossario mappe di Azure](glossary.md)

[1]: https://tools.ietf.org/html/rfc7946
