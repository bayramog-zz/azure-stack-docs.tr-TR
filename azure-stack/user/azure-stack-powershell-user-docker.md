---
title: Azure Stack'te PowerShell'i çalıştırmak için Docker'ı kullanma | Microsoft Docs
description: Azure Stack'te PowerShell'i çalıştırmak için Docker'ı kullanma
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
ms.date: 07/09/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 07/09/2019
ms.openlocfilehash: 27f2b4c1817c28cf5d345f5aa9387a26cd18316b
ms.sourcegitcommit: d2df594e8346a875967e3cfb04c23562a1bd2e3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67725758"
---
# <a name="use-docker-to-run-powershell-in-azure-stack"></a>Azure Stack'te PowerShell'i çalıştırmak için Docker'ı kullanma

Bu makalede, çeşitli arabirimleri ile çalışmak için gerekli PowerShell sürümünü çalıştırılacağı Windows tabanlı kapsayıcılar oluşturmak için Docker kullanın. Windows tabanlı kapsayıcılar, Docker kullanmanız gerekir.

## <a name="docker-prerequisites"></a>Docker önkoşulları

1. Yükleme [Docker](https://docs.docker.com/install/).

1. Powershell veya Bash gibi bir komut satırı programı girin:

    ```bash
        Docker --version
    ```

1. Windows 10 gerektiren Windows kapsayıcıları kullanarak Docker çalıştırmanız gerekir. Windows kapsayıcıları için Docker'ı çalıştırdığınızda, geçiş yapın.

1. Azure Stack ile aynı etki alanına katılmış bir makinedeki Docker'ı çalıştırın. Azure Stack geliştirme Seti'ni (ASDK) kullanıyorsanız, yüklemeniz gerekir [VPN uzak makinenizdeki](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn).

## <a name="set-up-a-service-principal-for-using-powershell"></a>Bir hizmet sorumlusu için PowerShell kullanarak ayarlama

Azure Stack'te kaynaklara erişmek üzere PowerShell kullanmak için Azure Active Directory (Azure AD) kiracınız bir hizmet sorumlusu gerekir. Bir temsilci izinleri ile kullanıcı rol tabanlı erişim denetimi (RBAC).

1. Hizmet sorumlunuzu ayarlamak için yönergeleri izleyin. [vermek uygulamalar erişim için Azure Stack kaynakları hizmet sorumluları oluşturma tarafından](azure-stack-create-service-principals.md).

2. Uygulama kimliği, gizli dizi ve daha sonra kullanmak için Kiracı Kimliğinizi unutmayın.

## <a name="docker---azure-stack-api-profiles-module"></a>Docker - Azure Stack API modülü profiller.

Dockerfile, Microsoft Görüntü açılır *microsoft/windowsservercore*, Windows PowerShell yüklü 5.1 sahiptir. Dosya sonra NuGet ve Azure Stack PowerShell modüllerini yükler ve araçları, Azure Stack Araçları'ndan indirir.

1. [Azure stack powershell deposuna indirme](https://github.com/mattbriggs/azure-stack-powershell) bir ZIP dosyası veya kopyası depo olarak.

2. Depo klasörü terminalinizden açın.

3. Deponuzda bir komut satırı arabirimi açın ve ardından aşağıdaki komutu girin:

    ```bash  
    docker build --tag azure-stack-powershell .
    ```

4. Görüntü oluşturulduğunda etkileşimli bir kapsayıcı girerek başlayın:

    ```bash  
        docker run -it azure-stack-powershell powershell
    ```

5. Kabuk cmdlet'lerinizi için hazırdır.

    ```bash
    Windows PowerShell
    Copyright (C) 2016 Microsoft Corporation. All rights reserved.

    PS C:\>
    ```

6. Hizmet sorumlusu kullanarak Azure Stack Örneğinize bağlanın. Artık, bir PowerShell istemi Docker'da kullanıyor. 

    ```powershell
    $passwd = ConvertTo-SecureString <Secret> -AsPlainText -Force
    $pscredential = New-Object System.Management.Automation.PSCredential('<ApplicationID>', $passwd)
    Connect-AzureRmAccount -ServicePrincipal -Credential $pscredential -TenantId <TenantID>
    ```

   PowerShell, hesabı nesnesini döndürür:

    ```powershell  
    Account    SubscriptionName    TenantId    Environment
    -------    ----------------    --------    -----------
    <AccountID>    <SubName>       <TenantID>  AzureCloud
    ```

7. Azure Stack'te bir kaynak grubu oluşturarak bağlantınızı sınayın.

    ```powershell  
    New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
    ```

## <a name="next-steps"></a>Sonraki adımlar

-  Genel bir bakış edinin [Azure Stack'te Azure Stack PowerShell](azure-stack-powershell-overview.md).
- Hakkında bilgi edinin [PowerShell için API profillerini](azure-stack-version-profiles.md) Azure Stack'te.
- Yükleme [Azure Stack PowerShell'e](../operator/azure-stack-powershell-install.md).
- Oluşturma hakkında bilgi [Azure Resource Manager şablonları](azure-stack-develop-templates.md) bulut tutarlılık için.
