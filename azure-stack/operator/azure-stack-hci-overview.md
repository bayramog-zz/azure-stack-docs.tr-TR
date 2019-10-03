---
title: Azure Stack HI genel bakış
description: Azure Stack HIP, şirket içinde sanallaştırılmış iş yüklerini çalıştırmak için doğrulanan donanımı kullanan hiper yakınsanmış bir Windows Server 2019 kümesidir. Ayrıca, bulut tabanlı yedekleme, site kurtarma ve daha fazlası için Azure hizmetlerine isteğe bağlı olarak bağlanabilirsiniz. Azure Stack HIP çözümleri, en iyi performans ve güvenilirliği sağlamak için Microsoft tarafından doğrulanan donanımlar kullanır ve NVMe sürücüleri, kalıcı bellek ve uzak doğrudan bellek erişimi (RDMA) ağı gibi teknolojiler için destek içerir.
ms.technology: storage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 05/31/2019
ms.openlocfilehash: 575887ddebf80b39cc0c848f4a6e6cc00abdc9d4
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829497"
---
# <a name="azure-stack-hci-overview"></a>Azure Stack HI genel bakış

>Şunlara uygulanır: Windows Server 2019

Azure Stack HIP, şirket içinde sanallaştırılmış iş yüklerini çalıştırmak için doğrulanan donanımı kullanan hiper yakınsanmış bir Windows Server 2019 kümesidir. Ayrıca, bulut tabanlı yedekleme, site kurtarma ve daha fazlası için Azure hizmetlerine isteğe bağlı olarak bağlanabilirsiniz. Azure Stack HIP çözümleri, en iyi performans ve güvenilirliği sağlamak için Microsoft tarafından doğrulanan donanımlar kullanır ve NVMe sürücüleri, kalıcı bellek ve uzak doğrudan bellek erişimi (RDMA) ağı gibi teknolojiler için destek içerir.

Azure Stack HCı birçok ürünü birleştiren bir çözümdür:

- OEM iş ortağından donanım

- Windows Server 2019 Datacenter sürümü

- Windows Admin Center

- Azure Hizmetleri (isteğe bağlı)

![Azure Stack HCı, Microsoft 'un çok çeşitli donanım iş ortaklarından erişilebilen hiper yakınsama çözümüdür.](media/azure-stack-hci/azure-stack-hci-solution.png)

Azure Stack HCı, Microsoft 'un çok çeşitli donanım iş ortaklarından erişilebilen hiper yakınsama çözümüdür. Azure Stack HCı 'nın gereksinimlerinize en uygun çözüm olup olmadığını belirlemenize yardımcı olmak için, hiper yakınsama çözümü için aşağıdaki senaryoları göz önünde bulundurun:

- **Eskime donanımını yenileyin.** Eski sunucuları ve depolama altyapısını değiştirin ve şirket içinde ve Windows ve Linux sanal makinelerini mevcut BT becerileri ve araçlarıyla birlikte çalıştırın.

- **Sanallaştırılmış iş yüklerini birleştirin.** Eski uygulamaları verimli, hiper yakınsama altyapısında birleştirin. Microsoft Azure gibi Hyper-ölçek veri merkezlerini çalıştırmak için kullanılan bulut verimliliği türlerine dokunun.

- **Karma bulut hizmetleri için Azure 'a bağlanın.** Şirket dışı yedekleme, site kurtarma, bulut tabanlı izleme ve daha fazlası dahil olmak üzere Azure 'daki bulut yönetimine ve güvenlik hizmetlerine erişimi kolaylaştırın.

## <a name="the-azure-stack-family"></a>Azure Stack ailesi

Azure Stack HI, aynı yazılım tanımlı işlem, depolama ve ağ yazılımının Azure Stack olarak kullanıldığı Azure ve Azure Stack ailesinin bir parçasıdır. Farklı çözümlere yönelik hızlı bir özet aşağıda verilmiştir:

- [Azure](https://azure.microsoft.com) -genel bulut hizmetlerini kullanma
- [Azure Stack](azure-stack-overview.md) -şirket içi bulut hizmetlerini çalıştırma
- [Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci) -şirket içinde sanallaştırılmış uygulamaları Azure 'a isteğe bağlı bağlantılarla çalıştırın

![Azure ve Azure Stack bulut hizmetlerini çalıştırın, ancak Azure Stack HI şirket içinde sanallaştırılmış uygulamalar çalıştırır](media/azure-stack-hci/azure-family.png)

|Azure: Genel bulut hizmetlerini kullanma|Azure Stack: Şirket içinde bulut hizmetleri çalıştırma|Azure Stack HCı: Şirket içinde sanallaştırılmış uygulamalar çalıştırma|
|-----------------|-----------------|-----------------|
|Mevcut uygulamaları geçirmek ve modernleştirin ve yeni bulutta yerel uygulamalar oluşturmak için isteğe bağlı, self servis bilgi işlem kaynakları için.|Şirket içinde tutarlı Azure hizmetlerini kullanarak,, bağlantısı kesildiğinde bulut uygulamaları oluşturun ve çalıştırın veya yasal gereksinimleri karşılayın.| Şirket içinde sanallaştırılmış uygulamalar çalıştırın, eskime sunucusu altyapısını değiştirin ve birleştirin ve bulut hizmetleri için Azure 'a bağlanın.|
|Dünyanın dört bir yanındaki 54 bölgede 100 'den fazla hizmet mevcuttur|Windows ve Linux Için Azure VM 'Leri, Azure Web Apps ve Işlevleri, Azure Key Vault, Azure Resource Manager, Azure Marketi, kapsayıcılar, Azure IoT ve Event Hubs, Yönetim Araçları (planlar, teklifler, RBAC)|Hyper V tarafından desteklenen ve Windows Server 2019 ile Depolama Alanları Doğrudan ve Azure hizmetleri 'ne yönelik yönetim ve tümleşik erişim için Windows Yönetim Merkezi ile desteklenen HIP çözümleri.|

Daha fazla bilgi için:

- [Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci) çözümleri Web sitemizden daha fazla bilgi edinin.
- [Yeni Azure Stack HI çözümlerini,](https://aka.ms/AzureStackOverviewVideo)Microsoft uzmanlarını ve Vijay Tewari çözümlerini izleyin.

## <a name="hyperconverged-efficiencies"></a>Hiper yakınsanmış verimlilik

Azure Stack HCı çözümleri, sektör standardı x86 sunucuları ve bileşenleri üzerinde yüksek oranda sanallaştırılmış işlem, depolama ve ağ oluşturma özellikleri sunuyor. Aynı kümedeki kaynakları birleştirmek, dağıtmanızı, yönetmenizi ve ölçeklendirmenizi kolaylaştırır. Komut satırı otomasyonu veya Windows Yönetim Merkezinizi tercih ettiğiniz şekilde yönetin.

Hyper-V ile sunucu uygulamalarınız için sektör lideri sanal makine performansına, Microsoft bulutunun temel hiper yönetici teknolojisine ve NVMe, kalıcı bellek için yerleşik destek ile Depolama Alanları Doğrudan teknolojisinden Uzaktan doğrudan bellek erişimi (RDMA) ağı.

Korumalı sanal makineler, ağ mikro kesimleme ve yerel şifreleme ile uygulamaların ve verilerin korunmasına yardımcı olun.

## <a name="hybrid-capabilities"></a>Hibrit özellikler

Ortak buluttaki bir hiper yakınsanmış altyapı platformu ile birlikte çalışan bulut ve şirket içi özelliklerden yararlanabilirsiniz. Takımınız, Azure altyapı yönetimi hizmetleri ile yerleşik tümleştirme ile bulut becerileri oluşturmaya başlayabilir:

- Bir hizmet olarak yüksek kullanılabilirlik ve olağanüstü durum kurtarma (DRaaS) için Azure Site Recovery.

- Azure Izleyici, uygulamalar, ağınız ve altyapınızda neler olduğunu izlemek için merkezi bir hub, AI tarafından desteklenen gelişmiş analizler.

- Bulut tanığı, Azure 'u küme çekirdeği için hafif bağlama kesici olarak kullanmaktır.

- Şirket dışı veri koruması ve fidye yazılımı ile korunmaya yönelik Azure Backup.

- Azure 'da ve şirket içinde çalışan Windows VM 'lerine yönelik güncelleştirme değerlendirmesi ve güncelleştirme dağıtımları için Azure Güncelleştirme Yönetimi.

- Azure 'da bir noktadan siteye VPN aracılığıyla Şirket içindeki VM 'larınızla kaynakları bağlamak için Azure ağ bağdaştırıcısı.

- Azure Dosya Eşitleme kullanarak dosya sunucunuzu buluta eşitleyin.

Ayrıntılar için bkz. [Windows Server 'ı Azure hibrit hizmetlerine bağlama](https://docs.microsoft.com/windows-server/manage/windows-admin-center/azure/index).

## <a name="management-tools-and-system-center"></a>Yönetim Araçları ve System Center

Azure Stack HCı, Azure Stack olarak aynı sanallaştırma ve yazılım tanımlı depolama ve Ağ yazılımını kullanır. Bununla birlikte, Azure Stack HI ile küme üzerinde tam yönetici haklarına sahip olursunuz ve bu teknolojilerin herhangi birini doğrudan yönetebilir:

- [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)
- [Depolama Alanları Doğrudan](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
- [Yazılım tanımlı ağ](https://docs.microsoft.com/windows-server/networking/sdn/)
- [Yük Devretme Kümelemesi](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview)

Bu teknolojileri yönetmek için aşağıdaki yönetim araçlarını kullanabilirsiniz:

- [Windows Yönetim Merkezi](https://docs.microsoft.com/windows-server/manage/windows-admin-center/overview)
- [System Center](https://www.microsoft.com/cloud-platform/system-center)
- [PowerShell](https://docs.microsoft.com/powershell/?view=powershell-6)
- [Sunucu Yöneticisi](https://docs.microsoft.com/windows-server/administration/server-manager/server-manager)ve MMC ek bileşenleri gibi diğer yönetim araçları
- 5 dokuz yönetici gibi Microsoft dışı araçlar

Altyapınızı dağıtmak ve yönetmek için System Center kullanmayı seçerseniz, System Center sanal makine yönetimi (VMM) ve System Center Operations Manager kullanırsınız. VMM ile, sanal makineleri ve hizmetleri oluşturmak ve özel bulutlara dağıtmak için gereken kaynakları hazırlar ve yönetebilirsiniz. Operations Manager ile, acil eylem sorunlarını belirlemek için kuruluşunuzdaki Hizmetleri, cihazları ve işlemleri izlersiniz.

## <a name="hardware-partners"></a>Donanım iş ortakları

15 iş ortaklarından Windows Server 2019 çalıştıran, doğrulanan Azure Stack HCı çözümlerini satın alabilirsiniz. Tercih ettiğiniz Microsoft iş ortağı, uzun tasarım ve derleme zamanı olmadan çalışmaya başlamanızı ve çalışır. Ayrıca, uygulama ve destek hizmetleri için tek bir iletişim noktası sunar.

Şu anda bu Microsoft iş ortaklarından sunulan 70 + Azure Stack hımızın çözümlerini görüntülemek için [Azure Stack HCI Web sitesini](https://azure.microsoft.com/overview/azure-stack/hci) ziyaret edin: ASUS, Axellio, blueyonga, DataON, Dell EMC, Fujitsu, HPE, Hitachi, Huawei, Lenovo, NEC, primeLine Solutions, QCT, secu, ve Supermicro.

## <a name="faq"></a>SSS

### <a name="what-do-azure-stack-and-azure-stack-hci-solutions-have-in-common"></a>Azure Stack ve Azure Stack HCı çözümleri ortak olarak nedir?

Azure Stack HI çözümleri, aynı Hyper-V tabanlı yazılım tanımlı işlem, depolama ve ağ teknolojilerinin Azure Stack olarak özelliğidir. Her iki teklif da, temel alınan donanım platformuyla güvenilirlik ve uyumluluk sağlamak için kapsamlı test ve doğrulama ölçütlerini karşılar.

### <a name="how-are-they-different"></a>Bunların birbirinden farkı nedir?

Azure Stack, şirket içinde bulut hizmetlerini çalıştırırsınız. Azure IaaS ve PaaS hizmetlerini şirket içinde, şirket içi Azure portal yönetilen her yerde tutarlı bir şekilde derlemek ve çalıştırmak için kullanabilirsiniz.

Azure Stack HI ile, Windows Yönetim Merkezi ve tanıdık Windows Server araçlarıyla yönetilen, şirket içinde sanallaştırılmış iş yüklerini çalıştırırsınız. İsteğe bağlı olarak, bulut tabanlı site kurtarma, izleme ve diğerleri gibi Karma senaryolar için Azure 'a bağlanabilirsiniz.

### <a name="why-is-microsoft-bringing-its-hci-offering-to-the-azure-stack-family"></a>Microsoft neden HıM teklifini Azure Stack ailesine getiriyor?

Microsoft 'un hiper yakınsama teknolojisi zaten Azure Stack temelini.

Birçok Microsoft müşterisi, karmaşık BT ortamlarına sahiptir ve hedefimiz, doğru iş ihtiyacı için doğru teknolojiyle oldukları yerleri karşılayan çözümler sağlamaktır. Azure Stack HCı, daha önce donanım iş ortaklarımız tarafından kullanılabilen Windows Server 2016 tabanlı Windows Server yazılım tanımlı (WSSD) çözümlerinin bir evrimi. Altyapı Yönetimi Hizmetleri için Azure ile sorunsuz bir şekilde bağlanmak üzere yeni seçenekler sunmaya başladığımız için Azure Stack ailesine aldık.

### <a name="does-azure-stack-hci-need-to-be-connected-to-azure"></a>Azure Stack HCı 'nin Azure 'a bağlanması gerekiyor mu?

Hayır, isteğe bağlıdır. Site dışı yedekleme ve olağanüstü durum kurtarma gibi Karma senaryolar için Azure ile tümleştirmeden yararlanabilir ve bulut tabanlı izleme ve güncelleştirme yönetimi, ancak isteğe bağlıdır. Internet bağlantısı kesik olarak çalıştırılbir sorun değildir.

### <a name="how-does-azure-stack-hci-relate-to-windows-server"></a>Azure Stack HCı Windows Server ile nasıl ilişkilidir?

Windows Server 2019, neredeyse her Azure ürününün temelidir. Girdiğiniz tüm özellikler, Windows Server 'da göndermeye ve desteklenmeye devam eder. Azure Stack HIP, iş ortaklarımızın Microsoft tarafından doğrulanan donanımlar kullanılarak şirket içi HIP dağıtımı için önerilen yoldur.

### <a name="can-i-upgrade-from-azure-stack-hci-to-azure-stack"></a>Azure Stack HI 'den Azure Stack sürümüne yükseltebilir miyim? 

Hayır, ancak müşteriler iş yüklerini Azure Stack HI 'den Azure Stack veya Azure 'a geçirebilirler.

### <a name="what-azure-services-can-i-connect-to-azure-stack-hci"></a>Azure Stack HI 'ye hangi Azure hizmetlerini bağlayabilirim?

Azure Stack HCı 'ye bağlayabilmeniz için Azure hizmetlerinin güncelleştirilmiş bir listesi için bkz. [Windows Server 'ı Azure hibrit Services 'A bağlama](https://docs.microsoft.com/windows-server/manage/windows-admin-center/azure/index).

### <a name="how-does-the-cost-of-azure-stack-hci-compare-to-azure-stack"></a>Azure Stack HIP 'nin maliyeti nasıl Azure Stack? 

Azure Stack, hizmetleri ve desteği içeren tamamen tümleşik bir sistem olarak satılır. Azure Stack yönettiğiniz bir sistem veya iş ortaklarımız tarafından tam olarak yönetilen bir hizmet olarak satın alabilirsiniz. Temel sisteme ek olarak, Azure Stack veya Azure üzerinde çalışan Azure Hizmetleri, Kullandıkça Öde esasına göre satılabilir.

Azure Stack HCı çözümleri geleneksel satın alma modelini izler. Azure Stack HIP iş ortaklarından ve yazılımından (Windows Server 2019 Datacenter Edition, yazılım tanımlı veri merkezi özellikleri ve Windows Yönetim Merkezi), çeşitli mevcut kanallardan doğrulanan donanımlar satın alabilirsiniz. Windows Yönetim Merkezi ile kullanabileceğiniz Azure hizmetleri için bir Azure aboneliğiyle ödeme yaparsınız.

### <a name="how-do-i-buy-azure-stack-hci-solutions"></a>Azure Stack HCı çözümlerini satın Nasıl yaparım? misiniz?

Şu adımları uygulayın:

1. Tercih ettiğiniz donanım ortağından Microsoft tarafından doğrulanan bir donanım sistemi satın alın.
1. Yönetim için Windows Server 2019 Datacenter Edition ve Windows Yönetim Merkezi 'ni ve bulut hizmetleri için Azure 'a bağlanma olanağını yükleyin
1. İsteğe bağlı olarak, iş yüklerinize bulut tabanlı yönetim ve güvenlik hizmetleri eklemek için Azure hesabınızı kullanın.

![Azure Stack HCı çözümlerini satın almak için gereksinimlerinize en uygun donanım ortağını ve yapılandırmayı seçin.](media/azure-stack-hci/buying-azure-stack-hci.png)

## <a name="compare-azure-stack-and-azure-stack-hci"></a>Azure Stack ve Azure Stack HIP karşılaştırması

Kuruluşunuz dijital olarak dönüştürmesinde, modern mimarilerde derlemek ve eski uygulamaları yenilemek için ortak bulut hizmetlerini kullanarak daha hızlı hareket edebilirsiniz. Ancak, teknolojik ve yasal düzenlemeler içeren nedenlerle birçok iş yükünün şirket içinde kalması gerekir. Aşağıdaki tablo, hangi Microsoft hibrit bulut stratejisinin, nerede olursa olsun iş yükleri için bulut yeniliği sunabilmeniz gerektiğini belirlemenize yardımcı olur.

|Azure Stack|Azure Stack HCI|
|--------|-------|
|Yeni yetenekler, yenilikçi süreçler|Aynı yetenekler, tanıdık süreçler|
|Veri merkezinizdeki Azure hizmetleri|Veri merkezinizi Azure hizmetlerine bağlama|

### <a name="when-to-use-azure-stack"></a>Ne zaman kullanılacağı Azure Stack

|Azure Stack|Azure Stack HCI|
|--------|-------|
|Self servis hizmet olarak altyapı (IaaS) için Azure Stack kullanarak, güçlü yalıtım ve hassas kullanım izleme ve birden fazla birlikte bulunan kiracılar için geri ödeme yapın. Hizmet sağlayıcıları ve kurumsal özel bulutlar için idealdir. Azure Marketi 'ndeki şablonlar.|Azure Stack HCı, çoklu kiracı için yerel olarak zorunlu değildir veya sağlamaz.|
|Şirket içi Web Apps, Işlevler veya Event Hubs gibi hizmet olarak platform (PaaS) hizmetlerini kullanan uygulamalar geliştirmek ve çalıştırmak için Azure Stack kullanın. Bu hizmetler, tutarlı bir karma geliştirme ve çalışma zamanı ortamı sağlayarak Azure 'da yaptıkları gibi Azure Stack çalışır.|Azure Stack HCı şirket içinde PaaS hizmetlerini çalıştırmaz.
|Kod olarak altyapı, sürekli tümleştirme ve sürekli dağıtım (CI/CD) gibi DevOps uygulamalarıyla ve Azure ile tutarlı VM uzantıları gibi kullanışlı özelliklerle uygulama dağıtımı ve işlem modernleştirin için Azure Stack kullanın. Geliştirme ve DevOps takımları için idealdir.|Azure Stack HCı, hiçbir DevOps aracı bulundurmaz.

### <a name="when-to-use-azure-stack-hci"></a>Azure Stack HCı ne zaman kullanılır?

|Azure Stack|Azure Stack HCI|
|---------------|---------------|
|Azure Stack en az 4 düğüm ve kendi ağ anahtarı gerektirir.|Uzak ofislerin ve dalların en düşük ayak izi için Azure Stack HCı kullanın. En yüksek basitlik ve uygun maliyet için yalnızca 2 sunucu düğümü ve switchdaha geriye doğru ağ ile başlayın. Donanım, 4 sürücü, 64 GB bellek ve $10.000/düğüm altında başlar.
|Azure Stack, Hyper V yapılandırması ve özellik kümesini Azure ile tutarlı bir şekilde kısıtlar.|Exchange, SharePoint ve SQL Server gibi klasik kurumsal uygulamalar için ve dosya sunucusu, DNS, DHCP, IIS ve AD gibi Windows Server rollerini sanallaştırmak için, No-frills Hyper-V sanallaştırma için Azure Stack HI kullanın. Korumalı VM 'Ler gibi tüm Hyper-V özelliklerine Kısıtlanmamış erişim.|
|Azure Stack bu altyapısal teknolojilerini kullanıma sunmaz.|Büyük bir yeniden mimaride, depolama dizileri veya ağ gereçlerinin yerine yazılım tanımlı altyapıyı kullanmak için Azure Stack HI kullanın. Yerleşik Hyper-V, Depolama Alanları Doğrudan ve yazılım tanımlı ağ (SDN) doğrudan erişilebilir ve yönetilebilir.|
