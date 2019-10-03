---
title: Azure Stack MySQL kaynak sağlayıcısı 'nı koruma | Microsoft Docs
description: Azure Stack MySQL kaynak sağlayıcısı hizmetini nasıl koruyabileceğinizi öğrenin.
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
ms.openlocfilehash: 6667fd3db21cd6138e756c16eb8e68b8ecd1b3e9
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829423"
---
# <a name="mysql-resource-provider-maintenance-operations"></a>MySQL kaynak sağlayıcısı bakım işlemleri

MySQL kaynak sağlayıcısı, kilitli bir sanal makinede çalışır. Bakım işlemlerini etkinleştirmek için, sanal makinenin güvenliğini güncelleştirmeniz gerekir. Bunu, en az ayrıcalık ilkesini kullanarak yapmak için PowerShell 'in yeterli yönetim (JEA) uç noktası DBAdapterMaintenance komutunu kullanabilirsiniz. Kaynak sağlayıcısı yükleme paketi bu işlem için bir komut dosyası içerir.

## <a name="update-the-virtual-machine-operating-system"></a>Sanal makine işletim sistemini Güncelleştir

Kaynak sağlayıcı bir *Kullanıcı* sanal makinesinde çalıştığından, yayımlandıklarında gerekli düzeltme eklerini ve güncelleştirmeleri uygulamanız gerekir. Güncelleştirmeleri VM 'ye uygulamak için düzeltme eki ve güncelleştirme döngüsünün bir parçası olarak sunulan Windows Update paketlerini kullanabilirsiniz.

Aşağıdaki yöntemlerden birini kullanarak sağlayıcı sanal makinesini güncelleştirin:

- En son düzeltme eki uygulanmış Windows Server 2016 çekirdek görüntüsünü kullanarak en son kaynak sağlayıcısı paketini yükler.
- Yüklemesi sırasında bir Windows Update paketi yükleme veya kaynak sağlayıcısına güncelleştirme.

## <a name="update-the-virtual-machine-windows-defender-definitions"></a>Sanal makineyi güncelleştirme Windows Defender tanımları

Defender tanımlarını güncelleştirmek için şu adımları izleyin:

1. Windows Defender tanımları güncelleştirmesini [Windows Defender tanımından](https://www.microsoft.com/en-us/wdsi/definitions)indirin.

    Tanımlar sayfasında, aşağı kaydırarak "tanımları El Ile indir ve Yükle" seçeneğine gidin. "Windows 10 için Windows Defender virüsten koruma ve Windows 8.1" 64-bit dosyasını indirin.

    Alternatif olarak, fpam-Fe. exe dosyasını indirmek/çalıştırmak için [Bu doğrudan bağlantıyı](https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64) kullanın.

2. MySQL kaynak sağlayıcısı bağdaştırıcısı sanal makinesinin bakım uç noktasında bir PowerShell oturumu açın.

3. Tanımlar güncelleştirme dosyasını bakım uç noktası oturumunu kullanarak kaynak sağlayıcısı bağdaştırıcısı VM 'sine kopyalayın.

4. Bakım PowerShell oturumunda _Update-Dbadapterwindowssavunma Derdefinitions_ komutunu çalıştırın.

5. Tanımları yükledikten sonra, tanımları güncelleştirme dosyasını _Remove-ItemOnUserDrive)_ komutunu kullanarak silmenizi öneririz.

**Tanımları güncelleştirmek için PowerShell betiği örneği.**

Defender tanımlarını güncelleştirmek için aşağıdaki betiği düzenleyip çalıştırabilirsiniz. Betikteki değerleri ortamınızdaki değerlerle değiştirin.

```powershell
# Set credentials for the local admin on the resource provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "<local admin user password>" -AsPlainText -Force
$vmLocalAdminUser = "<local admin user name>"
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential `
    ($vmLocalAdminUser, $vmLocalAdminPass)

# Provide the public IP address for the adapter VM.
$databaseRPMachine  = "<RP VM IP address>"
$localPathToDefenderUpdate = "C:\DefenderUpdates\mpam-fe.exe"

# Download Windows Defender update definitions file from https://www.microsoft.com/en-us/wdsi/definitions.  
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

## <a name="secrets-rotation"></a>Gizli dizileri döndürme

*Bu yönergeler yalnızca Azure Stack tümleşik sistemler için geçerlidir.*

Azure Stack tümleşik sistemlerle SQL ve MySQL kaynak sağlayıcılarını kullanırken, Azure Stack operatörü, süreleri dolmadan emin olmak için aşağıdaki kaynak sağlayıcısı altyapı gizli dizilerini döndürmekten sorumludur:

- [Dağıtım sırasında](azure-stack-pki-certs.md)dış SSL sertifikası belirtildi.
- Dağıtım sırasında sağlanmış kaynak sağlayıcısı VM yerel yönetici hesabı parolası.
- Kaynak sağlayıcısı Tanılama Kullanıcı (dbadapterdiag) parolası.

### <a name="powershell-examples-for-rotating-secrets"></a>Gizli dizileri döndürme için PowerShell örnekleri

**Tüm sırları aynı anda değiştirin.**

```powershell
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -DiagnosticsUserPassword $passwd `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd `  
    -VMLocalCredential $localCreds

```

**Tanılama Kullanıcı parolasını değiştirin.**

```powershell
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -DiagnosticsUserPassword  $passwd

```

**VM yerel yönetici hesabı parolasını değiştirin.**

```powershell
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -VMLocalCredential $localCreds

```

**SSL sertifikası parolasını değiştirin.**

```powershell
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd

```

### <a name="secretrotationmysqlproviderps1-parameters"></a>SecretRotationMySQLProvider. ps1 parametreleri

|Parametre|Açıklama|
|-----|-----|
|AzCredential|Azure Stack hizmeti yönetici hesabı kimlik bilgileri.|
|CloudAdminCredential|Azure Stack bulut Yöneticisi etki alanı hesabı kimlik bilgileri.|
|Ayrıcalıklı Gedendpoint|Get-Azurestackstampınformation öğesine erişmek için ayrıcalıklı uç nokta.|
|DiagnosticsUserPassword|Tanılama Kullanıcı hesabı parolası.|
|VMLocalCredential|MySQLAdapter VM 'deki yerel yönetici hesabı.|
|DefaultSSLCertificatePassword|Varsayılan SSL sertifikası (* PFX) parolası.|
|DependencyFilesLocalPath|Bağımlılık dosyaları yerel yolu.|
|     |     |

### <a name="known-issues"></a>Bilinen sorunlar

**Sorun:**<br>
Gizli anahtar betiği çalıştırıldığında gizli dizi için Günlükler döndürme otomatik olarak toplanmaz.

**Sorunu**<br>
AzureStack. DatabaseAdapter. SecretRotation. ps1 _*. log de dahil olmak üzere, C:\Logs. içinde kaydedilen tüm kaynak sağlayıcısı günlüklerini toplamak için Get-AzsDBAdapterLogs cmdlet 'ini kullanın.

## <a name="collect-diagnostic-logs"></a>Tanılama günlüklerini topla

Kilitli sanal makineden günlükleri toplamak için PowerShell 'in yeterli yönetim (JEA) uç noktası DBAdapterDiagnostics 'i kullanabilirsiniz. Bu uç nokta aşağıdaki komutları sağlar:

- **Get-AzsDBAdapterLog**. Bu komut, kaynak sağlayıcısı tanılama günlüklerinin bir ZIP paketini oluşturur ve dosyayı oturumun Kullanıcı sürücüsüne kaydeder. Bu komutu herhangi bir parametre olmadan çalıştırabilirsiniz ve son dört saatlik günlük toplanmaktadır.

- **Remove-AzsDBAdapterLog**. Bu komut, kaynak sağlayıcısı VM 'sinde var olan günlük paketlerini kaldırır.

### <a name="endpoint-requirements-and-process"></a>Uç nokta gereksinimleri ve süreci

Bir kaynak sağlayıcısı yüklendiğinde veya güncelleştirilirken dbadapterdiag Kullanıcı hesabı oluşturulur. Bu hesabı tanılama günlüklerini toplamak için kullanacaksınız.

>[!NOTE]
>Dbadapterdiag hesabı parolası, bir sağlayıcı dağıtımı veya güncelleştirmesi sırasında oluşturulan sanal makinede yerel yönetici için kullanılan parolayla aynıdır.

_Dbadapterdiagnostics_ komutlarını kullanmak için, kaynak sağlayıcısı sanal makinesine bir uzak PowerShell oturumu oluşturun ve **Get-AzsDBAdapterLog** komutunu çalıştırın.

**FromDate** ve **ToDate** parametrelerini kullanarak günlük koleksiyonu için zaman aralığı ayarlarsınız. Bu parametrelerin birini veya ikisini birden belirtmezseniz, aşağıdaki varsayılanlar kullanılır:

* FromDate, geçerli zamandan dört saat öncesine ait.
* ToDate geçerli zaman.

**Günlükleri toplamak için PowerShell betiği örneği.**

Aşağıdaki betik, kaynak sağlayıcısı VM 'sinden tanılama günlüklerinin nasıl toplanacağını göstermektedir.

```powershell
# Create a new diagnostics endpoint session.
$databaseRPMachineIP = '<RP VM IP address>'
$diagnosticsUserName = 'dbadapterdiag'
$diagnosticsUserPassword = '<Enter Diagnostic password>'
$diagCreds = New-Object System.Management.Automation.PSCredential `
        ($diagnosticsUserName, (ConvertTo-SecureString -String $diagnosticsUserPassword -AsPlainText -Force))
$session = New-PSSession -ComputerName $databaseRPMachineIP -Credential $diagCreds
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

# Cleanup the logs.
$cleanup = Invoke-Command -Session $session -ScriptBlock {Remove-AzsDBAdapterLog}
# Close the session.
$session | Remove-PSSession

```

## <a name="next-steps"></a>Sonraki adımlar

[MySQL kaynak sağlayıcısını kaldır](azure-stack-mysql-resource-provider-remove.md)
