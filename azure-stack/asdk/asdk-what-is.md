---
title: Azure Stack hub Development Kit (ASDK) nedir? | Microsoft Belgeleri
description: Azure Stack hub Development Kit ve Azure Stack hub 'ı değerlendirmek için nasıl kullanıldığı hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.date: 02/08/2019
ms.author: justinha
ms.reviewer: misainat
ms.lastreviewed: 02/08/2019
ms.openlocfilehash: 2669c8e9a4d684fbbd9340c6dbae43191f0261dc
ms.sourcegitcommit: 5ef433aa6b75cdfb557fab0ef9308ff2118e66e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73594903"
---
# <a name="what-is-the-azure-stack-hub-development-kit-asdk"></a>Azure Stack hub Development Kit (ASDK) nedir?
[Merkez tümleşik sistemleri](../operator/azure-stack-overview.md) 4-16 düğümden boyutunda Microsoft Azure Stack ve bir donanım ortağı ve Microsoft tarafından ortaklaşa desteklenir. Üretim iş yükleriniz için yeni senaryolar sağlamak üzere Azure Stack hub tümleşik sistemleri kullanın. Tümleşik sistemler altyapısını yöneten ve hizmet sunan bir Azure Stack hub operatörünüz varsa, [İşletmen belgelerimize](/azure-stack/operator)bakın.

ASDK, indirebileceğiniz ve **ücretsiz**olarak kullanabileceğiniz Azure Stack hub 'ın tek düğümlü bir dağıtımdır. Tüm ASDK bileşenleri, [En düşük donanım gereksinimlerini](asdk-deploy-considerations.md#hardware)karşılaması veya aşması gereken tek bir ana bilgisayar bilgisayarında çalışan sanal makinelere (VM) yüklenir. ASDK, Azure Stack hub 'ı değerlendirebileceğiniz ve API 'Leri kullanarak modern uygulamalar geliştirmenin yanı sıra *Üretim dışı* bir ortamda Azure ile tutarlı bir ortam sağlamak için tasarlanmıştır. 

> [!IMPORTANT]
> ASDK 'nin bir üretim ortamında kullanılması veya desteklenmesi amaçlanmamaktadır.

Tüm ASDK bileşenleri tek bir ana bilgisayara dağıtıldığından, kullanılabilir sınırlı sayıda fiziksel kaynak vardır. ASDK dağıtımları ile, hem Azure Stack hub altyapısı VM 'Leri hem de Kiracı VM 'Leri aynı sunucu bilgisayarında birlikte bulunamaz. Bu yapılandırma, ölçek veya performans değerlendirmesi için tasarlanmamıştır.

ASDK, için Azure ile tutarlı bir karma bulut deneyimi sağlamak üzere tasarlanmıştır:
- **Yöneticiler** (Azure Stack hub işleçleri): asdk, kullanılabilir Azure Stack Hub hizmetlerini değerlendirmek ve bu hizmetleri öğrenmek için harika bir kaynaktır.
- **Geliştiriciler**: asdk, şirket içinde karma veya modern uygulamalar geliştirmek için kullanılabilir (geliştirme ve test ortamları). Bu esneklik, Azure Stack hub üretim dağıtımlarının önüne veya yanına geliştirme deneyiminin yinelenebilirlik sunar.

ASDK hakkında daha fazla bilgi edinmek için bu kısa videoyu izleyin:

> [!VIDEO https://www.youtube.com/embed/dbVWDrl00MM]


## <a name="asdk-and-multi-node-azure-stack-hub-differences"></a>ASDK ve çok düğümlü Azure Stack hub farklılıkları
Tek düğümlü ASDK dağıtımları, çok düğümlü Azure Stack hub dağıtımlarından birkaç önemli şekilde farklılık gösterir:

|Açıklama|ASDK|Çok düğümlü Azure Stack hub 'ı|
|-----|-----|-----|
|**Ölçeklendirme**|Tüm bileşenler tek düğümlü bir sunucu bilgisayara yüklenir.|4-16 düğümden boyut aralığında değişebilir.|
|**Esnekliği**|Tek düğümlü yapılandırma yüksek kullanılabilirlik sağlamıyor|[Yüksek kullanılabilirlik](../operator/azure-stack-overview.md#providing-high-availability) özellikleri desteklenir.|
|**Ağ**|ASDK ana bilgisayarı tüm ASDK ağ trafiğini yönlendirir. Ek anahtar gereksinimi yoktur.|Çok düğümlü dağıtımlardaki daha karmaşık [ağ yönlendirme altyapısı](../operator/azure-stack-network.md#network-infrastructure) , raf üstü (Tor), temel kart yönetim DENETLEYICISI (BMC) ve sınır (Datacenter ağı) anahtarları dahil olmak üzere gereklidir.|
|**Düzeltme Eki ve güncelleştirme işlemi**|ASDK 'nin yeni bir sürümüne geçmek için, asdk ana bilgisayar bilgisayarındaki ASDK 'yi yeniden dağıtmanız gerekir.|Yüklü Azure Stack hub sürümünü güncelleştirmek için [Düzeltme Eki ve güncelleştirme](../operator/azure-stack-updates.md) işlemi kullanılır.|
|**Destek**|MSDN Azure Stack Forumu. Microsoft Müşteri Hizmetleri ve desteği (CSS) desteği, üretim dışı ortamlar *için kullanılamaz.*|[MSDN Azure Stack Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStack) ve tam CSS desteği.|
| | |

## <a name="learn-about-available-services"></a>Kullanılabilir hizmetler hakkında bilgi edinin
Azure Stack hub operatörü olarak, kullanıcılarınız için hangi hizmetleri kullanacağınızı bilmeniz gerekir. Azure Stack hub, Azure hizmetlerinin bir alt kümesini destekler ve desteklenen hizmetlerin listesi zaman içinde gelişmeye devam edecektir.

### <a name="foundational-services"></a>Temel hizmetler
Varsayılan olarak, Azure Stack hub, ASDK 'yi dağıtırken aşağıdaki "temel hizmetler" i içerir:
- İşlem
- Depolama
- Ağ
- Anahtar Kasası

Bu temel hizmetlerle, en az yapılandırmayla kullanıcılarınıza hizmet olarak altyapı (IaaS) sunabilirsiniz.

### <a name="additional-services"></a>Ek hizmetler
Şu anda aşağıdaki ek hizmet olarak platform (PaaS) Hizmetleri desteklenir:
- App Service
- Azure İşlevleri
- SQL ve MySQL veritabanları

> [!NOTE]
> Bu hizmetler, kullanıcılarınız için kullanılabilir hale gelmeden önce ek yapılandırma gerektirir ve ASDK 'yi yüklediğinizde varsayılan olarak kullanılabilir olmaz.

## <a name="service-roadmap"></a>Hizmet yol haritası
Azure Stack hub, ek Azure hizmetleri için destek eklemeye devam eder. Azure Stack hub 'ında yakında sunulacak hakkında daha fazla bilgi edinmek için [Azure Stack yol haritasını](https://azure.microsoft.com/roadmap/?tag=azure-stack)inceleyin. 


## <a name="next-steps"></a>Sonraki adımlar
Azure Stack hub 'ı değerlendirmeye başlamak için öncelikle [en son ASDK 'yi indirmeniz](asdk-download.md) ve asdk ana bilgisayarı hazırlamanız gerekir. Daha sonra, Azure Stack hub 'ı kullanmaya başlamak için, ASDK 'yi yükleyebilir ve yönetici ve Kullanıcı portalında oturum açabilirsiniz.