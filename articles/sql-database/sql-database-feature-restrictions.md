---
title: Limitazioni delle funzionalità del database SQL di Azure
description: Limitazioni delle funzionalità del database SQL di Azure consente di migliorare la sicurezza del database limitando le funzionalità del database che possono essere da utenti malintenzionati per ottenere l'accesso alle informazioni in esse contenute.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: barmichal
ms.author: mibar
ms.reviewer: vanto
ms.date: 03/22/2019
ms.openlocfilehash: e9518065b2240d72698ed75f2fa8a7aed343b7bf
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73690055"
---
# <a name="azure-sql-database-feature-restrictions"></a>Limitazioni delle funzionalità del database SQL di Azure

Una fonte comune di attacchi SQL Server è costituita dalle applicazioni Web che accedono al database in cui vengono usate diverse forme di attacchi SQL Injection per raccogliere informazioni sul database.  Idealmente, il codice dell'applicazione viene sviluppato in modo da non consentire l'inserimento SQL injection.  Tuttavia, nelle basi di codice di grandi dimensioni che includono codice legacy ed esterno, non si può mai essere sicuri che tutti i case siano stati risolti, quindi gli inserimenti SQL sono un fatto di vita in cui è necessario proteggersi.  L'obiettivo delle restrizioni di funzionalità è impedire che alcune forme di attacchi SQL injection riportino informazioni sul database, anche in caso di esito positivo dell'attacco SQL injection.

## <a name="enabling-feature-restrictions"></a>Abilitazione delle restrizioni delle funzionalità

L'abilitazione delle restrizioni delle funzionalità viene eseguita usando il `sp_add_feature_restriction` stored procedure come indicato di seguito:

```sql
EXEC sp_add_feature_restriction <feature>, <object_class>, <object_name>
```

Le funzionalità seguenti possono essere limitate:

| Funzionalità          | Descrizione |
|------------------|-------------|
| N'ErrorMessages' | Con restrizioni, eventuali dati utente all'interno del messaggio di errore verranno mascherati. Vedere la [limitazione delle funzionalità dei messaggi di errore](#error-messages-feature-restriction) |
| N'Waitfor'       | Quando viene limitato, il comando restituisce immediatamente un risultato senza alcun ritardo. Vedere [restrizione delle funzionalità di aspetter](#waitfor-feature-restriction) |

Il valore di `object_class` può essere `N'User'` o `N'Role'` per indicare se `object_name` è un nome utente o un nome di ruolo nel database.

Nell'esempio seguente tutti i messaggi di errore per `MyUser` utente verranno mascherati:

```sql
EXEC sp_add_feature_restriction N'ErrorMessages', N'User', N'MyUser'
```

## <a name="disabling-feature-restrictions"></a>Disabilitazione delle restrizioni delle funzionalità

La disabilitazione delle restrizioni delle funzionalità viene eseguita utilizzando il `sp_drop_feature_restriction` stored procedure come indicato di seguito:

```sql
EXEC sp_drop_feature_restriction <feature>, <object_class>, <object_name>
```

Nell'esempio seguente viene disabilitata la maschera dei messaggi di errore per `MyUser`utente:

```sql
EXEC sp_drop_feature_restriction N'ErrorMessages', N'User', N'MyUser'
```

## <a name="viewing-feature-restrictions"></a>Visualizzazione delle restrizioni delle funzionalità

La visualizzazione `sys.sql_feature_restrictions` presenta tutte le restrizioni sulle funzionalità attualmente definite nel database. Sono incluse le colonne seguenti:

| Nome colonna | Tipo di dati | Descrizione |
|-------------|-----------|-------------|
| class       | nvarchar(128) | Classe dell'oggetto a cui viene applicata la restrizione |
| oggetto      | nvarchar(256) | Nome dell'oggetto a cui viene applicata la restrizione |
| Funzionalità     | nvarchar(128) | Funzionalità con restrizioni |

## <a name="feature-restrictions"></a>Limitazioni delle funzionalità

### <a name="error-messages-feature-restriction"></a>Restrizione della funzionalità dei messaggi di errore

Un metodo comune di attacco SQL injection consiste nell'inserire codice che causa un errore.  Esaminando il messaggio di errore, un utente malintenzionato può apprendere informazioni sul sistema, consentendo ulteriori attacchi mirati.  Questo attacco può essere particolarmente utile quando l'applicazione non Visualizza i risultati di una query, ma Visualizza i messaggi di errore.

Si consideri un'applicazione Web con una richiesta sotto forma di:

```html
http://www.contoso.com/employee.php?id=1
```

Che esegue la query di database seguente:

```sql
SELECT Name FROM EMPLOYEES WHERE Id=$EmpId
```

Se il valore passato come parametro `id` alla richiesta dell'applicazione Web viene copiato per sostituire $EmpId nella query del database, un utente malintenzionato potrebbe effettuare la richiesta seguente:

```html
http://www.contoso.com/employee.php?id=1 AND CAST(DB_NAME() AS INT)=0
```

Viene restituito l'errore seguente, che consente all'autore dell'attacco di apprendere il nome del database:

```sql
Conversion failed when converting the nvarchar value 'HR_Data' to data type int.
```

Una volta abilitata la restrizione delle funzionalità dei messaggi di errore per l'utente dell'applicazione nel database, il messaggio di errore restituito viene mascherato in modo che non vengano perse informazioni interne sul database:

```sql
Conversion failed when converting the ****** value '******' to data type ******.
```

Analogamente, l'autore dell'attacco potrebbe effettuare la richiesta seguente:

```html
http://www.contoso.com/employee.php?id=1 AND CAST(Salary AS TINYINT)=0
```

Viene restituito l'errore seguente, che consente all'autore dell'attacco di apprendere lo stipendio del dipendente:

```sql
Arithmetic overflow error for data type tinyint, value = 140000.
```

Utilizzando la restrizione della funzionalità dei messaggi di errore, il database restituirà:

```sql
Arithmetic overflow error for data type ******, value = ******.
```

### <a name="waitfor-feature-restriction"></a>Restrizione della funzionalità ASPETTER

Un attacco SQL cieco si verifica quando un'applicazione non fornisce un utente malintenzionato con i risultati dell'istruzione SQL inserita o con un messaggio di errore, ma l'autore dell'attacco può dedurre le informazioni dal database creando una query condizionale in cui i due branch condizionali richiedere una quantità di tempo diversa per l'esecuzione. Confrontando il tempo di risposta, l'autore dell'attacco può sapere quale ramo è stato eseguito e quindi ottenere informazioni sul sistema. La variante più semplice di questo attacco consiste nell'utilizzare l'istruzione `WAITFOR` per introdurre il ritardo.

Si consideri un'applicazione Web con una richiesta sotto forma di:

```html
http://www.contoso.com/employee.php?id=1
```

Che esegue la query di database seguente:

```sql
SELECT Name FROM EMPLOYEES WHERE Id=$EmpId
```

Se il valore passato come parametro ID alle richieste dell'applicazione Web viene copiato per sostituire $EmpId nella query del database, un utente malintenzionato può effettuare la richiesta seguente:

```html
http://www.contoso.com/employee.php?id=1; IF SYSTEM_USER='sa' WAITFOR DELAY '00:00:05'
```

E la query richiederebbe altri 5 secondi se è stato usato l'account `sa`. Se `WAITFOR` restrizione della funzionalità è disabilitata nel database, l'istruzione `WAITFOR` verrà ignorata e non verrà persa alcuna informazione utilizzando questo attacco.