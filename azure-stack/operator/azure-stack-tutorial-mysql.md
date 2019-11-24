---
title: Azure Stack içinde yüksek oranda kullanılabilir MySQL veritabanları sunun
description: Azure Stack bir MySQL Server kaynak sağlayıcısı ana bilgisayarı ve yüksek oranda kullanılabilir MySQL veritabanları oluşturmayı öğrenin.
services: azure-stack
author: BryanLa
manager: femila
editor: ''
ms.service: azure-stack
ms.topic: article
ms.date: 10/07/2019
ms.author: bryanla
ms.reviewer: xiaofmao
ms.lastreviewed: 10/23/2018
ms.openlocfilehash: a03fbf9170e6cc1840bea62efeb33b960a25f99c
ms.sourcegitcommit: d159652f50de7875eb4be34c14866a601a045547
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72283394"
---
# <a name="offer-highly-available-mysql-databases"></a>Yüksek oranda kullanılabilir olan MySQL veritabanları sunar.

Azure Stack operatörü olarak sunucu VM 'lerini MySQL Server veritabanlarını barındıracak şekilde yapılandırabilirsiniz. Bir MySQL kümesi başarıyla oluşturulduktan ve Azure Stack tarafından yönetiliyorsa, MySQL hizmetlerine abone olan kullanıcılar yüksek oranda kullanılabilir MySQL veritabanlarını kolayca oluşturabilir.

Bu makalede [, çoğaltma kümesi ile bir MySQL](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/bitnami.mysql-cluster)oluşturmak için Azure Stack marketi öğelerinin nasıl kullanılacağı gösterilmektedir. Bu çözüm, veritabanlarını ana düğümden yapılandırılabilir sayıda çoğaltmaya çoğaltmak için birden çok VM kullanır. Oluşturulduktan sonra, küme bir Azure Stack MySQL barındırma sunucusu olarak eklenebilir ve ardından kullanıcılar yüksek oranda kullanılabilir bir MySQL veritabanı oluşturabilir.

> [!IMPORTANT]
> Çoğaltma Azure Stack Market öğesi **olan MySQL** , tüm Azure bulut aboneliği ortamları için kullanılamayabilir. Bu tutoral 'ın kalanını izlemeyi denemeden önce Market ' in aboneliğinizde kullanılabilir olduğunu doğrulayın.

Öğrenirsiniz:

> [!div class="checklist"]
> * Market öğelerinden bir MySQL Server kümesi oluşturma
> * Azure Stack MySQL barındırma sunucusu oluşturma
> * Yüksek oranda kullanılabilir bir MySQL veritabanı oluşturma

Kullanılabilir Azure Stack Market öğeleri kullanılarak üç VM MySQL sunucusu kümesi oluşturulup yapılandırılır. 

Başlamadan önce, [MySQL Server kaynak sağlayıcısının](azure-stack-mysql-resource-provider-deploy.md) başarıyla yüklendiğinden ve Azure Stack marketi 'nde aşağıdaki öğelerin kullanılabildiğinden emin olun:

> [!IMPORTANT]
> MySQL kümesini oluşturmak için aşağıdakiler gereklidir.

- [Çoğaltmaya sahip MySQL](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/bitnami.mysql-cluster). Bu, MySQL küme dağıtımı için kullanılacak Bitnamı çözüm şablonudur.
- [8 "JESSIE"](https://azuremarketplace.microsoft.com/marketplace/apps/credativ.Debian). Credavtiv tarafından sunulan Microsoft Azure için arka bağlantı noktası çekirdeki 8 "Jessıya". GNU/Linux, en popüler Linux dağıtımlarından biridir.
- [Linux 2,0 Için özel betik](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft.custom-script-linux?tab=Overview). Özel Betik uzantısı, VM Özelleştirme görevlerinizi sanal makine hazırlamanızı yürütmek için bir araçtır. Bu uzantı bir sanal makineye eklendiğinde, Azure Storage 'dan betikleri indirebilir ve bunları VM 'de çalıştırabilir. Özel Betik uzantısı görevleri, Azure PowerShell cmdlet 'leri ve Azure platformlar arası komut satırı arabirimi (xPlat CLı) kullanılarak otomatik olarak da yapılabilir.
- Linux uzantısı Için VM erişimi 1.4.7. VM erişimi uzantısı, VM 'nize erişimi yeniden kazanmak için parolayı, SSH anahtarını veya SSH yapılandırmasını sıfırlamanıza olanak sağlar. Parola veya SSH anahtarıyla yeni bir kullanıcı ekleyebilir ya da bu uzantıyı kullanarak bir kullanıcıyı silebilirsiniz. Bu uzantı, Linux VM 'lerini hedefler.

Azure Stack Market 'e öğe ekleme hakkında daha fazla bilgi için bkz. [Azure Stack Market 'e genel bakış](azure-stack-marketplace.md).

Ayrıca, dağıtıldıktan sonra, Linux VM 'lerinde oturum açmak için [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) gıbı bir SSH istemcisine ihtiyacınız vardır.

## <a name="create-a-mysql-server-cluster"></a>MySQL Server kümesi oluşturma 
MySQL sunucu kümesini, çoğaltma marketi öğesi [Ile MySQL](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/bitnami.mysql-cluster) kullanarak dağıtmak için bu bölümdeki adımları kullanın. Bu şablon, yüksek oranda kullanılabilir bir MySQL kümesinde yapılandırılan üç MySQL sunucusu örneğini dağıtır. Varsayılan olarak, aşağıdaki kaynakları oluşturur:

- Bir sanal ağ
- Bir ağ güvenlik grubu
- depolama hesabı
- Bir kullanılabilirlik kümesi
- Üç ağ arabirimi (varsayılan VM 'lerin her biri için bir tane)
- Genel bir IP adresi (birincil MySQL küme VM 'si için)
- MySQL kümesini barındırmak için üç Linux VM

1. 
   [!INCLUDE [azs-admin-portal](../includes/azs-admin-portal.md)]

2. **İşlem** >  **\+** **kaynak oluştur** ' u ve sonra da bir **çoğaltma ile MySQL**' i seçin.

   ![Özel şablon dağıtımı](media/azure-stack-tutorial-mysqlrp/1.png)

3. Temel dağıtım **bilgilerini temel bilgiler sayfasında belirtin** . Varsayılan değerleri gözden geçirin ve gerekirse değiştirin ve **Tamam**' a tıklayın.<br><br>En azından aşağıdakileri sağlayın:
   - Dağıtım adı (varsayılan olarak mymysql)
   - Uygulama kök parolası. **Özel karakter içermeyen** 12 karakterlik alfasayısal parola sağlayın
   - Uygulama veritabanı adı (varsayılan olarak bitnamı)
   - Oluşturulacak MySQL veritabanı çoğaltma VM 'lerinin sayısı (varsayılan 2 ' dir)
   - Kullanılacak aboneliği seçin
   - Kullanılacak kaynak grubunu seçin veya yeni bir tane oluşturun
   - Konumu seçin (varsayılan, ASDK için yereldir)

   [![](media/azure-stack-tutorial-mysqlrp/2-sm.PNG "Dağıtım temelleri")](media/azure-stack-tutorial-mysqlrp/2-lg.PNG#lightbox)

4. **Ortam yapılandırması** sayfasında, aşağıdaki bilgileri sağlayın ve ardından **Tamam**' a tıklayın: 
   - Secure Shell (SSH) kimlik doğrulaması için kullanılacak parola veya SSH ortak anahtarı. Parola kullanılıyorsa, harf, sayı **içermeli ve özel karakterler içerebilir**
   - VM boyutu (varsayılan değer standart D1 v2 VM)
   - GB cinsinden veri diski boyutu **Tamam 'A** tıklayın

   [![](media/azure-stack-tutorial-mysqlrp/3-sm.PNG "Ortam yapılandırması")](media/azure-stack-tutorial-mysqlrp/3-lg.PNG#lightbox)

5. Dağıtım **özetini**gözden geçirin. İsteğe bağlı olarak, özelleştirilmiş şablon ve parametreleri indirebilir ve ardından **Tamam**' a tıklayabilirsiniz.

   [![](media/azure-stack-tutorial-mysqlrp/4-sm.PNG "Özetleme")](media/azure-stack-tutorial-mysqlrp/4-lg.PNG#lightbox)

6. Dağıtımı başlatmak için **satın alma** sayfasında **Oluştur** ' a tıklayın.

   ![Satın Al](media/azure-stack-tutorial-mysqlrp/5.png)

    > [!NOTE]
    > Dağıtım yaklaşık bir saat sürer. Dağıtımın tamamlandığından ve MySQL kümesinin devam etmeden önce tamamen yapılandırıldığından emin olun. 

7. Tüm dağıtımlar başarıyla tamamlandıktan sonra, kaynak grubu öğelerini gözden geçirin ve **mysqlıp** genel IP adresi öğesini seçin. Küme için genel IP adresini ve tam FQDN 'yi kaydedin.<br><br>Bunu bir Azure Stack Işlecine sağlamanız gerekir, böylelikle bu MySQL kümesinden yararlanan bir MySQL barındırma sunucusu oluşturamazlar.


### <a name="create-a-network-security-group-rule"></a>Ağ güvenlik grubu kuralı oluşturma
Varsayılan olarak, ana bilgisayar VM 'sine MySQL için genel erişim yapılandırılmaz. MySQL kümesini bağlamak ve yönetmek için MySQL kaynak sağlayıcısının Azure Stack bir gelen ağ güvenlik grubu (NSG) kuralının oluşturulması gerekir.

1. Yönetici portalında, MySQL kümesini dağıtmada oluşturulan kaynak grubuna gidin ve ağ güvenlik grubunu seçin (**varsayılan-alt ağ-SG**):

   ![açık](media/azure-stack-tutorial-mysqlrp/6.png)

2. **Gelen güvenlik kuralları** ' nı seçin ve ardından **Ekle**' ye tıklayın.<br><br>**Hedef bağlantı noktası aralığına** **3306** girin ve isteğe bağlı olarak **ad** ve **Açıklama** alanlarına bir açıklama sağlayın. Gelen güvenlik kuralı iletişim kutusunu kapatmak için Ekle ' ye tıklayın.

   ![açık](media/azure-stack-tutorial-mysqlrp/7.png)

### <a name="configure-external-access-to-the-mysql-cluster"></a>MySQL kümesine dış erişimi yapılandırma
MySQL kümesinin bir Azure Stack MySQL Server Konağı olarak eklenebilmesi için, dış erişimin etkinleştirilmesi gerekir.

1. Bu örnek, bir SSH istemcisi [kullanarak, genel](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)IP 'ye erişebilen bir bilgisayardan birincil MySQL makinesinde oturum açın. Birincil MySQL VM adı genellikle **0** ile biter ve kendisine atanmış BIR genel IP 'ye sahiptir.<br><br>Genel IP 'yi kullanın ve **bitnamı** Kullanıcı adı ve daha önce özel karakterler olmadan oluşturduğunuz uygulama PAROLASı ile VM 'de oturum açın.

   ![LinuxLogin](media/azure-stack-tutorial-mysqlrp/bitnami1.png)


2. SSH istemcisi penceresinde, bitnamı hizmetinin etkin ve çalışır durumda olduğundan emin olmak için aşağıdaki komutu kullanın. İstendiğinde bitnamı parolasını yeniden girin:

   `sudo service bitnami status`

   ![Hizmeti denetle](media/azure-stack-tutorial-mysqlrp/bitnami2.png)

3. MySQL 'e bağlanmak ve ardından SSH istemcisinden çıkmak için Azure Stack MySQL barındırma sunucusu tarafından kullanılacak bir uzaktan erişim kullanıcı hesabı oluşturun.<br><br>Daha önce oluşturulan kök parolayı kullanarak MySQL içinde kök olarak oturum açmak için aşağıdaki komutları çalıştırın ve yeni bir yönetici kullanıcı oluşturun, *\<Kullanıcı adı\>* ve *\<Password\>* ortamınız için gerektiği gibi değiştirin. Bu örnekte, oluşturulacak Kullanıcı **SQLSA** olarak adlandırılır ve güçlü bir parola kullanılır:

   ```mysql
   mysql -u root -p
   create user <username>@'%' identified by '<password>';
   grant all privileges on *.* to <username>@'%' with grant option;
   flush privileges;
   ```
   ![Yönetici Kullanıcı Oluştur](media/azure-stack-tutorial-mysqlrp/bitnami3.png)


4. Yeni MySQL Kullanıcı bilgilerini kaydedin.<br><br>Bu Kullanıcı adını ve parolayı, küme için genel IP adresi veya genel IP 'nin tam FQDN 'SI ile birlikte sağlamanız gerekir. bu sayede, bu MySQL kümesini kullanarak bir MySQL barındırma sunucusu oluşturabilmeleri için Azure Stack Işlecine sahip olursunuz.


## <a name="create-an-azure-stack-mysql-hosting-server"></a>Azure Stack MySQL barındırma sunucusu oluşturma
MySQL Server kümesi oluşturulduktan ve düzgün şekilde yapılandırıldıktan sonra, kullanıcıların veritabanı oluşturmalarına ek kapasite sağlamak için bir Azure Stack Işlecinin Azure Stack MySQL barındırma sunucusu oluşturması gerekir. 

MySQL kümesinin kaynak grubu oluşturulduğunda (**mysqlıp**), daha önce kaydedilen MySQL KÜMESI Birincil VM 'nın genel IP 'si IÇIN genel IP veya tam FQDN 'yi kullandığınızdan emin olun. Ayrıca, işlecin MySQL küme veritabanına uzaktan erişmek için oluşturduğunuz MySQL Server kimlik doğrulama kimlik bilgilerini bilmeleri gerekir.

> [!NOTE]
> Bu adımın Azure Stack yönetim portalından bir Azure Stack Işleci tarafından çalıştırılması gerekir.

MySQL kümesinin genel IP ve MySQL kimlik doğrulaması oturum açma bilgilerini kullanarak, bir Azure Stack Işleci artık [Yeni MySQL kümesini kullanarak bir MySQL barındırma sunucusu oluşturabilir](azure-stack-mysql-resource-provider-hosting-servers.md#connect-to-a-mysql-hosting-server). 

Ayrıca, MySQL veritabanı oluşturmayı kullanıcılar için kullanılabilir hale getirmek için planlar ve teklifler oluşturduğunuzdan emin olun. Bir işlecin **Microsoft. MySqlAdapter** hizmetini bir plana eklemesi ve yüksek oranda kullanılabilir veritabanları için özel olarak yeni bir kota oluşturması gerekir. Plan oluşturma hakkında daha fazla bilgi için bkz. [hizmet, plan, teklif, aboneliğe genel bakış](service-plan-offer-subscription-overview.md).

> [!TIP]
> **Microsoft. MySqlAdapter** hizmeti, [MySQL Server kaynak sağlayıcısı dağıtılana](azure-stack-mysql-resource-provider-deploy.md)kadar planlara eklemek için kullanılamaz.



## <a name="create-a-highly-available-mysql-database"></a>Yüksek oranda kullanılabilir bir MySQL veritabanı oluşturma
MySQL kümesi oluşturulduktan, yapılandırıldıktan ve bir Azure Stack Işleci tarafından Azure Stack MySQL barındırma sunucusu olarak eklendikten sonra, MySQL Server veritabanı özellikleri dahil olmak üzere aboneliği olan bir Kiracı Kullanıcı, Bu bölümdeki adımları takip edin. 

> [!NOTE]
> Bu adımları Azure Stack Kullanıcı portalından, MySQL Server özellikleri (Microsoft. MySQLAdapter hizmeti) sağlayan abonelikle Kiracı Kullanıcı olarak çalıştırın.

1. 
   [!INCLUDE [azs-user-portal](../includes/azs-user-portal.md)]

2.  > **veri \+ depolama**ve ardından **MySQL veritabanı** **\+** **kaynak oluştur** ' u seçin.<br><br>Ad, harmanlama, kullanılacak abonelik ve dağıtım için kullanılacak konum dahil olmak üzere gerekli veritabanı özellik bilgilerini sağlayın. 

   ![MySQL veritabanı oluştur](./media/azure-stack-tutorial-mysqlrp/createdb1.png)

3. **SKU** ' yı seçin ve ardından kullanılacak uygun MySQL BARıNDıRMA sunucusu SKU 'sunu seçin. Bu örnekte Azure Stack Işleci, MySQL kümesi veritabanları için yüksek kullanılabilirliği desteklemek üzere **MySQL-ha** SKU 'su oluşturmuştur.

   ![SKU seçin](./media/azure-stack-tutorial-mysqlrp/createdb2.png)

4. **Oturum aç** ' ı seçin > **Yeni bir oturum oluşturun** ve yeni veritabanı için kullanılacak MySQL kimlik doğrulama kimlik bilgilerini sağlayın. İşiniz bittiğinde, **Tamam** ' a tıklayın ve ardından **Oluştur** ' a tıklayarak veritabanı dağıtım sürecini başlatın.

   ![Oturum açma Ekle](./media/azure-stack-tutorial-mysqlrp/createdb3.png)

5. MySQL veritabanı dağıtımı başarıyla tamamlandığında, yeni yüksek oranda kullanılabilir veritabanına bağlanırken kullanılacak bağlantı dizesini saptamak için veritabanı özelliklerini gözden geçirin. 

   ![Bağlantı dizesini görüntüle](./media/azure-stack-tutorial-mysqlrp/createdb4.png)

## <a name="next-steps"></a>Sonraki adımlar

[MySQL kaynak sağlayıcısını güncelleştirme](azure-stack-mysql-resource-provider-update.md)
