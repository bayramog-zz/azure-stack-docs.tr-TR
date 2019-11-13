---
title: Azure Stack SQL kaynak sağlayıcısı güncelleştiriliyor | Microsoft Docs
description: Azure Stack SQL kaynak sağlayıcısını nasıl güncelleştirebileceğinizi öğrenin.
services: azure-stack
documentationCenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/11/2019
ms.author: mabrigg
ms.reviewer: jiahan
ms.lastreviewed: 11/11/2019
ms.openlocfilehash: b37e4c9f5e7b1aaa1a476b0665a9558e8e86365f
ms.sourcegitcommit: 102ef41963b5d2d91336c84f2d6af3fdf2ce11c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73955410"
---
# <a name="update-the-sql-resource-provider"></a>SQL kaynak sağlayıcısını güncelleştirme

*Uygulama hedefi: Azure Stack tümleşik sistemler.*

Azure Stack yeni bir yapıya güncelleştirildiği zaman yeni bir SQL kaynak sağlayıcısı serbest kalabilir. Mevcut kaynak sağlayıcı çalışmaya devam etse de en kısa sürede en son yapıya güncelleştirmenizi öneririz. 

SQL kaynak sağlayıcısı sürüm 1.1.33.0 sürümünden başlayarak, güncelleştirmeler birikimlidir ve yayımlandıkları sıraya göre yüklenmesi gerekmez; sürümünden itibaren 1.1.24.0 veya sonraki bir sürümden başladığınızı sürece. Örneğin, SQL kaynak sağlayıcısı 'nın 1.1.24.0 sürümünü çalıştırıyorsanız, önce sürüm 1.1.30.0 'yi yüklemeye gerek kalmadan 1.1.33.0 veya üzeri sürüme yükseltebilirsiniz. Kullanılabilir kaynak sağlayıcısı sürümlerini ve bunların desteklendiği Azure Stack sürümünü gözden geçirmek için, [kaynak sağlayıcısı önkoşullarını dağıtma](./azure-stack-sql-resource-provider-deploy.md#prerequisites)' daki sürümler listesine bakın.

Kaynak sağlayıcısını güncelleştirmek için *Updatesqlprovider. ps1* betiğini kullanın. Bu komut dosyası, yeni SQL kaynak sağlayıcısı 'nın indirilmesine dahildir. Güncelleştirme işlemi, [kaynak sağlayıcısını dağıtmak](./azure-stack-sql-resource-provider-deploy.md)için kullanılan işleme benzerdir. Güncelleştirme betiği, DeploySqlProvider. ps1 betiği ile aynı bağımsız değişkenleri kullanır ve sertifika bilgilerini sağlamanız gerekir.

 > [!IMPORTANT]
 > Kaynak sağlayıcısını yükseltmeden önce, yeni işlevsellik, düzeltmeler ve dağıtımınızı etkileyebilecek bilinen sorunlar hakkında bilgi edinmek için sürüm notlarını gözden geçirin.

## <a name="update-script-processes"></a>Betik süreçlerini Güncelleştir

*Updatesqlprovider. ps1* betiği, en son kaynak sağlayıcısı kodu ile yeni bir sanal makıne (VM) oluşturur.

> [!NOTE]
> Market yönetiminden en son Windows Server 2016 çekirdek görüntüsünü indirmeniz önerilir. Bir güncelleştirme yüklemeniz gerekiyorsa, **tek** bir msu paketini yerel bağımlılık yoluna yerleştirebilirsiniz. Bu konumda birden fazla MSU dosyası varsa betik başarısız olur.

*Updatesqlprovider. ps1* betiği yenı bir VM oluşturduktan sonra, komut dosyası eskı sağlayıcı VM 'sinden aşağıdaki ayarları geçirir:

* Veritabanı bilgileri
* barındırma sunucusu bilgileri
* gerekli DNS kaydı

## <a name="update-script-parameters"></a>Betik parametrelerini Güncelleştir

**Updatesqlprovider. ps1** PowerShell betiğini çalıştırdığınızda komut satırından aşağıdaki parametreleri belirtebilirsiniz. Aksi takdirde veya herhangi bir parametre doğrulaması başarısız olursa, gerekli parametreleri sağlamanız istenir.

| Parametre adı | Açıklama | Açıklama veya varsayılan değer |
| --- | --- | --- |
| **CloudAdminCredential** | Ayrıcalıklı uç noktaya erişmek için gerekli olan bulut yöneticisinin kimlik bilgileri. | _Gerekli_ |
| **AzCredential** | Azure Stack hizmeti yönetici hesabı için kimlik bilgileri. Azure Stack dağıtmak için kullandığınız kimlik bilgilerini kullanın. | _Gerekli_ |
| **VMLocalCredential** | SQL kaynak sağlayıcısı VM 'sinin yerel yönetici hesabının kimlik bilgileri. | _Gerekli_ |
| **Ayrıcalıklı Gedendpoint** | Ayrıcalıklı uç noktanın IP adresi veya DNS adı. |  _Gerekli_ |
| **AzureEnvironment** | Azure Stack dağıtmak için kullandığınız hizmet yönetici hesabının Azure ortamı. Yalnızca Azure AD dağıtımları için gereklidir. Desteklenen ortam adları **Azurecsesli**, **AzureUSGovernment**veya Çin Azure AD, **AzureChinaCloud**kullanıyorsa. | AzureCloud |
| **DependencyFilesLocalPath** | Certificate. pfx dosyanızı da bu dizine yerleştirmeniz gerekir. | _Tek düğüm için isteğe bağlıdır, ancak çoklu düğüm için zorunludur_ |
| **DefaultSSLCertificatePassword** | . Pfx sertifikası için parola. | _Gerekli_ |
| **MaxRetryCount** | Bir hata oluşursa her işlemi yeniden denemek istediğiniz zaman sayısı.| 2 |
| **RetryDuration** |Yeniden denemeler arasındaki zaman aşımı aralığı (saniye cinsinden). | 120 |
| **Kaldırma** | Kaynak sağlayıcıyı ve tüm ilişkili kaynakları kaldırır. | Hayır |
| **DebugMode** | Hata durumunda otomatik temizlemeyi önler. | Hayır |

## <a name="update-script-powershell-example"></a>Güncelleştirme betiği PowerShell örneği
Aşağıda, yükseltilmiş bir PowerShell konsolundan çalıştırabileceğiniz *Updatesqlprovider. ps1* betiğini kullanmanın bir örneği verilmiştir. Değişken bilgilerini ve parolaları gerektiği gibi değiştirdiğinizden emin olun:  

> [!NOTE]
> Bu güncelleştirme işlemi yalnızca tümleşik sistemler Azure Stack için geçerlidir.

```powershell
# Install the AzureRM.Bootstrapper module, set the profile and install the AzureStack module
# Note that this might not be the most currently available version of Azure Stack PowerShell.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force
Install-Module -Name AzureStack -RequiredVersion 1.6.0

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but this might have been changed at installation.
$domain = "AzureStack"

# For integrated systems, use the IP address of one of the ERCS virtual machines.
$privilegedEndpoint = "AzS-ERCS01"

# Provide the Azure environment used for deploying Azure Stack. Required only for Azure AD deployments. Supported values for the <environment name> parameter are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using. 
$AzureEnvironment = "<EnvironmentName>"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service administrator account (this can be Azure AD or AD FS).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set the credentials for the new resource provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# Add the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\UpdateSQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -AzureEnvironment $AzureEnvironment `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert 

 ```

## <a name="next-steps"></a>Sonraki adımlar

[SQL kaynak sağlayıcısını koruyun](azure-stack-sql-resource-provider-maintain.md)
