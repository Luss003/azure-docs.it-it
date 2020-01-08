---
title: Panoramica delle definizioni dei ruoli personalizzati
description: Viene descritto il concetto di creazione di definizioni di ruolo personalizzate per le applicazioni gestite.
ms.topic: conceptual
ms.author: jobreen
author: jjbfour
ms.date: 09/16/2019
ms.openlocfilehash: 88e42fd9626276f6c77b46b33c138407f91d06ca
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/03/2020
ms.locfileid: "75650760"
---
# <a name="custom-role-definition-artifact-in-azure-managed-applications"></a>Elemento di definizione del ruolo personalizzato nelle applicazioni gestite di Azure

La definizione di ruolo personalizzata è un elemento facoltativo nelle applicazioni gestite. Viene usato per determinare le autorizzazioni necessarie all'applicazione gestita per eseguire le funzioni.

Questo articolo fornisce una panoramica dell'artefatto di definizione del ruolo personalizzato e delle relative funzionalità.

## <a name="custom-role-definition-artifact"></a>Artefatto di definizione del ruolo personalizzato

È necessario denominare l'artefatto di definizione del ruolo personalizzato customRoleDefinition. JSON. Posizionare lo stesso livello di createUiDefinition. JSON e mainTemplate. JSON nel pacchetto con estensione zip che consente di creare una definizione di applicazione gestita. Per informazioni su come creare il pacchetto zip e pubblicare una definizione di applicazione gestita, vedere [pubblicare una definizione di applicazione gestita.](publish-managed-app-definition-quickstart.md)

## <a name="custom-role-definition-schema"></a>Schema di definizione del ruolo personalizzato

Il file customRoleDefinition. JSON ha una proprietà di `roles` di primo livello che è una matrice di ruoli. Questi ruoli sono le autorizzazioni necessarie per il funzionamento dell'applicazione gestita. Attualmente, sono consentiti solo i ruoli predefiniti, ma è possibile specificare più ruoli. È possibile fare riferimento a un ruolo mediante l'ID della definizione di ruolo o il nome del ruolo.

Esempio di JSON per la definizione di ruolo personalizzata:

```json
{
    "contentVersion": "0.0.0.1",
    "roles": [
        {
            "properties": {
                "roleName": "Contributor"
            }
        },
        {
            "id": "acdd72a7-3385-48ef-bd42-f606fba81ae7"
        },
        {
            "id": "/providers/Microsoft.Authorization/roledefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
        }
    ]
}
```

## <a name="roles"></a>Ruoli

Un ruolo è costituito da un `$.properties.roleName` o da un `id`:

```json
{
    "id": null,
    "properties": {
        "roleName": "Contributor"
    }
}
```

> [!NOTE]
> È possibile usare il campo `id` o `roleName`. È necessario solo un. Questi campi vengono usati per cercare la definizione di ruolo da applicare. Se vengono specificati entrambi, verrà usato il campo `id`.

|Proprietà|Obbligatorio?|Description|
|---------|---------|---------|
|id|Sì|ID del ruolo predefinito. È possibile usare l'ID completo o solo il GUID.|
|roleName|Sì|Nome del ruolo predefinito.|