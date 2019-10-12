---
title: Azure Stack ile akıllı kenar için DevOps stili | Microsoft Docs
description: Azure Stack ile akıllı kenar için DevOps kalıbı hakkında bilgi edinin.
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
ms.date: 10/10/2019
ms.author: mabrigg
ms.reviewer: anajod
ms.lastreviewed: 06/11/2019
ms.openlocfilehash: deab520045d50acefb03691b9b127f99676061a0
ms.sourcegitcommit: a6d47164c13f651c54ea0986d825e637e1f77018
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72277723"
---
# <a name="geo-distributed-pattern"></a>Coğrafi olarak dağıtılmış desenler

Birden çok bölge arasında uygulama uç noktaları sağlayın. Kullanıcı trafiğini konum ve uyumluluk ihtiyaçlarına göre yönlendirin.

## <a name="context-and-problem"></a>Bağlam ve sorun

Geniş erişime sahip kuruluşlar, her Kullanıcı, konum ve bir cihaz için gerekli güvenlik, uyumluluk ve performans düzeylerini sağlarken verilere erişimi güvenle ve doğru bir şekilde dağıtabilir ve etkinleştirir.

## <a name="solution"></a>Çözüm

Azure Stack coğrafi trafik yönlendirme deseninin veya coğrafi olarak Dağıtılmış uygulamaların, trafiğin çeşitli ölçümlere göre belirli uç noktalara yönlendirilmesine izin verir. Coğrafi tabanlı Yönlendirme ve uç nokta yapılandırması ile Traffic Manager oluşturmak, bölgesel gereksinimlere, şirkete ve uluslararası yönetmelere ve veri ihtiyaçlarına göre trafiği uç noktalara yönlendirir.

![Coğrafi olarak dağıtılmış desenler](media/azure-stack-edge-pattern-geo-distribution/geo-distribution.png)

**Traffic Manager**  
Diyagramda bu, genel bulutun dışında bulunur, ancak hem yerel veri merkezinde hem de genel bulutta trafiği koordine edebilmelidir. Dengeleyici trafiği coğrafi konumlara yönlendirir.

**Etki alanı adı sistemi (DNS)**  
Etki alanı adı sistemi veya DNS, IP adresine bir Web sitesi veya hizmet adı çevirmekten (veya çözümlemeden) sorumludur.

### <a name="public-cloud"></a>Genel bulut

**Bulut uç noktası**  
Genel IP adresleri, Trafik Yöneticisi üzerinden gelen trafiği genel bulut uygulama kaynakları uç noktasına yönlendirmek için kullanılır.  

### <a name="local-clouds"></a>Yerel bulutlar

**Yerel uç nokta**  
Genel IP adresleri, Trafik Yöneticisi üzerinden gelen trafiği genel bulut uygulama kaynakları uç noktasına yönlendirmek için kullanılır.

## <a name="issues-and-considerations"></a>Sorunlar ve konular

Bu düzenin nasıl uygulanacağını saptarken aşağıdaki noktaları göz önünde bulundurmanız gerekir:

### <a name="scalability-considerations"></a>Ölçeklenebilirlik konuları

Bu model, trafiğin artışlarını karşılamak için ölçeklendirilmesi yerine coğrafi trafik yönlendirmeyi işler. Ancak, bu stili diğer Azure ve şirket içi çözümlerle birleştirebilirsiniz. Örneğin, bu model, platformlar arası ölçeklendirme düzeniyle birlikte kullanılabilir.

### <a name="availability-considerations"></a>Kullanılabilirlik konuları

Yerel olarak dağıtılan uygulamaların şirket içi donanım yapılandırması ve yazılım dağıtımı aracılığıyla yüksek kullanılabilirlik için yapılandırıldığından emin olun.

### <a name="manageability-considerations"></a>Yönetilebilirlik konuları

Bu model, ortamlar arasında sorunsuz yönetim ve tanıdık arabirim sağlar.

## <a name="when-to-use-this-pattern"></a>Bu düzenin ne zaman kullanılacağı

-   Kuruluşumun özel bölgesel güvenlik ve dağıtım ilkeleri gerektiren Uluslararası dalları vardır.

-   Kuruluş ofislerimin her biri, her yerel yönetmeliğe ve saat dilimine göre raporlama etkinliği gerektiren çalışan, iş ve tesis verilerini çeker.

-   Yüksek ölçekli gereksinimler, tek bir bölgede birden fazla uygulama dağıtımı ve bölgeler arasında, çok fazla yük gereksinimini işlemek üzere, uygulamalar arasında yatay olarak ölçeklendirerek karşılanacak.

-   Uygulamalar, tek bir bölgede kesinti olması durumunda bile yüksek oranda kullanılabilir ve istemci isteklerine yanıt vermelidir.

## <a name="example"></a>Örnek

Coğrafi olarak dağıtılmış uygulamalar modelini kullanarak çeşitli ölçümleri temel alarak trafiğin belirli uç noktalara nasıl yönlendirileceğini öğrenin. Coğrafi tabanlı Yönlendirme ve uç nokta yapılandırmasıyla Traffic Manager profili oluşturma, bilgilerin bölgesel gereksinimlere, şirkete ve uluslararası yönetmelere ve veri gereksinimlerinize göre uç noktalara yönlendirilmesini sağlar.

[Azure ve Azure Stack coğrafi olarak dağıtılmış bir uygulama çözümü oluşturma](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-geo-distributed)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack Için karma bulut tasarım desenleri](azure-stack-edge-pattern-overview.md) hakkında bilgi edinin
