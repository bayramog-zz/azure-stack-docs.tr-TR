---
title: Azure Stack 1904 sürüm notları | Microsoft Docs
description: Bilinen sorunlar ve arşivlenmiş Azure Stack sürüm notları ve güncelleştirme karşıdan yükleme konumu yenilikler dahil olmak üzere, Azure Stack tümleşik sistemleri 1904 güncelleştirmesi hakkında bilgi edinin.
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
ms.date: 05/30/2019
ms.author: sethm
ms.reviewer: ''
ms.lastreviewed: 05/30/2019
ms.openlocfilehash: 770d3bc462750816e55f1c224496d9dde01f5a36
ms.sourcegitcommit: 4e0b450c91c6515794b663a39f9a4b8b49999918
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66411623"
---
# <a name="azure-stack-1904-update"></a>Azure Stack 1904 güncelleştirme

*Uygulama hedefi: Azure Stack tümleşik sistemleri*

Bu makalede 1904 güncelleştirme paketinin içeriğini açıklar. Yenilikler güncelleştirme içerir. iyileştirmeler ve düzeltmeler bu sürümü, Azure Stack için. Bu makalede, aşağıdaki bilgileri içerir:

- [Yeniliklerin açıklaması, geliştirmeleri, düzeltmeler ve güvenlik güncelleştirmeleri](azure-stack-release-notes-1904.md#whats-in-this-update)
- [Planlama güncelleştir](azure-stack-release-notes-1904.md#update-planning)

> [!IMPORTANT]  
> Yalnızca Azure Stack tümleşik sistemleri için bu güncelleştirme paketidir. Bu güncelleştirme paketi için Azure Stack geliştirme Seti'ni geçerli değildir.

## <a name="archived-release-notes"></a>Arşivlenmiş sürüm notları

Gördüğünüz [Azure Stack eski sürümlerinin sürüm notları TechNet Galerisi'ndeki](http://aka.ms/azsarchivedrelnotes). Bu sürüm notları yalnızca başvuru amacıyla sağlanmıştır ve bu sürümleri için destek anlamına gelmediğini arşivlenir. Daha fazla yardım için Microsoft Müşteri Destek Hizmetleri'ne başvurun.

## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack 1904 güncelleştirmenin yapı numarasıdır **1.1904.0.36**.

## <a name="whats-in-this-update"></a>Bu güncelleştirmede nedir

<!-- The current theme (if any) of this release. -->

<!-- What's new, also net new experiences and features. -->

### <a name="improvements"></a>Geliştirmeleri

<!-- Changes and product improvements with tangible customer-facing value. -->

- 1904 güncelleştirme yükü temel işletim sistemini barındıran Azure Stack içermeyen bileşenleri Azure Stack için bir güncelleştirme içerir. Bu, Kapsamınızın belirli güncelleştirmeler sağlar. Sonuç olarak, beklenen bu alır 1904 güncelleştirmesinin tamamlanması daha az zaman (yaklaşık 16 saat, ancak tam süreleri değişebilir). Çalışma zamanı bu azalma 1903 güncelleştirmeye benzerdir. Ancak, sonraki güncelleştirmeler işletim sistemi farklı çalışma zamanlarını olduğunu belirtmek için güncelleştirmeleri içerebilir. Gelecekteki güncelleştirmelerin güncelleştirmenin tamamlanması dahil yüküne bağlı olarak beklenen süresini üzerinde benzer yönergeler sağlar.

- Önemli geliştirmeler, yazılım tanımlı ağ (SDN) yığına 1904 yapılmıştır. Bu geliştirmeler, genel bakım ve Azure Stack'te SDN yığınını güvenilirliğini artırın.

- Bir bildirim Yönetici portalı'nda eklenmesi şu anda oturum açmış kullanıcının düzgün bir şekilde yüklemek bir Pano sağlayan gerekli izinleri yok. Ayrıca, hangi hesapların dağıtımı sırasında kullanılan kimlik sağlayıcısına bağlı olarak uygun izinlere sahip açıklayan belgeler için bir bağlantı içerir.

- VM dayanıklılığını ve tüm sanal makineler sanal makine yapılandırma dosyalarını içeren depolama birimi çevrimdışı olursa Çevrimdışı Git senaryo çözümler çalışma süresi geliştirmeleri eklendi.

<!-- 1901,2,3 related hotfix -->
- Sanal makine sayısı için eklenen iyileştirme eşzamanlı olarak evacuated ve adresi VM kısmi ya da ağ ağır yük altında değilse blackouts tüketilen bant genişliği üst sınır yerleştirildiği. Bu değişiklik, bir sistem güncelleştirirken VM'nin çalışma süresine artırır.

<!-- 1901,2,3 related hotfix -->
- Uygun ölçekte başarısız işlemler Portalı'nda bunun sonucunda, platform kaynakları tüketme iç işlemlere karşı korumak için bir sistem çalışırken geliştirilmiş kaynak azaltma.

- Geliştirilmiş filtreleme yetenekleri aynı anda birden fazla filtre uygulamak olanak tanır. Yalnızca üzerinde sıralayabilir **adı** yeni kullanıcı arabiriminde sütun.

- Tekliflerini, planları, kotalar ve abonelikleri silme işlemi iyileştirmeleri. Silmek istediğiniz nesne hiçbir bağımlılıkları varsa, artık başarıyla teklifleri, kotalar, planlar ve abonelikleri Yönetici portalı'ndan silebilirsiniz. Daha fazla bilgi için [bu makaleye](azure-stack-delete-offer.md) bakın.  

<!-- this applies to bug 3725384 and bug #4225643 -->
- Syslog ileti hacmi gereksiz olayları da filtreleme ve yönlendirilmiş iletiler için istediğiniz önem düzeyi seçmek için bir yapılandırma parametresi sağlayarak geliştirildi. Önem düzeyi yapılandırma hakkında daha fazla bilgi için bkz. [Azure Stack'i veri merkezi tümleştirmesi - syslog iletmeyi](azure-stack-integrate-security.md).

- Azure Stack altyapısının ek 12 GB kullanır + (4 GB * Azure Stack numarası konakları) 1904 güncelleştirmek ve sonraki sürümler. Bu bir 4 düğümlü damgası olacağına dair bir ek kapasite tüketimini 28 GB anlamına gelir (12 GB + 4 GB * 4) Azure Stack Yönetici portalı'nın kapasitesi ekranında yansıtılır. Ek bellek tüketimini Azure Stack damganız kapasite aşımı koyar bile 1904 sürümüne güncelleştirmenizi başarılı olması gerekir. Güncelleştirme tamamlandıktan sonra Azure Stack damganız göre bellek kullanımı, bazı VM'ler ayırmayı iptal etmek için düzeltme adımları ile bu durumunu yansıtan bir uyarı görürsünüz.

<!--this applied to Bug 1473487 -->
- Yeni bir özellik için eklenen **Get-AzureStackLog** ek bir parametre ekleme tarafından cmdlet'i `-OutputSASUri`. Artık ortamınızı Azure Stack günlükleri toplayıp bunları belirtilen Azure depolama blob kapsayıcısında yer depolar. Daha fazla bilgi için [Azure Stack'te tanılama](azure-stack-diagnostics.md#examples).

- Yeni bellek onay işareti eklenen **Test AzureStack** `UpdateReadiness` grubu yığında güncelleştirmesinin başarıyla tamamlamak yeterli bellek olup olmadığını denetler.

<!-- Bug/Task 4311058 -->
- Yapılan geliştirmeler **Test AzureStack** Service Fabric sistem durumunu değerlendirme.

<!-- feature: 2976966 -->
- Sürücü bellenimini tamamlamak için gereken süreyi azaltır donanım güncelleştirmeler, geliştirmeler, 2-4 saat olarak güncelleştirin. Güncelleştirme altyapısı, dinamik olarak bağlı olarak paketteki içerik yürütmek hangi bölümlerinin güncelleştirmesi gerekir belirler.

<!-- Feature 3906611 -->
- Kullanılabilirliği etkileyen kesintiye uğratan altyapı rol örneği işlemleri önlemek için eklenen sağlam işlemi ön denetimler.

<!-- Feature 3780326 -->
- Altyapı yedekleme eylem planı Teklik iyileştirmeleri.

<!--Bug/Task 3139609 -->
- Azure Stack günlük toplama iyileştirmeleri. Bu geliştirmeler, günlükleri kümesini almak için gereken süreyi azaltın. Ayrıca, [Get-AzureStackLog](azure-stack-diagnostics.md#run-get-azurestacklog-on-an-azure-stack-development-kit-asdk-system) cmdlet artık OEM rolü için varsayılan günlükleri oluşturur. Yürütülmesi gereken [Invoke-AzureStackOnDemandLog](azure-stack-diagnostics.md#invoke-azurestackondemandlog) cmdlet'i, OEM günlükleri almak için rol belirtme. Daha fazla bilgi için [Azure Stack'te tanılama](azure-stack-diagnostics.md#run-get-azurestacklog-on-an-azure-stack-development-kit-asdk-system).

- Azure Stack artık ADFS ile veri merkezi tümleştirmesi için sağlanan Federasyon veri URL'si izler. Bu, müşteri ADFS örneğinin veya grup gizli dönüşü sırasında güvenilirliğini artırır.

### <a name="changes"></a>Değişiklikler

<!-- Feature 3906611 -->
- Yönetici portalını altyapı rol örneğini kapatmak, Azure Stack operatörleri için seçeneğini kaldırdık. Yeniden başlatma işlevleri altyapı rol örneği yeniden başlatmadan önce bir temiz kapatma girişimi sağlar. Gelişmiş senaryolar için API ve PowerShell işlevselliği kullanılabilir kalır.

<!-- Feature ## 4199257 -->
- Market görüntülerini ve kaynak sağlayıcıları için ayrı ekranlara ile yeni bir Market yönetim deneyimi vardır. Şimdilik **kaynak sağlayıcıları** penceresi boşsa, ancak gelecekteki sürümlerde yeni PaaS tekliflerini görünür ve içinde yönetilen **kaynak sağlayıcıları** penceresi.

<!-- Feature ## 4199257 -->
- Güncelleştirme deneyimini işleci Portalı'nda yapılan değişiklikler. Kaynak sağlayıcı güncelleştirmeleri için yeni bir kılavuz vardır. Kaynak sağlayıcıları güncelleştirilmesini sağlayan özelliği henüz kullanılamıyor.

<!-- Task ## 3748423  -->
- Güncelleştirme yükleme deneyiminde işleci Portalı'nda yapılan değişiklikler. Azure yığını işleçleri yanıt uygun bir güncelleştirme sorunu gidermek için portal otomatik olarak çalıştırarak türetilmiş gibi daha belirli önerileri, Ölçek birimi Durumu'na göre artık sağlar **Test AzureStack** ve ayrıştırma sonuçları. Sonuca bağlı, işleci iki işlemlerden birini yapmanız için size bildirir:

  - "Soft" uyarı "en son güncelleştirme ilgilenilmesi. okuyan portalda görüntülenir Microsoft, normal iş saatleri sırasında bir hizmet isteği açma önerir. Güncelleştirme işleminin bir parçası olarak, Test AzureStack gerçekleştirilen tabanlı ve çıktı size en uygun olan uyarıyı oluşturur. Bu durumda, Test AzureStack geçirildi."

  - "Sabit" Kritik Uyarı okur, "en son güncelleştirme başarısız oldu. portalda görüntülenir. Microsoft, hizmet isteğini olabildiğince çabuk açma önerir. Güncelleştirme işleminin bir parçası olarak, Test AzureStack gerçekleştirilen tabanlı ve çıktı size en uygun olan uyarıyı oluşturur. Bu durumda, Test AzureStack de başarısız oldu."

- Güncelleştirilmiş Azure Linux Aracısı sürümü 2.2.38.0. Bu destek, müşterilerin Azure ve Azure Stack arasında tutarlı Linux görüntüleri tutmak olanak tanır.

- Bu güncelleştirmede yapılan değişiklikler işleci Portalı'nda günlüğe kaydeder. Başarılı güncelleştirme günlükleri almak için istekleri artık kullanılamaz. Tanılama için eyleme dönüştürülebilir olduğundan başarısız güncelleştirme günlükleri için yükleme yine kullanılabilir durumdadır.

### <a name="fixes"></a>Düzeltmeleri

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there is an SR/ICM associated to it. -->

- Hangi syslog yapılandırmasını güncelleştirme döngüsü boyunca, syslog istemci yapılandırmasını ve iletilen durdurmak için syslog iletileri kaybetmenize neden trvalý değil bir sorun düzeltildi. Syslog yapılandırmasını şimdi korunur.

- Vm'leri ayırmayı kaldırma engellenen CRP bir sorun düzeltildi. Bir VM büyük birden fazla yönetilen disk içeriyorsa, daha önce VM serbest bırakılıyor bir zaman aşımı hatası ile başarısız olmuş olabilir.

- Windows Defender altyapısıyla ölçek birimi depolama erişimi etkileyen bir sorun düzeltildi.

- Blob depolama hesapları için erişim ilkesi penceresine yüklenemedi, bir kullanıcı portalı bir sorun düzeltildi.

- Hatalı bildirimler hakkında genel Azure portalında görüntülendiği hem yönetici hem de Kullanıcı Portalı içinde bir sorun düzeltildi.

- Hangi seçme içinde bir kullanıcı portalı sorun düzeltildi **geri bildirim** kutucuk neden bir boş tarayıcı sekmesini açın.

- Hangi ağa bağlanan bir IP yapılandırması için statik bir IP adresi değiştirmekle bağdaştırıcısı bir VM örneğine iliştirilmiş neden görüntülenecek hata iletisi portal bir sorun düzeltildi.

- Hangi çalışırken, bir kullanıcı portalı sorun düzeltildi **ağ arabirimi ekleme** var olan bir sanal makineye **ağ** penceresi işlemi bir hata iletisiyle başarısız olmasına neden oldu.

- Hangi Azure Stack 4'ten fazla ağ arabirimlerini (NIC'ler) bir sanal makine örneğine eklenmesini desteklememektedir bir sorun düzeltildi.

- Bir gelen güvenlik kuralı ekleme ve seçerek bir portal sorun düzeltildi **hizmet etiketi** kaynak olarak Azure Stack için kullanılabilir olmayan çeşitli seçenekler görüntülenir.

- Hangi ağ güvenlik grupları (Nsg'ler) Azure Stack'te genel Azure aynı şekilde çalışmaması sorunu düzeltildi.

- Kayıt süresi dolana veya sözleşme kaldırılır, indirilen tüm ürünleri gizler Market Yönetimi'nde, bir sorun düzeltildi.

- Hangi veren bir sorunu düzeltildi bir **Set-AzureRmVirtualNetworkGatewayConnection** var olan bir sanal ağ geçidi bağlantısı için PowerShell komutu başarısız oldu, hata iletisiyle **geçersiz paylaşılan anahtarı yapılandırılmış ...** .

- İstenen kaynağa bunun sonucunda, Ağ denetleyicisi ile eşitlenmemiş olması için ağ kaynak sağlayıcısı (NRP) kaynaklanan bir sorun düzeltildi. Bazı durumlarda, bir hata durumunda üst kaynak bırakarak içinde sonuçlandı.

- Okuma okuma izinleri, bir aboneliğe katkıda bulunan rolü atandı, ancak açıkça verilmemiş bir kullanıcı, bir hata oluşturulduğu bir sorun düzeltildi **... İstemci 'somelogonaccount@domain.com' nesne kimliği {GUID} eylemi gerçekleştirme yetkisi yok...**  bir kaynağa bir değişikliği kaydetmek çalışırken.

- İçinde Market yönetim ekran çevrimdışı Dağıtım Aracı'nı, görüntüleri karşıya yüklemek için kullanılan ve bunları herhangi biri URI(s) simgesi eksik, boş bir sorun düzeltildi.

- Market yönetiminde silinmiş indirilemedi ürünleri engelleyen bir sorun düzeltildi.

### <a name="security-updates"></a>Güvenlik güncelleştirmeleri

Bu güncelleştirme, Azure Stack, Azure Stack barındıran temel işletim sistemi için güvenlik güncelleştirmeleri içermez. Bilgi için [Azure Stack güvenlik güncelleştirmeleri.](azure-stack-release-notes-security-updates-1904.md)

## <a name="update-planning"></a>Planlama güncelleştir

Güncelleştirmeyi uygulamadan önce aşağıdaki bilgileri gözden geçirdiğinizden emin olun:

- [Bilinen sorunlar](azure-stack-release-notes-known-issues-1904.md)
- [Güvenlik güncelleştirmeleri](azure-stack-release-notes-security-updates-1904.md)
- [Önce ve güncelleştirmeyi uyguladıktan sonra etkinlikler listesi](azure-stack-release-notes-checklist.md)

> [!NOTE]
> En son sürümünü kullandığınızdan emin olun [Azure Stack Capacity Planner](https://aka.ms/azstackcapacityplanner) iş yükünüz planlama ve boyutlandırma gerçekleştirmek için aracı. En son sürüm hata düzeltmeleri içerir ve yayımlanan yeni özellikler sağlayan her Azure Stack güncelleştirme.

## <a name="download-the-update"></a>Güncelleştirmeyi indirin

Azure Stack 1904 güncelleştirme paketinden indirebileceğiniz [Azure Stack indirme sayfasına](https://aka.ms/azurestackupdatedownload).

## <a name="hotfixes"></a>Düzeltmeler

Azure Stack düzeltmeleri düzenli olarak serbest bırakır. Azure Stack için 1904 güncelleştirmeden önce en son Azure Stack düzeltme için 1903 yüklediğinizden emin olun.

Azure Stack düzeltmeleri yalnızca Azure Stack tümleşik sistemleri için geçerlidir; üzerinde ASDK düzeltmelerini çalışmayın.

### <a name="before-applying-the-1904-update"></a>1904 uygulamadan önce güncelleştirme

Azure Stack 1904 sürüm aşağıdaki düzeltmeleri 1903 sürümle uygulanması gerekir:

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack düzeltme 1.1903.2.39](https://support.microsoft.com/help/4500638)

### <a name="after-successfully-applying-the-1904-update"></a>1904 başarıyla uygulandıktan sonra güncelleştir

Bu güncelleştirme yüklendikten sonra geçerli düzeltmeleri yükleyin. Daha fazla bilgi için müşterilerimize [hizmet İlkesi](azure-stack-servicing-policy.md).

- [Azure Stack düzeltme 1.1904.4.45](https://support.microsoft.com/help/4505688)

## <a name="automatic-update-notifications"></a>Otomatik güncelleştirme bildirimleri

Müşteriler altyapı ağdan internet erişimi olan sistemlerle **güncelleştirme kullanılabilir** işleci portalında iletisi. İnternet erişimi olmayan sistemleri, indirin ve karşılık gelen .xml ile .zip dosyasını içeri aktarabilirsiniz.

> [!TIP]  
> Aşağıdaki abone olmak *RSS* veya *Atom* Azure Stack düzeltmelerle birlikte kalmasını sağlamak için akışları:
>
> - [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss)
> - [Atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom)

## <a name="next-steps"></a>Sonraki adımlar

- Azure Stack'te güncelleştirme yönetimi genel bakış için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md).  
- Azure Stack güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için bkz. [güncelleştirmelerini Azure Stack'te](azure-stack-apply-updates.md).
- Azure Stack tümleşik sistemleri ve desteklenen bir duruma sisteminizi tutmak için yapmanız gerekenlere bakım ilkeyi gözden geçirmek için bkz: [Azure Stack hizmet İlkesi](azure-stack-servicing-policy.md).  
- Ayrıcalıklı uç noktasına (CESARETLENDİRİCİ) izlemek ve güncelleştirmelerini sürdürmek üzere kullanmak için bkz [izleme ayrıcalıklı uç noktayı kullanarak Azure stack'teki güncelleştirmeleri](azure-stack-monitor-update.md).  
- Sağlamak üzere anketi doldurmaya [sürüm notları geribildirim](https://forms.microsoft.com).
