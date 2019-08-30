---
title: Azure Stack plan, teklif, kota ve aboneliğe genel bakış | Microsoft Docs
description: Bulut operatörü olarak Azure Stack planları, teklifleri, kotaları ve abonelikleri anlamak istiyorum.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 3dc92e5c-c004-49db-9a94-783f1f798b98
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: sethm
ms.reviewer: efemmano
ms.lastreviewed: 08/29/2019
ms.openlocfilehash: 72787bda5dd5fad372af33141e276b4852f86c39
ms.sourcegitcommit: 701685f0b59e5a3d1a8d39fe477b8df701a51cd2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70159509"
---
# <a name="plan-offer-quota-and-subscription-overview"></a>Plan, teklif, kota ve aboneliğe genel bakış

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

[Azure Stack](azure-stack-overview.md) , sanal makineler, SQL Server veritabanları, SharePoint, Exchange ve hatta [Azure Market öğeleri](azure-stack-marketplace-azure-items.md)gibi çok çeşitli hizmetler sunmanızı sağlar. Azure Stack operatörü olarak, planlar, teklifler ve Kotalar kullanarak Azure Stack bu hizmetleri yapılandırıp teslim edersiniz.

Teklifler bir veya daha fazla plan içerir ve her plan bir veya daha fazla hizmet içerir. Planlar oluşturup bunları farklı tekliflerle birleştirerek şunları yönetebilirsiniz:

- Kullanıcılarınızın erişebileceği hizmet ve kaynaklar.
- Kullanıcıların tüketebileceği kaynak miktarı.
- Kaynaklara erişimi olan bölgeler.

Bir hizmet sundığınızda, bu üst düzey adımları izleyin:

1. Kullanıcılarınıza teslim etmek istediğiniz bir hizmet ekleyin.
2. Bir veya daha fazla hizmet içeren bir plan oluşturun. Bir plan oluştururken, plandaki her bir hizmetin kaynak sınırlarını tanımlayan Kotalar ' ı seçin veya oluşturun.
3. Bir veya daha fazla plan içeren bir teklif oluşturun. Teklif, temel planları ve isteğe bağlı eklenti planlarını içerebilir.

Teklifi oluşturduktan sonra kullanıcılarınız, teklifin sağladığı hizmetlere ve kaynaklara erişmek için bu hizmete abone olabilir. Kullanıcılar istedikleri sayıda teklife abone olabilir. Aşağıdaki şekilde, iki tekliflere abone olan bir kullanıcıya ilişkin basit bir örnek gösterilmektedir. Her teklifin bir planı veya ikisi vardır ve her plan onlara hizmetlere erişim sağlar.

![Tekliflerle ve planlarla kiracı aboneliği](media/azure-stack-key-features/image4.png)

## <a name="plans"></a>Planlar

Planlar, bir veya birden fazla hizmetten oluşan gruplardır. Azure Stack operatörü olarak kullanıcılarınıza sunmak için [planlar oluşturursunuz](azure-stack-create-plan.md) . Böylece kullanıcılarınız tekliflerinize abone olarak, bu tekliflerin içerdiği planları ve hizmetleri kullanabilir. Planlar oluştururken, kotalarınızı ayarladığınızdan, temel planlarınızı tanımladığınız ve isteğe bağlı eklenti planları dahil etmeyi göz önünde bulundurduğunuzdan emin olun.

### <a name="quotas"></a>Kotalar

Bulut kapasitenizi yönetmenize yardımcı olmak için önceden yapılandırılmış *kotaları*kullanabilir veya bir plandaki her hizmet için yeni bir kota oluşturabilirsiniz. Kotalar, bir Kullanıcı aboneliğinin sağlayabileceği veya tüketebileceği üst kaynak sınırlarını tanımlar. Örneğin, bir kota, kullanıcının en fazla beş sanal makine (VM) oluşturmasına izin verebilir.

> [!IMPORTANT]
> Yeni kotaların Kullanıcı portalında veya değiştirilen bir kota zorlanmadan önce kullanılabilmesi iki saate kadar sürebilir.

Kotaları bölgeye göre yapılandırabilirsiniz. Örneğin, A bölgesi için işlem hizmetleri sağlayan bir planda iki VM kotası olabilir.

>[!NOTE]
>Azure Stack Geliştirme Seti, yalnızca bir bölge ( *Yerel*olarak adlandırılır) kullanılabilir.

[Azure Stack 'de kota türleri](azure-stack-quota-types.md)hakkında daha fazla bilgi edinin.

### <a name="base-plan"></a>Temel plan

Bir teklif oluştururken, hizmet yöneticisi bir temel plan içerebilir. Bu temel planlar, bir Kullanıcı bu teklifine abone olduğunda varsayılan olarak dahil edilir. Bir kullanıcı abone olduğunda, bu temel planlarda belirtilen tüm kaynak sağlayıcılarına erişimi vardır (ilgili kotalarla birlikte).

### <a name="add-on-plans"></a>Eklenti planları

Eklenti planları, bir teklifine eklediğiniz isteğe bağlı planlardır. Eklenti planları abonelikte varsayılan olarak dahil edilmez. Eklenti planları, bir abonenin aboneliklerine ekleyebileceği bir teklifte ek planlardır (kotalar ile). Örneğin, deneme için sınırlı kaynaklarla bir temel plan ve hizmeti benimsemeye karar veren müşterilere daha fazla önemli kaynak içeren bir eklenti planı sunabilirsiniz.

## <a name="offers"></a>Teklifler

, Kullanıcıların bunlara abone olabilmesi için oluşturduğunuz bir veya daha fazla plan gruplarıdır. Örneğin, teklif Alpha, bir dizi depolama ve ağ hizmeti sağlayan bir dizi işlem hizmeti ve plan B sağlayan bir plan A içerebilir.

[Bir teklif oluşturduğunuzda](azure-stack-create-offer.md), en az bir temel plan eklemeniz gerekir, ancak kullanıcıların aboneliğine ekleyebileceği eklenti planları da oluşturabilirsiniz.

## <a name="subscriptions"></a>Abonelikler

Bir abonelik, kullanıcıların tekliflerinizi nasıl eriştedir. Bir hizmet sağlayıcısı için Azure Stack operatörünüz varsa, kullanıcılarınız (kiracılar) tekliflerinize abone olarak hizmetlerinizi satın alır. Bir kuruluşta Azure Stack işleçseniz kullanıcılarınız (çalışanlar), ödeyerek Sunduğunuz hizmetlere abone olabilir.

Bir kullanıcının teklifini içeren her birleşimi benzersiz bir abonelik olur. Bir kullanıcının birden fazla tekliflere abonelikleri olabilir, ancak her abonelik yalnızca bir teklif için geçerlidir. Planlar, teklifler ve Kotalar yalnızca benzersiz bir abonelik için geçerlidir; abonelikler arasında paylaştırılamaz. Bir kullanıcının oluşturduğu her kaynak bir abonelikle ilişkilendirilir.

### <a name="default-provider-subscription"></a>Varsayılan sağlayıcı aboneliği

Varsayılan sağlayıcı aboneliği, Azure Stack Geliştirme Seti dağıtırken otomatik olarak oluşturulur. Bu abonelik Azure Stack yönetmek, ek kaynak sağlayıcıları dağıtmak ve kullanıcılar için planlar ve teklifler oluşturmak için kullanılabilir. Güvenlik ve lisanslama nedenleriyle, müşteri iş yüklerini ve uygulamalarını çalıştırmak için kullanılmamalıdır. Varsayılan sağlayıcı aboneliğinin kotası değiştirilemez.

## <a name="next-steps"></a>Sonraki adımlar

Planlar ve teklifler hakkında daha fazla bilgi için bkz. [plan oluşturma](azure-stack-create-plan.md).
