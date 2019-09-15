---
title: Start-AzsReadinessChecker cmdlet başvurusu | Microsoft Docs
description: Azure Stack hazırlık denetleyicisi modülü için PowerShell cmdlet yardımı.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/13/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 05/09/2019
ms.openlocfilehash: 9e92101b6d00da397359ed25e8682f18305f5a83
ms.sourcegitcommit: 245a4054a52e54d5989d6148fbbe386e1b2aa49c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70974739"
---
# <a name="start-azsreadinesschecker-cmdlet-reference"></a>Start-AzsReadinessChecker cmdlet başvurusu

Birimi **Microsoft. AzureStack. ReadinessChecker**

Bu modül yalnızca tek bir cmdlet içerir. Cmdlet 'i Azure Stack için bir veya daha fazla dağıtım öncesi veya hizmet öncesi işlev gerçekleştirir.

## <a name="syntax"></a>Sözdizimi

```powershell
Start-AzsReadinessChecker
       [-CertificatePath <String>]
       -PfxPassword <SecureString>
       -RegionName <String>
       -FQDN <String>
       -IdentitySystem <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       [-CertificatePath <String>]
       -PfxPassword <SecureString>
       -DeploymentDataJSONPath <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -PaaSCertificates <Hashtable>
       -DeploymentDataJSONPath <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -PaaSCertificates <Hashtable>
       -RegionName <String>
       -FQDN <String>
       -IdentitySystem <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -RegionName <String>
       -FQDN <String>
       -IdentitySystem <String>
       -Subject <OrderedDictionary>
       -RequestType <String>
       [-IncludePaaS]
       -OutputRequestPath <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -PfxPassword <SecureString>
       -PfxPath <String>
       -ExportPFXPath <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -AADServiceAdministrator <PSCredential>
       -AADDirectoryTenantName <String>
       -IdentitySystem <String>
       -AzureEnvironment <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -AADServiceAdministrator <PSCredential>
       -DeploymentDataJSONPath <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -RegistrationAccount <PSCredential>
       -RegistrationSubscriptionID <Guid>
       -AzureEnvironment <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -RegistrationAccount <PSCredential>
       -RegistrationSubscriptionID <Guid>
       -DeploymentDataJSONPath <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -ReportPath <String>
       [-ReportSections <String>]
       [-Summary]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

## <a name="description"></a>Açıklama

**Start-AzsReadinessChecker** cmdlet 'i sertifikaları, Azure hesaplarını, Azure aboneliklerini ve Azure Active Directory 'YI (AAD) doğrular. Azure Stack dağıtılmadan önce veya gizli döndürme gibi eylemlere Azure Stack önce doğrulamayı çalıştırın. Cmdlet 'i, altyapı sertifikaları için sertifika imzalama istekleri ve isteğe bağlı olarak PaaS sertifikaları oluşturmak için de kullanılabilir. Son olarak, cmdlet ortak paketleme sorunlarını düzeltmek için PFX sertifikalarını yeniden paketleyebilir.

## <a name="examples"></a>Örnekler

### <a name="example-generate-certificate-signing-request"></a>Örnek: sertifika imzalama isteği oluştur

```powershell
$regionName = 'east'
$externalFQDN = 'azurestack.contoso.com'
$subjectHash = [ordered]@{"OU"="AzureStack";"O"="Microsoft";"L"="Redmond";"ST"="Washington";"C"="US"}
Start-AzsReadinessChecker -regionName $regionName -externalFQDN $externalFQDN -subject $subjectHash -IdentitySystem ADFS -requestType MultipleCSR
```

Bu örnekte, `Start-AzsReadinessChecker` bir bölge adı **Doğu** ve **azurestack.contoso.com**dış FQDN 'si olan bir AD FS Azure Stack dağıtımına uygun sertifikalar için birden çok sertifika imzalama isteği (CSR) oluşturulur.

### <a name="example-validate-certificates"></a>Örnek: sertifikaları doğrulama

```powershell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -CertificatePath .\Certificates\ -PfxPassword $password -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD
```

Bu örnekte, güvenlik için PFX parolası gereklidir ve `Start-AzsReadinessChecker` bölge adı **Doğu** ve azurestack.contoso.com dış FQDN 'sine sahip bir AAD dağıtımı için geçerli sertifikalar için göreli klasör **sertifikalarını** denetler.

### <a name="example-validate-certificates-with-deployment-data-deployment-and-support"></a>Örnek: dağıtım verileriyle sertifikaları doğrulama (dağıtım ve destek)

```powershell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -CertificatePath .\Certificates\ -PfxPassword $password -DeploymentDataJSONPath .\deploymentdata.json
```

Bu dağıtımda ve destek örneğinde, güvenlik için PFX parolası gerekir ve `Start-AzsReadinessChecker` kimliğin, bölgenin ve dış FQDN 'nin dağıtımdan okunduğu bir dağıtım için geçerli sertifikalar için göreli klasör **sertifikalarını** denetler dağıtım için veri JSON dosyası oluşturuldu.

### <a name="example-validate-paas-certificates"></a>Örnek: PaaS sertifikalarını doğrulama

```powershell
$PaaSCertificates = @{
    'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
}
Start-AzsReadinessChecker -PaaSCertificates $PaaSCertificates -RegionName east -FQDN azurestack.contoso.com
```

Bu örnekte, her PaaS sertifikasına yönelik yollar ve parolalarla bir Hashtable oluşturulur. Sertifikalar atlanabilir. `Start-AzsReadinessChecker`Her bir PFX yolunun var olduğunu denetler ve **Doğu** ve dış FQDN **azurestack.contoso.com**bölgelerini kullanarak bunları doğrular.

### <a name="example-validate-paas-certificates-with-deployment-data"></a>Örnek: dağıtım verileriyle PaaS sertifikalarını doğrulama

```powershell
$PaaSCertificates = @{
    'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
}
Start-AzsReadinessChecker -PaaSCertificates $PaaSCertificates -DeploymentDataJSONPath .\deploymentdata.json
```

Bu örnekte, her PaaS sertifikasına yönelik yollar ve parolalarla bir Hashtable oluşturulur. Sertifikalar atlanabilir. `Start-AzsReadinessChecker`Her bir PFX yolunun var olduğunu denetler ve dağıtım için oluşturulan dağıtım verileri JSON dosyasından okunan bölgeyi ve dış FQDN 'yi kullanarak doğrular.

### <a name="example-validate-azure-identity"></a>Örnek: Azure kimliğini doğrulama

```powershell
$serviceAdminCredential = Get-Credential -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant e.g. serviceadmin@contoso.onmicrosoft.com"
# Supported values for the <environment name> parameter are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -AzureEnvironment "<environment name>" -AzureDirectoryTenantName azurestack.contoso.com
```

Bu örnekte, hizmet yöneticisi hesabı kimlik bilgileri güvenlik için gereklidir ve `Start-AzsReadinessChecker` Azure hesabının ve AAD 'nin **azurestack.contoso.com**kiracı dizin adına sahip bir AAD dağıtımı için geçerli olduğunu denetler.

### <a name="example-validate-azure-identity-with-deployment-data-deployment-support"></a>Örnek: Azure kimliğini dağıtım verileriyle doğrulama (dağıtım desteği)

```PowerShell
$serviceAdminCredential = Get-Credential -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant e.g. serviceadmin@contoso.onmicrosoft.com"
Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -DeploymentDataJSONPath .\contoso-deploymentdata.json
```

Bu örnekte, hizmet yöneticisi hesabı kimlik bilgileri güvenlik için gereklidir ve Azure hesabının ve `Start-AzsReadinessChecker` AAD 'nin bir AAD dağıtımı için geçerli olduğunu ve bu durumda **azurecı** ve **TenantName** ' in, JSON dağıtım verilerinden okunduğu denetlenir dağıtım için oluşturulan dosya.

### <a name="example-validate-azure-registration"></a>Örnek: Azure kaydını doğrulama

```powershell
$registrationCredential = Get-Credential -Message "Enter Credentials for Subscription Owner e.g. subscriptionowner@contoso.onmicrosoft.com"
$subscriptionID = "<subscription ID"
# Supported values for the <environment name> parameter are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -RegistrationSubscriptionID $subscriptionID -AzureEnvironment "<environment name>"
```

Bu örnekte, abonelik sahibi kimlik bilgileri güvenlik için gereklidir ve `Start-AzsReadinessChecker` Azure Stack kayıt için kullanılabilir olduğundan emin olmak için verilen hesap ve aboneliğe göre doğrulama gerçekleştirir.

### <a name="example-validate-azure-registration-with-deployment-data-deployment-team"></a>Örnek: Azure kaydını dağıtım verileriyle doğrulama (dağıtım ekibi)

```powershell
$registrationCredential = Get-Credential -Message "Enter Credentials for Subscription Owner e.g. subscriptionowner@contoso.onmicrosoft.com"
$subscriptionID = "<subscription ID>"
Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -RegistrationSubscriptionID $subscriptionID -DeploymentDataJSONPath .\contoso-deploymentdata.json
```

Bu örnekte, abonelik sahibi kimlik bilgileri güvenlik için gereklidir ve `Start-AzsReadinessChecker` sonra, Azure Stack kayıt için kullanılabilir olduğundan emin olmak için verilen hesap ve aboneliğe karşı doğrulama gerçekleştirir, burada ek ayrıntılar okunduğunda dağıtım için dağıtım verileri JSON dosyası oluşturuldu.

### <a name="example-importexport-pfx-package"></a>Örnek: PFX paketini içeri/dışarı aktar

```powershell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -PfxPassword $password -PfxPath .\certificates\ssl.pfx -ExportPFXPath .\certificates\ssl_new.pfx
```

Bu örnekte, güvenlik için PFX parolası gerekir. SSL. pfx dosyası yerel makine sertifika deposuna aktarılır, aynı parolayla yeniden aktarılır ve Ssl_new. pfx olarak kaydedilir. Bu yordam, özel bir anahtarın **yerel makine** özniteliği ayarlı olmadığı, sertifika zinciri bozuk, çok ılgısız sertifikaların PFX 'de var olduğu veya Sertifika zincirinin yanlış sırada olduğu durumlarda kullanılır.

### <a name="example-view-validation-report-deployment-and-support"></a>Örnek: doğrulama raporunu görüntüleme (dağıtım ve destek)

```powershell
Start-AzsReadinessChecker -ReportPath Contoso-AzsReadinessReport.json
```

Bu örnekte, dağıtım veya destek ekibi, hazırlık raporunu müşteriden (contoso) alır ve gerçekleştirilen doğrulama yürütmelerinin durumunu görüntülemek `Start-AzsReadinessChecker` için kullanır.

### <a name="example-view-validation-report-summary-for-certificate-validation-only-deployment-and-support"></a>Örnek: yalnızca sertifika doğrulaması için doğrulama raporu özetini görüntüle (dağıtım ve destek)

```powershell
Start-AzsReadinessChecker -ReportPath Contoso-AzsReadinessReport.json -ReportSections Certificate -Summary
```

Bu örnekte, dağıtım veya destek ekibi, hazırlık raporunu müşteriden (contoso) alır ve sertifika doğrulama yürütmelerinin, uygulanan `Start-AzsReadinessChecker` contoso 'nun özetlenen durumunu görüntülemek için kullanır.

## <a name="required-parameters"></a>Gerekli parametreler

### <a name="-regionname"></a>-RegionName

Azure Stack dağıtım bölgesi adını belirtir.

|  |  |
|----------------------------|--------------|
|Şunu yazın:                       |Dize        |
|Yerine                   |Adlandırılır         |
|Varsayılan değer:              |Yok.          |
|İşlem hattı girişini kabul et:      |False         |
|Joker karakterleri kabul et: |False         |

### <a name="-fqdn"></a>-FQDN

**Externalfqdn** ve **externaldomainname**olarak da DIĞER ad dağıtım dış FQDN Azure Stack belirtir.

|  |  |
|----------------------------|--------------|
|Şunu yazın:                       |Dize        |
|Yerine                   |Adlandırılır         |
|Varsayılan değer:              |ExternalFQDN, ExternalDomainName |
|İşlem hattı girişini kabul et:      |False         |
|Joker karakterleri kabul et: |False         |

### <a name="-identitysystem"></a>-Identitysystem

Azure Active Directory ve Active Directory Federasyon Hizmetleri için Azure Stack dağıtım kimliği sistemi geçerli değerlerini, AAD veya ADFS 'yi belirtir.

|  |  |
|----------------------------|--------------|
|Şunu yazın:                       |Dize        |
|Yerine                   |Adlandırılır         |
|Varsayılan değer:              |Yok.          |
|Geçerli değerler:               |' AAD ', ' ADFS '  |
|İşlem hattı girişini kabul et:      |False         |
|Joker karakterleri kabul et: |False         |

### <a name="-pfxpassword"></a>-Pfxparolası

PFX Sertifika dosyalarıyla ilişkili parolayı belirtir.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |SecureString |
|Yerine                   |Adlandırılır    |
|Varsayılan değer:              |Yok.     |
|İşlem hattı girişini kabul et:      |False    |
|Joker karakterleri kabul et: |False    |

### <a name="-paascertificates"></a>-Paascertifikalar

PaaS sertifikalarına yönelik yollar ve parolalar içeren Hashtable 'ı belirtir.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Hashtable |
|Yerine                   |Adlandırılır    |
|Varsayılan değer:              |Yok.     |
|İşlem hattı girişini kabul et:      |False    |
|Joker karakterleri kabul et: |False    |

### <a name="-deploymentdatajsonpath"></a>-DeploymentDataJSONPath

Azure Stack dağıtım verileri JSON yapılandırma dosyasını belirtir. Bu dosya dağıtım için oluşturulmuştur.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Yerine                   |Adlandırılır    |
|Varsayılan değer:              |Yok.     |
|İşlem hattı girişini kabul et:      |False    |
|Joker karakterleri kabul et: |False    |

### <a name="-pfxpath"></a>-PfxPath

Bu araçtaki sertifika doğrulaması tarafından belirtildiği gibi, düzeltilmesi gereken bir içeri/dışarı aktarma yordamı gerektiren sorunlu bir sertifikanın yolunu belirtir.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Yerine                   |Adlandırılır    |
|Varsayılan değer:              |Yok.     |
|İşlem hattı girişini kabul et:      |False    |
|Joker karakterleri kabul et: |False    |

### <a name="-exportpfxpath"></a>-ExportPFXPath  

İçeri/dışarı aktarma yordamından elde edilen PFX dosyası için hedef yolu belirtir.  

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Yerine                   |Adlandırılır    |
|Varsayılan değer:              |Yok.     |
|İşlem hattı girişini kabul et:      |False    |
|Joker karakterleri kabul et: |False    |

### <a name="-subject"></a>-Konu

Sertifika isteği oluşturma konusunun sıralı sözlüğünü belirtir.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |OrderedDictionary   |
|Yerine                   |Adlandırılır    |
|Varsayılan değer:              |Yok.     |
|İşlem hattı girişini kabul et:      |False    |
|Joker karakterleri kabul et: |False    |

### <a name="-requesttype"></a>-RequestType

Sertifika isteğinin SAN türünü belirtir. Geçerli değerler **çoğulcsr**, **tekcsr**'dir.

- **Çoğulcsr** , her hizmet için bir tane olmak üzere birden çok sertifika isteği oluşturur.
- **Singlecsr** tüm hizmetler için bir sertifika isteği oluşturur.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Yerine                   |Adlandırılır    |
|Varsayılan değer:              |Yok.     |
|Geçerli değerler:               |' MultipleCSR ', ' SingleCSR ' |
|İşlem hattı girişini kabul et:      |False    |
|Joker karakterleri kabul et: |False    |

### <a name="-outputrequestpath"></a>-OutputRequestPath

Sertifika istek dosyaları için hedef yolu belirtir. Dizin zaten var olmalıdır.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Yerine                   |Adlandırılır    |
|Varsayılan değer:              |Yok.     |
|İşlem hattı girişini kabul et:      |False    |
|Joker karakterleri kabul et: |False    |

### <a name="-aadserviceadministrator"></a>-AADServiceAdministrator

Azure Stack dağıtımı için kullanılacak AAD hizmeti yöneticisini belirtir.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |PSCredential   |
|Yerine                   |Adlandırılır    |
|Varsayılan değer:              |Yok.     |
|İşlem hattı girişini kabul et:      |False    |
|Joker karakterleri kabul et: |False    |

### <a name="-aaddirectorytenantname"></a>-AADDirectoryTenantName

Azure Stack dağıtımı için kullanılacak AAD adını belirtir.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Yerine                   |Adlandırılır    |
|Varsayılan değer:              |Yok.     |
|İşlem hattı girişini kabul et:      |False    |
|Joker karakterleri kabul et: |False    |

### <a name="-azureenvironment"></a>-AzureEnvironment

Azure Stack dağıtım ve kayıt için kullanılacak hesapları, dizinleri ve abonelikleri içeren Azure hizmetleri örneğini belirtir.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Yerine                   |Adlandırılır    |
|Varsayılan değer:              |Yok.     |
|Geçerli değerler:               |' Azurecyüksek ', ' AzureChinaCloud ', ' AzureUSGovernment ' |
|İşlem hattı girişini kabul et:      |False    |
|Joker karakterleri kabul et: |False    |

### <a name="-registrationaccount"></a>-RegistrationAccount

Azure Stack kaydı için kullanılacak kayıt hesabını belirtir.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Yerine                   |Adlandırılır    |
|Varsayılan değer:              |Yok.     |
|İşlem hattı girişini kabul et:      |False    |
|Joker karakterleri kabul et: |False    |

### <a name="-registrationsubscriptionid"></a>-Registrationsubscriptionıd

Azure Stack kaydı için kullanılacak kayıt abonelik KIMLIĞINI belirtir.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Guid     |
|Yerine                   |Adlandırılır    |
|Varsayılan değer:              |Yok.     |
|İşlem hattı girişini kabul et:      |False    |
|Joker karakterleri kabul et: |False    |

### <a name="-reportpath"></a>-ReportPath

Hazır olma raporunun yolunu belirtir, varsayılan olarak geçerli dizin ve varsayılan rapor adı olur.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Yerine                   |Adlandırılır    |
|Varsayılan değer:              |Tümü      |
|İşlem hattı girişini kabul et:      |False    |
|Joker karakterleri kabul et: |False    |

## <a name="optional-parameters"></a>İsteğe bağlı parametreler

### <a name="-certificatepath"></a>-CertificatePath

Altında yalnızca sertifika gerekli sertifika klasörlerinin bulunduğu yolu belirtir.

AAD kimlik sistemiyle Azure Stack dağıtım için gerekli klasörler şunlardır:

- ACSBlob, ACSQueue, ACSTable, Yönetici portalı, ARM Yöneticisi, ARM genel, Keykasası, Keyvaultınternal, genel Portal

Active Directory Federasyon Hizmetleri (AD FS) Identity System ile Azure Stack dağıtım için gerekli klasörler şunlardır:

- ACSBlob, ACSQueue, ACSTable, ADFS, Yönetici portalı, ARM Yöneticisi, ARM genel, Graf, Keykasası, Keyvaultınternal, genel Portal

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Yerine                   |Adlandırılır    |
|Varsayılan değer:              |.\Certificates |
|İşlem hattı girişini kabul et:      |False    |
|Joker karakterleri kabul et: |False    |

### <a name="-includepaas"></a>-IncludePaaS  

PaaS hizmetleri/ana bilgisayar adlarının sertifika isteğine eklenip eklenmeyeceğini belirtir.

|  |  |
|----------------------------|------------------|
|Şunu yazın:                       |SwitchParameter   |
|Yerine                   |Adlandırılır             |
|Varsayılan değer:              |False             |
|İşlem hattı girişini kabul et:      |False             |
|Joker karakterleri kabul et: |False             |

### <a name="-reportsections"></a>-ReportSections

Rapor özetinin yalnızca gösterilip gösterilmeyeceğini belirtir, ayrıntıları atlar.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Yerine                   |Adlandırılır    |
|Varsayılan değer:              |Tümü      |
|Geçerli değerler:               |' Certificate ', ' AzureRegistration ', ' AzureIdentity ', ' Jobs ', ' All ' |
|İşlem hattı girişini kabul et:      |False    |
|Joker karakterleri kabul et: |False    |

### <a name="-summary"></a>-Özet

Rapor özetinin yalnızca gösterilip gösterilmeyeceğini belirtir, ayrıntıları atlar.

|  |  |
|----------------------------|------------------|
|Şunu yazın:                       |SwitchParameter   |
|Yerine                   |Adlandırılır             |
|Varsayılan değer:              |False             |
|İşlem hattı girişini kabul et:      |False             |
|Joker karakterleri kabul et: |False             |

### <a name="-cleanreport"></a>-CleanReport

Önceki yürütme ve doğrulama geçmişini kaldırır ve doğrulamaları yeni bir rapora yazar.

|  |  |
|----------------------------|------------------|
|Şunu yazın:                       |SwitchParameter   |
|Deyim                    |CF                |
|Yerine                   |Adlandırılır             |
|Varsayılan değer:              |False             |
|İşlem hattı girişini kabul et:      |False             |
|Joker karakterleri kabul et: |False             |

### <a name="-outputpath"></a>-OutputPath

Hazır olma JSON raporu ve ayrıntılı günlük dosyası kaydetmek için özel bir yol belirtir. Yol yoksa, komut dizin oluşturmayı dener.

|  |  |
|----------------------------|------------------|
|Şunu yazın:                       |Dize            |
|Yerine                   |Adlandırılır             |
|Varsayılan değer:              |$ENV: TEMP\AzsReadinessChecker  |
|İşlem hattı girişini kabul et:      |False             |
|Joker karakterleri kabul et: |False             |

### <a name="-confirm"></a>-Onayla

Cmdlet 'ini çalıştırmadan önce onay ister.

|  |  |
|----------------------------|------------------|
|Şunu yazın:                       |SwitchParameter   |
|Deyim                    |CF                |
|Yerine                   |Adlandırılır             |
|Varsayılan değer:              |False             |
|İşlem hattı girişini kabul et:      |False             |
|Joker karakterleri kabul et: |False             |

### <a name="-whatif"></a>-WhatIf

Cmdlet çalıştırılıyorsa ne olacağını gösterir. Cmdlet çalıştırılmadı.

|  |  |
|----------------------------|------------------|
|Şunu yazın:                       |SwitchParameter   |
|Deyim                    |Wi                |
|Yerine                   |Adlandırılır             |
|Varsayılan değer:              |False             |
|İşlem hattı girişini kabul et:      |False             |
|Joker karakterleri kabul et: |False             |
