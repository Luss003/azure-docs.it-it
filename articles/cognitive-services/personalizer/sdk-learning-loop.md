---
title: 'Avvio rapido: Creare e usare un ciclo di apprendimento con SDK - Personalizza esperienze'
description: Questo argomento di avvio rapido illustra come creare e gestire una knowledge base usando l'SDK client.
ms.topic: quickstart
ms.date: 01/15/2020
zone_pivot_groups: programming-languages-set-six
ms.openlocfilehash: 7ebe22227b4323b2e6b1c3fc9ca31e171d1d97cd
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/21/2020
ms.locfileid: "77524870"
---
# <a name="quickstart-personalizer-client-library"></a>Avvio rapido: Libreria client di Personalizza esperienze

Visualizzare il contenuto personalizzato in questa guida di avvio rapido con il servizio Personalizza esperienze.

Introduzione alla libreria client di Personalizza esperienze. Seguire questi passaggi per installare il pacchetto e provare il codice di esempio per le attività di base.

 * API Classificazione: seleziona l'elemento migliore, da elementi di contenuto, in base alle informazioni in tempo reale fornite in merito al contenuto e al contesto.
 * API Premio: è possibile determinare il punteggio di ricompensa in base alle esigenze aziendali e quindi inviarlo a Personalizza esperienze con questa API. Questo punteggio può essere un singolo valore, ad esempio 1 per positivo e 0 per negativo, oppure un algoritmo creato in base alle esigenze aziendali.

::: zone pivot="programming-language-csharp"
[!INCLUDE [Get intent with C# SDK](./includes/quickstart-sdk-csharp.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [Get intent with Node.js SDK](./includes/quickstart-sdk-nodejs.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Get intent with Python SDK](./includes/quickstart-sdk-python.md)]
::: zone-end

## <a name="clean-up-resources"></a>Pulire le risorse

Se si vuole pulire e rimuovere una sottoscrizione a Servizi cognitivi, è possibile eliminare la risorsa o il gruppo di risorse. Eliminando il gruppo di risorse vengono eliminate anche tutte le altre risorse associate.

* [Portale](../cognitive-services-apis-create-account.md#clean-up-resources)
* [Interfaccia della riga di comando di Azure](../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
>[Funzionamento di Personalizza esperienze](how-personalizer-works.md)

* [Informazioni su Personalizza esperienze](what-is-personalizer.md)
* [In quali situazioni è possibile usare Personalizza esperienze?](where-can-you-use-personalizer.md)
* [Risoluzione dei problemi](troubleshooting.md)
* Il codice sorgente per questo esempio è disponibile su [GitHub](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/blob/master/quickstarts/python/sample.py).
