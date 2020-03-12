---
title: Funzioni JavaScript definite dall'utente in Analisi di flusso di Azure
description: In questa esercitazione si eseguono meccanismi di query avanzate con funzioni JavaScript definite dall'utente
author: rodrigoamicrosoft
ms.author: rodrigoa
ms.service: stream-analytics
ms.topic: tutorial
ms.reviewer: mamccrea
ms.custom: mvc
ms.date: 04/01/2018
ms.openlocfilehash: feb0361b460f5b18b5a8aaa585332e2179023458
ms.sourcegitcommit: f5e4d0466b417fa511b942fd3bd206aeae0055bc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78851173"
---
# <a name="tutorial-azure-stream-analytics-javascript-user-defined-functions"></a>Esercitazione: Funzioni JavaScript definite dall'utente in Analisi di flusso di Azure
 
L'Analisi di flusso di Azure supporta le funzioni definite dall'utente nel linguaggio JavaScript. Con il vasto set di metodi **String**, **RegExp**, **Math**, **Array** e **Date** offerti da JavaScript, risulta più facile creare trasformazioni di dati complessi con processi di Analisi di flusso.

In questa esercitazione verranno illustrate le procedure per:

> [!div class="checklist"]
> * Definire una funzione JavaScript definita dall'utente
> * Aggiungere la funzione al portale
> * Definire una query che esegue la funzione

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.

## <a name="javascript-user-defined-functions"></a>Funzioni JavaScript definite dall'utente
Le funzioni JavaScript definite dall'utente supportano funzioni senza stato e di solo calcolo che non richiedono connettività esterna. Il valore restituito di una funzione può essere solo un valore scalare singolo. Dopo aver aggiunto una funzione JavaScript definita dall'utente a un processo, è possibile utilizzare la funzione in un punto qualsiasi nella query, ad esempio una funzione scalare incorporata.

Ecco alcuni scenari in cui potrebbero essere utili le funzioni JavaScript definite dall'utente:
* Analisi e manipolazione delle stringhe con funzioni di espressione regolare, ad esempio **Regexp_Replace()** e **Regexp_Extract()**
* Decodifica e codifica dati, ad esempio conversione dal formato binario al formato esadecimale
* Esecuzione di calcoli matematici con funzioni JavaScript **Math**
* Esecuzione di operazioni di matrice come ordinamento, aggiunta, ricerca e riempimento

Ecco alcune operazioni non eseguibili con una funzione JavaScript definita dall'utente in Analisi di flusso:
* Chiamate agli endpoint REST esterni, ad esempio per l'esecuzione di una ricerca inversa degli indirizzi IP o per la raccolta di dati di riferimento da un'origine esterna
* Esecuzione della serializzazione o deserializzazione negli input e output di un formato evento personalizzato
* Creazione di aggregazioni personalizzate

Sebbene le funzioni come **Date.GetDate()** o **Math.random()** non sono bloccate nella definizione delle funzioni, è consigliabile evitare di utilizzarle. Queste funzioni, infatti, **non** restituiscono lo stesso risultato ogni volta che vengono chiamate e Analisi di flusso di Azure non conserva un giornale di chiamate di funzione e dei risultati restituiti. Se una funzione restituisce risultati diversi per gli stessi eventi, non viene garantita la ripetibilità in caso di processo riavviato dall'utente o dal servizio di Analisi di flusso.

## <a name="add-a-javascript-user-defined-function-in-the-azure-portal"></a>Aggiungere una funzione JavaScript definita dall'utente nel portale di Azure
Per creare una semplice funzione JavaScript definita dall'utente in un processo esistente di Analisi di flusso, seguire questa procedura:

> [!NOTE]
> Questi passaggi funzionano sui processi di Analisi di flusso configurati per l'esecuzione nel cloud. Se il processo di Analisi di flusso è configurato per l'esecuzione su Azure IoT Edge, usare invece Visual Studio e [scrivere la funzione definita dall'utente usando C#](stream-analytics-edge-csharp-udf.md).

1.  Nel portale di Azure individuare il processo di Analisi di flusso.

2. Nell'intestazione **Topologia processo** selezionare **Funzioni**. Viene visualizzato un elenco vuoto di funzioni.

3.  Per creare una nuova funzione definita dall'utente, selezionare **+ Aggiungi**.

4.  Sul pannello **Nuova funzione** selezionare **JavaScript** per **Tipo funzione**. Nell'editor viene visualizzato un modello di funzione predefinita.

5.  Per l'**alias della funzione definita dall'utente**, inserire **hex2Int** e modificare l'implementazione della funzione come indicato di seguito:

    ```javascript
    // Convert Hex value to integer.
    function hex2Int(hexValue) {
        return parseInt(hexValue, 16);
    }
    ```

6.  Selezionare **Salva**. La funzione viene visualizzata nell'elenco delle funzioni.
7.  Selezionare la nuova funzione **hex2Int** e controllare la definizione di funzione. Per ogni funzione, all'alias della funzione viene aggiunto un prefisso della **funzione definita dall'utente**. È necessario *includere il prefisso* quando si chiama la funzione nella query Analisi di flusso. In questo caso, si chiama **UDF.hex2Int**.

## <a name="testing-javascript-udfs"></a>Test delle funzioni definite dall'utente JavaScript 
È possibile testare ed eseguire il debug della logica delle funzioni definite dall'utente JavaScript in qualsiasi browser. Il debug e il test della logica di queste funzioni definite dall'utente non sono attualmente supportati nel portale di Analisi di flusso. Quando la funzione si comporta come previsto, è possibile aggiungerla al processo di analisi di flusso come indicato in precedenza e quindi richiamarla direttamente dalla query.

## <a name="call-a-javascript-user-defined-function-in-a-query"></a>Chiamare una funzione JavaScript definita dall'utente nella query

1. Nell'editor di query, nell'intestazione **Topologia processo**, selezionare **Query**.
2.  Modificare la query e quindi chiamare la funzione definita dall'utente, simile alla seguente:

    ```SQL
    SELECT
        time,
        UDF.hex2Int(offset) AS IntOffset
    INTO
        output
    FROM
        InputStream
    ```

3.  Per caricare un file di dati di esempio, fare clic con il tasto destro del mouse sull'input del processo.
4.  Per testare la query, selezionare **Test**.


## <a name="supported-javascript-objects"></a>Oggetti JavaScript supportati
Le funzioni JavaScript definite dall'utente in Analisi di flusso di Azure supportano oggetti JavaScript standard e incorporati. Per un elenco di questi oggetti, vedere [Oggetti globali](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).

### <a name="stream-analytics-and-javascript-type-conversion"></a>Conversione dei tipi di Analisi di flusso e JavaScript

Esistono delle differenze tra i tipi supportati da JavaScript e dal linguaggio di query di Analisi di flusso. Questa tabella elenca i mapping di conversione tra i due:

Analisi di flusso | JavaScript
--- | ---
bigint | Numero (per la precisione, JavaScript può rappresentare solo numeri interi fino a 2^53)
Datetime | Data (JavaScript supporta solo millisecondi)
double | Number
nvarchar(MAX) | string
Record | Oggetto
Array | Array
NULL | Null


Ecco le conversioni da JavaScript ad Analisi di flusso:


JavaScript | Analisi di flusso
--- | ---
Number | Bigint (se il numero è arrotondato e compreso tra long.MinValue e long.MaxValue, in caso contrario è doppio)
Data | Datetime
string | nvarchar(MAX)
Oggetto | Record
Array | Array
Null, Undefined | NULL
Qualsiasi altro tipo (ad esempio, una funzione o un errore) | Non supportato (risultati nell'errore di runtime)

Il linguaggio JavaScript distingue tra maiuscole/minuscole e le maiuscole e le minuscole dei campi oggetto nel codice JavaScript devono corrispondere alle maiuscole e alle minuscole dei campi nei dati in ingresso. Notare che i processi con livello di compatibilità 1.0 convertiranno i campi dall'istruzione SQL SELECT in modo che siano in minuscolo. Nel livello di compatibilità 1.1 e superiore, i campi dall'istruzione SELECT avranno le stesse maiuscole e minuscole come specificate nella query SQL.

## <a name="troubleshooting"></a>Risoluzione dei problemi
Gli errori di runtime in JavaScript sono considerati irreversibili ed esposti tramite il log attività. Per recuperare il log, nel portale di Azure passare al processo e selezionare **Log attività**.

## <a name="other-javascript-user-defined-function-patterns"></a>Altri modelli della funzione JavaScript definita dall'utente

### <a name="write-nested-json-to-output"></a>Scrivere una stringa JSON nidificata in output
In caso di passaggi di elaborazione successivi che utilizzano un output di un processo di Analisi di flusso come input e l'output richiede un formato JSON, è possibile scrivere una stringa JSON in output. L'esempio successivo chiama la funzione **JSON.stringify()** per raccogliere tutte le coppie nome/valore dell'input e quindi scriverle come valore di stringa singola nell'output.

**Definizione della funzione JavaScript definita dall'utente:**

```javascript
function main(x) {
return JSON.stringify(x);
}
```

**Query di esempio:**
```SQL
SELECT
    DataString,
    DataValue,
    HexValue,
    UDF.jsonstringify(input) As InputEvent
INTO
    output
FROM
    input PARTITION BY PARTITIONID
```

## <a name="clean-up-resources"></a>Pulire le risorse

Quando non sono più necessari, eliminare il gruppo di risorse, il processo di streaming e tutte le risorse correlate. Eliminando il processo si evita di pagare per le unità di streaming usate dal processo. Se si prevede di usare il processo in futuro, è possibile arrestarlo e riavviarlo in un secondo momento, quando è necessario. Se non si intende continuare a usare il processo, eliminare tutte le risorse create tramite questa guida introduttiva seguendo questa procedura:

1. Scegliere **Gruppi di risorse** dal menu a sinistra del portale di Azure e quindi fare clic sul nome della risorsa creata.  
2. Nella pagina del gruppo di risorse fare clic su **Elimina**, digitare il nome della risorsa da eliminare nella casella di testo e quindi fare clic su **Elimina**.

## <a name="get-help"></a>Ottenere aiuto
Per ulteriore assistenza, provare il [Forum di Analisi di flusso di Azure](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione è stato creato un processo di Analisi di flusso che esegue una semplice funzione JavaScript definita dall'utente. Per altre informazioni su Analisi di flusso, vedere gli articoli sugli scenari in tempo reale:

> [!div class="nextstepaction"]
> [Analisi del sentiment su Twitter in tempo reale in Analisi di flusso di Azure](stream-analytics-twitter-sentiment-analysis-trends.md)
