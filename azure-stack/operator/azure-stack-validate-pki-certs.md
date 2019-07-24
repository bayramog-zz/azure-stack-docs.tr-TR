---
title: Azure Stack tümleşik sistemler dağıtımı için ortak anahtar altyapısı sertifikalarının Azure Stack doğrulama | Microsoft Docs
description: Azure Stack tümleşik sistemler için Azure Stack PKI sertifikalarının nasıl doğrulanacağını açıklar. Azure Stack sertifika Denetleyicisi aracının kullanımını içerir.
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
ms.date: 07/23/2019
ms.author: mabrigg
ms.reviewer: ppacent
ms.lastreviewed: 01/08/2019
ms.openlocfilehash: 8d929a3b1fd67f3ec73137b2e1a87f594ea5b544
ms.sourcegitcommit: b95983e6e954e772ca5267304cfe6a0dab1cfcab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68417833"
---
# <a name="validate-azure-stack-pki-certificates"></a>Azure Stack PKI sertifikalarını doğrulama

Bu makalede açıklanan Azure Stack hazırlık Denetleyicisi aracı [PowerShell Galerisi](https://aka.ms/AzsReadinessChecker)sunulmaktadır. Aracı, [oluşturulan PKI sertifikalarının](azure-stack-get-pki-certs.md) dağıtım öncesi için uygun olduğunu doğrulamak için kullanabilirsiniz. Gerektiğinde sertifikaları test etmek ve yeniden vermek için yeterli zaman bırakarak sertifikaları doğrulayın.

Hazırlık Denetleyicisi aracı aşağıdaki sertifika doğrulamaları gerçekleştirir:

- **PFX oku**  
    Geçerli PFX dosyasını, parolayı doğru parolayı ve genel bilgilerin parola tarafından korunup korunmadığını denetler. 
- **İmza algoritması**  
    İmza algoritmasının SHA1 olmadığını denetler.
- **Özel anahtar**  
    Özel anahtarın mevcut olduğunu ve yerel makine özniteliğiyle birlikte verildiğini denetler. 
- **Sertifika zinciri**  
    Otomatik olarak imzalanan sertifikalara yönelik bir denetim dahil olmak üzere, sertifika zincirini denetler.
- **DNS adları**  
    SAN 'ın her bir uç nokta için ilgili DNS adlarını içerip içermediğini denetler veya bir destekleyici joker karakter varsa.
- **Anahtar kullanımı**  
    Anahtar kullanımının dijital imza ve anahtar şifreleme içerip içermediğini denetler ve gelişmiş anahtar kullanımı sunucu kimlik doğrulaması ve istemci kimlik doğrulaması içerir.
- **Anahtar boyutu**  
    Anahtar boyutunun 2048 veya daha büyük olup olmadığını denetler.
- **Zincir sırası**  
    Diğer sertifikaların sırasını, siparişin doğru olduğunu doğrulayan kontrol eder.
- **Diğer sertifikalar**  
    PFX 'de, ilgili yaprak sertifikası ve zinciri dışında başka bir sertifika paketlenmiş olmadığından emin olun.
- **Profil yok**  
    Yeni bir kullanıcının bir kullanıcı profili yüklenmeden PFX verilerini yükleyip yükleyemediğini denetler. Bu, sertifika Bakımı sırasında gMSA hesaplarının davranışını göz önyükler.

> [!IMPORTANT]  
> PKI sertifikası bir PFX dosyasıdır ve parola, hassas bilgiler olarak değerlendirilmelidir.

## <a name="prerequisites"></a>Önkoşullar

Bir Azure Stack dağıtımı için PKI sertifikalarını doğrulamadan önce sisteminizin aşağıdaki önkoşulları karşılaması gerekir:

- Microsoft Azure Stack hazırlık denetleyicisi
- SSL sertifikaları, [hazırlama yönergelerinden](azure-stack-prepare-pki-certs.md) sonra verildi
- DeploymentData. JSON
- Windows 10 veya Windows Server 2016

## <a name="perform-core-services-certificate-validation"></a>Çekirdek Hizmetleri sertifika doğrulamasını gerçekleştirme

Dağıtım ve gizli anahtar döndürme için Azure Stack PKI sertifikalarını doğrulamak ve doğrulamak üzere bu adımları kullanın:

1. Aşağıdaki cmdlet 'i çalıştırarak **Azsreadinesschecker** 'Yi bir PowerShell isteminden (5,1 veya üzeri) çalıştırın:

    ```powershell  
        Install-Module Microsoft.AzureStack.ReadinessChecker -force 
    ```

2. Sertifika dizin yapısını oluşturun. Aşağıdaki örnekte, tercih ettiğiniz yeni bir dizin `<c:\certificates>` yoluna geçiş yapabilirsiniz.
    ```powershell  
    New-Item C:\Certificates -ItemType Directory
    
    $directories = 'ACSBlob', 'ACSQueue', 'ACSTable', 'Admin Extension Host', 'Admin Portal', 'ARM Admin', 'ARM Public', 'KeyVault', 'KeyVaultInternal', 'Public Extension Host', 'Public Portal'
    
    $destination = 'c:\certificates'
    
    $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}
    ```
    
    > [!Note]  
    > Kimlik sisteminiz olarak AD FS kullanıyorsanız AD FS ve grafik gereklidir. Örneğin:
    >
    > ```powershell  
    > $directories = 'ACSBlob', 'ACSQueue', 'ACSTable', 'ADFS', 'Admin Extension Host', 'Admin Portal', 'ARM Admin', 'ARM Public', 'Graph', 'KeyVault', 'KeyVaultInternal', 'Public Extension Host', 'Public Portal'
    > ```
    
     - Sertifika (ler) i önceki adımda oluşturulan uygun dizinlere yerleştirin. Örneğin:  
        - `c:\certificates\ACSBlob\CustomerCertificate.pfx`
        - `c:\certificates\Admin Portal\CustomerCertificate.pfx`
        - `c:\certificates\ARM Admin\CustomerCertificate.pfx`

3. PowerShell penceresinde, **RegionName** ve **FQDN** değerlerini Azure Stack ortamına uygun şekilde değiştirin ve aşağıdakileri çalıştırın:

    ```powershell  
    $pfxPassword = Read-Host -Prompt "Enter PFX Password" -AsSecureString 

    Invoke-AzsCertificateValidation -CertificatePath c:\certificates -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD  
    ```

4. Çıktıyı denetleyin ve tüm sertifikalar tüm testleri iletir. Örneğin:

```powershell
Invoke-AzsCertificateValidation v1.1809.1005.1 started.
Testing: ARM Public\ssl.pfx
Thumbprint: 7F6B27****************************E9C35A
    PFX Encryption: OK
    Signature Algorithm: OK
    DNS Names: OK
    Key Usage: OK
    Key Size: OK
    Parse PFX: OK
    Private Key: OK
    Cert Chain: OK
    Chain Order: OK
    Other Certificates: OK
Testing: Admin Extension Host\ssl.pfx
Thumbprint: A631A5****************************35390A
    PFX Encryption: OK
    Signature Algorithm: OK
    DNS Names: OK
    Key Usage: OK
    Key Size: OK
    Parse PFX: OK
    Private Key: OK
    Cert Chain: OK
    Chain Order: OK
    Other Certificates: OK
Testing: Public Extension Host\ssl.pfx
Thumbprint: 4DBEB2****************************C5E7E6
    PFX Encryption: OK
    Signature Algorithm: OK
    DNS Names: OK
    Key Usage: OK
    Key Size: OK
    Parse PFX: OK
    Private Key: OK
    Cert Chain: OK
    Chain Order: OK
    Other Certificates: OK

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsCertificateValidation Completed
```

### <a name="known-issues"></a>Bilinen sorunlar

**Belirti**: Testler atlandı

**Neden**: AzsReadinessChecker bir bağımlılık karşılanmazsa belirli testleri atlar:

 - Sertifika zinciri başarısız olursa diğer sertifikalar atlanır.

    ```powershell  
    Testing: ACSBlob\singlewildcard.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: Fail
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: Skipped
    Details:
    The certificate records '*.east.azurestack.contoso.com' do not contain a record that is valid for '*.blob.east.azurestack.contoso.com'. Please refer to the documentation for how to create the required certificate file.
    The Other Certificates check was skipped because Cert Chain and/or DNS Names failed. Follow the guidance to remediate those issues and recheck. 
    Detailed log can be found C:\AzsReadinessChecker\CertificateValidation\CertChecker.log

    Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
    Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
    Invoke-AzsCertificateValidation Completed
    ```

**Çözüm**: Her sertifika için her bir test kümesi altındaki Ayrıntılar bölümünde bulunan araç kılavuzunu izleyin.

## <a name="perform-platform-as-a-service-certificate-validation"></a>Hizmet sertifikası doğrulaması olarak platform gerçekleştirme

SQL/MySQL veya App Services dağıtımları planlansa, hizmet olarak platform (PaaS) sertifikalarına yönelik Azure Stack PKI sertifikalarını hazırlamak ve doğrulamak için bu adımları kullanın.

1.  Aşağıdaki cmdlet 'i çalıştırarak **Azsreadinesschecker** 'Yi bir PowerShell isteminden (5,1 veya üzeri) çalıştırın:

    ```powershell  
      Install-Module Microsoft.AzureStack.ReadinessChecker -force
    ```

2.  Doğrulamaya ihtiyaç duyan her PaaS sertifikasına yönelik yollar ve parola içeren iç içe bir Hashtable oluşturun. PowerShell penceresinde şunu çalıştırın:

    ```powershell  
        $PaaSCertificates = @{
        'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        }
    ```

3.  Doğrulamayı başlatmak için **RegionName** ve **FQDN** değerlerini Azure Stack ortamınızla eşleşecek şekilde değiştirin. Ardından şunu çalıştırın:

    ```powershell  
    Invoke-AzsCertificateValidation -PaaSCertificates $PaaSCertificates -RegionName east -FQDN azurestack.contoso.com 
    ```
4.  Çıktının ve tüm sertifikaların tüm testleri geçirmediğinden emin olun.

    ```powershell
    Invoke-AzsCertificateValidation v1.0 started.
    Thumbprint: 95A50B****************************FA6DDA
        Signature Algorithm: OK
        Parse PFX: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Thumbprint: EBB011****************************59BE9A
        Signature Algorithm: OK
        Parse PFX: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Thumbprint: 76AEBA****************************C1265E
        Signature Algorithm: OK
        Parse PFX: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Thumbprint: 8D6CCD****************************DB6AE9
        Signature Algorithm: OK
        Parse PFX: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
    ```

## <a name="certificates"></a>Sertifikalar

| Dizin | Sertifika |
| ---    | ----        |
| acsBlob | wildcard_blob_\<Region > _\<externalfqdn > |
| ACSQueue  |  wildcard_queue_\<Region > _\<externalfqdn > |
| ACSTable  |  wildcard_table_\<Region > _\<externalfqdn > |
| Yönetici uzantısı ana bilgisayarı  |  wildcard_adminhosting_\<Region > _\<externalfqdn > |
| Yönetici portalı  |  adminportal_\<Region > _\<externalfqdn > |
| ARM Yöneticisi  |  adminmanagement_\<Region > _\<externalfqdn > |
| ARM genel  |  management_\<Region > _\<externalfqdn > |
| KeyVault  |  wildcard_vault_\<Region > _\<externalfqdn > |
| Keyvaultınternal  |  wildcard_adminvault_\<Region > _\<externalfqdn > |
| Ortak uzantı Konağı  |  wildcard_hosting_\<Region > _\<externalfqdn > |
| Ortak Portal  |  portal_\<Region > _\<externalfqdn > |

## <a name="using-validated-certificates"></a>Doğrulanan sertifikaları kullanma

Sertifikalarınız AzsReadinessChecker tarafından doğrulandıktan sonra bunları Azure Stack dağıtımınızda veya Azure Stack gizli dizi dönüştürme için kullanmaya hazır olursunuz. 

 - Dağıtım için sertifikalarınızı, [Azure Stack PKI gereksinimleri belgelerinde](azure-stack-pki-certs.md)belirtilen şekilde dağıtım konağına kopyalayabilmeleri için dağıtım Mühendisinize güvenli bir şekilde aktarın.
 - Gizli anahtar döndürmesi için, [Azure Stack gizli döndürme belgelerini](azure-stack-rotate-secrets.md)izleyerek Azure Stack ortamınızın ortak altyapı uç noktaları için eski sertifikaları güncelleştirmek üzere sertifikaları kullanabilirsiniz.
 - PaaS hizmetleri için, [Azure Stack belgelerindeki hizmetleri sunma genel görünümünü](azure-stack-offer-services-overview.md)IZLEYEREK Azure Stack SQL, MySQL ve App Services kaynak sağlayıcılarını yüklemek için sertifikaları kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Veri merkezi kimlik tümleştirmesi](azure-stack-integrate-identity.md)
