---
title: Azure Stack 'de PowerShell kullanarak şablon dağıtma | Microsoft Docs
description: Azure Stack 'de PowerShell kullanarak şablon dağıtma.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/07/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 09/23/2019
ms.openlocfilehash: 38c3c428443afa251c8a6185929bfe0b80680b86
ms.sourcegitcommit: 7226979ece29d9619c959b11352be601562b41d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2019
ms.locfileid: "72304072"
---
# <a name="deploy-a-template-using-powershell-in-azure-stack"></a>Azure Stack 'de PowerShell kullanarak şablon dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack için Azure Resource Manager şablonları dağıtmak üzere PowerShell 'i kullanabilirsiniz. Bu makalede, bir şablonu dağıtmak için PowerShell 'in nasıl kullanılacağı açıklanır.

## <a name="run-azurerm-powershell-cmdlets"></a>Azurerd PowerShell cmdlet 'lerini Çalıştır

Bu örnek **Azurermpowershell** cmdlet 'Leri ve GitHub 'da depolanan bir şablon kullanır. Şablon bir Windows Server 2012 R2 Datacenter sanal makinesi oluşturur.

>[!NOTE]
> Bu örneği denemeden önce, PowerShell 'i bir Azure Stack Kullanıcı için [yapılandırdığınızdan](azure-stack-powershell-configure-user.md) emin olun.

1. [Azurestack-hızlı başlangıç-şablonlar](https://aka.ms/AzureStackGitHub) deposuna gözatıp **101-Simple-Windows-VM** şablonunu bulun. Şablonu şu konuma kaydedin: `C:\templates\azuredeploy-101-simple-windows-vm.json`.
2. Yükseltilmiş bir PowerShell komut istemi açın.
3. Aşağıdaki betikteki `username` ve `password` ' i Kullanıcı adınızla ve parolanızla değiştirin, sonra betiği çalıştırın:

    ```powershell
    # Set deployment variables
    $myNum = "001" # Modify this per deployment
    $RGName = "myRG$myNum"
    $myLocation = "yourregion" # local for the ASDK

    # Create resource group for template deployment
    New-AzureRmResourceGroup -Name $RGName -Location $myLocation

    # Deploy simple IaaS template
    New-AzureRmResourceGroupDeployment `
        -Name myDeployment$myNum `
        -ResourceGroupName $RGName `
        -TemplateUri <path>\AzureStack-QuickStart-Templates\101-vm-windows-create\azuredeploy.json `
        -AdminUsername <username> `
        -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force)
    ```

    >[!IMPORTANT]
    > Bu betiği her çalıştırdığınız zaman, dağıtımınızın üzerine yazılmasını engellemek için `$myNum` parametresinin değerini artırın.

4. Azure Stack portalını açın, **Araştır**' ı seçin ve ardından **sanal makineler** ' i seçerek yeni sanal makinenizi bulun (**myDeployment001**).

## <a name="cancel-a-running-template-deployment"></a>Çalışan bir şablon dağıtımını iptal etme

Çalışan bir şablon dağıtımını iptal etmek için `Stop-AzureRmResourceGroupDeployment` PowerShell cmdlet 'ini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- [Visual Studio ile şablon dağıtma](azure-stack-deploy-template-visual-studio.md)
