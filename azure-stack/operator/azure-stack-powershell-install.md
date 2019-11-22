---
title: Azure Stack için PowerShell 'i yükler | Microsoft Docs
description: Azure Stack için PowerShell 'i yüklemeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 09/19/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 09/19/2019
ms.openlocfilehash: 813cfb72a2fad2b22dfce5baff8680b30d2c599d
ms.sourcegitcommit: cefba8d6a93efaedff303d3c605b02bd28996c5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74298813"
---
# <a name="install-powershell-for-azure-stack"></a>Azure Stack için PowerShell yükleme

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure PowerShell, Azure Stack kaynaklarınızı yönetmek için Azure Resource Manager modelini kullanan bir cmdlet kümesi sağlar.

Bulutunuz ile çalışmak için Azure Stack uyumlu PowerShell modülleri yüklemeniz gerekir. Azure Stack, küresel Azure 'da kullanılan yeni **Azureaz** modülü yerine **Azurerd** modülünü kullanır. Ayrıca, Azure Stack kaynak sağlayıcıları için uyumlu uç noktaları belirtmek üzere *API profillerini* de kullanmanız gerekir.

API profilleri, Azure ile Azure Stack arasındaki sürüm farklarını yönetmek için bir yol sağlar. API sürümü profili, belirli API sürümleriyle Azure Resource Manager PowerShell modülleri kümesidir. Her bulut platformunda desteklenen bir API sürüm profili kümesi vardır. Örneğin Azure Stack, **2019-03-01 karma**gibi belirli bir profil sürümünü destekler. Bir profil yüklediğinizde, belirtilen profile karşılık gelen Azure Resource Manager PowerShell modülleri yüklenir.

İnternet bağlantılı, kısmen bağlı veya bağlantısı kesik senaryolarda Azure Stack uyumlu PowerShell modülleri yükleyebilirsiniz. Bu makalede, bu senaryolara yönelik ayrıntılı yönergeler adım adım açıklanmaktadır.

## <a name="1-verify-your-prerequisites"></a>1. ön koşulları doğrulayın

Azure Stack ve PowerShell ile çalışmaya başlamadan önce aşağıdaki önkoşullara sahip olmanız gerekir:

- **PowerShell sürüm 5,0** <br>
Sürümünüzü denetlemek için **$PSVersionTable. psversion** çalıştırın ve **ana** sürümü karşılaştırın. PowerShell 5,0 ' i yoksa, [Windows PowerShell 'ı yükleme](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell)' yi izleyin.

  > [!Note]
  > PowerShell 5,0, bir Windows makinesi gerektirir.

- **PowerShell 'i yükseltilmiş bir komut Isteminde çalıştırın**.

- **PowerShell Galerisi erişim** <br>
  [PowerShell Galerisi](https://www.powershellgallery.com)erişiminizin olması gerekir. Galeri, PowerShell içeriği için merkezi depodur. **PowerShellGet** modülü PowerShell yapılarını bulmak, yüklemek, güncelleştirmek ve yayımlamak için cmdlet 'leri içerir. Bu yapıtlara örnek olarak modüller, DSC kaynakları, rol özellikleri ve PowerShell Galerisi ve diğer özel depolardaki betikler verilebilir. PowerShell 'i bağlantısı kesik bir senaryoda kullanıyorsanız, internet bağlantısı olan bir makineden kaynak almalısınız ve bunları, bağlantısı kesilen makinenize erişilebilen bir konumda depolıyoruz.

## <a name="2-validate-the-powershell-gallery-accessibility"></a>2. erişilebilirliği PowerShell Galerisi doğrulayın

PSGallery 'nin bir depo olarak kaydedilip kaydedilmediği doğrulayın.

> [!Note]  
> Bu adım internet erişimi gerektirir.

Yükseltilmiş bir PowerShell istemi açın ve aşağıdaki cmdlet 'leri çalıştırın:

```powershell
Import-Module -Name PowerShellGet -ErrorAction Stop
Import-Module -Name PackageManagement -ErrorAction Stop
Get-PSRepository -Name "PSGallery"
```

Depo kayıtlı değilse, yükseltilmiş bir PowerShell oturumu açın ve aşağıdaki komutu çalıştırın:

```powershell
Register-PSRepository -Default
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```

## <a name="3-uninstall-existing-versions-of-the-azure-stack-powershell-modules"></a>3. Azure Stack PowerShell modüllerinin var olan sürümlerini kaldırın

Gerekli sürümü yüklemeden önce, daha önce yüklenmiş Azure Stack Azurerd PowerShell modüllerini kaldırdığınızdan emin olun. Aşağıdaki iki yöntemden birini kullanarak modülleri kaldırın:

1. Var olan Azurere PowerShell modüllerini kaldırmak için, tüm etkin PowerShell oturumlarını kapatın ve aşağıdaki cmdlet 'leri çalıştırın:

    ```powershell
    Get-Module -Name Azs.* -ListAvailable | Uninstall-Module -Force -Verbose
    Get-Module -Name Azure* -ListAvailable | Uninstall-Module -Force -Verbose
    ```

    ' Modül zaten kullanımda ' gibi bir hatayla karşılaşırsanız, modülleri kullanan PowerShell oturumlarını kapatın ve yukarıdaki betiği yeniden çalıştırın.

2. `Azure` veya `Azs.` ile başlayan tüm klasörleri `C:\Program Files\WindowsPowerShell\Modules` ve `C:\Users\{yourusername}\Documents\WindowsPowerShell\Modules` klasörlerinden silin. Bu klasörlerin silinmesi, var olan tüm PowerShell modüllerini kaldırır.

## <a name="4-connected-install-powershell-for-azure-stack-with-internet-connectivity"></a>4. bağlı: internet bağlantısı ile Azure Stack için PowerShell 'i yükler

İhtiyaç duyduğunuz API sürüm profili ve Azure Stack PowerShell modülleri, çalıştırmakta olduğunuz Azure Stack sürümüne göre değişir.

### <a name="install-azure-stack-powershell"></a>Azure Stack PowerShell’i yükleme

Bu modülleri geliştirme iş istasyonunuza yüklemek için aşağıdaki PowerShell betiğini çalıştırın:

- Azure Stack 1904 veya üzeri için:

    ```powershell  
    # Install the AzureRM.BootStrapper module. Select Yes when prompted to install NuGet
    Install-Module -Name AzureRM.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2019-03-01-hybrid -Force
    Install-Module -Name AzureStack -RequiredVersion 1.7.2
    ```
  
- Azure Stack sürüm 1903 veya önceki sürümlerde yalnızca aşağıdaki iki modülü de yüklersiniz:

    ```powershell  
    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.

    Install-Module -Name AzureRM -RequiredVersion 2.4.0
    Install-Module -Name AzureStack -RequiredVersion 1.7.1
    ```

    > [!Note]  
    > - Azure Stack Module sürümü 1.7.1, bir son değişiklik sürümüdür. Azure Stack 1.6.0 'den geçiş yapmak için lütfen [geçiş kılavuzuna](https://aka.ms/azspshmigration171)bakın.
    > - Azurermmodule sürümü 2.4.0, Remove-AzureRmStorageAccount cmdlet 'i için bir son değişiklik ile birlikte gelir. Bu cmdlet, hiçbir onay olmadan depolama hesabını kaldırmak için `-Force` parametrenin belirtilmesini bekler.
    > - Azure Stack sürüm 1901 veya üzeri için modülleri yüklemek üzere **Azurerd. BootStrapper** yüklemeniz gerekmez.
    > - 2018-03-01 karma profilini, Azure Stack sürüm 1901 veya üzeri sürümlerde yukarıdaki Azurerd modüllerini kullanmaya ek olarak yüklemeyin.

### <a name="confirm-the-installation-of-powershell"></a>PowerShell yüklenmesini onaylama

Aşağıdaki komutu çalıştırarak yüklemeyi onaylayın:

```powershell
Get-Module -Name "Azure*" -ListAvailable
Get-Module -Name "Azs*" -ListAvailable
```

Yükleme başarılı olursa `AzureRM` ve `AzureStack` modülleri çıktıda görüntülenir.

## <a name="5-disconnected-install-powershell-without-an-internet-connection"></a>5. bağlantısı kesik: internet bağlantısı olmadan PowerShell 'i yükler

Bağlantısı kesik bir senaryoda, önce PowerShell modüllerini internet bağlantısı olan bir makineye indirmelisiniz. Ardından, bunları yükleme için Azure Stack Geliştirme Seti (ASDK) olarak aktarırsınız.

İnternet bağlantısı olan bir bilgisayarda oturum açın ve Azure Stack sürümünüze bağlı olarak Azure Resource Manager ve Azure Stack paketlerini indirmek için aşağıdaki komut dosyalarını kullanın.

Yüklemede dört adım vardır:

1. Azure Stack PowerShell 'i bağlı bir makineye yükler.
2. Ek depolama özelliklerini etkinleştirin.
3. PowerShell paketlerini, bağlantısı kesilen iş istasyonunuza aktarma.
4. NuGet sağlayıcısını, bağlantısı kesilen iş istasyonunuzda el ile önyükleyebilirsiniz.
5. PowerShell 'in yüklenmesini onaylayın.

### <a name="install-azure-stack-powershell"></a>Azure Stack PowerShell’i yükleme

- Azure Stack 1904 veya üzeri.

    ```powershell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.5.0
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.7.2
    ```

- Azure Stack 1903 veya daha önceki bir sürüm.

    ```powershell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.4.0
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.7.1
    ```

    > [!Note]  
    > Azure Stack Module sürümü 1.7.1 bir son değişiklik. Azure Stack 1.6.0 'den geçiş yapmak için lütfen [geçiş kılavuzuna](https://github.com/Azure/azure-powershell/tree/AzureRM/documentation/migration-guides/Stack)bakın.

    > [!NOTE]
    > İnternet bağlantısı olmayan makinelerde, telemetri veri toplamayı devre dışı bırakmak için aşağıdaki cmdlet 'i yürütmenizi öneririz. Telemetri veri toplamayı devre dışı bırakmadan cmdlet 'lerde bir performans düşüşü yaşayabilirsiniz. Bu yalnızca İnternet bağlantısı olmayan makineler için geçerlidir
    > ```powershell
    > Disable-AzureRmDataCollection
    > ```

### <a name="add-your-packages-to-your-workstation"></a>Paketlerinizi iş istasyonunuza ekleyin

1. İndirilen paketleri bir USB cihazına kopyalayın.

2. Bağlantısı kesilen iş istasyonunda oturum açın ve paketleri USB cihazdan iş istasyonundaki bir konuma kopyalayın.

3. NuGet sağlayıcısını, bağlantısı kesilen iş istasyonunuzda el ile önyükleyebilirsiniz. Yönergeler için bkz. [Internet 'e bağlı olmayan bir makinede NuGet sağlayıcısını el ile önyükleme](https://docs.microsoft.com/powershell/gallery/how-to/getting-support/bootstrapping-nuget#manually-bootstrapping-the-nuget-provider-on-a-machine-that-is-not-connected-to-the-internet).

4. Bu konumu varsayılan depo olarak Kaydet ve bu depodan Azurerd ve `AzureStack` modüllerini yükler:

   ```powershell
   # requires -Version 5
   # requires -RunAsAdministrator
   # requires -Module PowerShellGet
   # requires -Module PackageManagement

   $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
   $RepoName = "MyNuGetSource"

   Register-PSRepository -Name $RepoName -SourceLocation $SourceLocation -InstallationPolicy Trusted

   Install-Module -Name AzureRM -Repository $RepoName

   Install-Module -Name AzureStack -Repository $RepoName
   ```

### <a name="confirm-the-installation-of-powershell"></a>PowerShell yüklenmesini onaylama

Aşağıdaki komutu çalıştırarak yüklemeyi onaylayın:

```powershell
Get-Module -Name "Azure*" -ListAvailable
Get-Module -Name "Azs*" -ListAvailable
```

## <a name="6-configure-powershell-to-use-a-proxy-server"></a>6. PowerShell 'i bir proxy sunucu kullanacak şekilde yapılandırma

Bir proxy sunucusunun İnternet 'e erişmesini gerektiren senaryolarda, önce PowerShell 'i mevcut bir proxy sunucusunu kullanacak şekilde yapılandırırsınız:

1. Yükseltilmiş bir PowerShell istemi açın.
2. Aşağıdaki komutları çalıştırın:

   ```powershell
   #To use Windows credentials for proxy authentication
   [System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials

   #Alternatively, to prompt for separate credentials that can be used for #proxy authentication
   [System.Net.WebRequest]::DefaultWebProxy.Credentials = Get-Credential
   ```

## <a name="next-steps"></a>Sonraki adımlar

- [GitHub 'dan Azure Stack araçları indirin](azure-stack-powershell-download.md)
- [Azure Stack kullanıcının PowerShell ortamını yapılandırma](../user/azure-stack-powershell-configure-user.md)
- [Azure Stack işlecinin PowerShell ortamını yapılandırma](azure-stack-powershell-configure-admin.md)
- [Azure Stack’de API sürümü profillerini yönetme](../user/azure-stack-version-profiles.md)
