---
title: Azure Stack 'de şablon doğrulama aracını kullanma | Microsoft Docs
description: Şablon doğrulama aracıyla Azure Stack dağıtım için şablonları denetleyin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 12/27/2018
ms.openlocfilehash: a5e86f0f0719e30ef693c736ac4c70f05830c3bb
ms.sourcegitcommit: b2d19e12a50195bb8925879ee75c186c9604f313
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71961660"
---
# <a name="use-the-template-validation-tool-in-azure-stack"></a>Azure Stack içinde şablon doğrulama aracını kullanma

*Için geçerli: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti @ no__t-0

Azure Resource Manager [şablonlarınızın](azure-stack-arm-templates.md) Azure Stack dağıtıma hazırlanma olup olmadığını denetlemek için şablon doğrulama aracını kullanın. Şablon doğrulama aracı, Azure Stack araçları GitHub deposunun bir parçası olarak kullanılabilir. Azure Stack araçlarını [GitHub 'Dan indirme araçları](../operator/azure-stack-powershell-download.md)bölümünde açıklanan adımları kullanarak indirin.

## <a name="overview"></a>Genel Bakış

Bir şablonu doğrulamak için, önce bir bulut özellikleri dosyası oluşturmanız ve ardından doğrulama aracını çalıştırmanız gerekir. Azure Stack araçlarından aşağıdaki PowerShell modüllerini kullanın:

- **Cloudcapabilities** klasöründe: **Azurerm. CloudCapabilities. psm1** , Azure Stack buluttaki Hizmetleri ve sürümleri temsil eden bir bulut yetenekleri JSON dosyası oluşturur.
- **Templatevalidator** klasöründe: **Azurere. TemplateValidator. psm1** , Azure Stack dağıtım için şablonları test etmek üzere bir bulut yetenekleri JSON dosyası kullanır.

## <a name="build-the-cloud-capabilities-file"></a>Bulut özellikleri dosyasını oluşturun

Şablon Doğrulayıcı 'yı kullanmadan önce, bir JSON dosyası derlemek için **Azurere. CloudCapabilities** PowerShell modülünü çalıştırın.

>[!NOTE]
> Tümleşik sisteminizi güncelleştirir veya yeni hizmetleri ya da sanal uzantıları eklerseniz, bu modülü yeniden çalıştırmalısınız.

1. Azure Stack bağlantınız olduğundan emin olun. Bu adımlar Azure Stack Geliştirme Seti (ASDK) konağından yapılabilir veya iş istasyonunuzdan bağlanmak için bir [VPN](../asdk/asdk-connect.md#connect-to-azure-stack-using-vpn) kullanabilirsiniz.
2. **Azurerd. CloudCapabilities** PowerShell modülünü içeri aktarın:

    ```powershell
    Import-Module .\CloudCapabilities\AzureRM.CloudCapabilities.psm1
    ```

3. Hizmet sürümlerini almak ve bir bulut yetenekleri JSON dosyası oluşturmak için **Get-CloudCapabilities** cmdlet 'ini kullanın. @No__t-0 ' ı belirtmezseniz, **Azurechoparlör Capabilities. JSON** dosyası geçerli dizinde oluşturulur. Gerçek Azure konumunuzu kullanın:

    ```powershell
    Get-AzureRMCloudCapability -Location <your location> -Verbose
    ```

## <a name="validate-templates"></a>Şablonları doğrulama

**Azurere. TemplateValidator** PowerShell modülünü kullanarak şablonları doğrulamak için bu adımları kullanın. Kendi şablonlarınızı kullanabilir veya [Azure Stack hızlı başlangıç şablonlarını](https://github.com/Azure/AzureStack-QuickStart-Templates)kullanabilirsiniz.

1. **Azurerd. TemplateValidator. psm1** PowerShell modülünü içeri aktarın:

    ```powershell
    cd "c:\AzureStack-Tools-master\TemplateValidator"
    Import-Module .\AzureRM.TemplateValidator.psm1
    ```

2. Şablon doğrulayıcısı 'nı çalıştırın:

    ```powershell
    Test-AzureRMTemplate -TemplatePath <path to template.json or template folder> `
    -CapabilitiesPath <path to cloudcapabilities.json> `
    -Verbose
    ```

Şablon doğrulama uyarıları veya hataları PowerShell konsolunda görüntülenir ve kaynak dizindeki bir HTML dosyasına yazılır. Aşağıdaki ekran görüntüsü, doğrulama raporuna bir örnektir:

![Şablon doğrulama raporu](./media/azure-stack-validate-templates/image1.png)

### <a name="parameters"></a>Parametreler

Şablon Doğrulayıcı cmdlet 'i aşağıdaki parametreleri destekler.

| Parametre | Açıklama | Gerekli |
| ----- | -----| ----- |
| `TemplatePath` | Azure Resource Manager şablonlarının yinelemeli olarak bulunacağı yolu belirtir. | Evet |
| `TemplatePattern` | Eşleştirilecek şablon dosyalarının adını belirtir. | Hayır |
| `CapabilitiesPath` | Bulut yetenekleri JSON dosyasının yolunu belirtir. | Evet |
| `IncludeComputeCapabilities` | , VM boyutları ve VM uzantıları gibi IaaS kaynaklarının değerlendirilmesini içerir. | Hayır |
| `IncludeStorageCapabilities` | SKU türleri gibi depolama kaynaklarının değerlendirmesini içerir. | Hayır |
| `Report` | Oluşturulan HTML raporunun adını belirtir. | Hayır |
| `Verbose` | Hataları ve uyarıları konsola kaydeder. | Hayır|

### <a name="examples"></a>Örnekler

Bu örnek, yerel depolamaya indirilen [Azure Stack hızlı başlangıç şablonlarının](https://github.com/Azure/AzureStack-QuickStart-Templates) tümünü doğrular. Örnek ayrıca, sanal makine (VM) boyutlarını ve uzantılarını ASDK özelliklerine göre doğrular:

```powershell
test-AzureRMTemplate -TemplatePath C:\AzureStack-Quickstart-Templates `
-CapabilitiesPath .\TemplateValidator\AzureStackCloudCapabilities_with_AddOns_20170627.json `
-TemplatePattern MyStandardTemplateName.json `
-IncludeComputeCapabilities `
-Report TemplateReport.html
```

## <a name="next-steps"></a>Sonraki adımlar

- [Şablonları Azure Stack dağıtma](azure-stack-arm-templates.md)
- [Şablonları Azure Stack için geliştirme](azure-stack-develop-templates.md)
