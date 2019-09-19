---
title: Azure Stack güncelleştirme 6 sürüm notlarında App Service | Microsoft Docs
description: Azure Stack, bilinen sorunlar ve güncelleştirmenin indirileceği App Service için altı güncelleştirme hakkında bilgi edinin.
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
ms.date: 06/24/2019
ms.author: anwestg
ms.reviewer: ''
ms.openlocfilehash: 93ec7faec82fad101c7bab2d2cec2783c8a060da
ms.sourcegitcommit: c46d913ebfa4cb6c775c5117ac5c9e87d032a271
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71101013"
---
# <a name="app-service-on-azure-stack-update-6-release-notes"></a>App Service Azure Stack güncelleştirme 6 sürüm notları

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu sürüm notları, Azure App Service Azure Stack güncelleştirme 6 ' daki iyileştirmeler ve düzeltmeleri ve bilinen sorunları anlatmaktadır. Bilinen sorunlar doğrudan dağıtım, güncelleştirme işlemi ve derleme ile ilgili sorunlar (yükleme sonrası) ile ilgili sorunlara bölünmüştür.

> [!IMPORTANT]
> 1904 güncelleştirmesini Azure Stack tümleşik sisteminize uygulayın veya en son Azure Stack geliştirme setini Azure App Service 1,6 ' i dağıtmadan önce dağıtın.


## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack güncelleştirme 6 derleme numarasında App Service **82.0.1.50**

### <a name="prerequisites"></a>Önkoşullar

Dağıtıma başlamadan önce, başlamadan [önce belgelerini](azure-stack-app-service-before-you-get-started.md) inceleyin.

Azure App Service Azure Stack 1,6 sürümüne yükseltmeye başlamadan önce:

- Tüm rollerin Azure Stack yönetici portalındaki Azure App Service yönetimi için kullanılabilir olduğundan emin olun

- App Service ve ana veritabanlarını yedekleyin:
  - AppService_Hosting;
  - AppService_Metering;
  - Ana Şablon

- Kiracı uygulaması içerik dosyası payını yedekleme

- Market 'ten **Özel Betik uzantısı** **1.9.1** sürümünü genel olarak

### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler

Azure Stack güncelleştirme 6 ' Azure App Service, aşağıdaki geliştirmeleri ve düzeltmeleri içerir:

- **Kiracı, yönetici, işlevler portalları ve kudu araçları App Service**güncelleştirmeler. Azure Stack Portal SDK sürümü ile tutarlıdır.

- **Azure işlevleri çalışma zamanını** **v 1.0.12299**'a güncelleştirir.

- Genel sorunların daha kolay tanılanmasını sağlayan güvenilirlik ve hata iletilerini artırmak için çekirdek hizmette güncelleştirmeler.

- **Aşağıdaki uygulama çerçeveleri ve araçları Için güncelleştirmeler**:
  - ASP.NET Core 2.2.4
  - NodeJS 10.15.2
  - Zulu OpenJDK 8.36.0.1
  - Tomcat 7.0.81
  - Tomcat 8.5.37
  - Tomcat 9.0.14
  - PHP 5.6.39
  - PHP 7.0.33
  - PHP 7.1.25
  - PHP 7.2.13
  - Kudu 81.10329.3844 'e güncelleştirildi

- **Tüm rollerin temeldeki işletim sisteminde güncelleştirmeler**:
  - [2019-04 x64 tabanlı sistemler için Windows Server 2016 toplu güncelleştirmesi (KB4493473)](https://support.microsoft.com/help/4493473/windows-10-update-kb4493473)

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

[Azure Stack 1908 sürüm notlarındaki](/azure-stack/operator/release-notes?view=azs-1908) belgelere bakın

### <a name="known-issues-for-tenants-deploying-applications-on-azure-app-service-on-azure-stack"></a>Azure Stack üzerinde Azure App Service uygulamalar dağıtan kiracılar için bilinen sorunlar

- Dağıtım Merkezi gri

Kiracılar, en geç 2018 ' de genel bulutta yayınlanan bir özellik olan dağıtım merkezini henüz kullanamaz.  Kiracılar Portal, CLı ve PowerShell aracılığıyla standart dağıtım yöntemlerini (FTP, Web Dağıtımı, git, vb.) kullanmaya devam edebilir.

- Dağıtım seçenekleri (klasik) UX ve dağıtım kimlik bilgileri portalı seçenekleri kullanılamıyor

Azure Stack dağıtımında Dağıtım seçeneklerine ve dağıtım kimlik bilgileri Kullanıcı deneyimlerine ulaşmak için kiracılar bu URL biçimini https://portal.&lt kullanarak portala erişmelidir; *bölge.* &gt;&lt; *FQDN* /? websitesExtension_oldvsts = true (asdk [https://portal.local.azurestack.external/?websitesExtension_oldvsts=true](https://portal.local.azurestack.external/?websitesExtension_oldvsts=true) için) ve ardından uygulamalarına normal olarak gider. &gt;

- Azure Işlev Izleme sürekli olarak portalda "yükleniyor" olarak gösteriliyor

Bağımsız Işlevleri izlemeye çalıştığınızda, kiracı portalında hiçbir çağrı günlüğü, başarı sayısı veya hata sayısı görmezsiniz.  Bu işlevi yeniden etkinleştirmek için **işlev uygulaması**gidin, **platform özellikleri**' ne gidin ve **uygulama ayarları**' na gidin.  Yeni bir uygulama ayarı ekleyin-ad **AzureWebJobsDashboard** ve değeri AzureWebJobsStorage ' de ayarla aynı değere ayarlayın.  Sonra işlevinizdeki Izleyici görünümüne gidin ve izleme bilgilerini görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

- Azure App Service genel bakış için bkz. [Azure Stack genel bakış Azure App Service](azure-stack-app-service-overview.md).
- Azure Stack App Service dağıtmaya hazırlanma hakkında daha fazla bilgi için, bkz. App Service kullanmaya [başlamadan önce Azure Stack](azure-stack-app-service-before-you-get-started.md).
