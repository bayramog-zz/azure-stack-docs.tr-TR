---
title: Azure Stack PowerShell kullanarak güncelleştirmeleri izleme | Microsoft Docs
description: Azure Stack güncelleştirmeleri PowerShell kullanarak izlemeyi öğrenin
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
ms.date: 08/23/2019
ms.author: mabrigg
ms.lastreviewed: 08/23/2019
ms.reviewer: ppacent
ms.openlocfilehash: a49f2f5c6b06f3362e4cb85553b765ea9fea1a72
ms.sourcegitcommit: b8260ef3e43f3703dd0df16fb752610ec8a86942
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70010214"
---
# <a name="monitor-updates-in-azure-stack-using-powershell"></a>Azure Stack PowerShell kullanarak güncelleştirmeleri izleme

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Azure Stack yönetim uç noktalarını kullanarak güncelleştirmelerinizi izleyebilir ve yönetebilirsiniz. Bunlara PowerShell ile erişilebilir. Azure Stack 'de PowerShell ile ayarlama hakkında yönergeler için bkz. [Azure Stack Için PowerShell 'ı kurma](azure-stack-powershell-install.md).

Güncelleştirmelerinizi yönetmek için aşağıdaki PowerShell cmdlet 'ini kullanabilirsiniz:

| Cmdlet | Açıklama |
|------------------------------------------------------|-------------|
| [Get-AzsUpdate](https://docs.microsoft.com/powershell/module/azs.update.admin/Get-AzsUpdate?view=azurestackps-1.7.2) | Kullanılabilir güncelleştirmelerin listesini alın. |
| [Get-AzsUpdateLocation](https://docs.microsoft.com/powershell/module/azs.update.admin/Get-AzsUpdateLocation?view=azurestackps-1.7.2)| Güncelleştirme konumlarının listesini alın. |
| [Get-AzsUpdateRun](https://docs.microsoft.com/powershell/module/azs.update.admin/Get-AzsUpdateRun?view=azurestackps-1.7.2) | Güncelleştirme çalışmalarının listesini alın.  |
| [Install-AzsUpdate](https://docs.microsoft.com/powershell/module/azs.update.admin/Install-AzsUpdate?view=azurestackps-1.7.2) | Güncelleştirme konumunda belirli bir güncelleştirmeyi uygulayın. |
| [Özgeçmişi-AzsUpdateRun](https://docs.microsoft.com/powershell/module/azs.update.admin/Resume-AzsUpdateRun?view=azurestackps-1.7.2) | Daha önce başlatılan bir güncelleştirme çalıştırmasını sürdürür. |

## <a name="get-a-list-of-update-runs"></a>Güncelleştirme çalışmalarının bir listesini alın

Güncelleştirme çalıştırmaları komutunun listesini almak için:

```powershell
Get-AzsUpdateRun -UpdateName Microsoft1.0.180302.1
```

## <a name="resume-a-failed-update-operation"></a>Başarısız bir güncelleştirme işlemini sürdürür

Güncelleştirme başarısız olursa, aşağıdaki komutu çalıştırarak güncelleştirme çalıştırmasını kaldığınız yerden sürdürebilirsiniz:

```powershell
Get-AzsUpdateRun -Name 5173e9f4-3040-494f-b7a7-738a6331d55c -UpdateName Microsoft1.0.180305.1 | Resume-AzsUpdateRun
```

## <a name="next-steps"></a>Sonraki adımlar

-   [Azure Stack güncelleştirmeleri yönetme](https://docs.microsoft.com/azure-stack/operator/azure-stack-updates)