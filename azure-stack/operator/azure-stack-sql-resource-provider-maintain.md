---
title: Azure Stack SQL kaynak sağlayıcısı 'nı koruma | Microsoft Docs
description: Azure Stack üzerinde SQL kaynak sağlayıcısı hizmetini nasıl koruyabileceğinizi öğrenin.
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
ms.openlocfilehash: bf5bd23fc9d497034dfb51c76f28e5b17fbd8e33
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829300"
---
# <a name="sql-resource-provider-maintenance-operations"></a>SQL kaynak sağlayıcısı bakım işlemleri

SQL kaynak sağlayıcısı, kilitli bir sanal makinede çalışır. Bakım işlemlerini etkinleştirmek için, sanal makinenin güvenliğini güncelleştirmeniz gerekir. Bunu, en az ayrıcalık sorumlusunu kullanarak yapmak için [PowerShell 'In yeterli yönetim (JEA)](https://docs.microsoft.com/powershell/scripting/learn/remoting/jea/overview) uç noktası *Dbadaptermaintenance*komutunu kullanabilirsiniz. Kaynak sağlayıcısı yükleme paketi bu işlem için bir komut dosyası içerir.

## <a name="patching-and-updating"></a>Düzeltme eki uygulama ve güncelleştirme

SQL kaynak sağlayıcısı, bir eklenti bileşeni olduğundan Azure Stack bir parçası olarak hizmet vermez. Microsoft, gerektiğinde SQL kaynak sağlayıcısına güncelleştirmeler sağlar. Güncelleştirilmiş bir SQL bağdaştırıcısı yayınlandığında, güncelleştirmeyi uygulamak için bir komut dosyası sağlanır. Bu betik, eski sağlayıcı VM 'sinin durumunu yeni VM 'ye geçirerek yeni bir kaynak sağlayıcısı sanal makinesi oluşturur. Daha fazla bilgi için bkz. [SQL kaynak sağlayıcısını güncelleştirme](azure-stack-sql-resource-provider-update.md).

### <a name="provider-virtual-machine"></a>Sağlayıcı sanal makinesi

Kaynak sağlayıcı bir *Kullanıcı* sanal makinesinde çalıştığından, yayımlandıklarında gerekli düzeltme eklerini ve güncelleştirmeleri uygulamanız gerekir. Güncelleştirmeleri VM 'ye uygulamak için düzeltme eki ve güncelleştirme döngüsünün bir parçası olarak sunulan Windows Update paketlerini kullanabilirsiniz.

## <a name="updating-sql-credentials"></a>SQL kimlik bilgileri güncelleştiriliyor

SQL sunucularınızda sysadmin hesaplarını oluşturmaktan ve korumadan sorumlu olursunuz. Kaynak sağlayıcısı, kullanıcıların veritabanlarına yönelik veritabanlarını yönetmek için bu ayrıcalıklara sahip bir hesap gerektirir, ancak kullanıcıların verilerine erişmesi gerekmez. SQL sunucularınızda sysadmin parolalarını güncelleştirmeniz gerekiyorsa, depolanan bir parolayı değiştirmek için kaynak sağlayıcının yönetici arabirimini kullanabilirsiniz. Bu parolalar Azure Stack örneğiniz üzerinde Key Vault depolanır.

Ayarları değiştirmek için, **Yönetim kaynakları** &gt; **SQL barındırma sunucuları** &gt; **SQL oturum açmaları** ' **nı seçin** &gt; ve bir Kullanıcı adı seçin. Değişiklik, önce SQL örneğinde (ve gerekirse tüm çoğaltmalarda) yapılmalıdır. **Ayarlar**altında, **parola**' yı seçin.

![Yönetici parolasını güncelleştirme](./media/azure-stack-sql-rp-deploy/sqlrp-update-password.PNG)

## <a name="secrets-rotation"></a>Gizli dizileri döndürme

*Bu yönergeler yalnızca Azure Stack tümleşik sistemler için geçerlidir.*

Azure Stack tümleşik sistemlerle SQL ve MySQL kaynak sağlayıcılarını kullanırken, Azure Stack operatörü, süreleri dolmadan emin olmak için aşağıdaki kaynak sağlayıcısı altyapı gizli dizilerini döndürmekten sorumludur:

- [Dağıtım sırasında](azure-stack-pki-certs.md)dış SSL sertifikası belirtildi.
- Dağıtım sırasında sağlanmış kaynak sağlayıcısı VM yerel yönetici hesabı parolası.
- Kaynak sağlayıcısı Tanılama Kullanıcı (dbadapterdiag) parolası.

### <a name="powershell-examples-for-rotating-secrets"></a>Gizli dizileri döndürme için PowerShell örnekleri

**Tüm sırları aynı anda değiştirin.**

```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -DiagnosticsUserPassword $passwd `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd  `
    -VMLocalCredential $localCreds
```

**Tanılama Kullanıcı parolasını değiştirin.**

```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -DiagnosticsUserPassword  $passwd
```

**VM yerel yönetici hesabı parolasını değiştirin.**

```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -VMLocalCredential $localCreds
```

**SSL sertifikası parolasını değiştirin.**

```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd
```

### <a name="secretrotationsqlproviderps1-parameters"></a>SecretRotationSQLProvider. ps1 parametreleri

|Parametre|Açıklama|
|-----|-----|
|AzCredential|Azure Stack hizmeti yönetici hesabı kimlik bilgileri.|
|CloudAdminCredential|Azure Stack bulut Yöneticisi etki alanı hesabı kimlik bilgileri.|
|Ayrıcalıklı Gedendpoint|Get-Azurestackstampınformation öğesine erişmek için ayrıcalıklı uç nokta.|
|DiagnosticsUserPassword|Tanılama Kullanıcı hesabı parolası.|
|VMLocalCredential|MySQLAdapter VM 'de yerel yönetici hesabı.|
|DefaultSSLCertificatePassword|Varsayılan SSL sertifikası (* PFX) parolası.|
|DependencyFilesLocalPath|Bağımlılık dosyaları yerel yolu.|
|     |     |

### <a name="known-issues"></a>Bilinen sorunlar

**Sorun**: Gizli dizi döndürme günlükleri.<br>
Gizli dizi dönüşü özel betik çalıştırıldığında başarısız olursa gizli dizi için Günlükler otomatik olarak toplanmaz.

**Geçici çözüm**:<br>
AzureStack. DatabaseAdapter. SecretRotation. ps1 _*. log dahil tüm kaynak sağlayıcısı günlüklerini toplamak için Get-AzsDBAdapterLogs cmdlet 'ini kullanın, C:\Logs. dizinine kaydedilir.

## <a name="update-the-virtual-machine-operating-system"></a>Sanal makine işletim sistemini Güncelleştir

Sanal makine işletim sistemini güncelleştirmek için aşağıdaki yöntemlerden birini kullanın.

- En son düzeltme eki uygulanmış Windows Server 2016 çekirdek görüntüsünü kullanarak en son kaynak sağlayıcısı paketini yükler.
- Kaynak sağlayıcısına yükleme veya güncelleştirme sırasında Windows Update paketini yükleme.

## <a name="update-the-virtual-machine-windows-defender-definitions"></a>Sanal makineyi güncelleştirme Windows Defender tanımları

Windows Defender tanımlarını güncelleştirmek için:

1. Windows Defender tanımları güncelleştirmesini [Windows Defender tanımından](https://www.microsoft.com/en-us/wdsi/definitions)indirin.

   Tanımlar güncelleştirme sayfasında, aşağı kaydırarak "tanımları El Ile indir ve Yükle" seçeneğine gidin. "Windows 10 için Windows Defender virüsten koruma ve Windows 8.1" 64-bit dosyasını indirin.

   Alternatif olarak, fpam-Fe. exe dosyasını indirmek/çalıştırmak için [Bu doğrudan bağlantıyı](https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64) kullanın.

2. SQL kaynak sağlayıcısı bağdaştırıcısı sanal makinesinin bakım uç noktasında bir PowerShell oturumu oluşturun.

3. Tanımlamalar güncelleştirme dosyasını bakım uç noktası oturumunu kullanarak sanal makineye kopyalayın.

4. Bakım PowerShell oturumunda *Update-Dbadapterwindowssavunma Derdefinitions* komutunu çalıştırın.

5. Tanımları yükledikten sonra, *Remove-ItemOnUserDrive* komutunu kullanarak tanımları güncelleştirme dosyasını silmenizi öneririz.

**Tanımları güncelleştirmek için PowerShell betiği örneği.**

Defender tanımlarını güncelleştirmek için aşağıdaki betiği düzenleyip çalıştırabilirsiniz. Betikteki değerleri ortamınızdaki değerlerle değiştirin.

```powershell
# Set credentials for local admin on the resource provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "<local admin user password>" -AsPlainText -Force
$vmLocalAdminUser = "<local admin user name>"
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential `
    ($vmLocalAdminUser, $vmLocalAdminPass)

# Provide the public IP address for the adapter VM.
$databaseRPMachine  = "<RP VM IP address>"
$localPathToDefenderUpdate = "C:\DefenderUpdates\mpam-fe.exe"

# Download the Windows Defender update definitions file from https://www.microsoft.com/en-us/wdsi/definitions.
Invoke-WebRequest -Uri 'https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64' `
    -Outfile $localPathToDefenderUpdate

# Create a session to the maintenance endpoint.
$session = New-PSSession -ComputerName $databaseRPMachine `
    -Credential $vmLocalAdminCreds -ConfigurationName DBAdapterMaintenance
# Copy the defender update file to the adapter virtual machine.
Copy-Item -ToSession $session -Path $localPathToDefenderUpdate `
     -Destination "User:\"
# Install the update definitions.
Invoke-Command -Session $session -ScriptBlock `
    {Update-AzSDBAdapterWindowsDefenderDefinition -DefinitionsUpdatePackageFile "User:\mpam-fe.exe"}
# Cleanup the definitions package file and session.
Invoke-Command -Session $session -ScriptBlock `
    {Remove-AzSItemOnUserDrive -ItemPath "User:\mpam-fe.exe"}
$session | Remove-PSSession
```

## <a name="collect-diagnostic-logs"></a>Tanılama günlüklerini topla

Kilitli sanal makineden günlükleri toplamak için PowerShell 'in yeterli yönetim (JEA) uç noktası *Dbadapterdiagnostics*'i kullanabilirsiniz. Bu uç nokta aşağıdaki komutları sağlar:

- **Get-AzsDBAdapterLog**. Bu komut, kaynak sağlayıcısı tanılama günlüklerinin bir ZIP paketini oluşturur ve dosyayı oturumun Kullanıcı sürücüsüne kaydeder. Bu komutu herhangi bir parametre olmadan çalıştırabilirsiniz ve son dört saatlik günlük toplanmaktadır.
- **Remove-AzsDBAdapterLog**. Bu komut, kaynak sağlayıcısı VM 'sinde var olan günlük paketlerini kaldırır.

### <a name="endpoint-requirements-and-process"></a>Uç nokta gereksinimleri ve süreci

Bir kaynak sağlayıcısı yüklendiğinde veya güncelleştirilirken **dbadapterdiag** Kullanıcı hesabı oluşturulur. Bu hesabı tanılama günlüklerini toplamak için kullanacaksınız.

>[!NOTE]
>Dbadapterdiag hesabı parolası, bir sağlayıcı dağıtımı veya güncelleştirmesi sırasında oluşturulan sanal makinede yerel yönetici için kullanılan parolayla aynıdır.

*Dbadapterdiagnostics* komutlarını kullanmak için, kaynak sağlayıcısı sanal makinesine bir uzak PowerShell oturumu oluşturun ve **Get-AzsDBAdapterLog** komutunu çalıştırın.

**FromDate** ve **ToDate** parametrelerini kullanarak günlük koleksiyonu için zaman aralığı ayarlarsınız. Bu parametrelerin birini veya ikisini birden belirtmezseniz, aşağıdaki varsayılanlar kullanılır:

- FromDate, geçerli zamandan dört saat öncesine ait.
- ToDate geçerli zaman.

**Günlükleri toplamak için PowerShell betiği örneği.**

Aşağıdaki betik, kaynak sağlayıcısı VM 'sinden tanılama günlüklerinin nasıl toplanacağını göstermektedir.

```powershell
# Create a new diagnostics endpoint session.
$databaseRPMachineIP = '<RP VM IP address>'
$diagnosticsUserName = 'dbadapterdiag'
$diagnosticsUserPassword = '<Enter Diagnostic password>'

$diagCreds = New-Object System.Management.Automation.PSCredential `
        ($diagnosticsUserName, (ConvertTo-SecureString -String $diagnosticsUserPassword -AsPlainText -Force))
$session = New-PSSession -ComputerName $databaseRPMachineIP -Credential $diagCreds `
        -ConfigurationName DBAdapterDiagnostics

# Sample that captures logs from the previous hour.
$fromDate = (Get-Date).AddHours(-1)
$dateNow = Get-Date
$sb = {param($d1,$d2) Get-AzSDBAdapterLog -FromDate $d1 -ToDate $d2}
$logs = Invoke-Command -Session $session -ScriptBlock $sb -ArgumentList $fromDate,$dateNow

# Copy the logs to the user drive.
$sourcePath = "User:\{0}" -f $logs
$destinationPackage = Join-Path -Path (Convert-Path '.') -ChildPath $logs
Copy-Item -FromSession $session -Path $sourcePath -Destination $destinationPackage

# Clean up the logs.
$cleanup = Invoke-Command -Session $session -ScriptBlock {Remove-AzsDBAdapterLog}
# Close the session.
$session | Remove-PSSession
```

## <a name="next-steps"></a>Sonraki adımlar

[SQL Server barındırma sunucuları ekleme](azure-stack-sql-resource-provider-hosting-servers.md)
