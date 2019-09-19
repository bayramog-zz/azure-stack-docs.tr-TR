---
title: Azure Stack Kullanıcı aboneliği için fatura sahibini değiştirme | Microsoft Docs
description: Azure Stack Kullanıcı aboneliği için faturalandırma sahibini değiştirmeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: conceptual
ms.date: 09/17/2019
ms.author: justinha
ms.reviewer: shnatara
ms.lastreviewed: 10/19/2018
ms.openlocfilehash: 3c4453974092fd1873e5f77b1074c82851fc1be1
ms.sourcegitcommit: c196463492732218d2474d3a964f88e995272c80
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71094368"
---
# <a name="change-the-billing-owner-for-an-azure-stack-user-subscription"></a>Azure Stack Kullanıcı aboneliği için fatura sahibini değiştirme

Azure Stack işleçler, kullanıcı aboneliğine ait faturalandırma sahibini değiştirmek için PowerShell kullanabilir. Örneğin, sahibi değiştirme nedenlerinden biri, kuruluşunuzu bırakan bir kullanıcıyı değiştirmek içindir.

Bir aboneliğe atanan iki tür *sahip* vardır:

- **Faturalama sahibi**: Varsayılan olarak faturalandırma sahibi, bir tekliften aboneliği alan ve daha sonra bu abonelik için faturalandırma ilişkisine sahip olan kullanıcı hesabıdır. Bu hesap aynı zamanda aboneliğin bir yöneticisidir. Bir abonelikte yalnızca bir kullanıcı hesabı bu gösterimi alabilir. Faturalama sahibi genellikle bir kuruluş veya ekip lideri olur.

  Faturalandırma sahibini değiştirmek için [set-AzsUserSubscription](/powershell/module/azs.subscriptions.admin/set-azsusersubscription) PowerShell cmdlet 'ini kullanabilirsiniz.  

- **RBAC rolleri aracılığıyla eklenen sahipler** -ek kullanıcılara [rol tabanlı erişim denetimi](azure-stack-manage-permissions.md) (RBAC) kullanılarak **sahip** rolü verilebilir. Herhangi bir sayıda ek kullanıcı hesabı, faturalandırma sahibini karmaşıklama etmek için sahip olarak eklenebilir. Ek sahipler Ayrıca aboneliğin yöneticileridir ve abonelik sahibini silme izni hariç olmak üzere aboneliğin tüm ayrıcalıklarına sahiptir.

  Ek sahipleri yönetmek için PowerShell 'i kullanabilirsiniz. Daha fazla bilgi için [bu makaleye](/azure/role-based-access-control/role-assignments-powershell) bakın.

## <a name="change-the-billing-owner"></a>Faturalandırma sahibini değiştirme

Bir Kullanıcı aboneliğinin fatura sahibini değiştirmek için aşağıdaki betiği çalıştırın. Betiği çalıştırmak için kullandığınız bilgisayar Azure Stack bağlanmalıdır ve Azure Stack PowerShell modülünü 1.3.0 veya üstünü çalıştırmalıdır. Daha fazla bilgi için bkz. [Azure Stack PowerShell 'ı Install](azure-stack-powershell-install.md).

>[!NOTE]
>Çok kiracılı bir Azure Stack, yeni sahibin mevcut sahibiyle aynı dizinde olması gerekir. Aboneliğin sahipliğini başka bir dizinde olan bir kullanıcıya sağlayabilmeniz için önce [Bu kullanıcıyı dizininize Konuk olarak davet](/azure/active-directory/b2b/add-users-administrator)etmeniz gerekir.

Komut dosyasındaki aşağıdaki değerleri çalıştırmadan önce değiştirin:

- **$ArmEndpoint**: Ortamınız için Kaynak Yöneticisi uç noktası.
- **$TenantId**: Kiracı KIMLIĞINIZ.
- **$SubscriptionID**: Abonelik KIMLIĞINIZ.
- **$OwnerUpn**: Yeni faturalandırma sahibi olarak eklemek için bir hesap, örneğin **Kullanıcı\@example.com**.

```powershell
# Set up Azure Stack admin environment
Add-AzureRmEnvironment -ARMEndpoint $ArmEndpoint -Name AzureStack-admin
Add-AzureRmAccount -Environment AzureStack-admin -TenantId $TenantId

# Select admin subscription
$providerSubscriptionId = (Get-AzureRmSubscription -SubscriptionName "Default Provider Subscription").Id
Write-Output "Setting context to the Default Provider Subscription: $providerSubscriptionId"
Set-AzureRmContext -Subscription $providerSubscriptionId

# Change user subscription owner
$subscription = Get-AzsUserSubscription -SubscriptionId $SubscriptionId
$Subscription.Owner = $OwnerUpn
Set-AzsUserSubscription -InputObject $subscription
```

[!include[Remove Account](../../includes/remove-account.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Rol tabanlı Access Control yönetme](azure-stack-manage-permissions.md)
