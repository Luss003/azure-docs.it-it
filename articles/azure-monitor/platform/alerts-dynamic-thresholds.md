---
title: Creazione di avvisi con soglie dinamiche in Monitoraggio di Azure
description: Creare avvisi con apprendimento automatico in base a soglie dinamiche
author: yanivlavi
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: yalavi
ms.reviewer: mbullwin
ms.openlocfilehash: 0d6c578186dab9622ce650f535e11d505efcecb3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "65067609"
---
# <a name="metric-alerts-with-dynamic-thresholds-in-azure-monitor"></a>Avvisi delle metriche con soglie dinamiche in Monitoraggio di Azure

La funzionalità Avviso metrica con rilevamento di soglie dinamiche sfrutta la tecnologia avanzata di Machine Learning (ML) per ottenere informazioni sul comportamento delle metriche nel tempo e identificare modelli e anomalie che indicano possibili problemi del servizio. Offre un'interfaccia utente semplice e supporto per operazioni su larga scala, consentendo agli utenti di configurare regole di avviso tramite l'API di Azure Resource Manager, in modo completamente automatico.

Una volta creata, una regola di avviso viene attivata solo quando la metrica monitorata non si comporta come previsto, in base alle relative soglie personalizzate.

Il feedback degli utenti è bene accetto ed è possibile inviarlo all'indirizzo <azurealertsfeedback@microsoft.com>.

## <a name="why-and-when-is-using-dynamic-condition-type-recommended"></a>Perché e quando è consigliabile usare condizioni di tipo dinamico?

1. **Scalabilità nella generazione di avvisi**: le regole di avviso con soglie dinamiche possono definire soglie personalizzate per centinaia di serie di metriche alla volta, ma con la stessa facilità con cui si definisce una regola di avviso per una singola metrica. Che si usi l'interfaccia utente o l'API di Azure Resource Manager, ciò si traduce in un numero minore di regole di avviso da gestire. L'approccio scalabile è particolarmente utile quando si usano metriche con dimensioni o quando si applicano le metriche a più risorse, ad esempio tutte le risorse della sottoscrizione. Ciò si traduce in un notevole risparmio di tempo nel gestire e creare regole di avviso. [Altre informazioni su come configurare gli avvisi delle metriche con soglie dinamiche usando modelli](alerts-metric-create-templates.md).

1. **Riconoscimento intelligente di modelli di metriche**: usando l'esclusiva tecnologia di Machine Learning offerta da Microsoft, è possibile rilevare automaticamente modelli di metriche e adattare la generazione di avvisi in base alla variazione delle metriche nel tempo, che spesso può avere carattere ricorrente (su base oraria, giornaliera o settimanale). L'adattamento al comportamento delle metriche nel tempo e la generazione di avvisi in base alle deviazioni dal modello evita la necessità di conoscere la soglia "giusta" per ogni metrica. L'algoritmo di Machine Learning usato per le soglie dinamiche è stato progettato in modo da impedire valori soglia non significativi (bassa precisione) o troppo ampi (basso richiamo) che non seguono un modello previsto.

1. **Configurazione intuitiva**: le soglie dinamiche consentono di configurare gli avvisi delle metriche usando concetti generali, riducendo così la necessità di avere informazioni complete sulla metrica a livello di dominio.

## <a name="how-to-configure-alerts-rules-with-dynamic-thresholds"></a>Come configurare le regole di avviso con soglie dinamiche?

Gli avvisi con soglie dinamiche possono essere configurati tramite la funzionalità Avvisi delle metriche in Monitoraggio di Azure. [Altre informazioni su come configurare gli avvisi delle metriche](alerts-metric.md).

## <a name="how-are-the-thresholds-calculated"></a>Come vengono calcolate le soglie?

Le soglie dinamiche apprendono costantemente i dati della serie di metriche e provano a modellarli usando un set di algoritmi e metodi. Rileva modelli nei dati, ad esempio la ricorrenza (su base oraria, giornaliera o settimanale), ed è in grado di gestire le metriche non significative (come la memoria o la CPU del computer) e quelle con bassa dispersione (come la percentuale di disponibilità ed errore).

Le soglie vengono selezionate in modo che un'eventuale deviazione da queste indichi un'anomalia nel comportamento delle metriche.

> [!NOTE]
> Rilevamento di modelli stagionali è impostato su un'ora, giorno o intervallo di settimane. Ciò significa che gli altri modelli bihourly modello like o semiweekly potrebbero non essere rilevati.

## <a name="what-does-sensitivity-setting-in-dynamic-thresholds-mean"></a>A cosa serve l'impostazione "Sensibilità" per le soglie dinamiche?

La sensibilità delle soglie di avviso è un concetto generale che controlla il grado di deviazione dal comportamento della metrica necessario per attivare un avviso.
Questa opzione non richiede una conoscenza della metrica a livello di dominio come la soglia statica. Le opzioni disponibili sono:

- Alta: le soglie saranno estremamente vicine al modello della serie di metriche. Una regola di avviso verrà attivata sulla deviazione minima, generando più avvisi.
- Media: le soglie saranno meno sensibili e più bilanciate, generando così meno avvisi rispetto alla sensibilità alta (impostazione predefinita).
- Bassa: le soglie saranno meno rigorose, con maggiore distanza dal modello della serie di metriche. Una regola di avviso si attiverà solo sulle deviazioni grandi, generando un minor numero di avvisi.

## <a name="what-are-the-operator-setting-options-in-dynamic-thresholds"></a>Quali sono le opzioni dell'impostazione "Operatore" per le soglie dinamiche?

Usando la stessa regola di avviso con soglie dinamiche è possibile creare soglie personalizzate in base al comportamento delle metriche per il limite minimo e quello massimo.
È possibile specificare che l'avviso venga attivato se si verifica una delle tre condizioni seguenti:

- Valore maggiore della soglia superiore o minore della soglia inferiore (impostazione predefinita)
- Valore maggiore della soglia superiore
- Valore minore della soglia inferiore.

## <a name="what-do-the-advanced-settings-in-dynamic-thresholds-mean"></a>A cosa servono le impostazioni avanzate per le soglie dinamiche?

**Periodi di errore**: le soglie dinamiche consentono anche di configurare l'opzione "Number violations to trigger the alert" (Numero di violazioni per attivare l'avviso), ovvero un numero minimo di deviazioni che devono verificarsi entro un determinato intervallo di tempo affinché il sistema generi un avviso (l'intervallo di tempo predefinito è quattro deviazioni in 20 minuti). L'utente può configurare i periodi di errore e scegliere il criterio in base al quale essere avvisato modificando i periodi di errore e l'intervallo di tempo. Questa opzione consente di ridurre la generazione di avvisi non significativi a causa di picchi temporanei. Ad esempio:

Per attivare un avviso quando il problema continua per 20 minuti, 4 volte consecutive con periodicità di 5 minuti, usare le impostazioni seguenti:

![Impostazioni di periodi di errore relativi a un problema che continua per 20 minuti, 4 volte consecutive con periodicità di 5 minuti](media/alerts-dynamic-thresholds/0008.png)

Per attivare un avviso quando si è verificata una violazione di una soglia dinamica per 20 minuti sugli ultimi 30, con periodicità di 5 minuti, usare le impostazioni seguenti:

![Impostazioni di periodi di errore relativi a un problema che si è verificato per 20 minuti sugli ultimi 30, con periodicità di 5 minuti](media/alerts-dynamic-thresholds/0009.png)

**Ignorare i dati precedenti**: gli utenti possono anche definire una data a partire dalla quale il sistema dovrebbe iniziare il calcolo delle soglie. Un tipico caso d'uso può verificarsi quando una risorsa era in esecuzione in modalità di test e ora è stata promossa per gestire un carico di lavoro di produzione, e pertanto il comportamento di qualsiasi metrica durante la fase di test deve essere ignorato.

## <a name="how-do-you-find-out-why-a-dynamic-thresholds-alert-was-triggered"></a>Come scoprire il motivo per cui è stato attivato un avviso di soglie dinamiche?

È possibile esplorare le istanze di avviso attivate nella vista avvisi uno facendo clic sul collegamento nell'indirizzo di posta elettronica o SMS o browser per visualizzare gli avvisi di visualizzare nel portale di Azure. [Altre informazioni sulla visualizzazione degli avvisi](alerts-overview.md#alerts-experience).

Consente di visualizzare la vista avvisi:

- Tutti i dettagli delle metriche nel momento in cui viene generato l'avviso soglie dinamiche.
- Un grafico del periodo in cui è stato generato l'avviso che include le soglie dinamiche utilizzate a questo punto nel tempo.
- Possibilità di fornire commenti e suggerimenti su avviso soglie dinamiche e gli avvisi esperienza di visualizzazione, che potrebbe migliorare i rilevamenti future.

## <a name="will-slow-behavior-change-in-the-metric-trigger-an-alert"></a>Una lenta variazione nel comportamento della metrica attiverà un avviso?

La risposta è probabilmente negativa. Le soglie dinamiche sono utili per rilevare deviazioni significative anziché problemi che si evolvono lentamente.

## <a name="how-much-data-is-used-to-preview-and-then-calculate-thresholds"></a>Quanti dati vengono usati per visualizzare in anteprima le soglie e quindi calcolarle?

Le soglie visualizzati nel grafico, prima che venga creata una regola di avviso sulla metrica, vengono calcolate in base dati cronologici sufficienti per la quale calcolare oraria o giornaliera modelli stagionali (10 giorni). Dopo aver creata una regola di avviso, le soglie dinamiche utilizzerà tutti i dati cronologici necessari che è disponibile e verranno continuamente apprendere e adattarsi in base ai nuovi dati per rendere più accurate le soglie. Ciò significa che dopo il calcolo, il grafico verrà anche visualizzato modelli settimanali.

## <a name="how-much-data-is-needed-to-trigger-an-alert"></a>Quanti dati sono necessarie per attivare un avviso?

Se si dispone di una nuova risorsa o i dati di metrica mancanti, soglie dinamiche non attiva gli avvisi prima di tre giorni di dati sono disponibili per verificare che le soglie accurate.

## <a name="dynamic-thresholds-best-practices"></a>Procedure consigliate per le soglie dinamiche

Le soglie dinamiche possono essere applicate a qualsiasi piattaforma o metrica personalizzata in Monitoraggio di Azure e sono state ottimizzate per le comuni metriche delle applicazioni e delle infrastrutture.
Di seguito sono illustrate le procedure consigliate per configurare gli avvisi per alcune di queste metriche usando le soglie dinamiche.

### <a name="dynamic-thresholds-on-virtual-machine-cpu-percentage-metrics"></a>Soglie dinamiche per le metriche relative alle percentuali di CPU delle macchine virtuali

1. Nel [portale di Azure](https://portal.azure.com) fare clic su **Monitoraggio**. La vista Monitoraggio consolida tutte le impostazioni e i dati di monitoraggio in una vista.

2. Fare clic su **Avvisi** e su **+Nuova regola di avviso**.

    > [!TIP]
    > Anche la maggior parte dei pannelli di risorse include la voce **Avvisi** nel menu delle risorse, in **Monitoraggio**: è possibile creare avvisi anche qui.

3. Fare clic su **Selezionare la destinazione** e, nel riquadro del contesto caricato, selezionare una risorsa di destinazione su cui si vuole impostare una regola di avviso. Usare gli elenchi a discesa **Sottoscrizione** e **Tipo di risorsa "Macchine virtuali"** per trovare la risorsa da monitorare. È anche possibile usare la barra di ricerca per trovare la risorsa.

4. Dopo aver selezionato una risorsa di destinazione, fare clic su **Aggiungi condizione**.

5. Selezionare **"Percentuale CPU"** .

6. Se si vuole, ridefinire la metrica modificando i valori di **Periodo** e **Aggregazione**. Non è consigliabile usare il tipo di aggregazione "Massima" per questo tipo di metrica perché è meno rappresentativo del comportamento. Per il tipo di aggregazione "Massima" può essere più appropriata la soglia statica.

7. Verrà visualizzato un grafico per la metrica per le ultime 6 ore. Definire i parametri dell'avviso:
    1. **Tipo di condizione**: scegliere l'opzione "Dinamica".
    1. **Sensibilità**: scegliere la sensibilità media/bassa per ridurre la frequenza degli avvisi.
    1. **Operatore**: scegliere "Maggiore di" a meno che il comportamento non rappresenti l'utilizzo dell'applicazione.
    1. **Frequenza**: valutare l'opportunità di abbassarla in base all'impatto aziendale dell'avviso.
    1. **Periodi con errori** (opzione avanzata): la finestra di tempo da considerare deve essere di almeno 15 minuti. Se ad esempio il periodo viene impostato su cinque minuti, i periodi con errori devono essere almeno tre.

8. Il grafico delle metriche mostrerà le soglie calcolate in base ai dati recenti.

9. Fare clic su **Done**.

10. Compilare **Dettagli avviso**, ad esempio **Nome regola di avviso**, **Descrizione** e **Gravità**.

11. Aggiungere un gruppo di azioni per l'avviso selezionando un gruppo di azioni esistente o creandone uno nuovo.

12. Fare clic su **Fine** per salvare la regola di avviso per la metrica.

> [!NOTE]
> Le regole di avviso per le metriche create mediante il portale vengono generate nello stesso gruppo di risorse della risorsa di destinazione.

### <a name="dynamic-thresholds-on-application-insights-http-request-execution-time"></a>Soglie dinamiche per il tempo di esecuzione delle richieste HTTP di Application Insights

1. Nel [portale di Azure](https://portal.azure.com) fare clic su **Monitoraggio**. La vista Monitoraggio consolida tutte le impostazioni e i dati di monitoraggio in una vista.

2. Fare clic su **Avvisi** e su **+Nuova regola di avviso**.

    > [!TIP]
    > Anche la maggior parte dei pannelli di risorse include la voce **Avvisi** nel menu delle risorse, in **Monitoraggio**: è possibile creare avvisi anche qui.

3. Fare clic su **Selezionare la destinazione** e, nel riquadro del contesto caricato, selezionare una risorsa di destinazione su cui si vuole impostare una regola di avviso. Usare gli elenchi a discesa **Sottoscrizione** e **Tipo di risorsa "Application Insights"** per trovare la risorsa da monitorare. È anche possibile usare la barra di ricerca per trovare la risorsa.

4. Dopo aver selezionato una risorsa di destinazione, fare clic su **Aggiungi condizione**.

5. Selezionare il **"tempo di esecuzione delle richieste HTTP"** .

6. Se si vuole, ridefinire la metrica modificando i valori di **Periodo** e **Aggregazione**. Non è consigliabile usare il tipo di aggregazione "Massima" per questo tipo di metrica perché è meno rappresentativo del comportamento. Per il tipo di aggregazione "Massima" può essere più appropriata la soglia statica.

7. Verrà visualizzato un grafico per la metrica per le ultime 6 ore. Definire i parametri dell'avviso:
    1. **Tipo di condizione**: scegliere l'opzione "Dinamica".
    1. **Operatore**: scegliere "Maggiore di" per ridurre gli avvisi generati in caso di miglioramento della durata.
    1. **Frequenza**: valutare l'opportunità di abbassarla in base all'impatto aziendale dell'avviso.

8. Il grafico delle metriche mostrerà le soglie calcolate in base ai dati recenti.

9. Fare clic su **Done**.

10. Compilare **Dettagli avviso**, ad esempio **Nome regola di avviso**, **Descrizione** e **Gravità**.

11. Aggiungere un gruppo di azioni per l'avviso selezionando un gruppo di azioni esistente o creandone uno nuovo.

12. Fare clic su **Fine** per salvare la regola di avviso per la metrica.

> [!NOTE]
> Le regole di avviso per le metriche create mediante il portale vengono generate nello stesso gruppo di risorse della risorsa di destinazione.
