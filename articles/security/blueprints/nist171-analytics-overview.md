---
title: Modello Blueprint per sicurezza e conformità di Azure - Analisi dei dati per NIST SP 800-171
description: Modello Blueprint per sicurezza e conformità di Azure - Analisi dei dati per NIST SP 800-171
services: security
author: jomolesk
ms.assetid: ca75d2a8-4e20-489b-9a9f-3a61e74b032d
ms.service: security
ms.topic: article
ms.date: 07/31/2018
ms.author: jomolesk
ms.openlocfilehash: 0bed9f96ce04fae313672f2fa627c2e20bea2f6f
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/04/2019
ms.locfileid: "73496422"
---
# <a name="azure-security-and-compliance-blueprint---data-analytics-for-nist-sp-800-171"></a>Modello Blueprint per sicurezza e conformità di Azure - Analisi dei dati per NIST SP 800-171

## <a name="overview"></a>Panoramica
Il documento [NIST Special Publication 800-171](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-171.pdf) fornisce indicazioni per la protezione di informazioni non classificate controllate (CUI, Controlled Unclassified Information) presenti in organizzazioni e sistemi informatici non federali. Il documento NIST SP 800-171 stabilisce 14 famiglie di requisiti di sicurezza per proteggere la riservatezza delle informazioni non classificate controllate.

Questo progetto per la sicurezza e la conformità di Azure fornisce indicazioni per aiutare i clienti a distribuire un'architettura di analisi dei dati in Azure che implementa un subset di controlli NIST SP 800-171. Questa soluzione illustra i modi in cui i clienti possono soddisfare requisiti specifici di sicurezza e conformità. I clienti possono anche usarla come base per creare e configurare le proprie soluzioni di analisi dei dati in Azure.

Questa architettura di riferimento, la guida all'implementazione associata e il modello di minaccia devono essere considerati soluzioni di base che i clienti possono adattare ai propri requisiti specifici. Non devono essere usati così come sono in un ambiente di produzione. La distribuzione di questa architettura senza apportare modifiche è insufficiente a soddisfare completamente i requisiti di NIST SP 800-171. I clienti hanno la responsabilità di eseguire valutazioni appropriate della sicurezza e della conformità di qualsiasi soluzione creata che usa questa architettura. I requisiti possono variare in base alle specifiche di implementazione di ogni cliente.

## <a name="architecture-diagram-and-components"></a>Diagramma e componenti dell'architettura
Questa soluzione offre una piattaforma di analisi su cui i clienti possono creare i propri strumenti. L'architettura di riferimento descrive un caso d'uso generico. I clienti possono usarla per inserire i dati con importazioni di dati in blocco tramite l'amministratore di SQL/dati. È anche possibile usarla per inserire i dati con gli aggiornamenti dei dati operativi tramite un utente operativo. Entrambe le sequenze di lavoro incorporano Funzioni di Azure per l'importazione di dati nel database SQL di Azure. È necessario configurare Funzioni di Azure nel portale di Azure per gestire le attività di importazione univoche per i singoli requisiti di analisi del cliente.

Azure offre svariati servizi di creazione di report e di analisi per i clienti. Questa soluzione USA Azure Machine Learning e il database SQL per esplorare rapidamente i dati e offrire risultati più rapidi grazie alla modellazione dei dati più intelligente. Machine Learning è progettato per aumentare la velocità di query individuando nuove relazioni tra i set di dati. Inizialmente, viene eseguito il training dei dati attraverso varie funzioni statistiche. In seguito, è possibile sincronizzare fino a sette pool di query con gli stessi modelli tabulari per distribuire il carico di lavoro delle query e ridurre i tempi di risposta. Il server del cliente porta a otto il totale dei pool di query.

Per operazioni di analisi e di report avanzate, il database SQL di Azure può essere configurato con indici dell'archivio colonne. Azure Machine Learning e il database SQL di Azure possono essere ridimensionati o disattivati completamente in risposta all'utilizzo del cliente. Tutto il traffico SQL viene crittografato con il protocollo SSL tramite l'inclusione di certificati autofirmati. È consigliabile usare un'Autorità di certificazione attendibile per una sicurezza avanzata.

Dopo aver caricato i dati nel database SQL di Azure e averne eseguito il training tramite Azure Machine Learning, ai dati viene applicato un digest da parte dell'utente operativo e dall'amministratore SQL/dati con Power BI. Power BI consente di visualizzare i dati in modo intuitivo e raggruppa le informazioni tra più set di dati per realizzare schemi più dettagliati. Power BI ha un elevato livello di adattabilità e si integra facilmente con il database SQL di Azure. I clienti possono configurarlo in modo da gestire un'ampia gamma di scenari necessari per le proprie esigenze aziendali.

L'intera soluzione si basa su Archiviazione di Azure che i clienti possono configurare nel portale di Azure. Archiviazione di Azure crittografa tutti i dati tramite la crittografia del servizio di archiviazione per mantenere la riservatezza dei dati inattivi. L'archiviazione con ridondanza geografica garantisce che il verificarsi di un evento avverso nel data center primario del cliente non comporti la perdita di dati. Una seconda copia viene archiviata in una posizione separata distante centinaia di chilometri.

Per una sicurezza ottimale, tutte le risorse in questa soluzione vengono gestite come gruppo di risorse tramite Azure Resource Manager. Il controllo degli accessi in base al ruolo di Azure Active Directory (Azure AD) viene usato per controllare l'accesso alle risorse distribuite. Queste risorse includono le chiavi dei clienti in Azure Key Vault. L'integrità del sistema viene monitorata tramite Centro sicurezza di Azure e Monitoraggio di Azure. I clienti configurano entrambi i servizi di monitoraggio per acquisire i log. L'integrità del sistema viene visualizzata in un unico dashboard facile da usare.

Il database SQL di Azure è in genere gestito tramite SQL Server Management Studio. Viene eseguito da un computer locale configurato per accedere al database SQL tramite una VPN sicura o una connessione Azure ExpressRoute. *Si consiglia di configurare una connessione ExpressRoute o VPN per gestire e importare i dati nel gruppo di risorse*.

![Diagramma dell'architettura di riferimento di Data Analytics per NIST SP 800-171](images/nist171-analytics-architecture.png "Diagramma dell'architettura di riferimento di Data Analytics per NIST SP 800-171")

Questa soluzione usa i servizi di Azure seguenti. Per altre informazioni, vedere la sezione relativa all'[architettura di distribuzione](#deployment-architecture).

- Application Insights
- Azure Active Directory
- Azure Data Catalog
- Azure Disk Encryption
- Griglia di eventi di Azure
- Funzioni di Azure
- Azure Key Vault
- Azure Machine Learning
- Monitoraggio di Azure (log)
- Centro sicurezza di Azure
- Database SQL di Azure
- Archiviazione di Azure
- Rete virtuale di Azure
    - (1) Rete /16
    - (2) Reti /24
    - (2) Gruppi di sicurezza di rete
- dashboard di Power BI

## <a name="deployment-architecture"></a>Architettura di distribuzione
La sezione seguente descrive in modo dettagliato gli elementi di sviluppo e implementazione.

**Griglia di eventi di Azure**: con [Griglia di eventi](https://docs.microsoft.com/azure/event-grid/overview) i clienti possono compilare facilmente applicazioni con architetture basate su eventi. Gli utenti selezionano la risorsa di Azure a cui desiderano eseguire la sottoscrizione. Assegnano quindi un endpoint al gestore dell'evento o al webhook a cui inviare l'evento. I clienti possono proteggere gli endpoint del webhook aggiungendo i parametri di query all'URL del webhook durante la creazione di una sottoscrizione di eventi. Griglia di eventi supporta solo endpoint del webhook HTTPS. Con la Griglia di eventi, i clienti possono controllare il livello di accesso assegnato a utenti diversi per eseguire varie operazioni di gestione. Gli utenti possono elencare le sottoscrizioni di eventi, crearne di nuovi e generare le chiavi. Griglia di eventi usa il controllo degli accessi in base al ruolo Azure.

**Funzioni di Azure**: [Funzioni di Azure](https://docs.microsoft.com/azure/azure-functions/functions-overview) è un servizio di calcolo serverless che esegue codice su richiesta. Non è necessario eseguire il provisioning o gestire l'infrastruttura in modo esplicito. Usare Funzioni di Azure per eseguire uno script o una porzione di codice in risposta a diversi eventi.

**Azure Machine Learning**: [Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/) è una tecnica di data science che consente ai computer di usare i dati esistenti per prevedere comportamenti, tendenze e risultati futuri.

**Azure Data Catalog**: [Azure Data Catalog](../../data-catalog/overview.md) rende le origini dati facilmente individuabili e comprensibili per gli utenti che gestiscono i dati. Le origini dati comuni possono essere registrate, contrassegnate con tag e sottoposte a ricerche di dati. I dati rimangono nella posizione esistente, ma una copia dei relativi metadati viene aggiunta a Data Catalog. È incluso un riferimento alla posizione dell'origine dati. I metadati vengono indicizzati per semplificare l'individuazione delle origini dati tramite la ricerca. L'indicizzazione li rende anche comprensibili agli utenti che li individuano.

### <a name="virtual-network"></a>Rete virtuale
Questa architettura di riferimento definisce una rete privata virtuale con uno spazio degli indirizzi 10.0.0.0/16.

**Gruppi di sicurezza di rete**: i [gruppi di sicurezza di rete](../../virtual-network/virtual-network-vnet-plan-design-arm.md) (NSG) contengono elenchi di controllo di accesso che consentono o negano il traffico in una rete virtuale. Gli NSG possono essere usati per proteggere il traffico a livello di subnet o di singola macchina virtuale. Sono disponibili i gruppi di sicurezza di rete seguenti:
  - Un gruppo di sicurezza di rete per Active Directory
  - Un gruppo di sicurezza di rete per il carico di lavoro

Per ogni gruppo di sicurezza di rete sono aperti protocolli e porte specifici per garantire il funzionamento protetto e corretto della soluzione. Per ogni gruppo di sicurezza di rete sono abilitate anche le configurazioni seguenti:
  - [Log ed eventi di diagnostica](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) abilitati e archiviati in un account di archiviazione
  - I log di monitoraggio di Azure sono connessi alla [diagnostica di NSG](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Subnet**: ogni subnet è associata al gruppo di sicurezza di rete corrispondente.

### <a name="data-in-transit"></a>Dati in transito
Per impostazione predefinita, Azure esegue la crittografia di tutte le comunicazioni da e verso i data center di Azure. Tutte le transazioni verso Archiviazione di Azure mediante il portale di Azure hanno luogo tramite HTTPS.

### <a name="data-at-rest"></a>Dati inattivi

L'architettura protegge i dati inattivi tramite la crittografia, il controllo del database e altre misure.

**Archiviazione di Azure**: per soddisfare i requisiti dei dati inattivi crittografati, tutte le [risorse di archiviazione](https://azure.microsoft.com/services/storage/) usano la [crittografia del servizio di archiviazione](../../storage/common/storage-service-encryption.md). Questa funzionalità consente di proteggere e salvaguardare i dati, supportando l'impegno a livello di sicurezza dell'organizzazione e i requisiti di conformità definiti da NIST SP 800-171.

**Crittografia dischi di Azure**: [Crittografia dischi](../azure-security-disk-encryption-overview.md) usa la funzionalità BitLocker di Windows per eseguire la crittografia del volume per i dischi dati. La soluzione si integra con Azure Key Vault per semplificare il controllo e la gestione delle chiavi di crittografia dei dischi.

**Database SQL di Azure**: l'istanza di database SQL usa le misure di sicurezza del database seguenti:
-   L'[autenticazione e l'autorizzazione di Active Directory](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) consentono la gestione delle identità degli utenti del database e di altri servizi Microsoft in una posizione centrale.
-   Il [controllo del database SQL](../../sql-database/sql-database-auditing.md) tiene traccia degli eventi che si verificano nel database e li registra in un log di controllo in un account di Archiviazione di Azure.
-   Il database SQL di Azure è configurato per usare [Transparent Data Encryption](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) che esegue in tempo reale la crittografia e la decrittografia del database, dei backup associati e dei file di log delle transazioni per proteggere i dati inattivi. Transparent Data Encryption garantisce che i dati archiviati non siano soggetti ad accesso non autorizzato.
-   Le [regole del firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) impediscono completamente l'accesso ai server di database fino alla concessione delle autorizzazioni appropriate. Il firewall concede l'accesso ai database in base all'indirizzo IP di origine di ogni richiesta.
-   [Rilevamento delle minacce SQL](../../sql-database/sql-database-threat-detection.md) consente il rilevamento e la risposta alle minacce potenziali non appena si verificano, visualizzando avvisi relativi alla sicurezza per attività sospette del database, vulnerabilità potenziali, attacchi SQL injection e modelli di accesso al database anomali.
-   Le [colonne crittografate](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) assicurano che i dati sensibili non vengano mai visualizzati come testo non crittografato all'interno del sistema di database. Dopo l'abilitazione della crittografia dei dati, solo le applicazioni client o i server applicazioni con accesso alle chiavi possono accedere ai dati di testo non crittografato.
- [Maschera dati dinamica del database SQL](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) limita l'esposizione dei dati sensibili nascondendoli agli utenti o alle applicazioni senza privilegi. Può individuare automaticamente dati sensibili potenziali e può suggerire le maschere appropriate da applicare. Maschera dati dinamica consente di ridurre l'accesso in modo che i dati sensibili non escano dal database tramite accesso non autorizzato. *I clienti sono tenuti a modificare le impostazioni in modo da adattarle al proprio schema del database.*

### <a name="identity-management"></a>Gestione delle identità
Le tecnologie seguenti offrono le funzionalità necessarie per gestire l'accesso ai dati nell'ambiente di Azure:
-   [Azure AD](https://azure.microsoft.com/services/active-directory/) è il servizio Microsoft multi-tenant di gestione di identità e directory basato sul cloud. Tutti gli utenti della soluzione sono stati creati in Azure AD, inclusi quelli che accedono al database SQL di Azure.
-   L'autenticazione per l'applicazione viene eseguita tramite Azure AD. Per altre informazioni, vedere l'articolo che illustra come [integrare applicazioni con Azure AD](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md). Anche la crittografia delle colonne del database usa Azure AD per autenticare l'applicazione nel database SQL di Azure. Per altre informazioni, vedere la procedura per [proteggere i dati sensibili nel database SQL](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Controllo degli accessi in base al ruolo di Azure](../../role-based-access-control/role-assignments-portal.md) può essere usato dagli amministratori per definire le autorizzazioni di accesso con granularità fine. In questo modo è possibile concedere agli utenti solo il livello di accesso necessario per lavorare. Invece di concedere a ogni utente autorizzazioni senza limiti per le risorse di Azure, gli amministratori possono consentire solo determinate azioni per l'accesso ai dati. L'accesso alla sottoscrizione è limitato all'amministratore della sottoscrizione.
- [Azure Active Directory Privileged Identity Management](../../active-directory/privileged-identity-management/pim-getting-started.md) può essere usato dai clienti per ridurre al minimo il numero di utenti autorizzati ad accedere a determinate informazioni, ad esempio i dati. Gli amministratori possono usare Azure AD Privileged Identity Management per individuare, limitare e monitorare le identità con privilegi e il relativo accesso alle risorse. Questa funzionalità può essere usata anche per applicare l'accesso amministrativo on demand e JIT quando necessario.
-   [Azure Active Directory Identity Protection](../../active-directory/identity-protection/overview.md) rileva le potenziali vulnerabilità che interessano le identità dell'organizzazione e configura le risposte automatizzate alle azioni sospette rilevate correlate alle identità di un'organizzazione. Esamina anche gli eventi imprevisti sospetti per eseguire l'azione appropriata per risolverli.

### <a name="security"></a>Sicurezza
**Gestione dei segreti**: la soluzione usa [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) per la gestione di chiavi e segreti. Key Vault consente di proteggere i segreti e le chiavi di crittografia usati da servizi e applicazioni cloud. Le funzionalità di Key Vault seguenti aiutano gli utenti a proteggere i dati:
- I criteri di accesso avanzati vengono configurati in base alle necessità.
- I criteri di accesso di Key Vault sono definiti con le autorizzazioni minime necessarie per le chiavi e i segreti.
- Tutti i segreti e le chiavi in Key Vault hanno date di scadenza.
- Tutte le chiavi in Key Vault sono protette da moduli di protezione hardware specializzati. Le chiavi sono di tipo RSA a 2048 bit protette dal modulo di protezione hardware.
- A tutti gli utenti e a tutte le identità vengono concesse le autorizzazioni minime richieste tramite il controllo degli accessi in base al ruolo.
- I log di diagnostica per Key Vault sono abilitati con un periodo di conservazione di almeno 365 giorni
- Le operazioni di crittografia consentite per le chiavi sono limitate a quelle necessarie.

**Centro sicurezza di Azure**: con il [Centro sicurezza](https://docs.microsoft.com/azure/security-center/security-center-intro) i clienti possono applicare e gestire centralmente i criteri di sicurezza nei carichi di lavoro, limitare l'esposizione alle minacce, rilevare e rispondere agli attacchi. Il Centro sicurezza accede inoltre alle configurazioni esistenti dei servizi di Azure in modo da fornire elementi consigliati di configurazione e servizi utili per migliorare le condizioni di sicurezza e proteggere i dati.

 Il Centro sicurezza usa una serie di funzionalità di rilevamento per avvisare i clienti riguardo a potenziali attacchi diretti agli ambienti in cui operano. Questi avvisi contengono informazioni importanti relative a cosa ha attivato l'avviso, alle risorse interessate e all'origine dell'attacco. Il Centro sicurezza include un set di [avvisi di sicurezza predefiniti](https://docs.microsoft.com/azure/security-center/security-center-alerts-type), che vengono attivati in caso di minaccia o di attività sospetta. I clienti possono usare le [regole di avviso personalizzate](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) per definire nuovi avvisi di sicurezza in base ai dati già raccolti dall'ambiente.

 Il Centro sicurezza dispone avvisi di sicurezza ed eventi imprevisti in ordine di priorità e rende più semplice per i clienti individuare e risolvere potenziali problemi di sicurezza. Per ogni minaccia rilevata viene generato un [report di intelligence sulle minacce](https://docs.microsoft.com/azure/security-center/security-center-threat-report). I team di risposta agli eventi imprevisti possono usare i report quando analizzano e correggono le minacce.

### <a name="logging-and-auditing"></a>Registrazione e controllo

I servizi di Azure registrano in modo completo le attività di sistema e degli utenti e l'integrità del sistema:
- **Log attività**: i [log attività](../../azure-monitor/platform/activity-logs-overview.md) includono informazioni dettagliate sulle operazioni eseguite sulle risorse di una sottoscrizione. I log attività possono essere utili per determinare l'iniziatore di un'operazione, l'ora in cui si è verificata e lo stato.
- **Log di diagnostica**: i [log di diagnostica](../../azure-monitor/platform/resource-logs-overview.md) includono tutti i log generati da ogni risorsa, ovvero i registri di sistema degli eventi del sistema Windows, i log di archiviazione, i log di controllo di Key Vault e i log degli accessi e del firewall del gateway applicazione Azure. Tutti i log di diagnostica eseguono operazioni di scrittura in un account di archiviazione di Azure centralizzato e crittografato per finalità di archiviazione. Gli utenti possono configurare il periodo di conservazione, fino a 730 giorni, per soddisfare requisiti specifici.

**Log di monitoraggio di Azure**: i log vengono consolidati nei [log di monitoraggio di Azure](https://azure.microsoft.com/services/log-analytics/) per l'elaborazione, l'archiviazione e il reporting del dashboard. Dopo la raccolta, i dati vengono organizzati in tabelle separate per ogni tipo di dati all'interno di aree di lavoro di Log Analytics. In questo modo tutti i dati possono essere analizzati insieme, indipendentemente dall'origine. Il Centro sicurezza si integra con i log di monitoraggio di Azure. I clienti possono usare le query kusto per accedere ai dati degli eventi di sicurezza e combinarli con i dati di altri servizi.

Come parte di questa architettura sono incluse le [soluzioni di monitoraggio](../../monitoring/monitoring-solutions.md) di Azure seguenti:
-   [Valutazione Active Directory](../../azure-monitor/insights/ad-assessment.md): la soluzione Controllo integrità Active Directory valuta i rischi e l'integrità degli ambienti server a intervalli regolari e offre un elenco con priorità di elementi consigliati specifici per l'infrastruttura distribuita dei server.
- [Valutazione SQL](../../azure-monitor/insights/sql-assessment.md): la soluzione Controllo integrità SQL valuta i rischi e l'integrità degli ambienti server a intervalli regolari e offre ai clienti un elenco con priorità di elementi consigliati specifici per l'infrastruttura distribuita dei server.
- [Integrità agente](../../monitoring/monitoring-solution-agenthealth.md): la soluzione Integrità agente indica quanti agenti vengono distribuiti e la relativa distribuzione geografica. Indica anche quanti agenti non rispondono e il numero di agenti che inviano dati operativi.
-   [Analisi log attività](../../azure-monitor/platform/collect-activity-logs.md): questa soluzione fornisce assistenza per l'analisi dei log attività di Azure in tutte le sottoscrizioni di Azure per un cliente.

**Automazione di Azure**: la soluzione [Automazione](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) archivia, esegue e gestisce i runbook. In questa soluzione i runbook consentono di raccogliere log dal database SQL di Azure. I clienti possono usare la soluzione [Rilevamento modifiche](../../automation/change-tracking.md) di Automazione per identificare con facilità le modifiche apportate all'ambiente.

**Monitoraggio di Azure**: [Monitoraggio](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) consente agli utenti di tenere traccia delle prestazioni, gestire la sicurezza e identificare le tendenze. Le organizzazioni possono usarlo per effettuare controlli, creare avvisi e archiviare i dati. Possono inoltre tenere traccia delle chiamate API nelle risorse di Azure.

**Application Insights**:[Application Insights](https://docs.microsoft.com/azure/application-insights/) è un servizio estendibile di gestione delle prestazioni delle applicazioni per sviluppatori Web su più piattaforme. Rileva le anomalie delle prestazioni e include potenti strumenti di analisi. Gli strumenti consentono di diagnosticare i problemi e aiutare i clienti a comprendere come gli utenti usano l'app. Il servizio è progettato per supportare il miglioramento continuo delle prestazioni e dell'usabilità per i clienti.

## <a name="threat-model"></a>Modello di minaccia

Il diagramma di flusso di dati per questa architettura di riferimento è disponibile per il [download](https://aka.ms/nist171-analytics-tm) o è riportato qui. Il modello può aiutare i clienti comprendere i punti di rischio potenziale nell'infrastruttura del sistema quando apportano modifiche.

![Analisi dei dati per il modello di minaccia NIST SP 800-171](images/nist171-analytics-threat-model.png "Analisi dei dati per il modello di minaccia NIST SP 800-171")

## <a name="compliance-documentation"></a>Documentazione sulla conformità
Il [Progetto per la conformità e la sicurezza di Azure - Matrice di responsabilità clienti per NIST SP 800-171](https://aka.ms/nist171-crm) elenca tutti i controlli di sicurezza richiesti da NIST SP 800-171. La matrice descrive in dettaglio se l'implementazione di ogni controllo è responsabilità di Microsoft, del cliente o di entrambi.

Il [progetto di sicurezza e conformità di Azure - NIST SP 800-171 - Matrice di implementazione del controllo di analisi dei dati](https://aka.ms/nist171-analytics-cim) fornisce informazioni sui controlli di NIST SP 800-171 a cui l'architettura dell'analisi dei dati fa riferimento, incluse descrizioni dettagliate del modo in cui l'implementazione rispetta i requisiti di ogni controllo specifico.


## <a name="guidance-and-recommendations"></a>Indicazioni e consigli
### <a name="vpn-and-expressroute"></a>VPN ed ExpressRoute
È necessario configurare un tunnel VPN o un'istanza di [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) sicuri per stabilire una connessione sicura alle risorse distribuite nell'ambito di questa architettura di riferimento di analisi dei dati. La configurazione corretta di una VPN o di una connessione ExpressRoute consente ai clienti di aggiungere un livello di protezione per i dati in transito.

L'implementazione di un tunnel VPN sicuro con Azure consente di creare una connessione virtuale privata tra una rete locale e una rete virtuale di Azure. Questa connessione viene eseguita tramite Internet. I clienti possono usare la connessione per "veicolare" in modo sicuro le informazioni all'interno di un collegamento crittografato tra la rete e Azure. La rete VPN da sito a sito è una tecnologia sicura e collaudata, che viene distribuita da aziende di ogni dimensione ormai da decenni. La [modalità tunnel IPsec](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) viene usata in questa opzione come meccanismo di crittografia.

Poiché il traffico all'interno del tunnel VPN attraversa Internet con una connessione VPN da sito a sito, Microsoft offre un'altra opzione di connessione ancora più sicura. ExpressRoute è un collegamento WAN dedicato tra Azure e una posizione locale o un provider di hosting di Exchange. Le connessioni ExpressRoute si connettono direttamente al provider di telecomunicazioni del cliente. Di conseguenza, i dati non vengono trasmessi via Internet e non sono esposti alla rete. Tali connessioni offrono maggiore affidabilità, velocità più elevate, latenze minori e sicurezza superiore rispetto alle connessioni tipiche. 

Sono [disponibili](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid) procedure consigliate per l'implementazione di una rete ibrida protetta che estende una rete locale in Azure.

### <a name="extract-transform-load-process"></a>Processo di estrazione, trasformazione e caricamento
[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) può caricare dati nel database SQL di Azure senza che sia necessario uno strumento di estrazione, trasformazione, carico o importazione separato. PolyBase consente l'accesso ai dati tramite query T-SQL. Con PolyBase è possibile usare lo stack di business intelligence e analisi Microsoft, oltre a strumenti di terze parti compatibili con SQL Server.

### <a name="azure-ad-setup"></a>Configurazione di Azure AD
[Azure AD](../../active-directory/fundamentals/active-directory-whatis.md) è essenziale per la gestione della distribuzione e per il provisioning dell'accesso al personale che interagisce con l'ambiente. Active Directory in locale può essere integrata con Azure AD in [quattro clic](../../active-directory/hybrid/how-to-connect-install-express.md). I clienti possono anche associare l'infrastruttura distribuita di Active Directory (controller di dominio) ad Azure AD. A tale scopo, rendere l'infrastruttura distribuita di Active Directory un sottodominio di una foresta di AD Azure.

## <a name="disclaimer"></a>Dichiarazione di non responsabilità

 - Questo documento è esclusivamente a scopo informativo. MICROSOFT NON RILASCIA ALCUNA GARANZIA, ESPRESSA, IMPLICITA O DI LEGGE, RIGUARDO ALLE INFORMAZIONI CONTENUTE IN QUESTO DOCUMENTO. Questo documento viene fornito "così com'è". Le informazioni e le indicazioni riportate nel presente documento, inclusi URL e altri riferimenti a siti Internet, sono soggette a modifica senza preavviso. I clienti che fanno riferimento a questo documento lo usano a proprio rischio.
 - Questo documento non concede ai clienti diritti legali di proprietà intellettuale per alcun prodotto o soluzione Microsoft.
 - I clienti possono copiare e usare questo documento per scopi di riferimento interni.
 - Alcune indicazioni contenute in questo documento possono comportare un maggiore utilizzo di risorse di dati, rete o calcolo in Azure, aumentando di conseguenza i costi di licenza o di sottoscrizione di Azure per il cliente.
 - Questa architettura è una soluzione di base che i clienti possono adattare ai propri requisiti specifici e che non deve essere usata così com'è in un ambiente di produzione.
 - Questo documento è stato sviluppato come riferimento e non deve essere usato per definire tutti i mezzi attraverso cui un cliente può soddisfare requisiti e normative di conformità specifici. I clienti dovranno richiedere supporto legale all'organizzazione riguardo alle implementazioni approvate.
