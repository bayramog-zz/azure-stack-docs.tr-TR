---
title: Gizli dizileri Azure Stack döndür | Microsoft Docs
description: Azure Stack ' de sırlarınızı döndürme hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2019
ms.reviewer: ppacent
ms.author: mabrigg
ms.lastreviewed: 09/30/2019
monikerRange: '>=azs-1803'
ms.openlocfilehash: f32a25997e4336a24dfb9b673202882cff1845e9
ms.sourcegitcommit: 70147e858956443bc66b3541ec278c102bb45c07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72381467"
---
# <a name="rotate-secrets-in-azure-stack"></a>Azure Stack gizli dizileri döndürme

*Bu yönergeler yalnızca Azure Stack tümleşik sistemler sürüm 1803 ve üzeri için geçerlidir. 1802 öncesi Azure Stack sürümlerde gizli anahtar dönüşü yapmayı denemeyin*

Gizlilikler, Azure Stack altyapı kaynakları ve hizmetleri arasında güvenli iletişimi korumanıza yardımcı olur.

## <a name="overview-to-rotate-secrets"></a>Gizli dizileri döndürme için genel bakış

1. Gizli anahtar döndürme için kullanılacak sertifikaları hazırlayın.
2. Azure Stack [ortak anahtar altyapısı sertifika gereksinimlerini](https://docs.microsoft.com/azure-stack/operator/azure-stack-pki-certs)gözden geçirin.
3. Her şeyin iyi olduğunu onaylamak için [ayrıcalıklı uç noktasını kullanın](azure-stack-privileged-endpoint.md) ve **Test-azurestack** komutunu çalıştırın.  
4. [Gizli anahtar döndürme adımları](#pre-steps-for-secret-rotation)hakkında daha fazla bilgi edinebilirsiniz.
5. [Azure Stack PKI sertifikalarını doğrulayın](https://docs.microsoft.com/azure-stack/operator/azure-stack-validate-pki-certs). Parolada `*` veya `)` gibi özel karakterler bulunmadığından emin olun.
6. PFX şifrelemesinin **TripleDES-SHA1**olduğundan emin olun. Bir sorunla karşılaşırsanız, bkz. [Azure Stack PKI sertifikaları için yaygın sorunları giderme](https://docs.microsoft.com/azure-stack/operator/azure-stack-remediate-certs#pfx-encryption).
7. Klasör yapısını hazırlayın.  [Dönen dış gizlilikler](https://docs.microsoft.com/azure-stack/operator/azure-stack-rotate-secrets#rotating-external-secrets) bölümünde bir örnek bulabilirsiniz.
8. [Gizli anahtarı döndürmeyi başlatın](#use-powershell-to-rotate-secrets).

## <a name="rotate-secrets"></a>Gizli dizileri döndürme

Azure Stack, Azure Stack altyapısı kaynakları ve hizmetleri arasında güvenli iletişim sağlamak için çeşitli gizli dizileri kullanır.

- **İç gizlilikler**

    Azure Stack Işlecini müdahale etmeden Azure Stack altyapısı tarafından kullanılan tüm sertifikalar, parolalar, güvenli dizeler ve anahtarlar.

- **Dış gizlilikler**

    Azure Stack Işleci tarafından sunulan dışarıdan yönelik hizmetler için altyapı hizmeti sertifikaları. Dış gizlilikler aşağıdaki hizmetler için sertifikaları içerir:

    - Yönetici portalı
    - Ortak Portal
    - Yönetici Azure Resource Manager
    - Genel Azure Resource Manager
    - Yönetici Keykasası
    - KeyVault
    - Yönetici uzantısı ana bilgisayarı
    - ACS (blob, tablo ve kuyruk depolama dahil)
    - FS
    - Çıkarılamıyor
    
    \* yalnızca ortamın kimlik sağlayıcısı federe hizmetler Active Directory (AD FS) ise geçerlidir.

> [!Note]
> BMC ve anahtar parolaları dahil diğer tüm güvenli anahtar ve dizeler, Kullanıcı ve yönetici hesabı parolaları hala yönetici tarafından el ile güncelleştirilir.

> [!Important]
> Azure Stack 1811 sürümü ile başlayarak, iç ve dış sertifikalar için gizli döndürme ayrılmıştır.

Azure Stack altyapısının bütünlüğünü sürdürmek için, işleçlerin altyapısının gizli dizilerini kuruluşun güvenlik gereksinimleriyle tutarlı sıklıklarla düzenli olarak döndürme yeteneğinin olması gerekir.

### <a name="rotating-secrets-with-external-certificates-from-a-new-certificate-authority"></a>Yeni bir sertifika yetkilisinden dış sertifikalarla gizli dizileri döndürme

Azure Stack, aşağıdaki bağlamlarda yeni bir sertifika yetkilisinden (CA) dış sertifikalarla gizli döndürmeyi destekler:

|Yüklü sertifika CA 'sı|Döndürülecek CA|Desteklenen|Desteklenen Azure Stack sürümleri|
|-----|-----|-----|-----|
|Otomatik olarak Imzalanan|Kuruluşa|Desteklenen|& daha sonra 1903|
|Otomatik olarak Imzalanan|Kendinden Imzalı için|Desteklenmiyor||
|Otomatik olarak Imzalanan|Genel<sup>*</sup>|Desteklenen|& daha sonra 1803|
|Kurumsal|Kuruluşa|Destekleniyor. 1803-1903 ' den itibaren, müşteriler dağıtımda kullanılan kurumsal CA 'yı kullandığı sürece desteklenir.|& daha sonra 1803|
|Kurumsal|Kendinden Imzalı için|Desteklenmiyor||
|Kurumsal|Genel<sup>*</sup>|Desteklenen|& daha sonra 1803|
|Ortak<sup>*</sup> ' den|Kuruluşa|Desteklenen|& daha sonra 1903|
|Ortak<sup>*</sup> ' den|Kendinden Imzalı için|Desteklenmiyor||
|Ortak<sup>*</sup> ' den|Genel<sup>*</sup>|Desteklenen|& daha sonra 1803|

<sup>*</sup> Ortak sertifika yetkililerinin Windows güvenilen kök programın bir parçası olan kişiler olduğunu gösterir. Tam listeyi [Microsoft güvenilen kök sertifika programı: katılımcılar (27 haziran 2017 itibariyle)](https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca)makalesinde bulabilirsiniz.

## <a name="alert-remediation"></a>Uyarı Düzeltme

Parolaların süresi 30 gün içinde olduğunda, yönetici portalında aşağıdaki uyarılar oluşturulur:

- Bekleyen hizmet hesabı parolası süre sonu
- İç sertifika süre sonu bekleniyor
- Beklemedeki dış sertifika süre sonu

Aşağıdaki yönergeleri kullanarak gizli bir döndürme çalıştırmak, bu uyarıları düzeltir.

> [!Note]
> 1811 öncesi sürümlerde Azure Stack ortamlar, bekleyen iç sertifika veya gizli süre sonları için uyarıları görebilir.
> Bu uyarılar yanlış ve iç gizli döndürme çalıştırmadan yok sayılacak.
> Yanlış iç gizli zaman aşımı uyarıları 1811 ' de çözümlenen bilinen bir sorundur ve ortam iki yıl boyunca etkin değilse iç gizli dizileri sona ermeyecektir.

## <a name="pre-steps-for-secret-rotation"></a>Gizli anahtar döndürme için ön adımlar

   > [!IMPORTANT]
   > Azure Stack ortamınızda gizli döndürme zaten gerçekleştirilirse, gizli anahtarı yeniden yürütmeden önce sistemi 1811 veya sonraki bir sürüme güncelleştirmeniz gerekir.
   > Gizli [uç nokta](azure-stack-privileged-endpoint.md) aracılığıyla gizli bir döndürme yürütülmelidir ve Azure Stack operatör kimlik bilgileri gerekir.
   > Ortamınız Azure Stack Işletmenlerinizin gizli döndürme ortamınızda çalıştırılıp çalıştırılmadığını bilmez ise, gizli döndürmeyi yeniden yürütmeden önce 1811 olarak güncelleştirin.

1. Azure Stack örneğinizi 1811 sürümüne güncelleştirmeniz kesinlikle önerilir.

    > [!Note] 
    > 1811 öncesi sürümler için uzantı ana bilgisayar sertifikaları eklemek üzere gizli dizileri döndürmenize gerek yoktur. Uzantı konak sertifikaları eklemek için [Azure Stack uzantı ana bilgisayarına hazırlanma](azure-stack-extension-host-prepare.md) makalesindeki yönergeleri izlemelisiniz.

2. Operatörler Azure Stack gizli dizilerinin rotasyonu sırasında uyarıların açıldığını ve otomatik olarak kapatıldığını görebilir.  Bu davranış beklenmektedir ve uyarılar yoksayılabilir.  İşleçler, **Test-AzureStack**komutunu çalıştırarak bu uyarıların geçerliliğini doğrulayabilirler.  Azure Stack sistemlerini izlemek için System Center Operations Manager kullanan operatörler için, sistemi bakım moduna almak bu uyarıların ıTSM sistemlerine ulaşmasını engeller ancak Azure Stack sistemi ulaşılamaz hale gelirse uyarı almaya devam edecektir.

3. Kullanıcılarınıza tüm bakım işlemlerini bildirin. İş saatleri dışında normal bakım pencerelerini mümkün olduğunca düzenli olarak zamanlayın. Bakım işlemleri, hem Kullanıcı iş yüklerini hem de Portal işlemlerini etkileyebilir.

    > [!Note]
    > Sonraki adımlar yalnızca Azure Stack dış gizli dizileri döndürürken geçerlidir.

4. **[Test-AzureStack](azure-stack-diagnostic-test.md)** komutunu çalıştırın ve gizli dizileri döndürmeden önce tüm test çıktılarının sağlıklı olduğunu onaylayın.
5. Yeni bir değiştirme dış sertifika kümesi hazırlayın. Yeni küme, [Azure Stack PKI sertifika gereksinimlerinde](azure-stack-pki-certs.md)özetlenen sertifika belirtimleriyle eşleşir. [PKI sertifikaları](azure-stack-get-pki-certs.md) oluşturma ' da açıklanan adımları kullanarak yeni sertifikalar satın alma veya oluşturma için bir sertifika imzalama ISTEğI (CSR) oluşturabilir ve [Azure Stack PKI hazırlama bölümündeki adımları kullanarak bunları Azure Stack ortamınızda kullanıma hazırlarsınız Sertifikalar](azure-stack-prepare-pki-certs.md). Hazırladığınız sertifikaları, [PKI sertifikalarının doğrulanması](azure-stack-validate-pki-certs.md)bölümünde özetlenen adımlarla doğruladığınızdan emin olun.
6. Güvenli bir yedekleme konumunda, döndürme için kullanılan sertifikalara bir yedekleme depolayın. Döndürme işlemi çalışırsa ve başarısız olursa, döndürmeyi yeniden çalıştırmadan önce dosya paylaşımındaki sertifikaları yedek kopyalarla değiştirin. Yedekleme kopyalarını güvenli yedekleme konumunda tutun.
7. ERCS sanal makinelerinden erişebileceğiniz bir dosya paylaşımı oluşturun. **CloudAdmin** kimliği için dosya paylaşımının okunabilir ve yazılabilir olması gerekir.
8. Dosya paylaşımına erişiminizin bulunduğu bir bilgisayardan PowerShell ıSE konsolunu açın. Dosya paylaşımına gidin.
9. Dış sertifikalarınız için gerekli dizinleri oluşturmak üzere **[Certdirectorymaker. ps1](https://www.aka.ms/azssecretrotationhelper)** komutunu çalıştırın.

> [!IMPORTANT]
> CertDirectoryMaker betiği, aşağıdakileri barındıracak bir klasör yapısı oluşturur:
>
> Azure Stack için kullanılan kimlik sağlayıcınıza bağlı olarak **.\Certificates\aad** veya ***.\Certificates\adfs***
>
> Klasör yapınızın **AAD** veya **ADFS** klasörleri ile sona erdiği ve tüm alt dizinlerin bu yapıda olduğu en önemli öneme sahiptir; Aksi takdirde, **Start-SecretRotation** şu şekilde görünür:
> ```powershell
> Cannot bind argument to parameter 'Path' because it is null.
> + CategoryInfo          : InvalidData: (:) [Test-Certificate], ParameterBindingValidationException
> + FullyQualifiedErrorId : ParameterArgumentValidationErrorNullNotAllowed,Test-Certificate
> + PSComputerName        : xxx.xxx.xxx.xxx
> ```
>
> Hata maszu, FileShare 'e erişirken bir sorun olduğunu gösterir ancak gerçekte bu, burada zorlanmakta olan klasör yapısıdır.
> Microsoft AzureStack hazırlık denetleyicisi- [Publiccerthelper modülünde](https://www.powershellgallery.com/packages/Microsoft.AzureStack.ReadinessChecker/1.1811.1101.1/Content/CertificateValidation%5CPublicCertHelper.psm1) daha fazla bilgi bulunabilir
>
> Ayrıca, FileShare klasör yapınızın **Sertifikalar** klasörüyle başlaması de önemlidir, aksi takdirde doğrulama işlemi de başarısız olur.
> FileShare Mount **\\ @ no__t-2 @ no__t-3ıpaddress > \\ @ no__t-5ShareName > \\** ve içinde **Certificates\aad** veya **certificates\adfs** klasörü içermelidir.
>
> Örnek:
> - FileShare = **\\ @ no__t-2 @ no__t-3ıpaddress > \\ @ no__t-5ShareName > \\**
> - CertFolder = **Certificates\aad**
> - FullPath = **\\ @ no__t-2 @ no__t-3ıpaddress > \\ @ no__t-5ShareName > \Certificates\AAD**

## <a name="rotating-external-secrets"></a>Dış gizli dizileri döndürme

Dış gizli dizileri döndürmek için:

1. Ön adımlarda oluşturulan yeni oluşturulan **\Certificates @ no__t-1 @ no__t-2ıdentityprovider >** dizininde, yeni bir değiştirme dış sertifika kümesini dizin yapısına, zorunlu olarak belirtilen biçime göre yerleştirin [Azure Stack PKI sertifikası gereksinimlerinin](azure-stack-pki-certs.md#mandatory-certificates)sertifikalar bölümü.

    Azure AD kimlik sağlayıcısı için klasör yapısına örnek:
    ```powershell
        <ShareName>
        │   │
        │   ├───Certificates
        │   └───AAD
        │       ├───ACSBlob
        │       │       <CertName>.pfx
        │       │
        │       ├───ACSQueue
        │       │       <CertName>.pfx
        │       │
        │       ├───ACSTable
        │       │       <CertName>.pfx
        │       │
        │       ├───Admin Extension Host
        │       │       <CertName>.pfx
        │       │
        │       ├───Admin Portal
        │       │       <CertName>.pfx
        │       │
        │       ├───ARM Admin
        │       │       <CertName>.pfx
        │       │
        │       ├───ARM Public
        │       │       <CertName>.pfx
        │       │
        │       ├───KeyVault
        │       │       <CertName>.pfx
        │       │
        │       ├───KeyVaultInternal
        │       │       <CertName>.pfx
        │       │
        │       ├───Public Extension Host
        │       │       <CertName>.pfx
        │       │
        │       └───Public Portal
        │               <CertName>.pfx

    ```

2. **CloudAdmin** hesabını kullanarak [ayrıcalıklı uç nokta](azure-stack-privileged-endpoint.md) ile bir PowerShell oturumu oluşturun ve oturumları bir değişken olarak depolayın. Bu değişkeni bir sonraki adımda parametresi olarak kullanacaksınız.

    > [!IMPORTANT]  
    > Oturumu girmeyin ve oturumu bir değişken olarak depolayın.

3. **[Invoke-Command komutunu](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/Invoke-Command?view=powershell-5.1)** çalıştırın. Ayrıcalıklı uç nokta PowerShell oturum değişkeninizi **oturum** parametresi olarak geçirin.

4. Şu parametrelerle **Start-SecretRotation** çalıştırın:
    - **PfxFilesPath**  
    Daha önce oluşturduğunuz sertifika dizininizin ağ yolunu belirtin.  
    - **PathAccessCredential**  
    Paylaşımdaki kimlik bilgileri için bir PSCredential nesnesi.
    - **CertificatePassword**  
    Oluşturulan tüm PFX Sertifika dosyaları için kullanılan parolanın güvenli bir dizesi.

5. Gizli dizileri döndürürken bekleyin. Dış parola döndürme genellikle yaklaşık bir saat sürer.

    Gizli döndürme başarıyla tamamlandığında, konsolunuz **genel eylem durumu: başarılı**' i görüntüler.

    > [!Note]
    > Gizli döndürme başarısız olursa, hata iletisindeki yönergeleri izleyin ve **-yeniden çalıştır** parametresiyle **Start-secrecontrols** öğesini yeniden çalıştırın.

    ```powershell
    Start-SecretRotation -ReRun
    ```
    Yinelenen gizli döndürme hatalarıyla karşılaşırsanız desteğe başvurun.

6. Gizli anahtar başarıyla tamamlandıktan sonra, ön adımla oluşturulan paylaşımdan sertifikalarınızı kaldırın ve bunları güvenli yedekleme konumlarına depolayın.

## <a name="use-powershell-to-rotate-secrets"></a>Gizli dizileri döndürmek için PowerShell 'i kullanma

Aşağıdaki PowerShell örneği, sırlarınızı döndürmek için çalıştırılacak cmdlet 'leri ve parametreleri gösterir.

```powershell
# Create a PEP Session
winrm s winrm/config/client '@{TrustedHosts= "<IpOfERCSMachine>"}'
$PEPCreds = Get-Credential
$PEPSession = New-PSSession -ComputerName <IpOfERCSMachine> -Credential $PEPCreds -ConfigurationName "PrivilegedEndpoint"

# Run Secret Rotation
$CertPassword = ConvertTo-SecureString "<CertPasswordHere>" -AsPlainText -Force
$CertShareCreds = Get-Credential
$CertSharePath = "<NetworkPathOfCertShare>"
Invoke-Command -Session $PEPSession -ScriptBlock {
    Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCreds -CertificatePassword $using:CertPassword
}
Remove-PSSession -Session $PEPSession
```

## <a name="rotating-only-internal-secrets"></a>Yalnızca iç gizli dizileri döndürme

> [!Note]
> İç gizli anahtar, yalnızca bir iç parolanın kötü amaçlı bir varlık tarafından tehlikeye alındığını veya iç sertifikaların süresi dolmak üzere olduğunu belirten bir uyarı (derleme 1811 veya sonraki bir sürüm) aldıysanız yapılmalıdır.
> 1811 öncesi sürümlerde Azure Stack ortamlar, bekleyen iç sertifika veya gizli süre sonları için uyarıları görebilir.
> Bu uyarılar yanlış ve iç gizli döndürme çalıştırmadan yok sayılacak.
> Yanlış iç gizli zaman aşımı uyarıları 1811 ' de çözümlenen bilinen bir sorundur ve ortam iki yıl boyunca etkin değilse iç gizli dizileri sona ermeyecektir.

1. [Ayrıcalıklı uç nokta](azure-stack-privileged-endpoint.md)Ile bir PowerShell oturumu oluşturun.
2. Ayrıcalıklı uç nokta oturumunda **Start-SecretRotation-Internal**' ı çalıştırın.

    > [!Note]
    > 1811 öncesi sürümlerde Azure Stack ortamlar **-Internal** bayrağını gerektirmez. **Start-SecretRotation** yalnızca iç gizli dizileri döndürebilir.

3. Gizli dizileri döndürürken bekleyin.

   Gizli döndürme başarıyla tamamlandığında, konsolunuz **genel eylem durumu: başarılı**' i görüntüler.
    > [!Note]
    > Gizli döndürme başarısız olursa, hata iletisindeki yönergeleri uygulayın ve **-Internal** ve **-yeniden çalıştır** parametreleriyle **Start-secretrotation** ' ı yeniden çalıştırın.  

```powershell
Start-SecretRotation -Internal -ReRun
```

Yinelenen gizli döndürme hatalarıyla karşılaşırsanız desteğe başvurun.

## <a name="start-secretrotation-reference"></a>Start-SecretRotation başvurusu

Azure Stack sisteminin gizli dizilerini döndürür. Yalnızca Azure Stack ayrıcalıklı uç nokta ile yürütülür.

### <a name="syntax"></a>Sözdizimi

#### <a name="for-external-secret-rotation"></a>Dış gizli dizi dönüşü için

```powershell
Start-SecretRotation [-PfxFilesPath <string>] [-PathAccessCredential <PSCredential>] [-CertificatePassword <SecureString>]  
```

#### <a name="for-internal-secret-rotation"></a>İç gizli anahtar için

```powershell
Start-SecretRotation [-Internal]  
```

#### <a name="for-external-secret-rotation-rerun"></a>Dış gizli anahtar döndürme için yeniden çalıştır

```powershell
Start-SecretRotation [-ReRun]
```

#### <a name="for-internal-secret-rotation-rerun"></a>İç gizli anahtar döndürme için yeniden çalıştır

```powershell
Start-SecretRotation [-ReRun] [-Internal]
```

### <a name="description"></a>Açıklama

**Start-SecretRotation** cmdlet 'i bir Azure Stack sisteminin altyapı gizli dizilerini döndürür. Varsayılan olarak, yalnızca tüm dış ağ altyapısı uç noktalarına ait sertifikaları döndürür. -Internal bayrağıyla kullanılırsa iç altyapı gizli dizileri döndürülür. Dış ağ altyapısı uç noktaları döndürürken, **Start-SecretRotation** , Azure Stack ortamının ayrıcalıklı uç noktası oturumunun **oturum** parametresi olarak geçirildiği bir **Invoke-Command** betik bloğu ile çalıştırılmalıdır.

### <a name="parameters"></a>Parametreler

| Parametre | Tür | Gereklidir | Yerine | Varsayılan | Açıklama |
| -- | -- | -- | -- | -- | -- |
| `PfxFilesPath` | Dize  | False  | Adlandırılır  | Hiçbiri  | Tüm dış ağ uç noktası sertifikalarını içeren **\Certificates** dizininin FileShare yolu. Yalnızca dış gizlilikler döndürme sırasında gereklidir. Son dizin **\ sertifikalar**olmalıdır. |
| `CertificatePassword` | SecureString | False  | Adlandırılır  | Hiçbiri  | -PfXFilesPath içinde belirtilen tüm sertifikaların parolası. Dış gizlilikler döndürüldüğünde PfxFilesPath sağlanırsa gerekli değer. |
| `Internal` | Dize | False | Adlandırılır | Hiçbiri | İç altyapının gizli dizilerini döndürmek için bir Azure Stack işleci her zaman kullanılması gerekir. |
| `PathAccessCredential` | PSCredential | False  | Adlandırılır  | Hiçbiri  | Tüm dış ağ uç noktası sertifikalarını içeren **\Certificates** dizininin dosya paylaşımının PowerShell kimlik bilgileri. Yalnızca dış gizlilikler döndürme sırasında gereklidir.  |
| `ReRun` | SwitchParameter | False  | Adlandırılır  | Hiçbiri  | Yeniden çalıştırma işlemi, başarısız bir denemeden sonra her zaman gizli dizi rotasyonda yeniden denenmelidir. |

### <a name="examples"></a>Örnekler

#### <a name="rotate-only-internal-infrastructure-secrets"></a>Yalnızca iç altyapı gizli dizilerini döndür

Bunun, Azure Stack [ortamınızın ayrıcalıklı uç noktası](azure-stack-privileged-endpoint.md)aracılığıyla çalıştırılması gerekir.

```powershell
PS C:\> Start-SecretRotation -Internal
```

Bu komut, iç ağ Azure Stack kullanıma sunulan tüm altyapı gizli dizilerini döndürür.

#### <a name="rotate-only-external-infrastructure-secrets"></a>Yalnızca dış altyapı gizli dizilerini döndür  

```powershell
# Create a PEP Session
winrm s winrm/config/client '@{TrustedHosts= "<IpOfERCSMachine>"}'
$PEPCreds = Get-Credential
$PEPSession = New-PSSession -ComputerName <IpOfERCSMachine> -Credential $PEPCreds -ConfigurationName "PrivilegedEndpoint"

# Create Credentials for the fileshare
$CertPassword = ConvertTo-SecureString "<CertPasswordHere>" -AsPlainText -Force
$CertShareCreds = Get-Credential
$CertSharePath = "<NetworkPathOfCertShare>"
# Run Secret Rotation
Invoke-Command -Session $PEPSession -ScriptBlock {  
    Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCreds -CertificatePassword $using:CertPassword
}
Remove-PSSession -Session $PEPSession
```

Bu komut, Azure Stack dış ağ altyapısı uç noktaları için kullanılan TLS sertifikalarını döndürür.

#### <a name="rotate-internal-and-external-infrastructure-secrets-pre-1811-only"></a>İç ve dış altyapı gizli dizilerini döndürme (yalnızca**1811 öncesi** )

> [!IMPORTANT]
> Bu komut yalnızca, döndürme iç ve dış sertifikalara bölündüğü için Azure Stack **ön 1811** için geçerlidir.
>
> ***1811 +* ' dan hem iç hem de dış sertifikaları daha fazla!!! döndüremezsiniz**

```powershell
# Create a PEP Session
winrm s winrm/config/client '@{TrustedHosts= "<IpOfERCSMachine>"}'
$PEPCreds = Get-Credential
$PEPSession = New-PSSession -ComputerName <IpOfERCSMachine> -Credential $PEPCreds -ConfigurationName "PrivilegedEndpoint"

# Create Credentials for the fileshare
$CertPassword = ConvertTo-SecureString "<CertPasswordHere>" -AsPlainText -Force
$CertShareCreds = Get-Credential
$CertSharePath = "<NetworkPathOfCertShare>"
# Run Secret Rotation
Invoke-Command -Session $PEPSession -ScriptBlock {
    Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCreds -CertificatePassword $using:CertPassword
}
Remove-PSSession -Session $PEPSession
```

Bu komut, iç ağ Azure Stack ve Azure Stack dış ağ altyapısı uç noktaları için kullanılan TLS sertifikalarının yanı sıra kullanıma sunulan tüm altyapı gizli dizilerini döndürür. Start-SecretRotation, yığın tarafından oluşturulan tüm gizli dizileri döndürür ve sağlanmış sertifikalar olduğundan, dış uç nokta sertifikaları da döndürülecektir.  

## <a name="update-the-baseboard-management-controller-bmc-credential"></a>Temel kart yönetim denetleyicisi (BMC) kimlik bilgilerini güncelleştirme

Temel kart yönetim denetleyicisi (BMC), sunucularınızın fiziksel durumunu izler. BMC 'nin Kullanıcı hesabı adını ve parolasını güncelleştirme yönergeleri için özgün ekipman üreticisi (OEM) donanım satıcınıza başvurun. 

>[!NOTE]
> OEM 'niz, ek yönetim uygulamaları sağlayabilir. Diğer yönetim uygulamalarının Kullanıcı adını veya parolasını güncelleştirmek, BMC Kullanıcı adı veya parola üzerinde hiçbir etkisi olmaz.   

1. Azure Stack fiziksel sunucularındaki BMC 'yi, OEM yönergelerinizi izleyerek güncelleştirin. Ortamınızdaki her bir BMC için Kullanıcı adı ve parola aynı olmalıdır. BMC Kullanıcı adları 16 karakteri aşamaz.
2. Azure Stack oturumlarında ayrıcalıklı bir uç nokta açın. Yönergeler için, bkz. [Azure Stack ayrıcalıklı uç noktası kullanma](azure-stack-privileged-endpoint.md).
3. PowerShell istemizin **[IP adresi veya ERCS VM Name] olarak değiştirildikten sonra: ps >** veya **[AZS-ERCS01]: PS >** , ortama bağlı olarak, `Invoke-Command` çalıştırarak `Set-BmcCredential` ' yi çalıştırın. Ayrıcalıklı uç nokta oturum değişkeninizi parametre olarak geçirin. Örnek:

    ```powershell
    # Interactive Version
    $PEPIp = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PEPCreds = Get-Credential "<Domain>\CloudAdmin" -Message "PEP Credentials"
    $NewBmcPwd = Read-Host -Prompt "Enter New BMC password" -AsSecureString
    $NewBmcUser = Read-Host -Prompt "Enter New BMC user name"

    $PEPSession = New-PSSession -ComputerName $PEPIp -Credential $PEPCreds -ConfigurationName "PrivilegedEndpoint"

    Invoke-Command -Session $PEPSession -ScriptBlock {
        # Parameter BmcPassword is mandatory, while the BmcUser parameter is optional.
        Set-BmcCredential -BmcPassword $using:NewBmcPwd -BmcUser $using:NewBmcUser
    }
    Remove-PSSession -Session $PEPSession
    ```

    Statik PowerShell sürümünü parolalarla birlikte kod satırları olarak da kullanabilirsiniz:

    ```powershell
    # Static Version
    $PEPIp = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PEPUser = "<Privileged Endpoint user for example Domain\CloudAdmin>"
    $PEPPwd = ConvertTo-SecureString "<Privileged Endpoint Password>" -AsPlainText -Force
    $PEPCreds = New-Object System.Management.Automation.PSCredential ($PEPUser, $PEPPwd)
    $NewBmcPwd = ConvertTo-SecureString "<New BMC Password>" -AsPlainText -Force
    $NewBmcUser = "<New BMC User name>"

    $PEPSession = New-PSSession -ComputerName $PEPIp -Credential $PEPCreds -ConfigurationName "PrivilegedEndpoint"

    Invoke-Command -Session $PEPSession -ScriptBlock {
        # Parameter BmcPassword is mandatory, while the BmcUser parameter is optional.
        Set-BmcCredential -BmcPassword $using:NewBmcPwd -BmcUser $using:NewBmcUser
    }
    Remove-PSSession -Session $PEPSession
    ```

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack güvenliği hakkında daha fazla bilgi edinin](azure-stack-security-foundations.md)
