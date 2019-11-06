---
title: Azure ve Azure Stack hub uygulamaları için karma bulut kimliğini yapılandırma
description: Azure ve Azure Stack hub uygulamaları için karma bulut kimliğini yapılandırmayı öğrenin.
author: BryanLa
ms.service: azure-stack
ms.topic: article
ms.date: 11/05/2019
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 11/05/2019
ms.openlocfilehash: a38a7541d4184e89cb0a821db04a2fcb8c267a31
ms.sourcegitcommit: 5c92a669007ab4aaffe4484f1d8836a40340dde1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73638304"
---
# <a name="configure-hybrid-cloud-identity-for-azure-and-azure-stack-hub-applications"></a>Azure ve Azure Stack hub uygulamaları için karma bulut kimliğini yapılandırma

*Uygulama hedefi: Azure Stack hub tümleşik sistemleri ve Azure Stack hub Development Kit*

Azure ve Azure Stack hub uygulamalarınız için karma bulut kimliğini yapılandırmayı öğrenin.

Hem genel Azure hem de Azure Stack hub 'ında uygulamalarınıza erişim vermek için iki seçeneğiniz vardır.

 * Azure Stack hub 'ının internete sürekli bağlantısı olduğunda, Azure Active Directory (Azure AD) kullanabilirsiniz.
 * Azure Stack hub 'ı Internet bağlantısı kesildiğinde, Azure Directory Federasyon Hizmetleri 'ni (AD FS) kullanabilirsiniz.

Hizmet sorumlularını, Azure Stack hub 'daki Azure Resource Manager kullanarak dağıtım veya yapılandırma için Azure Stack hub uygulamalarınıza erişim vermek üzere kullanırsınız.

Bu çözümde, aşağıdakileri yapmak için bir örnek ortam oluşturacaksınız:

> [!div class="checklist"]
> - Küresel Azure ve Azure Stack hub 'da karma kimlik oluşturma
> - Azure Stack hub API 'sine erişmek için bir belirteç alın.

Bu çözümdeki adımlarda Azure Stack hub işleci izinlerinizin olması gerekir.

> [!Tip]  
> ![Hybrid-Pillars. png](./media/solution-deployment-guide-cross-cloud-scaling/hybrid-pillars.png)  
> Microsoft Azure Stack hub, Azure uzantısıdır. Azure Stack hub, bulut bilgi işlemin çevikliğini ve yeniliklerini şirket içi ortamınıza sunarak, karma uygulamaları her yerde derleyip dağıtmanıza imkan tanıyan tek karma bulutu etkinleştirir.  
> 
> [Karma uygulamalar Için tasarım konuları](overview-app-design-considerations.md) , karma uygulamalar tasarlamak, dağıtmak ve çalıştırmak için yazılım kalitesine (yerleştirme, ölçeklenebilirlik, kullanılabilirlik, dayanıklılık, yönetilebilirlik ve güvenlik) göre önemli noktalar inceler. Tasarım konuları karma uygulama tasarımını iyileştirirken, üretim ortamlarındaki zorlukları en aza indirmeyle ilgili olarak size yardımcı olur.


## <a name="create-a-service-principal-for-azure-ad-in-the-portal"></a>Portalda Azure AD için bir hizmet sorumlusu oluşturma

Kimlik deposu olarak Azure AD 'yi kullanarak Azure Stack hub dağıttıysanız, Azure için yaptığınız gibi hizmet sorumluları oluşturabilirsiniz. [Kaynaklara erişmek için bir uygulama kimliği kullanmak](../operator/azure-stack-create-service-principals.md#manage-an-azure-ad-service-principal) , bu adımları Portal üzerinden nasıl gerçekleştirekullanacağınızı gösterir. Başlamadan önce [gerekli Azure AD izinlerine](/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) sahip olduğunuzdan emin olun.

## <a name="create-a-service-principal-for-ad-fs-using-powershell"></a>PowerShell kullanarak AD FS için hizmet sorumlusu oluşturma

AD FS ile Azure Stack hub 'ı dağıttıysanız, bir hizmet sorumlusu oluşturmak, erişim için bir rol atamak ve bu kimliği kullanarak PowerShell 'den oturum açmak için PowerShell 'i kullanabilirsiniz. [Kaynaklara erişmek için bir uygulama kimliği kullanmak](../operator/azure-stack-create-service-principals.md#manage-an-ad-fs-service-principal) , PowerShell kullanarak gerekli adımların nasıl gerçekleştirileceğini gösterir.

## <a name="using-the-azure-stack-hub-api"></a>Azure Stack hub API 'sini kullanma

[Azure Stack hub API](../user/azure-stack-rest-api-use.md) çözümü, Azure Stack hub API 'sine erişmek için belirteç alma sürecinde size yol gösterir.

## <a name="connect-to-azure-stack-hub-using-powershell"></a>PowerShell kullanarak Azure Stack hub 'a bağlanma

[Azure Stack hub 'Da PowerShell ile çalışmaya başlamanız için](../operator/azure-stack-powershell-install.md) hızlı başlangıç, Azure PowerShell yüklemek ve Azure Stack hub yüklemenize bağlanmak için gereken adımlarda size yol gösterir.

### <a name="prerequisites"></a>Önkoşullar

Azure Stack hub yüklemesi, erişebileceğiniz bir abonelikle Azure Active Directory bağlı olmalıdır. Azure Stack hub yüklemeniz yoksa, bu yönergeleri kullanarak bir [Azure Stack hub geliştirme setini](../asdk/asdk-install.md)ayarlayabilirsiniz.

#### <a name="connect-to-azure-stack-hub-using-code"></a>Kod kullanarak Azure Stack hub 'a bağlanma

Kodu kullanarak Azure Stack hub 'ına bağlanmak için, Azure Stack hub yüklemenizin kimlik doğrulama ve grafik uç noktalarını almak üzere Azure Resource Manager uç noktaları API 'sini kullanın ve ardından REST istekleri kullanarak kimlik doğrulaması yapın. [GitHub](https://github.com/shriramnat/HybridARMApplication)üzerinde örnek bir istemci uygulaması bulabilirsiniz.

>[!Note]
>Tercih ettiğiniz dilde Azure SDK 'sı Azure API profillerini destekliyorsa, SDK Azure Stack hub ile çalışmayabilir. Azure API profilleri hakkında daha fazla bilgi edinmek için [API sürüm profillerini yönetme](../user/azure-stack-version-profiles.md) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

 - Azure Stack hub 'ında kimliğin nasıl işlendiği hakkında daha fazla bilgi için bkz. [Azure Stack hub Için kimlik mimarisi](../operator/azure-stack-identity-architecture.md).
 - Azure bulut desenleri hakkında daha fazla bilgi edinmek için bkz. [bulut tasarım desenleri](https://docs.microsoft.com/azure/architecture/patterns).
