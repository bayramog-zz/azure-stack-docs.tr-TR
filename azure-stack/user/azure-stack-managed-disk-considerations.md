---
title: Farklılıklar ve dikkat edilmesi gerekenler için yönetilen diskleri ve görüntüleri Azure Stack'te yönetilen | Microsoft Docs
description: Yönetilen diskler ve Azure Stack'te yönetilen görüntüleri ile çalışırken farklılıklar ve dikkat edilmesi gerekenler hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2019
ms.author: sethm
ms.reviewer: jiahan
ms.lastreviewed: 03/23/2019
ms.openlocfilehash: d32d7d8d484d6e9819440fc9261bae52d6395ae2
ms.sourcegitcommit: 85c3acd316fd61b4e94c991a9cd68aa97702073b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2019
ms.locfileid: "64985908"
---
# <a name="azure-stack-managed-disks-differences-and-considerations"></a>Azure Stack yönetilen diskler: farklılıklar ve dikkat edilmesi gerekenler

Bu makalede bilinen farklılıkları özetler [Azure Stack'te yönetilen diskler](azure-stack-manage-vm-disks.md) ve [yönetilen diskler için Azure](/azure/virtual-machines/windows/managed-disks-overview). Azure Stack ve Azure arasında üst düzey farklılıklar hakkında bilgi edinmek için bkz. [anahtar konuları](azure-stack-considerations.md) makalesi.

Yönetilen diskler yöneterek Iaas Vm'leri için disk yönetimini basitleştirir [depolama hesapları](../operator/azure-stack-manage-storage-accounts.md) VM diskleriyle ilişkili.

> [!Note]  
> Azure Stack'te yönetilen diskler, 1808 güncelleştirmeyi kullanılabilir. 1811 güncelleştirmesinden itibaren Azure Stack portalını kullanarak sanal makineler oluştururken, varsayılan olarak etkindir.
  
## <a name="cheat-sheet-managed-disk-differences"></a>Kopya kağıdı: yönetilen disk farkları

| Özellik | Azure (Genel) | Azure Stack |
| --- | --- | --- |
|Bekleyen veriler için şifreleme |Azure depolama hizmeti şifrelemesi (SSE), Azure Disk şifrelemesi (ADE)     |BitLocker 128 bit AES şifrelemesi      |
|Image          | Yönetilen bir özel görüntü desteği |Desteklenen|
|Yedekleme seçenekleri |Azure Yedekleme hizmetini destekler |Henüz desteklenmiyor |
|Olağanüstü durum kurtarma seçenekleri |Azure Site kurtarma desteği |Henüz desteklenmiyor|
|Disk türleri     |Premium SSD, standart bir SSD ve HDD standart |Premium SSD, standart HDD |
|Premium diskler  |Tam olarak desteklenir |Performans sınır sağlanabilir veya garanti  |
|Premium disklerde IOPS  |Disk boyutuna bağlıdır.  |2300 disk başına IOPS |
|Premium disk aktarım hızı |Disk boyutuna bağlıdır. |Disk başına 145 MB/saniye |
|Disk boyutu  |Azure Premium Disk: P4 (32 GiB) için P80 (32 tib'a kadar)<br>Azure standart SSD Disk: E10 (128 Gib'a) için E80 (32 tib'a kadar)<br>Azure standart HDD Disk: S4 (32 GiB) için S80 (32 tib'a kadar) |M4: 32 GiB<br>M6: 64 GiB<br>M10: 128 GiB<br>M15: 256 GiB<br>M20: 512 GiB<br>M30: 1024 giB |
|Disk anlık görüntü kopyalama|Anlık görüntü Azure yönetilen diskler desteklenen bir çalışan VM bağlı|Henüz desteklenmiyor |
|Disk performansı analiz |Ölçümleri toplamak ve desteklenen disk ölçüm başına |Henüz desteklenmiyor |
|Geçiş      |Mevcut yönetilen Azure Resource Manager sanal Makineyi yeniden oluşturmaya gerek kalmadan geçirmek için araç sağlar  |Henüz desteklenmiyor |

> [!NOTE]  
> Yönetilen diskler IOPS ve aktarım hızı Azure Stack'te uç donanım ve Azure Stack'te çalışan iş yükleri tarafından etkilenebilir bir sağlanan sayı yerine sayıdır.

## <a name="metrics"></a>Ölçümler

Depolama ölçümleri farklılıklar vardır:

- Azure Stack ile depolama ölçümleri işlem verilerinde iç veya dış ağ bant genişliği ayırmaz.
- Azure Stack işlem verileri depolama ölçümleri, bağlama diskleri için sanal makine erişimini içermez.

## <a name="api-versions"></a>API sürümleri

Azure Stack diskleri destekleyen aşağıdaki API sürümleri yönetilen:

- 2017-03-30
- 2017-12-01

## <a name="convert-to-managed-disks"></a>Yönetilen disklere dönüştürme

Şu anda sağlanan VM'yi yönetilmeyen disklerden yönetilen disklere dönüştürme için aşağıdaki betiği kullanabilirsiniz. Yer tutucuları kendi değerlerinizle değiştirin:

```powershell
$subscriptionId = 'subid'

# The name of your resource group where your VM to be converted exists
$resourceGroupName ='rgmgd'

# The name of the managed disk
$diskName = 'unmgdvm'

# The size of the disks in GB. It should be greater than the VHD file size.
$diskSize = '50'

# The URI of the VHD file that will be used to create the managed disk.
# The VHD file can be deleted as soon as the managed disk is created.
$vhdUri = 'https://rgmgddisks347.blob.local.azurestack.external/vhds/unmgdvm20181109013817.vhd' 

# The storage type for the managed disk; PremiumLRS or StandardLRS.
$accountType = 'StandardLRS'

# The Azure Stack location where the managed disk will be located.
# The location should be the same as the location of the storage account in which VHD file is stored.
# Configure the new managed VM point to the old unmanaged VM's configuration (network config, vm name, location).
$location = 'local'
$virtualMachineName = 'mgdvm'
$virtualMachineSize = 'Standard_D1'
$pipname = 'unmgdvm-ip'
$virtualNetworkName = 'rgmgd-vnet'
$nicname = 'unmgdvm295'

# Set the context to the subscription ID in which the managed disk will be created
Select-AzureRmSubscription -SubscriptionId $SubscriptionId

#Delete old VM, but keep the OS disk
Remove-AzureRmVm -Name $virtualMachineName -ResourceGroupName $resourceGroupName

$diskConfig = New-AzureRmDiskConfig -AccountType $accountType  -Location $location -DiskSizeGB $diskSize -SourceUri $vhdUri -CreateOption Import

# Create managed disk
New-AzureRmDisk -DiskName $diskName -Disk $diskConfig -ResourceGroupName $resourceGroupName
$disk = get-azurermdisk -DiskName $diskName -ResourceGroupName $resourceGroupName
$VirtualMachine = New-AzureRmVMConfig -VMName $virtualMachineName -VMSize $virtualMachineSize

# Use the managed disk resource ID to attach it to the virtual machine.
# Change the OS type to Linux if the OS disk has Linux OS.
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -ManagedDiskId $disk.Id -CreateOption Attach -Linux

# Create a public IP for the VM
$publicIp = Get-AzureRmPublicIpAddress -Name $pipname -ResourceGroupName $resourceGroupName 

# Get the virtual network where the virtual machine will be hosted
$vnet = Get-AzureRmVirtualNetwork -Name $virtualNetworkName -ResourceGroupName $resourceGroupName

# Create NIC in the first subnet of the virtual network
$nic = Get-AzureRmNetworkInterface -Name $nicname -ResourceGroupName $resourceGroupName

$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $nic.Id

# Create the virtual machine with managed disk
New-AzureRmVM -VM $VirtualMachine -ResourceGroupName $resourceGroupName -Location $location
```

## <a name="managed-images"></a>Yönetilen görüntüleri

Azure Stack destekler *yönetilen görüntüleri*, hangi etkinleştir, yalnızca yönetilen oluşturabilirsiniz (hem de yönetilmeyen ve yönetilen) bir genelleştirilmiş sanal makine üzerinde bir yönetilen bir görüntü nesnesi oluşturmak için sunulacağından Vm'leri disk. Aşağıdaki iki senaryoda yönetilen görüntüleri etkinleştirin:

- Yönetilmeyen VM genelleştirilmiş ve bundan sonra yönetilen diskleri kullanmak istiyorsanız.
- Genelleştirilmiş bir yönetilen VM sahip ve birden çok oluşturmak istersiniz, benzer Vm'leri yönetilen.

### <a name="step-1-generalize-the-vm"></a>1. Adım: VM'yi Genelleştirme

Windows için izleyin [Windows Sysprep kullanarak VM'yi Genelleştirme](/azure/virtual-machines/windows/capture-image-resource#generalize-the-windows-vm-using-sysprep) bölümü. Linux için 1. adım izleyin [burada](/azure/virtual-machines/linux/capture-image#step-1-deprovision-the-vm).

> [!NOTE]
> Sanal makinenizin genelleştirmek emin olun. Düzgün genelleştirilmiş bir görüntüden VM oluşturma neden olacak şekilde bir **VMProvisioningTimeout** hata.

### <a name="step-2-create-the-managed-image"></a>2. Adım: Yönetilen bir görüntü oluşturma

Portal, PowerShell veya CLI ile yönetilen bir görüntü oluşturmak için kullanabilirsiniz. Azure makaledeki adımları [burada](/azure/virtual-machines/windows/capture-image-resource).

### <a name="step-3-choose-the-use-case"></a>3. Adım: Kullanım örneği seçin

#### <a name="case-1-migrate-unmanaged-vms-to-managed-disks"></a>1. durum: Yönetilmeyen Vm'leri yönetilen disklere geçirme

Bu adımı gerçekleştirmeden önce sanal Makinenizin doğru genelleştirmek emin olun. Genelleştirme sonra artık bu VM kullanabilirsiniz. Düzgün genelleştirilmiş bir görüntüden VM oluşturma neden olacak şekilde bir **VMProvisioningTimeout** hata.

Yönergeleri izleyerek [burada](/azure/virtual-machines/windows/capture-image-resource#create-an-image-from-a-vhd-in-a-storage-account) bir depolama hesabında genelleştirilmiş bir VHD'den yönetilen bir görüntü oluşturmak için. Yönetilen sanal makineler oluşturmak için ileride bu resmi kullanabilirsiniz.

#### <a name="case-2-create-managed-vm-from-managed-image-using-powershell"></a>2. durum: Powershell kullanarak yönetilen görüntüden yönetilen VM oluşturma

Bir görüntü mevcut bir yönetilen oluşturduktan sonra komut dosyasını kullanarak VM disk [burada](/azure/virtual-machines/windows/capture-image-resource#create-an-image-from-a-managed-disk-using-powershell), aşağıdaki örnek betik, mevcut bir görüntü nesneden benzer bir Linux VM oluşturur:

Azure Stack PowerShell modülü 1.7.0 veya üzeri: yönergeleri [burada](/azure/virtual-machines/windows/create-vm-generalized-managed).

Azure Stack PowerShell modülü 1.6.0 veya önceki sürümleri:

```powershell
# Variables for common values
$resourceGroup = "myResourceGroup"
$location = "redmond"
$vmName = "myVM"
$imagerg = "managedlinuxrg"
$imagename = "simplelinuxvmm-image-2019122"

# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a resource group
New-AzureRmResourceGroup -Name $resourceGroup -Location $location

# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Location $location `
  -Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup -Location $location `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
  -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location $location `
  -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName $resourceGroup -Location $location `
  -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id

$image = get-azurermimage -ResourceGroupName $imagerg -ImageName $imagename

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize Standard_D1 | `
Set-AzureRmVMOperatingSystem -Linux -ComputerName $vmName -Credential $cred | `
Set-AzureRmVMSourceImage -Id $image.Id | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create a virtual machine
New-AzureRmVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig
```

Ayrıca, yönetilen bir görüntüden bir VM oluşturmak için portalı kullanabilirsiniz. Daha fazla bilgi için bkz. Azure yönetilen görüntü makaleleri [Azure'da bir genelleştirilmiş VM'nin yönetilen görüntüsünü oluşturma](/azure/virtual-machines/windows/capture-image-resource) ve [yönetilen bir görüntüden VM oluşturma](/azure/virtual-machines/windows/create-vm-generalized-managed).

## <a name="configuration"></a>Yapılandırma

1808 uyguladıktan sonra güncelleştirme veya daha sonra yönetilen diskler kullanmadan önce aşağıdaki yapılandırma gerçekleştirmeniz gerekir:

- Bir abonelik 1808 güncelleştirmeden önce oluşturulduysa, abonelik güncelleştirmek için aşağıdaki adımları izleyin. Aksi takdirde, bu abonelikte Vm'leri dağıtma "Disk yöneticisinde iç hata." hata iletisi ile başarısız olabilir
   1. Kiracı Portalı'nda Git **abonelikleri** ve aboneliği bulunamıyor. Tıklayın **kaynak sağlayıcıları**, ardından **Microsoft.Compute**ve ardından **yeniden kaydettirin**.
   2. Aynı abonelik altında Git **erişim denetimi (IAM)**, doğrulayın **Azure Stack - yönetilen Disk** listelenir.
- (Kendi kuruluşunuzda ya da hizmet sağlayıcısından olabilir), bulut operatörü için çok kiracılı bir ortam kullanıyorsanız, sorun her yer alan adımları uygulayarak, Konuk dizinlerinizi yeniden [bu makalede](../operator/azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory). Aksi takdirde, bu Konuk dizin ile ilişkili bir abonelik içindeki Vm'leri dağıtma "Disk yöneticisinde iç hata." hata iletisi ile başarısız olabilir

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack sanal makineleri hakkında bilgi edinin](azure-stack-compute-overview.md)
