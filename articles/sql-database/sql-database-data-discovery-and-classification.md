---
title: Individuazione dati e classificazione
description: Classificazione & di database SQL di Azure e individuazione dati
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
titleSuffix: Azure SQL Database and Azure Synapse
ms.devlang: ''
ms.topic: conceptual
author: DavidTrigano
ms.author: datrigan
ms.reviewer: vanto
ms.date: 02/05/2020
tags: azure-synapse
ms.openlocfilehash: e22205e81178ac0caff4b71462ece776238900f6
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/29/2020
ms.locfileid: "78191946"
---
# <a name="azure-sql-database-and-azure-synapse-analytics-data-discovery--classification"></a>Individuazione dati & classificazione del database SQL di Azure e di analisi delle sinapsi di Azure

Individuazione dati & classificazione offre funzionalità avanzate incorporate nel database SQL di **Azure per l'individuazione,** la **classificazione**, l' **assegnazione di etichette** & la **creazione di report** sui dati sensibili nei database.

L'individuazione e la classificazione dei dati più sensibili (dati commerciali, finanziari e relativi all'assistenza sanitaria, informazioni personali e così via) possono svolgere un ruolo fondamentale per il livello di protezione delle informazioni aziendali. Possono costituire l'infrastruttura per:

- Contribuire a soddisfare gli standard e i requisiti di conformità alle normative sulla privacy dei dati.
- Vari scenari di sicurezza, ad esempio monitoraggio (controllo) e invio di avvisi sulle anomalie di accesso a dati sensibili.
- Controllare l'accesso ai database che contengono dati riservati e rafforzare la sicurezza di questi.

L'individuazione dei dati & classificazione fa parte dell'offerta [Advanced Data Security](sql-database-advanced-data-security.md) (ADS), che è un pacchetto unificato per le funzionalità avanzate di sicurezza di SQL. È possibile accedere e gestire Individuazione dati e classificazione tramite il portale centrale di Sicurezza dei dati avanzata di SQL.

> [!NOTE]
> Questo documento è correlato al database SQL di Azure e alla sinapsi di Azure. Per semplicità, il database SQL viene usato quando si fa riferimento sia al database SQL che alla sinapsi di Azure. Per SQL Server (in locale), vedere [individuazione e classificazione dei dati SQL](https://go.microsoft.com/fwlink/?linkid=866999).

## <a id="subheading-1"></a>Informazioni su Individuazione dati e classificazione

Individuazione dati e classificazione introduce un set di servizi avanzati e nuove funzionalità SQL, che formano un nuovo paradigma di Information Protection per SQL finalizzato a proteggere i dati e non solo il database:

- **Individuazione e suggerimenti**

  Il motore di classificazione esegue l'analisi del database e identifica le colonne contenenti dati potenzialmente sensibili. Costituisce quindi un mezzo semplice e intuitivo per rivedere e applicare i suggerimenti di classificazione appropriati tramite il portale di Azure.

- **Assegnazione di etichette**

  Le colonne possono essere contrassegnate in modo permanente con etichette di classificazione della sensibilità tramite i nuovi attributi dei metadati di classificazione introdotti nel motore SQL. Questi metadati possono quindi essere usati per scenari avanzati di protezione e controllo basati sulla sensibilità.

- **Sensibilità del set di risultati della query**

  La sensibilità del set di risultati della query viene calcolata in tempo reale a scopo di controllo.

- **Visibilità**

  Lo stato di classificazione del database può essere visualizzato in un dashboard dettagliato nel portale. È possibile anche scaricare un report (in formato Excel) da usare a scopo di conformità e controllo o per altre esigenze.

## <a id="subheading-2"></a>Individuare, classificare e assegnare un'etichetta alle colonne sensibili

Nella sezione seguente vengono descritti i passaggi per l'individuazione, la classificazione e l'assegnazione di etichette a colonne del database contenenti dati sensibili, nonché per visualizzare lo stato di classificazione corrente del database e per esportare report.

La classificazione include due attributi di metadati:

- Etichette: gli attributi di classificazione principali, usati per definire il livello di riservatezza dei dati archiviati nella colonna.  
- Tipi di informazioni: offrono maggiore granularità del tipo di dati archiviati nella colonna.

## <a name="define-and-customize-your-classification-taxonomy"></a>Definire e personalizzare la tassonomia di classificazione

Individuazione dati e classificazione SQL include un set predefinito di etichette di riservatezza e un set predefinito di tipi di informazioni e logica di individuazione. Ora è possibile personalizzare la tassonomia e definire un set e una classificazione di pertinenza di costrutti di classificazione in modo specifico per l'ambiente.

La definizione e la personalizzazione della tassonomia di classificazione viene eseguita in un'unica posizione centrale per l'intero tenant di Azure. Si trova nel [Centro sicurezza di Azure](https://docs.microsoft.com/azure/security-center/security-center-intro) come parte dei criteri di sicurezza. Solo chi è in possesso dei diritti amministrativi per il gruppo di gestione radice di Tenant può eseguire questa attività.

Nell'ambito della gestione dei criteri di Information Protection, è possibile definire le etichette personalizzate, classificarle e associarle a un insieme selezionato di tipi di informazioni. È anche possibile aggiungere i tipi di informazioni personalizzate e configurarle con criteri di stringa, che vengono aggiunti alla logica di individuazione per l'identificazione di questo tipo di dati nei database.
Per altre informazioni sulla personalizzazione e la gestione di criteri, consulta la [Guida pratica dei criteri di Information Protection](https://go.microsoft.com/fwlink/?linkid=2009845&clcid=0x409).

Una volta definiti i criteri a livello di tenant, è possibile continuare con la classificazione dei singoli database usando il criterio personalizzato.

## <a name="classify-your-sql-database"></a>Classificare il database SQL

1. Accedere al [portale di Azure](https://portal.azure.com).

2. Passare a **Sicurezza dei dati avanzata** sotto l'intestazione Sicurezza nel riquadro del database SQL di Azure. Fare clic per abilitare la sicurezza avanzata dei dati, quindi fare clic sulla scheda **individuazione dati & classificazione** .

   ![Eseguire l'analisi di un database](./media/sql-data-discovery-and-classification/data_classification.png)

3. Nella scheda **Panoramica** è disponibile un riepilogo dello stato di classificazione corrente del database e un elenco dettagliato di tutte le colonne classificate, che è possibile anche filtrare in modo da visualizzare solo parti dello schema, tipi di informazioni o etichette specifiche. Se non è ancora stata classificata alcuna colonna, [andare al passaggio 5](#step-5).

   ![Riepilogo dello stato corrente di classificazione](./media/sql-data-discovery-and-classification/2_data_classification_overview_dashboard.png)

4. Per scaricare un report in formato Excel, fare clic sull'opzione **Esporta** nel menu superiore della finestra.

   ![Esportare in Excel](./media/sql-data-discovery-and-classification/3_data_classification_export_report.png)

5. <a id="step-5"></a>Per iniziare la classificazione dei dati, fare clic sulla scheda **Classificazione** nella parte superiore della finestra.

    ![Classificare i dati](./media/sql-data-discovery-and-classification/4_data_classification_classification_tab_click.png)

6. Il motore di classificazione esegue l'analisi del database per identificare eventuali colonne contenenti dati potenzialmente sensibili e fornisce un elenco di **classificazioni di colonna consigliate**. Per visualizzare e applicare i suggerimenti di classificazione:

   - Per visualizzare l'elenco delle classificazioni di colonna consigliate, fare clic sul pannello dei suggerimenti nella parte inferiore della finestra:

      ![Classificare i dati](./media/sql-data-discovery-and-classification/5_data_classification_recommendations_panel.png)

   - Esaminare l'elenco di suggerimenti: per accettare un suggerimento per una determinata colonna, selezionare la casella di controllo nella colonna sinistra della riga corrispondente. È anche possibile contrassegnare *tutti i consigli* come accettati selezionando la casella di controllo nell'intestazione della tabella dei consigli.

       ![Esaminare l'elenco degli elementi consigliati](./media/sql-data-discovery-and-classification/6_data_classification_recommendations_list.png)

   - Per applicare i consigli selezionati, fare clic sul pulsante blu **Accettare i consigli selezionati**.

      ![Applicare le raccomandazioni](./media/sql-data-discovery-and-classification/7_data_classification_accept_selected_recommendations.png)

7. È anche possibile **classificare manualmente** le colonne in alternativa o in aggiunta alla classificazione basata sui consigli:

   - Fare clic su **Aggiungi classificazione** nel menu superiore della finestra.

      ![Aggiungere manualmente la classificazione](./media/sql-data-discovery-and-classification/8_data_classification_add_classification_button.png)

   - Nella finestra di contesto che si apre selezionare lo schema, la tabella e la colonna che si vuole classificare, nonché il tipo di informazioni e l'etichetta di riservatezza. In seguito fare clic sul pulsante blu **Aggiungi classificazione** nella parte inferiore della finestra di contesto.

      ![Selezionare una colonna da classificare](./media/sql-data-discovery-and-classification/9_data_classification_manual_classification.png)

8. Per completare la classificazione e assegnare in modo permanente le etichette alle colonne del database con i nuovi metadati di classificazione, fare clic su **Salva** nel menu superiore della finestra.

   ![Salva](./media/sql-data-discovery-and-classification/10_data_classification_save.png)

## <a id="subheading-3"></a>Controllo dell'accesso ai dati sensibili

Un aspetto importante del paradigma di Information Protection è la possibilità di monitorare l'accesso ai dati sensibili. Il [servizio di controllo del database SQL di Azure](sql-database-auditing.md) è stato aggiornato e nel log di controllo è stato aggiunto un nuovo campo denominato *data_sensitivity_information*, in cui vengono registrate le classificazioni (etichette) di sensibilità dei dati effettivi restituiti dalla query.

![Log di controllo](./media/sql-data-discovery-and-classification/11_data_classification_audit_log.png)

## <a id="subheading-4"></a>Autorizzazioni

I ruoli predefiniti seguenti possono leggere la classificazione dei dati di un database SQL di Azure: `Owner`, `Reader`, `Contributor`, `SQL Security Manager` e `User Access Administrator`.

I ruoli predefiniti seguenti possono modificare la classificazione dei dati di un database SQL di Azure: `Owner`, `Contributor`, `SQL Security Manager`.

Scopri di più su [RBAC per le risorse di Azure](https://docs.microsoft.com/azure/role-based-access-control/overview)

## <a id="subheading-5"></a>Gestire le classificazioni

# <a name="t-sql"></a>[T-SQL](#tab/azure-t-sql)
È possibile utilizzare T-SQL per aggiungere o rimuovere le classificazioni di colonna, nonché recuperare tutte le classificazioni per l'intero database.

> [!NOTE]
> Quando si usa T-SQL per gestire le etichette, non vi è convalida che le etichette aggiunte a una colonna siano presenti nei criteri organizzativi di protezione delle informazioni (il set di etichette che compare nelle raccomandazioni del portale). La convalida spetta quindi all'utente.

- Aggiungere o aggiornare la classificazione di una o più colonne: [AGGIUNGI CLASSIFICAZIONE DI RISERVATEZZA](https://docs.microsoft.com/sql/t-sql/statements/add-sensitivity-classification-transact-sql)
- Rimuovere la classificazione da una o più colonne: [ELIMINA CLASSIFICAZIONE DI RISERVATEZZA](https://docs.microsoft.com/sql/t-sql/statements/drop-sensitivity-classification-transact-sql)
- Visualizzare tutte le classificazioni sul database: [sys.sensitivity_classifications](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql)

# <a name="rest-apis"></a>[API REST](#tab/azure-rest-api)
È possibile usare le API REST per gestire le classificazioni e le raccomandazioni a livello di codice. Le API REST pubblicate supportano le operazioni seguenti:

- [Crea o aggiorna](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/createorupdate) : crea o aggiorna l'etichetta di riservatezza di una determinata colonna
- [Eliminazione](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/delete): elimina l'etichetta Riservatezza di una colonna specifica
- [Disabilita Raccomandazione: Disabilita](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/disablerecommendation) le raccomandazioni di riservatezza in una colonna specifica
- [Abilita raccomandazione](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/enablerecommendation) : Abilita le raccomandazioni di riservatezza in una determinata colonna (le indicazioni sono abilitate per impostazione predefinita in tutte le colonne)
- [Recupero](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/get): ottiene l'etichetta Riservatezza di una colonna specifica
- [List Current By Database](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/listcurrentbydatabase) - (Elenca elementi correnti per database): consente di ottenere le etichette Riservatezza correnti di un database specifico
- [Elenco consigliato per database](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/listrecommendedbydatabase) : ottiene le etichette di riservatezza consigliate di un determinato database

# <a name="powershell-cmdlet"></a>[Cmdlet di PowerShell](#tab/azure-powelshell)
È possibile usare PowerShell per gestire le classificazioni e i consigli per il database SQL di Azure e Istanza gestita.

### <a name="powershell-cmdlet-for-azure-sql-database"></a>Cmdlet di PowerShell per il database SQL di Azure
- [Get-AzSqlDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasesensitivityclassification)
- [Set-AzSqlDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasesensitivityclassification)
- [Remove-AzSqlDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqldatabasesensitivityclassification)
- [Get-AzSqlDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasesensitivityrecommendation)
- [Enable-AzSqlDatabaSesensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/enable-azsqldatabasesensitivityrecommendation)
- [Disable-AzSqlDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/disable-azsqldatabasesensitivityrecommendation)

### <a name="powershell-cmdlets-for-managed-instance"></a>Cmdlet di PowerShell per Istanza gestita
- [Get-AzSqlInstanceDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabasesensitivityclassification)
- [Set-AzSqlInstanceDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstancedatabasesensitivityclassification)
- [Remove-AzSqlInstanceDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstancedatabasesensitivityclassification)
- [Get-AzSqlInstanceDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabasesensitivityrecommendation)
- [Enable-AzSqlInstanceDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/enable-azsqlinstancedatabasesensitivityrecommendation)
- [Disable-AzSqlInstanceDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/disable-azsqlinstancedatabasesensitivityrecommendation)

---

## <a id="subheading-6"></a>Passaggi successivi

- Altre informazioni su [Sicurezza dei dati avanzata](sql-database-advanced-data-security.md).
- Valutare l'opportunità di configurare [il servizio di controllo del database SQL di Azure](sql-database-auditing.md) per il monitoraggio e il controllo dell'accesso ai dati sensibili classificati.
- Per una presentazione di YouTube che include l'individuazione dei dati & classificazione, vedere [individuazione, classificazione, assegnazione di etichette & protezione dei dati SQL | Dati esposti](https://www.youtube.com/watch?v=itVi9bkJUNc).

<!--Anchors-->
[What is data discovery & classification]: #subheading-1
[Discovering, classifying & labeling sensitive columns]: #subheading-2
[Auditing access to sensitive data]: #subheading-3
[Permissions]: #subheading-4
[Manage classifications]: #subheading-5
[Next Steps]: #subheading-6
