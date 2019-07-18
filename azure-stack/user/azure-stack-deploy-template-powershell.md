---
title: Azure Stack 'de PowerShell kullanarak şablon dağıtma | Microsoft Docs
description: Azure Stack 'de PowerShell kullanarak şablon dağıtma.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 12fe32d7-0a1a-4c02-835d-7b97f151ed0f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: 6824e6bfd0b6c824783c82041fb1a51ba8f5213f
ms.sourcegitcommit: 2063332b4d7f98ee944dd1f443847eea70eb5614
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68303110"
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
3. Aşağıdaki komut dosyasını kullanıcı adınızla ve parolanızla değiştirin `username` , ardından betiği çalıştırın: `password`

    ```powershell
    # Set deployment variables
    $myNum = "001" # Modify this per deployment
    $RGName = "myRG$myNum"
    $myLocation = "local"

    # Create resource group for template deployment
    New-AzureRmResourceGroup -Name $RGName -Location $myLocation

    # Deploy simple IaaS template
    New-AzureRmResourceGroupDeployment `
        -Name myDeployment$myNum `
        -ResourceGroupName $RGName `
        -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
        -NewStorageAccountName mystorage$myNum `
        -DnsNameForPublicIP mydns$myNum `
        -AdminUsername <username> `
        -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
        -VmName myVM$myNum `
        -WindowsOSVersion 2012-R2-Datacenter
    ```

    >[!IMPORTANT]
    > Bu betiği her çalıştırdığınızda, dağıtımınızın üzerine yazılmasını engellemek için `$myNum` parametresinin değerini artırın.

4. Azure Stack portalını açın, **Araştır**' ı seçin ve ardından **sanal makineler** ' i seçerek yeni sanal makinenizi bulun (**myDeployment001**).

## <a name="next-steps"></a>Sonraki adımlar

- [Visual Studio ile şablon dağıtma](azure-stack-deploy-template-visual-studio.md)
