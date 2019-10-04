---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 05/06/2019
ms.author: wolfma
ms.openlocfilehash: d5a4b3a07854c2664de7ec60f3677b666798a9bd
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/18/2019
ms.locfileid: "67180437"
---
### <a name="standard-and-neural-voices"></a>Voices standard e neurale

Usare questa tabella per determinare la disponibilità di standard e neurale voices area/endpoint:

| Region | Endpoint | Voices standard | Voices neurale |
|--------|----------|-----------------|---------------|
| Australia orientale | `https://australiaeast.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Yes |
| Canada centrale | `https://canadacentral.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Yes |
| Stati Uniti centrali | `https://centralus.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | No |
| Asia orientale | `https://eastasia.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | No |
| Stati Uniti orientali | `https://eastus.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Yes |
| Stati Uniti orientali 2 | `https://eastus2.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | No |
| Francia centrale | `https://francecentral.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | No |
| India centrale | `https://centralindia.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Yes |
| Giappone orientale | `https://japaneast.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | No |
| Corea del Sud centrale | `https://koreacentral.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | No |
| Stati Uniti centro-settentrionali | `https://northcentralus.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | No |
| Europa settentrionale | `https://northeurope.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | No |
| Stati Uniti centro-meridionali | `https://southcentralus.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Yes |
| Asia sud-orientale | `https://southeastasia.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Yes |
| Regno Unito meridionale | `https://uksouth.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Yes |
| Europa occidentale | `https://westeurope.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Yes |
| Stati Uniti occidentali | `https://westus.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | No |
| Stati Uniti occidentali 2 | `https://westus2.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Yes |

### <a name="custom-voices"></a>Voci personalizzate

Se è stato creato un carattere voce personalizzato, usare l'endpoint creato. È anche possibile usare gli endpoint elencati di seguito, sostituendo il `{deploymentId}` con l'ID di distribuzione per il modello vocali.

| Region | Endpoint |
|--------|----------|
| Australia orientale | `https://australiaeast.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Canada centrale | `https://canadacentral.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Stati Uniti centrali | `https://centralus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Asia orientale | `https://eastasia.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Stati Uniti orientali | `https://eastus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Stati Uniti orientali 2 | `https://eastus2.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Francia centrale | `https://francecentral.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| India centrale | `https://centralindia.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Giappone orientale | `https://japaneast.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Corea del Sud centrale | `https://koreacentral.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Stati Uniti centro-settentrionali | `https://northcentralus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Europa settentrionale | `https://northeurope.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Stati Uniti centro-meridionali | `https://southcentralus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Asia sud-orientale | `https://southeastasia.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Regno Unito meridionale | `https://uksouth.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Europa occidentale | `https://westeurope.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Stati Uniti occidentali | `https://westus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Stati Uniti occidentali 2 | `https://westus2.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
