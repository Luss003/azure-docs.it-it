---
title: Guida introduttiva per la gestione di condivisioni file di Azure con l'interfaccia della riga di comando di Azure
description: Questa guida introduttiva illustra come usare l'interfaccia della riga di comando di Azure per gestire File di Azure.
author: roygara
ms.service: storage
ms.topic: quickstart
ms.date: 10/26/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 93baf275e93c28283836a92c71eb9b24151392fc
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/31/2019
ms.locfileid: "68699587"
---
# <a name="quickstart-create-and-manage-azure-file-shares-using-azure-cli"></a>Guida introduttiva: Creare e gestire condivisioni file di Azure tramite l'interfaccia della riga di comando di Azure
Questa guida contiene tutte le informazioni essenziali sull'uso delle [condivisioni file di Azure](storage-files-introduction.md) con l'interfaccia della riga di comando di Azure. Le condivisioni file di Azure sono esattamente come le altre condivisioni file, ma vengono archiviate nel cloud e sono supportate dalla piattaforma Azure. Le condivisioni file di Azure supportano il protocollo SMB standard di settore e consentono la condivisione di file in più computer, applicazioni e istanze. 

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se si decide di installare e usare l'interfaccia della riga di comando di Azure in locale, per le procedure in questo articolo è necessario eseguire l'interfaccia della riga di comando di Azure versione 2.0.4 o successiva. Eseguire **az --version** per trovare la versione dell'interfaccia della riga di comando di Azure in uso. Se è necessario eseguire l'installazione o l'aggiornamento, vedere [Installare l'interfaccia della riga di comando di Azure 2.0](/cli/azure/install-azure-cli). 

Per impostazione predefinita, i comandi dell'interfaccia della riga di comando di Azure restituiscono codice JSON (JavaScript Object Notation). JSON è il formato standard per l'invio e la ricezione di messaggi dalle API REST. Per facilitare l'uso delle risposte JSON, alcuni esempi in questo articolo usano il parametro *query* nei comandi dell'interfaccia della riga di comando di Azure. Questo parametro usa il [linguaggio di query JMESPath](http://jmespath.org/) per analizzare il codice JSON. Per altre informazioni su come usare i risultati dei comandi dell'interfaccia della riga di comando di Azure seguendo il linguaggio di query JMESPath, vedere l'[esercitazione di JMESPath](http://jmespath.org/tutorial.html).

## <a name="sign-in-to-azure"></a>Accedere ad Azure
Se si usa l'interfaccia della riga di comando di Azure in locale, aprire il prompt e accedere ad Azure, se non è già stato fatto.

```bash 
az login
```

## <a name="create-a-resource-group"></a>Creare un gruppo di risorse
Un gruppo di risorse è un contenitore logico in cui le risorse di Azure vengono distribuite e gestite. Se non è già disponibile un gruppo di risorse di Azure, è possibile usare il comando [az group create](/cli/azure/group) per crearne uno. 

L'esempio seguente crea un gruppo di risorse denominato *myResourceGroup* nella località *Stati Uniti orientali*:

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-storage-account"></a>Creare un account di archiviazione
Un account di archiviazione è un pool condiviso di spazio di archiviazione in cui è possibile distribuire condivisioni file di Azure o altre risorse di archiviazione, ad esempio BLOB o code. Un account di archiviazione può contenere un numero illimitato di condivisioni file. In una condivisione può essere archiviato un numero illimitato di file, fino ai limiti di capacità dell'account di archiviazione.

L'esempio seguente crea un account di archiviazione denominato *mystorageaccount\<numero casuale\>* usando il comando [az storage account create](/cli/azure/storage/account) e quindi inserisce il nome di tale account di archiviazione nella variabile `$STORAGEACCT`. I nomi degli account di archiviazione devono essere univoci, pertanto assicurarsi di sostituire "mystorageacct" con un nome univoco.

```azurecli-interactive 
STORAGEACCT=$(az storage account create \
    --resource-group "myResourceGroup" \
    --name "mystorageacct" \
    --location eastus \
    --sku Standard_LRS \
    --query "name" | tr -d '"')
```

### <a name="get-the-storage-account-key"></a>Ottenere la chiave dell'account di archiviazione
Le chiavi dell'account di archiviazione controllano l'accesso alle risorse in un account di archiviazione. Vengono create automaticamente quando si crea un account di archiviazione. È possibile ottenere le chiavi per l'account di archiviazione con il comando [az storage account keys list](/cli/azure/storage/account/keys): 

```azurecli-interactive 
STORAGEKEY=$(az storage account keys list \
    --resource-group "myResourceGroup" \
    --account-name $STORAGEACCT \
    --query "[0].value" | tr -d '"')
```

## <a name="create-an-azure-file-share"></a>Creare una condivisione file di Azure
È ora possibile creare la prima condivisione file di Azure. Creare le condivisioni file con il comando [az storage share create](/cli/azure/storage/share). Questo esempio crea una condivisione file di Azure denominata *myshare*: 

```azurecli-interactive
az storage share create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" 
```

I nomi delle condivisioni possono contenere solo lettere minuscole, numeri e trattini singoli (non possono però iniziare con un trattino). Per informazioni dettagliate su come denominare le condivisioni e i file, vedere [Naming and referencing shares, directories, files, and metadata](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata) (Denominazione e riferimento a condivisioni, directory, file e metadati).

## <a name="use-your-azure-file-share"></a>Usare la condivisione file di Azure
File di Azure offre due metodi per usare i file e le cartelle all'interno della condivisione file di Azure: il [protocollo SMB (Server Message Block)](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) standard di settore e il [protocollo REST di File](https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api). 

Per montare una condivisione file con SMB, vedere il documento riportato di seguito in base al sistema operativo in uso:
- [Linux](storage-how-to-use-files-linux.md)
- [macOS](storage-how-to-use-files-mac.md)
- [Windows](storage-how-to-use-files-windows.md)

### <a name="using-an-azure-file-share-with-the-file-rest-protocol"></a>Uso di una condivisione file di Azure con il protocollo REST di File 
È possibile usare il protocollo REST File direttamente, ovvero creando personalmente chiamate HTTP REST, ma il metodo più comune consiste nell'usare l'interfaccia della riga di comando di Azure, il [modulo di Azure PowerShell](storage-how-to-use-files-powershell.md) o un SDK di Archiviazione di Azure, perché offrono un pratico wrapper per il protocollo REST File nel linguaggio di scripting/programmazione preferito.  

È previsto che la maggior parte degli utenti di File di Azure vorrà usare la propria condivisione file di Azure tramite il protocollo SMB, perché questo consente loro di usare le applicazioni e gli strumenti esistenti con cui possono avere familiarità, ma esistono diversi motivi per cui è vantaggioso usare l'API REST di File invece di SMB, ad esempio:

- Si esplora la condivisione file da Azure Cloud Shell di Bash, che non può montare condivisioni file tramite SMB.
- È necessario eseguire uno script o un'applicazione da client in cui non può essere montata una condivisione SMB, ad esempio client locali in cui la porta 445 non è sbloccata.
- Si sfruttano risorse serverless, ad esempio [Funzioni di Azure](../../azure-functions/functions-overview.md). 

Gli esempi seguenti mostrano come usare l'interfaccia della riga di comando di Azure per modificare la condivisione file di Azure con il protocollo REST File. 

### <a name="create-a-directory"></a>Creare una directory
Per creare una nuova directory denominata *myDirectory* nella radice della condivisione file di Azure, usare il comando [`az storage directory create`](/cli/azure/storage/directory):

```azurecli-interactive
az storage directory create \
   --account-name $STORAGEACCT \
   --account-key $STORAGEKEY \
   --share-name "myshare" \
   --name "myDirectory" 
```

### <a name="upload-a-file"></a>Caricare un file
Per una dimostrazione del caricamento di un file con il comando [`az storage file upload`](/cli/azure/storage/file), per prima cosa creare un file da caricare nell'unità dei file temporanei di Cloud Shell. Nell'esempio seguente si crea e quindi si carica il file:

```azurecli-interactive
date > ~/clouddrive/SampleUpload.txt

az storage file upload \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --source "~/clouddrive/SampleUpload.txt" \
    --path "myDirectory/SampleUpload.txt"
```

Se si esegue l'interfaccia della riga di comando di Azure in locale, sostituire `~/clouddrive` con un percorso esistente nel computer.

Dopo il caricamento del file, è possibile usare il comando [`az storage file list`](/cli/azure/storage/file) per verificare che il file sia stato caricato nella condivisione file di Azure:

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory" \
    --output table
```

### <a name="download-a-file"></a>Scaricare un file
È possibile usare il comando [`az storage file download`](/cli/azure/storage/file) per scaricare una copia del file caricato nell'unità dei file temporanei di Cloud Shell:

```azurecli-interactive
# Delete an existing file by the same name as SampleDownload.txt, if it exists, because you've run this example before
rm -rf ~/clouddrive/SampleDownload.txt

az storage file download \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory/SampleUpload.txt" \
    --dest "~/clouddrive/SampleDownload.txt"
```

### <a name="copy-files"></a>Copiare i file
Un'attività comune è la copia di file da una condivisione file a un'altra oppure da/verso un contenitore di archiviazione BLOB di Azure. Per una dimostrazione di questa funzionalità, creare una nuova condivisione. Copiare il file caricato in questa nuova condivisione usando il comando [az storage file copy](/cli/azure/storage/file/copy): 

```azurecli-interactive
az storage share create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare2"

az storage directory create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare2" \
    --name "myDirectory2"

az storage file copy start \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --source-share "myshare" \
    --source-path "myDirectory/SampleUpload.txt" \
    --destination-share "myshare2" \
    --destination-path "myDirectory2/SampleCopy.txt"
```

Se ora si elencano i file nella nuova condivisione, verrà visualizzato il file copiato:

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare2" \
    --output table
```

Nonostante il comando `az storage file copy start` sia pratico per gli spostamenti di file ad hoc tra condivisioni file di Azure e contenitori di archiviazione BLOB di Azure, è consigliabile usare AzCopy per spostamenti più grandi in termini di numero o di dimensioni dei file da spostare. Sono disponibili altre informazioni su [AzCopy per Linux](../common/storage-use-azcopy-linux.md) e [AzCopy per Windows](../common/storage-use-azcopy.md). AzCopy deve essere installato in locale. Non è disponibile in Cloud Shell. 

## <a name="create-and-manage-share-snapshots"></a>Creare e gestire gli snapshot di condivisione
Un'altra attività utile che è possibile eseguire con una condivisione file di Azure è la creazione di snapshot di condivisione. Uno snapshot mantiene una copia di una condivisione file di Azure eseguita in un momento specifico. Gli snapshot di condivisione sono simili ad alcune tecnologie del sistema operativo con cui probabilmente si ha già familiarità:

- Snapshot [Logical Volume Manager (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) per i sistemi Linux
- Snapshot [Apple File System (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) per macOS
- [Servizio Copia Shadow del volume](https://docs.microsoft.com/windows/desktop/VSS/volume-shadow-copy-service-portal) per i file system Windows, come NTFS e ReFS. È possibile creare uno snapshot di condivisione con il comando [`az storage share snapshot`](/cli/azure/storage/share):

```azurecli-interactive
SNAPSHOT=$(az storage share snapshot \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" \
    --query "snapshot" | tr -d '"')
```

### <a name="browse-share-snapshot-contents"></a>Esplorare i contenuti di uno snapshot di condivisione
È possibile esplorare il contenuto di uno snapshot di condivisione passando il timestamp dello snapshot di condivisione acquisito nella variabile `$SNAPSHOT` al comando `az storage file list`:

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --snapshot $SNAPSHOT \
    --output table
```

### <a name="list-share-snapshots"></a>Elencare gli snapshot di condivisione
Per visualizzare l'elenco degli snapshot creati per la condivisione, usare il comando seguente:

```azurecli-interactive
az storage share list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --include-snapshot \
    --query "[? name=='myshare' && snapshot!=null]" | tr -d '"'
```

### <a name="restore-from-a-share-snapshot"></a>Eseguire il ripristino da snapshot di condivisione
È possibile ripristinare un file con il comando `az storage file copy start` usato in precedenza. Per prima cosa, eliminare il file SampleUpload.txt che è stato caricato, per poterlo ripristinare dallo snapshot:

```azurecli-interactive
# Delete SampleUpload.txt
az storage file delete \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory/SampleUpload.txt"
 # Build the source URI for a snapshot restore
URI=$(az storage account show \
    --resource-group "myResourceGroup" \
    --name $STORAGEACCT \
    --query "primaryEndpoints.file" | tr -d '"')
 URI=$URI"myshare/myDirectory/SampleUpload.txt?sharesnapshot="$SNAPSHOT
 # Restore SampleUpload.txt from the share snapshot
az storage file copy start \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --source-uri $URI \
    --destination-share "myshare" \
    --destination-path "myDirectory/SampleUpload.txt"
```

### <a name="delete-a-share-snapshot"></a>Eliminare uno snapshot di condivisione
È possibile eliminare uno snapshot di condivisione con il comando [`az storage share delete`](/cli/azure/storage/share). Usare la variabile contenente il riferimento `$SNAPSHOT` al parametro `--snapshot`:

```azurecli-interactive
az storage share delete \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" \
    --snapshot $SNAPSHOT
```

## <a name="clean-up-resources"></a>Pulire le risorse
Al termine, è possibile usare il comando [`az group delete`](/cli/azure/group) per rimuovere il gruppo di risorse e tutte le risorse correlate: 

```azurecli-interactive 
az group delete --name "myResourceGroup"
```

In alternativa, è possibile rimuovere le risorse singolarmente.
- Per rimuovere le condivisioni file di Azure create per questo articolo:

    ```azurecli-interactive
    az storage share delete \
        --account-name $STORAGEACCT \
        --account-key $STORAGEKEY \
        --name "myshare" \
        --delete-snapshots include

    az storage share delete \
        --account-name $STORAGEACCT \
        --account-key $STORAGEKEY \
        --name "myshare2" \
        --delete-snapshots include
    ```

- Per rimuovere l'account di archiviazione, rimuovendo così implicitamente le condivisioni file di Azure e le eventuali altre risorse di archiviazione, come ad esempio un contenitore di archiviazione BLOB di Azure, che sono state create:

    ```azurecli-interactive
    az storage account delete \
        --resource-group "myResourceGroup" \
        --name $STORAGEACCT \
        --yes
    ```

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Che cos'è File di Azure?](storage-files-introduction.md)
