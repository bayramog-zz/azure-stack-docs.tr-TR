---
title: Azure Stack İlkesi modülünü kullanma | Microsoft Docs
description: Azure aboneliğini Azure Stack bir abonelik gibi davranmaya kısıtlama hakkında bilgi edinin
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 937ef34f-14d4-4ea9-960b-362ba986f000
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/13/2019
ms.author: sethm
ms.lastreviewed: 03/26/2019
ms.openlocfilehash: c0872c598cc621250c3b2c5d39aca0e392f71b29
ms.sourcegitcommit: aefcf9c61bd8089a0aaa569af7643e5e15f4947c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68991611"
---
# <a name="manage-azure-policy-using-the-azure-stack-policy-module"></a>Azure Stack Ilkesi modülünü kullanarak Azure ilkesini yönetme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack ilke modülü, aynı sürüm oluşturma ve hizmet kullanılabilirliğiyle Azure Stack ile bir Azure aboneliği yapılandırmanıza olanak sağlar. Modül, bir abonelikte bulunan kaynak türlerini ve Hizmetleri sınırlayan bir Azure ilkesi oluşturmak için [New-AzureRmPolicyDefinition](/powershell/module/azurerm.resources/new-azurermpolicydefinition) PowerShell cmdlet 'ini kullanır. Ardından, [New-AzureRmPolicyAssignment](/powershell/module/azurerm.resources/new-azurermpolicyassignment) cmdlet 'ini kullanarak uygun kapsam içinde bir ilke ataması oluşturursunuz. İlkeyi yapılandırdıktan sonra, Azure aboneliğinizi Azure Stack için hedeflenen uygulamalar geliştirmek üzere kullanabilirsiniz.

## <a name="install-the-module"></a>Modülü yükler

1. [Azure Stack Için PowerShell 'ı Install](../operator/azure-stack-powershell-install.md)ın 1. adımında açıklandığı gibi Azurerd PowerShell modülünün gerekli sürümünü yükler.
2. [Azure Stack araçlarını GitHub 'Dan indirin](../operator/azure-stack-powershell-download.md).
3. [PowerShell 'i Azure Stack ile kullanmak üzere yapılandırın](azure-stack-powershell-configure-user.md).
4. *Azurestack. Policy. psm1* modülünü içeri aktarın:

   ```powershell
   Import-Module .\Policy\AzureStack.Policy.psm1
   ```

## <a name="apply-policy-to-azure-subscription"></a>İlkeyi Azure aboneliğine Uygula

Azure aboneliğinize karşı varsayılan bir Azure Stack ilkesi uygulamak için aşağıdaki komutları kullanabilirsiniz. Bu komutları çalıştırmadan önce, ' `Azure subscription name` yi Azure aboneliğinizin adıyla değiştirin:

```powershell
Add-AzureRmAccount
$s = Select-AzureRmSubscription -SubscriptionName "Azure subscription name"
$policy = New-AzureRmPolicyDefinition -Name AzureStackPolicyDefinition -Policy (Get-AzsPolicy)
$subscriptionID = $s.Subscription.SubscriptionId
New-AzureRmPolicyAssignment -Name AzureStack -PolicyDefinition $policy -Scope /subscriptions/$subscriptionID
```

## <a name="apply-policy-to-a-resource-group"></a>İlkeyi bir kaynak grubuna Uygula

Daha ayrıntılı olan ilkeler uygulamak isteyebilirsiniz. Örneğin, aynı abonelikte çalışan başka kaynaklarınız olabilir. İlke uygulamasını, Azure kaynaklarını kullanarak uygulamalarınızı Azure Stack test etmenizi sağlayan belirli bir kaynak grubuna kapsamını belirtebilirsiniz. Aşağıdaki komutları çalıştırmadan önce, ' yi `Azure subscription name` Azure aboneliğinizin adıyla değiştirin:

```powershell
Add-AzureRmAccount
$rgName = 'myRG01'
$s = Select-AzureRmSubscription -SubscriptionName "Azure subscription name"
$policy = New-AzureRmPolicyDefinition -Name AzureStackPolicyDefinition -Policy (Get-AzsPolicy)
$subscriptionID = $s.Subscription.SubscriptionId
New-AzureRmPolicyAssignment -Name AzureStack -PolicyDefinition $policy -Scope /subscriptions/$subscriptionID/resourceGroups/$rgName
```

## <a name="policy-in-action"></a>İlke eyleminde

Azure ilkesini dağıttıktan sonra, ilke tarafından yasaklanmış bir kaynağı dağıtmaya çalıştığınızda bir hata alırsınız:

![İlke kısıtlaması nedeniyle kaynak dağıtım başarısızlığının sonucu](./media/azure-stack-policy-module/image1.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Şablonları PowerShell ile dağıtma](azure-stack-deploy-template-powershell.md)
* [Azure CLı ile şablonları dağıtma](azure-stack-deploy-template-command-line.md)
* [Şablonları Visual Studio ile dağıtma](azure-stack-deploy-template-visual-studio.md)
