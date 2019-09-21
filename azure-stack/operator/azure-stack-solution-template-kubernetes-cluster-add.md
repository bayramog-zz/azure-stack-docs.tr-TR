---
title: Azure Stack marketi 'ne Kubernetes ekleyin | Microsoft Docs
description: Azure Stack marketi 'ne Kubernetes ekleme hakkında bilgi edinin.
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
ms.date: 06/18/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 06/18/2019
ms.openlocfilehash: 501040273eca6ce80972ae2a69eca2018f9f8d2d
ms.sourcegitcommit: 3af71025e85fc53ce529de2f6a5c396b806121ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71159610"
---
# <a name="add-kubernetes-to-the-azure-stack-marketplace"></a>Azure Stack marketi 'ne Kubernetes ekleme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

> [!note]  
> Azure Stack Kubernetes önizleme aşamasındadır. Azure Stack bağlantısı kesik bir senaryo şu anda önizleme tarafından desteklenmiyor. Yalnızca geliştirme ve test senaryoları için Market öğesini kullanın.
Kullanıcılarınıza Market öğesi olarak Kubernetes sunabilirsiniz. Kullanıcılarınız, Kubernetes 'i tek ve eşgüdümlü bir işlemde dağıtabilir.

Aşağıdaki makalede, tek başına bir Kubernetes kümesine yönelik kaynakları dağıtmak ve sağlamak için bir Azure Resource Manager şablonu kullanma bölümüne bakın. Başlamadan önce Azure Stack ve genel Azure kiracı ayarlarınızı denetleyin. Azure Stack hakkında gerekli bilgileri toplayın. Kiracınıza ve Azure Stack Market 'e gerekli kaynakları ekleyin. Küme, bir Ubuntu sunucusuna, özel bir betiğe ve Kubernetes kümesi Market öğesini Market 'te olacak şekilde değişir.

## <a name="create-a-plan-an-offer-and-a-subscription"></a>Plan, teklif ve abonelik oluşturma

Kubernetes Market öğesi için bir plan, teklif ve abonelik oluşturun. Ayrıca var olan bir planı ve teklifi de kullanabilirsiniz.

1. [Yönetim portalında](https://adminportal.local.azurestack.external) oturum açın.

1. Temel plan olarak bir plan oluşturun. Yönergeler için bkz. [Azure Stack plan oluşturma](azure-stack-create-plan.md).

1. Bir teklif oluşturun. Yönergeler için bkz. [Azure Stack teklif oluşturma](azure-stack-create-offer.md).

1. **Teklifler**' i seçin ve oluşturduğunuz teklifi bulun.

1. Teklif dikey penceresinde **Genel Bakış ' ı** seçin.

1. **Durumu Değiştir**' i seçin. **Ortak**seçeneğini belirleyin.

1. Abonelik oluşturmak için **+ kaynak** > **teklifleri ve planlar** > **aboneliği** oluştur ' u seçin.

    a. Bir **görünen ad**girin.

    b. Bir **Kullanıcı**girin. Kiracınızla ilişkili Azure AD hesabını kullanın.

    c. **Sağlayıcı açıklaması**

    d. Azure Stack için **Dizin kiracısını** Azure AD kiracısı olarak ayarlayın. 

    e. **Teklif**' i seçin. Oluşturduğunuz teklifin adını seçin. Abonelik KIMLIĞINI bir yere getirin.

## <a name="create-a-service-principal-and-credentials-in-ad-fs"></a>AD FS bir hizmet sorumlusu ve kimlik bilgileri oluşturma

Kimlik yönetimi hizmetiniz için Active Directory Federasyon Hizmetleri (AD FS) kullanıyorsanız, bir Kubernetes kümesi dağıtan kullanıcılar için bir hizmet sorumlusu oluşturmanız gerekir. İstemci parolası kullanarak hizmet sorumlusu oluşturun. Yönergeler için bkz. [istemci gizli dizisi kullanarak hizmet sorumlusu oluşturma](azure-stack-create-service-principals.md#create-a-service-principal-that-uses-client-secret-credentials).

## <a name="add-an-ubuntu-server-image"></a>Ubuntu sunucu görüntüsü ekleme

Aşağıdaki Ubuntu Server görüntüsünü Market 'e ekleyin:

1. Oturum [Yönetim Portalı](https://adminportal.local.azurestack.external).

1. **Tüm hizmetler**' i seçin ve ardından **Yönetim** kategorisi altında **Market yönetimi**' ni seçin.

1. **Azure 'dan + Ekle**' yi seçin.

1. `Ubuntu Server` yazın.

1. Sunucunun en yeni sürümünü seçin. Tam sürümü denetleyin ve en yeni sürüme sahip olduğunuzdan emin olun:
    - **Yayımcı**: Canonical
    - **Teklif**: UbuntuServer
    - **Sürüm**: 16.04.201806120 (veya en son sürüm)
    - **SKU**: 16.04-LTS

1. Indir ' i seçin **.**

## <a name="add-a-custom-script-for-linux"></a>Linux için özel betik ekleme

Market 'ten Kubernetes ekleyin:

1. [Yönetim portalını](https://adminportal.local.azurestack.external)açın.

1. **Tüm hizmetler** ' i seçin ve ardından **Yönetim** kategorisi altında **Market yönetimi**' ni seçin.

1. **Azure 'dan + Ekle**' yi seçin.

1. `Custom Script for Linux` yazın.

1. Aşağıdaki profille betiği seçin:
   - **Teklif**: Linux 2,0 için özel betik
   - **Sürüm**: 2.0.6 (veya en son sürüm)
   - **Yayımcı**: Microsoft Corp

     > [!Note]  
     > Linux için özel komut dosyasının birden fazla sürümü listelenmiş olabilir. Öğenin son sürümünü eklemeniz gerekir.

1. Indir ' i seçin **.**


## <a name="add-kubernetes-to-the-marketplace"></a>Market 'e Kubernetes ekleme

1. [Yönetim portalını](https://adminportal.local.azurestack.external)açın.

1. **Tüm hizmetler** ' i seçin ve ardından **Yönetim** kategorisi altında **Market yönetimi**' ni seçin.

1. **Azure 'dan + Ekle**' yi seçin.

1. `Kubernetes` yazın.

1. `Kubernetes Cluster` öğesini seçin.

1. Indir ' i seçin **.**

    > [!note]  
    > Market öğesinin Market 'te görünmesi beş dakika sürebilir.

    ![Kubernetes](../user/media/azure-stack-solution-template-kubernetes-deploy/marketplaceitem.png)

## <a name="update-or-remove-the-kubernetes"></a>Kubernetes 'i güncelleştirme veya kaldırma 

Kubernetes öğesini güncelleştirirken Market 'teki önceki öğeyi kaldırırsınız. Kubernetes güncelleştirmesini Market 'e eklemek için bu makaledeki yönergeleri izleyin.

Kubernetes öğesini kaldırmak için:

1. PowerShell ile Azure Stack bir operatör olarak bağlanın. Yönergeler için bkz. [PowerShell ile Azure Stack 'ye bir operatör olarak bağlanma](azure-stack-powershell-configure-admin.md).

2. Galerideki geçerli Kubernetes küme öğesini bulun.

    ```powershell  
    Get-AzsGalleryItem | Select Name
    ```
    
3. Geçerli öğenin adı, örneğin`Microsoft.AzureStackKubernetesCluster.0.3.0`

4. Öğeyi kaldırmak için aşağıdaki PowerShell cmdlet 'ini kullanın:

    ```powershell  
    $Itemname="Microsoft.AzureStackKubernetesCluster.0.3.0"

    Remove-AzsGalleryItem -Name $Itemname
    ```

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack için Kubernetes dağıtma](../user/azure-stack-solution-template-kubernetes-deploy.md)

[Azure Stack hizmetleri sunma hakkında genel bakış](azure-stack-offer-services-overview.md)
