---
title: 'Guida introduttiva: Progetto Anteprima URL, Node.js'
titlesuffix: Azure Cognitive Services
description: Introduzione all'uso di URL Preview in Servizi cognitivi Microsoft in Azure.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: url-preview
ms.topic: quickstart
ms.date: 03/16/2018
ms.author: rosh
ROBOTS: NOINDEX
ms.openlocfilehash: 4a1045be62f3bdbf75f399c894f825fa99f8e671
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/01/2019
ms.locfileid: "68706903"
---
# <a name="quickstart-url-preview-with-nodejs"></a>Guida introduttiva: Anteprima URL con Node.js 

L'esempio in Node seguente crea un'anteprima URL per il sito Web SwiftKey: https://swiftkey.com/en.

## <a name="prerequisites"></a>Prerequisiti

Ottenere una chiave di accesso per la versione di valutazione gratuita di [Lab di Servizi cognitivi](https://labs.cognitive.microsoft.com/en-us/project-answer-search)

## <a name="code-scenario"></a>Scenario di codice 

Il codice seguente ottiene i dati di URL Preview.
Viene implementato nei passaggi seguenti:
1. Dichiarare le variabili per specificare l'endpoint dall'host e il percorso.
2. Specificare l'URL della query da visualizzare in anteprima e aggiungere il parametro di query.  
3. Creare una funzione del gestore per la risposta.
4. Definire la funzione di ricerca che crea la richiesta e aggiunge l'intestazione *Ocp-Apim-Subscription-Key*.
5. Eseguire la funzione di ricerca. 

Il codice completo per questa demo è il seguente:

```
'use strict';

let https = require('https');

// Replace the subscriptionKey string value with your valid subscription key.
let subscriptionKey = 'YOUR-ACCESS-KEY'; 
let host = 'api.labs.cognitive.microsoft.com';
let path = '/urlpreview/v7.0/search';

let mkt = 'en-US';
let q = 'https://swiftkey.com/';

let params = '?q=' + encodeURI(q);

let response_handler = function (response) {
    let body = '';
    response.on('data', function (d) {
        body += d;
    });
    response.on('end', function () {
        let body_ = JSON.parse(body);
        let body__ = JSON.stringify(body_, null, '  ');
        console.log(body__);
    });
    response.on('error', function (e) {
        console.log('Error: ' + e.message);
    });
};

let Search = function () {
    let request_params = {
        method: 'GET',
        hostname: host,
        path: path + params,
        headers: {
            'Ocp-Apim-Subscription-Key': subscriptionKey,
        }
    };

    let req = https.request(request_params, response_handler);
    req.end();
}

Search();

```

## <a name="next-steps"></a>Passaggi successivi
- [Codice C# di esempio](csharp.md)
- [Guida introduttiva in Java](java-quickstart.md)
- [Guida introduttiva in JavaScript](javascript.md)
- [Guida introduttiva in Python](python-quickstart.md)
