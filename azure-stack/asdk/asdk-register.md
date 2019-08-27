---
title: ASDK 'yi Azure ile kaydedin | Microsoft Docs
description: Market dağıtımı ve kullanım Raporlamayı etkinleştirmek için Azure ile Azure Stack Geliştirme Seti (ASDK) kaydetmeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2019
ms.author: justinha
ms.reviewer: misainat
ms.lastreviewed: 06/14/2019
ms.openlocfilehash: 886271e99b10d3fec0801f977a693a01e59fc0a5
ms.sourcegitcommit: 7968f9f0946138867323793be9966ee2ef99dcf4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70025857"
---
# <a name="register-the-asdk-with-azure"></a>ASDK 'yi Azure ile kaydetme

Azure 'dan Market öğelerini indirmek ve ticari veri raporlamayı Microsoft 'a geri ayarlamak için Azure ile Azure Stack Geliştirme Seti (ASDK) yüklemenizi kaydedebilirsiniz. Market dağıtımı da dahil olmak üzere tam Azure Stack işlevselliği desteklemek için kayıt gereklidir. Market dağıtımı ve kullanım raporlaması gibi önemli Azure Stack işlevlerini sınamanızı sağlamak için kayıt gereklidir. Azure Stack kaydolduktan sonra, kullanım Azure ticareti ' ne bildirilir. Bunu, kayıt için kullandığınız abonelik altında görebilirsiniz. Ancak, ASDK kullanıcıları rapor ettikleri tüm kullanımlar için ücretlendirilmez.

ASDK 'yi kaydetmezseniz, ASDK 'nizi kaydetmenizi öneren bir **etkinleştirme gerekli** uyarı uyarısı görebilirsiniz. Bu davranış beklenmektedir.

## <a name="prerequisites"></a>Önkoşullar

ASDK 'yi Azure ile kaydetmek için bu yönergeleri kullanmadan önce Azure Stack PowerShell 'i yüklediğinizden ve [dağıtım sonrası yapılandırma](asdk-post-deploy.md) makalesinde açıklanan Azure Stack araçları indirdiğinizden emin olun.

PowerShell dil modu, ASDK 'yi Azure ile kaydetmek için kullanılan bilgisayarda da **Fulllanguage** olarak ayarlanmalıdır. Geçerli dil modunun tam olarak ayarlandığını doğrulamak için, yükseltilmiş bir PowerShell penceresi açın ve aşağıdaki PowerShell komutlarını çalıştırın:

```powershell  
$ExecutionContext.SessionState.LanguageMode
```

Çıktının **Fulllanguage**döndürdüğünden emin olun. Başka bir dil modu döndürülürse, kayıt devam etmeden önce kaydın başka bir bilgisayarda çalıştırılması veya dil modunun **Fulllanguage** olarak ayarlanması gerekir.

Kayıt için kullanılan Azure AD hesabının Azure aboneliğine erişimi olması ve bu abonelikle ilişkili dizinde kimlik uygulamaları ve hizmet sorumluları oluşturma izinlerine sahip olması gerekir. Genel yönetici kimlik bilgilerini kullanmak yerine [kayıt için kullanılacak bir hizmet hesabı oluşturarak](../operator/azure-stack-registration-role.md) Azure ile Azure Stack kaydetmenizi öneririz.

## <a name="register-the-asdk"></a>ASDK 'yi kaydetme

ASDK 'yi Azure ile kaydetmek için aşağıdaki adımları izleyin.

> [!NOTE]
> Tüm bu adımların ayrıcalıklı uç noktaya erişimi olan bir bilgisayardan çalıştırılması gerekir. ASDK için, bu, ASDK ana bilgisayardır.

1. Bir PowerShell konsolunu yönetici olarak açın.  

2. Azure ile ASDK yüklemenizi kaydetmek için aşağıdaki PowerShell komutlarını çalıştırın. Hem Azure Faturalandırma abonelik KIMLIĞINIZ hem de yerel ASDK yüklemesinde oturum açın. Henüz bir Azure Faturalandırma abonelik KIMLIĞINIZ yoksa [burada ücretsiz bir Azure hesabı oluşturabilirsiniz](https://azure.microsoft.com/free/?b=17.06). Kayıt Azure Stack Azure aboneliğinizde hiçbir ücret alınmaz.<br><br>**Set-AzsRegistration** cmdlet 'ini çalıştırdığınızda kayıt için benzersiz bir ad ayarlayın. **Registrationname** parametresinin varsayılan bir **Azurestackregistration**değeri vardır. Ancak, birden fazla Azure Stack örneğinde aynı adı kullanırsanız, komut dosyası başarısız olur.

    ```powershell  
    # Add the Azure cloud subscription environment name. 
    # Supported environment names are AzureCloud, AzureChinaCloud, or AzureUSGovernment depending which Azure subscription you're using.
    Add-AzureRmAccount -EnvironmentName "<environment name>"

    # Register the Azure Stack resource provider in your Azure subscription
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack

    # Import the registration module that was downloaded with the GitHub tools
    Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

    # If you have multiple subscriptions, run the following command to select the one you want to use:
    # Get-AzureRmSubscription -SubscriptionID "<subscription ID>" | Select-AzureRmSubscription
    
    # Register Azure Stack
    $AzureContext = Get-AzureRmContext
    $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
    $RegistrationName = "<unique-registration-name>"
    Set-AzsRegistration `
    -PrivilegedEndpointCredential $CloudAdminCred `
    -PrivilegedEndpoint AzS-ERCS01 `
    -BillingModel Development `
    -RegistrationName $RegistrationName `
    -UsageReportingEnabled:$true
    ```

3. Betik tamamlandığında şu iletiyi görmeniz gerekir: **Ortamınız artık, girilen parametreler kullanılarak kaydedilir ve etkinleştirilir.**

    ![Ortamınız artık kayıtlı](media/asdk-register/1.PNG)

## <a name="register-in-disconnected-environments"></a>Bağlantısı kesilen ortamlara kaydolma

Bağlantısı kesilen bir ortama Azure Stack kaydediyorsanız (internet bağlantısı olmadan), Azure Stack ortamından bir kayıt belirteci almanız ve ardından bu belirteci Azure 'a bağlanarak bir etkinleştirmeyi kaydettirmek ve oluşturmak üzere Azure 'a bağlanabilecek bir bilgisayarda kullanmanız gerekir ASDK ortamınız için kaynak.

 > [!IMPORTANT]
 > Azure Stack kaydetmek için bu yönergeleri kullanmadan önce, Azure Stack için PowerShell 'i yüklediğinizden ve hem ASDK ana bilgisayar bilgisayarındaki [dağıtım sonrası yapılandırma](asdk-post-deploy.md) makalesinde açıklandığı gibi Azure Stack araçlarını indirdiğinizden emin olun ve İnternet erişimi olan ve Azure 'a bağlanmak ve kaydolmak için kullanılan bilgisayar.

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Azure Stack ortamından bir kayıt belirteci alın

ASDK ana bilgisayarında, PowerShell 'i yönetici olarak başlatın ve Azure Stack araçları 'nı indirdiğinizde oluşturulan **Azurestack-Tools-Master** dizinindeki **kayıt** klasörüne gidin. **Registerwithazure. psm1** modülünü içeri aktarmak Için aşağıdaki PowerShell komutlarını kullanın ve ardından kayıt belirtecini almak için **Get-azsregistrationtoken** cmdlet 'ini kullanın:  

   ```powershell  
   # Import the registration module that was downloaded with the GitHub tools
   Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

   # Create registration token
   $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
   # File path to save the token. This example saves the file as C:\RegistrationToken.txt.
   $FilePathForRegistrationToken = "$env:SystemDrive\RegistrationToken.txt"
   $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential $CloudAdminCred `
   -UsageReportingEnabled:$false `
   -PrivilegedEndpoint AzS-ERCS01 `
   -BillingModel Development `
   -MarketplaceSyndicationEnabled:$false `
   -TokenOutputFilePath $FilePathForRegistrationToken
   ```

Bu kayıt belirtecini Internet 'e bağlı bilgisayarda kullanmak üzere kaydedin. Dosya veya metni `$FilePathForRegistrationToken` parametresi tarafından oluşturulan dosyadan kopyalayabilirsiniz.

### <a name="connect-to-azure-and-register"></a>Azure 'a bağlanma ve kaydetme

Internet 'e bağlı bilgisayarda, **Registerwithazure. psm1** modülünü içeri aktarmak Için aşağıdaki PowerShell komutlarını kullanın ve ardından yeni oluşturduğunuz kayıt belirtecini kullanarak Azure 'a kaydolmak için **register-azsenvironment** cmdlet 'ini kullanın ve benzersiz bir kayıt adı:  

  ```powershell  
  # Add the Azure cloud subscription environment name. 
  # Supported environment names are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
  Add-AzureRmAccount -EnvironmentName "<environment name>"

  # If you have multiple subscriptions, run the following command to select the one you want to use:
  # Get-AzureRmSubscription -SubscriptionID "<subscription ID>" | Select-AzureRmSubscription

  # Register the Azure Stack resource provider in your Azure subscription
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack

  # Import the registration module that was downloaded with the GitHub tools
  Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

  # Register with Azure
  # This example uses the C:\RegistrationToken.txt file.
  $registrationToken = Get-Content -Path "$env:SystemDrive\RegistrationToken.txt"
  $RegistrationName = "<unique-registration-name>"
  Register-AzsEnvironment -RegistrationToken $registrationToken `
  -RegistrationName $RegistrationName
  ```

Alternatif olarak, **Get-Content** cmdlet 'ini kullanarak kayıt belirtecinizi içeren bir dosyayı işaret edebilirsiniz:

  ```powershell  
  # Add the Azure cloud subscription environment name. 
  # Supported environment names are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
  Add-AzureRmAccount -EnvironmentName "<environment name>"

  # If you have multiple subscriptions, run the following command to select the one you want to use:
  # Get-AzureRmSubscription -SubscriptionID "<subscription ID>" | Select-AzureRmSubscription

  # Register the Azure Stack resource provider in your Azure subscription
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack

  # Import the registration module that was downloaded with the GitHub tools
  Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

  # Register with Azure 
  # This example uses the C:\RegistrationToken.txt file.
  $registrationToken = Get-Content -Path "$env:SystemDrive\RegistrationToken.txt"
  Register-AzsEnvironment -RegistrationToken $registrationToken `
  -RegistrationName $RegistrationName
  ```

Kayıt tamamlandığında şuna benzer bir ileti görmeniz gerekir: **Azure Stack ortamınız artık Azure 'a kayıtlı.**

> [!IMPORTANT]
> PowerShell penceresini kapatmayın.

Daha sonra başvurmak üzere kayıt belirtecini ve kayıt kaynağı adını kaydedin.

### <a name="retrieve-an-activation-key-from-the-azure-registration-resource"></a>Azure kayıt kaynağından bir etkinleştirme anahtarı alma

Hala internet 'e bağlı bilgisayarı **ve aynı PowerShell konsolu penceresini**kullanarak, Azure 'a kaydolduysanız oluşturulan kayıt kaynağından bir etkinleştirme anahtarı alın.

Etkinleştirme anahtarını almak için aşağıdaki PowerShell komutlarını çalıştırın. Önceki adımda Azure ile kaydolurken verdiğiniz benzersiz kayıt adı değerini kullanın:  

  ```Powershell
  $RegistrationResourceName = "<unique-registration-name>"
  # File path to save the activation key. This example saves the file as C:\ActivationKey.txt.
  $KeyOutputFilePath = "$env:SystemDrive\ActivationKey.txt"
  $ActivationKey = Get-AzsActivationKey -RegistrationName $RegistrationResourceName `
  -KeyOutputFilePath $KeyOutputFilePath
  ```

### <a name="create-an-activation-resource-in-azure-stack"></a>Azure Stack bir etkinleştirme kaynağı oluşturma

**Get-AzsActivationKey**öğesinden oluşturulan etkinleştirme anahtarından dosya veya metin ile Azure Stack ortamına dönün. Bu etkinleştirme anahtarını kullanarak Azure Stack bir etkinleştirme kaynağı oluşturmak için aşağıdaki PowerShell komutlarını çalıştırın:   

  ```Powershell
  # Import the registration module that was downloaded with the GitHub tools
  Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1
  
  $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
  $ActivationKey = "<activation key>"
  New-AzsActivationResource -PrivilegedEndpointCredential $CloudAdminCred `
  -PrivilegedEndpoint AzS-ERCS01 `
  -ActivationKey $ActivationKey
  ```

Alternatif olarak, **Get-Content** cmdlet 'ini kullanarak kayıt belirtecinizi içeren bir dosyayı işaret edebilirsiniz:

  ```Powershell
  # Import the registration module that was downloaded with the GitHub tools
  Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

  $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
  # This example uses the C:\ActivationKey.txt file.
  $ActivationKey = Get-Content -Path "$env:SystemDrive\Activationkey.txt"
  New-AzsActivationResource -PrivilegedEndpointCredential $CloudAdminCred `
  -PrivilegedEndpoint AzS-ERCS01 `
  -ActivationKey $ActivationKey
  ```

Etkinleştirme tamamlandığında şuna benzer bir ileti görmeniz gerekir: **Ortamınız kayıt ve etkinleştirme sürecini bitirdi.**

## <a name="verify-the-registration-was-successful"></a>Kaydın başarılı olduğunu doğrulayın

Azure Stack kaydın başarılı olduğunu doğrulamak için **Bölge yönetimi** kutucuğunu kullanabilirsiniz. Bu kutucuk, yönetim portalındaki varsayılan panoda bulunur.

1. [Azure Stack yönetici portalında](https://adminportal.local.azurestack.external)oturum açın.

2. Panodan **Bölge yönetimi**' ni seçin.

    [![Azure Stack yönetim portalı 'Nda bölge yönetimi kutucuğu](media/asdk-register/admin1sm.png "Bölge yönetimi kutucuğu")](media/asdk-register/admin1.png#lightbox)

3. Seçin **özellikleri**. Bu dikey pencere, ortamınızın durumunu ve ayrıntılarını gösterir. Durum **kaydedilebilir** veya **kaydettirilmemiş**olabilir. Kaydolduysanız, kayıt kaynak grubu ve adıyla birlikte Azure Stack kaydetmek için kullandığınız Azure abonelik KIMLIĞINI de gösterir.

## <a name="move-a-registration-resource"></a>Kayıt kaynağını taşıma
Aynı abonelik altındaki kaynak grupları arasında bir kayıt kaynağını taşıma desteklenir . Kaynakları yeni bir kaynak grubuna taşıma hakkında daha fazla bilgi için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack Market öğesi ekleme](../operator/azure-stack-marketplace.md)
