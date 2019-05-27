---
title: Bir kullanıcı olarak PowerShell ile Azure stack'e bağlanma | Microsoft Docs
description: Azure Stack PowerShell ile bağlanma hakkında bilgi edinin.
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
ms.date: 04/26/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 01/24/2019
ms.openlocfilehash: c9ef9c1e936c71a8b0a2a0eb636da1eac5bf69da
ms.sourcegitcommit: be5382f715a9c1c18c660b630d8fcd823f13aae3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66197318"
---
# <a name="connect-to-azure-stack-with-powershell-as-a-user"></a>PowerShell ile Azure Stack için kullanıcı olarak bağlanma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack kaynaklarınızı yönetmek için PowerShell ile Azure Stack bağlanabilirsiniz. Örneğin, tekliflere abone, sanal makineler (VM) oluşturun ve Azure Resource Manager şablonlarını dağıtmak için PowerShell kullanabilirsiniz.

Kurulum almak için:
  - Gereksinimlerine sahip olduğunuzdan emin olun.
  - Azure'la Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS). 
  - Kaynak sağlayıcılarını kaydedin.
  - Bağlantınızı test edin.

## <a name="prerequisites-to-connecting-with-powershell"></a>PowerShell ile bağlama önkoşulları

Bu önkoşulları yapılandırma [Geliştirme Seti](../asdk/asdk-connect.md#connect-to-azure-stack-using-rdp), veya size bir istemciden Windows tabanlı dış [VPN üzerinden bağlı](../asdk/asdk-connect.md#connect-to-azure-stack-using-vpn):

* Yükleme [Azure Stack ile uyumlu Azure PowerShell modüllerini](../operator/azure-stack-powershell-install.md).
* İndirme [Azure Stack ile çalışması için gereken araçları](../operator/azure-stack-powershell-download.md).

Aşağıdaki komut dosyası değişkenleri, Azure Stack yapılandırmasından değerlerle değiştirin emin olun:

- **Azure AD Kiracı adı**  
  Azure Stack yönetmek için kullanılan Azure AD kiracınızın adıdır. Örneğin, yourdirectory.onmicrosoft.com.
- **Azure Resource Manager uç noktası**  
  Azure Stack Geliştirme Seti için bu değeri ayarlamak https://management.local.azurestack.external. Azure Stack tümleşik sistemleri için bu değeri almak için hizmet sağlayıcınıza başvurun.

## <a name="connect-to-azure-stack-with-azure-ad"></a>Azure AD ile Azure stack'e bağlanma

```powershell  
    Add-AzureRMEnvironment -Name "AzureStackUser" -ArmEndpoint "https://management.local.azurestack.external"
    # Set your tenant name
    $AuthEndpoint = (Get-AzureRmEnvironment -Name "AzureStackUser").ActiveDirectoryAuthority.TrimEnd('/')
    $AADTenantName = "<myDirectoryTenantName>.onmicrosoft.com"
    $TenantId = (invoke-restmethod "$($AuthEndpoint)/$($AADTenantName)/.well-known/openid-configuration").issuer.TrimEnd('/').Split('/')[-1]

    # After signing in to your environment, Azure Stack cmdlets
    # can be easily targeted at your Azure Stack instance.
    Add-AzureRmAccount -EnvironmentName "AzureStackUser" -TenantId $TenantId
```

## <a name="connect-to-azure-stack-with-ad-fs"></a>AD FS ile Azure stack'e bağlanma

  ```powershell  
  # Register an Azure Resource Manager environment that targets your Azure Stack instance
  Add-AzureRMEnvironment -Name "AzureStackUser" -ArmEndpoint "https://management.local.azurestack.external"

  # Sign in to your environment
  Login-AzureRmAccount -EnvironmentName "AzureStackUser"
  ```

## <a name="register-resource-providers"></a>Kaynak sağlayıcılarını kaydetme

Kaynak sağlayıcıları için portal üzerinden dağıtılan herhangi bir kaynağa sahip olmayan yeni kullanıcı aboneliklerini otomatik olarak kayıtlı değil. Aşağıdaki betiği çalıştırarak, bir kaynak sağlayıcısı açıkça kaydedebilirsiniz:

```powershell  
foreach($s in (Get-AzureRmSubscription)) {
        Select-AzureRmSubscription -SubscriptionId $s.SubscriptionId | Out-Null
        Write-Progress $($s.SubscriptionId + " : " + $s.SubscriptionName)
Get-AzureRmResourceProvider -ListAvailable | Register-AzureRmResourceProvider
    }
```

## <a name="test-the-connectivity"></a>Bağlantısını test etme

Süreyi bulduğunuzda, her şeyi Kurulum, Azure Stack'te kaynakları oluşturmak için PowerShell kullanarak test bağlantısı. Bir test olarak bir uygulama için bir kaynak grubu oluşturun ve bir sanal makine ekleyin. "MyResourceGroup" adlı bir kaynak grubu oluşturmak için aşağıdaki komutu çalıştırın:

```powershell  
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Sonraki adımlar

- [Şablonları Azure Stack için geliştirme](azure-stack-develop-templates.md)
- [Şablonları PowerShell ile dağıtma](azure-stack-deploy-template-powershell.md)
- [Azure Stack PowerShell modülü başvurusu](https://docs.microsoft.com/powershell/azure/azure-stack/overview)
- PowerShell'i bulut işleci ortamı ayarlamak istiyorsanız, başvurmak [Azure Stack işlecin PowerShell ortamını yapılandırma](../operator/azure-stack-powershell-configure-admin.md) makalesi.
