---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 11/09/2018
ms.author: cephalin
ms.openlocfilehash: f42a97cdd74d360bc047ef561cbe626d526f9e4a
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/18/2019
ms.locfileid: "67180726"
---
Domain Name System (DNS) consente di individuare elementi su Internet. Se ad esempio si immette un indirizzo nel browser o si fa clic su un collegamento in una pagina Web, viene usato DNS per convertire il dominio in un indirizzo IP. L'indirizzo IP è paragonabile a un indirizzo postale, solo che non è altrettanto intuitivo. È ad esempio molto più facile ricordare un nome DNS come **contoso.com** anziché un indirizzo IP come 192.168.1.88 o 2001:0:4137:1f67:24a2:3888:9cce:fea3.

Il sistema DNS è basato su *record*. I record associano uno specifico *nome*, come **contoso.com**, a un indirizzo IP o a un altro nome DNS. Quando un'applicazione, ad esempio un Web browser, cerca un nome in DNS, trova il record e usa come indirizzo il valore a cui punta. Se il valore a cui punta è un indirizzo IP, il browser userà questo valore. Se punta a un altro nome DNS, l'applicazione deve risolverlo di nuovo. In ultima analisi, la risoluzione del nome terminerà sempre in un indirizzo IP.

A ogni sito Web di Azure creato verrà automaticamente assegnato un nome DNS. Questo nome avrà il formato **&lt;nomesito&gt;.azurewebsites.net**. Quando si aggiunge il sito Web a un endpoint di Gestione traffico di Azure, il sito Web diventa accessibile tramite il dominio **&lt;profilogestionetraffico&gt;.trafficmanager.net**.

> [!NOTE]
> Quando il sito Web è configurato come endpoint di Gestione traffico, si userà l'indirizzo **.trafficmanager.net** per la creazione di record DNS.
> 
> Con Gestione traffico è possibile usare unicamente record CNAME.
> 
> 

Sono anche disponibili più tipi di record, ognuno con funzioni e limitazioni specifiche, ma per i siti Web configurati come endpoint di Gestione traffico è importante solo un tipo, ovvero i record *CNAME* .

### <a name="cname-or-alias-record"></a>Record CNAME o Alias
Esegue il mapping di un record CNAME una *specifici* nome DNS, ad esempio **mail.contoso.com** oppure **www\.contoso.com**, a un altro nome di dominio (canonico). Nel caso in cui Siti Web di Azure venga usato con Gestione traffico di Azure, il nome di dominio canonico è **&lt;myapp>.trafficmanager.net** del profilo di Gestione traffico. Dopo la creazione, il record CNAME crea a sua volta un alias per il nome di dominio **&lt;myapp>.trafficmanager.net**. La voce CNAME viene automaticamente risolta nell'indirizzo IP del nome di dominio **&lt;myapp>.trafficmanager.net**, quindi se l'indirizzo IP del sito Web cambia non sarà necessaria alcuna azione.

Quando il traffico raggiunge Gestione traffico, quest'ultimo lo indirizza al sito Web usando il metodo di bilanciamento del carico per il quale è stato configurato. Il processo è completamente trasparente ai visitatori del sito Web, che vedranno unicamente il nome di dominio personalizzato nei propri browser.

> [!NOTE]
> Alcuni Registrar consentono solo di eseguire il mapping di sottodomini quando si usa un record CNAME, ad esempio **www\.contoso.com**e dei nomi non radice, ad esempio **contoso.com**. Per altre informazioni sui record CNAME, vedere la documentazione fornita dal registrar, la <a href="https://en.wikipedia.org/wiki/CNAME_record">voce di Wikipedia sui record CNAME</a> oppure il documento di IETF relativo a <a href="https://tools.ietf.org/html/rfc1035">implementazione e specifiche dei nomi di dominio</a>.

