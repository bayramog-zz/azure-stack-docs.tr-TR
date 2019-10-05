---
title: VPN kullanarak Azure Stack Azure 'a bağlanma | Microsoft Docs
description: VPN kullanarak Azure Stack sanal ağları Azure 'daki sanal ağlara bağlama.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/04/2019
ms.author: sethm
ms.reviewer: scottnap
ms.lastreviewed: 10/24/2018
ms.openlocfilehash: 844162e4f31a6f543a9fe774aa40bd606dad85b9
ms.sourcegitcommit: f91979c1613ea1aa0e223c818fc208d902b81299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2019
ms.locfileid: "71974102"
---
# <a name="connect-azure-stack-to-azure-using-vpn"></a>VPN kullanarak Azure Stack Azure 'a bağlanma

*Için geçerli: Azure Stack tümleşik sistemler @ no__t-0

Bu makalede, Azure 'daki bir sanal ağa Azure Stack bir sanal ağı bağlamak için siteden siteye VPN oluşturma açıklanır.

## <a name="before-you-begin"></a>Başlamadan önce

Bağlantı yapılandırmasını gerçekleştirmek için başlamadan önce aşağıdaki öğelere sahip olduğunuzdan emin olun:

* Doğrudan internet 'e bağlı Azure Stack tümleşik sistemler (çok düğümlü) dağıtım. Dış genel IP adresi aralığınızdan doğrudan genel İnternet 'ten erişilebilir olması gerekir.
* Geçerli bir Azure aboneliği. Azure aboneliğiniz yoksa [burada ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/?b=17.06)oluşturabilirsiniz.

### <a name="vpn-connection-diagram"></a>VPN bağlantısı diyagramı

Aşağıdaki şekilde, bağlantı yapılandırmasının işiniz bittiğinde nasıl görünmesi gerektiği gösterilmektedir:

![Siteden siteye VPN bağlantısı yapılandırması](media/azure-stack-connect-vpn/image2.png)

### <a name="network-configuration-example-values"></a>Ağ yapılandırması örnek değerleri

Ağ yapılandırması örnekler tablosu, bu makaledeki örnekler için kullanılan değerleri gösterir. Bu değerleri kullanabilirsiniz veya bu makaledeki örnekleri daha iyi anlamak için bunlara başvurabilirsiniz:

|   |Azure Stack|Azure|
|---------|---------|---------|
|Sanal ağ adı     |AZS-VNet|AzureVNet |
|Sanal ağ adres alanı |10.1.0.0/16|10.100.0.0/16|
|Alt ağ adı     |Ön Uç|Ön Uç|
|Alt ağ adres aralığı|10.1.0.0/24 |10.100.0.0/24 |
|Ağ geçidi alt ağı     |10.1.1.0/24|10.100.1.0/24|

## <a name="create-the-network-resources-in-azure"></a>Azure 'da ağ kaynaklarını oluşturma

İlk olarak, Azure için ağ kaynaklarını oluşturun. Aşağıdaki yönergelerde [Azure Portal](https://portal.azure.com/)kullanarak kaynakların nasıl oluşturulacağı gösterilmektedir.

### <a name="create-the-virtual-network-and-virtual-machine-vm-subnet"></a>Sanal ağ ve sanal makine (VM) alt ağını oluşturma

1. Azure hesabınızı kullanarak [Azure Portal](https://portal.azure.com/) oturum açın.

2. Kullanıcı portalında **+ kaynak oluştur**' u seçin.
3. **Market**' e gidin ve ardından **ağ**' ı seçin.
4. **Sanal ağ**'ı seçin.
5. Azure **adı**, **Adres alanı**, **alt ağ adı**ve **alt ağ adres aralığı**değerlerini belirlemek için ağ yapılandırması tablosundaki bilgileri kullanın.
6. **Kaynak grubu**için yeni bir kaynak grubu oluşturun veya zaten bir tane varsa, **var olanı kullan**' ı seçin.
7. VNet 'iniz **konumunu** seçin.  Örnek değerleri kullanıyorsanız **Doğu ABD** seçin veya başka bir konum kullanın.
8. **Panoya sabitle**’yi seçin.
9. **Oluştur**’u seçin.

### <a name="create-the-gateway-subnet"></a>Ağ geçidi alt ağını oluşturma

1. Panodan oluşturduğunuz sanal ağ kaynağını (**AzureVNet**) açın.
2. **Ayarlar** bölümünde **alt ağlar**' ı seçin.
3. Sanal ağa bir ağ geçidi alt ağı eklemek için **ağ geçidi alt ağı** ' nı seçin.
4. Alt ağın adı varsayılan olarak **GatewaySubnet** şeklinde ayarlanır.

   >[!IMPORTANT]
   >Ağ geçidi alt ağları özeldir ve düzgün şekilde çalışabilmesi için bu ada sahip olmalıdır.

5. **Adres aralığı** alanında, adresin **10.100.1.0/24**olduğunu doğrulayın.
6. Ağ geçidi alt ağını oluşturmak için **Tamam ' ı** seçin.

### <a name="create-the-virtual-network-gateway"></a>Sanal ağ geçidini oluşturma

1. Azure portal **+ kaynak oluştur**' u seçin.

2. **Market**' e gidin ve ardından **ağ**' ı seçin.
3. Ağ kaynakları listesinden **sanal ağ geçidi**' ni seçin.
4. **Ad** alanına **Azure-GW**yazın.
5. Bir sanal ağ seçmek için **sanal ağ**' ı seçin. Sonra listeden **AzureVnet** öğesini seçin.
6. **Genel IP adresi**'ni seçin. **Ortak IP adresi Seç** bölümü açıldığında **Yeni oluştur**' u seçin.
7. **Ad** alanına **Azure-GW-PI**yazın ve ardından **Tamam**' ı seçin.
8. **Abonelik** ve **Konum** seçeneklerinin doğruluğunu onaylayın. Kaynağı panoya sabitleyebilir. **Oluştur**’u seçin.

### <a name="create-the-local-network-gateway-resource"></a>Yerel ağ geçidi kaynağı oluşturma

1. Azure portal **+ kaynak oluştur**' u seçin.

2. **Market**' e gidin ve ardından **ağ**' ı seçin.
3. Kaynak listesinden **yerel ağ geçidi**' ni seçin.
4. **Ad** alanına **AZS-GW**yazın.
5. **IP adresi** alanına, daha önce ağ yapılandırma tablosunda listelenen Azure Stack sanal ağ geçidinizin genel IP adresini yazın.
6. **Adres alanı** alanına, Azure Stack, **AzureVNet**için **10.1.0.0/24** ve **10.1.1.0/24** adres alanını yazın.
7. **Aboneliğinizin**, **kaynak grubunuzun**ve **konumun** doğru olduğundan emin olun ve ardından **Oluştur**' u seçin.

## <a name="create-the-connection"></a>Bağlantı oluşturma

1. Kullanıcı portalında **+ kaynak oluştur**' u seçin.
2. **Market**' e gidin ve ardından **ağ**' ı seçin.
3. Kaynak listesinden **bağlantı**' yı seçin.
4. **Temel** ayarlar bölümünde, **bağlantı türü**Için **siteden siteye (IPSec)** öğesini seçin.
5. **Abonelik**, **kaynak grubu**ve **konum**' u seçip **Tamam**' ı seçin.
6. **Ayarlar** bölümünde, **sanal ağ geçidi**' ni seçin ve ardından **Azure-GW**' yi seçin.
7. **Yerel ağ geçidi**' ni seçin ve ardından **AZS-GW**' ı seçin.
8. **Bağlantı adı**alanına **Azure-AZS**yazın.
9. **Paylaşılan anahtar (PSK)** alanına **12345**yazın ve ardından **Tamam**' ı seçin.

   >[!NOTE]
   >Paylaşılan anahtar için farklı bir değer kullanırsanız, bağlantının diğer ucunda oluşturduğunuz paylaşılan anahtarın değeriyle eşleşmesi gerektiğini unutmayın.

10. **Özet** bölümünü gözden geçirin ve ardından **Tamam**' ı seçin.

## <a name="create-a-vm"></a>VM oluşturma

Şimdi Azure 'da bir VM oluşturun ve sanal ağınızdaki VM alt ağına yerleştirin.

1. Azure portal **+ kaynak oluştur**' u seçin.
2. **Market**' e gidin ve ardından **işlem**' ı seçin.
3. VM görüntüleri listesinde, **Windows Server 2016 Datacenter eval** görüntüsünü seçin.
4. **Temel bilgiler** bölümünde, **ad**için **AzureVM**yazın.
5. Geçerli bir Kullanıcı adı ve parola yazın. Bu hesabı, oluşturulduktan sonra VM 'de oturum açmak için kullanırsınız.
6. Bir **abonelik**, **kaynak grubu**ve **konum**sağlayın ve **Tamam**' ı seçin.
7. **Boyut** bölümünde, bu örnek IÇIN bir VM boyutu seçin ve ardından **Seç**' i seçin.
8. **Ayarlar** bölümünde varsayılan ayarları kullanabilirsiniz. **Tamam 'ı**seçmeden önce şunları onaylayın:

   * **AzureVnet** sanal ağı seçilidir.
   * Alt ağ **10.100.0.0/24**olarak ayarlanır.

   **Tamam**’ı seçin.

9. **Özet** bölümündeki ayarları gözden geçirin ve ardından **Tamam**' ı seçin.

## <a name="create-the-network-resources-in-azure-stack"></a>Azure Stack ağ kaynaklarını oluşturma

Sonra, Azure Stack ağ kaynaklarını oluşturun.

### <a name="sign-in-as-a-user"></a>Kullanıcı olarak oturum açın

Bir hizmet yöneticisi, kullanıcılarının kullanabileceği planları, teklifleri ve abonelikleri test etmek için Kullanıcı olarak oturum açabilir. Henüz bir hesabınız yoksa, oturum açmadan önce [bir kullanıcı hesabı oluşturun](../operator/azure-stack-add-new-user-aad.md) .

### <a name="create-the-virtual-network-and-a-vm-subnet"></a>Sanal ağ ve VM alt ağı oluşturma

1. Kullanıcı portalında oturum açmak için bir kullanıcı hesabı kullanın.
2. Kullanıcı portalında **+ kaynak oluştur**' u seçin.

    ![Yeni sanal ağ oluştur](media/azure-stack-connect-vpn/image3.png)

3. **Market**' e gidin ve ardından **ağ**' ı seçin.
4. **Sanal ağ**'ı seçin.
5. **Ad**, **Adres alanı**, **alt ağ adı**ve **alt ağ adres aralığı**için, ağ yapılandırması tablosundaki değerleri kullanın.
6. **Abonelikte**, daha önce oluşturduğunuz abonelik görüntülenir.
7. **Kaynak grubu**için, bir kaynak grubu oluşturabilir veya zaten bir tane varsa **var olanı kullan**' ı seçin.
8. Varsayılan konumu doğrulayın.
9. **Panoya sabitle**’yi seçin.
10. **Oluştur**’u seçin.

### <a name="create-the-gateway-subnet"></a>Ağ geçidi alt ağını oluşturma

1. Panoda, oluşturduğunuz AZS-VNet sanal ağ kaynağını açın.
2. **Ayarlar** bölümünde **alt ağlar**' ı seçin.
3. Sanal ağa bir ağ geçidi alt ağı eklemek için **ağ geçidi alt ağı**' nı seçin.

    ![Ağ geçidi alt ağı ekle](media/azure-stack-connect-vpn/image4.png)

4. Varsayılan olarak, alt ağ adı **Gatewaysubnet**olarak ayarlanır. Ağ geçidi alt ağlarının düzgün çalışması için **Gatewaysubnet** adını kullanmaları gerekir.
5. **Adres aralığı**' nda, adresin **10.1.1.0/24**olduğunu doğrulayın.
6. Ağ geçidi alt ağını oluşturmak için **Tamam ' ı** seçin.

### <a name="create-the-virtual-network-gateway"></a>Sanal ağ geçidini oluşturma

1. Azure Stack portalında **+ kaynak oluştur**' u seçin.
2. **Market**' e gidin ve ardından **ağ**' ı seçin.
3. Ağ kaynakları listesinden **sanal ağ geçidi**' ni seçin.
4. **Ad**alanına **AZS-GW**yazın.
5. Bir sanal ağ seçmek için **sanal ağ** öğesini seçin. Listeden **AZS-VNET** ' i seçin.
6. **Genel IP adresi** menü öğesini seçin. **Ortak IP adresi Seç** bölümü açıldığında **Yeni oluştur**' u seçin.
7. **Ad**alanına **AZS-GW-PI**yazın ve ardından **Tamam**' ı seçin.
8. Varsayılan olarak, **VPN türü**için **Rota tabanlı** seçilidir. **Rota tabanlı** VPN türünü saklayın.

9. **Abonelik** ve **Konum** seçeneklerinin doğruluğunu onaylayın. Kaynağı panoya sabitleyebilir. **Oluştur**’u seçin.

### <a name="create-the-local-network-gateway"></a>Yerel ağ geçidini oluşturma

Azure Stack bir *yerel ağ geçidi* kavramı, Azure dağıtımından farklı bir bit değildir.

Bir Azure dağıtımında, yerel ağ geçidi, Azure 'da bir sanal ağ geçidine bağlandığınız şirket içi (Kullanıcı konumu) fiziksel cihazı temsil eder. Ancak Azure Stack, bağlantının her iki ucunun de sanal ağ geçitleri vardır.

Daha genel bir açıklama, yerel ağ geçidi kaynağının bağlantının diğer ucundaki uzak ağ geçidini her zaman gösterdiği anlamına gelir.

### <a name="create-the-local-network-gateway-resource"></a>Yerel ağ geçidi kaynağı oluşturma

1. Azure Stack portalında oturum açın.
2. Kullanıcı portalında **+ kaynak oluştur**' u seçin.
3. **Market**' e gidin ve ardından **ağ**' ı seçin.
4. Kaynak listesinden **yerel ağ geçidi**' ni seçin.
5. **Ad** alanına **Azure-GW**yazın.
6. **IP adresi** alanında, Azure **Azure-GW-PI**' de sanal ağ geçidi için genel IP adresini yazın. Bu adres, daha önce ağ yapılandırma tablosunda görünür.
7. **Adres alanı** alanında, oluşturduğunuz Azure VNET 'in adres alanı için **10.100.0.0/24** ve **10.100.1.0/24**yazın.

8. **Aboneliğinizin**, **kaynak grubunuzun**ve **konum** değerlerinizin doğru olduğundan emin olun ve ardından **Oluştur**' u seçin.

### <a name="create-the-connection"></a>Bağlantı oluşturma

1. Kullanıcı portalında **+ kaynak oluştur**' u seçin.
2. **Market**' e gidin ve ardından **ağ**' ı seçin.
3. Kaynak listesinden **bağlantı**' yı seçin.
4. **Temel bilgiler** ayarları bölümünde, **bağlantı türü**Için **siteden siteye (IPSec)** seçeneğini belirleyin.
5. **Abonelik**, **kaynak grubu**ve **konum**' u seçip **Tamam**' ı seçin.
6. **Ayarlar** bölümünde, **sanal ağ geçidi**' ni seçin ve ardından **AZS-GW**' ı seçin.
7. **Yerel ağ geçidi**' ni seçin ve ardından **Azure-GW**' yi seçin.
8. **Bağlantı adı**alanına **AZS-Azure**yazın.
9. **Paylaşılan anahtar (PSK)** alanına **12345**yazın ve ardından **Tamam**' ı seçin.

10. **Özet** bölümünde **Tamam**' ı seçin.

### <a name="create-a-vm"></a>VM oluşturma

VPN bağlantısını denetlemek için Azure 'da bir tane olmak üzere iki VM oluşturun: bir diğeri Azure Stack. Bu VM 'Leri oluşturduktan sonra, VPN tüneli üzerinden veri göndermek ve almak için bunları kullanabilirsiniz.

1. Azure portal **+ kaynak oluştur**' u seçin.
2. **Market**' e gidin ve ardından **işlem**' ı seçin.
3. VM görüntüleri listesinde, **Windows Server 2016 Datacenter eval** görüntüsünü seçin.
4. **Temel bilgiler** bölümünde, **ad**bölümünde **AZS-VM**yazın.
5. Geçerli bir Kullanıcı adı ve parola yazın. Bu hesabı, oluşturulduktan sonra VM 'de oturum açmak için kullanırsınız.
6. Bir **abonelik**, **kaynak grubu**ve **konum**sağlayın ve **Tamam**' ı seçin.
7. **Boyut** bölümünde, bu örnek IÇIN bir VM boyutu seçin ve ardından **Seç**' i seçin.
8. **Ayarlar** bölümünde, Varsayılanları kabul edin. **AZS-VNET** sanal ağının seçili olduğundan emin olun. Alt ağın **10.1.0.0/24**olarak ayarlandığını doğrulayın. Sonra **Tamam**’ı seçin.

9. **Özet** bölümünde ayarları gözden geçirin ve ardından **Tamam**' ı seçin.

## <a name="test-the-connection"></a>Bağlantıyı sınama

Siteden siteye bağlantı kurulduktan sonra, her iki yönde de veri akışı alabildiğinizi doğrulamanız gerekir. Bağlantıyı test etmenin en kolay yolu bir ping testi yapar:

* Azure Stack oluşturduğunuz VM 'de oturum açın ve Azure 'da VM 'ye ping gönderin.
* Azure 'da oluşturduğunuz VM 'de oturum açın ve Azure Stack VM 'ye ping gönderin.

>[!NOTE]
>Siteden siteye bağlantı üzerinden trafik göndermekte olduğunuzdan emin olmak için VIP değil, uzak alt ağdaki VM 'nin doğrudan IP (DIP) adresine ping gönderin.

### <a name="sign-in-to-the-user-vm-in-azure-stack"></a>Azure Stack Kullanıcı sanal makinesinde oturum açın

1. Azure Stack portalında oturum açın.
2. Sol gezinti çubuğunda **sanal makineler**' i seçin.
3. VM 'Ler listesinde, daha önce oluşturduğunuz **AZS-VM** ' yi bulun ve ardından seçin.
4. VM 'nin bölümünde, **Bağlan**' ı seçin ve ardından AZS-VM. rdp dosyasını açın.

     ![Bağlan düğmesi](media/azure-stack-connect-vpn/image17.png)

5. VM oluştururken yapılandırdığınız hesapla oturum açın.
6. Yükseltilmiş bir Windows PowerShell istemi açın.
7. Tür **ipconfig/all**.
8. Çıktıda, **IPv4 adresini**bulun ve daha sonra kullanmak üzere adresi kaydedin. Bu, Azure 'dan ping yaptığınız adrestir. Örnek ortamda, adres **10.1.0.4**olur, ancak ortamınızda farklı olabilir. Daha önce oluşturduğunuz **10.1.0.0/24** alt ağı içinde yer almalıdır.
9. VM 'nin ping komutuna yanıt vermesini sağlayan bir güvenlik duvarı kuralı oluşturmak için aşağıdaki PowerShell komutunu çalıştırın:

   ```powershell
   New-NetFirewallRule `
    -DisplayName "Allow ICMPv4-In" `
    -Protocol ICMPv4
   ```

### <a name="sign-in-to-the-tenant-vm-in-azure"></a>Azure 'da Kiracı VM 'de oturum açma

1. Azure Portal’da oturum açın.
2. Sol gezinti çubuğunda **sanal makineler**' i seçin.
3. VM 'Ler listesinden daha önce oluşturduğunuz **Azure-VM** ' yi bulun ve ardından seçin.
4. VM 'nin bölümünde **Bağlan**' ı seçin.
5. VM oluştururken yapılandırdığınız hesapla oturum açın.
6. Yükseltilmiş bir **Windows PowerShell** penceresi açın.
7. Tür **ipconfig/all**.
8. **10.100.0.0/24**içinde kalan bir IPv4 adresi görmeniz gerekir. Örnek ortamda, adres **10.100.0.4**olur, ancak adresiniz farklı olabilir.
9. VM 'nin ping komutuna yanıt vermesini sağlayan bir güvenlik duvarı kuralı oluşturmak için aşağıdaki PowerShell komutunu çalıştırın:

   ```powershell
   New-NetFirewallRule `
    -DisplayName "Allow ICMPv4-In" `
    -Protocol ICMPv4
   ```

10. Azure 'daki VM 'den, tünel aracılığıyla Azure Stack sanal makinesine ping gönderin. Bunu yapmak için AZS-VM ' d e kaydettiğiniz DIP 'e ping atayın. Örnek ortamda, bu **10.1.0.4**'dir, ancak laboratuvarınızda not ettiğiniz adrese ping atadığınızdan emin olun. Aşağıdaki ekran yakalama gibi görünen bir sonuç görmeniz gerekir:

    ![Başarılı ping](media/azure-stack-connect-vpn/image19b.png)

11. Uzak VM 'den bir yanıt, başarılı bir test olduğunu gösterir. VM penceresini kapatabilirsiniz.

Ayrıca daha kapsamlı veri aktarımı testi de yapmalısınız (örneğin, farklı boyutlardaki dosyaları her iki yönde de kopyalama).

### <a name="viewing-data-transfer-statistics-through-the-gateway-connection"></a>Ağ geçidi bağlantısı üzerinden veri aktarımı istatistiklerini görüntüleme

Siteden siteye bağlantınızın ne kadar veri geçireceğini öğrenmek isterseniz, bu bilgiler **bağlantı** bölümünde bulunur. Bu test, henüz VPN bağlantısı üzerinden gönderdiğiniz ping bağlantısını doğrulamak için de başka bir yoldur.

1. Azure Stack içinde Kullanıcı sanal makinesinde oturum açana sırada Kullanıcı portalında oturum açmak için kullanıcı hesabınızı kullanın.
2. **Tüm kaynaklar**' a gidin ve ardından **AZS-Azure** bağlantısı ' nı seçin. **Bağlantılar** görüntülenir.
3. **Bağlantı** bölümünde, **içindeki verilerin** istatistikleri ve **veri çıkışı** görüntülenir. Aşağıdaki ekran yakalamada, büyük sayılar ek dosya aktarımına atanır. Burada sıfır dışında bir değer görmeniz gerekir.

    ![Veri ve çıkış](media/azure-stack-connect-vpn/Connection.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Uygulamaları Azure 'a dağıtma ve Azure Stack](azure-stack-solution-pipeline.md)
