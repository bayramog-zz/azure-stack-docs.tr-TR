---
title: Azure Stack 'de PowerShell kullanarak şablon dağıtma | Microsoft Docs
description: Azure Stack 'de PowerShell kullanarak şablon dağıtma.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2019
ms.author: sethm
ms.reviewer: sijuman
ms.lastreviewed: 09/23/2019
ms.openlocfilehash: af4ac82e03b96b4fc3f6d728cbebf5a6fa9d6388
ms.sourcegitcommit: e8aa26b078a9bab09c8fafd888a96785cc7abb4d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71708954"
---
# <a name="deploy-a-template-using-powershell-in-azure-stack"></a>Azure Stack 'de PowerShell kullanarak şablon dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack için Azure Resource Manager şablonları dağıtmak üzere PowerShell 'i kullanabilirsiniz. Bu makalede, bir şablonu dağıtmak için PowerShell 'in nasıl kullanılacağı açıklanır.

## <a name="run-azurerm-powershell-cmdlets"></a>Azurerd PowerShell cmdlet 'lerini Çalıştır

Bu örnek **Azurermpowershell** cmdlet 'Leri ve GitHub 'da depolanan bir şablon kullanır. Şablon bir Windows Server 2012 R2 Datacenter sanal makinesi oluşturur.

>[!NOTE]
> Bu örneği denemeden önce, PowerShell 'i bir Azure Stack Kullanıcı için [yapılandırdığınızdan](azure-stack-powershell-configure-user.md) emin olun.

1. [Azurestackgithub](https://aka.ms/AzureStackGitHub) deposuna gözatıp **101-Simple-Windows-VM** şablonunu bulun. Şablonu bu konuma kaydet: `C:\templates\azuredeploy-101-simple-windows-vm.json`.
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
    > Bu betiği her çalıştırdığınızda, dağıtımınızın üzerine yazılmasını engellemek için `$myNum` parametresinin değerini artırın.

4. Azure Stack portalını açın, **Araştır**' ı seçin ve ardından **sanal makineler** ' i seçerek yeni sanal makinenizi bulun (**myDeployment001**).

## <a name="cancel-a-running-template-deployment"></a>Çalışan bir şablon dağıtımını iptal etme

Çalışan bir şablon dağıtımını iptal etmek için `Stop-AzureRmResourceGroupDeployment` PowerShell cmdlet 'ini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- [Visual Studio ile şablon dağıtma](azure-stack-deploy-template-visual-studio.md)
