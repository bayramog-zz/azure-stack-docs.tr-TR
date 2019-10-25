---
title: Azure Stack Hizmetleri, planları, teklifleri, abonelikleri genel bakış | Microsoft Docs
description: Azure Stack Hizmetleri, planları, teklifleri ve abonelikleri 'ne genel bakış.
services: azure-stack
documentationcenter: ''
author: BryanLa
manager: femila
editor: ''
ms.service: azure-stack
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: bryanla
ms.reviewer: efemmano
ms.lastreviewed: 10/01/2019
ms.openlocfilehash: 2242da61eaa457a8cc234fb3310787eb26b9148e
ms.sourcegitcommit: acebda8a42ac8ecdeba490fc1738e9041479dab0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72814065"
---
# <a name="azure-stack-services-plans-offers-subscriptions-overview"></a>Azure Stack Hizmetleri, planları, teklifleri, abonelikleri genel bakış

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

[Microsoft Azure Stack](azure-stack-overview.md) , veri merkezinizden hizmet sunmanıza olanak tanıyan bir karma bulut platformudur. Hizmetler arasında sanal makineler (VM), SQL Server veritabanları, SharePoint, Exchange ve hatta [Azure Market öğeleri](azure-stack-marketplace-azure-items.md)bulunur. Hizmet sağlayıcı olarak, kiracılarınıza hizmet sunabilir. Bir iş veya devlet kurumu içinde, çalışanlarınıza şirket içi hizmetler sunabilirsiniz.

## <a name="overview"></a>Genel Bakış

Azure Stack operatörü olarak, teklifleri, planları ve abonelikleri kullanarak hizmetleri yapılandırıp teslim edersiniz. Teklifler bir veya daha fazla plan içerir ve her bir plan, her biri kotalar ile yapılandırılmış bir veya daha fazla hizmet içerir. Planlar oluşturup bunları farklı tekliflerle birleştirerek, kullanıcılar tekliflerinize abone olabilir ve kaynakları dağıtabilir. Bu yapı şunları yönetmenizi sağlar:

- Kullanıcılarınızın erişebileceği hizmet ve kaynaklar.
- Kullanıcıların tüketebileceği kaynak miktarı.
- Kaynaklara erişimi olan bölgeler.

Bir hizmet sunmak için aşağıdaki üst düzey adımları izleyin:

1. Aşağıdakileri kullanarak hizmet teklifinizi planlayın:

   - İşlem, depolama, ağ veya Key Vault gibi temel hizmetler.
   - App Service, SQL Server veya MySQL Server gibi eklenti hizmetleri.

2. Bir veya daha fazla hizmetten oluşan bir plan oluşturun. Bir plan oluştururken, plandaki her bir hizmetin kaynak sınırlarını tanımlayan Kotalar ' ı seçin veya oluşturun.
3. Bir veya daha fazla plan içeren bir teklif oluşturun. Teklif, temel planları ve isteğe bağlı eklenti planlarını içerebilir.

Teklifi oluşturduktan sonra, kullanıcılarınız hizmetlere erişmek ve kaynakları dağıtmak için bu hizmete abone olabilir. Kullanıcılar istedikleri sayıda teklife abone olabilir. Aşağıdaki şekilde, iki tekliflere abone olan bir kullanıcıya ilişkin basit bir örnek gösterilmektedir. Her teklifin bir planı veya ikisi vardır ve her plan onlara belirli hizmetlere erişim sağlar.

![Tekliflerle ve planlarla kiracı aboneliği](media/azure-stack-key-features/image4.png)

## <a name="services"></a>Hizmetler

Kullanıcılarınızın Azure Stack Kullanıcı portalından sağlanmış ve yönetilen bir isteğe bağlı bilgi işlem altyapısı oluşturmasını sağlayan [hizmet olarak altyapı](https://azure.microsoft.com/overview/what-is-iaas/) (IaaS) Hizmetleri sunabilirsiniz.

Ayrıca, Microsoft ve diğer üçüncü taraf sağlayıcılardan Azure Stack için [hizmet olarak platform](https://azure.microsoft.com/overview/what-is-paas/) (PaaS) hizmetleri de dağıtabilirsiniz. Sunabilmeniz gereken PaaS hizmetleri şunlardır ancak bunlarla sınırlı değildir:

- [App Service](azure-stack-app-service-overview.md)
- [SQL Server](azure-stack-sql-resource-provider-deploy.md)
- [MySQL sunucusu](azure-stack-mysql-resource-provider-deploy.md)

Ayrıca, farklı kullanıcılar için karmaşık çözümler bütünleştirmek ve derlemek üzere Hizmetleri birleştirebilirsiniz.

### <a name="quotas"></a>Kotalar

Bulut kapasitenizi yönetmeye yardımcı olmak için önceden yapılandırılmış *kotaları*kullanabilir veya bir plandaki her hizmet için yeni bir kota oluşturabilirsiniz. Kotalar, bir Kullanıcı aboneliğinin sağlayabileceği veya tüketebileceği üst kaynak sınırlarını tanımlar. Örneğin bir kullanıcının en fazla beş VM oluşturmasına izin veren bir kota oluşturabilirsiniz.

> [!IMPORTANT]
> Yeni kotaların Kullanıcı portalında veya değiştirilen bir kota zorlanmadan önce kullanılabilmesi iki saate kadar sürebilir.

Kotaları bölgeye göre ayarlayabilirsiniz. Örneğin, A bölgesi için işlem hizmetleri sağlayan bir planda iki VM kotası olabilir.

>[!NOTE]
>Azure Stack Geliştirme Seti (ASDK) içinde, yalnızca bir bölge ( *Yerel*olarak adlandırılır) kullanılabilir.

[Azure Stack 'de kota türleri](azure-stack-quota-types.md)hakkında daha fazla bilgi edinin.

## <a name="plans"></a>Planlar

Planlar, bir veya birden fazla hizmetten oluşan gruplardır. Azure Stack operatörü olarak kullanıcılarınıza sunmak için [planlar oluşturursunuz](azure-stack-create-plan.md) . Böylece kullanıcılarınız tekliflerinize abone olarak, bu tekliflerin içerdiği planları ve hizmetleri kullanabilir. Planlar oluştururken, kotalarınızı ayarladığınızdan, temel planlarınızı tanımladığınız ve isteğe bağlı eklenti planları dahil etmeyi göz önünde bulundurduğunuzdan emin olun.

### <a name="base-plan"></a>Temel plan

Bir teklif oluştururken, hizmet yöneticisi bir temel plan içerebilir. Bu temel planlar, bir Kullanıcı bu teklifine abone olduğunda varsayılan olarak dahil edilir. Bir kullanıcı abone olduğunda, bu temel planlarda belirtilen tüm kaynak sağlayıcılarına erişimi vardır (ilgili kotalarla birlikte).

### <a name="add-on-plans"></a>Eklenti planları

Eklenti planları, bir teklifine eklediğiniz isteğe bağlı planlardır. Eklenti planları abonelikte varsayılan olarak dahil edilmez. Eklenti planları, bir abonenin aboneliklerine ekleyebileceği bir teklifte ek planlardır (kotalar ile). Örneğin, deneme için sınırlı kaynaklarla bir temel plan ve hizmeti benimsemeye karar veren müşterilere daha fazla önemli kaynak içeren bir eklenti planı sunabilirsiniz.

## <a name="offers"></a>Teklifler

, Kullanıcıların bunlara abone olabilmesi için oluşturduğunuz bir veya daha fazla plan gruplarıdır. Örneğin: teklif Alpha, bir dizi işlem hizmeti sağlayan plan A ve bir dizi depolama ve ağ hizmeti sağlayan plan B 'yi içerebilir.

[Bir teklif oluşturduğunuzda](azure-stack-create-offer.md), en az bir temel plan eklemeniz gerekir, ancak kullanıcıların aboneliğine ekleyebileceği eklenti planları da oluşturabilirsiniz.

Tekliflerinizi planlarken, aşağıdaki noktaları göz önünde bulundurun:

**Deneme teklifleri**: deneme tekliflerini, daha sonra ek hizmetlere yükseltebilen yeni kullanıcılar için çekmek üzere kullanırsınız. Deneme teklifi oluşturmak için, isteğe bağlı daha büyük eklenti planına sahip küçük bir [temel plan](service-plan-offer-subscription-overview.md#base-plan) oluşturun. Alternatif olarak, küçük bir temel plandan ve daha büyük bir "Kullandıkça öde" planına sahip ayrı bir tekliften oluşan bir deneme teklifi oluşturabilirsiniz.

**Kapasite planlama**: büyük miktarlarda kaynak alan kullanıcılar ve sistemi tüm kullanıcılar için günlüğe kaydet konusunda endişe duyabilirsiniz. Performansa yardımcı olmak için, planlarınızı kullanım kullanımı için [Kotalar ile yapılandırabilirsiniz](service-plan-offer-subscription-overview.md#plans) .

**Atanan sağlayıcılar**: başkalarına ortamınızda teklifler oluşturma olanağı verebilirsiniz. Örneğin, bir hizmet sağlayıcısıysanız, satıcılarınız için bu yeteneği [temsilci](azure-stack-delegated-provider.md) seçebilirsiniz. Ya da bir kurumunuzda, diğer bölümlere/yan kuruluşlarla temsilci atayabilirsiniz.

## <a name="subscriptions"></a>Abonelikler

Abonelikler, kullanıcıların tekliflerinize erişmesine izin verir. Bir hizmet sağlayıcısı için Azure Stack işletmeni kullanıyorsanız, kullanıcılarınız (kiracılar) tekliflerinize abone olarak hizmetlerinizi satın alır. Bir kuruluşta Azure Stack işleçseniz kullanıcılarınız (çalışanlar), ödeyerek Sunduğunuz hizmetlere abone olabilir.

Kullanıcılar yeni abonelikler oluşturur ve Azure Stack oturum açarak mevcut aboneliklere erişim sağlar. Her abonelik tek bir teklifle bir ilişkilendirmeyi temsil eder. Bir aboneliğe atanan teklif (ve planları ve kotaları) diğer aboneliklerle paylaşılamaz. Bir kullanıcının oluşturduğu her kaynak bir abonelikle ilişkilendirilir.

### <a name="default-provider-subscription"></a>Varsayılan sağlayıcı aboneliği

Varsayılan sağlayıcı aboneliği, ASDK 'yi dağıtırken otomatik olarak oluşturulur. Bu abonelik Azure Stack yönetmek, ek kaynak sağlayıcıları dağıtmak ve kullanıcılar için planlar ve teklifler oluşturmak için kullanılabilir. Güvenlik ve lisanslama nedenleriyle, müşteri iş yüklerini ve uygulamalarını çalıştırmak için kullanılmamalıdır. Varsayılan sağlayıcı aboneliğinin kotası değiştirilemez.

## <a name="next-steps"></a>Sonraki adımlar

Planlar, teklifler ve abonelikler oluşturma hakkında daha fazla bilgi edinmek için [plan oluşturma](azure-stack-create-plan.md)ile başlayın.
