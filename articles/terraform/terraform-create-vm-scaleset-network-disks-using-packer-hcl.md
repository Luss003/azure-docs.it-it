---
title: "Esercitazione: Creare un set di scalabilità di macchine virtuali di Azure da un'immagine personalizzata Packer usando Terraform"
description: Usare Terraform per configurare e specificare la versione di un set di scalabilità di macchine virtuali da un'immagine personalizzata generata da Packer (completo di una rete virtuale e di dischi collegati gestiti).
ms.topic: tutorial
ms.date: 11/07/2019
ms.openlocfilehash: 92a8221d625f8b6b73343f74b85fdfcf5e578b23
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77472206"
---
# <a name="tutorial-create-an-azure-virtual-machine-scale-set-from-a-packer-custom-image-by-using-terraform"></a>Esercitazione: Creare un set di scalabilità di macchine virtuali di Azure da un'immagine personalizzata Packer usando Terraform

In questa esercitazione viene usato [Terraform](https://www.terraform.io/) per creare e distribuire un [set di scalabilità di macchine virtuali di Azure](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview) creato con un'immagine personalizzata generata usando [Packer](https://www.packer.io/intro/index.html) con dischi gestiti che usano [HLC (HashiCorp Configuration Language)](https://www.terraform.io/docs/configuration/syntax.html). 

In questa esercitazione verranno illustrate le procedure per:

> [!div class="checklist"]
> * Configurare la distribuzione di Terraform.
> * Usare variabili e output per la distribuzione di Terraform.
> * Creare e distribuire un'infrastruttura di rete.
> * Creare un'immagine di macchina virtuale personalizzata usando Packer.
> * Creare e distribuire un set di scalabilità di macchine virtuali usando l'immagine personalizzata.
> * Creare e distribuire un jumpbox.

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.

## <a name="prerequisites"></a>Prerequisites

- **Terraform**: [installare Terraform e configurare l'accesso ad Azure](terraform-install-configure.md).
- **Coppia di chiavi SSH**: [creare una coppia di chiavi SSH](/azure/virtual-machines/linux/mac-create-ssh-keys).
- **Packer**:  [installare Packer](https://www.packer.io/docs/install/index.html).

## <a name="create-the-file-structure"></a>Creare la struttura di file

Creare tre nuovi file in una directory vuota con i nomi seguenti:

- `variables.tf`: Questo file contiene i valori delle variabili usate nel modello.
- `output.tf`: Questo file descrive le impostazioni visualizzate dopo la distribuzione.
- `vmss.tf`: Questo file contiene il codice dell'infrastruttura da distribuire.

##  <a name="create-the-variables"></a>Creare le variabili 

In questo passaggio vengono definite le variabili che consentono di personalizzare le risorse create da Terraform.

Modificare il file `variables.tf`, copiare il codice seguente e quindi salvare le modifiche.

```hcl
variable "location" {
  description = "The location where resources are created"
  default     = "East US"
}

variable "resource_group_name" {
  description = "The name of the resource group in which the resources are created"
  default     = ""
}

```

> [!NOTE]
> L'impostazione del valore predefinito della variabile resource_group_name viene annullata. Definire il proprio valore.

Salvare il file.

Quando si distribuisce il modello di Terraform, è necessario ottenere il nome di dominio completo usato per accedere all'applicazione. Usare il tipo di risorsa `output` di Terraform e ottenere la proprietà `fqdn` della risorsa. 

Modificare il file `output.tf` e copiare il codice seguente per esporre il nome di dominio completo per le macchine virtuali. 

```hcl 
output "vmss_public_ip" {
    value = azurerm_public_ip.vmss.fqdn
}
```

## <a name="define-the-network-infrastructure-in-a-template"></a>Definire l'infrastruttura di rete in un modello 

In questo passaggio viene creata l'infrastruttura di rete seguente in un nuovo gruppo di risorse di Azure: 
  - Una rete virtuale con lo spazio indirizzi 10.0.0.0/16.
  - Una subnet con lo spazio indirizzi 10.0.2.0/24.
  - Due indirizzi IP pubblici. Uno viene usato dal servizio di bilanciamento del carico del set di scalabilità di macchine virtuali. L'altro viene usato per la connessione al jumpbox SSH.

È anche necessario un gruppo di risorse in cui tutte le risorse vengono create. 

Modificare e copiare il codice seguente nel file `vmss.tf`: 

```hcl

resource "azurerm_resource_group" "vmss" {
  name     = var.resource_group_name
  location = var.location

  tags {
    environment = "codelab"
  }
}

resource "azurerm_virtual_network" "vmss" {
  name                = "vmss-vnet"
  address_space       = ["10.0.0.0/16"]
  location            = var.location
  resource_group_name = azurerm_resource_group.vmss.name

  tags {
    environment = "codelab"
  }
}

resource "azurerm_subnet" "vmss" {
  name                 = "vmss-subnet"
  resource_group_name  = azurerm_resource_group.vmss.name
  virtual_network_name = azurerm_virtual_network.vmss.name
  address_prefix       = "10.0.2.0/24"
}

resource "azurerm_public_ip" "vmss" {
  name                         = "vmss-public-ip"
  location                     = var.location
  resource_group_name          = azurerm_resource_group.vmss.name
  allocation_method            = "static"
  domain_name_label            = azurerm_resource_group.vmss.name

  tags {
    environment = "codelab"
  }
}

``` 

> [!NOTE]
> Contrassegnare le risorse da distribuire in Azure per semplificarne l'identificazione in futuro.

## <a name="create-the-network-infrastructure"></a>Creare l'infrastruttura di rete

Inizializzare l'ambiente Terraform eseguendo il comando seguente nella directory in cui sono stati creati i file `.tf`:

```bash
terraform init 
```
 
Il provider avvia il download dal registro di Terraform alla cartella `.terraform` nella directory in cui è stato eseguito il comando.

Eseguire il comando seguente per distribuire l'infrastruttura in Azure.

```bash
terraform apply
```

Verificare che il nome di dominio completo dell'indirizzo IP pubblico corrisponda alla configurazione.

![Nome di dominio completo di Terraform del set di scalabilità di macchine virtuali per l'indirizzo IP pubblico](./media/terraform-create-vm-scaleset-network-disks-using-packer-hcl/tf-create-vmss-step4-fqdn.png)

Il gruppo di risorse contiene le risorse seguenti:

![Risorse di rete di Terraform del set di scalabilità di macchine virtuali](./media/terraform-create-vm-scaleset-network-disks-using-packer-hcl/tf-create-vmss-step4-rg.png)


## <a name="create-an-azure-image-by-using-packer"></a>Creare un'immagine di Azure usando Packer
Creare un'immagine Linux personalizzata seguendo i passaggi dell'esercitazione [Come usare Packer per creare immagini di macchine virtuali Linux in Azure](/azure/virtual-machines/linux/build-image-with-packer).
 
Seguire l'esercitazione per creare un'immagine di Ubuntu di cui è stato effettuato il deprovisioning con Nginx installato.

![Dopo avere creato l'immagine di Packer, è disponibile un'immagine](./media/terraform-create-vm-scaleset-network-disks-using-packer-hcl/packerimagecreated.png)

> [!NOTE]
> Ai fini dell'esercitazione, nell'immagine di Packer viene eseguito un comando per installare Nginx. È anche possibile eseguire il proprio script durante la creazione.

## <a name="edit-the-infrastructure-to-add-the-virtual-machine-scale-set"></a>Modificare l'infrastruttura per aggiungere il set di scalabilità di macchine virtuali

In questo passaggio si creano le risorse seguenti nella rete distribuita prima:
- Un servizio di bilanciamento del carico di Azure per l'applicazione. Collegarlo all'indirizzo IP pubblico distribuito in precedenza.
- Un servizio di bilanciamento del carico e regole di Azure per l'applicazione. Collegarlo all'indirizzo IP pubblico configurato in precedenza.
- Un pool di indirizzi back-end di Azure. Assegnarlo al servizio di bilanciamento del carico.
- Una porta probe di integrità usata dall'applicazione e configurata nel servizio di bilanciamento del carico.
- Un set di scalabilità di macchine virtuali che si trova dietro il servizio di bilanciamento del carico e viene eseguito nella rete virtuale distribuita in precedenza.
- [Nginx](https://nginx.org/) nei nodi del set di scalabilità di macchine virtuali installato dall'immagine personalizzata.


Aggiungere il codice seguente alla fine del file `vmss.tf`.

```hcl

resource "azurerm_lb" "vmss" {
  name                = "vmss-lb"
  location            = var.location
  resource_group_name = azurerm_resource_group.vmss.name

  frontend_ip_configuration {
    name                 = "PublicIPAddress"
    public_ip_address_id = azurerm_public_ip.vmss.id
  }

  tags {
    environment = "codelab"
  }
}

resource "azurerm_lb_backend_address_pool" "bpepool" {
  resource_group_name = azurerm_resource_group.vmss.name
  loadbalancer_id     = azurerm_lb.vmss.id
  name                = "BackEndAddressPool"
}

resource "azurerm_lb_probe" "vmss" {
  resource_group_name = azurerm_resource_group.vmss.name
  loadbalancer_id     = azurerm_lb.vmss.id
  name                = "ssh-running-probe"
  port                = var.application_port
}

resource "azurerm_lb_rule" "lbnatrule" {
  resource_group_name            = azurerm_resource_group.vmss.name
  loadbalancer_id                = azurerm_lb.vmss.id
  name                           = "http"
  protocol                       = "Tcp"
  frontend_port                  = var.application_port
  backend_port                   = var.application_port
  backend_address_pool_id        = azurerm_lb_backend_address_pool.bpepool.id
  frontend_ip_configuration_name = "PublicIPAddress"
  probe_id                       = azurerm_lb_probe.vmss.id
}

data "azurerm_resource_group" "image" {
  name = "myResourceGroup"
}

data "azurerm_image" "image" {
  name                = "myPackerImage"
  resource_group_name = data.azurerm_resource_group.image.name
}

resource "azurerm_virtual_machine_scale_set" "vmss" {
  name                = "vmscaleset"
  location            = var.location
  resource_group_name = azurerm_resource_group.vmss.name
  upgrade_policy_mode = "Manual"

  sku {
    name     = "Standard_DS1_v2"
    tier     = "Standard"
    capacity = 2
  }

  storage_profile_image_reference {
    id=data.azurerm_image.image.id
  }

  storage_profile_os_disk {
    name              = ""
    caching           = "ReadWrite"
    create_option     = "FromImage"
    managed_disk_type = "Standard_LRS"
  }

  storage_profile_data_disk {
    lun          = 0
    caching        = "ReadWrite"
    create_option  = "Empty"
    disk_size_gb   = 10
  }

  os_profile {
    computer_name_prefix = "vmlab"
    admin_username       = "azureuser"
    admin_password       = "Passwword1234"
  }

  os_profile_linux_config {
    disable_password_authentication = true

    ssh_keys {
      path     = "/home/azureuser/.ssh/authorized_keys"
      key_data = file("~/.ssh/id_rsa.pub")
    }
  }

  network_profile {
    name    = "terraformnetworkprofile"
    primary = true

    ip_configuration {
      name                                   = "IPConfiguration"
      subnet_id                              = azurerm_subnet.vmss.id
      load_balancer_backend_address_pool_ids = [azurerm_lb_backend_address_pool.bpepool.id]
      primary = true
    }
  }
  
  tags {
    environment = "codelab"
  }
}

```

Personalizzare la distribuzione aggiungendo il codice seguente a `variables.tf`:

```hcl
variable "application_port" {
    description = "The port that you want to expose to the external load balancer"
    default     = 80
}

variable "admin_password" {
    description = "Default password for admin"
    default = "Passwwoord11223344"
}
``` 


## <a name="deploy-the-virtual-machine-scale-set-in-azure"></a>Distribuire il set di scalabilità di macchine virtuali in Azure

Eseguire il comando seguente per visualizzare la distribuzione del set di scalabilità di macchine virtuali:

```bash
terraform plan
```

L'output del comando è simile all'immagine seguente:

![Piano di aggiunta del set di scalabilità di macchine virtuali di Terraform](./media/terraform-create-vm-scaleset-network-disks-using-packer-hcl/tf-create-vmss-step6-plan.png)

Distribuire le risorse aggiuntive in Azure: 

```bash
terraform apply 
```

Il contenuto del gruppo di risorse è simile all'immagine seguente:

![Gruppo di risorse del set di scalabilità di macchine virtuali di Terraform](./media/terraform-create-vm-scaleset-network-disks-using-packer-hcl/tf-create-vmss-step6-apply.png)

Aprire un browser e connettersi al nome di dominio completo restituito dal comando. 


## <a name="add-a-jumpbox-to-the-existing-network"></a>Aggiungere un jumpbox alla rete esistente 

Questo passaggio facoltativo consente l'accesso SSH alle istanze del set di scalabilità di macchine virtuali usando un jumpbox.

Aggiungere le risorse seguenti alla distribuzione esistente:
- Un'interfaccia di rete connessa alla stessa subnet del set di scalabilità di macchine virtuali
- Una macchina virtuale con questa interfaccia di rete

Aggiungere il codice seguente alla fine del file `vmss.tf`:

```hcl 
resource "azurerm_public_ip" "jumpbox" {
  name                         = "jumpbox-public-ip"
  location                     = var.location
  resource_group_name          = azurerm_resource_group.vmss.name
  allocation_method            = "static"
  domain_name_label            = "${azurerm_resource_group.vmss.name}-ssh"

  tags {
    environment = "codelab"
  }
}

resource "azurerm_network_interface" "jumpbox" {
  name                = "jumpbox-nic"
  location            = var.location
  resource_group_name = azurerm_resource_group.vmss.name

  ip_configuration {
    name                          = "IPConfiguration"
    subnet_id                     = azurerm_subnet.vmss.id
    private_ip_address_allocation = "dynamic"
    public_ip_address_id          = azurerm_public_ip.jumpbox.id
  }

  tags {
    environment = "codelab"
  }
}

resource "azurerm_virtual_machine" "jumpbox" {
  name                  = "jumpbox"
  location              = var.location
  resource_group_name   = azurerm_resource_group.vmss.name
  network_interface_ids = [azurerm_network_interface.jumpbox.id]
  vm_size               = "Standard_DS1_v2"

  storage_image_reference {
    publisher = "Canonical"
    offer     = "UbuntuServer"
    sku       = "16.04-LTS"
    version   = "latest"
  }

  storage_os_disk {
    name              = "jumpbox-osdisk"
    caching           = "ReadWrite"
    create_option     = "FromImage"
    managed_disk_type = "Standard_LRS"
  }

  os_profile {
    computer_name  = "jumpbox"
    admin_username = "azureuser"
    admin_password = "Password1234!"
  }

  os_profile_linux_config {
    disable_password_authentication = true

    ssh_keys {
      path     = "/home/azureuser/.ssh/authorized_keys"
      key_data = file("~/.ssh/id_rsa.pub")
    }
  }

  tags {
    environment = "codelab"
  }
}
```

Modificare `outputs.tf` per aggiungere il codice seguente che visualizza il nome host del jumpbox al termine della distribuzione:

```
output "jumpbox_public_ip" {
    value = azurerm_public_ip.jumpbox.fqdn
}
```

## <a name="deploy-the-jumpbox"></a>Distribuire il jumpbox

Distribuire il jumpbox.

```bash
terraform apply 
```

Una volta completata la distribuzione, il contenuto del gruppo di risorse sarà simile all'immagine seguente:

![Gruppo di risorse del set di scalabilità di macchine virtuali di Terraform](./media/terraform-create-vm-scaleset-network-disks-using-packer-hcl/tf-create-create-vmss-step8.png)

> [!NOTE]
> L'accesso con una password è disabilitato nel jumpbox e nel set di scalabilità di macchine virtuali distribuito. Accedere con SSH per accedere alle macchine virtuali.

## <a name="clean-up-the-environment"></a>Pulire l'ambiente

I comandi seguenti eliminano le risorse create in questa esercitazione:

```bash
terraform destroy
```

Immettere *Sì* quando viene chiesto di confermare l'eliminazione delle risorse. Il processo di eliminazione può richiedere alcuni minuti.

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"] 
> [Vedere altre informazioni sull'uso di Terraform in Azure](/azure/terraform)
