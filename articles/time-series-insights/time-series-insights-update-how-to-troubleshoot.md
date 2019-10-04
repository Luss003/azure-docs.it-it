---
title: Diagnosticare e risolvere i problemi dell'anteprima di Azure Time Series Insights | Microsoft Docs
description: Informazioni su come diagnosticare e risolvere i problemi con l'anteprima di Azure Time Series Insights.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: cdcbe62fdba4f111233451680f95abc757e80ee3
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/09/2019
ms.locfileid: "68883331"
---
# <a name="diagnose-and-troubleshoot"></a>Diagnosticare e risolvere i problemi

Questo articolo riepiloga i diversi problemi comuni che possono verificarsi quando si lavora nell'ambiente di anteprima di Azure Time Series Insights. L'articolo descrive anche possibili cause e soluzioni per ogni problema.

## <a name="problem-i-cant-find-my-environment-in-the-preview-explorer"></a>Problema: Impossibile trovare l'ambiente in Esplora anteprime

Questo problema può verificarsi se non si dispone delle autorizzazioni per accedere all'ambiente di Time Series Insights. Gli utenti necessitano di un ruolo di lettore a livello di accesso per visualizzare l'ambiente Time Series Insights. Per verificare i livelli di accesso correnti e concedere un accesso aggiuntivo, visitare la sezione relativa ai Criteri di accesso ai dati per la risorsa Time Series Insights nel [portale di Azure](https://portal.azure.com/).

  [![Ambiente](media/v2-update-diagnose-and-troubleshoot/environment.png)](media/v2-update-diagnose-and-troubleshoot/environment.png#lightbox)

## <a name="problem-no-data-is-seen-in-the-preview-explorer"></a>Problema: non vengono visualizzati dati in Esplora anteprime

Ecco alcuni motivi per cui i dati potrebbero non essere visualizzati nello [strumento di esplorazione dell'anteprima di Azure Time Series Insights](https://insights.timeseries.azure.com/preview).

- L'origine evento potrebbe non ricevere correttamente i dati.

    Verificare che l'origine evento, ovvero un hub eventi o un hub IoT, riceva dati da tag o istanze. Per verificare ciò, passare alla pagina di panoramica della risorsa nel portale di Azure.

    [![Dashboard-informazioni dettagliate](media/v2-update-diagnose-and-troubleshoot/dashboard-insights.png)](media/v2-update-diagnose-and-troubleshoot/dashboard-insights.png#lightbox)

- I dati dell'origine evento non sono in formato JSON.

    Time Series Insights supporta solo dati in formato JSON. Per alcuni esempi di dati in formato JSON, vedere [Forme JSON supportate](./how-to-shape-query-json.md).

- Alla chiave dell'origine evento manca un'autorizzazione obbligatoria.

  * Per un hub IoT è necessario fornire la chiave con l'autorizzazione di **connessione al servizio**.

    [![Configurazione](media/v2-update-diagnose-and-troubleshoot/configuration.png)](media/v2-update-diagnose-and-troubleshoot/configuration.png#lightbox)

  * Come illustrato nell'immagine precedente, entrambi i criteri **iothubowner** e **service** funzionano, in quanto entrambi dispongono dell'autorizzazione di **connessione al servizio**.
  * Per un hub eventi è necessario fornire la chiave con l'autorizzazione di **ascolto**.
  
    [![Autorizzazioni](media/v2-update-diagnose-and-troubleshoot/permissions.png)](media/v2-update-diagnose-and-troubleshoot/permissions.png#lightbox)

  * Come illustrato nell'immagine precedente, entrambi i criteri **read** e **manage** funzionano, in quanto entrambi dispongono dell'autorizzazione di **ascolto**.

- Il gruppo di consumer specificato non è esclusivo di Time Series Insights.

    Durante la registrazione di un hub IoT o un hub eventi viene specificato il gruppo di consumer da usare per la lettura dei dati. Non condividere tale gruppo di consumer. Se viene condiviso, l'hub eventi sottostante disconnette automaticamente uno dei lettori in modo casuale. Specificare un gruppo di consumer univoco per la lettura dei dati da parte di Time Series Insights.

- La proprietà ID di Time Series specificata al momento del provisioning è errata, mancante o null.

    Questo problema può verificarsi se la proprietà ID di Time Series non è configurata in modo corretto al momento del provisioning dell'ambiente. Per altre informazioni, leggere [Best practices for choosing a Time Series ID](./time-series-insights-update-how-to-id.md) (Procedure consigliate per la scelta di un ID Time Series). In questo momento, non è possibile aggiornare un ambiente Time Series Insights esistente per usare un ID Time Series differente.

## <a name="problem-some-data-shows-but-some-is-missing"></a>Problema: alcuni dati risultano mancanti.

È possibile che si stia inviando i dati senza ID Time Series.

- Questo problema può verificarsi quando si inviano gli eventi senza il campo ID Time Series nel payload. Per altre informazioni, vedere [Forme JSON supportate](./how-to-shape-query-json.md).

- Questo problema può verificarsi perché l'ambiente è limitato nelle richieste.

    > [!NOTE]
    > Attualmente, Time Series Insights supporta una velocità di inserimento massima di 6 MB/s.

## <a name="problem-my-event-sources-timestamp-property-name-doesnt-work"></a>Problema: il nome della proprietà timestamp dell'origine evento non funziona

Verificare che il nome e il valore siano conformi alle regole seguenti:

* Il nome della proprietà Timestamp fa distinzione fra maiuscole e minuscole.
* Il valore della proprietà timestamp, che proviene dall'origine evento come stringa JSON, deve avere il formato `yyyy-MM-ddTHH:mm:ss.FFFFFFFK`. Un esempio di tale stringa è `“2008-04-12T12:53Z”`.

Il modo più semplice per garantire l'acquisizione e il corretto funzionamento del nome della proprietà Timestamp consiste nell'usare lo strumento di esplorazione di anteprima di Time Series Insights. All'interno dello strumento di esplorazione di anteprima di Time Series Insights, usando il grafico, selezionare un periodo di tempo dopo aver specificato il nome della proprietà Timestamp. Fare clic con il pulsante destro del mouse sulla selezione e selezionare l'opzione **Esplora eventi**. L'intestazione della prima colonna contiene il nome della proprietà Timestamp. Deve avere `($ts)` accanto alla parola `Timestamp`, invece di:

* `(abc)`, che indica che Time Series Insights legge i valori dei dati come stringhe.
* Icona del calendario, che indica che Time Series Insights legge i valori dei dati come datetime.
* `#`, che indica che Time Series Insights legge i valori dei dati come integer.

Se la proprietà Timestamp non è specificata in modo esplicito, l'hub IoT o hub eventi di un evento viene ora usato come timestamp predefinito.

## <a name="problem-i-cant-view-or-edit-my-time-series-model"></a>Problema: Non è possibile visualizzare o modificare il modello Time Series

- È possibile accedere a un ambiente Time Series Insights S1 o S2.

   I modelli Time Series sono supportati solo negli ambienti con pagamento in base al consumo. Per altre informazioni su come accedere all'ambiente S1 o S2 dallo strumento di esplorazione dell'anteprima di Time Series Insights, vedere [Visualizzare i dati nello strumento di esplorazione](./time-series-insights-update-explorer.md).

   [![Accesso](media/v2-update-diagnose-and-troubleshoot/access.png)](media/v2-update-diagnose-and-troubleshoot/access.png#lightbox)

- Non si dispone delle autorizzazioni per visualizzare e modificare il modello.

   Gli utenti devono accedere a livello di collaboratore per modificare e visualizzare il modello Time Series. Per verificare i livelli di accesso correnti e concedere un accesso aggiuntivo, visitare la sezione relativa ai Criteri di accesso ai dati per la risorsa Time Series Insights nel portale di Azure.

## <a name="problem-all-my-instances-in-the-preview-explorer-lack-a-parent"></a>Problema: tutte le istanze personali in Esplora anteprime non dispongono di un elemento padre

Questo problema può verificarsi se l'ambiente non dispone di una gerarchia di modelli Time Series definita. Per altre informazioni, vedere [Lavorare con modelli di Time Series](./time-series-insights-update-how-to-tsm.md).

  [![Modelli Time Series](media/v2-update-diagnose-and-troubleshoot/tsm.png)](media/v2-update-diagnose-and-troubleshoot/tsm.png#lightbox)

## <a name="next-steps"></a>Passaggi successivi

- Leggere [Lavorare con modelli di Time Series](./time-series-insights-update-how-to-tsm.md).

- Informazioni sulle [forme JSON supportate](./how-to-shape-query-json.md).
