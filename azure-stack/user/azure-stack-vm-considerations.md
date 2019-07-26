---
title: Azure Stack VM özellikleri | Microsoft Docs
description: Azure Stack ' de VM 'lerle çalışırken farklı özellikler ve önemli noktalar hakkında bilgi edinin.
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
ms.date: 07/16/2019
ms.author: mabrigg
ms.reviewer: kivenkat
ms.lastreviewed: 07/16/2019
ms.openlocfilehash: ac0bca9d7073af68e7d2e7c2b7d5ce56bec856ac
ms.sourcegitcommit: 82d09bbae3e5398d2fce7e2f998dfebff018716c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68497872"
---
# <a name="azure-stack-vm-features"></a>Azure Stack VM özellikleri

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack sanal makineler (VM 'Ler) isteğe bağlı, ölçeklenebilir bilgi işlem kaynakları sağlar. VM 'Leri dağıtmadan önce, Azure Stack ve Microsoft Azure ' de bulunan VM özellikleri arasındaki farkları öğrenmelisiniz. Bu makalede bu farklılıklar açıklanmakta ve VM Dağıtımlarını Planlama ile ilgili önemli noktalar tanımlanmaktadır. Azure Stack ile Azure arasındaki üst düzey farklılıklar hakkında bilgi edinmek için bkz. [temel konular](azure-stack-considerations.md) makalesi.

## <a name="vm-differences"></a>VM farkları

| Özellik | Azure (genel) | Azure Stack |
| --- | --- | --- |
| Sanal makine görüntüleri | Azure Marketi 'Nde bir VM oluşturmak için kullanabileceğiniz görüntüler vardır. Azure Marketi 'nde bulunan görüntülerin listesini görüntülemek için bkz. [Azure Marketi](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) sayfası. | Varsayılan olarak, Azure Stack marketi 'nde kullanılabilir görüntü yok. Azure Stack bulut Yöneticisi, kullanıcıların bunları kullanabilmesi için görüntüleri Azure Stack Market 'te yayımlamalıdır veya indirmelidir. |
| Sanal makine boyutları | Azure, VM 'Ler için çok çeşitli boyutları destekler. Kullanılabilir boyutlar ve seçenekler hakkında bilgi edinmek için [Windows VM boyutları](/azure/virtual-machines/virtual-machines-windows-sizes) ve [Linux VM boyutları](/azure/virtual-machines/linux/sizes) konularına bakın. | Azure Stack, Azure 'da kullanılabilen VM boyutlarının bir alt kümesini destekler. Desteklenen boyutların listesini görüntülemek için bu makalenin [VM boyutları](#vm-sizes) bölümüne bakın. |
| Sanal makine kotaları | [Kota sınırları](/azure/azure-subscription-service-limits#service-specific-limits) Microsoft tarafından ayarlanır. | Azure Stack bulut Yöneticisi, kullanıcıları için VM 'leri sunmaya başlamadan önce kotalar atamalıdır. |
| Sanal makine uzantıları |Azure, çok çeşitli VM uzantılarını destekler. Kullanılabilir uzantılar hakkında bilgi edinmek için [VM uzantıları ve özellikleri](/azure/virtual-machines/windows/extensions-features) makalesine bakın.| Azure Stack, Azure 'da kullanılabilen uzantıların bir alt kümesini destekler ve uzantıların her birinde belirli sürümler vardır. Azure Stack bulut Yöneticisi, kullanıcıları için hangi uzantıların kullanılabilir hale getirilmelerini seçebilir. Desteklenen uzantıların listesini görüntülemek için bu makalenin [VM uzantıları](#vm-extensions) bölümüne bakın. |
| Sanal makine ağı | Bir Kiracı VM 'sine atanan genel IP adreslerine Internet üzerinden erişilebilir.<br><br><br>Azure VM 'lerinin sabit bir DNS adı vardır. | Bir Kiracı VM 'sine atanan genel IP adreslerine yalnızca Azure Stack Geliştirme Seti ortam içinden erişilebilir. Bir kullanıcının, Azure Stack oluşturulan bir sanal makineye bağlanmak için [RDP](../asdk/asdk-connect.md#connect-to-azure-stack-using-rdp) veya [VPN](../asdk/asdk-connect.md#connect-to-azure-stack-using-vpn) aracılığıyla Azure Stack geliştirme seti erişimi olması gerekir.<br><br>Belirli bir Azure Stack örneği içinde oluşturulan VM 'Ler, bulut Yöneticisi tarafından yapılandırılan değere göre bir DNS adına sahiptir. |
| Sanal makine depolaması | [Yönetilen diskleri destekler.](/azure/virtual-machines/windows/managed-disks-overview) | Yönetilen diskler, sürüm 1808 ve üzeri sürümleriyle Azure Stack desteklenir. |
| Sanal makine disk performansı | Disk türüne ve boyutuna bağlıdır. | , Disklerin eklendiği VM 'nin VM boyutuna bağlıdır. Daha fazla bilgi için [Azure Stack makalesinde desteklenen VM boyutlarına](azure-stack-vm-sizes.md) bakın.
| API sürümleri | Azure her zaman tüm VM özellikleri için en son API sürümlerine sahiptir. | Azure Stack, bu hizmetlere yönelik belirli Azure hizmetlerini ve belirli API sürümlerini destekler. Desteklenen API sürümlerinin listesini görüntülemek için, bu makalenin [API sürümleri](#api-versions) bölümüne bakın. |
| Azure Instance Metadata Service | Azure Instance Metadata Service, VM 'nizi yönetmek ve ayarlamak için kullanılabilen sanal makine örnekleri hakkında bilgi sağlar.  | Azure Instance Metadata Service Azure Stack desteklenmez. |
| Sanal makine kullanılabilirlik kümeleri|Birden çok hata etki alanı (bölge başına 2 veya 3).<br>Birden çok güncelleştirme etki alanı.|Birden çok hata etki alanı (bölge başına 2 veya 3).<br>Güncelleştirme sırasında iş yüklerini korumak için dinamik geçiş ile tek bir güncelleştirme etki alanı.|
| Sanal makine ölçek kümeleri|Otomatik ölçeklendirme destekleniyor.|Otomatik ölçeklendirme desteklenmiyor.<br><br>Portal, Kaynak Yöneticisi şablonları veya PowerShell 'i kullanarak bir ölçek kümesine daha fazla örnek ekleyin. |
| Bulut tanığı | Azure Stack bulunan depolama hesabı özelliklerindeki uç noktaları seçin. | [Bulut tanığı](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness) , küme çekirdeği üzerinde oy sağlamak için Microsoft Azure kullanan bir yük devretme kümesi çekirdek tanığı türüdür.<br>Genel Azure 'daki Azure Stack ile karşılaştırıldığında uç noktalar şu şekilde görünebilir:<br>Küresel Azure için:<br>`https://mywitness.blob.core.windows.net/`<br>Azure Stack için:<br>`https://mywitness.blob.<region>.<FQDN>/`|
| Sanal makine tanılama | Linux VM tanılaması desteklenir. | Linux VM tanılaması Azure Stack desteklenmez. VM tanılaması etkin bir Linux sanal makinesi dağıttığınızda, dağıtım başarısız olur. Tanılama ayarları aracılığıyla Linux VM temel ölçümleri etkinleştirirseniz, ayrıca dağıtım başarısız olur. |

## <a name="vm-sizes"></a>VM boyutları

Azure Stack kaynakların (sunucu yerel ve hizmet düzeyi) kullanımını önlemek için kaynak sınırlarını uygular. Bu sınırlar, diğer kiracılara göre kaynak tüketimini azaltarak kiracı deneyimini geliştirir.

- VM 'den ağ çıkışı için, bant genişliği üst sınırı vardır. Azure Stack üst sınırı, Azure 'daki büyük harfler ile aynıdır.
- Depolama kaynakları için Azure Stack depolama ıOPS 'yi (saniye başına giriş/çıkış Işlemi), depolama alanı kullanımı için kiracılarca kaynakların temel aşırı kullanımını önlemek
- VM diskleri için Azure Stack üzerindeki disk ıOPS, disk türü yerine VM boyutunun bir işlevidir. Bu, bir Standard_Fs serisi VM için, SSD veya HDD 'yi disk türü için seçip seçmeksizin bağımsız olarak, ikinci bir veri diski için ıOPS sınırının 2300 ıOPS olduğunu belirtir.

Aşağıdaki tabloda, Azure Stack üzerinde desteklenen VM 'Ler, yapılandırmasıyla birlikte listelenmektedir:

| Type           | Size          | Desteklenen boyut aralığı |
| ---------------| ------------- | ------------------------ |
|Genel amaçlı |Temel A        |[A0-A4](azure-stack-vm-sizes.md#basic-a)                   |
|Genel amaçlı |Standart A     |[A0-A7](azure-stack-vm-sizes.md#standard-a)              |
|Genel amaçlı |D Serisi       |[D1-D4](azure-stack-vm-sizes.md#d-series)              |
|Genel amaçlı |Dv2 Serisi     |[D1_v2 - D5_v2](azure-stack-vm-sizes.md#ds-series)        |
|Genel amaçlı |DS serisi      |[DS1-DS4](azure-stack-vm-sizes.md#dv2-series)            |
|Genel amaçlı |DSv2 serisi    |[DS1_v2 - DS5_v2](azure-stack-vm-sizes.md#dsv2-series)      |
|Bellek için iyileştirilmiş|D Serisi       |[D11-D14](azure-stack-vm-sizes.md#mo-d)            |
|Bellek için iyileştirilmiş|DS serisi      |[DS11-DS14](azure-stack-vm-sizes.md#mo-ds)|
|Bellek için iyileştirilmiş|Dv2 Serisi     |[D11_v2 - DS14_v2](azure-stack-vm-sizes.md#mo-dv2)     |
|Bellek için iyileştirilmiş|DSv2-serisi-  |[DS11_v2 - DS14_v2](azure-stack-vm-sizes.md#mo-dsv2)    |

VM boyutları ve ilişkili kaynak miktarları Azure Stack ile Azure arasında tutarlıdır. Bu tutarlılık bellek miktarını, çekirdek sayısını ve oluşturulabilecek veri disklerinin sayısını/boyutunu içerir. Ancak, aynı boyuta sahip sanal makinelerin performansı, belirli bir Azure Stack ortamının temel özelliklerine bağlıdır.

## <a name="vm-extensions"></a>VM uzantıları

Azure Stack küçük bir uzantılar kümesi içerir. Güncelleştirmeler ve ek uzantılar Market dağıtımı aracılığıyla kullanılabilir.

Azure Stack ortamınızda kullanılabilir olan VM uzantılarının listesini almak için aşağıdaki PowerShell betiğini kullanın:

```powershell
Get-AzureRmVmImagePublisher -Location local | `
  Get-AzureRmVMExtensionImageType | `
  Get-AzureRmVMExtensionImage | `
  Select Type, Version | `
  Format-Table -Property * -AutoSize
```

VM dağıtımında bir uzantının sağlanması çok uzun sürerse, VM 'yi serbest bırakma veya silme işlemini durdurmayı denemek yerine sağlama zaman aşımına uğramasına izin verin.

## <a name="api-versions"></a>API sürümleri

Azure Stack VM özellikleri aşağıdaki API sürümlerini destekler:

![VM kaynak türleri](media/azure-stack-vm-considerations/vm-resoource-types.png)

Azure Stack ortamınızda bulunan VM özelliklerinin API sürümlerini almak için aşağıdaki PowerShell betiğini kullanabilirsiniz:

```powershell
Get-AzureRmResourceProvider | `
  Select ProviderNamespace -Expand ResourceTypes | `
  Select * -Expand ApiVersions | `
  Select ProviderNamespace, ResourceTypeName, @{Name="ApiVersion"; Expression={$_}} | `
  where-Object {$_.ProviderNamespace -like "Microsoft.compute"}
```

Desteklenen kaynak türlerinin ve API sürümlerinin listesi, bulut işlecinin Azure Stack ortamınızı daha yeni bir sürüme güncelleştirmiş olması halinde farklılık gösterebilir.

## <a name="windows-activation"></a>Windows etkinleştirmesi

Ürün kullanım hakları ve Microsoft lisans koşullarına uygun olarak Windows ürünlerinin kullanılması gerekir. Azure Stack, Windows Server VM 'lerini etkinleştirmek için [Otomatik sanal makine etkinleştirme](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v%3dws.11)) (AVMA) kullanır.

- Azure Stack Host Windows Server 2016 için AVMA anahtarlarıyla Windows 'ı etkinleştirir. Windows Server 2012 R2 veya üstünü çalıştıran tüm VM 'Ler otomatik olarak etkinleştirilir.
- Windows Server 2012 veya önceki sürümleri çalıştıran VM 'Ler otomatik olarak etkinleştirilmez ve [mak etkinleştirmesi](https://technet.microsoft.com/library/ff793438.aspx)kullanılarak etkinleştirilmelidir. MAK etkinleştirmesini kullanmak için kendi ürün anahtarınızı sağlamanız gerekir.

Microsoft Azure, Windows VM 'Leri etkinleştirmek için KMS etkinleştirme kullanır. Bir VM 'yi Azure Stack Azure 'a taşıyıp etkinleştirme sorunlarıyla karşılaşırsanız bkz. [Azure WINDOWS VM etkinleştirme sorunlarını giderme](https://docs.microsoft.com/azure/virtual-machines/windows/troubleshoot-activation-problems). [Azure VM 'Lerinde Windows etkinleştirme hatalarında sorun giderme](https://blogs.msdn.microsoft.com/mast/2017/06/14/troubleshooting-windows-activation-failures-on-azure-vms/) Azure destek ekibi blog gönderisine ek bilgi bulunabilir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack 'de PowerShell ile Windows VM oluşturma](azure-stack-quick-create-vm-windows-powershell.md)
