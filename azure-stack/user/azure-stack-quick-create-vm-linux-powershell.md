---
title: Azure Stack 'de PowerShell kullanarak bir Linux VM oluşturma | Microsoft Docs
description: Azure Stack 'de PowerShell kullanarak bir Linux VM oluşturun.
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
ms.date: 11/11/2019
ms.author: mabrigg
ms.custom: mvc
ms.lastreviewed: 11/11/2019
ms.openlocfilehash: 9d1c9f963433e0a41218bb1984d07f0b47d032eb
ms.sourcegitcommit: 102ef41963b5d2d91336c84f2d6af3fdf2ce11c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73955698"
---
# <a name="quickstart-create-a-linux-server-vm-by-using-powershell-in-azure-stack"></a>Hızlı başlangıç: Azure Stack 'de PowerShell kullanarak bir Linux sunucu VM oluşturma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack PowerShell kullanarak bir Ubuntu Server 16,04 LTS sanal makinesi (VM) oluşturabilirsiniz. Bu makalede, bir sanal makine oluşturup kullanacaksınız. Bu makalede ayrıca nasıl yapılacağı gösterilmektedir:

* Uzak bir istemciyle VM 'ye bağlanın.
* Bir NGıNX Web sunucusu yükleyip varsayılan giriş sayfasını görüntüleyin.
* Kullanılmayan kaynakları temizleyin.

## <a name="prerequisites"></a>Önkoşullar

* Azure Stack Market 'teki bir Linux görüntüsü. Azure Stack marketi 'nin varsayılan olarak bir Linux görüntüsü yoktur. Azure Stack işlecine ihtiyacınız olan Ubuntu Server 16,04 LTS görüntüsünü sağlayın. İşleci, [Azure 'Dan Market öğelerini indirme bölümündeki yönergeleri Azure Stack için](../operator/azure-stack-download-azure-marketplace-item.md)kullanabilir.

* Azure Stack, kaynaklarını oluşturmak ve yönetmek için Azure CLı 'nin belirli bir sürümünü gerektirir. 
  * Azure Stack için PowerShell 'i yapılandırmadıysanız, bkz. [Azure Stack Için PowerShell 'ı yüklemeyi](../operator/azure-stack-powershell-install.md). 
  * Azure Stack PowerShell kurulduktan sonra, Azure Stack ortamınıza bağlanırsınız. Yönergeler için bkz. [PowerShell ile Kullanıcı olarak Azure Stack bağlama](azure-stack-powershell-configure-user.md).

* Windows Kullanıcı profilinizin *. SSH* dizinine kayıtlı *id_rsa. pub* adlı BIR genel Secure Shell (SSH) anahtarı. SSH anahtarları oluşturma hakkında ayrıntılı bilgi için bkz. [SSH ortak anahtarı kullanma](azure-stack-dev-start-howto-ssh-public-key.md).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu Azure Stack kaynaklarını dağıtabileceğiniz ve yönetebileceğiniz bir mantıksal kapsayıcıdır. Azure Stack Geliştirme Seti (ASDK) veya Azure Stack tümleşik sistemden bir kaynak grubu oluşturmak için aşağıdaki kod bloğunu çalıştırın: 

> [!NOTE]
> Aşağıdaki kod örneklerinde tüm değişkenler için değerler atandık. Ancak, kendi değerlerinizi atayabilirsiniz.

```powershell  
# Create variables to store the location and resource group names.
$location = "local"
$ResourceGroupName = "myResourceGroup"

New-AzureRmResourceGroup `
  -Name $ResourceGroupName `
  -Location $location
```

## <a name="create-storage-resources"></a>Depolama kaynakları oluşturma

Bir depolama hesabı oluşturun ve ardından Ubuntu Server 16,04 LTS görüntüsü için bir depolama kapsayıcısı oluşturun.

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

Bir sanal ağ, alt ağ ve genel IP adresi oluşturun. Bu kaynaklar VM 'ye ağ bağlantısı sağlamak için kullanılır.

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

Ağ güvenlik grubu, gelen ve giden kurallarını kullanarak VM 'nin güvenliğini sağlar. Gelen Uzak Masaüstü bağlantılarına izin vermek için bağlantı noktası 3389 için bir gelen kuralı ve gelen Web trafiğine izin vermek için 80 numaralı bağlantı noktası için bir gelen kuralı oluşturun.

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

### <a name="create-a-network-card-for-the-vm"></a>VM için bir ağ kartı oluşturma

Ağ kartı VM 'yi bir alt ağa, ağ güvenlik grubuna ve genel IP adresine bağlar.

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

VM yapılandırması oluşturun. Bu yapılandırma, VM 'yi dağıtırken kullanılacak ayarları (örneğin, Kullanıcı kimlik bilgileri, boyut ve VM görüntüsü) içerir.

```powershell
# Define a credential object
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

# Set the operating system disk properties on a VM
$VirtualMachine = Set-AzureRmVMOSDisk `
  -VM $VirtualMachine `
  -CreateOption FromImage | `
  Set-AzureRmVMBootDiagnostics -ResourceGroupName $ResourceGroupName `
  -StorageAccountName $StorageAccountName -Enable |`
  Add-AzureRmVMNetworkInterface -Id $nic.Id

# Configure SSH keys
$sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"

# Add the SSH key to the VM
Add-AzureRmVMSshPublicKey -VM $VirtualMachine `
 -KeyData $sshPublicKey `
 -Path "/home/azureuser/.ssh/authorized_keys"

# Create the VM
New-AzureRmVM `
  -ResourceGroupName $ResourceGroupName `
 -Location $location `
  -VM $VirtualMachine
```

## <a name="vm-quick-create-full-script"></a>VM hızlı oluşturma: tam betik

> [!NOTE]
> Bu adım temel olarak, bir önceki kod birlikte birleştirilir, ancak kimlik doğrulaması için SSH anahtarı yerine bir parola kullanılır.

```powershell
## Create a resource group

<#
A resource group is a logical container where you can deploy and manage Azure Stack resources. From your development kit or the Azure Stack integrated system, run the following code block to create a resource group. Though we've assigned values for all the variables in this article, you can use these values or assign new ones.
#>

# Edit your variables, if required

# Create variables to store the location and resource group names
$location = "local"
$ResourceGroupName = "myResourceGroup"

# Create variables to store the storage account name and the storage account SKU information
$StorageAccountName = "mystorageaccount"
$SkuName = "Standard_LRS"

# Create variables to store the network security group and rules names
$nsgName = "myNetworkSecurityGroup"
$nsgRuleSSHName = "myNetworkSecurityGroupRuleSSH"
$nsgRuleWebName = "myNetworkSecurityGroupRuleWeb"

# Create variable for VM password
$VMPassword = 'Password123!'

# End of variables - no need to edit anything past that point to deploy a single VM

# Create a resource group
New-AzureRmResourceGroup `
  -Name $ResourceGroupName `
  -Location $location

## Create storage resources

# Create a storage account, and then create a storage container for the Ubuntu Server 16.04 LTS image

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

# Create a virtual network, a subnet, and a public IP address, resources that are used provide network connectivity to the VM

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

# Define a credential object
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

# Set the operating system disk properties on a VM
$VirtualMachine = Set-AzureRmVMOSDisk `
  -VM $VirtualMachine `
  -Name $osDiskName `
  -VhdUri $OsDiskUri `
  -CreateOption FromImage | `
  Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create the VM
New-AzureRmVM `
  -ResourceGroupName $ResourceGroupName `
 -Location $location `
  -VM $VirtualMachine
```

## <a name="connect-to-the-vm"></a>VM’ye bağlanma

VM 'yi dağıttıktan sonra, bunun için bir SSH bağlantısı yapılandırın. VM 'nin genel IP adresini almak için [Get-Azurermpublicıpaddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) komutunu kullanın:

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

SSH yüklü bir istemci sisteminden, sanal makineye bağlanmak için aşağıdaki komutu kullanın. Windows üzerinde çalışıyorsanız, bağlantıyı oluşturmak için [Putty](https://www.putty.org/) kullanabilirsiniz.

```
ssh <Public IP Address>
```

İstendiğinde, **azureuser**olarak oturum açın. SSH anahtarlarını oluştururken bir parola kullandıysanız, parolayı sağlamanız gerekir.

## <a name="install-the-nginx-web-server"></a>NGıNX Web sunucusunu yükler

Paket kaynaklarını güncelleştirmek ve en son NGıNX paketini yüklemek için aşağıdaki betiği çalıştırın:

```bash
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-nginx-welcome-page"></a>NGINX karşılama sayfasını görüntüleme

NGıNX Web sunucusu yüklü ve VM 'niz üzerinde bağlantı noktası 80 açık olduğunda, VM 'nin genel IP adresini kullanarak Web sunucusuna erişebilirsiniz. Bir Web tarayıcısı açın ve ```http://<public IP address>```gidin.

![NGıNX Web sunucusu karşılama sayfası](./media/azure-stack-quick-create-vm-linux-cli/nginx.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak, gerek duymayacak kaynakları temizleyebilirsiniz. Kaynak grubunu ve tüm kaynaklarını silmek için aşağıdaki komutu çalıştırın:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, temel bir Linux sunucu VM 'si dağıttınız. Azure Stack VM 'Ler hakkında daha fazla bilgi edinmek için [Azure Stack sanal makinelere yönelik hususlar](azure-stack-vm-considerations.md)bölümüne gidin.
