---
title: Azure Stack nedir? | Microsoft Docs
description: Azure Stack, veri merkezinizde Azure hizmetlerini çalıştırmanıza nasıl olanak sağladığını öğrenin.
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
ms.openlocfilehash: 060258b07e3eef385298600794a6dd8154a461fc
ms.sourcegitcommit: d159652f50de7875eb4be34c14866a601a045547
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72283411"
---
# <a name="azure-stack-overview"></a>Azure Stack genel bakış

Azure Stack, bir Azure uzantısıdır ve şirket içi bir ortamda uygulama çalıştırmak ve veri merkezinizde Azure hizmetleri sunmak için bir yol sağlar. Tutarlı bir bulut platformu sayesinde kuruluşlar, teknoloji sınırlamalarına bağlı olarak iş kararları verirken iş gereksinimlerine bağlı olarak teknoloji kararları verebilir.

## <a name="why-use-azure-stack"></a>Neden Azure Stack kullanmalıyım?

Azure, geliştiricilerin modern uygulamalar oluşturmalarına yönelik zengin bir platform sağlar. Ancak, bazı bulut tabanlı uygulamalar, gecikme süresi, aralıklı bağlantı ve düzenlemeler gibi engelleri ortaya ayırır. Azure ve Azure Stack hem müşterilere yönelik hem de dahili iş kolu uygulamaları için yeni hibrit bulut kullanım örneklerinin kilidini açın:

- **Kenar ve bağlantısı kesik çözümler**. Azure Stack, verileri yerel olarak işleyerek ve daha sonra Azure 'da daha fazla analiz için Azure 'da toplayarak, her ikisi için de genel uygulama mantığı olan adres gecikmesi ve bağlantı gereksinimleri. Hatta, Azure bağlantısı olmadan internet bağlantısı kesilen Azure Stack da dağıtabilirsiniz. Örnek olarak fabrika katlarının, CRUISE ve Mine shafts göz önünde bulundurun.

- **Değişen düzenlemeleri karşılayan bulut uygulamaları**. Kod değişikliği gerektirmeden, yasal bir şekilde veya ilke gereksinimlerini karşılamak üzere Azure Stack ile şirket içi dağıtımı yapmak için Azure 'da uygulama geliştirin ve dağıtın. Uygulama örnekleri arasında küresel denetim, finansal raporlama, yabancı Exchange ticareti, çevrimiçi oyun ve gider raporlaması bulunur.

- **Şirket Içi bulut uygulama modeli**. Mevcut uygulamaları güncelleştirmek ve genişletmek ya da yenilerini oluşturmak için Azure Hizmetleri, kapsayıcılar, sunucusuz ve mikro hizmet mimarileri kullanın. Bulutta Azure üzerinde tutarlı DevOps süreçlerini kullanın ve temel görev açısından kritik uygulamalar için uygulama modernleştirmesini hızlandırmak üzere şirket içinde Azure Stack.

Azure Stack, bu kullanım senaryolarını şunları sağlar:

- Güvenilen donanım iş ortaklarından amaç tarafından oluşturulan Azure Stack tümleşik sistemlerle hızlıca çalışmaya başlamanıza yönelik tümleşik bir teslim deneyimi. Teslimin ardından Azure Stack, System Center Operations Manager yönetim paketi veya Nagios uzantısı aracılığıyla izleme ile kolayca veri merkezine tümleştirilir.

- Azure için Azure Active Directory (Azure AD) ve karma ortamları Azure Stack ve bağlantısı kesilen dağıtımlar için Active Directory Federasyon Hizmetleri (AD FS) (AD FS) kullanan esnek kimlik yönetimi. 

- Geliştirici üretkenliğini en üst düzeye çıkarmak ve karma ortamlarda yaygın DevOps yaklaşımlarını etkinleştirmek için Azure ile tutarlı bir uygulama geliştirme ortamıdır.

- Hibrit bulut bilgi işlem gücü kullanılarak şirket içi Azure hizmetleri teslimi. Azure ile aynı yönetim deneyimlerini ve araçlarını kullanarak Azure IaaS ve PaaS hizmetlerini dağıtmak ve çalıştırmak için Azure ve Azure Stack üzerinde yaygın işletimsel uygulamaları benimseyin. Microsoft, yeni Azure Hizmetleri, mevcut hizmetlere yönelik güncelleştirmeler ve ek Azure Marketi uygulamaları ve görüntüleri gibi Azure Stack için sürekli Azure yeniliklerini sunar.

## <a name="azure-stack-architecture"></a>Azure Stack mimarisi
Azure Stack tümleşik sistemler, güvenilen donanım iş ortakları tarafından oluşturulan ve doğrudan veri merkezinize sunulan 4-16 sunucu raflarında bulunur. Teslim ettikten sonra bir çözüm sağlayıcısı, tümleşik sistemi dağıtmak ve Azure Stack çözümünün iş gereksinimlerinizi karşıladığından emin olmak için sizinle birlikte çalışır. Tüm gerekli güç ve soğutma, sınır bağlantısı ve diğer gerekli veri merkezi tümleştirme gereksiniminin yerinde olduğundan veri merkezinizi hazırlamanız gerekir. 

> Azure Stack veri merkezi tümleştirme deneyimi hakkında daha fazla bilgi için bkz. [Azure Stack veri merkezi tümleştirmesi](azure-stack-customer-journey.md).

Azure Stack, sektör standardı donanımlar üzerine kurulmuştur ve Azure aboneliklerini yönetmek için kullanmakta olduğunuz araçlarla yönetilir. Sonuç olarak, Azure 'a bağlı olup olmadığına bakılmaksızın tutarlı DevOps işlemleri uygulayabilirsiniz. 

Azure Stack mimarisi, internet bağlantısı kesilen uzak konumlar veya aralıklı bağlantı için Azure hizmetlerini bir kenara sağlamanıza olanak tanır. Azure Stack verileri yerel olarak işleyen Karma çözümler oluşturabilir ve ardından ek işlem ve analiz için Azure 'da toplayabilirsiniz. Son olarak, Azure Stack şirket içinde yüklü olduğundan, herhangi bir kodu değiştirmeden şirket içinde bulut uygulaması dağıtma esnekliği ile belirli yasal düzenlemeler veya ilke gereksinimlerini karşılayabilirsiniz. 

## <a name="deployment-options"></a>Dağıtım seçenekleri

### <a name="production-or-evaluation-environments"></a>Üretim veya değerlendirme ortamları
Azure Stack, gereksinimlerinizi karşılamak için iki dağıtım seçeneği sunulur, üretim kullanımı için tümleşik sistemleri ve Azure Stack değerlendirmek için Azure Stack Geliştirme Seti (ASDK) Azure Stack:

- **Tümleşik sistemler Azure Stack**. Azure Stack tümleşik sistemler, Microsoft ve donanım iş ortaklarının bir ortaklığı aracılığıyla sunulur ve bulut kolaylığı ve bilgi işlem yönetimi basitliği sunan bir çözüm oluşturur. Azure Stack tümleşik bir donanım ve yazılım sistemi olarak sunulduğundan, ihtiyacınız olan esneklik ve denetim, buluttan yenilik yapın özelliği ile birlikte sağlanır. 4-16 düğümlerden oluşan boyut olarak tümleşik sistemler Azure Stack ve donanım ortağı ve Microsoft tarafından ortaklaşa desteklenir. Yeni senaryolar oluşturmak ve üretim iş yükleriniz için yeni çözümler dağıtmak üzere Azure Stack tümleşik sistemleri kullanın.

- **Azure Stack geliştirme seti**. [Azure Stack geliştirme seti (ASDK)](../asdk/asdk-what-is.md) , Azure Stack hakkında değerlendirmek ve bu bilgileri öğrenmek için kullanabileceğiniz, ücretsiz ve Azure Stack tek düğümlü bir dağıtımdır. Ayrıca, Azure ile tutarlı olan API 'Leri ve araçları kullanarak uygulamalar oluşturmak için bir geliştirici ortamı olarak ASDK 'yi de kullanabilirsiniz. Ancak, ASDK, üretim ortamı olarak kullanılmak üzere tasarlanmamıştır ve tam tümleşik sistemler üretim dağıtımına kıyasla aşağıdaki sınırlamalara sahiptir:

    - ASDK yalnızca tek bir Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) (AD FS) kimlik sağlayıcısıyla ilişkilendirilebilir.
    - Azure Stack bileşenleri tek bir ana bilgisayarda dağıtıldığından, kiracı kaynakları için kullanılabilen sınırlı sayıda fiziksel kaynak vardır. Bu yapılandırmanın ölçeklendirilmesi veya performans değerlendirmesi için tasarlanmamıştır.
    - Tek konak ve NIC dağıtım gereksinimleri nedeniyle ağ senaryoları sınırlıdır.

### <a name="connection-models"></a>Bağlantı modelleri
İnternet 'e **bağlı** olan veya **bağlantısı kesilen** Azure Stack dağıtmayı seçebilirsiniz. Bu seçenek, kimlik deponuzda (Azure AD veya AD FS) ve fatura modelinize (kullandığınız veya kapasite tabanlı faturalandırma olarak öde) hangi seçeneklerin kullanılabileceğini tanımlar.

> Daha fazla bilgi için bkz. [bağlı](azure-stack-connected-deployment.md) ve [bağlantısı kesilen](azure-stack-disconnected-deployment.md) dağıtım modelleriyle ilgili hususlar. 

### <a name="identity-provider"></a>Kimlik sağlayıcısı 
Azure Stack kimlikleri sağlamak için Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) (AD FS) kullanır. Azure AD, Microsoft 'un bulut tabanlı, çok kiracılı kimlik sağlayıcısıdır. Internet 'e bağlı dağıtımlarla en çok Karma senaryolar, kimlik deposu olarak Azure AD kullanır. 

Azure Stack bağlantısı kesilen dağıtımlar için Active Directory Federasyon Hizmetleri (AD FS) (AD FS) kullanmanız gerekir. Azure Stack kaynak sağlayıcıları ve diğer uygulamalar AD FS veya Azure AD ile benzer şekilde çalışır. Azure Stack kendi Active Directory örneğini ve bir Active Directory Graph API içerir.

> [!IMPORTANT]
> Dağıtımdan sonra kimlik sağlayıcısını değiştiremezsiniz. Farklı bir kimlik sağlayıcısı kullanmak için Azure Stack yeniden dağıtmanız gerekir.

> Azure Stack kimlik konuları hakkında daha fazla bilgi [için Azure Stack kimliğe genel bakış](azure-stack-identity-overview.md)hakkında daha fazla bilgi edinebilirsiniz.

## <a name="how-is-azure-stack-managed"></a>Azure Stack nasıl yönetilir?
Yönetim Portalı, Kullanıcı Portalı veya [PowerShell](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.7.1)ile Azure Stack yönetebilirsiniz. Azure Stack portalları her biri, Azure Resource Manager ayrı örnekleri tarafından desteklenir. **Azure Stack işleci** , Azure Stack yönetmek ve kiracı teklifleri oluşturma gibi işlemleri yapmak ve tümleşik sistemin sistem durumunu ve durumunu izlemek için yönetim portalını kullanır. Kullanıcı Portalı (Kiracı portalı olarak da adlandırılır), sanal makineler, depolama hesapları ve Web uygulamaları gibi bulut kaynaklarının tüketimine yönelik bir self servis deneyimi sağlar. 

> Yönetim portalını kullanarak Azure Stack yönetme hakkında daha fazla bilgi için [Azure Stack yönetim portalı hızlı](azure-stack-manage-portals.md)başlangıcını kullanma bölümüne bakın.

Azure Stack operatörü olarak, [sanal makineler](azure-stack-tutorial-tenant-vm.md), [Web uygulamaları](azure-stack-app-service-overview.md), yüksek oranda kullanılabilir [SQL Server](azure-stack-tutorial-sql.md)ve [MySQL Server](azure-stack-tutorial-mysql.md) veritabanları gibi çok çeşitli hizmet ve uygulamalar sağlayabilirsiniz. SharePoint, Exchange ve daha fazlasını dağıtmak için [Azure Stack hızlı başlangıç Azure Resource Manager şablonlarını](https://github.com/Azure/AzureStack-QuickStart-Templates) da kullanabilirsiniz. 

Yönetim portalını kullanarak, planları, kotaları, teklifleri ve abonelikleri kullanarak kiracılar için [hizmet sunmaya yönelik Azure Stack yapılandırabilirsiniz](service-plan-offer-subscription-overview.md) . Kiracı kullanıcıları birden çok teklife abone olabilir. Tekliflerin bir veya daha fazla planı olabilir ve planlar bir veya daha fazla hizmete sahip olabilir. İşleçler Ayrıca kapasiteyi yönetir ve uyarılara yanıt verir. 

Azure Stack yapılandırıldığında, bir **Azure Stack Kullanıcı** (kiracı olarak da bilinir), işlecin sunduğu hizmetleri kullanır. Kullanıcılar, abone oldukları Hizmetleri (Web Apps, depolama ve sanal makineler gibi) sağlayabilir, izleyebilir ve yönetebilir.

> ' Yi Azure Stack yönetme hakkında daha fazla bilgi edinmek için, hangi hesapların nerede, tipik operatör sorumluluklarına, kullanıcılarınıza ne söyleyeceğinizi ve yardım alma hakkında daha fazla bilgi edinmek için [Azure Stack yönetim temellerini](azure-stack-manage-basics.md)inceleyin.

## <a name="resource-providers"></a>Kaynak sağlayıcıları 
Kaynak sağlayıcıları tüm Azure Stack IaaS ve PaaS hizmetleri için temel oluşturan Web hizmetlerdir. Azure Resource Manager hizmetlere erişim sağlamak için farklı kaynak sağlayıcılarına bağımlıdır. Her kaynak sağlayıcı ilgili kaynaklarını yapılandırmanıza ve denetlemenize yardımcı olur. Hizmet yöneticileri, yeni özel kaynak sağlayıcıları da ekleyebilir. 

### <a name="foundational-resource-providers"></a>Temel kaynak sağlayıcıları 
Üç temel IaaS kaynak sağlayıcısı vardır: 

- **İşlem**. Işlem kaynak sağlayıcısı, Azure Stack kiracının kendi sanal makinelerini oluşturmalarına olanak tanır. Işlem kaynak sağlayıcısı, sanal makinelerin yanı sıra sanal makine uzantıları oluşturma özelliğini de içerir. Sanal makine uzantısı hizmeti, Windows ve Linux sanal makineleri için IaaS özellikleri sağlamaya yardımcı olur. Örnek olarak, VM 'yi yapılandırmak için bir Linux sanal makinesi sağlamak ve dağıtım sırasında Bash betiklerini çalıştırmak için Işlem Kaynak sağlayıcısını kullanabilirsiniz.
- **Ağ kaynak sağlayıcısı**. Ağ kaynak sağlayıcısı, özel bulut için bir dizi yazılım tanımlı ağ (SDN) ve ağ Işlevi sanallaştırma (NGD) özelliği sunar. Yazılım yük dengeleyiciler, genel IP 'Ler, ağ güvenlik grupları ve sanal ağlar gibi kaynaklar oluşturmak için ağ kaynak sağlayıcısını kullanabilirsiniz.
- **Depolama kaynak sağlayıcısı**. Depolama kaynak sağlayıcısı, Azure ile tutarlı dört depolama hizmeti sunar: [BLOB](https://docs.microsoft.com/azure/storage/common/storage-introduction#blob-storage), [kuyruk](https://docs.microsoft.com/azure/storage/common/storage-introduction#queue-storage), [tablo](https://docs.microsoft.com/azure/storage/common/storage-introduction#table-storage)ve [keykasa](https://docs.microsoft.com/azure/key-vault/) hesap yönetimi, parolalar ve sertifikalar gibi gizli dizileri yönetme ve denetleme olanağı sağlar. Depolama kaynak sağlayıcısı Ayrıca, Azure ile tutarlı depolama hizmetleri 'nin hizmet sağlayıcısı yönetimini kolaylaştırmak için bir depolama bulut yönetim hizmeti sunar. Azure depolama, Azure Blob 'larına sahip belgeler ve medya dosyaları ve Azure tabloları ile yapılandırılmış NoSQL tabanlı veriler gibi büyük miktarlarda yapılandırılmamış verileri depolama ve alma esnekliği sağlar. 

### <a name="optional-resource-providers"></a>İsteğe bağlı kaynak sağlayıcıları
Azure Stack ile dağıtabileceğiniz ve kullanabileceğiniz üç isteğe bağlı PaaS kaynak sağlayıcısı vardır: 

- **App Service**. [Azure Stack Azure App Service](azure-stack-app-service-overview.md) , Azure Stack Microsoft Azure sunulan bir hizmet olarak platform (PaaS) tekliftir. Hizmet, iç veya dış müşterilerinizin herhangi bir platform veya cihaz için Web, API ve Azure Işlevleri uygulamaları oluşturmalarına olanak sağlar. 
- **SQL Server**. SQL veritabanlarını Azure Stack bir hizmet olarak sunmak için [SQL Server kaynak sağlayıcısını](azure-stack-sql-resource-provider.md) kullanın. Kaynak sağlayıcısını yükledikten ve bir veya daha fazla SQL Server örneğine bağladığınızda, siz ve kullanıcılarınız bulutta yerel uygulamalar, SQL kullanan Web siteleri ve SQL kullanan diğer iş yükleri için veritabanları oluşturabilir.
- **MySQL sunucusu**. MySQL veritabanlarını Azure Stack bir hizmet olarak göstermek için [MySQL Server kaynak sağlayıcısını](azure-stack-mysql-resource-provider-deploy.md) kullanın. MySQL kaynak sağlayıcısı, bir Windows Server 2016 Server Core sanal makinesi (VM) üzerinde bir hizmet olarak çalışır.

## <a name="providing-high-availability"></a>Yüksek kullanılabilirlik sağlama
Azure 'da çoklu VM üretim sisteminin yüksek oranda kullanılabilir olmasını sağlamak için, VM 'Ler birden çok hata etki alanına ve güncelleştirme etki alanlarına yayılan bir [kullanılabilirlik kümesine](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) yerleştirilir. Azure Stack küçük ölçekte, bir kullanılabilirlik kümesindeki bir hata etki alanı, ölçek biriminde tek bir düğüm olarak tanımlanır.  

Azure Stack altyapısı hatalara karşı zaten dayanıklı olsa da, temel alınan teknoloji (Yük Devretme Kümelemesi), bir donanım arızası olması durumunda etkilenen fiziksel sunucudaki VM 'Ler için bazı kesinti süreleri doğurur. Azure Stack, en fazla üç hata etki alanı içeren bir kullanılabilirlik kümesinin Azure ile tutarlı olmasını destekler.

- **Hata etki alanları**. Bir kullanılabilirlik kümesine yerleştirilmiş VM 'Ler, birden çok hata etki alanına (Azure Stack düğümünde) olabildiğince eşit bir şekilde yayarak birbirinden fiziksel olarak yalıtılır. Bir donanım hatası varsa, başarısız olan etki alanındaki VM 'Ler diğer hata etki alanlarında yeniden başlatılır, ancak mümkünse aynı Kullanılabilirlik kümesindeki diğer VM 'lerden ayrı hata etki alanlarında tutulur. Donanım yeniden çevrimiçi olduğunda, yüksek kullanılabilirlik sağlamak için VM 'Ler yeniden dengelenebilir. 
 
- **Etki alanlarını güncelleştirin**. Güncelleştirme etki alanları, kullanılabilirlik kümelerinde yüksek kullanılabilirlik sağlayan başka bir Azure kavramıdır. Bir güncelleştirme etki alanı, aynı anda Bakımı düşük bir şekilde temel alınan donanımların mantıksal grubudur. Aynı güncelleştirme etki alanında bulunan VM 'Ler, planlı bakım sırasında birlikte yeniden başlatılır. Kiracılar bir kullanılabilirlik kümesi içinde VM 'Ler oluştururken, Azure platformu VM 'Leri Bu güncelleştirme etki alanlarına otomatik olarak dağıtır. Azure Stack, VM 'Ler, temel ana bilgisayar güncellenmadan önce kümedeki diğer çevrimiçi ana bilgisayarlar arasında dinamik olarak geçirilir. Bir konak güncelleştirmesi sırasında kiracı kapalı kalma süresi olmadığından, Azure Stack etki alanı güncelleştirme özelliği yalnızca Azure ile şablon uyumluluğu için vardır. Bir kullanılabilirlik kümesindeki VM 'Ler, portalda güncelleştirme etki alanı numarası olarak **0** gösterir. 

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi
Rol tabanlı erişim denetimi 'ni (RBAC), bir abonelik, kaynak grubu veya tek kaynak düzeyinde roller atayarak yetkili kullanıcılara, gruplara ve hizmetlere sistem erişimi sağlamak için kullanabilirsiniz. Her rol, bir Kullanıcı, Grup veya hizmetin Microsoft Azure Stack kaynakları üzerinde sahip olduğu erişim düzeyini tanımlar.

RBAC Azure Stack tüm kaynak türleri için uygulanan üç temel role sahiptir: sahip, katkıda bulunan ve okuyucu. Sahibinin, başkalarına erişim yetkisi verme hakkı dahil tüm kaynaklara tam erişimi vardır. Katkıda bulunan her türlü Azure kaynağı oluşturabilir ve yönetebilir, ancak diğer kullanıcılara erişim izni veremez. Okuyucu yalnızca mevcut kaynakları görüntüleyebilir. RBAC rollerinin geri kalanı belirli Azure kaynaklarının yönetimine izin verir. Örneğin, sanal makine katılımcısı rolü sanal makinelerin oluşturulmasına ve yönetilmesine izin verir, ancak sanal ağın veya sanal makinenin bağlandığı alt ağın yönetimine izin vermez.

> Daha fazla bilgi için bkz. [rol tabanlı erişim denetimini yönetme](azure-stack-manage-permissions.md) . 

## <a name="reporting-usage-data"></a>Kullanım verilerini raporlama
Azure Stack tüm kaynak sağlayıcıları genelinde kullanım verilerini toplayıp toplar ve Azure ticareti tarafından işlenmek üzere Azure 'a aktarır. Azure Stack toplanan kullanım verileri bir REST API üzerinden görüntülenebilir. Tüm kiracı aboneliklerinde kullanım verilerini almak için sağlayıcı ve sağlayıcı API 'Lerinin yanı sıra, Azure ile tutarlı bir Kiracı API 'SI de vardır. Bu veriler, faturalandırma veya geri ödeme için bir dış araçla veya hizmetle tümleştirilecek şekilde kullanılabilir. Kullanım Azure ticareti tarafından işlendikten sonra Azure Faturalandırma portalında görüntülenebilir.

> [Kullanım verilerini Azure 'a raporlama Azure Stack](azure-stack-usage-reporting.md)hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack ve küresel Azure karşılaştırması](compare-azure-azure-stack.md)

[Yönetim Temelleri](azure-stack-manage-basics.md)

[Hızlı başlangıç: Azure Stack yönetim portalını kullanma](azure-stack-manage-portals.md)