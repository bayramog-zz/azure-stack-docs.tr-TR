---
title: Azure ve Azure Stack hub kullanılarak akıllı kenar için coğrafi olarak dağıtılmış uygulama deseninin.
description: Azure ve Azure Stack hub kullanarak akıllı kenar için coğrafi olarak dağıtılmış uygulama düzeniyle ilgili bilgi edinin.
author: BryanLa
ms.service: azure-stack
ms.topic: article
ms.date: 11/05/2019
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 11/05/2019
ms.openlocfilehash: e1afb19c1ff7c32bbc15e40ce4fbe898be01ca0e
ms.sourcegitcommit: 5c92a669007ab4aaffe4484f1d8836a40340dde1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73638612"
---
# <a name="geo-distributed-pattern"></a>Coğrafi olarak dağıtılmış desenler

Birden çok bölgede uygulama uç noktaları sağlamayı ve Kullanıcı trafiğini konum ve uyumluluk ihtiyaçlarına göre yönlendirme hakkında bilgi edinin.

## <a name="context-and-problem"></a>Bağlam ve sorun

Geniş erişime sahip kuruluşlar, her Kullanıcı, konum ve bir cihaz için gerekli güvenlik, uyumluluk ve performans düzeylerini sağlarken verilere erişimi güvenle ve doğru bir şekilde dağıtabilir ve etkinleştirir.

## <a name="solution"></a>Çözüm

Azure Stack hub coğrafi trafik yönlendirme deseninin veya coğrafi olarak Dağıtılmış uygulamaların, trafiğin çeşitli ölçümlere göre belirli uç noktalara yönlendirilmesine izin verir. Coğrafi tabanlı Yönlendirme ve uç nokta yapılandırması ile Traffic Manager oluşturmak, bölgesel gereksinimlere, şirkete ve uluslararası yönetmelere ve veri ihtiyaçlarına göre trafiği uç noktalara yönlendirir.

![Coğrafi olarak dağıtılmış desenler](media/pattern-geo-distributed/geo-distribution.png)

## <a name="components"></a>Bileşenler

**Traffic Manager**  
Diyagramda Traffic Manager, genel bulutun dışında bulunur, ancak hem yerel veri merkezinde hem de genel bulutta trafiği koordine edebilmelidir. Dengeleyici trafiği coğrafi konumlara yönlendirir.

**Etki alanı adı sistemi (DNS)**  
Etki alanı adı sistemi veya DNS, IP adresine bir Web sitesi veya hizmet adı çevirmekten (veya çözümlemeden) sorumludur.

### <a name="public-cloud"></a>Genel bulut

**Bulut uç noktası**  
Genel IP adresleri, Trafik Yöneticisi üzerinden gelen trafiği genel bulut uygulama kaynakları uç noktasına yönlendirmek için kullanılır.  

### <a name="local-clouds"></a>Yerel bulutlar

**Yerel uç nokta**  
Genel IP adresleri, Trafik Yöneticisi üzerinden gelen trafiği genel bulut uygulama kaynakları uç noktasına yönlendirmek için kullanılır.

## <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler

Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:

### <a name="scalability"></a>Ölçeklenebilirlik

Bu model, trafiğin artışlarını karşılamak için ölçeklendirilmesi yerine coğrafi trafik yönlendirmeyi işler. Ancak, bu stili diğer Azure ve şirket içi çözümlerle birleştirebilirsiniz. Örneğin, bu model, platformlar arası ölçeklendirme düzeniyle birlikte kullanılabilir.

### <a name="availability"></a>Erişilebilirlik

Yerel olarak dağıtılan uygulamaların şirket içi donanım yapılandırması ve yazılım dağıtımı aracılığıyla yüksek kullanılabilirlik için yapılandırıldığından emin olun.

### <a name="manageability"></a>Yönetilebilirlik

Bu model, ortamlar arasında sorunsuz yönetim ve tanıdık arabirim sağlar.

## <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar

- Kuruluşumun özel bölgesel güvenlik ve dağıtım ilkeleri gerektiren Uluslararası dalları vardır.
- Kuruluş ofislerimin her biri, her yerel yönetmeliğe ve saat dilimine göre raporlama etkinliği gerektiren çalışan, iş ve tesis verilerini çeker.
- Yüksek ölçekli gereksinimler, tek bir bölgede birden fazla uygulama dağıtımı ve bölgeler arasında, çok fazla yük gereksinimini işlemek üzere, uygulamalar arasında yatay olarak ölçeklendirerek karşılanacak.
- Uygulamalar, tek bir bölgede kesinti olması durumunda bile yüksek oranda kullanılabilir ve istemci isteklerine yanıt vermelidir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede tanıtılan konular hakkında daha fazla bilgi edinmek için:
- Bu DNS tabanlı trafik yük dengeleyicinin nasıl çalıştığı hakkında daha fazla bilgi edinmek için bkz. [Azure Traffic Manager genel bakış](/azure/traffic-manager/traffic-manager-overview) .
- En iyi uygulamalar hakkında daha fazla bilgi edinmek ve ek soruları yanıtlamak için bkz. [karma uygulama tasarımı konuları](overview-app-design-considerations.md) .
- Ürünlerin ve çözümlerin tamamı hakkında daha fazla bilgi edinmek için [ürün ve çözümlerin Azure Stack ailesine](/azure-stack)bakın.

Çözüm örneğini test etmeye hazırsanız, [coğrafi olarak dağıtılmış uygulama çözümü dağıtım kılavuzu](solution-deployment-guide-geo-distributed.md)ile devam edin. Dağıtım Kılavuzu, bileşenlerinin dağıtılması ve test edilmesi için adım adım yönergeler sağlar. Coğrafi olarak dağıtılmış uygulamalar modelini kullanarak çeşitli ölçümlere göre trafiği belirli uç noktalara yönlendirirsiniz. Coğrafi tabanlı Yönlendirme ve uç nokta yapılandırmasıyla Traffic Manager profili oluşturma, bilgilerin bölgesel gereksinimlere, şirkete ve uluslararası yönetmelere ve veri gereksinimlerinize göre uç noktalara yönlendirilmesini sağlar.