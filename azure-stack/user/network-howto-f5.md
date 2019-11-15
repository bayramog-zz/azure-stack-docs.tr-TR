---
title: SANAL cihazınızı, Commkasasıyla Azure Stack hub 'da yedekleme | Microsoft Docs
description: SANAL cihazınızı, Azure Stack hub 'ında Commkasasıyla nasıl yedekleyeceğinizi öğrenin.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: how-to
ms.date: 11/06/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 11/06/2019
ms.openlocfilehash: 64fef6bdcd0315bc9dc289b48f80b406cefba0eb
ms.sourcegitcommit: bb2bbfad8061f7677954f6ce5a435b4e6f9299b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2019
ms.locfileid: "74103008"
---
# <a name="how-to-deploy-f5-across-two-azure-stack-hub-instances"></a>İki Azure Stack hub örneği üzerinde F5 Dağıtımı

Bu makalede, iki Azure Stack hub ortamında bir dış yük dengeleyici ayarlama işlemi adım adım açıklanmaktadır. Farklı iş yüklerini yönetmek için bu yapılandırmayı kullanabilirsiniz. Bu makalede, F5 'i iki bağımsız Azure Stack hub örneği üzerinde genel bir yük dengeleme çözümü olarak dağıtacaksınız. Ayrıca, bir NGıNX sunucusunda çalışan yük dengeli bir Web uygulamasını iki örneğiz üzerinde de dağıtırsınız. Bunlar, yüksek kullanılabilirliğe sahip bir F5 sanal araçları 'nın arkasında çalışır.

Azure Resource Manager şablonlarını [F5-azurestack-GSLB](https://github.com/Mikej81/f5-azurestack-gslb) GitHub deposunda bulabilirsiniz.

## <a name="overview-of-load-balancing-with-f5"></a>F5 ile yük dengelemeye genel bakış

F5 donanımı, yük dengeleyici, Azure Stack dışında ve Azure Stack barındıran veri merkezi içinde olabilir. Azure Stack, iki ayrı Azure Stack dağıtımda yük dengelemesi yükünü dengelemek için yerel bir özelliğe sahip değildir. F5's BIG-IP Virtual Edition (VE) her iki platformda de çalışır. Bu ayarlama, destekleyici uygulama hizmetlerinin çoğaltılmasıyla Azure ile Azure Stack mimarileri arasında eşliği destekler. Bir ortamda uygulama geliştirebilir ve başka bir ortamda taşıyabilirsiniz. Aynı zamanda, aynı büyük IP yapılandırması, ilkeler ve uygulama hizmetleri de dahil olmak üzere, üretime hazırlamış Azure Stack tamamını yansıtabilir. Yaklaşım, uygulama yeniden düzenleme ve test etme için daha az saat ihtiyacını ortadan kaldırır ve kod yazmaya başlamanıza olanak sağlar.

Uygulamaların ve verilerin güvenliğini sağlamak genellikle uygulamaları genel buluta taşıyan geliştiricilerin bir kaygısını sağlar. Bunun durum olması gerekmez. Azure Stack ortamınızda bir uygulama oluşturabilirsiniz, ancak güvenlik mimarı F5's Web uygulaması güvenlik duvarı (WAF) üzerinde gerekli ayarları yapılandırır. Tüm yığın, uygulamanın aynı sektör lideri WAF tarafından korunabileceği bilgilerle Azure Stack ' de çoğaltılabilir. Aynı ilkeler ve RuleSets 'ler ile farklı WAFs kullanılarak oluşturulabilecek güvenlik loopveya güvenlik açıkları olmayacaktır.

Azure Stack Azure 'dan ayrı bir market 'e sahiptir. Yalnızca belirli öğeler eklenir. Bu durumda, her bir Azure yığını üzerinde yeni bir kaynak grubu oluşturmak ve zaten kullanılabilir olan F5 Sanal Gereç 'yi dağıtmak istiyorsanız. Buradan Azure Stack örnekleri arasında ağ bağlantısına izin vermek için **Genel BIR IP** adresi gerektiğini göreceksiniz. Temelde, her iki adacılardır ve **genel IP** , her iki konumda da iletişim kurmasına izin verir.

## <a name="prerequisites-for-big-ip-ve"></a>BÜYÜK IP önkoşulları VE

-  **F5 BIG-IP ve-All (KLG, 2 önyükleme konumu)** her Azure Stack Market 'e indirin. Portalda sizin için kullanılabilir durumda değilse, bulut işletmenizin ile iletişime geçin.

-  Azure Resource Manager şablonunu şu GitHub deposunda bulabilirsiniz: https://github.com/Mikej81/f5-azurestack-gslb.

## <a name="deploy-f5-big-ip-ve-on-each-instance"></a>Her örnek için F5 BIG-IP VE dağıtma

Azure Stack örneğine ve örnek B 'ye dağıtın.

1. Azure Stack hub Kullanıcı portalında oturum açın.

2. **+ Kaynak oluştur**' u seçin.

3. `F5`yazarak Market 'te arama yapın.

4. **F5 BIG-IP ve – All (KLG, 2 önyükleme konumu)** seçeneğini belirleyin.

    ![](./media/network-howto-f5/image1.png)

5. Sonraki sayfanın alt kısmında, seçili **Oluştur**.

    ![](./media/network-howto-f5/image2.png)

6. **F5-GSLB**adlı yeni bir kaynak grubu oluşturun.

7. Dağıtımı tamamlamaya yönelik örnek olarak aşağıdaki değerleri kullanın:

    ![](./media/network-howto-f5/image3.png)

8. Dağıtımınızın başarıyla tamamlandığını doğrulayın.

    ![](./media/network-howto-f5/image4.png)

    > [!Note]  
    > Her büyük IP dağıtımı 20 dakika sürer.

## <a name="configure-big-ip-appliances"></a>BÜYÜK IP gereçlerini yapılandırma

Hem Azure Stack A hem de B için bu adımların ihtiyaçlarını takip edin.

1. BÜYÜK IP şablonu dağıtımından oluşturulan kaynakları gözden geçirmek için A Azure Stack örneği üzerinde Azure Stack hub Kullanıcı portalında oturum açın.

    ![](./media/network-howto-f5/image18.png)

2. [Büyük IP yapılandırma öğeleri](https://clouddocs.f5.com/training/community/dns/html/class1/class1.html)için F5 'teki yönergeleri izleyin. 

3. A ve B Azure Stack örneğine dağıtılan her iki gereçde dinlemek için büyük IP geniş IP listesini yapılandırın. Yönergeler için bkz. [büyük IP GTM yapılandırması](https://techdocs.f5.com/kb/en-us/products/big-ip_gtm/manuals/product/gtm-concepts-11-5-0/4.html).


4. BÜYÜK IP gereçlerinin yük devretmesini doğrulayın. Bir test sisteminde, DNS sunucularınızı aşağıdakileri kullanacak şekilde yapılandırın:
    - Azure Stack örneği A = `f5stack1-ext` genel IP adresi
    - Azure Stack örneği B = `f5stack1-ext` genel IP adresi

5. `www.contoso.com` gidin ve tarayıcınız NGıNX varsayılan sayfasını yükler.

## <a name="create-a-dns-sync-group"></a>DNS eşitleme grubu oluşturma

1. Güven sağlamak için kök hesabı etkinleştirin. [Sistem bakım hesabı parolalarını değiştirme (11. x-15. x)](https://support.f5.com/csp/article/K13121)bölümündeki yönergeyi izleyin. Güveni ayarladıktan sonra (sertifika değişimi), kök hesabı devre dışı bırakın.

1. BÜYÜK IP 'de oturum açın ve bir DNS eşitleme grubu oluşturun. Yönergeler için bkz. [büyük IP DNS eşitleme grubu oluşturma](https://f5-dns-automation-demo-12-1-x.readthedocs.io/en/latest/lab2/sync-group.html).

    > [!Note]  
    > BIP-IP gerecinin yerel IP 'sini **F5-GSLB** kaynak grubunuzda bulabilirsiniz. Ağ arabirimi "f5stack1-EXT" olur ve genel veya özel IP 'ye (erişime bağlı olarak) bağlanmak istiyorsunuz.

    ![](./media/network-howto-f5/image5.png)
          
    ![](./media/network-howto-f5/image6.png)

1. **F5-GSLB** kaynak grubunu seçin ve **f5stack1** sanal makinesini seçin, **Ayarlar** altında **ağ**' ı seçin.

## <a name="post-install-configurations"></a>Yüklemeleri sonrası yapılandırma

Yükledikten sonra, Azure Stack NSG 'nizi yapılandırıp kaynak IP adreslerini kilitlemeniz gerekir.

1. Güven kurulduktan sonra 22 numaralı bağlantı noktasını devre dışı bırakın.

2. Sisteminiz çevrimiçi olduğunda, kaynak NSG 'leri engelleyin. Yönetim NSG yönetim kaynağına kilitlenmelidir, dış (4353/TCP) NSG, eşitleme için diğer örneğe kilitlenmelidir. Sanal sunucularla uygulamalar dağıtılana kadar 443 de kilitlenir.

3. GTM_DNS kural, ' de bağlantı noktası 53 (DNS) trafiğine izin verecek şekilde ayarlanır ve büyük IP çözümleyici bir kez çalışmaya başlar. Dinleyiciler oluşturulur.

    ![](./media/network-howto-f5/image7.png)

4. BÜYÜK IP 'nin arkasındaki yük dengelemeye Azure Stack ortamınızda temel bir Web uygulaması iş yükü dağıtın. [Docker 'da NGINX ve NGINX Plus dağıtımı](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-docker/)sırasında NGNIX sunucusunu kullanmaya ilişkin bir örnek bulabilirsiniz.

    > [!Note]  
    > Hem Azure Stack A hem de B Azure Stack bir NGNX örneği dağıtın.

5. NGıNX, Azure Stack örneklerinin her biri içindeki bir Ubuntu VM üzerindeki bir Docker kapsayıcısına dağıtıldıktan sonra, sunuculardaki varsayılan Web sayfasına ulaşabildiğini doğrulayın.

    ![](./media/network-howto-f5/image8.png)

6. BÜYÜK IP gerecinin yönetim arabiriminde oturum açın. Bu örnekte, **F5-stack1-ext** genel IP adresini kullanın.

    ![](./media/network-howto-f5/image9.png)

7. BÜYÜK IP aracılığıyla NGıNX 'e erişimi yayımlayın.
    
    -  Bu görevde, WordPress uygulamasına gelen Internet erişimine izin vermek için büyük IP 'yi bir sanal sunucu ve havuz ile yapılandıracaksınız. İlk olarak NGıNX örneği için özel IP adresini belirlemeniz gerekir.

8. Azure Stack hub Kullanıcı portalında oturum açın. 

9. NGıNX ağ arabiriminizi seçin.

    ![](./media/network-howto-f5/image10.png)

10. BÜYÜK IP konsolundan, **Yerel trafik > havuzlar > havuz listesi** ' ne gidin ve **+** ' yı seçin. Tablodaki değerleri kullanarak havuzu yapılandırın. Diğer tüm alanları varsayılan değerlerine bırakın.

    ![](./media/network-howto-f5/image11.png)
    
    | Anahtar | Değer |
    | --- | --- |
    | Adı | NGINX_Pool |
    | Sistem durumu Izleyicisi | HTTPS |
    | Düğüm adı | NGINX |
    | Adres | NGıNX özel IP adresinizi \< |
    | Hizmet bağlantı noktası | 443 |

11. **Tamamlandı**' ı seçin. Doğru yapılandırıldığında, havuz durumu yeşil olur.

    ![](./media/network-howto-f5/image12.png)

    Artık sanal sunucuyu yapılandırmanız gerekir. Bunu yapmak için, önce F5 BIG-IP 'nizin özel IP 'sini bulmanız gerekir.

12. BÜYÜK IP konsolundan, **ağ > Self** IP adresine gıdın ve IP adresini aklınızda olun.

    ![](./media/network-howto-f5/image13.png)

13. Sanal **sunucular** > sanal **sunucu listesini** > **Yerel trafiğe** giderek sanal sunucu oluşturun ve **+** ' ı seçin. Tablodaki değerleri kullanarak havuzu yapılandırın. Diğer tüm alanları varsayılan değerlerine bırakın.

    | Anahtar | Değer |
    | --- | --- |
    |Adı | NGINX |
    |Hedef adres | BÜYÜK IP 'nin kendi kendine IP adresini \< |
    |Hizmet bağlantı noktası | 443 |
    |SSL profili (Istemci) | Clientssl |
    |Kaynak adresi çevirisi | Otomatik eşleme |
        
    ![](./media/network-howto-f5/image14.png)

    ![](./media/network-howto-f5/image15.png)

14. Artık NGıNX uygulaması için büyük IP yapılandırmasını tamamladınız. Doğru işlevselliği doğrulamak için, siteye gözatıp F5 Statistics ' i doğrulayın.

15. `https://<F5-public-VIP-IP>` için bir tarayıcı açın ve NGıNX varsayılan sayfanızı görüntülediğinden emin olun.

    ![](./media/network-howto-f5/image16.png)

16. Bu durumda, **Yerel trafikte > > modül istatistiklerine**giderek trafik akışını doğrulamak için sanal sunucunuzun istatistiklerini kontrol edin.

17. **Istatistik türü**altında **sanal sunucular**' ı seçin.

    ![](./media/network-howto-f5/image17.png)


## <a name="for-more-information"></a>Daha fazla bilgi edinmek için

F5 kullanımı ile ilgili bazı başvuru makalelerini bulabilirsiniz:

- [BÜYÜK IP DNS kullanan veri merkezi kullanılabilirlik Hizmetleri](https://clouddocs.f5.com/training/community/dns/html/class3/class3.html)
- [HTTP uygulamalarıyla büyük IP sistemini dağıtma](https://www.f5.com/content/dam/f5/corp/global/pdf/deployment-guides/iapp-http-dg.pdf)
- [GSLB için geniş bir IP oluşturma](https://clouddocs.f5.com/training/community/big-iq-cloud-edition/html/class10/module2/lab1.html)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack ağ oluşturma farklılıkları ve konuları](azure-stack-network-differences.md) 