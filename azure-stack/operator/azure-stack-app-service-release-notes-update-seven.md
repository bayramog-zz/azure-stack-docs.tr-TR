---
title: Azure Stack güncelleştirme 7 sürüm notlarına App Service | Microsoft Docs
description: Azure Stack App Service için yedi güncelleştirme, bilinen sorunlar ve güncelleştirmenin indirileceği hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2019
ms.author: anwestg
ms.reviewer: ''
ms.openlocfilehash: 77fb9772f027d25b0d8c0d8355e3a868c2142d6c
ms.sourcegitcommit: d159652f50de7875eb4be34c14866a601a045547
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282610"
---
# <a name="app-service-on-azure-stack-update-7-release-notes"></a>App Service Azure Stack güncelleştirme 7 sürüm notları

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu sürüm notlarında, Azure App Service Azure Stack güncelleştirme 7 ve bilinen tüm sorunlar hakkında iyileştirmeler ve düzeltmeler açıklanır. Bilinen sorunlar doğrudan dağıtım, güncelleştirme işlemi ve derleme ile ilgili sorunlar (yükleme sonrası) ile ilgili sorunlara bölünmüştür.

> [!IMPORTANT]
> 1907 güncelleştirmesini Azure Stack tümleşik sisteminize uygulayın veya en son Azure Stack geliştirme setini Azure App Service 1,7 ' i dağıtmadan önce dağıtın.


## <a name="build-reference"></a>Derleme başvurusu

Azure Stack güncelleştirme 7 derleme numarasında App Service **84.0.2.10**

### <a name="prerequisites"></a>Prerequisites

Dağıtıma başlamadan önce, başlamadan [önce belgelerini](azure-stack-app-service-before-you-get-started.md) inceleyin.

Azure App Service Azure Stack 1,7 sürümüne yükseltmeye başlamadan önce:

- Tüm rollerin Azure Stack yönetici portalındaki Azure App Service yönetimi için kullanılabilir olduğundan emin olun

- App Service ve ana veritabanlarını yedekleyin:
  - AppService_Hosting;
  - AppService_Metering;
  - Şablonu

- Kiracı uygulaması içerik dosyası payını yedekleme

- Market 'ten **Özel Betik uzantısı** **1.9.3** sürümünü genel olarak

### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler

Azure Stack güncelleştirme 7 ' Azure App Service aşağıdaki geliştirmeleri ve düzeltmeleri içerir:

- [CVE-2019-1372](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2019-1372) uzaktan kod yürütme güvenlik açığı için çözüm

- **Kiracı, yönetici, işlevler portalları ve kudu araçları App Service**güncelleştirmeler. Azure Stack Portal SDK sürümü ile tutarlıdır.

- **Azure işlevleri çalışma zamanını** **v 1.0.12582**'a güncelleştirir.

- Genel sorunların daha kolay tanılanmasını sağlayan güvenilirlik ve hata iletilerini artırmak için çekirdek hizmette güncelleştirmeler.

- **Aşağıdaki uygulama çerçeveleri ve araçları Için güncelleştirmeler**:
  - ASP.NET Core 2.2.46
  - Zul OpenJDK 8.38.0.13
  - Tomcat 7.0.94
  - Tomcat 8.5.42
  - Tomcat 9.0.21
  - PHP 5.6.40
  - PHP 7.3.6
  - Kudu 82.10503.3890 'e güncelleştirildi

- **Tüm rollerin temeldeki işletim sisteminde güncelleştirmeler**:
  - [2019-08 x64 tabanlı sistemler için Windows Server 2016 toplu güncelleştirmesi (KB4512495)](https://support.microsoft.com/help/4512495)

- **Erişim kısıtlamaları artık Kullanıcı portalında etkinleştirilmiştir**:
  - Bu sürümden itibaren, kullanıcılar Web/API/Işlevleri uygulamalarına yönelik erişim kısıtlamalarını yayınlanan [Azure App Service erişim kısıtlamalarına](https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions)göre yapılandırabilir, **not**: Azure Stack Azure App Service desteklemez Hizmet uç noktaları.

- **Dağıtım seçenekleri (klasik) Işlevleri geri yüklendi**:
  - Kullanıcılar, uygulamasının GitHub, Bitbucket, Dropbox, OneDrive, yerel ve dış depolardan dağıtımını yapılandırmak ve uygulamalarına ait dağıtım kimlik bilgilerini ayarlamak için dağıtım seçenekleri 'ni (klasik) yeniden kullanabilir.

- **Azure Işlev izleme** doğru şekilde yapılandırıldı.

- **Windows Update davranış**: müşteri geri bildirimlerine bağlı olarak, güncelleştirme 7 ' deki App Service rollerde Windows Update yapılandırma biçimini değiştirdik:
  - Üç mod:
    - **Devre dışı** -Windows Update hizmeti devre dışı bırakıldı, Windows Azure Stack sürümlerde Azure App Service bırlıkte gelen KB ile güncelleştirilir;
    - **Otomatik** Windows Update hizmeti etkin ve Windows Update nasıl ve ne zaman güncelleyeceğini tespit eder;
    - **Yönetilen** -Windows Update hizmeti devre dışı Azure App Service, tek bir rolün OnStart işlemi sırasında bir Windows Update döngüsünü gerçekleştirecek.

  **Yeni** Dağıtımlar-Windows Update hizmet varsayılan olarak devre dışıdır.

  **Mevcut** Dağıtımlar-denetleyicideki ayarı değiştirdiyseniz, değer artık **false** Iken **devre dışı** olarak değişir ve önceki **true** değeri **Otomatik** olur

### <a name="post-deployment-steps"></a>Dağıtım sonrası adımlar

> [!IMPORTANT]
> App Service kaynak sağlayıcısını bir SQL Always on örneğiyle birlikte sağladıysanız, [appservice_hosting ve appservice_metering veritabanlarını bir kullanılabilirlik grubuna eklemeli](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/availability-group-add-a-database) ve şu durumda hizmet kaybını engellemek için VERITABANLARıNı eşitlemeniz gerekir veritabanı yük devretmesi.

### <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)

- App Service var olan bir sanal ağda dağıtıldığında ve dosya sunucusu yalnızca özel ağda kullanılabilir olduğunda, Azure Stack dağıtım belgelerindeki Azure App Service olarak çağrıldığında, çalışan dosya sunucusuna ulaşamamaktadır.

Dosya sunucunuza bağlanmak için mevcut bir sanal ağa ve bir iç IP adresine dağıtmayı seçerseniz, çalışan alt ağ ve dosya sunucusu arasında SMB trafiğini etkinleştirerek bir giden güvenlik kuralı eklemeniz gerekir. Yönetim portalında WorkersNsg adresine gidin ve aşağıdaki özelliklerle bir giden güvenlik kuralı ekleyin:
 * Kaynak: any
 * Kaynak bağlantı noktası aralığı: *
 * Hedef: IP adresleri
 * Hedef IP adresi aralığı: dosya sunucunuz için IP aralığı
 * Hedef bağlantı noktası aralığı: 445
 * Protokol: TCP
 * Eylem: Izin ver
 * Öncelik: 700
 * Ad: Outbound_Allow_SMB445

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Azure Stack üzerinde Cloud Admins işletim Azure App Service ilgili bilinen sorunlar

[Azure Stack 1907 sürüm notlarındaki](azure-stack-release-notes-1907.md) belgelere bakın

## <a name="next-steps"></a>Sonraki adımlar

- Azure App Service genel bakış için bkz. [Azure Stack genel bakış Azure App Service](azure-stack-app-service-overview.md).
- Azure Stack App Service dağıtmaya hazırlanma hakkında daha fazla bilgi için, bkz. App Service kullanmaya [başlamadan önce Azure Stack](azure-stack-app-service-before-you-get-started.md).
