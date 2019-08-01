---
title: Azure Stack Geliştirme Seti için dağıtım sonrası yapılandırma (ASDK) | Microsoft Docs
description: Azure Stack Geliştirme Seti yükledikten sonra yapılacak önerilen yapılandırma değişikliklerini açıklar (ASDK).
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2019
ms.author: justinha
ms.reviewer: misainat
ms.lastreviewed: 07/31/2019
ms.openlocfilehash: cbf9872fa75013fdb3e933c102b813924d396a83
ms.sourcegitcommit: bf4d265a3522cbfdd9dd295a0f4ad0daf2ed5eca
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68692104"
---
# <a name="post-asdk-installation-configuration-tasks"></a>ASDK yükleme yapılandırma görevlerinin sonrası

[Azure Stack geliştirme seti (asdk)](asdk-install.md)yüklendikten sonra, asdk ana bilgisayarında AzureStack\AzureStackAdmin olarak oturum açtığınızda birkaç önerilen yükleme sonrası yapılandırma değişikliği yapmanız gerekir.

## <a name="install-azure-stack-powershell"></a>Azure Stack PowerShell’i yükleme

Azure Stack uyumlu Azure PowerShell modüllerinin Azure Stack ile çalışması gerekir.

Azure Stack için PowerShell komutları PowerShell Galerisi aracılığıyla yüklenir. PSGallery deposunu kaydetmek için, yükseltilmiş bir PowerShell oturumu açın ve aşağıdaki komutu çalıştırın:

``` Powershell
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```

Azure Stack uyumlu Azurerd modüllerini belirtmek için API sürüm profillerini kullanabilirsiniz.  API sürüm profilleri, Azure ile Azure Stack arasındaki sürüm farklarını yönetmek için bir yol sağlar. API sürümü profili, belirli API sürümleriyle bir Azurerd PowerShell modülleri kümesidir. PowerShell Galerisi aracılığıyla kullanılabilen **Azurerm. BootStrapper** modülü, API sürüm profilleriyle çalışmak için gerekli olan PowerShell cmdlet 'lerini sağlar.

En son Azure Stack PowerShell modülünü, ASDK ana bilgisayarına Internet bağlantısı olmadan veya bunlarla yükleyebilirsiniz:

> [!IMPORTANT]
> Gerekli sürümü yüklemeden önce, [var olan Azure PowerShell modüllerini kaldırdığınızdan](../operator/azure-stack-powershell-install.md#3-uninstall-existing-versions-of-the-azure-stack-powershell-modules)emin olun.

- ASDK ana bilgisayarından **İnternet bağlantısı ile** . Bu modülleri geliştirme seti yüklemenize yüklemek için aşağıdaki PowerShell betiğini çalıştırın:


  ```powershell  
  Get-Module -Name Azs.* -ListAvailable | Uninstall-Module -Force -Verbose
  Get-Module -Name Azure* -ListAvailable | Uninstall-Module -Force -Verbose

  # Install the AzureRM.BootStrapper module. Select Yes when prompted to install NuGet
  Install-Module -Name AzureRM.BootStrapper

  # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
  Use-AzureRmProfile -Profile 2019-03-01-hybrid -Force
  Install-Module -Name AzureStack -RequiredVersion 1.7.2
  ```

  Yükleme başarılı olursa Azurerd ve AzureStack modülleri çıktıda görüntülenir.

- ASDK ana bilgisayarından **İnternet bağlantısı olmadan** . Bağlantısı kesilmiş bir senaryoda, önce aşağıdaki PowerShell komutlarını kullanarak PowerShell modüllerini Internet bağlantısı olan bir makineye indirmeniz gerekir:

  ```powershell
  $Path = "<Path that is used to save the packages>"

  Save-Package `
    -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.3.0
  
  Save-Package `
    -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.5.0
  ```

  Ardından, indirilen paketleri ASDK bilgisayarına kopyalayın ve konumu varsayılan depo olarak kaydedin ve bu depodan Azurere ve AzureStack modüllerini yükler:

    ```powershell  
    $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
    $RepoName = "MyNuGetSource"

    Register-PSRepository -Name $RepoName -SourceLocation $SourceLocation -InstallationPolicy Trusted

    Install-Module AzureRM -Repository $RepoName

    Install-Module AzureStack -Repository $RepoName
    ```

## <a name="download-the-azure-stack-tools"></a>Azure Stack araçlarını indirin

[Azurestack-Tools](https://github.com/Azure/AzureStack-Tools) , Azure Stack kaynakları yönetmek ve dağıtmak için PowerShell modüllerini barındıran bir GitHub deposudur. Bu araçları edinmek için GitHub deposunu kopyalayın veya aşağıdaki betiği çalıştırarak AzureStack-Tools klasörünü indirin:

  ```powershell
  # Change directory to the root directory.
  cd \

  # Enforce usage of TLSv1.2 to download the Azure Stack tools archive from GitHub
  [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
  Invoke-WebRequest `
    -Uri https://github.com/Azure/AzureStack-Tools/archive/master.zip `
    -OutFile master.zip

  # Expand the downloaded files.
  Expand-Archive -Path master.zip -DestinationPath . -Force

  # Change to the tools directory.
  cd AzureStack-Tools-master
  ```

## <a name="validate-the-asdk-installation"></a>ASDK yüklemesini doğrulama

ASDK dağıtımınızın başarılı olduğundan emin olmak için, aşağıdaki adımları izleyerek test-AzureStack cmdlet 'ini kullanabilirsiniz:

1. ASDK ana bilgisayarında AzureStack\AzureStackAdmin olarak oturum açın.
2. PowerShell 'i yönetici olarak açın (PowerShell ıSE).
3. Çalıştırın: `Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint`
4. Çalıştırın: `Test-AzureStack`

Testlerin tamamlanması birkaç dakika sürer. Yükleme başarılı olduysa, çıkış aşağıdakine benzer şekilde görünür:

![test-azurestack](media/asdk-post-deploy/test-azurestack.png)

Bir hata oluşursa, yardım almak için sorun giderme adımlarını izleyin.

## <a name="enable-multi-tenancy"></a>Çok kiracılı modeli etkinleştirme

Azure AD kullanan dağıtımlar için, ASDK yüklemeniz için [çok kiracılı hale](../operator/azure-stack-enable-multitenancy.md#enable-multi-tenancy) getirmeniz gerekir.

> [!NOTE]
> Azure Stack kaydetmek için kullanılan etki alanlarından yönetici veya Kullanıcı hesapları Azure Stack portalında oturum açmak için kullanıldığında, Azure Stack kaydetmek için kullanılan etki alanı adının Portal URL 'sine eklenmesi gerekir. Örneğin, Azure Stack fabrikam.onmicrosoft.com ile kaydedilmişse ve Kullanıcı hesabı oturum açma ise admin@contoso.com, Kullanıcı portalında oturum açmak için kullanılacak URL şöyle olacaktır:. https://portal.local.azurestack.external/fabrikam.onmicrosoft.com

## <a name="next-steps"></a>Sonraki adımlar

[ASDK 'yi Azure ile kaydetme](asdk-register.md)
