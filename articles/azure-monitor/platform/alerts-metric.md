---
title: Creare, visualizzare e gestire gli avvisi delle metriche con Monitoraggio di Azure
description: Informazioni su come usare il portale di Azure o l'interfaccia della riga di comando per creare, visualizzare e gestire regole di avviso per le metriche.
author: harelbr
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: harelbr
ms.subservice: alerts
ms.openlocfilehash: 00f5f37591ed2ed250cb756c686ea15136921512
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/23/2020
ms.locfileid: "76705531"
---
# <a name="create-view-and-manage-metric-alerts-using-azure-monitor"></a>Creare, visualizzare e gestire gli avvisi delle metriche con Monitoraggio di Azure

Gli avvisi delle metriche in Monitoraggio di Azure consentono di ricevere una notifica quando una delle metriche supera una soglia. Gli avvisi delle metriche funzionano su una gamma di metriche di piattaforme multidimensionali, personalizzate, standard e personalizzate di Application Insights. In questo articolo verrà illustrato come creare, visualizzare e gestire le regole di avviso per le metriche tramite il portale di Azure e l'interfaccia della riga di comando di Azure. È anche possibile creare regole di avviso per le metriche usando modelli di Azure Resource Manager, come descritto in [un articolo distinto](alerts-metric-create-templates.md).

Altre informazioni sul funzionamento degli avvisi delle metriche sono disponibili nella [panoramica degli avvisi delle metriche](alerts-metric-overview.md).

## <a name="create-with-azure-portal"></a>Creare con il portale di Azure

La procedura seguente descrive come creare una regola di avviso per la metrica nel portale di Azure:

1. Nel [portale di Azure](https://portal.azure.com) fare clic su **Monitoraggio**. Il pannello Monitoraggio consolida tutte le impostazioni e i dati di monitoraggio in una vista.

2. Fare clic su **Avvisi** e su **+Nuova regola di avviso**.

    > [!TIP]
    > Anche la maggior parte dei pannelli di risorse include la voce **Avvisi** nel menu delle risorse, in **Monitoraggio**: è possibile creare avvisi anche qui.

3. Fare clic su **Selezionare la destinazione** e, nel riquadro del contesto caricato, selezionare una risorsa di destinazione su cui si vuole impostare una regola di avviso. Usare gli elenchi a discesa **Sottoscrizione** e **Tipo di risorsa** per trovare la risorsa da monitorare. È anche possibile usare la barra di ricerca per trovare la risorsa.

4. Se la risorsa selezionata include metriche su cui creare avvisi, **Segnali disponibili** nella parte inferiore destra includerà le metriche. L'elenco completo dei tipi di risorse supportati per gli avvisi delle metriche è disponibile in questo [articolo](../../azure-monitor/platform/alerts-metric-near-real-time.md#metrics-and-dimensions-supported).

5. Dopo aver selezionato una risorsa di destinazione, fare clic su **Aggiungi condizione**.

6. Verrà visualizzato un elenco dei segnali supportati per la risorsa. Selezionare la metrica su cui creare un avviso.

7. Se si vuole, ridefinire la metrica modificando i valori di **Periodo** e **Aggregazione**. Se la metrica include dimensioni, verrà visualizzata la tabella **Dimensioni**. Selezionare uno o più valori per ogni dimensione. L'avviso della metrica valuterà la condizione per tutte le combinazioni di valori selezionate. [Altre informazioni sul funzionamento degli avvisi sulle metriche multidimensionali](alerts-metric-overview.md). È anche possibile scegliere **Seleziona\*** per qualsiasi dimensione. **Seleziona \*** ridimensiona dinamicamente la selezione a tutti i valori correnti e futuri per una dimensione.

8. Verrà visualizzato un grafico per la metrica per le ultime 6 ore. Definire i parametri dell'avviso, ossia **Tipo di condizione**, **Frequenza**, **Operatore** e **Soglia** o **Sensibilità**, per determinare la logica che verrà valutata dalla regola di avviso delle metriche. [Altre informazioni sulle opzioni di sensibilità e i tipi di condizione delle soglie dinamiche](alerts-dynamic-thresholds.md).

9. Se si usa una soglia statica, il grafico delle metriche può essere utile per determinare quale potrebbe essere una soglia ragionevole. Se si usa una soglia dinamica, il grafico delle metriche mostrerà le soglie calcolate in base ai dati recenti.

10. Fare clic su **Fine**

11. Facoltativamente, aggiungere altri criteri per monitorare una regola di avviso complessa. Attualmente gli utenti possono ricevere regole di avviso con criteri di soglia dinamica come singolo criterio.

12. Compilare **Dettagli avviso** come **Nome regola di avviso**, **Descrizione** e **Gravità**

13. Aggiungere un gruppo di azioni per l'avviso selezionando un gruppo di azioni esistente o creandone uno nuovo.

14. Fare clic su **Fine** per salvare la regola di avviso per la metrica.

> [!NOTE]
> Le regole di avviso per le metriche create mediante il portale vengono generate nello stesso gruppo di risorse della risorsa di destinazione.

## <a name="view-and-manage-with-azure-portal"></a>Visualizzare e gestire con il portale di Azure

È possibile visualizzare e gestire le regole di avviso per le metriche tramite il pannello Gestisci le regole in Avvisi. La procedura riportata di seguito spiega come visualizzare le regole di avviso per le metriche e modificare una di esse.

1. Nel portale di Azure passare a **Monitoraggio**

2. Fare clic su **Avvisi** e **Gestisci le regole**

3. Nel pannello **Gestisci le regole** è possibile visualizzare tutte le regole di avviso tra le sottoscrizioni. È possibile filtrare ulteriormente le regole usando **Gruppo di risorse**, **Tipo di risorsa** e **Risorsa**. Per visualizzare solo gli avvisi delle metriche, selezionare **Tipi di segnali** come Metriche.

    > [!TIP]
    > Nel pannello **Gestisci le regole** è possibile selezionare più regole di avviso e abilitarle/disabilitarle. Questa operazione potrebbe essere utile quando è necessario sottoporre a manutenzione alcune risorse di destinazione

4. Fare clic sul nome della regola di avviso per le metriche da modificare

5. In Modifica regola fare clic sui **criteri di avviso** da modificare. È possibile modificare la metrica, la condizione di soglia e altri campi in base alle esigenze

    > [!NOTE]
    > Non è possibile modificare **Risorsa di destinazione** e **Nome regola di avviso** dopo aver creato l'avviso per le metriche.

6. Fare clic su **Fine** per salvare le modifiche.

## <a name="with-azure-cli"></a>Con l'interfaccia della riga di comando di Azure

Le sezioni precedenti descrivono come creare, visualizzare e gestire le regole di avviso per le metriche tramite il portale di Azure. Questa sezione descrive come eseguire la stessa operazione usando l'[interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest) multipiattaforma. [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest) è il metodo più rapido per iniziare a usare l'interfaccia della riga di comando di Azure. In questo articolo si userà Cloud Shell.

1. Passare al portale di Azure e fare clic su **Cloud Shell**.

2. Al prompt è possibile usare i comandi con l'opzione ``--help`` per altre informazioni sul comando e su come usarlo. Ad esempio, il comando seguente mostra l'elenco di comandi disponibili per la creazione, la visualizzazione e la gestione degli avvisi delle metriche

    ```azurecli
    az monitor metrics alert --help
    ```

3. È possibile creare una regola di avviso delle metriche semplice che monitora se il valore medio di Percentuale CPU in una macchina virtuale è maggiore di 90

    ```azurecli
    az monitor metrics alert create -n {nameofthealert} -g {ResourceGroup} --scopes {VirtualMachineResourceID} --condition "avg Percentage CPU > 90" --description {descriptionofthealert}
    ```

4. È possibile visualizzare tutti gli avvisi delle metriche in un gruppo di risorse usando il comando seguente

    ```azurecli
    az monitor metrics alert list  -g {ResourceGroup}
    ```

5. È possibile visualizzare i dettagli di una particolare regola di avviso delle metriche con il nome o l'ID di risorsa della regola.

    ```azurecli
    az monitor metrics alert show -g {ResourceGroup} -n {AlertRuleName}
    ```

    ```azurecli
    az monitor metrics alert show --ids {RuleResourceId}
    ```

6. È possibile disabilitare una regola di avviso per le metriche con il comando seguente.

    ```azurecli
    az monitor metrics alert update -g {ResourceGroup} -n {AlertRuleName} --enabled false
    ```

7. È possibile eliminare una regola di avviso per le metriche con il comando seguente.

    ```azurecli
    az monitor metrics alert delete -g {ResourceGroup} -n {AlertRuleName}
    ```

## <a name="next-steps"></a>Passaggi successivi

- [Creare avvisi delle metriche mediante modelli di Azure Resource Manager](../../azure-monitor/platform/alerts-enable-template.md).
- [Comprendere il funzionamento degli avvisi delle metriche](alerts-metric-overview.md).
- [Comprendere il funzionamento degli avvisi delle metriche con il tipo di condizione delle soglie dinamiche](alerts-dynamic-thresholds.md).
- [Understand the web hook schema for metric alerts](../../azure-monitor/platform/alerts-metric-near-real-time.md#payload-schema) (Comprendere lo schema webhook per gli avvisi delle metriche)

