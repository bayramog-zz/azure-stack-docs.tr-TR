---
title: Azure Stack 1907 sürüm notları | Microsoft Docs
description: Yenilikler, bilinen sorunlar ve güncelleştirmenin indirileceği gibi Azure Stack tümleşik sistemler için 1907 güncelleştirmesi hakkında bilgi edinin.
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
ms.date: 08/16/2019
ms.author: sethm
ms.reviewer: hectorl
ms.lastreviewed: 08/16/2019
ms.openlocfilehash: 526343889bbd5cc784ebb87c39603e2152a98f3b
ms.sourcegitcommit: b8260ef3e43f3703dd0df16fb752610ec8a86942
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70008744"
---
# <a name="azure-stack-1907-update"></a>Azure Stack 1907 güncelleştirmesi

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Bu makalede 1907 güncelleştirme paketinin içeriği açıklanır. Güncelleştirme yenilikleri ve bu Azure Stack sürümü için düzeltmeler içerir.

> [!IMPORTANT]  
> Yalnızca Azure Stack tümleşik sistemleri için bu güncelleştirme paketidir. Bu güncelleştirme paketi için Azure Stack geliştirme Seti'ni geçerli değildir.

## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack 1907 güncelleştirme yapı numarası **1.1907.0.20**.

### <a name="update-type"></a>Güncelleştirme türü

Azure Stack 1907 güncelleştirme yapı türü **Express**'dir. Güncelleştirme oluşturma türleri hakkında daha fazla bilgi için [Azure Stack güncelleştirmeleri yönetme](azure-stack-updates.md) makalesine bakın. İç teste bağlı olarak, 1907 güncelleştirmesinin tamamlaması için gereken süre yaklaşık 13 saattir.

- Tam güncelleştirme çalışma zamanları, genellikle kiracı iş yükleri, sistem ağ bağlantınız (internet 'e bağlıysa) ve sistem donanım yapılandırmanız tarafından sisteminizde kullanılan kapasiteye bağlıdır.
- Beklenenden uzun süre içinde olan çalışma zamanları seyrek değildir ve güncelleştirme başarısız olmazsa Azure Stack işleçlere göre eylem gerektirmez.
- Bu çalışma zamanı yaklaşık 1907 güncelleştirmesine özeldir ve diğer Azure Stack güncelleştirmeleriyle karşılaştırılmamalıdır.

## <a name="whats-in-this-update"></a>Bu güncelleştirmede neler vardır

<!-- The current theme (if any) of this release. -->

### <a name="whats-new"></a>Yenilikler

<!-- What's new, also net new experiences and features. -->

- Tanılama günlüğü toplamayı kolaylaştırmak ve geliştirmek için Azure Stack tanılama günlüğü toplama hizmetinin genel kullanım sürümü. Azure Stack tanılama günlüğü toplama hizmeti, Microsoft Müşteri Destek Hizmetleri (CSS) ile tanılama günlüklerini toplayıp paylaşmak için basitleştirilmiş bir yöntem sağlar. Bu tanılama günlüğü toplama hizmeti, operatörlerin bir depolama blobuna belirli kritik uyarıların ne zaman yapıldığında otomatik olarak yüklenmesini veya aynı işlemi gerçekleştirmesini sağlayan Azure Stack yönetici portalında yeni bir kullanıcı deneyimi sağlar. isteğe bağlı işlem. Daha fazla bilgi için bkz. [tanılama günlüğü toplama](azure-stack-diagnostic-log-collection-overview.md) makalesi.

- Azure Stack doğrulama aracının **Test-AzureStack**'in bir parçası olarak Azure Stack ağ altyapısı doğrulamasının genel kullanım sürümü. Azure Stack ağ altyapısı, Azure Stack ağ altyapısında bir hata oluşup oluşmadığını belirlemek için **Test-AzureStack**'in bir parçası olacaktır. Test, yazılım tanımlı Azure Stack ağı atlayarak ağ altyapısının bağlantısını denetler. Ortak bir VIP 'den yapılandırılmış DNS ileticilerine, NTP sunucularına ve kimlik uç noktalarına olan bağlantıyı gösterir. Ayrıca, kimlik sağlayıcısı olarak Azure AD 'yi kullanırken veya ADFS kullanılırken Federasyon sunucusu ile Azure 'a bağlantıyı denetler. Daha fazla bilgi için [Azure Stack doğrulama aracı](azure-stack-diagnostic-test.md) makalesine bakın.

- Bir sistem güncelleştirmesi sırasında, iç SQL TLS sertifikalarını gereken şekilde döndürmek için bir iç gizli dizi döndürme yordamı eklenmiştir.

### <a name="improvements"></a>Geliştirmeleri

<!-- Changes and product improvements with tangible customer-facing value. -->

- Azure Stack güncelleştirme dikey penceresinde artık etkin güncelleştirmeler için **son adım tamamlanmış** bir zaman görüntülenir. Bu, güncelleştirme dikey penceresine gidip çalışan bir güncelleştirmeye tıklanarak görülebilir. **Son adım tamamlandığında** **güncelleştirme çalıştırması ayrıntıları** bölümünde kullanılabilir.

- **Start-AzureStack** ve **stop-azurestack** işleç eylemlerine yönelik iyileştirmeler. Azure Stack başlama zamanı, ortalama% 50 oranında azaltılmıştır. Azure Stack kapatma süresi, ortalama% 30 oranında azaltılmıştır. Ortalama başlatma ve kapatılma süreleri, bir ölçek birimindeki düğümlerin sayısı arttıkça aynı kalır.

- Bağlantısı kesilen Market aracı için geliştirilmiş hata işleme. **Export-AzSOfflineMarketplaceItem**kullanılırken bir indirme başarısız olursa veya kısmen başarılı olursa, hata ve risk azaltma adımları hakkında daha fazla ayrıntı içeren ayrıntılı bir hata iletisi görüntülenir.

- Büyük bir sayfa blobundan/anlık görüntüsünden yönetilen disk oluşturma performansı geliştirilmiştir. Daha önce, büyük bir disk oluştururken bir zaman aşımı tetikledi.  

<!-- https://icm.ad.msft.net/imp/v3/incidents/details/127669774/home -->
- Beklenmedik sanal disk ayırmayı önlemek için bir düğümü kapatmadan önce sanal disk sistem durumu denetimi geliştirildi.

- Yönetici işlemlerine yönelik iç günlüklerin depolanması geliştirildi. Bu durum, iç günlük işlemlerinin bellek ve depolama tüketimini en aza indirerek yönetici işlemleri sırasında performansı ve güvenilirliği elde ediyor. Yönetici portalındaki güncelleştirme dikey penceresinin Gelişmiş sayfa yükleme sürelerini de fark edebilirsiniz. Bu iyileştirmesinin bir parçası olarak, 6 aydan eski olan güncelleştirme günlükleri sistemde artık kullanılamaz. Bu güncelleştirmeler için günlüklere ihtiyacınız varsa, 1907 güncelleştirmesini gerçekleştirmeden önce 6 aydan eski tüm güncelleştirme çalıştırmaları için [Özeti indirdiğinizden](azure-stack-apply-updates.md) emin olun.

### <a name="changes"></a>Değişiklikler

- Azure Stack sürüm 1907, operatörlere 1908 güncelleştirme **yapmadan önce** işletmenlerinin sistem OEM paketini 2,1 veya sonraki bir sürüme güncelleştirdiğinizden emin olan bir uyarı uyarısı içerir. Donanım sağlayıcıları, Azure Stack sürüm 1908 ile aynı zamanda OEM paketi 2,1 ' i serbest bırakır. OEM paketi 2,1 veya üzeri, Azure Stack sürüm 1908 için bir önkoşul olacaktır. OEM paketi 2,1 veya sonraki bir sürümünü indirme hakkında daha fazla bilgi için sisteminizin donanım sağlayıcısına başvurun.

- Azure Stack tanılama günlüğü toplama hizmeti için iletişimi etkinleştirmek üzere yeni bir giden kuralı (HTTPS) eklendi. Daha fazla bilgi için bkz. [Azure Stack Datacenter Integration-son nokta yayımlama](azure-stack-integrate-endpoints.md#ports-and-urls-outbound).

- Altyapı yedekleme hizmeti artık dış depolama konumu kapasiteyi tükeniyorsa kısmen karşıya yüklenen yedeklemeleri siler.

- Altyapı yedeklemeleri artık etki alanı Hizmetleri verilerinin bir yedeklemesini içermez. Bu, yalnızca kimlik sağlayıcısı olarak Azure Active Directory kullanan sistemler için geçerlidir.

- Artık, **işlem > VM görüntüleri** dikey penceresine alınan bir görüntünün Sayfa Blobu türünde olduğunu doğrulayacağız.

### <a name="fixes"></a>Düzeltmeleri

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there is an SR/ICM associated to it. -->
- Yayımcı, teklif ve SKU 'nun bir Kaynak Yöneticisi şablonunda büyük/küçük harfe duyarlı olduğu bir sorun düzeltildi: görüntü parametreleri Yayımcı, teklif ve SKU ile aynı durumda değilse, görüntü dağıtım için getirilmedi.

<!-- https://icm.ad.msft.net/imp/v3/incidents/details/129536438/home -->
- Depolama hizmeti meta verilerinin yedeklenmesi sırasında zaman aşımları nedeniyle, bir **Partialsucceeded** hata iletisiyle başarısız olan yedeklemelerle ilgili bir sorun düzeltildi.  

- Kullanıcı aboneliklerinin silinmesinin yalnız bırakılmış kaynaklarla sonuçlandığı bir sorun düzeltildi.

- Bir teklif oluşturulurken açıklama alanı kaydedilmemiş bir sorun düzeltildi.

- **Salt okuma** izinlerine sahip bir kullanıcının kaynakları oluşturabildiği, düzenleyebilen ve silebileceği bir sorun düzeltildi. Artık Kullanıcı, **katkıda** bulunan izin atandığında yalnızca kaynak oluşturabiliyor. 

<!-- https://icm.ad.msft.net/imp/v3/incidents/details/127772311/home -->
- WMI sağlayıcısı ana bilgisayarı tarafından kilitlenen bir DLL dosyası nedeniyle güncelleştirmenin başarısız olduğu bir sorun düzeltildi.

- Güncelleştirme hizmetinde, güncelleştirme kutucuğunda veya kaynak sağlayıcısında kullanılabilir güncelleştirmelerin görüntülenmesini önleyen bir sorun düzeltildi. Bu sorun 1906 sürümünde bulunur ve düzeltme [KB4511282](https://support.microsoft.com/help/4511282/)içinde düzeltildi.

- Hatalı bir yapılandırma nedeniyle yönetim düzlemi sağlıksız olduğundan güncelleştirmelerin başarısız olmasına neden olabilecek bir sorun düzeltildi. Bu sorun 1906 sürümünde bulunur ve düzeltme [KB4512794](https://support.microsoft.com/help/4512794/)içinde düzeltildi.

- Kullanıcıların Market 'ten üçüncü taraf görüntülerin dağıtımını tamamlamasını engelleyen bir sorun düzeltildi. Bu sorun 1906 sürümünde bulunur ve düzeltme [KB4511259](https://support.microsoft.com/help/4511259/)içinde düzeltildi.

- Kullanıcı görüntüsü Yöneticisi hizmeti kilitlenmekte olduğundan yönetilen görüntülerden VM oluşturma 'nın başarısız olmasına neden olabilecek bir sorun düzeltildi. Bu sorun 1906 sürümünde bulunur ve düzeltme [KB4512794](https://support.microsoft.com/help/4512794/) düzeltildi

- Uygulama ağ geçidi önbelleğinin beklendiği şekilde yenilenmediği için VM CRUD işlemlerinin başarısız olmasına neden olan bir sorun düzeltildi. Bu sorun 1906 sürümünde bulunur ve düzeltme [KB4513119](https://support.microsoft.com/en-us/help/4513119/) düzeltildi

- Sistem durumu kaynak sağlayıcısında, yönetici portalındaki bölge ve uyarı dikey pencerelerinin kullanılabilirliğini etkileyen bir sorun düzeltildi. Bu sorun 1906 sürümünde bulunur ve düzeltme [KB4512794](https://support.microsoft.com/help/4512794)içinde düzeltildi.

## <a name="security-updates"></a>Güvenlik güncelleştirmeleri

Bu Azure Stack güncelleştirmesinde güvenlik güncelleştirmeleri hakkında daha fazla bilgi için bkz. [Azure Stack Security Updates](azure-stack-release-notes-security-updates-1907.md).

## <a name="update-planning"></a>Planlamayı Güncelleştir

Güncelleştirmeyi uygulamadan önce, aşağıdaki bilgileri gözden geçirdiğinizden emin olun:

- [Bilinen sorunlar](azure-stack-release-notes-known-issues-1907.md)
- [Güvenlik güncelleştirmeleri](azure-stack-release-notes-security-updates-1907.md)
- [Güncelleştirme uygulandıktan önceki ve sonraki etkinliklerin denetim listesi](azure-stack-release-notes-checklist.md)

## <a name="download-the-update"></a>Güncelleştirmeyi indirin

Azure Stack 1907 güncelleştirme paketini [Azure Stack indirme sayfasından](https://aka.ms/azurestackupdatedownload)indirebilirsiniz.

## <a name="hotfixes"></a>Düzeltmeler

Düzeltmeleri düzenli aralıklarla yayınlar Azure Stack. Azure Stack 1907 ' e güncelleştirmeden önce 1906 için en son Azure Stack düzeltmesini yüklediğinizden emin olun.

Azure Stack düzeltmeler yalnızca Azure Stack tümleşik sistemler için geçerlidir; ASDK 'ye düzeltme yüklemeyi denemeyin.

### <a name="before-applying-the-1907-update"></a>1907 güncelleştirmesini uygulamadan önce

Azure Stack 1907 sürümü aşağıdaki düzeltmelere sahip 1906 sürümüne uygulanmalıdır:

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hotfix 1.1906.12.54](https://support.microsoft.com/help/4518365)

### <a name="after-successfully-applying-the-1907-update"></a>1907 güncelleştirmesini başarıyla uyguladıktan sonra

Bu güncelleştirmenin yüklenmesinden sonra, geçerli düzeltmeleri yükleme. Daha fazla bilgi için bkz. [hizmet ilkeniz](azure-stack-servicing-policy.md).

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hotfix 1.1907.12.44](https://support.microsoft.com/help/4517473)

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
