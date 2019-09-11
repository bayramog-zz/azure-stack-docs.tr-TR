---
title: Kaynaklara erişmek için bir uygulama kimliği kullanın
description: Rol tabanlı erişim denetimi ile kullanılabilen, oturum açma ve kaynaklara erişim için bir hizmet sorumlusunun nasıl yönetileceğini açıklar.
services: azure-stack
documentationcenter: na
author: BryanLa
manager: femila
ms.service: azure-stack
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/09/2019
ms.author: bryanla
ms.lastreviewed: 06/20/2019
ms.openlocfilehash: 6855ca4d6453c152bc46584248865bb1934419ca
ms.sourcegitcommit: 305536bfd49319455ca3ca270fe3644b1796bad1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876560"
---
# <a name="use-an-app-identity-to-access-resources"></a>Kaynaklara erişmek için bir uygulama kimliği kullanın

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti (ASDK)*

Kaynakları Azure Resource Manager aracılığıyla dağıtması veya yapılandırması gereken bir uygulama, bir hizmet sorumlusu tarafından temsil etmelidir. Bir Kullanıcı bir Kullanıcı sorumlusu tarafından temsil edildiğinde, hizmet sorumlusu, bir uygulamayı temsil eden bir güvenlik sorumlusu türüdür. Hizmet sorumlusu, uygulamanız için bir kimlik sağlar ve bu hizmet sorumlusu için yalnızca gerekli izinleri atamanıza olanak tanır.  

Örnek olarak, Azure kaynaklarını envantere almak için Azure Resource Manager kullanan bir yapılandırma yönetimi uygulamanız olabilir. Bu senaryoda, bir hizmet sorumlusu oluşturabilir, okuyucu rolünü bu hizmet sorumlusuna verebilir ve yapılandırma yönetimi uygulamasını salt okuma erişimiyle sınırlayabilirsiniz. 

## <a name="overview"></a>Genel Bakış

Bir Kullanıcı sorumlusuna benzer şekilde, bir hizmet sorumlusu kimlik doğrulama sırasında kimlik bilgileri sunmalıdır ve bu iki öğeden oluşur:

- Bazen Istemci KIMLIĞI olarak da adlandırılan bir **uygulama kimliği**. Bu, uygulamanın kaydını Active Directory kiracınızda benzersiz bir şekilde tanımlayan bir GUID 'dir.
- Uygulama KIMLIĞIYLE ilişkili **gizli** dizi. Bir istemci gizli dizesi (bir parolaya benzer) oluşturabilir ya da bir x509 sertifikası (ortak anahtarını kullanır) belirtebilirsiniz. 

Bir hizmet sorumlusu kimliği altında bir uygulamanın çalıştırılması, bir Kullanıcı sorumlusu altında çalıştırılması tercih edilir, çünkü:

 - Hizmet sorumlusu, **daha güçlü kimlik bilgileri**için x509 sertifikası kullanabilir.  
 - Bir hizmet sorumlusuna **daha kısıtlayıcı izinler** atayabilirsiniz. Genellikle, bu izinler yalnızca uygulamanın yapması gereken, *en az ayrıcalık ilkesi*olarak bilinen şekilde kısıtlanır.
 - Hizmet sorumlusu **kimlik bilgileri ve izinleri** Kullanıcı kimlik bilgileri kadar sık değişmez. Örneğin, kullanıcının sorumlulukları değiştiğinde parola gereksinimleri bir değişiklik veya bir kullanıcı şirketten çıkar.

Dizinde, uygulamanın kimliğini temsil etmek için ilişkili bir [hizmet sorumlusu nesnesi](/azure/active-directory/develop/developer-glossary#service-principal-object) oluşturan, dizininizde yeni bir uygulama kaydı oluşturarak başlayın. Bu belgede, Azure Stack örneğiniz için seçtiğiniz dizine bağlı olarak bir hizmet sorumlusu oluşturma ve yönetme işlemi açıklanmaktadır:

- Azure Active Directory (Azure AD). Azure AD, çok kiracılı, bulut tabanlı bir dizin ve kimlik yönetimi hizmetidir. Azure AD 'yi, bağlı bir Azure Stack örneğiyle birlikte kullanabilirsiniz.
- Active Directory Federasyon Hizmetleri (AD FS) (AD FS). AD FS Basitleştirilmiş, güvenli kimlik Federasyonu ve Web çoklu oturum açma (SSO) özellikleri sağlar. Hem bağlı hem de bağlantısı kesik Azure Stack örneklerle AD FS kullanabilirsiniz.

İlk olarak bir hizmet sorumlusunu yönetmeyi, ardından hizmet sorumlusunu bir role atamayı ve kaynak erişimini kısıtlamanızı öğrenirsiniz.

## <a name="manage-an-azure-ad-service-principal"></a>Azure AD hizmet sorumlusunu yönetme 

Kimlik yönetimi hizmetiniz olarak Azure Active Directory (Azure AD) Azure Stack dağıttıysanız, Azure 'da olduğu gibi hizmet sorumluları oluşturabilirsiniz. Bu bölümde Azure portal adımları nasıl gerçekleştireceğiniz gösterilmektedir. Başlamadan önce [gerekli Azure AD izinlerine](/azure/active-directory/develop/howto-create-service-principal-portal#required-permissions) sahip olup olmadığınızı denetleyin.

### <a name="create-a-service-principal-that-uses-a-client-secret-credential"></a>İstemci gizli bilgisi kullanan bir hizmet sorumlusu oluşturma

Bu bölümde, Azure AD kiracınızda hizmet sorumlusu nesnesini oluşturan Azure portal kullanarak uygulamanızı kaydedersiniz. Bu örnekte, hizmet sorumlusu bir istemci gizli bilgisiyle oluşturulur, ancak Portal x509 sertifika tabanlı kimlik bilgilerini de destekler.

1. Azure hesabınızı kullanarak [Azure Portal](https://portal.azure.com) oturum açın.
2. **Yeni kayıt** **uygulama kayıtları** > AzureActiveDirectory > seçin.
3. Uygulama için bir **ad** sağlayın. 
4. Uygun **Desteklenen hesap türlerini**seçin.
5. **Yeniden yönlendirme URI 'si**altında, uygulama türü olarak **Web** ' i seçin ve (isteğe bağlı olarak) uygulamanız gerektiriyorsa bir yeniden yönlendirme URI 'si belirtin. 
6. Değerleri ayarladıktan sonra **Kaydet**' i seçin. Uygulama kaydı oluşturulur ve **genel bakış** sayfası gösterilir.
7. Uygulama kodunuzda kullanmak için **uygulama kimliğini** kopyalayın. Bu değer, Istemci KIMLIĞI olarak da adlandırılır.
8. Bir istemci parolası oluşturmak için **sertifikalar & gizlilikler** sayfasını seçin. **Yeni istemci gizli dizisi**’ni seçin.
9. Gizli anahtar için bir **Açıklama** ve süre **sonu** süresi sağlar. 
10. İşiniz bittiğinde **Ekle**' yi seçin.
11. Gizli anahtar değeri görüntülenir. Daha sonra geri alamazsınız, bu değeri başka bir konuma kopyalayın ve kaydedin. Hizmet sorumlusu oturum açma sırasında istemci uygulamanızda uygulama KIMLIĞIYLE gizli dizi sağlarsınız. 

    ![Kayıtlı anahtar](./media/azure-stack-create-service-principal/create-service-principal-in-azure-stack-secret.png)

## <a name="manage-an-ad-fs-service-principal"></a>AD FS hizmet sorumlusunu yönetme

Kimlik yönetimi hizmetiniz olarak Active Directory Federasyon Hizmetleri (AD FS) (AD FS) Azure Stack dağıttıysanız, hizmet sorumlusunu yönetmek için PowerShell kullanmanız gerekir. Örnek olarak, hem x509 sertifikasını hem de bir istemci gizliliğini gösteren hizmet sorumlusu kimlik bilgilerini yönetmek için aşağıda verilmiştir.

Betikler, Azure Stack örneğiniz için ayrıcalıklı bir uç nokta barındıran bir sanal makineye başka bir oturum açan yükseltilmiş ("yönetici olarak çalıştır") PowerShell konsolunda çalıştırılmalıdır. Ayrıcalıklı uç nokta oturumu kurulduktan sonra, ek cmdlet 'ler hizmet sorumlusunu yürütür ve yönetir. Ayrıcalıklı uç nokta hakkında daha fazla bilgi için, bkz. [Azure Stack ayrıcalıklı uç noktası kullanma](azure-stack-privileged-endpoint.md).

### <a name="create-a-service-principal-that-uses-a-certificate-credential"></a>Sertifika kimlik bilgilerini kullanan bir hizmet sorumlusu oluşturma

Hizmet sorumlusu kimlik bilgileri için bir sertifika oluştururken, aşağıdaki gereksinimlerin karşılanması gerekir:

 - Üretim için sertifika, bir iç sertifika yetkilisinden veya bir genel sertifika yetkilisinden verilmelidir. Ortak sertifika yetkilisi kullanıyorsanız, Microsoft güvenilen kök yetkili programının bir parçası olarak yetkiyi temel işletim sistemi görüntüsüne dahil etmeniz gerekir. Tam listeyi [Microsoft güvenilen kök sertifika programı ' na bulabilirsiniz: Katılımcılar](https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca). Bir [hizmet sorumlusunun sertifika kimlik bilgisini güncelleştirme](#update-a-service-principals-certificate-credential)sırasında, "otomatik olarak imzalanan" test sertifikası oluşturma örneği de daha sonra gösterilir. 
 - Şifreleme sağlayıcısının bir Microsoft eski şifreleme hizmeti sağlayıcısı (CSP) anahtar sağlayıcısı olarak belirtilmesi gerekir.
 - Hem ortak hem de özel anahtarlar gerekli olduğundan, sertifika biçimi PFX dosyasında olmalıdır. Windows Server 'lar ortak anahtar dosyası (SSL sertifika dosyası) ve ilişkili özel anahtar dosyasını içeren. pfx dosyalarını kullanır.
 - Azure Stack altyapınız, sertifika yetkilisinin sertifika Iptal listesi (CRL) konumunda yayımlanan sertifika için ağ erişimine sahip olmalıdır. Bu CRL bir HTTP uç noktası olmalıdır.

Bir sertifikanız olduktan sonra, uygulamanızı kaydetmek ve hizmet sorumlusu oluşturmak için aşağıdaki PowerShell betiğini kullanın. Ayrıca hizmet sorumlusunu Azure 'da oturum açmak için de kullanabilirsiniz. Aşağıdaki yer tutucular için kendi değerlerinizi değiştirin:

| Yer tutucu | Açıklama | Örnek |
| ----------- | ----------- | ------- |
| \<PepVM\> | Azure Stack örneğindeki ayrıcalıklı uç nokta VM 'sinin adı. | "AzS-ERCS01" |
| \<YourCertificateLocation\> | X509 sertifikanızın yerel sertifika deposundaki konumu. | "Cert:\CurrentUser\My\AB5A8A3533CC7AA2025BF05120117E06DE407B34" |
| \<Uygulamanızınadı\> | Yeni uygulama kaydı için açıklayıcı bir ad | "Yönetim aracınız" |

1. Yükseltilmiş bir Windows PowerShell oturumu açın ve aşağıdaki betiği çalıştırın:

   ```powershell  
    # Sign in to PowerShell interactively, using credentials that have access to the VM running the Privileged Endpoint (typically <domain>\cloudadmin)
    $Creds = Get-Credential

    # Create a PSSession to the Privileged Endpoint VM
    $Session = New-PSSession -ComputerName "<PepVm>" -ConfigurationName PrivilegedEndpoint -Credential $Creds

    # Use the Get-Item cmdlet to retrieve your certificate.
    # If you don't want to use a managed certificate, you can produce a self signed cert for testing purposes: 
    # $Cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=<YourAppName>" -KeySpec KeyExchange
    $Cert = Get-Item "<YourCertificateLocation>"
    
    # Use the privileged endpoint to create the new app registration (and service principal object)
    $SpObject = Invoke-Command -Session $Session -ScriptBlock {New-GraphApplication -Name "<YourAppName>" -ClientCertificates $using:cert}
    $AzureStackInfo = Invoke-Command -Session $Session -ScriptBlock {Get-AzureStackStampInformation}
    $Session | Remove-PSSession

    # Using the stamp info for your Azure Stack instance, populate the following variables:
    # - AzureRM endpoint used for Azure Resource Manager operations 
    # - Audience for acquiring an OAuth token used to access Graph API 
    # - GUID of the directory tenant
    $ArmEndpoint = $AzureStackInfo.TenantExternalEndpoints.TenantResourceManager
    $GraphAudience = "https://graph." + $AzureStackInfo.ExternalDomainFQDN + "/"
    $TenantID = $AzureStackInfo.AADTenantID

    # Register and set an AzureRM environment that targets your Azure Stack instance
    Add-AzureRMEnvironment -Name "AzureStackUser" -ArmEndpoint $ArmEndpoint
    Set-AzureRmEnvironment -Name "AzureStackUser" -GraphAudience $GraphAudience -EnableAdfsAuthentication:$true

    # Sign in using the new service principal identity
    $SpSignin = Connect-AzureRmAccount -Environment "AzureStackUser" `
    -ServicePrincipal `
    -CertificateThumbprint $SpObject.Thumbprint `
    -ApplicationId $SpObject.ClientId `
    -TenantId $TenantID

    # Output the service principal details
    $SpObject

   ```
   
2. Betik tamamlandıktan sonra, hizmet sorumlusunun kimlik bilgileri de dahil olmak üzere uygulama kayıt bilgilerini görüntüler. Gösterildiği `ClientID` gibi, ve `Thumbprint` , hizmet sorumlusunun kimliği altında oturum açmak için kullanılır. Başarılı oturum açma işlemi tamamlandıktan sonra, hizmet sorumlusu kimliği, sonraki yetkilendirme ve Azure Resource Manager tarafından yönetilen kaynaklara erişim için kullanılacaktır. 

   ```shell
   ApplicationIdentifier : S-1-5-21-1512385356-3796245103-1243299919-1356
   ClientId              : 3c87e710-9f91-420b-b009-31fa9e430145
   Thumbprint            : 30202C11BE6864437B64CE36C8D988442082A0F1
   ApplicationName       : Azurestack-MyApp-c30febe7-1311-4fd8-9077-3d869db28342
   ClientSecret          :
   PSComputerName        : azs-ercs01
   RunspaceId            : a78c76bb-8cae-4db4-a45a-c1420613e01b
   ```

Sonraki bölümde bulunan `ApplicationIdentifier` değerle birlikte kullanırken PowerShell konsolu oturumunuzu açık tutun.

### <a name="update-a-service-principals-certificate-credential"></a>Hizmet sorumlusunun sertifika kimlik bilgisini güncelleştirme

Artık bir hizmet sorumlusu oluşturduğunuza göre, bu bölümde nasıl yapılacağı gösterilmektedir:

1. Test için yeni bir otomatik olarak imzalanan x509 sertifikası oluşturun.
2. **Parmak izi** özelliğini yeni sertifikayla eşleşecek şekilde güncelleştirerek hizmet sorumlusunun kimlik bilgilerini güncelleştirin.

Aşağıdaki yer tutucular için kendi değerlerinizi değiştirerek, PowerShell kullanarak sertifika kimlik bilgilerini güncelleştirin:

| Yer tutucu | Açıklama | Örnek |
| ----------- | ----------- | ------- |
| \<PepVM\> | Azure Stack örneğindeki ayrıcalıklı uç nokta VM 'sinin adı. | "AzS-ERCS01" |
| \<Uygulamanızınadı\> | Yeni uygulama kaydı için açıklayıcı bir ad | "Yönetim aracınız" |
| \<YourCertificateLocation\> | X509 sertifikanızın yerel sertifika deposundaki konumu. | "Cert:\CurrentUser\My\AB5A8A3533CC7AA2025BF05120117E06DE407B34" |
| \<Appıdentifier\> | Uygulama kaydına atanan tanımlayıcı | "S-1-5-21-1512385356-3796245103-1243299919-1356" |

1. Yükseltilmiş Windows PowerShell oturumunuzu kullanarak aşağıdaki cmdlet 'leri çalıştırın:

     ```powershell
     # Create a PSSession to the PrivilegedEndpoint VM
     $Session = New-PSSession -ComputerName "<PepVM>" -ConfigurationName PrivilegedEndpoint -Credential $Creds

     # Create a self-signed certificate for testing purposes. 
     $NewCert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=<YourAppName>" -KeySpec KeyExchange
     # In production, use Get-Item and a managed certificate instead.
     # $Cert = Get-Item "<YourCertificateLocation>"

     # Use the privileged endpoint to update the certificate thumbprint, used by the service principal associated with <AppIdentifier>
     $SpObject = Invoke-Command -Session $Session -ScriptBlock {Set-GraphApplication -ApplicationIdentifier "<AppIdentifier>" -ClientCertificates $using:NewCert}
     $Session | Remove-PSSession

     # Output the updated service principal details
     $SpObject
     ```

2. Betik tamamlandıktan sonra, yeni otomatik olarak imzalanan sertifika için parmak izi değeri de dahil olmak üzere güncelleştirilmiş uygulama kayıt bilgilerini görüntüler.

     ```Shell  
     ApplicationIdentifier : S-1-5-21-1512385356-3796245103-1243299919-1356
     ClientId              : 
     Thumbprint            : AF22EE716909041055A01FE6C6F5C5CDE78948E9
     ApplicationName       : Azurestack-MyApp-c30febe7-1311-4fd8-9077-3d869db28342
     ClientSecret          : 
     PSComputerName        : azs-ercs01
     RunspaceId            : a580f894-8f9b-40ee-aa10-77d4d142b4e5
     ```

### <a name="create-a-service-principal-that-uses-client-secret-credentials"></a>İstemci gizli kimlik bilgilerini kullanan bir hizmet sorumlusu oluşturma

> [!IMPORTANT]
> Bir istemci parolasının kullanılması x509 sertifika kimlik bilgisi kullanmaktan daha az güvenlidir. Kimlik doğrulama mekanizması daha az güvenlidir, ancak genellikle, istemci uygulama kaynak kodunda gizli anahtar katıştırmayı gerektirir. Bu nedenle, üretim uygulamaları için bir sertifika kimlik bilgisi kullanmanız önemle önerilir.

Şimdi başka bir uygulama kaydı oluşturursunuz, ancak bu kez bir istemci gizli kimlik bilgileri belirtin. Sertifika kimlik bilgilerinden farklı olarak, dizin bir istemci gizli kimlik bilgisi oluşturma olanağına sahiptir. Bu nedenle, istemci parolasını belirtmek yerine, oluşturulmasını istemek için `-GenerateClientSecret` anahtarını kullanırsınız. Aşağıdaki yer tutucular için kendi değerlerinizi değiştirin:

| Yer tutucu | Açıklama | Örnek |
| ----------- | ----------- | ------- |
| \<PepVM\> | Azure Stack örneğindeki ayrıcalıklı uç nokta VM 'sinin adı. | "AzS-ERCS01" |
| \<Uygulamanızınadı\> | Yeni uygulama kaydı için açıklayıcı bir ad | "Yönetim aracınız" |

1. Yükseltilmiş bir Windows PowerShell oturumu açın ve aşağıdaki cmdlet 'leri çalıştırın:

     ```powershell  
     # Sign in to PowerShell interactively, using credentials that have access to the VM running the Privileged Endpoint (typically <domain>\cloudadmin)
     $Creds = Get-Credential

     # Create a PSSession to the Privileged Endpoint VM
     $Session = New-PSSession -ComputerName "<PepVM>" -ConfigurationName PrivilegedEndpoint -Credential $Creds

     # Use the privileged endpoint to create the new app registration (and service principal object)
     $SpObject = Invoke-Command -Session $Session -ScriptBlock {New-GraphApplication -Name "<YourAppName>" -GenerateClientSecret}
     $AzureStackInfo = Invoke-Command -Session $Session -ScriptBlock {Get-AzureStackStampInformation}
     $Session | Remove-PSSession

     # Using the stamp info for your Azure Stack instance, populate the following variables:
     # - AzureRM endpoint used for Azure Resource Manager operations 
     # - Audience for acquiring an OAuth token used to access Graph API 
     # - GUID of the directory tenant
     $ArmEndpoint = $AzureStackInfo.TenantExternalEndpoints.TenantResourceManager
     $GraphAudience = "https://graph." + $AzureStackInfo.ExternalDomainFQDN + "/"
     $TenantID = $AzureStackInfo.AADTenantID

     # Register and set an AzureRM environment that targets your Azure Stack instance
     Add-AzureRMEnvironment -Name "AzureStackUser" -ArmEndpoint $ArmEndpoint
     Set-AzureRmEnvironment -Name "AzureStackUser" -GraphAudience $GraphAudience -EnableAdfsAuthentication:$true

     # Sign in using the new service principal identity
     $securePassword = $SpObject.ClientSecret | ConvertTo-SecureString -AsPlainText -Force
     $credential = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $SpObject.ClientId, $securePassword
     $SpSignin = Connect-AzureRmAccount -Environment "AzureStackUser" -ServicePrincipal -Credential $credential -TenantId $TenantID

     # Output the service principal details
     $SpObject
     ```

2. Betik tamamlandıktan sonra, hizmet sorumlusunun kimlik bilgileri de dahil olmak üzere uygulama kayıt bilgilerini görüntüler. Gösterildiği `ClientID` gibi, hizmet sorumlusunun kimliği `ClientSecret` altında oturum açmak için ve oluşturulur. Başarılı oturum açma işlemi tamamlandıktan sonra, hizmet sorumlusu kimliği, sonraki yetkilendirme ve Azure Resource Manager tarafından yönetilen kaynaklara erişim için kullanılacaktır.

     ```shell  
     ApplicationIdentifier : S-1-5-21-1634563105-1224503876-2692824315-2623
     ClientId              : 8e0ffd12-26c8-4178-a74b-f26bd28db601
     Thumbprint            : 
     ApplicationName       : Azurestack-YourApp-6967581b-497e-4f5a-87b5-0c8d01a9f146
     ClientSecret          : 6RUWLRoBw3EebBLgaWGiowCkoko5_j_ujIPjA8dS
     PSComputerName        : azs-ercs01
     RunspaceId            : 286daaa1-c9a6-4176-a1a8-03f543f90998
     ```

Sonraki bölümde bulunan `ApplicationIdentifier` değerle birlikte kullanırken PowerShell konsolu oturumunuzu açık tutun.

### <a name="update-a-service-principals-client-secret"></a>Hizmet sorumlusunun istemci parolasını güncelleştirme

İstemci gizliliğini anında değiştiren **Resetclientsecret** parametresini kullanarak PowerShell kullanarak istemci gizli kimlik bilgilerini güncelleştirin. Aşağıdaki yer tutucular için kendi değerlerinizi değiştirin:

| Yer tutucu | Açıklama | Örnek |
| ----------- | ----------- | ------- |
| \<PepVM\> | Azure Stack örneğindeki ayrıcalıklı uç nokta VM 'sinin adı. | "AzS-ERCS01" |
| \<Appıdentifier\> | Uygulama kaydına atanan tanımlayıcı | "S-1-5-21-1634563105-1224503876-2692824315-2623" |

1. Yükseltilmiş Windows PowerShell oturumunuzu kullanarak aşağıdaki cmdlet 'leri çalıştırın:

     ```powershell
     # Create a PSSession to the PrivilegedEndpoint VM
     $Session = New-PSSession -ComputerName "<PepVM>" -ConfigurationName PrivilegedEndpoint -Credential $Creds

     # Use the privileged endpoint to update the client secret, used by the service principal associated with <AppIdentifier>
     $SpObject = Invoke-Command -Session $Session -ScriptBlock {Set-GraphApplication -ApplicationIdentifier "<AppIdentifier>" -ResetClientSecret}
     $Session | Remove-PSSession

     # Output the updated service principal details
     $SpObject
     ```

2. Betik tamamlandıktan sonra, yeni oluşturulan istemci parolası da dahil olmak üzere güncelleştirilmiş uygulama kayıt bilgilerini görüntüler.

     ```shell  
     ApplicationIdentifier : S-1-5-21-1634563105-1224503876-2692824315-2623
     ClientId              : 8e0ffd12-26c8-4178-a74b-f26bd28db601
     Thumbprint            : 
     ApplicationName       : Azurestack-YourApp-6967581b-497e-4f5a-87b5-0c8d01a9f146
     ClientSecret          : MKUNzeL6PwmlhWdHB59c25WDDZlJ1A6IWzwgv_Kn
     PSComputerName        : azs-ercs01
     RunspaceId            : 6ed9f903-f1be-44e3-9fef-e7e0e3f48564
     ```

### <a name="remove-a-service-principal"></a>Hizmet sorumlusunu kaldırma

Artık dizininizden bir uygulama kaydını ve PowerShell 'i kullanarak ilişkili hizmet sorumlusu nesnesini kaldırma/silme hakkında bilgi edineceksiniz. 

Aşağıdaki yer tutucular için kendi değerlerinizi değiştirin:

| Yer tutucu | Açıklama | Örnek |
| ----------- | ----------- | ------- |
| \<PepVM\> | Azure Stack örneğindeki ayrıcalıklı uç nokta VM 'sinin adı. | "AzS-ERCS01" |
| \<Appıdentifier\> | Uygulama kaydına atanan tanımlayıcı | "S-1-5-21-1634563105-1224503876-2692824315-2623" |

```powershell  
# Sign in to PowerShell interactively, using credentials that have access to the VM running the Privileged Endpoint (typically <domain>\cloudadmin)
$Creds = Get-Credential

# Create a PSSession to the PrivilegedEndpoint VM
$Session = New-PSSession -ComputerName "<PepVM>" -ConfigurationName PrivilegedEndpoint -Credential $Creds

# OPTIONAL: Use the privileged endpoint to get a list of applications registered in AD FS
$AppList = Invoke-Command -Session $Session -ScriptBlock {Get-GraphApplication}

# Use the privileged endpoint to remove the application and associated service principal object for <AppIdentifier>
Invoke-Command -Session $Session -ScriptBlock {Remove-GraphApplication -ApplicationIdentifier "<AppIdentifier>"}
```

Ayrıcalıklı uç noktada Remove-GraphApplication cmdlet 'ini çağırarak hiçbir çıkış döndürülmeyecektir, ancak cmdlet 'in yürütülmesi sırasında konsola tam onay çıktısı görürsünüz:

```shell
VERBOSE: Deleting graph application with identifier S-1-5-21-1634563105-1224503876-2692824315-2623.
VERBOSE: Remove-GraphApplication : BEGIN on AZS-ADFS01 on ADFSGraphEndpoint
VERBOSE: Application with identifier S-1-5-21-1634563105-1224503876-2692824315-2623 was deleted.
VERBOSE: Remove-GraphApplication : END on AZS-ADFS01 under ADFSGraphEndpoint configuration
```

## <a name="assign-a-role"></a>Rol atama

Kullanıcılara ve uygulamalara göre Azure kaynaklarına erişim, rol tabanlı Access Control (RBAC) aracılığıyla yetkilendirilir. Bir uygulamanın hizmet sorumlusunu kullanarak aboneliğinizdeki kaynaklara erişmesine izin vermek için, hizmet sorumlusunu belirli bir *kaynak*için bir *role* *atamanız* gerekir. İlk olarak, uygulamanın hangi rolün doğru *izinleri* temsil ettiğini belirleyin. Kullanılabilir roller hakkında bilgi edinmek için bkz. [Azure kaynakları Için yerleşik roller](/azure/role-based-access-control/built-in-roles).

Seçtiğiniz kaynak türü, hizmet sorumlusu için *erişim kapsamını* da belirler. Erişim kapsamını abonelikte, kaynak grubunda veya kaynak düzeyinde ayarlayabilirsiniz. Daha düşük düzeyde kapsam için izinler devralınmıştır. Örneğin, bir kaynak grubu için "okuyucu" rolüne bir uygulama eklemek, kaynak grubunu ve içerdiği kaynakları okuyabileceği anlamına gelir.

1. Azure Stack yüklemesi sırasında belirttiğiniz dizine (örneğin, Azure AD Azure portal veya AD FS için Azure Stack Kullanıcı portalına) bağlı olarak uygun portalda oturum açın. Bu örnekte, Azure Stack Kullanıcı portalında oturum açmış bir Kullanıcı gösteririz.

   > [!NOTE]
   > Belirli bir kaynağa rol atamaları eklemek için, Kullanıcı hesabınızın `Microsoft.Authorization/roleAssignments/write` izin bildiren bir role ait olması gerekir. Örneğin, [sahip](/azure/role-based-access-control/built-in-roles#owner) veya [Kullanıcı erişimi Yöneticisi](/azure/role-based-access-control/built-in-roles#user-access-administrator) yerleşik rolleridir.  
2. Hizmet sorumlusunun erişimine izin vermek istediğiniz kaynağa gidin. Bu örnekte, abonelik kapsamındaki bir role hizmet sorumlusunu, **abonelikler**' i ve ardından belirli bir aboneliği seçerek atayın. Bunun yerine, bir kaynak grubu veya bir sanal makine gibi belirli bir kaynağı seçebilirsiniz. 

     ![Atama için abonelik seçin](./media/azure-stack-create-service-principal/select-subscription.png)

3. RBAC 'yi destekleyen tüm kaynaklar genelinde evrensel olan **Access Control (IAM)** sayfasını seçin.
4. **+ Ekle** ' yi seçin
5. **Rol**altında, uygulamaya atamak istediğiniz rolü seçin.
6. **Seç**' in altında, tam veya kısmi uygulama adı kullanarak uygulamanızı arayın. Kayıt sırasında, uygulama adı *azurestack\<-YourAppName-\>\<ClientID\>* olarak oluşturulur. Örneğin, *app2*uygulamasının bir uygulama adını kullandıysanız ve oluşturma sırasında ClientID *2bbe67d8-3FDB-4b62-87cf-cc41dd4344ff* atandıktan sonra tam ad *azurestack-app2-2bbe67d8-3FDB-4b62-87cf-cc41dd4344ff*olur. Tam dize veya *azurestack* ya da *azurestack-app2*gibi bir bölüm için arama yapabilirsiniz.
7. Uygulamayı bulduktan sonra bunu seçin ve **Seçilen Üyeler**altında görünür.
8. Seçin **Kaydet** rol atama tamamlanması. 

     [![Rol ata](media/azure-stack-create-service-principal/assign-role.png)](media/azure-stack-create-service-principal/assign-role.png#lightbox)

9. İşlem tamamlandığında, uygulama, belirtilen rol için geçerli kapsam için atanan sorumlular listesinde gösterilir.

     [![Atanan rol](media/azure-stack-create-service-principal/assigned-role.png)](media/azure-stack-create-service-principal/assigned-role.png#lightbox)

Bir hizmet sorumlusu oluşturup bir rol atadığınıza göre, Azure Stack kaynaklarına erişmek için uygulamanızı içinde kullanmaya başlayabilirsiniz.  

## <a name="next-steps"></a>Sonraki adımlar

[Kullanıcıları AD FS’ye ekleme](azure-stack-add-users-adfs.md)  
[Kullanıcı izinlerini yönetme](azure-stack-manage-permissions.md)  
[Azure Active Directory Belgeleri](https://docs.microsoft.com/azure/active-directory)  
[Active Directory Federasyon Hizmetleri](https://docs.microsoft.com/windows-server/identity/active-directory-federation-services)
