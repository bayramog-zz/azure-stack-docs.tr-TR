---
title: Azure Stack veri merkezi tümleştirmesi-kimlik
description: Azure Stack AD FS veri merkezinizle nasıl tümleştirileceğini öğrenin AD FS
services: azure-stack
author: PatAltimore
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 05/10/2019
ms.author: patricka
ms.reviewer: thoroet
ms.lastreviewed: 05/10/2019
ms.openlocfilehash: f51b0bdd4e433dd3083701e8cc967b3105d23ed6
ms.sourcegitcommit: 820ec8d10ddab1fee136397d3aa609e676f8b39d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71127511"
---
# <a name="azure-stack-datacenter-integration---identity"></a>Azure Stack veri merkezi tümleştirmesi-kimlik

Azure Stack, kimlik sağlayıcıları olarak Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) (AD FS) kullanarak dağıtabilirsiniz. Azure Stack dağıtmadan önce seçimi yapmalısınız. Bağlı bir senaryoda Azure AD ' yi veya AD FS seçebilirsiniz. Bağlantısı kesik bir senaryo için yalnızca AD FS desteklenir.

> [!IMPORTANT]
> Tüm Azure Stack çözümünü yeniden dağıtmaya gerek kalmadan kimlik sağlayıcısını değiştiremezsiniz.

## <a name="active-directory-federation-services-and-graph"></a>Active Directory Federasyon Hizmetleri (AD FS) ve grafik

AD FS ile dağıtmak, mevcut bir Active Directory ormanındaki kimliklerin Azure Stack kaynaklarla kimlik doğrulaması yapmasına olanak sağlar. Bu mevcut Active Directory ormanı, bir AD FS Federasyon güveni oluşturulmasına izin vermek için AD FS dağıtımı gerektirir.

Kimlik doğrulaması, kimliğin bir parçasıdır. Rol tabanlı Access Control (RBAC) Azure Stack yönetmek için Graph bileşeninin yapılandırılması gerekir. Bir kaynağa erişim atandığında, Graph bileşeni LDAP protokolünü kullanarak mevcut Active Directory ormanında Kullanıcı hesabını arar.

![Azure Stack AD FS mimarisi](media/azure-stack-integrate-identity/Azure-Stack-ADFS-architecture.png)

Mevcut AD FS, talepleri Azure Stack AD FS (kaynak STS) Gönderen hesap güvenlik belirteci hizmetidir (STS). Azure Stack, otomasyon, mevcut AD FS için meta veri uç noktasıyla talep sağlayıcı güveni oluşturur.

Mevcut AD FS, bağlı olan taraf güveninin yapılandırılması gerekir. Bu adım Otomasyon tarafından yapılmaz ve işleç tarafından yapılandırılmalıdır. AD FS için Azure Stack VIP uç noktası, model `https://adfs.<Region>.<ExternalFQDN>/`kullanılarak oluşturulabilir.

Bağlı olan taraf güveni yapılandırması, Microsoft tarafından sunulan talep dönüştürme kurallarını yapılandırmanızı de gerektirir.

Grafik yapılandırması için, var olan Active Directory okuma iznine sahip bir hizmet hesabı sağlanmalıdır. Bu hesap, RBAC senaryolarını etkinleştirmek üzere otomasyon girişi olarak gereklidir.

Son adım için, varsayılan sağlayıcı aboneliği için yeni bir sahip yapılandırılır. Bu hesabın Azure Stack yönetici portalında oturum açarken tüm kaynaklara tam erişimi vardır.

Gereksinimler:

|Bileşen|Gereksinim|
|---------|---------|
|Graf|Microsoft Active Directory 2012/2012 R2/2016|
|AD FS|Windows Server 2012/2012 R2/2016|

## <a name="setting-up-graph-integration"></a>Grafik tümleştirmeyi ayarlama

Graph yalnızca tek bir Active Directory ormanı ile tümleştirmeyi destekler. Birden çok orman varsa, kullanıcıları ve grupları getirmek için yalnızca yapılandırmada belirtilen orman kullanılacaktır.

Aşağıdaki bilgiler Otomasyon parametrelerinin girişleri olarak gereklidir:

|Parametre|Dağıtım çalışma sayfası parametresi|Açıklama|Örnek|
|---------|---------|---------|---------|
|`CustomADGlobalCatalog`|AD FS orman FQDN 'SI|Hedef Active Directory ormanın FQDN 'SI<br>ile tümleştirmek istediğiniz|Contoso.com|
|`CustomADAdminCredentials`| |LDAP okuma izni olan bir Kullanıcı|YOURDOMAIN\graphservice|

### <a name="configure-active-directory-sites"></a>Active Directory siteleri yapılandırma

Birden çok siteye sahip Active Directory dağıtımları için en yakın Active Directory sitesini Azure Stack dağıtımınıza yapılandırın. Yapılandırma Azure Stack Graph hizmetinin uzak bir siteden küresel katalog sunucusu kullanarak sorguları çözümlamalarını engeller.

Azure Stack [genel VIP ağ](azure-stack-network.md#public-vip-network) alt ağını Azure Stack en yakın Active Directory sitesine ekleyin. Örneğin, Active Directory Seattle sitesinde dağıtılan Azure Stack iki sitede Seattle ve Redmond, Seattle için Active Directory sitesine Azure Stack genel VIP ağ alt ağını eklersiniz.

Active Directory siteleri hakkında daha fazla bilgi için bkz. [site topolojisini tasarlama](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/designing-the-site-topology).

> [!Note]  
> Active Directory tek bir siteden oluşur, bu adımı atlayabilirsiniz. Bir catch-all alt ağının yapılandırılmış olması durumunda, Azure Stack genel VIP ağ alt ağının bunun bir parçası olduğunu doğrulayın.

### <a name="create-user-account-in-the-existing-active-directory-optional"></a>Mevcut Active Directory Kullanıcı hesabı oluştur (isteğe bağlı)

İsteğe bağlı olarak, mevcut Active Directory grafik hizmeti için bir hesap oluşturabilirsiniz. Kullanmak istediğiniz bir hesabınız yoksa bu adımı gerçekleştirin.

1. Mevcut Active Directory, aşağıdaki kullanıcı hesabını (öneri) oluşturun:
   - **Kullanıcı adı**: graphservice
   - **Parola**: güçlü bir parola kullanın<br>Parolayı süresiz olarak yapılandırın.

   Özel izin veya üyelik gerekmez.

#### <a name="trigger-automation-to-configure-graph"></a>Grafiği yapılandırmak için Otomasyonu tetikleme

Bu yordamda, veri merkezi ağınızda Azure Stack ayrıcalıklı uç noktayla iletişim kurabilen bir bilgisayar kullanın.

1. Yükseltilmiş bir Windows PowerShell oturumu açın (yönetici olarak çalıştır) ve ayrıcalıklı uç noktanın IP adresine bağlanın. Kimlik doğrulamak için **CloudAdmin** kimlik bilgilerini kullanın.

   ```powershell  
   $creds = Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Ayrıcalıklı uç noktaya bağlı olduğunuza göre, şu komutu çalıştırın: 

   ```powershell  
   Register-DirectoryService -CustomADGlobalCatalog contoso.com
   ```

   İstendiğinde, Graph hizmeti (graphservice gibi) için kullanmak istediğiniz kullanıcı hesabının kimlik bilgisini belirtin. Register-DirectoryService cmdlet 'inin girişi, ormandaki diğer etki alanı yerine Orman adı/kök etki alanı olmalıdır.

   > [!IMPORTANT]
   > Kimlik bilgileri açılır penceresi için bekleyin (ayrıcalıklı uç noktada kimlik bilgisi al desteklenmez) ve Graph Service hesabı kimlik bilgilerini girin.

3. **Register-DirectoryService** cmdlet 'i, mevcut Active Directory doğrulamanın başarısız olduğu belirli senaryolarda kullanabileceğiniz isteğe bağlı parametrelere sahiptir. Bu cmdlet yürütüldüğünde, belirtilen etki alanının kök etki alanı olduğunu, bir genel katalog sunucusuna ulaşılmadığını ve belirtilen hesabın okuma erişimi verdiğini doğrular.

   |Parametre|Açıklama|
   |---------|---------|
   |`-SkipRootDomainValidation`|Önerilen kök etki alanı yerine bir alt etki alanı kullanılması gerektiğini belirtir.|
   |`-Force`|Tüm doğrulama denetimlerini atlar.|

#### <a name="graph-protocols-and-ports"></a>Grafik protokolleri ve bağlantı noktaları

Azure Stack grafik hizmeti, hedef Active Directory ormanında oturum açma isteklerini işleyebilde olan yazılabilir bir genel katalog sunucusu (GC) ve Anahtar Dağıtım Merkezi (KDC) ile iletişim kurmak için aşağıdaki protokolleri ve bağlantı noktalarını kullanır.

Azure Stack grafik hizmeti, hedef Active Directory iletişim kurmak için aşağıdaki protokolleri ve bağlantı noktalarını kullanır:

|Type|Port|Protocol|
|---------|---------|---------|
|LDAP|389|TCP & UDP|
|LDAP SSL|636|TCP|
|LDAP GC|3268|TCP|
|LDAP GC SSL|3269|TCP|

## <a name="setting-up-ad-fs-integration-by-downloading-federation-metadata"></a>Federasyon meta verilerini indirerek AD FS tümleştirmeyi ayarlama

Aşağıdaki bilgiler Otomasyon parametrelerinin girişi olarak gereklidir:

|Parametre|Dağıtım çalışma sayfası parametresi|Açıklama|Örnek|
|---------|---------|---------|---------|
|Customadfname|AD FS sağlayıcı adı|Talep sağlayıcısının adı.<br>AD FS giriş sayfasında bu şekilde görünür.|Contoso|
|CustomAD<br>FSFederationMetadataEndpointUri|AD FS meta veri URI 'SI|Federasyon meta veri bağlantısı| https:\//AD01.contoso.com/federationmetadata/2007-06/federationmetadata.xml |
|SigningCertificateRevocationCheck|NA|CRL denetimini atlamak için isteğe bağlı parametre|Yok.|


### <a name="trigger-automation-to-configure-claims-provider-trust-in-azure-stack"></a>Azure Stack 'da talep sağlayıcı güveni yapılandırmak için Otomasyonu tetikleme

Bu yordam için Azure Stack ayrıcalıklı uç noktayla iletişim kurabilen bir bilgisayar kullanın. **AD FS hesap STS** tarafından kullanılan sertifikanın Azure Stack güvenilir olması beklenir.

1. Yükseltilmiş bir Windows PowerShell oturumu açın ve ayrıcalıklı uç noktaya bağlanın.

   ```powershell  
   $creds = Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Ayrıcalıklı uç noktaya bağlı olduğunuza göre, ortamınız için uygun parametreleri kullanarak aşağıdaki komutu çalıştırın:

   ```powershell  
   Register-CustomAdfs -CustomAdfsName Contoso -CustomADFSFederationMetadataEndpointUri https://win-SQOOJN70SGL.contoso.com/federationmetadata/2007-06/federationmetadata.xml
   ```

3. Ortamınız için uygun parametreleri kullanarak varsayılan sağlayıcı aboneliğinin sahibini güncelleştirmek için aşağıdaki komutu çalıştırın:

   ```powershell  
   Set-ServiceAdminOwner -ServiceAdminOwnerUpn "administrator@contoso.com"
   ```

## <a name="setting-up-ad-fs-integration-by-providing-federation-metadata-file"></a>Federasyon meta veri dosyası sağlayarak AD FS tümleştirme ayarlama

Sürüm 1807 ' den başlayarak, aşağıdaki koşullardan biri doğru ise bu yöntemi kullanın:

- Sertifika zinciri, Azure Stack diğer tüm uç noktalara kıyasla AD FS farklıdır.
- Azure Stack AD FS örneğinden mevcut AD FS sunucusuna ağ bağlantısı yok.

Aşağıdaki bilgiler Otomasyon parametrelerinin girişi olarak gereklidir:


|Parametre|Açıklama|Örnek|
|---------|---------|---------|
|Customadfname|Talep sağlayıcısının adı. AD FS giriş sayfasında bu şekilde görünür.|Contoso|
|Customadffederationmetadatafilecontent|Meta veri içeriği|$using:federationMetadataFileContent|

### <a name="create-federation-metadata-file"></a>Federasyon meta veri dosyası oluştur

Aşağıdaki yordam için, mevcut AD FS dağıtımına ağ bağlantısı olan bir bilgisayar kullanmanız gerekir. Bu, hesap STS 'si haline gelir. Ayrıca, gerekli sertifikaların yüklenmesi gerekir.

1. Yükseltilmiş bir Windows PowerShell oturumu açın ve ortamınız için uygun parametreleri kullanarak aşağıdaki komutu çalıştırın:

   ```powershell  
    $url = "https://win-SQOOJN70SGL.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml"
    $webclient = New-Object System.Net.WebClient
    $webclient.Encoding = [System.Text.Encoding]::UTF8
    $metadataAsString = $webclient.DownloadString($url)
    Set-Content -Path c:\metadata.xml -Encoding UTF8 -Value $metadataAsString
   ```

2. Meta veri dosyasını ayrıcalıklı uç noktayla iletişim kurabildiği bir bilgisayara kopyalayın.

### <a name="trigger-automation-to-configure-claims-provider-trust-in-azure-stack"></a>Azure Stack 'da talep sağlayıcı güveni yapılandırmak için Otomasyonu tetikleme

Bu yordamda, Azure Stack ayrıcalıklı uç noktayla iletişim kurabilen ve önceki adımda oluşturduğunuz meta veri dosyasına erişebilen bir bilgisayar kullanın.

1. Yükseltilmiş bir Windows PowerShell oturumu açın ve ayrıcalıklı uç noktaya bağlanın.

   ```powershell  
   $federationMetadataFileContent = get-content c:\metadata.xml
   $creds=Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Ayrıcalıklı uç noktaya bağlı olduğunuza göre, ortamınız için uygun parametreleri kullanarak aşağıdaki komutu çalıştırın:

    ```powershell
    Register-CustomAdfs -CustomAdfsName Contoso -CustomADFSFederationMetadataFileContent $using:federationMetadataFileContent
    ```

3. Ortamınız için uygun parametreleri kullanarak varsayılan sağlayıcı aboneliğinin sahibini güncelleştirmek için aşağıdaki komutu çalıştırın:

   ```powershell  
   Set-ServiceAdminOwner -ServiceAdminOwnerUpn "administrator@contoso.com"
   ```

   > [!Note]  
   > Sertifikayı mevcut AD FS (Account STS) üzerinde döndürdüğünüzde AD FS tümleştirmesini yeniden ayarlamanız gerekir. Meta veri uç noktası ulaşılabilir olsa veya meta veri dosyası sağlanarak yapılandırıldıysa bile tümleştirmeyi ayarlamanız gerekir.

## <a name="configure-relying-party-on-existing-ad-fs-deployment-account-sts"></a>Mevcut AD FS dağıtımında bağlı olan tarafı yapılandırma (hesap STS)

Microsoft, talep dönüştürme kuralları da dahil olmak üzere, bağlı olan taraf güvenini yapılandıran bir betik sağlar. Komutları elle çalıştırabileceğiniz için betiği kullanmak isteğe bağlıdır.

Yardım betiğini GitHub 'daki [Azure Stack araçlarından](https://github.com/Azure/AzureStack-Tools/tree/vnext/DatacenterIntegration/Identity) indirebilirsiniz.

Komutları el ile çalıştırmaya karar verirseniz, aşağıdaki adımları izleyin:

1. Aşağıdaki içeriği, veri merkezinizin AD FS örneğine veya grup üyesine bir. txt dosyasına (örneğin, C:\claimkurallarını .txt olarak kaydedilir) kopyalayın:

   ```text
   @RuleTemplate = "LdapClaims"
   @RuleName = "Name claim"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
   => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"), query = ";userPrincipalName;{0}", param = c.Value);

   @RuleTemplate = "LdapClaims"
   @RuleName = "UPN claim"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
   => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"), query = ";userPrincipalName;{0}", param = c.Value);

   @RuleTemplate = "LdapClaims"
   @RuleName = "ObjectID claim"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid"]
   => issue(Type = "http://schemas.microsoft.com/identity/claims/objectidentifier", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, Value = c.Value, ValueType = c.ValueType);

   @RuleName = "Family Name and Given claim"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
   => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname", "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"), query = ";sn,givenName;{0}", param = c.Value);

   @RuleTemplate = "PassThroughClaims"
   @RuleName = "Pass through all Group SID claims"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
   => issue(claim = c);

   @RuleTemplate = "PassThroughClaims"
   @RuleName = "Pass through all windows account name claims"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
   => issue(claim = c);
   ```

2. Extranet ve intranet için Windows Forms tabanlı kimlik doğrulamasının etkinleştirildiğini doğrulayın. İlk olarak, aşağıdaki cmdlet 'i çalıştırarak zaten etkinleştirilmiş olduğunu doğrulayın:

   ```powershell  
   Get-AdfsAuthenticationProvider | where-object { $_.name -eq "FormsAuthentication" } | select Name, AllowedForPrimaryExtranet, AllowedForPrimaryIntranet
   ```

    > [!Note]  
    > Windows tümleşik kimlik doğrulaması (WIA) desteklenen Kullanıcı Aracısı dizeleri güncel olmayabilir AD FS dağıtım, en son istemcileri destekleyecek şekilde güncelleştirilmeleri gerekebilir. WIA destekli Kullanıcı Aracısı dizelerini güncelleştirme hakkında daha fazla bilgi edinmek [için, WIA desteklemeyen cihazlar için intranet Forms tabanlı kimlik doğrulaması yapılandırma](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-intranet-forms-based-authentication-for-devices-that-do-not-support-wia)makalesinde bulabilirsiniz.<br>Form tabanlı kimlik doğrulama ilkesini etkinleştirme adımları makalede, [kimlik doğrulama Ilkelerini yapılandırma](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-authentication-policies)bölümünde belgelenmiştir.

3. Bağlı olan taraf güvenini eklemek için, aşağıdaki Windows PowerShell komutunu AD FS örneğiniz veya bir grup üyesi üzerinde çalıştırın. AD FS uç noktasını güncelleştirdiğinizden emin olun ve adım 1 ' de oluşturulan dosyanın üzerine gelin.

   **AD FS 2016 için**

   ```powershell  
   Add-ADFSRelyingPartyTrust -Name AzureStack -MetadataUrl "https://YourAzureStackADFSEndpoint/FederationMetadata/2007-06/FederationMetadata.xml" -IssuanceTransformRulesFile "C:\ClaimIssuanceRules.txt" -AutoUpdateEnabled:$true -MonitoringEnabled:$true -enabled:$true -AccessControlPolicyName "Permit everyone" -TokenLifeTime 1440
   ```

   **AD FS 2012/2012 R2 için**

   ```powershell  
   Add-ADFSRelyingPartyTrust -Name AzureStack -MetadataUrl "https://YourAzureStackADFSEndpoint/FederationMetadata/2007-06/FederationMetadata.xml" -IssuanceTransformRulesFile "C:\ClaimIssuanceRules.txt" -AutoUpdateEnabled:$true -MonitoringEnabled:$true -enabled:$true -TokenLifeTime 1440
   ```

   > [!IMPORTANT]  
   > Windows Server 2012 veya 2012 R2 AD FS kullanırken verme yetkilendirme kurallarını yapılandırmak için AD FS MMC ek bileşenini kullanmanız gerekir.

4. Azure Stack erişmek için Internet Explorer veya Microsoft Edge tarayıcısı kullandığınızda, belirteç bağlamalarını göz ardı etmeniz gerekir. Aksi takdirde, oturum açma girişimleri başarısız olur. AD FS örneğiniz veya bir grup üyesi üzerinde aşağıdaki komutu çalıştırın:

   > [!note]  
   > Windows Server 2012 veya 2012 R2 AD FS kullanılırken bu adım geçerli değildir. Bu komutu atlamak ve tümleştirmeyle devam etmek güvenlidir.

   ```powershell  
   Set-AdfsProperties -IgnoreTokenBinding $true
   ```

## <a name="spn-creation"></a>SPN oluşturma

Kimlik doğrulaması için bir hizmet asıl adı (SPN) kullanılması gereken birçok senaryo vardır. Aşağıda bazı örnekler verilmiştir:

- Azure Stack AD FS dağıtımıyla CLı kullanımı
- AD FS ile dağıtıldığında Azure Stack için System Center yönetim paketi
- AD FS ile dağıtıldığında Azure Stack kaynak sağlayıcıları
- Çeşitli uygulamalar
- Etkileşimli olmayan bir oturum açma gerekir

> [!Important]  
> AD FS yalnızca etkileşimli oturum açma oturumlarını destekler. Otomatik senaryo için etkileşimli olmayan bir oturum açma gerekiyorsa, SPN kullanmanız gerekir.

SPN oluşturma hakkında daha fazla bilgi için bkz. [AD FS için hizmet sorumlusu oluşturma](azure-stack-create-service-principals.md).


## <a name="troubleshooting"></a>Sorun giderme

### <a name="configuration-rollback"></a>Yapılandırma geri alma

Daha önce kimlik doğrulayabileceğiniz bir durumda ortamı atan bir hata oluşursa, geri alma seçeneği kullanılabilir.

1. Yükseltilmiş bir Windows PowerShell oturumu açın ve aşağıdaki komutları çalıştırın:

   ```powershell  
   $creds = Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Ardından aşağıdaki cmdlet 'i çalıştırın:

   ```powershell  
   Reset-DatacenterIntegrationConfiguration
   ```

   Geri alma eylemini çalıştırdıktan sonra tüm yapılandırma değişiklikleri geri alınır. Yalnızca yerleşik **CloudAdmin** kullanıcısı ile kimlik doğrulaması mümkündür.

   > [!IMPORTANT]
   > Varsayılan sağlayıcı aboneliğinin orijinal sahibini yapılandırmanız gerekir

   ```powershell  
   Set-ServiceAdminOwner -ServiceAdminOwnerUpn "azurestackadmin@[Internal Domain]"
   ```

### <a name="collecting-additional-logs"></a>Ek Günlükler toplanıyor

Cmdlet 'lerden herhangi biri başarısız olursa `Get-Azurestacklogs` cmdlet 'ini kullanarak ek Günlükler toplayabilirsiniz.

1. Yükseltilmiş bir Windows PowerShell oturumu açın ve aşağıdaki komutları çalıştırın:

   ```powershell  
   $creds = Get-Credential
   Enter-pssession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Ardından, aşağıdaki cmdlet 'i çalıştırın:

   ```powershell  
   Get-AzureStackLog -OutputPath \\myworkstation\AzureStackLogs -FilterByRole ECE
   ```


[Dış izleme çözümlerini tümleştirme](azure-stack-integrate-monitor.md)
