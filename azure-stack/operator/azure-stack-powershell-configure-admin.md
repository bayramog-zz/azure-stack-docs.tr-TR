---
title: PowerShell ile Azure Stack bağlanma | Microsoft Docs
description: PowerShell ile Azure Stack nasıl bağlanacağınızı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 09/19/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 09/19/2019
ms.openlocfilehash: 988d2e75dfecd499293576bff3d9722e7ff2c96f
ms.sourcegitcommit: cefba8d6a93efaedff303d3c605b02bd28996c5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74298862"
---
# <a name="connect-to-azure-stack-with-powershell"></a>PowerShell ile Azure Stack bağlanma

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Teklif, plan, kota ve uyarı oluşturma gibi kaynakları yönetmek için PowerShell 'i kullanmak üzere Azure Stack yapılandırabilirsiniz. Bu konuda işleci ortam yapılandırmanıza yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

[VPN aracılığıyla asdk 'ye bağlıysanız](../asdk/asdk-connect.md#connect-with-vpn), [Azure Stack geliştirme seti (asdk)](../asdk/asdk-connect.md#connect-with-rdp) veya Windows tabanlı bir dış istemciden aşağıdaki önkoşulları çalıştırın.

- Yükleme [Azure Stack ile uyumlu Azure PowerShell modüllerini](azure-stack-powershell-install.md).  
- İndirme [Azure Stack ile çalışması için gereken araçları](azure-stack-powershell-download.md).  

## <a name="connect-with-azure-ad"></a>Azure AD'ye bağlanma

PowerShell ile Azure Stack operatör ortamını yapılandırmak için aşağıdaki komut dosyalarından birini çalıştırın. Azure Active Directory (Azure AD) tenantName ve Azure Resource Manager Endpoint değerlerini kendi ortam yapılandırmanızla değiştirin.

[!include[Remove Account](../../includes/remove-account.md)]

```powershell  
    # Register an Azure Resource Manager environment that targets your Azure Stack instance. Get your Azure Resource Manager endpoint value from your service provider.
    Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint "https://adminmanagement.local.azurestack.external" `
      -AzureKeyVaultDnsSuffix adminvault.local.azurestack.external `
      -AzureKeyVaultServiceEndpointResourceId https://adminvault.local.azurestack.external

    # Set your tenant name.
    $AuthEndpoint = (Get-AzureRmEnvironment -Name "AzureStackAdmin").ActiveDirectoryAuthority.TrimEnd('/')
    $AADTenantName = "<myDirectoryTenantName>.onmicrosoft.com"
    $TenantId = (invoke-restmethod "$($AuthEndpoint)/$($AADTenantName)/.well-known/openid-configuration").issuer.TrimEnd('/').Split('/')[-1]

    # After signing in to your environment, Azure Stack cmdlets
    # can be easily targeted at your Azure Stack instance.
    Add-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantId
```

## <a name="connect-with-ad-fs"></a>AD FS ile bağlanma

PowerShell ile Azure Active Directory Federasyon Hizmetleri (Azure AD FS) ile Azure Stack operatörü ortama bağlanın. Bu Azure Resource Manager uç noktası, ASDK için `https://adminmanagement.local.azurestack.external`olarak ayarlanır. Azure Stack tümleşik sistemleri için Azure Resource Manager uç noktası almak için hizmet sağlayıcınıza başvurun.

  ```powershell  
  # Register an Azure Resource Manager environment that targets your Azure Stack instance. Get your Azure Resource Manager endpoint value from your service provider.
    Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint "https://adminmanagement.local.azurestack.external" `
      -AzureKeyVaultDnsSuffix adminvault.local.azurestack.external `
      -AzureKeyVaultServiceEndpointResourceId https://adminvault.local.azurestack.external

  # Sign in to your environment.
  Login-AzureRmAccount -EnvironmentName "AzureStackAdmin"
  ```

> [!Note]  
> AD FS yalnızca Kullanıcı kimlikleriyle etkileşimli kimlik doğrulamasını destekler. Bir kimlik bilgisi nesnesi gerekliyse, bir hizmet sorumlusu (SPN) kullanmanız gerekir. Azure Stack ve AD FS kimlik yönetimi hizmetiniz olarak bir hizmet sorumlusu ayarlama hakkında daha fazla bilgi için bkz. [AD FS hizmet sorumlusunu yönetme](azure-stack-create-service-principals.md#manage-an-ad-fs-service-principal).

## <a name="test-the-connectivity"></a>Bağlantısını test etme

Artık her şeyi kendinizi, Kurulum, Azure Stack içindeki kaynakları oluşturmak için PowerShell kullanın. Örneğin, bir uygulama için bir kaynak grubu oluşturabilir ve sanal makine ekleyebilirsiniz. Adlı bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın **MyResourceGroup**.

```powershell  
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack için şablonlar geliştirin](../user/azure-stack-develop-templates.md).
- [Şablonları PowerShell Ile dağıtın](../user/azure-stack-deploy-template-powershell.md).
  - [Modül başvurusunu Azure Stack](https://docs.microsoft.com/powershell/azure/azure-stack/overview).
