---
title: Azure Stack MySQL kaynak sağlayıcısını güncelleştirin | Microsoft Docs
description: Azure Stack Azure Stack MySQL kaynak sağlayıcısını güncelleştirme hakkında bilgi edinin.
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
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: jiahan
ms.lastreviewed: 01/11/2019
ms.openlocfilehash: 0c37b61cf56b1b730ce36e0574fea5cea6e2e7ec
ms.sourcegitcommit: a23b80b57668615c341c370b70d0a106a37a02da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72682093"
---
# <a name="update-the-mysql-resource-provider-in-azure-stack"></a>Azure Stack MySQL kaynak sağlayıcısını güncelleştirme

*Uygulama hedefi: Azure Stack tümleşik sistemler.*

Azure Stack derlemeleri güncelleştirilirken yeni bir MySQL kaynak sağlayıcısı bağdaştırıcısı serbest kalabilir. Mevcut bağdaştırıcı çalışmaya devam ederken, en kısa sürede en son yapıya güncelleştirmenizi öneririz.

MySQL kaynak sağlayıcısı sürüm 1.1.33.0 sürümünden başlayarak, güncelleştirmeler birikimlidir ve sürüm 1.1.24.0 veya sonraki bir sürümden itibaren yayımlandıkları sırada yüklenmeleri gerekmez. Örneğin, MySQL kaynak sağlayıcısı 'nın 1.1.24.0 sürümünü çalıştırıyorsanız, önce sürüm 1.1.30.0 'yi yüklemeye gerek kalmadan 1.1.33.0 veya üzeri sürüme yükseltebilirsiniz. Kullanılabilir kaynak sağlayıcısı sürümlerini ve bunların desteklendiği Azure Stack sürümünü gözden geçirmek için, [kaynak sağlayıcısı önkoşullarını dağıtma](./azure-stack-mysql-resource-provider-deploy.md#prerequisites)' daki sürümler listesine bakın.

Kaynak sağlayıcısını güncelleştirmek için, **Updatemyısqlprovider. ps1** betiğini kullanın. İşlem, bu makalenin kaynak sağlayıcısını dağıtma bölümünde açıklandığı gibi, bir kaynak sağlayıcısını yüklemek için kullanılan işleme benzerdir. Betik, kaynak sağlayıcının indirileceği dahil edilmiştir. 

 > [!IMPORTANT]
 > Kaynak sağlayıcısını yükseltmeden önce, yeni işlevsellik, düzeltmeler ve dağıtımınızı etkileyebilecek bilinen sorunlar hakkında bilgi edinmek için sürüm notlarını gözden geçirin.

## <a name="update-script-processes"></a>Betik süreçlerini Güncelleştir

**Updatemyısqlprovider. ps1** betiği, en son kaynak sağlayıcısı kodu ile yeni bir sanal makıne (VM) oluşturur ve ayarları eski VM 'den yeni VM 'ye geçirir. Geçirilecek ayarlar veritabanı ve barındırma sunucusu bilgilerini ve gerekli DNS kaydını içerir.

>[!NOTE]
>Market yönetiminden en son Windows Server 2016 çekirdek görüntüsünü indirmeniz önerilir. Bir güncelleştirme yüklemeniz gerekiyorsa, **tek** bir msu paketini yerel bağımlılık yoluna yerleştirebilirsiniz. Bu konumda birden fazla MSU dosyası varsa betik başarısız olur.

Betik, DeployMySqlProvider. ps1 betiği için tanımlanan aynı bağımsız değişkenlerin kullanılmasını gerektirir. Sertifikayı burada da sağlayın.  


## <a name="update-script-parameters"></a>Betik parametrelerini Güncelleştir 
**Updatemyısqlprovider. ps1** PowerShell betiğini çalıştırdığınızda komut satırından aşağıdaki parametreleri belirtin. Aksi takdirde veya herhangi bir parametre doğrulaması başarısız olursa, gerekli parametreleri sağlamanız istenir.

| Parametre Adı | Açıklama | Açıklama veya varsayılan değer | 
| --- | --- | --- | 
| **CloudAdminCredential** | Ayrıcalıklı uç noktaya erişmek için gerekli olan bulut yöneticisinin kimlik bilgileri. | _Gerekli_ | 
| **AzCredential** | Azure Stack hizmeti yönetici hesabı için kimlik bilgileri. Azure Stack dağıtmak için kullandığınız kimlik bilgilerini kullanın. | _Gerekli_ | 
| **VMLocalCredential** |SQL kaynak sağlayıcısı VM 'sinin yerel yönetici hesabının kimlik bilgileri. | _Gerekli_ | 
| **Ayrıcalıklı Gedendpoint** | Ayrıcalıklı uç noktanın IP adresi veya DNS adı. |  _Gerekli_ | 
| **AzureEnvironment** | Azure Stack dağıtmak için kullanılan hizmet yönetici hesabının Azure ortamı. Yalnızca Azure AD dağıtımları için gereklidir. Desteklenen ortam adları **Azurecsesli**, **AzureUSGovernment**veya Çin Azure AD, **AzureChinaCloud**kullanıyorsa. | AzureCloud |
| **DependencyFilesLocalPath** | Certificate. pfx dosyanızın da bu dizine yerleştirilmesi gerekir. | _Isteğe bağlı_ (çok düğümlü için_zorunlu_ ) | 
| **DefaultSSLCertificatePassword** | . Pfx sertifikası için parola. | _Gerekli_ | 
| **MaxRetryCount** | Bir hata oluşursa her işlemi yeniden denemek istediğiniz zaman sayısı.| 2 | 
| **RetryDuration** | Yeniden denemeler arasındaki zaman aşımı aralığı (saniye cinsinden). | 120 | 
| **Kaldırma** | Kaynak sağlayıcısını ve tüm ilişkili kaynakları kaldırın (aşağıdaki notlara bakın). | Hayır | 
| **DebugMode** | Hata durumunda otomatik temizlemeyi önler. | Hayır | 
| **AcceptLicense** | GPL lisansını kabul etmek için isteği atlar.  (https://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | | 

## <a name="update-script-example"></a>Betik örneğini Güncelleştir
Aşağıdaki örnekte, yükseltilmiş bir PowerShell konsolundan çalıştırabileceğiniz *Updatemyısqlprovider. ps1* betiği gösterilmektedir. Değişken bilgilerini ve parolaları gerektiği gibi değiştirdiğinizden emin olun:

> [!NOTE] 
> Güncelleştirme işlemi yalnızca tümleşik sistemler için geçerlidir.

```powershell 
# Install the AzureRM.Bootstrapper module, set the profile and install the AzureStack module
# Note that this might not be the most currently available version of Azure Stack PowerShell.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force
Install-Module -Name AzureStack -RequiredVersion 1.6.0

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time.
$domain = "AzureStack" 

# For integrated systems, use the IP address of one of the ERCS VMs.
$privilegedEndpoint = "AzS-ERCS01" 

# Provide the Azure environment used for deploying Azure Stack. Required only for Azure AD deployments. Supported environment names are AzureCloud, AzureUSGovernment, or AzureChinaCloud. 
$AzureEnvironment = "<EnvironmentName>"

# Point to the directory where the resource provider installation files were extracted. 
$tempDir = 'C:\TEMP\MYSQLRP' 

# The service admin account (can be Azure Active Directory or Active Directory Federation Services).
$serviceAdmin = "admin@mydomain.onmicrosoft.com" 
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass) 
 
# Set credentials for the new resource provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("mysqlrpadmin", $vmLocalAdminPass) 
 
# And the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass) 

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 
 
# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
.$tempDir\UpdateMySQLProvider.ps1 -AzCredential $AdminCreds ` 
-VMLocalCredential $vmLocalAdminCreds ` 
-CloudAdminCredential $cloudAdminCreds ` 
-PrivilegedEndpoint $privilegedEndpoint ` 
-AzureEnvironment $AzureEnvironment `
-DefaultSSLCertificatePassword $PfxPass ` 
-DependencyFilesLocalPath $tempDir\cert ` 
-AcceptLicense 
```  

## <a name="next-steps"></a>Sonraki adımlar
[MySQL kaynak sağlayıcısını koru](azure-stack-mysql-resource-provider-maintain.md)
