---
title: Microsoft Defender Advanced Threat Protection-Centro sicurezza di Azure
description: Questo documento introduce l'integrazione tra il Centro sicurezza di Azure e Microsoft Defender Advanced Threat Protection.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/24/2019
ms.author: memildin
ms.openlocfilehash: 13852acb39a420e2f0da84e18bef4df823c1fa78
ms.sourcegitcommit: 1fa2bf6d3d91d9eaff4d083015e2175984c686da
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2020
ms.locfileid: "78206266"
---
# <a name="microsoft-defender-advanced-threat-protection-with-azure-security-center"></a>Microsoft Defender Advanced Threat Protection con il Centro sicurezza di Azure

Il Centro sicurezza di Azure estende le piattaforme di protezione dei carichi di lavoro cloud offrendo l'integrazione con [Microsoft Defender Advanced Threat Protection](https://www.microsoft.com/microsoft-365/windows/microsoft-defender-atp) (ATP).
introducendo così funzionalità EDR (Endpoint Detection and Response) complete. Con l'integrazione di Microsoft Defender ATP è possibile individuare le anomalie. nonché rilevare e rispondere agli attacchi avanzati negli endpoint server monitorati dal Centro sicurezza di Azure.

## <a name="microsoft-defender-atp-features-in-security-center"></a>Funzionalità di Microsoft Defender ATP nel centro sicurezza

Quando si usa Microsoft Defender ATP, si ottiene quanto segue:

- **Sensori di rilevamento post-violazione avanzati**: i sensori di Microsoft Defender ATP per i server Windows raccolgono una vasta gamma di segnali comportamentali.

- **Rilevamento di violazioni post basate su analisi basate sul cloud**: Microsoft Defender ATP si adatta rapidamente alle mutevoli minacce. sfruttando strumenti di analisi avanzata e Big Data. Microsoft Defender ATP è stato amplificato dalla potenza del Intelligent Security Graph con segnali in Windows, Azure e Office per rilevare le minacce sconosciute. Fornisce avvisi interattivi e consente di reagire con tempestività.

- **Intelligence**per le minacce: Microsoft Defender ATP genera avvisi quando identifica gli strumenti, le tecniche e le procedure per gli utenti malintenzionati. Usa i dati generati dagli specialisti di minacce e dai team di sicurezza di Microsoft, integrati con intelligence fornita da partner.

Nel centro sicurezza di Azure sono ora disponibili le funzionalità seguenti:

- **Onboarding automatico**: il sensore Microsoft Defender ATP viene abilitato automaticamente per i server Windows caricati nel centro sicurezza di Azure.

- **Riquadro singolo**: nella console del Centro sicurezza di Azure vengono visualizzati gli avvisi di Microsoft Defender ATP.

- **Analisi dettagliata del computer**: i clienti del Centro sicurezza di Azure possono usare la console di Microsoft Defender ATP per condurre un'indagine dettagliata per individuare l'ambito di una violazione.

![Il Centro sicurezza di Azure, con un elenco di avvisi e informazioni generali relative a ogni avviso](media/security-center-wdatp/image1.png)

Per ulteriori approfondimenti, utilizzare Microsoft Defender ATP. Microsoft Defender ATP fornisce informazioni aggiuntive, ad esempio l'albero del processo di avviso e il grafico dell'evento imprevisto. È anche possibile visualizzare una sequenza temporale dettagliata che mostra ogni comportamento per un determinato periodo, fino a un massimo di sei mesi.

![Pagina di Microsoft Defender ATP con informazioni dettagliate su un avviso](media/security-center-wdatp/image3.png)

## <a name="platform-support"></a>Piattaforme supportate

Microsoft Defender ATP nel centro sicurezza supporta il rilevamento in Windows Server 2016, 2012 R2 e 2008 R2 SP1. per le macchine virtuali di Azure è necessaria una sottoscrizione di livello standard e per le macchine virtuali non di Azure è necessario un livello standard solo a livello di area di lavoro.

> [!NOTE]
> Quando si usa il Centro sicurezza di Azure per monitorare i server, viene creato automaticamente un tenant di Microsoft Defender ATP e i dati di Microsoft Defender ATP vengono archiviati in Europa per impostazione predefinita. Se è necessario spostare i dati in un'altra posizione, è necessario contattare supporto tecnico Microsoft per reimpostare il tenant. Il monitoraggio degli endpoint server che usa questa integrazione è stato disabilitato per i clienti di Office 365 GCC.

## <a name="onboarding-servers-to-security-center"></a>Onboarding dei server al Centro sicurezza 

Per caricare i server nel centro sicurezza, fare clic su **Vai al centro sicurezza di Azure per** caricare i server dall'onboarding del server Microsoft Defender ATP.

1. Nell'area **onboarding** selezionare o creare un'area di lavoro in cui archiviare i dati. <br>
2. Se non è possibile visualizzare tutte le aree di lavoro, ciò potrebbe dipendere dalla mancanza di autorizzazioni. Verificare che l'area di lavoro sia impostata sul livello Standard del Centro sicurezza di Azure. Per altre informazioni, vedere [eseguire l'aggiornamento al livello standard del Centro sicurezza per una maggiore sicurezza](security-center-pricing.md).
    
3. Selezionare **Aggiungi server** per visualizzare le istruzioni su come installare Microsoft Monitoring Agent. 

4. Dopo l'onboarding, è possibile monitorare i computer in **Compute and apps** (Servizi di calcolo e app).

   ![Onboarding di computer](media/security-center-wdatp/onboard-computers.png)

## <a name="enable-microsoft-defender-atp-integration"></a>Abilita l'integrazione di Microsoft Defender ATP

Per visualizzare se l'integrazione di Microsoft Defender ATP è abilitata, selezionare **Centro sicurezza** > **prezzi & impostazioni** > fare clic sulla sottoscrizione.
In questa schermata è possibile visualizzare le integrazioni attualmente abilitate.

  ![Pagina delle impostazioni di rilevamento delle minacce del Centro sicurezza di Azure con l'integrazione di Microsoft Defender ATP abilitata](media/security-center-wdatp/enable-integrations.png)

- Se si è già eseguito l'onboarding dei server nel livello Standard del Centro sicurezza di Azure, non è necessario eseguire altre operazioni. Il Centro sicurezza di Azure effettuerà automaticamente l'onboarding dei server a Microsoft Defender ATP. Il caricamento potrebbe richiedere fino a 24 ore.

- Se non è mai stato eseguito l'onboarding dei server nel livello Standard del Centro sicurezza di Azure, eseguirne l'onboarding nel Centro sicurezza di Azure come di consueto.

- Se i server sono stati caricati tramite Microsoft Defender ATP:
  - Vedere la documentazione per istruzioni su [come eseguire l'offboarding dei computer server](https://go.microsoft.com/fwlink/p/?linkid=852906).
  - Eseguire l'onboarding di questi server nel Centro sicurezza di Azure.

## <a name="access-to-the-microsoft-defender-atp-portal"></a>Accesso al portale di Microsoft Defender ATP

Seguire le istruzioni riportate in [Assegnare l'accesso degli utenti al portale](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/assign-portal-access).

## <a name="set-the-firewall-configuration"></a>Impostare la configurazione del firewall

Se si ha un proxy o un firewall che blocca il traffico anonimo, perché un sensore Microsoft Defender ATP si connette dal contesto del sistema, assicurarsi che sia consentito il traffico anonimo. Seguire le istruzioni in [abilitare l'accesso agli URL del servizio Microsoft Defender ATP nel server proxy](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/configure-proxy-internet#enable-access-to-microsoft-defender-atp-service-urls-in-the-proxy-server).

## <a name="test-the-feature"></a>Testare la funzionalità

Per generare un avviso di test di Microsoft Defender ATP benigno:

1. Creare una cartella ' C:\test-MDATP-test '.

1. Usare Desktop remoto per accedere a una macchina virtuale Windows Server 2012 R2 o Windows Server 2016. Aprire una finestra della riga di comando.

1. Al prompt copiare ed eseguire il comando seguente. La finestra del prompt dei comandi verrà chiusa automaticamente.

    ```
    powershell.exe -NoExit -ExecutionPolicy Bypass -WindowStyle Hidden (New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/1.exe', 'C:\\test-MDATP-test\\invoice.exe'); Start-Process 'C:\\test-MDATP-test\\invoice.exe'
    ```

   ![Finestra del prompt dei comandi con il comando precedente](media/security-center-wdatp/image4.jpeg)

3. Se il comando ha esito positivo, verrà visualizzato un nuovo avviso nel dashboard del Centro sicurezza di Azure e nel portale di Microsoft Defender ATP. Possono trascorrere alcuni minuti prima che l'avviso venga visualizzato.

4. Per esaminare l'avviso nel centro sicurezza, passare a **avvisi di sicurezza** > riga di comando di **PowerShell sospetta**.

5. Nella finestra di analisi selezionare il collegamento per passare al portale di Microsoft Defender ATP.

## <a name="next-steps"></a>Passaggi successivi

- [Funzionalità e piattaforme supportate dal Centro sicurezza di Azure](security-center-os-coverage.md)
- [Impostazione dei criteri di sicurezza nel Centro sicurezza di Azure](tutorial-security-policy.md): informazioni su come configurare i criteri di sicurezza per le sottoscrizioni e i gruppi di risorse di Azure.
- [Gestione delle raccomandazioni di sicurezza nel Centro sicurezza di Azure](security-center-recommendations.md): informazioni sul modo in cui le raccomandazioni semplificano la protezione delle risorse di Azure.
- [Monitoraggio dell'integrità della sicurezza nel Centro sicurezza di Azure](security-center-monitoring.md): informazioni su come monitorare l'integrità delle risorse di Azure.
