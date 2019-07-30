---
title: Azure ve Azure Stack uygulamalarıyla karma bulut kimliğini yapılandırma | Microsoft Docs
description: Azure ve Azure Stack uygulamalarıyla karma bulut kimliğini yapılandırmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: bryanla
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/26/2019
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 06/26/2019
ms.openlocfilehash: bc614cb514b9d35523749944486aa375bb8fce9c
ms.sourcegitcommit: 35b13ea6dc0221a15cd0840be796f4af5370ddaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602998"
---
# <a name="configure-hybrid-cloud-identity-for-azure-and-azure-stack-applications"></a>Azure ve Azure Stack uygulamaları için karma bulut kimliğini yapılandırma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure ve Azure Stack uygulamalarınız için karma bulut kimliğini yapılandırmayı öğrenin.

Hem küresel Azure hem de Azure Stack uygulamalarınıza erişim vermek için iki seçeneğiniz vardır.

 * Azure Stack internete sürekli bir bağlantı olduğunda, Azure Active Directory (Azure AD) kullanabilirsiniz.
 * Azure Stack internet bağlantısı kesildiğinde Azure Directory Federasyon Hizmetleri 'ni (AD FS) kullanabilirsiniz.

Hizmet sorumlularını, Azure Stack Azure Resource Manager kullanarak dağıtım veya yapılandırma için Azure Stack uygulamalarınıza erişim vermek üzere kullanırsınız.

Bu çözümde, aşağıdakileri yapmak için bir örnek ortam oluşturacaksınız:

> [!div class="checklist"]
> - Küresel Azure ve Azure Stack karma kimlik oluşturma
> - Azure Stack API 'sine erişmek için bir belirteç alın.

Bu çözümdeki adımlarda Azure Stack operatör izinlerinizin olması gerekir.

> [!Tip]  
> ![karma pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack, Azure'nın bir uzantısıdır. Azure Stack, bulut bilgi işlemin çevikliğini ve yeniliklerini şirket içi ortamınıza getirerek, karma uygulamaları her yerde oluşturup dağıtmanıza imkan tanıyan tek karma bulutu etkinleştirir.  
> 
> [Karma uygulamalar Için tasarım konuları](azure-stack-edge-pattern-overview.md) , karma uygulamalar tasarlamak, dağıtmak ve çalıştırmak için yazılım kalitesine (yerleştirme, ölçeklenebilirlik, kullanılabilirlik, dayanıklılık, yönetilebilirlik ve güvenlik) göre önemli noktalar inceler. Tasarım konuları karma uygulama tasarımını iyileştirirken, üretim ortamlarındaki zorlukları en aza indirmeyle ilgili olarak size yardımcı olur.


## <a name="create-a-service-principal-for-azure-ad-in-the-portal"></a>Portalda Azure AD için bir hizmet sorumlusu oluşturma

Kimlik deposu olarak Azure AD 'yi kullanarak Azure Stack dağıttıysanız, Azure 'da olduğu gibi hizmet sorumluları oluşturabilirsiniz. [Kaynaklara erişmek için bir uygulama kimliği kullanmak](../operator/azure-stack-create-service-principals.md#manage-an-azure-ad-service-principal) , bu adımları Portal üzerinden nasıl gerçekleştirekullanacağınızı gösterir. Başlamadan önce [gerekli Azure AD izinlerine](/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) sahip olduğunuzdan emin olun.

## <a name="create-a-service-principal-for-ad-fs-using-powershell"></a>PowerShell kullanarak AD FS için hizmet sorumlusu oluşturma

AD FS Azure Stack dağıttıysanız, bir hizmet sorumlusu oluşturmak, erişim için bir rol atamak ve bu kimliği kullanarak PowerShell 'den oturum açmak için PowerShell 'i kullanabilirsiniz. [Kaynaklara erişmek için bir uygulama kimliği kullanmak](../operator/azure-stack-create-service-principals.md#manage-an-ad-fs-service-principal) , PowerShell kullanarak gerekli adımların nasıl gerçekleştirileceğini gösterir.

## <a name="using-the-azure-stack-api"></a>Azure Stack API 'sini kullanma

[Azure Stack API](azure-stack-rest-api-use.md) çözümü, Azure Stack API 'sine erişmek için belirteç alma sürecinde size yol gösterir.

## <a name="connect-to-azure-stack-using-powershell"></a>PowerShell kullanarak Azure Stack bağlanma

[Azure Stack PowerShell ile çalışmaya başlamanız için](../operator/azure-stack-powershell-install.md) hızlı başlangıç, Azure PowerShell yüklemek ve Azure Stack yüklemenize bağlanmak için gereken adımlarda size yol gösterir.

### <a name="prerequisites"></a>Önkoşullar

Erişebileceğiniz bir abonelikle Azure Active Directory bağlı Azure Stack yüklemesi gerekir. Azure Stack yüklemeniz yoksa, bu yönergeleri kullanarak bir [Azure Stack geliştirme seti](../asdk/asdk-install.md)ayarlayabilirsiniz.

#### <a name="connect-to-azure-stack-using-code"></a>Kodu kullanarak Azure Stack bağlanma

Kodu kullanarak Azure Stack bağlanmak için Azure Resource Manager uç noktaları API 'sini kullanarak Azure Stack yüklemenizin kimlik doğrulama ve grafik uç noktalarını alın ve ardından REST istekleri kullanarak kimlik doğrulaması yapın. [GitHub](https://github.com/shriramnat/HybridARMApplication)üzerinde örnek bir istemci uygulaması bulabilirsiniz.

>[!Note]
>Tercih ettiğiniz dilde Azure SDK 'sı Azure API profillerini destekliyorsa, SDK Azure Stack ile çalışmayabilir. Azure API profilleri hakkında daha fazla bilgi edinmek için [API sürüm profillerini yönetme](azure-stack-version-profiles.md) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

 - Azure Stack ' de kimliğin nasıl işlendiği hakkında daha fazla bilgi için bkz. [Azure Stack Için kimlik mimarisi](../operator/azure-stack-identity-architecture.md).
 - Azure bulut desenleri hakkında daha fazla bilgi için bkz: [bulut tasarımı desenleri](https://docs.microsoft.com/azure/architecture/patterns).
