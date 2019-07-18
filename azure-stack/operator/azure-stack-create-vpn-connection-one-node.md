---
title: Farklı Azure Stack Geliştirme Seti ortamlarındaki iki sanal ağ arasında siteden siteye VPN bağlantısı oluşturma | Microsoft Docs
description: Bir bulut yöneticisinin iki tek düğümlü Azure Stack Geliştirme Seti ortamları arasında siteden siteye VPN bağlantısı oluşturmak için kullandığı adım adım yordam.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 3f1b4e02-dbab-46a3-8e11-a777722120ec
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/16/2019
ms.author: sethm
ms.reviewer: scottnap
ms.lastreviewed: 09/12/2018
ROBOTS: NOINDEX
ms.openlocfilehash: f34ed3459ad8346860872a4b63a25e214501a2dd
ms.sourcegitcommit: 4139b507d6da98a086929da48e3b4661b70bc4f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68299474"
---
# <a name="create-a-site-to-site-vpn-connection-between-two-virtual-networks-in-different-azure-stack-development-kit-environments"></a>Farklı Azure Stack Geliştirme Seti ortamlarındaki iki sanal ağ arasında siteden siteye VPN bağlantısı oluşturma

## <a name="overview"></a>Genel Bakış

Bu makalede iki ayrı Azure Stack Geliştirme Seti (ASDK) ortamında iki sanal ağ arasında siteden siteye VPN bağlantısı oluşturma açıklanır. Bağlantıları yapılandırırken Azure Stack ' deki VPN ağ geçitlerinin nasıl çalıştığını öğrenirsiniz.

### <a name="connection"></a>Bağlantı

Aşağıdaki şekilde, bağlantı yapılandırmasının bitirdiğinizde nasıl görünmesi gerektiği gösterilmektedir.

![Siteden siteye VPN bağlantısı yapılandırması](media/azure-stack-create-vpn-connection-one-node-tp2/OneNodeS2SVPN.png)

### <a name="before-you-begin"></a>Başlamadan önce

Bağlantı yapılandırmasını gerçekleştirmek için, başlamadan önce aşağıdaki öğelere sahip olduğunuzdan emin olun:

* Hızlı Başlangıç bölümünde [açıklandığı gibi, asdk donanım gereksinimlerini karşılayan iki sunucu ve diğer Önkoşullar: Azure Stack Geliştirme Seti](../asdk/asdk-download.md)değerlendirin.
* [Azure Stack geliştirme seti](https://azure.microsoft.com/overview/azure-stack/try/) dağıtım paketi.

## <a name="deploy-the-azure-stack-development-kit-environments"></a>Azure Stack Geliştirme Seti ortamlarını dağıtma

Bağlantı yapılandırmasını gerçekleştirmek için iki adet ASDK ortamı dağıtmanız gerekir.

> [!NOTE]
> Dağıttığınız her bir ASDK için [dağıtım talimatlarını](../asdk/asdk-install.md)izleyin. Bu makalede, ASDK ortamları **POC1** ve **POC2**olarak adlandırılır.

## <a name="prepare-an-offer-on-poc1-and-poc2"></a>POC1 ve POC2 üzerinde teklif hazırlama

Hem POC1 hem de POC2 ' de bir teklif hazırlayın, bir Kullanıcı teklife abone olabilir ve sanal makineleri dağıtabilir. Teklif oluşturma hakkında daha fazla bilgi için bkz. [sanal makineleri Azure Stack kullanıcılarınız için kullanılabilir hale getirme](azure-stack-tutorial-tenant-vm.md).

## <a name="review-and-complete-the-network-configuration-table"></a>Ağ yapılandırma tablosunu gözden geçirin ve doldurun

Aşağıdaki tabloda hem ASDK ortamları için ağ yapılandırması özetlenmektedir. Ağınız için özel dış BGPNAT adresini eklemek üzere tablodan sonra görüntülenen yordamı kullanın.

### <a name="network-configuration-table"></a>Ağ yapılandırma tablosu

|   |POC1|POC2|
|---------|---------|---------|
|Sanal ağ adı     |VNET-01|VNET-02 |
|Sanal ağ adres alanı |10.0.10.0/23|10.0.20.0/23|
|Alt ağ adı     |Alt ağ-01|Alt ağ-02|
|Alt ağ adres aralığı|10.0.10.0/24 |10.0.20.0/24 |
|Ağ geçidi alt ağı     |10.0.11.0/24|10.0.21.0/24|
|Dış BGPNAT adresi     |         |         |

> [!NOTE]
> Örnek ortamdaki dış BGPNAT IP adresleri POC1 için 10.16.167.195 ve POC2 için 10.16.169.131. ASDK konaklarınızın dış BGPNAT IP adreslerini öğrenmek için aşağıdaki yordamı kullanın ve ardından bunları önceki ağ yapılandırması tablosuna ekleyin.

### <a name="get-the-ip-address-of-the-external-adapter-of-the-nat-vm"></a>NAT VM dış bağdaştırıcısının IP adresini alma

1. POC1 için Azure Stack fiziksel makinesinde oturum açın.
2. Yönetici parolanızı eklemek için aşağıdaki PowerShell kodunu düzenleyin ve ardından kodu POC konağında çalıştırın:

   ```powershell
   cd \AzureStack-Tools-master\connect
   Import-Module .\AzureStack.Connect.psm1
   $Password = ConvertTo-SecureString "<your administrator password>" `
    -AsPlainText `
    -Force
   Get-AzureStackNatServerAddress `
    -HostComputer "AzS-bgpnat01" `
    -Password $Password
   ```

3. IP adresini, önceki bölümde görünen ağ yapılandırması tablosuna ekleyin.

4. Bu yordamı POC2 üzerinde yineleyin.

## <a name="create-the-network-resources-in-poc1"></a>POC1 'de ağ kaynaklarını oluşturma

Artık ağ geçitlerini kurmak için ihtiyacınız olan POC1 ağ kaynaklarını oluşturabilirsiniz. Aşağıdaki yönergelerde Azure Stack Kullanıcı Portalı kullanılarak kaynakların nasıl oluşturulacağı açıklanır. Kaynakları oluşturmak için PowerShell kodunu da kullanabilirsiniz.

![Kaynak oluşturmak için iş akışı](media/azure-stack-create-vpn-connection-one-node-tp2/image2.png)

### <a name="sign-in-as-a-tenant"></a>Kiracı olarak oturum açın

Hizmet Yöneticisi, kiracılarının kullanabileceği planları, teklifleri ve abonelikleri test etmek için kiracı olarak oturum açabilir. Henüz bir tane yoksa, oturum açmadan önce [bir kiracı hesabı oluşturun](azure-stack-add-new-user-aad.md) .

### <a name="create-the-virtual-network-and-vm-subnet"></a>Sanal ağ ve VM alt ağı oluşturma

1. Kullanıcı portalında oturum açmak için kiracı hesabı kullanın.
2. Kullanıcı portalında **+ kaynak oluştur**' u seçin.
3. **Market**' e gidin ve ardından **ağ**' ı seçin.
4. **Sanal ağ**'ı seçin.
5. **Ad**, **Adres alanı**, **alt ağ adı**ve **alt ağ adres aralığı**için, ağ yapılandırma tablosunda daha önce görünen değerleri kullanın.
6. **Abonelikte**, daha önce oluşturduğunuz abonelik görüntülenir.
7. **Kaynak grubu**için, bir kaynak grubu oluşturabilir veya zaten bir tane varsa **var olanı kullan**' ı seçin.
8. Varsayılan konumu doğrulayın.
9. **Panoya sabitle**’yi seçin.
10. **Oluştur**’u seçin.

### <a name="create-the-gateway-subnet"></a>Ağ geçidi alt ağını oluşturma

1. Panoda, daha önce oluşturduğunuz VNET-01 sanal ağ kaynağını açın.
2. **Ayarlar** dikey penceresinde **Alt ağlar**’ı seçin.
3. Sanal ağa bir ağ geçidi alt ağı eklemek için **ağ geçidi alt ağı**' nı seçin.

    ![Ağ geçidi alt ağı ekle](media/azure-stack-create-vpn-connection-one-node-tp2/image4.png)

4. Varsayılan olarak, alt ağ adı **Gatewaysubnet**olarak ayarlanır. Ağ geçidi alt ağları özeldir. Düzgün çalışması için **Gatewaysubnet** adını kullanmaları gerekir.
5. **Adres aralığı**' nda, adresin **10.0.11.0/24**olduğunu doğrulayın.
6. Ağ geçidi alt ağını oluşturmak için **Tamam ' ı** seçin.

### <a name="create-the-virtual-network-gateway"></a>Sanal ağ geçidini oluşturma

1. Azure portal **+ kaynak oluştur**' u seçin.
2. **Market**' e gidin ve ardından **ağ**' ı seçin.
3. Ağ kaynakları listesinden **sanal ağ geçidi**' ni seçin.
4. **Ad**alanına **GW1**girin.
5. Bir sanal ağ seçmek için **sanal ağ** öğesini seçin. Listeden **VNET-01** ' i seçin.
6. **Genel IP adresi** menü öğesini seçin. **Ortak IP adresi Seç** penceresi açıldığında **Yeni oluştur**' u seçin.
7. **Ad**alanına **GW1-PIP**girin ve **Tamam**' ı seçin.
8. Varsayılan olarak, **VPN türü**için **Rota tabanlı** seçilidir. **Rota tabanlı** VPN türünü saklayın.
9. **Abonelik** ve **Konum** seçeneklerinin doğruluğunu onaylayın. Kaynağı panoya sabitleyebilir. **Oluştur**’u seçin.

### <a name="create-the-local-network-gateway"></a>Yerel ağ geçidini oluşturma

Bu Azure Stack değerlendirme dağıtımındaki *yerel ağ geçidi* uygulaması, gerçek bir Azure dağıtımındaki uygulamadan biraz farklıdır.

Bir Azure dağıtımında, yerel ağ geçidi, Azure 'daki bir sanal ağ geçidine bağlanmak için kullandığınız şirket içi (kiracı) fiziksel cihazını temsil eder. Bu Azure Stack değerlendirme dağıtımında, bağlantının her iki ucu da sanal ağ geçitleri ' dir.

Bu konuda daha ayrıntılı bir şekilde düşünmek için, yerel ağ geçidi kaynağının bağlantının diğer ucundaki uzak ağ geçidini her zaman gösterdiği bir yoldur. Azure Stack Geliştirme Seti tasarlandığından, yerel ağ geçidinin genel IP adresi olarak diğer ASDK 'nin ağ adresi çevirisi (NAT) VM 'sinde dış ağ bağdaştırıcısının IP adresini sağlamanız gerekir. Daha sonra her iki ucunun de düzgün bağlandığından emin olmak için NAT sanal makinesinde NAT eşlemeleri oluşturursunuz.

### <a name="create-the-local-network-gateway-resource"></a>Yerel ağ geçidi kaynağı oluşturma

1. POC1 için Azure Stack fiziksel makinesinde oturum açın.
2. Kullanıcı portalında **+ kaynak oluştur**' u seçin.
3. **Market**' e gidin ve ardından **ağ**' ı seçin.
4. Kaynak listesinden **yerel ağ geçidi**' ni seçin.
5. **Ad**alanına **POC2-GW**yazın.
6. **IP adresi**' nde, POC2 için dış BGPNAT adresini girin. Bu adres, daha önce ağ yapılandırma tablosunda görünür.
7. **Adres alanı**' nda, daha sonra oluşturduğunuz POC2 VNET 'in adres alanı için **10.0.20.0/23**girin.
8. **Aboneliğinizin**, **kaynak grubunuzun**ve **konum** değerlerinizin doğru olduğundan emin olun ve ardından **Oluştur**' u seçin.

### <a name="create-the-connection"></a>Bağlantı oluşturma

1. Kullanıcı portalında **+ kaynak oluştur**' u seçin.
2. **Market**' e gidin ve ardından **ağ**' ı seçin.
3. Kaynak listesinden **bağlantı**' yı seçin.
4. **Temel bilgiler** dikey penceresinde, **bağlantı türü**Için **siteden siteye (IPSec)** seçeneğini belirleyin.
5. **Abonelik**, **kaynak grubu**ve **konum**' u seçip **Tamam**' ı seçin.
6. **Ayarlar** dikey penceresinde **sanal ağ geçidi**' ni seçin ve ardından **GW1**' ı seçin.
7. **Yerel ağ geçidi**' ni seçin ve ardından **POC2-GW**' yi seçin.
8. **Bağlantı adı**alanına **POC1-POC2**girin.
9. **Paylaşılan anahtar (PSK)** alanına **12345**yazın ve ardından **Tamam**' ı seçin.
10. **Özet** dikey penceresinde **Tamam**' ı seçin.

### <a name="create-a-vm"></a>VM oluşturma

VPN bağlantısı üzerinden taşınan verileri doğrulamak için, sanal makinelerin her bir Azure Stack Geliştirme Seti veri alıp alabilmesi gerekir. POC1 ' de şu anda bir sanal makine oluşturun ve sonra sanal ağınızda VM alt ağına yerleştirin:

1. Azure portal **+ kaynak oluştur**' u seçin.
2. **Market**' e gidin ve ardından **işlem**' ı seçin.
3. Sanal makine görüntüleri listesinde, **Windows Server 2016 Datacenter eval** görüntüsünü seçin.
4. **Temel bilgiler** dikey penceresinde **ad**alanına **VM01**girin.
5. Geçerli bir Kullanıcı adı ve parola girin. Bu hesabı, oluşturulduktan sonra VM 'de oturum açmak için kullanırsınız.
6. Bir **abonelik**, **kaynak grubu**ve **konum**sağlayın ve **Tamam**' ı seçin.
7. **Boyut** dikey penceresinde, bu örnek için bir sanal makine boyutu seçin ve ardından **Seç**' i seçin.
8. **Ayarlar** dikey penceresinde varsayılan değerleri kabul edin. **VNET-01** sanal ağının seçili olduğundan emin olun. Alt ağın **10.0.10.0/24**olarak ayarlandığını doğrulayın. Sonra **Tamam**’ı seçin.
9. **Özet** dikey penceresinde ayarları gözden geçirin ve ardından **Tamam**' ı seçin.

## <a name="create-the-network-resources-in-poc2"></a>POC2 'de ağ kaynaklarını oluşturma

Sonraki adım POC2 için ağ kaynakları oluşturmaktır. Aşağıdaki yönergelerde, Kullanıcı Portalı kullanılarak kaynakların nasıl oluşturulacağı gösterilmektedir.

### <a name="sign-in-as-a-tenant-again"></a>Kiracı olarak tekrar oturum açın

Hizmet Yöneticisi, kiracılarının kullanabileceği planları, teklifleri ve abonelikleri test etmek için kiracı olarak oturum açabilir. Henüz bir tane yoksa, oturum açmadan önce [bir kiracı hesabı oluşturun](azure-stack-add-new-user-aad.md) .

### <a name="create-virtual-network-and-vm-subnet"></a>Sanal ağ ve VM alt ağı oluşturma

1. Kiracı hesabı kullanarak oturum açın.
2. Kullanıcı portalında **+ kaynak oluştur**' u seçin.
3. **Market**' e gidin ve ardından **ağ**' ı seçin.
4. **Sanal ağ**'ı seçin.
5. POC2 **adı**, **Adres alanı**, **alt ağ adı**ve **alt ağ adres aralığı**değerlerini belirlemek için ağ yapılandırması tablosunda daha önce görünen bilgileri kullanın.
6. **Abonelikte**, daha önce oluşturduğunuz abonelik görüntülenir.
7. **Kaynak grubu**için yeni bir kaynak grubu oluşturun veya zaten bir tane varsa, **var olanı kullan**' ı seçin.
8. Varsayılan **konumu**doğrulayın.
9. **Panoya sabitle**’yi seçin.
10. **Oluştur**’u seçin.

### <a name="create-gateway-subnet"></a>Ağ geçidi alt ağı oluştur

1. Panoda oluşturduğunuz sanal ağ kaynağını (**VNET-02**) açın.
2. **Ayarlar** dikey penceresinde **Alt ağlar**’ı seçin.
3. Sanal ağa bir ağ geçidi alt ağı eklemek için **ağ geçidi alt ağı** ' nı seçin.
4. Alt ağın adı varsayılan olarak **GatewaySubnet** şeklinde ayarlanır. Ağ geçidi alt ağları özeldir ve düzgün şekilde çalışabilmesi için bu ada sahip olmalıdır.
5. **Adres aralığı** alanında, adresin **10.0.21.0/24**olduğunu doğrulayın.
6. Ağ geçidi alt ağını oluşturmak için **Tamam ' ı** seçin.

### <a name="create-virtual-network-gateway"></a>Sanal ağ geçidi oluştur

1. Azure portal **+ kaynak oluştur**' u seçin.  
2. **Market**' e gidin ve ardından **ağ**' ı seçin.
3. Ağ kaynakları listesinden **sanal ağ geçidi**' ni seçin.
4. **Ad**alanına **GW2**girin.
5. Bir sanal ağ seçmek için **sanal ağ**' ı seçin. Sonra listeden **VNET-02** ' i seçin.
6. **Genel IP adresi**'ni seçin. **Ortak IP adresi Seç** dikey penceresi açıldığında **Yeni oluştur**' u seçin.
7. **Ad**alanına **GW2-PIP**girin ve **Tamam**' ı seçin.
8. Varsayılan olarak, **VPN türü**için **Rota tabanlı** seçilidir. **Rota tabanlı** VPN türünü saklayın.
9. **Abonelik** ve **Konum** seçeneklerinin doğruluğunu onaylayın. Kaynağı panoya sabitleyebilir. **Oluştur**’u seçin.

### <a name="create-local-network-gateway-resource"></a>Yerel ağ geçidi kaynağı oluştur

1. POC2 Kullanıcı portalında **+ kaynak oluştur**' u seçin.
2. **Market**' e gidin ve ardından **ağ**' ı seçin.
3. Kaynak listesinden **yerel ağ geçidi**' ni seçin.
4. **Ad**alanına **POC1-GW**yazın.
5. **IP adresi**' nde, daha önce ağ yapılandırma tablosunda listelenen POC1 için dış BGPNAT adresini girin.
6. **Adres alanı**' nda, POC1 adresinden **VNET-01** **10.0.10.0/23** adres alanını girin.
7. **Aboneliğinizin**, **kaynak grubunuzun**ve **konumun** doğru olduğundan emin olun ve ardından **Oluştur**' u seçin.

## <a name="create-connection"></a>Bağlantı oluşturma

1. Kullanıcı portalında **+ kaynak oluştur**' u seçin.
2. **Market**' e gidin ve ardından **ağ**' ı seçin.
3. Kaynak listesinden **bağlantı**' yı seçin.
4. **Temel** ayarlar dikey penceresinde, **bağlantı türü**Için **siteden siteye (IPSec)** öğesini seçin.
5. **Abonelik**, **kaynak grubu**ve **konum**' u seçip **Tamam**' ı seçin.
6. **Ayarlar** dikey penceresinde **sanal ağ geçidi**' ni seçin ve ardından **GW2**' ı seçin.
7. **Yerel ağ geçidi**' ni seçin ve ardından **POC1-GW**' yi seçin.
8. **Bağlantı adı**alanına **POC2-POC1**girin.
9. **Paylaşılan anahtar (PSK)** alanına **12345**yazın. Farklı bir değer seçerseniz, POC1 üzerinde oluşturduğunuz paylaşılan anahtarın değeriyle eşleşmesi gerektiğini unutmayın. **Tamam**’ı seçin.
10. **Özet** dikey penceresini gözden geçirin ve ardından **Tamam**' ı seçin.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Şimdi POC2 içinde bir sanal makine oluşturun ve sanal ağınızdaki VM alt ağına yerleştirin:

1. Azure portal **+ kaynak oluştur**' u seçin.
2. **Market**' e gidin ve ardından **işlem**' ı seçin.
3. Sanal makine görüntüleri listesinde, **Windows Server 2016 Datacenter eval** görüntüsünü seçin.
4. **Temel bilgiler** dikey penceresinde, **ad**için **VM02**girin.
5. Geçerli bir Kullanıcı adı ve parola girin. Bu hesabı, oluşturulduktan sonra sanal makinede oturum açmak için kullanırsınız.
6. Bir **abonelik**, **kaynak grubu**ve **konum**sağlayın ve **Tamam**' ı seçin.
7. **Boyut** dikey penceresinde bu örnek için bir sanal makine boyutu seçin ve ardından **Seç**' i seçin.
8. **Ayarlar** dikey penceresinde varsayılan değerleri kabul edebilirsiniz. **VNET-02** sanal ağının seçili olduğundan emin olun ve alt ağın **10.0.20.0/24**olarak ayarlandığını doğrulayın. **Tamam**’ı seçin.
9. **Özet** dikey penceresinde ayarları gözden geçirin ve ardından **Tamam**' ı seçin.

## <a name="configure-the-nat-virtual-machine-on-each-azure-stack-development-kit-for-gateway-traversal"></a>Her Azure Stack Geliştirme Seti ağ geçidi geçişi için NAT sanal makinesini yapılandırın

ASDK, fiziksel konağın dağıtıldığı ağdan bağımsız ve yalıtılmış olduğundan, ağ geçitlerinin bağlı olduğu *dış* VIP ağı aslında dış değildir. Bunun yerine, VIP ağı ağ adresi çevirisini gerçekleştiren bir yönlendiricinin arkasında gizlenir.

Yönlendirici, ASDK altyapısında yönlendirme ve uzaktan erişim Hizmetleri (RRAS) rolünü çalıştıran **AZS-bgpnat01**adlı bir Windows Server sanal makinedir. Site-Site VPN bağlantısının her iki uçta da bağlanmasına izin vermek için AzS-bgpnat01 sanal makinesinde NAT 'yi yapılandırmanız gerekir.

VPN bağlantısını yapılandırmak için, BGPNAT sanal makinesindeki dış arabirimi kenar ağ geçidi havuzunun VIP 'sine eşleyen bir statik NAT eşleme yolu oluşturmanız gerekir. Bir VPN bağlantısındaki her bağlantı noktası için statik bir NAT eşleme yolu gereklidir.

> [!NOTE]
> Bu yapılandırma yalnızca Azure Stack Geliştirme Seti ortamları için gereklidir.

### <a name="configure-the-nat"></a>NAT 'yi yapılandırma

> [!IMPORTANT]
> Bu yordamı hem ASDK ortamları için gerçekleştirmeniz gerekir.

1. Aşağıdaki PowerShell betiğinden kullanılacak **Iç IP adresini** saptayın. Sanal ağ geçidi ' ni (GW1 ve GW2) açın ve **genel bakış** dikey penceresinde, daha sonra kullanmak üzere **genel IP adresi** için değeri kaydedin.

   ![İç IP adresi](media/azure-stack-create-vpn-connection-one-node-tp2/InternalIP.PNG)

2. POC1 için Azure Stack fiziksel makinesinde oturum açın.
3. Aşağıdaki PowerShell betiğini kopyalayın ve düzenleyin. Her bir Azure Stack Geliştirme Seti NAT yapılandırmak için betiği yükseltilmiş bir Windows PowerShell ISE çalıştırın. Betikte, `External BGPNAT address` ve `Internal IP address` yer tutuculara değerler ekleyin:

   ```powershell
   # Designate the external NAT address for the ports that use the IKE authentication.
   Invoke-Command `
    -ComputerName AzS-bgpnat01 `
     {Add-NetNatExternalAddress `
      -NatName BGPNAT `
      -IPAddress <External BGPNAT address> `
      -PortStart 499 `
      -PortEnd 501}
   Invoke-Command `
    -ComputerName AzS-bgpnat01 `
     {Add-NetNatExternalAddress `
      -NatName BGPNAT `
      -IPAddress <External BGPNAT address> `
      -PortStart 4499 `
      -PortEnd 4501}
   # create a static NAT mapping to map the external address to the Gateway
   # Public IP Address to map the ISAKMP port 500 for PHASE 1 of the IPSEC tunnel
   Invoke-Command `
    -ComputerName AzS-bgpnat01 `
     {Add-NetNatStaticMapping `
      -NatName BGPNAT `
      -Protocol UDP `
      -ExternalIPAddress <External BGPNAT address> `
      -InternalIPAddress <Internal IP address> `
      -ExternalPort 500 `
      -InternalPort 500}
   # Finally, configure NAT traversal which uses port 4500 to
   # successfully establish the complete IPSEC tunnel over NAT devices
   Invoke-Command `
    -ComputerName AzS-bgpnat01 `
     {Add-NetNatStaticMapping `
      -NatName BGPNAT `
      -Protocol UDP `
      -ExternalIPAddress <External BGPNAT address> `
      -InternalIPAddress <Internal IP address> `
      -ExternalPort 4500 `
      -InternalPort 4500}
   ```

4. Bu yordamı POC2 üzerinde yineleyin.

## <a name="test-the-connection"></a>Bağlantıyı sınama

Siteden siteye bağlantı kuruldığına göre, bundan sonra trafiği akışa alabildiğinizi doğrulamanız gerekir. Doğrulamak için, herhangi bir ASDK ortamında oluşturduğunuz sanal makinelerden birinde oturum açın. Daha sonra, diğer ortamda oluşturduğunuz sanal makineye ping gönderin.

Trafiği siteden siteye bağlantı üzerinden gönderdiğinizden emin olmak için, VIP değil, uzak alt ağdaki sanal makinenin doğrudan IP (DIP) adresine ping atatığınızdan emin olun. Bunu yapmak için bağlantının diğer ucundaki DIP adresini bulun. Daha sonra kullanmak üzere adresi kaydedin.

### <a name="sign-in-to-the-tenant-vm-in-poc1"></a>POC1 içinde Kiracı VM 'de oturum açma

1. POC1 için Azure Stack fiziksel makinesinde oturum açın ve ardından Kullanıcı portalında oturum açmak için bir kiracı hesabı kullanın.
2. Sol gezinti çubuğunda **işlem**' i seçin.
3. VM 'Ler listesinde, daha önce oluşturduğunuz **VM01** bulun ve ardından seçin.
4. Sanal makinenin dikey penceresinde **Bağlan**' a tıklayın ve ardından VM01. rdp dosyasını açın.

     ![Bağlan düğmesi](media/azure-stack-create-vpn-connection-one-node-tp2/image17.png)

5. Sanal makineyi oluştururken yapılandırdığınız hesapla oturum açın.
6. Yükseltilmiş bir **Windows PowerShell** penceresi açın.
7. **İpconfig/all**yazın.
8. Çıktıda, **IPv4 adresini**bulun ve daha sonra kullanmak üzere adresi kaydedin. Bu, POC2 adresinden ping yapılacak adrestir. Örnek ortamda adres **10.0.10.4** şeklindedir, ancak sizin ortamınızda farklı olabilir. Daha önce oluşturduğunuz **10.0.10.0/24** alt ağı içinde yer almalıdır.
9. Sanal makinenin ping komutuna yanıt vermesini sağlayan bir güvenlik duvarı kuralı oluşturmak için aşağıdaki PowerShell komutunu çalıştırın:

   ```powershell
   New-NetFirewallRule `
    -DisplayName "Allow ICMPv4-In" `
    -Protocol ICMPv4
   ```

### <a name="sign-in-to-the-tenant-vm-in-poc2"></a>POC2 içinde Kiracı VM 'de oturum açma

1. POC2 için Azure Stack fiziksel makinesinde oturum açın ve ardından Kullanıcı portalında oturum açmak için bir kiracı hesabı kullanın.
2. Sol gezinti çubuğunda, **işlem**' e tıklayın.
3. Sanal makineler listesinden daha önce oluşturduğunuz **VM02** bulun ve ardından seçin.
4. Sanal makine için dikey pencere üzerinde **Bağlan**’a tıklayın.
5. Sanal makineyi oluştururken yapılandırdığınız hesapla oturum açın.
6. Yükseltilmiş bir **Windows PowerShell** penceresi açın.
7. **İpconfig/all**yazın.
8. **10.0.20.0/24**içinde kalan bir IPv4 adresi görüntülenir. Örnek ortamda, adres **10.0.20.4**olur, ancak adresiniz farklı olabilir.
9. Sanal makinenin ping komutuna yanıt vermesini sağlayan bir güvenlik duvarı kuralı oluşturmak için aşağıdaki PowerShell komutunu çalıştırın:

   ```powershell
   New-NetFirewallRule `
    -DisplayName "Allow ICMPv4-In" `
    -Protocol ICMPv4
   ```

10. POC2 üzerindeki sanal makineden, tünel üzerinden POC1 üzerindeki sanal makineye ping gönderin. Bunu yapmak için, VM01 adresinden kaydettiğiniz DIP 'e ping atayın. Örnek ortamda, bu **10.0.10.4**'dir, ancak laboratuvarınızda not ettiğiniz adrese ping atadığınızdan emin olun. Aşağıdaki örnekteki gibi görünen bir sonuç görmeniz gerekir:

    ![Başarılı ping](media/azure-stack-create-vpn-connection-one-node-tp2/image19b.png)
11. Uzak sanal makineden alınan bir yanıt, başarılı bir test olduğunu gösterir. Sanal makine penceresini kapatabilirsiniz. Bağlantınızı test etmek için, bir dosya kopyası gibi diğer veri aktarımı türlerini deneyebilirsiniz.

### <a name="viewing-data-transfer-statistics-through-the-gateway-connection"></a>Ağ geçidi bağlantısı üzerinden veri aktarımı istatistiklerini görüntüleme

Siteden siteye bağlantınızın ne kadar veri geçireceğini öğrenmek isterseniz, bu bilgiler **bağlantı** dikey penceresinde kullanılabilir. Bu test, az önce gönderdiğiniz ping 'un VPN bağlantısı üzerinden gittiğini doğrulamak için de başka bir yoldur.

1. POC2 ' de kiracı sanal makinesinde oturum açtığınızdan, Kullanıcı portalında oturum açmak için kiracı hesabınızı kullanın.
2. **Tüm kaynaklara**gidin ve **POC2-POC1** bağlantısını seçin. **Bağlantılar** görüntülenir.
3. **Bağlantı** penceresinde, **içindeki verilerin** istatistikleri ve **veri çıkışı** görüntülenir. Aşağıdaki ekran görüntüsünde, büyük sayılar ek dosya aktarımına atanır. Burada sıfır dışında bir değer görmeniz gerekir.

    ![Veri ve çıkış](media/azure-stack-create-vpn-connection-one-node-tp2/image20.png)
