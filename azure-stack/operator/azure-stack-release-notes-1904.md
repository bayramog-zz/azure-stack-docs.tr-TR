---
title: Azure Stack 1904 sürüm notları | Microsoft Docs
description: Yenilikler, bilinen sorunlar ve güncelleştirme Azure Stack sürüm notlarının nerede indirileceği dahil Azure Stack tümleşik sistemler için 1904 güncelleştirmesi hakkında bilgi edinin.
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
ms.reviewer: hectorl
ms.lastreviewed: 05/30/2019
ms.openlocfilehash: 71ad5812472534ae4a7e3dc34feabf7a1a02faa0
ms.sourcegitcommit: f6ea6daddb92cbf458f9824cd2f8e7e1bda9688e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68494042"
---
# <a name="azure-stack-1904-update"></a>Azure Stack 1904 güncelleştirmesi

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Bu makalede 1904 güncelleştirme paketinin içeriği açıklanır. Güncelleştirme yenilikleri ve bu Azure Stack sürümü için düzeltmeler içerir. Bu makalede aşağıdaki bilgiler yer alır:

- [Yenilikler, iyileştirmeler, düzeltmeler ve güvenlik güncelleştirmelerinin açıklaması](azure-stack-release-notes-1904.md#whats-in-this-update)
- [Planlamayı Güncelleştir](azure-stack-release-notes-1904.md#update-planning)

> [!IMPORTANT]  
> Yalnızca Azure Stack tümleşik sistemleri için bu güncelleştirme paketidir. Bu güncelleştirme paketi için Azure Stack geliştirme Seti'ni geçerli değildir.

## <a name="archived-release-notes"></a>Arşivlenmiş sürüm notları

[Azure Stack sürüm notlarının eski sürümlerini TechNet galerisinde](http://aka.ms/azsarchivedrelnotes)görebilirsiniz. Bu arşivlenmiş sürüm notları yalnızca başvuru amacıyla sağlanır ve bu sürümler için destek göstermez. Daha fazla yardım için Microsoft Müşteri Destek Hizmetleri 'ne başvurun.

## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack 1904 güncelleştirme yapı numarası **1.1904.0.36**.

### <a name="update-type"></a>Güncelleştirme türü

Azure Stack 1904 güncelleştirme yapı türü **Express**'dir. Güncelleştirme oluşturma türleri hakkında daha fazla bilgi için [Azure Stack güncelleştirmeleri yönetme](azure-stack-updates.md) makalesine bakın. 1904 güncelleştirmesinin tamamlaması için gereken süre yaklaşık 16 saat, ancak tam süreler farklılık gösterebilir. Bu çalışma zamanı yaklaşık 1904 güncelleştirmesine özeldir ve diğer Azure Stack güncelleştirmeleriyle karşılaştırılmamalıdır.

## <a name="whats-in-this-update"></a>Bu güncelleştirmede neler vardır

<!-- The current theme (if any) of this release. -->

<!-- What's new, also net new experiences and features. -->

### <a name="improvements"></a>Geliştirmeleri

<!-- Changes and product improvements with tangible customer-facing value. -->

- Yazılım tanımlı ağ (SDN) yığınında 1904 içinde önemli iyileştirmeler yapılmıştır. Bu geliştirmeler, Azure Stack içindeki SDN yığınının genel bakımını ve güvenilirliğini artırır.

- Şu anda oturum açmış olan kullanıcı gerekli izinlere sahip olmadığında yönetici portalına bir bildirim eklediniz. Bu, panonun düzgün yüklenmesine olanak sağlar. Ayrıca, dağıtım sırasında kullanılan kimlik sağlayıcısına bağlı olarak hangi hesapların uygun izinlere sahip olduğunu açıklayan belgelere bir bağlantı içerir.

- VM esnekliği ve çalışma süresine yönelik iyileştirmeler, VM yapılandırma dosyalarını içeren depolama birimi çevrimdışı kaldığında tüm VM 'Lerin çevrimdışı olduğu senaryoyu çözümler.

<!-- 1901,2,3 related hotfix -->
- Eş zamanlı olarak ayrılan VM sayısına en iyi duruma getirme eklendi ve ağ ağır yük altındaysa VM 'de veya blackouts ' e bir sınır yerleştirdi. Bu değişiklik, bir sistem güncelleştirilirken VM çalışma süresini artırır.

<!-- 1901,2,3 related hotfix -->
- Bir sistem, platform kaynaklarını tükettirilen iç işlemlere karşı koruma sağlamak üzere ölçek üzerinde çalışırken geliştirilmiş kaynak azaltma, portalda başarısız olan işlemlere neden olur.

- Geliştirilmiş filtreleme özellikleri, işleçlerin aynı anda birden çok filtre uygulamasına olanak tanır. Yalnızca yeni kullanıcı arabirimindeki **ad** sütununda sıralama yapabilirsiniz.

- Teklifler, planlar, Kotalar ve abonelikler silme sürecinde iyileştirmeler. Artık, silmek istediğiniz nesnenin bağımlılığı yoksa yönetici portalından teklifleri, kotaları, planları ve abonelikleri başarıyla silebilirsiniz. Daha fazla bilgi için [bu makaleye](azure-stack-delete-offer.md) bakın.  

<!-- this applies to bug 3725384 and bug #4225643 -->
- Gereksiz olayları filtreleyerek ve iletilen iletiler için istenen önem derecesini seçmek üzere bir yapılandırma parametresi sağlayarak iyileştirilmiş Syslog ileti birimi. Önem düzeyini yapılandırma hakkında daha fazla bilgi için bkz. [Azure Stack Datacenter Integration-Syslog iletme](azure-stack-integrate-security.md).

<!--this applied to Bug 1473487 -->
- **Get-AzureStackLog** cmdlet 'ine ek bir parametre `-OutputSASUri`ekleyerek yeni bir özellik eklendi. Artık ortamınızdan Azure Stack günlüklerini toplayıp belirtilen Azure Depolama Blobu kapsayıcısında depolayabiliriz. Daha fazla bilgi için bkz. [tanılama Azure Stack](azure-stack-configure-on-demand-diagnostic-log-collection.md#examples).

- Güncelleştirmenin başarıyla tamamlanabilmesi için yığında yeterli kullanılabilir bellek olup olmadığını kontrol eden **Test-azurestack** `UpdateReadiness` grubunda yeni bir bellek denetimi eklendi.

<!-- Bug/Task 4311058 -->
- Service Fabric sistem durumunu değerlendirmek için **Test-AzureStack** geliştirmeleri.

<!-- feature: 2976966 -->
- Donanım güncelleştirmelerine yönelik iyileştirmeler, bu sayede sürücü üretici yazılımının güncelleştirme 2-4 saat için geçen süreyi azaltır. Güncelleştirme altyapısı, paketteki içeriğe bağlı olarak güncelleştirmenin hangi bölümlerinin yürütülmesi gerektiğini dinamik olarak belirler.

<!-- Feature 3906611 -->
- Kullanılabilirliği etkileyen altyapı rolü örnek işlemlerinin kesintiye uğramasını engellemek için sağlam işlem ön denetimleri eklendi.

<!-- Feature 3780326 -->
- Altyapı yedeklemesi eylem planının ssotne kadar iyileştirmeleri.

<!--Bug/Task 3139609 -->
- Azure Stack günlük toplama geliştirmeleri. Bu geliştirmeler, günlük kümesini almak için gereken süreyi azaltır. Ayrıca, [Get-AzureStackLog](azure-stack-configure-on-demand-diagnostic-log-collection.md#run-get-azurestacklog-on-an-azure-stack-development-kit-asdk-system) cmdlet 'ı artık OEM rolü için varsayılan Günlükler oluşturmaz. OEM günlüklerini almak için rolü belirterek [Invoke-Azurestackon, log](azure-stack-configure-on-demand-diagnostic-log-collection.md#invoke-azurestackondemandlog) cmdlet 'ini yürütmeniz gerekir. Daha fazla bilgi için bkz. [tanılama Azure Stack](azure-stack-configure-on-demand-diagnostic-log-collection.md#run-get-azurestacklog-on-an-azure-stack-development-kit-asdk-system).

- Azure Stack artık ADFS ile veri merkezi tümleştirmesi için sunulan Federasyon veri URL 'sini izler. Bu, müşteri ADFS örneğinin veya grubunun gizli bir dönüşü sırasında güvenilirliği artırır.

### <a name="changes"></a>Değişiklikler

<!-- Feature 3906611 -->
- Yönetici portalındaki altyapı rolü örneklerini kapatmak için Azure Stack işleçleri seçeneği kaldırılmıştır. Yeniden başlatma işlevselliği, altyapı rolü örneğini yeniden başlatmadan önce temiz bir kapatılma denemesi sağlar. Gelişmiş senaryolar için API ve PowerShell işlevleri kullanılabilir kalır.

<!-- Feature ## 4199257 -->
- Market görüntüleri ve kaynak sağlayıcıları için ayrı ekranlar içeren yeni bir market yönetim deneyimi vardır. Şimdilik **kaynak sağlayıcıları** penceresi boştur, ancak gelecek sürümlerde yeni PaaS hizmet teklifleri görünür ve **kaynak sağlayıcıları** penceresinde yönetilir.

<!-- Feature ## 4199257 -->
- Operatör portalındaki güncelleştirme deneyiminde yapılan değişiklikler. Kaynak sağlayıcısı güncelleştirmeleri için yeni bir kılavuz vardır. Kaynak sağlayıcılarını güncelleştirme özelliği henüz kullanılamıyor.

<!-- Task ## 3748423  -->
- İşletmen portalındaki güncelleştirme yükleme deneyiminde yapılan değişiklikler. Azure Stack işleçlerinin bir güncelleştirme sorununa uygun bir şekilde yanıt vermesini sağlamak için Portal artık, **Test-AzureStack** çalıştırılarak ve sonuçları ayrıştırırken otomatik olarak türetilen ölçek biriminin durumuna göre daha özel öneriler sağlar. Sonuca bağlı olarak, işleci iki eylemden birini alacak şekilde bilgilendirir:

  - Portalda "geçici" bir uyarı uyarısı görüntülenir ve bu, en son güncelleştirme Için ilgilenilmesi gereken "' i okur. Microsoft, normal çalışma saatlerinde bir hizmet isteği açılmasını öneriyor. Güncelleştirme sürecinin bir parçası olarak, test-AzureStack gerçekleştirilir ve en uygun uyarıyı ürettiğimiz çıktıyı temel alır. Bu durumda, test-AzureStack geçildi. "

  - Portalda "en son güncelleştirme başarısız oldu" yazan "Hard" kritik uyarısı görüntülenir. Microsoft, bir hizmet isteğini mümkün olan en kısa sürede açmayı öneriyor. Güncelleştirme sürecinin bir parçası olarak, test-AzureStack gerçekleştirilir ve en uygun uyarıyı ürettiğimiz çıktıyı temel alır. Bu durumda, test-AzureStack de başarısız oldu. "

- Azure Linux Aracısı sürümü 2.2.38.0 güncelleştirildi. Bu destek, müşterilerin Azure ile Azure Stack arasında tutarlı Linux görüntüleri korumasına olanak tanır.

- Operatör portalındaki güncelleştirme günlüklerinde yapılan değişiklikler. Başarılı güncelleştirme günlüklerini alma istekleri artık kullanılamıyor. Tanılama için kullanılabilir durumda olduklarından, günlükleri güncelleştirme başarısız olduğundan, hala indirilmek üzere kullanılabilir.

### <a name="fixes"></a>Düzeltmeleri

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there is an SR/ICM associated to it. -->

- Syslog yapılandırmasının bir güncelleştirme döngüsüyle kalıcı olmadığı, syslog istemcisinin yapılandırmasını kaybetmesine ve ileti iletilmesi durdurulacak Syslog iletilerinin kaybolmasına neden olan bir sorun düzeltildi. Syslog yapılandırması artık korunuyor.

- CRP 'de VM 'lerin ayırmayı engelleyen bir sorun düzeltildi. Daha önce, bir VM birden çok büyük yönetilen disk içeriyorsa, VM 'nin ayırmayı kaldırma işlemi zaman aşımı hatası vererek başarısız olmuş olabilir.

- Windows Defender altyapısında, ölçek birimi depolamaya erişimi etkileyen sorun düzeltildi.

- BLOB depolama hesapları için erişim Ilkesi penceresinin yüklenmesi başarısız olan bir Kullanıcı Portalı sorunu düzeltildi.

- Yönetici ve Kullanıcı portallarında, genel Azure portal ilgili hatalı bildirimlerin görüntülendiği bir sorun düzeltildi.

- **Geri bildirim** kutucuğunu seçen bir Kullanıcı Portalı sorunu düzeltildi, boş bir tarayıcı sekmesinin açılmasını neden oldu.

- Bir VM örneğine bağlı bir ağ bağdaştırıcısına bağlı bir IP yapılandırması için statik IP adresini değiştirmenin bir hata iletisinin görüntülenmesine neden olan bir portal sorunu düzeltildi.

- Ağ **penceresi aracılığıyla** mevcut bir VM 'ye **ağ arabirimi eklemeye** çalışan bir Kullanıcı Portalı sorunu düzeltildi, işlemin bir hata iletisiyle başarısız olmasına neden oldu.

- Azure Stack, bir VM örneğine 4 ' ten fazla ağ arabirimi (NIC) eklenmesi desteklenmeyen bir sorun düzeltildi.

- Bir gelen güvenlik kuralı ekleyen ve kaynak olarak **hizmet etiketi** seçen bir portal sorunu düzeltildi, Azure Stack için kullanılamayan çeşitli seçenekler gösteriliyor.

- Ağ güvenlik gruplarının (NSG 'ler) Azure Stack içinde, küresel Azure ile aynı şekilde çalışmayan sorun düzeltildi.

- Market yönetiminde bir sorun düzeltildi, bu, kaydın süresi dolarsa veya kaldırılırsa indirilen tüm ürünleri gizler.

- PowerShell 'de var olan bir **set-AzureRmVirtualNetworkGatewayConnection** komutunun mevcut bir sanal ağ geçidi bağlantısına verilme sorunu düzeltildi, şu hata iletisiyle başarısız oldu **paylaşılan anahtar yapılandırıldı...** .

- Ağ kaynak sağlayıcısı 'nın (NRP) ağ denetleyicisiyle eşitlenmesine neden olan bir sorun düzeltildi ve bu, istenen yinelenen kaynaklara neden oldu. Bazı durumlarda, bu, üst kaynağın bir hata durumunda ayrılmasıyla sonuçlandı.

- Bir aboneliğe katkıda bulunan bir rol atanmış, ancak açıkça okuma izinleri verilmemişse bir sorun düzeltildi...  **{GUID}somelogonaccount@domain.comnesne kimliğine sahip ' ' istemcisinin,** bir kaynağın değişikliğini kaydetmeye çalışırken eylem gerçekleştirme yetkisi yok...

- Çevrimdışı dağıtım aracı resimleri karşıya yüklemek için kullanılmışsa ve bunlardan birinde simge URI 'leri eksikse Market yönetimi ekranının boş olduğu bir sorun düzeltildi.

- Market yönetiminde, indirmeyen ürünlerin silinmesinin engellenmediği bir sorun düzeltildi.

### <a name="security-updates"></a>Güvenlik güncelleştirmeleri

Bu Azure Stack güncelleştirmesi, Azure Stack barındıran temel işletim sistemine yönelik güvenlik güncelleştirmelerini içermez. Bilgi için bkz [. Azure Stack güvenlik güncelleştirmeleri.](azure-stack-release-notes-security-updates-1904.md)

## <a name="update-planning"></a>Planlamayı Güncelleştir

Güncelleştirmeyi uygulamadan önce, aşağıdaki bilgileri gözden geçirdiğinizden emin olun:

- [Bilinen sorunlar](azure-stack-release-notes-known-issues-1904.md)
- [Güvenlik güncelleştirmeleri](azure-stack-release-notes-security-updates-1904.md)
- [Güncelleştirme uygulandıktan önceki ve sonraki etkinliklerin denetim listesi](azure-stack-release-notes-checklist.md)

> [!NOTE]
> İş yükü planlama ve boyutlandırmasını gerçekleştirmek için [Azure Stack Capacity Planner](https://aka.ms/azstackcapacityplanner) aracının en son sürümünü kullandığınızdan emin olun. En son sürüm hata düzeltmeleri içerir ve her bir Azure Stack güncelleştirmesiyle yayınlanan yeni özellikler sağlar.

## <a name="download-the-update"></a>Güncelleştirmeyi indirin

Azure Stack 1904 güncelleştirme paketini [Azure Stack indirme sayfasından](https://aka.ms/azurestackupdatedownload)indirebilirsiniz.

## <a name="hotfixes"></a>Düzeltmeler

Düzeltmeleri düzenli aralıklarla yayınlar Azure Stack. Azure Stack 1904 ' e güncelleştirmeden önce 1903 için en son Azure Stack düzeltmesini yüklediğinizden emin olun.

Azure Stack düzeltmeler yalnızca Azure Stack tümleşik sistemler için geçerlidir; ASDK 'ye düzeltme yüklemeyi denemeyin.

### <a name="before-applying-the-1904-update"></a>1904 güncelleştirmesini uygulamadan önce

Azure Stack 1904 sürümü aşağıdaki düzeltmelere sahip 1903 sürümüne uygulanmalıdır:

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hotfix 1.1903.2.39](https://support.microsoft.com/help/4500638)

### <a name="after-successfully-applying-the-1904-update"></a>1904 güncelleştirmesini başarıyla uyguladıktan sonra

Bu güncelleştirmenin yüklenmesinden sonra, geçerli düzeltmeleri yükleme. Daha fazla bilgi için bkz. [hizmet ilkeniz](azure-stack-servicing-policy.md).

- [Azure Stack Hotfix 1.1904.4.45](https://support.microsoft.com/help/4505688)

## <a name="automatic-update-notifications"></a>Otomatik güncelleştirme bildirimleri

Altyapı ağından internet 'e erişebilen sistemlere sahip müşteriler, operatör portalındaki **güncelleştirme kullanılabilir** iletisini görür. İnternet erişimi olmayan sistemler. zip dosyasını indirebilir ve ilgili. xml ile içe aktarabilir.

> [!TIP]  
> Azure Stack düzeltmelere devam etmek için aşağıdaki *RSS* veya *atom* akışlarına abone olun:
>
> - ['YI](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss)
> - [Atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom)

## <a name="next-steps"></a>Sonraki adımlar

- Azure Stack'te güncelleştirme yönetimi genel bakış için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md).  
- Azure Stack güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için bkz. [güncelleştirmelerini Azure Stack'te](azure-stack-apply-updates.md).
- Azure Stack tümleşik sistemleri ve desteklenen bir duruma sisteminizi tutmak için yapmanız gerekenlere bakım ilkeyi gözden geçirmek için bkz: [Azure Stack hizmet İlkesi](azure-stack-servicing-policy.md).  
- Ayrıcalıklı uç noktasına (CESARETLENDİRİCİ) izlemek ve güncelleştirmelerini sürdürmek üzere kullanmak için bkz [izleme ayrıcalıklı uç noktayı kullanarak Azure stack'teki güncelleştirmeleri](azure-stack-monitor-update.md).  

