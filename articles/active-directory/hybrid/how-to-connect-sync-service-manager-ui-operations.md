---
title: Operazioni di Synchronization Service Manager per Azure AD Connect | Microsoft Docs
description: Comprendere la scheda Connettori in Synchronization Service Manager di Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 97a26565-618f-4313-8711-5925eeb47cdc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: 474000d1d4d7e1358682d1421125d482e3782049
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "60381409"
---
# <a name="using-the-sync-service-manager-operations-tab"></a>Uso della scheda Operazioni di Synchronization Service Manager

![Sync Service Manager](./media/how-to-connect-sync-service-manager-ui-operations/operations.png)

La scheda Operazioni mostra i risultati delle ultime operazioni. Questa scheda è fondamentale per comprendere e risolvere i problemi.

## <a name="understand-the-information-visible-in-the-operations-tab"></a>Comprendere le informazioni della scheda Operazioni
La metà superiore mostra tutte le esecuzioni in ordine cronologico. Per impostazione predefinita, il log delle operazioni mantiene le informazioni relative agli ultimi sette giorni, ma è possibile modificare questa impostazione tramite l' [utilità di pianificazione](how-to-connect-sync-feature-scheduler.md). Si vogliono cercare le esecuzioni che non hanno lo stato di operazione riuscita. È possibile modificare l'ordinamento facendo clic sulle intestazioni.

La colonna **Status** (Stato) visualizza le informazioni più importanti e segnala il problema più grave di un'esecuzione. Ecco un breve riepilogo degli stati disponibili, ordinati in base alla priorità con cui vanno analizzati (dove * indica diverse stringhe di errore possibili).

| Stato | Commento |
| --- | --- |
| stopped-\* |Impossibile completare l'esecuzione. Ad esempio se il sistema remoto è inattivo e non può essere contattato. |
| stopped-error-limit |Sono presenti più di 5.000 errori. L'esecuzione è stata arrestata automaticamente a causa dell'elevato numero di errori. |
| completato-\*-errori |L'esecuzione è stata completata, ma sono presenti errori (meno di 5.000) che devono essere analizzati. |
| completato-\*-avvisi |L'esecuzione è stata completata, ma alcuni dati non si trovano nello stato previsto. Se sono presenti errori, questo messaggio è in genere solo un sintomo. Evitare di analizzare gli avvisi fino a quando gli errori non sono stati risolti. |
| esito positivo |Non sono presenti problemi. |

Quando si seleziona una riga, la parte inferiore viene aggiornata per visualizzare i dettagli di quella esecuzione. All'estrema sinistra della parte inferiore è possibile avere un elenco con voci del tipo **Step #** (Passaggio [n.]). L'elenco è mostrato solo quando nella foresta sono presenti più domini e ogni dominio è rappresentato da un passaggio. Il nome di dominio è visibile sotto l'intestazione **Partition**(Partizione). In **Synchronization Statistics**(Statistiche di sincronizzazione) sono presenti altre informazioni sul numero delle modifiche elaborate. È possibile fare clic sui collegamenti per ottenere un elenco degli oggetti modificati. Se alcuni oggetti presentano errori, questi verranno visualizzati in **Synchronization Errors**(Errori di sincronizzazione).

Per altre informazioni, vedere [Risoluzione dei problemi relativi a un oggetto che non esegue la sincronizzazione in Azure AD](tshoot-connect-object-not-syncing.md)

## <a name="next-steps"></a>Passaggi successivi
Ulteriori informazioni sulla configurazione della [sincronizzazione di Azure AD Connect](how-to-connect-sync-whatis.md).

Ulteriori informazioni su [Integrazione delle identità locali con Azure Active Directory](whatis-hybrid-identity.md).
