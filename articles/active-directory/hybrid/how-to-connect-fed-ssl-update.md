---
title: 'Azure AD Connect: Aggiornare il certificato SSL per una farm Active Directory Federation Services | Microsoft Docs'
description: Questo documento illustra la procedura necessaria per aggiornare il certificato SSL di una farm AD FS con Azure AD Connect.
services: active-directory
manager: daveba
editor: billmath
ms.assetid: 7c781f61-848a-48ad-9863-eb29da78f53c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/09/2018
ms.subservice: hybrid
author: billmath
ms.custom: seohack1
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4fcdf75586de9621bd0e42cba6fa9c80e5d30817
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595203"
---
# <a name="update-the-ssl-certificate-for-an-active-directory-federation-services-ad-fs-farm"></a>Aggiornare il certificato SSL per una farm Active Directory Federation Services (AD FS)

## <a name="overview"></a>Panoramica
Questo articolo descrive come usare Azure AD Connect per aggiornare il certificato SSL relativo a una farm Active Directory Federation Services (AD FS). È possibile usare lo strumento Azure AD Connect per aggiornare facilmente il certificato SSL per la farm AD FS anche il metodo di accesso utente selezionato non è AD FS.

È possibile eseguire l'intera operazione di aggiornamento del certificato SSL per la farm AD FS in tutti i server federativi e proxy applicazione Web in tre semplici passaggi:

![Tre passaggi](./media/how-to-connect-fed-ssl-update/threesteps.png)


>[!NOTE]
>Per altre informazioni sui certificati usati da AD FS, vedere [Informazioni sui certificati utilizzati da AD FS](https://technet.microsoft.com/library/cc730660.aspx).

## <a name="prerequisites"></a>Prerequisiti

* **Farm AD FS**: assicurarsi che la farm AD FS sia basata su Windows Server 2012 R2 o versioni successive.
* **Azure AD Connect**: assicurarsi che la versione di Azure AD Connect sia 1.1.553.0 o successiva. Si userà l'attività **Aggiorna il certificato SSL di AD FS**.

![Attività Aggiorna il certificato SSL di AD FS](./media/how-to-connect-fed-ssl-update/updatessltask.png)

## <a name="step-1-provide-ad-fs-farm-information"></a>Passaggio 1: Specificare informazioni sulla farm AD FS

Azure AD Connect tenta di ottenere automaticamente informazioni sulla farm AD FS nei modi seguenti:
1. Chiedendo informazioni sulla farm ad AD FS (Windows Server 2016 o versione successiva).
2. Facendo riferimento alle informazioni ottenute da esecuzioni precedenti, archiviate localmente con Azure AD Connect.

L'elenco dei server visualizzati può essere modificato aggiungendo o rimuovendo i server in modo da riflettere la configurazione corrente della farm AD FS. Non appena riesce a ottenere informazioni sui server, Azure AD Connect visualizza la connettività e lo stato corrente del certificato SSL.

![Informazioni sui server AD FS](./media/how-to-connect-fed-ssl-update/adfsserverinfo.png)

Se l'elenco contiene un server che non fa più parte della farm AD FS, fare clic su **Rimuovi** per eliminare il server dall'elenco dei server della farm AD FS.

![Server offline nell'elenco](./media/how-to-connect-fed-ssl-update/offlineserverlist.png)

>[!NOTE]
> La rimozione di un server dall'elenco dei server della farm AD FS in Azure AD Connect è un'operazione locale e aggiorna le informazioni sulla farm AD FS mantenute localmente da Azure AD Connect. A seguito di questa operazione Azure AD Connect non modifica la configurazione in AD FS.    

## <a name="step-2-provide-a-new-ssl-certificate"></a>Passaggio 2: Specificare un nuovo certificato SSL

Dopo aver confermato le informazioni sui server della farm AD FS, Azure AD Connect chiede il nuovo certificato SSL. Fornire un certificato PFX protetto da password per continuare l'installazione.

![Certificato SSL](./media/how-to-connect-fed-ssl-update/certificate.png)

Dopo aver specificato il certificato, Azure AD Connect esegue una serie di verifiche dei prerequisiti per assicurarsi che il certificato sia corretto per la farm AD FS:

-   Il nome soggetto o il nome soggetto alternativo del certificato deve corrispondere al nome del servizio federativo oppure il certificato deve contenere caratteri jolly.
-   Il certificato deve essere valido per più di 30 giorni.
-   La catena di certificati deve essere valida.
-   Il certificato deve essere protetto da password.

## <a name="step-3-select-servers-for-the-update"></a>Passaggio 3: Selezionare i server per l'aggiornamento

Il passaggio successivo consiste nel selezionare i server in cui deve essere aggiornato il certificato SSL. I server offline non possono essere selezionati per l'aggiornamento.

![Selezionare i server da aggiornare](./media/how-to-connect-fed-ssl-update/selectservers.png)

Dopo aver completato la configurazione, Azure AD Connect visualizza un messaggio che indica lo stato dell'aggiornamento e rende disponibile un'opzione per verificare l'accesso di AD FS.

![Configurazione completata](./media/how-to-connect-fed-ssl-update/configurecomplete.png)   

## <a name="faqs"></a>Domande frequenti

* **Quale deve essere il nome soggetto del certificato per il nuovo certificato SSL di AD FS?**

    Azure AD Connect verifica che il nome soggetto o il nome soggetto alternativo del certificato includa il nome del servizio federativo. Ad esempio, se il nome del servizio federativo è fs.contoso.com, il nome soggetto o il nome soggetto alternativo deve essere fs.contoso.com.  Sono accettati anche certificati con caratteri jolly.

* **Perché vengono nuovamente richieste le credenziali nella pagina del server WAP?**

    Se alle credenziali specificate per la connessione ai server AD FS non sono associati anche i privilegi necessari per gestire i server WAP, Azure AD Connect richiede credenziali con privilegi amministrativi per i server WAP.

* **Il server appare offline Cosa devo fare?**

    Se il server è offline, Azure AD Connect non può eseguire alcuna operazione. Se il server fa parte della farm AD FS, controllare la connettività al server. Dopo avere risolto il problema, premere l'icona di aggiornamento per aggiornare lo stato nella procedura guidata. Se il server faceva parte della farm in precedenza, ma ora non esiste più, fare clic su **Rimuovi** per eliminarlo dall'elenco dei server gestiti da Azure AD Connect. La rimozione di un server dall'elenco in Azure AD Connect non modifica la configurazione di AD FS. Se si usa AD FS in Windows Server 2016 o versioni successive, il server rimane nelle impostazioni di configurazione e verrà visualizzato nuovamente in occasione dell'esecuzione successiva dell'attività.

* **È possibile aggiornare un subset di server della farm con il nuovo certificato SSL?**

    Sì. In qualsiasi momento, è possibile eseguire nuovamente l'attività **Aggiorna certificato SSL** per aggiornare i server rimanenti. Nella pagina **Selezionare i server per l'aggiornamento del certificato SSL** è possibile ordinare l'elenco dei server in base a **Data di scadenza SSL** per accedere con facilità ai server non ancora aggiornati.

* **Un server che era stato rimosso durante l'esecuzione precedente è ancora visualizzato come offline ed elencato nella pagina dei server AD FS. Perché il server offline è ancora presente dopo essere stato rimosso?**

    La rimozione di un server dall'elenco di Azure AD Connect non lo rimuove dalla configurazione di AD FS. Azure AD Connect fa riferimento ad AD FS (Windows Server 2016 o versioni successive) per qualsiasi informazione sulla farm. Se il server è ancora presente nella configurazione di AD FS, verrà nuovamente incluso nell'elenco.  

## <a name="next-steps"></a>Passaggi successivi

- [Azure AD Connect e federazione](how-to-connect-fed-whatis.md)
- [Gestione e personalizzazione di Active Directory Federation Services con Azure AD Connect](how-to-connect-fed-management.md)

