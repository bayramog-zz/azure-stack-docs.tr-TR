---
title: Dağıtım yapılandırmaları Azure Stack geliştirme Seti'ni (ASDK) için gönderin | Microsoft Docs
description: Azure Stack geliştirme Seti'ni (ASDK) yükledikten sonra yapmak için önerilen yapılandırma değişiklikleri açıklar.
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
ms.date: 05/08/2019
ms.author: justinha
ms.reviewer: misainat
ms.lastreviewed: 10/10/2018
ms.openlocfilehash: aac9bb8edce4b15d3d058cdb3b6cc6e23aa58493
ms.sourcegitcommit: 23816ec68f67f3ac51f78de925b7631590743a29
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66835002"
---
# <a name="post-asdk-installation-configuration-tasks"></a>ASDK yükleme sonrası yapılandırma görevleri

Sonra [Azure Stack geliştirme Seti'ni (ASDK) yükleme](asdk-install.md), AzureStack\AzureStackAdmin ASDK ana bilgisayarda oturum açtıktan sonra birkaç önerilen yükleme sonrası yapılandırma değişiklikleri yapmak.

## <a name="install-azure-stack-powershell"></a>Azure Stack PowerShell’i yükleme

Azure Stack uyumlu Azure PowerShell modülleri, Azure Stack ile çalışmak için gereklidir.

Azure Stack için PowerShell komutları PowerShell Galerisi'nde yüklenir. PSGallery depo kaydetmek için yükseltilmiş bir PowerShell oturumu açın ve aşağıdaki komutu çalıştırın:

``` Powershell
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```

Azure Stack uyumlu AzureRM modülleri belirtmek için API sürümü profillerini kullanabilirsiniz.  API sürümü profillerini Azure ve Azure Stack arasında sürümü farkları yönetmek için bir yol sağlar. Bir API Sürüm profili, AzureRM PowerShell modülleri belirli API sürümleri ile kümesidir. **AzureRM.BootStrapper** PowerShell Galerisi'nde kullanılabilir modül ile API Sürüm profillerini çalışması için gerekli olan PowerShell cmdlet'leri sağlar.

En son Azure Stack PowerShell modülü ile veya ASDK konak bilgisayara Internet bağlantısı olmadan yükleyebilirsiniz:

> [!IMPORTANT]
> Gerekli sürümü yüklemeden önce emin olun, [tüm mevcut Azure PowerShell modülleri kaldırma](../operator/azure-stack-powershell-install.md#3-uninstall-existing-versions-of-the-azure-stack-powershell-modules).

- **İnternet bağlantısı ile** ASDK ana bilgisayar. Bu modüller, Geliştirme Seti yüklemesine yüklemek için aşağıdaki PowerShell betiğini çalıştırın:

  - 1904 yapılar veya sonraki sürümler için:

    ```powershell  
      Get-Module -Name Azs.* -ListAvailable | Uninstall-Module -Force -Verbose
      Get-Module -Name Azure* -ListAvailable | Uninstall-Module -Force -Verbose

      # Install the AzureRM.BootStrapper module. Select Yes when prompted to install NuGet
      Install-Module -Name AzureRM.BootStrapper

      # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
      Use-AzureRmProfile -Profile 2019-03-01-hybrid -Force
      Install-Module -Name AzureStack -RequiredVersion 1.7.2
    ```

  - Azure Stack 1903 veya daha önceki bir sürümü, yalnızca aşağıdaki iki modülleri yükle:

    ```powershell
    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Install-Module -Name AzureRM -RequiredVersion 2.4.0
    Install-Module -Name AzureStack -RequiredVersion 1.7.1
    ```

    > [!Note]  
    > Azure Stack modülü sürüm 1.7.1 bölünmesi farklıdır. Azure yığını geçirme 1.6.0 Lütfen başvurmak için [Geçiş Kılavuzu](https://aka.ms/azspshmigration171).

  - Azure Stack 1811:

    ``` PowerShell
    # Install the AzureRM.BootStrapper module. Select Yes when prompted to install NuGet.
    Install-Module -Name AzureRM.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force

    # Install Azure Stack Module Version 1.6.0.
    Install-Module -Name AzureStack -RequiredVersion 1.6.0
    ```

  Yükleme başarılı olursa, AzureRM ve AzureStack modülleri çıktısında görüntülenir.

- **İnternet bağlantısı olmadan** ASDK ana bilgisayar. Bağlantısı kesilmiş bir senaryoda, PowerShell modüllerine aşağıdaki PowerShell komutlarını kullanarak internet bağlantısı olan bir makineye indirmeniz gerekir:

  ```powershell
  $Path = "<Path that is used to save the packages>"

  Save-Package `
    -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.3.0
  
  Save-Package `
    -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.5.0
  ```

  Ardından, indirilen paketler ASDK bilgisayara kopyalayın ve varsayılan depo konumu kaydetmek ve bu depodan AzureRM ve AzureStack modüllerini yükleyin:

    ```powershell  
    $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
    $RepoName = "MyNuGetSource"

    Register-PSRepository -Name $RepoName -SourceLocation $SourceLocation -InstallationPolicy Trusted

    Install-Module AzureRM -Repository $RepoName

    Install-Module AzureStack -Repository $RepoName
    ```

## <a name="download-the-azure-stack-tools"></a>Azure Stack araçları indirin

[AzureStack Araçları](https://github.com/Azure/AzureStack-Tools) yönetme ve dağıtma kaynakları Azure Stack için PowerShell modülleri barındıran bir GitHub deposudur. Bu araçları edinmek için GitHub deposunu kopyalayın veya AzureStack Araçlar klasörüne aşağıdaki komutu çalıştırarak yükleyebilirsiniz:

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

## <a name="validate-the-asdk-installation"></a>ASDK yüklemeyi doğrulama

ASDK dağıtımınızın başarılı olmasını sağlamak için aşağıdaki adımları izleyerek Test AzureStack cmdlet'i kullanabilirsiniz:

1. AzureStack\AzureStackAdmin ASDK ana bilgisayarda oturum açın.
2. PowerShell'i yönetici olarak (PowerShell ISE değil) açın.
3. Çalıştırın: `Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint`
4. Çalıştırın: `Test-AzureStack`

Sınamaların tamamlanması birkaç dakika sürebilir. Yükleme başarılı olduysa, çıktı şuna benzer:

![Test-azurestack](media/asdk-post-deploy/test-azurestack.png)

Bir hata oluştuğunda, Yardım almak için sorun giderme adımlarını izleyin.

## <a name="enable-multi-tenancy"></a>Çok kiracılı modeli etkinleştirme

Azure AD kullanarak dağıtımları için şunları yapmanız [çok kiracılı modeli etkinleştirme](../operator/azure-stack-enable-multitenancy.md#enable-multi-tenancy) ASDK yüklemenizin.

> [!NOTE]
> Yönetici veya kullanıcı hesaplarından farklı Azure Stack kaydetmek için kullanılan etki alanları, bir Azure Stack Portal'da oturum açmak için kullanıldığında, Azure Stack kaydetmek için kullanılan etki alanı adı portala eklenmesi gereken url. Örneğin, Azure Stack ile Fabrikam.onmicrosoft.com adresli kayıtlı olup olmadığını ve oturum açma kullanıcı hesabı olan admin@contoso.com, kullanıcı portalında oturum açmak için kullanılacak url şu şekilde olacaktır: https://portal.local.azurestack.external/fabrikam.onmicrosoft.com.

## <a name="next-steps"></a>Sonraki adımlar

[Azure ile ASDK kaydedin](asdk-register.md)
