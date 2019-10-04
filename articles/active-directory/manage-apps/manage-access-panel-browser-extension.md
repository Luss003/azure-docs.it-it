---
title: Risolvere i problemi di Azure Access Panel Extension per Internet Explorer | Microsoft Docs
description: Come usare Criteri di gruppo per distribuire il componente aggiuntivo di Internet Explorer per il portale App personali.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: mimart
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: a0269c87572e2a9242a54491103ae0fcc3637518
ms.sourcegitcommit: 0ebc62257be0ab52f524235f8d8ef3353fdaf89e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723914"
---
# <a name="troubleshoot-the-access-panel-extension-for-internet-explorer"></a>Risolvere i problemi di Access Panel Extension per Internet Explorer

Questo articolo semplifica la risoluzione dei problemi seguenti:

* Non è possibile accedere alle app tramite il portale App personali durante l'uso di Internet Explorer.
* Viene visualizzato il messaggio "Installa software" anche se è già stato installato il software.

Se sei un amministratore, vedere [come distribuire Access Panel Extension per Internet Explorer con criteri di gruppo](deploy-access-panel-browser-extension.md).

## <a name="run-the-diagnostic-tool"></a>Eseguire lo strumento di diagnostica

È possibile diagnosticare i problemi di installazione con Access Panel Extension scaricando ed eseguendo lo strumento di diagnostica del Pannello di accesso. 

Per scaricare e installare lo strumento di diagnostica:

1. [Selezionare questo collegamento per scaricare lo strumento di diagnostica.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
1. Aprire il file ed estrarre il contenuto nel computer.
1. Per eseguire lo strumento, fare clic sul file denominato *AccessPanelExtensionDiagnosticTool.js* e selezionare **aperta con** > **Microsoft Windows Based Script Host** .

    ![Apri con > Microsoft Windows Based Script Host](./media/manage-access-panel-browser-extension/open-access-panel-extension-diagnostic-tool.png)

1. Esaminare i risultati di diagnostica che vengono visualizzati e selezionare **Sì** per risolvere i problemi. Il **risultati della verifica** verrà visualizzata la finestra di dialogo con informazioni sulle operazioni da eseguire se l'estensione non funziona.  
1. Leggere il messaggio e selezionare **OK**.

## <a name="check-that-the-access-panel-extension-is-enabled"></a>Verificare che Access Panel Extension sia abilitato.

Per verificare di aver abilitato l'estensione Pannello di accesso in Internet Explorer:

1. In Internet Explorer, selezionare la **icona dell'ingranaggio** nell'angolo superiore destro della finestra e selezionare **Opzioni Internet**.
1. Andare alla **i programmi** scheda e selezionare **gestione dei componenti aggiuntivi**.
1. Selezionare **Access Panel Extension** nel **Microsoft Corporation** sezione e selezionare **abilitare**.
1. Per salvare le modifiche, chiudere tutte le finestre del browser Internet Explorer è aperto. La modifica avrà effetto alla successiva apertura di Internet Explorer.

## <a name="enable-extensions-for-inprivate-browsing"></a>Abilitare le estensioni per InPrivate Browsing

Per abilitare le estensioni per InPrivate Browsing:

1. In Internet Explorer, selezionare la **icona dell'ingranaggio** nell'angolo superiore destro della finestra e selezionare **Opzioni Internet**.
1. Andare alla **Privacy** scheda e verificare che il **disabilitare le estensioni e barre degli strumenti all'avvio di InPrivate Browsing** casella di controllo è deselezionata.
1. Per salvare le modifiche, chiudere tutte le finestre del browser Internet Explorer è aperto. La modifica avrà effetto alla successiva apertura di Internet Explorer.

## <a name="uninstall-the-access-panel-extension"></a>Disinstallare Access Panel Extension

Per disinstallare Access Panel Extension dal computer:

1. Nel Pannello di controllo, cercare *disinstallare*.
1. Nei risultati della ricerca, selezionare **Disinstalla un programma**.

    ![Selezionare un'opzione del programma di disinstallazione dal Pannello di controllo](./media/manage-access-panel-browser-extension/uninstall-program-control-panel.png)

1. Nell'elenco, selezionare **Access Panel Extension** e selezionare **Disinstalla**.

    ![Disinstallare Access Panel Extension](./media/manage-access-panel-browser-extension/uninstall-access-panel-extension.png)

1. È quindi possibile provare a installare di nuovo l'estensione per verificare se il problema è stato risolto.

Se si verificano problemi di disinstallazione dell'estensione, è possibile rimuovere anche tramite il [Microsoft Fix It](https://go.microsoft.com/?linkid=9779673) dello strumento.

## <a name="related-articles"></a>Articoli correlati

* [Accesso alle applicazioni e Single Sign-On con Azure Active Directory](what-is-single-sign-on.md)
* [Come distribuire Access Panel Extension per Internet Explorer con criteri di gruppo](deploy-access-panel-browser-extension.md)
