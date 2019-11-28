---
title: Comprendere la fattura di Azure
description: Informazioni su come leggere e comprendere l'utilizzo e la fattura per la sottoscrizione di Azure.
author: bandersmsft
manager: dougeby
tags: billing
ms.service: cost-management-billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: 9486d56a723bb311c05ab7aa776060dfa9561aae
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/20/2019
ms.locfileid: "74223047"
---
# <a name="understand-your-microsoft-azure-bill"></a>Informazioni sulla fattura di Microsoft Azure
Per comprendere la fattura di Azure, confrontarla con il file dei dettagli dell'utilizzo giornaliero e con i report della gestione dei costi nel portale di Azure.

Questo articolo non si applica ai clienti seguenti:
- Clienti di Azure con contratto Enterprise Agreement (clienti EA). Se si è un cliente EA, vedere [Informazioni sulla fattura per i clienti di Azure con contratto Enterprise Agreement](billing-understand-your-bill-ea.md).
- Clienti di Azure con [Contratto del cliente Microsoft](#check-access-to-a-microsoft-customer-agreement). Se si ha un Contratto del cliente Microsoft, vedere [Informazioni sugli addebiti di Azure nella fattura del Contratto del cliente Microsoft](billing-mca-understand-your-bill.md).

Per una spiegazione sul funzionamento della fatturazione nel programma Azure Cloud Solution Provider (Azure CSP), inclusi ciclo di fatturazione, prezzi e utilizzo, vedere [Panoramica della fatturazione di Azure CSP](/azure/cloud-solution-provider/billing/azure-csp-billing-overview/).

## <a name="charges"></a>Esaminare gli addebiti

>[!VIDEO https://www.youtube.com/embed/3YegFD769Pk]

Se viene rilevato un addebito in fattura e sono necessarie altre informazioni, è possibile confrontare i valori di utilizzo e costi con il file con i dettagli di utilizzo o con il portale di Azure.

### <a name="option-1-compare-usage-and-costs-with-usage-file"></a>Opzione 1: confrontare utilizzo e costi con il file con i dettagli di utilizzo

Il file con i dettagli di utilizzo in formato CSV illustra gli addebiti per periodo di fatturazione e utilizzo giornaliero. Per scaricare o visualizzare il file, vedere [Ottenere la fattura e i dati di utilizzo giornalieri di Azure](billing-download-azure-invoice-daily-usage-date.md).

Gli addebiti relativi all'utilizzo vengono visualizzati a livello di contatore. I termini seguenti hanno lo stesso significato sia nella fattura che nel file con i dettagli di utilizzo. Ad esempio, il ciclo di fatturazione indicato nella fattura corrisponde al periodo di fatturazione indicato nel file con i dettagli di utilizzo.

 | Fattura (PDF) | Dettagli di utilizzo (CSV)|
 | --- | --- |
|Ciclo di fatturazione | Periodo di fatturazione |
 |NOME |Categoria misuratore |
 |Type |Sottocategoria contatore |
 |Risorsa |Nome misuratore |
 |Region |Area misuratore |
 |Consumato |Quantità consumata |
 |Incluso |Quantità inclusa |
 |Fatturabile |Quantità in eccesso |

La sezione **Costi di utilizzo** della fattura mostra il valore totale per ogni contatore utilizzato durante il periodo di fatturazione. L'immagine seguente illustra ad esempio l'addebito per l'utilizzo del servizio Utilità di pianificazione di Azure.

![Addebiti di utilizzo nella fattura](./media/billing-understand-your-bill/1.png)

La sezione **Statement** (Rendiconto) del file CSV con i dettagli di utilizzo indica lo stesso addebito. Sia la quantità indicata in *Consumato* che *Valore* hanno una corrispondenza nella fattura.

![Addebiti di utilizzo nel file CSV](./media/billing-understand-your-bill/2.png)

Per visualizzare una scomposizione giornaliera di questo addebito, andare alla sezione **Utilizzo giornaliero** del file CSV. Filtrare *Utilità di pianificazione* in *Categoria misuratore*. È possibile visualizzare i giorni in cui il misuratore è stato usato e il relativo consumo. Vengono riportate anche le informazioni *Risorsa* e *Gruppo di risorse* a scopo di confronto. La somma dei valori in *Utilizzato* corrisponderà a quanto indicato nella fattura.

![Sezione Utilizzo giornaliero nel CSV](./media/billing-understand-your-bill/3.png)

Per ottenere il costo giornaliero, moltiplicare le quantità in *Consumato* con il valore *Tariffa* della sezione **Statement** (Rendiconto).

Per altre informazioni, vedere:

- [Comprendere la fattura di Azure](billing-understand-your-invoice.md)
- [Comprendere il file di utilizzo dettagliato di Azure](billing-understand-your-invoice.md)

### <a name="option-2-compare-the-usage-and-costs-in-the-azure-portal"></a>Opzione 2: confrontare utilizzo e costi nel portale di Azure

Il Portale di Azure consente anche di verificare gli addebiti. Per una rapida panoramica dell'utilizzo e degli addebiti fatturati, visualizzare i grafici di gestione dei costi.

1. Passare a [Sottoscrizioni](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) nel portale di Azure.
1. Selezionare la sottoscrizione e quindi **Analisi dei costi**.
1. Filtrare in base al **Timespan**.
1. Per continuare l'esempio precedente, verrà visualizzato un addebito di utilizzo per il servizio Utilità di pianificazione di Azure.

   ![Visualizzazione dell'analisi dei costi nel Portale di Azure](./media/billing-understand-your-bill/4.png)

1. Selezionare la riga contenente l'addebito per visualizzare la scomposizione giornaliera del costo.

   ![Visualizzazione Cronologia dei costi nel portale di Azure](./media/billing-understand-your-bill/5.png)

Per altre informazioni, vedere [Evitare costi imprevisti con la gestione dei costi e alla fatturazione di Azure](billing-getting-started.md#costs).

## <a name="external"></a>Servizi esterni fatturati separatamente

I servizi esterni o gli addebiti per Marketplace riguardano le risorse create da fornitori di software di terze parti. Tali risorse sono disponibili per l'uso in Azure Marketplace. Un firewall Barracuda è ad esempio una risorsa di Azure Marketplace offerta da terze parti. Tutti gli addebiti per il firewall e i relativi contatori corrispondenti verranno visualizzati come addebiti per servizi esterni.

Gli addebiti per i servizi esterni vengono fatturati separatamente. Questi addebiti non compaiono nella fattura di Azure. Per altre informazioni, vedere [Informazioni sugli addebiti per i servizi esterni](billing-understand-your-azure-marketplace-charges.md).

## <a name="resources-billed-by-usage-meters"></a>Risorse fatturate in base a contatori di utilizzo

Azure non fattura direttamente in base al costo della risorsa. Gli addebiti relativi a una risorsa vengono calcolati con uno o più contatori, che vengono usati per tenere traccia dell'utilizzo di una risorsa per tutta la sua durata. Questi contatori vengono quindi usati per calcolare la fattura.

Quando, ad esempio, si crea una singola risorsa di Azure, come una macchina virtuale, vengono create una o più istanze del contatore. I contatori vengono usati per tenere traccia dell'utilizzo della risorsa nel tempo. Ogni contatore genera record di utilizzo che vengono usati per elaborare la fattura.

Ad esempio, in una singola macchina virtuale creata in Azure possono essere creati i contatori seguenti per tenere traccia dell'utilizzo:

- Ore di calcolo
- Ore indirizzo IP
- Importazione dati
- Trasferimento dati in uscita
- Managed Disks Standard
- Operazioni disco gestito Standard
- I/O Standard - Disco
- I/O Standard - Lettura BLOB in blocchi
- I/O Standard - Scrittura BLOB in blocchi
- I/O Standard - Eliminazione BLOB in blocchi

Dopo la creazione della VM, ogni contatore inizia a emettere record di utilizzo. L'utilizzo e il costo indicato dal contantore vengono registrati nel sistema di misurazione di Azure.

## <a name="payment"></a>Pagare la fattura

Se si configura una carta di credito come metodo di pagamento, il pagamento viene addebitato automaticamente entro 10 giorni dalla fine del periodo di fatturazione. Nell'estratto conto della carta di credito la voce sarà **MSFT Azure**.

Per modificare la carta di credito su cui viene effettuato l'addebito, vedere [Aggiungere, aggiornare o rimuovere una carta di credito per Azure](billing-how-to-change-credit-card.md).

Se si [paga tramite fattura](billing-how-to-pay-by-invoice.md), inviare il pagamento al destinatario elencato nella parte inferiore della fattura.

Per controllare lo stato del pagamento, [creare un ticket di supporto](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).


## <a name="tips-for-cost-management"></a>Suggerimenti per la gestione dei costi

- Stimare i costi usando:
  - [Calcolatore prezzi di Azure](https://azure.microsoft.com/pricing/calculator/)
  - [Calcolatore del costo totale di proprietà](https://aka.ms/azure-tco-calculator)
  - [Informazioni dettagliate sui prezzi di ogni servizio](https://azure.microsoft.com/pricing/)
- [Esaminare regolarmente l'utilizzo e i costi nel portale di Azure](billing-getting-started.md#costs)

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Verificare l'accesso a un Contratto del cliente Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Richiesta di assistenza Contattaci.

In caso di domande o per assistenza, [creare una richiesta di supporto](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="learn-more"></a>Altre informazioni

- [Come ottenere la fattura e i dati di uso giornalieri di Azure](billing-download-azure-invoice-daily-usage-date.md)
- [Comprendere i termini sulla fattura di Microsoft Azure](billing-understand-your-invoice.md)
- [Comprendere i termini sui dati di utilizzo dettagliato di Microsoft Azure](billing-understand-your-usage.md)
- [Gestione dei costi del portale di Azure](https://docs.microsoft.com/azure/billing/billing-getting-started)
- [Evitare costi imprevisti con la gestione dei costi e alla fatturazione di Azure](billing-getting-started.md#costs)
