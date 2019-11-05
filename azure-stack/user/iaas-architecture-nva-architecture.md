---
title: Yüksek oranda kullanılabilir ağ sanal gereçlerini Azure Stack dağıtma | Microsoft Docs
description: Azure Stack üzerinde yüksek oranda kullanılabilir ağ sanal gereçlerini dağıtmayı öğrenin.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: how-to
ms.date: 11/01/2019
ms.author: mabrigg
ms.reviewer: kivenkat
ms.lastreviewed: 11/01/2019
ms.openlocfilehash: f3a590519fcfb6b6214b202920b2756484f51dfc
ms.sourcegitcommit: 5ef433aa6b75cdfb557fab0ef9308ff2118e66e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73595328"
---
# <a name="deploy-highly-available-network-virtual-appliances-on-azure-stack"></a>Yüksek oranda kullanılabilir ağ sanal gereçlerini Azure Stack dağıtma

Bu makalede, Azure Stack ' de yüksek kullanılabilirlik için ağ sanal gereçlerinin (NVA 'lar) nasıl dağıtılacağı gösterilmektedir. NVA genellikle bir DMZ olarak da bilinen bir çevre ağından diğer ağ veya alt ağlara ağ trafiği akışını denetlemek için kullanılır. Bu makale yalnızca giriş, yalnızca çıkış ve hem giriş hem çıkış için örnek mimariler içermektedir.

[Azure Stack marketi](https://docs.microsoft.com/azure-stack/operator/azure-stack-marketplace-azure-items)'nde bulunan farklı satıcıların NVA 'lar vardır, en iyi performans için bunlardan birini kullanın.

Mimari aşağıdaki bileşenlere sahiptir.

## <a name="networking-and-load-balancing"></a>Ağ ve Yük Dengeleme

-   **Sanal ağ ve alt ağlar**. Her Azure VM, alt ağlara bölünalabilecek bir sanal ağa dağıtılır. Her katman için ayrı bir alt ağ oluşturun.

-   **Katman 7 Load Balancer.** Azure Stack Application Gateway henüz kullanılamadığından, şu gibi [Azure Stack Pazar yerinde](https://docs.microsoft.com/azure-stack/operator/azure-stack-marketplace-azure-items) kullanılabilecek alternatifler vardır: [Kemp LOADMASTER Load Balancer ADC içerik anahtarı](https://azuremarketplace.microsoft.com/marketplace/apps/kemptech.vlm-azure)/ [F5 Big-IP Virtual Edition](https://azuremarketplace.microsoft.com/marketplace/apps/f5-networks.f5-big-ip-best) veya [a10 vThunder ADC](https://azuremarketplace.microsoft.com/marketplace/apps/a10networks.vthunder-414-gr1)

-   **Yük dengeleyiciler**. Web katmanından iş katmanına ve iş katmanından SQL Server kadar ağ trafiğini dağıtmak için [Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)kullanın.

-   **Ağ güvenlik grupları** (NSG 'ler). Sanal ağ içindeki ağ trafiğini kısıtlamak için NSG 'leri kullanın. Örneğin, burada gösterilen üç katmanlı mimaride, veritabanı katmanı Web ön ucundan gelen trafiği kabul etmez, yalnızca iş katmanından ve yönetim alt ağından.

-   **UDR.** Trafiği belirli yük dengeleyiciye yönlendirmek için [*Kullanıcı tanımlı yollar*](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview/) (udrs) kullanın.

Bu makalede, Azure Stack ağı hakkında temel bilgiler yer aldığı varsayılır.

## <a name="architecture-diagrams"></a>Mimari diyagramları

Bir NVA, birçok farklı mimaride bir çevre ağına dağıtılabilir. Örneğin, aşağıdaki şekilde giriş için tek bir NVA kullanımı gösterilmektedir.

![Otomatik olarak oluşturulan sosyal medya gönderi açıklamasının ekran görüntüsü](./media/iaas-architecture-nva-architecture/image1.png)

Bu mimaride, NVA tüm gelen ve giden ağ trafiğini denetleyip yalnızca ağ güvenlik kurallarını karşılayan trafiği geçirerek güvenli bir ağ sınırı sağlar. Tüm ağ trafiğinin NVA 'dan geçmesi gereken, NVA 'nın ağdaki tek bir başarısızlık noktası olduğu anlamına gelir. NVA başarısız olursa, ağ trafiği için başka bir yol kalmaz ve tüm arka uç alt ağları kullanılamaz hale gelir.

NVA başarısız olursa, bir kullanılabilirlik kümesine birden fazla NVA dağıtın.

Aşağıdaki mimarilerde yüksek oranda kullanılabilir NVA’lar için gerekli kaynaklar ve mimariler açıklanmaktadır:

| Çözüm | Avantajlar | Dikkat edilmesi gerekenler |
| --- | --- | --- |
| Katman 7 NVA’ları ile giriş | Tüm NVA düğümleri etkin. | Bağlantıları sonlandırabilen ve SNAT kullanan bir NVA gerektirir.<br>, Kurumsal ağ/Internet 'ten ve Azure Stack gelen trafik için ayrı bir NVA 'lar kümesi gerektirir.<br>Yalnızca Azure Stack dışında gelen trafik için kullanılabilir.  |
| Katman 7 NVA’ları ile çıkış | Tüm NVA düğümleri etkin. | Bağlantıları sonlandırabilen ve kaynak ağ adresi çevirisi (SNAT) uygulayan bir NVA gerektirir. |
| Katman 7 ile giriş çıkış NVA 'lar | Tüm düğümler etkin.<br>Azure Stack kaynaklı trafiği işleyebiliyor. | Bağlantıları sonlandırabilen ve SNAT kullanan bir NVA gerektirir.<br>, Kurumsal ağ/Internet 'ten ve Azure Stack gelen trafik için ayrı bir NVA 'lar kümesi gerektirir. |

## <a name="ingress-with-layer-7-nvas"></a>Katman 7 NVA’ları ile giriş

Aşağıdaki şekilde, internet 'e yönelik yük dengeleyicinin arkasındaki bir giriş çevre ağını uygulayan yüksek kullanılabilirlik mimarisi gösterilmektedir. Bu mimari, HTTP veya HTTPS gibi katman 7 trafiği için Azure Stack iş yüklerine bağlantı sağlamak üzere tasarlanmıştır:

![Otomatik olarak oluşturulan harita açıklamasının ekran görüntüsü](./media/iaas-architecture-nva-architecture/image2.png)

Bu mimarinin avantajı tüm NVA’ların etkin olması ve birinin başarısın olması durumunda yük dengeleyicinin ağ trafiğini diğer NVA’ya yönlendirmesidir. İki NVA da trafiği iç yük dengeleyiciye yönlendirdiğinden, bir NVA etkin olduğu sürece trafik akışı devam eder. NVA’ların web katmanı VM’ler için SSL trafiğini sonlandırması gerekir. Kurumsal ağ trafiği, kendi ağ yollarıyla başka bir adanmış NVA 'lar kümesi gerektirdiğinden bu NVA 'lar kurumsal ağ trafiğini işleyecek şekilde genişletilemiyor.

## <a name="egress-with-layer-7-nvas"></a>Katman 7 NVA’ları ile çıkış

Katman 7 NVA 'lar mimarisine Giriş, Azure Stack iş yükünde kaynaklanan istekler için çıkış çevre ağı sağlamak üzere genişletilebilir. Aşağıdaki mimari, HTTP veya HTTPS gibi katman 7 trafiği için çevre ağındaki NVA 'lar yüksek kullanılabilirlik sağlamak üzere tasarlanmıştır:

![Otomatik olarak oluşturulan bir cep telefonu açıklamasının ekran görüntüsü](./media/iaas-architecture-nva-architecture/image3.png)

Bu mimaride, Azure Stack kaynaklanan tüm trafik iç yük dengeleyiciye yönlendirilir. Yük dengeleyici giden istekleri bir NVA kümesi arasında dağıtır. Bu NVA’lar kendi ortak IP adreslerini kullanarak trafiği İnternet’e yönlendirir.

## <a name="ingress-egress-with-layer-7--nvas"></a>Katman 7 ile giriş çıkış NVA 'lar

İki giriş ve çıkış mimarilerinde, giriş ve çıkış için ayrı bir çevre ağı vardı. Aşağıdaki mimaride, HTTP veya HTTPS gibi katman 7 trafiği için hem giriş hem de çıkış için kullanılabilecek bir çevre ağı oluşturma işlemi gösterilmektedir:

![Otomatik olarak oluşturulan sosyal medya gönderi açıklamasının ekran görüntüsü](./media/iaas-architecture-nva-architecture/image4.png)

Katman 7 NVA 'lar mimarisine sahip giriş çıkışı içinde NVA 'lar bir katman 7 Load Balancer gelen istekleri işler. NVA’lar ayrıca yük dengeleyicinin arka uç havuzundaki iş yükü VM’lerinden giden istekleri işler. Gelen trafik bir katman 7 yük dengeleyicisiyle yönlendirildiği ve giden trafik SLB (Azure Stack temel Load Balancer) ile yönlendirildiği için, NVA 'lar oturum benzeşimi korunmasından sorumludur. Diğer bir deyişle, 7. katman yük dengeleyici gelen ve giden isteklerin eşlemesini tutarak, doğru yanıtı özgün istek sahibine iletebilirler. Ancak, iç yük dengeleyicinin katman 7 yük dengeleyici eşlemelerine erişimi yoktur ve NVA 'lar 'e yanıt göndermek için kendi mantığını kullanır. Yük dengeleyici, ilk olarak katman 7 yük dengeleyiciden isteği almamış bir NVA 'ya yanıt gönderebilir. Bu durumda, NVA 'lar, doğru NVA 'nın yanıtı katman 7 yük dengeleyiciye iletebilmesi için, aralarındaki yanıtı iletişim kurması ve aktarması gerekir.

> [!Note]  
> NVA’ların gelen kaynak ağ adresi çevirisi (SNAT) gerçekleştirmesini sağlayarak da asimetrik yönlendirme sorununu çözebilirsiniz. Bu, istek sahibinin özgün kaynak IP’sini gelen akışta kullanılan NVA’nın IP adreslerinden biriyle değiştirir. Bu rota simetrisini korurken bir seferde birden çok NVA kullanabilmenizi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Stack VM 'Ler hakkında daha fazla bilgi için bkz. [Azure Stack VM özellikleri](azure-stack-vm-considerations.md).  
- Azure bulut desenleri hakkında daha fazla bilgi edinmek için bkz. [bulut tasarım desenleri](https://docs.microsoft.com/azure/architecture/patterns).
