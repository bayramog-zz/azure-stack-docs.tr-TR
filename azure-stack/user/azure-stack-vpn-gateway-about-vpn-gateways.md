---
title: Azure Stack için VPN ağ geçitleri oluşturma | Microsoft Docs
description: Azure Stack için VPN ağ geçitleri oluşturun ve yapılandırın.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 0e30522f-20d6-4da7-87d3-28ca3567a890
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: sethm
ms.lastreviewed: 05/21/2019
ms.openlocfilehash: a66057ea2490f4510d28db8b07d03e4ed17ba3ad
ms.sourcegitcommit: bbf3edbfc07603d2c23de44240933c07976ea550
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71714764"
---
# <a name="create-vpn-gateways-for-azure-stack"></a>Azure Stack için VPN ağ geçitleri oluşturma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure sanal ağınız ile şirket içi siteniz arasında ağ trafiği gönderebilmeniz için önce sanal ağınız için bir sanal ağ (VPN) ağ geçidi oluşturmanız gerekir.

VPN ağ geçidi bir ortak bağlantı üzerinde şifrelenmiş trafik gönderen sanal ağ geçidi türüdür. Azure'da güvenli bir şekilde Azure Stack'te bir sanal ağ ve sanal ağ arasında trafik göndermek için VPN ağ geçidi'ni kullanabilirsiniz. Ayrıca, trafiği bir sanal ağ ve VPN cihazına bağlı başka bir ağ arasında güvenli bir şekilde gönderebilirsiniz.

Bir sanal ağ geçidi oluştururken, oluşturmak istediğiniz ağ geçidi türünü belirtirsiniz. Azure Stack, bir tür sanal ağ geçidini destekler: **VPN** türü.

Bir sanal ağın her ağ geçidi türünden yalnızca bir sanal ağ geçidi olabilir. Seçtiğiniz ayarlara bağlı olarak, tek bir VPN ağ geçidi ile birden fazla bağlantı oluşturabilirsiniz. Bu tür bir kurulum örneği, çok siteli bağlantı yapılandırması örneğidir.

Azure Stack için VPN ağ geçitleri oluşturup yapılandırmadan önce, Azure Stack yapılandırmalarının Azure 'dan farklı olduğunu öğrenmek için [Azure Stack ağ oluşturma konularını](azure-stack-network-differences.md) gözden geçirin.

>[!NOTE]
>Azure 'da, seçtiğiniz VPN Gateway SKU 'SU için bant genişliği performansı, ağ geçidine bağlı tüm bağlantılara bölünmelidir. Ancak Azure Stack ' de, VPN Gateway SKU 'SU için bant genişliği değeri, ağ geçidine bağlı her bağlantı kaynağına uygulanır.
>
> Örneğin:
>
> * Azure 'da, temel VPN ağ geçidi SKU 'SU yaklaşık 100 Mbps toplu işleme uyum sağlayabilir. Bu VPN ağ geçidine iki bağlantı oluşturursanız ve bir bağlantı 50 Mbps bant genişliği kullanıyorsa, diğer bağlantıda 50 Mbps kullanılabilir.
> * Azure Stack, temel VPN ağ geçidi SKU 'suna her bağlantı için 100 Mbps aktarım hızı ayrılır.

## <a name="configuring-a-vpn-gateway"></a>VPN ağ geçidini yapılandırma

Bir VPN ağ geçidi bağlantısı belirli ayarlarla yapılandırılmış birden çok kaynak kullanır. Bu kaynakların birçoğu ayrı ayrı yapılandırılabilir, ancak bazı durumlarda, belirli bir sırayla yapılandırılması gerekir.

### <a name="settings"></a>Ayarlar

Her kaynak için seçtiğiniz ayarlar, başarılı bir bağlantı oluşturmak için önemlidir.

Tek tek kaynakları ve VPN gateway ayarları hakkında daha fazla bilgi için bkz. [hakkında VPN gateway ayarları Azure Stack için](azure-stack-vpn-gateway-settings.md). Bu makale şunları anlamanıza yardımcı olur:

* Ağ geçidi türleri, VPN türleri ve bağlantı türleri.
* Ağ geçidi alt ağları, yerel ağ geçitleri ve kullanmayı isteyebileceğiniz diğer çeşitli ayarlar.

### <a name="deployment-tools"></a>Dağıtım araçları

Oluşturun ve Azure portalı gibi bir yapılandırma aracını kullanarak kaynakları yapılandırın. Daha sonra ek kaynaklar yapılandırmak ya da uygun olduğunda var olan kaynakları değiştirmek için PowerShell gibi başka bir araca geçiş yapabilirsiniz. Şu anda Azure portalında her kaynağı ve kaynak ayarını yapılandıramazsınız. Her bağlantı topolojisine ilişkin makalelerdeki yönergelerde, belirli bir aracının ne zaman gerekli olduğu belirtilmektedir.

## <a name="connection-topology-diagrams"></a>Bağlantı topolojisi diyagramları

VPN Gateway bağlantıları için kullanılabilir farklı yapılandırma vardır. Gereksinimlerinize en uygun yapılandırmayı en iyi belirleyin. Aşağıdaki bölümlerde, aşağıdaki VPN ağ geçidi bağlantıları hakkında bilgi ve topoloji diyagramlarını görüntüleyebilirsiniz:

* Kullanılabilir dağıtım modeli
* Kullanılabilir yapılandırma araçları
* Varsa sizi doğrudan bir makaleye yönlendiren bağlantılar

Diyagramları ve açıklamaları aşağıdaki bölümlerde, gereksinimlerinize uygun bağlantı topolojisini seçmenize yardımcı olabilir. Başlıca temel topolojileri diyagramları gösterir, ancak diyagramları bir kılavuz olarak kullanıp daha karmaşık yapılandırmalar oluşturmak mümkündür.

## <a name="site-to-site-and-multi-site-ipsecike-vpn-tunnel"></a>Siteden siteye ve çok siteli (IPSec/ıKE VPN tüneli)

### <a name="site-to-site"></a>Siteden siteye

*Siteden siteye* (S2S) VPN Ağ Geçidi bağlantısı, ıPSEC/Ike (ıKEV2) VPN tüneli üzerinden kurulan bir bağlantıdır. Bu tür bir bağlantı, şirket içinde bulunan ve genel bir IP adresi atanmış bir VPN cihazı gerektirir. Bu cihaz bir NAT 'ın arkasında bulunamıyor. S2S bağlantıları, şirket içi ve dışı yapılandırmalar ile birlikte karma yapılandırmalar için kullanılabilir.

![Siteden siteye VPN bağlantısı yapılandırma örneği](media/azure-stack-vpn-gateway-about-vpn-gateways/vpngateway-site-to-site-connection-diagram.png)

### <a name="multi-site"></a>Çok siteli

*Çok* siteli bağlantı, siteden siteye bağlantı çeşitçeşididir. Sanal ağ geçidinizden genellikle birden fazla şirket içi siteye bağlanan birden fazla VPN bağlantısı oluşturursunuz. Birden çok bağlantıyla çalışırken, rota tabanlı VPN türü (klasik VNET 'ler ile çalışırken dinamik ağ geçidi olarak bilinir) kullanmanız gerekir. Her sanal ağın yalnızca bir VPN ağ geçidi olabileceğinden, ağ geçidi boyunca tüm bağlantılar mevcut bant genişliğini paylaşır.

![Azure VPN Gateway Çok Siteli bağlantı örneği](media/azure-stack-vpn-gateway-about-vpn-gateways/vpngateway-multisite-connection-diagram.png)

## <a name="gateway-skus"></a>Ağ geçidi SKU'ları

Azure Stack için sanal ağ geçidi oluşturduğunuzda, kullanmak istediğiniz SKU ağ geçidi belirtin. Aşağıdaki VPN ağ geçidi SKU'ları desteklenir:

* Temel
* Standart
* Yüksek Performans

Temel üzerinden standart gibi daha yüksek bir ağ geçidi SKU 'SU seçtiğinizde veya standart veya temel üzerinde yüksek performanslı, ağ geçidine daha fazla CPU ve ağ bant genişliği ayrılır. Sonuç olarak, ağ geçidi, sanal ağı daha yüksek ağ verimliliğini destekleyebilir.

Azure Stack, yalnızca Express Route ile kullanılan ultra performans ağ geçidi SKU 'sunu desteklemez.

SKU 'YU seçtiğinizde aşağıdakileri göz önünde bulundurun:

* Azure Stack, ilke tabanlı ağ geçitlerini desteklemez.
* Sınır Ağ Geçidi Protokolü (BGP) temel SKU'da desteklenmiyor.
* ExpressRoute-VPN Gateway birlikte mevcut konfigürasyonları Azure Stack desteklenmez.

## <a name="gateway-availability"></a>Ağ Geçidi kullanılabilirliği

Yüksek kullanılabilirlik senaryoları yalnızca **yüksek performanslı ağ geçidi** bağlantı SKU 'sunda yapılandırılabilir. Hem etkin/etkin hem de etkin/Pasif yapılandırmalar aracılığıyla kullanılabilirlik sağlayan Azure 'un aksine, Azure Stack yalnızca etkin/Pasif yapılandırmayı destekler.

### <a name="failover"></a>Yük devret

Azure Stack içinde üç adet çok kiracılı ağ geçidi altyapısı VM vardır. Bu VM 'lerden ikisi etkin moddadır ve üçüncüsü yedekli moddadır. Etkin VM 'Ler üzerinde VPN bağlantıları oluşturulmasını etkinleştirir ve yedek VM yalnızca bir yük devretme işlemi gerçekleştiyse VPN bağlantılarını kabul eder. Etkin bir ağ geçidi sanal makinesi kullanılamaz hale gelirse, VPN bağlantısı, bağlantı kaybından kısa bir süre (birkaç saniye) sonra yedekli VM 'ye yük devreder.

## <a name="estimated-aggregate-throughput-by-sku"></a>SKU'ya göre tahmini toplam verimlilik

Aşağıdaki tabloda ağ geçidi türleri ve ağ geçidi SKU 'SU tarafından tahmini toplam aktarım hızı gösterilmektedir:

|| VPN Gateway işlemesi (1) | VPN Gateway maks. IPSec tünelleri (2) |
|-------|-------|-------|
|**Temel SKU** **(3)** | 100 Mbps | 20 |
|**Standart SKU** | 100 Mbps | 20 |
|**Yüksek performanslı SKU** | 200 Mbps | 10 |

### <a name="table-notes"></a>Tablo notları

**(1)** -VPN işleme, internet üzerinden şirket içi bağlantılar için garantili bir verimlilik değildir. Mümkün olan en yüksek verimlilik ölçümüdür.  
**(2)** -en fazla tünel, tüm abonelikler için Azure Stack dağıtımı başına toplam değildir.  
**(3)** -BGP YÖNLENDIRMESI temel SKU için desteklenmez.

>[!NOTE]
>İki Azure Stack dağıtımı arasında yalnızca bir siteden siteye VPN bağlantısı oluşturulabilir. Bunun nedeni, platformda yalnızca aynı IP adresine tek bir VPN bağlantısının yapılmasına izin veren bir kısıtlamadır. Azure Stack, Azure Stack sistemindeki tüm VPN ağ geçitleri için tek bir genel IP kullanan çok kiracılı bir ağ geçidi kullandığından, iki Azure Stack sistem arasında yalnızca bir VPN bağlantısı olabilir. Bu sınırlama aynı zamanda tek bir IP adresi kullanan herhangi bir VPN ağ geçidine birden çok siteden siteye VPN bağlantısı bağlamayı de uygular. Azure Stack, aynı IP adresi kullanılarak birden fazla yerel ağ geçidi kaynağının oluşturulmasını izin vermez.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stack için VPN gateway yapılandırma ayarları](azure-stack-vpn-gateway-settings.md)
