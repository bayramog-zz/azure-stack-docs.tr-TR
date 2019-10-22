---
title: Azure Stack MySQL kaynak sağlayıcısını dağıt | Microsoft Docs
description: MySQL kaynak sağlayıcısı bağdaştırıcısını ve MySQL veritabanlarını Azure Stack bir hizmet olarak dağıtmayı öğrenin.
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
ms.lastreviewed: 03/18/2019
ms.openlocfilehash: c3b3c30eb10e767cf20336af67bd094994def2f9
ms.sourcegitcommit: a23b80b57668615c341c370b70d0a106a37a02da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72682108"
---
# <a name="deploy-the-mysql-resource-provider-on-azure-stack"></a>MySQL kaynak sağlayıcısını Azure Stack dağıtma

MySQL veritabanlarını Azure Stack hizmeti olarak ortaya koymak için MySQL Server kaynak sağlayıcısını kullanın. MySQL kaynak sağlayıcısı, bir Windows Server 2016 Server Core sanal makinesi (VM) üzerinde bir hizmet olarak çalışır.

> [!IMPORTANT]
> Yalnızca kaynak sağlayıcı, SQL veya MySQL 'i barındıran sunucularda öğe oluşturmak için desteklenir. Kaynak sağlayıcısı tarafından oluşturulmamış bir ana sunucu üzerinde oluşturulan öğeler, eşleşmeyen bir duruma neden olabilirler.

## <a name="prerequisites"></a>Önkoşullar

Azure Stack MySQL kaynak sağlayıcısı 'nı dağıtabilmeniz için önce yerine getirmeniz gereken birkaç önkoşul vardır. Bu gereksinimleri karşılamak için, ayrıcalıklı uç nokta VM 'sine erişebilen bir bilgisayardaki bu makaledeki adımları izleyin.

* Henüz yapmadıysanız Azure Market öğelerini indirmek için Azure Stack Azure 'a [kaydolun](./azure-stack-registration.md) .
* Azure ve Azure Stack PowerShell modüllerini bu yüklemeyi çalıştıracağınız sisteme yükler. Bu sistem, .NET çalışma zamanının en son sürümüne sahip bir Windows 10 veya Windows Server 2016 görüntüsü olmalıdır. Bkz. [Azure Stack Için PowerShell 'ı Install](./azure-stack-powershell-install.md).
* **Windows server 2016 Datacenter-Server Core** görüntüsünü indirerek, gerekli Windows Server Core VM 'Sini Azure Stack marketi 'ne ekleyin.

* MySQL kaynak sağlayıcısı ikilisini indirin ve ardından içeriği geçici bir dizine çıkarmak için Self-Extractor ' i çalıştırın.

  >[!NOTE]
  >MySQL sağlayıcısını Internet erişimi olmayan bir sisteme dağıtmak için, [MySQL-Connector-Net-6.10.5. msi](https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-6.10.5.msi) dosyasını bir yerel yola kopyalayın. **Dependencyfileslocalpath** parametresini kullanarak yol adını sağlayın.

* Kaynak sağlayıcının en düşük karşılık gelen Azure Stack derlemesi vardır.

  |En düşük Azure Stack sürümü|MySQL RP sürümü|
  |-----|-----|
  |Sürüm 1808 (1.1808.0.97)|[MySQL RP sürüm 1.1.33.0](https://aka.ms/azurestackmysqlrp11330)|  
  |Sürüm 1808 (1.1808.0.97)|[MySQL RP sürüm 1.1.30.0](https://aka.ms/azurestackmysqlrp11300)|
  |Sürüm 1804 (1.0.180513.1)|[MySQL RP sürüm 1.1.24.0](https://aka.ms/azurestackmysqlrp11240)
  |     |     |

* Veri merkezi tümleştirme önkoşulların karşılandığından emin olun:

    |Önkoşul|Başvuru|
    |-----|-----|
    |Koşullu DNS iletme doğru şekilde ayarlanır.|[Azure Stack veri merkezi tümleştirmesi-DNS](azure-stack-integrate-dns.md)|
    |Kaynak sağlayıcıları için gelen bağlantı noktaları açıktır.|[Azure Stack veri merkezi tümleştirmesi-uç noktaları yayımlama](azure-stack-integrate-endpoints.md#ports-and-protocols-inbound)|
    |PKI sertifikası konusu ve SAN doğru şekilde ayarlanır.|Dağıtım[PaaS sertifikası önkoşulları Azure Stack](azure-stack-pki-certs.md#optional-paas-certificates) DAĞıTıM [zorunlu PKI önkoşulları Azure Stack](azure-stack-pki-certs.md#mandatory-certificates)|
    |     |     |

### <a name="certificates"></a>Sertifikalar

_Yalnızca tümleşik sistem yüklemeleri için_. [Azure Stack DAĞıTıM PKI gereksinimleri](./azure-stack-pki-certs.md#optional-paas-certificates)'nin Isteğe bağlı PaaS sertifikaları bölümünde açıklanan SQL PaaS PKI sertifikasını sağlamanız gerekir. . Pfx dosyasını **Dependencyfileslocalpath** parametresi tarafından belirtilen konuma yerleştirin. ASDK sistemleri için bir sertifika sağlamaymayın.

## <a name="deploy-the-resource-provider"></a>Kaynak sağlayıcısını dağıtma

Tüm önkoşulları yükledikten sonra, MySQL kaynak sağlayıcısını dağıtmak için **Deploymysqlprovider. ps1** betiğini çalıştırabilirsiniz. DeployMySqlProvider. ps1 betiği, Azure Stack sürümünüz için indirdiğiniz MySQL kaynak sağlayıcısı yükleme dosyalarının bir parçası olarak ayıklanır.

 > [!IMPORTANT]
 > Kaynak sağlayıcısını dağıtılmadan önce, yeni işlevsellik, düzeltmeler ve dağıtımınızı etkileyebilecek bilinen sorunlar hakkında bilgi edinmek için sürüm notlarını gözden geçirin.

MySQL kaynak sağlayıcısı 'nı dağıtmak için yeni bir yükseltilmiş PowerShell penceresi açın (PowerShell ıSE) ve MySQL kaynak sağlayıcısı ikili dosyalarını ayıkladığınız dizine geçin. Zaten yüklü olan PowerShell modüllerinin neden olduğu olası sorunları önlemek için yeni bir PowerShell penceresi kullanmanızı öneririz.

Aşağıdaki görevleri tamamlayan **Deploymysqlprovider. ps1** betiğini çalıştırın:

* Sertifikaları ve diğer yapıtları Azure Stack bir depolama hesabına yükler.
* Galeriyi kullanarak MySQL veritabanlarını dağıtabilmeniz için Galeri paketleri yayımlar.
* Barındırma sunucularını dağıtmak için bir galeri paketi yayımlar.
* İndirdiğiniz Windows Server 2016 çekirdek görüntüsünü kullanarak bir VM dağıtır ve ardından MySQL kaynak sağlayıcısını kurar.
* Kaynak sağlayıcısı VM 'niz ile eşleşen yerel bir DNS kaydını kaydeder.
* Kaynak sağlayıcınızı operatör hesabı için yerel Azure Resource Manager kaydeder.

> [!NOTE]
> MySQL kaynak sağlayıcısı dağıtımı başladığında, **System. Local. mysqladapter** kaynak grubu oluşturulur. Bu kaynak grubuna gereken dağıtımların tamamlanması 75 dakika sürebilir.

### <a name="deploymysqlproviderps1-parameters"></a>DeployMySqlProvider. ps1 parametreleri

Komut satırından bu parametreleri belirtebilirsiniz. Aksi takdirde veya herhangi bir parametre doğrulaması başarısız olursa, gerekli parametreleri sağlamanız istenir.

| Parametre adı | Açıklama | Açıklama veya varsayılan değer |
| --- | --- | --- |
| **CloudAdminCredential** | Ayrıcalıklı uç noktaya erişmek için gerekli olan bulut yöneticisinin kimlik bilgileri. | _Gerekli_ |
| **AzCredential** | Azure Stack hizmeti yönetici hesabı için kimlik bilgileri. Azure Stack dağıtmak için kullandığınız kimlik bilgilerini kullanın. | _Gerekli_ |
| **VMLocalCredential** | MySQL kaynak sağlayıcısı VM 'sinin yerel yönetici hesabının kimlik bilgileri. | _Gerekli_ |
| **Ayrıcalıklı Gedendpoint** | Ayrıcalıklı uç noktanın IP adresi veya DNS adı. |  _Gerekli_ |
| **AzureEnvironment** | Azure Stack dağıtmak için kullanılan hizmet yönetici hesabının Azure ortamı. Yalnızca Azure AD dağıtımları için gereklidir. Desteklenen ortam adları **Azurecsesli**, **AzureUSGovernment**veya Çin Azure AD, **AzureChinaCloud**kullanıyorsa. | AzureCloud |
| **DependencyFilesLocalPath** | Yalnızca tümleşik sistemler için Certificate. pfx dosyanızın bu dizine yerleştirilmesi gerekir. Bağlantısı kesilen ortamlar için, bu dizine [MySQL-Connector-Net-6.10.5. msi](https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-6.10.5.msi) dosyasını indirin. İsteğe bağlı olarak, bir Windows Update MSU paketini buraya kopyalayabilirsiniz. | _Isteğe bağlı_ (tümleşik sistemler veya bağlantısı kesilen ortamlar için_zorunlu_ ) |
| **DefaultSSLCertificatePassword** | . Pfx sertifikası için parola. | _Gerekli_ |
| **MaxRetryCount** | Bir hata oluşursa her işlemi yeniden denemek istediğiniz zaman sayısı.| 2 |
| **RetryDuration** | Yeniden denemeler arasındaki zaman aşımı aralığı (saniye cinsinden). | 120 |
| **Kaldırma** | Kaynak sağlayıcıyı ve tüm ilişkili kaynakları kaldırır (aşağıdaki notlara bakın). | Hayır |
| **DebugMode** | Hata durumunda otomatik temizlemeyi önler. | Hayır |
| **AcceptLicense** | GPL lisansını kabul etmek için isteği atlar.  <https://www.gnu.org/licenses/old-licenses/gpl-2.0.html> | |

## <a name="deploy-the-mysql-resource-provider-using-a-custom-script"></a>Özel bir betik kullanarak MySQL kaynak sağlayıcısını dağıtma

Kaynak sağlayıcıyı dağıttığınızda el ile yapılandırmayı ortadan kaldırmak için aşağıdaki betiği özelleştirebilirsiniz. Azure Stack dağıtımınız için gereken varsayılan hesap bilgilerini ve parolaları değiştirin.

```powershell
# Install the AzureRM.Bootstrapper module, set the profile and install the AzureStack module
# Note that this might not be the most currently available version of Azure Stack PowerShell
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

# Set the credentials for the new resource provider VM local admin account
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("mysqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Clear the existing login information from the Azure PowerShell context.
Clear-AzureRMContext -Scope CurrentUser -Force
Clear-AzureRMContext -Scope Process -Force

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change to the directory folder where you extracted the installation files. Don't provide a certificate on ASDK!
. $tempDir\DeployMySQLProvider.ps1 `
    -AzCredential $AdminCreds `
    -VMLocalCredential $vmLocalAdminCreds `
    -CloudAdminCredential $cloudAdminCreds `
    -PrivilegedEndpoint $privilegedEndpoint `
    -AzureEnvironment $AzureEnvironment `
    -DefaultSSLCertificatePassword $PfxPass `
    -DependencyFilesLocalPath $tempDir\cert `
    -AcceptLicense

```

Kaynak sağlayıcısı yükleme betiği tamamlandığında, en son güncelleştirmeleri görediğinizden emin olmak için tarayıcınızı yenileyin.

## <a name="verify-the-deployment-by-using-the-azure-stack-portal"></a>Azure Stack portalını kullanarak dağıtımı doğrulama

1. Hizmet Yöneticisi olarak yönetici portalında oturum açın.
2. **Kaynak grupları**' nı seçin.
3. **System. \<location \>. mysqladapter** kaynak grubunu seçin.
4. Kaynak grubuna genel bakış Özet sayfasında, başarısız dağıtımlar olmaması gerekir.
5. Son olarak, MySQL kaynak sağlayıcısı VM 'sinin başarıyla oluşturulduğunu ve çalıştığını doğrulamak için yönetici portalında **sanal makineler** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Barındırma sunucuları ekleme](azure-stack-mysql-resource-provider-hosting-servers.md)
