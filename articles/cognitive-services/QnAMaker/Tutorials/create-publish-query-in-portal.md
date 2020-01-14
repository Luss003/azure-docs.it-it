---
title: 'Esercitazione: Creare, pubblicare e rispondere in QnA Maker'
titleSuffix: Azure Cognitive Services
description: Questa esercitazione illustra come creare una nuova knowledge base con domande e risposte frequenti basate sul Web pubblico. Salvare, eseguire il training della knowledge base e pubblicarla. Dopo la pubblicazione della knowledge base, inviare una domanda e ricevere una risposta con un comando cURL. Creare quindi un bot e testarlo con la stessa domanda.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: tutorial
ms.date: 10/14/2019
ms.author: diberry
ms.openlocfilehash: 5b2f2decf3d070da2fb58bebd424b4209f353316
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75447394"
---
# <a name="tutorial-from-the-qna-maker-portal-create-a-knowledge-base"></a>Esercitazione: Creare una knowledge base dal portale di QnA Maker

Creare una nuova knowledge base con domande e risposte frequenti basate sul Web pubblico. Salvare, eseguire il training della knowledge base e pubblicarla. Dopo la pubblicazione della knowledge base, inviare una domanda e ricevere una risposta con un comando cURL. Creare quindi un bot e testarlo con la stessa domanda. 

In questa esercitazione verranno illustrate le procedure per: 

> [!div class="checklist"]
> * Creare una knowledge base nel portale di QnA Maker.
> * Esaminare e salvare la knowledge base ed eseguirne il training.
> * Pubblicare la knowledge base.
> * Usare cURL per eseguire query sulla knowledge base.
> * Creare un bot.
 

> [!NOTE]
> La versione programmatica di questa esercitazione è disponibile con una soluzione completa nel [repository GitHub  **Azure-Samples/cognitive-services-qnamaker-csharo**](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/tutorials/create-publish-answer-knowledge-base).

## <a name="prerequisites"></a>Prerequisites

Questa esercitazione richiede un [servizio QnA Maker](../How-To/set-up-qnamaker-service-azure.md) esistente. 

## <a name="create-a-knowledge-base"></a>Creare una knowledge base 

1. Accedere al portale di [QnA Maker](https://www.qnamaker.ai). 

1. Selezionare **Create a knowledge base** (Creare una knowledge base) nel menu in alto.

    ![Screenshot del portale di QnA Maker](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-1.png)

1. Ignorare il primo passaggio perché verrà usato il servizio QnA Maker esistente. 

1. Selezionare le impostazioni esistenti:  

    |Impostazione|Scopo|
    |--|--|
    |Microsoft Azure Directory ID (ID directory Microsoft Azure)|Questo ID è associato all'account usato per accedere al portale di Azure e al portale di QnA Maker. |
    |Nome della sottoscrizione di Azure|Account di fatturazione in cui è stata creata la risorsa QnA Maker.|
    |Servizio Azure QnA|Risorsa QnA Maker esistente.|

    ![Screenshot del portale di QnA Maker](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-2.png)

1. Immettere il nome della knowledge base, `My Tutorial kb`.

    ![Screenshot del portale di QnA Maker](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-3.png)

1. Popolare la knowledge base con le impostazioni seguenti:  

    |Nome impostazione|Valore impostazione|Scopo|
    |--|--|--|
    |URL|`https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs` |Il contenuto delle domande frequenti in tale URL viene formattato con una domanda seguita da una risposta. QnA Maker può interpretare questo formato per estrarre le domande e le risposte associate.|
    |File |_Non usato in questa esercitazione_|Consente di caricare i file per le domande e le risposte. |
    |Personalità Chit-chat|Gentile|Dà una [personalità](../Concepts/best-practices.md#chit-chat) amichevole e informale alle domande e risposte comuni. È possibile modificare queste domande e risposte in un secondo momento. |

    ![Screenshot del portale di QnA Maker](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-4.png)

1. Selezionare **Create your KB** (Crea la knowledge base) per completare il processo di creazione.

    ![Screenshot del portale di QnA Maker](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-5.png)

## <a name="review-save-and-train-the-knowledge-base"></a>Esaminare, salvare ed eseguire il training della knowledge base

1. Rivedere le domande e le risposte. La prima pagina è costituita da domande e risposte dell'URL. 

    ![Screenshot del portale di QnA Maker](../media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb.png)

1. Selezionare l'ultima pagina di domande e risposte nella parte inferiore della tabella. La pagina mostra le domande e le risposte della personalità Chit-chat. 

1. Sulla barra degli strumenti sopra l'elenco di domande e risposte selezionare l'icona **View options** (Opzioni visualizzazione) e quindi **Show metadata** (Mostra metadati). Vengono visualizzati i tag dei metadati per ogni domanda e risposta. Per le domande Chit-chat sono già impostati i metadati **editorial: chit-chat**. Questi metadati vengono restituiti all'applicazione client insieme alla risposta selezionata. L'applicazione client, ad esempio un chat bot, può usare questi metadati filtrati per determinare altre elaborazioni o interazioni con l'utente.

    ![Screenshot del portale di QnA Maker](../media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb-chit-chat.png)

1. Selezionare **Save and train** (Salva ed esegui training) nella barra dei menu in alto.

## <a name="publish-to-get-knowledge-base-endpoints"></a>Eseguire la pubblicazione per ottenere gli endpoint della knowledge base

Selezionare il pulsante **Publish** (Pubblica) nel menu in alto. Nella pagina di pubblicazione selezionare **Pubblica**.

![Screenshot del portale di QnA Maker](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-1.png)

Dopo la pubblicazione della knowledge base viene visualizzato l'endpoint.

![Screenshot delle impostazioni dell'endpoint](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-2.png)

Non chiudere la pagina **Publish** (Pubblica). Sarà necessaria più avanti in questa esercitazione per creare un bot. 

## <a name="use-curl-to-query-for-an-faq-answer"></a>Usare cURL per eseguire una query per una risposta alle domande frequenti

1. Selezionare la scheda **Curl**. 

    ![Screenshot della scheda Curl](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-3-curl.png)

1. Copiare il testo della scheda **Curl** ed eseguirlo in una riga di comando o un terminale abilitato per cURL. Il valore dell'intestazione dell'autorizzazione include il testo `Endpoint` con uno spazio finale seguito dalla chiave.

1. Sostituire `<Your question>` con `How large can my KB be?`. Il testo è simile alla domanda, `How large a knowledge base can I create?`, ma non esattamente uguale. QnA Maker applica l'elaborazione del linguaggio naturale per determinare che le due domande sono le stesse.     

1. Eseguire il comando cURL e ricevere la risposta JSON, che include il punteggio e la risposta. 

    ```TXT
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   581  100   543  100    38    418     29  0:00:01  0:00:01 --:--:--   447{
      "answers": [
        {
          "questions": [
            "How large a knowledge base can I create?"
          ],
          "answer": "The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment)for more details.",
          "score": 42.81,
          "id": 2,
          "source": "https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs",
          "metadata": []
        }
      ]
    }
    
    ```

    QnA Maker è abbastanza attendibile con il punteggio di 42,81%.  

## <a name="use-curl-to-query-for-a-chit-chat-answer"></a>Usare cURL per eseguire una query per una risposta Chit-chat

1. Nel terminale abilitato per cURL sostituire `How large can my KB be?` con una frase dell'utente del bot di fine conversazione, ad esempio `Thank you`.   

1. Eseguire il comando cURL e ricevere la risposta JSON, che include il punteggio e la risposta. 

    ```TXT
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   525  100   501  100    24    525     25 --:--:-- --:--:-- --:--:--   550{
      "answers": [
        {
          "questions": [
            "Thank you",
            "Thanks",
            "Thnx",
            "Kthx",
            "I appreciate it",
            "Thank you so much",
            "I thank you",
            "My sincere thank"
          ],
          "answer": "You're very welcome.",
          "score": 100.0,
          "id": 109,
          "source": "qna_chitchat_the_friend.tsv",
          "metadata": [
            {
              "name": "editorial",
              "value": "chitchat"
            }
          ]
        }
      ]
    }
   
    ```

    Poiché la domanda di `Thank you` corrisponde esattamente a una domanda Chit chat, QnA Maker è assolutamente attendibile con il punteggio di 100. QnA Maker ha restituito anche tutte le domande correlate e la proprietà dei metadati contenente le informazioni dei tag di metadati Chit-chat.  

## <a name="use-curl-to-query-for-the-default-answer"></a>Usare cURL per eseguire una query per la risposta predefinita

A ogni domanda per cui QnA Maker non ottiene un punteggio ritenuto attendibile si riceve la risposta predefinita. Questa risposta viene configurata nel portale di Azure. 

1. Nel terminale abilitato per cURL sostituire `Thank you` con `x`. 

1. Eseguire il comando cURL e ricevere la risposta JSON, che include il punteggio e la risposta. 

    ```TXT
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   186  100   170  100    16    272     25 --:--:-- --:--:-- --:--:--   297{
      "answers": [
        {
          "questions": [],
          "answer": "No good match found in KB.",
          "score": 0.0,
          "id": -1,
          "metadata": []
        }
      ]
    }
    ```
    
    QnA Maker ha restituito il punteggio `0`, che indica nessuna attendibilità, nonché la risposta predefinita. 

## <a name="create-a-knowledge-base-bot"></a>Creare un bot della knowledge base

Per altre informazioni, vedere come [creare un chatbot con questa knowledge base](create-qna-bot.md).

## <a name="clean-up-resources"></a>Pulire le risorse

Al termine delle operazioni con il bot della knowledge base, rimuovere il gruppo di risorse, `my-tutorial-rg`, in modo da rimuovere tutte le risorse di Azure create nel processo bot.

Al termine delle operazioni con la knowledge base, nel portale di QnA Maker selezionare **My knowledge bases** (Knowledge base personali). Selezionare quindi la knowledge base **My Tutorial kb** e l'icona di eliminazione all'estrema destra della riga.  

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sui formati di file supportati, vedere [Origini dati supportate](../Concepts/data-sources-supported.md). 

Altre informazioni sulle [personalità](../Concepts/best-practices.md#chit-chat) Chit-chat.

Per altre informazioni sula risposta predefinita, vedere [Nessuna corrispondenza trovata](../Concepts/confidence-score.md#no-match-found). 

> [!div class="nextstepaction"]
> [Crea un chatbot con questa knowledge base](create-qna-bot.md)