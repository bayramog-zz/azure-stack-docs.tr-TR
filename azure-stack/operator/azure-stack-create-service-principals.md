---
title: Kaynaklara erişmek için bir uygulama kimliğini kullan
description: Rol tabanlı erişim denetimi ile oturum açın ve kaynaklara erişim için kullanılan bir hizmet sorumlusu yönetme işlemi açıklanır.
services: azure-resource-manager
documentationcenter: na
author: BryanLa
manager: femila
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/25/2019
ms.author: bryanla
ms.lastreviewed: 06/20/2019
ms.openlocfilehash: 8c27948185df5f98926a3500db0981a1ccddc321
ms.sourcegitcommit: c9d11be7d27c73797bdf279d4fcabb7a22451541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67397322"
---
# <a name="use-an-app-identity-to-access-resources"></a>Kaynaklara erişmek için bir uygulama kimliğini kullan

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack geliştirme Seti'ni (ASDK)*

Dağıtma veya Azure Resource Manager üzerinden kaynaklarını yapılandırmak için gereken bir uygulama gerekir temsil edilemiyor bir hizmet sorumlusu. Yalnızca bir kullanıcı kullanıcı sorumlusu tarafından temsil edilen gibi bir hizmet sorumlusu uygulama temsil eden güvenlik sorumlusu türüdür. Hizmet sorumlusu, bu hizmet sorumlusu yalnızca gerekli izinleri devretmek için izin vererek, uygulamanız için bir kimlik sağlar.  

Örneğin, Azure kaynaklarını envantere almak üzere Azure Resource Manager'ı kullanan bir yapılandırma yönetim uygulaması olabilir. Bu senaryoda, bir hizmet sorumlusu oluşturma, bu hizmet sorumlusu okuyucu rolüne verin ve salt okunur erişim için yapılandırma yönetimi uygulamasını sınırlandırmak. 

## <a name="overview"></a>Genel Bakış

Benzer şekilde bir kullanıcı asıl, bir hizmet sorumlusu kimlik doğrulaması sırasında iki öğeden oluşur kimlik sunması gerekir:

- Bir **uygulama kimliği**bazen denilen bir istemci kimliği Bu, Active Directory kiracınızdaki uygulamanın kaydı benzersiz olarak tanımlayan bir GUID'dir.
- A **gizli** uygulama kimliği ile ilişkili (Parola gibi) bir istemci gizli dizesini oluştur veya x X509 belirtin (Bu, ortak anahtarı kullanır) sertifika. 

Bir hizmetin kimliğini altında uygulama asıl çalıştıran, bir kullanıcı altında asıl olmadığından çalışan için tercih edilir:

 - Bir hizmet sorumlusu x X509 kullanabilirsiniz sertifika için **daha güçlü kimlik bilgileri**.  
 - Atayabileceğiniz **daha kısıtlayıcı izinlerle** bir hizmet sorumlusu için. Genellikle, bu izinleri yalnızca hangi uygulamanın, bilinen yapması için kısıtlı *en az ayrıcalık ilkesini*.
 - Hizmet sorumlusu **kimlik bilgileri ve izinleri değişmez sık** kullanıcı kimlik bilgileri olarak. Örneğin, kullanıcının sorumlulukları değiştirdiğinizde, bir değişiklik parola gereksinimlerini dikte veya bir kullanıcı şirketten ayrılması.

İlişkili bir oluşturur, dizinde yeni bir uygulama kaydı oluşturarak başlayın [hizmet sorumlusu nesnesi](/azure/active-directory/develop/developer-glossary#service-principal-object) dizininden uygulamanın kimliğini temsil etmek için. Bu belgede, oluşturma ve Azure Stack Örneğiniz için seçtiğiniz dizine bağlı olarak bir hizmet sorumlusu yönetme işlemi açıklanmaktadır:

- Azure Active Directory (Azure AD). Azure AD sağlayan bir çok kiracılı, bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Azure AD ile bağlı bir Azure Stack örneği kullanabilirsiniz.
- Active Directory Federasyon Hizmetleri (AD FS). AD FS Basitleştirilmiş, güvenli Kimlik Federasyonu ve Web'de çoklu oturum açma (SSO) özellikleri sağlar. AD FS ile Azure Stack örnekleri bağlı ve bağlantısı kesilmiş kullanabilirsiniz.

Öncelikle, bir hizmet sorumlusu yönetme ve hizmet sorumlusu, rol atama kaynak erişimini sınırlandırma öğrenin.

## <a name="manage-an-azure-ad-service-principal"></a>Bir Azure AD hizmet sorumlusunu Yönet 

Azure Active Directory (Azure AD) ile Azure Stack, kimlik yönetimi hizmeti olarak dağıttıysanız, Azure için gibi hizmet sorumluları oluşturabilirsiniz. Bu bölümde Azure portalı üzerinden adımların nasıl gerçekleştirileceğini gösterir. Sahip olduğunuz denetimi [Azure AD izinleri gerekli](/azure/active-directory/develop/howto-create-service-principal-portal#required-permissions) başlamadan önce.

### <a name="create-a-service-principal-that-uses-a-client-secret-credential"></a>İstemci gizli kimlik bilgilerini kullanan bir hizmet sorumlusu oluşturma

Bu bölümde, Azure AD kiracınızda hizmet sorumlusu nesnesi oluşturur Azure portalını kullanarak uygulamanızı kaydedin. Bu örnekte, bir istemci gizli dizi kimlik bilgisi ile hizmet sorumlusu oluşturulur, ancak portal ayrıca X509 destekler sertifika tabanlı kimlik bilgileri.

1. Oturum [Azure portalında](https://portal.azure.com) Azure hesabınızı kullanarak.
2. Seçin **Azure Active Directory** > **uygulama kayıtları** > **yeni kayıt**.
3. Sağlayan bir **adı** uygulama için. 
4. Uygun seçin **desteklenen hesap türleri**.
5. Altında **yeniden yönlendirme URI'si**seçin **Web** uygulama türü olarak ve uygulamanız gerekiyorsa bir yeniden yönlendirme URI'si (isteğe bağlı olarak) belirtin. 
6. Değerleri ayarladıktan sonra seçin **kaydetme**. Uygulama kaydı oluşturulur ve **genel bakış** sayfa sunulur.
7. Kopyalama **uygulama kimliği** uygulama kodunuzda kullanmak için. Bu değer aynı zamanda istemci kimliği olarak adlandırılır
8. İstemci gizli anahtarı oluşturmak üzere **sertifikaları ve parolaları** sayfası. **Yeni istemci gizli dizisi**’ni seçin.
9. Sağlayan bir **açıklaması** için gizli ve bir **sona** süresi. 
10. İşiniz bittiğinde, seçin **Ekle**.
11. Gizli dizi değeri görüntülenir. Kopyalayın ve daha sonra geri alamazsınız, çünkü bu değeri başka bir konuma kaydedin. Uygulama kimliği ile hizmet sorumlusu oturum açma sırasında istemci uygulamanızda gizli dizi sağlar. 

    ![kaydedilen anahtar](./media/azure-stack-create-service-principal/create-service-principal-in-azure-stack-secret.png)

## <a name="manage-an-ad-fs-service-principal"></a>Bir AD FS hizmet sorumlusunu Yönet

Active Directory Federasyon Hizmetleri (AD FS) ile Azure Stack, kimlik yönetimi hizmeti olarak dağıttıysanız, hizmet sorumlusu yönetmek için PowerShell kullanmanız gerekir. Örnekler aşağıda verilmiştir hem x X509 gösteren hizmet sorumlusu kimlik bilgilerini yönetmek için sertifika ve istemci gizli anahtarı.

Betikleri Azure Stack Örneğiniz için ayrıcalıklı bir uç noktasını barındıran bir VM için başka bir oturum açan bir yükseltilmiş ("Yönetici olarak çalıştır") PowerShell konsolunda çalıştırmalısınız. Ayrıcalıklı uç nokta oturum kurulduktan sonra diğer cmdlet'lerle yürütün ve hizmet sorumlusunu Yönet. Ayrıcalıklı uç noktası hakkında daha fazla bilgi için bkz: [Azure Stack'te ayrıcalıklı uç noktayı kullanarak](azure-stack-privileged-endpoint.md).

### <a name="create-a-service-principal-that-uses-a-certificate-credential"></a>Sertifika kimlik bilgilerini kullanan bir hizmet sorumlusu oluşturma

Bir hizmet sorumlusu kimlik bilgileri için bir sertifika oluştururken, aşağıdaki gereksinimler karşılanmalıdır:

 - Şifreleme hizmeti sağlayıcısı (CSP), eski anahtar sağlayıcısı olmalıdır.
 - Ortak ve özel anahtarlar gerektiğinde PFX dosyasını sertifika biçimi olmalıdır. Windows sunucuları, ortak anahtar dosyasını (SSL sertifika dosyası) içeren .pfx dosyaları ve ilişkili özel anahtar dosyasını kullanın.
 - Üretim için sertifika bir iç sertifika yetkilisi veya bir ortak sertifika yetkilisi verilmiş olması gerekir. Bir ortak sertifika yetkilisi kullanmanız durumunda, içermesi yetkilisi temel işletim sistemi görüntüsüne Microsoft güvenilir kök yetkilisi programının bir parçası olarak. Tam listesini bulabilirsiniz [Microsoft güvenilen kök sertifika programı: Katılımcıların](https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca).
 - Azure Stack altyapınızı, sertifika yetkilisinin sertifika iptal listesi (CRL) konumuna sertifikada yayımlanan ağ erişimi olması gerekir. Bu CRL bir HTTP uç noktası olmalıdır.

Bir sertifika aldıktan sonra uygulamanızı kaydedin ve hizmet sorumlusu oluşturmak için aşağıdaki PowerShell betiğini kullanın. Ayrıca Azure'da oturum açmak için hizmet sorumlusu kullanın. Aşağıdaki yer tutucuları için kendi değerlerinizi yerleştirin:

| Yer tutucu | Açıklama | Örnek |
| ----------- | ----------- | ------- |
| \<PepVM\> | Ayrıcalıklı uç noktada VM, Azure Stack örneğinizin adı. | "AzS-ERCS01" |
| \<YourCertificateLocation\> | X509 konumunu yerel sertifika deposunda sertifika. | "Cert:\CurrentUser\My\AB5A8A3533CC7AA2025BF05120117E06DE407B34" |
| \<Uygulamanızınadı\> | Yeni uygulama kaydı için açıklayıcı bir ad | "Yönetim Aracı my" |

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
   
2. Betik bittikten sonra hizmet sorumlusunun kimlik bilgileri de dahil olmak üzere uygulama kayıt bilgileri görüntüler. Gösterildiği gibi `ClientID` ve `Thumbprint` hizmet sorumlusunun kimlik altında oturum açmak için kullanılır. Başarıyla oturum açıldığında, hizmet sorumlusu kimliği sonraki yetkilendirme ve Azure Resource Manager tarafından yönetilen kaynaklara erişim için kullanılır. 

   ```shell
   ApplicationIdentifier : S-1-5-21-1512385356-3796245103-1243299919-1356
   ClientId              : 3c87e710-9f91-420b-b009-31fa9e430145
   Thumbprint            : 30202C11BE6864437B64CE36C8D988442082A0F1
   ApplicationName       : Azurestack-MyApp-c30febe7-1311-4fd8-9077-3d869db28342
   ClientSecret          :
   PSComputerName        : azs-ercs01
   RunspaceId            : a78c76bb-8cae-4db4-a45a-c1420613e01b
   ```

İle kullandığınız gibi PowerShell Konsolu oturumunuzun açık tutun `ApplicationIdentifier` sonraki bölümde değeri.

### <a name="update-a-service-principals-certificate-credential"></a>Bir hizmet sorumlusunun sertifika kimlik bilgilerini güncelleştirme

Bir hizmet sorumlusu oluşturulur, bu bölümde şunları nasıl yapacağınızı gösterilecek için:

1. Yeni bir otomatik olarak imzalanan X509 oluşturma test etmek için sertifika.
2. Hizmet sorumlusunun kimlik bilgilerini güncelleştirmek, **parmak izi** yeni sertifikayı eşleştirilecek özelliği.

Aşağıdaki yer tutucular yerine kendi değerlerinizi koyarak, PowerShell kullanarak sertifika kimlik bilgilerini güncelleştirin:

| Yer tutucu | Açıklama | Örnek |
| ----------- | ----------- | ------- |
| \<PepVM\> | Ayrıcalıklı uç noktada VM, Azure Stack örneğinizin adı. | "AzS-ERCS01" |
| \<Uygulamanızınadı\> | Yeni uygulama kaydı için açıklayıcı bir ad | "Yönetim Aracı my" |
| \<YourCertificateLocation\> | X509 konumunu yerel sertifika deposunda sertifika. | "Cert:\CurrentUser\My\AB5A8A3533CC7AA2025BF05120117E06DE407B34" |
| \<AppIdentifier\> | Uygulama kaydı için atanan tanımlayıcı | "S-1-5-21-1512385356-3796245103-1243299919-1356" |

1. Yükseltilmiş Windows PowerShell oturumunuza kullanarak, aşağıdaki cmdlet'leri çalıştırın:

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

2. Betik bittikten sonra otomatik olarak imzalanan yeni sertifika parmak izi değeri de dahil olmak üzere güncelleştirilmiş uygulama kayıt bilgi görüntüler.

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
> İstemci gizli anahtarı kullanarak x X509 kullanmaktan daha az güvenli kimlik bilgisi sertifikası. Yalnızca kimlik doğrulama mekanizması daha az güvenlidir, ancak ayrıca genellikle istemci uygulamanın kaynak kodunda gizli dizi ekleme gerektirir. Bu nedenle, üretim uygulamaları için sertifika kimlik bilgilerini kullanmak için önerilir.

Artık, başka bir uygulama kaydı oluşturma, ancak bu kez, istemci gizli kimlik bilgilerini belirtin. Sertifika kimlik bilgilerini farklı olarak, dizin istemci gizli kimlik bilgilerini oluşturmak için özelliğine sahiptir. Kullandığınız istemci gizli anahtarı belirtmek yerine, bu nedenle `-GenerateClientSecret` oluşturulmuş olduğunu istemek için anahtar. Aşağıdaki yer tutucuları için kendi değerlerinizi yerleştirin:

| Yer tutucu | Açıklama | Örnek |
| ----------- | ----------- | ------- |
| \<PepVM\> | Ayrıcalıklı uç noktada VM, Azure Stack örneğinizin adı. | "AzS-ERCS01" |
| \<Uygulamanızınadı\> | Yeni uygulama kaydı için açıklayıcı bir ad | "Yönetim Aracı my" |

1. Yükseltilmiş bir Windows PowerShell oturumu açın ve aşağıdaki cmdlet'leri çalıştırın:

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

2. Betik bittikten sonra hizmet sorumlusunun kimlik bilgileri de dahil olmak üzere uygulama kayıt bilgileri görüntüler. Gösterildiği gibi `ClientID` ve oluşturulan `ClientSecret` hizmet sorumlusunun kimlik altında oturum açmak için kullanılır. Başarıyla oturum açıldığında, hizmet sorumlusu kimliği sonraki yetkilendirme ve Azure Resource Manager tarafından yönetilen kaynaklara erişim için kullanılır.

     ```shell  
     ApplicationIdentifier : S-1-5-21-1634563105-1224503876-2692824315-2623
     ClientId              : 8e0ffd12-26c8-4178-a74b-f26bd28db601
     Thumbprint            : 
     ApplicationName       : Azurestack-YourApp-6967581b-497e-4f5a-87b5-0c8d01a9f146
     ClientSecret          : 6RUWLRoBw3EebBLgaWGiowCkoko5_j_ujIPjA8dS
     PSComputerName        : azs-ercs01
     RunspaceId            : 286daaa1-c9a6-4176-a1a8-03f543f90998
     ```

İle kullandığınız gibi PowerShell Konsolu oturumunuzun açık tutun `ApplicationIdentifier` sonraki bölümde değeri.

### <a name="update-a-service-principals-client-secret"></a>Bir hizmet sorumlusunun istemci gizli anahtarı güncelleştirme

PowerShell kullanarak, kullanarak istemci gizli kimlik bilgilerini güncelleştirme **ResetClientSecret** parametresini hemen gizli değiştirir. Aşağıdaki yer tutucuları için kendi değerlerinizi yerleştirin:

| Yer tutucu | Açıklama | Örnek |
| ----------- | ----------- | ------- |
| \<PepVM\> | Ayrıcalıklı uç noktada VM, Azure Stack örneğinizin adı. | "AzS-ERCS01" |
| \<AppIdentifier\> | Uygulama kaydı için atanan tanımlayıcı | "S-1-5-21-1634563105-1224503876-2692824315-2623" |

1. Yükseltilmiş Windows PowerShell oturumunuza kullanarak, aşağıdaki cmdlet'leri çalıştırın:

     ```powershell
     # Create a PSSession to the PrivilegedEndpoint VM
     $Session = New-PSSession -ComputerName "<PepVM>" -ConfigurationName PrivilegedEndpoint -Credential $Creds

     # Use the privileged endpoint to update the client secret, used by the service principal associated with <AppIdentifier>
     $SpObject = Invoke-Command -Session $Session -ScriptBlock {Set-GraphApplication -ApplicationIdentifier "<AppIdentifier>" -ResetClientSecret}
     $Session | Remove-PSSession

     # Output the updated service principal details
     $SpObject
     ```

2. Betik bittikten sonra yeni oluşturulan istemci gizli anahtarı dahil olmak üzere güncelleştirilmiş uygulama kayıt bilgi görüntüler.

     ```shell  
     ApplicationIdentifier : S-1-5-21-1634563105-1224503876-2692824315-2623
     ClientId              : 8e0ffd12-26c8-4178-a74b-f26bd28db601
     Thumbprint            : 
     ApplicationName       : Azurestack-YourApp-6967581b-497e-4f5a-87b5-0c8d01a9f146
     ClientSecret          : MKUNzeL6PwmlhWdHB59c25WDDZlJ1A6IWzwgv_Kn
     PSComputerName        : azs-ercs01
     RunspaceId            : 6ed9f903-f1be-44e3-9fef-e7e0e3f48564
     ```

### <a name="remove-a-service-principal"></a>Bir hizmet sorumlusunu kaldırma

Artık PowerShell kullanarak nasıl dizin ve onun ilişkili hizmet sorumlusu nesnesi bir uygulama kaydı kaldırma/silme göreceksiniz. 

Aşağıdaki yer tutucuları için kendi değerlerinizi yerleştirin:

| Yer tutucu | Açıklama | Örnek |
| ----------- | ----------- | ------- |
| \<PepVM\> | Ayrıcalıklı uç noktada VM, Azure Stack örneğinizin adı. | "AzS-ERCS01" |
| \<AppIdentifier\> | Uygulama kaydı için atanan tanımlayıcı | "S-1-5-21-1634563105-1224503876-2692824315-2623" |

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

Ayrıcalıklı uç noktada Remove-GraphApplication cmdlet'ini çağırma döndürülen çıkış olacaktır, ancak konsola çıktı verbatim onay cmdlet'inin yürütülmesi sırasında görürsünüz:

```shell
VERBOSE: Deleting graph application with identifier S-1-5-21-1634563105-1224503876-2692824315-2623.
VERBOSE: Remove-GraphApplication : BEGIN on AZS-ADFS01 on ADFSGraphEndpoint
VERBOSE: Application with identifier S-1-5-21-1634563105-1224503876-2692824315-2623 was deleted.
VERBOSE: Remove-GraphApplication : END on AZS-ADFS01 under ADFSGraphEndpoint configuration
```

## <a name="assign-a-role"></a>Rol atama

Rol tabanlı erişim denetimi (RBAC) yetkili kullanıcılar ve uygulamalar tarafından Azure kaynaklarına erişimi. Kendi hizmet sorumlusunu kullanarak aboneliğinizdeki kaynaklara erişmek için bir uygulama izin vermek için şunları yapmalısınız *atama* hizmet sorumlusu için bir *rol* belirli *kaynak*. Hangi rol hakkını temsil eden ilk karar *izinleri* uygulama için. Kullanılabilir roller hakkında bilgi edinmek için [Azure kaynakları için yerleşik roller](/azure/role-based-access-control/built-in-roles).

Ayrıca seçtiğiniz kaynak türünü kurar *erişim kapsamının* hizmet sorumlusu için. Abonelik, kaynak grubu veya kaynak düzeyinde erişim kapsamı ayarlayabilirsiniz. Daha düşük düzeyde kapsam için izinler devralınmıştır. Örneğin, bir kaynak grubu için "Okuyucu" rolünü bir uygulamaya eklemek, kaynak grubunu ve içerdiği tüm kaynakları okuyun anlamına gelir.

1. Uygun portalı oturum açma sırasında Azure Stack belirtilen dizine göre yükleme (Azure AD için Azure portalı veya örneğin, AD FS için Azure Stack Kullanıcı Portalı). Bu örnekte, bir kullanıcı Azure Stack kullanıcı portalında oturum açmıştır göstereceğiz.

   > [!NOTE]
   > Belirli bir kaynak için rol atamalarını eklemek için kullanıcı hesabınızın bildiren bir role ait `Microsoft.Authorization/roleAssignments/write` izni. Örneğin, ya da [sahibi](/azure/role-based-access-control/built-in-roles#owner) veya [kullanıcı erişimi Yöneticisi](/azure/role-based-access-control/built-in-roles#user-access-administrator) yerleşik roller.  
2. Hizmet asıl erişimine izin vermek istediğiniz kaynağa gidin. Bu örnekte, hizmet sorumlusu abonelik kapsamda bir rol seçerek atama **abonelikleri**, ardından belirli bir abonelik. Bunun yerine, bir kaynak grubu veya bir sanal makine gibi belirli bir kaynak seçebilirsiniz. 

     ![Abonelik atama için seçin](./media/azure-stack-create-service-principal/select-subscription.png)

3. Seçin **erişim denetimi (IAM)** RBAC destekleyen tüm kaynaklar arasında Evrensel sayfasında.
4. Seçin **+ Ekle**
5. Altında **rol**, uygulamayı atamak istediğiniz rolü seçin.
6. Altında **seçin**, tam veya kısmi bir uygulama adı kullanarak uygulamanızı arayın. Kayıt sırasında uygulama adı olarak oluşturulan *Azurestack -\<Uygulamanızınadı\>-\<ClientID\>* . Örneğin, bir uygulama adını kullandıysanız *App2*ve ClientID *2bbe67d8-3fdb-4b62-87cf-cc41dd4344ff* atandığı oluşturma sırasında tam ad olacaktır  *Azurestack-App2-2bbe67d8-3fdb-4b62-87cf-cc41dd4344ff*. Tam dize veya bir bölümü için gibi arayabilirsiniz *Azurestack* veya *Azurestack App2*.
7. Uygulamayı bulduktan sonra onu seçin ve bunu görünür **seçili üyeleri**.
8. Seçin **Kaydet** rol atama tamamlanması. 

     [ ![Rol atama](media/azure-stack-create-service-principal/assign-role.png)](media/azure-stack-create-service-principal/assign-role.png#lightbox)

9. İşiniz bittiğinde uygulamayı belirli rol için geçerli kapsam için atanan ilkeleri listesinde gösterilir.

     [ ![Atanan rol](media/azure-stack-create-service-principal/assigned-role.png)](media/azure-stack-create-service-principal/assigned-role.png#lightbox)

Bir hizmet sorumlusu oluşturuldu ve atanan role göre bu Azure Stack kaynaklara erişmek için uygulamanızı içinde kullanmaya başlayabilirsiniz.  

## <a name="next-steps"></a>Sonraki adımlar

[Kullanıcıları AD FS’ye ekleme](azure-stack-add-users-adfs.md)  
[Kullanıcı izinlerini yönetme](azure-stack-manage-permissions.md)  
[Azure Active Directory Belgeleri](https://docs.microsoft.com/azure/active-directory)  
[Active Directory Federasyon Hizmetleri](https://docs.microsoft.com/windows-server/identity/active-directory-federation-services)
