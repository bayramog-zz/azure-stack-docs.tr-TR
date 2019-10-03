---
title: Azure Stack bir Linux VM dağıtma | Microsoft Docs
description: Azure Stack için bir uygulama dağıtın.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 10/02/2019
ms.openlocfilehash: d1fae6caf6ac37f29382f4d24ce0d8b2299aa1d7
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71824818"
---
# <a name="deploy-a-linux-vm-to-host-a-web-app-in-azure-stack"></a>Azure Stack bir Web uygulamasını barındırmak için bir Linux sanal makinesi dağıtma

Bir Web çerçevesiyle oluşturduğunuz bir Web uygulamasını barındırmak için Azure Marketi 'ndeki Ubuntu görüntüsünü kullanarak temel bir Linux sanal makinesi (VM) oluşturabilir ve dağıtabilirsiniz. 

Bu VM, kullanarak Web uygulamalarını barındırabilir:

- **Python**: Ortak Python web çerçeveleri Flask, şişe ve Docgo 'u içerir.
- **Git**: Ortak go çerçeveleri, Relevel, martini, Gocraft/Web ve Gorilla 'yı içerir. 
- **Ruby**: Ruby Web uygulamalarınızı teslim etmek için Ruby on rayını bir çerçeve olarak ayarlayın. 
- **Java**: Apache Tomcat sunucusuna nakledeceğiniz Web uygulamaları geliştirmek için Java kullanın. Linux 'a Tomcat yükleyebilir ve ardından Java WAR dosyalarınızı doğrudan sunucuya dağıtabilirsiniz. 

Linux işletim sistemini kullanan tüm Web uygulamalarını, çatılarını ve arka uç teknolojisini kullanmaya ve çalıştırmaya başlamanıza yönelik bu makaledeki yönergeleri kullanın. Daha sonra altyapınızı yönetmek için Azure Stack kullanabilir ve uygulamanızın bakım görevlerini işlemek için teknolojinizdeki yönetim araçlarını kullanabilirsiniz.

## <a name="deploy-a-linux-vm-for-a-web-app"></a>Bir Web uygulaması için Linux VM dağıtma

Bu işlemde, bir gizli anahtar oluşturur, Linux VM 'nin temel görüntüsünü kullanın, sanal makinenin belirli özniteliklerini belirtin ve ardından VM 'yi oluşturun. VM 'yi oluşturduktan sonra VM ile çalışmak için gerekli olan bağlantı noktalarını ve VM 'yi uygulamanızı barındırmak üzere açarsınız. Sonra, DNS adını oluşturursunuz. Son olarak, VM 'ye bağlanır ve apt-get yardımcı programını kullanarak makineyi güncelleştirebilirsiniz. İşlemi tamamladıktan sonra, Azure Stack Örneğinizde Web uygulamanızı barındırmak için hazırlamış bir VM 'niz olacaktır.

Başlamadan önce, ihtiyacınız olan her şeye sahip olduğunuzdan emin olun.

## <a name="prerequisites"></a>Önkoşullar

- Ubuntu Server 16,04 LTS görüntüsüne erişimi olan Azure Stack bir abonelik. Görüntünün sonraki bir sürümünü kullanabilirsiniz, ancak bu yönergeler aklınızda 16,04 LTS ile yazılır. Bu resme sahip değilseniz, görüntüyü Azure Stack Market 'e almak için bulut işleçle iletişim kurun.

## <a name="deploy-the-vm-by-using-the-portal"></a>Portalı kullanarak VM 'yi dağıtma

VM 'yi dağıtmak için, sonraki birkaç bölümde bulunan yönergeleri izleyin.

### <a name="create-your-vm"></a>VM oluşturma

1. Sunucunuz için bir Secure Shell (SSH) ortak anahtarı oluşturun. Daha fazla bilgi için bkz. [SSH ortak anahtarı kullanma](azure-stack-dev-start-howto-ssh-public-key.md).
1. Azure Stack portalında **kaynak oluştur** > **işlem** > **Ubuntu Server 16,04 LTS**' yi seçin.

    ![Bir Web uygulamasını Azure Stack VM 'ye dağıtma](media/azure-stack-dev-start-howto-deploy-linux/001-portal-compute.png)

4. @No__t-11 için **sanal makine oluştur** bölmesinde. Temel ayarları yapılandırma @ no__t-0:

    a. **Sanal makinenizin adını**girin.

    b. **VM disk türünü**(Premium DISKLER [SSD] için) veya **Standart HDD** (Standart diskler [HDD] için) **Premium SSD** seçin.

    c. **Kullanıcı adınızı**girin.

    d. **Kimlik doğrulama türünü** **SSH ortak anahtarı**olarak seçin.

    e. Oluşturduğunuz SSH ortak anahtarını alın. Bir metin düzenleyicisinde açın, anahtarı kopyalayın ve **SSH ortak anahtar** kutusuna yapıştırın. @No__t-0 ' dan `---- END SSH2 PUBLIC KEY ----` ' e kadar olan metni ekleyin. Tüm metin bloğunu anahtar kutusuna yapıştırın:

    ```text  
    ---- BEGIN SSH2 PUBLIC KEY ----
    Comment: "rsa-key-20190207"
    <Your key block>
    ---- END SSH2 PUBLIC KEY ----
    ```

    f. Azure Stack örneğiniz için abonelik seçin.

    g. Uygulamanızın kaynaklarını nasıl düzenlemek istediğinize bağlı olarak, yeni bir kaynak grubu oluşturun veya var olan bir kaynak grubunu kullanın.

    h. Konumunuzu seçin. Azure Stack Geliştirme Seti (ASDK) genellikle *Yerel* bir bölgede bulunur. Konum Azure Stack örneğine bağlıdır.
1. @No__t-02 için. Boyut @ no__t-0, tür:
    - Azure Stack örneğiniz için kullanılabilir olan sanal makinenizin veri boyutunu ve RAM 'i seçin.
    - **İşlem türü**, **CPU 'lar**ve **depolama alanı**ile sanal makinenizin boyutuna ilişkin listeye veya filtrelemeye gidebilirsiniz.
    
    > [!NOTE]
    > - Sunulan fiyatlar, yerel para biriminizdeki tahminlerdir. Bunlar yalnızca Azure altyapı maliyetlerini ve abonelik ve konum için tüm indirimleri içerir. Bunlar, ilgili yazılım maliyetlerini içermez. 
    > - Önerilen Boyutlar, seçili görüntünün yayımcısı tarafından belirlenir ve donanım ve yazılım gereksinimlerini temel alır.
    > - Premium diskler (SSD) yerine standart diskler (HDD) kullanmak işletim sistemi performansını etkileyebilir.

1. @No__t-03 ' de. İsteğe bağlı @ no__t-0 özelliklerini yapılandırın, şunu yazın:

    a. **Yüksek kullanılabilirlik** için bir kullanılabilirlik kümesi seçin. Uygulamanıza yedeklilik sağlamak için bir kullanılabilirlik kümesinde iki veya daha fazla sanal makineyi gruplayın. Bu yapılandırma, planlı veya plansız bir bakım olayı sırasında en az bir sanal makinenin kullanılabilir olmasını ve% 99,95 Azure hizmet düzeyi sözleşmesini (SLA) karşılamasını sağlar. Bir sanal makinenin kullanılabilirlik kümesi oluşturulduktan sonra değiştirilemez.

    b. **Depolama**için **PREMIUM diskler (SSD)** veya **Standart diskler (HDD)** öğesini seçin. Premium diskler (SSD) katı hal sürücüleriyle desteklenir ve tutarlı, düşük gecikme süreli performans sunar. Bunlar, Fiyat ve performans arasında en iyi dengeyi sağlar ve g/ç kullanımı yoğun uygulamalar ve üretim iş yükleri için idealdir. Standart diskler manyetik sürücüler tarafından desteklenir ve verilere Seyrek erişilen uygulamalar için tercih edilir. Bölgesel olarak yedekli diskler, verileri birden çok bölgede çoğaltan, bölgesel olarak yedekli depolama (ZRS) tarafından desteklenir ve tek bir bölge çalışmıyor olsa bile kullanılabilir. 

    c. **Yönetilen diskleri kullan**' ı seçin. Bu özelliği etkinleştirdiğinizde, Azure disklerin kullanılabilirliğini otomatik olarak yönetir. Depolama hesapları oluşturup yönetmek zorunda kalmadan, veri yedekliliği ve hataya dayanıklılık avantajına sahip olursunuz. Yönetilen diskler tüm bölgelerde kullanılamayabilir. Daha fazla bilgi için bkz. [Azure yönetilen disklere giriş](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview).

    d. Ağınızı yapılandırmak için **sanal ağ**' ı seçin. Sanal ağlar, Azure 'da birbirinden mantıksal olarak yalıtılmıştır. Kendi IP adresi aralıklarını, alt ağlarını, yol tablolarını, ağ geçitlerini ve güvenlik ayarlarını, veri merkezinizdeki geleneksel bir ağla aynı şekilde yapılandırabilirsiniz. Aynı sanal ağdaki sanal makineler, varsayılan olarak birbirlerine erişebilir. 

    e. Alt ağını yapılandırmak için **alt ağ**' ı seçin. Alt ağ, sanal ağınızdaki bir IP adresi aralığıdır. Sanal makineleri birbirinden veya internet 'ten ayırmak için bir alt ağ kullanabilirsiniz. 

    f. VM 'nize veya VM 'niz üzerinde çalışan hizmetlere erişimi yapılandırmak için **genel IP adresi**' ni seçin. Sanal Ağ dışından sanal makine ile iletişim kurmak için bir genel IP adresi kullanın. 

    g. **Ağ güvenlik grubu**, **temel**veya **Gelişmiş**' i seçin. VM 'ye ağ trafiğine izin veren veya reddeden kuralları ayarlayın. 

    h. VM 'nize ortak veya özel protokoller için erişim ayarlamak için **Genel gelen bağlantı noktaları**' nı seçin. Hizmet, bu kural için hedef protokolü ve bağlantı noktası aralığını belirtir. Uzak Masaüstü Protokolü (RDP) veya SSH gibi önceden tanımlanmış bir hizmeti seçebilir veya özel bir bağlantı noktası aralığı sağlayabilirsiniz. 
        Web sunucusu için HTTP (80), HTTPS (443) ve SSH (22) açık kullanın. Makineyi bir RDP bağlantısı kullanarak yönetmeyi planlıyorsanız 3389 numaralı bağlantı noktasını açın.

    i. Sanal makinenize uzantı eklemek için **Uzantılar**' ı seçin. Uzantılar, sanal makinenize yapılandırma yönetimi veya virüsten koruma koruması gibi yeni özellikler ekler. 

    j. **İzlemeyi**devre dışı bırakın veya etkinleştirin. Başlatma sorunlarını tanılamaya yardımcı olmak için, bir konakta çalışan bir sanal makinenin seri konsol çıkışını ve ekran görüntülerini yakalamak için izlemeyi kullanabilirsiniz. 

    k. Ölçümlerinizi tutan depolama hesabını belirtmek için **Tanılama depolama hesabı**' nı seçin. Ölçümler bir depolama hesabına yazılır, bu sayede bunları kendi araçlarınızla çözümleyebilirsiniz. 

    girişindeki. **Tamam**’ı seçin.

1. @No__t-04 ' i gözden geçirin. Özet @ no__t-0:
    - Portal, ayarlarınızı doğrular.
    - Ayarlarınızı bir Azure Resource Manager iş akışıyla yeniden kullanmak için, sanal makinenizin Azure Resource Manager şablonunu indirebilirsiniz.
    - Doğrulama geçtiğinde **Tamam**' ı seçin. VM dağıtımı birkaç dakika sürer.

### <a name="specify-the-open-ports-and-dns-name"></a>Açık bağlantı noktalarını ve DNS adını belirtin

Web uygulamanızı ağınızdaki kullanıcılar için erişilebilir hale getirmek için, makineye bağlanmak için kullanılan bağlantı noktalarını açın ve kullanıcıların Web tarayıcılarında kullanabileceği *MyWebApp. Local. cloudapp. azurestack. external*gibi kolay bir DNS adı ekleyin.

#### <a name="open-inbound-ports"></a>Gelen bağlantı noktalarını açma

RDP veya SSH gibi önceden tanımlanmış bir hizmetin hedef protokolünü ve bağlantı noktası aralığını değiştirebilir veya özel bir bağlantı noktası aralığı sağlayabilirsiniz. Örneğin, Web çerçevesinin bağlantı noktası aralığıyla çalışmak isteyebilirsiniz. Örneğin, bağlantı noktası 3000 üzerinde iletişim kurar.

1. Kiracınız için Azure Stack portalını açın.

1. VM 'niz için arama yapın. Sanal makineyi panonuza sabitlemiş olabilirsiniz veya **arama kaynakları** kutusunda arama yapabilirsiniz.

1. VM bölmesinizdeki **ağ** ' ı seçin.

1. Bir bağlantı noktası açmak için **gelen bağlantı noktası kuralı ekle** ' yi seçin.

1. **Kaynak**için, varsayılan seçimi bırakın, **herhangi bir**.

1. **Kaynak bağlantı noktası aralığı**için joker karakteri (*) bırakın.

1. **Hedef bağlantı noktası aralığı**için, açmak istediğiniz bağlantı noktasını girin, örneğin **3000**.

1. **Protokol** **için, varsayılan seçimi bırakın.**

1. **Eylem** alanında **İzin ver**'i seçin.

1. **Öncelik**için varsayılan seçimi bırakın.

1. Bağlantı noktasının neden açık olduğunu anımsamanıza yardımcı olması için bir **ad** ve **Açıklama** girin.

1. **Add (Ekle)** seçeneğini belirleyin.

#### <a name="add-a-dns-name-for-your-server"></a>Sunucunuz için bir DNS adı ekleyin

Ayrıca, kullanıcıların bir URL kullanarak Web sitenize bağlanabilmesi için sunucunuz için bir DNS adı oluşturabilirsiniz.

1. Kiracınız için Azure Stack portalını açın.

1. VM 'niz için arama yapın. Sanal makineyi panonuza sabitlemiş olabilirsiniz veya **arama kaynakları** kutusunda arama yapabilirsiniz.

1. **Genel Bakış**’ı seçin.

1. **VM**altında **Yapılandır**' ı seçin.

1. **Atama**için **dinamik**' i seçin.

1. Tam URL 'nizin *MyWebApp. Local. cloudapp. azurestack. external* (bır asdk uygulaması için) haline gelmesi için **MYWEBAPP**gibi DNS adı etiketini girin.

### <a name="connect-via-ssh-to-update-your-vm"></a>VM 'nizi güncelleştirmek için SSH aracılığıyla bağlanma

1. Azure Stack örneğiniz ile aynı ağda, SSH istemcinizi açın. Daha fazla bilgi için bkz. [SSH ortak anahtarı kullanma](azure-stack-dev-start-howto-ssh-public-key.md).

1. Aşağıdaki komutları girin:

    ```bash  
        sudo apt-get update
        sudo apt-get -y upgrade
    ```

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack bir geliştirme ortamı ayarlamayı](azure-stack-dev-start.md)öğrenin.
