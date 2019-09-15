---
title: App Service Azure Stack güncelleştirme 2 sürüm notları | Microsoft Docs
description: Azure Stack App Service için güncelleştirme 2 ' deki geliştirmeler, düzeltmeler ve bilinen sorunlar hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: bryanla
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 05/18/2018
ms.openlocfilehash: f427a31001f8f486fd231af7e59ef2bb30592661
ms.sourcegitcommit: 245a4054a52e54d5989d6148fbbe386e1b2aa49c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70974801"
---
# <a name="app-service-on-azure-stack-update-2-release-notes"></a>App Service Azure Stack güncelleştirme 2 sürüm notları

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu sürüm notları, Azure Stack güncelleştirme 2 ' de Azure App Service geliştirmeleri, düzeltmeleri ve bilinen sorunları anlatmaktadır. Bilinen sorunlar üç bölüme ayrılmıştır: doğrudan dağıtım, güncelleştirme işlemiyle ilgili sorunlar ve yapıyla ilgili sorunlar (yükleme sonrası).

> [!IMPORTANT]
> 1804 güncelleştirmesini Azure Stack tümleşik sisteminize uygulayın veya Azure App Service 1,2 ' i dağıtmadan önce en son Azure Stack Geliştirme Seti (ASDK) dağıtın.

## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack güncelleştirme 2 yapı numarasında App Service **72.0.13698.10**.

### <a name="prerequisites"></a>Önkoşullar

> [!IMPORTANT]
> Azure Stack Azure App Service yeni dağıtımları artık, kudu için SSO 'nun Azure App Service işlendiği şekilde iyileştirmeler nedeniyle [üç konuyla ilgili bir joker sertifikaya](azure-stack-app-service-before-you-get-started.md#get-certificates) gerek duyar. Yeni Konu:  **\*. SSO.\< appservice. Bölge\>.\< DomainName\>.\< uzantının\>**

Dağıtıma başlamadan önce [Azure Stack App Service dağıtmaya yönelik önkoşulları](azure-stack-app-service-before-you-get-started.md) inceleyin.

### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler

Azure Stack güncelleştirme 2 ' Azure App Service aşağıdaki geliştirmeleri ve düzeltmeleri içerir:

- **Kiracı, yönetici, işlevler portalları ve kudu araçları App Service**güncelleştirmeler. Azure Stack Portal SDK sürümü ile tutarlıdır.

- **Azure işlevleri çalışma zamanını** **v 1.0.11612**'a güncelleştirir.

- Genel sorunların daha kolay tanılanmasını sağlayan güvenilirlik ve hata iletilerini artırmak için çekirdek hizmette güncelleştirmeler.

- **Aşağıdaki uygulama çerçeveleri ve araçları Için güncelleştirmeler**:
  - .NET Framework 4.7.1 eklendi
  - **Node. js** sürümleri eklendi:
    - NodeJS 6.12.3
    - NodeJS 8.9.4
    - NodeJS 8.10.0
    - NodeJS 8.11.1
  - **NPM** sürümleri eklendi:
    - 5.6.0
  - .NET Core bileşenleri, genel buluttaki Azure App Service tutarlı olacak şekilde güncelleştirildi.
  - Kudu güncelleştirildi

- Dağıtım yuvaları özelliğinin Otomatik takas özelliği etkin- [otomatik değiştirmeyi yapılandırma](https://docs.microsoft.com/azure/app-service/deploy-staging-slots#configure-auto-swap).

- Üretim özelliği etkinken test etme- [üretimde teste giriş](https://azure.microsoft.com/resources/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/).

- Azure İşlev Proxy'leri etkin- [Azure işlev proxy'leri çalışın](https://docs.microsoft.com/azure/azure-functions/functions-proxies).

- İçin App Service Yönetici uzantısı UX desteği eklendi:
  - Gizli anahtar döndürme
  - Sertifika döndürme
  - Sistem kimlik bilgisi döndürme
  - Bağlantı dizesi döndürme

### <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)

- Mevcut bir sanal ağda App Service dağıtıldığında ve dosya sunucusu yalnızca özel ağda kullanılabilir olduğunda çalışanlar dosya sunucusuna ulaşamamakta olur.

Dosya sunucunuza bağlanmak için mevcut bir sanal ağa ve bir iç IP adresine dağıtmayı seçerseniz, çalışan alt ağ ve dosya sunucusu arasında SMB trafiği sağlayan bir giden güvenlik kuralı eklemeniz gerekir. Yönetici portalında WorkersNsg adresine gidin ve aşağıdaki özelliklerle bir giden güvenlik kuralı ekleyin:

* Kaynak: Any
* Kaynak bağlantı noktası aralığı: *
* Hedef: IP adresleri
* Hedef IP adresi aralığı: Dosya sunucunuz için IP aralığı
* Hedef bağlantı noktası aralığı: 445
* Protokol: TCP
* Eylem: Allow
* Öncelik: 700
* Ad: Outbound_Allow_SMB445

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Azure Stack üzerinde Cloud Admins işletim Azure App Service ilgili bilinen sorunlar

[Azure Stack 1804 sürüm notlarındaki](azure-stack-update-1903.md) belgelere bakın

## <a name="next-steps"></a>Sonraki adımlar

- Azure App Service genel bakış için bkz. [Azure Stack genel bakış Azure App Service](azure-stack-app-service-overview.md).
- Azure Stack App Service dağıtmaya hazırlanma hakkında daha fazla bilgi için bkz. [Azure Stack üzerinde App Service dağıtmak Için Önkoşullar](azure-stack-app-service-before-you-get-started.md).
