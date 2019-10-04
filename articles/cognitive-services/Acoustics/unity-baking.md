---
title: Esercitazione sul bake in Unity con Progetto Acustica
titlesuffix: Azure Cognitive Services
description: Questa esercitazione descrive il bake con Progetto Acustica in Unity.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: b7249c3048ba3af3adbaac01f43770482a0d38ad
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933241"
---
# <a name="project-acoustics-unity-bake-tutorial"></a>Esercitazione sul bake in Unity con Progetto Acustica
Questa esercitazione descrive il bake con Progetto Acustica in Unity.

Requisiti software:
* [Unity 2018.2+](https://unity3d.com) per Windows o MacOS
* [Plug-in di Progetto Acustica integrato nel progetto Unity](unity-integration.md) o [contenuto di esempio di Progetto acustica per Unity](unity-quickstart.md)
* Facoltativo: Un account [Azure Batch](create-azure-account.md) per accelerare il bake tramite cloud computing

## <a name="open-the-project-acoustics-bake-window"></a>Aprire la finestra di bake di Progetto Acustica nell'editor
Scegliere **Window > Acoustics** (Finestra > Acustica) dal menu di Unity:

![Screenshot dell'editor Unity con l'opzione di menu della finestra Acoustics evidenziata](media/window-acoustics.png)

## <a name="create-a-navigation-mesh"></a>Creare una mesh di navigazione
Progetto Acustica usa una mesh di navigazione per posizionare i punti di probe dell'ascoltatore per la simulazione. È possibile usare il [flusso di lavoro della mesh di navigazione](https://docs.unity3d.com/Manual/nav-BuildingNavMesh.html) di Unity o un altro pacchetto di modellazione 3D per progettare la mesh. 

## <a name="mark-acoustic-scene-objects"></a>Contrassegnare oggetti acustici della scena
Progetto Acustica si basa su due tipi di oggetti scena per la simulazione: gli oggetti che riflettono e occludono il suono nella simulazione e la mesh di navigazione del giocatore, che vincola i punti probe dell'ascoltatore nella simulazione. Entrambi i tipi di oggetto vengono contrassegnati usando la scheda **Oggetti**. 

Dato che contrassegnando gli oggetti si aggiungono semplicemente i componenti **AcousticsGeometry** o **AcousticsNavigation** all'oggetto, è anche possibile usare il [flusso di lavoro dei componenti Unity standard](https://docs.unity3d.com/Manual/UsingComponents.html) per contrassegnare o rimuovere il contrassegno degli oggetti. È possibile contrassegnare solo Mesh Renderers (Renderer di mesh) e Terrains (Terreni). Tutti gli altri tipi di oggetto verranno ignorati. Le caselle di controllo consentiranno di applicare o rimuovere il contrassegno in tutti gli oggetti interessati.

### <a name="mark-acoustic-occlusion-and-reflection-geometry"></a>Contrassegnare la geometria di occlusione e riflessione acustica
Aprire la scheda **Objects** (Oggetti) della finestra **Acoustics** (Acustica). Contrassegnare come **Acoustics Geometry** (Geometria acustica) tutti gli oggetti che devono occludere, riflettere o assorbire il suono. La geometria acustica può includere elementi come suolo, pareti, tetti, finestre, vetrate, tappeti e mobili di grandi dimensioni. È possibile usare qualsiasi livello arbitrario di complessità per questi oggetti. Dato che la scena viene voxelizzata prima della simulazione, il bake di mesh estremamente dettagliate, come ad esempio alberi con molte foglie di piccole dimensioni, non è più costoso di quello di oggetti semplificati.

Non includere elementi che non influiranno sull'acustica, come mesh di collisione invisibili.

La trasformazione di un oggetto in fase di calcolo dei probe, tramite la scheda **Probes** (Probe) sotto, è fissa nei risultati del bake. Se si sposta uno qualsiasi degli oggetti contrassegnati nella scena, sarà necessario ripetere il calcolo dei probe ed effettuare di nuovo il bake della scena.

### <a name="mark-the-navigation-mesh"></a>Contrassegnare la mesh di navigazione
Le mesh di navigazione create con il flusso di lavoro di Unity verranno selezionate dal sistema di acustica. Per usare le proprie mesh, contrassegnarle dalla scheda **Objects** (Oggetti).

### <a name="for-reference-the-objects-tab-parts"></a>Informazioni di riferimento: parti della scheda Objects (Oggetti)
Le parti della pagina della scheda sono:

1. Pulsanti di selezione della scheda, con la scheda **Objects** (Oggetti) selezionata. Usare questi pulsanti per scorrere i vari passaggi della creazione di un bake di acustica, da sinistra a destra.
2. Breve descrizione di che cosa fare quando si usa questa pagina.
3. Filtri disponibili per la finestra della gerarchia. Usarli per filtrare nella finestra della gerarchia gli oggetti del tipo specificato, per poterli contrassegnare più facilmente. Se non è ancora stato selezionato alcun valore per l'acustica, selezionando le ultime due opzioni non verrà visualizzato nulla. Possono tuttavia essere utili per trovare gli oggetti che verranno contrassegnati in seguito.
4. Quando non è selezionato alcun oggetto, questa sezione mostra lo stato di tutti gli oggetti nella scena:
    * Total (Totale): numero totale di oggetti attivi non nascosti nella scena.
    * Ignored (Ignorati): numero di oggetti diversi da Mesh Renderers (Renderer di mesh) o dai Terrains (Terreni).
    * Mesh: numero di oggetti Mesh Renderers (Renderer di mesh) nella scena
    * Terrain (Terreno): numero di oggetti terreno nella scena
    * Geometry (Geometria): numero di oggetti Mesh o Terrain (Terreno) nella scena contrassegnati come "Acoustics Geometry" (Geometria acustica)
    * Navigation (Navigazione): numero di oggetti Mesh o Terrain (Terreno) nella scena contrassegnati come "Acoustics Navigation" (Navigazione acustica). Questo numero non include gli elementi NavMesh di Unity.
5. Mostra il numero totale di oggetti contrassegnabili nella scena, ovvero solo i renderer di mesh e i terreni. Mostra le caselle di controllo che è possibile usare per contrassegnare (aggiungere il componente appropriato a) tali oggetti come geometria o navigazione per l'acustica
6. Quando non viene selezionato nulla, questa nota ricorda di selezionare gli oggetti per contrassegnarli, se necessario. È anche possibile selezionare una o entrambe le caselle di controllo per contrassegnare tutti gli oggetti nella scena senza effettuare selezioni.
7. Quando sono presenti oggetti selezionati, questa sezione mostra lo stato solo degli oggetti selezionati.
8. Mostra il numero totale di oggetti selezionati contrassegnabili. Selezionando o deselezionando le caselle di controllo, il contrassegno verrà applicato o rimosso solo per gli oggetti selezionati.

Se nella scena non sono selezionati elementi, l'aspetto della scheda Objects (Oggetti) sarà simile all'immagine seguente:

![Screenshot della scheda Objects della finestra Acoustics](media/objects-tab-no-selection-detail.png)

Se nella finestra della scena o della gerarchia sono selezionati elementi, l'aspetto sarà simile a quello dell'immagine seguente:

![Screenshot della scheda Objects della finestra Acoustics con la selezione visualizzata](media/objects-tab-selection-detail.png)

Se alcuni oggetti sono contrassegnati e altri no, la casella di controllo appropriata mostrerà un valore "misto":

![Screenshot della scheda Objects della finestra Acoustics con l'icona di selezione mista evidenziata](media/mixed-object-selection-detail.png)

Facendo clic sulla casella di controllo, tutti gli oggetti verranno contrassegnati e, facendo di nuovo clic, il contrassegno verrà rimosso da tutti gli oggetti.

Gli oggetti possono essere contrassegnati sia per la geometria che per la navigazione.

## <a name="select-acoustic-materials"></a>Selezionare i materiali acustici
Dopo che gli oggetti sono stati contrassegnati, fare clic sul pulsante **Materials** (Materiali) e assegnare materiali acustici. Il sistema dei materiali di Progetto Acustica è legato al sistema dei materiali visivi di Unity: affinché due oggetti abbiano materiali acustici separati, è necessario che abbiano materiali visivi separati.

I materiali acustici controllano la quantità di energia sonora riflessa da ogni superficie. Il materiale acustico predefinito ha un assorbimento simile al cemento. Progetto Acustica suggerisce i materiali in base al nome dei materiali visivi. È possibile assegnare il materiale acustico "personalizzato" a un materiale per abilitare un dispositivo di scorrimento del coefficiente di assorbimento.

Il tempo di riverbero di un determinato materiale in una stanza è inversamente proporzionale al coefficiente di assorbimento e la maggior parte dei materiali ha valori di assorbimento compresi tra 0,01 e 0,20. I materiali con coefficienti di assorbimento non compresi in questo intervallo sono molto assorbenti.

![Grafico che mostra la correlazione negativa del tempo di riverbero con il coefficiente di assorbimento](media/reverb-time-graph.png)

### <a name="for-reference-parts-of-the-materials-tab"></a>Informazioni di riferimento: Parti della scheda Materials (Materiali)
![Screenshot della scheda Materials della finestra Acoustics in Unity](media/materials-tab-detail.png)

1. Pulsante della scheda **Materials** (Materiali), usato per visualizzare questa pagina.
2. Breve descrizione di che cosa fare quando si usa questa pagina.
3. Se selezionata, verranno elencati solo i materiali usati dagli oggetti contrassegnati come **Acoustics Geometry** (Geometria acustica). In caso contrario, verranno elencati tutti i materiali usati nella scena.
4. Usare queste opzioni per modificare l'ordine del menu a discesa che viene visualizzato quando si fa clic su un elenco a discesa nella colonna Acoustics (Acustica) sotto (n. 6). **Name** (Nome) ordina i materiali acustici per nome. "Absorptivity" (Assorbività) li elenca in ordine crescente di assorbività.
5. Elenco dei materiali usati nella scena, in ordine alfabetico. Se la casella di controllo **Show Marked Only** (Mostra solo contrassegnati) viene selezionata (n. 3), vengono visualizzati solo i materiali usati dagli oggetti contrassegnati come**Acoustics Geometry** (Geometria acustica). Facendo clic su un materiale qui, verranno selezionati tutti gli oggetti della scena che usano tale materiale.
6. Mostra il materiale acustico a cui è stato assegnato il materiale della scena. Fare clic su un elenco a discesa per riassegnare un materiale della scena a un materiale acustico diverso. È possibile modificare l'ordinamento del menu visualizzato quando si fa clic su uno di questi elementi usando le opzioni **Sort Acoustics By:** (Ordina acustica per) precedenti (n. 4).
7. Mostra il coefficiente di assorbimento acustico del materiale selezionato nella colonna precedente. Un valore pari a zero indica un materiale perfettamente riflettente (nessun assorbimento), mentre un valore pari a 1 indica un materiale perfettamente assorbente (nessun riflesso). Il coefficiente di assorbimento può essere modificato solo se il materiale selezionato è "Custom" (Personalizzato).
8. Per un materiale assegnato a "Custom" (Personalizzato), il dispositivo di scorrimento non viene più disabilitato ed è possibile scegliere il coefficiente di assorbimento usando il dispositivo di scorrimento o digitando un valore.

## <a name="calculate-and-review-listener-probe-locations"></a>Calcolare ed esaminare le posizioni dei probe per l'ascoltatore
Dopo aver assegnato i materiali, passare alla scheda **Probes** (Probe) e fare clic su **Calculate** (Calcola) per posizionare i punti probe dell'ascoltatore per la simulazione.

### <a name="what-the-calculate-button-calculates"></a>Calcoli eseguiti con il pulsante "Calculate..." (Calcola)
Il pulsante **Calculate...** (Calcola) usa la geometria della scena acustica selezionata per preparare la scena per la simulazione:

1. Usa la geometria delle mesh della scena per calcolare un volume in voxel. Il volume in voxel è un volume tridimensionale che racchiude l'intera scena ed è costituito da piccoli "voxel" cubici. Le dimensioni dei voxel sono determinate dalla frequenza della simulazione, che dipende dall'impostazione **Simulation Resolution** (Risoluzione simulazione). Ogni voxel viene contrassegnato come "open air" o contenente la geometria della scena. Se un voxel contiene la geometria, viene contrassegnato con il coefficiente di assorbimento del materiale assegnato a tale geometria.
2. Usa le mesh di navigazione per posizionare i punti probe dell'ascoltatore. L'algoritmo bilancia il conflitto tra copertura spaziale e durata e dimensioni dei file della simulazione, garantendo nel contempo che corridoi stretti e piccoli spazi abbiano sempre una copertura. Il numero di punti di probe varia generalmente da 100 per scene piccole a poche migliaia per scene grandi.

A seconda delle dimensioni della scena e della velocità del computer, questi calcoli possono richiedere alcuni minuti.

### <a name="review-voxel-and-probe-placement"></a>Esaminare il posizionamento di voxel e probe
Visualizzare in anteprima il posizionamento dei dati voxel e dei punti di probe per verificare che sia possibile effettuare il bake della scena. Una mesh di navigazione incompleta o una geometria acustica mancante o in eccesso sarà di solito rapidamente visibile nell'anteprima. Il posizionamento di voxel e probe può essere abilitato o disabilitato usando il menu Gizmos:

![Screenshot del menu Gizmos in Unity](media/gizmos-menu.png)

I voxel che contengono la geometria acustica sono mostrati come cubi verdi. Esplorare la scena e verificare che tutti gli elementi che devono costituire la geometria siano formati da voxel. Per visualizzare i voxel, la telecamera della scena deve trovarsi a non più di circa 5 metri dall'oggetto.

Se si confrontano i voxel creati con risoluzione grossolana con quelli creati con risoluzione fine, si noterà che nel primo caso i voxel sono due volte più grandi.

![Screenshot dell'anteprima dei voxel con risoluzione grossolana nell'editor Unity](media/voxel-cubes-preview.png)

I risultati della simulazione vengono interpolati tra le posizioni dei punti di probe dell'ascoltatore in runtime. Verificare che siano presenti punti di probe nei pressi di qualsiasi punto in cui è previsto che il giocatore si sposti nella scena.

![Screenshot dell'anteprima dei probe nell'editor Unity](media/probes-preview.png)

### <a name="take-care-with-scene-renames"></a>Prestare attenzione ai nomi delle scene
Il nome della scena viene usato per connettere la scena ai file in cui sono archiviate la voxelizzazione e la posizione dei punti di probe. Se la scena viene rinominata dopo che i punti di probe sono stati calcolati, i dati di posizionamento e di assegnazione dei materiali vengono persi e devono essere eseguiti di nuovo.

### <a name="for-reference-parts-of-the-probes-tab"></a>Informazioni di riferimento: Parti della scheda Probes (Probe)
![Screenshot della scheda Probes della finestra Acoustics in Unity](media/probes-tab-detail.png)

1. Pulsante della scheda **Probes** (Probe) usato per visualizzare questa pagina
2. Breve descrizione di che cosa fare quando si usa questa pagina
3. Usarli per scegliere una risoluzione per la simulazione grossolana o fine. Quella grossolana è più veloce, ma presenta alcuni svantaggi. Per informazioni dettagliate, vedere[Risoluzione del bake](bake-resolution.md) più avanti.
4. Questo campo consente di scegliere la posizione in cui i file di dati di acustica devono essere inseriti. Fare clic sul pulsante con "..." per usare la selezione cartelle. La posizione predefinita è **Assets/AcousticsData**. In questa posizione verrà creata anche una sottocartella **Editor**. Per altre informazioni sui file di dati, vedere [File di dati](#Data-Files) più avanti.
5. I file di dati per questa scena verranno denominati usando il prefisso fornito qui. Il prefisso predefinito è "Acoustics_[Nome scena]".
6. Dopo aver calcolato i probe, i controlli precedenti verranno disabilitati. Fare clic sul pulsante **Clear** (Cancella) per cancellare i calcoli e abilitare i controlli per poter eseguire di nuovo i calcoli con nuove impostazioni.
7. Fare clic sul pulsante **Calculate...** (Calcola) per voxelizzare la scena e calcolare le posizioni dei punti di probe. Questa operazione viene eseguita in locale sul computer e deve essere eseguita prima di creare un bake.

In questa versione di Project Acoustics i probe non possono essere inseriti manualmente, ma devono essere inseriti tramite il processo automatizzato fornito nella scheda **Probes** (Probe).

Per informazioni più dettagliate sulla risoluzione grossolana o fine, vedere [Risoluzione del bake](bake-resolution.md).

## <a name="bake-your-scene-using-azure-batch"></a>Effettuare il bake della scena con Azure Batch
È possibile effettuare il bake della scena con un cluster di elaborazione nel cloud usando il servizio Azure Batch. Il plug-in Progetto Acustica di Unity si connette direttamente ad Azure Batch per creare un'istanza di un cluster di Azure Batch per ogni bake e gestire ed eliminare tale cluster. Nella scheda **Bake** immettere le credenziali di Azure, selezionare un tipo e una dimensione di computer cluster e fare clic su **Bake**.

### <a name="for-reference-parts-of-the-bake-tab"></a>Informazioni di riferimento: parti della scheda Bake
![Screenshot della scheda Bake della finestra Acoustics in Unity](media/bake-tab-details.png)

1. Pulsante della scheda Bake usato per visualizzare questa pagina.
2. Breve descrizione di che cosa fare in questa pagina.
3. Campi in cui immettere le credenziali di Azure dopo aver creato l'account Azure. Per altre informazioni, vedere [Creare un account Azure Batch](create-azure-account.md).
4. Tag di immagine docker per il set di strumenti di acustica.
5. Avviare il portale di Azure per gestire le sottoscrizioni, monitorare l'utilizzo, visualizzare le informazioni di fatturazione e così via. 
6. Tipo di nodo di calcolo di Azure Batch da usare per il calcolo. Il tipo di nodo deve essere supportato dalla località del data center di Azure. Se non si è sicuri, lasciare **Standard_F8s_v2**.
7. Numero di nodi da usare per questo calcolo. Il numero immesso in questo campo influisce sul tempo necessario per completare il bake ed è limitato dall'allocazione di core di Azure Batch. L'allocazione predefinita consente solo due nodi da 8 core o un nodo da 16 core, ma può essere espansa. Per altre informazioni sui vincoli delle allocazioni di core, vedere [Creare un account Azure Batch](create-azure-account.md).
8. Selezionare questa casella di controllo per configurare il pool di calcolo per l'uso di [nodi a priorità bassa](https://docs.microsoft.com/azure/batch/batch-low-pri-vms). I nodi di calcolo a priorità bassa hanno un costo molto minore ma potrebbero non essere sempre disponibili o essere interrotti in qualsiasi momento.
9. Numero di probe per la scena, calcolato nella scheda **Probes** (Probe). Il numero di probe determina il numero di simulazioni che devono essere eseguite nel cloud. Non è possibile specificare più nodi che probe.
10. Intervallo di tempo trascorso previsto per l'esecuzione del processo nel cloud. Non include il tempo di avvio dei nodi. Dopo l'avvio dell'esecuzione del processo, indica quanto tempo deve trascorrere prima di ottenere i risultati. Si noti che questa è solo una stima.
11. Tempo di elaborazione totale necessario per eseguire le simulazioni. Corrisponde alla quantità totale di tempo di calcolo dei nodi che verrà usata in Azure. Per altre informazioni sull'uso di questo valore, vedere [Stima del costo del bake](#Estimating-bake-cost).
12. Questo messaggio indica dove verranno salvati i risultati del bake dopo il completamento del processo.
13. (Solo uso avanzato) Se per qualche motivo è necessario fare in modo che Unity non tenga più in considerazione un bake inviato (ad esempio perché i risultati sono stati scaricati usando un altro computer), fare clic sul pulsante **Clear State** (Cancella stato) per ignorare il processo inviato. Si noti che in tal modo il file dei risultati, quando sarà pronto, **non** verrà scaricato, ma **questo non equivale ad annullare il processo**. Il processo, se in esecuzione, continuerà a essere eseguito nel cloud.
14. Fare clic sul pulsante **Bake** per inviare il bake al cloud. Mentre un processo è in esecuzione, questo pulsante è invece **Cancel Job** (Annulla processo).
15. Prepara l'elaborazione della [simulazione acustica nel PC](#Local-bake).
16. Quest'area mostra lo stato del bake. Al termine, verrà visualizzato **Downloaded** (Scaricato).

È sempre possibile ottenere informazioni complete su processi attivi, pool di calcolo e spazio di archiviazione nel [portale di Azure](https://portal.azure.com).

Mentre un processo è in esecuzione, il pulsante **Bake** viene sostituito da **Cancel Job** (Annulla processo). Usare questo pulsante per annullare il processo in corso. Verrà chiesta conferma prima che il processo venga annullato. L'annullamento di un processo non può essere cancellato, non saranno disponibili risultati e il tempo di calcolo di Azure usato verrà ugualmente addebitato.

Dopo aver avviato un bake, è possibile chiudere Unity. A seconda del progetto, del tipo di nodo e del numero di nodi, un bake cloud può richiedere diverse ore. Lo stato del processo bake verrà aggiornato quando si ricarica il progetto e si apre la finestra Acoustics (Acustica). Se il processo è stato completato, verrà scaricato il file di output.

Le credenziali di Azure vengono archiviate in modo sicuro nel computer locale e associate all'editor Unity. Vengono usate esclusivamente per stabilire una connessione sicura ad Azure.

## <a name="to-find-the-status-of-a-running-job-on-the-azure-portal"></a>Per trovare lo stato di un processo in esecuzione nel portale di Azure

1. Trovare l'ID del processo bake nella scheda Bake:

![Screenshot dell'ID del processo bake in Unity](media/unity-job-id.png)  

2. Aprire il [portale di Azure](https://portal.azure.com), passare all'account Batch usato per il bake e selezionare **Processi**

![Screenshot del collegamento Processi](media/azure-batch-jobs.png)  

3. Cercare l'ID processo nell'elenco dei processi

![Screenshot dello stato del processo bake](media/azure-bake-job-status.png)  

4. Fare clic sull'ID processo per visualizzare lo stato delle attività correlate e lo stato generale del processo

![Screenshot dello stato delle attività del processo bake](media/azure-batch-task-state.png)  


### <a name="Estimating-bake-cost"></a> Stima dei costi del bake di Azure

Per calcolare quanto costerà un determinato bake, moltiplicare il valore visualizzato di **Estimated Compute Cost** (Costo di calcolo stimato), che indica una durata, per il costo orario nella valuta locale del **tipo di nodo di VM** selezionato. Il risultato non includerà il tempo necessario per avere i nodi operativi. Ad esempio, se come tipo di nodo si seleziona **Standard_F8s_v2**, che ha un costo di $ 0,40/ora e il costo di calcolo stimato è di 3 ore e 57 minuti, il costo stimato per l'esecuzione del processo sarà $ 0,40 * 4 ore circa = all'incirca $ 1,60. Il costo effettivo sarà probabilmente di poco superiore a causa del tempo aggiuntivo necessario per avviare i nodi. È possibile trovare il costo orario del nodo nella pagina [Prezzi di Azure Batch](https://azure.microsoft.com/pricing/details/virtual-machines/linux). Come categoria selezionare "Con ottimizzazione per il calcolo" o "High Performance Computing".

## <a name="Local-bake"></a> Effettuare il bake della scena nel PC
È possibile effettuare il bake della scena nel PC. Può essere utile per eseguire esperimenti con l'acustica in scene di piccole dimensioni prima di creare un account Azure Batch. Si noti che la simulazione dell'acustica può richiedere molto tempo, a seconda delle dimensioni della scena.

### <a name="minimum-hardware-requirements"></a>Requisiti hardware minimi
* Un processore x86-64 con almeno 8 core e 32 GB di RAM
* [Hyper-V abilitato](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v) per l'esecuzione di Docker

Ad esempio, nei test di Microsoft su un computer a 8 core con Intel Xeon E5-1660 a 3 GHz e 32 GB di RAM -
* Una piccola scena con 100 probe può richiedere circa 2 ore per un bake grossolano o 32 ore per un bake fine.
* Una scena di medie dimensioni con 1000 probe può richiedere circa 20 ore per un bake grossolano o 21 giorni per un bake fine.

### <a name="setup-docker"></a>Configurare Docker
Installare e configurare Docker nel computer che elaborerà la simulazione:
1. Installare [Docker Desktop](https://www.docker.com/products/docker-desktop).
2. Accedere alle impostazioni di Docker, passare alle opzioni "Avanzate" e configurare le risorse con almeno 8 GB di RAM. Più CPU si riesce ad allocare a Docker, più velocemente sarà completato il bake.  
![Screenshot delle impostazioni di esempio di Docker](media/docker-settings.png)
1. Passare a "Shared Drives" ("Unità condivise") e attivare la condivisione per l'unità utilizzata per l'elaborazione.  
![Screenshot delle opzioni di Docker per le unità condivise](media/docker-shared-drives.png)

### <a name="run-local-bake"></a>Eseguire il bake locale
1. Fare clic sul pulsante "Prepare Local Bake" ("Prepara bake locale") nella scheda **Bake** e selezionare la cartella in cui verranno salvati i file di input e gli script di esecuzione. Quindi è possibile eseguire il bake in qualsiasi computer, purché soddisfi i requisiti hardware minimi e abbia Docker installato, copiando la cartella in tale computer.
2. Avviare la simulazione usando lo script "runlocalbake.bat" in Windows o lo script "runlocalbake.sh" in MacOS. Lo script recupera l'immagine Docker Project Acoustics con il set di strumenti necessario per l'elaborazione di simulazione e avvia la simulazione. 
3. Al termine della simulazione copiare il file .ace risultante nel progetto Unity. Per assicurarsi che Unity lo riconosca come file binario, aggiungere ".bytes" all'estensione del file (ad esempio "Scene1.ace.bytes"). I log dettagliati per la simulazione sono archiviati in "AcousticsLog.txt." In caso di problemi, condividere questo file per facilitare la diagnosi.

## <a name="Data-Files"></a> File di dati aggiunti dal processo di bake

Durante il processo di bake vengono creati quattro file di dati. Uno contiene i risultati della simulazione e ha il titolo specificato dall'utente. Gli altri archiviano i dati correlati all'editor di Unity.

Risultato della simulazione:
* **Assets/AcousticsData/Acoustics\_[NomeScena].ace.bytes**: questa è la tabella di ricerca runtime e contiene i risultati della simulazione e gli elementi della scena acustica voxelizzati. Il percorso e il nome di questo file possono essere modificati usando i campi nella scheda **Probes** (Probe).

Prestare attenzione a non eliminare il file dei risultati della simulazione. Questo file non è ripristinabile se non effettuando di nuovo il bake della scena.

File di dati dell'editor:
* **Assets/Editor/[NomeScena]\_AcousticsParameters.asset**: in questo file vengono archiviati i dati immessi nei campi nell'interfaccia utente di Acoustics. Il percorso e il nome di questo file non possono essere modificati.
* **Assets/AcousticsData/Editor/Acoustics_[NomeScena].vox**: questo file archivia la geometria dell'acustica voxelizzata e le proprietà dei materiali che vengono calcolate usando il pulsante**Calculate...** (Calcola) nella scheda Probes (Probe). Il percorso e il nome di questo file possono essere modificati usando i campi nella scheda **Probes** (Probe).
* **Assets/AcousticsData/Editor/Acoustics\_[NomeScena]\_config.xml**: questo file contiene i parametri di simulazione calcolati usando il pulsante **Calculate...** (Calcola) nella scheda **Probes** (Probe). Il percorso e il nome di questo file possono essere modificati usando i campi nella scheda **Probes** (Probe).

## <a name="set-up-the-acoustics-lookup-table"></a>Configurare la tabella di ricerca dell'acustica
Trascinare e rilasciare l'oggetto Prefab **Progetto Acustica** dal pannello del progetto alla scena:

![Screenshot della scheda Prefab della finestra Acoustics in Unity](media/acoustics-prefab.png)

Fare clic sull'oggetto gioco **ProjectAcoustics** e passare al relativo pannello inspector. Specificare il percorso del risultato del bake (il file con estensione ACE, in **Assets/AcousticsData**) trascinandolo nello script Acoustics Manager o facendo clic sul pulsante circolare accanto alla casella di testo.

![Screenshot della scheda Manager Prefab della finestra Acoustics in Unity](media/acoustics-manager.png)  

## <a name="next-steps"></a>Passaggi successivi
* Esaminare i [controlli di progettazione per Unity](unity-workflow.md)
* Esplorare i [concetti relativi alla progettazione per Progetto Acustica](design-process.md)

