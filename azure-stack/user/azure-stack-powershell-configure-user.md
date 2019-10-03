---
title: Bir kullanıcı olarak PowerShell ile Azure stack'e bağlanma | Microsoft Docs
description: PowerShell ile Azure Stack nasıl bağlanacağınızı öğrenin.
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
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 10/02/2019
ms.openlocfilehash: 6a75eb788afd84b6619326293ae2399d8ed5b0e1
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71824218"
---
# <a name="connect-to-azure-stack-with-powershell-as-a-user"></a>PowerShell ile Azure Stack için kullanıcı olarak bağlanma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack kaynaklarını yönetmek için PowerShell ile Azure Stack bağlanabilirsiniz. Örneğin, tekliflere abone olmak, sanal makineler (VM 'Ler) oluşturmak ve Azure Resource Manager şablonları dağıtmak için PowerShell 'i kullanabilirsiniz.

Kurulumu almak için:
  - Gereksinimlerine sahip olduğunuzdan emin olun.
  - Azure'la Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS). 
  - Kaynak sağlayıcılarını kaydedin.
  - Bağlantınızı test edin.

## <a name="prerequisites-to-connecting-with-powershell"></a>PowerShell ile bağlantı kurma önkoşulları

Bu önkoşulları [geliştirme seti](../asdk/asdk-connect.md#connect-to-azure-stack-using-rdp)'NDEN veya [VPN üzerinden bağlıysanız](../asdk/asdk-connect.md#connect-to-azure-stack-using-vpn)Windows tabanlı bir dış istemciden yapılandırın:

* Yükleme [Azure Stack ile uyumlu Azure PowerShell modüllerini](../operator/azure-stack-powershell-install.md).
* İndirme [Azure Stack ile çalışması için gereken araçları](../operator/azure-stack-powershell-download.md).

Aşağıdaki komut dosyası değişkenleri, Azure Stack yapılandırmasından değerlerle değiştirin emin olun:

- **Azure AD Kiracı adı**  
  Azure Stack yönetmek için kullanılan Azure AD kiracınızın adı. Örneğin, yourdirectory.onmicrosoft.com.
- **Azure Resource Manager uç noktası**  
  Azure Stack Geliştirme Seti için bu değeri ayarlamak https://management.local.azurestack.external. Azure Stack tümleşik sistemleri için bu değeri almak için hizmet sağlayıcınıza başvurun.

## <a name="connect-to-azure-stack-with-azure-ad"></a>Azure AD ile Azure Stack bağlanma

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

## <a name="connect-to-azure-stack-with-ad-fs"></a>AD FS Azure Stack bağlanma

  ```powershell  
  # Register an Azure Resource Manager environment that targets your Azure Stack instance
  Add-AzureRMEnvironment -Name "AzureStackUser" -ArmEndpoint "https://management.local.azurestack.external"

  # Sign in to your environment
  Login-AzureRmAccount -EnvironmentName "AzureStackUser"
  ```

## <a name="register-resource-providers"></a>Kaynak sağlayıcılarını kaydetme

Kaynak sağlayıcıları, Portal aracılığıyla dağıtılan bir kaynağı olmayan yeni Kullanıcı abonelikleri için otomatik olarak kayıtlı değildir. Aşağıdaki betiği çalıştırarak, bir kaynak sağlayıcısı açıkça kaydedebilirsiniz:

```powershell  
foreach($s in (Get-AzureRmSubscription)) {
        Select-AzureRmSubscription -SubscriptionId $s.SubscriptionId | Out-Null
        Write-Progress $($s.SubscriptionId + " : " + $s.SubscriptionName)
Get-AzureRmResourceProvider -ListAvailable | Register-AzureRmResourceProvider
    }
```

## <a name="test-the-connectivity"></a>Bağlantısını test etme

Her şeyin kurulumunu yaptıktan sonra, Azure Stack ' de kaynak oluşturmak için PowerShell kullanarak bağlantıyı test edin. Test olarak, bir uygulama için bir kaynak grubu oluşturun ve bir VM ekleyin. "MyResourceGroup" adlı bir kaynak grubu oluşturmak için aşağıdaki komutu çalıştırın:

```powershell  
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Sonraki adımlar

- [Şablonları Azure Stack için geliştirme](azure-stack-develop-templates.md)
- [Şablonları PowerShell ile dağıtma](azure-stack-deploy-template-powershell.md)
- [PowerShell modülü başvurusunu Azure Stack](https://docs.microsoft.com/powershell/azure/azure-stack/overview)
- PowerShell'i bulut işleci ortamı ayarlamak istiyorsanız, başvurmak [Azure Stack işlecin PowerShell ortamını yapılandırma](../operator/azure-stack-powershell-configure-admin.md) makalesi.
