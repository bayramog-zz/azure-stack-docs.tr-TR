---
title: Azure Stack 1906 sürüm notları | Microsoft Docs
description: Yenilikler, bilinen sorunlar ve güncelleştirmenin indirileceği gibi Azure Stack tümleşik sistemler için 1906 güncelleştirmesi hakkında bilgi edinin.
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
ms.date: 07/15/2019
ms.author: sethm
ms.reviewer: hectorl
ms.lastreviewed: 07/15/2019
monikerRange: azs-1906
ms.openlocfilehash: bf8e32d6395b3ddfc54fe586568b51b76b71dc63
ms.sourcegitcommit: 71d7990a2b21576c44bb2aea13ae2026e9510c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70188298"
---
# <a name="azure-stack-1906-update"></a>Azure Stack 1906 güncelleştirmesi

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Bu makalede 1906 güncelleştirme paketinin içeriği açıklanır. Güncelleştirme yenilikleri ve bu Azure Stack sürümü için düzeltmeler içerir.

> [!IMPORTANT]  
> Yalnızca Azure Stack tümleşik sistemleri için bu güncelleştirme paketidir. Bu güncelleştirme paketi için Azure Stack geliştirme Seti'ni geçerli değildir.

## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack 1906 güncelleştirme yapı numarası **1.1906.0.30**.

### <a name="update-type"></a>Güncelleştirme türü

Azure Stack 1906 güncelleştirme yapı türü **Express**'dir. Güncelleştirme oluşturma türleri hakkında daha fazla bilgi için [Azure Stack güncelleştirmeleri yönetme](azure-stack-updates.md) makalesine bakın. 1906 güncelleştirmesinin tamamlaması için gereken süre, Azure Stack ortamınızdaki fiziksel düğümlerin sayısından bağımsız olarak yaklaşık 10 saattir. Tamamen güncelleştirme çalışma zamanları, genellikle kiracı iş yükleri, sistem ağ bağlantınız (internet 'e bağlıysa) ve sistem donanımınızın belirtimleri tarafından sisteminizde kullanılan kapasiteye bağlıdır. Beklenen değerden daha uzun süre içinde yer kalmayan çalışma zamanları, güncelleştirme başarısız olmadığı için Azure Stack işleçlere göre işlem gerektirmemektedir. Bu çalışma zamanı yaklaşık 1906 güncelleştirmesine özeldir ve diğer Azure Stack güncelleştirmeleriyle karşılaştırılmamalıdır.

## <a name="whats-in-this-update"></a>Bu güncelleştirmede neler vardır

<!-- The current theme (if any) of this release. -->

<!-- What's new, also net new experiences and features. -->

- Tüm uç noktalarında TLS 1,2 zorlamak için ayrıcalıklı uç noktaya (PEP) bir **set-TLSPolicy** cmdlet 'i eklendi. Daha fazla bilgi için bkz. [Azure Stack güvenlik denetimleri](azure-stack-security-configuration.md).

- Uygulanan TLS ilkesini almak için ayrıcalıklı uç noktaya (PEP) bir **Get-TLSPolicy** cmdlet 'i eklendi. Daha fazla bilgi için bkz. [Azure Stack güvenlik denetimleri](azure-stack-security-configuration.md).

- Bir sistem güncelleştirmesi sırasında iç TLS sertifikalarını gereken şekilde döndürmek için bir iç gizli anahtar prosedürü eklendi.

- Süresi dolan gizli dizi uyarısı yok sayıldığında iç gizli dizileri zorlayarak iç Parolaların süresinin dolmasını önlemek için bir koruma eklendi. Bu, normal bir işletim yordamı olarak güvenilmemelidir. Gizli dizi yönü, bakım penceresi sırasında planlanmalıdır. Daha fazla bilgi için bkz. [Azure Stack gizli döndürme](azure-stack-rotate-secrets.md).

- Visual Studio Code artık AD FS kullanarak Azure Stack dağıtımı ile desteklenmektedir.

### <a name="improvements"></a>Geliştirmeleri

<!-- Changes and product improvements with tangible customer-facing value. -->

- Ayrıcalıklı uç noktasındaki **Get-GraphApplication** cmdlet 'i artık kullanılmakta olan sertifikanın parmak izini görüntüler. Bu, Azure Stack AD FS dağıtıldığında hizmet sorumluları için sertifika yönetimini geliştirir.

- Uyarı verme özelliği de dahil olmak üzere AD grafiğinin ve AD FS kullanılabilirliğini doğrulamak için yeni sistem durumu izleme kuralları eklenmiştir.

- Altyapı yedekleme hizmeti başka bir örneğe taşırken, yedekleme kaynak sağlayıcısının güvenilirliğini artırmaya yönelik geliştirmeler.

- Bakım penceresinin planlanmasını kolaylaştırmak için Tekdüzen yürütme süresi sağlamak üzere dış gizli anahtar döndürme yordamının performans iyileştirmesi.

- **Test-AzureStack** cmdlet 'i artık sona ermek üzere olan iç gizli dizileri (kritik uyarılar) bildiriyor.

- AD FS için Federasyon güveni yapılandırılırken, sertifika iptal listesi denetimini atlamayı sağlayan ayrıcalıklı uç noktada **register-CustomAdfs** cmdlet 'i için yeni bir parametre mevcuttur.

- 1906 sürümü güncelleştirme ilerleme durumunun daha büyük bir görünürlüğünü sunarak güncelleştirmelerin duraklatılmadığından emin olabilirsiniz. Bu, **güncelleştirme** dikey penceresindeki işleçlere gösterilen güncelleştirme adımlarının toplam sayısının artmasına neden olur. Ayrıca, önceki güncelleştirmelerden daha fazla güncelleştirme adımı olduğunu fark edebilirsiniz.

#### <a name="networking-updates"></a>Ağ güncelleştirmeleri

- DHCP Yanıtlayıcının Azure ile tutarlı olması için ayarlanan kira süresi güncelleştirildi.

- Kaynak sağlayıcıya, başarısız kaynakların dağıtımı senaryosunda geliştirilmiş yeniden deneme oranları.

- **Standart** SKU seçeneği şu anda desteklenmeyen şekilde hem yük dengeleyiciden hem de genel IP 'den kaldırılmıştır.

### <a name="changes"></a>Değişiklikler

- Depolama hesabı deneyimi oluşturmak artık Azure ile tutarlıdır.

- İç gizli dizi süresi sonu için uyarı Tetikleyicileri değiştirildi:
  - Uyarı uyarıları artık gizli dizi süresi dolmadan 90 gün önce tetiklenir.
  - Kritik uyarılar artık gizli dizileri sona ermeden önce 30 gün sonra tetiklenir.

- Tutarlı terminoloji için altyapı yedekleme kaynak sağlayıcısındaki dizeler güncelleştirildi.

### <a name="fixes"></a>Düzeltmeleri

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there is an SR/ICM associated to it. -->

- Yönetilen bir disk VM 'sinin yeniden boyutlandırılırken bir **Iç Işlem hatasıyla**başarısız olan bir sorun düzeltildi.

- Başarısız bir Kullanıcı görüntüsü oluşturmanın, görüntüleri yöneten hizmeti kötü durumda olduğu bir sorun düzeltildi; Bu, başarısız olan görüntünün silinmesini ve yeni görüntülerin oluşturulmasını engeller. Bu, 1905 düzeltmesinde de düzeltilir.

- Süresi dolan iç gizliliklerin etkin uyarıları, iç gizli döndürmenin başarılı bir şekilde yürütülmesinden sonra otomatik olarak kapatılır.

- Güncelleştirme geçmişi sekmesindeki güncelleştirme süresinin 99 saatten uzun bir süredir çalışıyor olması durumunda ilk basamağı kırpabileceği bir sorun düzeltildi.

- **Güncelleştirme** dikey penceresinde, başarısız güncelleştirmeler Için bir **özgeçmişi** seçeneği bulunur.

- Yönetici ve Kullanıcı portalları ' nde, Docker uzantısının aramadan hatalı şekilde döndürüldüğü, ancak başka bir eylem gerçekleştirilmediğinden, Azure Stack ' de kullanılamadığından Market 'teki sorunu düzeltildi.

- Şablon adı ' _ ' alt çizgiyle başlıyorsa parametreleri doldurmayan şablon dağıtım Kullanıcı arabirimindeki bir sorun düzeltildi.

- Sanal makine ölçek kümesi oluşturma deneyiminin dağıtım seçeneği olarak CentOS tabanlı 7,2 sağladığı bir sorun düzeltildi. CentOS 7,2 Azure Stack kullanılamıyor. Şimdi dağıtım için bir seçenek olarak CentOS 7,5 sunuyoruz

- Artık **Sanal Makine Ölçek Kümeleri** dikey penceresinden bir ölçek kümesini kaldırabilirsiniz.

## <a name="security-updates"></a>Güvenlik güncelleştirmeleri

Bu Azure Stack güncelleştirmesinde güvenlik güncelleştirmeleri hakkında daha fazla bilgi için bkz. [Azure Stack Security Updates](azure-stack-release-notes-security-updates.md).

## <a name="update-planning"></a>Planlamayı Güncelleştir

Güncelleştirmeyi uygulamadan önce, aşağıdaki bilgileri gözden geçirdiğinizden emin olun:

- [Bilinen sorunlar](azure-stack-release-notes-known-issues-1906.md)
- [Güvenlik güncelleştirmeleri](azure-stack-release-notes-security-updates.md)
- [Güncelleştirme uygulandıktan önceki ve sonraki etkinliklerin denetim listesi](azure-stack-release-notes-checklist.md)

## <a name="download-the-update"></a>Güncelleştirmeyi indirin

Azure Stack 1906 güncelleştirme paketini [Azure Stack indirme sayfasından](https://aka.ms/azurestackupdatedownload)indirebilirsiniz.

## <a name="hotfixes"></a>Düzeltmeler

Düzeltmeleri düzenli aralıklarla yayınlar Azure Stack. Azure Stack 1906 ' e güncelleştirmeden önce 1905 için en son Azure Stack düzeltmesini yüklediğinizden emin olun. Güncelleştirme sonrasında, [1906 için kullanılabilir düzeltmeleri](#after-successfully-applying-the-1906-update)yüklersiniz.

Azure Stack düzeltmeler yalnızca Azure Stack tümleşik sistemler için geçerlidir; ASDK 'ye düzeltme yüklemeyi denemeyin.

### <a name="before-applying-the-1906-update"></a>1906 güncelleştirmesini uygulamadan önce

Azure Stack 1906 sürümü aşağıdaki düzeltmelere sahip 1905 sürümüne uygulanmalıdır:

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hotfix 1.1905.3.48](https://support.microsoft.com/help/4510078)

### <a name="after-successfully-applying-the-1906-update"></a>1906 güncelleştirmesini başarıyla uyguladıktan sonra

Bu güncelleştirmenin yüklenmesinden sonra, geçerli düzeltmeleri yükleme. Daha fazla bilgi için bkz. [hizmet ilkeniz](azure-stack-servicing-policy.md).

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hotfix 1.1906.12.54](https://support.microsoft.com/help/4518365)

## <a name="automatic-update-notifications"></a>Otomatik güncelleştirme bildirimleri

Altyapı ağından internet 'e erişebilen sistemlere sahip müşteriler, operatör portalındaki **güncelleştirme kullanılabilir** iletisini görür. İnternet erişimi olmayan sistemler. zip dosyasını indirebilir ve ilgili. xml ile içe aktarabilir.

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
