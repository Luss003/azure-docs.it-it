---
title: Service Fabric Azure Files Volume Driver (GA) | Microsoft Docs
description: Service Fabric supporta l'uso di File di Azure per eseguire il backup di volumi dal contenitore.
services: service-fabric
author: athinanthny
manager: chackdan
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.topic: conceptual
ms.date: 6/10/2018
ms.author: atsenthi
ms.openlocfilehash: 1287df567c60b7ad851c94a8ba787270255d0f35
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/23/2019
ms.locfileid: "74422777"
---
# <a name="service-fabric-azure-files-volume-driver"></a>Service Fabric Azure Files Volume Driver
The Azure Files volume plugin, a [Docker volume plugin](https://docs.docker.com/engine/extend/plugins_volume/) that provides [Azure Files](/azure/storage/files/storage-files-introduction) based volumes for Docker containers is now **GA (Generally Available)** .

Questo plug-in di volume Docker viene offerto come pacchetto di applicazione di Service Fabric distribuibile nei cluster di Service Fabric, con lo scopo di fornire volumi basati su File di Azure per altre applicazioni contenitore di Service Fabric distribuite nel cluster.

> [!NOTE]
> Version 6.5.661.9590 of the Azure Files volume plugin is a GA(Generally available) release. 
>

## <a name="prerequisites"></a>Prerequisiti
* La versione Windows del plug-in di volume di File di Azure può essere usata solo in [Windows Server versione 1709](/windows-server/get-started/whats-new-in-windows-server-1709), [Windows 10 versione 1709](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1709) o sistemi operativi successivi.

* La versione Linux del plug-in di volume di File di Azure può essere usata in tutte le versioni del sistema operativo supportate da Service Fabric.

* Il plug-in di volume di File di Azure funziona solo con Service Fabric 6.2 e versioni successive.

* Per creare una condivisione file per l'applicazione contenitore di Service Fabric da usare come volume, seguire le istruzioni riportate nella [documentazione di File di Azure](/azure/storage/files/storage-how-to-create-file-share).

* È necessario che sia installato [Powershell con il modulo Service Fabric](/azure/service-fabric/service-fabric-get-started) o [SFCTL](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli).

* If you are using Hyper-V containers, the following snippets need to be added in the ClusterManifest (local cluster) or fabricSettings section in your Azure Resource Manager template (Azure cluster) or ClusterConfig.json (standalone cluster).

In ClusterManifest è necessario aggiungere il codice seguente nella sezione Hosting. In this example, the volume name is **sfazurefile** and the port it listens to on the cluster is **19100**. Replace them with the correct values for your cluster.

``` xml 
<Section Name="Hosting">
  <Parameter Name="VolumePluginPorts" Value="sfazurefile:19100" />
</Section>
```

In the fabricSettings section in your Azure Resource Manager template (for Azure deployments) or ClusterConfig.json (for standalone deployments), the following snippet needs to be added. Again, replace the volume name and port values with your own.

```json
"fabricSettings": [
  {
    "name": "Hosting",
    "parameters": [
      {
          "name": "VolumePluginPorts",
          "value": "sfazurefile:19100"
      }
    ]
  }
]
```


## <a name="deploy-a-sample-application-using-service-fabric-azure-files-volume-driver"></a>Deploy a sample application using Service Fabric Azure Files volume driver

### <a name="using-azure-resource-manager-via-the-provided-powershell-script-recommended"></a>Using Azure Resource Manager via the provided Powershell script (recommended)

If your cluster is based in Azure, we recommend deploying applications to it using the Azure Resource Manager application resource model for ease of use and to help move towards the model of maintaining infrastructure as code. This approach eliminates the need to keep track of the app version for the Azure Files volume driver. It also enables you to maintain separate Azure Resource Manager templates for each supported OS. The script assumes you are deploying the latest version of the Azure Files application and takes parameters for OS type, cluster subscription ID, and resource group. You can download the script from the [Service Fabric download site](https://sfazfilevd.blob.core.windows.net/sfazfilevd/DeployAzureFilesVolumeDriver.zip). Note that this automatically sets the ListenPort, which is the port on which the Azure Files volume plugin listens for requests from the Docker daemon, to 19100. You can change it by adding parameter named "listenPort". Ensure that the port does not conflict with any other port that the cluster or your applications uses.
 

Azure Resource Manager deployment command for Windows:
```powershell
.\DeployAzureFilesVolumeDriver.ps1 -subscriptionId [subscriptionId] -resourceGroupName [resourceGroupName] -clusterName [clusterName] -windows
```

Azure Resource Manager deployment command for Linux:
```powershell
.\DeployAzureFilesVolumeDriver.ps1 -subscriptionId [subscriptionId] -resourceGroupName [resourceGroupName] -clusterName [clusterName] -linux
```

Once you've successfully run the script, you can skip to the [configuring your application section.](/azure/service-fabric/service-fabric-containers-volume-logging-drivers#configure-your-applications-to-use-the-volume)


### <a name="manual-deployment-for-standalone-clusters"></a>Manual deployment for standalone clusters

The Service Fabric application that provides the volumes for your containers can be downloaded from the [Service Fabric download site](https://sfazfilevd.blob.core.windows.net/sfazfilevd/AzureFilesVolumePlugin.6.5.661.9590.zip). L'applicazione può essere distribuita nel cluster tramite [PowerShell](./service-fabric-deploy-remove-applications.md), l'[interfaccia della riga di comando](./service-fabric-application-lifecycle-sfctl.md) o le [API di FabricClient](./service-fabric-deploy-remove-applications-fabricclient.md).

1. Using the command line, change directory to the root directory of the downloaded application package.

    ```powershell
    cd .\AzureFilesVolume\
    ```

    ```bash
    cd ~/AzureFilesVolume
    ```

2. Next, copy the application package to the image store  with the appropriate values for [ApplicationPackagePath] and [ImageStoreConnectionString]:

    ```powershell
    Copy-ServiceFabricApplicationPackage -ApplicationPackagePath [ApplicationPackagePath] -ImageStoreConnectionString [ImageStoreConnectionString] -ApplicationPackagePathInImageStore AzureFilesVolumePlugin
    ```

    ```bash
    sfctl cluster select --endpoint https://testcluster.westus.cloudapp.azure.com:19080 --pem test.pem --no-verify
    sfctl application upload --path [ApplicationPackagePath] --show-progress
    ```

3. Registrare il tipo di applicazione

    ```powershell
    Register-ServiceFabricApplicationType -ApplicationPathInImageStore AzureFilesVolumePlugin
    ```

    ```bash
    sfctl application provision --application-type-build-path [ApplicationPackagePath]
    ```

4. Create the application, paying close attention to the **ListenPort** application parameter value. This value is the port on which the Azure Files volume plugin listens for requests from the Docker daemon. Ensure that the port provided to the application matches the VolumePluginPorts in the ClusterManifest and does not conflict with any other port that the cluster or your applications uses.

    ```powershell
    New-ServiceFabricApplication -ApplicationName fabric:/AzureFilesVolumePluginApp -ApplicationTypeName AzureFilesVolumePluginType -ApplicationTypeVersion 6.5.661.9590   -ApplicationParameter @{ListenPort='19100'}
    ```

    ```bash
    sfctl application create --app-name fabric:/AzureFilesVolumePluginApp --app-type AzureFilesVolumePluginType --app-version 6.5.661.9590  --parameter '{"ListenPort":"19100"}'
    ```

> [!NOTE]
> 
> Windows Server 2016 Datacenter non supporta il mapping dei montaggi SMB nei contenitori ([supportati solo in Windows Server versione 1709](/virtualization/windowscontainers/manage-containers/container-storage)). Questa limitazione impedisce il mapping del volume della rete e l'uso dei driver di volume di File di Azure precedenti alla versione 1709.

#### <a name="deploy-the-application-on-a-local-development-cluster"></a>Distribuire l'applicazione in un cluster di sviluppo locale
Follow steps 1-3 from the [above.](/azure/service-fabric/service-fabric-containers-volume-logging-drivers#manual-deployment-for-standalone-clusters)

 Il numero predefinito di istanze del servizio per l'applicazione del plug-in di volume di File di Azure è -1, corrispondente alla distribuzione di un'istanza del servizio in ogni nodo del cluster. Quando si distribuisce l'applicazione del plug-in di volume di File di Azure in un cluster di sviluppo locale, tuttavia, il numero di istanze del servizio specificato dovrà essere 1. A questo scopo è possibile usare il parametro **InstanceCount** dell'applicazione. Therefore, the command for creating the Azure Files volume plugin application on a local development cluster is:

```powershell
New-ServiceFabricApplication -ApplicationName fabric:/AzureFilesVolumePluginApp -ApplicationTypeName AzureFilesVolumePluginType -ApplicationTypeVersion 6.5.661.9590  -ApplicationParameter @{ListenPort='19100';InstanceCount='1'}
```

```bash
sfctl application create --app-name fabric:/AzureFilesVolumePluginApp --app-type AzureFilesVolumePluginType --app-version 6.5.661.9590  --parameter '{"ListenPort": "19100","InstanceCount": "1"}'
```

## <a name="configure-your-applications-to-use-the-volume"></a>Configurare le applicazioni per l'uso del volume
The following snippet shows how an Azure Files based volume can be specified in the application manifest file of your application. L'elemento di specifico interesse è il tag **Volume**:

```xml
?xml version="1.0" encoding="UTF-8"?>
<ApplicationManifest ApplicationTypeName="WinNodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
    <Description>Calculator Application</Description>
    <Parameters>
      <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
      <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
      <Parameter Name="MyStorageVar" DefaultValue="c:\tmp"></Parameter>
    </Parameters>
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeServicePackage" ServiceManifestVersion="1.0"/>
     <Policies>
       <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv">
            <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            <RepositoryCredentials PasswordEncrypted="false" Password="****" AccountName="test"/>
            <Volume Source="azfiles" Destination="c:\VolumeTest\Data" Driver="sfazurefile">
                <DriverOption Name="shareName" Value="" />
                <DriverOption Name="storageAccountName" Value="" />
                <DriverOption Name="storageAccountKey" Value="" />
                <DriverOption Name="storageAccountFQDN" Value="" />
            </Volume>
       </ContainerHostPolicies>
   </Policies>
    </ServiceManifestImport>
    <ServiceTemplates>
        <StatelessService ServiceTypeName="StatelessNodeService" InstanceCount="5">
            <SingletonPartition></SingletonPartition>
        </StatelessService>
    </ServiceTemplates>
</ApplicationManifest>
```

Il nome del driver per il plug-in di volume di File di Azure è **sfazurefile**. This value is set for the **Driver** attribute of the **Volume** tag element in the application manifest.

In the **Volume** tag in the snippet above, the Azure Files volume plugin requires the following attributes:
- **Source**: corrisponde al nome del volume. L'utente può scegliere qualsiasi nome per il volume.
- **Destination** - This attribute is the location that the volume is mapped to within the running container. La destinazione non può quindi essere una posizione già esistente nel contenitore.

Come illustrato negli elementi **DriverOption** nel frammento precedente, il plug-in di volume di File di Azure supporta le opzioni del driver seguenti.
- **shareName**: nome della condivisione file di File di Azure che rende disponibile il volume per il contenitore.
- **storageAccountName**: nome dell'account di archiviazione di Azure contenente la condivisione file di File di Azure.
- **storageAccountKey**: chiave di accesso dell'account di archiviazione di Azure contenente la condivisione file di File di Azure.
- **storageAccountFQDN**: nome di dominio associato all'account di archiviazione. Se storageAccountFQDN non è specificato, il nome di dominio è costituito dal suffisso predefinito (.file.core.windows.net) con il valore di storageAccountName.  

    ```xml
    - Example1: 
        <DriverOption Name="shareName" Value="myshare1" />
        <DriverOption Name="storageAccountName" Value="myaccount1" />
        <DriverOption Name="storageAccountKey" Value="mykey1" />
        <!-- storageAccountFQDN will be "myaccount1.file.core.windows.net" -->
    - Example2: 
        <DriverOption Name="shareName" Value="myshare2" />
        <DriverOption Name="storageAccountName" Value="myaccount2" />
        <DriverOption Name="storageAccountKey" Value="mykey2" />
        <DriverOption Name="storageAccountFQDN" Value="myaccount2.file.core.chinacloudapi.cn" />
    ```

## <a name="using-your-own-volume-or-logging-driver"></a>Uso di un driver di volume o di registrazione personalizzato
Service Fabric consente anche di usare driver di [volume](https://docs.docker.com/engine/extend/plugins_volume/) o di [registrazione](https://docs.docker.com/engine/admin/logging/overview/) personalizzati. Se il driver di volume/registrazione Docker non è installato nel cluster, è possibile installarlo manualmente usando i protocolli RDP/SSH. È possibile eseguire l'installazione con questi protocolli tramite un [script di avvio del set di scalabilità di macchine virtuali](https://azure.microsoft.com/resources/templates/201-vmss-custom-script-windows/) o uno [script SetupEntryPoint](/azure/service-fabric/service-fabric-application-model).

Un esempio di script per installare il [driver di volume Docker per Azure](https://docs.docker.com/docker-for-azure/persistent-data-volumes/) è riportato di seguito:

```bash
docker plugin install --alias azure --grant-all-permissions docker4x/cloudstor:17.09.0-ce-azure1  \
    CLOUD_PLATFORM=AZURE \
    AZURE_STORAGE_ACCOUNT="[MY-STORAGE-ACCOUNT-NAME]" \
    AZURE_STORAGE_ACCOUNT_KEY="[MY-STORAGE-ACCOUNT-KEY]" \
    DEBUG=1
```

Nelle applicazioni, per usare il driver di volume o di registrazione installato sarà necessario specificare i valori appropriati negli elementi **Volume** e **LogConfig** inclusi in **ContainerHostPolicies** nel manifesto dell'applicazione.

```xml
<ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv">
    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
    <RepositoryCredentials PasswordEncrypted="false" Password="****" AccountName="test"/>
    <LogConfig Driver="[YOUR_LOG_DRIVER]" >
        <DriverOption Name="test" Value="vale"/>
    </LogConfig>
    <Volume Source="c:\workspace" Destination="c:\testmountlocation1" IsReadOnly="false"></Volume>
    <Volume Source="[MyStorageVar]" Destination="c:\testmountlocation2" IsReadOnly="true"> </Volume>
    <Volume Source="myvolume1" Destination="c:\testmountlocation2" Driver="[YOUR_VOLUME_DRIVER]" IsReadOnly="true">
        <DriverOption Name="[name]" Value="[value]"/>
    </Volume>
</ContainerHostPolicies>
```

Quando si specifica un plug-in di volume, Service Fabric crea automaticamente il volume usando i parametri specificati. Il tag **Source** per l'elemento **Volume** è il nome del volume e il tag **Driver** specifica il plug-in del driver di volume. Il tag **Destination** è il percorso in cui viene eseguito il mapping di **Source** all'interno del contenitore in esecuzione. La destinazione non può quindi essere un percorso già esistente all'interno del contenitore. Le opzioni possono essere specificate tramite il tag **DriverOption** come indicato di seguito:

```xml
<Volume Source="myvolume1" Destination="c:\testmountlocation4" Driver="azure" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
</Volume>
```

I parametri dell'applicazione sono supportati per i volumi, come illustrato nel frammento di manifesto precedente (cercare `MyStorageVar` per un esempio d'uso).

Se viene specificato un driver di log Docker, è necessario distribuire gli agenti o i contenitori per gestire i registri nel cluster. Il tag **DriverOption** può essere usato per specificare le opzioni per il driver di log.

## <a name="next-steps"></a>Passaggi successivi
* Per visualizzare esempi dei contenitori con il driver di volume, vedere gli [esempi di contenitori di Service Fabric](https://github.com/Azure-Samples/service-fabric-containers)
* Per distribuire i contenitori in un cluster di Service Fabric, vedere l'articolo su come [distribuire un contenitore in Service Fabric](service-fabric-deploy-container.md)
