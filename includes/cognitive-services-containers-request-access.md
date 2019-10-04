---
author: IEvangelist
ms.author: dapine
ms.service: cognitive-services
ms.topic: include
ms.date: 7/5/2019
ms.openlocfilehash: 993b0e8cc5b1ec482b2f6041dfc970dc7e7409dd
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/15/2019
ms.locfileid: "68229232"
---
Compilare e inviare il [modulo di richiesta di contenitori visione servizi cognitivi](https://aka.ms/VisionContainersPreview) per richiedere l'accesso al contenitore. Il modulo richiede informazioni sull'utente, sull'azienda e sullo scenario utente per cui si userà il contenitore. Dopo aver inviato il form, il team di servizi cognitivi di Azure esamina per assicurarsi che siano soddisfatti i criteri di accesso al registro contenitori privato.

> [!IMPORTANT]
> È necessario usare un indirizzo di posta elettronica associato con un Account Microsoft (MSA) o un account di Azure Active Directory (Azure AD) nel form.

Se la richiesta viene approvata, si riceverà un messaggio di posta elettronica con istruzioni che descrivono come ottenere le credenziali e accedere al registro contenitori privato.

## <a name="log-in-to-the-private-container-registry"></a>Accedere al registro contenitori privato

Esistono diversi modi per eseguire l'autenticazione con registro contenitori privato per i contenitori di servizi cognitivi. È consigliabile usare il metodo della riga di comando usando il [CLI di Docker](https://docs.docker.com/engine/reference/commandline/cli/).

Usare la [accesso a docker](https://docs.docker.com/engine/reference/commandline/login/) comando, come illustrato nell'esempio seguente, per accedere a `containerpreview.azurecr.io`, che è il registro contenitori privato per i contenitori di servizi cognitivi. Sostituire *\<username\>* con il nome utente e *\<password\>* con la password specificata nelle credenziali ricevute dal team di Servizi cognitivi di Azure.

```
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Se si è protetto le credenziali in un file di testo, è possibile concatenare il contenuto del file di testo per il `docker login` comando. Usare il `cat` comando, come illustrato nell'esempio seguente. Sostituire *\<passwordFile\>* con il percorso e nome del file di testo che contiene la password. Sostituire *\<username\>* con il nome utente fornito le credenziali.

```
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```

