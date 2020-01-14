---
title: Come gestire l'agente di Monitoraggio di Azure per i contenitori | Microsoft Docs
description: Questo articolo descrive la gestione delle attività di manutenzione più comuni con l'agente di Log Analytics con contenitori usato da Monitoraggio di Azure per i contenitori.
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: b1fd9b70865dfb6bb71dadfe76620129e053acbb
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75932868"
---
# <a name="how-to-manage-the-azure-monitor-for-containers-agent"></a>Come gestire l'agente di Monitoraggio di Azure per i contenitori

Monitoraggio di Azure per i contenitori usa una versione con contenitori dell'agente di Log Analytics per Linux. Dopo la distribuzione iniziale, esistono attività di routine o facoltative che può essere necessario eseguire durante il ciclo di vita dell'agente. Questo articolo contiene informazioni dettagliate su come aggiornare l'agente manualmente e disabilitare la raccolta di variabili di ambiente da un contenitore specifico. 

## <a name="how-to-upgrade-the-azure-monitor-for-containers-agent"></a>Come eseguire l'aggiornamento dell'agente di Monitoraggio di Azure per i contenitori

Monitoraggio di Azure per i contenitori usa una versione con contenitori dell'agente di Log Analytics per Linux. Quando viene rilasciata una nuova versione dell'agente, l'agente viene aggiornato automaticamente nei cluster Kubernetes gestiti ospitati nel servizio Azure Kubernetes.  

Se l'aggiornamento dell'agente non riesce, questo articolo descrive il processo per aggiornare manualmente l'agente. Per seguire le versioni rilasciate, consultare gli [annunci relativi alla versione dell'agente](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).   

### <a name="upgrading-agent-on-monitored-kubernetes-cluster"></a>Aggiornamento dell'agente nel cluster Kubernetes monitorato

Il processo di aggiornamento dell'agente nei cluster, diverso da Azure Red Hat OpenShift, è costituito da due semplici passaggi. Il primo passaggio prevede la disabilitazione del monitoraggio con Monitoraggio di Azure per i contenitori tramite l'interfaccia della riga di comando di Azure.  Seguire i passaggi descritti nell'articolo [Disabilitare il monitoraggio](container-insights-optout.md?#azure-cli). Usando l'interfaccia della riga di comando di Azure è possibile rimuovere l'agente dai nodi del cluster senza conseguenze per la soluzione e i dati corrispondenti archiviati nell'area di lavoro. 

>[!NOTE]
>Mentre si esegue questa attività di manutenzione, i nodi del cluster non inoltrano i dati raccolti e le viste Prestazioni non mostreranno i dati tra la rimozione dell'agente e l'installazione della nuova versione. 
>

Per installare la nuova versione dell'agente, seguire i passaggi descritti nell' [Abilitazione del monitoraggio mediante l'interfaccia](container-insights-enable-new-cluster.md#enable-using-azure-cli)della riga di comando di Azure per completare questo processo.  

Dopo aver abilitato nuovamente il monitoraggio, potrebbero essere necessari circa 15 minuti prima di poter visualizzare le metriche di integrità aggiornate per il cluster. Per verificare che l'aggiornamento dell'agente sia stato completato, eseguire il comando: `kubectl logs omsagent-484hw --namespace=kube-system`

Lo stato dovrebbe essere simile al seguente, dove il valore per *omi* e *omsagent* deve corrispondere alla versione più recente specificata nella [cronologia delle versioni dell'agente](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).  

    User@aksuser:~$ kubectl logs omsagent-484hw --namespace=kube-system
    :
    :
    instance of Container_HostInventory
    {
        [Key] InstanceID=3a4407a5-d840-4c59-b2f0-8d42e07298c2
        Computer=aks-nodepool1-39773055-0
        DockerVersion=1.13.1
        OperatingSystem=Ubuntu 16.04.3 LTS
        Volume=local
        Network=bridge host macvlan null overlay
        NodeRole=Not Orchestrated
        OrchestratorType=Kubernetes
    }
    Primary Workspace: b438b4f6-912a-46d5-9cb1-b44069212abc    Status: Onboarded(OMSAgent Running)
    omi 1.4.2.5
    omsagent 1.6.0-163
    docker-cimprov 1.0.0.31

## <a name="how-to-disable-environment-variable-collection-on-a-container"></a>Come disabilitare la raccolta di variabili di ambiente in un contenitore

Monitoraggio di Azure per i contenitori raccoglie le variabili di ambiente dai contenitori in esecuzione in un pod e le presenta nel riquadro delle proprietà del contenitore selezionato nella visualizzazione **Contenitori**. È possibile controllare questo comportamento disabilitando la raccolta per un contenitore specifico durante la distribuzione del cluster Kubernetes o dopo aver impostato la variabile di ambiente *AZMON_COLLECT_ENV*. Questa funzionalità è disponibile dalla versione dell'agente ciprod11292018 e successive.  

Per disabilitare la raccolta di variabili di ambiente in un contenitore nuovo o esistente, impostare la variabile **AZMON_COLLECT_ENV** con il valore **False** nel file di configurazione yaml della distribuzione Kubernetes. 

```  
- name: AZMON_COLLECT_ENV  
  value: "False"  
```  

Eseguire il comando seguente per applicare la modifica ai cluster Kubernetes diversi da Azure Red Hat OpenShift): `kubectl apply -f  <path to yaml file>`. Per modificare ConfigMap e applicare questa modifica per i cluster OpenShift di Azure Red Hat, eseguire il comando:

``` bash
oc edit configmaps container-azm-ms-agentconfig -n openshift-azure-logging
```

Verrà aperto l'editor di testo predefinito. Dopo aver impostato la variabile, salvare il file nell'editor.

Per verificare che la modifica della configurazione sia stata eseguita, selezionare un contenitore nella visualizzazione **Contenitori** in Monitoraggio di Azure per i contenitori ed espandere **Variabili di ambiente** nel pannello delle proprietà.  La sezione dovrebbe mostrare solo la variabile creata in precedenza, ovvero **AZMON_COLLECT_ENV = FALSE**. Per tutti gli altri contenitori, la sezione delle variabili di ambiente dovrebbe elencare tutte le variabili di ambiente individuate.

Per abilitare di nuovo il rilevamento delle variabili di ambiente, applicare la stessa procedura precedente e modificare il valore da **False** a **True**, quindi eseguire nuovamente il comando `kubectl` per aggiornare il contenitore.  

```  
- name: AZMON_COLLECT_ENV  
  value: "True"  
```  

## <a name="next-steps"></a>Passaggi successivi

Se si verificano problemi durante l'aggiornamento dell'agente, esaminare la [guida risoluzione dei problemi](container-insights-troubleshoot.md) per il supporto.
