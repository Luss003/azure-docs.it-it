---
title: Configurare un certificato di crittografia nei cluster Windows
description: Informazioni su come configurare un certificato di crittografia e crittografare segreti in cluster Windows.
author: vturecek
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: vturecek
ms.openlocfilehash: d9413a37be221adc375836719dc1f467a5571fa0
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/02/2020
ms.locfileid: "75610183"
---
# <a name="set-up-an-encryption-certificate-and-encrypt-secrets-on-windows-clusters"></a>Configurare un certificato di crittografia e crittografare segreti in cluster Windows
Questo articolo illustra come configurare un certificato di crittografia e usarlo per crittografare segreti in cluster Windows. Per i cluster Linux, vedere [configurare un certificato di crittografia e crittografare i segreti nei cluster Linux.][secret-management-linux-specific-link]

[Azure Key Vault][key-vault-get-started] viene usato come percorso di archiviazione sicuro per i certificati e come modo per ottenere i certificati installati nei cluster Service fabric in Azure. Se non si esegue la distribuzione in Azure, non è necessario usare l'insieme di credenziali delle chiavi per gestire i segreti nelle applicazioni di Service Fabric. Tuttavia, l' *uso* di segreti in un'applicazione è indipendente dalla piattaforma cloud per consentire alle applicazioni di essere distribuite in un cluster ospitato in un punto qualsiasi. 

## <a name="obtain-a-data-encipherment-certificate"></a>Ottenere un certificato di crittografia dei dati
Un certificato di crittografia dei dati viene usato esclusivamente per la crittografia e la decrittografia dei [parametri][parameters-link] nelle variabili Settings. XML e di [ambiente][environment-variables-link] di un servizio in ServiceManifest. XML di un servizio. Non viene usato per l'autenticazione o per la firma di testo crittografato. Il certificato deve soddisfare i requisiti seguenti:

* Il certificato deve includere una chiave privata.
* Il certificato deve essere stato creato per lo scambio di chiave, esportabile in un file con estensione pfx (Personal Information Exchange).
* L'uso delle chiavi del certificato deve includere la crittografia dei dati (10) e non deve includere l'autenticazione del server o del client. 
  
  Ad esempio, quando si crea un certificato autofirmato tramite PowerShell, il flag `KeyUsage` deve essere impostato su `DataEncipherment`:
  
  ```powershell
  New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
  ```

## <a name="install-the-certificate-in-your-cluster"></a>Installare il certificato nel cluster
Questo certificato deve essere installato su ogni nodo del cluster. Per istruzioni sull'installazione, vedere [come creare un cluster usando Azure Resource Manager][service-fabric-cluster-creation-via-arm] . 

## <a name="encrypt-application-secrets"></a>Eseguire la crittografia dei segreti dell'applicazione
Il comando PowerShell seguente viene usato per crittografare un segreto. Questo comando consente unicamente di crittografare il valore e **non** firma il testo crittografato. È necessario usare lo stesso certificato di crittografia installato nel cluster per produrre il testo crittografato per i valori del segreto:

```powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

La stringa con codifica Base 64 risultante contiene sia il testo crittografato del segreto sia le informazioni relative al certificato usato per crittografarlo.

## <a name="next-steps"></a>Passaggi successivi
Informazioni su come [specificare i segreti crittografati in un'applicazione.][secret-management-specify-encrypted-secrets-link]

<!-- Links -->
[key-vault-get-started]:../key-vault/key-vault-overview.md
[service-fabric-cluster-creation-via-arm]: service-fabric-cluster-creation-via-arm.md
[parameters-link]:service-fabric-how-to-parameterize-configuration-files.md
[environment-variables-link]: service-fabric-how-to-specify-environment-variables.md
[secret-management-linux-specific-link]: service-fabric-application-secret-management-linux.md
[secret-management-specify-encrypted-secrets-link]: service-fabric-application-secret-management.md#specify-encrypted-secrets-in-an-application
