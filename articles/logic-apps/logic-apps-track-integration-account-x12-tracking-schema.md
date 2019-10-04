---
title: Schemi di rilevamento X12 per i messaggi B2B - App per la logica di Azure | Microsoft Docs
description: Creare schemi di rilevamento X12 che consentono di monitorare i messaggi B2B negli account di integrazione per App per la logica di Azure con Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: a5413f80-eaad-4bcf-b371-2ad0ef629c3d
ms.date: 01/27/2017
ms.openlocfilehash: 1db324006e1e6332b5fdd8afd28ebed8a32ac707
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "60845767"
---
# <a name="create-schemas-for-tracking-x12-messages-in-integration-accounts-for-azure-logic-apps"></a>Creare schemi per il rilevamento di messaggi X12 negli account di integrazione per App per la logica di Azure

Per monitorare più facilmente il completamento delle operazioni, gli errori e le proprietà dei messaggi per le transazioni business-to-business (B2B) è possibile usare questi schemi di rilevamento X12 nell'account di integrazione:

* Schema di rilevamento X12 per set di transazioni
* Schema di rilevamento X12 per acknowledgement di set di transazioni
* Schema di rilevamento X12 per interscambio
* Schema di rilevamento X12 per acknowledgement di interscambio
* Schema di rilevamento X12 per gruppi funzionali
* Schema di rilevamento X12 per acknowledgement di gruppi funzionali

## <a name="x12-transaction-set-tracking-schema"></a>Schema di rilevamento X12 per set di transazioni

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "functionalGroupControlNumber": "",
      "transactionSetControlNumber": "",
      "CorrelationMessageId": "",
      "messageType": "",
      "isMessageFailed": "",
      "isTechnicalAcknowledgmentExpected": "",
      "isFunctionalAcknowledgmentExpected": "",
      "needAk2LoopForValidMessages":  "",
      "segmentsCount": ""
   }
}
```

| Proprietà | Type | Descrizione |
| --- | --- | --- |
| senderPartnerName | String | Nome partner del mittente del messaggio X12. Facoltativa |
| receiverPartnerName | String | Nome partner del destinatario del messaggio X12. Facoltativa |
| senderQualifier | String | Invia il qualificatore del partner. Obbligatoria |
| senderIdentifier | String | Invia l'identificatore del partner. Obbligatoria |
| receiverQualifier | String | Riceve il qualificatore del partner. (Obbligatoria) |
| receiverIdentifier | String | Riceve l'identificatore del partner. Obbligatoria |
| agreementName | String | Nome del contratto X12 in base al quale vengono risolti i messaggi. Facoltativa |
| direction | Enum | Direzione del flusso dei messaggi, ricezione o invio. Obbligatoria |
| interchangeControlNumber | String | Numero di controllo interscambio. Facoltativa |
| functionalGroupControlNumber | String | Numero di controllo funzionale. Facoltativa |
| transactionSetControlNumber | String | Numero di controllo set transazioni. Facoltativa |
| CorrelationMessageId | String | ID messaggio di correlazione. Una combinazione di {AgreementName}{*GroupControlNumber*}{TransactionSetControlNumber}. Facoltativa |
| messageType | String | Set di transazioni o tipo di documento. Facoltativa |
| isMessageFailed | Boolean | Indica se il messaggio X12 non è riuscito. Obbligatoria |
| isTechnicalAcknowledgmentExpected | Boolean | Indica se l'acknowledgement tecnico è configurato nel contratto X12. Obbligatoria |
| isFunctionalAcknowledgmentExpected | Boolean | Indica se l'acknowledgement funzionale è configurato nel contratto X12. Obbligatoria |
| needAk2LoopForValidMessages | Boolean | Indica se il ciclo AK2 è necessario per un messaggio valido. Obbligatoria |
| segmentsCount | Integer | Numero di segmenti nel set di transazioni X12. Facoltativa |
||||

## <a name="x12-transaction-set-acknowledgement-tracking-schema"></a>Schema di rilevamento X12 per acknowledgement di set di transazioni

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "functionalGroupControlNumber": "",
      "isaSegment": "",
      "gsSegment": "",
      "respondingfunctionalGroupControlNumber": "",
      "respondingFunctionalGroupId": "",
      "respondingtransactionSetControlNumber": "",
      "respondingTransactionSetId": "",
      "statusCode": "",
      "processingStatus": "",
      "CorrelationMessageId": "",
      "isMessageFailed": "",
      "ak2Segment": "",
      "ak3Segment": "",
      "ak5Segment": ""
   }
}
```

| Proprietà | Type | Descrizione |
| --- | --- | --- |
| senderPartnerName | String | Nome partner del mittente del messaggio X12. Facoltativa |
| receiverPartnerName | String | Nome partner del destinatario del messaggio X12. Facoltativa |
| senderQualifier | String | Invia il qualificatore del partner. Obbligatoria |
| senderIdentifier | String | Invia l'identificatore del partner. Obbligatoria |
| receiverQualifier | String | Riceve il qualificatore del partner. (Obbligatoria) |
| receiverIdentifier | String | Riceve l'identificatore del partner. Obbligatoria |
| agreementName | String | Nome del contratto X12 in base al quale vengono risolti i messaggi. Facoltativa |
| direction | Enum | Direzione del flusso dei messaggi, ricezione o invio. Obbligatoria |
| interchangeControlNumber | String | Numero di controllo interscambio dell'acknowledgement funzionale. Il valore viene popolato solo per il lato di invio che ha ricevuto un acknowledgement funzionale per il messaggio inviato al partner. Facoltativa |
| functionalGroupControlNumber | String | Numero di controllo del gruppo funzionale dell'acknowledgement funzionale. Il valore viene popolato solo per il lato di invio che ha ricevuto un acknowledgement funzionale per il messaggio inviato al partner. Facoltativa |
| isaSegment | String | Segmento ISA del messaggio. Il valore viene popolato solo per il lato di invio che ha ricevuto un acknowledgement funzionale per il messaggio inviato al partner. Facoltativa |
| gsSegment | String | Segmento GS del messaggio. Il valore viene popolato solo per il lato di invio che ha ricevuto un acknowledgement funzionale per il messaggio inviato al partner. Facoltativa |
| respondingfunctionalGroupControlNumber | String | Numero di controllo interscambio di risposta. Facoltativa |
| respondingFunctionalGroupId | String | ID del gruppo funzionale di risposta, che esegue il mapping ad AK101 nell'acknowledgement. Facoltativa |
| respondingtransactionSetControlNumber | String | Numero di controllo set transazioni di risposta. Facoltativa |
| respondingTransactionSetId | String | ID del set di transazioni di risposta, che esegue il mapping ad AK201 nell'acknowledgement. Facoltativa |
| statusCode | Boolean | Codice di stato dell'acknowledgement del set di transazioni. Obbligatoria |
| segmentsCount | Enum | Codice di stato dell'acknowledgement. I valori consentiti sono **Accepted**, **Rejected** e **AcceptedWithErrors**. Obbligatoria |
| processingStatus | Enum | Stato di elaborazione dell'acknowledgement. I valori consentiti sono **Received**, **Generated** e **Sent**. Obbligatoria |
| CorrelationMessageId | String | ID messaggio di correlazione. Una combinazione di {AgreementName}{*GroupControlNumber*}{TransactionSetControlNumber}. Facoltativa |
| isMessageFailed | Boolean | Indica se il messaggio X12 non è riuscito. Obbligatoria |
| ak2Segment | String | Acknowledgement per un set di transazioni nel gruppo funzionale ricevuto. Facoltativa |
| ak3Segment | String | Segnala gli errori in un segmento di dati. Facoltativa |
| ak5Segment | String | Indica se il set di transazioni identificato nel segmento AK2 è accettato o rifiutato e il motivo. Facoltativa |
||||

## <a name="x12-interchange-tracking-schema"></a>Schema di rilevamento X12 per interscambio

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "isaSegment": "",
      "isTechnicalAcknowledgmentExpected": "",
      "isMessageFailed": "",
      "isa09": "",
      "isa10": "",
      "isa11": "",
      "isa12": "",
      "isa14": "",
      "isa15": "",
      "isa16": ""
   }
}
```

| Proprietà | Type | Descrizione |
| --- | --- | --- |
| senderPartnerName | String | Nome partner del mittente del messaggio X12. Facoltativa |
| receiverPartnerName | String | Nome partner del destinatario del messaggio X12. Facoltativa |
| senderQualifier | String | Invia il qualificatore del partner. Obbligatoria |
| senderIdentifier | String | Invia l'identificatore del partner. Obbligatoria |
| receiverQualifier | String | Riceve il qualificatore del partner. (Obbligatoria) |
| receiverIdentifier | String | Riceve l'identificatore del partner. Obbligatoria |
| agreementName | String | Nome del contratto X12 in base al quale vengono risolti i messaggi. Facoltativa |
| direction | Enum | Direzione del flusso dei messaggi, ricezione o invio. Obbligatoria |
| interchangeControlNumber | String | Numero di controllo interscambio. Facoltativa |
| isaSegment | String | Segmento ISA del messaggio. Facoltativa |
| isTechnicalAcknowledgmentExpected | Boolean | Indica se l'acknowledgement tecnico è configurato nel contratto X12. Obbligatoria |
| isMessageFailed | Boolean | Indica se il messaggio X12 non è riuscito. Obbligatoria |
| isa09 | String | Data di interscambio del documento X12. Facoltativa |
| isa10 | String | Ora di interscambio del documento X12. Facoltativa |
| isa11 | String | Identificatore standard del controllo di interscambio X12. Facoltativa |
| isa12 | String | Numero di versione del controllo di interscambio X12. Facoltativa |
| isa14 | String | È necessario l'acknowledgement X12. Facoltativa |
| isa15 | String | Indicatore per test o produzione. Facoltativa |
| isa16 | String | Separatore elementi. Facoltativa |
||||

## <a name="x12-interchange-acknowledgement-tracking-schema"></a>Schema di rilevamento X12 per acknowledgement di interscambio

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "isaSegment": "",
      "respondingInterchangeControlNumber": "",
      "isMessageFailed": "",
      "statusCode": "",
      "processingStatus": "",
      "ta102": "",
      "ta103": "",
      "ta105": ""
   }
}
```

| Proprietà | Type | Descrizione |
| --- | --- | --- |
| senderPartnerName | String | Nome partner del mittente del messaggio X12. Facoltativa |
| receiverPartnerName | String | Nome partner del destinatario del messaggio X12. Facoltativa |
| senderQualifier | String | Invia il qualificatore del partner. Obbligatoria |
| senderIdentifier | String | Invia l'identificatore del partner. Obbligatoria |
| receiverQualifier | String | Riceve il qualificatore del partner. (Obbligatoria) |
| receiverIdentifier | String | Riceve l'identificatore del partner. Obbligatoria |
| agreementName | String | Nome del contratto X12 in base al quale vengono risolti i messaggi. Facoltativa |
| direction | Enum | Direzione del flusso dei messaggi, ricezione o invio. Obbligatoria |
| interchangeControlNumber | String | Numero di controllo interscambio dell'acknowledgement tecnico ricevuto dai partner. Facoltativa |
| isaSegment | String | Segmento ISA per l'acknowledgement tecnico ricevuto dai partner. (Facoltativa) |
| respondingInterchangeControlNumber |String | Numero di controllo interscambio per l'acknowledgement tecnico ricevuto dai partner. Facoltativa |
| isMessageFailed | Boolean | Indica se il messaggio X12 non è riuscito. Obbligatoria |
| statusCode | Enum | Codice di stato dell'acknowledgement di interscambio. I valori consentiti sono **Accepted**, **Rejected** e **AcceptedWithErrors**. Obbligatoria |
| processingStatus | Enum | Stato dell'acknowledgement. I valori consentiti sono **Received**, **Generated** e **Sent**. Obbligatoria |
| ta102 | String | Data di interscambio. Facoltativa |
| ta103 | String | Ora di interscambio. Facoltativa |
| ta105 | String | Codice della nota di interscambio. Facoltativa |
||||

## <a name="x12-functional-group-tracking-schema"></a>Schema di rilevamento X12 per gruppi funzionali

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "functionalGroupControlNumber": "",
      "gsSegment": "",
      "isTechnicalAcknowledgmentExpected": "",
      "isFunctionalAcknowledgmentExpected": "",
      "isMessageFailed": "",
      "gs01": "",
      "gs02": "",
      "gs03": "",
      "gs04": "",
      "gs05": "",
      "gs07": "",
      "gs08": ""
   }
}
```

| Proprietà | Type | Descrizione |
| --- | --- | --- |
| senderPartnerName | String | Nome partner del mittente del messaggio X12. Facoltativa |
| receiverPartnerName | String | Nome partner del destinatario del messaggio X12. Facoltativa |
| senderQualifier | String | Invia il qualificatore del partner. Obbligatoria |
| senderIdentifier | String | Invia l'identificatore del partner. Obbligatoria |
| receiverQualifier | String | Riceve il qualificatore del partner. (Obbligatoria) |
| receiverIdentifier | String | Riceve l'identificatore del partner. Obbligatoria |
| agreementName | String | Nome del contratto X12 in base al quale vengono risolti i messaggi. Facoltativa |
| direction | Enum | Direzione del flusso dei messaggi, ricezione o invio. Obbligatoria |
| interchangeControlNumber | String | Numero di controllo interscambio. Facoltativa |
| functionalGroupControlNumber | String | Numero di controllo funzionale. Facoltativa |
| gsSegment | String | Segmento GS del messaggio. Facoltativa |
| isTechnicalAcknowledgmentExpected | Boolean | Indica se l'acknowledgement tecnico è configurato nel contratto X12. Obbligatoria |
| isFunctionalAcknowledgmentExpected | Boolean | Indica se l'acknowledgement funzionale è configurato nel contratto X12. Obbligatoria |
| isMessageFailed | Boolean | Indica se il messaggio X12 non è riuscito. Obbligatoria|
| gs01 | String | Codice identificatore funzionale. Facoltativa |
| gs02 | String | Codice del mittente dell'applicazione. Facoltativa |
| gs03 | String | Codice del ricevitore dell'applicazione. Facoltativa |
| gs04 | String | Data del gruppo funzionale. Facoltativa |
| gs05 | String | Ora del gruppo funzionale. Facoltativa |
| gs07 | String | Codice agenzia responsabile. Facoltativa |
| gs08 | String | Codice identificatore versione/rilascio/settore. Facoltativa |
||||

## <a name="x12-functional-group-acknowledgement-tracking-schema"></a>Schema di rilevamento X12 per acknowledgement di gruppi funzionali

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "functionalGroupControlNumber": "",
      "isaSegment": "",
      "gsSegment": "",
      "respondingfunctionalGroupControlNumber": "",
      "respondingFunctionalGroupId": "",
      "isMessageFailed": "",
      "statusCode": "",
      "processingStatus": "",
      "ak903": "",
      "ak904": "",
      "ak9Segment": ""
   }
}
```

| Proprietà | Type | Descrizione |
| --- | --- | --- |
| senderPartnerName | String | Nome partner del mittente del messaggio X12. Facoltativa |
| receiverPartnerName | String | Nome partner del destinatario del messaggio X12. Facoltativa |
| senderQualifier | String | Invia il qualificatore del partner. Obbligatoria |
| senderIdentifier | String | Invia l'identificatore del partner. Obbligatoria |
| receiverQualifier | String | Riceve il qualificatore del partner. (Obbligatoria) |
| receiverIdentifier | String | Riceve l'identificatore del partner. Obbligatoria |
| agreementName | String | Nome del contratto X12 in base al quale vengono risolti i messaggi. Facoltativa |
| direction | Enum | Direzione del flusso dei messaggi, ricezione o invio. Obbligatoria |
| interchangeControlNumber | String | Numero di controllo interscambio, che viene popolato per il lato di invio quando un acknowledgement tecnico viene ricevuto dai partner. Facoltativa |
| functionalGroupControlNumber | String | Numero di controllo gruppo funzionale dell'acknowledgement tecnico, che viene popolato per il lato di invio quando un acknowledgement tecnico viene ricevuto dai partner. Facoltativa |
| isaSegment | String | Come il numero di controllo interscambio, ma viene popolato solo in casi specifici. Facoltativa |
| gsSegment | String | Come il numero di controllo gruppo funzionale, ma viene popolato solo in casi specifici. Facoltativa |
| respondingfunctionalGroupControlNumber | String | Numero di controllo del gruppo funzionale originale. Facoltativa |
| respondingFunctionalGroupId | String | Esegue il mapping ad AK101 nell'ID del gruppo funzionale dell'acknowledgement. Facoltativa |
| isMessageFailed | Boolean | Indica se il messaggio X12 non è riuscito. Obbligatoria |
| statusCode | Enum | Codice di stato dell'acknowledgement. I valori consentiti sono **Accepted**, **Rejected** e **AcceptedWithErrors**. Obbligatoria |
| processingStatus | Enum | Stato di elaborazione dell'acknowledgement. I valori consentiti sono **Received**, **Generated** e **Sent**. Obbligatoria |
| ak903 | String | Numero di set transazioni ricevuti. Facoltativa |
| ak904 | String | Numero di set di transazioni accettati nel gruppo funzionale identificato. Facoltativa |
| ak9Segment | String | Indica se il gruppo funzionale identificato nel segmento AK1 è accettato o rifiutato e il motivo. Facoltativa |
|||| 

## <a name="b2b-protocol-tracking-schemas"></a>Schemi di rilevamento per il protocollo B2B

Per informazioni sugli schemi di rilevamento per il protocollo B2B, vedere:

* [Schemi di rilevamento AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [Schemi di rilevamento personalizzati B2B](logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Passaggi successivi

* Altre informazioni sul [monitoraggio dei messaggi B2B](logic-apps-monitor-b2b-message.md).
* Scopri [rilevamento dei messaggi B2B in log di monitoraggio di Azure](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).
