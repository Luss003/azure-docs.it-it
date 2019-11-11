---
title: 'Guida introduttiva: Come usare gli argomenti del bus di servizio di Azure con Python'
description: 'Guida introduttiva: Informazioni su come usare gli argomenti e le sottoscrizioni del bus di servizio da Python.'
services: service-bus-messaging
documentationcenter: python
author: axisc
manager: timlt
editor: spelluru
ms.assetid: c4f1d76c-7567-4b33-9193-3788f82934e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: quickstart
ms.date: 11/05/2019
ms.author: aschhab
ms.openlocfilehash: 8f7d47879a025742dbca6a5cafa634899e60ee68
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/07/2019
ms.locfileid: "73719169"
---
# <a name="quickstart-how-to-use-service-bus-topics-and-subscriptions-with-python"></a>Guida introduttiva: Come usare gli argomenti e le sottoscrizioni del bus di servizio con Python

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Questo articolo descrive come usare gli argomenti e le sottoscrizioni del bus di servizio. Gli esempi sono scritti in Python e usano il [pacchetto Azure SDK per Python][Azure Python package]. Gli scenari trattati includono:

- Creazione di argomenti e sottoscrizioni 
- Creazione di filtri di sottoscrizione 
- Invio di messaggi a un argomento 
- Ricezione di messaggi da una sottoscrizione
- Eliminazione di argomenti e sottoscrizioni

## <a name="prerequisites"></a>Prerequisiti
1. Una sottoscrizione di Azure. Per completare l'esercitazione, è necessario un account Azure. È possibile attivare i [vantaggi della sottoscrizione Visual Studio o MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) oppure iscriversi per ottenere un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
2. Seguire la procedura descritta in [Avvio rapido: Usare il portale di Azure per creare un argomento del bus di servizio e le sottoscrizioni all'argomento](service-bus-quickstart-topics-subscriptions-portal.md) per creare uno **spazio dei nomi** del bus di servizio e ottenere la **stringa di connessione**.

    > [!NOTE]
    > In questa guida di avvio rapido verranno creati un **argomento** e una **sottoscrizione** all'argomento usando **Python**. 
3. Installare il [pacchetto Azure per Python][Azure Python package]. Vedere la [Guida all'installazione di Python](/azure/python/python-sdk-azure-install).

## <a name="create-a-topic"></a>Creare un argomento

L'oggetto **ServiceBusService** consente di usare gli argomenti. Aggiungere il seguente codice all'inizio di ogni file Python da cui si desidera accedere al bus di servizio a livello di codice:

```python
from azure.servicebus.control_client import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

Il codice seguente consente di creare un oggetto **ServiceBusService**. Sostituire `mynamespace`, `sharedaccesskeyname` e `sharedaccesskey` con lo spazio dei nomi effettivo e il nome e il valore della chiave di firma di accesso condiviso.

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

È possibile ottenere i valori per il nome e il valore della chiave di firma di accesso condiviso dal [portale di Azure][Azure portal].

```python
bus_service.create_topic('mytopic')
```

Il metodo `create_topic` supporta anche opzioni aggiuntive che consentono di eseguire l'override delle impostazioni predefinite degli argomenti, come la durata (TTL) dei messaggi o le dimensioni massime dell'argomento. L'esempio seguente illustra come impostare la dimensione massima dell'argomento su 5 GB e un valore di durata (TTL) di un minuto:

```python
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Creare sottoscrizioni

È possibile creare sottoscrizioni ad argomenti anche con l'oggetto **ServiceBusService**. Le sottoscrizioni sono denominate e possono includere un filtro facoltativo che limita il set di messaggi recapitati alla coda virtuale della sottoscrizione.

> [!NOTE]
> Per impostazione predefinita, le sottoscrizioni sono permanenti e continueranno a esistere fintanto che esse o l'argomento che hanno sottoscritto non verrà eliminato.
> 
> È possibile eliminare automaticamente le sottoscrizioni impostando la [proprietà auto_delete_on_idle](https://docs.microsoft.com/python/api/azure-mgmt-servicebus/azure.mgmt.servicebus.models.sbsubscription?view=azure-python).

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Creare una sottoscrizione con il filtro (MatchAll) predefinito

Se non vengono specificati altri filtri durante la creazione di una nuova sottoscrizione, viene usato il filtro (predefinito) **MatchAll**. Quando si usa il filtro **MatchAll**, tutti i messaggi pubblicati nell'argomento vengono inseriti nella coda virtuale della sottoscrizione. L'esempio seguente crea una sottoscrizione denominata `AllMessages` e usa il filtro **MatchAll** predefinito.

```python
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a>Creare sottoscrizioni con i filtri

È anche possibile definire i filtri che consentono di specificare quali messaggi inviati a un argomento devono essere presenti in una specifica sottoscrizione dell'argomento.

Il tipo di filtro più flessibile tra quelli supportati dalle sottoscrizioni è **SqlFilter**, che implementa un subset di SQL92. I filtri SQL agiscono sulle proprietà dei messaggi pubblicati nell'argomento. Per altre informazioni sulle espressioni che possono essere usate con un filtro SQL, vedere la sintassi [SqlFilter.SqlExpression][SqlFilter.SqlExpression].

È possibile aggiungere filtri a una sottoscrizione usando il metodo **create\_rule** dell'oggetto **ServiceBusService**. Questo metodo consente di aggiungere nuovi filtri a una sottoscrizione esistente.

> [!NOTE]
> Poiché il filtro predefinito viene applicato automaticamente a tutte le nuove sottoscrizioni, è necessario prima di tutto rimuovere il filtro predefinito, altrimenti **MatchAll** eseguirà l'override di qualsiasi altro filtro specificato. È possibile rimuovere la regola predefinita con il metodo `delete_rule` dell'oggetto **ServiceBusService**.
> 
> 

L'esempio seguente crea una sottoscrizione denominata `HighMessages` con un filtro **SqlFilter** che seleziona solo i messaggi che hanno una proprietà `messagenumber` personalizzata con valore maggiore di 3:

```python
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Analogamente, l'esempio seguente crea una sottoscrizione denominata `LowMessages` con un filtro **SqlFilter** che seleziona solo i messaggi che hanno una proprietà `messagenumber` con valore minore o uguale a 3:

```python
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Di conseguenza, quando un messaggio viene inviato a `mytopic`, viene sempre recapitato ai ricevitori aderenti alla sottoscrizione dell'argomento **AllMessages** e viene recapitato selettivamente ai ricevitori aderenti alle sottoscrizioni dell'argomento **HighMessages** e **LowMessages**, a seconda del contenuto del messaggio.

## <a name="send-messages-to-a-topic"></a>Inviare messaggi a un argomento

Per inviare un messaggio a un argomento del bus di servizio, l'applicazione deve usare il metodo `send_topic_message` dell'oggetto **ServiceBusService**.

Il seguente esempio illustra come inviare cinque messaggi di test a `mytopic`. Il valore della proprietà `messagenumber` di ogni messaggio varia nell'iterazione del ciclo, determinando quali sottoscrizioni lo ricevono:

```python
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'),
                  custom_properties={'messagenumber': i})
    bus_service.send_topic_message('mytopic', msg)
```

Gli argomenti del bus di servizio supportano messaggi di dimensioni massime fino a 256 KB nel [livello Standard](service-bus-premium-messaging.md) e fino a 1 MB nel [livello Premium](service-bus-premium-messaging.md). Le dimensioni massime dell'intestazione, che include le proprietà standard e personalizzate dell'applicazione, non possono superare 64 KB. Non esiste alcun limite al numero di messaggi mantenuti in un argomento, mentre è prevista una limitazione alla dimensione totale dei messaggi di un argomento. Questa dimensione dell'argomento viene definita al momento della creazione, con un limite massimo di 5 GB. Per altre informazioni sulle quote, vedere [Quote del bus di servizio][Service Bus quotas].

## <a name="receive-messages-from-a-subscription"></a>Ricevere messaggi da una sottoscrizione

I messaggi vengono ricevuti da una sottoscrizione usando il metodo `receive_subscription_message` nell'oggetto **ServiceBusService**:

```python
msg = bus_service.receive_subscription_message(
    'mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

I messaggi vengono eliminati dalla sottoscrizione non appena vengono letti quando il parametro `peek_lock` è impostato su **False**. È possibile leggere e bloccare il messaggio senza eliminarlo dalla coda impostando il parametro `peek_lock` su **True**.

Il comportamento di lettura ed eliminazione del messaggio nell'ambito dell'operazione di ricezione costituisce il modello più semplice ed è adatto per scenari in cui un'applicazione può tollerare la mancata elaborazione di un messaggio in caso di errore. Per comprendere meglio questo comportamento, si consideri uno scenario in cui il consumer invia la richiesta di ricezione e viene arrestato in modo anomalo prima dell'elaborazione. Poiché il bus di servizio ha contrassegnato il messaggio come utilizzato, quando l'applicazione viene riavviata e inizia a usare nuovamente i messaggi, il messaggio usato prima dell'arresto anomalo del sistema risulta perso.

Se il parametro `peek_lock` è impostato su **True**, l'operazione di ricezione viene suddivisa in due fasi, in modo da consentire il supporto di applicazioni che non possono tollerare messaggi mancanti. Quando il bus di servizio riceve una richiesta, individua il messaggio successivo da usare, lo blocca per impedirne la ricezione da parte di altri consumer e quindi lo restituisce all'applicazione. Dopo aver elaborato il messaggio o averlo archiviato in modo affidabile per una successiva elaborazione, l'applicazione esegue la seconda fase del processo di ricezione chiamando il metodo `delete` nell'oggetto **Message**. Il metodo `delete` contrassegna il messaggio come utilizzato e lo rimuove dalla sottoscrizione.

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
if msg.body is not None:
print(msg.body)
msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Come gestire arresti anomali e messaggi illeggibili dell'applicazione

Il bus di servizio fornisce funzionalità per il ripristino gestito automaticamente in caso di errori nell'applicazione o di problemi di elaborazione di un messaggio. Se un'applicazione ricevente non riesce a elaborare il messaggio per qualsiasi motivo, può chiamare il metodo `unlock` nell'oggetto **Message**. Con questo metodo, il bus di servizio sblocca il messaggio nella sottoscrizione rendendolo nuovamente disponibile per la ricezione da parte della stessa o da un'altra applicazione consumer.

Al messaggio bloccato nella sottoscrizione è anche associato un timeout. Se l'applicazione non riesce a elaborare il messaggio prima della scadenza del timeout, ad esempio a causa di un arresto anomalo, il bus di servizio sbloccherà automaticamente il messaggio rendendolo nuovamente disponibile per la ricezione.

In caso di arresto anomalo dell'applicazione dopo l'elaborazione del messaggio ma prima della chiamata del metodo `delete`, il messaggio verrà nuovamente recapitato all'applicazione al riavvio. Questo processo di elaborazione viene spesso definito di tipo At-Least-Once\*, per indicare che ogni messaggio verrà elaborato almeno una volta, ma che in determinate situazioni potrà essere recapitato una seconda volta. Se lo scenario non tollera la doppia elaborazione, gli sviluppatori dovranno aggiungere logica aggiuntiva all'applicazione per gestire il secondo recapito del messaggio. A tale scopo, è possibile usare la proprietà **MessageId** del messaggio, che rimane costante in tutti i tentativi di recapito.

## <a name="delete-topics-and-subscriptions"></a>Eliminare argomenti e sottoscrizioni

Gli argomenti e le sottoscrizioni sono permanenti, a meno che non venga impostata la [proprietà auto_delete_on_idle](https://docs.microsoft.com/python/api/azure-mgmt-servicebus/azure.mgmt.servicebus.models.sbsubscription?view=azure-python). Possono essere eliminati tramite il [portale di Azure][Azure portal] o a livello di codice. L'esempio seguente illustra come eliminare l'argomento denominato `mytopic`:

```python
bus_service.delete_topic('mytopic')
```

Se si elimina un argomento, vengono eliminate anche tutte le sottoscrizioni registrate con l'argomento. Le sottoscrizioni possono essere eliminate anche in modo indipendente. Il codice seguente illustra come eliminare una sottoscrizione denominata `HighMessages` dall'argomento `mytopic`:

```python
bus_service.delete_subscription('mytopic', 'HighMessages')
```

> [!NOTE]
> È possibile gestire le risorse del bus di servizio con [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/). Service Bus Explorer consente agli utenti di connettersi a uno spazio dei nomi del bus di servizio e di amministrare le entità di messaggistica in modo semplice. Lo strumento offre caratteristiche avanzate, tra cui funzionalità di importazione/esportazione o la possibilità di testare argomenti, code, sottoscrizioni, servizi di inoltro, hub di notifica e hub eventi. 

## <a name="next-steps"></a>Passaggi successivi

A questo punto, dopo aver appreso le nozioni di base degli argomenti del bus di servizio, usare i seguenti collegamenti per altre informazioni.

* Vedere [Code, argomenti e sottoscrizioni del bus di servizio][Queues, topics, and subscriptions].
* Informazioni di riferimento per [SqlFilter.SqlExpression][SqlFilter.SqlExpression].

[Azure portal]: https://portal.azure.com
[Azure Python package]: https://pypi.python.org/pypi/azure  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Service Bus quotas]: service-bus-quotas.md 
