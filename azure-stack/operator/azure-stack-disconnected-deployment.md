---
title: Azure bağlantısı kesilmiş dağıtım kararları için Azure Stack tümleşik sistemleri | Microsoft Docs
description: Dağıtım kararları Azure Stack Azure bağlı çok düğümlü dağıtımlar için planlama saptayın.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: mabrigg
ms.reviewer: wfayed
ms.lastreviewed: 12/11/2018
ms.openlocfilehash: df0989886d34581950eca04ebe63104d477d003f
ms.sourcegitcommit: 2a4321a9cf7bef2955610230f7e057e0163de779
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65618010"
---
# <a name="azure-disconnected-deployment-planning-decisions-for-azure-stack-integrated-systems"></a>Azure bağlantısı kesilmiş dağıtım planlama kararları için Azure Stack tümleşik sistemleri
Karar verdim sonra [Azure Stack, hibrit bulut ortamına nasıl tümleştirilecek](azure-stack-connection-models.md), Azure Stack dağıtım kararlarınızı son haline getir.

Dağıtma ve Azure Stack internet bağlantısı olmadan kullanın. Ancak, bağlantısı kesilmiş bir dağıtım ile bir AD FS kimlik deposunu ve kapasite tabanlı faturalandırma modeli için sınırlı olursunuz. Çok kiracılı mimari Azure Active Directory (Azure AD) gerektirir, çünkü çok kiracılı mimari bağlantısı kesilmiş dağıtımları için desteklenmiyor. 

Bu seçeneği belirleyin:
- Güvenlik veya Internet'e bağlı olmayan bir ortamda Azure Stack dağıtmak ihtiyaç duyduğunuz başka kısıtlamalar vardır.
- (Kullanım verileri de dahil) engellemek Azure'da gönderilmesini istersiniz.
- Yalnızca şirket intranetinize dağıtılır ve karma senaryolar ilginizi olmayan bir özel bulut çözümü olarak Azure Stack kullanmak istiyorsunuz.

> [!TIP]
> Bazı durumlarda, bu ortam türünü de verilir bir *Denizaltı senaryo*.

Bağlantısı kesilmiş bir dağıtım, daha sonra Azure Stack Örneğiniz için Azure hibrit Kiracı VM senaryoları için bağlanmasını kısıtlamaz. Dağıtım sırasında Azure bağlantısı yoksa veya Azure AD, kimlik deposu olarak kullanmak istemediğiniz anlamına gelir.

## <a name="features-that-are-impaired-or-unavailable-in-disconnected-deployments"></a>Engelli ya da kaldırılmış bağlantısı kesilmiş dağıtımları olan özellikler 
Azure Stack, bazı özellikler ve İşlevler engelli veya bağlantı kesik modda tamamen kullanılamaz olduğunu dikkate almak önemlidir, Azure'a bağlıyken en iyi çalışacak şekilde tasarlanmıştır. 

|Özellik|Bağlantı kesik modda etkisi|
|-----|-----|
|VM Dağıtım sonrası yapılandırma DSC uzantısı ile VM dağıtımı|Engelli - DSC uzantısı için en son WMF Internet'e benzer.|
|Docker komutlarını çalıştırmak için Docker uzantısı ile VM dağıtımı|Engelli - Docker İnternet'e en son sürümünü kontrol eder ve bu denetimi başarısız olur.|
|Azure Stack portalı belgeleri bağlantıları|Yok - bağlantılar gibi geri bildirim vermek, Yardım, hızlı, vb. kullanan bir Internet URL çalışmaz.|
|Uyarı düzeltme/çevrimiçi düzeltme Kılavuzu başvuran azaltma|Yok - Internet URL çalışmaz kullanan herhangi bir uyarı düzeltme bağlar.|
|Market - seçin ve doğrudan Azure Marketi'nden galeri paketleri ekleme olanağı|Engelli - bağlantısız bir modda (olmadan, herhangi bir Internet bağlantısı) Azure Stack dağıtımı yaptığınızda Azure Stack portalını kullanarak Market öğelerini indirme olamaz. Ancak, kullanabileceğiniz [Market dağıtım aracı](azure-stack-download-azure-marketplace-item.md) internet bağlantısı olan bir makineye Market öğelerini indirme ve sonra da bunları Azure Stack ortamınıza aktarın.|
|Azure Stack dağıtımı yönetmek için Azure Active Directory Federasyon hesaplarını kullanma|Kullanılamıyor - bu özellik Azure bağlantısı gerektirir. Bunun yerine yerel bir Active Directory örneği ile AD FS kullanılması gerekir.|
|Uygulama Hizmetleri|Engelli - Web uygulamaları için güncelleştirilmiş içerik Internet erişimi gerektirebilir.|
|Komut Satırı Arabirimi (CLI)|CLI, engelli - kimlik doğrulaması ve hizmet sorumluları, sağlama açısından işlevselliği azalttı.|
|Visual Studio - bulut bulma|Engelli - Cloud Discovery, ya da farklı Bulutlar bulur veya hiç çalışmaz.|
|Visual Studio - AD FS|Visual Studio Enterprise'ı AD FS destekler yalnızca - görme.
Telemetri|Yok - Azure Stack için Telemetri verilerini telemetri verilerine bağlı her üçüncü taraf Galerisi paketler.|
|Sertifikalar|Sertifika iptal listesi (CRL) ve çevrimiçi sertifika durumu Protokolü (OSCP) hizmeti HTTPS bağlamında kullanılamıyor - Internet bağlantısı gereklidir.|
|Anahtar kasası|Key Vault için yaygın bir kullanım örneği, engelli - uygulamanın çalışma zamanında gizli anahtarları okumak zorunda kalmaz. Bunun için bir hizmet sorumlusu dizinde uygulama gerekir. Azure Active Directory'de Hizmet sorumluları eklemek için izin verilen varsayılan olarak normal kullanıcıların (Yönetici olmayanlar) var. (AD FS kullanarak) AD'de değiller. Bir her zaman herhangi bir uygulama eklemek için dizin Yöneticisi gitmeniz gerekir çünkü bu bir hurdle uçtan uca deneyim yerleştirir.| 

## <a name="learn-more"></a>Daha fazla bilgi edinin
- Kullanım örnekleri, satın alma, iş ortakları ve OEM donanım satıcıları hakkında daha fazla bilgi için bkz. [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) ürün sayfası.
- Tümleşik sistemler, Azure Stack için yol haritası ve coğrafi kullanılabilirlik hakkında bilgi teknik incelemesine bakın: [Azure Stack: Bir Azure uzantısı](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 
- Microsoft Azure Stack paketleme ve fiyatlandırma hakkında daha fazla bilgi edinmek için [.pdf indirme](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf). 

## <a name="next-steps"></a>Sonraki adımlar
[Veri Merkezi ağ tümleştirmesi](azure-stack-network.md)
