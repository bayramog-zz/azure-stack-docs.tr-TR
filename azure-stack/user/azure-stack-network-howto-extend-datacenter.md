---
title: Azure Stack hub 'ında veri merkezini genişletme | Microsoft Docs
description: Azure Stack veri merkezini genişletmeyi öğrenin.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: how-to
ms.date: 11/07/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 11/07/2019
ms.openlocfilehash: 92e82f549cddf51b1cbd6764cc122acc3ca8fdfc
ms.sourcegitcommit: ed44d477b9fd11573d1e0d1ed3a3c0ef4512df53
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2019
ms.locfileid: "73845910"
---
# <a name="how-to-extend-the-data-center-on--azure-stack-hub"></a>Azure Stack hub 'da veri merkezini genişletme

*Uygulama hedefi: Azure Stack hub tümleşik sistemleri ve Azure Stack hub Development Kit*

Bu makalede, Azure Stack mevcut ağ ortamınıza nasıl tümleştirileceğine karar vermenize yardımcı olmak üzere Azure Stack hub depolama altyapısı bilgileri sağlanmaktadır. Veri merkezinizi genişletmekle genel bir tartışma sağlamaktan sonra, makalede iki farklı senaryo sunulmaktadır. Windows dosya depolama sunucusuna bağlanabilirsiniz. Ayrıca, bir Windows Iscsı sunucusuna da bağlanabilirsiniz.

## <a name="overview-of-extending-storage-to-azure-stack-hub"></a>Depolamayı Azure Stack hub 'a genişletmeye genel bakış

Verilerinizin genel bulutta yer aldığı senaryolar yeterli değildir. Belki de yoğun işlem gücü olan sanallaştırılmış bir veritabanı iş yükünüz var, gecikme süresine duyarlıdır ve genel buluta gidiş dönüş süresi veritabanı iş yükünün performansını etkileyebilir. Şirket içinde, şirket içi iş yükleri tarafından erişilmesi gereken bir dosya sunucusu, NAS veya Iscsı depolama dizisinde tutulan ve şirket içinde bulunan ve yasal düzenlemeler ya da uyumluluk hedeflerini karşılamak için şirket içinde bulunması gereken veriler olabilir.  Bunlar, verilerin şirket içinde bulunduğu senaryolardan yalnızca iki ikisi de birçok kuruluş için önemli olmaya devam etmektedir.

Bu nedenle, bu verileri neden Azure Stack veya Azure Stack sisteminde çalışan sanallaştırılmış dosya sunucuları içinde depolama hesaplarında barındırmamsınız? Azure 'dan farklı olarak Azure Stack depolama sınırlı değildir. Kullanımınız için kullanabileceğiniz kapasite, sahip olduğunuz düğüm sayısına ek olarak, satın almayı seçtiğiniz düğüm başına kapasiteye göre değişir. Azure Stack hiper yakınsanmış bir çözüm olduğundan, kullanım taleplerini karşılamak için depolama kapasitenizi büyütmek istemeniz gerekir, Ayrıca, düğümlerin eklenmesi sırasında işlem parmak izinizi de büyütmeniz gerekir.  Bu durum, özellikle ek kapasiteye ihtiyaç duyduğunuzda, Azure Stack sisteminin dışında düşük maliyetli için eklenebilecek arşiv depolama alanı olması durumunda potansiyel olarak maliyetli olabilir.

Bu, sizi aşağıda yer alacak senaryoya getirir. Ağ üzerinden erişilebilen Azure Stack sistemlerini, Azure Stack çalıştıran sanallaştırılmış iş yüklerini Azure Stack dışındaki depolama sistemlerine nasıl bağlayabileceksiniz.

## <a name="design-for-extending-storage"></a>Depolama alanı genişletme tasarımı

Diyagram, tek bir sanal makinenin, bir iş yükü çalıştıran, veri okuma/yazma amaçları doğrultusunda dış (VM 'ye ve Azure Stack kendisi) depolama alanı kullanır bir senaryoyu gösterir. Bu makalede, dosyaların basit alımı üzerine odaklanırsınız, ancak bu örneği, veritabanı dosyalarının uzak depolama birimi gibi daha karmaşık senaryolar için genişletebilirsiniz.

![Depolamayı Azure Stack uzat](./media/azure-stack-network-howto-extend-datacenter/image1.png)

Diyagramda, Azure Stack sistemindeki VM 'nin birden çok NIC ile dağıtıldığını görürsünüz. Her iki yedekten de depolama en iyi yöntemi olarak, hedef ve hedef arasında birden çok yol olması önemlidir. Her şey daha karmaşık hale geldiğinde, Azure Stack VM 'Lerin Azure 'da olduğu gibi hem genel hem de özel IP 'Leri vardır. Dış depolamanın VM 'ye ulaşması gerekiyorsa, özel IP 'Ler birincil olarak Azure Stack sistemlerde, sanal ağlar ve alt ağlarda kullanıldığı için yalnızca genel IP aracılığıyla bunu yapabilir. Dış depolama, vNet 'e bağlanmak için siteden siteye VPN üzerinden geçmedikleri müddetçe VM 'nin özel IP alanı ile iletişim kuramaz. Bu nedenle, bu örnekte genel IP alanı üzerinden iletişime odaklanırsınız. Grafik içindeki genel IP alanı ile ilgili dikkat edilecek bir şey, iki farklı genel IP havuzu alt ağı olmasıdır. Varsayılan olarak, Azure Stack genel IP adresi için yalnızca bir havuz gerektirir, ancak Artıklı yönlendirme için bir saniye eklemek gerekebilir. Ancak, belirli bir havuzdan bir IP adresi seçmek mümkün değildir, bu nedenle birden çok sanal ağ kartında aynı havuzdan ortak IP 'Ler içeren VM 'lerle karşılaşabilirsiniz.

Bu örnekte, sınır cihazları ve harici depolama arasındaki yönlendirmenin ele alındığını ve trafiğin ağı uygun şekilde geçebileceği varsayabilirsiniz. Bu örnekte, omurga 'in 1 GbE, 10 GbE, 25 GbE veya daha hızlı olması önemli değildir, ancak bu dış depolamaya erişen uygulamaların performans ihtiyaçlarını karşılamak için, tümleştirmenizi planlarken göz önünde bulundurmanız önemlidir.

## <a name="connect-to-a-windows-server-file-server-storage"></a>Windows Server dosya sunucusu depolamasına bağlanma

Bu senaryoda, Azure Stack Windows Server 2019 sanal makinesini dağıtıp yapılandırın ve Windows Server 2019 ' i de çalıştıran bir dış dosya sunucusuna bağlanmaya hazırlayın. Uygun olduğunda, VM ile dış depolama arasındaki performansı ve bağlantıyı iyileştirmek için çok kanallı SMB gibi önemli özellikleri ayarlayın.

### <a name="deploy-the-windows-server-2019-vm-on-azure-stack"></a>Azure Stack 'de Windows Server 2019 VM dağıtma

1.  **Azure Stack yönetici portalından**, bu sistemin doğru şekilde kaydedildiğini ve Market 'e bağlı olduğunu varsayarsak, **Market yönetimi** ' ni seçin, daha önce bir Windows Server 2019 yansımanıza **sahip olmadığınız varsayılarak, Azure 'dan ekleyin** ve Windows server **2019 Datacenter** görüntüsünü ekleyerek **Windows Server 2019**' i arayın.

    ![](./media/azure-stack-network-howto-extend-datacenter/image2.png)

    Windows Server 2019 görüntüsünü indirmek biraz zaman alabilir.

2.  Azure Stack ortamınızda Windows Server 2019 görüntüsü olduktan sonra, Azure Stack Kullanıcı Portalı 'nda oturum açın * *.

3.  Azure Stack Kullanıcı portalında oturum açtıktan sonra, bir [teklif aboneliğine](https://docs.microsoft.com/azure-stack/operator/azure-stack-subscribe-plan-provision-vm?view=azs-1908)sahip olduğunuzdan emin olun. Bu, IaaS kaynaklarını (Işlem, depolama ve ağ) sağlamanıza olanak tanır.

4.  Kullanılabilir bir aboneliğiniz varsa, **Pano 'yu** Azure Stack Kullanıcı portalında yeniden **oluşturun, kaynak oluştur**' u seçin, **işlem** ' i seçin ve ardından **Windows Server 2019 Datacenter Galeri öğesini**seçin.

5.  **Temel bilgiler** dikey penceresinde:

    a.  **Ad**: VM001

    b.  **Kullanıcı adı**: YerelYönetici

    c.  **Parola ve parolayı** **onaylayın**: tercih ettiğiniz > parolayı \<

    d.  **Abonelik**: işlem/depolama/ağ kaynakları ile tercih ettiğiniz aboneliği \<>

    e. **Kaynak grubu**: Yeni oluştur: storagetesting

    f.  Ardından **Tamam 'ı** seçin

6.  **Boyut seç** dikey penceresinde bir **Standard_F8s_v2**seçin.

7.  **Ayarlar** dikey penceresinde **sanal ağı** seçin ve **sanal ağ oluştur** dikey penceresinde Adres alanını **10.10.10.0/23** olacak şekilde ayarlayın ve alt ağ adres aralığını **10.10.10.0/24** olacak şekilde güncelleştirin ve sonra **Tamam**' ı seçin.

8.  **Genel IP adresini**seçin ve **genel IP adresi oluştur** dikey penceresinde **statik**' ı seçin.

9.  **Ortak gelen bağlantı noktalarından Seç**' den **RDP (3389)** seçeneğini belirleyin.

10. Diğer varsayılanları bırakın ve **Tamam**' ı seçin.
    
    ![](./media/azure-stack-network-howto-extend-datacenter/image3.png)

11. Özeti okuyun, doğrulama için bekleyin, sonra dağıtımı başlatmak için **Tamam** ' ı seçin. Dağıtım yaklaşık 10 dakika içinde tamamlanmalıdır.

12. Dağıtım tamamlandıktan sonra, **kaynak** ' ın altında, *genel bakış* dikey penceresine gitmek için **VM001** sanal makine adını seçin.
    
    ![](./media/azure-stack-network-howto-extend-datacenter/image4.png)

13. DNS adı altında **Yapılandır** ' ı seçin ve bir DNS ad etiketi girin, * * Vm001 ve **Kaydet**' i seçin ve ardından *genel bakış* dikey penceresine dönmek için içerik haritasında **vm001** ' yi seçin.

14. Genel Bakış dikey penceresinin sağ tarafında, sanal ağ/alt ağ metni altında **storagetesting-VNET/varsayılan** ' i seçin.

15. Storagetesting-VNET dikey penceresinde **alt ağlar** ' ı ve ardından **alt ağ**' ı seçin, ardından yeni alt ağ ekle dikey penceresinde aşağıdaki bilgileri girin: 

    a.  **Ad**: subnet2

    b.  **Adres aralığı (CIDR bloğu)** : 10.10.11.0/24

    c. **Ağ güvenlik grubu**: yok

    d.  **Yol tablosu**: yok

    e. **Tamam ' ı**seçin:

16. Kaydedildikten sonra, genel bakış dikey penceresine geri dönmek için, içerik haritasında **VM001** öğesini seçin.

17. **Ağ**' ı seçin.

18. **Ağ arabirimi Ekle** ' yi ve ardından **ağ arabirimi oluştur**' u seçin.

19. **Ağ arabirimi oluştur** dikey penceresinde:

    a.  **Ad**: vm001nic2

    b.  **Alt ağ**: alt ağın 10.10.11.0/24 olduğundan emin olun

    c. **Ağ güvenlik grubu**: VM001-NSG

    d.  **Kaynak grubu**: storagetesting

20. Başarılı bir şekilde oluşturulduktan sonra, içerik haritasında **VM001** ' ı seçin ve VM 'yi kapatmak için **Durdur** ' u seçin.

21. VM durdurulduğunda (serbest bırakıldığında) **ağ**' ı seçin, **ağ arabirimi Ekle** ' yi seçin ve ardından **Vm001nic2**' ı seçin ve **Tamam**' ı seçin. Birkaç dakika içinde VM 'ye Ek NIC ekleyeceksiniz.

22. **Ağ** dikey penceresinde **vm001nic2** sekmesini seçin ve ardından **ağ arabirimi: vm001nic2**' nı seçin.

23. Vm001nic arabirimi dikey penceresinde **IP yapılandırması**' nı seçin ve dikey pencerenin merkezinde **ipconfig1**' yi seçin.

24. İpconfig1 ayarları dikey penceresinde, genel IP adresi için **etkin** ' i seçin ve **gerekli ayarları Yapılandır**, **Yeni oluştur** ' u seçin ve ad için vm001nic2pip girin, **statik** **' i seçin ve ardından** **Kaydet**' i seçin.

25. Başarıyla kaydedildikten sonra, VM001 genel bakış dikey penceresine geri dönün ve yapılandırılmış Windows Server 2019 VM 'nizi başlatmak için **Başlat** ' ı seçin.

### <a name="configure-the-windows-server-2019-file-server-storage"></a>Windows Server 2019 dosya sunucusu depolamayı yapılandırma

Bu ilk örnekte, Windows Server 2019 dosya sunucusu 'nun Hyper-V üzerinde çalışan bir sanal makine olduğu bir yapılandırmayı doğruluyoruz. Bu sanal makine, sekiz sanal işlemci, tek bir VHDX dosyası ve en önemlisi iki sanal ağ bağdaştırıcısı ile yapılandırılır. İdeal bir senaryoda, bu ağ bağdaştırıcılarının farklı yönlendirilebilir alt ağları olacaktır, ancak bu testte aynı alt ağda ağ bağdaştırıcıları olacaktır.

![](./media/azure-stack-network-howto-extend-datacenter/image5.png)

Dosya sunucunuz için, Hyper-V, VMware veya tercih ettiğiniz alternatif bir platformda çalışan Windows Server 2016 veya 2019, fiziksel veya sanal olabilir. Burada anahtar odak, Azure Stack sistemine bağlantı sağlar, ancak kaynak ile hedef arasında birden çok yol olması tercihen daha fazla artıklık sağladığından, daha gelişmiş yetenekler kullanılmasına olanak tanır. çok kanallı SMB gibi performans.

Dosya paylaşımını yapılandırmaya devam etmeden önce, dosya sunucunuzu en son toplu güncelleştirmeler ve düzeltmelerle güncelleştirin, yeniden başlatılıyor.

Güncelleştirildikten ve yeniden başlatıldıktan sonra, artık bu sunucuyu bir dosya sunucusu olarak yapılandırabilirsiniz.

1) Dosya sunucusu makinenizde, **cmd** **/All komutunu** çalıştırın ve dosya sunucunuzun kullandığı **DNS sunucusunu** bir yere göz önünde yapın.

2)  **Sunucu Yöneticisi** açın ve **Yönet**' i seçip **rol ve Özellik Ekle**' yi seçin.

3)  **İleri**' yi seçin, **rol tabanlı veya özellik tabanlı yükleme**' yi seçin ve **sunucu rollerini Seç** sayfasına ulaşana kadar seçimleri ilerletin.

4)  **Dosya ve depolama hizmetleri**' ni genişletip **Dosya & iSCSI Hizmetleri** ' ni genişletin ve **dosya sunucusu**' nu seçin. Tamamlandıktan sonra **Sunucu Yöneticisi**kapatın.

5)  **Sunucu Yöneticisi** açın ve **dosya ve depolama hizmetleri**' ni seçin.

6)  **Paylaşımlar**' ı seçin.

7)  **Paylaşımlar kutusunda**, **dosya paylaşımı oluşturmayı seçin, Yeni Paylaşım Sihirbazı bağlantısını başlatın** .

8)  **Yeni paylaşma Sihirbazı** kutusunda, **SMB paylaşma – hızlı**' yı seçin, ardından **İleri**' yi seçin ve sihirbazda Ilerleyin ve **C:\\\\ birimini**seçin.

9)  Paylaşıma bir ad verin, **Teststorage** daha sonra **İleri**' yi seçin.

10) **Yeni paylaşma sihirbazına**dönün, **İleri** ' yi ve ardından **Oluştur**' u ve ardından **Kapat**' ı seçin.

Dosya sunucunuzda dosya paylaşımınızı oluşturdunuz.

### <a name="testing-file-storage-performance-and-connectivity"></a>Dosya depolama performansını ve bağlantısını test etme

İletişimi denetlemek ve bazı ilkel testlerini çalıştırmak için, **Azure Stack** sisteminizdeki Azure Stack Kullanıcı portalında oturum açın ve **VM001**için **genel bakış** dikey penceresine gidin.

1)  VM001 ' e bir RDP bağlantısı kurmak için **Bağlan** ' ı seçin.

2)  Ana bilgisayar adı üzerinden iletişim kurmak için **Hosts** dosyasını düzenlersiniz; Ancak, bir üretim ortamında DNS, adlar otomatik olarak çözülecek şekilde en iyi şekilde yapılandırılır. IP adreslerini de kullanabilirsiniz. Bizim örneğimizde, **Hosts** dosyasını düzenlersiniz.

3)  **Yönetici olarak**çalıştırdığınızdan emin olmak Için **Not defteri**'ni açın.

4)  Not defteri 'ni açtıktan sonra **Dosya**' yı seçin **ve c** :\\\, Windows\System32\Drivers\etc ' a gidin ve Aç iletişim kutusunun sağ alt köşesindeki **tüm dosyalar** ' ı seçin. **Konaklar** dosyasını seçin ve **Aç**' ı seçin.

5)  Dosya sunucunuz için bir IP adresi ve DNS adı ekleyerek ana bilgisayar dosyanızı düzenleyin.

    ![](./media/azure-stack-network-howto-extend-datacenter/image6.png)

6)  Dosyayı kaydedin ve Not defteri 'Ni kapatın.

7)  **Cmd** 'yi açın ve Hosts dosyasına girdiğiniz dosya sunucusunun adına ping gönderin. Bu, dosya sunucusundaki ağ bağdaştırıcılarından birinin IP adresini döndürmelidir, bu nedenle IP adresini ping yaparak diğer bağdaştırıcıya olan iletişimi el ile test edin.

## <a name="connect-to-a-windows-server-iscsi-target-server"></a>Windows Server Iscsı hedef sunucusuna bağlanma

Bu senaryoda, Azure Stack bir Windows Server 2019 sanal makinesini dağıtıp yapılandıracaksınız ve bunu, Windows Server 2019 ' i de çalıştıran bir dış Iscsı hedef sunucusuna bağlanacak şekilde hazırlarsınız.

> [!Note]  
> Bir senaryoyu zaten tamamladıysanız makinelerinizi özelleştirmeye devam edin.

### <a name="deploy-the-windows-server-2019-vm-on-azure-stack"></a>Azure Stack 'de Windows Server 2019 VM dağıtma

Windows Server 2019 VM 'yi zaten Azure Stack dağıttıysanız, [Azure Stack üzerinde Windows server 2019 VM dağıtma](#deploy-the-windows-server-2019-vm-on-azure-stack)bölümündeki adımları uygulayın. Daha sonra Windows Server 2019 Iscsı hedefini yapılandırabilirsiniz.

### <a name="configure-the-windows-server-2019-iscsi-target"></a>Windows Server 2019 Iscsı hedefini yapılandırma

Bu ikinci senaryonun amacı doğrultusunda, Windows Server 2019 Iscsı hedefinin Hyper-V üzerinde çalışan bir sanal makine olduğu bir yapılandırmayı doğrulayın. Bu sanal makine, sekiz sanal işlemci, tek bir VHDX dosyası ve en önemlisi iki sanal ağ bağdaştırıcısı ile yapılandırılır. İdeal bir senaryoda, bu ağ bağdaştırıcılarının farklı yönlendirilebilir alt ağları olacaktır, ancak bu testte aynı alt ağ üzerinde ağ bağdaştırıcılarınız olacaktır.

![](./media/azure-stack-network-howto-extend-datacenter/image5.png)

Iscsı hedefiniz için, Hyper-V, VMware veya seçtiğiniz alternatif bir platformda çalışan Windows Server 2016 veya 2019, fiziksel veya sanal olabilir. Burada anahtar odak, Azure Stack sistemine bağlantı sağlar, ancak kaynak ile hedef arasında birden çok yol olması tercihen, daha fazla artıklık ve verimlilik sağladığından tercihen tercihen.

Dosya sunucunuzu en son toplu güncelleştirmeler ve düzeltmelerle güncelleştirin, Iscsı hedef sunucusu yapılandırmasına devam etmeden önce yeniden başlatma yapın.

Güncelleştirildikten ve yeniden başlatıldıktan sonra, artık bu sunucuyu bir Iscsı hedef sunucusu olarak yapılandırabilirsiniz.

1. **Sunucu Yöneticisi** açın > **yönetme** > **rol ve özellik ekleme**.

2. Açıldıktan sonra, **İleri**' yi seçin, **rol tabanlı veya özellik tabanlı yükleme**' yi seçin ve **sunucu rollerini Seç** sayfasına ulaşana kadar seçimleri ilerleyin.

3. **Dosya ve depolama hizmetleri**' ni genişletin, **Dosya & iSCSI Hizmetleri** ' ni genişletin ve **iSCSI hedef sunucusu**' nu seçin, yeni özellikler eklemek için istemleri kabul edin ve ardından tamamlamaya ilerleyin.

    ![](./media/azure-stack-network-howto-extend-datacenter/image8.png)
    
    Tamamlandıktan sonra **Sunucu Yöneticisi kapatın.**

4. **Dosya Gezgini 'ni açın,** `C:\\` gidin ve `iSCSI`adlı **Yeni bir klasör oluşturun**.

5. **Sunucu Yöneticisi** > **dosya ve depolama hizmetleri** 'ni sol taraftaki menüden yeniden açın.

6. **İSCSI** ' yi seçin ve "**iSCSI sanal diski oluşturmak Için yeni Iscsı sanal disk Sihirbazı 'nı başlatın**.

7. **İSCSI sanal disk konumunu seçin** sayfasında, **özel yol yaz** ' ı seçin ve `C:\\iSCSI`gidin. **İleri**’yi seçin.

8. Iscsı sanal diskine `iSCSIdisk1` adı ve isteğe bağlı olarak bir açıklama verin ve ardından **İleri**' yi seçin.

9. Sanal diskin boyutunu `10GB` olarak ayarlayın ve **sabit boyut** ' u seçin ve Ileri ' **yi**seçin.

    ![](./media/azure-stack-network-howto-extend-datacenter/image9.png)

10. Bu yeni bir hedef olduğundan, **Yeni iSCSI hedefi** ' ni seçin ve **İleri**' yi seçin.

11. **Hedef adı belirtin** sayfasında, **TARGET1** girin ve **İleri**' yi seçin.

12. **Erişim sunucularını belirtin** sayfasında **Ekle**' yi seçin. Bu, Iscsı hedefine bağlanma yetkisine sahip olacak belirli **başlatıcıları** girmek için bir iletişim kutusu açar.

13. **BAŞLATıCı kimliği Ekle penceresinde** **Seçilen tür Için bir değer girin** ' i seçin ve **tür** ' ın altına açılan menüde IQN ' nin seçili olduğundan emin olun. **VM001** **bilgisayar adı** olan `<computername>` `iqn.1991-05.com.microsoft:<computername>` yazın. **İleri**’yi seçin.

    ![](./media/azure-stack-network-howto-extend-datacenter/image10.png)

14. **Kimlik doğrulamasını etkinleştir** sayfasında, kutuları boş bırakın ve **İleri**' yi seçin.

15. Seçimlerinizi onaylayın ve **Oluştur**' u ve ardından Kapat ' ı seçin.

    ![](./media/azure-stack-network-howto-extend-datacenter/image11.png)

Iscsı sanal diskinizin Sunucu Yöneticisi ' de oluşturulduğunu görmeniz gerekir.

### <a name="configure-the-windows-server-2019-iscsi-initiator-and-mpio"></a>Windows Server 2019 Iscsı Başlatıcısı ve MPIO 'YU yapılandırma

Iscsı Başlatıcısı 'nı ayarlamak için **Azure Stack** sisteminizde **Kullanıcı portalında** oturum açın ve **VM001**için **genel bakış** dikey penceresine gidin.

1.  VM001 için bir RDP bağlantısı kurun. Bağlandıktan sonra **Sunucu Yöneticisi**açın.

2.  **Rol ve Özellik Ekle**' yi seçin ve **Özellikler** sayfasına ulaşana kadar Varsayılanları kabul edin.

3.  **Özellikler** sayfasında **çok yollu g/ç** ekleyin ve **İleri ' yi**seçin.

    ![](./media/azure-stack-network-howto-extend-datacenter/image12.png)

4.  Onay kutusu **gerekirse hedef sunucuyu otomatik olarak yeniden başlatın** ve sonra da Kapat **' ı**seçin **.** Yeniden başlatma büyük olasılıkla gerekli olacaktır, bu nedenle tamamlandığında, VM001 adresine yeniden bağlanın.

5.  **Sunucu Yöneticisi**dönün, **MPIO yüklemesinin tamamlanmasını**bekleyin, **Kapat**' ı seçin ve ardından **Araçlar** ' ı seçin ve **MPIO**' u seçin.

6.  **Birden çok yol bul** sekmesini seçin ve **Ekle** > **iSCSI cihazları için destek eklemek** üzere kutuyu Işaretleyin ve sonra **yeniden başlatmak** için **Evet** ' i seçin VM001. **Tamam '** ı seçin ve ardından el ile yeniden başlatın.

    ![](./media/azure-stack-network-howto-extend-datacenter/image13.png)

1.  Yeniden başlatıldıktan sonra, **VM001 ile yeni BIR RDP bağlantısı**kurun.

2.  Bağlandıktan sonra **Sunucu Yöneticisi**açın, **Iscsı Başlatıcısı** > **Araçlar** ' ı seçin.

3.  Iscsı hizmetinin varsayılan olarak çalışmasına izin vermek için Microsoft Iscsı açıldığında **Evet** ' i seçin.

    ![](./media/azure-stack-network-howto-extend-datacenter/image17.png)

4.  **Bulma** sekmesini seçin.

5.  **Portalı bul** düğmesini seçin. Şimdi iki hedef ekleyeceksiniz.

6.  Iscsı hedef sunucunuzun ilk IP adresini girin ve **Gelişmiş**' i seçin.

    ![](./media/azure-stack-network-howto-extend-datacenter/image15.png)

7. **Gelişmiş ayarlar**için aşağıdakileri seçin:

    - Yerel bağdaştırıcı: Microsoft Iscsı Başlatıcısı

    - Başlatıcı IP 'si: 10.10.10.4

    - Ayarlandığında **Tamam**' ı seçin.

8.  **Hedef portalı bul**' da **Tamam ' ı** seçin.

9.  İşlemi aşağıdaki gibi yineleyin:

    - IP adresi: ikinci Iscsı hedef IP adresiniz

    - Yerel bağdaştırıcı: Microsoft Iscsı Başlatıcısı

    - Başlatıcı IP 'si: 10.10.11.4

10. Hedef portallarınız, **Adres** sütunu altında kendi Iscsı hedef IP 'larınız ile şöyle görünmelidir.

    ![](./media/azure-stack-network-howto-extend-datacenter/image16.png)

11. **Hedefler** sekmesini seçin ve ardından iSCSI hedefini seçin. **Bağlan**’ı seçin.

12. **Hedefe Bağlan**' da **Çoklu yolu etkinleştir** ' i seçin ve **Gelişmiş**' i seçin.

    ![](./media/azure-stack-network-howto-extend-datacenter/image17.png)

13. **Hedefe Bağlan**için aşağıdaki bilgileri girin:

    - Yerel bağdaştırıcı: Microsoft Iscsı Başlatıcısı

    - Başlatıcı IP 'si: 10.10.10.4

    - Hedef Portal IP 'si: ilk Iscsı hedefi IP/3260 \<>

    - **Tamam**’ı seçin.

    ![](./media/azure-stack-network-howto-extend-datacenter/image18.png)

1.  İkinci Başlatıcı/hedef birleşimi için işlemi tekrarlayın.

    - Yerel bağdaştırıcı: Microsoft Iscsı Başlatıcısı

    - Başlatıcı IP 'si: 10.10.11.4

    - Hedef Portal IP 'si: ikinci Iscsı hedef IP 'si/3260 > \<

    ![](./media/azure-stack-network-howto-extend-datacenter/image19.png)

1.  **Birimler ve aygıtlar** sekmesini seçin ve ardından **Otomatik Yapılandır** ' ı SEÇIN. sunulan bir MPIO birimi görmeniz gerekir:

    ![](./media/azure-stack-network-howto-extend-datacenter/image20.png)

2.  **Hedefler** sekmesine dönün, **cihazlar** ' ı seçin ve daha önce oluşturduğunuz tek iSCSI VHD 'sine yönelik iki bağlantı görmeniz gerekir.

    ![](./media/azure-stack-network-howto-extend-datacenter/image20.png)

3.  Yük Dengeleme İlkesi ve yolları hakkında daha fazla bilgi görmek için **MPIO düğmesini** seçin.

    ![](./media/azure-stack-network-howto-extend-datacenter/image21.png)

4.  Windows ve Iscsı başlatıcısından çıkmak için **Tamam ' ı** üç kez seçin.

5.  Disk Yönetimi 'ni (Diskmgmt. msc) açın ve bir **disk başlatma** açılır penceresi istenir.

    ![](./media/azure-stack-network-howto-extend-datacenter/image22.png)

6.  Varsayılanları kabul etmek için **Tamam** ' ı seçin, ardından yeni diske kaydırın, sağ tıklayın ve **Yeni basit birim**' i seçin.

7.  Sihirbazı gözden geçir, Varsayılanları kabul edin. Birim etiketini **iSCSIdisk1** olarak değiştirip **son**' u seçin.

    ![](./media/azure-stack-network-howto-extend-datacenter/image23.png)

8.  Sürücü daha sonra biçimlendirilmelidir ve bir sürücü harfiyle sunulacak.

9.  VM001 'e eklenen yeni sürücünüzü görmek için **Bu bilgisayarı** > **Dosya Gezgini** 'ni açın.

### <a name="test-external-storage-connectivity"></a>Dış depolama bağlantısını test et

İletişimi doğrulamak ve bir ilkel dosya kopyası testi çalıştırmak için, **Azure Stack** sisteminizde **Kullanıcı portalında** oturum açın ve **VM001**için **genel bakış** dikey penceresine gidin.

1. **VM001**' e bir RDP bağlantısı kurmak için **Bağlan** ' ı seçin.

2. **Görev Yöneticisi 'ni** açın ve ardından **performans** SEKMESINI seçin. pencereyi RDP oturumunun sağ tarafına yasla.

3. **Windows PowerShell ISE** yönetici olarak açın ve RDP oturumunun sol tarafına yaslayabilirsiniz. ISE 'nin sağ tarafında, **Komutlar** bölmesini kapatın ve Ise penceresinin en üstündeki beyaz betik bölmesini genişletmek Için **komut dosyası** düğmesini seçin.

4. Bu VM 'de, Iscsı hedefine dosya aktarımını test etmek için büyük bir dosya olarak kullanacağınız bir VHD oluşturmak için yerel bir PowerShell modülü yoktur. Bu durumda, bir VHD dosyası oluşturmak için DiskPart 'ı çalıştıracaksınız. ISE 'de aşağıdakileri çalıştırın:

    1. `Start-Process Diskpart`

    2. Yeni bir komut istemi açılır. Aşağıdaki cmd 'yi girin:<br>`Create vdisk file="c:\\test.vhd" type=fixed maximum=5120`

    ![](./media/azure-stack-network-howto-extend-datacenter/image24.png)

    Bu, oluşturulması birkaç dakika sürer. Oluşturulduktan sonra, oluşturmayı doğrulamak için **Dosya Gezgini** 'ni açın ve C:\\ adresine gidin. yeni test. vhd dosyasını ve 5 GB boyutunu görmeniz gerekir.

    ![](./media/azure-stack-network-howto-extend-datacenter/image25.png)

    CMD komut istemi ' ni kapatın. ISE 'ye geri dönüp ıSE 'de aşağıdaki komutu girin. F:\\ değerini daha önce uygulanmış olan Iscsı hedef sürücü harfiyle değiştirin.

    1. `Copy-Item "C:\\test.vhd" -Destination "F:\\"`

    2. ISE 'deki satırı seçin. Klavyenizde **F8** tuşuna basın.

    3. Komut çalışırken, iki ağ bağdaştırıcısını izleyin ve VM001 içindeki her iki ağ bağdaştırıcısında de veri aktarımını görün. Ayrıca, her bir ağ bağdaştırıcısının yükü eşit bir şekilde paylaşması gerektiğini fark etmelisiniz.

        ![](./media/azure-stack-network-howto-extend-datacenter/image26.png)

Bu senaryo, Azure Stack üzerinde çalışan bir iş yükü ve bu durumda Windows Server tabanlı bir Iscsı hedefi olan bir dış depolama dizisi arasındaki bağlantıyı vurgulamak için tasarlanmıştır. Bu, bir performans testi olacak şekilde tasarlanmamıştır ve alternatif bir Iscsı tabanlı gereç kullanıyorsanız gerçekleştirmeniz gereken adımların bir kısmını, Azure Stack üzerinde iş yüklerini dağıtmada yaptığınız bazı temel noktaları vurgulamaz. ve bunları Azure Stack ortamı dışındaki depolama sistemlerine bağlama.

## <a name="next-steps"></a>Sonraki Adımlar

[Azure Stack ağ oluşturma farklılıkları ve konuları](azure-stack-network-differences.md)  