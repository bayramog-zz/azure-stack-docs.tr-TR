---
title: Azure Stack ile akıllı uç ölçeklendirme desenini Bulutlar arası | Microsoft Docs
description: Azure Stack ile akıllı uç Bulutlar arası ölçeklendirme desenini hakkında bilgi edinin
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
ms.openlocfilehash: 2b97be7ecfffe83e560d32fbaac6480c8c17bb88
ms.sourcegitcommit: 51ec68b5e6dbf437aaca19a9f35ba07d2c402892
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67856381"
---
# <a name="cross-cloud-scaling-pattern"></a>Bulutlar arası ölçeklendirme deseni

Yük artışı uyum sağlamak için var olan bir uygulamaya otomatik olarak kaynakları ekleyin.

## <a name="context-and-problem"></a>Bağlam ve sorun

Uygulamanızı isteğe bağlı olarak artan beklenmeyen karşılamak üzere kapasite artıramıyor. Bu, kullanıcıların uygulamayı yoğun kullanım zamanlarda ulaşmasını değil sonuçlanır. Uygulama, kullanıcıların sabit sayıda servis edebilirsiniz.

Genel güvenli, güvenilir ve kullanılabilir bulut tabanlı uygulamalar gerektirir. İsteğe bağlı olarak toplantı artırır ve bu talebi karşılamak için doğru altyapı kritik öneme sahiptir. İşletmeler, maliyetleri ve bakımı iş veri güvenliği, depolama ve gerçek zamanlı kullanılabilirlik dengelemek uğraşır.

Uygulamanızı genel bulutta çalıştırmak mümkün olmayabilir. Ancak, bu uygulama için talep artışlarını işlemek için kendi şirket içi ortamda gereken kapasiteyi korumak iş için ekonomik uygun olmayabilir. Bu düzende, şirket içi çözüm ile genel bulut esnekliğinin kullanabilirsiniz.

## <a name="solution"></a>Çözüm

Genel bulut kaynaklarına sahip yerel bir bulutta bulunan bir uygulama Bulutlar arası ölçeklendirme düzeni genişletir. Desen artışı tarafından tetiklenen veya isteğe bağlı olarak, azaltma ve sırasıyla ekler veya kaynakları bulutta kaldırır. Bu kaynaklar, yedeklilik, hızlı kullanılabilirlik ve coğrafi uyumlu yönlendirme sağlar.

![Bulutlar arası ölçeklendirme deseni](media/azure-stack-edge-pattern-cross-cloud-scaling/cross-cloud-scaling.png)

> Not:  
> Bu düzen, yalnızca durum bilgisi olmayan uygulamalar için geçerlidir.

Bulutlar arası ölçeklendirme desen aşağıdaki bileşenlerden oluşur.

**Traffic Manager**  
** Diyagramda bu genel bulut grubu dışında bulunur, ancak yerel veri merkezinde hem de genel bulut trafiği koordine etmek izinlere gerekir. Dengeleyici uç noktalarınızı izleyerek ve gerekli olduğunda yük devretme yeniden dağıtımı sağlayarak uygulama için yüksek kullanılabilirlik sunar.

**Domain Name System (DNS)**  
DNS veya etki alanı adı sistemi çevirmek için sorumludur (veya çözümleme) IP adresini bir Web sitesi veya hizmet adı.

### <a name="cloud"></a>Bulut

**Barındırılan yapı sunucusu**  
Derleme işlem hattınızı barındırmak için bir ortam.

**Uygulama kaynakları**  
Ölçek kümesinde kullanamaz ve VM ScaleSets ve kapsayıcıları gibi ölçek genişletme, uygulama kaynaklarının gerekir.

**Özel etki alanı adı**  
Yönlendirme isteklerini glob bir özel etki alanı adını kullanın.

**Genel IP adresleri**  
Genel IP adresleri, genel bulut uygulama kaynakları uç noktaya gelen trafik traffic manager aracılığıyla yönlendirmek için kullanılır.  

### <a name="local-cloud"></a>Yerel bulut

**Barındırılan yapı sunucusu**  
Derleme işlem hattınızı barındırmak için bir ortam.

**Uygulama kaynakları**  
Ölçek kümesinde kullanamaz ve VM ScaleSets ve kapsayıcıları gibi ölçek genişletme, uygulama kaynaklarının gerekir.

**Özel etki alanı adı**  
Yönlendirme isteklerini glob bir özel etki alanı adını kullanın.

**Genel IP adresleri**  
Genel IP adresleri, genel bulut uygulama kaynakları uç noktaya gelen trafik traffic manager aracılığıyla yönlendirmek için kullanılır. 

## <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler


Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:

### <a name="scalability-considerations"></a>Ölçeklenebilirlik konusunda dikkat edilmesi gerekenler

Genel ve yerel bulut altyapısı, tutarlı ve güvenilir bir hizmet talebi tarafından belirtilen şekilde kanıtlama arasında isteğe bağlı ölçeklendirme teslim olanağı Bulutlar arası ölçeklendirmenin temel bileşenidir.

### <a name="availability-considerations"></a>Kullanılabilirlik konusunda dikkat edilmesi gerekenler

Yerel olarak dağıtılan uygulamaları, şirket içi donanım yapılandırması ve yazılım dağıtımı aracılığıyla yüksek kullanılabilirlik için yapılandırıldığından emin olun.

### <a name="manageability-considerations"></a>Yönetilebilirlik konusunda dikkat edilmesi gerekenler

Bulutlar arası deseni sorunsuz yönetimi ve ortamlar arasında tanıdık arabirimi sağlar.

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar

Bu düzeni kullanarak:

-   Ne zaman uygulama kapasitenizi beklenmeyen taleplerini veya düzenli taleplerin talep artırmam gerekiyor.

-   Yalnızca en yüksek sayılar sırasında kullanılacak olan kaynaklara yatırım istediğinizde değil. Kullandığınız kadar ödeyin.

Bu düzen, ne zaman önerilmez:

-   Çözümünüzü internet üzerinden bağlanan kullanıcıları gerektirir.

-   İşletmenizi gerektiren ilgili yerel düzenlemelere bir yerinde çağrısından gelen için kaynak bağlantısı vardır.

-   Ölçeklendirme performansı kısıtlar normal performans sorunlarını ağınıza karşılaşır.

-   Ortamınız Internet bağlantısı kesildiğinde ve genel bulut ulaşamıyor.

## <a name="example"></a>Örnek

Bir Azure yığını geçiş yapmak için el ile tetiklenen bir işlem barındırılan web uygulaması, trafik Yöneticisi, esnek ve ölçeklenebilir bulut yardımcı programı, yük altında sağlayarak aracılığıyla otomatik olarak ölçeklendirme ile web uygulaması için bir Azure barındırılan sağlamak için bir Bulutlar arası çözümü oluşturmayı öğrenin.

[Azure ile Bulutlar arası ölçeklendirme çözümleri oluşturun](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-cloud-burst)

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [Azure Stack için hibrit bulut tasarımı desenleri](azure-stack-edge-pattern-overview.md)
