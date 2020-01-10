---
title: Trasferire dati da e verso l'archiviazione BLOB di Azure tramite AzCopy V10 | Microsoft Docs
description: Questo articolo contiene una raccolta di comandi di esempio AzCopy che semplificano la creazione di contenitori, la copia di file e la sincronizzazione di directory tra i file System e i contenitori locali.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 10/22/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: dineshm
ms.openlocfilehash: e7d5438c03fa8fd61dc0d5b89bbb197092c6873e
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/09/2020
ms.locfileid: "75772075"
---
# <a name="transfer-data-with-azcopy-and-blob-storage"></a>Trasferire i dati con AzCopy e l'archiviazione BLOB

AzCopy è un'utilità da riga di comando che è possibile usare per copiare i dati in, da o tra account di archiviazione. Questo articolo contiene i comandi di esempio che funzionano con l'archivio BLOB.

## <a name="get-started"></a>Inizia oggi stesso

Vedere l'articolo [Introduzione a AzCopy](storage-use-azcopy-v10.md) per scaricare AzCopy e informazioni sui modi in cui è possibile fornire le credenziali di autorizzazione al servizio di archiviazione.

> [!NOTE]
> Gli esempi in questo articolo presuppongono che l'identità sia stata autenticata usando il comando `AzCopy login`. AzCopy usa quindi l'account Azure AD per autorizzare l'accesso ai dati nell'archivio BLOB.
>
> Se si preferisce usare un token di firma di accesso condiviso per autorizzare l'accesso ai dati BLOB, è possibile aggiungere tale token all'URL della risorsa in ogni comando AzCopy.
>
> Ad esempio: `'https://<storage-account-name>.blob.core.windows.net/<container-name>?<SAS-token>'`.

## <a name="create-a-container"></a>Creare un contenitore

> [!TIP]
> Gli esempi in questa sezione racchiudono gli argomenti del percorso con virgolette singole (''). Usare le virgolette singole in tutte le shell dei comandi eccetto la shell dei comandi di Windows (cmd. exe). Se si usa una shell dei comandi di Windows (cmd. exe), racchiudere gli argomenti del percorso con virgolette doppie ("") anziché virgolette singole ('').

È possibile usare il comando [azcopy make](storage-ref-azcopy-make.md) per creare un contenitore. Negli esempi di questa sezione viene creato un contenitore denominato `mycontainer`.

|    |     |
|--------|-----------|
| **Sintassi** | `azcopy make 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>'` |
| **Esempio** | `azcopy make 'https://mystorageaccount.blob.core.windows.net/mycontainer'` |
| **Esempio** (spazio dei nomi gerarchico) | `azcopy make 'https://mystorageaccount.dfs.core.windows.net/mycontainer'` |

Per informazioni dettagliate sulla documentazione di riferimento, vedere [azcopy make](storage-ref-azcopy-make.md).

## <a name="upload-files"></a>Caricare file

È possibile usare il comando [copy di azcopy](storage-ref-azcopy-copy.md) per caricare file e directory dal computer locale.

Questa sezione contiene gli esempi seguenti:

> [!div class="checklist"]
> * Caricare un file
> * Caricare una directory
> * Caricare il contenuto di una directory 
> * Carica file specifici

Per informazioni dettagliate sulla documentazione di riferimento, vedere [azcopy Copy](storage-ref-azcopy-copy.md).

> [!TIP]
> Gli esempi in questa sezione racchiudono gli argomenti del percorso con virgolette singole (''). Usare le virgolette singole in tutte le shell dei comandi eccetto la shell dei comandi di Windows (cmd. exe). Se si usa una shell dei comandi di Windows (cmd. exe), racchiudere gli argomenti del percorso con virgolette doppie ("") anziché virgolette singole ('').

### <a name="upload-a-file"></a>Caricare un file

|    |     |
|--------|-----------|
| **Sintassi** | `azcopy copy '<local-file-path>' 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-name>'` |
| **Esempio** | `azcopy copy 'C:\myDirectory\myTextFile.txt' 'https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt'` |
| **Esempio** (spazio dei nomi gerarchico) | `azcopy copy 'C:\myDirectory\myTextFile.txt' 'https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt'` |

È anche possibile caricare un file usando un carattere jolly (*) in qualsiasi punto del percorso o del nome file. Ad esempio: `'C:\myDirectory\*.txt'`o `C:\my*\*.txt`.

> [!NOTE]
> Per impostazione predefinita, AzCopy carica i dati in BLOB in blocchi. Per caricare i file come BLOB di accodamento o BLOB di pagine, usare il flag `--blob-type=[BlockBlob|PageBlob|AppendBlob]`.

### <a name="upload-a-directory"></a>Caricare una directory

Questo esempio copia una directory (e tutti i file in tale directory) in un contenitore BLOB. Il risultato è una directory nel contenitore con lo stesso nome.

|    |     |
|--------|-----------|
| **Sintassi** | `azcopy copy '<local-directory-path>' 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>' --recursive` |
| **Esempio** | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer' --recursive` |
| **Esempio** (spazio dei nomi gerarchico) | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.dfs.core.windows.net/mycontainer' --recursive` |

Per eseguire la copia in una directory all'interno del contenitore, è sufficiente specificare il nome della directory nella stringa di comando.

|    |     |
|--------|-----------|
| **Esempio** | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory' --recursive` |
| **Esempio** (spazio dei nomi gerarchico) | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobDirectory' --recursive` |

Se si specifica il nome di una directory che non esiste nel contenitore, AzCopy crea una nuova directory con tale nome.

### <a name="upload-the-contents-of-a-directory"></a>Caricare il contenuto di una directory

È possibile caricare il contenuto di una directory senza copiare la directory che lo contiene usando il carattere jolly (*).

|    |     |
|--------|-----------|
| **Sintassi** | `azcopy copy '<local-directory-path>\*' 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<directory-path>'` |
| **Esempio** | `azcopy copy 'C:\myDirectory\*' 'https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory'` |
| **Esempio** (spazio dei nomi gerarchico) | `azcopy copy 'C:\myDirectory\*' 'https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobDirectory'` |

> [!NOTE]
> Aggiungere il flag di `--recursive` per caricare i file in tutte le sottodirectory.

### <a name="upload-specific-files"></a>Carica file specifici

È possibile specificare nomi di file completi oppure utilizzare nomi parziali con caratteri jolly (*).

#### <a name="specify-multiple-complete-file-names"></a>Specificare più nomi di file completi

Usare il comando [azcopy Copy](storage-ref-azcopy-copy.md) con l'opzione `--include-path`. Separare i singoli nomi di file usando un punto e virgola (`;`).

|    |     |
|--------|-----------|
| **Sintassi** | `azcopy copy '<local-directory-path>' 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>' --include-path <semicolon-separated-file-list>` |
| **Esempio** | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer' --include-path 'photos;documents\myFile.txt' --recursive` |
| **Esempio** (spazio dei nomi gerarchico) | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.dfs.core.windows.net/mycontainer' --include-path 'photos;documents\myFile.txt' --recursive` |

In questo esempio, AzCopy trasferisce la directory `C:\myDirectory\photos` e il file di `C:\myDirectory\documents\myFile.txt`. È necessario includere l'opzione `--recursive` per trasferire tutti i file nella directory `C:\myDirectory\photos`.

È anche possibile escludere i file utilizzando l'opzione `--exclude-path`. Per altre informazioni, vedere la documentazione di riferimento per la [copia di azcopy](storage-ref-azcopy-copy.md) .

#### <a name="use-wildcard-characters"></a>Usa caratteri jolly

Usare il comando [azcopy Copy](storage-ref-azcopy-copy.md) con l'opzione `--include-pattern`. Specificare i nomi parziali che includono i caratteri jolly. Separare i nomi usando un semicolin (`;`). 

|    |     |
|--------|-----------|
| **Sintassi** | `azcopy copy '<local-directory-path>' 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>' --include-pattern <semicolon-separated-file-list-with-wildcard-characters>` |
| **Esempio** | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer' --include-pattern 'myFile*.txt;*.pdf*'` |
| **Esempio** (spazio dei nomi gerarchico) | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.dfs.core.windows.net/mycontainer' --include-pattern 'myFile*.txt;*.pdf*'` |

È anche possibile escludere i file utilizzando l'opzione `--exclude-pattern`. Per altre informazioni, vedere la documentazione di riferimento per la [copia di azcopy](storage-ref-azcopy-copy.md) .

Le opzioni `--include-pattern` e `--exclude-pattern` sono valide solo per i nomi file e non per il percorso.  Se si desidera copiare tutti i file di testo presenti in un albero di directory, utilizzare l'opzione `–recursive` per ottenere l'intero albero di directory, quindi utilizzare l'`–include-pattern` e specificare `*.txt` per ottenere tutti i file di testo.

## <a name="download-files"></a>Scaricare i file

È possibile usare il comando [copy di azcopy](storage-ref-azcopy-copy.md) per scaricare i BLOB, le directory e i contenitori nel computer locale.

Questa sezione contiene gli esempi seguenti:

> [!div class="checklist"]
> * Scaricare un file
> * Scaricare una directory
> * Scarica il contenuto di una directory
> * Scarica file specifici

> [!NOTE]
> Se il valore della proprietà `Content-md5` di un BLOB contiene un hash, AzCopy calcola un hash MD5 per i dati scaricati e verifica che l'hash MD5 archiviato nella proprietà `Content-md5` del BLOB corrisponda all'hash calcolato. Se questi valori non corrispondono, il download ha esito negativo a meno che non si esegua l'override di questo comportamento accodando `--check-md5=NoCheck` o `--check-md5=LogOnly` al comando copy.

Per informazioni dettagliate sulla documentazione di riferimento, vedere [azcopy Copy](storage-ref-azcopy-copy.md).

> [!TIP]
> Gli esempi in questa sezione racchiudono gli argomenti del percorso con virgolette singole (''). Usare le virgolette singole in tutte le shell dei comandi eccetto la shell dei comandi di Windows (cmd. exe). Se si usa una shell dei comandi di Windows (cmd. exe), racchiudere gli argomenti del percorso con virgolette doppie ("") anziché virgolette singole ('').

### <a name="download-a-file"></a>Scaricare un file

|    |     |
|--------|-----------|
| **Sintassi** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-path>' '<local-file-path>'` |
| **Esempio** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt' 'C:\myDirectory\myTextFile.txt'` |
| **Esempio** (spazio dei nomi gerarchico) | `azcopy copy 'https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt' 'C:\myDirectory\myTextFile.txt'` |

### <a name="download-a-directory"></a>Scaricare una directory

|    |     |
|--------|-----------|
| **Sintassi** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<directory-path>' '<local-directory-path>' --recursive` |
| **Esempio** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory' 'C:\myDirectory'  --recursive` |
| **Esempio** (spazio dei nomi gerarchico) | `azcopy copy 'https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobDirectory' 'C:\myDirectory'  --recursive` |

Questo esempio genera una directory denominata `C:\myDirectory\myBlobDirectory` che contiene tutti i file scaricati.

### <a name="download-the-contents-of-a-directory"></a>Scarica il contenuto di una directory

È possibile scaricare il contenuto di una directory senza copiare la directory che lo contiene usando il carattere jolly (*).

> [!NOTE]
> Attualmente, questo scenario è supportato solo per gli account che non dispongono di uno spazio dei nomi gerarchico.

|    |     |
|--------|-----------|
| **Sintassi** | `azcopy copy 'https://<storage-account-name>.blob.core.windows.net/<container-name>/*' '<local-directory-path>/'` |
| **Esempio** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory/*' 'C:\myDirectory'` |

> [!NOTE]
> Aggiungere il flag di `--recursive` per scaricare i file in tutte le sottodirectory.

### <a name="download-specific-files"></a>Scarica file specifici

È possibile specificare nomi di file completi oppure utilizzare nomi parziali con caratteri jolly (*).

#### <a name="specify-multiple-complete-file-names"></a>Specificare più nomi di file completi

Usare il comando [azcopy Copy](storage-ref-azcopy-copy.md) con l'opzione `--include-path`. Separare i singoli nomi di file usando un semicolin (`;`).

|    |     |
|--------|-----------|
| **Sintassi** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-or-directory-name>' '<local-directory-path>'  --include-path <semicolon-separated-file-list>` |
| **Esempio** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory' 'C:\myDirectory'  --include-path 'photos;documents\myFile.txt' --recursive` |
| **Esempio** (spazio dei nomi gerarchico) | `azcopy copy 'https://mystorageaccount.dfs.core.windows.net/mycontainer/FileDirectory' 'C:\myDirectory'  --include-path 'photos;documents\myFile.txt'--recursive` |

In questo esempio, AzCopy trasferisce la directory `https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory/photos` e il file di `https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory/documents/myFile.txt`. È necessario includere l'opzione `--recursive` per trasferire tutti i file nella directory `https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory/photos`.

È anche possibile escludere i file utilizzando l'opzione `--exclude-path`. Per altre informazioni, vedere la documentazione di riferimento per la [copia di azcopy](storage-ref-azcopy-copy.md) .

#### <a name="use-wildcard-characters"></a>Usa caratteri jolly

Usare il comando [azcopy Copy](storage-ref-azcopy-copy.md) con l'opzione `--include-pattern`. Specificare i nomi parziali che includono i caratteri jolly. Separare i nomi usando un semicolin (`;`).

|    |     |
|--------|-----------|
| **Sintassi** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-or-directory-name>' '<local-directory-path>' --include-pattern <semicolon-separated-file-list-with-wildcard-characters>` |
| **Esempio** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory' 'C:\myDirectory'  --include-pattern 'myFile*.txt;*.pdf*'` |
| **Esempio** (spazio dei nomi gerarchico) | `azcopy copy 'https://mystorageaccount.dfs.core.windows.net/mycontainer/FileDirectory' 'C:\myDirectory'  --include-pattern 'myFile*.txt;*.pdf*'` |

È anche possibile escludere i file utilizzando l'opzione `--exclude-pattern`. Per altre informazioni, vedere la documentazione di riferimento per la [copia di azcopy](storage-ref-azcopy-copy.md) .

Le opzioni `--include-pattern` e `--exclude-pattern` sono valide solo per i nomi file e non per il percorso.  Se si desidera copiare tutti i file di testo presenti in un albero di directory, utilizzare l'opzione `–recursive` per ottenere l'intero albero di directory, quindi utilizzare l'`–include-pattern` e specificare `*.txt` per ottenere tutti i file di testo.

## <a name="copy-blobs-between-storage-accounts"></a>Copiare i BLOB tra account di archiviazione

È possibile usare AzCopy per copiare i BLOB in altri account di archiviazione. L'operazione di copia è sincrona, quindi quando il comando restituisce, che indica che tutti i file sono stati copiati. 

AzCopy usa le [API](https://docs.microsoft.com/rest/api/storageservices/put-page-from-url) [da server a server](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) , quindi i dati vengono copiati direttamente tra i server di archiviazione. Queste operazioni di copia non utilizzano la larghezza di banda di rete del computer. È possibile aumentare la velocità effettiva di queste operazioni impostando il valore della variabile di ambiente `AZCOPY_CONCURRENCY_VALUE`. Per altre informazioni, vedere [ottimizzare la velocità effettiva](storage-use-azcopy-configure.md#optimize-throughput).

> [!NOTE]
> Questo scenario presenta le limitazioni seguenti nella versione corrente.
>
> - È necessario aggiungere un token SAS a ogni URL di origine. Se si forniscono le credenziali di autorizzazione usando Azure Active Directory (AD), è possibile omettere il token SAS solo dall'URL di destinazione.
>-  Gli account di archiviazione BLOB in blocchi Premium non supportano i livelli di accesso. Omettere il livello di accesso di un BLOB dall'operazione di copia impostando il `s2s-preserve-access-tier` su `false`, ad esempio `--s2s-preserve-access-tier=false`.

Questa sezione contiene gli esempi seguenti:

> [!div class="checklist"]
> * Copiare un BLOB in un altro account di archiviazione
> * Copiare una directory in un altro account di archiviazione
> * Copiare un contenitore in un altro account di archiviazione
> * Copia tutti i contenitori, le directory e i file in un altro account di archiviazione

Per informazioni dettagliate sulla documentazione di riferimento, vedere [azcopy Copy](storage-ref-azcopy-copy.md).

> [!TIP]
> Gli esempi in questa sezione racchiudono gli argomenti del percorso con virgolette singole (''). Usare le virgolette singole in tutte le shell dei comandi eccetto la shell dei comandi di Windows (cmd. exe). Se si usa una shell dei comandi di Windows (cmd. exe), racchiudere gli argomenti del percorso con virgolette doppie ("") anziché virgolette singole ('').

 Questi esempi funzionano anche con gli account che hanno uno spazio dei nomi gerarchico. L'accesso a più [protocolli su data Lake storage](../blobs/data-lake-storage-multi-protocol-access.md) consente di usare la stessa sintassi URL (`blob.core.windows.net`) per tali account. 

### <a name="copy-a-blob-to-another-storage-account"></a>Copiare un BLOB in un altro account di archiviazione

Utilizzare la stessa sintassi URL (`blob.core.windows.net`) per gli account che dispongono di uno spazio dei nomi gerarchico.

|    |     |
|--------|-----------|
| **Sintassi** | `azcopy copy 'https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<blob-path>?<SAS-token>' 'https://<destination-storage-account-name>.blob.core.windows.net/<container-name>/<blob-path>'` |
| **Esempio** | `azcopy copy 'https://mysourceaccount.blob.core.windows.net/mycontainer/myTextFile.txt?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net/mycontainer/myTextFile.txt'` |
| **Esempio** (spazio dei nomi gerarchico) | `azcopy copy 'https://mysourceaccount.blob.core.windows.net/mycontainer/myTextFile.txt?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net/mycontainer/myTextFile.txt'` |

### <a name="copy-a-directory-to-another-storage-account"></a>Copiare una directory in un altro account di archiviazione

Utilizzare la stessa sintassi URL (`blob.core.windows.net`) per gli account che dispongono di uno spazio dei nomi gerarchico.

|    |     |
|--------|-----------|
| **Sintassi** | `azcopy copy 'https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<directory-path>?<SAS-token>' 'https://<destination-storage-account-name>.blob.core.windows.net/<container-name>' --recursive` |
| **Esempio** | `azcopy copy 'https://mysourceaccount.blob.core.windows.net/mycontainer/myBlobDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net/mycontainer' --recursive` |
| **Esempio** (spazio dei nomi gerarchico)| `azcopy copy 'https://mysourceaccount.blob.core.windows.net/mycontainer/myBlobDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net/mycontainer' --recursive` |

### <a name="copy-a-container-to-another-storage-account"></a>Copiare un contenitore in un altro account di archiviazione

Utilizzare la stessa sintassi URL (`blob.core.windows.net`) per gli account che dispongono di uno spazio dei nomi gerarchico.

|    |     |
|--------|-----------|
| **Sintassi** | `azcopy copy 'https://<source-storage-account-name>.blob.core.windows.net/<container-name>?<SAS-token>' 'https://<destination-storage-account-name>.blob.core.windows.net/<container-name>' --recursive` |
| **Esempio** | `azcopy copy 'https://mysourceaccount.blob.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net/mycontainer' --recursive` |
| **Esempio** (spazio dei nomi gerarchico)| `azcopy copy 'https://mysourceaccount.blob.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net/mycontainer' --recursive` |

### <a name="copy-all-containers-directories-and-blobs-to-another-storage-account"></a>Copiare tutti i contenitori, le directory e i BLOB in un altro account di archiviazione

Utilizzare la stessa sintassi URL (`blob.core.windows.net`) per gli account che dispongono di uno spazio dei nomi gerarchico.

|    |     |
|--------|-----------|
| **Sintassi** | `azcopy copy 'https://<source-storage-account-name>.blob.core.windows.net/?<SAS-token>' 'https://<destination-storage-account-name>.blob.core.windows.net/' --recursive` |
| **Esempio** | `azcopy copy 'https://mysourceaccount.blob.core.windows.net?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net' --recursive` |
| **Esempio** (spazio dei nomi gerarchico)| `azcopy copy 'https://mysourceaccount.blob.core.windows.net?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net' --recursive` |

## <a name="synchronize-files"></a>Sincronizzare i file

È possibile sincronizzare il contenuto di un file system locale con un contenitore BLOB. È anche possibile sincronizzare i contenitori e le directory virtuali tra loro. La sincronizzazione è unidirezionale. In altre parole, è possibile scegliere quali di questi due endpoint sono l'origine e quella di destinazione. La sincronizzazione usa anche API da server a server. Gli esempi presentati in questa sezione funzionano anche con gli account che hanno uno spazio dei nomi gerarchico. 

> [!NOTE]
> La versione corrente di AzCopy non esegue la sincronizzazione tra altre origini e destinazioni, ad esempio file storage o Amazon Web Services (AWS) S3 bucket).

Il comando di [sincronizzazione](storage-ref-azcopy-sync.md) Confronta i nomi di file e i timestamp dell'Ultima modifica. Impostare il flag `--delete-destination` facoltativo sul valore `true` o `prompt` per eliminare i file nella directory di destinazione se tali file non sono più presenti nella directory di origine.

Se si imposta il flag di `--delete-destination` su `true` AzCopy Elimina i file senza fornire un prompt. Se si desidera che venga visualizzato un messaggio prima che AzCopy elimini un file, impostare il flag `--delete-destination` su `prompt`.

> [!NOTE]
> Per evitare eliminazioni accidentali, assicurarsi di abilitare la funzionalità di [eliminazione](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) temporanea prima di usare il flag di `--delete-destination=prompt|true`.

Per informazioni dettagliate sulla documentazione di riferimento, vedere [azcopy Sync](storage-ref-azcopy-sync.md).

> [!TIP]
> Gli esempi in questa sezione racchiudono gli argomenti del percorso con virgolette singole (''). Usare le virgolette singole in tutte le shell dei comandi eccetto la shell dei comandi di Windows (cmd. exe). Se si usa una shell dei comandi di Windows (cmd. exe), racchiudere gli argomenti del percorso con virgolette doppie ("") anziché virgolette singole ('').

### <a name="update-a-container-with-changes-to-a-local-file-system"></a>Aggiornare un contenitore con modifiche a una file system locale

In questo caso, il contenitore è la destinazione e la file system locale è l'origine. 

|    |     |
|--------|-----------|
| **Sintassi** | `azcopy sync '<local-directory-path>' 'https://<storage-account-name>.blob.core.windows.net/<container-name>' --recursive` |
| **Esempio** | `azcopy sync 'C:\myDirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer' --recursive` |

### <a name="update-a-local-file-system-with-changes-to-a-container"></a>Aggiornare un file system locale con le modifiche apportate a un contenitore

In questo caso, il file system locale è la destinazione e il contenitore è l'origine.

|    |     |
|--------|-----------|
| **Sintassi** | `azcopy sync 'https://<storage-account-name>.blob.core.windows.net/<container-name>' 'C:\myDirectory' --recursive` |
| **Esempio** | `azcopy sync 'https://mystorageaccount.blob.core.windows.net/mycontainer' 'C:\myDirectory' --recursive` |

### <a name="update-a-container-with-changes-in-another-container"></a>Aggiornare un contenitore con le modifiche in un altro contenitore

Il primo contenitore visualizzato in questo comando è l'origine. Il secondo è la destinazione.

|    |     |
|--------|-----------|
| **Sintassi** | `azcopy sync 'https://<source-storage-account-name>.blob.core.windows.net/<container-name>' 'https://<destination-storage-account-name>.blob.core.windows.net/<container-name>' --recursive` |
| **Esempio** | `azcopy sync 'https://mysourceaccount.blob.core.windows.net/mycontainer' 'https://mydestinationaccount.blob.core.windows.net/mycontainer' --recursive` |

### <a name="update-a-directory-with-changes-to-a-directory-in-another-file-share"></a>Aggiornare una directory con le modifiche apportate a una directory in un'altra condivisione file

La prima directory visualizzata in questo comando è l'origine. Il secondo è la destinazione.

|    |     |
|--------|-----------|
| **Sintassi** | `azcopy sync 'https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<directory-name>' 'https://<destination-storage-account-name>.blob.core.windows.net/<container-name>/<directory-name>' --recursive` |
| **Esempio** | `azcopy sync 'https://mysourceaccount.blob.core.windows.net/<container-name>/myDirectory' 'https://mydestinationaccount.blob.core.windows.net/mycontainer/myDirectory' --recursive` |

## <a name="next-steps"></a>Passaggi successivi

Altri esempi sono disponibili in uno di questi articoli:

- [Introduzione ad AzCopy](storage-use-azcopy-v10.md)

- [Esercitazione: eseguire la migrazione di dati locali nell'archiviazione cloud usando AzCopy](storage-use-azcopy-migrate-on-premises-data.md)

- [Trasferire dati con AzCopy e l'archivio file](storage-use-azcopy-files.md)

- [Trasferire dati con AzCopy e bucket Amazon S3](storage-use-azcopy-s3.md)

- [Configurare, ottimizzare e risolvere i problemi di AzCopy](storage-use-azcopy-configure.md)
