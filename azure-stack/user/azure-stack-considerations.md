---
title: Hizmetler kullanılırken ve uygulamalar derlerken Azure Stack ile Azure arasındaki farklar | Microsoft Docs
description: Hizmetleri kullanırken ve uygulamaları derlerken Azure ile Azure Stack arasındaki farkları anlayın.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: c81f551d-c13e-47d9-a5c2-eb1ea4806228
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 07/17/2019
ms.author: sethm
ms.lastreviewed: 12/27/2018
ms.openlocfilehash: a7a61e8eef33ee6a6efb87001504fe5234e3cf16
ms.sourcegitcommit: 2063332b4d7f98ee944dd1f443847eea70eb5614
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68303149"
---
# <a name="differences-between-azure-stack-and-azure-when-using-services-and-building-apps"></a>Hizmetler kullanılırken Azure Stack ile Azure arasındaki farklar ve uygulama oluşturma

Azure Stack için Hizmetleri kullanmadan veya uygulamalar oluşturmadan önce, Azure Stack ve Azure arasındaki farkları anlamak önemlidir. Bu makalede karma bulut geliştirme ortamınız olarak Azure Stack kullanırken farklı özellikler ve önemli konular tanımlanmaktadır.

## <a name="overview"></a>Genel Bakış

Azure Stack, şirketinizin veya hizmet sağlayıcısı veri merkezinizden Azure hizmetlerini kullanmanızı sağlayan bir karma bulut platformudur. Azure Stack bir uygulama oluşturabilir ve ardından bunu Azure Stack, Azure veya Azure hibrit buluta dağıtabilirsiniz.

Azure Stack operatörünüz kullanabileceğiniz Hizmetleri ve nasıl destek alınacağını bilmenizi sağlar. Bu hizmetleri kendi özelleştirilmiş planları ve teklifleri aracılığıyla sunarız.

Azure teknik belgeleri içeriği, uygulamaların Azure Stack için değil, Azure hizmeti için geliştirmekte olduğunu varsayar. Azure Stack için uygulama oluşturup dağıtırken, bazı önemli farklılıkları anlamanız gerekir, örneğin:

* Azure Stack, Azure 'da kullanılabilen hizmetlerin ve özelliklerin bir alt kümesini sunar.
* Şirketiniz veya hizmet sağlayıcınız, hangi hizmetleri sunmak istediğinizi seçebilir. Kullanılabilir seçenekler özelleştirilmiş hizmetler veya uygulamalar içerebilir. Kendi özelleştirilmiş belgelerini sunabilir.
* Doğru Azure Stack özgü uç noktaları kullanmanız gerekir (örneğin, Portal adresi URL 'Leri ve Azure Resource Manager uç noktası).
* Azure Stack tarafından desteklenen PowerShell ve API sürümlerini kullanmanız gerekir. Desteklenen sürümlerin kullanılması, uygulamalarınızın hem Azure Stack hem de Azure 'da çalışmasını sağlar.

## <a name="cheat-sheet-high-level-differences"></a>Tek sayfa: Üst düzey farklılıklar

Aşağıdaki tabloda Azure Stack ve Azure arasındaki üst düzey farklılıklar açıklanmaktadır. Azure Stack geliştirirken veya Azure Stack hizmetlerini kullandığınızda bu farklılıkları göz önünde bulundurun:

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

| Alan | Azure (genel) | Azure Stack |
| -------- | ------------- | ----------|
| Kim çalışıyor? | Microsoft | Kuruluşunuz veya hizmet sağlayıcınız.|
| Destek için kimler sizinle iletişim kurabilirim? | Microsoft | Tümleşik bir sistem için destek için Azure Stack işleçle (kuruluşunuz veya hizmet sağlayıcınız) başvurun.<br><br>Azure Stack Geliştirme Seti (ASDK) desteği için [Microsoft forumlarını](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStack)ziyaret edin. Geliştirme seti bir değerlendirme ortamı olduğundan, Microsoft Müşteri Destek Hizmetleri (CSS) ile sunulan resmi bir destek yoktur.
| Kullanılabilir hizmetler | [Azure ürünlerinin](https://azure.microsoft.com/services/?b=17.04b)listesini görüntüleyin. Kullanılabilir hizmetler Azure bölgesine göre farklılık gösterir. | Azure Stack, Azure hizmetlerinin bir alt kümesini destekler. Gerçek hizmetler, kuruluşunuzun veya hizmet sağlayıcınızın ne kadar önerdiklerine göre farklılık gösterir.
| Azure Resource Manager uç noktası * | https://management.azure.com | Azure Stack tümleşik bir sistem için Azure Stack operatörünüzün sağladığı uç noktayı kullanın.<br><br>Geliştirme seti için şunu kullanın: https://management.local.azurestack.external.
| Portal URL 'SI * | [https://portal.azure.com](https://portal.azure.com) | Azure Stack tümleşik bir sistem için Azure Stack operatörünüzün sağladığı URL 'YI kullanın.<br><br>Geliştirme seti için şunu kullanın: https://portal.local.azurestack.external.
| Bölge | Hangi bölgeyi dağıtmak istediğinizi seçebilirsiniz. | Azure Stack tümleşik bir sistem için sisteminizde bulunan bölgeyi kullanın.<br><br>Geliştirme seti için bölge her zaman **Yerel**olur.
| Kaynak grupları | Bir kaynak grubu, bölgelere yayılabilir. | Hem tümleşik sistemler hem de geliştirme seti için yalnızca bir bölge vardır.
|Desteklenen ad alanları, kaynak türleri ve API sürümleri | En son (veya henüz kullanım dışı olmayan önceki sürümler). | Azure Stack belirli sürümleri destekler. Bu makalenin [sürüm gereksinimleri](#version-requirements) bölümüne bakın.
| | |

\* Azure Stack operatörünüz varsa, daha fazla bilgi için bkz. [Yönetici portalını](../operator/azure-stack-manage-portals.md) ve [Yönetim temellerini](../operator/azure-stack-manage-basics.md) kullanma.

## <a name="helpful-tools-and-best-practices"></a>Yardımcı araçlar ve en iyi uygulamalar

Microsoft, Azure Stack için geliştirme yapmanıza yardımcı olacak araçlar ve kılavuzluk sağlar.

| Öneri | Referanslar |
| -------- | ------------- |
| Geliştirici iş istasyonunuza doğru araçları yükler. | - [PowerShell 'i yükler](../operator/azure-stack-powershell-install.md)<br>- [İndirme araçları](../operator/azure-stack-powershell-download.md)<br>- [PowerShell 'i yapılandırma](azure-stack-powershell-configure-user.md)<br>- [Visual Studio 'Yu yükler](azure-stack-install-visual-studio.md) 
| Aşağıdaki öğeler hakkındaki bilgileri gözden geçirin:<br>-Azure Resource Manager şablonu konuları<br>-Hızlı Başlangıç şablonlarını bulma<br>-Azure Stack için geliştirme yapmak üzere Azure kullanmanıza yardımcı olması için bir ilke modülü kullanın | [Azure Stack için geliştirme](azure-stack-developer.md) | 
| Şablonlar için en iyi uygulamaları gözden geçirin ve izleyin. | [Kaynak Yöneticisi hızlı başlangıç şablonları](https://github.com/Azure/azure-quickstart-templates/blob/master/1-CONTRIBUTION-GUIDE/best-practices.md)
| | |

## <a name="version-requirements"></a>Sürüm gereksinimleri

Azure Stack, Azure PowerShell ve Azure hizmet API 'Lerinin belirli sürümlerini destekler. Uygulamanızın hem Azure Stack hem de Azure 'a dağıtmasını sağlamak için desteklenen sürümleri kullanın.

Azure PowerShell doğru bir sürümünü kullandığınızdan emin olmak için [API sürüm profilleri](azure-stack-version-profiles.md)' ni kullanın. Kullanabileceğiniz en son API sürümü profilini öğrenmek için, kullanmakta olduğunuz Azure Stack yapısını bulun. Bu bilgileri Azure Stack yöneticinizden edinebilirsiniz.

> [!NOTE]
> Azure Stack Geliştirme Seti kullanıyorsanız ve yönetici erişiminiz varsa, Azure Stack derlemeyi öğrenmek için [geçerli sürümü belirleme](../operator/azure-stack-updates.md#determine-the-current-version) bölümüne bakın.

Diğer API 'Ler için, Azure Stack aboneliğinizde desteklenen ad alanlarını, kaynak türlerini ve API sürümlerini çıkarmak için aşağıdaki PowerShell komutunu çalıştırın (bir özellik düzeyinde farklılık gösterebilir). Bu komutun çalışması için, zaten bir Azure Stack ortamı için PowerShell 'i [yüklemiş](../operator/azure-stack-powershell-install.md) ve [yapılandırmış](azure-stack-powershell-configure-user.md) olmanız gerekir. Ayrıca, bir Azure Stack teklifine aboneliğiniz olması gerekir.

```powershell
Get-AzureRmResourceProvider | Select ProviderNamespace -Expand ResourceTypes | Select * -Expand ApiVersions | `
Select ProviderNamespace, ResourceTypeName, @{Name="ApiVersion"; Expression={$_}} 
```

Örnek çıkış (kesilmiş): ![Get-AzureRmResourceProvider komutunun örnek çıktısı](media/azure-stack-considerations/image1.png)

## <a name="next-steps"></a>Sonraki adımlar

Hizmet düzeyindeki farklılıklar hakkında daha ayrıntılı bilgi için bkz.:

* [Azure Stack VM 'lerle ilgili konular](azure-stack-vm-considerations.md)
* [Azure Stack depolama ile ilgili konular](azure-stack-acs-differences.md)
* [Azure Stack ağ iletişiminde dikkat edilmesi gerekenler](azure-stack-network-differences.md)
