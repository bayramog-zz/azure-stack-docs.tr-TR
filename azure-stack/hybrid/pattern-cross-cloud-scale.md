---
title: Azure 'da ve Azure Stack hub 'da çapraz bulutu ölçeklendiryen bir uygulama oluşturmaya yönelik bir model.
description: Ölçeklenebilir bir çoklu bulut uygulaması oluşturmak için Azure ve Azure Stack hub 'ı nasıl kullanacağınızı öğrenin.
author: BryanLa
ms.service: azure-stack
ms.topic: article
ms.date: 11/05/2019
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 11/05/2019
ms.openlocfilehash: ddc8218aadbf3940ea18ce80012441c5e43c7472
ms.sourcegitcommit: 5c92a669007ab4aaffe4484f1d8836a40340dde1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73638810"
---
# <a name="cross-cloud-scaling-pattern"></a>Platformlar arası ölçeklendirme kalıbı

Yük artışına uyum sağlamak için mevcut bir uygulamaya kaynakları otomatik olarak ekleyin.

## <a name="context-and-problem"></a>Bağlam ve sorun

Uygulamanız, isteğe bağlı olarak beklenmeyen artışları karşılamak için kapasiteyi artırabilir. Bu ölçeklenebilirlik olmaması, kullanıcıların yoğun kullanım süreleri sırasında uygulamaya ulaşmasına neden olur. Uygulama, sabit sayıda kullanıcıya hizmet verebilir.

Küresel kuruluşlar, güvenli, güvenilir ve kullanılabilir bulut tabanlı uygulamalar gerektirir. Toplantı, isteğe bağlı olarak artar ve bu talebi desteklemek için doğru altyapıyı kullanmak önemlidir. İşletmeler, iş verileri güvenliği, depolama ve gerçek zamanlı kullanılabilirliğiyle maliyetleri ve bakımı dengelemeye uğraşır.

Uygulamanızı genel bulutta çalıştırameyebilirsiniz. Bununla birlikte, iş için şirket içi ortamlarında gereken kapasiteyi korumak, uygulama için talepte ani artışları karşılamak amacıyla ekonomik bir şekilde uygulanabilir olmayabilir. Bu düzende, şirket içi çözümünüzle ortak bulutun elakliğini kullanabilirsiniz.

## <a name="solution"></a>Çözüm

Platformlar arası ölçeklendirme stili, genel bulut kaynaklarıyla yerel bir bulutta bulunan bir uygulamayı genişletir. Bu, isteğe bağlı olarak bir artış veya azalmayla tetiklenir ve sırasıyla bulutta kaynak ekler veya kaldırır. Bu kaynaklar yedeklilik, hızlı kullanılabilirlik ve coğrafi uyumlu yönlendirme sağlar.

![Platformlar arası ölçeklendirme kalıbı](media/pattern-cross-cloud-scale/cross-cloud-scaling.png)

> [!NOTE]
> Bu model yalnızca uygulamanızın durum bilgisiz bileşenleri için geçerlidir.

## <a name="components"></a>Bileşenler

Platformlar arası ölçeklendirme stili aşağıdaki bileşenlerden oluşur.

**Traffic Manager**  

Diyagramda bu, genel bulut grubunun dışında bulunur, ancak hem yerel veri merkezinde hem de genel bulutta trafiği koordine edebilmelidir. Dengeleyici, uç noktaları izleyerek ve gerektiğinde yük devretme yeniden dağıtımı sağlayarak uygulama için yüksek kullanılabilirlik sağlar.

**Etki alanı adı sistemi (DNS)**  

Etki alanı adı sistemi veya DNS, IP adresine bir Web sitesi veya hizmet adı çevirmekten (veya çözümlemeden) sorumludur.

### <a name="cloud"></a>Bulut

**Barındırılan derleme sunucusu**  
Yapı ardışık yapınızı barındırmak için bir ortam.

**Uygulama kaynakları**  
Uygulama kaynaklarının, sanal makine ölçek kümeleri ve kapsayıcılar gibi ölçeklendirilmesi ve ölçeğini genişletmek gerekir.

**Özel etki alanı adı**  
Yönlendirme istekleri için özel bir etki alanı adı kullanın glob.

**Genel IP Adresleri**  
Genel IP adresleri, Trafik Yöneticisi üzerinden gelen trafiği genel bulut uygulama kaynakları uç noktasına yönlendirmek için kullanılır.  

### <a name="local-cloud"></a>Yerel bulut

**Barındırılan derleme sunucusu**  
Yapı ardışık yapınızı barındırmak için bir ortam.

**Uygulama kaynakları**  
Uygulama kaynakları, sanal makine ölçek kümeleri ve kapsayıcılar gibi ölçeklendirme ve genişleme özelliği gerektirir.

**Özel etki alanı adı**  
Yönlendirme istekleri için özel bir etki alanı adı kullanın glob.

**Genel IP Adresleri**  
Genel IP adresleri, Trafik Yöneticisi üzerinden gelen trafiği genel bulut uygulama kaynakları uç noktasına yönlendirmek için kullanılır. 

## <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler

Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:

### <a name="scalability"></a>Ölçeklenebilirlik

Platformlar arası ölçeklendirmenin anahtar bileşeni, isteğe bağlı ölçeklendirmeyi sunma olanağıdır. Ölçeklendirme, ortak ve yerel bulut altyapısı arasında gerçekleşmelidir ve isteğe göre tutarlı ve güvenilir bir hizmet sağlar.

### <a name="availability"></a>Erişilebilirlik

Yerel olarak dağıtılan uygulamaların şirket içi donanım yapılandırması ve yazılım dağıtımı aracılığıyla yüksek kullanılabilirlik için yapılandırıldığından emin olun.

### <a name="manageability"></a>Yönetilebilirlik

Platformlar arası desenler, ortamlar arasında sorunsuz yönetim ve tanıdık arabirim sağlar.

## <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar

Bu düzeni kullanarak:

- Uygulama kapasitenizi, istek üzerine beklenmeyen talepler veya düzenli talepler ile artırmanız gerektiğinde.
- Yalnızca tepe noktaları sırasında kullanılacak kaynaklara yatırım yapmak istemezsiniz. Kullandığınız kadar ödeyin.

Bu model şu durumlarda önerilmez:

- Çözümünüz, kullanıcıların internet üzerinden bağlanmasını gerektirir.
- İşletmenizde, kaynak bağlantının yerinde bir çağrıdan gelmesini gerektiren yerel yönetmelikler vardır.
- Ağınız, ölçeklendirmenin performansını kısıtlayan düzenli performans sorunları yaşıyor.
- Ortamınızın Internet bağlantısı kesildi ve genel buluta ulaşılamıyor.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede tanıtılan konular hakkında daha fazla bilgi edinmek için:
- Bu DNS tabanlı trafik yük dengeleyicinin nasıl çalıştığı hakkında daha fazla bilgi edinmek için bkz. [Azure Traffic Manager genel bakış](/azure/traffic-manager/traffic-manager-overview) .
- En iyi uygulamalar hakkında daha fazla bilgi edinmek ve ek soruları yanıtlamak için bkz. [karma uygulama tasarımı konuları](overview-app-design-considerations.md) .
- Ürünlerin ve çözümlerin tamamı hakkında daha fazla bilgi edinmek için [ürün ve çözümlerin Azure Stack ailesine](/azure-stack)bakın.

Çözüm örneğini test etmeye hazırsanız, [platformlar arası ölçeklendirme çözümü dağıtım kılavuzu](solution-deployment-guide-cross-cloud-scaling.md)ile devam edin. Dağıtım Kılavuzu, bileşenlerinin dağıtılması ve test edilmesi için adım adım yönergeler sağlar. Azure Stack merkezi barındırılan bir Web uygulamasından Azure 'da barındırılan bir Web uygulamasına geçiş için el ile tetiklenen bir işlem sağlamak üzere bir çoklu bulut çözümü oluşturmayı öğreneceksiniz. Ayrıca, yük altında esnek ve ölçeklenebilir bulut yardımcı programını sağlayarak Traffic Manager aracılığıyla otomatik ölçeklendirmeyi nasıl kullanacağınızı öğreneceksiniz.
