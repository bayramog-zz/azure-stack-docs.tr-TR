---
title: Azure Stack yönetilen diskler&#58; farklılıklar ve dikkat edilmesi gerekenler | Microsoft Docs
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
ms.date: 06/25/2019
ms.author: sethm
ms.reviewer: jiahan
ms.lastreviewed: 03/23/2019
ms.openlocfilehash: 8ccaa5bf3ae4b53b2c4fe70995a751f87756c039
ms.sourcegitcommit: b36d078e699c7924624b79641dbe9021af9606ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67816235"
---
# <a name="azure-stack-managed-disks-differences-and-considerations"></a>Azure Stack yönetilen diskler: farklılıklar ve dikkat edilmesi gerekenler

Bu makalede arasındaki farklar özetlenmektedir [yönetilen diskleri Azure Stack'te](azure-stack-manage-vm-disks.md) ve [yönetilen diskler azure'da](/azure/virtual-machines/windows/managed-disks-overview). Azure Stack ve Azure arasında üst düzey farklılıklar hakkında bilgi edinmek için bkz. [anahtar konuları](azure-stack-considerations.md) makalesi.

Yönetilen diskler yöneterek Iaas sanal makinelerini (VM) için disk yönetimini basitleştirme [depolama hesapları](../operator/azure-stack-manage-storage-accounts.md) VM diskleriyle ilişkili.

> [!NOTE]  
> Azure Stack'te yönetilen diskler, 1808 güncelleştirmesinden itibaren kullanılabilir. 1811 güncelleştirmesi ile başlayarak, bu Azure Stack portalını kullanarak bir VM oluştururken, varsayılan olarak etkindir.
  
## <a name="cheat-sheet-managed-disk-differences"></a>Kopya kağıdı: yönetilen disk farkları

| Özellik | Azure (Genel) | Azure Stack |
| --- | --- | --- |
|Bekleyen verilerin şifrelenmesi |Azure depolama hizmeti şifrelemesi (SSE), Azure Disk şifrelemesi (ADE)     |BitLocker 128 bit AES şifrelemesi      |
|Image          | Yönetilen özel görüntü |Desteklenen|
|Yedekleme seçenekleri | Azure Backup hizmeti |Henüz desteklenmiyor |
|Olağanüstü durum kurtarma seçenekleri | Azure Site Recovery |Henüz desteklenmiyor|
|Disk türleri     |Premium SSD, standart bir SSD ve HDD standart |Premium SSD, standart HDD |
|Premium diskler  |Tam olarak desteklenir |Performans sınır sağlanabilir veya garanti  |
|Premium disklerde IOPS  |Disk boyutuna bağlıdır.  |2300 disk başına IOPS |
|Premium disk aktarım hızı |Disk boyutuna bağlıdır. |Disk başına 145 MB/saniye |
|Disk boyutu  |Azure Premium Disk: P4 (32 GiB) için P80 (32 tib'a kadar)<br>Azure standart SSD Disk: E10 (128 Gib'a) için E80 (32 tib'a kadar)<br>Azure standart HDD Disk: S4 (32 GiB) için S80 (32 tib'a kadar) |M4: 32 GiB<br>M6: 64 GiB<br>M10: 128 GiB<br>M15: 256 GiB<br>M20: 512 GiB<br>M30: 1024 giB |
|Disk anlık görüntü kopyalama|Azure yönetilen diskler desteklenen çalışan bir VM'ye ekli anlık görüntüsünü alın|Henüz desteklenmiyor |
|Disk performansı analiz |Ölçümleri toplamak ve desteklenen disk ölçüm başına |Henüz desteklenmiyor |
|Geçiş      |Var olan yönetilmeyen Azure Resource Manager sanal Makineyi yeniden oluşturmaya gerek kalmadan geçirmek için araç sağlar  |Henüz desteklenmiyor |

> [!NOTE]  
> Yönetilen diskler IOPS ve aktarım hızı Azure Stack'te uç donanım ve Azure Stack'te çalışan iş yükleri tarafından etkilenebilir bir sağlanan sayı yerine sayıdır.

## <a name="metrics"></a>Ölçümler

Depolama ölçümleri farklılıklar vardır:

- Azure Stack ile depolama ölçümleri işlem verilerinde iç veya dış ağ bant genişliği ayrım yapmaz.
- Azure Stack işlem verileri depolama ölçümleri, bağlama diskleri için sanal makine erişimini içermez.

## <a name="api-versions"></a>API sürümleri

Azure Stack diskleri destek şu API sürümlerinde yönetilen:

- 2017-03-30
- 2017-12-01

## <a name="convert-to-managed-disks"></a>Yönetilen disklere dönüştürme

> [!NOTE]  
> Azure PowerShell cmdlet'ini **ConvertTo-AzureRmVMManagedDisk** yönetilmeyen disk Azure Stack'te yönetilen diske dönüştürme için kullanılamaz. Azure Stack, bu cmdlet şu anda desteklemiyor.

Şu anda sağlanan VM'yi yönetilmeyen disklerden yönetilen disklere dönüştürme için aşağıdaki betiği kullanabilirsiniz. Yer tutucuları kendi değerlerinizle değiştirin:

```powershell
$SubscriptionId = "SubId"

# The name of your resource group where your VM to be converted exists.
$ResourceGroupName ="MyResourceGroup"

# The name of the managed disk to be created.
$DiskName = "mngddisk"

# The size of the disks in GB. It should be greater than the VHD file size.
$DiskSize = "50"

# The URI of the VHD file that will be used to create the managed disk.
# The VHD file can be deleted as soon as the managed disk is created.
$VhdUri = "https://rgmgddisks347.blob.local.azurestack.external/vhds/unmngdvm20181109013817.vhd"

# The storage type for the managed disk: PremiumLRS or StandardLRS.
$AccountType = "StandardLRS"

# The Azure Stack location where the managed disk will be located.
# The location should be the same as the location of the storage account in which VHD file is stored.
# Configure the new managed VM point to the old unmanaged VM configuration (network config, VM name, location).
$Location = "local"
$VirtualMachineName = "unmngdvm"
$VirtualMachineSize = "Standard_D1"
$PIpName = "unmngdvm-ip"
$VirtualNetworkName = "unmngdrg-vnet"
$NicName = "unmngdvm"

# Set the context to the subscription ID in which the managed disk will be created.
Select-AzureRmSubscription -SubscriptionId $SubscriptionId

# Delete old VM, but keep the OS disk.
Remove-AzureRmVm -Name $VirtualMachineName -ResourceGroupName $ResourceGroupName

# Create the managed disk configuration.
$DiskConfig = New-AzureRmDiskConfig -AccountType $AccountType -Location $Location -DiskSizeGB $DiskSize -SourceUri $VhdUri -CreateOption Import

# Create managed disk.
New-AzureRmDisk -DiskName $DiskName -Disk $DiskConfig -ResourceGroupName $resourceGroupName
$Disk = Get-AzureRmDisk -DiskName $DiskName -ResourceGroupName $ResourceGroupName
$VirtualMachine = New-AzureRmVMConfig -VMName $VirtualMachineName -VMSize $VirtualMachineSize

# Use the managed disk resource ID to attach it to the virtual machine.
# Change the OS type to "-Windows" if the OS disk has the Windows OS.
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -ManagedDiskId $Disk.Id -CreateOption Attach -Linux

# Create a public IP for the VM.
$PublicIp = Get-AzureRmPublicIpAddress -Name $PIpName -ResourceGroupName $ResourceGroupName

# Get the virtual network where the virtual machine will be hosted.
$VNet = Get-AzureRmVirtualNetwork -Name $VirtualNetworkName -ResourceGroupName $ResourceGroupName

# Create NIC in the first subnet of the virtual network.
$Nic = Get-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $ResourceGroupName

$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Nic.Id

# Create the virtual machine with managed disk.
New-AzureRmVM -VM $VirtualMachine -ResourceGroupName $ResourceGroupName -Location $Location
```

## <a name="managed-images"></a>Yönetilen görüntüleri

Azure Stack destekler *yönetilen görüntüleri*, hangi etkinleştir, yalnızca yönetilen oluşturabilirsiniz (hem de yönetilmeyen ve yönetilen) bir genelleştirilmiş sanal makine üzerinde bir yönetilen bir görüntü nesnesi oluşturmak için sunulacağından Vm'leri disk. Aşağıdaki iki senaryoda yönetilen görüntüleri etkinleştirin:

- Yönetilmeyen VM genelleştirilmiş ve bundan sonra yönetilen diskleri kullanmak istiyorsanız.
- Genelleştirilmiş bir yönetilen VM sahip ve birden çok oluşturmak istersiniz, benzer Vm'leri yönetilen.

### <a name="step-1-generalize-the-vm"></a>1\. adım: VM'yi Genelleştirme

Windows için izleyin [Windows Sysprep kullanarak VM'yi Genelleştirme](/azure/virtual-machines/windows/capture-image-resource#generalize-the-windows-vm-using-sysprep) bölümü. Linux için 1. adım izleyin [burada](/azure/virtual-machines/linux/capture-image#step-1-deprovision-the-vm).

> [!NOTE]
> Sanal makinenizin genelleştirmek emin olun. Düzgün genelleştirilmemiş bir görüntüden VM oluşturma neden olacak şekilde bir **VMProvisioningTimeout** hata.

### <a name="step-2-create-the-managed-image"></a>2\. adım: Yönetilen bir görüntü oluşturma

Yönetilen bir görüntü oluşturmak için portalı, PowerShell veya CLI kullanın. Bağlantısındaki [yönetilen görüntüsünü oluşturma](/azure/virtual-machines/windows/capture-image-resource).

### <a name="step-3-choose-the-use-case"></a>3\. adım: Kullanım örneği seçin

#### <a name="case-1-migrate-unmanaged-vms-to-managed-disks"></a>1\. durum: Yönetilmeyen Vm'leri yönetilen disklere geçirme

Bu adımı uygulamadan önce sanal Makinenizin doğru genelleştirmek emin olun. Genelleştirme sonra artık bu VM kullanabilirsiniz. Düzgün genelleştirilmemiş bir görüntüden VM oluşturma neden olacak şekilde bir **VMProvisioningTimeout** hata.

Bölümündeki yönergeleri [bir depolama hesabında bir VHD'den Görüntü Oluştur](/azure/virtual-machines/windows/capture-image-resource#create-an-image-from-a-vhd-in-a-storage-account) bir depolama hesabında genelleştirilmiş bir VHD'den yönetilen bir görüntü oluşturmak için. Yönetilen sanal makineler oluşturmak için bu görüntü gelecekte kullanabilirsiniz.

#### <a name="case-2-create-managed-vm-from-managed-image-using-powershell"></a>2\. durum: Powershell kullanarak yönetilen görüntüden yönetilen VM oluşturma

Betik kullanarak VM görüntü mevcut bir yönetilen oluşturduktan sonra disk [PowerShell kullanarak bir yönetilen diskten görüntü oluşturma](/azure/virtual-machines/windows/capture-image-resource#create-an-image-from-a-managed-disk-using-powershell), varolan bir görüntü nesneden benzer bir Linux VM oluşturmak için aşağıdaki örnek komut dosyasını kullanın.

Azure Stack PowerShell modülü 1.7.0 veya daha sonra: Bölümündeki yönergeleri [yönetilen bir görüntüden VM oluşturma](/azure/virtual-machines/windows/create-vm-generalized-managed).

Azure Stack PowerShell modülü 1.6.0 veya önceki sürümleri:

```powershell
# Variables for common values
$ResourceGroupName = "MyResourceGroup"
$Location = "local"
$VirtualMachineName = "MyVM"
$ImageRG = "managedlinuxrg"
$ImageName = "simplelinuxvmm-image-2019122"

# Create credential object
$Cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a resource group
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

# Create a subnet configuration
$SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name "MySubnet" -AddressPrefix "192.168.1.0/24"

# Create a virtual network
$VNet = New-AzureRmVirtualNetwork -ResourceGroupName $ResourceGroupName -Location $Location `
  -Name "MyVNet" -AddressPrefix "192.168.0.0/16" -Subnet $SubnetConfig

# Create a public IP address and specify a DNS name
$PIp = New-AzureRmPublicIpAddress -ResourceGroupName $ResourceGroupName -Location $Location `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4

# Create an inbound network security group rule for port 3389
$NsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name "MyNetworkSecurityGroupRuleRDP"  -Protocol Tcp `
  -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389 -Access Allow

# Create a network security group
$Nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $ResourceGroupName -Location $Location `
  -Name "MyNetworkSecurityGroup" -SecurityRules $NsgRuleRDP

# Create a virtual network card and associate with public IP address and NSG
$Nic = New-AzureRmNetworkInterface -Name "MyNic" -ResourceGroupName $ResourceGroupName -Location $Location `
  -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PIp.Id -NetworkSecurityGroupId $Nsg.Id

$Image = Get-AzureRmImage -ResourceGroupName $ImageRG -ImageName $ImageName

# Create a virtual machine configuration
$VmConfig = New-AzureRmVMConfig -VMName $VirtualMachineName -VMSize "Standard_D1" | `
Set-AzureRmVMOperatingSystem -Linux -ComputerName $VirtualMachineName -Credential $Cred | `
Set-AzureRmVMSourceImage -Id $Image.Id | `
Add-AzureRmVMNetworkInterface -Id $Nic.Id

# Create a virtual machine
New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VmConfig
```

Ayrıca, yönetilen bir görüntüden bir VM oluşturmak için portalı kullanabilirsiniz. Daha fazla bilgi için Azure tarafından yönetilen bir görüntü makalelerine bakın [Azure'da bir genelleştirilmiş VM'nin yönetilen görüntüsünü oluşturma](/azure/virtual-machines/windows/capture-image-resource) ve [yönetilen bir görüntüden VM oluşturma](/azure/virtual-machines/windows/create-vm-generalized-managed).

## <a name="configuration"></a>Yapılandırma

1808 uyguladıktan sonra güncelleştirebilir veya aşağıdaki yapılandırma değişikliğini yönetilen diskler kullanmadan önce daha sonra yapmanız gerekir:

- Bir abonelik 1808 güncelleştirmeden önce oluşturulduysa, abonelik güncelleştirmek için aşağıdaki adımları izleyin. Aksi takdirde, bu abonelikte Vm'leri dağıtma "Disk yöneticisinde iç hata." hata iletisi ile başarısız olabilir
   1. Azure Stack Kullanıcı Portalı'nda Git **abonelikleri** ve aboneliği bulunamıyor. Tıklayın **kaynak sağlayıcıları**, ardından **Microsoft.Compute**ve ardından **yeniden kaydettirin**.
   2. Aynı abonelik altında Git **erişim denetimi (IAM)** , doğrulayın **Azure Stack - yönetilen Disk** listelenir.
- (Kendi kuruluşunuzda ya da hizmet sağlayıcısından olabilir), bulut operatörü için çok kiracılı bir ortam kullanıyorsanız, sorun her yer alan adımları uygulayarak, Konuk dizinlerinizi yeniden [bu makalede](../operator/azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory). Aksi takdirde, bu Konuk dizin ile ilişkili bir abonelik içindeki Vm'leri dağıtma "Disk yöneticisinde iç hata." hata iletisi ile başarısız olabilir

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack sanal makineleri hakkında bilgi edinin](azure-stack-compute-overview.md)
