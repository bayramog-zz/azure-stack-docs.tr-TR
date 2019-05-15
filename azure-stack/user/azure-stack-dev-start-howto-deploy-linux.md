---
title: Bir Linux VM için Azure Stack dağıtma | Microsoft Docs
description: Bir uygulama için Azure Stack dağıtma.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 04/24/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: 61d9f21f35edf1a0e8ebf61c81580c4d53218970
ms.sourcegitcommit: 2a4321a9cf7bef2955610230f7e057e0163de779
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65617657"
---
# <a name="deploy-a-linux-vm-to-host-a-web-app-in-azure-stack"></a>Azure Stack'te bir web uygulaması barındırmak üzere bir Linux VM dağıtma

Oluşturun ve bir web çerçevesi ile oluşturduğunuz bir web uygulamasını barındırmak için Azure Marketi'nde Ubuntu görüntüsünü kullanarak temel bir Linux sanal makinesi (VM) dağıtın. 

Bu VM, web apps kullanarak barındırabilirsiniz:

- **Python**: Flask, Bottle ve Django Python web çerçeveleri ortak içerir.
- **Git**: Ortak çerçeveleri dahil Revel, Martini Gocraft/web ve Gorilla gidin. 
- **Ruby**: Ruby on Rails çerçevesi Ruby web uygulamalarınızı sunmak için ayarlayın. 
- **Java**: Bir Apache Tomcat sunucusuna göndermesine web uygulamaları geliştirmek için Java kullanın. Tomcat Linux'ta yükleme ve Java WAR dosyalarınızın doğrudan sunucuya dağıtabilirsiniz. 

Çalışan tüm web uygulaması, framework ve Linux işletim sistemi kullanan arka uç teknolojisi ile başlamak için bu makaledeki yönergeleri kullanın. Azure Stack ardından altyapınızı yönetin ve uygulamanız için bakım görevlerini işlemek için teknolojinizi içinde yönetim araçları kullanmak için de kullanabilirsiniz.

## <a name="deploy-a-linux-vm-for-a-web-app"></a>Bir Linux VM için bir web uygulaması dağıtma

Bu işlem bir gizli anahtar oluşturun, temel bir Linux VM görüntüsü kullanma, sanal makinenin belirli öznitelikleri belirtin ve ardından VM oluşturun. VM oluşturduktan sonra VM ile çalışmaya ve uygulamanızı barındırmak sanal makine için gerekli olan bağlantı noktalarını açın. Ardından, DNS adı oluşturun. Son olarak, VM'ye bağlanın ve makinenin apt-get yardımcı programını kullanarak güncelleştirin. İşlemi tamamladıktan sonra Azure Stack Örneğinizde web uygulamanızı barındırmak hazır olan bir VM gerekir.

Başlamadan önce yerinde ihtiyacınız olan her şeye sahip olduğunuzdan emin olun.

## <a name="prerequisites"></a>Önkoşullar

- Ubuntu Server 16.04 LTS görüntüsüne erişimi olan bir Azure Stack aboneliğine. Görüntüyü daha sonraki bir sürümünü kullanabilirsiniz, ancak bu yönergeleri göz önünde 16.04 LTS ile yazılır. Bu görüntü yoksa, bulut operatörünüze, görüntüyü Azure Stack marketini almak için iletişime geçin.

## <a name="deploy-the-vm-by-using-the-portal"></a>Portalı kullanarak VM dağıtma

VM'yi dağıtmak için sonraki birkaç bölümlerde'ndaki yönergeleri izleyin.

### <a name="create-your-vm"></a>VM oluşturma

1. Sunucunuz için bir Secure Shell (SSH) ortak anahtar oluşturun. Daha fazla bilgi için [SSH ortak anahtarı kullanmayı](azure-stack-dev-start-howto-ssh-public-key.md).
1. Azure Stack portalında **kaynak Oluştur** > **işlem** > **Ubuntu Server 16.04 LTS**.

    ![Azure Stack VM için bir web uygulaması dağıtma](media/azure-stack-dev-start-howto-deploy-linux/001-portal-compute.png)

4. İçinde **sanal makine oluşturma** bölmesinde için **1. Temel ayarları yapılandırma**:

    a. Girin **sanal makinenizin adı**.

    b. Seçin **VM disk türü**, ya da **Premium SSD** (için Premium diskler [SSD]) veya **standart HDD** (için standart diskleri [HDD]).

    c. Girin, **Username**.

    d. Seçin **kimlik doğrulama türü** olarak **SSH ortak anahtarından**.

    e. Oluşturduğunuz SSH ortak anahtarını alın. Bir metin düzenleyicisinde açın, anahtarı kopyalayın ve ardından yapıştırın **SSH ortak anahtarı** kutusu. Metinden dahil `---- BEGIN SSH2 PUBLIC KEY ----` için `---- END SSH2 PUBLIC KEY ----`. Metnin tüm bloğu anahtar kutuya yapıştırın:

    ```text  
    ---- BEGIN SSH2 PUBLIC KEY ----
    Comment: "rsa-key-20190207"
    <Your key block>
    ---- END SSH2 PUBLIC KEY ----
    ```

    f. Azure Stack Örneğiniz için bir abonelik seçin.

    g. Yeni bir kaynak grubu oluşturun veya nasıl uygulamanız için kaynakları düzenlemek istediğinize bağlı olarak bir var olanı kullanın.

    h. Konumunuzu seçin. Azure Stack geliştirme Seti'ni (ASDK) genellikle bir *yerel* bölge. Konumun, Azure Stack Örneğinizde bağlıdır.
1. İçin **2. Boyutu**, türü:
    - Azure Stack Örneğinizde kullanılabilir VM'niz için veri ve RAM boyutu seçin.
    - Liste veya filtre tarafından sanal makinenizin boyutunu ya da göz atabilirsiniz **işlem türü**, **CPU'lar**, ve **depolama alanı**.
    
    > [!NOTE]
    > - Sunulan Fiyatlar, yerel para biriminizde. Bunlar, yalnızca Azure altyapı maliyetleri ve aboneliğe ve konuma yönelik indirimleri içerir. Bunlar, ilgili yazılım maliyetlerini içermez. 
    > - Önerilen boyut Seçili görüntü yayımcısı tarafından belirlenir ve donanım ve yazılım gereksinimlerine dayalıdır.
    > - Premium diskler (SSD) yerine standart diskler (HDD) kullanan işletim sistemi performansını etkileyebilir.

1. İçinde **3. İsteğe bağlı yapılandırma** özellikleri, türü:

    a. İçin **yüksek kullanılabilirlik** bir kullanılabilirlik kümesi seçin. Uygulamanıza yedeklilik sağlamak için bir kullanılabilirlik kümesinde iki veya daha fazla sanal makine gruplandırın. Bu yapılandırma planlı veya Plansız bakım olayı sırasında en az bir sanal makine kullanılabilir ve % 99,95 oranında karşılaması gerektiğini sağlar. Azure hizmet düzeyi sözleşmesi (SLA). Bir sanal makinenin kullanılabilirlik kümesi oluşturulduktan sonra değiştirilemez.

    b. İçin **depolama**seçin **Premium diskler (SSD)** veya **standart diskler (HDD)**. Premium diskler (SSD) katı hal sürücüleriyle desteklenir ve tutarlı, düşük gecikme süreli performans sunar. Bunlar, fiyat ve performans arasında en iyi dengeyi sağlar ve g/Ç açısından yoğun uygulamalar ve üretim iş yükleri için idealdir. Standart diskler manyetik sürücüler tarafından desteklenir ve verilere sık erişilmeyen uygulamalar için tercih edilebilir. Bölgesel olarak yedekli diskler, verilerinizi birden çok alanda çoğaltan, bölgesel olarak yedekli depolama ile (ZRS) desteklenir ve tek bir bölge çalışmıyor olsa bile, bunlar kullanılabilir. 

    c. Seçin **yönetilen diskleri kullan**. Bu özelliği etkinleştirmek, Azure disklerin kullanılabilirliğini otomatik olarak yönetir. Veri yedekliliği ve hataya dayanıklılık, kendi depolama hesaplarını oluşturmak ve yönetmek zorunda kalmadan yararlanır. Yönetilen diskler tüm bölgelerde kullanılamayabilir. Daha fazla bilgi için [giriş Azure yönetilen diskler](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview).

    d. Ağınızı yapılandırmak için seçin **sanal ağ**. Sanal ağlar Azure'da birbirinden mantıksal olarak yalıtılır. Kendi IP adres aralıkları, alt ağlar, rota tablolarını, ağ geçitleri ve, veri merkezinizdeki geleneksel bir ağa benzer güvenlik ayarlarını yapılandırabilirsiniz. Aynı sanal ağdaki sanal makineler, varsayılan olarak birbirine erişebilir. 

    e. Alt ağınızı yapılandırmak için seçin **alt**. Bir IP adresi aralığı sanal ağınızdaki bir alt ağdır. Bir alt ağ, sanal makineleri birbirinden veya internet'ten ayırmak için kullanabilirsiniz. 

    f. Sanal makinenize veya sanal makinenizde çalışan hizmetlere erişimi yapılandırmak için seçin **genel IP adresi**. Sanal ağ dışındaki sanal makineden ile iletişim kurmak için bir genel IP adresi kullanın. 

    g. Seçin **ağ güvenlik grubu**, **temel**, veya **Gelişmiş**. İzin veren veya VM ağ trafiği reddeden kuralları ayarlayın. 

    h. Sanal Makinenize erişim için ortak ve özel protokolleri ayarlamak için seçin **ortak gelen bağlantı noktası**. Hizmet, bu kural için hedef protokolü ve bağlantı noktası aralığını belirtir. Uzak Masaüstü Protokolü (RDP) veya SSH gibi önceden tanımlanmış bir hizmeti seçebilir veya özel bağlantı noktası aralığı belirtebilirsiniz. 
        Web sunucusu için SSH (22) açık HTTP (80) ve HTTPS (443) kullanın. Makine ile RDP bağlantısı kullanarak yönetmeyi planlıyorsanız, 3389 numaralı bağlantı noktasını açın.

    i. Sanal makinenizde uzantıları eklemek için seçin **uzantıları**. Uzantıları yapılandırma yönetimi ve virüsten koruma gibi yeni özellikler, sanal makinenize ekleyin. 

    j. Etkinleştirmek veya devre dışı **izleme**. Başlatma sorunlarının tanılanmasına yardımcı olmak için izleme seri konsol çıkışını ve ekran görüntüleri bir konakta çalışan bir sanal makinenin yakalamak için kullanabilirsiniz. 

    k. Ölçümlerinizin içeren depolama hesabını belirtmek için seçin **tanılama depolama hesabı**. Böylece, bunları kendi araçlarınızla çözümleyebilirsiniz ölçümler bir depolama hesabına yazılır. 

    m. **Tamam**’ı seçin.

1. Gözden geçirme **4. Özet**:
    - Portal ayarlarınızı doğrular.
    - Ayarlarınızı ile bir Azure Resource Manager iş akışı yeniden kullanmak için sanal Makineniz için Azure Resource Manager şablonu indirebilirsiniz.
    - Doğrulama geçtiğinde seçin **Tamam**. VM dağıtımı birkaç dakika sürer.

### <a name="specify-the-open-ports-and-dns-name"></a>Açık bağlantı noktaları ve DNS adını belirtin

Web uygulamanızı ağınızdaki kullanıcılar için erişilebilir hale getirmek için makineye bağlanmak ve kolay bir DNS adı gibi eklemek için kullanılan bağlantı noktaları açma *mywebapp.local.cloudapp.azurestack.external*, kullanıcıların web tarayıcılarında kullanabilirsiniz .

#### <a name="open-inbound-ports"></a>Gelen bağlantı noktalarını açma

RDP veya SSH gibi önceden tanımlanmış bir hizmeti için hedef protokolü ve bağlantı noktası aralığını değiştirme veya özel bağlantı noktası aralığı belirtebilirsiniz. Örneğin, web Çerçevenizi bağlantı noktası aralığı ile çalışmak isteyebilirsiniz. Git, örneğin, bağlantı noktası 3000 ile iletişim kurar.

1. Kiracınız için Azure Stack portalını açın.

1. Sanal makinenizin arayın. VM, panoya sabitlenmiş veya içinde arama yapabilirsiniz **kaynak Ara** kutusu.

1. Seçin **ağ** , VM bölmesinde.

1. Seçin **gelen bağlantı noktası Ekle** bir bağlantı noktasını açmak için kural.

1. İçin **kaynak**, varsayılan seçimi bırakın **herhangi**.

1. İçin **kaynak bağlantı noktası aralığı**, joker karakter (*) bırakın.

1. İçin **hedef bağlantı noktası aralığı**, örneğin, açmak istediğiniz bağlantı noktasını girin **3000**.

1. İçin **Protokolü**, varsayılan seçimi bırakın **herhangi**.

1. **Eylem** alanında **İzin ver**'i seçin.

1. İçin **öncelik**, varsayılan seçimi bırakın.

1. Girin bir **adı** ve **açıklama** neden bağlantı noktasının açık olduğundan anımsamanıza yardımcı olacak.

1. **Add (Ekle)** seçeneğini belirleyin.

#### <a name="add-a-dns-name-for-your-server"></a>Sunucunuz için bir DNS adı ekleme

Ayrıca, böylece kullanıcılar bir URL kullanarak Web sitenize bağlanabilir, sunucunuz için bir DNS adı oluşturabilirsiniz.

1. Kiracınız için Azure Stack portalını açın.

1. Sanal makinenizin arayın. VM, panoya sabitlenmiş veya içinde arama yapabilirsiniz **kaynak Ara** kutusu.

1. **Genel Bakış**’ı seçin.

1. Altında **VM**seçin **yapılandırma**.

1. İçin **atama**seçin **dinamik**.

1. DNS ad etiketi girin **mywebapp şeklindedir**, tam URL'nizi haline gelebilmesi *mywebapp.local.cloudapp.azurestack.external* (ASDK uygulama için).

### <a name="connect-via-ssh-to-update-your-vm"></a>Sanal makinenizin güncelleştirmek için SSH ile bağlanma

1. Azure Stack örnek olarak aynı ağ üzerinde SSH istemcisi açın. Daha fazla bilgi için [bir SSH ortak anahtarı kullanmak](azure-stack-dev-start-howto-ssh-public-key.md).

1. Aşağıdaki komutları girin:

    ```bash  
        sudo apt-get update
        sudo apt-get -y upgrade
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [Azure Stack geliştirme ortamında ayarlama](azure-stack-dev-start.md).
