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
ms.date: 09/18/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 09/18/2019
ms.openlocfilehash: da07fc0fe67c00f017a547a861d8ea4eb856864b
ms.sourcegitcommit: acebda8a42ac8ecdeba490fc1738e9041479dab0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72814036"
---
# <a name="connect-to-azure-stack-with-powershell"></a>PowerShell ile Azure Stack bağlanma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Teklif, plan, kota ve uyarı oluşturma gibi kaynakları yönetmek için PowerShell 'i kullanmak üzere Azure Stack yapılandırabilirsiniz. Bu konu, işleç ortamını yapılandırmanıza yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

[VPN aracılığıyla asdk 'ye bağlıysanız](../asdk/asdk-connect.md#connect-with-vpn), [Azure Stack geliştirme seti (asdk)](../asdk/asdk-connect.md#connect-with-rdp) veya Windows tabanlı bir dış istemciden aşağıdaki önkoşulları çalıştırın.

- [Azure Stack uyumlu Azure PowerShell modülleri](azure-stack-powershell-install.md)'ni yükler.  
- [Azure Stack çalışmak için gereken araçları](azure-stack-powershell-download.md)indirin.  

## <a name="connect-with-azure-ad"></a>Azure AD 'ye bağlanma

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

## <a name="connect-with-ad-fs"></a>AD FS bağlanma

Azure Active Directory Federasyon Hizmetleri (Azure AD FS) ile PowerShell ile Azure Stack operatör ortamına bağlanın. Bu Azure Resource Manager uç noktası, ASDK için `https://adminmanagement.local.azurestack.external`olarak ayarlanır. Azure Stack tümleşik sistemler için Azure Resource Manager uç noktası almak üzere hizmet sağlayıcınıza başvurun.

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

## <a name="test-the-connectivity"></a>Bağlantıyı test etme

Her şeyin hazır olduğuna göre, Azure Stack içinde kaynak oluşturmak için PowerShell 'i kullanın. Örneğin, bir uygulama için bir kaynak grubu oluşturabilir ve sanal makine ekleyebilirsiniz. **Myresourcegroup**adlı bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın.

```powershell  
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack için şablonlar geliştirin](../user/azure-stack-develop-templates.md).
- [Şablonları PowerShell Ile dağıtın](../user/azure-stack-deploy-template-powershell.md).
  - [Modül başvurusunu Azure Stack](https://docs.microsoft.com/powershell/azure/azure-stack/overview).
