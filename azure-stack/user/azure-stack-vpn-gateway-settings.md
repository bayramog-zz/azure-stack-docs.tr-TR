---
title: Azure Stack için VPN Gateway ayarlarını yapılandırma | Microsoft Docs
description: Azure Stack için VPN ağ geçitleri ayarlarını öğrenin ve yapılandırın.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: fa8d3adc-8f5a-4b4f-8227-4381cf952c56
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/03/2019
ms.author: sethm
ms.lastreviewed: 12/27/2018
ms.openlocfilehash: 650257a0bfe94741d00345f98b40fddd8d00cb44
ms.sourcegitcommit: b2d19e12a50195bb8925879ee75c186c9604f313
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71961460"
---
# <a name="configure-vpn-gateway-settings-for-azure-stack"></a>Azure Stack için VPN Gateway ayarlarını yapılandırma

*Için geçerli: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti @ no__t-0

VPN ağ geçidi, Azure Stack ve uzak VPN ağ geçidinde sanal ağınız arasında şifrelenmiş trafik gönderen bir sanal ağ geçidi türüdür. Uzak VPN Gateway, Azure 'da, veri merkezinizdeki bir cihazda veya başka bir sitedeki bir cihazda olabilir. İki uç nokta arasında ağ bağlantısı varsa, iki ağ arasında güvenli siteden siteye (S2S) VPN bağlantısı kurabilirsiniz.

Bir VPN Ağ Geçidi bağlantısı, her biri yapılandırılabilir ayarları içeren birden fazla kaynak yapılandırmasına dayanır. Bu makalede, Kaynak Yöneticisi dağıtım modelinde oluşturduğunuz bir sanal ağ için bir VPN ağ geçidi ile ilgili kaynak ve ayarlar açıklanmaktadır. [VPN Gateway Azure Stack hakkında](azure-stack-vpn-gateway-about-vpn-gateways.md)her bağlantı çözümü için açıklamaları ve topoloji diyagramlarını bulabilirsiniz.

## <a name="vpn-gateway-settings"></a>VPN gateway ayarları

### <a name="gateway-types"></a>Ağ geçidi türleri

Her Azure Stack sanal ağ, **VPN**türünde olması gereken tek bir sanal ağ geçidini destekler.  Bu destek, ek türleri destekleyen Azure 'dan farklıdır.

Bir sanal ağ geçidi oluşturduğunuzda, yapılandırmanız için ağ geçidi türünün doğru olduğundan emin olmanız gerekir. VPN ağ geçidi `-GatewayType Vpn` bayrağını gerektirir; Örneğin:

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased
```

### <a name="gateway-skus"></a>Ağ geçidi SKU'ları

Bir sanal ağ geçidi oluşturduğunuzda, kullanmak istediğiniz ağ geçidi SKU 'sunu belirtmeniz gerekir. İş yükü, aktarım hızı, özellik ve SLA türlerine bağlı olarak gereksinimlerinize uyan SKU'ları seçin.

Azure Stack, aşağıdaki tabloda gösterilen VPN Gateway SKU 'Larını sunmaktadır:

| | VPN Gateway aktarım hızı |VPN Gateway maksimum IPSec tüneli |
|-------|-------|-------|
|**Temel SKU**  | 100 Mbps  | 20    |
|**Standart SKU**   | 100 Mbps  | 20 |
|**Yüksek performanslı SKU** | 200 Mbps | 10 |

### <a name="resizing-gateway-skus"></a>Ağ Geçidi SKU 'Larını yeniden boyutlandırma

Azure Stack, desteklenen eski SKU 'Lar arasında SKU 'ların yeniden boyutlandırılmasını desteklemez.

Benzer şekilde, Azure Stack desteklenen bir eski SKU 'dan (**temel**, **Standart**ve **HighPerformance**) yeniden BOYUTLANDıRMAYı Azure tarafından desteklenen yeni bir SKU 'Ya (**VpnGw1**, **VpnGw2**ve **VpnGw3**) desteklemez.

### <a name="configure-the-gateway-sku"></a>Ağ Geçidi SKU 'sunu yapılandırma

#### <a name="azure-stack-portal"></a>Azure Stack portalı

Bir Kaynak Yöneticisi sanal ağ geçidi oluşturmak için Azure Stack portalını kullanıyorsanız, açılan listeyi kullanarak ağ geçidi SKU 'sunu seçebilirsiniz. Seçenekler, seçtiğiniz ağ geçidi türü ve VPN türüne karşılık gelir.

#### <a name="powershell"></a>PowerShell

Aşağıdaki PowerShell örneği, `-GatewaySku` parametresini **Standart**olarak belirtir:

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewaySku Standard `
-GatewayType Vpn -VpnType RouteBased
```

### <a name="connection-types"></a>Bağlantı türleri

Kaynak Yöneticisi dağıtım modelinde, her yapılandırma için belirli bir sanal ağ geçidi bağlantı türü gerekir. @No__t-0 için kullanılabilir Kaynak Yöneticisi PowerShell değerleri **IPSec**' lerdir.

Aşağıdaki PowerShell örneğinde IPSec bağlantı türü gerektiren bir S2S bağlantısı oluşturulur:

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
```

### <a name="vpn-types"></a>VPN türleri

Bir VPN ağ geçidi yapılandırması için sanal ağ geçidi oluşturduğunuzda, bir VPN türü belirtmeniz gerekir. Seçtiğiniz VPN türü, oluşturmak istediğiniz bağlantı topolojisine bağlıdır. Bir VPN türü, kullanmakta olduğunuz donanıma de bağlı olabilir. S2S yapılandırmalarının bir VPN cihazı olması gerekir. Bazı VPN cihazları yalnızca belirli bir VPN türünü destekler.

> [!IMPORTANT]  
> Şu anda Azure Stack yalnızca rota tabanlı VPN türünü destekler. Cihazınız yalnızca ilke tabanlı VPN 'Leri destekliyorsa, bu cihazlara Azure Stack bağlantıları desteklenmez.  
>
> Ayrıca Azure Stack, Özel IPSec/ıKE ilke yapılandırmalarının desteklenmediği için yol tabanlı ağ geçitleri için ilke tabanlı trafik seçicileri kullanmayı desteklemez.

* **Policybased**: İlke tabanlı VPN 'Ler, şirket içi ağınız ve Azure Stack VNet arasındaki adres ön ekleri birleşimleriyle yapılandırılan IPSec ilkelerine bağlı olarak paketleri IPSec tünellerine göre şifreler ve yönlendirir. İlke veya trafik Seçicisi, genellikle VPN cihaz yapılandırmasındaki bir erişim listesidir.

  >[!NOTE]
  >**Policybased** , Azure 'da desteklenir, ancak Azure Stack.

* **Routebased**: Rota tabanlı VPN 'Ler, paketleri karşılık gelen Tünel arabirimlerine yönlendirmek için IP iletme veya yönlendirme tablosunda yapılandırılan rotaları kullanır. Bundan sonra tünel arabirimleri, paketleri tünellerin içinde veya dışında şifreler veya şifrelerini çözer. **Routebased** VPN 'ler için ilke veya trafik Seçicisi herhangi bir any (veya joker karakter kullan) olarak yapılandırılır. Varsayılan olarak, bunlar değiştirilemez. **Routebased** VPN türü Için değer **routebased**' dir.

Aşağıdaki PowerShell örneği, `-VpnType` ' yı **Routebased**olarak belirtir. Bir ağ geçidi oluşturduğunuzda, yapılandırmanız için `-VpnType` ' ın doğru olduğundan emin olmanız gerekir.

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig `
-GatewayType Vpn -VpnType RouteBased
```

### <a name="gateway-requirements"></a>Ağ geçidi gereksinimleri

Aşağıdaki tabloda VPN ağ geçitleri için gereksinimler listelenmektedir.

| |İlke Temelli Temel VPN Gateway | Rota Temelli Temel VPN Gateway | Rota Temelli Standart VPN Gateway | Rota Temelli Yüksek Performanslı VPN Gateway|
|--|--|--|--|--|
| **Siteden siteye bağlantı (S2S bağlantısı)** | Desteklenmiyor | Rota temelli VPN yapılandırması | Rota temelli VPN yapılandırması | Rota temelli VPN yapılandırması |
| **Kimlik doğrulama yöntemi**  | Desteklenmiyor | S2S bağlantısı için Önceden paylaşılan anahtar  | S2S bağlantısı için Önceden paylaşılan anahtar  | S2S bağlantısı için Önceden paylaşılan anahtar  |
| **En fazla S2S bağlantısı sayısı**  | Desteklenmiyor | 20 | 20| 10|
|**Etkin yönlendirme desteği (BGP)** | Desteklenmiyor | Desteklenmiyor | Desteklenen | Desteklenen |

### <a name="gateway-subnet"></a>Ağ geçidi alt ağı

Bir VPN ağ geçidi oluşturmadan önce bir ağ geçidi alt ağı oluşturmanız gerekir. Ağ geçidi alt ağı, sanal ağ geçidi VM 'lerinin ve hizmetlerinin kullandığı IP adreslerini içerir. Sanal ağ geçidinizi oluşturduğunuzda, ağ geçidi VM 'Leri ağ geçidi alt ağına dağıtılır ve gerekli VPN Gateway ayarları ile yapılandırılır. Ağ geçidi alt ağına başka bir şey (örneğin, ek VM 'Ler) dağıtmayın.

>[!IMPORTANT]
>Ağ geçidi alt ağı düzgün çalışması için **GatewaySubnet** şeklinde adlandırılmalıdır. Azure Stack, sanal ağ geçidi VM 'lerini ve hizmetlerini dağıtacağınız alt ağı belirlemek için bu adı kullanır.

Ağ geçidi alt ağı oluştururken, alt ağın içerdiği IP adresi sayısını belirtirsiniz. Ağ geçidi alt ağındaki IP adresleri ağ geçidi VM 'lerine ve ağ geçidi hizmetlerine ayrılır. Bazı yapılandırmalar için diğerlerinden daha fazla IP adresi gerekir. Oluşturmak istediğiniz yapılandırmayla ilgili yönergelere bakın ve oluşturmak istediğiniz ağ geçidi alt ağının bu gereksinimleri karşıladığından emin olun.

Ayrıca, ağ geçidi alt ağınızın, gelecekteki diğer yapılandırmaların işlenmesi için yeterli IP adresine sahip olduğundan emin olun. /29 kadar küçük bir ağ geçidi alt ağı oluşturabilseniz de,/28 veya daha büyük (/28,/27,/26 vb.) bir ağ geçidi alt ağı oluşturmanızı öneririz. Bu şekilde, gelecekteki işlevleri daha sonra eklerseniz, ağ geçidinizi bölmek ve daha fazla IP adresine izin vermek için ağ geçidi alt ağını silip yeniden oluşturmanız gerekmez.

Aşağıdaki Kaynak Yöneticisi PowerShell örneği, **gatewaysubnet**adlı bir ağ geçidi alt ağını göstermektedir. CıDR gösteriminin bir/27 olduğunu görebilirsiniz. Bu, şu anda mevcut olan çoğu yapılandırma için yeterli IP adresi sağlar.

```powershell
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

> [!IMPORTANT]
> Ağ geçidi alt ağlarıyla çalışırken, ağ güvenlik grubunu (NSG) ağ geçidi alt ağıyla ilişkilendirmekten kaçının. Bir ağ güvenlik grubunun bu alt ağ ile ilişkilendirilmesi, VPN ağ geçidinizin beklendiği gibi çalışmayı durdurmasına neden olabilir. Ağ güvenlik grupları hakkında daha fazla bilgi için bkz. [ağ güvenlik grubu nedir?](/azure/virtual-network/virtual-networks-nsg).

### <a name="local-network-gateways"></a>Yerel ağ geçitleri

Azure 'da bir VPN ağ geçidi yapılandırması oluştururken, yerel ağ geçidi genellikle şirket içi konumunuzu temsil eder. Azure Stack, Azure Stack dışında kalan herhangi bir uzak VPN cihazını temsil eder. Bu cihaz, veri merkezinizdeki (veya uzak bir veri merkezinde) bir VPN cihazı veya Azure 'da bir VPN ağ geçidi olabilir.

Yerel ağ geçidine bir ad, VPN cihazının genel IP adresi verirsiniz ve şirket içi konumdaki adres öneklerini belirtebilirsiniz. Azure, ağ trafiği için hedef adres öneklerine bakar, yerel ağ geçidiniz için belirttiğiniz yapılandırmaya bakar ve paketleri buna göre yönlendirir.

Bu PowerShell örneği yeni bir yerel ağ geçidi oluşturur:

```powershell
New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```

Bazen yerel ağ geçidi ayarlarını değiştirmeniz gerekir; Örneğin, adres aralığını eklediğinizde veya değiştirdiğinizde ya da VPN cihazının IP adresi değişirse. Daha fazla bilgi için bkz. [PowerShell kullanarak yerel ağ geçidi ayarlarını değiştirme](/azure/vpn-gateway/vpn-gateway-modify-local-network-gateway).

## <a name="ipsecike-parameters"></a>IPSec/ıKE parametreleri

Azure Stack ' de bir VPN bağlantısı ayarladığınızda, bağlantıyı her iki uçta da yapılandırmanız gerekir. VPN ağ geçidi gibi davranan bir anahtar veya yönlendirici gibi Azure Stack ile bir donanım cihazı arasında bir VPN bağlantısı yapılandırıyorsanız, bu cihaz sizden ek ayarlar isteyebilir.

Hem Başlatıcı hem de Yanıtlayıcı olarak birden çok teklifi destekleyen Azure 'ın aksine, Azure Stack varsayılan olarak yalnızca bir teklifi destekler. VPN aygıtınızla çalışmak için farklı IPSec/ıKE ayarları kullanmanız gerekiyorsa, bağlantınızı el ile yapılandırmanız için kullanabileceğiniz daha fazla ayar vardır. Daha fazla bilgi için bkz. [siteden sıteye VPN bağlantıları Için IPSec/IKE Ilkesini yapılandırma](azure-stack-vpn-s2s.md).

### <a name="ike-phase-1-main-mode-parameters"></a>IKE Aşama 1 (Ana Mod) parametreleri

| Özellik              | Value|
|-|-|
| IKE Sürümü           | IKEv2 |
|Diffie-Hellman Grubu   | Grup 2 (1024 bit) |
| Kimlik Doğrulama Yöntemi | Önceden Paylaşılan Anahtar |
|Şifreleme ve Karma Algoritmaları | AES256, SHA256 |
|SA Yaşam Süresi (Zaman)     | 28.800 saniye|

### <a name="ike-phase-2-quick-mode-parameters"></a>IKE Aşama 2 (Hızlı Mod) parametreleri

| Özellik| Value|
|-|-|
|IKE Sürümü |IKEv2 |
|Şifreleme & karma algoritmaları (şifreleme)     | GCMAES256|
|Şifreleme & karma algoritmaları (kimlik doğrulaması) | GCMAES256|
|SA Yaşam Süresi (Zaman)  | 27.000 saniye  |
|SA yaşam süresi (kilobayt) | 33.553.408     |
|Kusursuz İletme Gizliliği (PFS) |Hiçbiri (bkz. **Note 1**) |
|Kullanılmayan Eş Algılama | Desteklenen|  

**1. nota:** 1807 sürümünden önce, Azure Stack kusursuz Iletme gizliliği (PFS) için bir PFS2048 değeri kullandı.

## <a name="next-steps"></a>Sonraki adımlar

* [ExpressRoute kullanarak bağlanma](../operator/azure-stack-connect-expressroute.md)
