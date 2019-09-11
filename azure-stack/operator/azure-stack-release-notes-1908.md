---
title: Azure Stack 1908 sürüm notları | Microsoft Docs
description: Yenilikler, bilinen sorunlar ve güncelleştirmenin indirileceği gibi Azure Stack tümleşik sistemler için 1908 güncelleştirmesi hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2019
ms.author: sethm
ms.reviewer: prchint
ms.lastreviewed: 08/30/2019
monikerRange: azs-1908
ms.openlocfilehash: 935a422aee68bb107e5bfe8b79a21d22ce1fffc8
ms.sourcegitcommit: 305536bfd49319455ca3ca270fe3644b1796bad1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876626"
---
# <a name="azure-stack-1908-update"></a>Azure Stack 1908 güncelleştirmesi

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Bu makalede 1908 güncelleştirme paketinin içeriği açıklanır. Güncelleştirme yenilikleri ve bu Azure Stack sürümü için düzeltmeler içerir.

> [!IMPORTANT]  
> Yalnızca Azure Stack tümleşik sistemleri için bu güncelleştirme paketidir. Bu güncelleştirme paketi için Azure Stack geliştirme Seti'ni geçerli değildir.

## <a name="previous-release-notes"></a>Önceki sürüm notları

1908 sürümünden itibaren, sürüm notlarının önceki sürümleri, sol taraftaki içindekiler tablosunda artık görünmez. Sürüm notlarının eski sürümlerine erişmek için başka bir makale (örneğin, [Azure Stack genel bakış](azure-stack-overview.md)) seçin, ardından sol taraftaki içindekiler tablosunun en üstündeki sürüm seçicisindeki 1905, 1906, 1907 veya 1908 ' i seçin. Sürüm notlarının önceki sürümleri için [arşivlenmiş sürüm notları](#archived-release-notes) bölümüne bakın.

## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack 1908 güncelleştirme yapı numarası **1.1908.0.20**.

### <a name="update-type"></a>Güncelleştirme türü

1908 için, Azure Stack çalıştığı temel işletim sistemi Windows Server 2019 ' e güncelleştirilmiştir. Bu, temel temel geliştirmelerin yanı sıra, yakın gelecekte Azure Stack için ek özellikler getirme özelliğini sağlar.

Azure Stack 1908 güncelleştirme yapı türü **dolu**. Sonuç olarak, 1908 güncelleştirmesinin 1906 ve 1907 gibi hızlı güncelleştirmelerden daha uzun bir çalışma zamanı vardır. Tam güncelleştirmeler için tam çalışma zamanları genellikle Azure Stack örneğinizin içerdiği düğüm sayısına, kiracı iş yükleri tarafından sisteminizde kullanılan kapasiteye, sisteminizin ağ bağlantısına (internet 'e bağlıysa) ve sistem donanımınıza bağlıdır yapılandırmada. 1908 güncelleştirmesinde, iç sınamamız içinde aşağıdaki beklenen çalışma zamanları vardı: 4 düğüm-42 saat, 8 düğüm-50 saat, 12 düğüm-60 saat, 16 düğüm-70 saat. Bu beklenen değerlerin daha uzun süre içinde güncelleştirilmesi, güncelleştirme başarısız olmadığı için Azure Stack işleçlere göre işlem gerektirmemektedir.

Güncelleştirme oluşturma türleri hakkında daha fazla bilgi için bkz. [Azure Stack güncelleştirmeleri yönetme](azure-stack-updates.md).

- Tam güncelleştirme çalışma zamanları, genellikle kiracı iş yükleri, sistem ağ bağlantınız (internet 'e bağlıysa) ve sistem donanım yapılandırmanız tarafından sisteminizde kullanılan kapasiteye bağlıdır.
- Beklenenden uzun süre içinde olan çalışma zamanları seyrek değildir ve güncelleştirme başarısız olmazsa Azure Stack işleçlere göre eylem gerektirmez.
- Bu çalışma zamanı yaklaşık 1908 güncelleştirmesine özeldir ve diğer Azure Stack güncelleştirmeleriyle karşılaştırılmamalıdır.

<!-- ## What's in this update -->

<!-- The current theme (if any) of this release. -->

### <a name="whats-new"></a>Yenilikler

<!-- What's new, also net new experiences and features. -->

- 1908 için, Azure Stack çalıştığı temeldeki işletim sisteminin Windows Server 2019 ' e güncelleştirildiğini unutmayın. Bu, temel temel geliştirmelerin yanı sıra, yakın gelecekte Azure Stack için ek özellikler getirme özelliğini sağlar.
- Azure Stack altyapısının tüm bileşenleri artık FIPS 140-2 modunda çalışır.
- Azure Stack işleçler artık Portal Kullanıcı verilerini kaldırabilir. Daha fazla bilgi için bkz. [Azure Stack Portal Kullanıcı verilerini temizleme](azure-stack-portal-clear.md).

### <a name="improvements"></a>Geliştirmeleri

<!-- Changes and product improvements with tangible customer-facing value. -->
- Fiziksel düğümlerin donanım Güvenilir Platform Modülü (TPM) ile gizli dizileri kalıcı hale getirmek için Azure Stack Rest şifrelemesinde veri geliştirmeleri.

### <a name="changes"></a>Değişiklikler

- Donanım sağlayıcıları, Azure Stack sürüm 1908 ile aynı anda OEM Uzantı paketi 2,1 veya üstünü serbest bırakır. OEM uzantısı paketi 2,1 veya üzeri, Azure Stack sürüm 1908 için bir önkoşuldur. OEM Uzantı paketi 2,1 veya sonraki bir sürümünü indirme hakkında daha fazla bilgi için sisteminizin donanım sağlayıcısına başvurun ve [OEM güncelleştirmeleri](azure-stack-update-oem.md#oem-contact-information) makalesine bakın.  

### <a name="fixes"></a>Düzeltmeleri

- Gelecekteki Azure Stack OEM güncelleştirmeleriyle uyumlulukla ilgili bir sorun ve müşteri Kullanıcı görüntülerini kullanarak VM dağıtımıyla ilgili bir sorun düzeltildi. Bu sorun 1907 sürümünde bulunur ve düzeltme [KB4517473](https://support.microsoft.com/en-us/help/4517473/azure-stack-hotfix-1-1907-12-44) düzeltildi  
- Yapı halka durumu için test-AzureStack 'te OEM üretici yazılımı güncelleştirmesiyle ilgili bir sorun düzeltildi. Bu sorun 1907 sürümünde bulunur ve düzeltme [KB4515310](https://support.microsoft.com/en-us/help/4515310/azure-stack-hotfix-1-1907-7-35) düzeltildi
- OEM üretici yazılımı güncelleştirme işlemiyle ilgili bir sorun düzeltildi. Bu sorun 1907 sürümünde bulunur ve düzeltme [KB4515650](https://support.microsoft.com/en-us/help/4515650/azure-stack-hotfix-1-1907-8-37) düzeltildi


<!-- Product fixes that came up from customer deployments worth highlighting, especially if there is an SR/ICM associated to it. -->

## <a name="security-updates"></a>Güvenlik güncelleştirmeleri

Bu Azure Stack güncelleştirmesinde güvenlik güncelleştirmeleri hakkında daha fazla bilgi için bkz. [Azure Stack Security Updates](azure-stack-release-notes-security-updates.md).

## <a name="update-planning"></a>Planlamayı Güncelleştir

Güncelleştirmeyi uygulamadan önce, aşağıdaki bilgileri gözden geçirdiğinizden emin olun:

- [Bilinen sorunlar](azure-stack-release-notes-known-issues-1908.md)
- [Güvenlik güncelleştirmeleri](azure-stack-release-notes-security-updates.md)
- [Güncelleştirme uygulandıktan önceki ve sonraki etkinliklerin denetim listesi](azure-stack-release-notes-checklist.md)

## <a name="download-the-update"></a>Güncelleştirmeyi indirin

Azure Stack 1908 güncelleştirme paketini [Azure Stack indirme sayfasından](https://aka.ms/azurestackupdatedownload)indirebilirsiniz.

## <a name="hotfixes"></a>Düzeltmeler

Düzeltmeleri düzenli aralıklarla yayınlar Azure Stack. Azure Stack 1908 ' e güncelleştirmeden önce 1907 için en son Azure Stack düzeltmesini yüklediğinizden emin olun.

Azure Stack düzeltmeler yalnızca Azure Stack tümleşik sistemler için geçerlidir; ASDK 'ye düzeltme yüklemeyi denemeyin.

### <a name="prerequisites-before-applying-the-1908-update"></a>Önkoşullar: 1908 güncelleştirmesini uygulamadan önce

Azure Stack 1908 sürümü aşağıdaki düzeltmelere sahip 1907 sürümüne uygulanmalıdır:

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hotfix 1.1907.12.44](https://support.microsoft.com/help/4517473)

Azure Stack 1908 güncelleştirmesi, sisteminizin donanım sağlayıcısından **Azure Stack OEM sürüm 2,1 veya üstünü** gerektirir. OEM güncelleştirmeleri, Azure Stack sistem Donanımınıza yönelik sürücü ve bellenim güncelleştirmelerini içerir. OEM güncelleştirmelerini uygulama hakkında daha fazla bilgi için bkz. [Apply Azure Stack özgün ekipman üreticisi güncelleştirmeleri](azure-stack-update-oem.md)

### <a name="after-successfully-applying-the-1908-update"></a>1908 güncelleştirmesini başarıyla uyguladıktan sonra

Bu güncelleştirmenin yüklenmesinden sonra, geçerli düzeltmeleri yükleme. Daha fazla bilgi için bkz. [hizmet ilkeniz](azure-stack-servicing-policy.md).

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- 1908 için kullanılabilir düzeltme yok.

## <a name="automatic-update-notifications"></a>Otomatik güncelleştirme bildirimleri

Altyapı ağından internet 'e erişebilen sistemler, operatör portalındaki **güncelleştirme kullanılabilir** iletisini görür. İnternet erişimi olmayan sistemler. zip dosyasını indirebilir ve ilgili. xml ile içe aktarabilir.

> [!TIP]  
> Azure Stack düzeltmelere devam etmek için aşağıdaki *RSS* veya *atom* akışlarına abone olun:
>
> - ['YI](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss)
> - [Atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom)

## <a name="archived-release-notes"></a>Arşivlenmiş sürüm notları

[Azure Stack sürüm notlarının eski sürümlerini TechNet galerisinde](https://aka.ms/azsarchivedrelnotes)görebilirsiniz. Bu arşivlenmiş sürüm notları yalnızca başvuru amacıyla sağlanır ve bu sürümler için destek göstermez. Azure Stack desteği hakkında bilgi için bkz. [Azure Stack hizmet ilkesi](azure-stack-servicing-policy.md). Daha fazla yardım için Microsoft Müşteri Destek Hizmetleri 'ne başvurun.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Stack'te güncelleştirme yönetimi genel bakış için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md).  
- Azure Stack güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için bkz. [güncelleştirmelerini Azure Stack'te](azure-stack-apply-updates.md).
- Azure Stack tümleşik sistemleri ve desteklenen bir duruma sisteminizi tutmak için yapmanız gerekenlere bakım ilkeyi gözden geçirmek için bkz: [Azure Stack hizmet İlkesi](azure-stack-servicing-policy.md).  
- Ayrıcalıklı uç noktasına (CESARETLENDİRİCİ) izlemek ve güncelleştirmelerini sürdürmek üzere kullanmak için bkz [izleme ayrıcalıklı uç noktayı kullanarak Azure stack'teki güncelleştirmeleri](azure-stack-monitor-update.md).
