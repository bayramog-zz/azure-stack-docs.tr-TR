---
title: PowerShell ile Azure Stack doğrulamayı otomatikleştirin | Microsoft Docs
description: PowerShell ile Azure Stack doğrulamayı otomatik hale getirebilirsiniz.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/28/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 10/28/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 7c6da84700c73e0976214bd999dfb3ca4ba0ee47
ms.sourcegitcommit: cc3534e09ad916bb693215d21ac13aed1d8a0dde
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73167334"
---
# <a name="automate-azure-stack-validation-with-powershell"></a>PowerShell ile Azure Stack doğrulamayı otomatikleştirme

Hizmet olarak doğrulama (VaaS), **Launchvaastests. ps1** betiğini kullanarak testlerin başlatılmasını otomatikleştirebilme yeteneği sağlar.

> [!NOTE]  
> Otomasyon yalnızca test geçiş iş akışı için kullanılabilir. Paket doğrulama ve çözüm doğrulama iş akışları yalnızca VaaS portalı aracılığıyla desteklenir.

Bu betik şu şekilde kullanılabilir:

> [!div class="checklist"]
> * Ön koşulları yükleme
> * Yerel aracıyı yükleyip başlatma
> * *Tümleştirme*, *işlev*, *güvenilirlik* gibi bir test kategorisi başlatın
> * Test sonuçlarını raporla

## <a name="launch-the-test-pass-workflow"></a>Test geçiş iş akışını Başlat

1. Yükseltilmiş bir PowerShell istemi açın.

2. Otomasyon betiğini indirmek için aşağıdaki betiği çalıştırın:

    ```powershell
    New-Item -ItemType Directory -Path <VaaSLaunchDirectory>
    Set-Location <VaaSLaunchDirectory>
    Invoke-WebRequest -Uri https://storage.azurestackvalidation.com/packages/Microsoft.VaaS.Scripts.latest.nupkg -OutFile "LaunchVaaS.zip"
    Expand-Archive -Path ".\LaunchVaaS.zip" -DestinationPath .\ -Force
    ```

3. Aşağıdaki betiği uygun parametre değerleriyle çalıştırın:

    ```powershell
    $VaaSAccountCreds = New-Object System.Management.Automation.PSCredential "<VaaSUserId>", (ConvertTo-SecureString "<VaaSUserPassword>" -AsPlainText -Force)
    .\LaunchVaaSTests.ps1 -VaaSAccountCreds $VaaSAccountCreds `
                          -VaaSAccountTenantId <VaaSAccountTenantId> `
                          -VaaSSolutionName <VaaSSolutionName> `
                          -VaaSTestPassName <VaaSTestPassName> `
                          -VaaSTestCategories Integration,Functional `
                          -MaxScriptWaitTimeInHours 12 `
                          -ServiceAdminUserName <AzSServiceAdminUser> `
                          -ServiceAdminUserPassword <AzSServiceAdminPassword> `
                          -TenantAdminUserName <AzSTenantAdminUser> `
                          -TenantAdminUserPassword <AzSTenantAdminPassword> `
                          -CloudAdminUserName <AzSCloudAdminUser> `
                          -CloudAdminUserPassword <AzSCloudAdminPassword>
    ```

    **Parametreler**

    | Parametre | Açıklama |
    | --- | --- |
    | Vaasuserıd | VaaS Kullanıcı KIMLIĞINIZ. |
    | VaaSUserPassword | VaaS parolanız. |
    | Vaasaccounttenantıd | VaaS kiracı GUID 'niz. |
    | VaaSSolutionName | Test geçişinin çalıştırılacağı VaaS çözümünün adı. |
    | VaaSTestPassName | Oluşturulacak VaaS testi geçiş iş akışının adı. |
    | VaaSTestCategories | `Integration`, `Functional`veya. `Reliability`. Birden çok değer kullanırsanız, her değeri virgülle ayırın.  |
    | ServiceAdminUserName | Azure Stack hizmeti yönetici hesabınız.  |
    | ServiceAdminPassword | Azure Stack hizmeti parolanız.  |
    | TenantAdminUserName | Birincil kiracının Yöneticisi.  |
    | TenantAdminPassword | Birincil kiracının parolası.  |
    | CloudAdminUserName | Bulut Yöneticisi Kullanıcı adı.  |
    | CloudAdminPassword | Bulut yöneticisinin parolası.  |

4. Testinizin sonuçlarını gözden geçirin. Diğer seçenekler için bkz. [VaaS portalındaki testleri izleme ve yönetme](azure-stack-vaas-monitor-test.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure Stack üzerinde PowerShell hakkında daha fazla bilgi edinmek için en son modülleri gözden geçirin.

> [!div class="nextstepaction"]
> [Azure Stack modülü](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.6.0)
