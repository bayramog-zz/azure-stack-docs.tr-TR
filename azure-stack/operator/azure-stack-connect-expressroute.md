---
title: ExpressRoute kullanarak Azure 'a Azure Stack bağlanma | Microsoft Docs
description: ExpressRoute kullanarak Azure 'daki sanal ağlara Azure Stack nasıl bağlanacağınızı öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 10/22/2018
ms.openlocfilehash: d7fa69b632ec6d205eff0ed0c388c1f9ec9b9c41
ms.sourcegitcommit: c196463492732218d2474d3a964f88e995272c80
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71094396"
---
# <a name="connect-azure-stack-to-azure-using-azure-expressroute"></a>Azure ExpressRoute kullanarak Azure 'a Azure Stack bağlama

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu makalede, [Microsoft Azure ExpressRoute](/azure/expressroute/) doğrudan bağlantı kullanarak bir Azure Stack sanal ağın Azure sanal ağına nasıl bağlanacağı açıklanır.

Bu makaleyi öğretici olarak kullanabilir ve örnekleri kullanarak aynı test ortamını ayarlayabilirsiniz. İsterseniz, kendi ExpressRoute ortamınızı ayarlama konusunda size kılavuzluk eden bir kılavuz olarak makaleyi kullanabilirsiniz.

## <a name="overview-assumptions-and-prerequisites"></a>Genel bakış, varsayımlar ve Önkoşullar

Azure ExpressRoute, bağlantı sağlayıcı tarafından sağlanan özel bir bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletmenizi sağlar. ExpressRoute, genel İnternet üzerinden bir VPN bağlantısı değildir.

Azure ExpressRoute hakkında daha fazla bilgi için bkz. [ExpressRoute 'a genel bakış](/azure/expressroute/expressroute-introduction).

### <a name="assumptions"></a>Varsayımlar

Bu makalede şu şekilde varsayılmaktadır:

* Azure ile çalışan bir bilginiz var.
* Azure Stack temel olarak anlaşılmış olursunuz.
* Ağın temel olarak anlaşılmasına sahip olursunuz.

### <a name="prerequisites"></a>Önkoşullar

ExpressRoute kullanarak Azure Stack ve Azure 'a bağlanmak için aşağıdaki gereksinimleri karşılamanız gerekir:

* Bir [bağlantı sağlayıcısı](/azure/expressroute/expressroute-locations)aracılığıyla sağlanan [ExpressRoute devresi](/azure/expressroute/expressroute-circuit-peerings) .
* Azure 'da bir ExpressRoute bağlantı hattı ve sanal ağlar oluşturmak için bir Azure aboneliği.
* Şunları yapmak zorunda olan bir yönlendirici:
  * , LAN arabirimi ve çok kiracılı ağ geçidi Azure Stack siteden siteye VPN bağlantılarını destekler.
  * Azure Stack dağıtımınızda birden fazla kiracı varsa birden çok VRFs (sanal Yönlendirme ve Iletme) oluşturmayı destekler.
* Şu olan bir yönlendirici:
  * ExpressRoute devresine bağlanmış bir WAN bağlantı noktası.
  * Azure Stack çok kiracılı ağ geçidine bağlı bir LAN bağlantı noktası.

### <a name="expressroute-network-architecture"></a>ExpressRoute ağ mimarisi

Aşağıdaki şekilde, bu makaledeki örnekleri kullanarak ExpressRoute kurulumunu tamamladıktan sonra Azure Stack ve Azure ortamları gösterilmektedir:

![ExpressRoute ağı](media/azure-stack-connect-expressroute/Conceptual.png)

Aşağıdaki şekilde, birden çok kiracının ExpressRoute yönlendiricisi aracılığıyla Azure Stack altyapısından Microsoft Edge 'de Azure 'a nasıl bağlanacağı gösterilmektedir:

![ExpressRoute ile çok kiracılı bağlantılar](media/azure-stack-connect-expressroute/Architecture.png)

Bu makaledeki örnek, ExpressRoute özel eşlemesini kullanarak Azure Stack Azure 'a bağlanmak için bu diyagramda gösterilen çok kiracılı mimariyi kullanır. Bağlantı, Azure Stack sanal ağ geçidinden bir ExpressRoute yönlendiricisine siteden siteye VPN bağlantısı kullanılarak yapılır.

Bu makaledeki adımlarda, Azure Stack ' de iki farklı kiracıdan iki sanal ağ arasında Azure 'da karşılık gelen VNET 'ler arasında uçtan uca bir bağlantının nasıl oluşturulacağı gösterilmektedir. İki kiracı ayarlama isteğe bağlıdır; Bu adımları tek bir kiracı için de kullanabilirsiniz.

## <a name="configure-azure-stack"></a>Azure Stack Yapılandır

İlk kiracının Azure Stack ortamını ayarlamak için aşağıdaki adımları kılavuz olarak kullanın. Birden fazla kiracı ayarlıyorsanız, bu adımları yineleyin:

>[!NOTE]
>Bu adımlarda Azure Stack portalını kullanarak kaynakların nasıl oluşturulacağı gösterilmektedir, ancak PowerShell de kullanabilirsiniz.

![Azure Stack ağ kurulumu](media/azure-stack-connect-expressroute/image2.png)

### <a name="before-you-begin"></a>Başlamadan önce

Azure Stack yapılandırmaya başlamadan önce şunları yapmanız gerekir:

* Azure Stack tümleşik bir sistem dağıtımı veya Azure Stack Geliştirme Seti (ASDK) dağıtımı. ASDK dağıtma hakkında daha fazla bilgi için [Azure Stack geliştirme seti dağıtım hızlı](../asdk/asdk-download.md)başlangıcı ' na bakın.
* Kullanıcılarınızın abone olabileceği Azure Stack bir teklif. Daha fazla bilgi için bkz. [planlar, teklifler ve abonelikler](azure-stack-plan-offer-quota-overview.md).

### <a name="create-network-resources-in-azure-stack"></a>Azure Stack ağ kaynakları oluşturma

Bir kiracının Azure Stack gereken ağ kaynaklarını oluşturmak için aşağıdaki yordamları kullanın.

#### <a name="create-the-virtual-network-and-vm-subnet"></a>Sanal ağ ve VM alt ağı oluşturma

1. Azure Stack Kullanıcı portalında oturum açın.

2. Portalda **+ kaynak oluştur**' u seçin.

3. **Azure Marketi**altında **ağ**' ı seçin.

4. **Öne çıkan**bölümünde **sanal ağ**' ı seçin.

5. **Sanal ağ oluştur**altında, aşağıdaki tabloda gösterilen değerleri uygun alanlara girin:

   |Alan  |Değer  |
   |---------|---------|
   |Name     |Tenant1VNet1         |
   |Adres alanı     |10.1.0.0/16|
   |Alt ağ adı     |Tenant1-Sub1|
   |Alt ağ adres aralığı     |10.1.1.0/24|

6. Daha önce oluşturduğunuz aboneliğin **abonelik** alanında doldurulmuş olduğunu görmeniz gerekir. Kalan alanlar için:

    * **Kaynak grubu**altında yeni **Oluştur** ' u seçerek yeni bir kaynak grubu oluşturun veya zaten bir tane varsa **var olanı kullan**' ı seçin.
    * Varsayılan **konumu**doğrulayın.
    * **Oluştur**'a tıklayın.
    * Seçim **Panoya sabitle**' ye tıklayın.

#### <a name="create-the-gateway-subnet"></a>Ağ geçidi alt ağını oluşturma

1. **Sanal ağ**altında **Tenant1VNet1**öğesini seçin.
1. **AYARLAR** altında **Alt ağlar**’ı seçin.
1. Sanal ağa bir ağ geçidi alt ağı eklemek için **+ Gateway alt ağını** seçin.
1. Alt ağın adı varsayılan olarak **GatewaySubnet** şeklinde ayarlanır. Ağ geçidi alt ağları özel bir durumdur ve doğru çalışması için bu adı kullanmalıdır.
1. **Adres aralığının** **10.1.0.0/24**olduğunu doğrulayın.
1. Ağ geçidi alt ağını oluşturmak için **Tamam** ' ı tıklatın.

#### <a name="create-the-virtual-network-gateway"></a>Sanal ağ geçidini oluşturma

1. Azure Stack Kullanıcı portalında **+ kaynak oluştur ' a**tıklayın.
1. **Azure Marketi**altında **ağ**' ı seçin.
1. Ağ kaynakları listesinden **Sanal ağ geçidi**’ni seçin.
1. **Ad** alanına **GW1**girin.
1. **Sanal ağ**'ı seçin.
1. Açılan listeden **Tenant1VNet1** öğesini seçin.
1. **Genel IP adresi**' ni seçin, sonra **genel IP adresi**' ni seçin ve **Yeni oluştur**' a tıklayın.
1. **Ad** alanına **GW1-PIP**yazın ve ardından **Tamam**' a tıklayın.
1. **VPN türü** için varsayılan olarak **yol tabanlı** seçili olmalıdır. Bu ayarı tutun.
1. **Abonelik** ve **Konum** seçeneklerinin doğruluğunu onaylayın. **Oluştur**'a tıklayın.

#### <a name="create-the-local-network-gateway"></a>Yerel ağ geçidini oluşturma

Yerel ağ geçidi kaynağı, VPN bağlantısının diğer ucundaki uzak ağ geçidini tanımlar. Bu örnekte, bağlantının uzak ucu ExpressRoute yönlendiricisinin LAN alt arabirimidir. Önceki diyagramda kiracı 1 için uzak adres 10.60.3.255 ' dir.

1. Azure Stack Kullanıcı portalında oturum açın ve **+ kaynak oluştur**' u seçin.
1. **Azure Marketi**altında **ağ**' ı seçin.
1. Kaynak listesinden **yerel ağ geçidi**’ni seçin.
1. **Ad** alanına **er-Router-GW**yazın.
1. **IP adresi** alanı için önceki şekle bakın. ExpressRoute yönlendirici LAN 10.60.3.255 alt arabiriminin IP adresi, kiracı 1 ' dir. Kendi ortamınız için, yönlendiricinizin karşılık gelen arabiriminin IP adresini girin.
1. **Adres alanı** alanına, Azure 'da bağlanmak istediğiniz sanal ağların adres alanını girin. Kiracı 1 için alt ağlar aşağıdaki gibidir:

   * 192.168.2.0/24, Azure 'daki hub sanal ağı.
   * 10.100.0.0/16, Azure 'daki bağlı olan VNet 'dir.

   > [!IMPORTANT]
   > Bu örnek, Azure Stack ağ geçidi ile ExpressRoute yönlendiricisi arasında siteden siteye VPN bağlantısı için statik yollar kullandığınızı varsayar.

1. **Aboneliğinizin**, **kaynak grubunuzun**ve **konumun** doğru olduğundan emin olun. Ardından **Oluştur**’u seçin.

#### <a name="create-the-connection"></a>Bağlantı oluşturma

1. Azure Stack Kullanıcı portalında **+ kaynak oluştur**' u seçin.
1. **Azure Marketi**altında **ağ**' ı seçin.
1. Kaynak listesinden **Bağlantı**’yı seçin.
1. **Temel bilgiler**altında, **bağlantı türü**olarak **siteden siteye (IPSec)** seçeneğini belirleyin.
1. **Abonelik**, **kaynak grubu**ve **konum**' u seçin. **Tamam**'ı tıklatın.
1. **Ayarlar**altında **sanal ağ geçidi**' ni seçin ve ardından **GW1**' ı seçin.
1. **Yerel ağ geçidi**' ni seçin ve ardından **er yönlendirici GW**' yi seçin.
1. **Bağlantı adı** alanına **connecttoazure**yazın.
1. **Paylaşılan anahtar (PSK)** alanına **abc123** girin ve ardından **Tamam**' ı seçin.
1. **Özet**altında **Tamam**' ı seçin.

#### <a name="get-the-virtual-network-gateway-public-ip-address"></a>Sanal ağ geçidi genel IP adresini al

Sanal ağ geçidini oluşturduktan sonra, ağ geçidinin genel IP adresini alabilirsiniz. Daha sonra dağıtımınız için ihtiyacınız olan bu adresi bir yere unutmayın. Dağıtımınıza bağlı olarak, bu adres **Iç IP adresi**olarak kullanılır.

1. Azure Stack Kullanıcı portalında **tüm kaynaklar**' ı seçin.
1. **Tüm kaynaklar**altında, örnekte **GW1** olan sanal ağ geçidini seçin.
1. **Sanal ağ geçidi**altında, kaynak listesinden **genel bakış** ' ı seçin. Alternatif olarak, **Özellikler**' i de seçebilirsiniz.
1. Notuna istediğiniz IP adresi **genel IP adresi**altında listelenmiştir. Örnek yapılandırma için, bu adres 192.68.102.1 ' dir.

#### <a name="create-a-virtual-machine-vm"></a>Sanal makine (VM) oluşturma

VPN bağlantısı üzerinden veri trafiğini test etmek için VM 'Lerin Azure Stack VNet 'te veri alıp gönderebilmesi gerekir. Bir VM oluşturun ve sanal ağınızın VM alt ağına dağıtın.

1. Azure Stack Kullanıcı portalında **+ kaynak oluştur**' u seçin.
1. **Azure Marketi**altında **işlem**' i seçin.
1. VM görüntüleri listesinde, **Windows Server 2016 Datacenter eval** görüntüsünü seçin.

   >[!NOTE]
   >Bu makale için kullanılan görüntü yoksa, Azure Stack işleçinizden farklı bir Windows Server görüntüsü sağlamasını isteyin.

1. **Sanal makine oluştur**' da, **temel bilgiler**' i seçin ve **ad**olarak **VM01** yazın.
1. Geçerli bir Kullanıcı adı ve parola girin. Bu hesabı, oluşturulduktan sonra VM 'de oturum açmak için kullanacaksınız.
1. Bir **abonelik**, **kaynak grubu**ve **konum**sağlayın. **Tamam**’ı seçin.
1. **Boyut seçin**altında Bu örnek IÇIN bir VM boyutu seçin ve ardından **Seç**' i seçin.
1. **Ayarlar**altında şunları onaylayın:

   * Sanal ağ **Tenant1VNet1**.
   * Alt ağ **10.1.1.0/24**olarak ayarlanır.

   Varsayılan ayarları kullanın ve **Tamam**' a tıklayın.

1. **Özet**altında VM yapılandırmasını gözden geçirin ve ardından **Tamam**' a tıklayın.

Daha fazla kiracı eklemek için, bu bölümlerde izlediğiniz adımları yineleyin:

* [Sanal ağ ve VM alt ağını oluşturma](#create-the-virtual-network-and-vm-subnet)
* [Ağ geçidi alt ağını oluşturma](#create-the-gateway-subnet)
* [Sanal ağ geçidini oluşturma](#create-the-virtual-network-gateway)
* [Yerel ağ geçidini oluşturma](#create-the-local-network-gateway)
* [Bağlantıyı oluşturun](#create-the-connection)
* [Sanal makine oluşturun](#create-a-virtual-machine)

Kiracı 2 ' yi örnek olarak kullanıyorsanız, örtüşmelere engel olmak için IP adreslerini değiştirmeyi unutmayın.

### <a name="configure-the-nat-vm-for-gateway-traversal"></a>Ağ Geçidi geçişi için NAT VM 'yi yapılandırma

> [!IMPORTANT]
> Bu bölüm yalnızca ASDK dağıtımları içindir. NAT, çok düğümlü dağıtımlar için gerekli değildir.

ASDK, kendi içinde bulunur ve fiziksel konağın dağıtıldığı ağdan yalıtılmıştır. Ağ geçitlerinin bağlandığı VIP ağı dış değil; Ağ adresi çevirisi (NAT) gerçekleştiren bir yönlendiricinin arkasında gizlenir.

Yönlendirici, Yönlendirme ve uzaktan erişim Hizmetleri (RRAS) rolünü çalıştıran ASDK ana bilgisayarı. Siteden siteye VPN bağlantısının her iki uçta da bağlanmasını sağlamak için, ASDK konağında NAT yapılandırmanız gerekir.

#### <a name="configure-the-nat"></a>NAT 'yi yapılandırma

1. Azure Stack ana bilgisayarda Yönetici hesabınızla oturum açın.
1. Betiği yükseltilmiş bir PowerShell ıSE 'de çalıştırın. Bu betik, **dış BGPNAT adresinizi**döndürür.

   ```powershell
   Get-NetNatExternalAddress
   ```

1. NAT 'yi yapılandırmak için aşağıdaki PowerShell betiğini kopyalayın ve düzenleyin. `External BGPNAT address` Ve`Internal IP address` değerlerini aşağıdaki örnek değerlerle değiştirecek şekilde betiği düzenleyin:

   * *Dış BGPNAT adresi* için 10.10.0.62 kullanın
   * *Iç IP adresi* için 192.168.102.1 kullanın

   Yükseltilmiş bir PowerShell ıSE 'den aşağıdaki betiği çalıştırın:

   ```powershell
   $ExtBgpNat = 'External BGPNAT address'
   $IntBgpNat = 'Internal IP address'

   # Designate the external NAT address for the ports that use the IKE authentication.
   Add-NetNatExternalAddress `
      -NatName BGPNAT `
      -IPAddress $Using:ExtBgpNat `
      -PortStart 499 `
      -PortEnd 501
   Add-NetNatExternalAddress `
      -NatName BGPNAT `
      -IPAddress $Using:ExtBgpNat `
      -PortStart 4499 `
      -PortEnd 4501
   # Create a static NAT mapping to map the external address to the Gateway public IP address to map the ISAKMP port 500 for PHASE 1 of the IPSEC tunnel.
   Add-NetNatStaticMapping `
      -NatName BGPNAT `
      -Protocol UDP `
      -ExternalIPAddress $Using:ExtBgpNat `
      -InternalIPAddress $Using:IntBgpNat `
      -ExternalPort 500 `
      -InternalPort 500
   # Configure NAT traversal which uses port 4500 to  establish the complete IPSEC tunnel over NAT devices.
   Add-NetNatStaticMapping `
      -NatName BGPNAT `
      -Protocol UDP `
      -ExternalIPAddress $Using:ExtBgpNat `
      -InternalIPAddress $Using:IntBgpNat `
      -ExternalPort 4500 `
      -InternalPort 4500
   ```

## <a name="configure-azure"></a>Azure 'ı yapılandırma

Azure Stack yapılandırmayı tamamladıktan sonra, Azure kaynaklarını dağıtabilirsiniz. Aşağıdaki şekilde, Azure 'daki bir kiracı sanal ağı örneği gösterilmektedir. Azure 'da VNet 'iniz için herhangi bir ad ve adres şeması kullanabilirsiniz. Ancak, Azure ve Azure Stack sanal ağları 'nın adres aralığı benzersiz olmalı ve örtüşmemelidir:

![Azure sanal ağları](media/azure-stack-connect-expressroute/AzureArchitecture.png)

Azure 'da dağıttığınız kaynaklar, Azure Stack dağıttığınız kaynaklara benzerdir. Aşağıdaki bileşenleri dağıtırsınız:

* Sanal ağlar ve alt ağlar
* Ağ geçidi alt ağı
* Bir sanal ağ geçidi
* Bir bağlantı
* ExpressRoute devresi

Örnek Azure ağ altyapısı aşağıdaki şekilde yapılandırılır:

* Standart Hub (192.168.2.0/24) ve bağlı bileşen (10.100.0.0./16) VNet modeli. Hub-ışınsal-uç ağ topolojisi hakkında daha fazla bilgi için bkz. [Azure 'da Merkez-uç ağ topolojisi uygulama](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke).
* İş yükleri, bağlı olan VNet 'e dağıtılır ve ExpressRoute bağlantı hattı hub VNet 'e bağlanır.
* İki sanal ağ, VNet eşlemesi kullanılarak bağlanır.

### <a name="configure-the-azure-vnets"></a>Azure sanal ağlarını yapılandırma

1. Azure portal Azure kimlik bilgilerinizle oturum açın.
1. 192.168.2.0/24 adres aralığını kullanarak hub VNet oluşturun.
1. 192.168.2.0/25 adres aralığını kullanarak bir alt ağ oluşturun ve 192.168.2.128/27 adres aralığını kullanarak bir ağ geçidi alt ağı ekleyin.
1. 10.100.0.0/16 adres aralığını kullanarak bağlı olan VNet ve alt ağ oluşturun.

Azure 'da sanal ağlar oluşturma hakkında daha fazla bilgi için bkz. [sanal ağ oluşturma](/azure/virtual-network/manage-virtual-network#create-a-virtual-network).

### <a name="configure-an-expressroute-circuit"></a>ExpressRoute devresini yapılandırma

1. [ExpressRoute önkoşulları & denetim listesini](/azure/expressroute/expressroute-prerequisites)gözden geçirin.

1. Azure aboneliğinizi kullanarak bir ExpressRoute bağlantı hattı oluşturmak için [bir ExpressRoute bağlantı hattı oluşturma ve değiştirme](/azure/expressroute/expressroute-howto-circuit-portal-resource-manager) bölümündeki adımları izleyin.

   >[!NOTE]
   >Kendi son ucunda ExpressRoute bağlantı hattını ayarlayabilmeleri için, hizmet anahtarınızı hizmetinize yönelik hizmet anahtarına verin.

1. ExpressRoute bağlantı hattının özel eşlemesini yapılandırmak için [bir ExpressRoute bağlantı hattı oluşturma ve değiştirme](/azure/expressroute/expressroute-howto-routing-portal-resource-manager) içindeki adımları izleyin.

### <a name="create-the-virtual-network-gateway"></a>Sanal ağ geçidini oluşturma

Hub VNet 'te ExpressRoute için sanal ağ geçidi oluşturmak üzere [PowerShell kullanarak ExpressRoute için sanal ağ geçidini yapılandırma](/azure/expressroute/expressroute-howto-add-gateway-resource-manager) bölümündeki adımları izleyin.

### <a name="create-the-connection"></a>Bağlantı oluşturma

ExpressRoute bağlantı hattını hub VNet 'e bağlamak için [sanal ağı bir ExpressRoute devresine bağlama](/azure/expressroute/expressroute-howto-linkvnet-portal-resource-manager)bölümündeki adımları izleyin.

### <a name="peer-the-vnets"></a>Sanal ağları eşleme

[Azure Portal kullanarak sanal ağ eşlemesi oluşturma](/azure/virtual-network/virtual-networks-create-vnetpeering-arm-portal)bölümündeki adımları kullanarak hub 'ı ve bağlı bileşen VNET 'leri eşler. VNet eşlemesini yapılandırırken, aşağıdaki seçenekleri kullandığınızdan emin olun:

* Hub 'dan bağlı olarak, **ağ geçidi aktarımına Izin verin**.
* Bağlı olan hub 'a, **uzak ağ geçidini kullanın**.

### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

İş yükü VM 'lerinizi, bağlı olan VNet 'e dağıtın.

Azure 'da ilgili ExpressRoute devreleri aracılığıyla bağlamak istediğiniz ek kiracı sanal ağları için bu adımları tekrarlayın.

## <a name="configure-the-router"></a>Yönlendiriciyi yapılandırma

ExpressRoute yönlendiricinizi yapılandırmak için aşağıdaki ExpressRoute yönlendirici yapılandırma diyagramını bir kılavuz olarak kullanabilirsiniz. Bu şekilde, ilgili ExpressRoute devrelerine sahip iki kiracı (kiracı 1 ve kiracı 2) gösterilmektedir. Her kiracı, ExpressRoute yönlendiricisinin LAN ve WAN tarafında kendi VRF 'ye (sanal Yönlendirme ve Iletme) bağlanır. Bu yapılandırma, iki kiracı arasında uçtan uca yalıtımın olmasını sağlar. Yapılandırma örneğini takip ettiğiniz için, yönlendirici arabirimlerinde kullanılan IP adreslerini göz önünde atın.

![ExpressRoute yönlendirici yapılandırması](media/azure-stack-connect-expressroute/EndToEnd.png)

Azure Stack 'ten siteye VPN bağlantısını sonlandırmak için IKEv2 VPN ve BGP 'yi destekleyen herhangi bir yönlendiriciyi kullanabilirsiniz. ExpressRoute bağlantı hattı kullanılarak Azure 'a bağlanmak için aynı yönlendirici kullanılır.

Aşağıdaki Cisco Site Recovery 1000 serisi toplama Hizmetleri yönlendirici yapılandırma örneği, *ExpressRoute yönlendirici yapılandırması* diyagramında gösterilen ağ altyapısını destekler.

```shell
ip vrf Tenant 1
 description Routing Domain for PRIVATE peering to Azure for Tenant 1
 rd 1:1
!
ip vrf Tenant 2
 description Routing Domain for PRIVATE peering to Azure for Tenant 2
 rd 1:5
!
crypto ikev2 proposal V2-PROPOSAL2
description IKEv2 proposal for Tenant 1
encryption aes-cbc-256
 integrity sha256
 group 2
crypto ikev2 proposal V4-PROPOSAL2
description IKEv2 proposal for Tenant 2
encryption aes-cbc-256
 integrity sha256
 group 2
!
crypto ikev2 policy V2-POLICY2
description IKEv2 Policy for Tenant 1
match fvrf Tenant 1
 match address local 10.60.3.255
 proposal V2-PROPOSAL2
description IKEv2 Policy for Tenant 2
crypto ikev2 policy V4-POLICY2
 match fvrf Tenant 2
 match address local 10.60.3.251
 proposal V4-PROPOSAL2
!
crypto ikev2 profile V2-PROFILE
description IKEv2 profile for Tenant 1
match fvrf Tenant 1
 match address local 10.60.3.255
 match identity remote any
 authentication remote pre-share key abc123
 authentication local pre-share key abc123
 ivrf Tenant 1
!
crypto ikev2 profile V4-PROFILE
description IKEv2 profile for Tenant 2
 match fvrf Tenant 2
 match address local 10.60.3.251
 match identity remote any
 authentication remote pre-share key abc123
 authentication local pre-share key abc123
 ivrf Tenant 2
!
crypto ipsec transform-set V2-TRANSFORM2 esp-gcm 256
 mode tunnel
crypto ipsec transform-set V4-TRANSFORM2 esp-gcm 256
 mode tunnel
!
crypto ipsec profile V2-PROFILE
 set transform-set V2-TRANSFORM2
 set ikev2-profile V2-PROFILE
!
crypto ipsec profile V4-PROFILE
 set transform-set V4-TRANSFORM2
 set ikev2-profile V4-PROFILE
!
interface Tunnel10
description S2S VPN Tunnel for Tenant 1
 ip vrf forwarding Tenant 1
 ip address 11.0.0.2 255.255.255.252
 ip tcp adjust-mss 1350
 tunnel source TenGigabitEthernet0/1/0.211
 tunnel mode ipsec ipv4
 tunnel destination 10.10.0.62
 tunnel vrf Tenant 1
 tunnel protection ipsec profile V2-PROFILE
!
interface Tunnel20
description S2S VPN Tunnel for Tenant 2
 ip vrf forwarding Tenant 2
 ip address 11.0.0.2 255.255.255.252
 ip tcp adjust-mss 1350
 tunnel source TenGigabitEthernet0/1/0.213
 tunnel mode ipsec ipv4
 tunnel destination 10.10.0.62
 tunnel vrf VNET3
 tunnel protection ipsec profile V4-PROFILE
!
interface GigabitEthernet0/0/1
 description PRIMARY ExpressRoute Link to AZURE over Equinix
 no ip address
 negotiation auto
!
interface GigabitEthernet0/0/1.100
description Primary WAN interface of Tenant 1
 description PRIMARY ER link supporting Tenant 1 to Azure
 encapsulation dot1Q 101
 ip vrf forwarding Tenant 1
 ip address 192.168.1.1 255.255.255.252
!
interface GigabitEthernet0/0/1.102
description Primary WAN interface of Tenant 2
 description PRIMARY ER link supporting Tenant 2 to Azure
 encapsulation dot1Q 102
 ip vrf forwarding Tenant 2
 ip address 192.168.1.17 255.255.255.252
!
interface GigabitEthernet0/0/2
 description BACKUP ExpressRoute Link to AZURE over Equinix
 no ip address
 negotiation auto
!
interface GigabitEthernet0/0/2.100
description Secondary WAN interface of Tenant 1
 description BACKUP ER link supporting Tenant 1 to Azure
 encapsulation dot1Q 101
 ip vrf forwarding Tenant 1
 ip address 192.168.1.5 255.255.255.252
!
interface GigabitEthernet0/0/2.102
description Secondary WAN interface of Tenant 2
description BACKUP ER link supporting Tenant 2 to Azure
 encapsulation dot1Q 102
 ip vrf forwarding Tenant 2
 ip address 192.168.1.21 255.255.255.252
!
interface TenGigabitEthernet0/1/0
 description Downlink to ---Port 1/47
 no ip address
!
interface TenGigabitEthernet0/1/0.211
 description LAN interface of Tenant 1
description Downlink to --- Port 1/47.211
 encapsulation dot1Q 211
 ip vrf forwarding Tenant 1
 ip address 10.60.3.255 255.255.255.254
!
interface TenGigabitEthernet0/1/0.213
description LAN interface of Tenant 2
 description Downlink to --- Port 1/47.213
 encapsulation dot1Q 213
 ip vrf forwarding Tenant 2
 ip address 10.60.3.251 255.255.255.254
!
router bgp 65530
 bgp router-id <removed>
 bgp log-neighbor-changes
 description BGP neighbor config and route advertisement for Tenant 1 VRF
 address-family ipv4 vrf Tenant 1
  network 10.1.0.0 mask 255.255.0.0
  network 10.60.3.254 mask 255.255.255.254
  network 192.168.1.0 mask 255.255.255.252
  network 192.168.1.4 mask 255.255.255.252
  neighbor 10.10.0.62 remote-as 65100
  neighbor 10.10.0.62 description VPN-BGP-PEER-for-Tenant 1
  neighbor 10.10.0.62 ebgp-multihop 5
  neighbor 10.10.0.62 activate
  neighbor 10.60.3.254 remote-as 4232570301
  neighbor 10.60.3.254 description LAN peer for CPEC:INET:2112 VRF
  neighbor 10.60.3.254 activate
  neighbor 10.60.3.254 route-map BLOCK-ALL out
  neighbor 192.168.1.2 remote-as 12076
  neighbor 192.168.1.2 description PRIMARY ER peer for Tenant 1 to Azure
  neighbor 192.168.1.2 ebgp-multihop 5
  neighbor 192.168.1.2 activate
  neighbor 192.168.1.2 soft-reconfiguration inbound
  neighbor 192.168.1.2 route-map Tenant 1-ONLY out
  neighbor 192.168.1.6 remote-as 12076
  neighbor 192.168.1.6 description BACKUP ER peer for Tenant 1 to Azure
  neighbor 192.168.1.6 ebgp-multihop 5
  neighbor 192.168.1.6 activate
  neighbor 192.168.1.6 soft-reconfiguration inbound
  neighbor 192.168.1.6 route-map Tenant 1-ONLY out
  maximum-paths 8
 exit-address-family
 !
description BGP neighbor config and route advertisement for Tenant 2 VRF
address-family ipv4 vrf Tenant 2
  network 10.1.0.0 mask 255.255.0.0
  network 10.60.3.250 mask 255.255.255.254
  network 192.168.1.16 mask 255.255.255.252
  network 192.168.1.20 mask 255.255.255.252
  neighbor 10.10.0.62 remote-as 65300
  neighbor 10.10.0.62 description VPN-BGP-PEER-for-Tenant 2
  neighbor 10.10.0.62 ebgp-multihop 5
  neighbor 10.10.0.62 activate
  neighbor 10.60.3.250 remote-as 4232570301
  neighbor 10.60.3.250 description LAN peer for CPEC:INET:2112 VRF
  neighbor 10.60.3.250 activate
  neighbor 10.60.3.250 route-map BLOCK-ALL out
  neighbor 192.168.1.18 remote-as 12076
  neighbor 192.168.1.18 description PRIMARY ER peer for Tenant 2 to Azure
  neighbor 192.168.1.18 ebgp-multihop 5
  neighbor 192.168.1.18 activate
  neighbor 192.168.1.18 soft-reconfiguration inbound
  neighbor 192.168.1.18 route-map VNET-ONLY out
  neighbor 192.168.1.22 remote-as 12076
  neighbor 192.168.1.22 description BACKUP ER peer for Tenant 2 to Azure
  neighbor 192.168.1.22 ebgp-multihop 5
  neighbor 192.168.1.22 activate
  neighbor 192.168.1.22 soft-reconfiguration inbound
  neighbor 192.168.1.22 route-map VNET-ONLY out
  maximum-paths 8
 exit-address-family
!
ip forward-protocol nd
!
ip as-path access-list 1 permit ^$
ip route vrf Tenant 1 10.1.0.0 255.255.0.0 Tunnel10
ip route vrf Tenant 2 10.1.0.0 255.255.0.0 Tunnel20
!
ip prefix-list BLOCK-ALL seq 5 deny 0.0.0.0/0 le 32
!
route-map BLOCK-ALL permit 10
 match ip address prefix-list BLOCK-ALL
!
route-map VNET-ONLY permit 10
 match as-path 1
!
```

## <a name="test-the-connection"></a>Bağlantıyı test et

Siteden siteye bağlantı kurup ExpressRoute devresini oluşturduktan sonra bağlantınızı test edin.

Aşağıdaki ping testlerini gerçekleştirin:

* Azure sanal makinelerinizdeki VM 'lerden birinde oturum açın ve Azure Stack oluşturduğunuz sanal makineye ping gönderin.
* Azure Stack oluşturduğunuz VM 'lerden birinde oturum açın ve Azure VNet 'te oluşturduğunuz sanal makineye ping gönderin.

>[!NOTE]
>Siteden siteye ve ExpressRoute bağlantıları üzerinden trafik gönderdiğinizden emin olmak için VM 'nin ayrılmış IP (DIP) adresini VM 'nin VIP adresini değil, her iki uçta da ping yapmanız gerekir.

### <a name="allow-icmp-in-through-the-firewall"></a>Güvenlik duvarında ıCMP 'ye izin ver

Varsayılan olarak, Windows Server 2016, güvenlik duvarı üzerinden gelen ıCMP paketlerine izin vermez. Ping testleri için kullandığınız her VM için, gelen ıCMP paketlerine izin vermeniz gerekir. ICMP için bir güvenlik duvarı kuralı oluşturmak üzere, yükseltilmiş bir PowerShell penceresinde aşağıdaki cmdlet 'i çalıştırın:

```powershell
# Create ICMP firewall rule.
New-NetFirewallRule `
  -DisplayName "Allow ICMPv4-In" `
  -Protocol ICMPv4
```

### <a name="ping-the-azure-stack-vm"></a>Azure Stack VM 'ye ping gönderin

1. Azure Stack Kullanıcı portalında oturum açın.

1. Oluşturduğunuz VM 'yi bulun ve seçin.

1. **Bağlan**’ı seçin.

1. Yükseltilmiş bir Windows veya PowerShell komut isteminde **ipconfig/all**yazın. Çıktıda döndürülen IPv4 adresini aklınızda edin.

1. Azure VNet 'teki VM 'den IPv4 adresine ping gönderin.

   Örnek ortamda, IPv4 adresi 10.1.1. x/24 alt ağından olur. Ortamınızda adres farklı olabilir, ancak kiracı VNet alt ağı için oluşturduğunuz alt ağda olmalıdır.

### <a name="view-data-transfer-statistics"></a>Veri aktarımı istatistiklerini görüntüleme

Bağlantınız üzerinden ne kadar trafik geçtiğini öğrenmek istiyorsanız, bu bilgileri Azure Stack Kullanıcı portalında bulabilirsiniz. Veri aktarımı istatistiklerini görüntülemek, ping testi verilerinizin VPN ve ExpressRoute bağlantıları üzerinden yapılıp yapılmayacağını öğrenmek için de iyi bir yoldur:

1. Azure Stack Kullanıcı portalında oturum açın ve **tüm kaynaklar**' ı seçin.
1. VPN Gateway için kaynak grubuna gidin ve **bağlantı** nesnesi türünü seçin.
1. Listeden **Connecttoazure** bağlantısını seçin.
1. Bağlantılara > **genel bakış**' ın altında, **içindeki verilerin** istatistiklerini ve **verileri dışarı**görebilirsiniz. Sıfır olmayan bazı değerler görmeniz gerekir.

   ![Içindeki veriler ve veriler](media/azure-stack-connect-expressroute/DataInDataOut.png)

## <a name="next-steps"></a>Sonraki adımlar

[Uygulamaları Azure 'a dağıtma ve Azure Stack](../user/azure-stack-solution-pipeline.md )
