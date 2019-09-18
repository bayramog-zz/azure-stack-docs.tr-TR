---
title: Azure Stack operatör olarak PowerShell ile bağlanma | Microsoft Docs
description: Azure Stack operatör olarak PowerShell ile bağlanma hakkında bilgi edinin
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 09/17/2019
ms.author: justinha
ms.reviewer: thoroet
ms.lastreviewed: 01/24/2019
ms.openlocfilehash: f18bf01938eb53a69f26f8bbcfa0320d359f0552
ms.sourcegitcommit: 95f30e32e5441599790d39542ff02ba90e70f9d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71070234"
---
# <a name="connect-to-azure-stack-with-powershell-as-an-operator"></a>PowerShell ile Azure Stack operatör bağlanma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack tekliflerini, planları, kotalar ve Uyarılar oluşturmak gibi kaynakları yönetmek için PowerShell kullanmak için yapılandırabilirsiniz. Bu konuda işleci ortam yapılandırmanıza yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki önkoşulları, [geliştirme seti](../asdk/asdk-connect.md#connect-with-rdp) 'Nden veya Windows tabanlı bir dış istemciden, [ASDK ile VPN üzerinden bağlıysanız](../asdk/asdk-connect.md#connect-with-vpn)çalıştırın. 

 - Yükleme [Azure Stack ile uyumlu Azure PowerShell modüllerini](azure-stack-powershell-install.md).  
 - İndirme [Azure Stack ile çalışması için gereken araçları](azure-stack-powershell-download.md).  

## <a name="connect-with-azure-ad"></a>Azure AD'ye bağlanma

PowerShell ile Azure Stack operatörü ortam yapılandırın. Aşağıdaki betiklerin birini çalıştırın: Azure Active Directory (Azure AD) tenantName ve Azure Resource Manager Endpoint değerlerini kendi ortam yapılandırmanızla değiştirin. 

[!include[Remove Account](../../includes/remove-account.md)]

```powershell  
    # Register an Azure Resource Manager environment that targets your Azure Stack instance. Get your Azure Resource Manager endpoint value from your service provider.
    Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint "https://adminmanagement.local.azurestack.external" `
      -AzureKeyVaultDnsSuffix adminvault.local.azurestack.external `
      -AzureKeyVaultServiceEndpointResourceId https://adminvault.local.azurestack.external

    # Set your tenant name
    $AuthEndpoint = (Get-AzureRmEnvironment -Name "AzureStackAdmin").ActiveDirectoryAuthority.TrimEnd('/')
    $AADTenantName = "<myDirectoryTenantName>.onmicrosoft.com"
    $TenantId = (invoke-restmethod "$($AuthEndpoint)/$($AADTenantName)/.well-known/openid-configuration").issuer.TrimEnd('/').Split('/')[-1]

    # After signing in to your environment, Azure Stack cmdlets
    # can be easily targeted at your Azure Stack instance.
    Add-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantId
```

## <a name="connect-with-ad-fs"></a>AD FS ile bağlanma

PowerShell ile Azure Active Directory Federasyon Hizmetleri (Azure AD FS) ile Azure Stack operatörü ortama bağlanın. Azure Stack Geliştirme Seti için bu Azure Resource Manager uç nokta kümesine `https://adminmanagement.local.azurestack.external`. Azure Stack tümleşik sistemleri için Azure Resource Manager uç noktası almak için hizmet sağlayıcınıza başvurun.


  ```powershell  
  # Register an Azure Resource Manager environment that targets your Azure Stack instance. Get your Azure Resource Manager endpoint value from your service provider.
    Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint "https://adminmanagement.local.azurestack.external" `
      -AzureKeyVaultDnsSuffix adminvault.local.azurestack.external `
      -AzureKeyVaultServiceEndpointResourceId https://adminvault.local.azurestack.external

  # Sign in to your environment
  Login-AzureRmAccount -EnvironmentName "AzureStackAdmin"
  ```

> [!Note]  
> AD FS yalnızca Kullanıcı kimlikleriyle etkileşimli kimlik doğrulamasını destekler. Bir kimlik bilgisi nesnesi gerekliyse, bir hizmet sorumlusu (SPN) kullanmanız gerekir. Azure Stack ve AD FS kimlik yönetimi hizmetiniz olarak bir hizmet sorumlusu ayarlama hakkında daha fazla bilgi için bkz. [AD FS hizmet sorumlusunu yönetme](azure-stack-create-service-principals.md#manage-an-ad-fs-service-principal).

## <a name="test-the-connectivity"></a>Bağlantısını test etme

Artık her şeyi kendinizi, Kurulum, Azure Stack içindeki kaynakları oluşturmak için PowerShell kullanın. Örneğin, bir uygulama için bir kaynak grubu oluşturun ve bir sanal makine ekleyin. Adlı bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın **MyResourceGroup**.

```powershell  
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Sonraki adımlar

- [Şablonları Azure Stack için geliştirme](../user/azure-stack-develop-templates.md)
- [Şablonları PowerShell ile dağıtma](../user/azure-stack-deploy-template-powershell.md)
  - [Azure Stack modülü başvurusu](https://docs.microsoft.com/powershell/azure/azure-stack/overview)  
