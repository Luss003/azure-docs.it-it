---
title: Concetti relativi al dispositivo per eseguire il reprovisioning per il servizio Device Provisioning in hub IoT di Azure| Microsoft Docs
description: Descrive i concetti relativi al dispositivo di reprovisioning per il servizio di Device Provisioning in hub IoT di Azure
author: wesmc7777
ms.author: wesmc
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: philmea
ms.openlocfilehash: fa8cb29f145c7658227f93d08a990c98563a0cfc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "60730021"
---
# <a name="iot-hub-device-reprovisioning-concepts"></a>Concetti di reprovisioning di un dispositivo hub IoT

Durante il ciclo di vita di una soluzione IoT, è comune spostare i dispositivi tra hub IoT. Le ragioni di questo spostamento possono includere gli scenari seguenti:

* **Georilevazione/GeoLatency**: Quando un dispositivo si sposta tra le posizioni, latenza di rete è stata migliorata facendo in modo che il dispositivo di eseguire la migrazione a un hub IoT più da vicino.

* **Multi-tenancy**: Un dispositivo può essere usato all'interno della stessa soluzione IoT e riassegnato a un nuovo cliente, o di un sito cliente. Questo nuovo cliente può essere servito usando un hub IoT diverso.

* **Modifica di soluzione**: Un dispositivo potrebbe essere spostato in una soluzione IoT nuova o aggiornata. Questa riassegnazione potrebbe richiedere al dispositivo di comunicare con un nuovo hub IoT collegato ad altri componenti di back-end.

* **Quarantine**: Simile a una modifica della soluzione. Un dispositivo malfunzionante, compromesso o obsoleto può essere riassegnato a un hub IoT che può solo aggiornare e ripristinare la conformità. Una volta che il dispositivo funziona correttamente, viene quindi migrato nuovamente al suo hub principale.

Il supporto di reprovisioning all'interno del Device Provisioning Service risponde a queste esigenze. I dispositivi possono essere automaticamente riassegnati al nuovo hub IoT in base al criterio di reprovisioning configurato nella voce di registrazione del dispositivo.

## <a name="device-state-data"></a>Dati relativi allo stato del dispositivo

I dati relativi allo stato del dispositivo sono costituiti dal [dispositivo gemello](../iot-hub/iot-hub-devguide-device-twins.md) e dalle funzionalità del dispositivo. Questi dati vengono archiviati nell'istanza del servizio Device Provisioning e nell'hub IoT al quale un dispositivo viene assegnato.

![Provisioning con il servizio Device Provisioning](./media/concepts-device-reprovisioning/dps-provisioning.png)

Quando un dispositivo viene inizialmente sottoposto a provisioning con un'istanza del servizio Device Provisioning, vengono eseguiti i passaggi seguenti:

1. Il dispositivo invia una richiesta di provisioning all'istanza del servizio Device Provisioning. L'istanza del servizio autentica l'identità del dispositivo basata su una voce di registrazione e crea la configurazione iniziale dei dati sullo stato del dispositivo. L'istanza del servizio assegna il dispositivo a un hub IoT in base alla configurazione della registrazione e restituisce l'assegnazione dell'hub IoT al dispositivo.

2. L'istanza del servizio di provisioning restituisce una copia di tutti i dati sullo stato iniziale del dispositivo all'hub IoT assegnato. Il dispositivo si connette all'hub Iot assegnato e inizia le operazioni.

Nel corso del tempo i dati sullo stato del dispositivo nell'hub IoT possono essere aggiornati dalle [operazioni del dispositivo](../iot-hub/iot-hub-devguide-device-twins.md#device-operations) e le [operazioni back-end](../iot-hub/iot-hub-devguide-device-twins.md#back-end-operations). Le informazioni sullo stato iniziale del dispositivo archiviate nell'istanza del servizio Device Provisioning rimangono inalterate. Questi dati inalterati sullo stato del dispositivo sono la configurazione iniziale.

![Provisioning con il servizio Device Provisioning](./media/concepts-device-reprovisioning/dps-provisioning-2.png)

A seconda dello scenario, mentre un dispositivo si sposta tra gli hub IoT, potrebbe anche essere necessario migrare lo stato del dispositivo aggiornato sul precedente hub IoT sul nuovo hub IoT. Questa migrazione è supportata da nuovi criteri di reprovisioning del servizio Device Provisioning.

## <a name="reprovisioning-policies"></a>Criteri di reprovisioning

In genere (in base allo scenario), un dispositivo invia una richiesta a un'istanza del servizio di provisioning al riavvio del sistema. Supporta anche un metodo per l'attivazione manuale del provisioning su richiesta. I criteri per rieffettuare il provisioning su una voce di registrazione determinano il modo in cui l'istanza del servizio Device Provisioning gestisce queste richieste di provisioning e stabilisce se i dati sullo stato del dispositivo devono essere migrati durante il nuovo provisioning. Gli stessi criteri sono disponibili per le registrazioni individuali e di gruppi:

* **Rieseguire il provisioning e la migrazione dei dati**: Questo criterio è il valore predefinito per nuove voci di registrazione. Questi criteri intervengono quando i dispositivi associati alla voce di registrazione presentano una nuova richiesta (1). A seconda della configurazione della voce di registrazione, il dispositivo può essere riassegnato a un altro hub IoT. Se il dispositivo sta cambiando l'hub IoT, la registrazione del dispositivo con l'hub IoT iniziale verrà rimossa. Le informazioni sullo stato dei dispositivi aggiornati da tale hub IoT iniziale verranno migrate sul nuovo hub IoT (2). Durante la migrazione, lo stato del dispositivo risulterà come **In fase di assegnazione**.

    ![Provisioning con il servizio Device Provisioning](./media/concepts-device-reprovisioning/dps-reprovisioning-migrate.png)

* **Rieseguire il provisioning e reimpostare la configurazione iniziale**: Questo criterio interviene quando i dispositivi associati alla voce di registrazione presentano una nuova richiesta di provisioning (1). A seconda della configurazione della voce di registrazione, il dispositivo può essere riassegnato a un altro hub IoT. Se il dispositivo sta cambiando l'hub IoT, la registrazione del dispositivo con l'hub IoT iniziale verrà rimossa. I dati di configurazione iniziali che l'istanza del servizio di provisioning ha ricevuto durante il provisioning del dispositivo vengono forniti al nuovo hub IoT (2). Durante la migrazione, lo stato del dispositivo risulterà come **In fase di assegnazione**.

    Questo criterio viene spesso usato per il ripristino senza modificare l'hub IoT.

    ![Provisioning con il servizio Device Provisioning](./media/concepts-device-reprovisioning/dps-reprovisioning-reset.png)

* **Mai nuovamente il provisioning**: Il dispositivo non viene mai riassegnato a un hub diversi. Questo criterio viene fornito per gestire la compatibilità con le versioni precedenti.

### <a name="managing-backwards-compatibility"></a>Gestire la compatibilità con le versioni precedenti

Prima di settembre 2018, le assegnazioni dei dispositivi agli hub IoT avevano un carattere definitivo. Quando un dispositivo torna indietro nel processo di provisioning, potrà essere assegnato solo allo stesso hub IoT.

Per le soluzioni che hanno aggiunto una dipendenza da questo comportamento, il servizio di provisioning include la retrocompatibilità. Questo comportamento viene mantenuto attualmente per i dispositivi in base ai criteri seguenti:

1. I dispositivi si connettono con una versione dell'API precedente alla disponibilità del supporto di reprovisioning nativo nel servizio Device Provisioning. Fare riferimento alla tabella API di seguito.

2. Alla voce di registrazione per i dispositivi non sono associati criteri di reprovisioning impostati.

Questa compatibilità garantisce che i dispositivi già distribuiti presentino lo stesso comportamento tenuto durante il test iniziale. Per mantenere il comportamento precedente, non salvare un criterio di reprovisioning su queste registrazioni. Se è impostato un criterio di reprovisioning, quest’ultimo ha la precedenza sul comportamento. Consentendo al criterio di reprovisioning di avere la precedenza, i clienti possono aggiornare il comportamento del dispositivo senza dover ricreare l'immagine del dispositivo.

Il seguente diagramma di flusso aiuta a capire quando è presente il comportamento:

![diagramma di flusso della compatibilità con le versioni precedenti](./media/concepts-device-reprovisioning/reprovisioning-compatibility-flow.png)

La tabella seguente mostra le versioni dell'API precedenti alla disponibilità del supporto di reprovisioning nativo nel servizio Device Provisioning:

| API REST | SDK per C | Python SDK |  Node SDK | SDK per Java | .NET SDK |
| -------- | ----- | ---------- | --------- | -------- | -------- |
| [2018-04-01 e versioni precedenti](/rest/api/iot-dps/createorupdateindividualenrollment/createorupdateindividualenrollment#uri-parameters) | [1.2.8 e versioni precedenti](https://github.com/Azure/azure-iot-sdk-c/blob/master/version.txt) | [1.4.2 e versioni precedenti](https://github.com/Azure/azure-iot-sdk-python/blob/0a549f21f7f4fc24bc036c1d2d5614e9544a9667/device/iothub_client_python/src/iothub_client_python.cpp#L53) | [1.7.3 e versioni precedenti](https://github.com/Azure/azure-iot-sdk-node/blob/074c1ac135aebb520d401b942acfad2d58fdc07f/common/core/package.json#L3) | [1.13.0 e versioni precedenti](https://github.com/Azure/azure-iot-sdk-java/blob/794c128000358b8ed1c4cecfbf21734dd6824de9/device/iot-device-client/pom.xml#L7) | [1.1.0 e versioni precedenti](https://github.com/Azure/azure-iot-sdk-csharp/blob/9f7269f4f61cff3536708cf3dc412a7316ed6236/provisioning/device/src/Microsoft.Azure.Devices.Provisioning.Client.csproj#L20)

> [!NOTE]
> Questi valori e i collegamenti sono soggetti a modifica. Questo è solo un tentativo di segnaposto per determinare dove le versioni possono essere determinate da un cliente e quali saranno le versioni previste.

## <a name="next-steps"></a>Passaggi successivi

* [Come rieffettuare il provisioning dei dispositivi](how-to-reprovision.md)
