---
title: Azure Stack SQL veritabanlarını kullanma | Microsoft Docs
description: SQL veritabanlarını Azure Stack bir hizmet olarak nasıl dağıtabileceğinizi ve SQL Server kaynak sağlayıcısı bağdaştırıcısını dağıtmaya yönelik hızlı adımları öğrenin.
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
ms.date: 07/23/2019
ms.lastreviewed: 03/18/2019
ms.author: mabrigg
ms.reviewer: jiahan
ms.openlocfilehash: 22fdf65ae4949468d3408097c787f0a5b93dd75f
ms.sourcegitcommit: b95983e6e954e772ca5267304cfe6a0dab1cfcab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68418089"
---
# <a name="deploy-the-sql-server-resource-provider-on-azure-stack"></a>Azure Stack SQL Server kaynak sağlayıcısını dağıtma

SQL veritabanlarını bir Azure Stack hizmeti olarak göstermek için Azure Stack SQL Server kaynak sağlayıcısını kullanın. SQL kaynak sağlayıcısı, bir Windows Server 2016 Server Core sanal makinesi (VM) üzerinde bir hizmet olarak çalışır.

> [!IMPORTANT]
> Yalnızca kaynak sağlayıcı, SQL veya MySQL 'i barındıran sunucularda öğe oluşturmak için desteklenir. Kaynak sağlayıcı tarafından oluşturulmamış bir konak sunucusunda oluşturulan öğeler, eşleşmeyen bir durumla sonuçlanabilir.

## <a name="prerequisites"></a>Önkoşullar

Azure Stack SQL kaynak sağlayıcısı 'nı dağıtabilmeniz için önce yerine getirmeniz gereken birkaç önkoşul vardır. Bu gereksinimleri karşılamak için, ayrıcalıklı uç nokta VM 'sine erişebilen bir bilgisayarda aşağıdaki adımları uygulayın:

- Daha önce yapmadıysanız Azure Market öğelerini indirebilmeniz için Azure Stack Azure 'a [kaydolun](azure-stack-registration.md) .
- Azure ve Azure Stack PowerShell modüllerini bu yüklemeyi çalıştıracağınız sisteme yüklemeniz gerekir. Bu sistem, .NET çalışma zamanının en son sürümüne sahip bir Windows 10 veya Windows Server 2016 görüntüsü olmalıdır. Bkz. [Azure Stack Için PowerShell 'ı Install](./azure-stack-powershell-install.md).
- **Windows server 2016 Datacenter-Server Core** görüntüsünü indirerek, gerekli Windows Server Core VM 'sini Azure Stack marketi 'ne ekleyin.
- SQL kaynak sağlayıcısı ikilisini indirin ve ardından içeriği geçici bir dizine çıkarmak için kendi kendine ayıklayıcısı 'nı çalıştırın. Kaynak sağlayıcının en düşük karşılık gelen Azure Stack derlemesi vardır.

  |En düşük Azure Stack sürümü|SQL RP sürümü|
  |-----|-----|
  |Sürüm 1808 (1.1808.0.97)|[SQL RP sürüm 1.1.33.0](https://aka.ms/azurestacksqlrp11330)|  
  |Sürüm 1808 (1.1808.0.97)|[SQL RP sürüm 1.1.30.0](https://aka.ms/azurestacksqlrp11300)|  
  |Sürüm 1804 (1.0.180513.1)|[SQL RP sürüm 1.1.24.0](https://aka.ms/azurestacksqlrp11240)  
  |     |     |

- Veri merkezi tümleştirme önkoşulların karşılandığından emin olun:

    |Önkoşul|Başvuru|
    |-----|-----|
    |Koşullu DNS iletme doğru şekilde ayarlanır.|[Azure Stack veri merkezi tümleştirmesi-DNS](azure-stack-integrate-dns.md)|
    |Kaynak sağlayıcıları için gelen bağlantı noktaları açıktır.|[Azure Stack veri merkezi tümleştirmesi-gelen bağlantı noktaları ve protokoller](azure-stack-integrate-endpoints.md#ports-and-protocols-inbound)|
    |PKI sertifikası konusu ve SAN doğru şekilde ayarlanır.|[Azure Stack dağıtım zorunlu PKI önkoşulları](azure-stack-pki-certs.md#mandatory-certificates)<br>[Azure Stack dağıtım PaaS sertifikası önkoşulları](azure-stack-pki-certs.md#optional-paas-certificates)|
    |     |     |

### <a name="certificates"></a>Sertifikalar

_Yalnızca tümleşik sistem yüklemeleri için_. [Azure Stack DAĞıTıM PKI gereksinimleri](./azure-stack-pki-certs.md#optional-paas-certificates)'nin Isteğe bağlı PaaS sertifikaları bölümünde açıklanan SQL PaaS PKI sertifikasını sağlamanız gerekir. . Pfx dosyasını **Dependencyfileslocalpath** parametresi tarafından belirtilen konuma yerleştirin. ASDK sistemleri için bir sertifika sağlamaymayın.

## <a name="deploy-the-sql-resource-provider"></a>SQL kaynak sağlayıcısı 'nı dağıtma

Tüm önkoşulları yükledikten sonra, SQL kaynak sağlayıcısını dağıtmak için **Deploysqlprovider. ps1** betiğini çalıştırabilirsiniz. DeploySqlProvider. ps1 betiği, Azure Stack sürümünüz için indirdiğiniz SQL kaynak sağlayıcısı ikilisinin bir parçası olarak ayıklanır.

 > [!IMPORTANT]
 > Kaynak sağlayıcısını dağıtılmadan önce, yeni işlevsellik, düzeltmeler ve dağıtımınızı etkileyebilecek bilinen sorunlar hakkında bilgi edinmek için sürüm notlarını gözden geçirin.
 
SQL kaynak sağlayıcısı 'nı dağıtmak için **Yeni** bir yükseltilmiş PowerShell penceresi açın (PowerShell ISE) ve SQL kaynak sağlayıcısı ikili dosyalarını ayıkladığınız dizine geçin. Zaten yüklü olan PowerShell modüllerinin neden olduğu olası sorunları önlemek için yeni bir PowerShell penceresi kullanmanızı öneririz.

Aşağıdaki görevleri tamamlayan DeploySqlProvider. ps1 betiğini çalıştırın:

- Sertifikaları ve diğer yapıtları Azure Stack bir depolama hesabına yükler.
- Galeri kullanarak SQL veritabanlarını dağıtabilmeniz için Galeri paketleri yayımlar.
- Barındırma sunucularını dağıtmak için bir galeri paketi yayımlar.
- İndirdiğiniz Windows Server 2016 çekirdek görüntüsünü kullanarak bir VM dağıtır ve ardından SQL kaynak sağlayıcısını kurar.
- Kaynak sağlayıcısı VM 'niz ile eşleşen yerel bir DNS kaydını kaydeder.
- Kaynak sağlayıcınızı operatör hesabı için yerel Azure Resource Manager kaydeder.

> [!NOTE]
> SQL kaynak sağlayıcısı dağıtımı başladığında, **System. Local. sqladapter** kaynak grubu oluşturulur. Bu kaynak grubuna yönelik gerekli dağıtımların tamamlanması 75 dakika sürebilir.

### <a name="deploysqlproviderps1-parameters"></a>DeploySqlProvider. ps1 parametreleri

Komut satırından aşağıdaki parametreleri belirtebilirsiniz. Aksi takdirde veya herhangi bir parametre doğrulaması başarısız olursa, gerekli parametreleri sağlamanız istenir.

| Parametre adı | Açıklama | Açıklama veya varsayılan değer |
| --- | --- | --- |
| **CloudAdminCredential** | Ayrıcalıklı uç noktaya erişmek için gerekli olan bulut yöneticisinin kimlik bilgileri. | _Gerekli_ |
| **AzCredential** | Azure Stack hizmeti yönetici hesabı için kimlik bilgileri. Azure Stack dağıtmak için kullandığınız kimlik bilgilerini kullanın. | _Gerekli_ |
| **VMLocalCredential** | SQL kaynak sağlayıcısı VM 'sinin yerel yönetici hesabının kimlik bilgileri. | _Gerekli_ |
| **PrivilegedEndpoint** | Ayrıcalıklı uç noktanın IP adresi veya DNS adı. |  _Gerekli_ |
| **AzureEnvironment** | Azure Stack dağıtmak için kullanılan hizmet yönetici hesabının Azure ortamı. Yalnızca Azure AD dağıtımları için gereklidir. Desteklenen ortam adları **Azurecsesli**, **AzureUSGovernment**veya Çin Azure Active Directory, **AzureChinaCloud**kullanıyor. | AzureCloud |
| **DependencyFilesLocalPath** | Yalnızca tümleşik sistemler için Certificate. pfx dosyanızın bu dizine yerleştirilmesi gerekir. İsteğe bağlı olarak, bir Windows Update MSU paketini buraya kopyalayabilirsiniz. | _Isteğe bağlı_ (tümleşik sistemler için_zorunlu_ ) |
| **DefaultSSLCertificatePassword** | . Pfx sertifikası için parola. | _Gerekli_ |
| **MaxRetryCount** | Bir hata oluşursa her işlemi yeniden denemek istediğiniz zaman sayısı.| 2 |
| **RetryDuration** | Yeniden denemeler arasındaki zaman aşımı aralığı (saniye cinsinden). | 120 |
| **Kaldırma** | Kaynak sağlayıcıyı ve tüm ilişkili kaynakları kaldırır (aşağıdaki notlara bakın). | Hayır |
| **DebugMode** | Hata durumunda otomatik temizlemeyi önler. | Hayır |

## <a name="deploy-the-sql-resource-provider-using-a-custom-script"></a>Özel bir komut dosyası kullanarak SQL kaynak sağlayıcısı dağıtma

Kaynak sağlayıcıyı dağıttığınızda el ile yapılandırmayı ortadan kaldırmak için aşağıdaki betiği özelleştirebilirsiniz.  

Azure Stack dağıtımınız için gereken varsayılan hesap bilgilerini ve parolaları değiştirin.


```powershell
# Install the AzureRM.Bootstrapper module, set the profile and install the AzureStack module
# Note that this might not be the most currently available version of Azure Stack PowerShell
Install-Module -Name AzureRm.BootStrapper -RequiredVersion 0.5.0 -Force
Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force
Install-Module -Name AzureStack -RequiredVersion 1.6.0

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time.
$domain = "AzureStack"

# For integrated systems, use the IP address of one of the ERCS virtual machines
$privilegedEndpoint = "AzS-ERCS01"

# Provide the Azure environment used for deploying Azure Stack. Required only for Azure AD deployments. Supported values for the <environment name> parameter are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using. 
$AzureEnvironment = "<EnvironmentName>"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account can be Azure Active Directory or Active Directory Federation Services.
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new resource provider VM local administrator account.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# Add the cloudadmin credential that's required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Clear the existing login information from the Azure PowerShell context.
Clear-AzureRMContext -Scope CurrentUser -Force
Clear-AzureRMContext -Scope Process -Force

# Change to the directory folder where you extracted the installation files. Do not provide a certificate on ASDK!
. $tempDir\DeploySQLProvider.ps1 `
    -AzCredential $AdminCreds `
    -VMLocalCredential $vmLocalAdminCreds `
    -CloudAdminCredential $cloudAdminCreds `
    -PrivilegedEndpoint $privilegedEndpoint `
    -AzureEnvironment $AzureEnvironment `
    -DefaultSSLCertificatePassword $PfxPass `
    -DependencyFilesLocalPath $tempDir\cert

 ```

Kaynak sağlayıcısı yükleme betiği tamamlandığında, en son güncelleştirmeleri görediğinizden emin olmak için tarayıcınızı yenileyin.

## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Azure Stack portalını kullanarak dağıtımı doğrulama

SQL kaynak sağlayıcısı 'nın başarıyla dağıtıldığını doğrulamak için aşağıdaki adımları kullanabilirsiniz.

1. Yönetim portalında hizmet yöneticisi olarak oturum açın.
2. **Kaynak grupları**' nı seçin.
3. Sistemi seçin **.\< Location\>. sqladapter** kaynak grubu.
4. Kaynak grubuna genel bakış Özet sayfasında, başarısız dağıtımlar olmaması gerekir.
      ![SQL kaynak sağlayıcısı 'nın dağıtımını doğrulama](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)
5. Son olarak, SQL kaynak sağlayıcısı VM 'sinin başarıyla oluşturulduğunu ve çalıştığını doğrulamak için yönetim portalındaki **sanal makineler** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Barındırma sunucuları ekleme](azure-stack-sql-resource-provider-hosting-servers.md)
