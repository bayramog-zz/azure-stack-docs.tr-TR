---
title: Azure Stack hub 'a genel bakış | Microsoft Docs
description: Azure Stack hub 'ın ne olduğu ve veri merkezinizde Azure hizmetlerini çalıştırmanıza nasıl izin veren hakkında genel bakış.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 09/16/2019
ms.author: justinha
ms.reviewer: unknown
ms.custom: ''
ms.lastreviewed: 05/14/2019
ms.openlocfilehash: 9461b7c306eac0c2a1467378a49d4e91e234a629
ms.sourcegitcommit: 5ef433aa6b75cdfb557fab0ef9308ff2118e66e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73595418"
---
# <a name="azure-stack-hub-overview"></a>Azure Stack hub 'a genel bakış

Azure Stack hub, bir Azure uzantısıdır ve şirket içi bir ortamda uygulama çalıştırmak ve veri merkezinizde Azure hizmetleri sunmak için bir yol sağlar. Tutarlı bir bulut platformu sayesinde kuruluşlar, teknoloji sınırlamalarına bağlı olarak iş kararları verirken iş gereksinimlerine bağlı olarak teknoloji kararları verebilir.

## <a name="why-use-azure-stack-hub-"></a>Azure Stack hub 'ı neden kullanmalısınız?

Azure, geliştiricilerin modern uygulamalar oluşturmalarına yönelik zengin bir platform sağlar. Ancak, bazı bulut tabanlı uygulamalar gecikme, aralıklı bağlantı ve yönetmeliklere benzer. Azure ve Azure Stack hub, hem müşterilere yönelik hem de dahili iş kolu uygulamaları için yeni hibrit bulut kullanım örneklerinin kilidini açar:

- **Kenar ve bağlantısı kesik çözümler**. Azure Stack hub 'ında verileri yerel olarak işleyerek ve daha sonra Azure 'da daha fazla analiz için Azure 'da, her ikisi için de ortak uygulama mantığı ile toplanan gecikme ve bağlantı gereksinimleri. Hatta, Azure bağlantısı olmadan Internet bağlantısı kesilen Azure Stack hub 'ı dağıtabilirsiniz. Örnek olarak fabrika katlarının, CRUISE ve Mine shafts göz önünde bulundurun.

- **Değişen düzenlemeleri karşılayan bulut uygulamaları**. Yasal ve ilke gereksinimlerini karşılamak üzere Azure Stack hub ile şirket içi dağıtımı yapmak için Azure 'da uygulama geliştirin ve dağıtın. Kod değişikliği gerekli değildir. Uygulama örnekleri arasında küresel denetim, finansal raporlama, yabancı Exchange ticareti, çevrimiçi oyun ve gider raporlaması bulunur.

- **Şirket Içi Cloud App model**. Mevcut uygulamaları güncelleştirmek ve genişletmek ya da yenilerini oluşturmak için Azure Hizmetleri, kapsayıcılar, sunucusuz ve mikro hizmet mimarileri kullanın. Bulutta Azure 'da tutarlı DevOps süreçlerini ve şirket içi iş açısından kritik uygulamalar için uygulama modernleştirmesini hızlandırmak üzere Azure Stack hub 'ı şirket içinde kullanın.

Azure Stack hub, aşağıdakileri sağlayarak bu kullanım senaryolarına olanak sağlar:

- Güvenilen donanım iş ortaklarından amaç tarafından oluşturulan Azure Stack hub tümleşik sistemleriyle hızlıca çalışmaya başlamanıza yönelik tümleşik bir teslim deneyimi. Teslimin ardından Azure Stack hub, System Center Operations Manager yönetim paketi veya Nagios uzantısı aracılığıyla izleme ile kolayca veri merkezine tümleştirilir.

- Azure ve Azure Stack Merkez karma ortamları için Azure Active Directory (Azure AD) kullanarak esnek kimlik yönetimi ve bağlantısı kesilmiş dağıtımlar için Active Directory Federasyon Hizmetleri (AD FS) (AD FS) kullanma.

- Geliştirici üretkenliğini en üst düzeye çıkarmak ve karma ortamlarda yaygın DevOps yaklaşımlarını etkinleştirmek için Azure ile tutarlı bir uygulama geliştirme ortamıdır.

- Hibrit bulut bilgi işlem gücü kullanılarak şirket içi Azure hizmetleri teslimi. Azure ile aynı yönetim deneyimlerini ve araçlarını kullanarak Azure IaaS (hizmet olarak altyapı) ve PaaS (hizmet olarak platform) hizmetlerini dağıtmak ve çalıştırmak için Azure ve Azure Stack Hub üzerinde yaygın işletimsel uygulamaları benimseyin. Microsoft yeni Azure Hizmetleri, mevcut hizmetlere yönelik güncelleştirmeler ve ek Azure Market uygulamaları ve görüntüleri dahil Azure Stack hub 'a sürekli Azure yenilik sunar.

## <a name="azure-stack-hub-architecture"></a>Azure Stack hub mimarisi

Azure Stack hub tümleşik sistemleri, güvenilen donanım iş ortakları tarafından oluşturulan ve doğrudan veri merkezinize sunulan 4-16 sunucu raflarında bulunur. Teslim ettikten sonra bir çözüm sağlayıcısı, tümleşik sistemi dağıtmak ve Azure Stack hub çözümünün iş gereksinimlerinizi karşıladığından emin olmak için sizinle birlikte çalışır. Tüm gerekli güç ve soğutma, sınır bağlantısı ve diğer gerekli veri merkezi tümleştirme gereksinimlerinin yerinde olmasını sağlayarak veri merkezinizi hazırlayın.

> Azure Stack Merkezi veri merkezi tümleştirme deneyimi hakkında daha fazla bilgi için bkz. [Azure Stack hub Datacenter Integration](azure-stack-customer-journey.md).

Azure Stack hub, sektör standardı donanımlar üzerine kurulmuştur ve Azure aboneliklerini yönetmek için kullanmakta olduğunuz araçlarla yönetilir. Sonuç olarak, Azure 'a bağlı olduğunuzda tutarlı DevOps işlemleri uygulayabilirsiniz.

Azure Stack hub mimarisi, internet bağlantısı kesilen uzak konumlar veya aralıklı bağlantı için Azure hizmetlerini bir kenara sağlamanıza olanak tanır. Verileri Azure Stack hub 'ında yerel olarak işleyen Karma çözümler oluşturabilir ve daha sonra ek işlem ve analiz için Azure 'da toplayabilirsiniz. Son olarak, şirket içinde Azure Stack Merkezi yüklü olduğundan, herhangi bir kodu değiştirmeden şirket içinde bulut uygulamaları dağıtma esnekliği ile belirli yasal düzenlemeler veya ilke gereksinimlerini karşılayabilirsiniz.

## <a name="deployment-options"></a>Dağıtım seçenekleri

### <a name="production-or-evaluation-environments"></a>Üretim veya değerlendirme ortamları

Azure Stack hub Azure Stack, ihtiyaçlarınızı karşılamak üzere iki dağıtım seçeneği sunulur: üretim kullanımı için hub tümleşik sistemleri ve Azure Stack hub 'ı değerlendirmek için Azure Stack hub geliştirme seti (ASDK).

- **Azure Stack hub tümleşik sistemleri**: Azure Stack hub tümleşik sistemleri, bulut kolaylığı ve bilgi işlem yönetimi basitliği sunan bir çözüm oluşturarak Microsoft ve donanım iş ortaklarının bir ortaklığı aracılığıyla sunulur. Azure Stack hub, tümleşik bir donanım ve yazılım sistemi olarak sunulduğundan, ihtiyacınız olan esneklik ve denetim, buluttan yenilik yapın özelliği ile birlikte sağlanır. Merkez tümleşik sistemleri 4-16 düğümden boyut olarak Azure Stack ve donanım ortağı ve Microsoft tarafından ortaklaşa desteklenir. Yeni senaryolar oluşturmak ve üretim iş yükleriniz için yeni çözümler dağıtmak üzere Azure Stack hub tümleşik sistemleri kullanın.

- **Azure Stack hub Development Kit (asdk)** : [asdk,](../asdk/asdk-what-is.md) Azure Stack hub 'ı değerlendirmek ve öğrenmek Için kullanabileceğiniz, Azure Stack hub 'ının ücretsiz, tek düğümlü bir dağıtımdır. Ayrıca, Azure ile tutarlı olan API 'Leri ve araçları kullanarak uygulamalar oluşturmak için bir geliştirici ortamı olarak ASDK 'yi de kullanabilirsiniz. Ancak, ASDK, üretim ortamı olarak kullanılmak üzere tasarlanmamıştır ve tam tümleşik sistemler üretim dağıtımına kıyasla aşağıdaki sınırlamalara sahiptir:

    - ASDK yalnızca tek bir Azure AD veya AD FS Identity sağlayıcısıyla ilişkilendirilebilir.
    - Azure Stack hub bileşenleri tek bir ana bilgisayarda dağıtıldığından, kiracı kaynakları için kullanılabilen sınırlı sayıda fiziksel kaynak vardır. Bu yapılandırmanın ölçeklendirilmesi veya performans değerlendirmesi için kullanılması amaçlanmamıştır.
    - Tek konak ve NIC dağıtım gereksinimleri nedeniyle ağ senaryoları sınırlıdır.

### <a name="connection-models"></a>Bağlantı modelleri

İnternet 'e **bağlı** olan veya **bağlantısı kesilen** Azure Stack hub 'ı dağıtmayı seçebilirsiniz. Bu seçenek, kimlik deponuzda (Azure AD veya AD FS) ve fatura modelinize (kullandığınız veya kapasite tabanlı faturalandırma olarak öde) hangi seçeneklerin kullanılabileceğini tanımlar.

> Daha fazla bilgi için bkz. [bağlı](azure-stack-connected-deployment.md) ve [bağlantısı kesilen](azure-stack-disconnected-deployment.md) dağıtım modelleriyle ilgili hususlar.

### <a name="identity-provider"></a>Kimlik sağlayıcısı 

Azure Stack hub, kimlik sağlamak için Azure AD veya Active AD FS kullanır. Azure AD, Microsoft 'un bulut tabanlı, çok kiracılı kimlik sağlayıcısıdır. Internet 'e bağlı dağıtımlarla en çok Karma senaryolar, kimlik deposu olarak Azure AD kullanır.

Azure Stack hub 'ına ait bağlantısı kesilen dağıtımlar için AD FS kullanmanız gerekir. Azure Stack hub kaynak sağlayıcıları ve diğer uygulamalar AD FS veya Azure AD ile benzer şekilde çalışır. Azure Stack hub, kendi Active Directory örneğini ve bir Active Directory Graph API içerir.

> [!IMPORTANT]
> Dağıtımdan sonra kimlik sağlayıcısını değiştiremezsiniz. Farklı bir kimlik sağlayıcısı kullanmak için Azure Stack hub 'ı yeniden dağıtmanız gerekir. Azure Stack hub kimliği konuları hakkında daha fazla bilgi edinmek [için Azure Stack hub kimliğiyle Ilgili genel bilgiler](azure-stack-identity-overview.md)bulabilirsiniz.

## <a name="how-is-azure-stack-hub-managed"></a>Azure Stack hub nasıl yönetilir?

Yönetici portalı, Kullanıcı Portalı veya [PowerShell](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.7.1)Ile Azure Stack hub 'ı yönetebilirsiniz. Azure Stack hub portalları her biri Azure Resource Manager ayrı örnekleri tarafından desteklenir. Bir **Azure Stack hub işleci** , Azure Stack hub 'ını yönetmek ve kiracı tekliflerini oluşturmak ve tümleşik sistemin sistem durumunu ve durumunu izlemek gibi işlemleri yapmak için Yönetici portalını kullanır. Kullanıcı Portalı, sanal makineler (VM 'Ler), depolama hesapları ve Web uygulamaları gibi bulut kaynaklarının tüketimine yönelik bir self servis deneyimi sağlar.

> Yönetici portalını kullanarak Azure Stack hub 'ı yönetme hakkında daha fazla bilgi için, bkz. [Azure Stack merkezi yönetim portalı hızlı](azure-stack-manage-portals.md)başlangıcı kullanma.

Azure Stack hub operatörü olarak, [VM 'ler](azure-stack-tutorial-tenant-vm.md), [Web uygulamaları](azure-stack-app-service-overview.md), yüksek oranda kullanılabilir [SQL Server](azure-stack-tutorial-sql.md)ve [MySQL Server](azure-stack-tutorial-mysql.md) veritabanları gibi çok sayıda hizmet ve uygulama sağlayabilirsiniz. SharePoint, Exchange ve daha fazlasını dağıtmak için [Azure Stack hub hızlı başlangıç Azure Resource Manager şablonlarını](https://github.com/Azure/AzureStack-QuickStart-Templates) da kullanabilirsiniz.

Yönetici portalını kullanarak, planları, kotaları, teklifleri ve abonelikleri kullanarak kiracılar için [hizmet sunmak üzere Azure Stack hub 'ı yapılandırabilirsiniz](service-plan-offer-subscription-overview.md) . Kiracı kullanıcıları birden çok teklife abone olabilir. Tekliflerin bir veya daha fazla planı olabilir ve planlar bir veya daha fazla hizmete sahip olabilir. İşleçler Ayrıca kapasiteyi yönetir ve uyarılara yanıt verir.

Azure Stack hub yapılandırıldığında, bir **Azure Stack hub kullanıcısı** işlecin sunduğu hizmetleri kullanır. Kullanıcılar, abone oldukları Hizmetleri, Web Apps, depolama ve VM 'Ler gibi temin edebilir, izleyebilir ve yönetebilir.

> Hangi hesapların kullanılacağı, tipik operatör sorumlulukları, kullanıcılarınız hakkında söyleme ve yardım alma Azure Stack hakkında daha fazla bilgi edinmek için, [Azure Stack Merkez Yönetimi temel bilgilerini](azure-stack-manage-basics.md)gözden geçirin.

## <a name="resource-providers"></a>Kaynak sağlayıcıları

Kaynak sağlayıcıları, tüm Azure Stack hub IaaS ve PaaS hizmetlerinin temelini oluşturan Web hizmetlerdir. Azure Resource Manager hizmetlere erişim sağlamak için farklı kaynak sağlayıcılarına bağımlıdır. Her kaynak sağlayıcı ilgili kaynaklarını yapılandırmanıza ve denetlemenize yardımcı olur. Service Admins, yeni özel kaynak sağlayıcıları da ekleyebilir.

### <a name="foundational-resource-providers"></a>Temel kaynak sağlayıcıları

Üç temel IaaS kaynak sağlayıcısı vardır:

- **İşlem**: Işlem kaynak sağlayıcısı, Azure Stack hub kiracılarının kendi sanal makinelerini oluşturmalarına olanak tanır. Işlem kaynak sağlayıcısı, VM 'Lerin yanı sıra VM uzantıları oluşturma yeteneğini de içerir. VM Uzantısı hizmeti, Windows ve Linux VM 'Leri için IaaS özellikleri sağlamaya yardımcı olur. Örnek olarak, VM 'yi yapılandırmak için bir Linux sanal makinesi sağlamak ve dağıtım sırasında Bash betiklerini çalıştırmak için Işlem Kaynak sağlayıcısını kullanabilirsiniz.
- **Ağ kaynak sağlayıcısı**: ağ kaynak sağlayıcısı, özel bulut için bir dizi yazılım tanımlı ağ (SDN) ve ağ işlevi sanallaştırma (NGD) özellikleri sunar. Yazılım yük dengeleyiciler, genel IP 'Ler, ağ güvenlik grupları ve sanal ağlar gibi kaynaklar oluşturmak için ağ kaynak sağlayıcısını kullanabilirsiniz.
- **Depolama kaynak sağlayıcısı**: depolama kaynak sağlayıcısı, dört Azure tutarlı depolama hizmeti sunar: [BLOB](https://docs.microsoft.com/azure/storage/common/storage-introduction#blob-storage), [kuyruk](https://docs.microsoft.com/azure/storage/common/storage-introduction#queue-storage), [tablo](https://docs.microsoft.com/azure/storage/common/storage-introduction#table-storage)ve [Key Vault](https://docs.microsoft.com/azure/key-vault/) hesap yönetimi, parolaların yönetimi ve denetimini sağlayan, parolalar ve sertifikalar gibi. Depolama kaynak sağlayıcısı Ayrıca, Azure ile tutarlı depolama hizmetleri 'nin hizmet sağlayıcısı yönetimini kolaylaştırmak için bir depolama bulut yönetim hizmeti sunar. Azure depolama, Azure Blobları ile belgeler ve medya dosyaları ve Azure tabloları ile yapılandırılmış NoSQL tabanlı veriler gibi büyük miktarlarda yapılandırılmamış verileri depolama ve alma esnekliği sağlar.

### <a name="optional-resource-providers"></a>İsteğe bağlı kaynak sağlayıcıları

Azure Stack hub ile dağıtabileceğiniz ve kullanabileceğiniz üç isteğe bağlı PaaS kaynak sağlayıcısı vardır:

- **App Service**: [Azure Stack hub 'daki Azure App Service](azure-stack-app-service-overview.md) , Azure Stack hub 'ına sunulan bir PaaS sunumu Microsoft Azure. Hizmet, iç veya dış müşterilerinizin herhangi bir platform veya cihaz için Web, API ve Azure Işlevleri uygulamaları oluşturmalarına olanak sağlar.
- **SQL Server**: SQL veritabanlarını Azure Stack hub 'ı hizmeti olarak sunmak için [SQL Server kaynak sağlayıcısını](azure-stack-sql-resource-provider.md) kullanın. Kaynak sağlayıcısını yükledikten ve bir veya daha fazla SQL Server örneğine bağladığınızda, siz ve kullanıcılarınız bulutta yerel uygulamalar, SQL kullanan Web siteleri ve SQL kullanan diğer iş yükleri için veritabanları oluşturabilir.
- **MySQL sunucusu**: mysql veritabanlarını Azure Stack hub hizmeti olarak göstermek Için [MySQL Server kaynak sağlayıcısını](azure-stack-mysql-resource-provider-deploy.md) kullanın. MySQL kaynak sağlayıcısı bir Windows Server 2016 Server Core VM 'de hizmet olarak çalışır.

## <a name="providing-high-availability"></a>Yüksek kullanılabilirlik sağlama

Azure 'da çoklu VM üretim sisteminin yüksek oranda kullanılabilir olmasını sağlamak için, VM 'Ler birden çok hata etki alanına ve güncelleştirme etki alanlarına yayılan bir [kullanılabilirlik kümesine](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) yerleştirilir. Azure Stack hub 'ının küçük ölçeğinde, bir kullanılabilirlik kümesindeki bir hata etki alanı, ölçek biriminde tek bir düğüm olarak tanımlanır.  

Azure Stack hub 'ın altyapısı hatalara karşı zaten dayanıklı olsa da, temel alınan teknoloji (Yük Devretme Kümelemesi) hala bir donanım arızası olması durumunda etkilenen fiziksel sunucudaki VM 'Ler için bazı kesinti süreleri doğurur. Azure Stack hub, en fazla üç hata etki alanı içeren bir kullanılabilirlik kümesinin Azure ile tutarlı olmasını destekler.

- **Hata etki alanları**: bir kullanılabilirlik kümesine yerleştirilmiş VM 'ler, birden çok hata etki alanına (Azure Stack hub düğümü) olabildiğince eşit bir şekilde yayarak birbirleriyle fiziksel olarak yalıtılır. Bir donanım arızası varsa, başarısız hata etki alanındaki VM 'Ler diğer hata etki alanlarında yeniden başlatılır. Bunlar, diğer VM 'lerden farklı hata etki alanlarında, ancak mümkünse aynı Kullanılabilirlik kümesinde tutulur. Donanım yeniden çevrimiçi olduğunda, yüksek kullanılabilirlik sağlamak için VM 'Ler yeniden dengelenebilir.

- **Güncelleştirme etki alanları**: güncelleştirme etki alanları, kullanılabilirlik kümelerinde yüksek kullanılabilirlik sağlayan başka bir Azure kavramıdır. Bir güncelleştirme etki alanı, aynı anda Bakımı düşük bir şekilde temel alınan donanımların mantıksal grubudur. Aynı güncelleştirme etki alanında bulunan VM 'Ler, planlı bakım sırasında birlikte yeniden başlatılır. Kiracılar bir kullanılabilirlik kümesi içinde VM 'Ler oluştururken, Azure platformu VM 'Leri Bu güncelleştirme etki alanlarına otomatik olarak dağıtır. Azure Stack hub 'ında, VM 'Ler, temel ana bilgisayar güncellenmadan önce kümedeki diğer çevrimiçi ana bilgisayarlar arasında dinamik olarak geçirilir. Bir konak güncelleştirmesi sırasında kiracı kapalı kalma süresi olmadığından, Azure Stack hub 'daki etki alanı güncelleştirme özelliği yalnızca Azure ile şablon uyumluluğu için mevcuttur. Bir kullanılabilirlik kümesindeki VM 'Ler, portalda güncelleştirme etki alanı numarası olarak **0** gösterir.

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Rol tabanlı erişim denetimi 'ni (RBAC), bir abonelik, kaynak grubu veya tek kaynak düzeyinde roller atayarak yetkili kullanıcılara, gruplara ve hizmetlere sistem erişimi sağlamak için kullanabilirsiniz. Her rol, bir Kullanıcı, Grup veya hizmetin Microsoft Azure Stack hub kaynakları üzerinde sahip olduğu erişim düzeyini tanımlar.

Azure Stack hub RBAC, tüm kaynak türleri için uygulanan üç temel role sahiptir: sahip, katkıda bulunan ve okuyucu. Sahibinin, başkalarına erişim yetkisi verme hakkı dahil tüm kaynaklara tam erişimi vardır. Katkıda bulunan her türlü Azure kaynağı oluşturabilir ve yönetebilir, ancak diğer kullanıcılara erişim izni veremez. Okuyucu yalnızca mevcut kaynakları görüntüleyebilir. RBAC rollerinin geri kalanı belirli Azure kaynaklarının yönetimine izin verir. Örneğin, sanal makine katılımcısı rolü VM 'lerin oluşturulmasına ve yönetimine izin verir ancak sanal ağın veya VM 'nin bağlandığı alt ağın yönetimine izin vermez.

> Daha fazla bilgi için bkz. [rol tabanlı erişim denetimini yönetme](azure-stack-manage-permissions.md).

## <a name="reporting-usage-data"></a>Kullanım verilerini raporlama

Azure Stack hub, tüm kaynak sağlayıcıları genelinde kullanım verilerini toplayıp toplar ve Azure ticareti tarafından işlenmek üzere Azure 'a aktarır. Azure Stack hub 'ında toplanan kullanım verileri bir REST API üzerinden görüntülenebilir. Tüm kiracı aboneliklerinde kullanım verilerini almak için sağlayıcı ve sağlayıcı API 'Lerinin yanı sıra, Azure ile tutarlı bir Kiracı API 'SI de vardır. Bu veriler, faturalandırma veya geri ödeme için bir dış araçla veya hizmetle tümleştirilecek şekilde kullanılabilir. Kullanım Azure ticareti tarafından işlendikten sonra Azure Faturalandırma portalında görüntülenebilir.

> [Azure 'a Azure Stack hub kullanım verilerini raporlama](azure-stack-usage-reporting.md)hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack hub 'ı ve küresel Azure 'U karşılaştırın](compare-azure-azure-stack.md).

[Yönetim Temelleri](azure-stack-manage-basics.md).

[Hızlı başlangıç: Azure Stack merkezi yönetim portalını kullanın](azure-stack-manage-portals.md).
