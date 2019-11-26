---
title: Associazioni di Archiviazione code di Azure per Funzioni di Azure
description: Informazioni su come usare il trigger di archiviazione code di Azure e l'associazione di output in Funzioni di Azure.
author: craigshoemaker
ms.topic: reference
ms.date: 09/03/2018
ms.author: cshoe
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 3c27ff06237336d37ad1b5bed1b90aaa6b076f0b
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/20/2019
ms.locfileid: "74231007"
---
# <a name="azure-queue-storage-bindings-for-azure-functions"></a>Associazioni di Archiviazione code di Azure per Funzioni di Azure

Questo articolo illustra come operare con le associazioni dell'archiviazione code di Azure in Funzioni di Azure. Funzioni di Azure supporta il trigger e le associazioni di output per le code.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Pacchetti: Funzioni 1.x

Le associazioni di Archiviazione code sono incluse nel pacchetto NuGet [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) versione 2.x. Il codice sorgente del pacchetto si trova nel repository GitHub [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.Storage/Queue).

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="packages---functions-2x"></a>Pacchetti: Funzioni 2.x

Le associazioni di archiviazione code sono incluse nel pacchetto NuGet [Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage) versione 3.x. Il codice sorgente del pacchetto si trova nel repository GitHub [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Queues).

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="encoding"></a>Codifica
Le funzioni richiedono una stringa codificata *base64*. Le modifiche al tipo di codifica, per preparare i dati come una stringa con codifica *base64*, devono essere implementate nel servizio di chiamata.

## <a name="trigger"></a>Trigger

Usare il trigger della coda per avviare una funzione quando viene ricevuto un nuovo elemento in una coda. Il messaggio in coda viene fornito come input alla funzione.

## <a name="trigger---example"></a>Trigger - esempio

Vedere l'esempio specifico per ciascun linguaggio:

* [C#](#trigger---c-example)
* [Script C# (.csx)](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)
* [Java](#trigger---java-example)
* [Python](#trigger---python-example)

### <a name="trigger---c-example"></a>Trigger - esempio in C#

L'esempio seguente illustra una [funzione C#](functions-dotnet-class-library.md) che esegue il polling della coda `myqueue-items` e scrive un log a ogni elaborazione di un elemento della coda.

```csharp
public static class QueueFunctions
{
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        ILogger log)
    {
        log.LogInformation($"C# function processed: {myQueueItem}");
    }
}
```

### <a name="trigger---c-script-example"></a>Trigger - esempio di script C#

L'esempio seguente illustra un'associazione di trigger della coda in un file *function.json* e il codice [script C# (file con estensione csx)](functions-reference-csharp.md) che usa l'associazione. La funzione esegue il polling della coda `myqueue-items` e scrive un log a ogni elaborazione di un elemento della coda.

Ecco il file *function.json*:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

Queste proprietà sono descritte nella sezione [configuration](#trigger---configuration).

Ecco il codice script C#:

```csharp
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.Extensions.Logging;
using Microsoft.WindowsAzure.Storage.Queue;
using System;

public static void Run(CloudQueueMessage myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem.AsString}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

Nella sezione [usage](#trigger---usage) è illustrato `myQueueItem`, denominato dalla proprietà `name` in function.json.  Nella sezione [message metadata](#trigger---message-metadata) sono illustrate tutte le altre variabili indicate.

### <a name="trigger---javascript-example"></a>Trigger - esempio JavaScript

L'esempio seguente illustra un'associazione di trigger della coda in un file *function.json* e una [funzione JavaScript](functions-reference-node.md) che usa l'associazione. La funzione esegue il polling della coda `myqueue-items` e scrive un log a ogni elaborazione di un elemento della coda.

Ecco il file *function.json*:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

Queste proprietà sono descritte nella sezione [configuration](#trigger---configuration).

> [!NOTE]
> Il parametro Name viene visualizzato come `context.bindings.<name>` nel codice JavaScript che contiene il payload dell'elemento della coda. Il payload viene anche passato come secondo parametro alla funzione.

Ecco il codice JavaScript:

```javascript
module.exports = async function (context, message) {
    context.log('Node.js queue trigger function processed work item', message);
    // OR access using context.bindings.<name>
    // context.log('Node.js queue trigger function processed work item', context.bindings.myQueueItem);
    context.log('expirationTime =', context.bindingData.expirationTime);
    context.log('insertionTime =', context.bindingData.insertionTime);
    context.log('nextVisibleTime =', context.bindingData.nextVisibleTime);
    context.log('id =', context.bindingData.id);
    context.log('popReceipt =', context.bindingData.popReceipt);
    context.log('dequeueCount =', context.bindingData.dequeueCount);
    context.done();
};
```

Nella sezione [usage](#trigger---usage) è illustrato `myQueueItem`, denominato dalla proprietà `name` in function.json.  Nella sezione [message metadata](#trigger---message-metadata) sono illustrate tutte le altre variabili indicate.

### <a name="trigger---java-example"></a>Trigger - esempio Java

L'esempio Java seguente illustra una funzione trigger di coda di archiviazione che registra il messaggio attivato inserito nella coda `myqueuename`.

 ```java
 @FunctionName("queueprocessor")
 public void run(
    @QueueTrigger(name = "msg",
                   queueName = "myqueuename",
                   connection = "myconnvarname") String message,
     final ExecutionContext context
 ) {
     context.getLogger().info(message);
 }
 ```

### <a name="trigger---python-example"></a>Trigger - Esempio di Python

Nell'esempio seguente viene illustrato come leggere un messaggio della coda passato a una funzione tramite un trigger.

Un trigger della coda di archiviazione è definito in *Function. JSON,* dove *Type* è impostato su `queueTrigger`.

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "msg",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "messages",
      "connection": "AzureStorageQueuesConnectionString"
    }
  ]
}
```

Il codice  *_\_init_\_. py* dichiara un parametro come `func.ServiceBusMessage` che consente di leggere il messaggio della coda nella funzione.

```python
import logging
import json

import azure.functions as func

def main(msg: func.QueueMessage):
    logging.info('Python queue trigger function processed a queue item.')

    result = json.dumps({
        'id': msg.id,
        'body': msg.get_body().decode('utf-8'),
        'expiration_time': (msg.expiration_time.isoformat()
                            if msg.expiration_time else None),
        'insertion_time': (msg.insertion_time.isoformat()
                           if msg.insertion_time else None),
        'time_next_visible': (msg.time_next_visible.isoformat()
                              if msg.time_next_visible else None),
        'pop_receipt': msg.pop_receipt,
        'dequeue_count': msg.dequeue_count
    })

    logging.info(result)
```

## <a name="trigger---attributes"></a>Trigger - attributi

Nelle [librerie di classi C#](functions-dotnet-class-library.md) usare i seguenti attributi per configurare un trigger di coda:

* [QueueTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Queues/QueueTriggerAttribute.cs)

  Il costruttore dell'attributo accetta il nome della coda da monitorare, come illustrato nell'esempio seguente:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items")] string myQueueItem, 
      ILogger log)
  {
      ...
  }
  ```

  È possibile impostare la proprietà `Connection` per specificare l'account di archiviazione da usare, come illustrato nell'esempio seguente:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items", Connection = "StorageConnectionAppSetting")] string myQueueItem, 
      ILogger log)
  {
      ....
  }
  ```

  Per un esempio completo, vedere [Trigger - esempio in C#](#trigger---c-example).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Offre un altro modo per specificare l'account di archiviazione da usare. Il costruttore accetta il nome di un'impostazione dell'app che contiene una stringa di connessione di archiviazione. L'attributo può essere applicato a livello di parametro, metodo o classe. L'esempio seguente illustra il livello classe e il livello metodo:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("QueueTrigger")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ...
  }
  ```

L'account di archiviazione da usare è determinato nell'ordine seguente:

* La proprietà `QueueTrigger` dell'attributo `Connection`.
* L'attributo `StorageAccount` applicato allo stesso parametro dell'attributo `QueueTrigger`.
* L'attributo `StorageAccount` applicato alla funzione.
* L'attributo `StorageAccount` applicato alla classe.
* L'impostazione dell'app "AzureWebJobsStorage".

## <a name="trigger---configuration"></a>Trigger - configurazione

Nella tabella seguente sono illustrate le proprietà di configurazione dell'associazione impostate nel file *function.json* e nell'attributo `QueueTrigger`.

|Proprietà di function.json | Proprietà dell'attributo |DESCRIZIONE|
|---------|---------|----------------------|
|**type** | N/D| Il valore deve essere impostato su `queueTrigger`. Questa proprietà viene impostata automaticamente quando si crea il trigger nel portale di Azure.|
|**direction**| N/D | Solo nel file *function.json*. Il valore deve essere impostato su `in`. Questa proprietà viene impostata automaticamente quando si crea il trigger nel portale di Azure. |
|**nome** | N/D |Nome della variabile che contiene il payload dell'elemento della coda nel codice della funzione.  |
|**queueName** | **QueueName**| Nome della coda sulla quale eseguire il polling. |
|**connessione** | **Connection** |Nome di un'impostazione dell'app che contiene la stringa di connessione di archiviazione da usare per questa associazione. Se il nome dell'impostazione dell'app inizia con "AzureWebJobs", è possibile specificare solo il resto del nome. Ad esempio, se si imposta `connection` su "MyStorage", il runtime di Funzioni di Azure cerca un'impostazione dell'app denominata "AzureWebJobsMyStorage". Se si lascia vuoto `connection`, il runtime di Funzioni di Azure usa la stringa di connessione di archiviazione predefinita nell'impostazione dell'app denominata `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Trigger - uso

In C# e nello script C# è possibile accedere ai dati del messaggio usando un parametro del metodo, ad esempio `string paramName`. Negli script C#, `paramName` è il valore specificato nella proprietà `name` di *function.json*. È possibile definire associazioni con uno dei seguenti tipi:

* Object: il runtime di Funzioni deserializza un payload JSON in un'istanza di una classe arbitraria definita nel codice. 
* `string`
* `byte[]`
* [CloudQueueMessage]

Se si prova a eseguire l'associazione a `CloudQueueMessage` e si riceve un messaggio di errore, assicurarsi di fare riferimento alla [versione corretta di Storage SDK](#azure-storage-sdk-version-in-functions-1x).

In JavaScript, usare `context.bindings.<name>` per accedere al payload dell'elemento della coda. Se il payload è JSON, viene deserializzato in un oggetto.

## <a name="trigger---message-metadata"></a>Trigger - metadati del messaggio

Il trigger della coda fornisce diverse [proprietà di metadati](./functions-bindings-expressions-patterns.md#trigger-metadata). Queste proprietà possono essere usate come parte delle espressioni di associazione in altre associazioni o come parametri nel codice. Queste sono le proprietà della classe [CloudQueueMessage](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueuemessage).

|Proprietà|digitare|DESCRIZIONE|
|--------|----|-----------|
|`QueueTrigger`|`string`|Payload della coda, se si tratta di una stringa valida. Se il payload della coda di messaggi è una stringa, `QueueTrigger` ha lo stesso valore della variabile denominata dalla proprietà `name` in *function.json*.|
|`DequeueCount`|`int`|Il numero di volte in cui questo messaggio è stato rimosso dalla coda.|
|`ExpirationTime`|`DateTimeOffset`|Ora di scadenza del messaggio.|
|`Id`|`string`|ID del messaggio in coda.|
|`InsertionTime`|`DateTimeOffset`|L'ora in cui il messaggio è stato aggiunto alla coda.|
|`NextVisibleTime`|`DateTimeOffset`|Ora in cui il messaggio sarà visibile.|
|`PopReceipt`|`string`|Ricezione del messaggio.|

## <a name="trigger---poison-messages"></a>Trigger - messaggi non elaborabili

Quando una funzione di trigger della coda ha esito negativo, Funzioni di Azure ritenta l'esecuzione fino a cinque volte per un dato messaggio della coda, incluso il primo tentativo. Se tutti i cinque tentativi hanno esito negativo, il runtime di Funzioni aggiunge un messaggio a una coda denominata *&lt;originalqueuename>-poison*. È possibile scrivere una funzione per elaborare i messaggi dalla coda non elaborabile archiviandoli o inviando una notifica della necessità di un intervento manuale.

Per gestire manualmente i messaggi non elaborabili, controllare [dequeueCount](#trigger---message-metadata) nel messaggio della coda.

## <a name="trigger---polling-algorithm"></a>Trigger - algoritmo di polling

Il trigger della coda implementa un algoritmo di backoff esponenziale casuale per ridurre l'effetto del polling delle code inattive sui costi delle transazioni di archiviazione.  Quando viene trovato un messaggio, il runtime attende due secondi e quindi controlla la presenza di un altro messaggio. Quando non viene trovato alcun messaggio, rimane in attesa per circa quattro secondi prima di riprovare. Dopo alcuni tentativi non riusciti per ottenere un messaggio nella coda, il tempo di attesa continua ad aumentare finché non raggiunge il tempo massimo di attesa, che per impostazione predefinita è di un minuto. Il tempo di attesa massimo può essere configurato tramite la proprietà `maxPollingInterval` nel [file host.json](functions-host-json.md#queues).

## <a name="trigger---concurrency"></a>Trigger - concorrenza

Quando sono in attesa più messaggi della coda, il trigger della coda recupera un batch di messaggi e richiama le istanze della funzione in modo simultaneo per l'elaborazione. Per impostazione predefinita, la dimensione del batch è pari a 16. Quando il numero elaborato scende a 8, il runtime ottiene un altro batch e inizia l'elaborazione dei messaggi. Di conseguenza, il numero massimo di messaggi simultanei elaborati per ogni funzione in una singola macchina virtuale è pari a 24. Questo limite si applica separatamente a ogni funzione attivata dalla coda in ogni macchina virtuale. Se il numero di istanze dell'app per le funzioni aumenta includendo più macchine virtuali, ogni macchina virtuale attenderà i trigger e proverà a eseguire le funzioni. Se ad esempio il numero di istanze di un'app per le funzioni aumento includendo 3 macchine virtuali, il numero massimo predefinito di istanze simultanee di una funzione attivata dalla coda è pari a 72.

La dimensione del batch e la soglia ottenere un nuovo batch possono essere configurate nel [file host.json](functions-host-json.md#queues). Se si desidera ridurre al minimo l'esecuzione parallela per le funzioni attivate dalla coda in un'app per le funzioni, è possibile impostare la dimensione del batch su 1. Questa impostazione elimina la concorrenza solo fino a quando l'app per le funzioni viene eseguita in una singola macchina virtuale. 

Il trigger della coda impedisce automaticamente a una funzione di elaborare un messaggio della coda più volte. Le funzioni non devono essere scritte per essere idempotenti.

## <a name="trigger---hostjson-properties"></a>Trigger - proprietà di host.json

Il file [host.json](functions-host-json.md#queues) contiene le impostazioni che controllano il comportamento del trigger della coda. Per informazioni dettagliate sulle impostazioni disponibili, vedere la sezione [impostazioni di host. JSON](#hostjson-settings) .

## <a name="output"></a>Output

Usare l'associazione di output dell'archiviazione code di Azure per scrivere i messaggi in una coda.

## <a name="output---example"></a>Output - esempio

Vedere l'esempio specifico per ciascun linguaggio:

* [C#](#output---c-example)
* [Script C# (.csx)](#output---c-script-example)
* [JavaScript](#output---javascript-example)
* [Java](#output---java-example)
* [Python](#output---python-example)

### <a name="output---c-example"></a>Output - esempio in C#

L'esempio seguente illustra una [funzione C#](functions-dotnet-class-library.md) che crea un messaggio nella coda per ogni richiesta HTTP ricevuta.

```csharp
[StorageAccount("AzureWebJobsStorage")]
public static class QueueFunctions
{
    [FunctionName("QueueOutput")]
    [return: Queue("myqueue-items")]
    public static string QueueOutput([HttpTrigger] dynamic input,  ILogger log)
    {
        log.LogInformation($"C# function processed: {input.Text}");
        return input.Text;
    }
}
```

### <a name="output---c-script-example"></a>Output - esempio di script C#

L'esempio seguente illustra un'associazione di trigger HTTP in un file *function.json* e il codice [script C# (file con estensione csx)](functions-reference-csharp.md) che usa l'associazione. La funzione crea un elemento della coda con un payload dell'oggetto **CustomQueueMessage** per ogni richiesta HTTP ricevuta.

Ecco il file *function.json*:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function",
      "name": "input"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "$return",
      "queueName": "outqueue",
      "connection": "MyStorageConnectionAppSetting"
    }
  ]
}
```

Queste proprietà sono descritte nella sezione [configuration](#output---configuration).

Ecco il codice script C# che crea un singolo messaggio nella coda:

```cs
public class CustomQueueMessage
{
    public string PersonName { get; set; }
    public string Title { get; set; }
}

public static CustomQueueMessage Run(CustomQueueMessage input, ILogger log)
{
    return input;
}
```

È possibile inviare più messaggi contemporaneamente usando `ICollector` o il parametro `IAsyncCollector`. Ecco il codice script C# che invia più messaggi, uno con i dati della richiesta HTTP e uno con i valori hardcoded:

```cs
public static void Run(
    CustomQueueMessage input, 
    ICollector<CustomQueueMessage> myQueueItems, 
    ILogger log)
{
    myQueueItems.Add(input);
    myQueueItems.Add(new CustomQueueMessage { PersonName = "You", Title = "None" });
}
```

### <a name="output---javascript-example"></a>Output - esempio JavaScript

L'esempio seguente illustra un'associazione di trigger HTTP in un file *function.json* e una [funzione JavaScript](functions-reference-node.md) che usa l'associazione. La funzione crea un elemento della coda per ogni richiesta HTTP ricevuta.

Ecco il file *function.json*:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function",
      "name": "input"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "$return",
      "queueName": "outqueue",
      "connection": "MyStorageConnectionAppSetting"
    }
  ]
}
```

Queste proprietà sono descritte nella sezione [configuration](#output---configuration).

Ecco il codice JavaScript:

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

È possibile inviare più messaggi contemporaneamente definendo un array di messaggio per l'associazione di output `myQueueItem`. Il codice JavaScript seguente invia due messaggi della coda con valori hardcoded per ogni richiesta HTTP ricevuta.

```javascript
module.exports = function(context) {
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

### <a name="output---java-example"></a>Output - esempio Java

 L'esempio seguente illustra una funzione Java che crea un messaggio nella coda quando la funzione viene attivata tramite richiesta HTTP.

```java
@FunctionName("httpToQueue")
@QueueOutput(name = "item", queueName = "myqueue-items", connection = "AzureWebJobsStorage")
 public String pushToQueue(
     @HttpTrigger(name = "request", methods = {HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS)
     final String message,
     @HttpOutput(name = "response") final OutputBinding&lt;String&gt; result) {
       result.setValue(message + " has been added.");
       return message;
 }
```

Nella [libreria di runtime di funzioni Java](/java/api/overview/azure/functions/runtime) usare l'annotazione `@QueueOutput` per i parametri il cui valore viene scritto nell'archiviazione code.  Il tipo di parametro deve essere `OutputBinding<T>`, dove T corrisponde a un qualsiasi tipo Java nativo di un oggetto POJO.

### <a name="output---python-example"></a>Output - Esempio di Python

Nell'esempio seguente viene illustrato come restituire valori singoli e multipli nelle code di archiviazione. La configurazione necessaria per *Function. JSON* è identica in entrambi i casi.

Un binding della coda di archiviazione è definito in *Function. JSON,* dove *Type* è impostato su `queue`.

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "msg",
      "queueName": "outqueue",
      "connection": "AzureStorageQueuesConnectionString"
    }
  ]
}
```

Per impostare un singolo messaggio nella coda, passare un singolo valore al metodo `set`.

```python
import azure.functions as func

def main(req: func.HttpRequest, msg: func.Out[str]) -> func.HttpResponse:

    input_msg = req.params.get('message')

    msg.set(input_msg)

    return 'OK'
```

Per creare più messaggi nella coda, dichiarare un parametro come tipo di elenco appropriato e passare una matrice di valori (che corrispondono al tipo di elenco) al metodo `set`.

```python
import azure.functions as func
import typing

def main(req: func.HttpRequest, msg: func.Out[typing.List[str]]) -> func.HttpResponse:

    msg.set(['one', 'two'])

    return 'OK'
```

## <a name="output---attributes"></a>Output - attributi

Nelle [librerie di classi C#](functions-dotnet-class-library.md) usare [QueueAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs).

L'attributo si applica a un parametro `out` o al valore restituito della funzione. Il costruttore dell'attributo accetta il nome della coda, come illustrato nell'esempio seguente:

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items")]
public static string Run([HttpTrigger] dynamic input,  ILogger log)
{
    ...
}
```

È possibile impostare la proprietà `Connection` per specificare l'account di archiviazione da usare, come illustrato nell'esempio seguente:

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items", Connection = "StorageConnectionAppSetting")]
public static string Run([HttpTrigger] dynamic input,  ILogger log)
{
    ...
}
```

Per un esempio completo, vedere [Output - esempio in C#](#output---c-example).

È possibile usare l'attributo `StorageAccount` per specificare l'account di archiviazione a livello di classe, metodo o parametro. Per altre informazioni, vedere Trigger - attributi.

## <a name="output---configuration"></a>Output - configurazione

Nella tabella seguente sono illustrate le proprietà di configurazione dell'associazione impostate nel file *function.json* e nell'attributo `Queue`.

|Proprietà di function.json | Proprietà dell'attributo |DESCRIZIONE|
|---------|---------|----------------------|
|**type** | N/D | Il valore deve essere impostato su `queue`. Questa proprietà viene impostata automaticamente quando si crea il trigger nel portale di Azure.|
|**direction** | N/D | Il valore deve essere impostato su `out`. Questa proprietà viene impostata automaticamente quando si crea il trigger nel portale di Azure. |
|**nome** | N/D | Nome della variabile che rappresenta la coda nel codice della funzione. Impostare su `$return` per fare riferimento al valore restituito della funzione.|
|**queueName** |**QueueName** | Nome della coda. |
|**connessione** | **Connection** |Nome di un'impostazione dell'app che contiene la stringa di connessione di archiviazione da usare per questa associazione. Se il nome dell'impostazione dell'app inizia con "AzureWebJobs", è possibile specificare solo il resto del nome. Ad esempio, se si imposta `connection` su "MyStorage", il runtime di Funzioni di Azure cerca un'impostazione dell'app denominata "AzureWebJobsMyStorage". Se si lascia vuoto `connection`, il runtime di Funzioni di Azure usa la stringa di connessione di archiviazione predefinita nell'impostazione dell'app denominata `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Output - uso

In C# e negli script C# scrivere un singolo messaggio nella coda tramite un parametro di metodo, ad esempio `out T paramName`. Negli script C#, `paramName` è il valore specificato nella proprietà `name` di *function.json*. È possibile usare il tipo restituito del metodo anziché un parametro `out` e `T` può essere uno dei seguenti tipi:

* Un oggetto serializzabile come JSON
* `string`
* `byte[]`
* [CloudQueueMessage] 

Se si prova a eseguire l'associazione a `CloudQueueMessage` e si riceve un messaggio di errore, assicurarsi di fare riferimento alla [versione corretta di Storage SDK](#azure-storage-sdk-version-in-functions-1x).

In C# e negli script C# scrivere più messaggi nella coda usando uno dei seguenti tipi: 

* `ICollector<T>` oppure `IAsyncCollector<T>`
* [CloudQueue](/dotnet/api/microsoft.azure.storage.queue.cloudqueue)

Nelle funzioni JavaScript usare `context.bindings.<name>` per accedere al messaggio nella coda di output. È possibile usare una stringa o un oggetto serializzabile in JSON per il payload dell'elemento della coda.


## <a name="exceptions-and-return-codes"></a>Eccezioni e codici restituiti

| Binding |  riferimento |
|---|---|
| Coda | [Codici di errore della coda](https://docs.microsoft.com/rest/api/storageservices/queue-service-error-codes) |
| Blob, Table, Queue | [Codici di errore di archiviazione](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| Blob, Table, Queue |  [Risoluzione dei problemi](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

<a name="host-json"></a>  

## <a name="hostjson-settings"></a>impostazioni host.json

Questa sezione descrive le impostazioni di configurazione globali disponibili per questa associazione nella versione 2.x. Il file host.json di esempio seguente contiene solo le impostazioni della versione 2.x per questa associazione. Per altre informazioni sulle impostazioni di configurazione globali nella versione 2.x, vedere [Informazioni di riferimento su host.json per Funzioni di Azure versione 2.x](functions-host-json.md).

> [!NOTE]
> Per informazioni di riferimento su host.json in Funzioni 1.x, vedere [Informazioni di riferimento su host.json per Funzioni di Azure 1.x](functions-host-json-v1.md).

```json
{
    "version": "2.0",
    "extensions": {
        "queues": {
            "maxPollingInterval": "00:00:02",
            "visibilityTimeout" : "00:00:30",
            "batchSize": 16,
            "maxDequeueCount": 5,
            "newBatchThreshold": 8
        }
    }
}
```


|Proprietà  |Default | DESCRIZIONE |
|---------|---------|---------|
|maxPollingInterval|00:00:01|L'intervallo massimo tra i polling di coda. Il valore minimo è 00:00:00.100 (100 ms) e incrementa fino a 00:01:00 (1 min).  In 1. x il tipo di dati è millisecondi e in 2. x si tratta di un intervallo di tempo.|
|visibilityTimeout|00:00:00|L'intervallo di tempo tra i tentativi se l'elaborazione di un messaggio ha esito negativo. |
|batchSize|16|Il numero di messaggi in coda che il runtime di Funzioni recupera simultaneamente e di processi in parallelo. Quando il numero elaborato viene ridotto a `newBatchThreshold`, il runtime ottiene un altro batch e inizia l'elaborazione dei messaggi. Di conseguenza, il numero massimo di messaggi simultanei elaborati per ogni funzione è `batchSize` più `newBatchThreshold`. Questo limite si applica separatamente a ogni funzione attivata dalla coda. <br><br>Se si vuole evitare l'esecuzione in parallelo per i messaggi ricevuti su una coda, è possibile impostare `batchSize` su 1. Tuttavia, questa impostazione elimina solo la concorrenza se l'app per le funzioni viene eseguita su una singola macchina virtuale (VM). Se l'app per le funzioni scala orizzontalmente più macchine virtuali, ogni macchina virtuale potrebbe eseguire un'istanza di ogni funzione attivata dalla coda.<br><br>Il valore massimo per `batchSize` è 32. |
|maxDequeueCount|5|Il numero di volte per provare l'elaborazione di un messaggio prima di essere spostato nella coda non elaborabile.|
|newBatchThreshold|batchSize/2|Ogni volta che il numero di messaggi elaborati simultaneamente viene ridotto a questo numero, il runtime recupera un altro batch.|

## <a name="next-steps"></a>Passaggi successivi

* [Altre informazioni su trigger e associazioni di Funzioni di Azure](functions-triggers-bindings.md)

<!--
> [!div class="nextstepaction"]
> [Go to a quickstart that uses a Queue storage trigger](functions-create-storage-queue-triggered-function.md)
-->

> [!div class="nextstepaction"]
> [Passare a un'esercitazione che usa l'associazione di output dell'archiviazione code.](functions-integrate-storage-queue-output-binding.md)

<!-- LINKS -->

[CloudQueueMessage]: /dotnet/api/microsoft.azure.storage.queue.cloudqueuemessage
