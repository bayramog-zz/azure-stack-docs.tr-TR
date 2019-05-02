---
title: Azure Stack için şablonları denetlemek için bir şablon doğrulama aracını kullanın | Microsoft Docs
description: Azure Stack dağıtım şablonları denetleme
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 04/08/2018
ms.date: 04/29/2019
ms.author: v-jay
ms.reviewer: unknown
ms.lastreviewed: 12/27/2018
ms.openlocfilehash: def5b2f49998cfc9a9bf3a857b56b5537b14b9f1
ms.sourcegitcommit: 0973dddb81db03cf07c8966ad66526d775ced8b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "64301071"
---
# <a name="check-your-templates-for-azure-stack-with-the-template-validation-tool"></a>Şablonlarınızı Azure Stack için şablon doğrulama aracı ile denetleyin.

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Şablon doğrulama aracını denetlemek için kullanabileceğiniz olup olmadığını, Azure Resource Manager [şablonları](azure-stack-arm-templates.md) Azure Stack'e dağıtmak için hazır olursunuz. Şablon doğrulama aracı, Azure Stack Araçları'nın bir parçası kullanılabilir. İçinde açıklanan adımları kullanarak Azure Stack araçları indirin [araçları Github'dan indirin](../operator/azure-stack-powershell-download.md) makalesi.

## <a name="overview"></a>Genel Bakış

Bir şablon doğrulamak için ilk bulut özellikleri dosyası oluşturun ve sonra doğrulama aracını çalıştırın. Aşağıdaki PowerShell modülleri Azure Stack araçları kullanabilirsiniz:

- İçinde **CloudCapabilities** klasör: `AzureRM.CloudCapabilities.psm1` Hizmetleri ve Azure Stack bulut sürümlerinde temsil eden bir bulut özellikleri JSON dosyası oluşturur.
- İçinde **TemplateValidator** klasör: `AzureRM.TemplateValidator.psm1` dağıtım için şablonlar Azure Stack'te test etmek için bir bulut özellikleri JSON dosyası kullanır.

## <a name="build-the-cloud-capabilities-file"></a>Bulut özellikleri dosyası oluşturma

Şablon Doğrulayıcı kullanmadan önce çalıştırmak **AzureRM.CloudCapabilities** bir JSON dosyası oluşturmak için PowerShell modülü.

>[!NOTE]
> Tümleşik sisteminizi güncelleştirin veya herhangi bir yeni hizmetleri veya sanal uzantıları ekleyin, bu modül yeniden çalıştırmanız gerekir.

1. Azure Stack bağlantısı olduğundan emin olun. Bu adımlar, Azure Stack Geliştirme Seti konaktan gerçekleştirilebilir veya kullanabileceğiniz bir [VPN](../asdk/asdk-connect.md#connect-to-azure-stack-using-vpn) istasyonunuzdan bağlanmak için.
2. İçeri aktarma **AzureRM.CloudCapabilities** PowerShell Modülü:

    ```powershell
    Import-Module .\CloudCapabilities\AzureRM.CloudCapabilities.psm1
    ```

3. Kullanım `Get-CloudCapabilities` cmdlet'ini hizmet sürümlerini almak ve bir bulut özellikleri JSON dosyası oluşturun. Belirtmezseniz **- OutputPath**, AzureCloudCapabilities.Json geçerli dizinde oluşturulan dosya. Gerçek Azure konumunuz kullanın:

    ```powershell
    Get-AzureRMCloudCapability -Location <your location> -Verbose
    ```

## <a name="validate-templates"></a>Şablonları doğrula

Şablonları kullanarak doğrulamak için aşağıdaki adımları kullanın **AzureRM.TemplateValidator** PowerShell modülü. Kendi şablonlarınızı kullanın veya doğrulama [Azure Stack hızlı başlangıç şablonları](https://github.com/Azure/AzureStack-QuickStart-Templates).

1. İçeri aktarma **AzureRM.TemplateValidator.psm1** PowerShell Modülü:

    ```powershell
    cd "c:\AzureStack-Tools-master\TemplateValidator"
    Import-Module .\AzureRM.TemplateValidator.psm1
    ```

2. Şablon Doğrulayıcı çalıştırın:

    ```powershell
    Test-AzureRMTemplate -TemplatePath <path to template.json or template folder> `
    -CapabilitiesPath <path to cloudcapabilities.json> `
    -Verbose
    ```

Şablon doğrulama uyarıları veya hataları PowerShell konsolunda görüntülenir ve kaynak dizininde bir HTML dosyasına yazılır. Aşağıdaki ekran görüntüsünde, bir doğrulama raporu örneğidir:

![Şablon doğrulama raporu](./media/azure-stack-validate-templates/image1.png)

### <a name="parameters"></a>Parametreler

Şablon Doğrulayıcı aşağıdaki parametreleri destekler.

| Parametre | Açıklama | Gerekli |
| ----- | -----| ----- |
| TemplatePath | Yinelemeli olarak yolunu bulun Azure Resource Manager şablonlarını belirtir. | Evet |
| TemplatePattern | Eşleştirilecek şablon dosyalarının adını belirtir. | Hayır |
| CapabilitiesPath | Bulut özellikleri JSON dosyasının yolunu belirtir. | Evet |
| IncludeComputeCapabilities | VM boyutları ve VM uzantıları gibi Iaas kaynaklarının değerlendirme içerir. | Hayır |
| IncludeStorageCapabilities | SKU türü gibi depolama kaynaklarını değerlendirmesini içerir. | Hayır |
| Rapor | Oluşturulan HTML raporu adını belirtir. | Hayır |
| Ayrıntılı | Konsola, hataları ve uyarıları günlüğe kaydeder. | Hayır|

### <a name="examples"></a>Örnekler

Bu örnekte, tüm doğrulama [Azure Stack hızlı başlangıç şablonları](https://github.com/Azure/AzureStack-QuickStart-Templates) yerel depolama birimine yüklenen. Örnek aynı zamanda sanal makine boyutları ve uzantıları Azure Stack geliştirme Seti'ni özellikleri karşı doğrular:

```powershell
test-AzureRMTemplate -TemplatePath C:\AzureStack-Quickstart-Templates `
-CapabilitiesPath .\TemplateValidator\AzureStackCloudCapabilities_with_AddOns_20170627.json `
-TemplatePattern MyStandardTemplateName.json `
-IncludeComputeCapabilities `
-Report TemplateReport.html
```

## <a name="next-steps"></a>Sonraki adımlar

- [Şablonları Azure Stack'e dağıtma](azure-stack-arm-templates.md)
- [Azure Stack için şablon geliştirme](azure-stack-develop-templates.md)

<!-- Update_Description: wording update -->