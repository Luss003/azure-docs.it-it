---
title: Configurare le impostazioni della tastiera Azure Media Clipper | Microsoft Docs
description: Passaggi per l'impostazione dei tasti di scelta rapida configurabili di Azure Media Clipper
services: media-services
keywords: clip;clip secondarie;codifica;file multimediali
author: Juliako
manager: femila
ms.author: juliako
ms.date: 03/14/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 013ca640d30baceeb41e9cb38fab69020b3c50dd
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73685065"
---
# <a name="configure-azure-media-clipper-keyboard-shortcuts"></a>Configurare le impostazioni della tastiera Azure Media Clipper 

Azure Media Clipper supporta la personalizzazione di tasti di scelta rapida predefiniti fornendo un parametro JSON `keymap` facoltativo.

Il file JSON di esempio seguente illustra i tasti di scelta rapida predefiniti. È possibile personalizzare queste impostazioni modificando i campi chiave e passando il parametro durante l'inizializzazione di Clipper.

```json
{
    "AZURE_MEDIA_SERVICE_SUBCLIPPER": {
        "UNDO": {
            "windows": "ctrl+z",
            "osx": "command+z"
        },
        "REDO": {
            "windows": "ctrl+shift+z",
            "osx": "command+shift+z"
        },
        "MARK_IN": {
            "windows": "ctrl+i",
            "osx": "command+i"
        },
        "MARK_OUT": {
            "windows": "ctrl+o",
            "osx": "command+o"
        },
        "MARK_IN_INPUT": {
            "windows": "ctrl+alt+i",
            "osx": "command+alt+i"
        },
        "MARK_OUT_INPUT": {
            "windows": "ctrl+alt+o",
            "osx": "command+alt+o"
        },
        "PREV_FRAME": {
            "windows": "ctrl+left",
            "osx": "command+left"
        },
        "NEXT_FRAME": {
            "windows": "ctrl+right",
            "osx": "command+right"
        },
        "SUBMIT_JOB": {
            "windows": "ctrl+s",
            "osx": "command+s"
        },
        "PLAY": {
            "windows": "ctrl+alt+p",
            "osx": "command+alt+p"
        },
        "PLAY_PREVIEW": {
            "windows": "ctrl+p",
            "osx": "command+p"
        },
        "MUTE": {
            "windows": "ctrl+m",
            "osx": "command+m"
        },
        "OUTPUT_TYPE": {
            "windows": "ctrl+y",
            "osx": "command+y"
        },
        "CLEAN_JOB": {
            "windows": "ctrl+alt+d",
            "osx": "command+alt+backspace"
        },
        "OPEN_ADVANCED_SETTINGS": {
            "windows": "ctrl+.",
            "osx": "command+."
        },
        "CLOSE_MODAL": "escape",
        "REMOVE_ASSET": {
            "windows": "ctrl+d",
            "osx": "command+backspace"
        },
        "ZOOM_LEFT_LESS": {
            "windows": "ctrl+shift+[",
            "osx": "command+shift+["
        },
        "ZOOM_LEFT_MORE": {
            "windows": "ctrl+[",
            "osx": "command+["
        },
        "ZOOM_RIGHT_LESS": {
            "windows": "ctrl+shift+]",
            "osx": "command+shift+]"
        },
        "ZOOM_RIGHT_MORE": {
           "windows": "ctrl+]",
            "osx": "command+]"
        }
    }
}
```
