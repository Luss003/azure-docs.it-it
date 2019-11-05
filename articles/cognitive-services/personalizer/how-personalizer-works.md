---
title: Funzionamento di Personalizza esperienze - Personalizza esperienze
titleSuffix: Azure Cognitive Services
description: Personalizza esperienze usa modelli di Machine Learning per individuare quale azione usare in un contesto. Ogni ciclo di apprendimento include un modello sottoposto a training esclusivamente con i dati inviati tramite chiamate a Classifica e a Ricompensa. Ogni ciclo di apprendimento è completamente indipendente rispetto agli altri.
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: diberry
ms.openlocfilehash: 902bf84ebf090cf9f0f886ad1e774ff7bdfeca93
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/04/2019
ms.locfileid: "73490743"
---
# <a name="how-personalizer-works"></a>Funzionamento di Personalizza esperienze

Personalizza esperienze usa modelli di Machine Learning per individuare quale azione usare in un contesto. Ogni ciclo di apprendimento include un modello sottoposto a training esclusivamente con i dati inviati tramite chiamate a **Classifica** e a **Ricompensa**. Ogni ciclo di apprendimento è completamente indipendente rispetto agli altri. Creare un ciclo di apprendimento per ogni parte o comportamento dell'applicazione da personalizzare.

Per ogni ciclo, **chiamare l'API Classifica** in base al contesto corrente con:

* Elenco di possibili azioni: contenuti da cui selezionare l'azione principale.
* Elenco di [caratteristiche del contesto](concepts-features.md): dati contestualmente pertinenti come utente, contenuto e contesto.

L'API **Classifica** decide di usare:

* _Exploit_: modello corrente per decidere l'azione migliore in base ai dati passati.
* _Esplora_: selezionare un'azione diversa anziché l'azione principale.

l'API **Ricompensa**:

* Raccoglie i dati per eseguire il training del modello registrando le caratteristiche e i punteggi di ricompensa di ogni chiamata a Classifica.
* USA i dati per aggiornare il modello in base alla configurazione specificata nei _criteri di apprendimento_.

## <a name="architecture"></a>Architettura

L'immagine seguente mostra il flusso architetturale delle chiamate a Classifica e Ricompensa:

![testo alternativo](./media/how-personalizer-works/personalization-how-it-works.png "Funzionamento della personalizzazione")

1. Personalizza esperienze usa un modello interno di intelligenza artificiale per determinare la posizione in classifica dell'azione.
1. Il servizio decide se sfruttare il modello corrente o esplorare nuove scelte.  
1. Il risultato della classificazione viene inviato all'hub eventi.
1. Quando Personalizza esperienze riceve la ricompensa, la invia all'hub eventi. 
1. La classifica e la ricompensa sono correlate.
1. Il modello di intelligenza artificiale viene aggiornato in base ai risultati della correlazione.
1. Il motore di inferenza viene aggiornato con il nuovo modello. 

## <a name="research-behind-personalizer"></a>Ricerca alla base di Personalizza esperienze

Personalizza esperienze si basa su dati scientifici e ricerche nel campo dell'[apprendimento per rinforzo](concepts-reinforcement-learning.md), tra cui documenti, attività di ricerca e aree di studio in corso in Microsoft Research.

## <a name="terminology"></a>Terminologia

* **Learning loop**: è possibile creare un ciclo di apprendimento per ogni parte dell'applicazione che può trarre vantaggio dalla personalizzazione. Nel caso di più esperienze da personalizzare, creare un ciclo per ognuna. 

* **Azioni**: le azioni sono elementi di contenuto, ad esempio prodotti o promozioni, tra cui scegliere. Personalizza esperienze sceglie l'azione principale da mostrare agli utenti, la cosiddetta _azione ricompensa_, tramite l'API Classifica. Per ogni azione è possibile inviare caratteristiche con la richiesta Classifica.

* **Contesto**: per fornire un rango più preciso, fornire informazioni sul contesto, ad esempio:
    * L'utente.
    * Il dispositivo che usa. 
    * Ora corrente.
    * Altri dati sulla situazione corrente.
    * Dati cronologici sull'utente o sul contesto.

    Le specifiche applicazioni possono avere informazioni diverse sul contesto. 

* **[Funzionalità](concepts-features.md)** : unità di informazioni relative a un elemento di contenuto o a un contesto utente.

* **Reward**: una misura della risposta dell'utente all'API Rank restituita, come un punteggio compreso tra 0 e 1. Il valore da 0 a 1 viene impostato dalla logica di business in base all'efficacia della scelta per realizzare gli obiettivi aziendali della personalizzazione. 

* **Esplorazione**: il servizio di personalizzazione sta esplorando quando, anziché restituire l'azione migliore, sceglie un'azione diversa per l'utente. Il servizio Personalizza esperienze evita scenari di deriva e di stallo e può adattarsi al comportamento in corso dell'utente tramite esplorazione. 

* **Durata dell'esperimento**: periodo di tempo durante il quale il servizio di personalizzazione resta in attesa di una ricompensa, a partire dal momento in cui si è verificata la chiamata di rango per quell'evento.

* **Eventi inattivi**: un evento inattivo è quello in cui è stato chiamato Rank, ma non si è certi che l'utente visualizzerà mai il risultato, a causa delle decisioni relative alle applicazioni client. Gli eventi inattivi consentono di creare e archiviare i risultati della personalizzazione e quindi di decidere di rimuoverli in seguito senza influire sul modello di Machine Learning.

* **Modello**: un modello di personalizzatore acquisisce tutti i dati appresi sul comportamento dell'utente, recuperando i dati di training dalla combinazione degli argomenti inviati a chiamate di rango e premi e con un comportamento di training determinato dai criteri di apprendimento. 

* **Criteri di apprendimento**: il modo in cui la personalizzazione del training di un modello su ogni evento verrà determinato da alcuni metadati che influiscono sul funzionamento degli algoritmi di machine learning. I nuovi cicli di personalizzazione inizieranno con criteri di apprendimento predefiniti che possono produrre prestazioni moderate. Quando si eseguono [valutazioni](concepts-offline-evaluation.md), il Personalizzatore può creare nuovi criteri di apprendimento specificamente ottimizzati per i casi d'uso del ciclo. Il Personalizzatore offre prestazioni significativamente migliori con i criteri ottimizzati per ogni ciclo specifico, generato durante la valutazione.

## <a name="example-use-cases-for-personalizer"></a>Casi d'uso di esempio per Personalizza esperienze

* Chiarimento e disambiguazione della finalità: offrire agli utenti un'esperienza migliore quando non hanno una chiara finalità, fornendo un'opzione personalizzata per ogni utente.
* Suggerimenti predefiniti per menu e opzioni: far sì che il bot suggerisca, come primo passaggio, l'elemento più probabile in modo personalizzato, invece di presentare un menu impersonale o un elenco di alternative.
* Tratti e tono del bot: per i bot che possono variare tono, livello di dettaglio e stile di scrittura, valutare l'opportunità di variare questi tratti in modo personalizzato.
* Contenuto di notifiche e avvisi: definire il testo da usare per gli avvisi in modo da coinvolgere maggiormente gli utenti.
* Tempistica di notifiche e avvisi: individuare in modo personalizzato il momento opportuno in cui inviare le notifiche per un maggiore coinvolgimento degli utenti.

## <a name="how-to-use-personalizer-in-a-web-application"></a>Come usare Personalizza esperienze in un'applicazione Web

L'aggiunta di un ciclo a un'applicazione Web include le operazioni seguenti:

* Determinare l'esperienza da personalizzare, le azioni e le funzionalità disponibili, le caratteristiche del contesto da usare e la ricompensa da impostare.
* Aggiungere nell'applicazione un riferimento all'SDK per la personalizzazione.
* Chiamare l'API per la classificazione quando si è pronti ad avviare la personalizzazione.
* Memorizzare l'ID dell'evento. La ricompensa tramite l'apposita API viene inviata in un secondo momento.
1. Chiamare la funzione di attivazione per l'evento quando si è certi che l'utente ha visualizzato la pagina personalizzata.
1. Attendere che l'utente esegua la selezione del contenuto classificato. 
1. Chiamare l'API per la ricompensa per specificare l'efficacia dell'output dell'API per la classificazione.

## <a name="how-to-use-personalizer-with-a-chat-bot"></a>Come usare Personalizza esperienze con un chatbot

In questo esempio si vedrà come usare Personalizza esperienze in modo da inviare un suggerimento predefinito anziché inviare ogni volta all'utente una serie di opzioni o menu.

* Scaricare il [codice](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/tree/master/samples/ChatbotExample) per questo esempio.
* Configurare la soluzione del bot. Assicurarsi di pubblicare l'applicazione LUIS. 
* Gestire per il bot le chiamate alle API per la classificazione e la ricompensa.
    * Aggiungere codice per gestire l'elaborazione della finalità di LUIS. Se come finalità principale viene restituito **None** o il punteggio della finalità risulta al di sotto della soglia della logica di business, inviare l'elenco delle finalità a Personalizza esperienze per classificarle.
    * Mostrare all'utente l'elenco delle finalità come collegamenti selezionabili, dove la prima finalità corrisponde a quella classificata con priorità più alta dalla risposta dell'API.
    * Acquisire la selezione dell'utente e includerla nella chiamata all'API per la ricompensa. 

### <a name="recommended-bot-patterns"></a>Modelli consigliati per il bot

* Eseguire chiamate all'API per la classificazione di Personalizza esperienze ogni volta che è necessaria una disambiguazione, invece di memorizzare nella cache i risultati per ogni utente. Il risultato della disambiguazione della finalità può cambiare nel tempo per una stessa persona. Consentendo all'API per la classificazione di esplorare le varianze si accelererà l'apprendimento complessivo.
* Scegliere un'interazione comune a molti utenti in modo da avere dati sufficienti per la personalizzazione. Ad esempio, le domande introduttive possono essere più appropriate rispetto ai piccoli chiarimenti introdotti più avanti nella conversazione, che potrebbero essere visualizzati solo da alcuni utenti.
* Usare chiamate all'API per la classificazione per abilitare conversazioni di tipo "il primo suggerimento è quello giusto", in cui all'utente viene chiesto "Vuoi X?" o "Intendi dire X?" e l'utente può solo confermare, anziché avere la possibilità di scegliere più opzioni da un menu. Ad esempio, conversazioni in cui l'utente chiede:"Vorrei ordinare un caffè" e il bot risponde:"Vuoi un espresso doppio?". In questo modo, anche il segnale di ricompensa è forte poiché fa direttamente riferimento all'unico suggerimento fornito.

## <a name="how-to-use-personalizer-with-a-recommendation-solution"></a>Come usare Personalizza esperienze con una soluzione di raccomandazione

Usare il motore di raccomandazione per filtrare un ampio catalogo fino a ottenere pochi elementi che possono quindi essere presentati come 30 possibili azioni inviate all'API per la classificazione.

È possibile usare motori di raccomandazione con Personalizza esperienze:

* Configurare la [soluzione di raccomandazione](https://github.com/Microsoft/Recommenders/). 
* Quando viene visualizzata una pagina, richiamare il modello di raccomandazione per ottenere un breve elenco di raccomandazioni.
* Chiamare la funzione di personalizzazione per classificare l'output della soluzione di raccomandazione.
* Inviare il feedback sull'azione dell'utente eseguendo una chiamata all'API per la ricompensa.


## <a name="pitfalls-to-avoid"></a>Errori da evitare

* Non usare Personalizza esperienze quando il comportamento personalizzato non è un comportamento individuabile per tutti gli utenti, ma riguarda utenti specifici o proviene da un elenco di alternative specifico di un utente. Può ad esempio essere utile usare Personalizza esperienze per suggerire la pizza da ordinare tra un elenco di 20 possibili tipi, ma la scelta del contatto da chiamare quando si ha bisogno di aiuto per i bambini (come "Nonna") non è un'opzione personalizzabile per un'intera base di utenti.


## <a name="adding-content-safeguards-to-your-application"></a>Aggiungere all'applicazione misure di sicurezza per il contenuto

Se l'applicazione consente la visualizzazione di ampie varianze di contenuto e parte di tale contenuto può essere poco sicuro o risultare inappropriato per alcuni utenti, è consigliabile controllare in anticipo che vengano implementate le opportune misure di sicurezza per impedire agli utenti di visualizzare contenuto inaccettabile. Il modello migliore per implementare le misure di sicurezza è:
    * Ottenere l'elenco delle azioni da classificare.
    * Escludere quelle che non sono attuabili per i destinatari.
    * Classificare solo le azioni attuabili.
    * Mostrare all'utente l'azione con la priorità più alta.

In alcune architetture la sequenza precedente può risultare difficile da implementare. In tal caso, esiste un approccio alternativo all'implementazione delle misure di sicurezza dopo la classificazione, ma è necessario fare in modo che le azioni esterne alla misura di sicurezza non vengano usate per il training del modello di Personalizza esperienze.

* Ottenere l'elenco delle azioni da classificare, con apprendimento disattivato.
* Classificare le azioni in ordine di priorità.
* Controllare se l'azione con priorità più alta è attuabile.
    * In caso affermativo, attivare l'apprendimento per questo livello di classificazione e quindi mostrarlo all'utente.
    * Se l'azione con priorità più alta non è attuabile, non attivare l'apprendimento per questa classificazione e decidere cosa mostrare all'utente in base alla propria logica o ad approcci alternativi. Anche se si usa la seconda opzione migliore, non attivare l'apprendimento per questa classificazione.

## <a name="verifying-adequate-effectiveness-of-personalizer"></a>Verifica dell'efficacia di Personalizza esperienze

È possibile monitorare periodicamente l'efficacia di Personalizza esperienze eseguendo [valutazioni offline](how-to-offline-evaluation.md)

## <a name="next-steps"></a>Passaggi successivi

Informazioni sui [casi in cui è possibile usare Personalizza esperienze](where-can-you-use-personalizer.md).
Eseguire [valutazioni offline](how-to-offline-evaluation.md)
