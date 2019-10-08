---
title: Azure Stack için kimlik mimarisi | Microsoft Docs
description: Azure Stack için kimlik mimarisi ve Azure AD ile AD FS arasındaki farklar hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/09/2019
ms.author: patricka
ms.reviewer: fiseraci
ms.lastreviewed: 05/09/2019
ms.openlocfilehash: 364028183445df7e74828605439bfd7b3784f01f
ms.sourcegitcommit: 451cfaa24b349393f36ae9d646d4d311a14dd1fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72019411"
---
# <a name="identity-architecture-for-azure-stack"></a>Azure Stack için kimlik mimarisi

Azure Stack ile kullanmak üzere bir kimlik sağlayıcısı seçerken, Azure Active Directory (Azure AD) ve Active Directory Federasyon Hizmetleri (AD FS) (AD FS) seçenekleri arasındaki önemli farklılıkları anlamalısınız.

## <a name="capabilities-and-limitations"></a>Yetenekler ve sınırlamalar

Seçtiğiniz kimlik sağlayıcısı, çok kiracılı destek dahil olmak üzere seçeneklerinizi sınırlayabilir.

|Yetenek veya senaryo        |Azure AD  |AD FS  |
|------------------------------|----------|-------|
|İnternet 'e bağlı     |Evet       |İsteğe Bağlı|
|Çoklu kiracı desteği     |Evet       |Hayır      |
|Market 'te teklif öğeleri |Evet       |Evet ( [çevrimdışı Market dağıtım](azure-stack-download-azure-marketplace-item.md#disconnected-or-a-partially-connected-scenario) aracı 'nın kullanılması gerekir)|
|Active Directory Authentication Library için destek (ADAL) |Evet |Evet|
|Azure CLı, Visual Studio ve PowerShell gibi araçlar için destek  |Evet |Evet|
|Azure portal aracılığıyla hizmet sorumluları oluşturma     |Evet |Hayır|
|Sertifikalarla hizmet sorumluları oluşturma      |Evet |Evet|
|Gizli dizileri olan hizmet sorumluları oluşturma (anahtarlar)    |Evet |Evet|
|Uygulamalar Graph hizmetini kullanabilir           |Evet |Hayır|
|Uygulamalar, oturum açma için kimlik sağlayıcısını kullanabilir |Evet |Evet (uygulamalar Şirket içi AD FS örnekleri ile federasyona eklemek için gereklidir) |

## <a name="topologies"></a>Topolojiler

Aşağıdaki bölümlerde kullanabileceğiniz farklı kimlik topolojileri ele alınmaktadır.

### <a name="azure-ad-single-tenant-topology"></a>Azure AD: tek kiracılı topoloji

Varsayılan olarak, Azure Stack yükleyip Azure AD kullandığınızda, Azure Stack tek kiracılı topoloji kullanır.

Tek kiracılı topoloji şu durumlarda yararlı olur:
- Tüm kullanıcılar aynı kiracının bir parçasıdır.
- Bir hizmet sağlayıcısı, bir kuruluş için Azure Stack örneğini barındırır.

![Azure AD ile tek kiracılı topoloji Azure Stack](media/azure-stack-identity-architecture/single-tenant.png)

Bu topoloji aşağıdaki özellikleri sunar:

- Azure Stack tüm uygulama ve hizmetleri aynı Azure AD kiracı dizinine kaydeder.
- Azure Stack, belirteçler de dahil olmak üzere yalnızca bu dizindeki kullanıcıların ve uygulamaların kimliğini doğrular.
- Yöneticiler (bulut İşletmenleri) ve kiracı kullanıcıları için kimlikler aynı dizin kiracısıdır.
- Bir kullanıcının başka bir dizinden bu Azure Stack ortamına erişmesini sağlamak için kullanıcıyı kiracı dizinine [Konuk olarak davet](azure-stack-identity-overview.md#guest-users) etmeniz gerekir.

### <a name="azure-ad-multi-tenant-topology"></a>Azure AD: çok kiracılı topoloji

Bulut işleçleri, bir veya daha fazla buluttan kiracılar tarafından uygulamalara erişime izin vermek için Azure Stack yapılandırabilir. Kullanıcılar Azure Stack Kullanıcı Portalı aracılığıyla uygulamalara erişir. Bu yapılandırmada, Yönetici portalı (bulut operatörü tarafından kullanılır), tek bir dizindeki kullanıcılarla sınırlıdır.

Çok kiracılı topoloji şu durumlarda yararlı olur:

- Bir hizmet sağlayıcısı, birden çok kuruluşun kullanıcılarının Azure Stack erişmesine izin vermek istiyor.

![Azure AD ile çok kiracılı topoloji Azure Stack](media/azure-stack-identity-architecture/multi-tenant.png)

Bu topoloji aşağıdaki özellikleri sunar:

- Kaynaklara erişim, kuruluş başına temelinde olmalıdır.
- Bir kuruluştan gelen kullanıcılar, kendi kuruluşu dışındaki kullanıcılara kaynaklara erişim izni veremez.
- Yöneticiler için kimlikler (bulut İşletmenleri), kullanıcılar için kimliklerden ayrı bir dizin kiracısında bulunabilir. Bu ayrım, kimlik sağlayıcısı düzeyinde hesap yalıtımı sağlar.
 
### <a name="ad-fs"></a>AD FS

Aşağıdaki koşullardan biri doğru olduğunda AD FS topolojisi gereklidir:

- Azure Stack internet 'e bağlanmıyor.
- Azure Stack internet 'e bağlanabilir ancak kimlik sağlayıcınız için AD FS kullanmayı tercih edebilirsiniz.
  
![AD FS kullanarak topoloji Azure Stack](media/azure-stack-identity-architecture/adfs.png)

Bu topoloji aşağıdaki özellikleri sunar:

- Üretimde bu topolojinin kullanımını desteklemek için, yerleşik Azure Stack AD FS örneğini, bir federasyon güveni aracılığıyla Active Directory tarafından desteklenen mevcut bir AD FS örneğiyle tümleştirmeniz gerekir.
- Grafik hizmetini Azure Stack mevcut Active Directory örneğiniz ile tümleştirebilirsiniz. Azure AD Graph API ile tutarlı API 'Leri destekleyen OData tabanlı Graph API hizmetini de kullanabilirsiniz.

  Active Directory örneğiniz ile etkileşim kurmak için Graph API, salt okuma izinlerine sahip Active Directory örneğinizin Kullanıcı kimlik bilgilerini gerektirir.
  - Yerleşik AD FS örneği Windows Server 2016 ' i temel alır.
  - AD FS ve Active Directory örneklerinizin Windows Server 2012 veya sonraki bir sürümü temel almalıdır.
  
  Active Directory örneğiniz ve yerleşik AD FS örneği arasında, etkileşimler OpenID Connect ile sınırlandırılmadı ve karşılıklı olarak desteklenen herhangi bir protokolü kullanabilirler.
  - Kullanıcı hesapları şirket içi Active Directory örneğiniz tarafından oluşturulup yönetilir.
  - Hizmet sorumluları ve uygulamalar için kayıtlar yerleşik Active Directory örneğinde yönetilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Kimliğe genel bakış](azure-stack-identity-overview.md)
- [Veri merkezi tümleştirmesi-kimlik](azure-stack-integrate-identity.md)
