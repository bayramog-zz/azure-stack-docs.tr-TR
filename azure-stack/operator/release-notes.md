---
title: Sürüm notlarını Azure Stack | Microsoft Docs
description: Yenilikler ve güncelleştirmenin indirileceği dahil Azure Stack tümleşik sistemler için güncelleştirmeler hakkında bilgi edinin.
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
ms.date: 09/27/2019
ms.author: sethm
ms.reviewer: prchint
ms.lastreviewed: 08/30/2019
ms.openlocfilehash: 62440c61ee637a0092b1e956b08ba0a9c94d8bab
ms.sourcegitcommit: c2ea4ffb42563c26faaf2993ba7b484bcb6d5cb7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71342925"
---
# <a name="azure-stack-updates-release-notes"></a>Azure Stack güncelleştirmeler: sürüm notları

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Bu makalede Azure Stack güncelleştirme paketlerinin içerikleri açıklanır. Güncelleştirme yenilikleri ve bu Azure Stack sürümü için düzeltmeler içerir.

Farklı bir sürüme yönelik sürüm notlarına erişmek için, sol taraftaki içindekiler tablosunun üzerindeki sürüm Seçicisi açılan listesini kullanın.

::: moniker range=">=azs-1905"
> [!IMPORTANT]  
> Yalnızca Azure Stack tümleşik sistemleri için bu güncelleştirme paketidir. Bu güncelleştirme paketi için Azure Stack geliştirme Seti'ni geçerli değildir.
::: moniker-end
::: moniker range="<azs-1905"
> [!IMPORTANT]  
> Azure Stack örneğiniz ikiden fazla güncelleştirme tarafından arkasındaysa uyumsuz olarak kabul edilir. [Destek almak için en az desteklenen sürüme güncelleştirmeniz](azure-stack-servicing-policy.md#keep-your-system-under-support)gerekir. 
::: moniker-end

## <a name="update-planning"></a>Planlamayı Güncelleştir

Güncelleştirmeyi uygulamadan önce, aşağıdaki bilgileri gözden geçirdiğinizden emin olun:

- [Bilinen sorunlar](known-issues.md)
- [Güvenlik güncelleştirmeleri](release-notes-security-updates.md)
- [Güncelleştirme uygulandıktan önceki ve sonraki etkinliklerin denetim listesi](release-notes-checklist.md)

Güncelleştirmeler ve güncelleştirme sürecinde sorun giderme hakkında yardım için bkz. [Azure Stack için düzeltme eki ve güncelleştirme sorunlarını giderme](azure-stack-updates-troubleshoot.md).

<!---------------------------------------------------------->
<!------------------- SUPPORTED VERSIONS ------------------->
<!---------------------------------------------------------->
::: moniker range="azs-1908"
## <a name="1908-build-reference"></a>1908 derleme başvurusu

Azure Stack 1908 güncelleştirme yapı numarası **1.1908.4.33**.

### <a name="update-type-1908"></a>Güncelleştirme türü

1908 için, Azure Stack çalıştığı temel işletim sistemi Windows Server 2019 ' e güncelleştirilmiştir. Bu, temel temel geliştirmelerin yanı sıra, yakın gelecekte Azure Stack için ek özellikler getirme özelliğini sağlar.

Azure Stack 1908 güncelleştirme yapı türü **dolu**. Sonuç olarak, 1908 güncelleştirmesinin 1906 ve 1907 gibi hızlı güncelleştirmelerden daha uzun bir çalışma zamanı vardır. Tam güncelleştirmeler için tam çalışma zamanları genellikle Azure Stack örneğinizin içerdiği düğüm sayısına, kiracı iş yükleri tarafından sisteminizde kullanılan kapasiteye, sisteminizin ağ bağlantısına (internet 'e bağlıysa) ve sistem donanımınıza bağlıdır yapılandırmada. 1908 güncelleştirmesinde, iç sınamamız içinde aşağıdaki beklenen çalışma zamanları vardı: 4 düğüm-42 saat, 8 düğüm-50 saat, 12 düğüm-60 saat, 16 düğüm-70 saat. Bu beklenen değerlerin daha uzun süre içinde güncelleştirilmesi, güncelleştirme başarısız olmadığı için Azure Stack işleçlere göre işlem gerektirmemektedir.

Güncelleştirme oluşturma türleri hakkında daha fazla bilgi için bkz. [Azure Stack güncelleştirmeleri yönetme](azure-stack-updates.md).

- Tam güncelleştirme çalışma zamanları, genellikle kiracı iş yükleri, sistem ağ bağlantınız (internet 'e bağlıysa) ve sistem donanım yapılandırmanız tarafından sisteminizde kullanılan kapasiteye bağlıdır.
- Beklenenden uzun süre içinde olan çalışma zamanları seyrek değildir ve güncelleştirme başarısız olmazsa Azure Stack işleçlere göre eylem gerektirmez.
- Bu çalışma zamanı yaklaşık 1908 güncelleştirmesine özeldir ve diğer Azure Stack güncelleştirmeleriyle karşılaştırılmamalıdır.

<!-- ## What's in this update -->

<!-- The current theme (if any) of this release. -->

### <a name="whats-new-1908"></a>Yenilikler

<!-- What's new, also net new experiences and features. -->

- 1908 için, Azure Stack çalıştığı temeldeki işletim sisteminin Windows Server 2019 ' e güncelleştirildiğini unutmayın. Bu, temel temel geliştirmelerin yanı sıra, yakın gelecekte Azure Stack için ek özellikler getirme özelliğini sağlar.
- Azure Stack altyapısının tüm bileşenleri artık FIPS 140-2 modunda çalışır.
- Azure Stack işleçler artık Portal Kullanıcı verilerini kaldırabilir. Daha fazla bilgi için bkz. [Azure Stack Portal Kullanıcı verilerini temizleme](azure-stack-portal-clear.md).

### <a name="improvements-1908"></a>Geliştirmeleri

<!-- Changes and product improvements with tangible customer-facing value. -->
- Fiziksel düğümlerin donanım Güvenilir Platform Modülü (TPM) ile gizli dizileri kalıcı hale getirmek için Azure Stack Rest şifrelemesinde veri geliştirmeleri.

### <a name="changes-1908"></a>Değişikliklerine

- Donanım sağlayıcıları, Azure Stack sürüm 1908 ile aynı anda OEM Uzantı paketi 2,1 veya üstünü serbest bırakır. OEM uzantısı paketi 2,1 veya üzeri, Azure Stack sürüm 1908 için bir önkoşuldur. OEM Uzantı paketi 2,1 veya sonraki bir sürümünü indirme hakkında daha fazla bilgi için sisteminizin donanım sağlayıcısına başvurun ve [OEM güncelleştirmeleri](azure-stack-update-oem.md#oem-contact-information) makalesine bakın.  

### <a name="fixes-1908"></a>Düzeltilen

- Gelecekteki Azure Stack OEM güncelleştirmeleriyle uyumlulukla ilgili bir sorun ve müşteri Kullanıcı görüntülerini kullanarak VM dağıtımıyla ilgili bir sorun düzeltildi. Bu sorun 1907 sürümünde bulunur ve düzeltme [KB4517473](https://support.microsoft.com/en-us/help/4517473/azure-stack-hotfix-1-1907-12-44) düzeltildi  
- Yapı halka durumu için test-AzureStack 'te OEM üretici yazılımı güncelleştirmesiyle ilgili bir sorun düzeltildi. Bu sorun 1907 sürümünde bulunur ve düzeltme [KB4515310](https://support.microsoft.com/en-us/help/4515310/azure-stack-hotfix-1-1907-7-35) düzeltildi
- OEM üretici yazılımı güncelleştirme işlemiyle ilgili bir sorun düzeltildi. Bu sorun 1907 sürümünde bulunur ve düzeltme [KB4515650](https://support.microsoft.com/en-us/help/4515650/azure-stack-hotfix-1-1907-8-37) düzeltildi


<!-- Product fixes that came up from customer deployments worth highlighting, especially if there is an SR/ICM associated to it. -->

## <a name="security-updates-1908"></a>Güvenlik güncelleştirmeleri

Bu Azure Stack güncelleştirmesinde güvenlik güncelleştirmeleri hakkında daha fazla bilgi için bkz. [Azure Stack Security Updates](release-notes-security-updates.md).

## <a name="download-the-update-1908"></a>Güncelleştirmeyi indir

Azure Stack 1908 güncelleştirme paketini [Azure Stack indirme sayfasından](https://aka.ms/azurestackupdatedownload)indirebilirsiniz.

## <a name="hotfixes-1908"></a>Başlatmayla

Düzeltmeleri düzenli aralıklarla yayınlar Azure Stack. Azure Stack 1908 ' e güncelleştirmeden önce 1907 için en son Azure Stack düzeltmesini yüklediğinizden emin olun.

Azure Stack düzeltmeler yalnızca Azure Stack tümleşik sistemler için geçerlidir; ASDK 'ye düzeltme yüklemeyi denemeyin.

### <a name="prerequisites-before-applying-the-1908-update"></a>Önkoşullar: 1908 güncelleştirmesini uygulamadan önce

Azure Stack 1908 sürümü aşağıdaki düzeltmelere sahip 1907 sürümüne uygulanmalıdır:

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hotfix 1.1907.17.54](https://support.microsoft.com/help/4523826)

Azure Stack 1908 güncelleştirmesi, sisteminizin donanım sağlayıcısından **Azure Stack OEM sürüm 2,1 veya üstünü** gerektirir. OEM güncelleştirmeleri, Azure Stack sistem Donanımınıza yönelik sürücü ve bellenim güncelleştirmelerini içerir. OEM güncelleştirmelerini uygulama hakkında daha fazla bilgi için bkz. [Apply Azure Stack özgün ekipman üreticisi güncelleştirmeleri](azure-stack-update-oem.md)

### <a name="after-successfully-applying-the-1908-update"></a>1908 güncelleştirmesini başarıyla uyguladıktan sonra

Bu güncelleştirmenin yüklenmesinden sonra, geçerli düzeltmeleri yükleme. Daha fazla bilgi için bkz. [hizmet ilkeniz](azure-stack-servicing-policy.md).

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- 1908 için kullanılabilir düzeltme yok.
::: moniker-end

::: moniker range="azs-1907"
## <a name="1907-build-reference"></a>1907 derleme başvurusu

Azure Stack 1907 güncelleştirme yapı numarası **1.1907.0.20**.

### <a name="update-type-1907"></a>Güncelleştirme türü

Azure Stack 1907 güncelleştirme yapı türü **Express**'dir. Güncelleştirme oluşturma türleri hakkında daha fazla bilgi için [Azure Stack güncelleştirmeleri yönetme](azure-stack-updates.md) makalesine bakın. İç teste bağlı olarak, 1907 güncelleştirmesinin tamamlaması için gereken süre yaklaşık 13 saattir.

- Tam güncelleştirme çalışma zamanları, genellikle kiracı iş yükleri, sistem ağ bağlantınız (internet 'e bağlıysa) ve sistem donanım yapılandırmanız tarafından sisteminizde kullanılan kapasiteye bağlıdır.
- Beklenenden uzun süre içinde olan çalışma zamanları seyrek değildir ve güncelleştirme başarısız olmazsa Azure Stack işleçlere göre eylem gerektirmez.
- Bu çalışma zamanı yaklaşık 1907 güncelleştirmesine özeldir ve diğer Azure Stack güncelleştirmeleriyle karşılaştırılmamalıdır.

## <a name="whats-in-this-update-1907"></a>Bu güncelleştirmede neler vardır

<!-- The current theme (if any) of this release. -->

### <a name="whats-new-1907"></a>Yenilikler

<!-- What's new, also net new experiences and features. -->

- Tanılama günlüğü toplamayı kolaylaştırmak ve geliştirmek için Azure Stack tanılama günlüğü toplama hizmetinin genel kullanım sürümü. Azure Stack tanılama günlüğü toplama hizmeti, Microsoft Müşteri Destek Hizmetleri (CSS) ile tanılama günlüklerini toplayıp paylaşmak için basitleştirilmiş bir yöntem sağlar. Bu tanılama günlüğü toplama hizmeti, operatörlerin bir depolama blobuna belirli kritik uyarıların ne zaman yapıldığında otomatik olarak yüklenmesini veya aynı işlemi gerçekleştirmesini sağlayan Azure Stack yönetici portalında yeni bir kullanıcı deneyimi sağlar. isteğe bağlı işlem. Daha fazla bilgi için bkz. [tanılama günlüğü toplama](azure-stack-diagnostic-log-collection-overview.md) makalesi.

- Azure Stack doğrulama aracının **Test-AzureStack**'in bir parçası olarak Azure Stack ağ altyapısı doğrulamasının genel kullanım sürümü. Azure Stack ağ altyapısı, Azure Stack ağ altyapısında bir hata oluşup oluşmadığını belirlemek için **Test-AzureStack**'in bir parçası olacaktır. Test, yazılım tanımlı Azure Stack ağı atlayarak ağ altyapısının bağlantısını denetler. Ortak bir VIP 'den yapılandırılmış DNS ileticilerine, NTP sunucularına ve kimlik uç noktalarına olan bağlantıyı gösterir. Ayrıca, kimlik sağlayıcısı olarak Azure AD 'yi kullanırken veya ADFS kullanılırken Federasyon sunucusu ile Azure 'a bağlantıyı denetler. Daha fazla bilgi için [Azure Stack doğrulama aracı](azure-stack-diagnostic-test.md) makalesine bakın.

- Bir sistem güncelleştirmesi sırasında, iç SQL TLS sertifikalarını gereken şekilde döndürmek için bir iç gizli dizi döndürme yordamı eklenmiştir.

### <a name="improvements-1907"></a>Geliştirmeleri

<!-- Changes and product improvements with tangible customer-facing value. -->

- Azure Stack güncelleştirme dikey penceresinde artık etkin güncelleştirmeler için **son adım tamamlanmış** bir zaman görüntülenir. Bu, güncelleştirme dikey penceresine gidip çalışan bir güncelleştirmeye tıklanarak görülebilir. **Son adım tamamlandığında** **güncelleştirme çalıştırması ayrıntıları** bölümünde kullanılabilir.

- **Start-AzureStack** ve **stop-azurestack** işleç eylemlerine yönelik iyileştirmeler. Azure Stack başlama zamanı, ortalama% 50 oranında azaltılmıştır. Azure Stack kapatma süresi, ortalama% 30 oranında azaltılmıştır. Ortalama başlatma ve kapatılma süreleri, bir ölçek birimindeki düğümlerin sayısı arttıkça aynı kalır.

- Bağlantısı kesilen Market aracı için geliştirilmiş hata işleme. **Export-AzSOfflineMarketplaceItem**kullanılırken bir indirme başarısız olursa veya kısmen başarılı olursa, hata ve risk azaltma adımları hakkında daha fazla ayrıntı içeren ayrıntılı bir hata iletisi görüntülenir.

- Büyük bir sayfa blobundan/anlık görüntüsünden yönetilen disk oluşturma performansı geliştirilmiştir. Daha önce, büyük bir disk oluştururken bir zaman aşımı tetikledi.  

<!-- https://icm.ad.msft.net/imp/v3/incidents/details/127669774/home -->
- Beklenmedik sanal disk ayırmayı önlemek için bir düğümü kapatmadan önce sanal disk sistem durumu denetimi geliştirildi.

- Yönetici işlemlerine yönelik iç günlüklerin depolanması geliştirildi. Bu durum, iç günlük işlemlerinin bellek ve depolama tüketimini en aza indirerek yönetici işlemleri sırasında performansı ve güvenilirliği elde ediyor. Yönetici portalındaki güncelleştirme dikey penceresinin Gelişmiş sayfa yükleme sürelerini de fark edebilirsiniz. Bu iyileştirmesinin bir parçası olarak, 6 aydan eski olan güncelleştirme günlükleri sistemde artık kullanılamaz. Bu güncelleştirmeler için günlüklere ihtiyacınız varsa, 1907 güncelleştirmesini gerçekleştirmeden önce 6 aydan eski tüm güncelleştirme çalıştırmaları için [Özeti indirdiğinizden](azure-stack-apply-updates.md) emin olun.

### <a name="changes-1907"></a>Değişikliklerine

- Azure Stack sürüm 1907, operatörlere 1908 güncelleştirme yapmadan önce işletmenlerinin sistem OEM paketini 2,1 veya sonraki bir sürüme güncelleştirdiğinizden emin olan bir uyarı uyarısı içerir. Azure Stack OEM güncelleştirmelerinin nasıl uygulanacağı hakkında daha fazla bilgi için, bkz. [Azure Stack orijinal donanım üreticisi güncelleştirmesi uygulama](azure-stack-update-oem.md).

- Azure Stack tanılama günlüğü toplama hizmeti için iletişimi etkinleştirmek üzere yeni bir giden kuralı (HTTPS) eklendi. Daha fazla bilgi için bkz. [Azure Stack Datacenter Integration-son nokta yayımlama](azure-stack-integrate-endpoints.md#ports-and-urls-outbound).

- Altyapı yedekleme hizmeti artık dış depolama konumu kapasiteyi tükeniyorsa kısmen karşıya yüklenen yedeklemeleri siler.

- Altyapı yedeklemeleri artık etki alanı Hizmetleri verilerinin bir yedeklemesini içermez. Bu, yalnızca kimlik sağlayıcısı olarak Azure Active Directory kullanan sistemler için geçerlidir.

- Artık, **işlem > VM görüntüleri** dikey penceresine alınan bir görüntünün Sayfa Blobu türünde olduğunu doğrulayacağız.

### <a name="fixes-1907"></a>Düzeltilen

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

## <a name="security-updates-1907"></a>Güvenlik güncelleştirmeleri

Bu Azure Stack güncelleştirmesinde güvenlik güncelleştirmeleri hakkında daha fazla bilgi için bkz. [Azure Stack Security Updates](release-notes-security-updates.md).

## <a name="update-planning-1907"></a>Planlamayı Güncelleştir

Güncelleştirmeyi uygulamadan önce, aşağıdaki bilgileri gözden geçirdiğinizden emin olun:

- [Bilinen sorunlar](known-issues.md)
- [Güvenlik güncelleştirmeleri](release-notes-security-updates.md)
- [Güncelleştirme uygulandıktan önceki ve sonraki etkinliklerin denetim listesi](release-notes-checklist.md)

## <a name="download-the-update-1907"></a>Güncelleştirmeyi indir

Azure Stack 1907 güncelleştirme paketini [Azure Stack indirme sayfasından](https://aka.ms/azurestackupdatedownload)indirebilirsiniz.

## <a name="hotfixes-1907"></a>Başlatmayla

Düzeltmeleri düzenli aralıklarla yayınlar Azure Stack. Azure Stack 1907 ' e güncelleştirmeden önce 1906 için en son Azure Stack düzeltmesini yüklediğinizden emin olun.

Azure Stack düzeltmeler yalnızca Azure Stack tümleşik sistemler için geçerlidir; ASDK 'ye düzeltme yüklemeyi denemeyin.

### <a name="before-applying-the-1907-update"></a>1907 güncelleştirmesini uygulamadan önce

Azure Stack 1907 sürümü aşağıdaki düzeltmelere sahip 1906 sürümüne uygulanmalıdır:

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hotfix 1.1906.13.56](https://support.microsoft.com/help/4520375)

### <a name="after-successfully-applying-the-1907-update"></a>1907 güncelleştirmesini başarıyla uyguladıktan sonra

Bu güncelleştirmenin yüklenmesinden sonra, geçerli düzeltmeleri yükleme. Daha fazla bilgi için bkz. [hizmet ilkeniz](azure-stack-servicing-policy.md).

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hotfix 1.1907.17.54](https://support.microsoft.com/help/4523826)
::: moniker-end

::: moniker range="azs-1906"
## <a name="1906-build-reference"></a>1906 derleme başvurusu

Azure Stack 1906 güncelleştirme yapı numarası **1.1906.0.30**.

### <a name="update-type-1906"></a>Güncelleştirme türü

Azure Stack 1906 güncelleştirme yapı türü **Express**'dir. Güncelleştirme oluşturma türleri hakkında daha fazla bilgi için [Azure Stack güncelleştirmeleri yönetme](azure-stack-updates.md) makalesine bakın. 1906 güncelleştirmesinin tamamlaması için gereken süre, Azure Stack ortamınızdaki fiziksel düğümlerin sayısından bağımsız olarak yaklaşık 10 saattir. Tamamen güncelleştirme çalışma zamanları, genellikle kiracı iş yükleri, sistem ağ bağlantınız (internet 'e bağlıysa) ve sistem donanımınızın belirtimleri tarafından sisteminizde kullanılan kapasiteye bağlıdır. Beklenen değerden daha uzun süre içinde yer kalmayan çalışma zamanları, güncelleştirme başarısız olmadığı için Azure Stack işleçlere göre işlem gerektirmemektedir. Bu çalışma zamanı yaklaşık 1906 güncelleştirmesine özeldir ve diğer Azure Stack güncelleştirmeleriyle karşılaştırılmamalıdır.

## <a name="whats-in-this-update-1906"></a>Bu güncelleştirmede neler vardır

<!-- The current theme (if any) of this release. -->

<!-- What's new, also net new experiences and features. -->

- Tüm uç noktalarında TLS 1,2 zorlamak için ayrıcalıklı uç noktaya (PEP) bir **set-TLSPolicy** cmdlet 'i eklendi. Daha fazla bilgi için bkz. [Azure Stack güvenlik denetimleri](azure-stack-security-configuration.md).

- Uygulanan TLS ilkesini almak için ayrıcalıklı uç noktaya (PEP) bir **Get-TLSPolicy** cmdlet 'i eklendi. Daha fazla bilgi için bkz. [Azure Stack güvenlik denetimleri](azure-stack-security-configuration.md).

- Bir sistem güncelleştirmesi sırasında iç TLS sertifikalarını gereken şekilde döndürmek için bir iç gizli anahtar prosedürü eklendi.

- Süresi dolan gizli dizi uyarısı yok sayıldığında iç gizli dizileri zorlayarak iç Parolaların süresinin dolmasını önlemek için bir koruma eklendi. Bu, normal bir işletim yordamı olarak güvenilmemelidir. Gizli dizi yönü, bakım penceresi sırasında planlanmalıdır. Daha fazla bilgi için bkz. [Azure Stack gizli döndürme](azure-stack-rotate-secrets.md).

- Visual Studio Code artık AD FS kullanarak Azure Stack dağıtımı ile desteklenmektedir.

### <a name="improvements-1906"></a>Geliştirmeleri

<!-- Changes and product improvements with tangible customer-facing value. -->

- Ayrıcalıklı uç noktasındaki **Get-GraphApplication** cmdlet 'i artık kullanılmakta olan sertifikanın parmak izini görüntüler. Bu, Azure Stack AD FS dağıtıldığında hizmet sorumluları için sertifika yönetimini geliştirir.

- Uyarı verme özelliği de dahil olmak üzere AD grafiğinin ve AD FS kullanılabilirliğini doğrulamak için yeni sistem durumu izleme kuralları eklenmiştir.

- Altyapı yedekleme hizmeti başka bir örneğe taşırken, yedekleme kaynak sağlayıcısının güvenilirliğini artırmaya yönelik geliştirmeler.

- Bakım penceresinin planlanmasını kolaylaştırmak için Tekdüzen yürütme süresi sağlamak üzere dış gizli anahtar döndürme yordamının performans iyileştirmesi.

- **Test-AzureStack** cmdlet 'i artık sona ermek üzere olan iç gizli dizileri (kritik uyarılar) bildiriyor.

- AD FS için Federasyon güveni yapılandırılırken, sertifika iptal listesi denetimini atlamayı sağlayan ayrıcalıklı uç noktada **register-CustomAdfs** cmdlet 'i için yeni bir parametre mevcuttur.

- 1906 sürümü güncelleştirme ilerleme durumunun daha büyük bir görünürlüğünü sunarak güncelleştirmelerin duraklatılmadığından emin olabilirsiniz. Bu, **güncelleştirme** dikey penceresindeki işleçlere gösterilen güncelleştirme adımlarının toplam sayısının artmasına neden olur. Ayrıca, önceki güncelleştirmelerden daha fazla güncelleştirme adımı olduğunu fark edebilirsiniz.

#### <a name="networking-updates-1906"></a>Ağ güncelleştirmeleri

- DHCP Yanıtlayıcının Azure ile tutarlı olması için ayarlanan kira süresi güncelleştirildi.

- Kaynak sağlayıcıya, başarısız kaynakların dağıtımı senaryosunda geliştirilmiş yeniden deneme oranları.

- **Standart** SKU seçeneği şu anda desteklenmeyen şekilde hem yük dengeleyiciden hem de genel IP 'den kaldırılmıştır.

### <a name="changes-1906"></a>Değişikliklerine

- Depolama hesabı deneyimi oluşturmak artık Azure ile tutarlıdır.

- İç gizli dizi süresi sonu için uyarı Tetikleyicileri değiştirildi:
  - Uyarı uyarıları artık gizli dizi süresi dolmadan 90 gün önce tetiklenir.
  - Kritik uyarılar artık gizli dizileri sona ermeden önce 30 gün sonra tetiklenir.

- Tutarlı terminoloji için altyapı yedekleme kaynak sağlayıcısındaki dizeler güncelleştirildi.

### <a name="fixes-1906"></a>Düzeltilen

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

## <a name="security-updates-1906"></a>Güvenlik güncelleştirmeleri

Bu Azure Stack güncelleştirmesinde güvenlik güncelleştirmeleri hakkında daha fazla bilgi için bkz. [Azure Stack Security Updates](release-notes-security-updates.md).

## <a name="update-planning-1906"></a>Planlamayı Güncelleştir

Güncelleştirmeyi uygulamadan önce, aşağıdaki bilgileri gözden geçirdiğinizden emin olun:

- [Bilinen sorunlar](known-issues.md)
- [Güvenlik güncelleştirmeleri](release-notes-security-updates.md)
- [Güncelleştirme uygulandıktan önceki ve sonraki etkinliklerin denetim listesi](release-notes-checklist.md)

## <a name="download-the-update-1906"></a>Güncelleştirmeyi indir

Azure Stack 1906 güncelleştirme paketini [Azure Stack indirme sayfasından](https://aka.ms/azurestackupdatedownload)indirebilirsiniz.

## <a name="hotfixes-1906"></a>Başlatmayla

Düzeltmeleri düzenli aralıklarla yayınlar Azure Stack. Azure Stack 1906 ' e güncelleştirmeden önce 1905 için en son Azure Stack düzeltmesini yüklediğinizden emin olun. Güncelleştirme sonrasında, [1906 için kullanılabilir düzeltmeleri](#after-successfully-applying-the-1906-update)yüklersiniz.

Azure Stack düzeltmeler yalnızca Azure Stack tümleşik sistemler için geçerlidir; ASDK 'ye düzeltme yüklemeyi denemeyin.

### <a name="before-applying-the-1906-update"></a>1906 güncelleştirmesini uygulamadan önce

Azure Stack 1906 sürümü aşağıdaki düzeltmelere sahip 1905 sürümüne uygulanmalıdır:

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hotfix 1.1905.3.48](https://support.microsoft.com/help/4510078)

### <a name="after-successfully-applying-the-1906-update"></a>1906 güncelleştirmesini başarıyla uyguladıktan sonra

Bu güncelleştirmenin yüklenmesinden sonra, geçerli düzeltmeleri yükleme. Daha fazla bilgi için bkz. [hizmet ilkeniz](azure-stack-servicing-policy.md).

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hotfix 1.1906.13.56](https://support.microsoft.com/help/4520375)
::: moniker-end

::: moniker range="azs-1905"
## <a name="1905-build-reference"></a>1905 derleme başvurusu

Azure Stack 1905 güncelleştirme yapı numarası **1.1905.0.40**.

### <a name="update-type-1905"></a>Güncelleştirme türü

Azure Stack 1905 güncelleştirme yapı türü **dolu**. Sonuç olarak, 1905 güncelleştirmesinin 1903 ve 1904 gibi hızlı güncelleştirmelerden daha uzun bir çalışma zamanı vardır. Tam güncelleştirmeler için tam çalışma zamanları genellikle Azure Stack örneğinizin içerdiği düğüm sayısına, kiracı iş yükleri tarafından sisteminizde kullanılan kapasiteye, sisteminizin ağ bağlantısına (internet 'e bağlıysa) ve sistem donanımınıza bağlıdır yapılandırmada. 1905 güncelleştirmesinde, iç sınamamız içinde aşağıdaki beklenen çalışma zamanları vardı: 4 düğüm-35 saat, 8 düğüm-45 saat, 12 düğüm-55 saat, 16 düğüm-70 saat. 1905 çalışma zamanı, bu beklenen değerlerden daha uzun bir süre daha seyrek değildir ve güncelleştirme başarısız olmazsa Azure Stack işleçlere göre eylem gerektirmez. Güncelleştirme oluşturma türleri hakkında daha fazla bilgi için bkz. [Azure Stack güncelleştirmeleri yönetme](azure-stack-updates.md).

## <a name="whats-in-this-update-1905"></a>Bu güncelleştirmede neler vardır

<!-- The current theme (if any) of this release. -->

<!-- What's new, also net new experiences and features. -->

- Bu güncelleştirmeyle, Azure Stack 'deki güncelleştirme altyapısı, ölçek birimi düğümlerinin bellenimini güncelleştirebilir. Bu, donanım iş ortaklarından uyumlu bir güncelleştirme paketi gerektirir. Kullanılabilirlik hakkındaki ayrıntılar için donanım iş ortağınıza ulaşın.

- Windows Server 2019 artık desteklenmektedir ve Azure Stack Marketi aracılığıyla genel olarak kullanılabilir.
Bu güncelleştirmeyle, Windows Server 2019 artık 2016 ana bilgisayarında başarıyla etkinleştirilebilir.

- Yeni bir [Azure hesap Visual Studio Code uzantısı](../user/azure-stack-dev-start-vscode-azure.md) , geliştiricilerin oturum açarak ve abonelikleri görüntüleyerek Azure Stack hedeflemesini sağlar ve birçok farklı hizmeti de görebilir. Azure Hesap uzantısı hem Azure Active Directory (Azure AD) hem de AD FS ortamlarında çalışarak, yalnızca Visual Studio Code Kullanıcı ayarlarında küçük bir değişiklik gerektirir. Visual Studio Code, bu ortamda çalıştırılmak üzere bir hizmet sorumlusuna izin verilmesini gerektirir. Bunu yapmak için kimlik betiğini içeri aktarın ve [Azure Stack Içinde çok kiracılı](../operator/azure-stack-enable-multitenancy.md)olarak belirtilen cmdlet 'leri çalıştırın. Bu, giriş dizini için bir güncelleştirme ve her bir dizin için konuk kiracı dizini kaydı gerektirir. Visual Studio Code hizmet sorumlusu 'nın dahil olduğu ana dizin kiracısını güncelleştirmek için 1905 veya sonraki bir sürüme güncelleştirildikten sonra bir uyarı görüntülenir. 

### <a name="improvements-1905"></a>Geliştirmeleri

<!-- Changes and product improvements with tangible customer-facing value. -->
- Azure Stack TLS 1,2 zorlama kapsamında, aşağıdaki uzantılar bu sürümlere güncelleştirilmiştir:

  - Microsoft. customscriptextension-ARM-1.9.3
  - Microsoft. ıaasdiagnostics-1.12.2.2
  - Microsoft. antimalware-Windows-ARM-1.5.5.9
  - Microsoft. DSC-ARM-2.77.0.0
  - microsoft.vmaccessforlinux-1.5.2

  Daha sonraki bir sürümde TLS 1,2 zorlandığında, uzantının yeni dağıtımlarının başarısız olması için lütfen uzantıların bu sürümlerini hemen indirin. Uzantılara yönelik alt sürüm güncelleştirmelerinin (örneğin, 1,8-1,9) otomatik olarak gerçekleştirilmesi için her zaman **otomatik yükseltme MinorVersion = true** olarak ayarlayın.

- Azure Stack portalında yeni bir **Yardım ve desteğe genel bakış** , operatörlerin destek seçeneklerini denetlemesini, uzman yardım almak ve Azure Stack hakkında daha fazla bilgi almasını kolaylaştırır. Tümleşik sistemlerde, destek isteği oluşturmak Azure Stack hizmeti ön seçmeyecektir. Müşterilerin küresel Azure portal kullanmak yerine bilet göndermek için bu deneyimi kullanmalarını önemle tavsiye ederiz. Daha fazla bilgi için bkz. [Azure Stack yardım ve destek](azure-stack-help-and-support-overview.md).

- Birden çok Azure Active Directory eklendi olduğunda ( [Bu işlem](azure-stack-enable-multitenancy.md)aracılığıyla), belirli güncelleştirmeler gerçekleştiğinde betiği yeniden çalıştırmak veya Azure AD hizmet sorumlusu yetkilendirmede yapılan değişiklikler hakların eksik olmasına neden olması mümkündür. Bu, belirli özellikler için engellenmiş erişimlerden, özgün soruna geri dönmek zor olan daha fazla ayrık hatalara neden olabilir. Bu sorunu engellemek için 1905, bu izinleri denetleyen yeni bir özellik sunar ve belirli yapılandırma sorunları bulunduğunda bir uyarı oluşturur. Bu doğrulama her saat çalışır ve sorunu çözmesi için gereken düzeltme eylemlerini görüntüler. Tüm kiracılar sağlıklı durumdaysa uyarı kapanır.

- Hizmet yük devretmesi sırasında altyapı Yedekleme işlemlerinin güvenilirliği geliştirildi.

- Kimlik doğrulaması için [Azure Active Directory kimlik doğrulama kitaplıklarını](/azure/active-directory/develop/active-directory-authentication-libraries) (ADAL) kullanan [Azure Stack Nagios eklentisinin](azure-stack-integrate-monitor.md#integrate-with-nagios) yeni bir sürümü kullanılabilir. Eklenti artık Azure AD ve Azure Stack Active Directory Federasyon Hizmetleri (AD FS) (AD FS) dağıtımlarını da destekliyor. Daha fazla bilgi için bkz. [Nagios eklentisi Exchange](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details) sitesi.

- Azure Stack yeni bir karma profil **2019-03-01-karma** yayımlandı ve bu, tüm en son özellikleri destekler. Hem Azure PowerShell hem de Azure CLı **2019-03-01 karma** profilini destekler. .NET, Ruby, Node. js, Go ve Python SDK 'larının **2019-03-01 karma** profilini destekleyen yayımlanmış paketleri vardır. İlgili belgeler ve bazı örnekler değişiklikleri yansıtacak şekilde güncelleştirilmiştir.

- [Node. js SDK](https://www.npmjs.com/search?q=2019-03-01-hybrid) 'Sı artık API profillerini destekliyor. **2019-03-01 karma** profili destekleyen paketler yayımlanır.

- 1905 Azure Stack güncelleştirmesi platform güvenilirliğini ve desteklenebilirliğini artırmak için iki yeni altyapı rolü ekler:

  - **Altyapı halkası**: Daha sonra altyapı halkası, mevcut altyapı rollerinin Kapsayıcılı sürümlerini (örneğin, şu anda kendi belirlenmiş altyapı VM 'lerini) barındıracaktır. Bu, platform güvenilirliğini artırır ve Azure Stack gereken altyapı VM 'lerinin sayısını azaltır. Bu, daha sonra Azure Stack altyapı rollerinin genel kaynak tüketimini azaltır.
  - **Destek halkası**: Gelecekte, destek halkası müşterilere yönelik gelişmiş destek senaryolarını işlemek için kullanılacaktır.  

  Ayrıca, bu rolün daha iyi kullanılabilirliği için etki alanı denetleyicisi VM 'sinin ek bir örneğini ekledik.

  Bu değişiklikler Azure Stack altyapısının kaynak tüketimini aşağıdaki yollarla artırır:
  
    | Azure Stack SKU 'SU | Işlem tüketiminde artış | Bellek tüketiminde artış |
    | -- | -- | -- |
    |4 düğüm|22 vCPU|28 GB|
    |8 düğüm|38 vCPU|44 GB|
    |12 düğüm|54 vCPU|60 GB|
    |16 düğüm|70 vCPU|76 GB|
  
### <a name="changes-1905"></a>Değişikliklerine

- Planlı ve plansız bakım senaryolarında güvenilirliği ve kullanılabilirliği artırmak için, Azure Stack etki alanı Hizmetleri için ek bir altyapı rol örneği ekler.

- Bu güncelleştirme ile, onarım ve düğüm ekleme işlemleri sırasında donanım, ölçek birimi içindeki homoölçek birim düğümlerini sağlamak üzere onaylanır.

- Zamanlanan yedeklemeler tamamlanmazsa ve tanımlanan saklama süresi aşılırsa, altyapı yedekleme denetleyicisi en az bir başarılı yedeklemenin korunduğundan emin olur. 

### <a name="fixes-1905"></a>Düzeltilen

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there is an SR/ICM associated to it. -->

- Ölçek biriminde bir düğüm yeniden başlatıldıktan sonra bir **işlem Konağı Aracısı** uyarısının görünmediği bir sorun düzeltildi.

- Yönetici portalında Market yönetiminde, filtreler uygulanırken hatalı sonuçlar gösterildiğinde ve yayımcı filtresinde yinelenen yayımcı adları gösterilen sorunlar düzeltildi. Ayrıca, sonuçları daha hızlı göstermek için performans iyileştirmeleri de yapılmıştır.

- Kullanılabilir yedekleme dikey penceresinde, dış depolama konumuna karşıya yükleme tamamlanmadan önce yeni bir yedekleme işlemi listelenmiş olan sorun düzeltildi. Artık kullanılabilir yedekleme, depolama konumuna başarıyla yüklendikten sonra listede görünür. 

<!-- ICM: 114819337; Task: 4408136 -->
- Yedekleme işlemi sırasında kurtarma anahtarları alınırken sorun düzeltildi. 

<!-- Bug: 4525587 -->
- İşleç portalında sürümü ' tanımsız ' olarak gösteren OEM güncelleştirme ile ilgili sorun düzeltildi.

### <a name="security-updates-1905"></a>Güvenlik güncelleştirmeleri

Bu Azure Stack güncelleştirmesinde güvenlik güncelleştirmeleri hakkında daha fazla bilgi için bkz. [Azure Stack Security Updates](release-notes-security-updates.md).

## <a name="update-planning-1905"></a>Planlamayı Güncelleştir

Güncelleştirmeyi uygulamadan önce, aşağıdaki bilgileri gözden geçirdiğinizden emin olun:

- [Bilinen sorunlar](known-issues.md)
- [Güvenlik güncelleştirmeleri](release-notes-security-updates.md)
- [Güncelleştirme uygulandıktan önceki ve sonraki etkinliklerin denetim listesi](release-notes-checklist.md)

## <a name="download-the-update-1905"></a>Güncelleştirmeyi indir

Azure Stack 1905 güncelleştirme paketini [Azure Stack indirme sayfasından](https://aka.ms/azurestackupdatedownload)indirebilirsiniz. Yükleyici aracını kullanırken, karşıdan yüklemeler dizininizden önbelleğe alınmış bir kopya değil, en son sürümü kullandığınızdan emin olun.

## <a name="hotfixes-1905"></a>Başlatmayla

Düzeltmeleri düzenli aralıklarla yayınlar Azure Stack. Azure Stack 1905 ' e güncelleştirmeden önce 1904 için en son Azure Stack düzeltmesini yüklediğinizden emin olun.

Azure Stack düzeltmeler yalnızca Azure Stack tümleşik sistemler için geçerlidir; ASDK 'ye düzeltme yüklemeyi denemeyin.

### <a name="before-applying-the-1905-update"></a>1905 güncelleştirmesini uygulamadan önce

Azure Stack 1905 sürümü aşağıdaki düzeltmelere sahip 1904 sürümüne uygulanmalıdır:

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hotfix 1.1904.4.45](https://support.microsoft.com/help/4505688)

### <a name="after-successfully-applying-the-1905-update"></a>1905 güncelleştirmesini başarıyla uyguladıktan sonra

Bu güncelleştirmenin yüklenmesinden sonra, geçerli düzeltmeleri yükleme. Daha fazla bilgi için bkz. [hizmet ilkeniz](azure-stack-servicing-policy.md).

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hotfix 1.1905.3.48](https://support.microsoft.com/help/4510078)
::: moniker-end

::: moniker range=">=azs-1905"
## <a name="automatic-update-notifications"></a>Otomatik güncelleştirme bildirimleri

Altyapı ağından internet 'e erişebilen sistemler, operatör portalındaki **güncelleştirme kullanılabilir** iletisini görür. İnternet erişimi olmayan sistemler. zip dosyasını indirebilir ve ilgili. xml ile içe aktarabilir.

> [!TIP]  
> Azure Stack düzeltmelere devam etmek için aşağıdaki *RSS* veya *atom* akışlarına abone olun:
>
> - ['YI](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss)
> - [Atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom)

## <a name="archive"></a>Arşiv

Daha eski bir sürüm için arşivlenmiş sürüm notlarına erişmek için, sol taraftaki içindekiler tablosunun üzerindeki sürüm Seçicisi açılan listesini kullanın ve görmek istediğiniz sürümü seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Stack'te güncelleştirme yönetimi genel bakış için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md).  
- Azure Stack güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için bkz. [güncelleştirmelerini Azure Stack'te](azure-stack-apply-updates.md).
- Azure Stack tümleşik sistemleri ve desteklenen bir duruma sisteminizi tutmak için yapmanız gerekenlere bakım ilkeyi gözden geçirmek için bkz: [Azure Stack hizmet İlkesi](azure-stack-servicing-policy.md).  
- Ayrıcalıklı uç noktasına (CESARETLENDİRİCİ) izlemek ve güncelleştirmelerini sürdürmek üzere kullanmak için bkz [izleme ayrıcalıklı uç noktayı kullanarak Azure stack'teki güncelleştirmeleri](azure-stack-monitor-update.md).
::: moniker-end

<!------------------------------------------------------------>
<!------------------- UNSUPPORTED VERSIONS ------------------->
<!------------------------------------------------------------>
::: moniker range="azs-1904"
## <a name="1904-archived-release-notes"></a>1904 arşivlenmiş sürüm notları
::: moniker-end
::: moniker range="azs-1903"
## <a name="1903-archived-release-notes"></a>1903 arşivlenmiş sürüm notları
::: moniker-end
::: moniker range="azs-1902"
## <a name="1902-archived-release-notes"></a>1902 arşivlenmiş sürüm notları
::: moniker-end
::: moniker range="azs-1901"
## <a name="1901-archived-release-notes"></a>1901 arşivlenmiş sürüm notları
::: moniker-end
::: moniker range="azs-1811"
## <a name="1811-archived-release-notes"></a>1811 arşivlenmiş sürüm notları
::: moniker-end
::: moniker range="azs-1809"
## <a name="1809-archived-release-notes"></a>1809 arşivlenmiş sürüm notları
::: moniker-end
::: moniker range="azs-1808"
## <a name="1808-archived-release-notes"></a>1808 arşivlenmiş sürüm notları
::: moniker-end
::: moniker range="azs-1807"
## <a name="1807-archived-release-notes"></a>1807 arşivlenmiş sürüm notları
::: moniker-end
::: moniker range="azs-1805"
## <a name="1805-archived-release-notes"></a>1805 arşivlenmiş sürüm notları
::: moniker-end
::: moniker range="azs-1804"
## <a name="1804-archived-release-notes"></a>1804 arşivlenmiş sürüm notları
::: moniker-end
::: moniker range="azs-1803"
## <a name="1803-archived-release-notes"></a>1803 arşivlenmiş sürüm notları
::: moniker-end
::: moniker range="azs-1802"
## <a name="1802-archived-release-notes"></a>1802 arşivlenmiş sürüm notları
::: moniker-end

::: moniker range="<azs-1905"
[Azure Stack sürüm notlarının eski sürümlerine TechNet Gallery ' de](https://aka.ms/azsarchivedrelnotes)erişebilirsiniz. Bu arşivlenmiş belgeler yalnızca başvuru amacıyla sağlanır ve bu sürümler için destek göstermez. Azure Stack desteği hakkında bilgi için bkz. [Azure Stack hizmet ilkesi](azure-stack-servicing-policy.md). Daha fazla yardım için Microsoft Müşteri Destek Hizmetleri 'ne başvurun.
::: moniker-end


