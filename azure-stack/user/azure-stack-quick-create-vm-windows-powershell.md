---
title: Azure Stack'te PowerShell kullanarak bir Windows Server VM oluşturma | Microsoft Docs
description: Azure Stack'te PowerShell ile Windows Server VM oluşturun.
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
ms.date: 04/09/2019
ms.author: mabrigg
ms.custom: mvc
ms.reviewer: kivenkat
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: 1b0f367540012b86da322329f0536b3c484c39b4
ms.sourcegitcommit: 797dbacd1c6b8479d8c9189a939a13709228d816
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66269554"
---
# <a name="quickstart-create-a-windows-server-vm-by-using-powershell-in-azure-stack"></a>Hızlı Başlangıç: Azure Stack'te PowerShell kullanarak bir Windows Server VM oluşturma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack PowerShell kullanarak bir Windows Server 2016 sanal makine (VM) oluşturabilirsiniz. Bir VM oluşturup bu makaledeki adımları izleyin. Bu makalede ayrıca adımları sunar:

* Bir uzak istemci ile VM'ye bağlanın.
* IIS web sunucusunu yükleme ve varsayılan giriş sayfasını görüntüleyin.
* Kaynaklarınızı temizleme.

> [!NOTE]
>  Bu makalede, Azure Stack geliştirme Seti'ni veya Windows tabanlı bir dış istemci VPN üzerinden bağlandığı sırada açıklanan adımları çalıştırabilirsiniz.

## <a name="prerequisites-for-windows-server-vm"></a>Windows Server sanal makinesi için Önkoşullar

* Azure Stack operatörünüze eklediğini doğrulayın **Windows Server 2016** Azure Stack marketini görüntüye.

* Azure Stack, Azure kaynaklarını oluşturmak ve yönetmek için PowerShell belirli bir sürümünü gerektirir. Azure Stack için yapılandırılmış PowerShell yoksa, adımları [yükleme](../operator/azure-stack-powershell-install.md) PowerShell.

* Azure Stack ayarlanan PowerShell ile Azure Stack ortamınıza bağlamanız gerekecektir. Yönergeler için bkz [bir kullanıcı olarak PowerShell ile Azure stack'e bağlanma](azure-stack-powershell-configure-user.md).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturun

Bir kaynak grubu ile hangi Azure Stack kaynakların dağıtıldığı ve yönetildiği mantıksal bir kapsayıcıdır. Uygulamanızı Geliştirme Seti veya Azure Stack tümleşik sistemi, bir kaynak grubu oluşturmak için aşağıdaki kod bloğu çalıştırın. 

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

Bir depolama hesabı ve Windows Server 2016 görüntüsü depolamak için bir depolama kapsayıcısı oluşturun.

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

Ağ güvenlik grubu, gelen ve giden kuralları kullanarak sanal Makinenin güvenliğini sağlar. Gelen Uzak Masaüstü bağlantılarına izin verecek şekilde 3389 numaralı bağlantı noktası için bir gelen kuralı ve gelen web trafiğine izin vermek için 80 numaralı bağlantı noktası için bir gelen kuralı oluşturalım.

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

Bir VM yapılandırması oluşturun. Bu yapılandırma, sanal Makineyi dağıtırken kullanılan ayarları içerir. Örneğin: kimlik bilgileri, boyutu ve VM görüntüsü.

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

İçinde önceki adımda oluşturduğunuz sanal makine için uzak, kendi genel IP adresi gereklidir. Sanal makinenin genel IP adresini almak için aşağıdaki komutu çalıştırın:

```powershell
Get-AzureRmPublicIpAddress `
  -ResourceGroupName $ResourceGroupName | Select IpAddress
```

VM ile Uzak Masaüstü oturumu oluşturmak için aşağıdaki komutu kullanın. IP adresini, sanal makinenizin *publicIPAddress* değeriyle değiştirin. İstendiğinde kullanıcı adını ve VM oluştururken kullandığınız parolayı girin.

```powershell
mstsc /v <publicIpAddress>
```

## <a name="install-iis-via-powershell"></a>PowerShell kullanarak IIS yükleme

Azure VM'ye açtıktan sonra IIS'i çalıştırmanız ve web trafiğine izin veren yerel güvenlik duvarı kuralı etkinleştirmek için tek satırlık bir PowerShell kullanabilirsiniz. Bir PowerShell istemi açın ve şu komutu çalıştırın:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>IIS karşılama sayfasını görüntüleme

IIS yüklü olan ve 80 numaralı bağlantı noktası sanal makinenizde, herhangi bir tarayıcıda, varsayılan IIS Karşılama sayfasını görüntülemek için kullanabilirsiniz. Kullanım *Publicıpaddress* varsayılan sayfayı ziyaret etmek için önceki bölümde belgelenmiştir.

![Varsayılan IIS sitesi](./media/azure-stack-quick-create-vm-windows-powershell/default-iis-website.png)

## <a name="delete-the-vm"></a>VM’yi silin

Artık gerekli değilse, VM'yi ve ilgili kaynaklarını içeren kaynak grubunu kaldırmak için aşağıdaki komutu kullanın:

```powershell
Remove-AzureRmResourceGroup `
  -Name $ResourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta basit bir Windows VM'ye dağıttınız. Azure Stack sanal makineleri hakkında daha fazla bilgi için devam [Azure Stack VM özellikleri](azure-stack-vm-considerations.md).
