---
title: Azure Stack yedeklemesini doğrulamak için ASDK kullanın | Microsoft Docs
description: Azure Stack tümleşik sistemler yedeklemesini doğrulamak için ASDK 'yi nasıl kullanacağınızı öğrenin.
services: azure-stack
author: justinha
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.topic: article
ms.date: 07/31/2019
ms.author: justinha
ms.reviewer: hectorl
ms.lastreviewed: 07/31/2019
ms.openlocfilehash: 8905a376a165776acde2fb792df1e8f35279140e
ms.sourcegitcommit: e8f7fe07b32be33ef621915089344caf1fdca3fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70118765"
---
# <a name="use-the-asdk-to-validate-an-azure-stack-backup"></a>Azure Stack yedeklemesini doğrulamak için ASDK kullanın
Azure Stack dağıttıktan ve kullanıcı kaynaklarını (teklifler, planlar, Kotalar ve abonelikler gibi) sağladıktan sonra, [Azure Stack altyapı yedeklemesini etkinleştirmeniz](../operator/azure-stack-backup-enable-backup-console.md)gerekir. Düzenli altyapı yedeklemeleri zamanlamak ve çalıştırmak, çok zararlı bir donanım veya hizmet hatası varsa altyapı yönetimi verilerinin kaybolmamasını sağlayacaktır.

> [!TIP]
> Kullanılabilir en son altyapı verilerinin bir kopyasına sahip olduğunuzdan emin olmak için bu yordama başlamadan önce [isteğe bağlı yedekleme çalıştırmanızı](../operator/azure-stack-backup-back-up-azure-stack.md) öneririz. Yedekleme başarıyla tamamlandıktan sonra yedekleme KIMLIĞINI yakaladığınızdan emin olun. Bu KIMLIK, bulut kurtarması sırasında gereklidir.

Azure Stack altyapı yedeklemeleri, bulutunuz hakkında Azure Stack yeniden dağıtımı sırasında geri yüklenebildiği önemli verileri içerir. Bu yedeklemeleri, üretim bulutunuzu etkilemeden doğrulamak için ASDK kullanabilirsiniz. 

Aşağıdaki senaryolarda, ASDK üzerindeki yedeklemelerin doğrulanması desteklenir:

|Senaryo|Amaç|
|-----|-----|
|Tümleşik bir çözümden altyapı yedeklerini doğrulayın.|Yedekteki verilerin geçerli olduğu kısa süreli doğrulama.|
|Uçtan uca kurtarma iş akışını öğrenin.|Tüm yedekleme ve geri yükleme deneyimini doğrulamak için ASDK kullanın.|
|     |     |

Şu senaryo, asdk üzerindeki yedeklemeler doğrulanırken desteklenmez:

|Senaryo|Amaç|
|-----|-----|
|Yedekleme ve geri yükleme oluşturmak için ASDK derlemesi.|Önceki bir sürümü olan yedekleme verilerini daha yeni bir sürüme geri yükleyin.|
|     |     |


## <a name="cloud-recovery-deployment"></a>Bulut kurtarma dağıtımı
Tümleşik sistemlerinizden altyapı yedeklemeleri, ASDK 'nin bir bulut kurtarma dağıtımı gerçekleştirerek doğrulanabilir. Bu dağıtımda, belirli hizmet verileri, ana bilgisayara ASDK yüklendikten sonra yedekten geri yüklenir.

### <a name="prereqs"></a>Bulut kurtarma önkoşulları
ASDK 'nin bulut kurtarma dağıtımına başlamadan önce aşağıdaki bilgilere sahip olduğunuzdan emin olun:

**UI yükleyicisi gereksinimleri**

*Geçerli UI yükleyicisi yalnızca şifreleme anahtarını destekliyor*

|Önkoşul|Açıklama|
|-----|-----|
|Yedekleme paylaşımının yolu|Azure Stack altyapı bilgilerini kurtarmak için kullanılacak en son Azure Stack yedeğinin UNC dosya paylaşma yolu. Bu yerel paylaşma, bulut kurtarma dağıtım işlemi sırasında oluşturulacaktır.|
|Geri yüklenecek yedekleme KIMLIĞI|"Xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" alfasayısal biçimindeki yedekleme KIMLIĞI, bulut kurtarması sırasında geri yüklenecek yedeklemeyi tanımlar.|
|Saat sunucusu IP 'si|Azure Stack dağıtımı için 132.163.97.2 gibi geçerli bir saat sunucusu IP 'si gereklidir.|
|Dış sertifika parolası|Azure Stack tarafından kullanılan dış sertifika için parola. CA yedeklemesi, bu parolayla geri yüklenmesi gereken dış sertifikaları içerir.|
|Yedekleme şifreleme anahtarı|Yedekleme ayarları ' de kullanım dışı olan bir şifreleme anahtarıyla yapılandırılmışsa gereklidir. Yükleyici, şifreleme anahtarını en az 3 sürüm için geriye dönük uyumluluk modunda destekleyecektir. Bir sertifika kullanmak için yedekleme ayarlarını güncelleştirdikten sonra, gerekli bilgiler için sonraki tabloya bakın.|

|     |     | 

**PowerShell yükleyicisi gereksinimleri**

*Geçerli PowerShell yükleyicisi şifreleme anahtarını veya şifre çözme sertifikasını destekliyor*

|Önkoşul|Açıklama|
|-----|-----|
|Yedekleme paylaşımının yolu|Azure Stack altyapı bilgilerini kurtarmak için kullanılacak en son Azure Stack yedeğinin UNC dosya paylaşma yolu. Bu yerel paylaşma, bulut kurtarma dağıtım işlemi sırasında oluşturulacaktır.|
|Geri yüklenecek yedekleme KIMLIĞI|"Xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" alfasayısal biçimindeki yedekleme KIMLIĞI, bulut kurtarması sırasında geri yüklenecek yedeklemeyi tanımlar.|
|Saat sunucusu IP 'si|Azure Stack dağıtımı için 132.163.97.2 gibi geçerli bir saat sunucusu IP 'si gereklidir.|
|Dış sertifika parolası|Azure Stack tarafından kullanılan dış sertifika için parola. CA yedeklemesi, bu parolayla geri yüklenmesi gereken dış sertifikaları içerir.|
|Şifre çözme sertifikası parolası|İsteğe bağlı. Yalnızca yedekleme bir sertifika kullanılarak şifrelendiyse gereklidir. Parola, yedekleme verilerinin şifresini çözmek için gereken özel anahtarı içeren otomatik olarak imzalanan sertifika (. pfx) içindir.|
|Yedekleme şifreleme anahtarı|İsteğe bağlı. Yedekleme ayarları hala bir şifreleme anahtarıyla yapılandırıldıysa gereklidir. Yükleyici, şifreleme anahtarını en az 3 sürüm için geriye dönük uyumluluk modunda destekleyecektir. Bir sertifika kullanmak için yedekleme ayarlarını güncelleştirdikten sonra şifre çözme sertifikası için parola sağlamalısınız.|
|     |     | 

## <a name="prepare-the-host-computer"></a>Ana bilgisayarı hazırlama 
Normal bir ASDK dağıtımında olduğu gibi, ASDK ana bilgisayar sistemi ortamı yüklenmek üzere hazırlanmalıdır. ASDK Konak bilgisayar hazırlanmışsa, ASDK dağıtımına başlamak için CloudBuilder. vhdx VM sabit sürücüsünden önyükleme yapılır.

ASDK ana bilgisayarında, yedeklenen Azure Stack aynı sürümüne karşılık gelen yeni bir cloudbuilder. vhdx indirin ve [asdk ana bilgisayarı hazırlama](asdk-prepare-host.md)yönergelerini izleyin.

Konak sunucusu cloudbuilder. vhdx öğesinden yeniden başlatıldıktan sonra, bir dosya paylaşma oluşturmanız ve yedekleme verilerinizi ' ye kopyalamanız gerekir. Dosya paylaşımının kurulum 'u çalıştıran hesap tarafından erişilebilir olması gerekir; Bu örnek PowerShell komutlarında yönetici: 

```powershell
$shares = New-Item -Path "c:\" -Name "Shares" -ItemType "directory"
$azsbackupshare = New-Item -Path $shares.FullName -Name "AzSBackups" -ItemType "directory"
New-SmbShare -Path $azsbackupshare.FullName -FullAccess ($env:computername + "\Administrator")  -Name "AzSBackups"
```

Sonra, en son Azure Stack Yedekleme dosyalarınızı yeni oluşturulan paylaşıma kopyalayın. Paylaşımın içindeki klasör yapısı: `\\<ComputerName>\AzSBackups\MASBackup\<BackupID>\`.

Son olarak, şifre çözme sertifikasını (. pfx) sertifika dizinine `C:\CloudDeployment\Setup\Certificates\` kopyalayın ve dosyayı olarak `BackupDecryptionCert.pfx`yeniden adlandırın.

## <a name="deploy-the-asdk-in-cloud-recovery-mode"></a>Bulut kurtarma modunda ASDK dağıtma

> [!IMPORTANT]
> 1. Geçerli yükleyici Kullanıcı arabirimi yalnızca şifreleme anahtarını destekler. Yalnızca şifreleme anahtarını kullanmaya devam eden sistemlerdeki yedeklemeleri doğrulayabilirsiniz. Yedekleme tümleşik bir sistemde veya bir sertifika kullanılarak kullanılıyorsa, PowerShell yükleyicisini (**ınstallazurestackpoc. ps1**) kullanmanız gerekir. 
> 2. PowerShell yükleyicisi (**ınstallazurestackpoc. ps1**) şifreleme anahtarını veya sertifikayı destekler.
> 3. ASDK yüklemesi ağ için tam olarak bir ağ arabirim kartını (NIC) destekler. Birden çok NIC varsa dağıtım betiğini çalıştırmadan önce yalnızca bir tane etkinleştirildiğinden (ve diğerlerinin tümünün devre dışı bırakıldığından) emin olun.

### <a name="use-the-installer-ui-to-deploy-the-asdk-in-recovery-mode"></a>ASDK 'yi kurtarma modunda dağıtmak için yükleyici Kullanıcı arabirimini kullanma
Bu bölümdeki adımlarda, **asdk-installer. ps1** PowerShell betiğini indirip çalıştırarak, bir grafik kullanıcı ARABIRIMI (GUI) kullanarak asdk dağıtma işlemi gösterilmektedir.

> [!NOTE]
> ASDK için yükleyici Kullanıcı arabirimi, WCF ve PowerShell tabanlı açık kaynaklı bir betiktir.

> [!IMPORTANT]
> Geçerli yükleyici Kullanıcı arabirimi yalnızca şifreleme anahtarını destekler.

1. Ana bilgisayar CloudBuilder. vhdx görüntüsüne başarıyla önyüklendiğinde, asdk [ana bilgisayarını](asdk-prepare-host.md) asdk yüklemesi için hazırladığınız sırada belirtilen yönetici kimlik bilgilerini kullanarak oturum açın. Bu kimlik bilgileri, ASDK ana bilgisayar yerel yönetici kimlik bilgileriyle aynı olmalıdır.
2. Yükseltilmiş bir PowerShell konsolu açın ve  **&lt;sürücü harfini > \azurestack_ınstaller\asdk-Installer.exe** adlı PowerShell betiği çalıştırın. Betik artık C:\ ' dan farklı bir sürücüde olabilir CloudBuilder. vhdx görüntüsünde. **Kurtar**' ı tıklatın.

    ![ASDK yükleyici betiği](media/asdk-validate-backup/1.PNG) 

3. Kimlik sağlayıcısı ve kimlik bilgileri sayfasında, ASDK ana bilgisayar için Azure AD dizin bilgilerinizi (isteğe bağlı) ve yerel yönetici parolasını girin. **İleri**'ye tıklayın.

    ![ASDK kimliği ve kimlik bilgileri sayfası](media/asdk-validate-backup/2.PNG) 

4. ASDK ana bilgisayar tarafından kullanılacak ağ bağdaştırıcısını seçin ve **İleri**' ye tıklayın. Diğer tüm ağ arabirimleri, ASDK yüklemesi sırasında devre dışı bırakılacak. 

    ![ASDK ağ bağdaştırıcısı arabirimi](media/asdk-validate-backup/3.PNG) 

5. Ağ yapılandırması sayfasında, geçerli saat sunucusu ve DNS ileticisi IP adresleri sağlayın. **İleri**'ye tıklayın.

    ![ASDK ağ yapılandırması sayfası](media/asdk-validate-backup/4.PNG) 

6. Ağ arabirim kartı özellikleri doğrulandığında, **İleri**' ye tıklayın. 

    ![ASDK ağ kartı ayarlarının doğrulanması](media/asdk-validate-backup/5.PNG) 

7. Yedekleme ayarları sayfasındaki [Önkoşullar bölümünde](#prereqs) açıklanan gerekli bilgileri ve paylaşıma erişmek için kullanılacak kullanıcı adını ve parolayı belirtin. **İleri**' ye tıklayın: 

   ![ASDK yedekleme ayarları sayfası](media/asdk-validate-backup/6.PNG) 

8. Özet sayfasında ASDK 'yi dağıtmak için kullanılacak dağıtım betiğini gözden geçirin. Dağıtıma başlamak için **Dağıt** ' a tıklayın. 

    ![ASDK Özet sayfası](media/asdk-validate-backup/7.PNG) 


### <a name="use-powershell-to-deploy-the-asdk-in-recovery-mode"></a>PowerShell kullanarak ASDK 'yi kurtarma modunda dağıtma

Ortamınız için aşağıdaki PowerShell komutlarını değiştirin ve bulut kurtarma modunda ASDK 'yi dağıtmak üzere çalıştırın:

**Bulut kurtarma 'yı şifreleme anahtarıyla başlatmak için ınstallazurestackpoc. ps1 betiğini kullanın.**

```powershell
cd C:\CloudDeployment\Setup     
$adminpass = Read-Host -AsSecureString -Prompt "Local Administrator password"
$certPass = Read-Host -AsSecureString -Prompt "Password for the external certificate"
$backupstorecredential = Read-Host -AsSecureString -Prompt "Credential for backup share"
$key = Read-Host -AsSecureString -Prompt "Your backup encryption key"

.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass `
 -BackupStorePath ("\\" + $env:COMPUTERNAME + "\AzSBackups") `
 -BackupEncryptionKeyBase64 $key `
 -BackupStoreCredential $backupstorecredential `
 -BackupId "<Backup ID to restore>" `
 -TimeServer "<Valid time server IP>" -ExternalCertPassword $certPass
```

**Şifre çözme sertifikasıyla bulut kurtarmayı başlatmak için ınstallazurestackpoc. ps1 betiğini kullanın.**

```powershell
cd C:\CloudDeployment\Setup     
$adminpass = Read-Host -AsSecureString -Prompt "Local Administrator password"
$certPass = Read-Host -AsSecureString -Prompt "Password for the external certificate"
$backupstorecredential = Read-Host -AsSecureString -Prompt "Credential for backup share"
$decryptioncertpassword  = Read-Host -AsSecureString -Prompt "Password for the decryption certificate"

.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass `
 -BackupStorePath ("\\" + $env:COMPUTERNAME + "\AzSBackups") `
 -BackupDecryptionCertPassword $decryptioncertpassword `
 -BackupStoreCredential $backupstorecredential `
 -BackupId "<Backup ID to restore>" `
 -TimeServer "<Valid time server IP>" -ExternalCertPassword $certPass
```

## <a name="complete-cloud-recovery"></a>Bulut kurtarma işleminin tamamını 
Başarılı bir bulut kurtarma dağıtımından sonra restore **-AzureStack** cmdlet 'ini kullanarak geri yüklemeyi doldurmanız gerekir. 

Azure Stack işleci olarak oturum açtıktan sonra, [Azure Stack PowerShell 'i yükleyin](asdk-post-deploy.md#install-azure-stack-powershell) ve yedekten geri yüklerken kullanılacak sertifikayı ve parolayı belirtmek için aşağıdaki komutları çalıştırın:

**Sertifika dosyası ile kurtarma modu**

> [!NOTE]
> Azure Stack dağıtım, güvenlik nedenleriyle şifre çözme sertifikasını kalıcı tutmaz. Şifre çözme sertifikası ve ilişkili parolayı yeniden sağlamanız gerekir.

```powershell
$decryptioncertpassword = Read-Host -AsSecureString -Prompt "Password for the decryption certificate"
Restore-AzsBackup -ResourceId "<BackupID>" `
 -DecryptionCertPath "<path to decryption certificate with file name (.pfx)>" `
 -DecryptionCertPassword $decryptioncertpassword
```

**Şifreleme anahtarı ile kurtarma modu**
```powershell
$decryptioncertpassword = Read-Host -AsSecureString -Prompt "Password for the decryption certificate"
Restore-AzsBackup -ResourceId "<BackupID>"
```

Bu cmdlet 'i çağırdıktan sonra, buluta kurtarılan ASDK üzerindeki yedekleme verilerinin doğrulanmasını başlatmak için 60 dakika bekleyin.

## <a name="next-steps"></a>Sonraki adımlar
[Kayıt Azure Stack](asdk-register.md)

