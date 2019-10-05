---
title: Yönetilen diskleri Azure Stack; farklar ve konular | Microsoft Docs
description: Azure Stack yönetilen disklerle ve yönetilen görüntülerle çalışırken farklılıklar ve dikkat edilmesi gerekenler hakkında bilgi edinin.
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
ms.date: 10/04/2019
ms.author: sethm
ms.reviewer: jiahan
ms.lastreviewed: 03/23/2019
ms.openlocfilehash: 97684f2a0ef9960854b192ca15f972bc15ff5b62
ms.sourcegitcommit: f91979c1613ea1aa0e223c818fc208d902b81299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2019
ms.locfileid: "71974056"
---
# <a name="azure-stack-managed-disks-differences-and-considerations"></a>Azure Stack yönetilen diskler: farklar ve konular

Bu makalede, [Azure 'da Azure Stack ve yönetilen disklerde](/azure/virtual-machines/windows/managed-disks-overview) [yönetilen diskler](azure-stack-manage-vm-disks.md) arasındaki farklılıklar özetlenmektedir. Azure Stack ile Azure arasındaki üst düzey farklılıklar hakkında bilgi edinmek için bkz. [temel konular](azure-stack-considerations.md) makalesi.

Yönetilen diskler, VM diskleriyle ilişkili [depolama hesaplarını](../operator/azure-stack-manage-storage-accounts.md) yöneterek IaaS sanal makineleri (VM 'ler) için disk yönetimini basitleştirir.

> [!NOTE]  
> Azure Stack yönetilen diskler 1808 güncelleştirmesiyle başlayarak kullanılabilir. 1811 güncelleştirmesiyle başlayarak, özellik, Azure Stack Portalı kullanılarak sanal makine oluşturulurken varsayılan olarak etkinleştirilir.
  
## <a name="cheat-sheet-managed-disk-differences"></a>Tek sayfa: yönetilen disk farklılıkları

| Özellik | Azure (genel) | Azure Stack |
| --- | --- | --- |
|Bekleyen verilerin şifrelenmesi |Azure Depolama Hizmeti Şifrelemesi (SSE), Azure disk şifrelemesi (ADE).     |BitLocker 128 bit AES şifrelemesi      |
|Image          | Yönetilen özel görüntü |Desteklenen|
|Yedekleme seçenekleri | Azure Backup hizmeti |Henüz desteklenmiyor |
|Olağanüstü durum kurtarma seçenekleri | Azure Site Recovery |Henüz desteklenmiyor|
|Disk türleri     |Premium SSD, Standart SSD ve Standart HDD. |Premium SSD, Standart HDD |
|Premium diskler  |Tam olarak desteklenir. |Sağlanabilir, ancak performans sınırı veya garanti yoktur  |
|Premium diskler IOPS 'si  |Disk boyutuna bağlıdır.  |disk başına 2300 IOPS |
|Premium diskler işleme |Disk boyutuna bağlıdır. |disk başına 145 MB/saniye |
|Disk boyutu  |Azure Premium disk: P4 (32 GiB) ile P80 (32 TiB)<br>Azure Standart SSD diski: E10 (128 GiB) ile E80 (32 TiB)<br>Azure Standart HDD diski: S4 (32 GiB)-S80 (32 TiB) |M4: 32 GiB<br>M6: 64 GiB<br>M10: 128 GiB<br>M15: 256 GiB<br>M20: 512 GiB<br>M30: 1023 GiB |
|Diskler anlık görüntü kopyası|Desteklenen, çalışan bir VM 'ye bağlı Azure tarafından yönetilen disklerin anlık görüntüsünü alın.|Henüz desteklenmiyor |
|Diskler performans analizi |Toplam ölçümler ve disk başına ölçümler desteklenir. |Henüz desteklenmiyor |
|Geçiş      |VM 'yi yeniden oluşturmaya gerek kalmadan mevcut yönetilmeyen Azure Resource Manager sanal makinelerinden geçiş yapmak için araç sağlayın.  |Henüz desteklenmiyor |

> [!NOTE]  
> Azure Stack yönetilen diskler IOPS ve aktarım hızı, sağlanan numara yerine bir büyük sayıdır ve bu, Azure Stack çalışan donanım ve iş yüklerinden etkilenebilir.

## <a name="metrics"></a>Ölçümler

Depolama ölçümleriyle de farklılıklar vardır:

- Azure Stack, depolama ölçümlerinde işlem verileri, iç veya dış ağ bant genişliğini ayırt etmez.
- Depolama ölçümlerinde Azure Stack işlem verileri, bağlı disklere sanal makine erişimi içermez.

## <a name="api-versions"></a>API sürümleri

Azure Stack yönetilen diskler aşağıdaki API sürümlerini destekler:

- 2017-03-30
- 2017-12-01

## <a name="convert-to-managed-disks"></a>Yönetilen disklere Dönüştür

> [!NOTE]  
> Bir yönetilmeyen diski Azure Stack yönetilen diske dönüştürmek için **ConvertTo-AzureRmVMManagedDisk** Azure PowerShell cmdlet 'i kullanılamaz. Azure Stack Şu anda bu cmdlet 'i desteklemiyor.

Aşağıdaki betiği, şu anda sağlanmış bir VM 'yi yönetilmeyenden yönetilen disklere dönüştürmek için kullanabilirsiniz. Yer tutucuları kendi değerlerinizle değiştirin:

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

## <a name="managed-images"></a>Yönetilen görüntüler

Azure Stack *yönetilen görüntüleri*destekler, bu, yalnızca ileri doğru yönetilen disk VM 'leri oluşturabileceğiniz GENELLEŞTIRILMIŞ bir VM 'de (yönetilmeyen ve yönetilen) yönetilen bir görüntü nesnesi oluşturmanızı sağlar. Yönetilen görüntüler aşağıdaki iki senaryoyu etkinleştirir:

- Yönetilmeyen VM 'Leri genelleştirmiş ve ileri doğru yönetilen diskler kullanmak istiyorsunuz.
- Genelleştirilmiş bir yönetilen sanal makinenize sahipsiniz ve birden çok, benzer yönetilen VM oluşturmak istiyorsunuz.

### <a name="step-1-generalize-the-vm"></a>1\. adım: VM 'yi Genelleştirme

Windows için, [Sysprep kullanarak WINDOWS VM 'Yi genelleştirin](/azure/virtual-machines/windows/capture-image-resource#generalize-the-windows-vm-using-sysprep) bölümüne uyun. Linux için [buradaki](/azure/virtual-machines/linux/capture-image#step-1-deprovision-the-vm)1. adımı izleyin.

> [!NOTE]
> VM 'nizi genelleştirdiğinizden emin olun. Düzgün Genelleştirilmiş olmayan bir görüntüden VM oluşturma, bir **Vmprovisioningtimeout** hatasına neden olur.

### <a name="step-2-create-the-managed-image"></a>2\. adım: Yönetilen görüntüyü oluşturma

Yönetilen görüntüyü oluşturmak için Portal, PowerShell veya CLı kullanabilirsiniz. [Yönetilen görüntü oluşturma](/azure/virtual-machines/windows/capture-image-resource)bölümündeki adımları izleyin.

### <a name="step-3-choose-the-use-case"></a>3\. adım: Kullanım durumunu seçin

#### <a name="case-1-migrate-unmanaged-vms-to-managed-disks"></a>Örnek 1: Yönetilmeyen VM 'Leri yönetilen disklere geçirme

Bu adımı gerçekleştirmeden önce VM 'nizi doğru şekilde genelleştirdiğinizden emin olun. Genelleştirdikten sonra artık bu VM 'yi kullanamazsınız. Düzgün Genelleştirilmiş olmayan bir görüntüden VM oluşturma, bir **Vmprovisioningtimeout** hatasına neden olur.

Depolama hesabında genelleştirilmiş bir VHD 'den yönetilen bir görüntü oluşturmak için [depolama hesabı kullanan BIR VM 'den görüntü oluşturma](/azure/virtual-machines/windows/capture-image-resource#create-an-image-from-a-vm-that-uses-a-storage-account) ' daki yönergeleri izleyin. Bu görüntüyü gelecekte yönetilen VM 'Ler oluşturmak için kullanabilirsiniz.

#### <a name="case-2-create-managed-vm-from-managed-image-using-powershell"></a>Örnek 2: Yönetilen görüntüden PowerShell kullanarak yönetilen VM oluşturma

[PowerShell kullanarak yönetilen bir diskten görüntü oluşturma](/azure/virtual-machines/windows/capture-image-resource#create-an-image-from-a-managed-disk-using-powershell)' daki betiği kullanarak var olan bir YÖNETILEN disk VM 'sinden görüntü oluşturduktan sonra, var olan bir görüntü nesnesinden benzer BIR Linux VM oluşturmak için aşağıdaki örnek betiği kullanın.

Azure Stack PowerShell modülü 1.7.0 veya üzeri: [Yönetilen GÖRÜNTÜDEN VM oluşturma](/azure/virtual-machines/windows/create-vm-generalized-managed)' daki yönergeleri izleyin.

Azure Stack PowerShell modülünü 1.6.0 veya öncesini:

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

Ayrıca, yönetilen görüntüden bir VM oluşturmak için portalını kullanabilirsiniz. Daha fazla bilgi için bkz. Azure tarafından yönetilen görüntü makaleleri [Azure 'da Genelleştirilmiş BIR VM 'nin yönetilen görüntüsünü oluşturma](/azure/virtual-machines/windows/capture-image-resource) ve [yönetilen görüntüden VM oluşturma](/azure/virtual-machines/windows/create-vm-generalized-managed).

## <a name="configuration"></a>Yapılandırma

1808 güncelleştirmesini veya sonrasını uyguladıktan sonra, yönetilen diskleri kullanmadan önce aşağıdaki yapılandırma değişikliğini yapmalısınız:

- 1808 güncelleştirmesinden önce bir abonelik oluşturulduysa, aboneliği güncelleştirmek için aşağıdaki adımları izleyin. Aksi halde, VM 'Leri bu abonelikte dağıtmak "Disk Yöneticisi 'nde Iç hata" hata iletisiyle başarısız olabilir.
   1. Kullanıcı portalında Azure Stack **abonelikler** ' e gidin ve aboneliği bulun. Tıklayın **kaynak sağlayıcıları**, ardından **Microsoft.Compute**ve ardından **yeniden kaydettirin**.
   2. Aynı abonelikte **Access Control (IAM)** bölümüne gidin ve **Azure Stack yönetilen diskin** listelendiğini doğrulayın.
- Çok kiracılı bir ortam kullanıyorsanız, [Bu makaledeki](../operator/azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory)adımları izleyerek Konuk dizinlerinizin her birini yeniden yapılandırmak için Bulut işletmenize (kendi kuruluşunuzda veya hizmet sağlayıcısından) sorun. Aksi takdirde, VM 'Leri bu konuk diziniyle ilişkili bir abonelikte dağıtmak "Disk Yöneticisi 'nde Iç hata" hata iletisiyle başarısız olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack sanal makineler hakkında bilgi edinin](azure-stack-compute-overview.md)
