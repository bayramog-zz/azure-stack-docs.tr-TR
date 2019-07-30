---
title: Azure ve Azure Stack karma bulut bağlantısı yapılandırma | Microsoft Docs
description: Azure ve Azure Stack karma bulut bağlantısını yapılandırmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: bryanla
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/14/2019
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: a2311466bc6949bbc1ea372410373654352418fa
ms.sourcegitcommit: 35b13ea6dc0221a15cd0840be796f4af5370ddaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68603027"
---
# <a name="configure-hybrid-cloud-connectivity-with-azure-and-azure-stack"></a>Azure ve Azure Stack ile karma bulut bağlantısı yapılandırma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Küresel Azure 'da güvenlik ve karma bağlantı modelini kullanarak Azure Stack kaynaklara erişebilirsiniz.

Bu çözümde, aşağıdakileri yapmak için bir örnek ortam oluşturacaksınız:

> [!div class="checklist"]
> - Şirket içindeki verileri gizlilik veya yasal gereksinimleri karşılayacak şekilde saklayın, ancak küresel Azure kaynaklarına erişimi sürdürün.
> - Bulut ölçekli uygulama dağıtımı ve küresel Azure 'da kaynakları kullanırken eski bir sistemi koruyun.

> [!Tip]  
> ![karma pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack, Azure'nın bir uzantısıdır. Azure Stack, bulut bilgi işlemin çevikliğini ve yeniliklerini şirket içi ortamınıza getirerek, karma uygulamaları her yerde oluşturup dağıtmanıza imkan tanıyan tek karma bulutu etkinleştirir.  
> 
> [Karma uygulamalar Için tasarım konuları](azure-stack-edge-pattern-overview.md) , karma uygulamalar tasarlamak, dağıtmak ve çalıştırmak için yazılım kalitesine (yerleştirme, ölçeklenebilirlik, kullanılabilirlik, dayanıklılık, yönetilebilirlik ve güvenlik) göre önemli noktalar inceler. Tasarım konuları karma uygulama tasarımını iyileştirirken, üretim ortamlarındaki zorlukları en aza indirmeyle ilgili olarak size yardımcı olur.


## <a name="prerequisites"></a>Önkoşullar

Karma bağlantı dağıtımı oluşturmak için birkaç bileşen gereklidir. Bu bileşenlerden bazılarının hazırlanması zaman alabilir, bu nedenle plana göre plan yapın.

**Azure Stack**

Bir Azure OEM/donanım ortağı bir üretim Azure Stack dağıtabilir ve tüm kullanıcılar bir Azure Stack Geliştirme Seti (ASDK) dağıtabilir.

**Azure Stack bileşenleri**

Azure Stack operatörü App Service dağıtmalı, planlar ve teklifler oluşturmalı, bir kiracı aboneliği oluşturacak ve Windows Server 2016 görüntüsünü ekleyecek. Bu bileşenlere zaten sahipseniz, bu çözüme başlamadan önce gereksinimleri karşıladığından emin olun.

Bu çözüm, Azure ve Azure Stack ilgili bazı temel bilgilere sahip olduğunuzu varsayar. Çözümü başlatmadan önce daha fazla bilgi edinmek için aşağıdaki makaleleri okuyun:

 - [Azure'a giriş](https://azure.microsoft.com/overview/what-is-azure/)
 - [Azure Stack temel kavramları](../operator/azure-stack-overview.md)

### <a name="azure"></a>Azure

 - Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
 - Azure 'da bir [Web uygulaması](https://docs.microsoft.com/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?view=vsts&tabs=vsts) oluşturun. Web uygulaması URL 'sini, çözümde gerekli olacak şekilde bir yere getirin.

### <a name="azure-stack"></a>Azure Stack

 - Üretim Azure Stack kullanın veya Azure Stack Geliştirme Seti ' dan https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1 dağıtın.
   >[!Note]
   >ASDK 'yi dağıtmak en fazla 7 saat sürebilir, bu nedenle plana göre plan yapabilirsiniz.

 - [App Service](../operator/azure-stack-app-service-deploy.md) PaaS hizmetlerini Azure Stack olarak dağıtın.
 - Azure Stack ortamında [planlar ve teklifler oluşturun](../operator/azure-stack-plan-offer-quota-overview.md) .
 - Azure Stack ortamı içinde [Kiracı aboneliği oluşturun](../operator/azure-stack-subscribe-plan-provision-vm.md) .

### <a name="before-you-begin"></a>Başlamadan önce

Karma bulut bağlantısını yapılandırmaya başlamadan önce aşağıdaki ölçütleri karşıladığınızdan emin olun:

 - VPN cihazınız için dışarıdan kullanıma açık bir genel IPv4 adresine ihtiyacınız vardır. Bu IP adresi bir NAT (ağ adresi çevirisi) arkasında bulunamaz.
 - Tüm kaynaklar aynı bölgeye/konuma dağıtılır.

####  <a name="solution-example-values"></a>çözüm örneği değerleri

Bu çözümdeki örnekler aşağıdaki değerleri kullanır. Bu değerleri kullanarak bir test ortamı oluşturabilir veya örnekleri daha iyi anlamak için bunlara başvurabilirsiniz. Genel olarak VPN Gateway ayarları hakkında daha fazla bilgi için bkz. [VPN Gateway ayarları hakkında](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings).

Bağlantı belirtimleri:

 - **VPN türü**: rota tabanlı
 - **Bağlantı türü**: siteden siteye (IPSec)
 - **Ağ geçidi türü**: VPN
 - **Azure bağlantı adı**: Azure-Gateway-AzureStack-S2SGateway (Portal bu değeri otomatik olarak dolduracaktır)
 - **Azure Stack bağlantı adı**: AzureStack-Gateway-Azure-S2SGateway (Portal bu değeri otomatik olarak dolduracaktır)
 - **Paylaşılan anahtar**: her iki bağlantı için eşleşen değerlerle birlikte VPN donanımıyla uyumlu
 - **Abonelik**: tercih edilen tüm abonelikler
 - **Kaynak grubu**: Test-Infra

Ağ ve alt ağ IP adresleri:

| Azure/Azure Stack bağlantısı | Ad | Subnet | IP Adresi |
|-------------------------------------|---------------------------------------------|---------------------------------------|-----------------------------|
| Azure vNet | ApplicationvNet<br>10.100.102.9/23 | ApplicationSubnet<br>10.100.102.0/24 |  |
|  |  | GatewaySubnet<br>10.100.103.0/24 |  |
| Azure Stack vNet | ApplicationvNet<br>10.100.100.0/23 | ApplicationSubnet <br>10.100.100.0/24 |  |
|  |  | GatewaySubnet <br>10.100101.0/24 |  |
| Azure sanal ağ geçidi | Azure-Gateway |  |  |
| Azure Stack sanal ağ geçidi | AzureStack-Gateway |  |  |
| Azure genel IP | Azure-Gatewaypublicıp |  | Oluşturma sırasında belirlenir |
| Azure Stack genel IP | AzureStack-Gatewaypublicıp |  | Oluşturma sırasında belirlenir |
| Azure yerel ağ geçidi | AzureStack-S2SGateway<br>   10.100.100.0/23 |  | Azure Stack genel IP değeri |
| Yerel ağ geçidi Azure Stack | Azure-S2SGateway<br>10.100.102.0/23 |  | Azure genel IP değeri |

## <a name="create-a-virtual-network-in-global-azure-and-azure-stack"></a>Küresel Azure ve Azure Stack sanal ağ oluşturma

Portalı kullanarak bir sanal ağ oluşturmak için aşağıdaki adımları kullanın. Bu makaleyi yalnızca bir çözüm olarak kullanıyorsanız bu [örnek değerleri](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#values) kullanabilirsiniz. Bu makaleyi bir üretim ortamını yapılandırmak için kullanıyorsanız, örnek ayarlarını kendi değerlerinizle değiştirin.

> [!IMPORTANT]
> Azure veya Azure Stack vNet adres alanlarında IP adreslerinden bir çakışma olmadığından emin olmanız gerekir.

Azure 'da vNet oluşturmak için:

1. [Azure Portal](https://portal.azure.com/) bağlanmak ve Azure hesabınızla oturum açmak için tarayıcınızı kullanın.
2. Seçin **kaynak Oluştur**. Market 'te **Ara** alanına ' sanal ağ ' yazın. Sonuçlardan **sanal ağ** ' ı seçin.
3. **Bir dağıtım modeli seçin** listesinden **Kaynak Yöneticisi**' yi seçin ve ardından **Oluştur**' u seçin.
4. **Sanal ağ oluştur**' da, VNET ayarlarını yapılandırın. Gerekli alan adlarına, kırmızı bir yıldız işareti eklenir.  Geçerli bir değer girdiğinizde, yıldız işareti yeşil onay işaretine dönüşür.

Azure Stack içinde vNet oluşturmak için:

* Azure Stack **kiracı portalını**kullanarak önceki adımları (1-4) yineleyin.

## <a name="add-a-gateway-subnet"></a>Ağ geçidi alt ağı ekleme

Sanal ağınızı bir ağ geçidine bağlamadan önce, bağlanmak istediğiniz sanal ağ için ağ geçidi alt ağını oluşturmanız gerekir. Ağ geçidi Hizmetleri ağ geçidi alt ağında belirttiğiniz IP adreslerini kullanır.

[Azure Portal](https://portal.azure.com/), sanal ağ geçidini oluşturmak istediğiniz kaynak yöneticisi sanal ağa gidin.

1. **Sanal ağ** sayfasını açmak için vNet ' i seçin.
2. **Ayarlar**' da **alt ağlar**' ı seçin.
3. Alt **ağlar** sayfasında, **+ Gateway alt** Ağı ' nı seçerek **alt ağ ekle** sayfasını açın.

    ![Ağ geçidi alt ağı ekle](media/azure-stack-solution-hybrid-connectivity/image4.png)

4. Alt ağ **adı** ' gatewaysubnet ' değeri ile otomatik olarak doldurulur. Azure alt ağın ağ geçidi alt ağı tanınması bu değer gereklidir.
5. Yapılandırma gereksinimlerinize uyacak şekilde sağlanmış olan **adres aralığı** değerlerini değiştirin ve ardından **Tamam**' ı seçin.

## <a name="create-a-virtual-network-gateway-in-azure-and-azure-stack"></a>Azure 'da ve Azure Stack bir sanal ağ geçidi oluşturun

Azure 'da bir sanal ağ geçidi oluşturmak için aşağıdaki adımları kullanın.

1. Portal sayfasının sol tarafında, arama alanına ' sanal ağ **+** Geçidi ' öğesini seçin ve girin.
2. **Sonuçlar**bölümünde **sanal ağ geçidi**' ni seçin.
3. Sanal **ağ geçidi**' nde **Oluştur** ' u seçerek **sanal ağ geçidi oluştur** sayfasını açın.
4. **Sanal ağ geçidi oluştur**sayfasında, ağ geçidinizin değerleri, **öğretici örnek değerleri**ve aşağıdaki ek değerler bölümünde gösterildiği gibi belirtin:

   - **SKU**: temel
   - **Sanal ağ**: Daha önce oluşturduğunuz sanal ağı seçin. Oluşturduğunuz ağ geçidi alt ağı otomatik olarak seçilir.
   - **Ilk IP yapılandırması**:  Ağ geçidinizin genel IP 'si.
     - **Ağ GEÇIDI IP yapılandırması oluştur**' u seçerek **genel IP adresi Seç** sayfasına gidersiniz.
     - **+ Yeni oluştur** ' u seçerek **genel IP adresi oluştur** sayfasını açın.
     - Genel IP adresiniz için bir **ad** girin. SKU 'YU **temel**olarak bırakın ve sonra değişikliklerinizi kaydetmek için **Tamam** ' ı seçin.

       > [!Note]
       > Şu anda VPN Gateway yalnızca dinamik genel IP adresi ayırmayı destekler. Ancak bu, IP adresinin VPN ağ geçidinize atandıktan sonra değiştiği anlamına gelmez. Genel IP adresi, yalnızca ağ geçidi silinip yeniden oluşturulduğunda değişir. VPN Gateway 'de yeniden boyutlandırma, sıfırlama veya diğer iç bakım/yükseltme işlemleri IP adresini değiştirmez.

4. Ağ Geçidi ayarlarınızı doğrulayın.
5. VPN ağ geçidini oluşturmak için **Oluştur** ' u seçin. Ağ Geçidi ayarları onaylanır ve panonuzda "sanal ağ geçidi dağıtma" kutucuğu görüntülenir.

   >[!Note]
   >Bir ağ geçidi oluşturma 45 dakika kadar sürebilir. Tamamlanma durumunu görmek için portal sayfanızı yenilemeniz gerekebilir.

    Ağ Geçidi oluşturulduktan sonra, portalda sanal ağa bakarak kendisine atanan IP adresini görebilirsiniz. Ağ geçidi bağlı bir cihaz gibi görüntülenir. Ağ Geçidi hakkında daha fazla bilgi görmek için cihazı seçin.

6. Azure Stack dağıtımınızda önceki adımları (1-5) yineleyin.

## <a name="create-the-local-network-gateway-in-azure-and-azure-stack"></a>Azure 'da yerel ağ geçidini oluşturma ve Azure Stack

Yerel ağ geçidi genellikle şirket içi konumunuz anlamına gelir. Siteye Azure veya Azure Stack başvurabileceği bir ad verin ve şunları belirtin:

- Bağlantısını oluşturmakta olduğunuz şirket içi VPN cihazının IP adresi.
- VPN ağ geçidi üzerinden VPN cihazına yönlendirilecek IP adresi önekleri. Belirttiğiniz adres ön ekleri, şirket içi adresinizde yer alan ön eklerdir.

  >[!Note]
  >Şirket içi ağınız değişirse veya VPN cihazının genel IP adresini değiştirmeniz gerekiyorsa, bu değerleri daha sonra kolayca güncelleştirebilirsiniz.

1. Portalda **+ kaynak oluştur**' u seçin.
2. Arama kutusuna **yerel ağ geçidi**' ni girip aramak için **ENTER** ' u seçin. Sonuçların bir listesi görüntülenir.
3. Yerel ağ **geçidi**oluştur ' u seçin ve ardından **Oluştur** ' u seçerek **yerel ağ geçidi oluştur** sayfasını açın.
4. **Yerel ağ geçidi oluştur**sayfasında, **öğretici örnek değerlerimizi**kullanarak yerel ağ geçidinizin değerlerini belirtin. Aşağıdaki ek değerleri ekleyin.

    - **IP adresi**: Azure veya Azure Stack 'in bağlanmasını istediğiniz VPN cihazının genel IP adresi. Azure 'un adresle iletişime geçebilmesi için bir NAT 'nin arkasında olmayan geçerli bir genel IP adresi belirtin. IP adresi şu anda yoksa, örnekteki bir değeri yer tutucu olarak kullanabilirsiniz, ancak geri dönüp yer tutucuyu VPN cihazınızın genel IP adresiyle değiştirmeniz gerekir. Azure, geçerli bir adres sağlamadan cihaza bağlanamaz.
    - **Adres alanı**: Bu yerel ağın temsil ettiği ağ için adres aralığıdır. Birden fazla adres alanı aralığı ekleyebilirsiniz. Belirttiğiniz aralıkların, bağlanmak istediğiniz diğer ağların aralıklarıyla çakışmadığından emin olun. Azure, belirttiğiniz adres aralığını şirket içi VPN cihazının IP adresine yönlendirir. Şirket içi sitenize bağlanmak istiyorsanız, örnek bir değer değil, kendi değerlerinizi kullanın.
    - **BGP ayarlarını yapılandır**: Yalnızca BGP yapılandırılırken kullanın. Aksi takdirde, bu seçeneği seçmeyin.
    - **Abonelik**: Doğru aboneliğin gösterildiğini doğrulayın.
    - **Kaynak grubu**: Kullanmak istediğiniz kaynak grubunu seçin. Yeni bir kaynak grubu oluşturabilir veya önceden oluşturduğunuz bir kaynak seçebilirsiniz.
    - **Konum**: Bu nesnenin oluşturulacağı konumu seçin. Sanal ağınızın bulunduğu konumu seçmek isteyebilirsiniz, ancak bunu yapmanız gerekmez.
5. Gerekli değerleri belirtmeyi bitirdiğinizde, yerel ağ geçidini oluşturmak için **Oluştur** ' u seçin.
6. Azure Stack dağıtımınızda bu adımları (1-5) yineleyin.

## <a name="configure-your-connection"></a>Bağlantınızı yapılandırın

Şirket içi ağa yönelik siteden siteye bağlantılar için bir VPN cihazı gerekir. Yapılandırdığınız VPN cihazına bağlantı adı verilir. Bağlantınızı yapılandırmak için şunlar gerekir:

- Paylaşılan bir anahtar. Bu anahtar, siteden siteye VPN bağlantınızı oluştururken belirttiğiniz aynı paylaşılan anahtardır. Bu örneklerde temel bir paylaşılan anahtar kullanılır. Kullanmak için daha karmaşık bir anahtar oluşturmanız önerilir.
- Sanal ağ geçidinizin genel IP adresi. Azure Portal, PowerShell veya CLI kullanarak genel IP adresini görüntüleyebilirsiniz. Azure portal kullanarak VPN ağ geçidinizin genel IP adresini bulmak için sanal ağ geçitleri ' ne gidin ve ağ geçidinizin adını seçin.

Sanal ağ geçidiniz ile şirket içi VPN cihazınız arasında siteden siteye VPN bağlantısı oluşturmak için aşağıdaki adımları kullanın.

1. Azure portal **+ kaynak oluştur**' u seçin.
2. **Bağlantı**ara.
3. **Sonuçlar**' da **Bağlantılar**' ı seçin.
4. **Bağlantı**sayfasında **Oluştur**' u seçin.
5. **Bağlantı oluştur**sayfasında, aşağıdaki ayarları yapılandırın:

    - **Bağlantı türü**: Siteden siteye (IPSec) seçeneğini belirleyin.
    - **Kaynak grubu**: Test kaynak grubunuzu seçin.
    - **Sanal ağ geçidi**: Oluşturduğunuz sanal ağ geçidini seçin.
    - **Yerel ağ geçidi**: Oluşturduğunuz yerel ağ geçidini seçin.
    - **Bağlantı adı**: Bu ad, iki ağ geçidinin değerleri kullanılarak otomatik olarak doldurulur.
    - **Paylaşılan anahtar**: Bu değer, yerel şirket içi VPN cihazınız için kullanmakta olduğunuz değerle eşleşmelidir. Öğretici örneği ' abc123 ' kullanır, ancak daha karmaşık bir işlem kullanmanız gerekir. Önemli olan şey, bu değerin VPN cihazınızı yapılandırırken belirttiğiniz değerle aynı olması gerekır.
    - **Abonelik**, **kaynak grubu**ve **konum** değerleri sabittir.

6. Bağlantınızı oluşturmak için **Tamam ' ı** seçin.

Bağlantıyı, sanal ağ geçidinin **Bağlantılar** sayfasında görebilirsiniz. Durum, biline,  sonra da  *başarılı*olur.

## <a name="next-steps"></a>Sonraki adımlar

- Azure bulut desenleri hakkında daha fazla bilgi için bkz: [bulut tasarımı desenleri](https://docs.microsoft.com/azure/architecture/patterns).
