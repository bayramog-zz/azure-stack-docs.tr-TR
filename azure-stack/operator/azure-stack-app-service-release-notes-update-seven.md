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
ms.date: 08/29/2019
ms.author: anwestg
ms.reviewer: ''
ms.openlocfilehash: ec07277043068835d1d1d5a41285ee5df5ee7691
ms.sourcegitcommit: 701685f0b59e5a3d1a8d39fe477b8df701a51cd2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70165345"
---
# <a name="app-service-on-azure-stack-update-7-release-notes"></a>App Service Azure Stack güncelleştirme 7 sürüm notları

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu sürüm notlarında, Azure App Service Azure Stack güncelleştirme 7 ve bilinen tüm sorunlar hakkında iyileştirmeler ve düzeltmeler açıklanır. Bilinen sorunlar doğrudan dağıtım, güncelleştirme işlemi ve derleme ile ilgili sorunlar (yükleme sonrası) ile ilgili sorunlara bölünmüştür.

> [!IMPORTANT]
> 1907 güncelleştirmesini Azure Stack tümleşik sisteminize uygulayın veya en son Azure Stack geliştirme setini Azure App Service 1,7 ' i dağıtmadan önce dağıtın.


## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack güncelleştirme 7 derleme numarasında App Service **84.0.2.10**

### <a name="prerequisites"></a>Önkoşullar

Dağıtıma başlamadan önce, başlamadan [önce belgelerini](azure-stack-app-service-before-you-get-started.md) inceleyin.

Azure App Service Azure Stack 1,7 sürümüne yükseltmeye başlamadan önce:

- Tüm rollerin Azure Stack yönetici portalındaki Azure App Service yönetimi için kullanılabilir olduğundan emin olun

- App Service ve ana veritabanlarını yedekleyin:
  - AppService_Hosting;
  - AppService_Metering;
  - Ana Şablon

- Kiracı uygulaması içerik dosyası payını yedekleme

- Market 'ten **Özel Betik uzantısı** **1.9.3** sürümünü genel olarak

### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler

Azure Stack güncelleştirme 7 ' Azure App Service aşağıdaki geliştirmeleri ve düzeltmeleri içerir:

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
  - Bu sürümden itibaren kullanıcılar Web/API/Işlevleri uygulamalarına yönelik erişim kısıtlamalarını yayınlanan [Azure App Service erişim kısıtlamalarına](https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions)göre yapılandırabilir. Azure Stack Azure App Service hizmet uç noktalarını desteklemez.

- **Dağıtım seçenekleri (klasik) Işlevleri geri yüklendi**:
  - Kullanıcılar, uygulamasının GitHub, Bitbucket, Dropbox, OneDrive, yerel ve dış depolardan dağıtımını yapılandırmak ve uygulamalarına ait dağıtım kimlik bilgilerini ayarlamak için dağıtım seçenekleri 'ni (klasik) yeniden kullanabilir.

- **Azure Işlev izleme** doğru şekilde yapılandırıldı.

- **Windows Update davranışı**: Müşteri geri bildirimlerine bağlı olarak, Windows Update güncelleştirme 7 ' den App Service rollerde yapılandırılan şekilde değiştirilmiştir:
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
 * Kaynak: Any
 * Kaynak bağlantı noktası aralığı: *
 * Hedef: IP Adresleri
 * Hedef IP adresi aralığı: Dosya sunucunuz için IP aralığı
 * Hedef bağlantı noktası aralığı: 445
 * Protokol: TCP
 * Eylem: Allow
 * Öncelik: 700
 * Ad: Outbound_Allow_SMB445

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Azure Stack üzerinde Cloud Admins işletim Azure App Service ilgili bilinen sorunlar

[Azure Stack 1907 sürüm notlarındaki](azure-stack-release-notes-1907.md) belgelere bakın

## <a name="next-steps"></a>Sonraki adımlar

- Azure App Service genel bakış için bkz. [Azure Stack genel bakış Azure App Service](azure-stack-app-service-overview.md).
- Azure Stack App Service dağıtmaya hazırlanma hakkında daha fazla bilgi için, bkz. App Service kullanmaya [başlamadan önce Azure Stack](azure-stack-app-service-before-you-get-started.md).
