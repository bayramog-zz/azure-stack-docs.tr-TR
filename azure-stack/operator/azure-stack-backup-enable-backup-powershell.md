---
title: PowerShell ile Azure Stack için yedeklemeyi etkinleştirme | Microsoft Docs
description: Bir hata oluşursa Azure Stack geri yüklenebilmesi için PowerShell ile Infrastructure Backup hizmetini nasıl etkinleştirebileceğinizi öğrenin.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2019
ms.author: justinha
ms.reviewer: hectorl
ms.lastreviewed: 03/14/2019
ms.openlocfilehash: 2e419c32caf78d97ee38e570ce0fa823cc94651a
ms.sourcegitcommit: 245a4054a52e54d5989d6148fbbe386e1b2aa49c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70975174"
---
# <a name="enable-backup-for-azure-stack-with-powershell"></a>PowerShell ile Azure Stack için yedeklemeyi etkinleştirme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Windows PowerShell ile Infrastructure Backup hizmeti 'ni, düzenli aralıklarla yedeklemeler alacak şekilde etkinleştirin:
 - İç kimlik hizmeti ve kök sertifika.
 - Kullanıcı planları, teklifler, abonelikler.
 - İşlem, depolama ve ağ Kullanıcı kotaları.
 - Kullanıcı parolaları Key Vault.
 - Kullanıcı RBAC rolleri ve ilkeleri.
 - Kullanıcı depolama hesapları.

Yedeklemeyi etkinleştirmek, yedeklemeyi başlatmak ve işletmen yönetimi uç noktası aracılığıyla yedekleme bilgilerini almak için PowerShell cmdlet 'lerine erişebilirsiniz.

## <a name="prepare-powershell-environment"></a>PowerShell ortamını hazırlama

PowerShell ortamını yapılandırma hakkında yönergeler için bkz. [Azure Stack Için PowerShell 'ı yükler](azure-stack-powershell-install.md). Azure Stack oturum açmak için bkz. [işleç ortamını yapılandırma ve Azure Stack oturum açma](azure-stack-powershell-configure-admin.md).

## <a name="provide-the-backup-share-credentials-and-encryption-key-to-enable-backup"></a>Yedeklemeyi etkinleştirmek için yedekleme paylaşımının, kimlik bilgilerinin ve şifreleme anahtarının sağlanması

Aynı PowerShell oturumunda, ortamınız için değişkenleri ekleyerek aşağıdaki PowerShell betiğini düzenleyin. Infrastructure Backup hizmetine yedekleme paylaşımının, kimlik bilgilerinin ve şifreleme anahtarının sağlanması için güncelleştirilmiş betiği çalıştırın.

| Değişken        | Açıklama   |
|---              |---                                        |
| `$username`       | Dosyaları okumak ve yazmak için yeterli erişimi olan paylaşılan sürücü konumunun etki alanını ve Kullanıcı adını kullanarak **Kullanıcı adını** yazın. Örneğin: `Contoso\backupshareuser`. |
| `$password`       | Kullanıcının **parolasını** yazın. |
| `$sharepath`      | **Yedekleme depolama konumunun**yolunu yazın. Ayrı bir cihazda barındırılan dosya paylaşımının yolu için bir evrensel adlandırma kuralı (UNC) dizesi kullanmanız gerekir. UNC dizesi, paylaşılan dosyalar veya cihazlar gibi kaynakların konumunu belirtir. Yedekleme verilerinin kullanılabilirliğini sağlamak için, cihazın ayrı bir konumda olması gerekir. |
| `$frequencyInHours` | Saat cinsinden sıklık, yedeklemelerin oluşturulma sıklığını belirler. Varsayılan değer 12 ' dir. Zamanlayıcı en fazla 12 ve en az 4 destekler.|
| `$retentionPeriodInDays` | Gün cinsinden saklama süresi, dış konumda kaç günlük yedeklemenin korunacağını belirler. Varsayılan değer 7 ' dir. Zamanlayıcı en fazla 14 ve en az 2 destekler. Saklama süresinden daha eski yedeklemeler, dış konumdan otomatik olarak silinir.|
| `$encryptioncertpath` | 1901 ve üzeri için geçerlidir. Parametresi Azure Stack modülü sürüm 1,7 ve sonrasında kullanılabilir. Şifreleme sertifikası yolu, dosyasının yolunu belirtir. Veri şifreleme için kullanılan ortak anahtara sahip CER dosyası. |
| `$encryptionkey` | Derleme 1811 veya önceki sürümler için geçerlidir. Parametre Azure Stack modülü 1,6 veya daha önceki bir sürümünde kullanılabilir. Şifreleme anahtarı veri şifreleme için kullanılır. Yeni bir anahtar oluşturmak için [New-AzsEncryptionKeyBase64](https://docs.microsoft.com/powershell/module/azs.backup.admin/new-azsencryptionkeybase64) cmdlet 'ini kullanın. |
|     |     |

### <a name="enable-backup-on-1901-and-later-using-certificate"></a>Sertifikayı kullanarak 1901 ve üzeri yedeklemeyi etkinleştir
```powershell
    # Example username:
    $username = "domain\backupadmin"
 
    # Example share path:
    $sharepath = "\\serverIP\AzSBackupStore\contoso.com\seattle"

    $password = Read-Host -Prompt ("Password for: " + $username) -AsSecureString

    # Create a self-signed certificate using New-SelfSignedCertificate, export the public key portion and save it locally.

    $cert = New-SelfSignedCertificate `
        -DnsName "www.contoso.com" `
        -CertStoreLocation "cert:\LocalMachine\My" 

    New-Item -Path "C:\" -Name "Certs" -ItemType "Directory" 

    #make sure to export the PFX format of the certificate with the public and private keys and then delete the certificate from the local certificate store of the machine where you created the certificate
    
    Export-Certificate `
        -Cert $cert `
        -FilePath c:\certs\AzSIBCCert.cer 

    # Set the backup settings with the name, password, share, and CER certificate file.
    Set-AzsBackupConfiguration -BackupShare $sharepath -Username $username -Password $password -EncryptionCertPath "c:\temp\cert.cer"
```
### <a name="enable-backup-on-1811-or-earlier-using-certificate"></a>Sertifikayı kullanarak 1811 veya önceki sürümlerde yedeklemeyi etkinleştir
```powershell
    # Example username:
    $username = "domain\backupadmin"
 
    # Example share path:
    $sharepath = "\\serverIP\AzSBackupStore\contoso.com\seattle"

    $password = Read-Host -Prompt ("Password for: " + $username) -AsSecureString

    # Create a self-signed certificate using New-SelfSignedCertificate, export the public key portion and save it locally.

    $key = New-AzsEncryptionKeyBase64
    $Securekey = ConvertTo-SecureString -String ($key) -AsPlainText -Force

    # Set the backup settings with the name, password, share, and CER certificate file.
    Set-AzsBackupConfiguration -BackupShare $sharepath -Username $username -Password $password -EncryptionKey $Securekey
```

   
##  <a name="confirm-backup-settings"></a>Yedekleme ayarlarını Onayla

Aynı PowerShell oturumunda aşağıdaki komutları çalıştırın:

   ```powershell
    Get-AzsBackupConfiguration | Select-Object -Property Path, UserName
   ```

Sonuç aşağıdaki örnek çıktı gibi görünmelidir:

   ```powershell
    Path                        : \\serverIP\AzsBackupStore\contoso.com\seattle
    UserName                    : domain\backupadmin
   ```

## <a name="update-backup-settings"></a>Yedekleme ayarlarını Güncelleştir
Aynı PowerShell oturumunda, yedeklemeler için bekletme dönemi ve sıklığı için varsayılan değerleri güncelleştirebilirsiniz. 

   ```powershell
    #Set the backup frequency and retention period values.
    $frequencyInHours = 10
    $retentionPeriodInDays = 5

    Set-AzsBackupConfiguration -BackupFrequencyInHours $frequencyInHours -BackupRetentionPeriodInDays $retentionPeriodInDays

    Get-AzsBackupConfiguration | Select-Object -Property Path, UserName, AvailableCapacity, BackupFrequencyInHours, BackupRetentionPeriodInDays
   ```

Sonuç aşağıdaki örnek çıktı gibi görünmelidir:

   ```powershell
    Path                        : \\serverIP\AzsBackupStore\contoso.com\seattle
    UserName                    : domain\backupadmin
    AvailableCapacity           : 60 GB
    BackupFrequencyInHours      : 10
    BackupRetentionPeriodInDays : 5
   ```

### <a name="azure-stack-powershell"></a>Azure Stack PowerShell 
Altyapı yedeklemesini yapılandırmak için PowerShell cmdlet 'i set-AzsBackupConfiguration ' dır. Önceki sürümlerde cmdlet, set-AzsBackupShare idi. Bu cmdlet 'in bir sertifika sağlaması gerekir. Altyapı yedeklemesi bir şifreleme anahtarıyla yapılandırıldıysa, şifreleme anahtarını güncelleştiremez veya özelliği görüntüleyemezsiniz. Yönetici PowerShell 'in 1,6 sürümünü kullanmanız gerekir.

Altyapı yedeklemesi 1901 sürümüne güncelleştirmeden önce yapılandırıldıysa, şifreleme anahtarını ayarlamak ve görüntülemek için yönetici PowerShell 'in 1,6 sürümünü kullanabilirsiniz. Sürüm 1,6, şifreleme anahtarından bir sertifika dosyasına güncelleştirme yapmanıza izin vermez.
Modülün doğru sürümünü yükleme hakkında daha fazla bilgi için [Azure Stack PowerShell 'ı yükleme](azure-stack-powershell-install.md) bölümüne bakın.


## <a name="next-steps"></a>Sonraki adımlar

Bir yedekleme çalıştırmayı öğrenin, bkz. Yedekleme [Azure Stack](azure-stack-backup-back-up-azure-stack.md).

Yedeğinizin çalıştığını doğrulamayı öğrenin, bkz. [yönetim portalında yedeklemeyi onaylama tamamlandı](azure-stack-backup-back-up-azure-stack.md).
