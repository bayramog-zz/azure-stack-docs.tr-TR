---
title: Azure Stack için Azure kaydını doğrula | Microsoft Docs
description: Azure kaydını doğrulamak için Azure Stack hazırlık denetleyicisi ' ni kullanın.
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
ms.date: 06/23/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 03/23/2019
ms.openlocfilehash: 9c4ddec0606556290e55850a9081c6665f2524d1
ms.sourcegitcommit: 3af71025e85fc53ce529de2f6a5c396b806121ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71159581"
---
# <a name="validate-azure-registration"></a>Azure kaydı doğrula

Azure aboneliğinizin Azure Stack kullanmaya hazır olduğunu doğrulamak için Azure Stack hazırlık Denetleyicisi aracı 'nı (**Azsreadinesschecker**) kullanın. Azure Stack dağıtımına başlamadan önce kayıt işlemini doğrulayın. Hazırlık denetleyicisi şunları doğrular:

- Kullandığınız Azure aboneliği desteklenen bir türdür. Abonelikler bir bulut hizmeti sağlayıcısı (CSP) veya Kurumsal Anlaşma (EA) olmalıdır.
- Aboneliğinizi Azure 'a kaydetmek için kullandığınız hesap Azure 'da oturum açabilir ve bir abonelik sahibidir.

Azure Stack kaydı hakkında daha fazla bilgi için bkz. [Azure Ile kayıt Azure Stack](azure-stack-registration.md).

## <a name="get-the-readiness-checker-tool"></a>Hazırlık Denetleyicisi aracını edinme

**Azsreadinesschecker** 'ın en son sürümünü [PowerShell Galerisi](https://aka.ms/AzsReadinessChecker)indirin.  

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki Önkoşullar gereklidir:

### <a name="the-computer-on-which-the-tool-runs"></a>Aracın çalıştığı bilgisayar

- Internet bağlantısı ile Windows 10 veya Windows Server 2016.
- PowerShell 5,1 veya sonraki bir sürümü. Sürümünüzü denetlemek için aşağıdaki PowerShell cmdlet 'ini çalıştırın ve ardından **birincil** ve **İkincil** sürümleri gözden geçirin:  

  ```powershell
  $PSVersionTable.PSVersion
  ```

- [Azure Stack için yapılandırılmış PowerShell](azure-stack-powershell-install.md).
- [Microsoft Azure Stack hazırlık denetleyicisi](https://aka.ms/AzsReadinessChecker)'nin en son sürümü.  

### <a name="azure-active-directory-environment"></a>Azure Active Directory ortamı

- Azure Stack birlikte kullanacağınız Azure aboneliğinin sahibi olan bir hesabın kullanıcı adını ve parolasını belirler.  
- Kullanacağınız Azure aboneliğinin abonelik KIMLIĞINI belirler.
- Kullanacağınız **AzureEnvironment** öğesini belirler. Kullandığınız Azure aboneliğine bağlı olarak, ortam adı parametresi için desteklenen değerler **Azurecsesli**, **AzureChinaCloud**veya **AzureUSGovernment**' dir.

## <a name="steps-to-validate-azure-registration"></a>Azure kaydını doğrulama adımları

1. Önkoşulları karşılayan bir bilgisayarda, yükseltilmiş bir PowerShell istemi açın ve ardından **Azsreadinesscontroller**' ı yüklemek için şu komutu çalıştırın:

   ```powershell
   Install-Module Microsoft.AzureStack.ReadinessChecker -Force
   ```

2. PowerShell komut isteminde, abonelik sahibi olan hesap olarak ayarlamak `$registrationCredential` için aşağıdaki komutu çalıştırın. Hesap `subscriptionowner@contoso.onmicrosoft.com` ve kiracı adınızla değiştirin:

   ```powershell
   $registrationCredential = Get-Credential subscriptionowner@contoso.onmicrosoft.com -Message "Enter Credentials for Subscription Owner"
   ```

   > [!NOTE]
   > CSP olarak, paylaşılan hizmetler veya ıUR aboneliği kullanılırken, ilgili Azure AD 'den bir kullanıcının kimlik bilgilerini sağlamanız gerekir. Genellikle bu şuna benzer `subscriptionowner@iurcontoso.onmicrosoft.com`olacaktır. Bu kullanıcının, önceki adımda açıklandığı gibi uygun kimlik bilgilerine sahip olması gerekir.

3. PowerShell komut isteminde, kullanılacak Azure aboneliği olarak ayarlamak `$subscriptionID` için aşağıdakileri çalıştırın. Kendi `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` abonelik Kimliğinizle değiştirin:

   ```powershell
   $subscriptionID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
   ```

4. Aboneliğinizi doğrulamaya başlamak için PowerShell komut isteminde aşağıdaki komutu çalıştırın:

   - **Azurecyüksek**, `AzureEnvironment` **AzureGermanCloud**veya **AzureChinaCloud**olarak değerini belirtin.  
   - Azure Active Directory yöneticinize ve Azure Active Directory kiracı adınızı sağlayın.

   ```powershell
   Invoke-AzsRegistrationValidation -RegistrationAccount $registrationCredential -AzureEnvironment AzureCloud -RegistrationSubscriptionID $subscriptionID
   ```

5. Araç çalıştıktan sonra çıktıyı gözden geçirin. Durumun hem oturum açma hem de kayıt gereksinimleri için doğru olduğunu onaylayın. Başarılı doğrulama çıktısı aşağıdaki örneğe benzer şekilde görünür:

   ```shell
   Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
   Checking Registration Requirements: OK
   Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
   Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
   Invoke-AzsRegistrationValidation Completed
   ```

## <a name="report-and-log-file"></a>Rapor ve günlük dosyası

Doğrulamanın her çalıştırılışında, sonuçları **Azsreadinesscontroller. log** ve **AzsReadinessCheckerReport. JSON**dosyasına kaydeder. Bu dosyaların konumu, PowerShell 'deki doğrulama sonuçlarıyla birlikte görüntülenir.

Bu dosyalar, Azure Stack dağıtmadan veya doğrulama sorunlarını araştırmadan önce doğrulama durumunu paylaşmanıza yardımcı olabilir. Her iki dosya da sonraki doğrulama denetiminin sonuçlarını kalıcı hale getir. Rapor, kimlik yapılandırması için dağıtım ekibi onayını sağlar. Günlük dosyası, dağıtımınızın veya destek ekibinizin doğrulama sorunlarını araştırmanıza yardımcı olabilir.

Varsayılan olarak, her iki dosya da **C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.JSON**yazılır.  

- Farklı bir rapor konumu belirtmek için komut satırını Çalıştır ' ın sonundaki **-OutputPath**  ***&lt;&gt; yol*** parametresini kullanın.
- **AzsReadinessCheckerReport. JSON**' dan aracın önceki çalıştırmaları hakkındaki bilgileri temizlemek için Çalıştır komutunun sonundaki **-cleanreport** parametresini kullanın.

Daha fazla bilgi için bkz. [Azure Stack doğrulama raporu](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Doğrulama sorunları

Doğrulama denetimi başarısız olursa, PowerShell penceresinde hata hakkındaki ayrıntılar görüntülenir. Araç ayrıca bilgileri **Azsreadinesschecker. log** dosyasına kaydeder.

Aşağıdaki örnekler, yaygın doğrulama hatalarıyla ilgili rehberlik sağlar:

### <a name="user-must-be-an-owner-of-the-subscription"></a>Kullanıcının aboneliğin sahibi olması gerekir

```shell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: Fail
Error Details for registration account admin@contoso.onmicrosoft.com:
The user admin@contoso.onmicrosoft.com is role(s) Reader for subscription 3f961d1c-d1fb-40c3-99ba-44524b56df2d. User must be an owner of the subscription to be used for registration.
Additional help URL https://aka.ms/AzsRemediateRegistration

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
```

**Neden** -hesap, Azure aboneliğinin Yöneticisi değil.

**Çözüm** -Azure Stack dağıtımından kullanım için faturalandırılacak Azure aboneliğinin Yöneticisi olan bir hesap kullanın.

### <a name="expired-or-temporary-password"></a>Zaman aşımına uğradı veya geçici parola

```shell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: Fail
Error Details for registration account admin@contoso.onmicrosoft.com:
Checking Registration failed with: Retrieving TenantId for subscription [subscription ID] using account admin@contoso.onmicrosoft.com failed with AADSTS50055: Force Change Password.
Trace ID: [Trace ID]
Correlation ID: [Correlation ID]
Timestamp: 2018-10-22 11:16:56Z: The remote server returned an error: (401) Unauthorized.

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
```

**Neden** -parolanın geçerliliği aşıldığı veya geçici olması nedeniyle hesap oturum açılamıyor.

**Çözüm** -PowerShell içinde, aşağıdaki komutu çalıştırın ve parolayı sıfırlamak için istemleri izleyin.

```powershell
Login-AzureRMAccount
```

Alternatif olarak, hesap sahibi olarak [Azure Portal](https://portal.azure.com) oturum açın ve Kullanıcı parolayı değiştirmeye zorlanır.

### <a name="unknown-user-type"></a>Bilinmeyen Kullanıcı türü  

```shell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: Fail
Error Details for registration account admin@contoso.onmicrosoft.com:
Checking Registration failed with: Retrieving TenantId for subscription <subscription ID> using <account> failed with unknown_user_type: Unknown User Type

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
```

**Neden** -hesap, belirtilen Azure Active Directory ortamında oturum açılamıyor. Bu örnekte, **AzureChinaCloud** **AzureEnvironment**olarak belirtilir.  

**Çözüm** -hesabın belirtilen Azure ortamı için geçerli olduğunu onaylayın. PowerShell 'de, hesabın belirli bir ortam için geçerli olduğunu doğrulamak üzere aşağıdaki komutu çalıştırın:

```powershell
Login-AzureRmAccount -EnvironmentName AzureChinaCloud
```

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure kimliğini doğrula](azure-stack-validate-identity.md)
- [Hazırlık raporunu görüntüleme](azure-stack-validation-report.md)
- [Genel Azure Stack tümleştirme konuları](azure-stack-datacenter-integration.md)
