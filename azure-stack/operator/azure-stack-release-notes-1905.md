---
title: Azure Stack 1905 sürüm notları | Microsoft Docs
description: Yenilikler, bilinen sorunlar ve güncelleştirmenin indirileceği gibi Azure Stack tümleşik sistemler için 1905 güncelleştirmesi hakkında bilgi edinin.
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
ms.date: 06/14/2019
ms.author: sethm
ms.reviewer: hectorl
ms.lastreviewed: 06/14/2019
monikerRange: azs-1905
ms.openlocfilehash: 27a41069c3406cdb843e263ee49a63e5c5919477
ms.sourcegitcommit: 71d7990a2b21576c44bb2aea13ae2026e9510c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70188326"
---
# <a name="azure-stack-1905-update"></a>Azure Stack 1905 güncelleştirmesi

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Bu makalede 1905 güncelleştirme paketinin içeriği açıklanır. Güncelleştirme yenilikleri ve bu Azure Stack sürümü için düzeltmeler içerir. Bu makalede aşağıdaki bilgiler yer alır:

- [Yenilikler, iyileştirmeler, düzeltmeler ve güvenlik güncelleştirmelerinin açıklaması](#whats-in-this-update)
- [Planlamayı Güncelleştir](#update-planning)

> [!IMPORTANT]  
> Yalnızca Azure Stack tümleşik sistemleri için bu güncelleştirme paketidir. Bu güncelleştirme paketi için Azure Stack geliştirme Seti'ni geçerli değildir.

## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack 1905 güncelleştirme yapı numarası **1.1905.0.40**.

### <a name="update-type"></a>Güncelleştirme türü

Azure Stack 1905 güncelleştirme yapı türü **dolu**. Sonuç olarak, 1905 güncelleştirmesinin 1903 ve 1904 gibi hızlı güncelleştirmelerden daha uzun bir çalışma zamanı vardır. Tam güncelleştirmeler için tam çalışma zamanları genellikle Azure Stack örneğinizin içerdiği düğüm sayısına, kiracı iş yükleri tarafından sisteminizde kullanılan kapasiteye, sisteminizin ağ bağlantısına (internet 'e bağlıysa) ve sistem donanımınıza bağlıdır yapılandırmada. 1905 güncelleştirmesinde, iç sınamamız içinde aşağıdaki beklenen çalışma zamanları vardı: 4 düğüm-35 saat, 8 düğüm-45 saat, 12 düğüm-55 saat, 16 düğüm-70 saat. 1905 çalışma zamanı, bu beklenen değerlerden daha uzun bir süre daha seyrek değildir ve güncelleştirme başarısız olmazsa Azure Stack işleçlere göre eylem gerektirmez. Güncelleştirme oluşturma türleri hakkında daha fazla bilgi için bkz. [Azure Stack güncelleştirmeleri yönetme](azure-stack-updates.md).

## <a name="whats-in-this-update"></a>Bu güncelleştirmede neler vardır

<!-- The current theme (if any) of this release. -->

<!-- What's new, also net new experiences and features. -->

- Bu güncelleştirmeyle, Azure Stack 'deki güncelleştirme altyapısı, ölçek birimi düğümlerinin bellenimini güncelleştirebilir. Bu, donanım iş ortaklarından uyumlu bir güncelleştirme paketi gerektirir. Kullanılabilirlik hakkındaki ayrıntılar için donanım iş ortağınıza ulaşın.

- Windows Server 2019 artık desteklenmektedir ve Azure Stack Marketi aracılığıyla genel olarak kullanılabilir.
Bu güncelleştirmeyle, Windows Server 2019 artık 2016 ana bilgisayarında başarıyla etkinleştirilebilir.

- Yeni bir [Azure hesap Visual Studio Code uzantısı](../user/azure-stack-dev-start-vscode-azure.md) , geliştiricilerin oturum açarak ve abonelikleri görüntüleyerek Azure Stack hedeflemesini sağlar ve birçok farklı hizmeti de görebilir. Azure Hesap uzantısı hem Azure Active Directory (Azure AD) hem de AD FS ortamlarında çalışarak, yalnızca Visual Studio Code Kullanıcı ayarlarında küçük bir değişiklik gerektirir. Visual Studio Code, bu ortamda çalıştırılmak üzere bir hizmet sorumlusuna izin verilmesini gerektirir. Bunu yapmak için kimlik betiğini içeri aktarın ve [Azure Stack Içinde çok kiracılı](../operator/azure-stack-enable-multitenancy.md)olarak belirtilen cmdlet 'leri çalıştırın. Bu, giriş dizini için bir güncelleştirme ve her bir dizin için konuk kiracı dizini kaydı gerektirir. Visual Studio Code hizmet sorumlusu 'nın dahil olduğu ana dizin kiracısını güncelleştirmek için 1905 veya sonraki bir sürüme güncelleştirildikten sonra bir uyarı görüntülenir. 

### <a name="improvements"></a>Geliştirmeleri

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
  
### <a name="changes"></a>Değişiklikler

- Planlı ve plansız bakım senaryolarında güvenilirliği ve kullanılabilirliği artırmak için, Azure Stack etki alanı Hizmetleri için ek bir altyapı rol örneği ekler.

- Bu güncelleştirme ile, onarım ve düğüm ekleme işlemleri sırasında donanım, ölçek birimi içindeki homoölçek birim düğümlerini sağlamak üzere onaylanır.

- Zamanlanan yedeklemeler tamamlanmazsa ve tanımlanan saklama süresi aşılırsa, altyapı yedekleme denetleyicisi en az bir başarılı yedeklemenin korunduğundan emin olur. 

### <a name="fixes"></a>Düzeltmeleri

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there is an SR/ICM associated to it. -->

- Ölçek biriminde bir düğüm yeniden başlatıldıktan sonra bir **işlem Konağı Aracısı** uyarısının görünmediği bir sorun düzeltildi.

- Yönetici portalında Market yönetiminde, filtreler uygulanırken hatalı sonuçlar gösterildiğinde ve yayımcı filtresinde yinelenen yayımcı adları gösterilen sorunlar düzeltildi. Ayrıca, sonuçları daha hızlı göstermek için performans iyileştirmeleri de yapılmıştır.

- Kullanılabilir yedekleme dikey penceresinde, dış depolama konumuna karşıya yükleme tamamlanmadan önce yeni bir yedekleme işlemi listelenmiş olan sorun düzeltildi. Artık kullanılabilir yedekleme, depolama konumuna başarıyla yüklendikten sonra listede görünür. 

<!-- ICM: 114819337; Task: 4408136 -->
- Yedekleme işlemi sırasında kurtarma anahtarları alınırken sorun düzeltildi. 

<!-- Bug: 4525587 -->
- İşleç portalında sürümü ' tanımsız ' olarak gösteren OEM güncelleştirme ile ilgili sorun düzeltildi.

### <a name="security-updates"></a>Güvenlik güncelleştirmeleri

Bu Azure Stack güncelleştirmesinde güvenlik güncelleştirmeleri hakkında daha fazla bilgi için bkz. [Azure Stack Security Updates](azure-stack-release-notes-security-updates.md).

## <a name="update-planning"></a>Planlamayı Güncelleştir

Güncelleştirmeyi uygulamadan önce, aşağıdaki bilgileri gözden geçirdiğinizden emin olun:

- [Bilinen sorunlar](azure-stack-release-notes-known-issues-1905.md)
- [Güvenlik güncelleştirmeleri](azure-stack-release-notes-security-updates.md)
- [Güncelleştirme uygulandıktan önceki ve sonraki etkinliklerin denetim listesi](azure-stack-release-notes-checklist.md)

## <a name="download-the-update"></a>Güncelleştirmeyi indirin

Azure Stack 1905 güncelleştirme paketini [Azure Stack indirme sayfasından](https://aka.ms/azurestackupdatedownload)indirebilirsiniz. Yükleyici aracını kullanırken, karşıdan yüklemeler dizininizden önbelleğe alınmış bir kopya değil, en son sürümü kullandığınızdan emin olun.

## <a name="hotfixes"></a>Düzeltmeler

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

## <a name="automatic-update-notifications"></a>Otomatik güncelleştirme bildirimleri

Altyapı ağından internet 'e erişebilen sistemlere sahip müşteriler, operatör portalındaki **güncelleştirme kullanılabilir** iletisini görür. İnternet erişimi olmayan sistemler. zip dosyasını indirebilir ve ilgili. xml ile içe aktarabilir.

> [!TIP]  
> Azure Stack düzeltmelere devam etmek için aşağıdaki *RSS* veya *atom* akışlarına abone olun:
>
> - ['YI](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss)
> - [Atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom)

## <a name="archived-release-notes"></a>Arşivlenmiş sürüm notları

[Azure Stack sürüm notlarının eski sürümlerini TechNet galerisinde](http://aka.ms/azsarchivedrelnotes)görebilirsiniz. Bu arşivlenmiş sürüm notları yalnızca başvuru amacıyla sağlanır ve bu sürümler için destek göstermez. Azure Stack desteği hakkında bilgi için bkz. [Azure Stack hizmet ilkesi](azure-stack-servicing-policy.md). Daha fazla yardım için Microsoft Müşteri Destek Hizmetleri 'ne başvurun.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Stack'te güncelleştirme yönetimi genel bakış için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md).  
- Azure Stack güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için bkz. [güncelleştirmelerini Azure Stack'te](azure-stack-apply-updates.md).
- Azure Stack tümleşik sistemleri ve desteklenen bir duruma sisteminizi tutmak için yapmanız gerekenlere bakım ilkeyi gözden geçirmek için bkz: [Azure Stack hizmet İlkesi](azure-stack-servicing-policy.md).  
- Ayrıcalıklı uç noktasına (CESARETLENDİRİCİ) izlemek ve güncelleştirmelerini sürdürmek üzere kullanmak için bkz [izleme ayrıcalıklı uç noktayı kullanarak Azure stack'teki güncelleştirmeleri](azure-stack-monitor-update.md).  

