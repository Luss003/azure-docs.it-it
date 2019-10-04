---
title: Gestire e richiedere quote per risorse
titleSuffix: Azure Machine Learning
description: Questa guida pratica presenta le diverse quote per le risorse per Azure Machine Learning e come visualizzare e richiedere quote maggiori.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nishankgu
ms.author: nigup
ms.date: 05/10/2019
ms.custom: seodec18
ms.openlocfilehash: 2ee819625f772c96ac6c5a771935da17ee05f3aa
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/15/2019
ms.locfileid: "71002680"
---
# <a name="manage-and-request-quotas-for-azure-resources"></a>Gestire e richiedere quote per risorse di Azure

Come per gli altri servizi di Azure, esistono limiti per determinate risorse associate a Azure Machine Learning. Questi limiti variano da un limite al numero di aree di lavoro che è possibile creare per limitare l'effettivo calcolo sottostante che viene usato per il training del modello o l'inferenza/assegnazione dei punteggi. 

Questo articolo offre maggiori dettagli sui limiti preconfigurati per diverse risorse di Azure per la sottoscrizione e contiene anche alcuni collegamenti utili per richiedere miglioramenti relativi alle quote per ogni tipo di risorsa. Questi limiti vengono applicati per evitare lo sforamento del budget a causa di illeciti e per rispettare i vincoli di capacità di Azure.

Tenere presenti queste quote durante la progettazione e la scalabilità delle risorse Azure Machine Learning per i carichi di lavoro di produzione. Se, ad esempio, il cluster non raggiunge il numero di nodi specificato, è possibile che sia stato raggiunto un limite di core di calcolo Azure Machine Learning per la sottoscrizione. Per aumentare il limite o la quota oltre il valore del limite predefinito, è possibile aprire una richiesta di assistenza clienti online senza alcun addebito. Non è possibile aumentare i limiti oltre il valore massimo indicato nelle tabelle seguenti a causa dei vincoli di capacità di Azure. Se non è presente una colonna Limite massimo, la risorsa specificata non è associata a limiti modificabili.

## <a name="special-considerations"></a>Considerazioni speciali

+ Una quota è un limite di credito, non una garanzia di capacità. Se si hanno esigenze di capacità su larga scala, contattare il supporto di Azure.

+ La quota è condivisa tra tutti i servizi nelle sottoscrizioni, tra cui Azure Machine Learning. L'unica eccezione è data dall'ambiente di calcolo di Azure Machine Learning che ha una quota separata rispetto alla quota dei core. Nel valutare le esigenze di capacità, assicurarsi di calcolare l'utilizzo della quota in tutti i servizi.

+ I limiti predefiniti variano in base al tipo di categoria dell'offerta, ad esempio Versione di valutazione gratuita o Pagamento in base al consumo, e alla serie, ad esempio Dv2, F, G e così via.

## <a name="default-resource-quotas"></a>Quote predefinite per le risorse

Ecco un riepilogo dei limiti di quota per i vari tipi di risorsa all'interno della sottoscrizione di Azure.

> [!Important]
> I limiti sono soggetti a modifiche. I limiti più aggiornati sono sempre disponibili nel [documento](https://docs.microsoft.com/azure/azure-subscription-service-limits/) sulle quote a livello di servizio per tutti i servizi di Azure.

### <a name="virtual-machines"></a>Macchine virtuali
È previsto un limite sul numero di macchine virtuali di cui è possibile effettuare il provisioning in una sottoscrizione di Azure tra i servizi o in modo autonomo. Questo limite è a livello di area sia per i core totali sia per ogni famiglia.

È importante sottolineare che i core delle macchine virtuali devono rispettare un limite totale per l'area e un limite di serie per dimensione (Dv2, F e così via) per l'area, applicati separatamente. Si consideri ad esempio una sottoscrizione con limite di core di VM totale per gli Stati Uniti orientali pari a 30, un limite di core di serie A pari a 30 e un limite di core di serie D pari a 30. Questa sottoscrizione sarà autorizzata a distribuire 30 VM A1 o 30 VM D1 o una combinazione delle due che non superi un totale di 30 core, ad esempio 10 VM A1 e 20 VM D1.

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../../../includes/azure-subscription-limits-azure-resource-manager.md)]

Per un elenco più dettagliato e aggiornato dei limiti di quota, vedere l'articolo sulle quote a livello di Azure [qui](https://docs.microsoft.com/azure/azure-subscription-service-limits).

### <a name="azure-machine-learning-compute"></a>Ambiente di calcolo di Azure Machine Learning
Nell'ambiente di calcolo di Azure Machine Learning è previsto un limite di quota predefinito per il numero di core e per il numero di risorse di calcolo consentite per ogni area in una sottoscrizione. Questa quota è separata da quella dei core delle macchine virtuali e i limiti relativi ai core non sono attualmente condivisi tra i due tipi di risorse.

Risorse disponibili:
+ I core dedicati per area hanno un limite predefinito di 24-300 a seconda del tipo di offerta di sottoscrizione.  Il numero di core dedicati per ogni sottoscrizione può essere aumentato. Contattare il supporto di Azure per ottenere informazioni sulle opzioni di aumento.

+ I core a bassa priorità per area hanno un limite predefinito di 24-300 a seconda del tipo di offerta di sottoscrizione.  Il numero di core a bassa priorità per ogni sottoscrizione può essere aumentato. Contattare il supporto di Azure per ottenere informazioni sulle opzioni di aumento.

+ I cluster per ogni area hanno un limite predefinito di 100 e un limite massimo di 200. Contattare il supporto di Azure se si vuole richiedere un aumento oltre questo limite.

+ Vi sono **altri limiti rigidi** che non possono essere superati una volta raggiunti.

| **Risorsa** | **Limite massimo** |
| --- | --- |
| Aree di lavoro massime per gruppo di risorse | 800 |
| Numero massimo di nodi in una singola risorsa dell'ambiente di calcolo di Azure Machine Learning (AmlCompute) | 100 nodi |
| Processi MPI GPU massimi per nodo | 1-4 |
| Numero massimo di ruoli di lavoro GPU per nodo | 1-4 |
| Durata massima del processo | 90 giorni<sup>1</sup> |
| Durata massima del processo in un nodo con priorità bassa | 1 giorno<sup>2</sup> |
| Parametri massimi dei server per nodo | 1 |

<sup>1</sup> La durata massima equivale al tempo che trascorre tra l'inizio e il termine dell'esecuzione di un processo. Le esecuzioni completate persistono per un tempo illimitato. I dati relativi alle esecuzioni non completate entro la durata massima non sono accessibili.
<sup>2</sup> i processi in un nodo con priorità bassa possono essere pre-annullata ogni volta che è presente un vincolo di capacità. È consigliabile implementare il checkpoint nel processo.

### <a name="azure-machine-learning-pipelines"></a>Azure Machine Learning Pipelines
Per Azure Machine Learning pipeline, esiste un limite di quota per il numero di passaggi in una pipeline e per il numero di esecuzioni basate su pianificazione di pipeline pubblicate per area in una sottoscrizione.
- Il numero massimo di passaggi consentiti in una pipeline è 30.000
- Il numero massimo della somma delle esecuzioni basate su pianificazione e dei pull di BLOB per le pianificazioni attivate da Blog di pipeline pubblicate per ogni sottoscrizione al mese è 100.000

> [!NOTE]
> Per aumentare questo limite, contattare il [supporto tecnico Microsoft](https://azure.microsoft.com/support/options/).

### <a name="container-instances"></a>Istanze di Container

È anche previsto un limite sul numero di istanze di contenitore che è possibile attivare in un determinato periodo di tempo (su base oraria con ambito) o nell'intera sottoscrizione.

[!INCLUDE [container-instances-limits](../../../includes/container-instances-limits.md)]

Per un elenco più dettagliato e aggiornato dei limiti di quota, vedere l'articolo sulle quote a livello di Azure [qui](https://docs.microsoft.com/azure/azure-subscription-service-limits#container-instances-limits).

### <a name="storage"></a>Archiviazione
È previsto un limite sul numero di account di archiviazione per ogni area, nonché in una sottoscrizione specifica. Il limite predefinito è 200 e include sia l'account di archiviazione Standard sia l'account di archiviazione Premium. Se sono necessari più di 200 account di archiviazione in una determinata area, inviare una richiesta tramite il [supporto tecnico di Azure](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/). Il team di Archiviazione di Azure esaminerà il caso aziendale e potrà approvare fino a un massimo di 250 account di archiviazione per una determinata area.


## <a name="find-your-quotas"></a>Individuazione delle quote

La visualizzazione delle quote per le diverse risorse, come Macchine virtuali, Archiviazione e Rete, è facile tramite il portale di Azure.

1. Nel riquadro a sinistra selezionare **Tutti i servizi** e quindi **Sottoscrizioni** nella categoria Generale.

1. Nell'elenco delle sottoscrizioni selezionare la sottoscrizione per cui si vuole individuare la quota.

   **Tenere conto dell'avvertenza** che riguarda la visualizzazione della quota dell'ambiente di calcolo di Azure Machine Learning. Come indicato in precedenza, questa quota è separata dalla quota di calcolo nella sottoscrizione.

1. Nel riquadro sinistro selezionare **Machine Learning servizio** e quindi selezionare un'area di lavoro dall'elenco visualizzato.

1. Nel pannello successivo, nella sezione **Supporto e risoluzione dei problemi**, selezionare **Utilizzo e quote** per visualizzare i limiti di quota correnti e l'utilizzo.

1. Selezionare una sottoscrizione per visualizzare i limiti di quota. Ricordare di filtrare i dati in base all'area a cui si è interessati.


## <a name="request-quota-increases"></a>Richiedere aumenti di quota

Per aumentare il limite o la quota oltre il valore del limite predefinito, è possibile [aprire una richiesta di assistenza clienti online](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/) senza alcun addebito.

I limiti non possono essere aumentati oltre il valore del limite massimo indicato nelle tabelle. Se non è indicato alcun limite massimo, la risorsa non è associata a limiti modificabili. [Questo](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quota-errors) articolo descrive il processo di aumento della quota in modo più dettagliato.

Quando si richiede un aumento di quota, è necessario selezionare il servizio per cui si richiede l'aumento, che può essere Machine Learning, Istanze di Container o Archiviazione. Con l'ambiente di calcolo di Azure Machine Learning è anche possibile fare semplicemente clic sul pulsante **Richiedi quota** quando è visualizzata la quota seguendo i passaggi precedenti.

> [!NOTE]
> Le [sottoscrizioni per la versione di valutazione gratuita](https://azure.microsoft.com/offers/ms-azr-0044p) non hanno i requisiti necessari per un aumento dei limiti o delle quote. Se si ha una [sottoscrizione per la versione di valutazione gratuita](https://azure.microsoft.com/offers/ms-azr-0044p), è possibile eseguire l'aggiornamento a una sottoscrizione con [pagamento in base al consumo](https://azure.microsoft.com/offers/ms-azr-0003p/). Per ulteriori informazioni, vedere [Aggiornare la versione di valutazione gratuita alla sottoscrizione con pagamento in base al consumo](../../billing/billing-upgrade-azure-subscription.md) e [Domande frequenti sulla sottoscrizione per la versione di valutazione gratuita](https://azure.microsoft.com/free/free-account-faq).
