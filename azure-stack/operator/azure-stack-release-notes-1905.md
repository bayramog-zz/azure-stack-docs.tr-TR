---
title: Azure Stack 1905 sürüm notları | Microsoft Docs
description: Yenilikler dahil olmak üzere, Azure Stack tümleşik sistemleri 1905 güncelleştirmesi hakkında bilinen sorunlar ve güncelleştirmeyi yüklemek nereye öğrenin.
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
ms.openlocfilehash: 6f178d0208f5111a7ae60d23e1d914fcf8e3aba5
ms.sourcegitcommit: c9d11be7d27c73797bdf279d4fcabb7a22451541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67397262"
---
# <a name="azure-stack-1905-update"></a>Azure Stack 1905 güncelleştirme

*Uygulama hedefi: Azure Stack tümleşik sistemleri*

Bu makalede 1905 güncelleştirme paketinin içeriğini açıklar. Yenilikler güncelleştirme içerir. iyileştirmeler ve düzeltmeler bu sürümü, Azure Stack için. Bu makalede, aşağıdaki bilgileri içerir:

- [Yeniliklerin açıklaması, geliştirmeleri, düzeltmeler ve güvenlik güncelleştirmeleri](#whats-in-this-update)
- [Planlama güncelleştir](#update-planning)

> [!IMPORTANT]  
> Yalnızca Azure Stack tümleşik sistemleri için bu güncelleştirme paketidir. Bu güncelleştirme paketi için Azure Stack geliştirme Seti'ni geçerli değildir.

## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack 1905 güncelleştirmenin yapı numarasıdır **1.1905.0.40**.

### <a name="update-type"></a>Güncelleştirme türü

Azure Stack 1905 güncelleştirme derleme türü **tam**. Sonuç olarak, express güncelleştirmeleri 1903 ve 1904 gibi daha uzun bir çalışma zamanı 1905 güncelleştirme vardır. Tam çalışma zamanları tam güncelleştirme için genellikle Azure Stack örneğinizin Kiracı İş yükleri tarafından sisteminizin ağ bağlantısı (internet'e bağlı değilse), sisteminizde kullanılan kapasite içerdiğinden emin düğüm sayısına bağlıdır ve sistem donanımınız yapılandırma. 1905 güncelleştirme iç test işlemlerimizi, aşağıdaki beklenen çalışma zamanları oluşturdu: 4 düğüm - 35 saati, 8 düğüm - saat 45, 12 düğümleri - 55 saati, 16 düğüme - 70 saat. Bu beklenen değerleri daha uzun uzun 1905 çalışma zamanları nadir değildir ve güncelleştirme başarısız sürece eylem Azure Stack operatörleri tarafından gerektirmez. Derleme türleri güncelleştirme hakkında daha fazla bilgi için bkz: [Azure Stack'te güncelleştirmeleri yönetme](azure-stack-updates.md).

## <a name="whats-in-this-update"></a>Bu güncelleştirmede nedir

<!-- The current theme (if any) of this release. -->

<!-- What's new, also net new experiences and features. -->

- Bu güncelleştirme ile Azure Stack'te güncelleştirme altyapısı ölçek biriminin düğümlerinin bellenim güncelleştirebilirsiniz. Bu donanım iş ortaklarından uyumlu güncelleştirme paketinden gerektirir. Kullanılabilirliği hakkındaki ayrıntılar için donanım ortağınıza ulaşın.

- Windows Server 2019 Azure Stack Market aracılığıyla desteklenen ve kullanılabilir alanınızdaki sunulmuştur.
Bu güncelleştirmeyle, Windows Server 2019 artık başarıyla 2016 konağında etkinleştirilebilir.

### <a name="improvements"></a>Geliştirmeleri

<!-- Changes and product improvements with tangible customer-facing value. -->
- Azure Stack'te TLS 1.2 zorlama bir parçası olarak, aşağıdaki uzantılar bu sürümleri için güncelleştirilmiştir:

  - microsoft.customscriptextension-arm-1.9.3
  - Microsoft.iaasdiagnostics 1.12.2.2
  - microsoft.antimalware-windows-arm-1.5.5.9
  - microsoft.dsc-arm-2.77.0.0
  - microsoft.vmaccessforlinux-1.5.2

  Lütfen Uzantılar'ın bu sürümlerinin gelecekteki bir sürümde TLS 1.2 uygulandığında uzantının yeni dağıtımlar başarısız olmayın böylece hemen indirin. Her zaman **autoUpgradeMinorVersion = true** böylece Uzantıları (örneğin, 1.8 için 1.9) için alt sürüm güncelleştirmeleri otomatik olarak gerçekleştirilir.

- Yeni bir **Yardım ve Desteği'ne genel bakış** Azure Stack'te portal, destek seçenekleri işaretleyin, Uzman Yardımı alın ve Azure Stack hakkında daha fazla bilgi, işleçler kolaylaştırır. Tümleşik sistemlerde, bir destek isteği oluşturma, Azure Stack hizmeti SCP'si. Yüksek oranda müşterilerin genel Azure portalını kullanarak yerine biletleri göndermek için bu deneyim kullanmanızı öneririz. Daha fazla bilgi için [Azure Stack Yardım ve Destek](azure-stack-help-and-support-overview.md).

- Birden çok Azure Active dizin eklenen olduğunda (aracılığıyla [bu işlem](azure-stack-enable-multitenancy.md)), belirli güncelleştirmeler olduğunda veya AAD hizmet sorumlusu yetkilendirme değişiklikleri neden olacak şekilde hakları betik artırarak algoritmanın yeniden çalıştırılması ihmal mümkündür eksik. Bu çeşitli sorunları engellenen erişim belirli özellikleri özgün verilecek izleme sabit olan daha fazla ayrı hatalarına neden olabilir. Bunu önlemek için bu izinleri denetler ve belirli yapılandırma sorunlarını bulunduğunda bir uyarı oluşturan yeni bir özellik 1905 tanıtır. Bu doğrulama saatte bir çalışır ve bu sorunu düzeltmek için gerekli düzeltme eylemleri görüntüler. Tüm kiracılar iyi durumda olduğunda, uyarıyı kapatır.

- Geliştirilmiş Güvenilirlik altyapı hizmeti yük devretme sırasında yedekleme işlemleri.

- Yeni bir sürümü [Azure Stack Nagios eklentisi](azure-stack-integrate-monitor.md#integrate-with-nagios) olan kullanılabilir kullanan [Azure Active Directory kimlik doğrulama kitaplıkları](/azure/active-directory/develop/active-directory-authentication-libraries) (ADAL) kimlik doğrulaması için. Eklenti ayrıca artık Azure Stack, Azure Active Directory (AAD) ve Active Directory Federasyon Hizmetleri (ADFS) dağıtımlarını destekler. Daha fazla bilgi için [Nagios eklentisi exchange](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details) site.

- Yeni bir karma profili **2019-03-01-karma** yayımlanan Azure Stack'te, en son özellikleri destekler. Hem Azure PowerShell ve Azure CLI desteği **2019-03-01-karma** profili. .NET, Ruby, Node.js, Go ve Python SDK'ları destekleyen paketleri yayımladıktan **2019-03-01-karma** profili. İlgili belgeler ve bazı örnekler, değişiklikleri yansıtacak şekilde güncelleştirildi.

- [Node.js SDK'sı](https://www.npmjs.com/search?q=2019-03-01-hybrid) artık API profillerini destekler. Paketleri destekleyen **2019-03-01-karma** profili yayımlanır.

- 1905 Azure Stack güncelleştirme platform güvenilirliği ve desteklenebilirliği geliştirmek için iki yeni altyapı rollerini ekler:

  - **Altyapı halkası**: Gelecekte, altyapı halka şu anda atanmış kendi altyapısı Vm'leri gerektiren var olan altyapı rolleri – örneğin xrp - kapsayıcılı sürümlerini barındıracak. Bu platform güvenilirliği artırmak ve altyapı Azure Stack gerektiren VM'lerin sayısını azaltın. Bu, daha sonra genel kaynak tüketimini Azure yığını'nın altyapısı rollerinin gelecekte de azaltır.
  - **Destek halkası**: Gelecekte destek halka müşteriler için gelişmiş destek senaryoları işlemek için kullanılır.  

  Ayrıca, bu rolün gelişmiş kullanılabilirlik için etki alanı denetleyicisi VM'SİNİN fazladan bir örneğini ekledik.

  Bu değişiklikler aşağıdaki yollarla Azure Stack altyapısının kaynak kullanımını artıracaktır:
  
    | Azure Stack SKU | İşlem tüketimi artırın | Bellek tüketimi artırın |
    | -- | -- | -- |
    |4 düğüm|22 vCPU|28 GB|
    |8 düğüm|38 vCPU|44 GB|
    |12 düğümleri|54 vCPU|60 GB|
    |16 düğüme|70 vCPU|76 GB|
  
- Artık Visual Studio Code üzerinde çalışan Azure Stack uzantı yoktur. Kullanarak **Azure hesabı** uzantısı, geliştiriciler hedef Azure Stack oturum açma ve diğer hizmetlerin sayısı yanı sıra, abonelikleri görüntüleme. Azure hesabı uzantısını hem AAD ortamları hem de ADFS ortamlarında çalışır ve yalnızca küçük bir değişiklik kullanıcı ayarlarında Visual Studio Code'un Azure Stack meta veri değerleri girmenizi gerektirir. Daha fazla bilgi için [belgelerine bakın](../user/azure-stack-dev-start-vscode-azure.md).

### <a name="changes"></a>Değişiklikler

- Planlanmış ve planlanmamış bakım senaryolarını sırasında güvenilirlik ve kullanılabilirliği artırmak için Azure Stack etki alanı Hizmetleri için ek altyapı rol örneği ekler.

- Bu güncelleştirme, onarma sırasında ve işlem düğümü, ekleme homojen ölçek bir ölçek birimi içinde birim düğümleri emin olmak için donanım doğrulanır.

- Zamanlanmış yedeklemeleri tamamlamak başarısız olan ve tanımlanan saklama süresi aşıldı, en az bir yedeğinizin korunur altyapısını yedekleme denetleyicisi sağlayacaktır. 

### <a name="fixes"></a>Düzeltmeleri

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there is an SR/ICM associated to it. -->

- Bir sorun, sabit bir **işlem konak Aracısı** uyarı görünen bir düğümü yeniden başlatıldıktan sonra ölçek birimi.

- Filtre uygulanan ve yayımcı filtrede yinelenen yayımcı adlarını gösterdi bağlandığınızda hatalı sonuçları gösterdi Yönetici portalında Market yönetiminde sorunlar düzeltildi. Ayrıca sonuçları daha hızlı bir şekilde görüntülemek için performans geliştirmeleri yapıldı.

- Dış depolama konumuna karşıya yükleme tamamlandı önce yeni bir kullanılabilir yedek listelenen kullanılabilir yedekleme dikey penceresinde bir sorun düzeltildi. Artık kullanılabilir yedekleme depolama konumuna başarıyla karşıya yüklendikten sonra listesinde gösterilir. 

<!-- ICM: 114819337; Task: 4408136 -->
- Yedekleme işlemi sırasında kurtarma anahtarlarını alma ile sorun düzeltildi. 

<!-- Bug: 4525587 -->
- OEM ilgili sorun düzeltildi 'undefined' işleci portalında görüntüleme sürüme güncelleştirin.

### <a name="security-updates"></a>Güvenlik güncelleştirmeleri

Azure Stack güvenlik güncelleştirmelerinde Bu güncelleştirme hakkında daha fazla bilgi için bkz. [Azure Stack güvenlik güncelleştirmeleri](azure-stack-release-notes-security-updates-1905.md).

## <a name="update-planning"></a>Planlama güncelleştir

Güncelleştirmeyi uygulamadan önce aşağıdaki bilgileri gözden geçirdiğinizden emin olun:

- [Bilinen sorunlar](azure-stack-release-notes-known-issues-1905.md)
- [Güvenlik güncelleştirmeleri](azure-stack-release-notes-security-updates-1905.md)
- [Önce ve güncelleştirmeyi uyguladıktan sonra etkinlikler listesi](azure-stack-release-notes-checklist.md)

## <a name="download-the-update"></a>Güncelleştirmeyi indirin

Azure Stack 1905 güncelleştirme paketinden indirebileceğiniz [Azure Stack indirme sayfasına](https://aka.ms/azurestackupdatedownload). Yükleyici aracını kullanırken son sürümü ve indirmeleri dizininizden önbelleğe alınmış bir kopya kullanmaya dikkat edin.

## <a name="hotfixes"></a>Düzeltmeler

Azure Stack düzeltmeleri düzenli olarak serbest bırakır. Azure Stack için 1905 güncelleştirmeden önce en son Azure Stack düzeltme için 1904 yüklediğinizden emin olun.

Azure Stack düzeltmeleri yalnızca Azure Stack tümleşik sistemleri için geçerlidir; üzerinde ASDK düzeltmelerini çalışmayın.

### <a name="before-applying-the-1905-update"></a>1905 uygulamadan önce güncelleştirme

Azure Stack 1905 sürüm aşağıdaki düzeltmeleri 1904 sürümle uygulanması gerekir:

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack düzeltme 1.1904.4.45](https://support.microsoft.com/help/4505688)

### <a name="after-successfully-applying-the-1905-update"></a>1905 başarıyla uygulandıktan sonra güncelleştir

Bu güncelleştirme yüklendikten sonra geçerli düzeltmeleri yükleyin. Daha fazla bilgi için müşterilerimize [hizmet İlkesi](azure-stack-servicing-policy.md).

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack düzeltme 1.1905.3.48](https://support.microsoft.com/help/4510078)

## <a name="automatic-update-notifications"></a>Otomatik güncelleştirme bildirimleri

Müşteriler altyapı ağdan internet erişimi olan sistemlerle **güncelleştirme kullanılabilir** işleci portalında iletisi. İnternet erişimi olmayan sistemleri, indirin ve karşılık gelen .xml ile .zip dosyasını içeri aktarabilirsiniz.

> [!TIP]  
> Aşağıdaki abone olmak *RSS* veya *Atom* Azure Stack düzeltmelerle birlikte kalmasını sağlamak için akışları:
>
> - [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss)
> - [Atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom)

## <a name="archived-release-notes"></a>Arşivlenmiş sürüm notları

Gördüğünüz [Azure Stack eski sürümlerinin sürüm notları TechNet Galerisi'ndeki](http://aka.ms/azsarchivedrelnotes). Bu sürüm notları yalnızca başvuru amacıyla sağlanmıştır ve bu sürümleri için destek anlamına gelmediğini arşivlenir. Azure Stack desteği hakkında daha fazla bilgi için bkz. [Azure Stack hizmet İlkesi](azure-stack-servicing-policy.md). Daha fazla yardım için Microsoft Müşteri Destek Hizmetleri'ne başvurun.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Stack'te güncelleştirme yönetimi genel bakış için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md).  
- Azure Stack güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için bkz. [güncelleştirmelerini Azure Stack'te](azure-stack-apply-updates.md).
- Azure Stack tümleşik sistemleri ve desteklenen bir duruma sisteminizi tutmak için yapmanız gerekenlere bakım ilkeyi gözden geçirmek için bkz: [Azure Stack hizmet İlkesi](azure-stack-servicing-policy.md).  
- Ayrıcalıklı uç noktasına (CESARETLENDİRİCİ) izlemek ve güncelleştirmelerini sürdürmek üzere kullanmak için bkz [izleme ayrıcalıklı uç noktayı kullanarak Azure stack'teki güncelleştirmeleri](azure-stack-monitor-update.md).  

