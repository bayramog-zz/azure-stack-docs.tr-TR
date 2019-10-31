---
title: Azure Stack 'de birden çok siteden siteye VPN tüneli ayarlama | Microsoft Docs
description: Azure Stack 'de birden çok siteden siteye VPN tüneli ayarlamayı öğrenin.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: how-to
ms.date: 09/19/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 09/19/2019
ms.openlocfilehash: d85de1892e2e6620249ff3a95ee2debb01b81981
ms.sourcegitcommit: cc3534e09ad916bb693215d21ac13aed1d8a0dde
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73167668"
---
# <a name="how-to-set-up-a-multiple-site-to-site-vpn-tunnel-in-azure-stack"></a>Azure Stack 'de birden çok siteden siteye VPN tüneli ayarlama

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu makalede, çözümü dağıtmak için bir Azure Stack Kaynak Yöneticisi şablonunun nasıl kullanılacağı gösterilmektedir. Çözüm, ilişkili sanal ağlarla birden fazla kaynak grubu oluşturur ve bu sistemlerin nasıl bağlanabileceğini öğrenin.

Şablonları [Azure akıllı kenar desenleri](https://github.com/Azure-Samples/azure-intelligent-edge-patterns) GitHub deposunda bulabilirsiniz. Şablon **RRAS-GRE-VNET-VNET** klasöründedir. 

## <a name="scenarios"></a>Senaryolar

![](./media/azure-stack-network-howto-vpn-tunnel/scenarios.png)

## <a name="create-multiple-vpn-tunnels"></a>Birden çok VPN tüneli oluşturma

![](./media/azure-stack-network-howto-vpn-tunnel/image1.png)

-  Üç katmanlı bir uygulama, Web, uygulama ve VERITABANı dağıtın.

-  İlk iki şablonu ayrı Azure Stack örneklerine dağıtın.

-  **Webtier** , PPE1 üzerine dağıtılacak ve **apptier** PPE2 üzerine dağıtılacak.

-  **Webtier** ve **apptier** 'ı bir IKE tüneliyle bağlayın.

-  **Apptier** 'ı, **dbtier**çağıracaksınız bir şirket içi sisteme bağlayın.

## <a name="steps-to-deploy-multiple-vpns"></a>Birden çok VPN dağıtma adımları

Bu, çok adımlı bir işlemdir. Bu çözüm için Azure Stack portalını kullanmaya devam edersiniz. Bununla birlikte, çıktıları yakalamak ve daha sonra giriş olarak kullanmak için PowerShell, Azure CLı veya kod olarak altyapı araç zincirlerini kullanabilirsiniz.

![alternatif metin](./media/azure-stack-network-howto-vpn-tunnel/image2.png)

## <a name="walkthrough"></a>Kılavuz

### <a name="deploy-web-tier-to-azure-stack-instances-ppe1"></a>Web katmanını Azure Stack örneklerine dağıtma PPE1

1.  Azure Stack Kullanıcı portalını açın ve **kaynak oluştur**' u seçin.

2.  **Şablon dağıtımı**' nı seçin.

    ![](./media/azure-stack-network-howto-vpn-tunnel/image3.png)

3.  Azuredeploy. JSON içeriğini kopyalayın ve **Azure-Intelligent-Edge-Patterns/RRAS-VNET-vpntunnel** deposundan şablon penceresine yapıştırın. Şablonda içerilen kaynakları görürsünüz, **Kaydet**' i seçin.

    ![](./media/azure-stack-network-howto-vpn-tunnel/image4.png)

4.  Bir **kaynak grubu** adı girin ve parametreleri denetleyin.

    > ! Notun  
    > WebTier adres alanı **10.10.0.0/16** olur ve kaynak grubu konumunun **PPE1** olduğunu görebilirsiniz

    ![](./media/azure-stack-network-howto-vpn-tunnel/image5.png)

### <a name="deploy-app-tier-to-the-second-azure-stack-instances"></a>İkinci Azure Stack örneklerine uygulama katmanını dağıtma

**Webtier** ile aynı işlemi, ancak burada gösterildiği gibi farklı parametreleri kullanabilirsiniz:

> [!Note]  
> AppTier adres alanı **10.20.0.0/16** olur ve kaynak grubu konumunu **WestUS2**olarak görebilirsiniz.

![](./media/azure-stack-network-howto-vpn-tunnel/image6.png)

### <a name="review-the-deployments-for-web-tier-and-app-tier-and-capture-outputs"></a>Web katmanı ve uygulama katmanı ve yakalama çıktıları için dağıtımları gözden geçirin

1.  Dağıtımın başarıyla tamamlandığını gözden geçirin. **Çıktılar**' i seçin.

    ![](./media/azure-stack-network-howto-vpn-tunnel/image7.png)

3.  İlk dört değeri Notepad uygulamanıza kopyalayın.

    ![](./media/azure-stack-network-howto-vpn-tunnel/image8.png)

5.  **Apptier** dağıtımı için tekrarlayın.

![](./media/azure-stack-network-howto-vpn-tunnel/image9.png)

![](./media/azure-stack-network-howto-vpn-tunnel/image10.png)

### <a name="create-tunnel-from-web-tier-to-app-tier"></a>Web katmanından uygulama katmanına Tünel Oluştur

1.  Azure Stack Kullanıcı portalını açın ve **kaynak oluştur**' u seçin.

2.  **Şablon dağıtımı**' nı seçin.

3.  İçeriği **azuredeploy. Tunnel. Ike. JSON**konumundan yapıştırın.

4.  **Parametreleri Düzenle**' yi seçin.

![](./media/azure-stack-network-howto-vpn-tunnel/image11.png)

### <a name="create-tunnel-from-app-tier-to-web-tier"></a>Uygulama katmanından Web katmanına tünel oluşturma

1.  Azure Stack Kullanıcı portalını açın ve **kaynak oluştur**' u seçin.

2.  **Şablon dağıtımı**'nı seçin.

3.  İçeriği **azuredeploy. Tunnel. Ike. JSON**konumundan yapıştırın.

4.  **Parametreleri Düzenle**' yi seçin.

![](./media/azure-stack-network-howto-vpn-tunnel/image12.png)

### <a name="viewing-tunnel-deployment"></a>Tünel dağıtımını görüntüleme

Özel Betik uzantısından çıktıyı görüntülediğinizde, oluşturulmakta olan tüneli görebilir ve durumu göstermesi gerekir. Diğer tarafın hazırlanmasının beklenmediğini ve diğer taraftan dağıtıldıktan sonra **bağlı** **olduğunu gösteren bir** tane görürsünüz.

![](./media/azure-stack-network-howto-vpn-tunnel/image13.png)

![](./media/azure-stack-network-howto-vpn-tunnel/image14.png)

![](./media/azure-stack-network-howto-vpn-tunnel/image15.png)

### <a name="troubleshooting-on-the-rras-vm"></a>RRAS VM 'de sorun giderme

1.  RDP kuralını **Reddet** durumundan **izin ver**olarak değiştirin.

2.  Dağıtım sırasında ayarladığınız kimlik bilgileri ile bir uzak masaüstü (DRP) istemcisiyle sisteme bağlanın.

3.  PowerShell 'i yükseltilmiş bir komut istemiyle açın ve `get-VPNS2SInterface`çalıştırın.

    ![](./media/azure-stack-network-howto-vpn-tunnel/image16.png)

5.  Sistemi yönetmek için **RemoteAccess** cmdlet 'lerini kullanın.

    ![](./media/azure-stack-network-howto-vpn-tunnel/image17.png)

### <a name="install-rras-on-a-on-premises-vm-db-tier"></a>Şirket içi VM DB katmanında RRAS 'yi yükler

1.  Hedef bir Windows 2016 görüntüsüdür.

2.  `Add-Site2SiteIKE.ps1` betiğini depodan kopyalayıp yerel olarak çalıştırırsanız, komut dosyası **WindowsFeature** ve **RemoteAccess**' ı kurar.

    > [!Note]
    > Ortamınıza bağlı olarak sisteminizi yeniden başlatmanız gerekebilir.

    Başvuru için şirket içi makine ağ yapılandırmasına başvurun.

    ![](./media/azure-stack-network-howto-vpn-tunnel/image18.png)

3.  AppTier şablon dağıtımından kaydedilen **Çıkış** parametrelerini ekleyerek betiği çalıştırın.

    ![](./media/azure-stack-network-howto-vpn-tunnel/image19.png)

5.  Tünel artık yapılandırıldı ve AppTier bağlantısı bekleniyor.

### <a name="configure-app-tier-to-db-tier"></a>Uygulama katmanını DB katmanına yapılandırma

1.  Azure Stack Kullanıcı portalını açın ve **kaynak oluştur**' u seçin.

2.  **Şablon dağıtımı**'nı seçin.

3.  İçeriği **azuredeploy. Tunnel. Ike. JSON**konumundan yapıştırın.

4.  **Parametreleri Düzenle**' yi seçin.

    ![](./media/azure-stack-network-howto-vpn-tunnel/image20.png)

5.  AppTier 'ın seçili olup olmadığını ve 10.99.0.1 adresinde uzak iç ağ kümesini denetleyin.

### <a name="confirm-tunnel-between-app-tier-and-db-tier"></a>Uygulama katmanı ve VERITABANı katmanı arasındaki tüneli onaylama

1.  SANAL makineye oturum açmadan tüneli denetlemek için özel bir betik uzantısı çalıştırın.

2.  RRAS VM 'sine (AppTier) gidin.

3.  **Uzantılar** ve R**Özel Betik uzantısı**' nı seçin.

4.  **Azure-Intelligent-Edge-Patterns/RRAS-VNET-vpntunnel** deposunda Scripts dizinine gidin. **Get-VPNS2SInterfaceStatus. ps1**öğesini seçin.

    ![](./media/azure-stack-network-howto-vpn-tunnel/image21.png)

5.  RDP 'yi etkinleştirir ve oturum açın, PowerShell 'i açın ve `get-vpns2sinterface`çalıştırın, tünelin bağlı olduğunu görebilirsiniz.

    **DBTier**

    ![](./media/azure-stack-network-howto-vpn-tunnel/image22.png)

    **AppTier**

    ![](./media/azure-stack-network-howto-vpn-tunnel/image23.png)

    > [!Note]  
    > RDP 'yi hem bir makineden ikincisine hem de ikinciden birincisine test edebilirsiniz.

    > [!Note]  
    > Bu çözümü şirket içinde uygulamak için, altyapıyı geçiş Azure Stack uzak ağa veya belirli VM 'lerde en düşük düzeyde dağıtmanız gerekir

### <a name="deploying-a-gre-tunnel"></a>GRE tüneli dağıtma

Bu şablon için, Bu izlenecek yol [Ike şablonunu](network-howto-vpn-tunnel-ipsec.md)kullandı. Ancak, bir [GRE tüneli](network-howto-vpn-tunnel-gre.md)de dağıtabilirsiniz. Bu tünel daha fazla verimlilik sağlar.

İşlem neredeyse aynıdır. Ancak, tünel şablonunu mevcut altyapıya dağıtırken, ilk üç giriş için diğer sistemden gelen çıktıları kullanmanız gerekir. Bağlanmaya çalıştığınız kaynak grubu yerine, dağıttığınız kaynak grubu için **LOCALTUNNELGATEWAY** bilmeniz gerekir.

![](./media/azure-stack-network-howto-vpn-tunnel/image24.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack ağ oluşturma farklılıkları ve konuları](azure-stack-network-differences.md)  
[GRE kullanarak VPN tüneli oluşturma](network-howto-vpn-tunnel-gre.md)  
[IPSEC kullanarak VPN tüneli oluşturma](network-howto-vpn-tunnel-ipsec.md)