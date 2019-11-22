---
title: Azure Kubernetes Hizmetleri (AKS) altyapısı önkoşullarını Azure Stack Market 'e ekleyin | Microsoft Docs
description: Azure Stack Market 'e AKS altyapısı önkoşulları eklemeyi öğrenin.
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
ms.date: 11/21/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 11/21/2019
ms.openlocfilehash: 823ea20bbddaceda970d95008e1214e77ca51f7c
ms.sourcegitcommit: 0b783e262ac87ae67929dbd4c366b19bf36740f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74310110"
---
# <a name="add-the-azure-kubernetes-services-aks-engine-prerequisites-to-the-azure-stack-marketplace"></a>Azure Kubernetes Hizmetleri (AKS) altyapısı önkoşullarını Azure Stack Market 'e ekleme

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Kullanıcılarınıza, bu makalede açıklanan öğeleri Azure Stack ekleyerek Azure Kubernetes Hizmetleri (AKS) altyapısını ayarlama olanağı sağlayabilirsiniz. Kullanıcılarınız daha sonra tek ve eşgüdümlü bir işlemde bir Kubernetes kümesi dağıtabilir. Bu makalede, AKS altyapısını hem bağlı hem de bağlantısı kesik ortamlarda kullanıcılarınız için kullanılabilir hale getirmek için gereken adımlarda adım adım gösterilmektedir. AKS altyapısı, bir hizmet asıl kimliğine ve Market 'te özel bir betik uzantısına ve AKS temel görüntüsüne bağlıdır.

> [!IMPORTANT]
> AKS altyapısı şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="check-your-users-service-offering"></a>Kullanıcılarınızın hizmet sunumunu denetleyin

Kullanıcılarınızın yeterli alana sahip Azure Stack bir plana, teklifine ve aboneliğe ihtiyacı olacak. Kullanıcılar genellikle en fazla altı sanal makineye sahip olmak üzere üç ana ve üç çalışan düğümünden oluşan kümeler dağıtmak ister. Yeterli sayıda kotasına sahip olduklarından emin olmak isteyeceksiniz.

Bir hizmet teklifi planlama ve ayarlama hakkında daha fazla bilgiye ihtiyacınız varsa, bkz. [Azure Stack hizmetleri sunma genel bakış](service-plan-offer-subscription-overview.md)

## <a name="create-a-service-principal-and-credentials"></a>Hizmet sorumlusu ve kimlik bilgileri oluşturma

Kubernetes kümesinin Azure Stack hizmet sorumlusu (SPN) ve rol tabanlı izinleri olması gerekir.

### <a name="create-an-spn-in-azure-ad"></a>Azure AD 'de SPN oluşturma

Kimlik yönetimi hizmetiniz için Azure Active Directory (Azure AD) kullanıyorsanız, bir Kubernetes kümesi dağıtan kullanıcılar için bir hizmet sorumlusu oluşturmanız gerekir. İstemci parolası kullanarak bir hizmet sorumlusu oluşturun. Yönergeler için, bkz. [istemci gizli bilgisi kullanan hizmet sorumlusu oluşturma](azure-stack-create-service-principals.md#create-a-service-principal-that-uses-a-client-secret-credential).

### <a name="create-an-spn-in-ad-fs"></a>AD FS içinde SPN oluşturma

Kimlik yönetimi hizmetiniz için Active Directory Federasyon Hizmetleri (AD FS) kullanıyorsanız, bir Kubernetes kümesi dağıtan kullanıcılar için bir hizmet sorumlusu oluşturmanız gerekir. İstemci parolası kullanarak bir hizmet sorumlusu oluşturun. Yönergeler için bkz. [istemci gizli dizisi kullanarak hizmet sorumlusu oluşturma](azure-stack-create-service-principals.md#create-a-service-principal-that-uses-client-secret-credentials).

## <a name="add-the-aks-base-image"></a>AKS temel görüntüsünü ekleyin

Azure 'dan öğeyi alarak AKS temel görüntüsünü Market 'e ekleyebilirsiniz. Ancak Azure Stack bağlantısı kesilirse, bu yönergeleri kullanarak öğeyi eklemek için [Azure 'daki Market öğelerini indirin](https://docs.microsoft.com/azure-stack/operator/azure-stack-download-azure-marketplace-item?view=azs-1908#disconnected-or-a-partially-connected-scenario) . 5\. adımda belirtilen öğeyi ekleyin.

Market 'e aşağıdaki öğeyi ekleyin:

1. Oturum [Yönetim Portalı](https://adminportal.local.azurestack.external).

1. **Tüm hizmetler**' i seçin ve ardından **Yönetim** kategorisi altında **Market yönetimi**' ni seçin.

1. **Azure 'dan + Ekle**' yi seçin.

1. `AKS Base` yazın.

1. AKS altyapısının sürümüyle eşleşen görüntü sürümünü seçin. AKS temel görüntüsünün listesini, [desteklenen Kubernetes sürümlerindeki](https://github.com/Azure/aks-engine/blob/master/docs/topics/azure-stack.md#supported-kubernetes-versions)aks altyapısı sürümüne bulabilirsiniz. 

    Listede şunları seçin:
    - **Yayımcı**: Azure Kubernetes hizmeti
    - **Teklif**: aks
    - **Sürüm**: aks Base Image 16,04-LTS Image dekze, 2019 Ekim (2019.10.24 veya aks altyapısıyla eşlenen sürüm)

1. Indir ' i seçin **.**

## <a name="add-a-custom-script-extension"></a>Özel Betik uzantısı ekleme

Öğeyi Azure 'dan alarak özel betiği Market 'e ekleyebilirsiniz. Ancak Azure Stack bağlantısı kesilirse, öğeyi eklemek için [Azure 'daki Market öğelerini indirme](https://docs.microsoft.com/azure-stack/operator/azure-stack-download-azure-marketplace-item?view=azs-1908#disconnected-or-a-partially-connected-scenario) yönergelerini kullanın.  5\. adımda belirtilen öğeyi ekleyin.

1. [Yönetim portalını](https://adminportal.local.azurestack.external)açın.

1. **Tüm hizmetler** ' i seçin ve ardından **Yönetim** kategorisi altında **Market yönetimi**' ni seçin.

1. **Azure 'dan + Ekle**' yi seçin.

1. `Custom Script for Linux` yazın.

1. Aşağıdaki profille betiği seçin:
   - **Teklif**: Linux 2,0 Için özel betik
   - **Sürüm**: 2.0.6 (veya en son sürüm)
   - **Yayımcı**: Microsoft Corp

     > [!Note]  
     > Linux için özel betiğin birden fazla sürümü listelenmiş olabilir. Öğenin son sürümünü eklemeniz gerekir.

1. Indir ' i seçin **.**

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack AKS Altyapısı nedir?](../user/azure-stack-kubernetes-aks-engine-overview.md)

[Azure Stack hizmetleri sunma hakkında genel bakış](service-plan-offer-subscription-overview.md)
