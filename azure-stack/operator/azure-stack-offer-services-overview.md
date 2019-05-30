---
title: Azure stack'teki hizmetleri sunan | Microsoft Docs
description: Bulut operatörü olarak, kullanıcılarınıza hizmet sunabilir.
services: azure-stack
documentationcenter: ''
author: BryanLa
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: mabrigg
ms.reviewer: unknown
ms.lastreviewed: 09/17/2018
ms.openlocfilehash: a85f7398e492a2d49c2f6c1b686f2e15042b0444
ms.sourcegitcommit: 797dbacd1c6b8479d8c9189a939a13709228d816
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66268364"
---
# <a name="overview-of-offering-services-in-azure-stack"></a>Azure stack'teki hizmetleri sunan genel bakış

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

[Microsoft Azure Stack](azure-stack-overview.md) merkezinizden Hizmetleri elde etmenizi sağlayan bir karma bulut platformudur. Bir hizmet sağlayıcısı olarak, kiracılarınıza hizmet sunabilir. Bir iş veya kamu kurumunda içinde çalışanlarınıza şirket içi hizmet sunabilir. 

Teklif [hizmet olarak altyapı](https://azure.microsoft.com/overview/what-is-iaas/) isteğe bağlı bilgi işlem altyapısı oluşturmak sağlayın (Iaas) Hizmetleri sağlanan ve Azure Stack Kullanıcı Portalı'ndan yönetilen.

Ayrıca dağıtabilirsiniz [bir hizmet olarak Platform](https://azure.microsoft.com/overview/what-is-paas/) Microsoft ve diğer 3. taraf sağlayıcılar Azure Stack için (PaaS) Hizmetleri. Sunabileceğiniz hizmetlerin dahil ancak bunlarla sınırlı değildir:

- [Azure Stack’e App Service kaynak sağlayıcısı ekleme](azure-stack-app-service-overview.md)

- [Azure Stack için bir SQL Server Kaynak sağlayıcısı ekleme](azure-stack-sql-resource-provider-deploy.md)

- [Azure Stack'e bir MySQL Server Kaynak sağlayıcısı ekleme](azure-stack-mysql-resource-provider-deploy.md)


Hizmetleri tümleştirin ve farklı kullanıcılar için karmaşık çözümlerinizi derlemek için bile birleştirebilirsiniz.

Kullanıcılarınız için bu hizmetleri sunmak için öncelikle oluşturmanız gerekir [planlar, teklifler ve kotalar](azure-stack-plan-offer-quota-overview.md). Kullanıcılarınızın sonra hizmetleri kullanmak üzere tekliflerinize abone olabilirsiniz.

## <a name="plan-your-service-offers"></a>Plan, hizmet teklifleri

Tekliflerinizi planlıyorsanız, aşağıdaki noktaları göz önünde bulundurun:

**Deneme tekliflerinden**: Deneme tekliflerinden daha sonra ek Hizmetleri'ne yükseltebilirsiniz yeni kullanıcılar çekmek için kullanabilirsiniz. Bir deneme teklifi oluşturmak için küçük bir oluşturma [temel plan](azure-stack-plan-offer-quota-overview.md#base-plan) isteğe bağlı daha büyük bir eklenti planı ile.

**Kapasite planlama**: Kaynaklar ve tüm kullanıcılar için sistem tıka basa dolduruyor büyük miktarlarda alın kullanıcıların endişe olabilir. Performans yardımcı olmak için [planlarınızı kotaları yapılandırın](azure-stack-plan-offer-quota-overview.md#plans) harf kullanımı.

**Sağlayıcı temsilcisi**: Başkalarının oluşturma olanağı sunan, ortamınızda verebilirsiniz. Örneğin, bir hizmet sağlayıcı barındırıyorsanız, yapabilecekleriniz [temsilci](azure-stack-delegated-provider.md) , satıcıları bu yeteneği. Veya, siz bir kuruluş gibiyseniz, diğer bölümler/kuruluşlarının devredebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack'te teklif oluşturma](azure-stack-create-offer.md)
