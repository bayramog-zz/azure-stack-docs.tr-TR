---
title: Azure Stack için VPN Gateway 'i ayarlama | Microsoft Docs
description: Azure Stack için VPN Gateway 'i ayarlamayı öğrenin.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: how-to
ms.date: 10/03/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 10/03/2019
ms.openlocfilehash: 722cd99a53a0c08e7b981a571726b378e54cd288
ms.sourcegitcommit: b2d19e12a50195bb8925879ee75c186c9604f313
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71962380"
---
# <a name="setup-vpn-gateway-for-azure-stack-using-fortigate-nva"></a>FortiGate NVA kullanarak Azure Stack için VPN Gateway kurma

Bu makalede, Azure Stack bir VPN bağlantısının nasıl oluşturulacağı açıklanır. VPN ağ geçidi, Azure Stack ve uzak VPN ağ geçidinde sanal ağınız arasında şifrelenmiş trafik gönderen bir sanal ağ geçidi türüdür. Aşağıdaki yordam, bir kaynak grubu içinde bir ağ sanal gereci olan FortiGate NVA ile bir VNET dağıtır. Ayrıca, FortiGate NVA 'da IPSec VPN ayarlama adımlarını da sağlar.

## <a name="prerequisites"></a>Önkoşullar

-  Bu çözüm için gerekli işlem, ağ ve kaynak gereksinimlerini dağıtmak için kullanılabilir kapasiteye sahip Azure Stack tümleşik sistemlere erişim. 

    > [!Note]  
    > Bu yönergeler, ASDK içindeki ağ limitions nedeniyle Azure Stack Geliştirme Seti (ASDK) **ile çalışmaz.** Daha fazla bilgi için bkz. [asdk gereksinimleri ve konuları](https://docs.microsoft.com/azure-stack/asdk/asdk-deploy-considerations).

-  Azure Stack tümleşik sistemi barındıran şirket içi ağdaki bir VPN cihazına erişim. Cihazın, [dağıtım parametrelerinde](#deployment-parameters)açıklanan parametreleri karşılayan bir IPSec tüneli oluşturması gerekir.

-  Azure Stack marketi 'nde bulunan bir ağ sanal gereci (NVA) çözümü. Bir NVA, ağ trafiğinin bir çevre ağdan diğer ağlara veya alt ağlara akışını denetler. Bu yordam, [Fortinet FortiGate yeni nesil güvenlik duvarı tek VM çözümünü](https://azuremarketplace.microsoft.com/marketplace/apps/fortinet.fortinet-FortiGate-singlevm)kullanır.

    > [!Note]  
    > **Azure KLG Için Fortinet FortiGate-VM** 'niz yoksa ve Azure Stack marketi 'Nde bulunan **FORTIGATE NGFW-tek VM dağıtımı (KLG)** yoksa, bulut işletmenine başvurun.

-  FortiGate NVA 'yı etkinleştirmek için en az bir kullanılabilir FortiGate lisans dosyasına ihtiyacınız olacaktır. Bu lisansları alma hakkında bilgi için bkz. [Lisansınızı kaydetme ve indirme](https://docs2.fortinet.com/vm/azure/FortiGate/6.2/azure-cookbook/6.2.0/19071/registering-and-downloading-your-license)Için Fortinet belge kitaplığı makalesi.

    Bu yordam tek bir [FortiGate-VM dağıtımını](ttps://docs2.fortinet.com/vm/azure/FortiGate/6.2/azure-cookbook/6.2.0/632940/single-FortiGate-vm-deployment)kullanır. FortiGate NVA 'yı şirket içi ağınızda Azure Stack VNET 'e nasıl bağlayabileceğinizi gösteren adımları bulabilirsiniz.

    FortiGate çözümünü etkin-Pasif (HA) ayarlanmış olarak dağıtma hakkında daha fazla bilgi için bkz. [Azure 'Da Fortigate-VM Için](https://docs2.fortinet.com/vm/azure/FortiGate/6.2/azure-cookbook/6.2.0/983245/ha-for-FortiGate-vm-on-azure)Fortinet belge kitaplığı makalesindeki ayrıntılar.

## <a name="deployment-parameters"></a>Dağıtım parametreleri

Aşağıdaki tabloda, bu dağıtımlarda başvuru için kullanılan parametreler özetlenmektedir.

| Parametre | Value |
|-----------------------------------|---------------------------|
| FortiGate örnek adı | forti1 |
| KLG Lisansı/sürümü | 6.0.3 |
| FortiGate Yönetici Kullanıcı adı | fortiadmin |
| Kaynak grubu adı | forti1-rg1 |
| Sanal ağ adı | forti1vnet1 |
| VNET adres alanı | 172.16.0.0/16 * |
| Ortak VNET alt ağ adı | forti1-PublicFacingSubnet |
| Ortak VNET adresi ön eki | 172.16.0.0/24 * |
| İç VNET alt ağ adı | forti1-ınsidesubnet |
| İç VNET alt ağ öneki | 172.16.1.0/24 * |
| FortiGate NVA 'nın VM boyutu | Standart F2s_v2 |
| Ortak IP adresi adı | forti1-publicip1 |
| Genel IP adresi türü | Statik |

> [!Note]
> `172.16.0.0/16` , şirket içi ağ veya Azure Stack VIP havuzuyla çakışırsa, farklı bir adres alanı ve alt ağ ön ekleri seçin.

## <a name="deploy-the-fortigate-ngfw-marketplace-items"></a>FortiGate NGFW Market öğelerini dağıtma

1. Azure Stack Kullanıcı portalını açın.

    ![](./media/azure-stack-network-howto-vnet-to-onprem/image5.png)

1. **Kaynak oluştur** ' u seçin ve `FortiGate` ' i arayın.

    ![](./media/azure-stack-network-howto-vnet-to-onprem/image6.png)

2. **Fortigate NGFW** ' i seçin ve **Oluştur**' u seçin.

3. [Dağıtım parametreleri](#deployment-parameters) tablosundaki parametreleri kullanarak **temel bilgileri** doldurun.

    ![](./media/azure-stack-network-howto-vnet-to-onprem/image7.png)

1. **Tamam**’ı seçin.

2. [Dağıtım parametreleri](#deployment-parameters) tablosunu kullanarak sanal ağ, alt ağ ve VM boyutu ayrıntılarını sağlayın.

    > [!Warning] 
    > Şirket içi ağ `172.16.0.0/16` IP aralığıyla çakışırsa, farklı bir ağ aralığı ve alt ağları seçmeniz ve ayarlamanız gerekir. [Dağıtım parametreleri](#deployment-parameters) tablosundan farklı adlar ve aralıklar kullanmak istiyorsanız, şirket içi **ağla çakışmayan parametreleri** kullanın. VNET 'in içindeki VNET IP aralığını ve alt ağ aralıklarını ayarlarken dikkatli olmanız. Aralığın, şirket içi ağınızda bulunan IP aralıklarıyla örtüşmesini istemezsiniz.

3. **Tamam**’ı seçin.

4. Fortigate NVA için genel IP 'yi yapılandırın:

    ![](./media/azure-stack-network-howto-vnet-to-onprem/image8.png)

5. **Tamam**’ı seçin. Sonrasında **Tamam**’ı seçin.

6. **Oluştur**’u seçin.

    Dağıtım yaklaşık 10 dakika sürer.

## <a name="configure-routes-udr-for-the-vnet"></a>VNET için yolları (UDR) yapılandırma

1. Azure Stack Kullanıcı portalını açın.

2. Kaynak grupları ' nı seçin. Filtreye `forti1-rg1` yazın ve forti1-RG1 kaynak grubuna çift tıklayın.

    ![](./media/azure-stack-network-howto-vnet-to-onprem/image9.png)

2. ' Forti1-forti1-ınsidesubnet-Routes-xxxx ' kaynağını seçin.

3. **Ayarlar**altında **rotalar** ' ı seçin.

    ![](./media/azure-stack-network-howto-vnet-to-onprem/image10.png)

4. **-Internet** yolunu silin.

    ![](./media/azure-stack-network-howto-vnet-to-onprem/image11.png)

5. *Evet*' i seçin.

6. Yeni bir yol eklemek için **Ekle** ' yi seçin.

7. @No__t-0 yolunu adlandırın.

8. VPN 'in bağlanacağı şirket içi ağın ağ aralığını tanımlayan IP ağ aralığını girin.

9. **Sonraki atlama türü** ve `172.16.1.4` için **Sanal Gereç** seçin. Farklı bir IP aralığı kullanıyorsanız, IP aralığınızı kullanın.

    ![](./media/azure-stack-network-howto-vnet-to-onprem/image12.png)

10. **Kaydet**’i seçin.

## <a name="activate-the-fortigate-nva"></a>FortiGate NVA 'yı etkinleştirin

FortiGate NVA 'yı etkinleştirin ve her NVA üzerinde bir IPSec VPN bağlantısı ayarlayın.

Her bir FortiGate 'i etkinleştirmek için, Fortinet 'den geçerli bir lisans dosyası gerekir. Her NVA 'yı etkinleştirmediğiniz sürece **NVA 'lar çalışmaz.** Lisans dosyası alma ve NVA 'yı etkinleştirme adımları hakkında daha fazla bilgi için, bkz. [Lisansınızı kaydetme ve indirme](https://docs2.fortinet.com/vm/azure/FortiGate/6.2/azure-cookbook/6.2.0/19071/registering-and-downloading-your-license)Için Fortinet belge kitaplığı makalesi.

NVA 'lar etkinleştirildikten sonra, NVA üzerinde bir IPSec VPN tüneli oluşturun.

1. Azure Stack Kullanıcı portalını açın.

2. Kaynak grupları ' nı seçin. Filtreye `forti1` girin ve forti1 kaynak grubuna çift tıklayın.

3. Kaynak grubu dikey penceresinde kaynak türleri listesinde **forti1** sanal makinesine çift tıklayın.

    ![](./media/azure-stack-network-howto-vnet-to-onprem/image13.png)

4. Atanan IP adresini kopyalayın, bir tarayıcı açın ve IP adresini Adres çubuğuna yapıştırın. Site, güvenlik sertifikasına güvenilmediğini bildiren bir uyarı tetiklenebilir. Yine de devam edin.

5. Dağıtım sırasında verdiğiniz FortiGate yönetici kullanıcı adını ve parolasını girin.

    ![](./media/azure-stack-network-howto-vnet-to-onprem/image14.png)

6. **System** > **üretici yazılımını**seçin.

7. En son üretici yazılımını gösteren kutuyu seçin (örneğin, `FortiOS v6.2.0 build0866`).

    ![](./media/azure-stack-network-howto-vnet-to-onprem/image15.png)

8. **Yedekleme yapılandırması ve yükseltme** > **devam et**' i seçin.

9. NVA, bellenimini en son yapıya güncelleştirir ve yeniden başlatır. İşlem yaklaşık beş dakika sürer. FortiGate web konsoluna tekrar oturum açın.

10. **VPN** > **IPSec Sihirbazı**' na tıklayın.

11. VPN **oluşturma SIHIRBAZıNDA**VPN için bir ad girin (örneğin, `conn1`).

12. **Bu sıtenın NAT 'nin arkasında olduğunu**seçin.

    ![](./media/azure-stack-network-howto-vnet-to-onprem/image16.png)

13. **İleri**’yi seçin.

14. Bağlanılacak şirket içi VPN cihazının uzak IP adresini girin.

15. **Giden arabirim**olarak **PORT1** öğesini seçin.

16. **Önceden paylaşılan anahtar** ' ı seçin ve önceden paylaşılan anahtarı girin (ve kaydedin). 

    > [!Note]  
    > Şirket içi VPN cihazında bağlantıyı kurmak için bu anahtara ihtiyacınız olacak, yani *tam olarak*eşleşmesi gerekir.

    ![](./media/azure-stack-network-howto-vnet-to-onprem/image17.png)

17. **İleri**’yi seçin.

18. **Yerel arabirim**için **PORT2** öğesini seçin.

19. Yerel alt ağ aralığını girin:
    - forti1: 172.16.0.0/16
    - forti2: 172.17.0.0/16

    Farklı bir IP aralığı kullanıyorsanız, IP aralığınızı kullanın.

20. Şirket içi VPN cihazından Bağlanılacak olan şirket içi ağı temsil eden uygun uzak alt ağ (lar) ı girin.

    [](./media/azure-stack-network-howto-vnet-to-onprem/image18.png)

21. **Oluştur**’u seçin

22. **Ağ** > **arabirimleri**' ni seçin.

    ![](./media/azure-stack-network-howto-vnet-to-onprem/image19.png)

23. **PORT2**çift tıklayın.

24. **Rol** listesinde **LAN** ' ı ve adresleme modu için **DHCP** ' yi seçin.

25. **Tamam**’ı seçin.

## <a name="configure-the-on-premises-vpn"></a>Şirket içi VPN 'yi yapılandırma

Şirket içi VPN cihazı, IPSec VPN tüneli oluşturulacak şekilde yapılandırılmalıdır. Aşağıdaki tabloda, şirket içi VPN cihazını ayarlamanız için gereken parametreler sağlanmıştır. Şirket içi VPN cihazını yapılandırma hakkında daha fazla bilgi için, cihazınızın belgelerine bakın.

| Parametre | Value |
| --- | --- |
| Uzak ağ geçidi IP 'si | Forti1 öğesine atanan genel IP adresi – bkz. [FortiGate NVA 'Yı etkinleştirme](#activate-the-fortigate-nva). |
| Uzak IP ağı | 172.16.0.0/16 (sanal ağ için Bu yönergelerde IP aralığı kullanılıyorsa). |
| 'U. Yöntem = önceden paylaşılmış anahtar (PSK) | Adım 16.
| IKE Sürümü | 1\. |
| IKE modu | Main (KIMLIK koruması) |
| 1\. aşama teklif algoritmaları | AES128-SHA256, AES256-SHA256, AES128-SHA1, AES256-SHA1 |
| Diffe-Hellman grupları | 14, 5 |

## <a name="create-the-vpn-tunnel"></a>VPN tüneli oluşturma

Şirket içi VPN cihazı uygun bir şekilde yapılandırıldıktan sonra, VPN tüneli artık kurulabilirler.

FortiGate NVA 'dan:

1. Forti1 FortiGate Web konsolunda **izleyici** > **IPSec İzleyicisi**' ne gidin.

    ![](./media/azure-stack-network-howto-vnet-to-onprem/image20.png)

2. **Conn1** vurgulayın **ve @no__t 2.** aşama 2 ' nin**Tüm 2 seçicilerini**seçin.

    ![](./media/azure-stack-network-howto-vnet-to-onprem/image21.png)

## <a name="test-and-validate-connectivity"></a>Bağlantıyı test edin ve doğrulayın

Şirket içi VPN cihazı aracılığıyla VNET ağı ile şirket içi ağ arasında yönlendirme yapabilirsiniz.

Bağlantıyı doğrulamak için:

1. Azure Stack VNET 'lerde ve şirket içi ağ üzerindeki bir sistemde bir VM oluşturun. @No__t-0Hızlı başlangıç konumunda VM oluşturma yönergelerini izleyebilirsiniz: Azure Stack portalı @ no__t-0 ile bir Windows Server VM 'si oluşturun.

2. Azure Stack sanal makinesini oluştururken ve şirket içi sistemi hazırlarken şunu denetleyin:

-  Azure Stack VM, sanal **ağın ınsidesubnet** 'ine yerleştirilir.

-  Şirket içi sistem, IPSec yapılandırmasında tanımlandığı şekilde, tanımlanan IP aralığı içinde şirket içi ağa yerleştirilir. Ayrıca, şirket içi VPN cihazının yerel arabirim IP adresinin şirket içi sisteme Azure Stack VNET ağına ulaşabilme yolu olarak sağlandığından emin olun; Örneğin, `172.16.0.0/16`.

-  Oluşturma sırasında Azure Stack VM 'ye hiçbir NSG **uygulamayın.** VM 'nin portaldan oluşturulması durumunda varsayılan olarak eklenen NSG 'yi kaldırmanız gerekebilir.

-  Şirket içi sistem IŞLETIM sistemi ve Azure Stack VM IŞLETIM sisteminin, bağlantıyı test etmek için kullanacağınız iletişimi önleyen işletim sistemi güvenlik duvarı kuralları olmadığından emin olun. Sınama amacıyla, her iki sistemin işletim sistemi içinde güvenlik duvarının tamamen devre dışı bırakılması önerilir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack ağ oluşturma farklılıkları ve konuları](azure-stack-network-differences.md)  
[Fortinet FortiGate ile Azure Stack bir ağ çözümü sunun](../operator/azure-stack-network-solutions-enable.md)  