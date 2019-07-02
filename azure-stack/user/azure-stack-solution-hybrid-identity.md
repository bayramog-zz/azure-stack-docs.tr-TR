---
title: Hibrit bulut kimliği ile Azure'da ve Azure Stack'te uygulama yapılandırma | Microsoft Docs
description: Hibrit bulut kimliği ile Azure ve Azure Stack uygulamaları yapılandırmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: bryanla
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/26/2019
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 06/26/2019
ms.openlocfilehash: 074d971c1f951797b5dc2d53a62eef56d0b7249f
ms.sourcegitcommit: eccbd0098ef652919f357ef6dba62b68abde1090
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67492334"
---
# <a name="tutorial-configure-hybrid-cloud-identity-for-azure-and-azure-stack-applications"></a>Öğretici: Uygulamaları Azure ve Azure Stack için hibrit bulut kimliği yapılandırma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure ve Azure Stack uygulamalarınız için bir karma bulut kimlik yapılandırmayı öğrenin.

Küresel Azure hem de Azure Stack kullanarak uygulamalarınıza erişim izni verme için iki seçeneğiniz vardır.

 * Azure Stack sürekli bir internet bağlantısı varsa, Azure Active Directory (Azure AD) kullanabilirsiniz.
 * Azure Stack, internet bağlantısı kesildiğinde, Azure Directory Federasyon Hizmetleri'nde (AD FS) kullanabilirsiniz.

Hizmet sorumluları, Azure Stack uygulamalarınızın dağıtım veya Azure Stack'te Azure Resource Manager kullanarak yapılandırması için erişim vermek için kullanın.

Bu öğreticide, bir örnek ortama oluşturacaksınız:

> [!div class="checklist"]
> - Genel Azure ve Azure Stack'te karma kimlik oluşturma
> - Azure Stack API'sine erişmek için bir belirteç alır.

Bu öğreticideki adımlar için Azure Stack operatör izinleri olmalıdır.

> [!Tip]  
> ![karma pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack, Azure'nın bir uzantısıdır. Azure Stack çevikliğini ve yenilik bulut, şirket içi ortamınıza bilgi işlem, hibrit uygulamaları her yerde oluşturup dağıtmayı olanak tanıyan tek hibrit Bulutu sunar.  
> 
> Teknik incelemeyi [karma uygulamaları için tasarım konuları](https://aka.ms/hybrid-cloud-applications-pillars) yazılım kalitesinin yapı taşları tasarlama, dağıtma ve çalıştırma için (yerleştirme, ölçeklenebilirlik, kullanılabilirlik, dayanıklılık, yönetilebilirlik ve güvenlik) gözden geçirmeleri karma uygulamalar. Tasarım konuları, üretim ortamlarında sorunlarını en aza karma uygulama tasarımının en iyi duruma getirme yardımcı olur.


## <a name="create-a-service-principal-for-azure-ad-in-the-portal"></a>Azure AD portalında için hizmet sorumlusu oluşturma

Azure Stack kimlik deposu olarak Azure AD'yi kullanarak dağıttıysanız, Azure için gibi hizmet sorumluları oluşturabilirsiniz. [Kaynaklara erişmek için bir uygulama kimliğini kullan](../operator/azure-stack-create-service-principals.md#manage-an-azure-ad-service-principal) portal üzerinden adımların nasıl gerçekleştirileceğini gösterir. Sahip olduğunuzdan emin olmanız [Azure AD izinleri gerekli](/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) başlamadan önce.

## <a name="create-a-service-principal-for-ad-fs-using-powershell"></a>PowerShell kullanarak AD FS için hizmet sorumlusu oluşturma

AD FS ile Azure Stack dağıttıysanız, hizmet sorumlusu oluşturma, erişim için rol atama ve o kimlik kullanarak PowerShell üzerinden oturum için PowerShell kullanabilirsiniz. [Kaynaklara erişmek için bir uygulama kimliğini kullan](../operator/azure-stack-create-service-principals.md#manage-an-ad-fs-service-principal) PowerShell kullanarak gerekli adımların nasıl gerçekleştirileceğini gösterir.

## <a name="using-the-azure-stack-api"></a>Azure yığını API'si kullanma

[Azure Stack API](azure-stack-rest-api-use.md) öğretici Azure Stack API'ye erişmek için bir belirteç alma sürecinde açıklar.

## <a name="connect-to-azure-stack-using-powershell"></a>Powershell kullanarak Azure Stack'e bağlanma

Bu hızlı başlangıçta [PowerShell'de Azure Stack ile başlamak için](../operator/azure-stack-powershell-install.md) , Azure PowerShell'i yükleme ve Azure Stack yüklemenizi bağlanmak için gereken adımlar anlatılmaktadır.

### <a name="prerequisites"></a>Önkoşullar

Azure Active Directory'ye bağlı bir abonelikle erişebileceğiniz bir Azure Stack yükleme ihtiyacınız vardır. Azure Stack yükleme yoksa, bu yönergeleri ayarlamak için kullanabileceğiniz bir [Azure Stack geliştirme Seti'ni](../asdk/asdk-install.md).

#### <a name="connect-to-azure-stack-using-code"></a>Kod kullanarak Azure Stack'e bağlanma

Azure Stack'e kod kullanarak bağlanmak için graf uç noktaları ve kimlik doğrulaması için Azure Stack yüklemenizi almak ve REST istekleri kullanılarak kimlik doğrulaması yapmak için Azure Resource Manager Uç noktalara API'sini kullanın. Örnek istemci uygulaması bulabilirsiniz [GitHub](https://github.com/shriramnat/HybridARMApplication).

>[!Note]
>İstediğiniz dilde için Azure SDK'sı Azure API profillerini desteklemedikçe SDK'sı, Azure Stack ile çalışmayabilir. Azure API profilleri hakkında daha fazla bilgi için bkz: [API sürümü profillerini yönetme](azure-stack-version-profiles.md) makalesi.

## <a name="next-steps"></a>Sonraki adımlar

 - Azure Stack'te kimlik nasıl işlendiğini hakkında daha fazla bilgi için bkz: [Azure Stack için kimlik mimarisi](../operator/azure-stack-identity-architecture.md).
 - Azure bulut desenleri hakkında daha fazla bilgi için bkz: [bulut tasarımı desenleri](https://docs.microsoft.com/azure/architecture/patterns).
