---
title: Azure Stack 'de PowerShell kullanarak Windows Server VM oluşturma | Microsoft Docs
description: Azure Stack 'de PowerShell ile bir Windows Server VM 'si oluşturun.
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
ms.reviewer: kivenkat
ms.lastreviewed: 11/11/2019
ms.openlocfilehash: a46630e7f1e55d248354639a35aa7a389effc353
ms.sourcegitcommit: 102ef41963b5d2d91336c84f2d6af3fdf2ce11c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73955648"
---
# <a name="quickstart-create-a-windows-server-vm-by-using-powershell-in-azure-stack"></a>Hızlı başlangıç: Azure Stack 'de PowerShell kullanarak Windows Server VM oluşturma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack PowerShell kullanarak bir Windows Server 2016 sanal makinesi (VM) oluşturabilirsiniz. Bir VM oluşturmak ve kullanmak için bu makaledeki adımları izleyin. Bu makale aşağıdaki adımları da sağlar:

* Uzak bir istemciyle VM 'ye bağlanın.
* IIS Web sunucusunu yükleyip varsayılan giriş sayfasını görüntüleyin.
* Kaynaklarınızı temizleyin.

> [!NOTE]
>  Bu makalede açıklanan adımları Azure Stack Geliştirme Seti veya bir VPN üzerinden bağlıysanız Windows tabanlı bir dış istemciden çalıştırabilirsiniz.

## <a name="prerequisites-for-windows-server-vm"></a>Windows Server VM önkoşulları

* Azure Stack operatörünüzün **Windows Server 2016** görüntüsünü Azure Stack Market 'e eklediğinizden emin olun.

* Azure Stack, kaynakları oluşturmak ve yönetmek için Azure PowerShell belirli bir sürümünü gerektirir. Azure Stack için PowerShell 'i yapılandırmadıysanız, PowerShell 'i [yüklemek](../operator/azure-stack-powershell-install.md) için adımları izleyin.

* Azure Stack PowerShell kurulumu ile Azure Stack ortamınıza bağlanmanız gerekir. Yönergeler için bkz. [PowerShell ile Kullanıcı olarak Azure Stack bağlama](azure-stack-powershell-configure-user.md).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure Stack kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Geliştirme setinizden veya tümleşik Azure Stack sisteminizde, bir kaynak grubu oluşturmak için aşağıdaki kod bloğunu çalıştırın. 

> [!NOTE]
> Kod örneklerindeki tüm değişkenler için değerler atanır. Ancak, isterseniz yeni değerler atayabilirsiniz.

```powershell
# Create variables to store the location and resource group names.
$location = "local"
$ResourceGroupName = "myResourceGroup"

New-AzureRmResourceGroup `
  -Name $ResourceGroupName `
  -Location $location
```

## <a name="create-storage-resources"></a>Depolama kaynakları oluşturma

Windows Server 2016 görüntüsünü depolamak için bir depolama hesabı ve depolama kapsayıcısı oluşturun.

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

Ağ güvenlik grubu, gelen ve giden kurallarını kullanarak VM 'nin güvenliğini sağlar. Gelen Uzak Masaüstü bağlantılarına izin vermek için 3389 numaralı bağlantı noktası için bir gelen kuralı ve gelen Web trafiğine izin vermek için 80 numaralı bağlantı noktası için bir gelen kuralı oluşturalım.

```powershell
# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNetworkSecurityGroupRuleRDP `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1000 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 3389 `
  -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNetworkSecurityGroupRuleWWW `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1001 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRuleRDP,$nsgRuleWeb
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

VM yapılandırması oluşturun. Bu yapılandırma, VM dağıtımında kullanılan ayarları içerir. Örneğin: kimlik bilgileri, boyut ve VM görüntüsü.

```powershell
# Define a credential object to store the username and password for the VM
$UserName='demouser'
$Password='Password@123'| ConvertTo-SecureString -Force -AsPlainText
$Credential=New-Object PSCredential($UserName,$Password)

# Create the VM configuration object
$VmName = "VirtualMachinelatest"
$VmSize = "Standard_A1"
$VirtualMachine = New-AzureRmVMConfig `
  -VMName $VmName `
  -VMSize $VmSize

$VirtualMachine = Set-AzureRmVMOperatingSystem `
  -VM $VirtualMachine `
  -Windows `
  -ComputerName "MainComputer" `
  -Credential $Credential

$VirtualMachine = Set-AzureRmVMSourceImage `
  -VM $VirtualMachine `
  -PublisherName "MicrosoftWindowsServer" `
  -Offer "WindowsServer" `
  -Skus "2016-Datacenter" `
  -Version "latest"

# Sets the operating system disk properties on a VM.
$VirtualMachine = Set-AzureRmVMOSDisk `
  -VM $VirtualMachine `
  -CreateOption FromImage | `
  Set-AzureRmVMBootDiagnostics -ResourceGroupName $ResourceGroupName `
  -StorageAccountName $StorageAccountName -Enable |`
  Add-AzureRmVMNetworkInterface -Id $nic.Id


# Create the VM.
New-AzureRmVM `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -VM $VirtualMachine
```

## <a name="connect-to-the-vm"></a>VM’ye bağlanma

Önceki adımda oluşturduğunuz sanal makineye uzak için genel IP adresi gerekir. VM 'nin genel IP adresini almak için aşağıdaki komutu çalıştırın:

```powershell
Get-AzureRmPublicIpAddress `
  -ResourceGroupName $ResourceGroupName | Select IpAddress
```

VM ile bir Uzak Masaüstü oturumu oluşturmak için aşağıdaki komutu kullanın. IP adresini, sanal makinenizin *publicIPAddress* değeriyle değiştirin. İstendiğinde, VM oluştururken kullanılan Kullanıcı adını ve parolayı girin.

```powershell
mstsc /v <publicIpAddress>
```

## <a name="install-iis-via-powershell"></a>PowerShell kullanarak IIS yükleme

Azure VM 'de oturum açmış olduğunuza göre, tek bir PowerShell satırı kullanarak IIS yükleyebilir ve Web trafiğine izin vermek için yerel güvenlik duvarı kuralını etkinleştirebilirsiniz. Bir PowerShell istemi açın ve şu komutu çalıştırın:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>IIS karşılama sayfasını görüntüleme

IIS yüklü ve VM 'niz üzerinde bağlantı noktası 80 ile birlikte, varsayılan IIS karşılama sayfasını görüntülemek için herhangi bir tarayıcıyı kullanabilirsiniz. Önceki bölümde belgelenen *Publicıpaddress* 'i kullanarak varsayılan sayfayı ziyaret edin.

![Varsayılan IIS sitesi](./media/azure-stack-quick-create-vm-windows-powershell/default-iis-website.png)

## <a name="delete-the-vm"></a>VM’yi silin

Artık gerekli değilse, sanal makineyi ve ilgili kaynaklarını içeren kaynak grubunu kaldırmak için aşağıdaki komutu kullanın:

```powershell
Remove-AzureRmResourceGroup `
  -Name $ResourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta basit bir Windows VM 'si dağıttık. Azure Stack VM 'Ler hakkında daha fazla bilgi edinmek için [Azure Stack VM özelliklerini](azure-stack-vm-considerations.md)kullanmaya devam edin.
