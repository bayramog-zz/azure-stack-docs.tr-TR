---
title: Bir kullanıcı olarak PowerShell ile Azure stack'e bağlanma | Microsoft Docs
description: PowerShell kullanarak Azure Stack'e bağlanma adımları.
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
ms.openlocfilehash: 855d7c03f4a18c4409d36b8ac5fd702c8549e413
ms.sourcegitcommit: 85c3acd316fd61b4e94c991a9cd68aa97702073b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2019
ms.locfileid: "64986241"
---
# <a name="connect-to-azure-stack-with-powershell-as-a-user"></a>PowerShell ile Azure Stack için kullanıcı olarak bağlanma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

PowerShell ile Azure Stack bağlanabilirsiniz. PowerShell ile Azure Stack kaynaklarını yönetmenizi bağlanmanız gerekir. Örneğin, tekliflere abone, sanal makineler oluşturmak ve Azure Resource Manager şablonlarını dağıtmak için PowerShell kullanabilirsiniz.

Ayarlamak için:
  - Gereksinimlerine sahip olduğunuzdan emin olun.
  - Azure'la Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS). 
  - Kaynak sağlayıcılarını kaydedin.
  - Bağlantınızı test edin.

## <a name="prerequisites-to-connect-using-powershell"></a>PowerShell kullanarak bağlanmak için Önkoşullar

Bu önkoşulları yapılandırma [Geliştirme Seti](../asdk/asdk-connect.md#connect-to-azure-stack-using-rdp), ya da Eğer bir Windows tabanlı dış istemciden [VPN üzerinden bağlı](../asdk/asdk-connect.md#connect-to-azure-stack-using-vpn):

* Yükleme [Azure Stack ile uyumlu Azure PowerShell modüllerini](../operator/azure-stack-powershell-install.md).
* İndirme [Azure Stack ile çalışması için gereken araçları](../operator/azure-stack-powershell-download.md).

Aşağıdaki komut dosyası değişkenleri, Azure Stack yapılandırmasından değerlerle değiştirin emin olun:

- **Azure AD Kiracı adı**  
  Azure Stack, örneğin, yourdirectory.onmicrosoft.com yönetmek için kullanılan Azure AD kiracınızın adıdır.
- **Azure Resource Manager uç noktası**  
  Azure Stack Geliştirme Seti için bu değeri ayarlamak https://management.local.azurestack.external. Azure Stack tümleşik sistemleri için bu değeri almak için hizmet sağlayıcınıza başvurun.

## <a name="connect-with-azure-ad"></a>Azure AD'ye bağlanma

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

## <a name="connect-with-ad-fs"></a>AD FS ile bağlanma

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

Her şeyi kendinizi, Azure Stack'te kaynakları oluşturmak için PowerShell kullanarak bağlantıyı test edin. Bir test olarak bir uygulama için bir kaynak grubu oluşturun ve bir sanal makine ekleyin. "MyResourceGroup" adlı bir kaynak grubu oluşturmak için aşağıdaki komutu çalıştırın:

```powershell  
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Sonraki adımlar

- [Şablonları Azure Stack için geliştirme](azure-stack-develop-templates.md)
- [Şablonları PowerShell ile dağıtma](azure-stack-deploy-template-powershell.md)
- [Azure Stack PowerShell modülü başvurusu](https://docs.microsoft.com/powershell/azure/azure-stack/overview)
- PowerShell'i bulut işleci ortamı ayarlamak istiyorsanız, başvurmak [Azure Stack işlecin PowerShell ortamını yapılandırma](../operator/azure-stack-powershell-configure-admin.md) makalesi.
