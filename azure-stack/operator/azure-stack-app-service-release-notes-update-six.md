---
title: Azure Stack üzerinde App Service'te güncelleştirme 6 sürüm notları | Microsoft Docs
description: Güncelleştirmede ne olduğu hakkında Azure Stack'te App Service için altı, bilinen sorunlar ve güncelleştirmeyi yüklemek nereye öğrenin.
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
ms.date: 06/10/2019
ms.author: anwestg
ms.reviewer: ''
ms.openlocfilehash: d3464681463cfb66a368210beed79d5ef4c28739
ms.sourcegitcommit: af63214919e798901399fdffef09650de4176956
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66828325"
---
# <a name="app-service-on-azure-stack-update-6-release-notes"></a>Güncelleştirme 6 sürüm notları Azure Stack üzerinde App Service'e

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu sürüm notları, iyileştirmeler ve düzeltmeler Azure uygulama Hizmeti'nde Azure Stack güncelleştirme 6 ve tüm bilinen sorunlar açıklanmaktadır. Bilinen sorunlar doğrudan dağıtım, güncelleştirme işlemi ve sorunları (yükleme sonrası) yapı ile ilgili sorunlar ayrılır.

> [!IMPORTANT]
> Azure Stack tümleşik sisteminize 1904 güncelleştirmesini veya Azure App Service 1.6 dağıtmadan önce en son Azure Stack geliştirme Seti'ni dağıtın.


## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack güncelleştirme 6 derleme numarası üzerinde App Service, **82.0.1.50**

### <a name="prerequisites"></a>Önkoşullar

Başvurmak [önce Get Started belgeleri](azure-stack-app-service-before-you-get-started.md) dağıtımına başlamadan önce.

Azure Stack'te Azure App Service'in 1.6 için yükseltmeye başlamadan önce:

- Tüm roller hazır olduğundan emin olun Azure Stack Yönetici portalı'nda Azure App Service Yönetim

- App Service ve ana veritabanlarını yedekleme:
  - AppService_Hosting;
  - AppService_Metering;
  - Master

- Kiracı uygulama içerik dosya paylaşımını yedekleme

- Alanınızdaki **özel betik uzantısı** sürüm **1.9.1** marketten

### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler

Azure Stack güncelleştirme 6 Azure uygulama hizmeti, aşağıdaki geliştirmeleri ve düzeltmeleri içerir:

- Güncelleştirmeleri **App Service Kiracı, yönetici, İşlevler portalları ve Kudu Araçları**. Azure Stack portalı SDK sürümü ile tutarlı.

- Güncelleştirmeleri **Azure işlevleri çalışma zamanı** için **v1.0.12299**.

- Güvenilirlik ve sık karşılaşılan sorunları daha kolay tanılanması etkinleştirme hata geliştirmek için çekirdek hizmet güncelleştirmeleri.

- **Aşağıdaki uygulama çerçeveleri ve araçları güncelleştirmeleri**:
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
  - Güncelleştirilmiş Kudu 81.10329.3844 için

- **Tüm rollerin temel işletim sistemi güncelleştirmeleri**:
  - [2019-04-x64 tabanlı sistemleri (KB4493473) için Windows Server 2016 için toplu güncelleştirme](https://support.microsoft.com/help/4493473/windows-10-update-kb4493473)

### <a name="post-deployment-steps"></a>Dağıtım sonrası adımlar

> [!IMPORTANT]
> Bir SQL her zaman şirket örneği ile App Service kaynak sağlayıcısı sağlamışsanız gerekir [appservice_hosting ve appservice_metering veritabanlarını bir kullanılabilirlik grubuna ekleme](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/availability-group-add-a-database) ve kaybını önlemek için veritabanlarını eşitleme Hizmet veritabanı yük devretme durumunda.

### <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)

- Çalışanları App Service, var olan bir sanal ağda dağıtılır ve dosya sunucusu yalnızca Azure Stack dağıtım belgeleri üzerinde Azure App Service'te adlandırıldığı gibi özel ağda kullanılabilir dosya sunucusuna erişemiyor.

Mevcut bir sanal ağ ve dosya sunucunuza bağlanmak için bir dahili IP adresine dağıtmayı seçerseniz, çalışan alt ağ ve dosya sunucusu arasında SMB trafiği etkinleştirme bir giden güvenlik kuralı eklemeniz gerekir. Yönetim Portalı'nda WorkersNsg gidin ve aşağıdaki özelliklere sahip bir giden güvenlik kuralı ekleyin:
 * Kaynak: Tüm
 * Kaynak bağlantı noktası aralığı: *
 * Hedef: IP Adresleri
 * Hedef IP adresi aralığı: Dosya sunucusu için IP aralığı
 * Hedef bağlantı noktası aralığı: 445
 * Protokol: TCP
 * Eylem: İzin Ver
 * Önceliği: 700
 * Ad: Outbound_Allow_SMB445

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Azure Stack üzerinde Azure App Service'te çalışan bulut yöneticileri için bilinen sorunlar

Belgeye başvurun [Azure Stack 1904 sürüm notları](azure-stack-release-notes-1904.md)

### <a name="known-issues-for-tenants-deploying-applications-on-azure-app-service-on-azure-stack"></a>Azure Stack'te Azure App Service'te uygulamaları dağıtma kiracılar için bilinen sorunlar

- Dağıtım Merkezi devre dışı

Kiracılar, genel bulutta geç 2018'de sunulan bir özelliktir dağıtım merkezi kullanımı henüz yapamaz.  Kiracılar, portal, CLI ve PowerShell aracılığıyla standart dağıtım yöntemleri (FTP, Web dağıtımı, Git, vb.) kullanmaya devam edebilirsiniz.

- Dağıtım seçenekleri (Klasik) UX ve dağıtım kimlik bilgilerini portalı seçenekleri kullanılamıyor

Azure Stack dağıtımda, kiracılar dağıtım kimlik bilgileri kullanıcı deneyimleri ve dağıtım seçenekleri ulaşmak için erişim portalı kullanarak bu URL formatı - https://portal.&lt; *Bölge*&gt;.&lt; *FQDN*&gt;/? websitesExtension_oldvsts = true - olduğu için ASDK olacaktır [ https://portal.local.azurestack.external/?websitesExtension_oldvsts=true ](https://portal.local.azurestack.external/?websitesExtension_oldvsts=true) ve ardından, uygulamalarına normalde gidin.

## <a name="next-steps"></a>Sonraki adımlar

- Azure App Service'e genel bakış için bkz. [Azure App Service, Azure Stack genel bakış](azure-stack-app-service-overview.md).
- Azure Stack üzerinde App Service'e dağıtmak hazırlanması hakkında daha fazla bilgi için bkz. [Azure Stack'te App Service ile çalışmaya başlamadan önce](azure-stack-app-service-before-you-get-started.md).
