---
title: 'Problemi noti: eseguire la migrazione da MongoDB ad Azure CosmosDB'
titleSuffix: Azure Database Migration Service
description: Informazioni sui problemi noti e sulle limitazioni della migrazione con le migrazioni da MongoDB a Azure Cosmos DB usando il servizio migrazione del database di Azure.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 02/27/2020
ms.openlocfilehash: 194da036260a78b27748dfc7f755212ab4f30b1e
ms.sourcegitcommit: d4a4f22f41ec4b3003a22826f0530df29cf01073
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/03/2020
ms.locfileid: "78256021"
---
# <a name="known-issuesmigration-limitations-with-migrations-from-mongodb-to-azure-cosmos-dbs-api-for-mongodb"></a>Problemi noti/limitazioni della migrazione con le migrazioni da MongoDB all'API di Azure Cosmos DB per MongoDB

Le sezioni seguenti illustrano i problemi noti e le limitazioni associate alle migrazioni da MongoDB all'API Cosmos DB per MongoDB.

## <a name="migration-fails-as-a-result-of-using-the-incorrect-ssl-cert"></a>La migrazione ha esito negativo a causa dell'utilizzo del certificato SSL errato

* **Sintomo**: questo problema è evidente quando un utente non è in grado di connettersi al server di origine MongoDB. Nonostante siano aperte tutte le porte del firewall, l'utente non è ancora in grado di connettersi.

| Causa         | Risoluzione |
| ------------- | ------------- |
| L'uso di un certificato autofirmato nel servizio migrazione del database di Azure può causare la mancata riuscita della migrazione a causa del certificato SSL errato. Il messaggio di errore può includere "il certificato remoto non è valido in base alla procedura di convalida". | Usare un certificato autentico dalla CA.  I certificati autofirmati vengono in genere utilizzati solo nei test interni. Quando si installa un certificato autentico da un'autorità di certificazione, è possibile usare il protocollo SSL nel servizio migrazione del database di Azure senza problemi (connessioni a Cosmos DB usare SSL sull'API Mongo).<br><br> |

## <a name="unable-to-get-the-list-of-databases-to-map-in-dms"></a>Non è possibile ottenere l'elenco dei database da mappare in DMS

* **Sintomo**: non è possibile ottenere l'elenco di database nel pannello delle **impostazioni del database** quando si usano **i dati della modalità di archiviazione di Azure** nel pannello **Seleziona origine** .

| Causa         | Risoluzione |
| ------------- | ------------- |
| Nella stringa di connessione dell'account di archiviazione mancano le informazioni sulla firma di accesso condiviso e pertanto non possono essere autenticate. | Creare la firma di accesso condiviso nel contenitore BLOB in Storage Explorer e usare l'URL con le informazioni sulla firma di accesso condiviso del contenitore come stringa di connessione del dettaglio di origine.<br><br> |

## <a name="using-an-unsupported-version-of-the-database"></a>Utilizzo di una versione non supportata del database

* **Sintomo**: la migrazione non riesce.

| Causa         | Risoluzione |
| ------------- | ------------- |
| Si tenta di eseguire la migrazione a Azure Cosmos DB da una versione non supportata di MongoDB. | Quando vengono rilasciate nuove versioni di MongoDB, vengono testate per garantire la compatibilità con il servizio migrazione del database di Azure e il servizio viene aggiornato periodicamente per accettare le versioni più recenti. Se è necessario eseguire la migrazione immediata, come soluzione alternativa è possibile esportare i database o le raccolte in archiviazione di Azure e puntare l'origine al dump risultante. Creare la firma di accesso condiviso nel contenitore BLOB in Storage Explorer e quindi usare l'URL con le informazioni sulla firma di accesso condiviso del contenitore come stringa di connessione del dettaglio di origine.<br><br> |

## <a name="next-steps"></a>Passaggi successivi

* Vedere l'esercitazione [eseguire la migrazione di MongoDB all'API Azure Cosmos DB per MongoDB online con DMS](tutorial-mongodb-cosmos-db-online.md).
* Vedere l'esercitazione [eseguire la migrazione di MongoDB all'API Azure Cosmos DB per MongoDB offline con DMS](tutorial-mongodb-cosmos-db.md).