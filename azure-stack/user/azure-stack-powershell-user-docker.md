---
title: Azure Stack için PowerShell'i çalıştırmak için Docker'ı kullanma | Microsoft Docs
description: PowerShell'i Azure Stack üzerinde çalıştırmak için Docker'ı kullanma
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
ms.date: 04/25/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: 0eacd2c5a058bca68e86f2d34df8d3cc91987c1c
ms.sourcegitcommit: 85c3acd316fd61b4e94c991a9cd68aa97702073b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2019
ms.locfileid: "64985554"
---
# <a name="use-docker-to-run-powershell"></a>PowerShell'i çalıştırmak için Docker'ı kullanma

Docker üzerinde çalıştırılacağı farklı arabirimler çalışmak için belirli bir PowerShell sürümüne gereken Windows tabanlı kapsayıcılar oluşturmak için kullanabilirsiniz. Windows tabanlı kapsayıcılar, Docker kullanmanız gerekir.

## <a name="docker-prerequisites"></a>Docker önkoşulları

1. Yükleme [Docker](https://docs.docker.com/install/).
2. Powershell veya Bash ve türü gibi bir komut satırı açın:

    ```bash
        Docker -version
    ```

3. Windows 10 gerektiren Windows kapsayıcıları kullanarak Docker çalıştırmanız gerekir. Windows kapsayıcıları için Docker çalıştırırken geçin.

4. Azure Stack ile aynı etki alanına katılmış bir makinedeki Docker çalıştırmanız gerekir. Yüklemenize gerek ASDK kullanıyorsanız [VPN uzak makinenizdeki](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn).

## <a name="service-principals-for-using-powershell"></a>Hizmet sorumluları için PowerShell'i kullanma

Azure stack'teki erişim kaynağa PowerShell kullanmak için Azure AD kiracınızdaki bir hizmet sorumlusu gerekir. Bir temsilci izinleri ile kullanıcı rol tabanlı erişim denetimi.

1. Makalede bulunan yönergeleri takip ederek sorumlunuzu kümesi [vermek uygulamalar erişim için Azure Stack kaynakları hizmet sorumluları oluşturma tarafından](azure-stack-create-service-principals.md).
2. Uygulama Kimliğini not edin gizli ve Kiracı kimliğiniz

## <a name="docker---azure-stack-api-profiles-module"></a>Docker - Azure Stack API profillerini Modülü

Dockerfile, Microsoft görüntü microsoft/Windows PowerShell 5.1 yüklü olan windowsservercore, açılır. Dosya sonra NuGet, Azure Stack PowerShell modüllerini yükler ve araçları, Azure Stack Araçları'ndan indirir.

1. Bu depo bir ZIP olarak indir veya depoyu kopyalayın:  
[https://github.com/mattbriggs/azure-stack-powershell](https://github.com/mattbriggs/azure-stack-powershell)

2. Depo klasörü terminalinizden açın.

3. Bir komut satırı arabirimi, depo içinde açıp şunu yazın:

    ```bash  
    docker build --tag azure-stack-powershell .
    ```

4. Görüntü oluşturulduğunda, etkileşimli bir kapsayıcı başlatabilirsiniz. Şunu yazın:

    ```bash  
        docker run -it azure-stack-powershell powershell
    ```

5. Kabuk cmdlet'lerinizi için hazırdır.

    ```bash
    Windows PowerShell
    Copyright (C) 2016 Microsoft Corporation. All rights reserved.

    PS C:\>
    ```

6. Hizmet sorumlusunu kullanarak, Azure Stack'e bağlanma. Artık, bir PowerShell istemi Docker'da kullanıyor. 

    ```Powershell
    $passwd = ConvertTo-SecureString <Secret> -AsPlainText -Force
    $pscredential = New-Object System.Management.Automation.PSCredential('<ApplicationID>', $passwd)
    Connect-AzureRmAccount -ServicePrincipal -Credential $pscredential -TenantId <TenantID>
    ```

   PowerShell, hesabı nesnesini döndürür:

    ```PowerShell  
    Account    SubscriptionName    TenantId    Environment
    -------    ----------------    --------    -----------
    <AccountID>    <SubName>       <TenantID>  AzureCloud
    ```

7. Azure Stack'te bir kaynak grubu oluşturarak bağlantınızı sınayın.

    ```PowerShell  
    New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
    ```

## <a name="next-steps"></a>Sonraki adımlar

-  Genel bir bakış edinin [Azure Stack'te Azure Stack PowerShell](azure-stack-powershell-overview.md).
- Hakkında bilgi edinin [PowerShell için API profillerini](azure-stack-version-profiles.md) Azure Stack üzerinde.
- [Azure Stack PowerShell'i yükleme](../operator/azure-stack-powershell-install.md).
- Oluşturma hakkında bilgi [Azure Resource Manager şablonları](azure-stack-develop-templates.md) bulut tutarlılık için.