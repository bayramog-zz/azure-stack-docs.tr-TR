---
title: ASDK nedir? | Microsoft Docs
description: Azure Stack Geliştirme Seti (ASDK) ve Azure Stack değerlendirmek için nasıl kullanıldığı hakkında bilgi edinin.
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
ms.openlocfilehash: 50a08480a098c9ca5a6d50e94dd6dc7dbfc78b09
ms.sourcegitcommit: e8f7fe07b32be33ef621915089344caf1fdca3fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70118614"
---
# <a name="what-is-the-asdk"></a>ASDK nedir?
4-16 düğümlerden oluşan boyut olarak [Tümleşik sistemleri Microsoft Azure Stack](../operator/azure-stack-overview.md) ve bir donanım ortağı ve Microsoft tarafından ortaklaşa desteklenir. Üretim iş yükleriniz için yeni senaryolar sağlamak üzere Azure Stack tümleşik sistemleri kullanın. Tümleşik sistemler altyapısını yöneten ve hizmet sunan bir Azure Stack işletmeni kullanıyorsanız, bkz. [operatör belgeleri](/azure-stack/operator).

Azure Stack Geliştirme Seti (ASDK), indirebileceğiniz ve **ücretsiz**olarak kullanabileceğiniz Azure Stack tek düğümlü bir dağıtımdır. Tüm ASDK bileşenleri, [En düşük donanım gereksinimlerini](asdk-deploy-considerations.md#hardware)karşılaması veya aşması gereken tek bir ana bilgisayar bilgisayarında çalışan sanal makinelere (VM) yüklenir. ASDK, Azure Stack değerlendirebileceğiniz ve Azure ile bir *Üretim dışı* ortamda Azure ile tutarlı araçlar kullanarak modern uygulamalar geliştirebileceğiniz bir ortam sağlamaktır. 

> [!IMPORTANT]
> ASDK 'nin bir üretim ortamında kullanılması veya desteklenmesi amaçlanmamaktadır.

Tüm ASDK bileşenleri tek bir ana bilgisayara dağıtıldığından, kullanılabilir sınırlı sayıda fiziksel kaynak vardır. ASDK dağıtımları ile, hem Azure Stack altyapısı VM 'Leri hem de Kiracı VM 'Leri aynı sunucu bilgisayarında birlikte bulunamaz. Bu yapılandırma, ölçek veya performans değerlendirmesi için tasarlanmamıştır.

ASDK, için Azure ile tutarlı bir karma bulut deneyimi sağlamak üzere tasarlanmıştır:
- **Yöneticiler** (Azure Stack Işleçler): ASDK, kullanılabilir Azure Stack hizmetlerini değerlendirmek ve öğrenmek için harika bir kaynaktır.
- **Geliştiriciler**: ASDK, şirket içinde karma veya modern uygulamalar geliştirmek için kullanılabilir (geliştirme ve test ortamları). Bu esneklik, Azure Stack Üretim dağıtımlarından önce ya da bunların yanı sıra geliştirme deneyiminin yinelenebilirlik sunar.

ASDK hakkında daha fazla bilgi edinmek için bu kısa videoyu izleyin:

> [!VIDEO https://www.youtube.com/embed/dbVWDrl00MM]


## <a name="asdk-and-multi-node-azure-stack-differences"></a>ASDK ve çok düğümlü Azure Stack farklılıkları
Tek düğümlü ASDK dağıtımları, çok düğümlü Azure Stack dağıtımlarından birkaç önemli şekilde farklılık gösterir:

|Açıklama|ASDK|Çok düğümlü Azure Stack|
|-----|-----|-----|
|**Ölçeklendirme**|Tüm bileşenler tek düğümlü bir sunucu bilgisayara yüklenir.|4-16 düğümden boyut aralığında değişebilir.|
|**Esnekliği**|Tek düğümlü yapılandırma yüksek kullanılabilirlik sağlamıyor|[Yüksek kullanılabilirlik](../operator/azure-stack-overview.md#providing-high-availability) özellikleri desteklenir.|
|**Ağ**|ASDK ana bilgisayarı tüm ASDK ağ trafiğini yönlendirir. Ek anahtar gereksinimi yoktur.|Çok düğümlü dağıtımlardaki daha karmaşık [ağ yönlendirme altyapısı](../operator/azure-stack-network.md#network-infrastructure) , raf üstü (Tor), temel kart yönetim DENETLEYICISI (BMC) ve sınır (Datacenter ağı) anahtarları dahil olmak üzere gereklidir.|
|**Düzeltme Eki ve güncelleştirme işlemi**|ASDK 'nin yeni bir sürümüne geçmek için, asdk ana bilgisayar bilgisayarındaki ASDK 'yi yeniden dağıtmanız gerekir.|Yüklü Azure Stack sürümünü güncelleştirmek için [Düzeltme Eki ve güncelleştirme](../operator/azure-stack-updates.md) işlemi kullanılır.|
|**Destek**|MSDN Azure Stack Forumu. Microsoft Müşteri Hizmetleri ve desteği (CSS) desteği, üretim dışı ortamlar için kullanılamaz.|[MSDN Azure Stack Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStack) ve tam CSS desteği.|
| | |

## <a name="learn-about-available-services"></a>Kullanılabilir hizmetler hakkında bilgi edinin
Azure Stack operatörü olarak, kullanıcılarınız için kullanabileceğiniz Hizmetleri bilmeniz gerekir. Azure Stack, Azure hizmetlerinin bir alt kümesini destekler ve desteklenen hizmetlerin listesi zaman içinde gelişmeye devam edecektir.

### <a name="foundational-services"></a>Temel hizmetler
Varsayılan olarak, Azure Stack, ASDK 'yi dağıtırken aşağıdaki "temel hizmetler" i içerir:
- İşlem
- Depolama
- Ağ
- Key Vault

Bu temel hizmetlerle, en az yapılandırmayla kullanıcılarınıza hizmet olarak altyapı (IaaS) sunabilirsiniz.

### <a name="additional-services"></a>Ek hizmetler
Şu anda aşağıdaki ek hizmet olarak platform (PaaS) Hizmetleri desteklenir:
- App Service
- Azure İşlevleri
- SQL ve MySQL veritabanları

> [!NOTE]
> Bu hizmetler, kullanıcılarınız için kullanılabilir hale gelmeden önce ek yapılandırma gerektirir ve ASDK 'yi yüklediğinizde varsayılan olarak kullanılabilir olmaz.

## <a name="service-roadmap"></a>Hizmet yol haritası
Azure Stack, ek Azure hizmetleri için destek eklemeye devam edecektir. Azure Stack sonraki bir sırada yakında sunulacak hakkında daha fazla bilgi edinmek için [Azure Stack yol haritasını](https://azure.microsoft.com/roadmap/?tag=azure-stack)inceleyin. 


## <a name="next-steps"></a>Sonraki adımlar
Azure Stack değerlendirmeye başlamak için öncelikle [en son ASDK 'yi indirmeniz](asdk-download.md) ve asdk ana bilgisayarı hazırlamanız gerekir. Daha sonra, Azure Stack kullanmaya başlamak için, ASDK 'yi yükleyebilir ve yönetici ve Kullanıcı portalında oturum açabilirsiniz.