---
title: File di inclusione
description: File di inclusione
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 02/20/2019
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 2498711a5b7e5bce29cd0054ba40257f8f996d43
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/25/2019
ms.locfileid: "71266824"
---
### <a name="enable-logging-with-diagnostics-settings"></a>Abilitare la registrazione con le impostazioni di diagnostica

[!INCLUDE [updated-for-az](./updated-for-az.md)]

1. Accedere al [portale di Azure](https://portal.azure.com) e passare all'hub IoT.

2. Selezionare **Impostazioni di diagnostica**.

3. Selezionare **Attiva diagnostica**.

   ![Abilita diagnostica](./media/iot-hub-diagnostics-settings/turnondiagnostics.png)

4. Assegnare un nome alle impostazioni di diagnostica.

5. Scegliere dove si vogliono inviare i log. È possibile selezionare qualsiasi combinazione delle tre opzioni:

   * Archivia in un account di archiviazione
   * Streaming in un hub eventi
   * Invia a Log Analytics

6. Scegliere le operazioni che si vuole monitorare e abilitare i log per tali operazioni. Le operazioni per cui le impostazioni di diagnostica possono inviare report sono:

   * Connessioni
   * Telemetria dei dispositivi
   * Messaggi da cloud a dispositivo
   * Operazioni relative alle identità dei dispositivi
   * Caricamenti di file
   * Routing messaggi
   * Operazioni da cloud a dispositivi gemelli
   * Operazioni da dispositivo gemello a cloud
   * Operazioni di dispositivo gemello
   * Operazioni di processo
   * Metodi diretti  
   * Traccia distribuita (anteprima)
   * Configurazioni
   * Flussi del dispositivo
   * Metriche del dispositivo

6. Salvare le nuove impostazioni. 

Per attivare le impostazioni di diagnostica con PowerShell, usare il codice seguente:

```azurepowershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName <subscription that includes your IoT Hub>
Set-AzDiagnosticSetting -ResourceId <your resource Id> -ServiceBusRuleId <your service bus rule Id> -Enabled $true
```

Le nuove impostazioni diventano effettive in circa 10 minuti. Trascorso questo tempo, i log vengono visualizzati nella destinazione di archiviazione configurata, nel pannello **Impostazioni di diagnostica**. Per altre informazioni sulla configurazione della diagnostica, vedere [Raccogliere e usare i dati dei log dalle risorse di Azure](../articles/azure-monitor/platform/resource-logs-overview.md).
