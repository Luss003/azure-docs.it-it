---
title: Guida alla risoluzione dei problemi del bus di servizio di Azure | Microsoft Docs
description: Questo articolo fornisce un elenco delle eccezioni di messaggistica del bus di servizio di Azure e le azioni consigliate da intraprendere quando si verifica l'eccezione.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 3d8526fe-6e47-4119-9f3e-c56d916a98f9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2020
ms.author: aschhab
ms.openlocfilehash: 37f316af68bc0b20f21eb606e2abc8232f29ce32
ms.sourcegitcommit: b5d646969d7b665539beb18ed0dc6df87b7ba83d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2020
ms.locfileid: "76759364"
---
# <a name="troubleshooting-guide-for-azure-service-bus"></a>Guida alla risoluzione dei problemi del bus di servizio di Azure
Questo articolo fornisce alcune eccezioni .NET generate dalle API di .NET Framework del bus di servizio e altri suggerimenti per la risoluzione dei problemi. 

## <a name="service-bus-messaging-exceptions"></a>Eccezioni della messaggistica del bus di servizio
Questa sezione elenca le eccezioni .NET generate dalle API .NET Framework. 

### <a name="exception-categories"></a>Categorie di eccezioni
Le API di messaggistica generano eccezioni che possono essere raggruppate nelle categorie seguenti e a ognuna delle quali è associata un'azione che è possibile eseguire per tentare di risolverla. Il significato e le cause di un'eccezione possono variare a seconda del tipo di entità di messaggistica:

1. Errore nella codifica dell'utente ([System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx)). Azione generale: provare a correggere il codice prima di continuare.
2. Errore di configurazione/installazione ([Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.servicebus.messagingentitynotfoundexception), [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). Azione generale: controllare la configurazione e modificarla, se necessario.
3. Eccezioni temporanee ([Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.azure.servicebus.serverbusyexception), [Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception)). Azione generale: ripetere l'operazione o inviare una notifica agli utenti. La classe `RetryPolicy` nell'SDK client può essere configurata in modo da gestire i tentativi automaticamente. Per altre informazioni, vedere [linee guida](/azure/architecture/best-practices/retry-service-specific#service-bus)per i tentativi.
4. Altre eccezioni ([System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx), [Microsoft.ServiceBus.Messaging.MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception), [Microsoft.ServiceBus.Messaging.SessionLockLostException](/dotnet/api/microsoft.azure.servicebus.sessionlocklostexception)). Azione generale: specifica del tipo di eccezione; vedere la tabella nella sezione seguente: 

### <a name="exception-types"></a>Tipi di eccezioni
La tabella seguente elenca i tipi di eccezioni di messaggistica, ne riporta le possibili cause, e indica l'azione suggerita che è possibile eseguire.

| **Tipo di eccezione** | **Descrizione/Causa/Esempi** | **Azione suggerita** | **Nota sulla ripetizione automatica/immediata** |
| --- | --- | --- | --- |
| [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |Il server non ha risposto all'operazione richiesta entro il tempo specificato, che è controllato da [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings). Il server può aver completato l'operazione richiesta. Ciò può verificarsi a causa di ritardi di rete o di altri ritardi nell'infrastruttura. |Controllare lo stato del sistema per verificarne la coerenza e, se necessario, ripetere l'operazione. Vedere [Eccezioni di timeout](#timeoutexception). |In alcuni casi può essere utile ripetere l'operazione; aggiungere al codice la logica di ripetizione dei tentativi. |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |L'operazione richiesta dall'utente non è consentita all'interno del server o del servizio. Per informazioni dettagliate, vedere il messaggio di eccezione. Ad esempio, [Complete()](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) genera questa eccezione se il messaggio è stato ricevuto in modalità [ReceiveAndDelete](/dotnet/api/microsoft.azure.servicebus.receivemode) . |Controllare il codice e la documentazione. Assicurarsi che l'operazione richiesta sia valida. |Il tentativo non è più utile. |
| [OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |È stato eseguito un tentativo di richiamare un'operazione su un oggetto già chiuso, interrotto o eliminato. In alcuni casi rari, la transazione di ambiente è già stata eliminata. |Controllare il codice e assicurarsi che non richiami le operazioni su un oggetto eliminato. |Il tentativo non è più utile. |
| [UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |L'oggetto [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) non è riuscito ad acquisire un token, il token non è valido oppure il token non contiene le attestazioni necessarie per eseguire l'operazione. |Assicurarsi che il provider di token sia stato creato con i valori corretti. Controllare la configurazione del servizio di controllo di accesso. |In alcuni casi può essere utile ripetere l'operazione; aggiungere al codice la logica di ripetizione dei tentativi. |
| [ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [ArgumentNullException](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[ArgumentOutOfRangeException](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Uno o più argomenti forniti al metodo non sono validi.<br /> L'URI fornito a [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) o [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) contiene segmenti di percorso.<br /> Lo schema URI fornito a [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) o [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) non è valido. <br />Il valore della proprietà è maggiore di 32 KB. |Controllare il codice chiamante e assicurarsi che gli argomenti siano corretti. |Il tentativo non è più utile. |
| [MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.servicebus.messagingentitynotfoundexception) |L'entità associata all'operazione non esiste o è stata eliminata. |Assicurarsi che l'entità esista. |Il tentativo non è più utile. |
| [MessageNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagenotfoundexception) |Tentativo di ricevere un messaggio con un particolare numero di sequenza. Questo messaggio non è stato trovato. |Verificare che il messaggio non sia già stato ricevuto. Controllare la coda dei messaggi non recapitabili per verificare che il messaggio non si trovi al suo interno. |Il tentativo non è più utile. |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) |Il client non è in grado di stabilire una connessione al bus di servizio. |Assicurarsi che il nome host fornito sia corretto e l'host sia raggiungibile. |Se sono presenti problemi di connettività intermittente, può essere utile ripetere l'operazione. |
| [ServerBusyException](/dotnet/api/microsoft.azure.servicebus.serverbusyexception) |Il servizio non è in grado di elaborare la richiesta in questo momento. |Il client può attendere per un certo periodo di tempo ed è quindi opportuno ripetere l'operazione. |Il client può riprovare dopo un determinato intervallo. Se viene generata un'eccezione diversa, controllare il comportamento di ripetizione del tentativo della nuova eccezione. |
| [MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception) |Il token di blocco associato al messaggio è scaduto oppure il token di blocco non è stato trovato. |Eliminare il messaggio. |Il tentativo non è più utile. |
| [SessionLockLostException](/dotnet/api/microsoft.azure.servicebus.sessionlocklostexception) |È stato perso il blocco associato a questa sessione. |Interrompere l'oggetto [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) . |Il tentativo non è più utile. |
| [MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception) |Eccezione di messaggistica generica che può essere generata nei casi seguenti:<br /> È stato eseguito un tentativo di creare una classe [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) usando un nome o un percorso appartenente a un tipo di entità diverso, ad esempio un argomento.<br />  È stato eseguito un tentativo di inviare un messaggio di dimensioni superiori a 256 kB. Si è verificato un errore nel server o nel servizio durante l'elaborazione della richiesta. Per informazioni dettagliate, vedere il messaggio di eccezione. Si tratta in genere di un'eccezione temporanea. |Controllare il codice e verificare che per il corpo del messaggio siano stati usati solo oggetti serializzabili (oppure usare un serializzatore personalizzato). Consultare la documentazione per identificare i tipi di valori delle proprietà supportati e usare solo quelli. Controllare la proprietà [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception) . Se è **true**, è possibile ripetere l'operazione. |Il comportamento di ripetizione dei tentativi non è definito e ripetere l'operazione può non essere utile. |
| [MessagingEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) |È stato eseguito un tentativo di creare un'entità con un nome già usato da un'altra entità dello stesso spazio dei nomi del servizio. |Eliminare l'entità esistente o scegliere un nome diverso per l'entità da creare. |Il tentativo non è più utile. |
| [QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) |L'entità di messaggistica ha raggiunto le dimensioni massime consentite oppure è stato superato il numero massimo di connessioni a uno spazio dei nomi. |Creare spazio nell'entità mediante la ricezione di messaggi dall'entità o dalle relative code secondarie. Vedere [QuotaExceededException](#quotaexceededexception). |Se nel frattempo sono stati rimossi i messaggi, può essere utile ripetere l'operazione. |
| [RuleActionException](/dotnet/api/microsoft.servicebus.messaging.ruleactionexception) |Il bus di servizio restituisce questa eccezione se si tenta di creare un'azione di regola non valida. Il bus di servizio assegna questa eccezione a un messaggio non recapitabile se si verifica un errore durante l'elaborazione dell'azione di regola per il messaggio. |Verificare la correttezza dell'azione di regola. |Il tentativo non è più utile. |
| [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) |Il bus di servizio restituisce questa eccezione se si tenta di creare un filtro non valido. Il bus di servizio assegna questa eccezione a un messaggio non recapitabile se si verifica un errore durante l'elaborazione del filtro per il messaggio. |Verificare la correttezza del filtro. |Il tentativo non è più utile. |
| [SessionCannotBeLockedException](/dotnet/api/microsoft.servicebus.messaging.sessioncannotbelockedexception) |È stato eseguito un tentativo di accettare una sessione con un ID di sessione specifico, ma la sessione è attualmente bloccata da un altro client. |Assicurarsi che la sessione venga sbloccata dagli altri client. |Se nel frattempo la sessione è stata rilasciata, può essere utile ripetere l'operazione. |
| [TransactionSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.transactionsizeexceededexception) |Nella transazione sono presenti troppe operazioni. |Ridurre il numero di operazioni presenti nella transazione. |Il tentativo non è più utile. |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.azure.servicebus.messagingentitydisabledexception) |È stata inoltrata una richiesta per un'operazione di runtime su un'entità disattivata. |Attivare l'entità. |Se nel frattempo l'entità è stata attivata, può essere utile ripetere l'operazione. |
| [NoMatchingSubscriptionException](/dotnet/api/microsoft.servicebus.messaging.nomatchingsubscriptionexception) |Il bus di servizio restituisce questa eccezione se si invia un messaggio a un argomento in cui è abilitato il filtro preliminare e nessuno dei filtri corrisponde. |Assicurarsi che almeno un filtro corrisponda. |Il tentativo non è più utile. |
| [MessageSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) |Un payload del messaggio supera il limite di 256 KB. Il limite di 256 KB rappresenta la dimensione totale del messaggio, che può includere proprietà di sistema ed eventuali overhead .NET. |Ridurre le dimensioni del payload del messaggio e quindi ripetere l'operazione. |Il tentativo non è più utile. |
| [TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx) |La transazione di ambiente (*Transaction.Current*) non è valida. È possibile che nel frattempo sia stata interrotta o completata. L'eccezione interna può fornire informazioni aggiuntive. | |Il tentativo non è più utile. |
| [TransactionInDoubtException](https://msdn.microsoft.com/library/system.transactions.transactionindoubtexception.aspx) |È stata tentata un'operazione su una transazione in dubbio oppure è stato tentato di eseguire il commit della transazione e la transazione è diventata in dubbio. |L'applicazione deve gestire questa eccezione (come caso speciale), poiché è possibile che sia già stato eseguito il commit della transazione. |- |

### <a name="quotaexceededexception"></a>QuotaExceededException
[QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) indica che è stata superata la quota di un'entità specifica.

#### <a name="queues-and-topics"></a>Code e argomenti
Per le code e gli argomenti, spesso si tratta della dimensione della coda. La proprietà del messaggio di errore contiene altri dettagli, come nell'esempio seguente:

```Output
Microsoft.ServiceBus.Messaging.QuotaExceededException
Message: The maximum entity size has been reached or exceeded for Topic: ‘xxx-xxx-xxx’. 
    Size of entity in bytes:1073742326, Max entity size in bytes:
1073741824..TrackingId:xxxxxxxxxxxxxxxxxxxxxxxxxx, TimeStamp:3/15/2013 7:50:18 AM
```

Il messaggio indica che l'argomento ha superato il limite di dimensioni, in questo caso 1 GB (limite di dimensioni predefinito). 

#### <a name="namespaces"></a>Spazi dei nomi

Per gli spazi dei nomi, [QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) può indicare che un'applicazione ha superato il numero massimo di connessioni a uno spazio dei nomi. Ad esempio:

```Output
Microsoft.ServiceBus.Messaging.QuotaExceededException: ConnectionsQuotaExceeded for namespace xxx.
<tracking-id-guid>_G12 ---> 
System.ServiceModel.FaultException`1[System.ServiceModel.ExceptionDetail]: 
ConnectionsQuotaExceeded for namespace xxx.
```

#### <a name="common-causes"></a>Cause comuni
Per questo errore, esistono due cause comuni: la coda dei messaggi non recapitabili e l'interruzione da parte dei destinatari dei messaggi.

1. **[Coda dei messaggi non recapitabili](service-bus-dead-letter-queues.md)** : un lettore non riesce a completare i messaggi e questi vengono restituiti alla coda o all'argomento alla scadenza del blocco. Può verificarsi se il lettore rileva un'eccezione che impedisce la chiamata a [BrokeredMessage. complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.complete). Dopo che un messaggio è stato letto 10 volte, si sposta alla coda dei messaggi non recapitabili per impostazione predefinita. Questo comportamento è controllato dalla proprietà [QueueDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount) e ha un valore predefinito di 10. Man mano che i messaggi si accumulano nella coda dei messaggi non recapitabili, occupano spazio.
   
    Per risolvere il problema, leggere e completare i messaggi nella coda dei messaggi non recapitabili, come si farebbe per qualsiasi altra coda. È possibile usare il metodo [FormatDeadLetterPath](/dotnet/api/microsoft.azure.servicebus.entitynamehelper.formatdeadletterpath) per la formattazione del percorso della coda dei messaggi non recapitabili.
2. **Interruzione da parte del destinatario**. Un destinatario ha interrotto la ricezione di messaggi da una coda o da una sottoscrizione. Per identificare questo problema, è necessario osservare la proprietà [QueueDescription.MessageCountDetails](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails) , che mostra la suddivisione completa dei messaggi. Se la proprietà [ActiveMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.activemessagecount) è alta o in crescita, i messaggi non vengono letti con la stessa velocità con cui vengono scritti.

### <a name="timeoutexception"></a>TimeoutException
Un'eccezione di tipo [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) indica che un'operazione avviata dall'utente richiede più tempo rispetto al timeout dell'operazione. 

Controllare il valore della proprietà [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit), in quanto il raggiungimento di questo limite può anche causare un evento di [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx).

#### <a name="queues-and-topics"></a>Code e argomenti
Per le code e gli argomenti, il timeout è specificato nella proprietà [MessagingFactorySettings.OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings), come parte della stringa di connessione, o tramite [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.azure.servicebus.servicebusconnectionstringbuilder). Il messaggio di errore stesso può variare, ma contiene sempre il valore di timeout specificato per l'operazione corrente. 

## <a name="connectivity-certificate-or-timeout-issues"></a>Problemi di connettività, certificato o timeout
I passaggi seguenti possono essere utili per la risoluzione dei problemi di connettività/certificato/timeout per tutti i servizi in *. servicebus.windows.net. 

- Passare a o [wget](https://www.gnu.org/software/wget/) `https://<yournamespace>.servicebus.windows.net/`. Consente di controllare se sono presenti problemi di filtro IP, rete virtuale o catena di certificati (più comuni quando si usa Java SDK).

    Esempio di messaggio riuscito:
    
    ```xml
    <feed xmlns="http://www.w3.org/2005/Atom"><title type="text">Publicly Listed Services</title><subtitle type="text">This is the list of publicly-listed services currently available.</subtitle><id>uuid:27fcd1e2-3a99-44b1-8f1e-3e92b52f0171;id=30</id><updated>2019-12-27T13:11:47Z</updated><generator>Service Bus 1.1</generator></feed>
    ```
    
    Esempio di messaggio di errore di errore:

    ```json
    <Error>
        <Code>400</Code>
        <Detail>
            Bad Request. To know more visit https://aka.ms/sbResourceMgrExceptions. . TrackingId:b786d4d1-cbaf-47a8-a3d1-be689cda2a98_G22, SystemTracker:NoSystemTracker, Timestamp:2019-12-27T13:12:40
        </Detail>
    </Error>
    ```
- Eseguire il comando seguente per verificare se una porta è bloccata sul firewall. Le porte utilizzate sono 443 (HTTPS), 5671 (AMQP) e 9354 (NET Messaging/SBMP). A seconda della libreria usata, vengono usate anche altre porte. Ecco il comando di esempio che controlla se la porta 5671 è bloccata. 

    ```powershell
    tnc <yournamespacename>.servicebus.windows.net -port 5671
    ```

    In Linux:

    ```shell
    telnet <yournamespacename>.servicebus.windows.net 5671
    ```
- Quando si verificano problemi di connettività intermittenti, eseguire il comando seguente per verificare se sono presenti pacchetti eliminati. Questo comando tenterà di stabilire 25 connessioni TCP diverse ogni secondo con il servizio. Quindi, è possibile controllare il numero di riuscite/non riuscite e vedere anche latenza di connessione TCP. È possibile scaricare lo strumento `psping` da [qui](/sysinternals/downloads/psping).

    ```shell
    .\psping.exe -n 25 -i 1 -q <yournamespace>.servicebus.windows.net:5671 -nobanner     
    ```
    È possibile utilizzare comandi equivalenti se si utilizzano altri strumenti, ad esempio `tnc`, `ping`e così via. 
- Ottenere una traccia di rete se i passaggi precedenti non sono utili e analizzarli tramite strumenti come [Wireshark](https://www.wireshark.org/). Se necessario, contattare [supporto tecnico Microsoft](https://support.microsoft.com/) . 


## <a name="next-steps"></a>Passaggi successivi

Per informazioni complete sull'API .NET del bus di servizio, vedere [Azure .NET API reference](/dotnet/api/overview/azure/service-bus) (Informazioni di riferimento sulle API di Azure per .NET).

Per altre informazioni sul [bus di servizio](https://azure.microsoft.com/services/service-bus/), vedere gli articoli seguenti:

* [Panoramica della messaggistica del bus di servizio](service-bus-messaging-overview.md)
* [Architettura del bus di servizio](service-bus-architecture.md)

