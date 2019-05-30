---
title: Azure Stack'te PowerShell kullanarak bir Linux VM oluşturma | Microsoft Docs
description: Azure Stack'te PowerShell kullanarak bir Linux VM oluşturun.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 03/11/2019
ms.author: mabrigg
ms.custom: mvc
ms.lastreviewed: 12/03/2018
ms.openlocfilehash: 4e60b2f2df2aae9cf27bdec41c6492ad3ff04fb3
ms.sourcegitcommit: 797dbacd1c6b8479d8c9189a939a13709228d816
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66269601"
---
# <a name="quickstart-create-a-linux-server-vm-using-powershell-in-azure-stack"></a>Hızlı Başlangıç: Bir Linux sunucusu VM'si oluşturma Azure Stack PowerShell'i kullanma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack PowerShell kullanarak Ubuntu Server 16.04 LTS sanal makine'de (VM) oluşturabilirsiniz. Bir VM oluşturup bu makaledeki adımları izleyin.  Bu makalede ayrıca adımları sunar:

* Bir uzak istemci ile VM'ye bağlanın.
* NGINX web sunucusu ve varsayılan giriş sayfasını görüntüleyin.
* Kullanılmayan kaynakları temizleyin.

## <a name="prerequisites"></a>Önkoşullar

* **Azure Stack marketini içindeki bir Linux görüntüsü**

   Azure Stack marketini varsayılan olarak bir Linux görüntüsü içermiyor. Azure Stack operatörü sağlamak için alma **Ubuntu Server 16.04 LTS** ihtiyacınız görüntü. İşleç açıklanan bu adımları kullanabilirsiniz [indirme Market öğesi Azure'dan Azure Stack'e](../operator/azure-stack-download-azure-marketplace-item.md) makalesi.

* Azure Stack, Azure kaynaklarını oluşturmak ve yönetmek için PowerShell belirli bir sürümünü gerektirir. Azure Stack için yapılandırılmış PowerShell yoksa, adımları [yükleme](../operator/azure-stack-powershell-install.md) PowerShell.

* Azure Stack ayarlanan PowerShell ile Azure Stack ortamınıza bağlamanız gerekecektir. Yönergeler için bkz [bir kullanıcı olarak PowerShell ile Azure stack'e bağlanma](azure-stack-powershell-configure-user.md).

* Bir ortak SSH anahtarının Windows kullanıcı profilinizin .ssh dizininde kaydedilen adı id_rsa.pub ile. SSH anahtarları oluşturma hakkında ayrıntılı bilgi için bkz: [SSH ortak anahtarı kullanmayı](azure-stack-dev-start-howto-ssh-public-key.md).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturun

Bir kaynak grubu, dağıtma ve Azure Stack kaynaklarını yönetme mantıksal bir kapsayıcıdır. Uygulamanızı Geliştirme Seti veya Azure Stack tümleşik sistemi, bir kaynak grubu oluşturmak için aşağıdaki kod bloğu çalıştırın. 

> [!NOTE]
> Kod örnekleri tüm değişkenler için değerler atanır. Ancak, isterseniz yeni değerler atayabilirsiniz.

```powershell  
# Create variables to store the location and resource group names.
$location = "local"
$ResourceGroupName = "myResourceGroup"

New-AzureRmResourceGroup `
  -Name $ResourceGroupName `
  -Location $location
```

## <a name="create-storage-resources"></a>Depolama kaynakları oluşturma

Bir depolama hesabı oluşturun ve ardından Ubuntu Server 16.04 LTS görüntüsüne için bir depolama kapsayıcısı oluşturun.

```powershell  
# Create variables to store the storage account name and the storage account SKU information
$StorageAccountName = "mystorageaccount"
$SkuName = "Standard_LRS"

# Create a new storage account
$StorageAccount = New-AzureRMStorageAccount `
  -Location $location `
  -ResourceGroupName $ResourceGroupName `
  -Type $SkuName `
  -Name $StorageAccountName

Set-AzureRmCurrentStorageAccount `
  -StorageAccountName $storageAccountName `
  -ResourceGroupName $resourceGroupName

```

## <a name="create-networking-resources"></a>Ağ kaynakları oluşturma

Bir sanal ağ, alt ağ ve genel IP adresi oluşturun. Bu kaynaklar VM ağ bağlantısı sağlamak için kullanılır.

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -Name MyVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -AllocationMethod Static `
  -IdleTimeoutInMinutes 4 `
  -Name "mypublicdns$(Get-Random)"

```

### <a name="create-a-network-security-group-and-a-network-security-group-rule"></a>Ağ güvenliği grubu ve ağ güvenliği grup kuralı oluşturma

Ağ güvenlik grubu, gelen ve giden kuralları kullanarak sanal Makinenin güvenliğini sağlar. Gelen Uzak Masaüstü bağlantılarına izin verecek şekilde 3389 numaralı bağlantı noktası için bir gelen kuralı ve gelen web trafiğine izin vermek için 80 numaralı bağlantı noktası için bir gelen kuralı oluşturun.

```powershell
# Create variables to store the network security group and rules names.
$nsgName = "myNetworkSecurityGroup"
$nsgRuleSSHName = "myNetworkSecurityGroupRuleSSH"
$nsgRuleWebName = "myNetworkSecurityGroupRuleWeb"


# Create an inbound network security group rule for port 22
$nsgRuleSSH = New-AzureRmNetworkSecurityRuleConfig -Name $nsgRuleSSHName -Protocol Tcp `
-Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 22 -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name $nsgRuleWebName -Protocol Tcp `
-Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $ResourceGroupName -Location $location `
-Name $nsgName -SecurityRules $nsgRuleSSH,$nsgRuleWeb
```

### <a name="create-a-network-card-for-the-vm"></a>VM için bir ağ kartı oluşturun

Ağ kartı VM alt ağ, ağ güvenlik grubu ve genel IP adresine bağlanır.

```powershell
# Create a virtual network card and associate it with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
  -Name myNic `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id `
  -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-a-vm"></a>VM oluşturma

Bir VM yapılandırması oluşturun. Bu yapılandırma, sanal Makineyi dağıtırken kullanılan ayarları içerir. Örneğin: kullanıcı kimlik bilgilerini, boyutu ve VM görüntüsü.

```powershell
# Define a credential object.
$UserName='demouser'
$securePassword = ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ($UserName, $securePassword)

# Create the VM configuration object
$VmName = "VirtualMachinelatest"
$VmSize = "Standard_D1"
$VirtualMachine = New-AzureRmVMConfig `
  -VMName $VmName `
  -VMSize $VmSize

$VirtualMachine = Set-AzureRmVMOperatingSystem `
  -VM $VirtualMachine `
  -Linux `
  -ComputerName "MainComputer" `
  -Credential $cred

$VirtualMachine = Set-AzureRmVMSourceImage `
  -VM $VirtualMachine `
  -PublisherName "Canonical" `
  -Offer "UbuntuServer" `
  -Skus "16.04-LTS" `
  -Version "latest"

# Sets the operating system disk properties on a VM.
$VirtualMachine = Set-AzureRmVMOSDisk `
  -VM $VirtualMachine `
  -CreateOption FromImage | `
  Set-AzureRmVMBootDiagnostics -ResourceGroupName $ResourceGroupName `
  -StorageAccountName $StorageAccountName -Enable |`
  Add-AzureRmVMNetworkInterface -Id $nic.Id


# Configure SSH Keys
$sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"

# Adds the SSH Key to the VM
Add-AzureRmVMSshPublicKey -VM $VirtualMachine `
 -KeyData $sshPublicKey `
 -Path "/home/azureuser/.ssh/authorized_keys"

# Create the VM.
New-AzureRmVM `
  -ResourceGroupName $ResourceGroupName `
 -Location $location `
  -VM $VirtualMachine
```

## <a name="quick-create-vm---full-script"></a>Hızlı Oluştur VM - tam betik

> [!NOTE]
> Bu arada ancak bir parola ile birleştirilmiş temelde Yukarıdaki kod, kimlik doğrulaması için SSH anahtarı yerine.

```powershell
## Create a resource group

<#
A resource group is a logical container where you can deploy and manage Azure Stack resources. From your development kit or the Azure Stack integrated system, run the following code block to create a resource group. Values are assigned for all the variables in this document, you can use these values or assign new values.
#>

# Edit your variables here if required

# Create variables to store the location and resource group names.
$location = "local"
$ResourceGroupName = "myResourceGroup"

# Create variables to store the storage account name and the storage account SKU information
$StorageAccountName = "mystorageaccount"
$SkuName = "Standard_LRS"

# Create variables to store the network security group and rules names.
$nsgName = "myNetworkSecurityGroup"
$nsgRuleSSHName = "myNetworkSecurityGroupRuleSSH"
$nsgRuleWebName = "myNetworkSecurityGroupRuleWeb"

# Create variable for VM password
$VMPassword = 'Password123!'

# End of Variables - no need to edit anything past that point to deploy a single VM

# Create Resource Group
New-AzureRmResourceGroup `
  -Name $ResourceGroupName `
  -Location $location

## Create storage resources

# Create a storage account and then create a storage container for the Ubuntu Server 16.04 LTS image.

# Create a new storage account
$StorageAccount = New-AzureRMStorageAccount `
  -Location $location `
  -ResourceGroupName $ResourceGroupName `
  -Type $SkuName `
  -Name $StorageAccountName

Set-AzureRmCurrentStorageAccount `
  -StorageAccountName $storageAccountName `
  -ResourceGroupName $resourceGroupName

# Create a storage container to store the VM image
$containerName = 'osdisks'
$container = New-AzureStorageContainer `
  -Name $containerName `
  -Permission Blob


## Create networking resources

# Create a virtual network, subnet, and a public IP address. These resources are used to provide network connectivity to the VM.

# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -Name MyVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -AllocationMethod Static `
  -IdleTimeoutInMinutes 4 `
  -Name "mypublicdns$(Get-Random)"


### Create a network security group and a network security group rule

<#
The network security group secures the VM by using inbound and outbound rules. Create an inbound rule for port 3389 to allow incoming Remote Desktop connections and an inbound rule for port 80 to allow incoming web traffic.
#>

# Create an inbound network security group rule for port 22
$nsgRuleSSH = New-AzureRmNetworkSecurityRuleConfig -Name $nsgRuleSSHName -Protocol Tcp `
-Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 22 -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name $nsgRuleWebName -Protocol Tcp `
-Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $ResourceGroupName -Location $location `
-Name $nsgName -SecurityRules $nsgRuleSSH,$nsgRuleWeb

### Create a network card for the VM

# The network card connects the VM to a subnet, network security group, and public IP address.

# Create a virtual network card and associate it with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
  -Name myNic `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id `
  -NetworkSecurityGroupId $nsg.Id

## Create a VM
<#
Create a VM configuration. This configuration includes the settings used when deploying the VM. For example: user credentials, size, and the VM image.
#>

# Define a credential object.
$UserName='demouser'
$securePassword = ConvertTo-SecureString $VMPassword -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ($UserName, $securePassword)

# Create the VM configuration object
$VmName = "VirtualMachinelatest"
$VmSize = "Standard_D1"
$VirtualMachine = New-AzureRmVMConfig `
  -VMName $VmName `
  -VMSize $VmSize

$VirtualMachine = Set-AzureRmVMOperatingSystem `
  -VM $VirtualMachine `
  -Linux `
  -ComputerName "MainComputer" `
  -Credential $cred

$VirtualMachine = Set-AzureRmVMSourceImage `
  -VM $VirtualMachine `
  -PublisherName "Canonical" `
  -Offer "UbuntuServer" `
  -Skus "16.04-LTS" `
  -Version "latest"

$osDiskName = "OsDisk"
$osDiskUri = '{0}vhds/{1}-{2}.vhd' -f `
  $StorageAccount.PrimaryEndpoints.Blob.ToString(),`
  $vmName.ToLower(), `
  $osDiskName

# Sets the operating system disk properties on a VM.
$VirtualMachine = Set-AzureRmVMOSDisk `
  -VM $VirtualMachine `
  -Name $osDiskName `
  -VhdUri $OsDiskUri `
  -CreateOption FromImage | `
  Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create the VM.
New-AzureRmVM `
  -ResourceGroupName $ResourceGroupName `
 -Location $location `
  -VM $VirtualMachine
```

## <a name="connect-to-the-vm"></a>VM’ye bağlanma

VM dağıtıldıktan sonra VM için bir SSH bağlantısı yapılandırın. Kullanım [Get-Azurermpublicıpaddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) VM'nin genel IP adresini almak için komutu.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

SSH yüklü olan bir istemci sisteminden sanal Makineye bağlanmak için aşağıdaki komutu kullanın. Windows üzerinde çalışıyorsanız, kullanabileceğiniz [Putty](https://www.putty.org/) bağlantı oluşturmak için.

```
ssh <Public IP Address>
```

İstendiğinde, olarak oturum açın **azureuser**. SSH anahtarları oluştururken bir parola kullandıysanız parolayı girmeniz gerekir.

## <a name="install-the-nginx-web-server"></a>NGINX web sunucusunu yükleme

Paket kaynaklarını güncelleştirmek ve en son NGINX paketini yüklemek için aşağıdaki betiği çalıştırın:

```bash
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-nginx-welcome-page"></a>NGINX karşılama sayfasını görüntüleme

NGINX ve 80 numaralı bağlantı noktası sanal makinenizde ile sanal makinenin genel IP adresini kullanarak web sunucusuna erişebilirsiniz. Bir web tarayıcısı açın ve gidin ```http://<public IP address>```.

![NGINX web-server-Hoş Geldiniz sayfası](./media/azure-stack-quick-create-vm-linux-cli/nginx.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız olmayan kaynakları temizleyin. Kullanabileceğiniz [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) bu kaynakları kaldırmak için komutu. Kaynak grubunu ve tüm kaynaklarını silmek için aşağıdaki komutu çalıştırın:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta basit bir Linux sunucusu VM dağıtılır. Azure Stack sanal makineleri hakkında daha fazla bilgi için Git [Azure Stack VM özellikleri](azure-stack-vm-considerations.md).
