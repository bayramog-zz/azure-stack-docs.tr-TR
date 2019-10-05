---
title: VNET eşlemesi aracılığıyla iki Azure yığınını bağlama | Microsoft Docs
description: VNET eşlemesi aracılığıyla iki Azure yığınını bağlamayı öğrenin.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: how-to
ms.date: 10/03/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 10/03/2019
ms.openlocfilehash: 7a9f293c35856a9d1e29652a097d789f86b7b03c
ms.sourcegitcommit: b2d19e12a50195bb8925879ee75c186c9604f313
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71962006"
---
# <a name="how-to-connect-two-vnets-through-peering"></a>Eşleme ile iki sanal ağı bağlama

Bu makalede, aynı ortamdaki iki sanal ağ arasında bir bağlantının nasıl oluşturulacağı açıklanır. Bağlantıları ayarlarken Azure Stack ' deki VPN ağ geçitlerinin nasıl çalıştığını öğrenirsiniz. Fortinet FortiGate kullanarak aynı Azure Stack ortamında iki sanal ağı birbirine bağlama. Bu yordam, her VNET 'te her bir ağ sanal gereci olan bir FortiGate NVA ile iki VNET 'i her biri ayrı bir kaynak grubu içinde dağıtır. Ayrıca, iki sanal ağ arasında IPSec VPN ayarlamak için gereken değişiklikleri ayrıntılı olarak görebilirsiniz. Her VNET dağıtımı için bu makaledeki adımları tekrarlayın.

## <a name="prerequisites"></a>Önkoşullar

-   Bu çözüm için gerekli işlem, ağ ve kaynak gereksinimlerini dağıtmak için kullanılabilir kapasiteye sahip Azure Stack tümleşik bir sisteme veya ASDK 'e erişin.

-  Azure Stack Market 'e indirilen ve yayımlayan bir ağ sanal gereci (NVA) çözümü. Bir NVA, ağ trafiğinin bir çevre ağdan diğer ağlara veya alt ağlara akışını denetler. Bu yordam, [Fortinet FortiGate yeni nesil güvenlik duvarı tek VM çözümünü](https://azuremarketplace.microsoft.com/marketplace/apps/fortinet.fortinet-FortiGate-singlevm)kullanır.

-  FortiGate NVA 'yı etkinleştirmek için en az iki kullanılabilir FortiGate lisans dosyası. Bu lisansların nasıl alınacağı hakkında bilgi için bkz. [Lisansınızı kaydetme ve indirme](https://docs2.fortinet.com/vm/azure/FortiGate/6.2/azure-cookbook/6.2.0/19071/registering-and-downloading-your-license)Için Fortinet belge kitaplığı makalesi.

    Bu yordam tek bir [FortiGate-VM dağıtımını](ttps://docs2.fortinet.com/vm/azure/FortiGate/6.2/azure-cookbook/6.2.0/632940/single-FortiGate-vm-deployment)kullanır. FortiGate NVA 'yı şirket içi ağınızda Azure Stack VNET 'e nasıl bağlayabileceğinizi gösteren adımları bulabilirsiniz.

    FortiGate çözümünü etkin-Pasif (HA) ayarlanmış olarak dağıtma hakkında daha fazla bilgi için bkz. [Azure 'Da Fortigate-VM Için](https://docs2.fortinet.com/vm/azure/FortiGate/6.2/azure-cookbook/6.2.0/983245/ha-for-FortiGate-vm-on-azure)Fortinet belge kitaplığı makalesindeki ayrıntılar.

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
> \*, yukarıdaki Azure Stack VIP havuzu dahil olmak üzere şirket içi ağ ortamıyla herhangi bir şekilde çakışırsa, farklı bir adres alanları ve alt ağ ön ekleri kümesi seçin. Ayrıca, adres aralıklarının birbiriyle çakışmadığından emin olun.

## <a name="deploy-the-fortigate-ngfw"></a>FortiGate NGFW dağıtma

1.  Azure Stack Kullanıcı portalını açın.

    ![](./media/azure-stack-network-howto-vnet-to-onprem/image5.png)

2.  **Kaynak oluştur** ' u seçin ve `FortiGate` ' i arayın.

    ![](./media/azure-stack-network-howto-vnet-to-onprem/image6.png)

3.  **Fortigate NGFW** ' i seçin ve **Oluştur**' u seçin.

4.  [Dağıtım parametreleri](#deployment-parameters) tablosundaki parametreleri kullanarak **temel bilgileri** doldurun.

    ![](./media/azure-stack-network-howto-vnet-to-onprem/image7.png)

5.  **Tamam**’ı seçin.

6.  [Dağıtım parametreleri](#deployment-parameters) tablosunu kullanarak sanal ağ, alt ağ ve VM boyutu ayrıntılarını sağlayın.

    > [!Warning] 
    > Şirket içi ağ `172.16.0.0/16` IP aralığıyla çakışırsa, farklı bir ağ aralığı ve alt ağları seçmeniz ve ayarlamanız gerekir. [Dağıtım parametreleri](#deployment-parameters) tablosundan farklı adlar ve aralıklar kullanmak istiyorsanız, şirket içi **ağla çakışmayan parametreleri** kullanın. VNET 'in içindeki VNET IP aralığını ve alt ağ aralıklarını ayarlarken dikkatli olmanız. Aralığın, şirket içi ağınızda bulunan IP aralıklarıyla örtüşmesini istemezsiniz.

7.  **Tamam**’ı seçin.

8.  Fortigate NVA için genel IP 'yi yapılandırın:

    ![](./media/azure-stack-network-howto-vnet-to-onprem/image8.png)

9.  **Tamam**’ı seçin. Sonrasında **Tamam**’ı seçin.

10.  **Oluştur**’u seçin.

Dağıtım yaklaşık 10 dakika sürer.

## <a name="configure-routes-udrs-for-each-vnet"></a>Her VNET için yolları (UDRs) yapılandırma

Her iki dağıtım için de, forti1-RG1 ve forti2-RG1 için bu adımları gerçekleştirin.

1. Azure Stack Kullanıcı portalını açın.

2. Kaynak grupları ' nı seçin. Filtreye `forti1-rg1` yazın ve forti1-RG1 kaynak grubuna çift tıklayın.

    ![kaynak grubu](./media/azure-stack-network-howto-vnet-to-onprem/image9.png)

2. **Forti1-forti1-ınsidesubnet-Routes-xxxx** kaynağını seçin.

3. **Ayarlar**altında **rotalar** ' ı seçin.

    ![Yol](./media/azure-stack-network-howto-vnet-to-onprem/image10.png)

4. **-Internet** yolunu silin.

    ![-Internet](./media/azure-stack-network-howto-vnet-to-onprem/image11.png)

5. *Evet*' i seçin.

6. Yeni bir yol eklemek için **Ekle** ' yi seçin.

7. @No__t-0 yolunu adlandırın.

8. VPN 'in bağlanacağı şirket içi ağın ağ aralığını tanımlayan IP ağ aralığını girin.

9. **Sonraki atlama türü** ve `172.16.1.4` için **Sanal Gereç** seçin. Farklı bir IP aralığı kullanıyorsanız, IP aralığınızı kullanın.

    ![Sonraki atlama türü](./media/azure-stack-network-howto-vnet-to-onprem/image12.png)

10. **Kaydet**’i seçin.

Her bir FortiGate NVA 'yı etkinleştirmek için Fortinet 'ten geçerli bir lisans dosyası gerekir. Her NVA 'yı etkinleştirmediğiniz sürece **NVA 'lar çalışmaz.** Lisans dosyası alma ve NVA 'yı etkinleştirme adımları hakkında daha fazla bilgi için, bkz. [Lisansınızı kaydetme ve indirme](https://docs2.fortinet.com/vm/azure/FortiGate/6.2/azure-cookbook/6.2.0/19071/registering-and-downloading-your-license)Için Fortinet belge kitaplığı makalesi.

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

7.  **Yedekleme yapılandırması ve yükseltme** > **devam et**' i seçin.

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

Yukarıdaki *her ikisi için de* NVA 'lar:

1.  Forti2 FortiGate Web konsolunda,  > **IPSec Izleyicisini** **izlemeyi**seçin. 

    ![](./media/azure-stack-network-howto-vnet-to-vnet/image20.png)

2.  @No__t-0 ' ı vurgulayın ve **getir**@no__t 2.**aşama 2**' yi seçin.

    ![](./media/azure-stack-network-howto-vnet-to-vnet/image21.png)

## <a name="test-and-validate-connectivity"></a>Bağlantıyı test edin ve doğrulayın

Artık FortiGate NVA 'lar aracılığıyla her VNET arasında yol açabiliyor olmanız gerekir. Bağlantıyı doğrulamak için, her VNET 'in ınsidesubnet 'inde bir Azure Stack sanal makinesi oluşturun. Azure Stack VM oluşturma, Portal, CLı veya PowerShell aracılığıyla yapılabilir. VM 'Leri oluştururken:

-   Azure Stack VM 'Ler her VNET 'in **ınsidesubnet** 'ine yerleştirilir.

-   Oluşturma işleminden sonra sanal makineye hiçbir NSG **uygulamaz (** diğer bir DEYIŞLE, VM 'yi portaldan oluşturursanız, varsayılan olarak eklenen NSG 'yi kaldırın.

-   VM Güvenlik Duvarı kurallarının bağlantıyı test etmek için kullanacağınız iletişime izin verildiğinden emin olun. Sınama amacıyla, tüm mümkünse güvenlik duvarını işletim sistemi içinde tamamen devre dışı bırakmanız önerilir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack ağ oluşturma farklılıkları ve konuları](azure-stack-network-differences.md)  
[Fortinet FortiGate ile Azure Stack bir ağ çözümü sunun](../operator/azure-stack-network-solutions-enable.md)  