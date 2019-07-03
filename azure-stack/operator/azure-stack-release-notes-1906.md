---
title: Azure Stack 1906 sürüm notları | Microsoft Docs
description: Yenilikler dahil olmak üzere, Azure Stack tümleşik sistemleri 1906 güncelleştirmesi hakkında bilinen sorunlar ve güncelleştirmeyi yüklemek nereye öğrenin.
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
ms.date: 07/01/2019
ms.author: sethm
ms.reviewer: hectorl
ms.lastreviewed: 07/01/2019
ms.openlocfilehash: cc348bba064ddb003bbe6a2a06d2694714ad7894
ms.sourcegitcommit: 068350a79805366e7e6536fb7df85a412bd0be99
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67511270"
---
# <a name="azure-stack-1906-update"></a>Azure Stack 1906 güncelleştirme

*Uygulama hedefi: Azure Stack tümleşik sistemleri*

Bu makalede 1906 güncelleştirme paketinin içeriğini açıklar. Yenilikler güncelleştirme içerir. iyileştirmeler ve düzeltmeler bu sürümü, Azure Stack için.

> [!IMPORTANT]  
> Yalnızca Azure Stack tümleşik sistemleri için bu güncelleştirme paketidir. Bu güncelleştirme paketi için Azure Stack geliştirme Seti'ni geçerli değildir.

## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack 1906 güncelleştirmenin yapı numarasıdır **1.1906.0.30**.

### <a name="update-type"></a>Güncelleştirme türü

Azure Stack 1906 güncelleştirme derleme türü **Express**. Derleme türleri güncelleştirme hakkında daha fazla bilgi için bkz: [Azure Stack'te güncelleştirmeleri yönetme](azure-stack-updates.md) makalesi. 1906 güncelleştirmenin tamamlanması için beklenen süresini, Azure Stack ortamınıza fiziksel düğüm sayısından bağımsız olarak yaklaşık 10 saat ' dir. Tam güncelleştirme çalışma zamanları genellikle sisteminizde kullanılan Kiracı İş yükleri tarafından (internet'e bağlı değilse), sistem ağ bağlantısı kapasitesine bağlıdır ve, sistemin donanım belirtimleri. Beklenen değer daha uzun uzun çalışma zamanları nadir değildir ve güncelleştirme başarısız sürece eylem Azure Stack operatörleri tarafından gerektirmez. Bu çalışma zamanı yaklaşık 1906 güncelleştirmeye özgüdür ve diğer Azure Stack güncelleştirmeler karşılaştırılmalıdır değil.

## <a name="whats-in-this-update"></a>Bu güncelleştirmede nedir

<!-- The current theme (if any) of this release. -->

<!-- What's new, also net new experiences and features. -->

- Eklenen bir **kümesi TLSPolicy** cmdlet'i ayrıcalıklı uç noktasını tüm uç noktaları TLS 1.2 zorlama (CESARETLENDİRİCİ). Daha fazla bilgi için [Azure Stack güvenlik denetimleri](azure-stack-security-configuration.md).

- Eklenen bir **Get-TLSPolicy** cmdlet'i ayrıcalıklı uç noktasını (CESARETLENDİRİCİ uygulanan TLS ilkeyi almak için). Daha fazla bilgi için [Azure Stack güvenlik denetimleri](azure-stack-security-configuration.md).

- Sistem Güncelleştirme sırasında gerekli olarak iç TLS sertifikalarını döndürmek için bir iç gizli döndürme prosedürünün eklendi.

- İç gizli dizilerinin sona erme süresi dolacak gizli diziler üzerinde önemli bir uyarı göz ardı edilir durumunda iç gizli anahtarları döndürme zorlayarak önlemek için bir önlem eklendi. Bu üzerinde bir normal işletim yordamı kullanılmamalıdır. Gizli dizileri döndürme bir bakım penceresi sırasında planlanması gerekir. Daha fazla bilgi için [Azure Stack gizli dönüş](azure-stack-rotate-secrets.md).

- Visual Studio Code ile AD FS kullanarak Azure Stack dağıtımı artık desteklenmektedir.

### <a name="improvements"></a>Geliştirmeleri

<!-- Changes and product improvements with tangible customer-facing value. -->

- **Get-GraphApplication** ayrıcalıklı uç noktasını cmdlet artık şu anda kullanılan sertifikanın parmak izini görüntüler. AD FS ile Azure Stack dağıtıldığında bu hizmet sorumluları için sertifika yönetimi artırır.

- Yeni sistem durumu izleme kuralları, AD Graph ve AD FS, oluşturulacak olanağı dahil olmak üzere kullanılabilirliğini doğrulamak için eklendi.

- Yedekleme kaynak sağlayıcısı altyapı yedekleme hizmeti başka bir örneğine taşındığında güvenilirlik geliştirmeleri.

- Bakım penceresi zamanlaması kolaylaştırmak için Tekdüzen bir yürütme süresi sağlamak için dış gizli döndürme prosedürünün performans iyileştirme.

- **Test AzureStack** cmdlet'i artık (kritik uyarılar) dolmak üzere olan iç parolaları raporlar.

- Yeni bir parametre için kullanılabilir **Register-CustomAdfs** cmdlet'i ayrıcalıklı uç noktasını sağlayan AD FS için federasyon güveni yapılandırırken denetimi sertifika iptal listesi atlanıyor.

- Güncelleştirmeleri değil duraklatma olabilirsiniz şekilde 1906 yayın güncelleştirme ilerleme durumunu, daha fazla görünürlük sunar. İşleçler için gösterilen güncelleştirme adımların toplam sayısı arasında bir artış sonuçlanır **güncelleştirme** dikey penceresi. Ayrıca daha fazla bilgi görebilirsiniz güncelleştirme önceki güncelleştirmelerin paralel'olmuyor adımları.

#### <a name="networking-updates"></a>Ağ güncelleştirmeleri

- Güncelleştirilmiş kira süresi, DHCP Yanıtlayıcı Azure ile tutarlı olacak şekilde ayarlayın.

- Kaynakların başarısız dağıtım senaryosunda kaynak sağlayıcısı için geliştirilmiş yeniden deneme hızları.

- Kaldırılan **standart** SKU, yük dengeleyici hem genel IP olarak şu anda desteklenmeyen seçenek.

### <a name="changes"></a>Değişiklikler

- Bir depolama hesabı deneyimi oluşturmak artık Azure ile tutarlı olur.

- Uyarı tetiklenmeden iç gizli dizilerinin sona erme için değiştirilmiştir:
  - Uyarı bildirimleri artık 90 gün önce sona erme tarihini gizli anahtarları oluşturulur.
  - Kritik uyarılar artık 30 gün önce sona erme tarihini gizli anahtarları oluşturulur.

- Tutarlı terminolojisi altyapı yedekleme kaynak sağlayıcısının güncelleştirilmiş dizeleri.

### <a name="fixes"></a>Düzeltmeleri

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there is an SR/ICM associated to it. -->

- Bir yönetilen diskin VM yeniden boyutlandırma başarısız olduğu ile bir sorun düzeltildi bir **iç işlem hatası**.

- Bir sorun düzeltildi: başarısız kullanıcı görüntüsü oluşturma puts görüntüleri yöneten hizmet hatalı bir durumda olduğunda; Bu, başarısız görüntünün silme ve yeni görüntüleri oluşturulmasını engeller. Bu ayrıca 1905 düzeltmeyi sabittir.

- İç süresi dolacak gizli diziler üzerinde etkin uyarılar artık iç gizli döndürme başarılı yürütme sonrasında otomatik olarak kapatılır.

- Güncelleştirme 99 saatten uzun bir süre çalışıyorsa ilk basamak güncelleştirme geçmişi sekmesinden güncelleştirme süresi içinde trim bir sorun düzeltildi.

- **Güncelleştirme** dikey içeren bir **sürdürme** başarısız güncelleştirmeleri seçeneği.

- Azure Stack'te kullanılamadığından yönetici ve Kullanıcı Portalı, Docker uzantısını yanlış arama ancak başka bir işlem iade edildiği Market'te sorun düzeltildi, alınması.

- Şablon dağıtımı şablon adı '_' alt çizgiyle başlıyorsa parametreleri doldurmaz UI içinde bir sorun düzeltildi.

## <a name="security-updates"></a>Güvenlik güncelleştirmeleri

Azure Stack güvenlik güncelleştirmelerinde Bu güncelleştirme hakkında daha fazla bilgi için bkz. [Azure Stack güvenlik güncelleştirmeleri](azure-stack-release-notes-security-updates-1906.md).

## <a name="update-planning"></a>Planlama güncelleştir

Güncelleştirmeyi uygulamadan önce aşağıdaki bilgileri gözden geçirdiğinizden emin olun:

- [Bilinen sorunlar](azure-stack-release-notes-known-issues-1906.md)
- [Güvenlik güncelleştirmeleri](azure-stack-release-notes-security-updates-1906.md)
- [Önce ve güncelleştirmeyi uyguladıktan sonra etkinlikler listesi](azure-stack-release-notes-checklist.md)

## <a name="download-the-update"></a>Güncelleştirmeyi indirin

Azure Stack 1906 güncelleştirme paketinden indirebileceğiniz [Azure Stack indirme sayfasına](https://aka.ms/azurestackupdatedownload).

## <a name="hotfixes"></a>Düzeltmeler

Azure Stack düzeltmeleri düzenli olarak serbest bırakır. Azure Stack için 1906 güncelleştirmeden önce en son Azure Stack düzeltme için 1905 yüklediğinizden emin olun.

Azure Stack düzeltmeleri yalnızca Azure Stack tümleşik sistemleri için geçerlidir; üzerinde ASDK düzeltmelerini çalışmayın.

### <a name="before-applying-the-1906-update"></a>1906 uygulamadan önce güncelleştirme

Azure Stack 1906 sürüm aşağıdaki düzeltmeleri 1905 sürümle uygulanması gerekir:

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack düzeltme 1.1905.3.48](https://support.microsoft.com/help/4510078)

### <a name="after-successfully-applying-the-1906-update"></a>1906 başarıyla uygulandıktan sonra güncelleştir

Bu güncelleştirme yüklendikten sonra geçerli düzeltmeleri yükleyin. Daha fazla bilgi için müşterilerimize [hizmet İlkesi](azure-stack-servicing-policy.md).

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack düzeltme 1.1906.2.37](https://support.microsoft.com/help/4511282)

## <a name="automatic-update-notifications"></a>Otomatik güncelleştirme bildirimleri

Müşteriler altyapı ağdan internet erişimi olan sistemlerle **güncelleştirme kullanılabilir** işleci portalında iletisi. İnternet erişimi olmayan sistemleri, indirin ve karşılık gelen .xml ile .zip dosyasını içeri aktarabilirsiniz.

> [!TIP]  
> Aşağıdaki abone olmak *RSS* veya *Atom* Azure Stack düzeltmelerle birlikte kalmasını sağlamak için akışları:
>
> - [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss)
> - [Atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom)

## <a name="archived-release-notes"></a>Arşivlenmiş sürüm notları

Gördüğünüz [Azure Stack eski sürümlerinin sürüm notları TechNet Galerisi'ndeki](https://aka.ms/azsarchivedrelnotes). Bu sürüm notları yalnızca başvuru amacıyla sağlanmıştır ve bu sürümleri için destek anlamına gelmediğini arşivlenir. Azure Stack desteği hakkında daha fazla bilgi için bkz. [Azure Stack hizmet İlkesi](azure-stack-servicing-policy.md). Daha fazla yardım için Microsoft Müşteri Destek Hizmetleri'ne başvurun.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Stack'te güncelleştirme yönetimi genel bakış için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md).  
- Azure Stack güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için bkz. [güncelleştirmelerini Azure Stack'te](azure-stack-apply-updates.md).
- Azure Stack tümleşik sistemleri ve desteklenen bir duruma sisteminizi tutmak için yapmanız gerekenlere bakım ilkeyi gözden geçirmek için bkz: [Azure Stack hizmet İlkesi](azure-stack-servicing-policy.md).  
- Ayrıcalıklı uç noktasına (CESARETLENDİRİCİ) izlemek ve güncelleştirmelerini sürdürmek üzere kullanmak için bkz [izleme ayrıcalıklı uç noktayı kullanarak Azure stack'teki güncelleştirmeleri](azure-stack-monitor-update.md).  
