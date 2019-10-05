---
title: Fortinet FortiGate NVA ile Azure Stack içinde VNET 'e VNet bağlantısı kurma | Microsoft Docs
description: Fortinet FortiGate NVA ile Azure Stack sanal ağa VNet bağlantısı kurmayı öğrenin
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: how-to
ms.date: 10/03/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 10/03/2019
ms.openlocfilehash: b68b0119e30d39b126aa43c2c8ed4c859073663e
ms.sourcegitcommit: b2d19e12a50195bb8925879ee75c186c9604f313
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71962193"
---
# <a name="how-to-establish-a-vnet-to-vnet-connection-in-azure-stack-with-fortinet-fortigate-nva"></a>Fortinet FortiGate NVA ile Azure Stack 'da VNET 'e VNet bağlantısı kurma

Bu makalede, bir ağ sanal gereci olan Fortinet FortiGate NVA kullanarak bir Azure Stack sanal ağı başka bir Azure Stack VNET 'e bağlayacaksınız.

Bu makalede, kiracıların iki ortamda yalnızca bir VPN bağlantısı kurma olanağı sağlayan geçerli Azure Stack sınırlaması ele alınmaktadır. Kullanıcılar, bir Linux sanal makinesinde farklı Azure Stack birden fazla VPN bağlantısına izin veren bir özel ağ geçidi ayarlamayı öğrenirsiniz. Bu makaledeki yordam her VNET 'te bir FortiGate NVA ile iki sanal ağ dağıtır: her bir Azure Stack ortamı için bir dağıtım. Ayrıca, iki sanal ağ arasında IPSec VPN ayarlamak için gereken değişiklikleri ayrıntılı olarak görebilirsiniz. Bu makaledeki adımların her bir Azure Stack her VNET için tekrarlanması gerekir. 

## <a name="prerequisites"></a>Önkoşullar

-  Bu çözüm için gerekli işlem, ağ ve kaynak gereksinimlerini dağıtmak için kullanılabilir kapasiteye sahip Azure Stack tümleşik sistemlere erişim. 

    > [!Note]  
    > Bu yönergeler, ASDK 'deki ağ sınırlamaları nedeniyle Azure Stack Geliştirme Seti (ASDK) ile **birlikte çalışmayacaktır.** Daha fazla bilgi için bkz. [asdk gereksinimleri ve konuları](https://docs.microsoft.com/azure-stack/asdk/asdk-deploy-considerations).

-  Azure Stack Market 'e indirilen ve yayımlayan bir ağ sanal gereci (NVA) çözümü. Bir NVA, ağ trafiğinin bir çevre ağdan diğer ağlara veya alt ağlara akışını denetler. Bu yordam, [Fortinet FortiGate yeni nesil güvenlik duvarı tek VM çözümünü](https://azuremarketplace.microsoft.com/marketplace/apps/fortinet.fortinet-FortiGate-singlevm)kullanır.

-  FortiGate NVA 'yı etkinleştirmek için en az iki kullanılabilir FortiGate lisans dosyası. Bu lisansların nasıl alınacağı hakkında bilgi için bkz. [Lisansınızı kaydetme ve indirme](https://docs2.fortinet.com/vm/azure/FortiGate/6.2/azure-cookbook/6.2.0/19071/registering-and-downloading-your-license)Için Fortinet belge kitaplığı makalesi.

    Bu yordam tek bir [FortiGate-VM dağıtımını](ttps://docs2.fortinet.com/vm/azure/FortiGate/6.2/azure-cookbook/6.2.0/632940/single-FortiGate-vm-deployment)kullanır. FortiGate NVA 'yı şirket içi ağınızda Azure Stack VNET 'e nasıl bağlayabileceğinizi gösteren adımları bulabilirsiniz.

    FortiGate çözümünü etkin-Pasif (HA) ayarlanmış olarak dağıtma hakkında daha fazla bilgi için bkz. [Azure 'Da Fortigate-VM Için](https://docs2.fortinet.com/vm/azure/FortiGate/6.2/azure-cookbook/6.2.0/983245/ha-for-FortiGate-vm-on-azure)Fortinet belge kitaplığı makalesi ha.

## <a name="deployment-parameters"></a>Dağıtım parametreleri

Aşağıdaki tabloda, bu dağıtımlarda başvuru için kullanılan parametreler özetlenmektedir:

### <a name="deployment-one-forti1"></a>Dağıtım bir: Forti1

| FortiGate örnek adı | Forti1 |
|-----------------------------------|---------------------------|
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

### <a name="deployment-two-forti2"></a>Dağıtım iki: Forti2

| FortiGate örnek adı | Forti2 |
|-----------------------------------|---------------------------|
| KLG Lisansı/sürümü | 6.0.3 |
| FortiGate Yönetici Kullanıcı adı | fortiadmin |
| Kaynak grubu adı | forti2-rg1 |
| Sanal ağ adı | forti2vnet1 |
| VNET adres alanı | 172.17.0.0/16 * |
| Ortak VNET alt ağ adı | forti2-PublicFacingSubnet |
| Ortak VNET adresi ön eki | 172.17.0.0/24 * |
| İç VNET alt ağ adı | Forti2-ınsidesubnet |
| İç VNET alt ağ öneki | 172.17.1.0/24 * |
| FortiGate NVA 'nın VM boyutu | Standart F2s_v2 |
| Ortak IP adresi adı | Forti2-publicip1 |
| Genel IP adresi türü | Statik |

> [!Note]
> \*, yukarıdaki Azure Stack VIP havuzu dahil olmak üzere şirket içi ağ ortamıyla herhangi bir şekilde çakışırsa, farklı bir adres alanları ve alt ağ ön ekleri kümesi seçin. Ayrıca, adres aralıklarının birbiriyle çakışmadığından emin olun. * *

## <a name="deploy-the-fortigate-ngfw-marketplace-items"></a>FortiGate NGFW Market öğelerini dağıtma

Azure Stack ortamları için bu adımları yineleyin. 

1. Azure Stack Kullanıcı portalını açın. Bir aboneliğe en az katkıda bulunan hakları olan kimlik bilgilerini kullandığınızdan emin olun.

    ![](./media/azure-stack-network-howto-vnet-to-vnet-stacks/image5.png)

1. **Kaynak oluştur** ' u seçin ve `FortiGate` ' i arayın.

    ![](./media/azure-stack-network-howto-vnet-to-vnet-stacks/image6.png)

2. **Fortigate NGFW** ' ı seçin ve **Oluştur**' u seçin.

3. [Dağıtım parametreleri](#deployment-parameters) tablosundaki parametreleri kullanarak **temel bilgileri** doldurun.

    Formunuz aşağıdaki bilgileri içermelidir:

    ![](./media/azure-stack-network-howto-vnet-to-vnet-stacks/image7.png)

4. **Tamam**’ı seçin.

5. [Dağıtım parametrelerinden](#deployment-parameters)sanal ağ, alt ağ ve VM boyutu ayrıntılarını sağlayın.

    Farklı adlar ve aralıklar kullanmak istiyorsanız, diğer Azure Stack ortamındaki diğer VNET ve FortiGate kaynaklarıyla çakışacak parametreleri kullanmayın. Bu, özellikle VNET 'in içindeki VNET IP aralığı ve alt ağ aralıklarını ayarlarken doğrudur. Oluşturduğunuz diğer VNET için IP aralıklarıyla çakışmadığından emin olun.

6. **Tamam**’ı seçin.

7. FortiGate NVA için kullanılacak genel IP 'yi yapılandırın:

    ![](./media/azure-stack-network-howto-vnet-to-vnet-stacks/image8.png)

8. **Tamam** ' ı ve ardından **Tamam**' ı seçin.

9. **Oluştur**’u seçin.

Dağıtım yaklaşık 10 dakika sürer. Artık diğer Azure Stack ortamında diğer FortiGate NVA ve VNET dağıtımını oluşturmak için adımları yineleyebilirsiniz.

## <a name="configure-routes-udrs-for-each-vnet"></a>Her VNET için yolları (UDRs) yapılandırma

Her iki dağıtım için de, forti1-RG1 ve forti2-RG1 için bu adımları gerçekleştirin.

1. Azure Stack portalındaki forti1-RG1 kaynak grubuna gidin.

    ![](./media/azure-stack-network-howto-vnet-to-vnet-stacks/image9.png)

2. ' Forti1-forti1-ınsidesubnet-Routes-xxxx ' kaynağında öğesini seçin.

3. **Ayarlar**altında **rotalar** ' ı seçin.

    ![](./media/azure-stack-network-howto-vnet-to-vnet-stacks/image10.png)

4. **-Internet** yolunu silin.

    ![](./media/azure-stack-network-howto-vnet-to-vnet-stacks/image11.png)

5. **Evet**' i seçin.

6. **Add (Ekle)** seçeneğini belirleyin.

7. @No__t-1 veya `to-forti2` **yolunu** adlandırın. Farklı bir IP aralığı kullanıyorsanız, IP aralığınızı kullanın.

8. Girmesini
    - forti1: `172.17.0.0/16`  
    - forti2: `172.16.0.0/16`  

    Farklı bir IP aralığı kullanıyorsanız, IP aralığınızı kullanın.

9. **Sonraki atlama türü**için **Sanal Gereç** seçin.
    - forti1: `172.16.1.4`  
    - forti2: `172.17.0.4`  

    Farklı bir IP aralığı kullanıyorsanız, IP aralığınızı kullanın.

    ![](./media/azure-stack-network-howto-vnet-to-vnet-stacks/image12.png)

10. **Kaydet**’i seçin.

Her kaynak grubu için her bir **ınsidesubnet** yolu için adımları tekrarlayın.

## <a name="activate-the-fortigate-nvas-and-configure-an-ipsec-vpn-connection-on-each-nva"></a>FortiGate NVA 'lar 'ı etkinleştirin ve her NVA üzerinde bir IPSec VPN bağlantısı yapılandırın

 Her bir FortiGate NVA 'yı etkinleştirmek için, Fortinet 'ten geçerli bir lisans dosyası gerekir. Her NVA 'yı etkinleştirmediğiniz sürece **NVA 'lar çalışmaz.** Lisans dosyası alma ve NVA 'yı etkinleştirme adımları hakkında daha fazla bilgi için, bkz. [Lisansınızı kaydetme ve indirme](https://docs2.fortinet.com/vm/azure/FortiGate/6.2/azure-cookbook/6.2.0/19071/registering-and-downloading-your-license)Için Fortinet belge kitaplığı makalesi.

İki lisans dosyasının her NVA için bir tane alınması gerekir.

## <a name="create-an-ipsec-vpn-between-the-two-nvas"></a>İki NVA 'lar arasında bir IPSec VPN oluşturma

NVA 'lar etkinleştirildikten sonra, iki NVA 'lar arasında bir IPSec VPN oluşturmak için bu adımları izleyin.

Hem forti1 NVA hem de forti2 NVA için aşağıdaki adımları takip edin:

1.  FortiX VM 'ye Genel Bakış sayfasına giderek atanan genel IP adresini alın:

    ![](./media/azure-stack-network-howto-vnet-to-vnet/image13.png)

2.  Atanan IP adresini kopyalayın, bir tarayıcı açın ve adresi adres çubuğuna yapıştırın. Tarayıcınız güvenlik sertifikasının güvenilir olmadığı konusunda sizi uyarabilir. Yine de devam edin.

4.  Dağıtım sırasında verdiğiniz FortiGate yönetici kullanıcı adını ve parolasını girin.

    ![](./media/azure-stack-network-howto-vnet-to-vnet/image14.png)

5.  **System** > **üretici yazılımını**seçin.

6.  En son üretici yazılımını gösteren kutuyu seçin (örneğin, `FortiOS v6.2.0 build0866`).

    ![](./media/azure-stack-network-howto-vnet-to-vnet/image15.png)

7.  **Yedekleme yapılandırması ' nı ve istendiğinde Yükselt** ' i seçin ve devam edin.

8.  NVA, bellenimini en son yapıya güncelleştirir ve yeniden başlatır. İşlem yaklaşık beş dakika sürer. FortiGate web konsoluna tekrar oturum açın.

10.  **VPN** > **IPSec Sihirbazı**' na tıklayın.

11. VPN **oluşturma SIHIRBAZıNDA**VPN için bir ad girin (örneğin, `conn1`).

12. **Bu sıtenın NAT 'nin arkasında olduğunu**seçin.

    ![](./media/azure-stack-network-howto-vnet-to-vnet/image16.png)

13. **İleri**’yi seçin.

14. Bağlanılacak şirket içi VPN cihazının uzak IP adresini girin.

15. **Giden arabirim**olarak **PORT1** öğesini seçin.

16. **Önceden paylaşılan anahtar** ' ı seçin ve önceden paylaşılan anahtarı girin (ve kaydedin). 

    > [!Note]  
    > Şirket içi VPN cihazında bağlantıyı kurmak için bu anahtara ihtiyacınız olacak, yani *tam olarak*eşleşmesi gerekir.

    ![](./media/azure-stack-network-howto-vnet-to-vnet/image17.png)

17. **İleri**’yi seçin.

18. **Yerel arabirim**için **PORT2** öğesini seçin.

19. Yerel alt ağ aralığını girin:
    - forti1: 172.16.0.0/16
    - forti2: 172.17.0.0/16

    Farklı bir IP aralığı kullanıyorsanız, IP aralığınızı kullanın.

20. Şirket içi VPN cihazından Bağlanılacak olan şirket içi ağı temsil eden uygun uzak alt ağ (lar) ı girin.
    - forti1: 172.16.0.0/16
    - forti2: 172.17.0.0/16

    Farklı bir IP aralığı kullanıyorsanız, IP aralığınızı kullanın.

    ![](./media/azure-stack-network-howto-vnet-to-vnet/image18.png)

21. **Oluştur**’u seçin

22. **Ağ** > **arabirimleri**' ni seçin.

    ![](./media/azure-stack-network-howto-vnet-to-vnet/image19.png)

23. **PORT2**çift tıklayın.

24. **Rol** listesinde **LAN** ' ı ve adresleme modu için **DHCP** ' yi seçin.

25. **Tamam**’ı seçin.

Diğer NVA için adımları tekrarlayın.


## <a name="bring-up-all-phase-2-selectors"></a>Tüm aşama 2 seçicileri getir 

Yukarıdaki **her ikisi için de** NVA 'lar:

1.  Forti2 FortiGate Web konsolunda,  > **IPSec Izleyicisini** **izlemeyi**seçin. 

    ![](./media/azure-stack-network-howto-vnet-to-vnet/image20.png)

2.  @No__t-0 ' ı vurgulayın ve **getir**@no__t 2.**aşama 2**' yi seçin.

    ![](./media/azure-stack-network-howto-vnet-to-vnet/image21.png)


## <a name="test-and-validate-connectivity"></a>Bağlantıyı test edin ve doğrulayın

Artık FortiGate NVA 'lar aracılığıyla her VNET arasında yol açabiliyor olmanız gerekir. Bağlantıyı doğrulamak için, her VNET 'in ınsidesubnet 'inde bir Azure Stack sanal makinesi oluşturun. Azure Stack VM oluşturma, Portal, CLı veya PowerShell aracılığıyla yapılabilir. VM 'Leri oluştururken:

-   Azure Stack VM 'Ler her VNET 'in **ınsidesubnet** 'ine yerleştirilir.

-   Oluşturma işleminden sonra VM 'ye herhangi bir NSG **uygulamaz (** diğer bir deyişle, portaldan VM oluşturulurken varsayılan olarak eklenen NSG 'yi kaldırın.

-   VM Güvenlik Duvarı kurallarının, bağlantıyı test etmek için kullanacağınız iletişime izin verildiğinden emin olun. Sınama amacıyla, tüm mümkünse güvenlik duvarını işletim sistemi içinde tamamen devre dışı bırakmanız önerilir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack ağ oluşturma farklılıkları ve konuları](azure-stack-network-differences.md)  
[Fortinet FortiGate ile Azure Stack bir ağ çözümü sunun](../operator/azure-stack-network-solutions-enable.md)  