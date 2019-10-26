---
title: Azure Stack VM disk depolaması oluştur | Microsoft Docs
description: Azure Stack sanal makineler için disk oluşturun.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/24/2019
ms.author: sethm
ms.reviewer: jiahan
ms.lastreviewed: 01/18/2019
ms.openlocfilehash: b42f21a3225194cfe50b5ae7d39d8d1a7cffb6d0
ms.sourcegitcommit: e6a738f674634e1d5dd4eb23b6c44b660ea2fe84
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2019
ms.locfileid: "72891255"
---
# <a name="create-vm-disk-storage-in-azure-stack"></a>Azure Stack VM disk depolaması oluşturma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu makalede, Azure Stack portalını veya PowerShell 'i kullanarak sanal makine (VM) disk depolamanın nasıl oluşturulacağı açıklanmaktadır.

## <a name="overview"></a>Genel Bakış

Sürüm 1808 ' den başlayarak, Azure Stack hem işletim sistemi (OS) hem de veri diski olarak VM 'lerde yönetilen disklerin ve yönetilmeyen disklerin kullanımını destekler. Sürüm 1808 ' den önce yalnızca yönetilmeyen diskler desteklenir.

[Yönetilen diskler](/azure/virtual-machines/windows/managed-disks-overview) , VM diskleriyle ilişkili depolama hesaplarını yöneterek Azure IaaS VM 'leri için disk yönetimini basitleştirir. Sizin yalnızca gereksinim duyduğunuz disk boyutunu belirtmeniz yeterlidir; disk Azure Stack tarafından sizin adınıza oluşturulur ve yönetilir.

Yönetilmeyen diskler, diskleri depolamak için bir depolama hesabı oluşturmanızı gerektirir. Oluşturduğunuz diskler VM diskleri olarak adlandırılır ve depolama hesabındaki kapsayıcılar içinde depolanır.

### <a name="best-practice-guidelines"></a>En iyi yöntem yönergeleri

Performansı artırmak ve genel maliyetleri azaltmak için, her bir VM diskini ayrı bir kapsayıcıya yerleştirmenizi öneririz. Bir kapsayıcı bir işletim sistemi diski veya veri diski içermelidir, ancak aynı anda ikisini de içermemelidir. Ancak, aynı kapsayıcıya her iki disk türünü de yerleştirebilirsiniz.

Bir VM 'ye bir veya daha fazla veri diski eklerseniz, bu diskleri depolamak için ek kapsayıcıları bir konum olarak kullanın. Ek VM 'Ler için işletim sistemi diski de kendi kapsayıcılarında olmalıdır.

VM 'Ler oluşturduğunuzda, her yeni sanal makine için aynı depolama hesabını yeniden kullanabilirsiniz. Yalnızca oluşturduğunuz kapsayıcılar benzersiz olmalıdır.

### <a name="adding-new-disks"></a>Yeni diskler ekleniyor

Aşağıdaki tablo, portalı kullanarak ve PowerShell kullanarak disklerin nasıl ekleneceğini özetler:

| Yöntem | Seçenekler
|-|-|
|Kullanıcı Portalı|-Mevcut bir VM 'ye yeni veri diskleri ekleyin. Yeni diskler Azure Stack tarafından oluşturulur. </br> </br> -Önceden oluşturulmuş bir VM 'ye var olan bir disk (. vhd) dosyası ekleyin. Bunu yapmak için,. vhd 'yi hazırlamanız ve ardından dosyayı Azure Stack dosyasına yüklemeniz gerekir. |
|[PowerShell](#use-powershell-to-add-multiple-unmanaged-disks-to-a-vm) | -İşletim sistemi diski ile yeni bir VM oluşturun ve aynı zamanda bu sanal makineye bir veya daha fazla veri diski ekleyin. |

## <a name="use-the-portal-to-add-disks-to-a-vm"></a>Bir sanal makineye disk eklemek için portalı kullanma

Varsayılan olarak, çoğu Market öğesi için bir VM oluşturmak üzere portalını kullandığınızda yalnızca işletim sistemi diski oluşturulur.

Bir VM oluşturduktan sonra, portalı kullanarak şunları yapabilirsiniz:

* Yeni bir veri diski oluşturun ve bunu sanal makineye ekleyin.
* Var olan bir veri diskini karşıya yükleyin ve sanal makineye ekleyin.

Eklediğiniz her yönetilmeyen disk ayrı bir kapsayıcıya yerleştirilmelidir.

>[!NOTE]  
>Azure tarafından oluşturulan ve yönetilen diskler [yönetilen diskler](/azure/virtual-machines/windows/managed-disks-overview)olarak adlandırılır.

### <a name="use-the-portal-to-create-and-attach-a-new-data-disk"></a>Yeni bir veri diski oluşturmak ve eklemek için portalı kullanma

1. Portalda **tüm hizmetler**' i ve ardından **sanal makineler**' i seçin.
   ![örnek: VM panosu](media/azure-stack-manage-vm-disks/vm-dashboard.png)

2. Daha önce oluşturulmuş bir VM seçin.
   ![örnek: panoda bir VM seçin](media/azure-stack-manage-vm-disks/select-a-vm.png)

3. VM için **diskler**' i seçin ve **veri diski ekleyin**.
   ![örnek: VM 'ye yeni bir disk Iliştirme](media/azure-stack-manage-vm-disks/Attach-disks.png)

4. Veri diski için:
   * **LUN 'u**girin. LUN geçerli bir sayı olmalıdır.
   * **Disk oluştur**' u seçin.
   ![örnek: VM 'ye yeni bir disk Iliştirme](media/azure-stack-manage-vm-disks/add-a-data-disk-create-disk.png)

5. **Yönetilen disk oluştur** dikey penceresinde:
   * Diskin **adını** girin.
   * Var olan bir **kaynak grubunu** seçin veya yeni bir tane oluşturun.
   * **Konumu**seçin. Varsayılan olarak, konum işletim sistemi diskinin tutulduğu aynı kapsayıcıya ayarlanır.
   * **Hesap türünü**seçin.
      ![örnek: VM 'ye yeni bir disk Iliştirme](media/azure-stack-manage-vm-disks/create-manage-disk.png)

      **Premium SSD**  
      Premium diskler (SSD) katı hal sürücüleriyle desteklenir ve tutarlı, düşük gecikme süreli performans sunar. Fiyat ve performans arasında en iyi dengeyi sağlar ve g/ç kullanımı yoğun uygulamalar ve üretim iş yükleri için idealdir.

      **Standart HDD**  
      Standart diskler (HDD) manyetik sürücüler tarafından desteklenir ve verilere Seyrek erişilen uygulamalar için tercih edilir. Bölgesel olarak yedekli diskler, verileri birden çok bölgede çoğaltan bölgesel olarak yedekli depolama (ZRS) tarafından desteklenir ve tek bir bölge çalışmıyor olsa bile verilerinizin kullanılabilir olmasını sağlar.

   * **Kaynak türünü**seçin.

     Başka bir diskin anlık görüntüsünden, depolama hesabındaki bir blobdan bir disk oluşturun veya boş bir disk oluşturun.

      **Anlık görüntü**: kullanılabiliyorsa bir anlık görüntü seçin. Anlık görüntü, VM 'nin aboneliğinde ve konumunda kullanılabilir olmalıdır.

      **Depolama Blobu**:
     * Disk görüntüsünü içeren depolama Blobun URI 'sini ekleyin.  
     * , Depolama hesapları dikey penceresini açmak için **Araştır** ' ı seçin. Yönergeler için bkz. [depolama hesabından veri diski ekleme](#add-a-data-disk-from-a-storage-account).
     * Görüntünün işletim sistemi türünü seçin: **Windows**, **Linux**veya **none (veri diski)** .

   * **Boyut (GiB)** seçeneğini belirleyin.

     Standart disk maliyetleri, diskin boyutuna göre artar. Premium disk maliyetleri ve performansı, diskin boyutuna bağlı olarak artar. Daha fazla bilgi için bkz. [yönetilen diskler fiyatlandırması](https://go.microsoft.com/fwlink/?linkid=843142).

   * **Oluştur**'u seçin. Azure Stack yönetilen diski oluşturup doğrular.

6. Azure Stack diski oluşturup VM 'ye iliştirdikten sonra, yeni disk, **VERI diskleri**altındaki VM disk ayarlarında listelenir.

   ![Örnek: diski görüntüleme](media/azure-stack-manage-vm-disks/view-data-disk.png)

### <a name="add-a-data-disk-from-a-storage-account"></a>Depolama hesabından veri diski ekleme

Azure Stack depolama hesaplarıyla çalışma hakkında daha fazla bilgi için bkz. [Azure Stack depolamaya giriş](azure-stack-storage-overview.md).

1. Kullanılacak **Depolama hesabını** seçin.
2. Veri diskini yerleştirmek istediğiniz **kapsayıcıyı** seçin. İsterseniz, **kapsayıcılar** dikey penceresinde yeni bir kapsayıcı oluşturabilirsiniz. Daha sonra yeni diskin konumunu kendi kapsayıcısına dönüştürebilirsiniz. Her disk için ayrı bir kapsayıcı kullandığınızda, performansı artıran veri diskinin yerleşimini dağıtırsınız.
3. Seçimi kaydetmek için **Seç ' i** seçin.

    ![Örnek: bir kapsayıcı seçin](media/azure-stack-manage-vm-disks/select-container.png)

## <a name="attach-an-existing-data-disk-to-a-vm"></a>Mevcut bir veri diskini bir VM 'ye iliştirme

1. Bir [. vhd dosyasını](/azure/virtual-machines/windows/classic/createupload-vhd) bir VM için veri diski olarak kullanmak üzere hazırlayın. Bu. vhd dosyasını,. vhd dosyasını iliştirmek istediğiniz VM ile kullandığınız bir depolama hesabına yükleyin.

    - İşletim sistemi diskini tutan kapsayıcıdan. vhd dosyasını tutmak için farklı bir kapsayıcı kullanmayı planlayın.  
    - Herhangi bir VHD 'yi Azure 'a yüklemeden önce, [Azure 'a yüklemek için bir WINDOWS VHD veya vhdx hazırlamanızı](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)izlemelisiniz.
    - [Yönetilen disklere](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview)geçişinizi başlatmadan önce [yönetilen disklere geçiş planını](https://docs.microsoft.com/azure/virtual-machines/windows/on-prem-to-azure#plan-for-the-migration-to-managed-disks) gözden geçirin.
    
    ![Örnek: bir VHD dosyasını karşıya yükle](media/azure-stack-manage-vm-disks/upload-vhd.png)



2. . Vhd dosyası karşıya yüklendikten sonra, VHD 'yi bir VM 'ye iliştirmeye hazırsınız demektir. Soldaki menüden **sanal makineler**' i seçin.  
 ![örnek: panoda bir VM seçin](media/azure-stack-manage-vm-disks/vm-dashboard.png)

3. Listeden VM 'yi seçin.

    ![Örnek: panoda bir VM seçin](media/azure-stack-manage-vm-disks/select-a-vm.png)

4. VM 'nin sayfasında, **diskler**' i seçin ve ardından **Varolanı Ekle**' yi seçin.

    ![Örnek: var olan bir diski Iliştirme](media/azure-stack-manage-vm-disks/attach-disks2.png)

5. **Varolan diski Ekle** sayfasında, **VHD dosyası**' nı seçin. **Depolama hesapları** sayfası açılır.

    ![Örnek: bir VHD dosyası seçin](media/azure-stack-manage-vm-disks/select-vhd.png)

6. **Depolama hesapları**altında kullanılacak hesabı seçin ve ardından daha önce karşıya yüklediğiniz. vhd dosyasını tutan bir kapsayıcı seçin. . Vhd dosyasını seçin ve seçimi kaydetmek için **Seç** ' i seçin.

    ![Örnek: bir kapsayıcı seçin](media/azure-stack-manage-vm-disks/select-container2.png)

7. **Mevcut diski Ekle**' nin altında, seçtiğiniz dosya **VHD dosyası**altında listelenir. Diskin **konak önbelleğe alma** ayarını güncelleştirin ve ardından sanal makine için yeni disk yapılandırmasını kaydetmek üzere **Tamam** ' ı seçin.

    ![Örnek: VHD dosyasını Iliştirme](media/azure-stack-manage-vm-disks/attach-vhd.png)

8. Azure Stack diski oluşturup VM 'ye iliştirdikten sonra, yeni disk, **veri diskleri**altındaki sanal makinenin disk ayarlarında listelenir.

    ![Örnek: disk eklemeyi doldurun](media/azure-stack-manage-vm-disks/complete-disk-attach.png)

## <a name="use-powershell-to-add-multiple-unmanaged-disks-to-a-vm"></a>SANAL makineye birden çok yönetilmeyen disk eklemek için PowerShell 'i kullanma

PowerShell kullanarak bir VM sağlayabilir ve yeni bir veri diski ekleyebilir ya da bir veri diski olarak önceden var olan bir. vhd dosyası ekleyebilirsiniz.

**Add-AzureRmVMDataDisk** cmdlet 'ı bir VM 'ye veri diski ekler. Bir VM oluştururken veri diski ekleyebilir veya mevcut bir VM 'ye veri diski ekleyebilirsiniz. Diskleri farklı kapsayıcılara dağıtmak için **Vhduri** parametresini belirtin.

### <a name="add-data-disks-to-a-new-vm"></a>Yeni bir VM 'ye veri diski ekleme

Aşağıdaki örneklerde, her biri farklı bir kapsayıcıya yerleştirilmiş üç veri diskine sahip bir VM oluşturmak için PowerShell komutları kullanılır.

İlk komut bir VM nesnesi oluşturur ve sonra `$VirtualMachine` değişkeninde depolar. Komut, VM 'ye bir ad ve boyut atar:

```powershell
$VirtualMachine = New-AzureRmVMConfig -VMName "VirtualMachine" `
                                      -VMSize "Standard_A2"
```

Sonraki üç komut üç veri diskinin yolunu `$DataDiskVhdUri01`, `$DataDiskVhdUri02`ve `$DataDiskVhdUri03` değişkenlerine atar. Diskleri farklı kapsayıcılara dağıtmak için URL 'de farklı bir yol adı tanımlayın:

```powershell
$DataDiskVhdUri01 = "https://contoso.blob.local.azurestack.external/test1/data1.vhd"
```

```powershell
$DataDiskVhdUri02 = "https://contoso.blob.local.azurestack.external/test2/data2.vhd"
```

```powershell
$DataDiskVhdUri03 = "https://contoso.blob.local.azurestack.external/test3/data3.vhd"
```

Son üç komut `$VirtualMachine`' de depolanan sanal makineye veri diskleri ekler. Her komut, diskin adını, konumunu ve ek özelliklerini belirtir. Her diskin URI 'SI `$DataDiskVhdUri01`, `$DataDiskVhdUri02`ve `$DataDiskVhdUri03`depolanır:

```powershell
$VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name 'DataDisk1' `
                -Caching 'ReadOnly' -DiskSizeInGB 10 -Lun 0 `
                -VhdUri $DataDiskVhdUri01 -CreateOption Empty
```

```powershell
$VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name 'DataDisk2' `
                -Caching 'ReadOnly' -DiskSizeInGB 11 -Lun 1 `
                -VhdUri $DataDiskVhdUri02 -CreateOption Empty
```

```powershell
$VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name 'DataDisk3' `
                -Caching 'ReadOnly' -DiskSizeInGB 12 -Lun 2 `
                -VhdUri $DataDiskVhdUri03 -CreateOption Empty
```

İşletim sistemi diskini ve ağ yapılandırmasını VM 'ye eklemek için aşağıdaki PowerShell komutlarını kullanın ve ardından yeni VM 'yi başlatın:

```powershell
# Set variables
$rgName = "myResourceGroup"
$location = "local"
# Set OS Disk
$osDiskUri = "https://contoso.blob.local.azurestack.external/vhds/osDisk.vhd"
$osDiskName = "osDisk"

$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $osDiskName -VhdUri $osDiskUri `
    -CreateOption FromImage -Windows

# Create a subnet configuration
$subnetName = "mySubNet"
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24

# Create a vnet configuration
$vnetName = "myVnetName"
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
    -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet

# Create a public IP
$ipName = "myIP"
$pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
    -AllocationMethod Dynamic

# Create a network security group configuration
$nsgName = "myNsg"
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule

# Create a NIC configuration
$nicName = "myNicName"
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
-Location $location -SubnetId $vnet.Subnets[0].Id -NetworkSecurityGroupId $nsg.Id -PublicIpAddressId $pip.Id

#Create the new VM
$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName VirtualMachine | `
    Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
    -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $VirtualMachine
```

### <a name="add-data-disks-to-an-existing-vm"></a>Mevcut bir VM 'ye veri diski ekleme

Aşağıdaki örneklerde, mevcut bir VM 'ye üç veri diski eklemek için PowerShell komutları kullanılır. İlk komut, **Get-AzureRmVM** cmdlet 'Ini kullanarak **VIRTUALMACHINE** adlı VM 'yi alır. Komut, VM 'yi `$VirtualMachine` değişkeninde depolar:

```powershell
$VirtualMachine = Get-AzureRmVM -ResourceGroupName "myResourceGroup" `
                                -Name "VirtualMachine"
```

Sonraki üç komut üç veri diskinin yolunu `$DataDiskVhdUri01`, `$DataDiskVhdUri02`ve `$DataDiskVhdUri03` değişkenlerine atar. VHD URI 'Lerinde farklı yol adları, disk yerleştirme için farklı kapsayıcılar belirtir:

```powershell
$DataDiskVhdUri01 = "https://contoso.blob.local.azurestack.external/test1/data1.vhd"
```

```powershell
$DataDiskVhdUri02 = "https://contoso.blob.local.azurestack.external/test2/data2.vhd"
```

```powershell
$DataDiskVhdUri03 = "https://contoso.blob.local.azurestack.external/test3/data3.vhd"
```

Sonraki üç komut, `$VirtualMachine` değişkeninde depolanan sanal makineye veri disklerini ekler. Her komut, diskin adını, konumunu ve ek özelliklerini belirtir. Her diskin URI 'SI `$DataDiskVhdUri01`, `$DataDiskVhdUri02`ve `$DataDiskVhdUri03`depolanır:

```powershell
Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk1" `
                      -VhdUri $DataDiskVhdUri01 -LUN 0 `
                      -Caching ReadOnly -DiskSizeinGB 10 -CreateOption Empty
```

```powershell
Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk2" `
                      -VhdUri $DataDiskVhdUri02 -LUN 1 `
                      -Caching ReadOnly -DiskSizeinGB 11 -CreateOption Empty
```

```powershell
Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk3" `
                      -VhdUri $DataDiskVhdUri03 -LUN 2 `
                      -Caching ReadOnly -DiskSizeinGB 12 -CreateOption Empty
```

Son komut, `-ResourceGroupName``$VirtualMachine` depolanan VM 'nin durumunu güncelleştirir:

```powershell
Update-AzureRmVM -ResourceGroupName "myResourceGroup" -VM $VirtualMachine
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Stack VM 'Ler hakkında daha fazla bilgi edinmek için bkz. [Azure Stack sanal makinelere yönelik hususlar](azure-stack-vm-considerations.md).
