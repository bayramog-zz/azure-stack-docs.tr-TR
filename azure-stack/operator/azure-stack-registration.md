---
title: Azure Stack tümleşik sistemler için Azure kaydı | Microsoft Docs
description: Çok düğümlü Azure Stack Azure 'a bağlı dağıtımlar için Azure kayıt işlemini açıklar.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2019
ms.author: mabrigg
ms.reviewer: avishwan
ms.lastreviewed: 03/04/2019
ms.openlocfilehash: 27dda5b79525c4a5d1c12c998077bb19cbea0079
ms.sourcegitcommit: 9f4c6e96f60b4c229316e7a4ab6e0e5ef0a9a232
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71061184"
---
# <a name="register-azure-stack-with-azure"></a>Azure ile Azure Stack kaydetme

Azure Stack'in Azure'a kaydedilmesi Azure'dan market öğeleri indirmenize ve Microsoft'a ticaret verileri raporları geri göndermeyi ayarlamanıza olanak tanır. Azure Stack kaydolduktan sonra, kullanım Azure ticareti ' ne bildirilir ve kayıt için kullanılan Azure Faturalandırma abonelik KIMLIĞI altında görebilirsiniz.

Bu makaledeki bilgiler, Azure ile Azure Stack tümleşik sistemleri kaydetmeyi açıklamaktadır. ASDK 'yi Azure ile kaydetme hakkında daha fazla bilgi için, bkz. [Azure Stack kaydolma](../asdk/asdk-register.md) , asdk belgeleri.

> [!IMPORTANT]  
> Market 'teki öğelerin sunumu dahil olmak üzere tam Azure Stack işlevselliği desteklemek için kayıt gereklidir. Ayrıca, Kullandıkça Öde faturalandırma modelini kullanırken kaydolmazsınız Azure Stack lisanslama koşullarını ihlal etmezsiniz. Azure Stack lisanslama modelleri hakkında daha fazla bilgi edinmek için lütfen [satın alma sayfasına](https://azure.microsoft.com/overview/azure-stack/how-to-buy/)bakın.

## <a name="prerequisites"></a>Önkoşullar

Kaydolmadan önce aşağıdaki önkoşulların yerine getirmeniz gerekir:

- Kimlik bilgilerinizi doğrulayın
- PowerShell dil modunu ayarlama
- Azure Stack için PowerShell yükleme
- Azure Stack araçlarını indirin
- Kayıt senaryonuzu belirleme

### <a name="verify-your-credentials"></a>Kimlik bilgilerinizi doğrulayın

Azure ile Azure Stack kaydolmadan önce, şunları yapmanız gerekir:

- Bir Azure aboneliğinin abonelik KIMLIĞI. Kayıt için yalnızca EA, CSP veya CSP paylaşılan hizmetleri abonelikleri desteklenir. CSP 'Lerin [BIR CSP veya APSS aboneliği](azure-stack-add-manage-billing-as-a-csp.md#create-a-csp-or-apss-subscription)kullanıp kullanmayacağınızı karar vermesini gerekir.<br><br>KIMLIĞI almak için Azure 'da oturum açın, **tüm hizmetler**' e tıklayın. Ardından, **genel** kategori altında, **abonelikler**' i seçin, kullanmak Istediğiniz aboneliğe tıklayın ve **temel parçalar** altında abonelik kimliğini bulabilirsiniz.

  > [!Note]  
  > Almanya bulut abonelikleri şu anda desteklenmemektedir.

- Abonelik sahibi olan bir hesabın Kullanıcı adı ve parolası.

- Kullanıcı hesabının Azure aboneliğine erişimi olması ve bu abonelikle ilişkili dizinde kimlik uygulamaları ve hizmet sorumluları oluşturma izinlerine sahip olması gerekir. Azure Stack Azure ile en az ayrıcalıklı yönetim kullanarak kaydetmenizi öneririz. Kayıt için aboneliğinize erişimi sınırlayan özel bir rol tanımı oluşturma hakkında daha fazla bilgi için, bkz. [Azure Stack için bir kayıt rolü oluşturma](azure-stack-registration-role.md).

- Azure Stack kaynak sağlayıcısı kaydedildi (Ayrıntılar için aşağıdaki kayıt Azure Stack kaynak sağlayıcısına bakın bölümüne bakın).

Kayıttan sonra, Azure Active Directory genel yönetici izni gerekli değildir. Ancak, bazı işlemler genel yönetici kimlik bilgilerini gerektirebilir. Örneğin, kaynak sağlayıcısı yükleyici betiği veya izin verilmesi gereken yeni bir özellik. Hesabın genel yönetici izinlerini geçici olarak yeniden başlatabilir ya da *varsayılan sağlayıcı aboneliğinin*sahibi olan ayrı bir genel yönetici hesabı kullanabilirsiniz.

Azure Stack kaydeden Kullanıcı, Azure Active Directory hizmet sorumlusunun sahibidir. Yalnızca Azure Stack kaydeden Kullanıcı Azure Stack kaydını değiştirebilir. Kayıt hizmeti sorumlusunun sahibi olmayan yönetici olmayan bir Kullanıcı Azure Stack kaydetmeyi veya yeniden kaydetmeyi denerse, 403 yanıtıyla karşılaşabilir. 403 yanıtı, kullanıcının işlemi gerçekleştirmek için yeterli izinlere sahip olduğunu gösterir.

Bu gereksinimleri karşılayan bir Azure aboneliğiniz yoksa [burada ücretsiz bir Azure hesabı oluşturabilirsiniz](https://azure.microsoft.com/free/?b=17.06). Kayıt Azure Stack Azure aboneliğinizde hiçbir ücret alınmaz.

> [!NOTE]
> Birden fazla Azure Stack varsa, her Azure Stack kendi aboneliğine kaydetmek en iyi uygulamadır. Bu, kullanımı izlemenize daha kolay hale getirir.

### <a name="powershell-language-mode"></a>PowerShell dil modu

Azure Stack başarıyla kaydolmak için, PowerShell dil modunun **Fulllanguagemode**olarak ayarlanması gerekir.  Geçerli dil modunun tam olarak ayarlandığını doğrulamak için, yükseltilmiş bir PowerShell penceresi açın ve aşağıdaki PowerShell cmdlet 'lerini çalıştırın:

```powershell  
$ExecutionContext.SessionState.LanguageMode
```

Çıktının **Fulllanguagemode**döndürdüğünden emin olun. Başka bir dil modu döndürülürse, kaydın başka bir makinede çalıştırılması veya dil modunun devam etmeden önce **Fulllanguagemode** olarak ayarlanması gerekir.

### <a name="install-powershell-for-azure-stack"></a>Azure Stack için PowerShell yükleme

Azure 'a kaydolmak için Azure Stack için en son PowerShell 'i kullanın.

En son sürüm zaten yüklü değilse bkz. [Azure Stack Için PowerShell 'i yükleme](azure-stack-powershell-install.md).

### <a name="download-the-azure-stack-tools"></a>Azure Stack araçlarını indirin

Azure Stack araçları GitHub deposu, Azure Stack işlevselliğini destekleyen PowerShell modülleri içerir; kayıt işlevselliği dahil. Kayıt işlemi sırasında, Azure Stack örneğinizi Azure 'a kaydetmek için Azure Stack araçları deposunda bulunan **Registerwithazure. psm1** PowerShell modülünü içeri aktarıp kullanmanız gerekir.

En son sürümü kullandığınızdan emin olmak için Azure Stack araçlarının var olan sürümlerini silmeniz ve Azure 'a kaydolmadan önce [GitHub 'dan en son sürümü indirmeniz](azure-stack-powershell-download.md) gerekir.

### <a name="determine-your-registration-scenario"></a>Kayıt senaryonuzu belirleme

Azure Stack dağıtımınız *bağlı* veya *bağlantısı kesilmiş*olabilir.

- **Yapıldı**  
 Bağlı, Internet 'e ve Azure 'a bağlanabilmeleri için Azure Stack dağıttığınız anlamına gelir. Kimlik deponuzda Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) (AD FS) sahip olursunuz. Bağlı bir dağıtım ile iki faturalandırma modeli arasından seçim yapabilirsiniz: Kullandıkça Öde veya kapasite tabanlı.
  - [**Kullandıkça Öde** faturalandırma modelini kullanarak Azure ile bağlı bir Azure Stack kaydetme](#register-connected-with-pay-as-you-go-billing)
  - [**Kapasite** faturalandırma modelini kullanarak Azure ile bağlı bir Azure Stack kaydetme](#register-connected-with-capacity-billing)

- **Bağlantınızı**  
 Azure dağıtım bağlantısı kesildi seçeneğiyle, Azure Stack Internet bağlantısı olmadan dağıtabilir ve kullanabilirsiniz. Ancak, bağlantısı kesilen bir dağıtımla AD FS kimlik deposu ve kapasite tabanlı faturalandırma modeliyle sınırlı olursunuz.
  - [**Kapasite** faturalandırma modelini kullanarak bağlantısı kesilen Azure Stack kaydetme](#register-disconnected-with-capacity-billing)

### <a name="determine-a-unique-registration-name-to-use"></a>Kullanılacak benzersiz bir kayıt adı belirleme 

Azure ile Azure Stack kaydettiğinizde, benzersiz bir kayıt adı belirtmeniz gerekir. Azure Stack aboneliğinizi Azure kaydıyla ilişkilendirmenin kolay bir yolu, Azure Stack **bulut kimliğinizi**kullanmaktır. 

> [!NOTE]
> Kapasite tabanlı faturalandırma modelini kullanan Azure Stack kayıtları, [tarihi dolan kaydı silip](azure-stack-registration.md#change-the-subscription-you-use) Azure 'a yeniden kaydolmadığınız müddetçe, bu yıllık aboneliklerden sonra yeniden kaydolurken benzersiz adı değiştirmesi gerekecektir.

Azure Stack dağıtımınızın bulut KIMLIĞINI öğrenmek için, PowerShell 'i ayrıcalıklı uç noktaya erişebilen bir bilgisayarda yönetici olarak açın, aşağıdaki komutları çalıştırın ve **Cloudıd** değerini kaydedin: 

```powershell
Run: Enter-PSSession -ComputerName <privileged endpoint computer name> -ConfigurationName PrivilegedEndpoint
Run: Get-AzureStackStampInformation 
```

## <a name="register-connected-with-pay-as-you-go-billing"></a>Kullandıkça Öde faturalandırmasıyla kaydolun

Kullandıkça Öde faturalandırma modelini kullanarak Azure ile Azure Stack kaydetmek için bu adımları kullanın.

> [!Note]  
> Tüm bu adımların ayrıcalıklı uç noktaya (PEP) erişimi olan bir bilgisayardan çalıştırılması gerekir. PEP hakkında daha fazla bilgi için, bkz. [Azure Stack ayrıcalıklı uç noktası kullanma](azure-stack-privileged-endpoint.md).

Bağlı ortamlar Internet ve Azure 'a erişebilir. Bu ortamlar için Azure Stack kaynak sağlayıcısını Azure ile kaydetmeniz ve ardından Faturalandırma modelinizi yapılandırmanız gerekir.

1. Azure Stack kaynak sağlayıcısını Azure ile kaydetmek için, PowerShell ıSE 'yi yönetici olarak başlatın ve aşağıdaki PowerShell cmdlet 'lerini uygun Azure abonelik türüne ayarlanmış olan **Environmentname** parametresiyle birlikte kullanın (aşağıdaki parametrelere bakın).

2. Azure Stack kaydetmek için kullandığınız Azure hesabını ekleyin. Hesabı eklemek için **Add-AzureRmAccount** cmdlet 'ini çalıştırın. Azure hesabı kimlik bilgilerinizi girmeniz istenir ve hesabınızın yapılandırmasına göre 2 öğeli kimlik doğrulaması kullanmanız gerekebilir.

   ```powershell
   Add-AzureRmAccount -EnvironmentName "<environment name>"
   ```

   | Parametre | Açıklama |  
   |-----|-----|
   | EnvironmentName | Azure bulut aboneliği ortam adı. Desteklenen ortam adları, **Azurecsesli**, **AzureUSGovernment**veya Çin Azure aboneliği kullanılıyorsa, **AzureChinaCloud**.  |

   >[!Note]
   > Oturumunuzun süresi dolarsa, parolanız değiştirilmiştir veya yalnızca hesapları değiştirmek istiyorsanız, Add-AzureRmAccount komutunu kullanarak oturum açmadan önce aşağıdaki cmdlet 'i çalıştırın:`Remove-AzureRmAccount-Scope Process`

3. Birden çok aboneliğiniz varsa, kullanmak istediğiniz birini seçmek için aşağıdaki komutu çalıştırın:  

   ```powershell  
   Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

4. Azure aboneliğinize Azure Stack kaynak sağlayıcısını kaydetmek için aşağıdaki komutu çalıştırın:

   ```powershell  
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

5. PowerShell ıSE 'yi yönetici olarak başlatın ve Azure Stack araçları 'nı indirdiğinizde oluşturulan **Azurestack-Tools-Master** dizinindeki **kayıt** klasörüne gidin. **Registerwithazure. psm1** modülünü PowerShell kullanarak içeri aktarın:

   ```powershell  
   Import-Module .\RegisterWithAzure.psm1
   ```

6. Ardından, aynı PowerShell oturumunda, doğru Azure PowerShell bağlamında oturum açtığınızdan emin olun. Bu, daha önce Azure Stack kaynak sağlayıcısını kaydetmek için kullanılan Azure hesabıdır. Çalıştırmak için PowerShell:

   ```powershell  
   Connect-AzureRmAccount -Environment "<environment name>"
   ```

   | Parametre | Açıklama |  
   |-----|-----|
   | EnvironmentName | Azure bulut aboneliği ortam adı. Desteklenen ortam adları, **Azurecsesli**, **AzureUSGovernment**veya Çin Azure aboneliği kullanılıyorsa, **AzureChinaCloud**.  |

7. Aynı PowerShell oturumunda, **set-AzsRegistration** cmdlet 'ini çalıştırın. Çalıştırmak için PowerShell:  

   ```powershell  
   $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
   $RegistrationName = "<unique-registration-name>"
   Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -BillingModel PayAsYouUse `
      -RegistrationName $RegistrationName
   ```
   Set-AzsRegistration cmdlet 'i hakkında daha fazla bilgi için bkz. [kayıt başvurusu](#registration-reference).

   İşlem 10 ile 15 dakika arasında sürer. Komut tamamlandığında, **"ortamınız artık kayıtlı ve belirtilen parametreler kullanılarak etkinleştirildi"** iletisini görürsünüz.

## <a name="register-connected-with-capacity-billing"></a>Kapasite faturalandırmaya bağlı kayıt

Kullandıkça Öde faturalandırma modelini kullanarak Azure ile Azure Stack kaydetmek için bu adımları kullanın.

> [!Note]  
> Tüm bu adımların ayrıcalıklı uç noktaya (PEP) erişimi olan bir bilgisayardan çalıştırılması gerekir. PEP hakkında daha fazla bilgi için, bkz. [Azure Stack ayrıcalıklı uç noktası kullanma](azure-stack-privileged-endpoint.md).

Bağlı ortamlar Internet ve Azure 'a erişebilir. Bu ortamlar için Azure Stack kaynak sağlayıcısını Azure ile kaydetmeniz ve ardından Faturalandırma modelinizi yapılandırmanız gerekir.

1. Azure Stack kaynak sağlayıcısını Azure ile kaydetmek için, PowerShell ıSE 'yi yönetici olarak başlatın ve aşağıdaki PowerShell cmdlet 'lerini uygun Azure abonelik türüne ayarlanmış olan **Environmentname** parametresiyle birlikte kullanın (aşağıdaki parametrelere bakın).

2. Azure Stack kaydetmek için kullandığınız Azure hesabını ekleyin. Hesabı eklemek için **Add-AzureRmAccount** cmdlet 'ini çalıştırın. Azure hesabı kimlik bilgilerinizi girmeniz istenir ve hesabınızın yapılandırmasına göre 2 öğeli kimlik doğrulaması kullanmanız gerekebilir.

   ```powershell  
   Connect-AzureRmAccount -Environment "<environment name>"
   ```

   | Parametre | Açıklama |  
   |-----|-----|
   | EnvironmentName | Azure bulut aboneliği ortam adı. Desteklenen ortam adları, **Azurecsesli**, **AzureUSGovernment**veya Çin Azure aboneliği kullanılıyorsa, **AzureChinaCloud**.  |

3. Birden çok aboneliğiniz varsa, kullanmak istediğiniz birini seçmek için aşağıdaki komutu çalıştırın:  

   ```powershell  
   Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

4. Azure aboneliğinize Azure Stack kaynak sağlayıcısını kaydetmek için aşağıdaki komutu çalıştırın:

   ```powershell  
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

5. PowerShell ıSE 'yi yönetici olarak başlatın ve Azure Stack araçları 'nı indirdiğinizde oluşturulan **Azurestack-Tools-Master** dizinindeki **kayıt** klasörüne gidin. **Registerwithazure. psm1** modülünü PowerShell kullanarak içeri aktarın:

   ```powershell  
   $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
   $RegistrationName = "<unique-registration-name>"
   Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -AgreementNumber <EA agreement number> `
      -BillingModel Capacity `
      -RegistrationName $RegistrationName
   ```
   > [!Note]  
   > Parametresini false olarak ayarlayarak **set-AzsRegistration** cmdlet 'inin UsageReportingEnabled parametresiyle kullanım raporlamayı devre dışı bırakabilirsiniz. 
   
   Set-AzsRegistration cmdlet 'i hakkında daha fazla bilgi için bkz. [kayıt başvurusu](#registration-reference).

## <a name="register-disconnected-with-capacity-billing"></a>Kapasite faturalandırmayla bağlantısı kesildi

Bağlantısı kesilmiş bir ortama Azure Stack kaydediyorsanız (internet bağlantısı olmadan), Azure Stack ortamından bir kayıt belirteci almanız ve ardından bu belirteci Azure 'a bağlanabilecek ve PowerShell 'e sahip olan bir bilgisayarda kullanmanız gerekir Azure Stack yüklendiyse.  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Azure Stack ortamından bir kayıt belirteci alın

1. PowerShell ıSE 'yi yönetici olarak başlatın ve Azure Stack araçları 'nı indirdiğinizde oluşturulan **Azurestack-Tools-Master** dizinindeki **kayıt** klasörüne gidin. **Registerwithazure. psm1** modülünü içeri aktarın:  

   ```powershell  
   Import-Module .\RegisterWithAzure.psm1
   ```

2. Kayıt belirtecini almak için aşağıdaki PowerShell cmdlet 'lerini çalıştırın:  

   ```Powershell
   $FilePathForRegistrationToken = "$env:SystemDrive\RegistrationToken.txt"
   $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential $YourCloudAdminCredential -UsageReportingEnabled:$False -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<EA agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
   ```
   Get-AzsRegistrationToken cmdlet 'i hakkında daha fazla bilgi için bkz. [kayıt başvurusu](#registration-reference).

   > [!Tip]  
   > Kayıt belirteci, *$FilePathForRegistrationToken*için belirtilen dosyaya kaydedilir. FilePath veya dosya adını dilediğiniz şekilde değiştirebilirsiniz.

3. Bu kayıt belirtecini Azure bağlı makinesinde kullanmak üzere kaydedin. $FilePathForRegistrationToken dosya veya metin kopyalayabilirsiniz.

### <a name="connect-to-azure-and-register"></a>Azure 'a bağlanma ve kaydetme

Internet 'e bağlı bilgisayarda, RegisterWithAzure. psm1 modülünü içeri aktarmak için aynı adımları gerçekleştirin ve doğru Azure PowerShell bağlamına oturum açın. Ardından Register-AzsEnvironment çağırın. Azure 'a kaydolmak için kayıt belirtecini belirtin. Aynı Azure abonelik KIMLIĞINI kullanarak Azure Stack birden fazla örneğini kaydediyorsanız, benzersiz bir kayıt adı belirtin.

Kayıt belirtecinize ve benzersiz bir belirteç adına ihtiyacınız olacaktır.

1. PowerShell ıSE 'yi yönetici olarak başlatın ve Azure Stack araçları 'nı indirdiğinizde oluşturulan **Azurestack-Tools-Master** dizinindeki **kayıt** klasörüne gidin. **Registerwithazure. psm1** modülünü içeri aktarın:  

   ```powershell  
   Import-Module .\RegisterWithAzure.psm1
   ```

2. Ardından aşağıdaki PowerShell cmdlet 'lerini çalıştırın:  

  ```powershell  
  $RegistrationToken = "<Your Registration Token>"
  $RegistrationName = "<unique-registration-name>"
  Register-AzsEnvironment -RegistrationToken $RegistrationToken -RegistrationName $RegistrationName
  ```

İsteğe bağlı olarak, Get-Content cmdlet 'ini kullanarak kayıt belirtecinizi içeren bir dosyayı işaret edebilir.

Kayıt belirtecinize ve benzersiz bir belirteç adına ihtiyacınız olacaktır.

1. PowerShell ıSE 'yi yönetici olarak başlatın ve Azure Stack araçları 'nı indirdiğinizde oluşturulan **Azurestack-Tools-Master** dizinindeki **kayıt** klasörüne gidin. **Registerwithazure. psm1** modülünü içeri aktarın:  

  ```powershell  
  Import-Module .\RegisterWithAzure.psm1
  ```

2. Ardından aşağıdaki PowerShell cmdlet 'lerini çalıştırın:  

  ```powershell  
  $RegistrationToken = Get-Content -Path '<Path>\<Registration Token File>'
  Register-AzsEnvironment -RegistrationToken $RegistrationToken -RegistrationName $RegistrationName
  ```

  > [!Note]  
  > Kayıt kaynağı adını ve kayıt belirtecini ileride başvurmak üzere kaydedin.

### <a name="retrieve-an-activation-key-from-azure-registration-resource"></a>Azure kayıt kaynağından bir etkinleştirme anahtarı alma

Ardından, Register-AzsEnvironment sırasında Azure 'da oluşturulan kayıt kaynağından bir etkinleştirme anahtarı almanız gerekir.

Etkinleştirme anahtarını almak için aşağıdaki PowerShell cmdlet 'lerini çalıştırın:  

  ```Powershell
  $RegistrationResourceName = "<unique-registration-name>"
  $KeyOutputFilePath = "$env:SystemDrive\ActivationKey.txt"
  $ActivationKey = Get-AzsActivationKey -RegistrationName $RegistrationResourceName -KeyOutputFilePath $KeyOutputFilePath
  ```

  > [!Tip]  
  > Etkinleştirme anahtarı *$KeyOutputFilePath*için belirtilen dosyaya kaydedilir. FilePath veya dosya adını dilediğiniz şekilde değiştirebilirsiniz.

### <a name="create-an-activation-resource-in-azure-stack"></a>Azure Stack bir etkinleştirme kaynağı oluşturma

Get-AzsActivationKey öğesinden oluşturulan etkinleştirme anahtarından dosya veya metin ile Azure Stack ortamına dönün. Daha sonra, bu etkinleştirme anahtarını kullanarak Azure Stack bir etkinleştirme kaynağı oluşturursunuz. Etkinleştirme kaynağı oluşturmak için aşağıdaki PowerShell cmdlet 'lerini çalıştırın: 

  ```Powershell
  $ActivationKey = "<activation key>"
  New-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -ActivationKey $ActivationKey
  ```

İsteğe bağlı olarak, Get-Content cmdlet 'ini kullanarak kayıt belirtecinizi içeren bir dosyayı işaret edebilirsiniz:

  ```Powershell
  $ActivationKey = Get-Content -Path '<Path>\<Activation Key File>'
  New-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -ActivationKey $ActivationKey
  ```

## <a name="verify-azure-stack-registration"></a>Azure Stack kaydı doğrulama

Azure Stack kaydın başarılı olduğunu doğrulamak için **Bölge yönetimi** kutucuğunu kullanabilirsiniz. Bu kutucuk, yönetim portalındaki varsayılan panoda bulunur. Durum kaydedilebilir veya kaydettirilmemiş olabilir. Kaydedilmişse, kayıt kaynağı grubu ve adı ile birlikte Azure Stack kaydetmek için kullandığınız Azure abonelik KIMLIĞINI de gösterir.

1. [Azure Stack yönetim portalında](https://adminportal.local.azurestack.external)oturum açın.

2. Panodan **Bölge yönetimi**' ni seçin.

3. Seçin **özellikleri**. Bu dikey pencere, ortamınızın durumunu ve ayrıntılarını gösterir. Durum **kaydedilebilir**, **kaydettirilmemiş**veya **geçerliliği zaman aşımına**eklenebilir.

    [![Bölge yönetimi kutucuğu](media/azure-stack-registration/admin1sm.png "Bölge yönetimi kutucuğu")](media/azure-stack-registration/admin1.png#lightbox)

    Kaydedilmişse, özellikler şunları içerir:
    
    - **Kayıt ABONELIK kimliği**: Azure Stack kayıtlı ve ilişkili Azure abonelik KIMLIĞI
    - **Kayıt kaynak grubu**: Azure Stack kaynaklarını içeren ilişkili abonelikte Azure Kaynak grubu.

4. Azure Stack kayıt kaynaklarını görüntülemek için Azure portal kullanabilir ve sonra kaydın başarılı olduğunu doğrulayabilirsiniz. Azure Stack kaydettirmek için kullandığınız abonelikle ilişkili bir hesabı kullanarak [Azure Portal](https://portal.azure.com) oturum açın. **Tüm kaynaklar**' ı seçin, **gizli türleri göster** onay kutusunu etkinleştirin ve kayıt adını seçin.
5. Kayıt başarılı olmadıysa, sorunu çözmek için [buradaki adımları](#change-the-subscription-you-use) izleyerek yeniden kaydolmanız gerekir.  

Alternatif olarak, Market yönetim özelliğini kullanarak kayıt işleminin başarılı olup olmadığını doğrulayabilirsiniz. Market yönetimi dikey penceresinde Market öğelerinin bir listesini görürseniz kaydınız başarılı oldu. Ancak, bağlantısı kesilen ortamlarda Market yönetimi 'nde Market öğelerini göremezsiniz.

> [!NOTE]
> Kayıt tamamlandıktan sonra, kayıt olmayan etkin uyarı artık görünmez. 1904 ' den önceki Azure Stack sürümlerde, bağlantısı kesilen senaryolarda, Market yönetimi 'nde, başarıyla kaydolduysanız bile Azure Stack kaydetmenizi ve etkinleştirmenizi isteyip istemediğinizi soran bir ileti görürsünüz. Bu ileti Release 1904 ve üzeri sürümlerde görünmez.

## <a name="renew-or-change-registration"></a>Kaydı yenileme veya değiştirme

### <a name="renew-or-change-registration-in-connected-environments"></a>Bağlı ortamlardaki kayıtları yenileme veya değiştirme

Aşağıdaki koşullarda kaydınızı güncelleştirmeniz veya yenilemeniz gerekir:

- Kapasite tabanlı yıllık aboneliğinizi yeniledikten sonra.
- Faturalama modelinizi değiştirdiğinizde.
- Kapasite tabanlı faturalandırma için değişiklikleri ölçeklendirirseniz (düğüm Ekle/Kaldır).

#### <a name="change-the-subscription-you-use"></a>Kullandığınız aboneliği değiştirme

Kullandığınız aboneliği değiştirmek istiyorsanız, önce **Remove-azsregistration** cmdlet 'ini çalıştırmanız, ardından doğru Azure PowerShell bağlamında oturum açtığınızdan emin olmanız ve son olarak, değiştirilen parametrelerle **set-azsregistration** komutunu çalıştırmanız gerekir Aşağıdakiler `<billing model>`dahil:

  ```powershell  
  Remove-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -RegistrationName $RegistrationName
  Set-AzureRmContext -SubscriptionId $NewSubscriptionId
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel <billing model> -RegistrationName $RegistrationName
  ```

#### <a name="change-the-billing-model-or-how-to-offer-features"></a>Faturalandırma modelini değiştirme veya özellikleri sunma

Faturalandırma modelini değiştirmek veya yüklemenizin özelliklerini sunmak isterseniz, yeni değerleri ayarlamak için kayıt işlevini çağırabilirsiniz. Önce geçerli kaydı kaldırmanız gerekmez:

  ```powershell  
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel <billing model> -RegistrationName $RegistrationName
  ```

### <a name="renew-or-change-registration-in-disconnected-environments"></a>Bağlantısı kesilen ortamlarda kaydı yenileme veya değiştirme

Aşağıdaki koşullarda kaydınızı güncelleştirmeniz veya yenilemeniz gerekir:

- Kapasite tabanlı yıllık aboneliğinizi yeniledikten sonra.
- Faturalama modelinizi değiştirdiğinizde.
- Kapasite tabanlı faturalandırma için değişiklikleri ölçeklendirirseniz (düğüm Ekle/Kaldır).

#### <a name="remove-the-activation-resource-from-azure-stack"></a>Etkinleştirme kaynağını Azure Stack kaldır

Önce etkinleştirme kaynağını Azure Stack ve ardından Azure 'daki kayıt kaynağını kaldırmanız gerekir.  

Etkinleştirme kaynağını Azure Stack kaldırmak için Azure Stack ortamınızda aşağıdaki PowerShell cmdlet 'lerini çalıştırın:  

  ```Powershell
  Remove-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  ```

Ardından, Azure 'daki kayıt kaynağını kaldırmak için Azure bağlı bir bilgisayarda olduğunuzdan emin olun, doğru Azure PowerShell bağlamında oturum açın ve aşağıda açıklandığı gibi uygun PowerShell cmdlet 'lerini çalıştırın.

Kaynağı oluşturmak için kullanılan kayıt belirtecini kullanabilirsiniz:  

  ```Powershell
  $RegistrationToken = "<registration token>"
  Unregister-AzsEnvironment -RegistrationToken $RegistrationToken
  ```

Ya da kayıt adını kullanabilirsiniz:

  ```Powershell
  $RegistrationName = "AzureStack-<unique-registration-name>"
  Unregister-AzsEnvironment -RegistrationName $RegistrationName
  ```

### <a name="re-register-using-disconnected-steps"></a>Bağlantısı kesik adımları kullanarak yeniden kaydolun

Bağlantısı kesilmiş bir senaryoda tamamen kaydolmamış ve bağlantısı kesilmiş bir senaryoya Azure Stack ortamı kaydetme adımlarını tekrarlamanız gerekir.

### <a name="disable-or-enable-usage-reporting"></a>Kullanım raporlamayı devre dışı bırakma veya etkinleştirme

Kapasite faturalandırma modeli kullanan Azure Stack ortamları için, **set-AzsRegistration** veya **Get-AzsRegistrationToken** cmdlet 'lerini kullanarak **UsageReportingEnabled** parametresiyle birlikte kullanım raporlamayı devre dışı bırakın. Azure Stack kullanım ölçümlerini varsayılan olarak raporlar. Kapasite kullanımları veya bağlantısı kesilmiş bir ortamı destekleyen operatörler, kullanım raporlamayı devre dışı bırakır.

#### <a name="with-a-connected-azure-stack"></a>Bağlı bir Azure Stack

   ```powershell  
   $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
   $RegistrationName = "<unique-registration-name>"
   Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -BillingModel Capacity
      -RegistrationName $RegistrationName
   ```

#### <a name="with-a-disconnected-azure-stack"></a>Bağlantısı kesik Azure Stack

1. Kayıt belirtecini değiştirmek için aşağıdaki PowerShell cmdlet 'lerini çalıştırın:  

   ```Powershell
   $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
   $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential -UsageReportingEnabled:$False
   $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<EA agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
   ```

   > [!Tip]  
   > Kayıt belirteci, *$FilePathForRegistrationToken*için belirtilen dosyaya kaydedilir. FilePath veya dosya adını dilediğiniz şekilde değiştirebilirsiniz.

2. Bu kayıt belirtecini Azure bağlı makinesinde kullanmak üzere kaydedin. $FilePathForRegistrationToken dosya veya metin kopyalayabilirsiniz.

## <a name="move-a-registration-resource"></a>Kayıt kaynağını taşıma

Aynı **abonelik altındaki** kaynak grupları arasında bir kayıt kaynağını taşımak tüm ortamlar için desteklenir. Ancak, bir kayıt kaynağını abonelikler arasında taşımak yalnızca her iki abonelik aynı Iş ortağı KIMLIĞINE çözümlenirse CSP 'Ler için desteklenir. Kaynakları yeni bir kaynak grubuna taşıma hakkında daha fazla bilgi için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](/azure/azure-resource-manager/resource-group-move-resources).

> [!IMPORTANT]
> Portalda kayıt kaynaklarının yanlışlıkla silinmesini engellemek için, kayıt betiği kaynağa otomatik olarak bir kilit ekler. Taşımadan veya silmeden önce bu kilidi kaldırmanız gerekir. Yanlışlıkla silinmeye engel olmak için kayıt kaynağına bir kilit eklemeniz önerilir.

## <a name="registration-reference"></a>Kayıt başvurusu

### <a name="set-azsregistration"></a>Set-AzsRegistration

**Set-AzsRegistration** kullanarak Azure ile Azure Stack kaydedebilir ve Market ve kullanım raporlamadaki öğelerin teklifini etkinleştirebilir veya devre dışı bırakabilirsiniz.

Cmdlet 'ini çalıştırmak için şunlar gerekir:

- Her türden küresel bir Azure aboneliği.
- Ayrıca, bu aboneliğe sahip veya katkıda bulunan bir hesapla Azure PowerShell için oturum açmış olmanız gerekir.

```powershell
Set-AzsRegistration [-PrivilegedEndpointCredential] <PSCredential> [-PrivilegedEndpoint] <String> [[-AzureContext]
    <PSObject>] [[-ResourceGroupName] <String>] [[-ResourceGroupLocation] <String>] [[-BillingModel] <String>]
    [-MarketplaceSyndicationEnabled] [-UsageReportingEnabled] [[-AgreementNumber] <String>] [[-RegistrationName]
    <String>] [<CommonParameters>]
```

| Parametre | Type | Açıklama |
|-------------------------------|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| PrivilegedEndpointCredential | PSCredential | [Ayrıcalıklı uç noktaya erişmek](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint)için kullanılan kimlik bilgileri. Kullanıcı adı **AzureStackDomain\CloudAdmin**biçimindedir. |
| Ayrıcalıklı Gedendpoint | Dize | Size günlük toplama ve diğer dağıtım sonrası görevleri gibi yetenekler sağlayan, önceden yapılandırılmış bir uzak PowerShell konsolu. Daha fazla bilgi edinmek için [ayrıcalıklı uç noktayı kullanma](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint) makalesini inceleyin. |
| AzureContext | PSObject |  |
| ResourceGroupName | Dize |  |
| ResourceGroupLocation | Dize |  |
| BillingModel | Dize | Aboneliğinizin kullandığı faturalandırma modeli. Bu parametre için izin verilen değerler şunlardır: Kapasite, Payasyoukullanım ve geliştirme. |
| Pazar Placesyndicationenabled | True/False | Market yönetim özelliğinin portalda kullanılabilir olup olmadığını belirler. İnternet bağlantısına kaydolurken doğru olarak ayarlayın. Bağlantısı kesilen ortamlara kaydolmak için false olarak ayarlayın. Bağlantısı kesilen kayıtlar için, Market öğelerini indirmek üzere [çevrimdışı dağıtım aracı](azure-stack-download-azure-marketplace-item.md#disconnected-or-a-partially-connected-scenario) kullanılabilir. |
| UsageReportingEnabled | True/False | Azure Stack kullanım ölçümlerini varsayılan olarak raporlar. Kapasite kullanımları veya bağlantısı kesilmiş bir ortamı destekleyen operatörler, kullanım raporlamayı devre dışı bırakır. Bu parametre için izin verilen değerler şunlardır: True, false. |
| AgreementNumber | Dize |  |
| RegistrationName | Dize | Kayıt betiğini aynı Azure abonelik KIMLIĞINI kullanarak Azure Stack birden fazla örneğinden çalıştırıyorsanız, kayıt için benzersiz bir ad ayarlayın. Parametrenin varsayılan bir **Azurestackregistration**değeri vardır. Ancak, birden fazla Azure Stack örneğinde aynı adı kullanırsanız, komut dosyası başarısız olur. |

### <a name="get-azsregistrationtoken"></a>Get-AzsRegistrationToken

Get-AzsRegistrationToken, Giriş parametrelerinden bir kayıt belirteci oluşturur.

```powershell  
Get-AzsRegistrationToken [-PrivilegedEndpointCredential] <PSCredential> [-PrivilegedEndpoint] <String>
    [-BillingModel] <String> [[-TokenOutputFilePath] <String>] [-UsageReportingEnabled] [[-AgreementNumber] <String>]
    [<CommonParameters>]
```
| Parametre | Type | Açıklama |
|-------------------------------|--------------|-------------|
| PrivilegedEndpointCredential | PSCredential | [Ayrıcalıklı uç noktaya erişmek](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint)için kullanılan kimlik bilgileri. Kullanıcı adı **AzureStackDomain\CloudAdmin**biçimindedir. |
| Ayrıcalıklı Gedendpoint | Dize |  Size günlük toplama ve diğer dağıtım sonrası görevleri gibi yetenekler sağlayan, önceden yapılandırılmış bir uzak PowerShell konsolu. Daha fazla bilgi edinmek için [ayrıcalıklı uç noktayı kullanma](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint) makalesini inceleyin. |
| AzureContext | PSObject |  |
| ResourceGroupName | Dize |  |
| ResourceGroupLocation | Dize |  |
| BillingModel | Dize | Aboneliğinizin kullandığı faturalandırma modeli. Bu parametre için izin verilen değerler şunlardır: Kapasite, Payasyoukullanım ve geliştirme. |
| Pazar Placesyndicationenabled | True/False |  |
| UsageReportingEnabled | True/False | Azure Stack kullanım ölçümlerini varsayılan olarak raporlar. Kapasite kullanımları veya bağlantısı kesilmiş bir ortamı destekleyen operatörler, kullanım raporlamayı devre dışı bırakır. Bu parametre için izin verilen değerler şunlardır: True, false. |
| AgreementNumber | Dize |  |

## <a name="registration-failures"></a>Kayıt hataları

Azure Stack kaydını gerçekleştirmeye çalışırken aşağıdaki hatalardan birini görebilirsiniz:
1. $HostName için zorunlu donanım bilgisi alınamadı. Fiziksel ana bilgisayarı ve bağlantıyı denetleyip kaydı yeniden çalıştırmayı deneyin.

2. Donanım bilgilerini almak için $hostName bağlanılamıyor. lütfen fiziksel ana bilgisayarı ve bağlantıyı denetleyip kaydı yeniden çalıştırmayı deneyin.

> Neden: Bu genellikle, etkinleştirmeyi denemek için ana bilgisayarlardan UUID, BIOS ve CPU gibi donanım ayrıntılarını edinmeyi denediğimiz ve fiziksel ana bilgisayara bağlanamama nedeniyle bağlanamadığımız için.

Market yönetimine erişmeye çalışırken, ürünleri yayınlamak isterken bir hata oluşur. 
> Neden: Bu genellikle Azure Stack kayıt kaynağına erişeamadığında oluşur. Bunun yaygın bir nedeni, Azure aboneliğinin Dizin kiracının kayıt değişikliğini sıfırlamasıdır. Aboneliğin Dizin kiracısını değiştirdiyseniz Azure Stack marketi veya rapor kullanımına erişemezsiniz. Bu sorunu giderecek şekilde yeniden kaydetmeniz gerekir.

Market yönetimi hala, bağlantısı kesilen işlemi kullanarak damganız zaten kaydolduktan sonra bile Azure Stack kaydetmenizi ve etkinleştirmenizi ister. 
> Neden: Bu, bağlantısı kesilen ortamlar için bilinen bir sorundur. Aşağıdaki [adımları](azure-stack-registration.md#verify-azure-stack-registration)izleyerek kayıt durumunuzu doğrulayabilirsiniz. Market yönetimini kullanabilmek için [çevrimdışı aracını](azure-stack-download-azure-marketplace-item.md#disconnected-or-a-partially-connected-scenario)kullanmanız gerekir. 

## <a name="next-steps"></a>Sonraki adımlar

[Azure 'dan Market öğelerini indirme](azure-stack-download-azure-marketplace-item.md)
