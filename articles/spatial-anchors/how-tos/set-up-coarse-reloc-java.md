---
title: Rilocalizzazione grossolana in Java
description: Descrizione approfondita di come creare e individuare ancoraggi usando la rilocalizzazione grossolana in Java.
author: bucurb
manager: dacoghl
services: azure-spatial-anchors
ms.author: bobuc
ms.date: 09/19/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 7a13b3e838bd3083741eb903f39f3f7683f39ee4
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/23/2020
ms.locfileid: "76545213"
---
# <a name="how-to-create-and-locate-anchors-using-coarse-relocalization-in-java"></a>Come creare e individuare ancoraggi usando la rilocalizzazione grossolana in Java

> [!div  class="op_single_selector"]
> * [Unity](set-up-coarse-reloc-unity.md)
> * [Objective-C](set-up-coarse-reloc-objc.md)
> * [Swift](set-up-coarse-reloc-swift.md)
> * [Android Java](set-up-coarse-reloc-java.md)
> * [C++/NDK](set-up-coarse-reloc-cpp-ndk.md)
> * [C++/WinRT](set-up-coarse-reloc-cpp-winrt.md)

Gli ancoraggi nello spazio di Azure possono associare i dati dei sensori di posizionamento su dispositivo agli ancoraggi creati. Questi dati possono anche essere usati per determinare rapidamente se sono presenti ancoraggi vicini al dispositivo. Per altre informazioni, vedere [Rilocalizzazione grossolana](../concepts/coarse-reloc.md).

## <a name="prerequisites"></a>Prerequisites

Per completare questa guida, accertarsi di avere:

- Conoscenze di base di Java.
- Letto la [Panoramica di ancoraggi nello spazio di Azure](../overview.md).
- Completato uno degli argomenti di [avvio rapido di 5 minuti](../index.yml).
- Lette le [procedure per creare e individuare ancoraggi](../create-locate-anchors-overview.md).

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-configure-provider.md)]

```java
// Create the sensor fingerprint provider
PlatformLocationProvider sensorProvider = new PlatformLocationProvider();

// Allow GPS
SensorCapabilities sensors = sensorProvider.getSensors();
sensors.setGeoLocationEnabled(true);

// Allow WiFi scanning
sensors.setWifiEnabled(true);

// Populate the set of known BLE beacons' UUIDs
String uuids[] = new String[2];
uuids[0] = "22e38f1a-c1b3-452b-b5ce-fdb0f39535c1";
uuids[1] = "a63819b9-8b7b-436d-88ec-ea5d8db2acb0";

// Allow the set of known BLE beacons
sensors.setBluetoothEnabled(true);
sensors.setKnownBeaconProximityUuids(uuids);
```

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-configure-session.md)]

```java
// Set the session's sensor fingerprint provider
cloudSpatialAnchorSession.setLocationProvider(sensorProvider);

// Configure the near-device criteria
NearDeviceCriteria nearDeviceCriteria = new NearDeviceCriteria();
nearDeviceCriteria.setDistanceInMeters(5.0f);
nearDeviceCriteria.setMaxResultCount(25);

// Set the session's locate criteria
AnchorLocateCriteria anchorLocateCriteria = new AnchorLocateCriteria();
anchorLocateCriteria.setNearDevice(nearDeviceCriteria);
cloudSpatialAnchorSession.createWatcher(anchorLocateCriteria);
```

[!INCLUDE [Locate](../../../includes/spatial-anchors-create-locate-anchors-locating-events.md)]

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-next-steps.md)]