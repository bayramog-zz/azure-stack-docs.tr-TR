---
title: Azure Stack HCI genel bakış
description: Azure Stack HCI, şirket içi sanallaştırılmış iş yükleri çalıştırmak için donanım kullanan hiper yakınsanmış bir Windows Server 2019 küme doğrulanmış olur. Ayrıca isteğe bağlı olarak, bulut tabanlı Yedekleme, site kurtarma ve diğer Azure hizmetlerine bağlanabilirsiniz. Azure Stack HCI çözümleri, en iyi performans ve güvenilirlik sağlamak için Microsoft doğrulanmış donanım kullanın ve NVMe sürücüler, kalıcı bellek ve uzak doğrudan bellek erişimi (RDMA) ağ gibi teknolojiler için destek içerir.
ms.technology: storage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 05/31/2019
ms.openlocfilehash: c3f25f38429b58d8d72037ff4ed9758d9993a2c5
ms.sourcegitcommit: 07cc716d97bf484c7260eb165ae205ae25e09589
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453543"
---
# <a name="azure-stack-hci-overview"></a>Azure Stack HCI genel bakış

>Şunlara uygulanır: Windows Server 2019

Azure Stack HCI, şirket içi sanallaştırılmış iş yükleri çalıştırmak için donanım kullanan hiper yakınsanmış bir Windows Server 2019 küme doğrulanmış olur. Ayrıca isteğe bağlı olarak, bulut tabanlı Yedekleme, site kurtarma ve diğer Azure hizmetlerine bağlanabilirsiniz. Azure Stack HCI çözümleri, en iyi performans ve güvenilirlik sağlamak için Microsoft doğrulanmış donanım kullanın ve NVMe sürücüler, kalıcı bellek ve uzak doğrudan bellek erişimi (RDMA) ağ gibi teknolojiler için destek içerir.

Azure Stack HCI çeşitli ürünleri bir araya getiren bir çözümdür:

- OEM ortağı donanımdan

- Windows Server 2019 Datacenter sürümü

- Windows Admin Center

- Azure Hizmetleri (isteğe bağlı)

![Azure Stack HCI Microsoft'un hiper yakınsanmış bir donanım iş ortaklarından oluşan geniş aralığından kullanılabilir bir çözümdür.](media/azure-stack-hci/azure-stack-hci-solution.png)

Azure Stack HCI Microsoft'un hiper yakınsanmış bir donanım iş ortaklarından oluşan geniş aralığından kullanılabilir bir çözümdür. Azure Stack HCI ihtiyaçlarınıza uygun çözümü olup olmadığını belirlemenize yardımcı olması bir hiper yakınsanmış çözüm için aşağıdaki senaryoları düşünün:

- **Eskime donanım yenileyin.** Windows ve Linux sanal makineleri şirket içi ve eski sunucular ve depolama altyapısı değiştirin ve var olan BT becerileri ve araçları ile uçta.

- **Sanallaştırılmış iş yükleri birleştirin.** Eski uygulamaları verimli bir hiper yakınsanmış altyapı birleştirin. Bulut verimliliğini ölçekli veri merkezlerinden Microsoft Azure gibi çalıştırmak için kullanılan aynı türde uygulamasına dokunun.

- **Hibrit bulut Hizmetleri için Azure'a bağlanın.** Yönetim ve güvenlik hizmetleri site dışında yedekleme, site recovery, bulut tabanlı izleme ve daha fazlası dahil olmak üzere azure'daki bulut erişimi kolaylaştırın.

## <a name="the-azure-stack-family"></a>Azure Stack ailesi

Azure Stack HCI Azure bir parçasıdır ve Azure Stack ailesi, aynı yazılım tanımlı bilgi işlem, depolama ve Azure Stack olarak ağ yazılımı. Farklı çözümlerin hızlı bir özeti aşağıda verilmiştir:

- [Azure](https://azure.microsoft.com) -genel bulut hizmetlerini kullanma
- [Azure Stack](azure-stack-overview.md) -çalışan şirket içi bulut Hizmetleri
- [Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci) -sanallaştırılmış şirket içi uygulama, Azure için isteğe bağlı bağlantılarıyla çalışma

![Şirket içi uygulamaları Azure ve Azure Stack bulut Hizmetleri, Azure Stack HCI çalışırken çalıştıran sanallaştırılmış](media/azure-stack-hci/azure-family.png)

|Azure: Genel bulut hizmetlerini kullanma|Azure Stack: Bulut Hizmetleri şirket içi çalıştırma|Azure Stack HCI: Şirket içi sanallaştırılmış uygulama çalıştırma|
|-----------------|-----------------|-----------------|
|Geçirme ve mevcut uygulamaları modernleştirebilirsiniz yeni bulutta yerel uygulamalar oluşturmak isteğe bağlı, Self Servis bilgi işlem kaynakları için.|Derleme ve çalıştırma Bulut uygulamalarının kesildiğinde uç cihazlarında, ya da yasal gereksinimleri karşılamak amacıyla, tutarlı Azure kullanarak şirket içi Hizmetleri.| Çalıştırma sanallaştırılmış uygulamaları şirket içi, değiştirin ve eskime sunucu altyapınızı birleştirin ve bulut Hizmetleri için Azure'a bağlanma.|
|Dünya çapındaki 54 bölgeler kullanılabilir 100'den fazla Hizmetleri|Azure Vm'leri için Windows ve Linux, Azure Web Apps ve İşlevler, Azure Key Vault, Azure Resource Manager, Azure marketi, kapsayıcılar, Azure IOT ve Event Hubs, (planları, teklifleri, RBAC) yönetim araçları|Yönetim ve tümleşik Azure hizmetlerine erişim için Hyper-V ve Windows Server 2019 ve Windows Admin Center ile depolama alanları doğrudan tarafından desteklenen HCI çözümleri doğrulandı.|

Daha fazla bilgi için:

- Daha fazla bilgi bizim [Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci) çözümleri Web sitesi.
- Jeff Woolsey ve Vijay Tewari Microsoft uzmanları izleyin [yeni Azure Stack HCI çözümleri tartışmak](https://aka.ms/AzureStackOverviewVideo).

## <a name="hyperconverged-efficiencies"></a>Hiper yakınsanmış verimliliği

Azure Stack HCI çözümleri bir araya getirir, yüksek oranda sanallaştırılmış bilgi işlem, depolama ve ağ üzerinde x86 endüstri standardı sunucuları ve bileşenleri. Aynı küme kaynaklarında birleştirme, dağıtmak, yönetmek ve ölçeklendirmek kolaylaştırır. Seçtiğiniz komut satırı Otomasyonu veya Windows Admin Center ile yönetin.

Hyper-V, Microsoft bulut teknolojisini temel hiper yönetici ve depolama alanları doğrudan teknolojisi ile server uygulamalarınız için NVMe, kalıcı bellek için yerleşik destek sunan sektör lideri sanal makine performansı elde etmek ve Uzak doğrudan bellek erişimi (RDMA) ağ.

Uygulamaları ve verileri korumalı sanal makineler, ağ microsegmentation ve yerel şifreleme korunmasına yardımcı olur.

## <a name="hybrid-capabilities"></a>Hibrit özellikler

Bulut ve şirket içinde genel bulutta hiper yakınsanmış bir altyapı platformu ile birlikte çalışma yararlanabilirsiniz. Takımınız, yerleşik hizmetlerle tümleştirme sağlayan Azure altyapı yönetimi ile bulut becerileri oluşturmaya başlayabilirsiniz:

- Yüksek kullanılabilirlik ve olağanüstü durum kurtarma (DRaaS) hizmet olarak Azure Site Recovery.

- Azure İzleyici, uygulamalar, ağ ve altyapı – arasında ile neler olduğunu izlemek için merkezi bir hub Gelişmiş yapay ZEKA ile desteklenen analiz'ü tıklatın.

- Bulut tanığı, küme çekirdeği için Azure basit KRAVAT ayırıcı kullanılacak.

- Şirket dışı veri koruması ve fidye yazılımlarına karşı korunmak için Azure yedekleme.

- Azure güncelleştirme yönetimi için güncelleştirme değerlendirmesi ve güncelleştirme dağıtımları için Windows Azure ve şirket içinde çalışan VM'ler.

- Vm'lerinizi Azure noktadan siteye VPN aracılığıyla şirket kaynaklarına bağlanmak için Azure ağ bağdaştırıcısı.

- Dosya sunucunuzu kullanarak Azure dosya eşitleme bulut ile eşitleyin.

Ayrıntılar için bkz [bağlanan Windows Server için Azure karma Hizmetleri](https://docs.microsoft.com/windows-server/manage/windows-admin-center/azure/index).

## <a name="management-tools-and-system-center"></a>Yönetim Araçları ve System Center

Azure Stack HCI aynı sanallaştırma ve yazılım tanımlı depolama ve ağ yazılımı, Azure Stack kullanır. Ancak, Azure Stack HCI ile küme üzerinde tam yönetici haklarına sahip ve kendi teknolojileri hiçbirini doğrudan yönetebilir:

- [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)
- [Depolama alanları doğrudan](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
- [Yazılım tanımlı ağ](https://docs.microsoft.com/windows-server/networking/sdn/)
- [Yük Devretme Kümelemesi](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview)

Bu teknolojiler yönetmek için aşağıdaki yönetim araçları kullanabilirsiniz:

- [Windows Admin Center](https://docs.microsoft.com/windows-server/manage/windows-admin-center/overview)
- [System Center](https://www.microsoft.com/cloud-platform/system-center)
- [PowerShell](https://docs.microsoft.com/powershell/?view=powershell-6)
- Diğer yönetim araçları gibi [Sunucu Yöneticisi'ni](https://docs.microsoft.com/windows-server/administration/server-manager/server-manager)ve MMC ek bileşenleri
- 5nıne Manager gibi Microsoft dışı araçlar

Uygulamanızın altyapısını dağıtma ve yönetme için System Center kullanmayı seçerseniz, System Center Virtual Machine Management'ı (VMM) ve System Center Operations Manager kullanırsınız. VMM ile sağlayın ve oluşturun ve sanal makinelerini ve hizmetlerini özel bulutlara dağıtmak için gereken kaynakları yönetin. Operations Manager ile Hizmetleri, aygıtları ve işlemleri sorunları saptayıp hemen önlem belirlemek için kurumunuzun tamamıyla izleyin.

## <a name="hardware-partners"></a>Donanım iş ortaklarından

Windows Server 2019 15 iş ortaklarından çalışan doğrulanmış Azure Stack HCI çözümleri satın alabilirsiniz. Tercih ettiğiniz Microsoft iş ortağı uzun tasarım ve derleme zamanı çalışmaya alır. Bunlar ayrıca, tek bir uygulama ve Destek hizmetlerine iletişim noktası sağlar.

Ziyaret [Azure Stack HCI Web sitesi](https://azure.microsoft.com/overview/azure-stack/hci) 70'ten fazla Azure Stack HCI Çözümlerimiz bu Microsoft iş ortaklarından şu anda kullanılabilir görüntülemek için: ASUS Axellio, bluechip, DataON, Dell EMC, Fujitsu, HPE, Hitachi, Huawei, Lenovo, NEC, primeLine çözümleri, QCT, SecureGUARD ve Supermicro.

## <a name="faq"></a>SSS

### <a name="what-do-azure-stack-and-azure-stack-hci-solutions-have-in-common"></a>Hangi Azure Stack ve Azure Stack HCI çözümleri ortak var mı?

Azure Stack HCI çözümleri Azure Stack yazılım tanımlı aynı Hyper-V tabanlı işlem, depolama ve ağ teknolojileri özellik. Her iki teklifleri, temel alınan donanım platformu ile güvenilirliği ve uyumluluğu sağlamak için sıkı sınama ve doğrulama ölçütlerini karşılayan.

### <a name="how-are-they-different"></a>Bunların birbirinden farkı nedir?

Azure Stack ile bulut hizmetlerini şirket içinde çalıştırın. Azure Iaas çalıştırabilir ve tutarlı bir şekilde oluşturup herhangi bir yerde, bulut uygulamalarını Azure portalı ile şirket içi yönetilen çalıştırmak için şirket PaaS Hizmetleri.

Azure Stack HCI ile sanallaştırılmış iş yükleri şirket içinde çalıştırmadan araçları Windows Admin Center ve bilinen Windows Server ile yönetiliyor. İsteğe bağlı olarak, bulut tabanlı bir site recovery, izleme ve diğerleri gibi karma senaryolar için Azure'a bağlanabilirsiniz.

### <a name="why-is-microsoft-bringing-its-hci-offering-to-the-azure-stack-family"></a>Neden Microsoft Azure Stack ailesine teklifi onun HCI getiriyor?

Microsoft'un hiper yakınsanmış teknolojisi zaten Azure Stack temelidir.

Çoğu Microsoft müşterisi karmaşık BT ortamları ve Amacımız bunları doğru iş için doğru teknolojiyi sahip oldukları karşılayan çözümler sağlamanız gerekir. Azure Stack HCI evrim donanım iş ortaklarımız için daha önce mevcut Windows Server 2016 tabanlı Windows Server Software-Defined (WSSD) çözümlerinin ' dir. Azure ile altyapı Yönetimi Hizmetleri için sorunsuz bir şekilde bağlanmak için yeni seçenekler sunmaya Başladık çünkü biz bunu Azure Stack ailesinde yaptı.

### <a name="does-azure-stack-hci-need-to-be-connected-to-azure"></a>Azure Stack HCI Azure'a bağlanmanız gerekiyor mu?

Hayır, isteğe bağlıdır. Azure ile tümleştirme için şirket dışı yedekleme ve olağanüstü durum kurtarma ve bulut tabanlı izleme ve güncelleştirme yönetimi gibi karma senaryolar yararlanabilir, ancak bunlar isteğe bağlı. Internet'ten bağlantısı kesilmiş çalıştırmak için sorun değil.

### <a name="how-does-azure-stack-hci-relate-to-windows-server"></a>Windows Server için Azure Stack HCI nasıl ilişkilidir?

Windows Server 2019 neredeyse tüm Azure ürün oluşturmaktadır. Değer devam gönderin ve Windows Server'da desteklenen için tüm özellikler. Azure Stack HCI HCI şirket içinde dağıtmak için önerilen yöntem Microsoft doğrulanmış donanım İş Ortaklarımızın kullanmaktır.

### <a name="can-i-upgrade-from-azure-stack-hci-to-azure-stack"></a>Azure Stack için Azure Stack HCI yükseltebilir miyim? 

Hayır, ancak müşterilerin geçiş, iş yüklerini Azure Stack HCI Azure Stack'te veya Azure'da için.

### <a name="what-azure-services-can-i-connect-to-azure-stack-hci"></a>Hangi Azure Hizmetleri için Azure Stack HCI bağlayabilirim?

Güncelleştirilmiş bir listesini Azure Hizmetleri için görmek için Azure Stack HCI bağlanabilir [bağlanan Windows Server için Azure karma Hizmetleri](https://docs.microsoft.com/windows-server/manage/windows-admin-center/azure/index).

### <a name="how-does-the-cost-of-azure-stack-hci-compare-to-azure-stack"></a>Azure Stack HCI maliyeti Azure Stack'e nasıl arasındaki fark nedir? 

Azure Stack Hizmetleri ve desteği içeren tam olarak tümleşik bir sistem satılır. Azure Stack tam olarak yönetilen bir hizmet olarak veya sizin yönettiğiniz bir sistem iş ortaklarımız satın alabilirsiniz. Temel sistem ek olarak, Azure Stack veya Azure üzerinde çalışan Azure Hizmetleri-,-kullandıkça olarak satılır.

Azure Stack HCI çözümleri geleneksel satın alma modeli izleyin. Azure Stack HCI iş ortakları ve var olan çeşitli kanallar (yazılım tanımlı veri merkezi özellikleri ve Windows Admin Center ile Windows Server 2019 Datacenter edition) yazılımından doğrulanmış donanım satın alabilirsiniz. Windows Admin Center ile kullandığınız Azure Hizmetleri için bir Azure aboneliği ile ödeme yaparsınız.

### <a name="how-do-i-buy-azure-stack-hci-solutions"></a>Azure Stack HCI çözümleri nasıl satın?

Şu adımları uygulayın:

1. Bir Microsoft doğrulanmış donanım sistemi, tercih edilen donanım ortağınızdan satın alın.
1. Windows Server 2019 Datacenter edition ve Windows Admin Center yönetimi ve bulut Hizmetleri için Azure'a bağlanma yeteneğini yükleyin
1. İsteğe bağlı olarak, iş yüklerinize bulut tabanlı yönetim ve güvenlik hizmetlerini eklemek için Azure hesabınızı kullanın.

![Azure Stack HCI çözümleri satın almak için donanım iş ortağı ve ihtiyaçlarınıza en uygun yapılandırma'yı seçin.](media/azure-stack-hci/buying-azure-stack-hci.png)

## <a name="compare-azure-stack-and-azure-stack-hci"></a>Azure Stack ve Azure Stack HCI karşılaştırın

Kuruluşunuzun dijital olarak dönüştüren gibi daha hızlı bir şekilde modern mimariler oluşturun ve eski uygulamaları yenilemek için genel bulut hizmetlerini kullanarak taşıyabilirsiniz olduğunu fark edebilirsiniz. Ancak, teknolojik ve yasal engellerini dahil nedeniyle, şirket içi çok sayıda iş yükü kalmalıdır. Aşağıdaki tabloda, nerede olurlarsa olsunlar iş yükleri için bulut yenilik sunma ihtiyacınız olduğunda gereken hangi Microsoft hibrit bulut stratejisi sunar belirlemenize yardımcı olur.

|Azure Stack|Azure Stack HCI|
|--------|-------|
|Yeni beceriler, yenilikçi işlemleri|Aynı yetenekleri, tanıdık işlemleri|
|Veri merkezinizde Azure Hizmetleri|Veri merkezinizi Azure hizmetlerine bağlanma|

### <a name="when-to-use-azure-stack"></a>Azure Stack kullanıldığı durumlar

|Azure Stack|Azure Stack HCI|
|--------|-------|
|Azure Stack, güçlü yalıtım, kesin kullanımı izleme ve geri ödemeye birlikte bulunan birden fazla Kiracı Self Servis altyapısı-bir hizmet olarak için (Iaas) kullanın. Hizmet sağlayıcıları ve kurumsal özel bulut için idealdir. Azure Market'ten şablonları.|Azure Stack HCI yerel olarak uygulayabilir veya sağlamak için çok kiracılı değildir.|
|Event hubs'ı şirket içi veya olarak bir-hizmet Platform (PaaS) hizmetleri kullanan uygulamaları geliştirin ve çalıştırın için kullanım Azure Stack Web Apps, İşlevler, gibi. Tam olarak tutarlı bir karma geliştirme ve çalışma zamanı ortamı sağlayan Azure, bunu gibi bu hizmetler, Azure Stack üzerinde çalıştırın.|Azure Stack HCI PaaS hizmetlerinin şirket içi çalıştırılmaz.
|Azure Stack, uygulama dağıtımı ve altyapısı gibi DevOps uygulamaları ile bir işlem kodu, sürekli tümleştirme ve sürekli dağıtım (CI/CD) ve Azure ile tutarlı sanal makine uzantıları gibi kullanışlı özellikler olarak modernize etme kullanın. Geliştirme ve DevOps takımları için idealdir.|Azure Stack HCI herhangi bir DevOps araç yerel olarak içermez.

### <a name="when-to-use-azure-stack-hci"></a>Azure Stack HCI kullanıldığı durumlar

|Azure Stack|Azure Stack HCI|
|---------------|---------------|
|Azure Stack gerektirir en az 4 düğüm ve kendi ağ anahtarları.|Azure Stack HCI, uzak ofisler ve dallar için minimum ayak için kullanın. Yalnızca 2 sunucu düğümleri ve yoğun Basitlik ve uygun fiyatı benzersizdir için switchless uçtan uca ağ başlayın. Donanım teklifler 4 sürücüleri, 64 GB bellek, iyi altında $10k başlangıcında / düğüm.
|Azure Stack Hyper V yapılandırmasına ve Azure ile tutarlılık özelliğini kısıtlar.|Azure Stack HCI no-frills Klasik Kurumsal uygulamaları gibi Exchange, SharePoint ve SQL Server ve dosya sunucusu, DNS, DHCP, IIS ve AD gibi Windows Server rolleri sanallaştırmak için Hyper-V sanallaştırma için kullanın. Korumalı VM'ler gibi tüm Hyper-V özelliklerine sınırsız erişim.|
|Azure Stack altyapısal teknolojiler ortaya çıkarmıyor.|Azure Stack HCI yazılım tanımlı altyapı eskime depolama dizileri veya ana rearchitecture olmadan ağ Gereçleri yerine kullanın. Yerleşik Hyper-V, depolama alanları doğrudan ve yazılım tanımlı ağ (SDN) doğrudan erişilebilir ve yönetilebilir.|