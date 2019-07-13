---
title: Azure Stack ile akıllı uç DevOps desenini | Microsoft Docs
description: Azure Stack ile akıllı uç DevOps desenini hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/11/2019
ms.author: mabrigg
ms.reviewer: anajod
ms.lastreviewed: 06/11/2019
ms.openlocfilehash: 07e0c3f0ca52c7079b879050ab36bf6a5f166f92
ms.sourcegitcommit: 51ec68b5e6dbf437aaca19a9f35ba07d2c402892
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67856370"
---
# <a name="geo-distributed-pattern"></a>Coğrafi olarak dağıtılmış deseni

Uygulama uç noktaları, birden çok bölgede sağlar. Konum ve uyumluluk ihtiyaçlarını temel alarak kullanıcı trafiği yönlendirme.

## <a name="context-and-problem"></a>Bağlam ve sorun

Geniş ulaşmasını kuruluşlarla coğrafyalar çaba güvenli bir şekilde ve doğru bir şekilde dağıtın ve Kenarlıklar arasında gerekli düzeyi güvenlik, uyumluluk ve kullanıcı, konum ve cihaz başına performans sağlarken verilere erişimi etkinleştirmek.

## <a name="solution"></a>Çözüm

Azure Stack coğrafi trafik yönlendirme desen veya Geo-Distributed uygulamaları, çeşitli ölçümlere göre belirli Uç noktalara yönlendirilmesi trafiğine izin verir. Traffic Manager yapılandırması rotalar coğrafi tabanlı Yönlendirme ve uç noktası ile oluşturma trafiği uç noktalarına bölgesel gereksinimleri, kurumsal ve uluslararası düzenleme ve veri gereksinimlerine göre.

![Coğrafi olarak dağıtılmış deseni](media/azure-stack-edge-pattern-geo-distribution/geo-distribution.png)

**Traffic Manager**  
Diyagramda bu genel bulut dışında bulunur, ancak yerel veri merkezinde hem de genel bulut trafiği koordine etmek izinlere gerekir. Dengeleyici coğrafi konumlara trafiği yönlendirir.

**Domain Name System (DNS)**  
DNS veya etki alanı adı sistemi çevirmek için sorumludur (veya çözümleme) IP adresini bir Web sitesi veya hizmet adı.

### <a name="public-cloud"></a>Genel bulut

**Bulut uç noktası**  
Genel IP adresleri, genel bulut uygulama kaynakları uç noktaya gelen trafik traffic manager aracılığıyla yönlendirmek için kullanılır.  

### <a name="local-clouds"></a>Yerel bulut

**Yerel uç noktası**  
Genel IP adresleri, genel bulut uygulama kaynakları uç noktaya gelen trafik traffic manager aracılığıyla yönlendirmek için kullanılır.

## <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler

Bu düzeni nasıl uygulayacağınıza karar verirken aşağıdaki noktaları dikkate almalısınız:

### <a name="scalability-considerations"></a>Ölçeklenebilirlik konusunda dikkat edilmesi gerekenler

Desen, trafik artışlarını karşılamak için ölçekleme yerine yönlendirme coğrafi trafiğini işler. Ancak, bu düzen diğer Azure ve şirket içi çözümler ile birleştirebilirsiniz. Örneğin, bu düzen, Bulutlar arası ölçeklendirme desen ile kullanılabilir.

### <a name="availability-considerations"></a>Kullanılabilirlik konusunda dikkat edilmesi gerekenler

Yerel olarak dağıtılan uygulamaları, şirket içi donanım yapılandırması ve yazılım dağıtımı aracılığıyla yüksek kullanılabilirlik için yapılandırıldığından emin olun.

### <a name="manageability-considerations"></a>Yönetilebilirlik konusunda dikkat edilmesi gerekenler

Desen, sorunsuz yönetimi ve ortamlar arasında tanıdık arabirimi sağlar.

## <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar

-   Kuruluşumun özel bölgesel güvenlik ve dağıtım ilkeleri gerektiren uluslararası dalları var.

-   Her kuruluşumun ofislerde çalışan, iş ve etkinlik başına yerel düzenlemeleri ve zaman dilimi raporlama gerektiren, tesis veri çeker.

-   Yüksek ölçek gereksinimlerine uygulamalarına göz aşırı yük gereksinimlerini karşılamak için tek bir bölgede yanı sıra bölgeler genelinde yapılan birden çok uygulama dağıtımları ile yatay ölçeklendirme tarafından karşılanabilir.

-   Uygulamaları, yüksek kullanılabilirliğe sahip ve tek bir bölgede kesinti olması durumunda bile istemci isteklerine hızlı yanıt veren olması gerekir.

## <a name="example"></a>Örnek

Trafiği coğrafi olarak dağıtılmış uygulamaları desenini kullanarak, çeşitli ölçümlere göre belirli Uç noktalara yönlendirmek öğrenin. Traffic Manager'ı oluşturma, coğrafi tabanlı Yönlendirme ve uç nokta yapılandırma profili bilgileri bölgesel gereksinimleri, kurumsal ve uluslararası düzenleme ve verileri gereksinimlerinize göre Uç noktalara yönlendirilir sağlar.

[Azure ve Azure Stack ile coğrafi olarak dağıtılmış bir uygulama çözümü oluşturma](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-geo-distributed)

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [Azure Stack için hibrit bulut tasarımı desenleri](azure-stack-edge-pattern-overview.md)
