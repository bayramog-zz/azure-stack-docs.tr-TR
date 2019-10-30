---
title: Kubernetes 'i Azure Stack kapsayıcıları kullanacak şekilde dağıtma | Microsoft Docs
description: Kubernetes 'i Azure Stack kapsayıcıları kullanacak şekilde dağıtmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 06/18/2019
ms.openlocfilehash: 0fc45ae0413fb7e269d8d7347e2279895c234950
ms.sourcegitcommit: 0d27456332031ab98ba2277117395ae5ffcbb79f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73047157"
---
# <a name="deploy-kubernetes-to-use-containers-with-azure-stack"></a>Azure Stack ile kapsayıcıları kullanmak için Kubernetes dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

> [!Note]  
> Kümeleri kavram kanıtı olarak dağıtmak için Kubernetes Azure Stack Market öğesini kullanın. Azure Stack üzerinde desteklenen Kubernetes kümeleri için [AKS altyapısını](azure-stack-kubernetes-aks-engine-overview.md)kullanın.

Kubernetes kaynaklarını tek ve eşgüdümlü bir işlemde dağıtmak ve ayarlamak için bu makaledeki adımları izleyebilirsiniz. Adımlarda Azure Resource Manager çözüm şablonu kullanılır. Azure Stack yüklemeniz hakkında gerekli bilgileri toplamanız, şablonu oluşturmanız ve ardından buluta dağıtmanız gerekir. Azure Stack şablonu, genel Azure 'da sunulan aynı yönetilen AKS hizmetini kullanmaz.

## <a name="kubernetes-and-containers"></a>Kubernetes ve kapsayıcılar

Kubernetes 'i Azure Stack üzerinde AKS altyapısı tarafından oluşturulan Azure Resource Manager şablonları kullanarak yükleyebilirsiniz. [Kubernetes](https://kubernetes.io) , kapsayıcılardaki uygulamaların dağıtılması, ölçeklendirilmesi ve yönetilmesi için açık kaynaklı bir sistemdir. Bir [kapsayıcı](https://www.docker.com/what-container) görüntüde. Kapsayıcı görüntüsü bir sanal makineye (VM) benzer, ancak bir VM 'nin aksine, kapsayıcı kod, kod yürütme çalışma zamanı kodu, belirli kitaplıklar ve ayarlar gibi bir uygulamayı çalıştırmak için ihtiyaç duyacağı kaynakları da içerir.

Kubernetes 'i kullanarak şunları yapabilirsiniz:

- Saniyeler içinde dağıtılabilecek yüksek düzeyde ölçeklenebilir, yükseltilebilir uygulamalar geliştirin. 
- Uygulamanızın tasarımını kolaylaştırın ve farklı Held uygulamalarına göre güvenilirliğini geliştirebilirsiniz. [Helk](https://github.com/kubernetes/helm) , Kubernetes uygulamalarının yaşam döngüsünü yüklemenize ve yönetmenize yardımcı olan bir açık kaynaklı paketleme aracıdır.
- Uygulamalarınızın sistem durumunu kolayca izleyip tanılayın.

Yalnızca kümenizi destekleyen düğümlerin gerektirdiği işlem kullanımı için ücretlendirilirsiniz. Daha fazla bilgi için, bkz. [Azure Stack kullanımı ve faturalama](../operator/azure-stack-billing-and-chargeback.md).

## <a name="deploy-kubernetes-to-use-containers"></a>Kapsayıcıları kullanmak için Kubernetes dağıtma

Azure Stack bir Kubernetes kümesi dağıtma adımları, kimlik yönetimi hizmetinize bağlıdır. Azure Stack yüklemeniz tarafından kullanılan kimlik yönetimi çözümünü doğrulayın. Kimlik yönetimi hizmetinizi doğrulamak için Azure Stack yöneticinize başvurun.

- **Azure Active Directory (Azure AD)**  
Azure AD kullanırken kümeyi yükleme yönergeleri için bkz. [Azure Active Directory (Azure AD) kullanarak Azure Stack Kubernetes dağıtma](azure-stack-solution-template-kubernetes-azuread.md).

- **Active Directory Federasyon Hizmetleri (AD FS)**  
AD FS kullanırken kümeyi yükleme yönergeleri için bkz. [Active Directory federe Hizmetleri kullanarak Azure Stack Için Kubernetes dağıtma (AD FS)](azure-stack-solution-template-kubernetes-adfs.md).

## <a name="connect-to-your-cluster"></a>Kümenize bağlanma

Şimdi kümenize bağlanmaya hazırsınız. Ana şablon, küme kaynak grubunuzda bulunabilir ve `k8s-master-<sequence-of-numbers>` olarak adlandırılır. Ana sunucuya bağlanmak için SSH istemcisi kullanın. Ana bilgisayarda, kümenizi yönetmek için Kubernetes komut satırı istemcisini **kubectl**' yi kullanabilirsiniz. Yönergeler için bkz. [Kubernetes.io](https://kubernetes.io/docs/reference/kubectl/overview).

Ayrıca, bir uygulama yüklemek ve kümenize dağıtmak için **hele** Package Manager 'ı yararlı bulabilirsiniz. Kümeniz ile Held 'yi yükleme ve kullanma hakkında yönergeler için bkz. [Helm.sh](https://helm.sh/).

## <a name="next-steps"></a>Sonraki adımlar

[Kubernetes panosunu etkinleştirme](azure-stack-solution-template-kubernetes-dashboard.md)

[Market 'e bir Kubernetes ekleme (Azure Stack işleci için)](../operator/azure-stack-solution-template-kubernetes-cluster-add.md)

[Azure Stack Azure Active Directory kullanarak Kubernetes dağıtma (Azure AD)](azure-stack-solution-template-kubernetes-azuread.md)

[Azure Stack Active Directory Federasyon Hizmetleri 'ni kullanarak Kubernetes dağıtma (AD FS)](azure-stack-solution-template-kubernetes-adfs.md)

[Azure 'da Kubernetes](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
