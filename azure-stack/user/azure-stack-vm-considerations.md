---
title: Azure Stack VM özellikleri | Microsoft Docs
description: Azure stack'teki vm'lerle çalışırken farklı özellikler ve dikkat edilmesi gerekenler hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/09/2019
ms.author: mabrigg
ms.reviewer: kivenkat
ms.lastreviewed: 12/19/2018
ms.openlocfilehash: 2a9a9a2c402538eee428bad08c9772b288fa1740
ms.sourcegitcommit: be5382f715a9c1c18c660b630d8fcd823f13aae3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66197334"
---
# <a name="azure-stack-vm-features"></a>Azure Stack VM özellikleri

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack sanal makineleri (VM'ler) isteğe bağlı, ölçeklenebilir bilgi işlem kaynakları sağlar. Vm'leri dağıtmadan önce Microsoft Azure ile Azure Stack'te VM özellikleri arasındaki farkı anlamanız gerekir. Bu makalede, bu farklılıkları açıklar ve anahtar VM dağıtımlarını planlama konuları tanımlar. Azure Stack ve Azure arasında üst düzey farklılıklar hakkında bilgi edinmek için bkz. [anahtar konuları](azure-stack-considerations.md) makalesi.

## <a name="vm-differences"></a>VM farkları

| Özellik | Azure (Genel) | Azure Stack |
| --- | --- | --- |
| Sanal makine görüntüleri | Azure marketi, bir VM oluşturmak için kullanabileceğiniz görüntüleri içerir. Bkz: [Azure Marketi](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) Azure Marketi'nde kullanılabilir görüntülerin listesini görüntülemek için sayfa. | Varsayılan olarak, yok herhangi bir görüntü kullanılabilir Azure Stack Market'te. Azure Stack bulut yönetici, yayımlamak veya kullanıcıların kullanabilmeniz için Azure Stack marketini görüntüleri indirin. |
| Sanal makine boyutları | Azure Vm'leri için çok çeşitli boyutlarını destekler. Kullanılabilir boyutlar ve seçenekler hakkında bilgi edinmek için bkz [Windows VM boyutları](/azure/virtual-machines/virtual-machines-windows-sizes) ve [Linux VM boyutları](/azure/virtual-machines/linux/sizes) konuları. | Azure Stack, Azure'da kullanılabilen VM boyutlarının kümesini destekler. Desteklenen boyut listesini görüntülemek için başvurmak [VM boyutları](#vm-sizes) bu makalenin. |
| Sanal makine kotaları | [Kota sınırları](/azure/azure-subscription-service-limits#service-specific-limits) Microsoft tarafından ayarlanır. | Azure Stack bulut yönetici kullanıcılarına VM sundukları kotalar atamanız gerekir. |
| Sanal makine uzantıları |Azure, çok çeşitli VM uzantıları destekler. Kullanılabilir uzantılar hakkında bilgi için bkz [VM uzantıları ve özellikleri](/azure/virtual-machines/windows/extensions-features) makalesi.| Azure Stack, Azure'da kullanılabilen uzantıları kümesini destekler ve her uzantısı olan belirli sürümler. Azure Stack bulut yönetici kullanıcıları için kullanılabilir yapılması hangi uzantıları seçebilirsiniz. Desteklenen uzantılar listesinde görüntülemek üzere başvurmak [VM uzantıları](#vm-extensions) bu makalenin. |
| Sanal makine ağı | Bir kiracı VM'ye atanan genel IP adresleri Internet üzerinden erişilebilir.<br><br><br>Azure sanal makineler, sabit bir DNS adına sahip. | Bir kiracı VM'ye atanan genel IP adresleri yalnızca Azure Stack geliştirme Seti'ni ortamı içinden erişilebilir. Bir kullanıcının bu Azure Stack geliştirme Seti'ni erişiminiz olmalıdır [RDP](../asdk/asdk-connect.md#connect-to-azure-stack-using-rdp) veya [VPN](../asdk/asdk-connect.md#connect-to-azure-stack-using-vpn) Azure Stack'te oluşturulan bir sanal makineye bağlanmak için.<br><br>Belirli bir Azure Stack örneğinde oluşturulan sanal makineler bulut yönetici tarafından yapılandırılan değere göre bir DNS adına sahip |
| Sanal makine depolama | Destekler [yönetilen diskler.](/azure/virtual-machines/windows/managed-disks-overview) | Yönetilen diskler, Azure Stack'te 1808 ve üzeri sürümü ile desteklenir. |
| Sanal makine disk performansı | Disk türünü ve boyutuna bağlıdır. | VM diskleri için eklenen VM boyutuna bağlıdır. Daha fazla bilgi için bkz [Azure Stack'te desteklenen VM boyutları](azure-stack-vm-sizes.md) makalesi.
| API sürümleri | Azure, her zaman en son API sürümleri tüm VM özellikleri vardır. | Azure yığını, bu hizmetler için belirli Azure hizmetlerinin ve belirli API sürümlerini destekler. Desteklenen API sürümlerinin listesi görüntülemek için başvurmak [API sürümlerini](#api-versions) bu makalenin. |
| Azure örnek meta veri hizmeti | Azure örnek meta veri hizmeti yönetmek ve sanal makinenize yapılandırmak için kullanılan sanal makine örneği çalıştırma hakkında bilgi sağlar.  | Azure örnek meta veri hizmeti, Azure Stack üzerinde desteklenmiyor. |
| Sanal makine kullanılabilirlik kümeleri|Birden çok hata etki alanları (2 veya 3 bölge).<br>Birden çok güncelleştirme etki alanları.|Birden çok hata etki alanları (2 veya 3 bölge).<br>Birden çok güncelleştirme etki alanı (en fazla 20).|
| Sanal makine ölçek kümeleri|Otomatik ölçeklendirme desteklenir.|Otomatik ölçeklendirme desteklenmiyor.<br><br>Portal, Resource Manager şablonları veya PowerShell kullanarak bir ölçek kümesine daha fazla örnek ekleyin. |
| Sanal makine tanılama | Linux VM tanılama desteklenir. | Linux VM tanılama Azure Stack'te desteklenmez. VM tanılaması etkin bir Linux sanal makinesi dağıttığınızda, dağıtım başarısız olur. Tanılama ayarları aracılığıyla Linux VM temel ölçümleri etkinleştirirseniz, ayrıca dağıtım başarısız olur.

## <a name="vm-sizes"></a>VM boyutları

Azure Stack uygular (server yerel hem de hizmet düzeyinde.) kaynakların tüketimini önlemek için kaynak sınırları Bu sınırlar, diğer kiracılar tarafından kaynak tüketimine etkisini azaltarak Kiracı deneyimini geliştirmek.

- Sanal makineden ağ çıkışı için yerinde bant genişliği sınırlaması vardır. Azure stack'teki Caps azure'da büyük harfler ile aynıdır.
- Depolama kaynakları için Azure Stack depolama erişimi için kiracılar tarafından kaynakların temel operasyonda ekstra tüketimi önlemek için depolama IOPS (giriş/çıkış işlem / saniye) sınırları uygular.
- VM diskleri için disk IOPS Azure Stack'te bir VM boyutu diskin türünü yerine işlevidir. Bu SSD veya HDD, disk türünü seçmenize bakılmaksızın Standard_Fs serisi VM için tek bir ek veri diski IOPS sınırı 2300 IOPS anlamına gelir.

Azure Stack'te kendi yapılandırması ile birlikte desteklenen VM'ler aşağıdaki tabloda listelenmektedir:

| Tür           | Boyutlandır          | Desteklenen boyutlar aralığı |
| ---------------| ------------- | ------------------------ |
|Genel amaçlı |Temel A        |[A0 - A4](azure-stack-vm-sizes.md#basic-a)                   |
|Genel amaçlı |Standart A     |[A0 - A7](azure-stack-vm-sizes.md#standard-a)              |
|Genel amaçlı |D Serisi       |[D1 - D4](azure-stack-vm-sizes.md#d-series)              |
|Genel amaçlı |Dv2 Serisi     |[D1_v2 - D5_v2](azure-stack-vm-sizes.md#ds-series)        |
|Genel amaçlı |DS serisi      |[DS1 - DS4](azure-stack-vm-sizes.md#dv2-series)            |
|Genel amaçlı |DSv2 serisi    |[DS1_v2 - DS5_v2](azure-stack-vm-sizes.md#dsv2-series)      |
|Bellek için iyileştirilmiş|D Serisi       |[D11 - D14](azure-stack-vm-sizes.md#mo-d)            |
|Bellek için iyileştirilmiş|DS serisi      |[DS11 - DS14](azure-stack-vm-sizes.md#mo-ds)|
|Bellek için iyileştirilmiş|Dv2 Serisi     |[D11_v2 - DS14_v2](azure-stack-vm-sizes.md#mo-dv2)     |
|Bellek için iyileştirilmiş|DSv2 serisi-  |[DS11_v2 - DS14_v2](azure-stack-vm-sizes.md#mo-dsv2)    |

VM boyutları ve bunların ilişkili kaynak miktarları ise, Azure Stack ve Azure arasında tutarlıdır. Bu tutarlılık, bellek, çekirdek sayısı ve sayı/boyutu oluşturulabilmesi için veri disklerinin miktarını içerir. Ancak, aynı boyutta olan sanal makinelerin performans belirli bir Azure Stack ortamının temel özelliklerine bağlıdır.

## <a name="vm-extensions"></a>VM uzantıları

Azure Stack, uzantılar, küçük bir kümesini içerir. Güncelleştirmeleri ve ek uzantı Market dağıtımı kullanılabilir.

Azure Stack ortamınıza kullanılabilir VM uzantılarının listesini almak için aşağıdaki PowerShell betiğini kullanın:

```powershell
Get-AzureRmVmImagePublisher -Location local | `
  Get-AzureRmVMExtensionImageType | `
  Get-AzureRmVMExtensionImage | `
  Select Type, Version | `
  Format-Table -Property * -AutoSize
```

Bir VM dağıtımı üzerinde bir uzantı sağlama çok uzun sürerse, serbest bırakma veya VM silme işlemini durdurulmaya çalışılırken yerine sağlama zaman aşımı sağlar.

## <a name="api-versions"></a>API sürümleri

Azure stack'teki VM özellikleri, aşağıdaki API sürümlerini destekler:

![VM kaynak türleri](media/azure-stack-vm-considerations/vm-resoource-types.png)

Azure Stack ortamınıza kullanılabilen VM özellikleri API sürümlerini almak için aşağıdaki PowerShell betiğini kullanabilirsiniz:

```powershell
Get-AzureRmResourceProvider | `
  Select ProviderNamespace -Expand ResourceTypes | `
  Select * -Expand ApiVersions | `
  Select ProviderNamespace, ResourceTypeName, @{Name="ApiVersion"; Expression={$_}} | `
  where-Object {$_.ProviderNamespace -like "Microsoft.compute"}
```

Desteklenen kaynak türlerini ve API sürümlerini listesi bulut operatörü, Azure Stack ortamınıza yeni bir sürüme güncelleştiriyorsa farklılık gösterebilir.

## <a name="windows-activation"></a>Windows etkinleştirme

Windows ürünlerinin ürün kullanım hakları ve Microsoft Lisans Koşulları'nı uygun olarak kullanılmalıdır. Azure Stack kullanan [otomatik sanal makine etkinleştirmesi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v%3dws.11)) Windows Server sanal makineleri etkinleştirme (AVMA).

- Azure Stack ana bilgisayar Windows AVMA anahtarları ile Windows Server 2016 için etkinleştirir. Windows Server 2012 R2 çalıştıran ya da daha sonra otomatik olarak etkinleştirilmiş tüm VM'ler.
- Windows Server 2012 çalıştıran veya önceki otomatik olarak etkin değildir ve kullanarak etkinleştirilmesi için kullanılması gereken sanal makineleri [MAK etkinleştirmesi](https://technet.microsoft.com/library/ff793438.aspx). MAK etkinleştirmesini kullanmak için kendi ürün anahtarı sağlamanız gerekir.

Microsoft Azure, Windows sanal makineleri etkinleştirme için KMS etkinleştirme kullanır. Azure ve karşılaştığınız etkinleştirme sorunlarını Azure yığını VM'yi taşıma değilse [etkinleştirme sorunlarını giderme Azure Windows VM](https://docs.microsoft.com/azure/virtual-machines/windows/troubleshoot-activation-problems). Ek bilgiler bulunabilir [Azure Vm'leri üzerinde sorun giderme Windows etkinleştirme hatalarını](https://blogs.msdn.microsoft.com/mast/2017/06/14/troubleshooting-windows-activation-failures-on-azure-vms/) Azure destek ekibi Blog Gönderisi.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack'te PowerShell ile bir Windows VM oluşturma](azure-stack-quick-create-vm-windows-powershell.md)
