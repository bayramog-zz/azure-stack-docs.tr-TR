---
title: Azure Stack 'de PowerShell çalıştırmak için Docker 'ı kullanma | Microsoft Docs
description: Azure Stack 'de PowerShell çalıştırmak için Docker kullanma
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: article
ms.date: 10/10/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 07/09/2019
ms.openlocfilehash: 118f29c46a1b11c07c62407f19b86aa28ada3bd1
ms.sourcegitcommit: a6d47164c13f651c54ea0986d825e637e1f77018
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72277792"
---
# <a name="use-docker-to-run-powershell-in-azure-stack"></a>Azure Stack 'de PowerShell çalıştırmak için Docker kullanma

Bu makalede, çeşitli arabirimlere çalışmak için gerekli olan PowerShell sürümünün çalıştırılacağı Windows tabanlı kapsayıcılar oluşturmak için Docker 'ı kullanırsınız. Docker 'da Windows tabanlı kapsayıcılar kullanmanız gerekir.

## <a name="docker-prerequisites"></a>Docker önkoşulları

1. [Docker](https://docs.docker.com/install/)'ı yükler.

1. PowerShell veya Bash gibi bir komut satırı programında şunu girin:

    ```bash
        Docker --version
    ```

1. Windows 10 gerektiren Windows kapsayıcıları ' nı kullanarak Docker çalıştırmanız gerekir. Docker çalıştırdığınızda Windows kapsayıcılarına geçiş yapın.

1. Azure Stack ile aynı etki alanına katılmış bir makineden Docker 'ı çalıştırın. Azure Stack Geliştirme Seti (ASDK) kullanıyorsanız, [VPN 'i uzak makinenize](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn)yüklemeniz gerekir.

## <a name="set-up-a-service-principal-for-using-powershell"></a>PowerShell 'i kullanmak için bir hizmet sorumlusu ayarlama

PowerShell 'i Azure Stack kaynaklara erişmek üzere kullanmak için, Azure Active Directory (Azure AD) kiracınızda bir hizmet sorumlusu olması gerekir. Kullanıcı rolü tabanlı erişim denetimi (RBAC) ile izinleri devredebilirsiniz.

1. Hizmet sorumlusunu ayarlamak için, [hizmet sorumlularını oluşturarak uygulama Azure Stack kaynaklarına erişim verme](azure-stack-create-service-principals.md)konusundaki yönergeleri izleyin.

2. Uygulama KIMLIĞI, gizli anahtar ve kiracı KIMLIĞINIZI daha sonra kullanmak üzere aklınızda edin.

## <a name="docker---azure-stack-api-profiles-module"></a>Docker-Azure Stack API profilleri modülü

Dockerfile, Windows PowerShell 5,1 'nin yüklü olduğu Microsoft */windowsservercore*Microsoft görüntüsünü açar. Dosya daha sonra NuGet ve Azure Stack PowerShell modüllerini yükler ve araçları Azure Stack araçlarından indirir.

1. [Azure-Stack-PowerShell deposunu](https://github.com/mattbriggs/azure-stack-powershell) bir ZIP dosyası olarak indirin veya depoyu kopyalayın.

2. Terminalinizden depo klasörünü açın.

3. Deponuzda bir komut satırı arabirimi açın ve aşağıdaki komutu girin:

    ```bash  
    docker build --tag azure-stack-powershell .
    ```

4. Görüntü yapılandırıldığında, şunu girerek etkileşimli bir kapsayıcı başlatın:

    ```bash  
        docker run -it azure-stack-powershell powershell
    ```

5. Kabuk cmdlet 'larınız için hazırlayın.

    ```bash
    Windows PowerShell
    Copyright (C) 2016 Microsoft Corporation. All rights reserved.

    PS C:\>
    ```

6. Hizmet sorumlusunu kullanarak Azure Stack örneğine bağlanın. Artık Docker 'da bir PowerShell istemi kullanıyorsunuz. 

    ```powershell
    $passwd = ConvertTo-SecureString <Secret> -AsPlainText -Force
    $pscredential = New-Object System.Management.Automation.PSCredential('<ApplicationID>', $passwd)
    Connect-AzureRmAccount -ServicePrincipal -Credential $pscredential -TenantId <TenantID>
    ```

   PowerShell, hesap nesneniz döndürür:

    ```powershell  
    Account    SubscriptionName    TenantId    Environment
    -------    ----------------    --------    -----------
    <AccountID>    <SubName>       <TenantID>  AzureCloud
    ```

7. Azure Stack ' de bir kaynak grubu oluşturarak bağlanabilirliğinizi test edin.

    ```powershell  
    New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
    ```

## <a name="next-steps"></a>Sonraki adımlar

-  [Azure Stack Azure Stack PowerShell](azure-stack-powershell-overview.md)'e genel bakış konusunu okuyun.
- Azure Stack 'de [PowerShell Için API profilleri](azure-stack-version-profiles.md) hakkında bilgi edinin.
- [Azure Stack PowerShell](../operator/azure-stack-powershell-install.md)'i yükler.
- Bulut tutarlılığı için [Azure Resource Manager şablonları](azure-stack-develop-templates.md) oluşturma hakkında bilgi edinin.
