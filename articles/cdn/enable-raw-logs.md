---
title: Log RAW HTTP della rete CDN di Azure
description: Questo articolo descrive i log non elaborati HTTP della rete CDN di Azure.
services: cdn
author: asudbring
manager: KumudD
ms.service: azure-cdn
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2020
ms.author: allensu
ms.openlocfilehash: 79ced4df8df12bf2ef1fbe0075b53d02fafb2aff
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/11/2020
ms.locfileid: "79129816"
---
# <a name="azure-cdn-http-raw-logs"></a>Log RAW HTTP della rete CDN di Azure
I log non elaborati forniscono informazioni dettagliate sulle operazioni e sugli errori importanti per il controllo e la risoluzione dei problemi. I log non elaborati differiscono dai log attività. I log attività offrono visibilità sulle operazioni eseguite sulle risorse di Azure. I log non elaborati forniscono un record delle operazioni della risorsa.

> [!IMPORTANT]
> La funzionalità log non elaborati HTTP è disponibile per la rete CDN di Azure di Microsoft.

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare. 

## <a name="sign-in-to-azure"></a>Accedere ad Azure

Accedere al portale di Azure all'indirizzo [https://portal.azure.com](https://portal.azure.com).

## <a name="configuration"></a>Configurazione

Per configurare i log non elaborati per la rete CDN di Azure dal profilo Microsoft: 

1. Scegliere **tutte le risorse** >> dal menu portale di Azure **\<> del profilo**di rete CDN.

2. In **monitoraggio**selezionare **impostazioni di diagnostica**.

3. Selezionare **+ Aggiungi impostazioni di diagnostica**.

    ![Impostazioni di diagnostica della rete CDN](./media/cdn-raw-logs/raw-logs-01.png)

    > [!IMPORTANT]
    > I log non elaborati sono disponibili solo nel livello di profilo mentre i log del codice di stato http aggregati sono disponibili a livello di endpoint.

4. In **impostazioni di diagnostica**immettere un nome per l'impostazione di diagnostica in **nome impostazioni**di diagnostica.

5. Selezionare il **log** e impostare la conservazione in giorni.

6. Selezionare i **Dettagli di destinazione**. Le opzioni di destinazione sono:
    * **Invia a Log Analytics**
        * Selezionare la **sottoscrizione** e l' **area di lavoro log Analytics**.
    * **Archivia in un account di archiviazione**
        * Selezionare la **sottoscrizione** e l' **account di archiviazione**.
    * **Trasmettere a un hub eventi**
        * Selezionare la **sottoscrizione**, **lo spazio dei nomi dell'hub eventi**, il **nome dell'hub eventi (facoltativo)** e il **nome del criterio dell'hub eventi**.

    ![Impostazioni di diagnostica della rete CDN](./media/cdn-raw-logs/raw-logs-02.png)

7. Selezionare **Salva**.

## <a name="raw-logs-properties"></a>Proprietà log non elaborati

La rete CDN di Azure del servizio Microsoft fornisce attualmente log non elaborati (in batch ogni ora). I log non elaborati forniscono singole richieste API a ogni voce con lo schema seguente: 

| Proprietà              | Descrizione                                                                                                                                                                                          |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| TrackingReference     | Stringa di riferimento univoca che identifica una richiesta servita dalla porta anteriore, inviata anche come intestazione X-Azure-Ref al client. Obbligatorio per la ricerca dei dettagli nei log di accesso per una richiesta specifica. |
| HttpMethod            | Metodo HTTP usato dalla richiesta.                                                                                                                                                                     |
| HttpVersion           | Tipo della richiesta o della connessione.                                                                                                                                                                   |
| RequestUri            | URI della richiesta ricevuta.                                                                                                                                                                         |
| RequestBytes          | Dimensioni del messaggio di richiesta HTTP in byte, incluse le intestazioni della richiesta e il corpo della richiesta.                                                                                                   |
| ResponseBytes         | Byte inviati dal server back-end come risposta.                                                                                                                                                    |
| UserAgent             | Tipo di browser utilizzato dal client.                                                                                                                                                               |
| ClientIp              | Indirizzo IP del client che ha eseguito la richiesta.                                                                                                                                                  |
| TimeTaken             | Periodo di tempo impiegato dall'azione, in millisecondi.                                                                                                                                            |
| SecurityProtocol      | Versione del protocollo TLS/SSL utilizzata dalla richiesta o null se non è presente alcuna crittografia.                                                                                                                           |
| Endpoint              | L'host dell'endpoint della rete CDN è stato configurato nel profilo della rete CDN padre.                                                                                                                                   |
| Nome host backend     | Nome dell'host o dell'origine back-end in cui vengono inviate le richieste.                                                                                                                                |
| Inviato a Shield di origine | Se true, significa che la risposta alla richiesta è stata ricevuta dalla cache dello scudo di origine anziché dal pop perimetrale. Shield di origine è una cache padre usata per migliorare la percentuale di riscontri nella cache.                                       |
| HttpStatusCode        | Codice di stato HTTP restituito dal proxy.                                                                                                                                                        |
| HttpStatusDetails     | Stato risultante della richiesta. Il significato di questo valore stringa è reperibile in una tabella di riferimento sullo stato.                                                                                              |
| Popup                   | Pop Edge, che ha risposto alla richiesta dell'utente. Le abbreviazioni dei pop sono codici aeroportuali delle rispettive metropolitane.                                                                                   |
| Stato cache          | Indica se l'oggetto è stato restituito dalla cache o proviene dall'origine.                                                                                                             |

## <a name="next-steps"></a>Passaggi successivi
In questo articolo sono stati abilitati i log non elaborati HTTP per il servizio rete CDN Microsoft.

Per ulteriori informazioni sulla rete CDN di Azure e sugli altri servizi di Azure indicati in questo articolo, vedere:

* [Analizza](cdn-log-analysis.md) Modelli di utilizzo della rete CDN di Azure.

* Altre informazioni su [Monitoraggio di Azure](https://docs.microsoft.com/azure/azure-monitor/overview).

* Configurare [log Analytics in monitoraggio di Azure](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal).