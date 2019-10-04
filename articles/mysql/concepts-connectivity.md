---
title: Gestione degli errori di connettività temporanei per Database di Azure per MySQL | Microsoft Docs
description: Informazioni su come gestire gli errori di connettività temporanei per Database di Azure per MySQL.
keywords: connessione mysql,stringa di connessione,problemi di connettività,errore temporaneo,errore di connessione
author: jan-eng
ms.author: janeng
ms.service: mysql
ms.topic: conceptual
ms.date: 11/09/2018
ms.openlocfilehash: 8942223ce233d424e2368e90d2fbac92b1a443f3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "60525456"
---
# <a name="handling-of-transient-connectivity-errors-for-azure-database-for-mysql"></a>Gestione degli errori di connettività temporanei per Database di Azure per MySQL

Questo articolo descrive come gestire gli errori temporanei nella connessione al Database di Azure per MySQL.

## <a name="transient-errors"></a>Errori temporanei

Un errore temporaneo è un errore che si risolve in modo autonomo. Questi errori generalmente si manifestano come una connessione al server di database che viene interrotta. Inoltre non è possibile aprire nuove connessioni a un server. Possono verificarsi errori temporanei, ad esempio nel caso di un errore hardware o di rete. Un altro motivo di errore potrebbe essere l'implementazione di una nuova versione di un servizio PaaS. La maggior parte di questi eventi viene attenuata automaticamente dal sistema in meno di 60 secondi. Una procedura consigliata per la progettazione e lo sviluppo di applicazioni nel cloud è la previsione di errori temporanei, presupponendo che possano accadere in qualsiasi componente e in qualunque momento e che si disponga della logica appropriata per affrontare tali situazioni.

## <a name="handling-transient-errors"></a>Gestione degli errori temporanei

Gli errori temporanei devono essere gestiti usando la logica di ripetizione dei tentativi. Situazioni da considerare:

* Si verifica un errore quando si tenta di aprire una connessione
* Una connessione inattiva viene eliminata sul lato server. Se si prova a eseguire un comando non si riesce a eseguirlo
* Una connessione attiva che sta attualmente eseguendo un comando viene eliminata.

Il primo e il secondo caso sono abbastanza semplici da gestire. Provare ad aprire di nuovo la connessione. Quando l'operazione riesce, l'errore temporaneo è stato attenuato dal sistema. ed è possibile usare nuovamente il Database di Azure per MySQL. È consigliabile attendere prima di riprovare a effettuare la connessione. Interrompere temporaneamente l'operazione se i tentativi iniziali hanno esito negativo. In questo modo il sistema può usare tutte le risorse disponibili per superare la situazione di errore. È consigliabile seguire questa procedura:

* Attendere 5 secondi prima di riprovare.
* Per ogni tentativo successivo, aumentare l'attesa in modo esponenziale, fino a 60 secondi.
* Impostare un numero massimo di tentativi nel punto in cui l'applicazione considera l'operazione non riuscita.

Quando una connessione con una transazione attiva ha esito negativo, la gestione corretta del ripristino è più difficile. I casi possibili sono due: Se la transazione è di sola lettura, è opportuno riaprire la connessione e ripetere la transazione. Se invece la transazione stava anche scrivendo nel database occorre stabilire se è stata sottoposta a rollback o se è riuscita prima che si verificasse l'errore temporaneo. In questo caso è possibile che la conferma di commit da parte del server di database non sia stata ricevuta.

Un modo per superare il problema consiste nel generare un ID univoco sul client usato per tutti i tentativi. Passare questo ID univoco come parte della transazione al server e archiviarlo in una colonna con un vincolo univoco. In questo modo è possibile riprovare a eseguire la transazione in tutta sicurezza. Il tentativo ha esito positivo se la transazione precedente è stata sottoposta a rollback e l'ID univoco generato dal client non esiste ancora nel sistema. Il tentativo ha esito negativo quando indica una violazione della chiave duplicata se l'ID univoco è stato già archiviato, perché la transazione precedente è stata completata correttamente.

Se il programma comunica con il Database di Azure per MySQL tramite middleware di terze parti, chiedere al fornitore se il middleware include la logica di ripetizione dei tentativi per errori temporanei.

Verificare di testare la logica di ripetizione dei tentativi. provando ad esempio a eseguire il codice mentre si ridimensionano le risorse di calcolo del server di Database di Azure per MySQL. L'applicazione deve gestire senza problemi il breve tempo di inattività che si verifica durante questa operazione.

## <a name="next-steps"></a>Passaggi successivi

* [Troubleshoot connection issues to Azure Database for MySQL](howto-troubleshoot-common-connection-issues.md) (Risolvere i problemi di connessione a database di Azure per MySQL)
