---
title: Impostare gli avvisi di monitoraggio per i processi di Analisi di flusso di Azure
description: Questo articolo descrive come usare il portale di Azure per impostare il monitoraggio e gli avvisi per i processi di Analisi di flusso di Azure.
author: jseb225
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: 836b7a489e3c73d745b128cbbc0c3566220ac409
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75458725"
---
# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Impostare gli avvisi per i processi di Analisi di flusso di Azure

È importante monitorare il processo di Analisi di flusso di Azure per garantire che l'esecuzione del processo sia continua e senza problemi. Questo articolo descrive come configurare gli avvisi per gli scenari comuni che devono essere monitorati. 

È possibile definire regole sulle metriche dei dati dei log delle operazioni tramite il portale, nonché [a livello di codice](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a).

## <a name="set-up-alerts-in-the-azure-portal"></a>Configurare gli avvisi nel portale di Azure
### <a name="get-alerted-when-a-job-stops-unexpectedly"></a>Ricevere un avviso quando un processo viene arrestato in modo imprevisto

L'esempio seguente dimostra come configurare gli avvisi per l'attivazione di uno stato di errore per il processo. Questo avviso è consigliato per tutti i processi.

1. Nel portale di Azure aprire il processo di Analisi di flusso per cui si intende creare un avviso.

2. Nella pagina **Processo** passare alla sezione **Monitoraggio**.  

3. Selezionare **metrica**, quindi **nuova regola di avviso**.

   ![Configurazione degli avvisi di Analisi di flusso nel portale di Azure](./media/stream-analytics-set-up-alerts/stream-analytics-set-up-alerts.png)  

4. Il nome del processo di Analisi di flusso dovrebbe essere visualizzato automaticamente in **RISORSA**. Fare clic su **Aggiungi condizione** e selezionare **All Administrative operations** (Tutte le operazioni amministrative) in **Configura logica dei segnali**.

   ![Selezionare il nome del segnale per l'avviso di Analisi di flusso](./media/stream-analytics-set-up-alerts/stream-analytics-condition-signal.png)  

5. In **Configura logica dei segnali** impostare **Livello evento** su **Tutti** e **Stato** su **Errore**. Lasciare l' **evento avviato da** Blank e selezionare **fine**.

   ![Configurare la logica dei segnali per l'avviso di Analisi di flusso](./media/stream-analytics-set-up-alerts/stream-analytics-configure-signal-logic.png) 

6. Selezionare un gruppo di azioni esistente o crearne uno nuovo. In questo esempio è stato creato un nuovo gruppo di azioni chiamato **TIDashboardGroupActions** con l'azione **Messaggi di posta elettronica** che invia un messaggio di posta elettronica agli utenti con il ruolo **Proprietario** di Azure Resource Manager.

   ![Configurazione di un avviso per un processo di Analisi di flusso di Azure](./media/stream-analytics-set-up-alerts/stream-analytics-add-group-email-action.png)

7. Dovrebbe essere presente una voce per **RISORSA**, **CONDIZIONE** e **GRUPPI DI AZIONI**. Si noti che è necessario che le condizioni definite siano soddisfatte affinché sia possibile attivare gli avvisi. Ad esempio, è possibile misurare il valore medio di una metrica negli ultimi 15 minuti, ogni 5 minuti.

   ![Creare una regola di avviso di Analisi di flusso](./media/stream-analytics-set-up-alerts/stream-analytics-create-alert-rule-2.png)

   Aggiungere **Nome regola di avviso**, **Descrizione** e **Gruppo di risorse** per **DETTAGLI AVVISO** e fare clic su **Crea regola di avviso** per creare la regola per il processo di Analisi di flusso.

   ![Creare una regola di avviso di Analisi di flusso](./media/stream-analytics-set-up-alerts/stream-analytics-create-alert-rule.png)
   
## <a name="scenarios-to-monitor"></a>Scenari da monitorare

Gli avvisi seguenti sono consigliati per il monitoraggio delle prestazioni del processo di Analisi di flusso. Queste metriche devono essere valutate ogni minuto nell'ultimo periodo di 5 minuti.

|Metrica|Condizione|Aggregazione temporale|Soglia|Azioni correttive|
|-|-|-|-|-|
|% utilizzo unità di streaming|Più di|Massimo|80|Esistono più fattori che determinano un maggiore utilizzo in percentuale delle unità di streaming. È possibile ridimensionare con la parallelizzazione delle query o aumentare il numero di unità di streaming. Per altre informazioni, vedere [Sfruttare i vantaggi della parallelizzazione delle query in Analisi di flusso di Azure](stream-analytics-parallelization.md).|
|Errori di runtime|Più di|Totale|0|Esaminare i log di attività o di diagnostica e apportare le modifiche appropriate per input, query o output.|
|Ritardo limite|Più di|Massimo|Quando il valore medio della metrica negli ultimi 15 minuti è maggiore della tolleranza per arrivo in ritardo (in secondi). Se non è stata modificata la tolleranza per arrivo in ritardo, il valore predefinito è impostato su 5 secondi.|Provare ad aumentare il numero di unità di streaming o la parallelizzazione delle query. Per altre informazioni sulle unità di streaming, vedere [Informazioni sulle unità di streaming e su come modificarle](stream-analytics-streaming-unit-consumption.md#how-many-sus-are-required-for-a-job). Per altre informazioni sulla parallelizzazione delle query, vedere [Sfruttare i vantaggi della parallelizzazione delle query in Analisi di flusso di Azure](stream-analytics-parallelization.md).|
|Errori di deserializzazione dell'input|Più di|Totale|0|Esaminare i log di attività o di diagnostica e apportare le modifiche appropriate per l'input. Per altre informazioni sui log di diagnostica, vedere [Risoluzione dei problemi di Analisi di flusso di Azure tramite i log di diagnostica](stream-analytics-job-diagnostic-logs.md)|

## <a name="get-help"></a>Ottenere supporto

Per altre informazioni dettagliate sulla configurazione degli avvisi nel portale di Azure, vedere [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) (Ricevere notifiche di avviso).  

Per ulteriore assistenza, provare il [Forum di Analisi dei flussi di Azure](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Passaggi successivi
* [Introduzione ad Analisi dei flussi di Azure](stream-analytics-introduction.md)
* [Introduzione all'uso di Analisi dei flussi di Azure](stream-analytics-get-started.md)
* [Ridimensionare i processi di Analisi dei flussi di Azure](stream-analytics-scale-jobs.md)
* [Informazioni di riferimento sul linguaggio di query di Analisi di flusso di Azure](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Informazioni di riferimento sulle API REST di gestione di Analisi di flusso di Azure](https://msdn.microsoft.com/library/azure/dn835031.aspx)

