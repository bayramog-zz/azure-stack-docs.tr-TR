---
title: Azure Stack App Service dağıtmaya yönelik önkoşullar | Microsoft Docs
description: Azure Stack App Service dağıtmadan önce, tamamlanacak önkoşul adımlarını öğrenin.
services: azure-stack
documentationcenter: ''
author: BryanLa
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2019
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 03/11/2019
ms.openlocfilehash: a12aceff00cf5be2d6ab70c4957ef04ea1c135d5
ms.sourcegitcommit: e2f6205e6469b39c2395ee09424bb7632cb94c40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70271703"
---
# <a name="prerequisites-for-deploying-app-service-on-azure-stack"></a>Azure Stack App Service dağıtmaya yönelik önkoşullar

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack Azure App Service dağıtmadan önce, bu makaledeki önkoşul adımlarını gerçekleştirmeniz gerekir.

> [!IMPORTANT]
> 1904 güncelleştirmesini Azure Stack tümleşik sisteminize uygulayın veya Azure App Service 1,6 ' i dağıtmadan önce en son Azure Stack Geliştirme Seti (ASDK) dağıtın.

## <a name="download-the-installer-and-helper-scripts"></a>Yükleyiciyi ve yardımcı betikleri indirin

1. [App Service Azure Stack dağıtım Yardımcısı betiklerine](https://aka.ms/appsvconmashelpers)indirin.
2. [App Service Azure Stack yükleyicisinden](https://aka.ms/appsvconmasinstaller)indirin.
3. Dosyaları yardımcı betikleri. zip dosyasından ayıklayın. Aşağıdaki dosyalar ve klasörler ayıklanır:

   - Common. ps1
   - Create-AADIdentityApp. ps1
   - Create-ADFSIdentityApp. ps1
   - Create-AppServiceCerts.ps1
   - Get-AzureStackRootCert.ps1
   - Remove-AppService.ps1
   - Modüller klasörü
     - GraphAPI.psm1

## <a name="syndicate-the-custom-script-extension-from-the-marketplace"></a>Market 'ten özel Betik uzantısı 'nı genel olarak

Azure Stack Azure App Service özel Betik uzantısı v 1.9.1 gerektirir.  Azure Stack Azure App Service dağıtım veya yükseltme işlemine başlamadan önce uzantının [Market 'ten](azure-stack-download-azure-marketplace-item.md) dağıtılması gerekir.

## <a name="get-certificates"></a>Sertifika Al

### <a name="azure-resource-manager-root-certificate-for-azure-stack"></a>Azure Stack için kök sertifika Azure Resource Manager

Azure Stack tümleşik sistem veya ASDK ana bilgisayarında ayrıcalıklı uç noktaya ulaşabilmiş bir bilgisayarda yükseltilmiş bir PowerShell oturumu açın.

Yardımcı betikleri ayıkladığınız klasörden *Get-AzureStackRootCert. ps1* betiğini çalıştırın. Betik, App Service sertifika oluşturmak için gereken komut dosyasıyla aynı klasörde bir kök sertifika oluşturur.

Aşağıdaki PowerShell komutunu çalıştırdığınızda, AzureStack\CloudAdmin. için ayrıcalıklı uç noktası ve kimlik bilgilerini sağlamanız gerekir.

```powershell
    Get-AzureStackRootCert.ps1
```

#### <a name="get-azurestackrootcertps1-script-parameters"></a>Get-AzureStackRootCert. ps1 betik parametreleri

| Parametre | Gerekli veya isteğe bağlı | Varsayılan değer | Açıklama |
| --- | --- | --- | --- |
| Ayrıcalıklı Gedendpoint | Gerekli | AzS-ERCS01 | Ayrıcalıklı uç nokta |
| CloudAdminCredential | Gerekli | AzureStack\CloudAdmin | Azure Stack Cloud Admins için etki alanı hesabı kimlik bilgileri |

### <a name="certificates-required-for-asdk-deployment-of-azure-app-service"></a>Azure App Service ASDK dağıtımı için gerekli sertifikalar

*Create-AppServiceCerts. ps1* betiği, App Service gereken dört sertifikayı oluşturmak için Azure Stack sertifika yetkilisi ile birlikte kullanılır.

| Dosya adı | Bir yönetim grubuna bağlanmak veya bağlı bir yönetim grubunun özelliklerini düzenlemek için Yönetim çalışma alanında |
| --- | --- |
| _.appservice.local.azurestack.external.pfx | App Service varsayılan SSL sertifikası |
| api.appservice.local.azurestack.external.pfx | API SSL sertifikası App Service |
| ftp.appservice.local.azurestack.external.pfx | App Service yayımcı SSL sertifikası |
| sso.appservice.local.azurestack.external.pfx | App Service kimlik uygulama sertifikası |

Sertifikaları oluşturmak için aşağıdaki adımları izleyin:

1. AzureStack\AzureStackAdmin hesabını kullanarak ASDK ana bilgisayarında oturum açın.
2. Yükseltilmiş bir PowerShell oturumu açın.
3. Yardımcı betikleri ayıkladığınız klasörden *Create-AppServiceCerts. ps1* betiğini çalıştırın. Bu betik, App Service sertifika oluşturmak için ihtiyaç duyacağı komut dosyasıyla aynı klasörde dört sertifika oluşturur.
4. . Pfx dosyalarının güvenliğini sağlamak için bir parola girin ve bunu bir yere unutmayın. Azure Stack yükleyicideki App Service girmeniz gerekir.

#### <a name="create-appservicecertsps1-script-parameters"></a>Create-AppServiceCerts. ps1 betik parametreleri

| Parametre | Gerekli veya isteğe bağlı | Varsayılan değer | Açıklama |
| --- | --- | --- | --- |
| Pfxparolası | Gerekli | Null | Sertifika özel anahtarını korumaya yardımcı olan parola |
| Etki | Gerekli | Local. azurestack. External | Azure Stack bölgesi ve etki alanı son eki |

### <a name="certificates-required-for-azure-stack-production-deployment-of-azure-app-service"></a>Azure App Service Azure Stack Üretim dağıtımı için gerekli sertifikalar

Kaynak sağlayıcısını üretimde çalıştırmak için aşağıdaki sertifikaları sağlamanız gerekir:

- Varsayılan etki alanı sertifikası
- API sertifikası
- Sertifika yayımlanıyor
- Kimlik sertifikası

#### <a name="default-domain-certificate"></a>Varsayılan etki alanı sertifikası

Varsayılan etki alanı sertifikası ön uç rolüne yerleştirilir. Joker karakter veya varsayılan etki alanı isteğine yönelik kullanıcı uygulamaları Azure App Service bu sertifikayı kullanır. Sertifika, kaynak denetimi işlemleri (kudu) için de kullanılır.

Sertifika. pfx biçiminde olmalı ve üç konuyla bir joker karakter sertifikası olmalıdır. Bu gereksinim, bir sertifikanın, kaynak denetimi işlemleri için hem varsayılan etki alanını hem de SCM uç noktasını kapsamasını sağlar.

| Biçimi | Örnek |
| --- | --- |
| `*.appservice.<region>.<DomainName>.<extension>` | `*.appservice.redmond.azurestack.external` |
| `*.scm.appservice.<region>.<DomainName>.<extension>` | `*.scm.appservice.redmond.azurestack.external` |
| `*.sso.appservice.<region>.<DomainName>.<extension>` | `*.sso.appservice.redmond.azurestack.external` |

#### <a name="api-certificate"></a>API sertifikası

API sertifikası Yönetim rolüne yerleştirilir. Kaynak sağlayıcı, API çağrılarının güvenliğini sağlamaya yardımcı olmak için bunu kullanır. Yayımlama sertifikası, API DNS girdisiyle eşleşen bir konu içermelidir.

| Biçimi | Örnek |
| --- | --- |
| api. appservice. \<bölge.\>\< DomainName\>.\< uzantının\> | api.appservice.redmond.azurestack.external |

#### <a name="publishing-certificate"></a>Sertifika yayımlanıyor

Yayımcı rolü sertifikası, içerik karşıya yüklerken uygulama sahipleri için FTPS trafiğinin güvenliğini sağlar. Yayımlama sertifikası, FTPS DNS girdisiyle eşleşen bir konu içermelidir.

| Biçimi | Örnek |
| --- | --- |
| FTP. appservice. \<bölge.\>\< DomainName\>.\< uzantının\> | ftp.appservice.redmond.azurestack.external |

#### <a name="identity-certificate"></a>Kimlik sertifikası

Kimlik uygulaması için sertifika şunları sunar:

- İşlem kaynak sağlayıcısı ile tümleştirmeyi desteklemek için Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) (AD FS) dizin, Azure Stack ve App Service arasında tümleştirme.
- Azure Stack Azure App Service içindeki gelişmiş geliştirici araçları için çoklu oturum açma senaryoları.

Kimliğin sertifikası aşağıdaki biçimle eşleşen bir konu içermelidir.

| Biçimi | Örnek |
| --- | --- |
| SSO. appservice. \<bölge.\>\< DomainName\>.\< uzantının\> | sso.appservice.redmond.azurestack.external |

### <a name="validate-certificates"></a>Sertifikaları doğrula

App Service kaynak sağlayıcısını dağıtmadan önce, [PowerShell Galerisi](https://aka.ms/AzsReadinessChecker)kullanılabilir Azure Stack hazırlık Denetleyicisi aracını kullanarak [kullanılacak sertifikaları doğrulamanız](azure-stack-validate-pki-certs.md#perform-platform-as-a-service-certificate-validation) gerekir. Azure Stack hazırlık Denetleyicisi Aracı, oluşturulan PKI sertifikalarının App Service dağıtım için uygun olduğunu doğrular.

En iyi uygulama olarak, gerekli [Azure Stack PKI sertifikalarıyla](azure-stack-pki-certs.md)çalışırken, gerektiğinde sertifikaları test etmek ve yeniden vermek için yeterli zaman planlamanız gerekir.

## <a name="virtual-network"></a>Sanal ağ

> [!NOTE]
> Azure Stack üzerindeki Azure App Service gerekli sanal ağı oluşturabileceği, ancak daha sonra genel IP adresleri aracılığıyla SQL ve dosya sunucusuyla iletişim kurması gereken için özel bir sanal ağın daha fazla olması isteğe bağlıdır.

Azure Stack Azure App Service, kaynak sağlayıcıyı mevcut bir sanal ağa dağıtmanızı sağlar veya dağıtımın bir parçası olarak bir sanal ağ oluşturmanızı sağlar. Mevcut bir sanal ağın kullanılması, dosya sunucusuna bağlanmak ve Azure Stack Azure App Service gereken SQL Server için iç IP 'lerin kullanılmasını sağlar. Azure Stack Azure App Service yüklemeden önce sanal ağın aşağıdaki adres aralığı ve alt ağlarla yapılandırılması gerekir:

Sanal ağ-/16

Alt ağlar

- ControllersSubnet/24
- ManagementServersSubnet/24
- FrontEndsSubnet/24
- PublishersSubnet/24
- WorkersSubnet/21

## <a name="licensing-concerns-for-required-file-server-and-sql-server"></a>Gerekli dosya sunucusu ve SQL Server lisanslama sorunları

Azure Stack Azure App Service, bir dosya sunucusu ve SQL Server çalışmasını gerektirir.  Azure Stack dağıtımınızın dışında bulunan önceden var olan kaynakları kullanabilirsiniz veya kaynakları Azure Stack varsayılan sağlayıcı abonelikleri içinde dağıtabilirsiniz.

Kaynakları Azure Stack varsayılan sağlayıcı aboneliğinizde dağıtmayı seçerseniz, bu kaynaklara yönelik lisanslar (Windows Server lisansları ve SQL Server lisansları), aşağıdakilere bağlı Azure Stack Azure App Service maliyetine dahil edilir kısıtlamaları

- Altyapı **varsayılan sağlayıcı aboneliğine**dağıtılır;
- altyapı, Azure Stack kaynak sağlayıcısındaki Azure App Service tarafından özel olarak kullanılır.  Diğer iş yükleri, Yönetim (örneğin, diğer kaynak sağlayıcıları) yok. SQL-RP) veya kiracı (örneğin: bir veritabanı gerektiren Kiracı uygulamaları), bu altyapıyı kullanma izni verilir.

## <a name="prepare-the-file-server"></a>Dosya sunucusunu hazırlama

Azure App Service, bir dosya sunucusunun kullanılmasını gerektirir. Üretim dağıtımları için, dosya sunucusu yüksek kullanılabilirliğe sahip olacak ve hataların işlenmesine sahip olacak şekilde yapılandırılmış olmalıdır.

### <a name="quickstart-template-for-file-server-for-deployments-of-azure-app-service-on-asdk"></a>Dosya sunucusu için, ASDK üzerinde Azure App Service dağıtımları için hızlı başlangıç şablonu.

Yalnızca ASDK dağıtımları için, yapılandırılmış bir tek düğümlü dosya sunucusunu dağıtmak üzere [örnek Azure Resource Manager Dağıtım şablonunu](https://aka.ms/appsvconmasdkfstemplate) kullanabilirsiniz. Tek düğümlü dosya sunucusu bir çalışma grubunda olacaktır.

### <a name="quickstart-template-for-highly-available-file-server-and-sql-server"></a>Yüksek oranda kullanılabilir dosya sunucusu ve SQL Server hızlı başlangıç şablonu

Bir [başvuru mimarisi hızlı başlangıç şablonu](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/appservice-fileserver-sqlserver-ha) artık kullanılabilir ve bu, bir dosya sunucusu ve SQL Server dağıtır. Bu şablon, Azure Stack Azure App Service yüksek oranda kullanılabilir bir dağıtımını desteklemek üzere yapılandırılmış bir sanal ağda Active Directory altyapısını destekler.

### <a name="steps-to-deploy-a-custom-file-server"></a>Özel bir dosya sunucusu dağıtma adımları

>[!IMPORTANT]
> App Service mevcut bir sanal ağda dağıtmayı seçerseniz, dosya sunucusu App Service farklı bir alt ağa dağıtılmalıdır.

>[!NOTE]
> Yukarıda bahsedilen hızlı başlangıç şablonlarından birini kullanarak bir dosya sunucusu dağıtmayı seçtiyseniz, dosya sunucuları şablon dağıtımının bir parçası olarak yapılandırıldığı için bu bölümü atlayabilirsiniz.

#### <a name="provision-groups-and-accounts-in-active-directory"></a>Active Directory grupları ve hesapları sağlama

1. Aşağıdaki Active Directory genel güvenlik gruplarını oluşturun:

   - FileShareOwners
   - FileShareUsers

2. Aşağıdaki Active Directory hesaplarını hizmet hesapları olarak oluşturun:

   - FileShareOwner
   - FileShareUser

   En iyi güvenlik uygulaması olarak, bu hesapların (ve tüm Web rollerinin) kullanıcıları benzersiz olmalı ve güçlü Kullanıcı adları ve parolalara sahip olmalıdır. Parolaları aşağıdaki koşullara göre ayarlayın:

   - Parolayı etkinleştir ayarı **hiçbir zaman dolmaz**.
   - **Kullanıcının parolayı değiştire,** izin vermez.
   - **Kullanıcının bir sonraki oturum açışında parolayı değiştirmesi gerekir**.

3. Hesapları grup üyeliğine aşağıdaki şekilde ekleyin:

   - **Fileshareowner** öğesini **fileshareowners** grubuna ekleyin.
   - **Fileshareuser** grubunu **fileshareusers** grubuna ekleyin.

#### <a name="provision-groups-and-accounts-in-a-workgroup"></a>Bir çalışma grubunda gruplar ve hesaplar sağlama

>[!NOTE]
> Bir dosya sunucusu yapılandırırken, **yönetici komut isteminden**aşağıdaki komutları çalıştırın. <br>***PowerShell kullanmayın.***

Azure Resource Manager şablonunu kullandığınızda kullanıcılar zaten oluşturulur.

1. FileShareOwner ve FileShareUser hesaplarını oluşturmak için aşağıdaki komutları çalıştırın. Kendi `<password>` değerlerinizle değiştirin.

   ``` DOS
   net user FileShareOwner <password> /add /expires:never /passwordchg:no
   net user FileShareUser <password> /add /expires:never /passwordchg:no
   ```

2. Aşağıdaki WMIC komutlarını çalıştırarak, hesapların parolalarını hiçbir zaman dolmayacak şekilde ayarlayın:

   ``` DOS
   WMIC USERACCOUNT WHERE "Name='FileShareOwner'" SET PasswordExpires=FALSE
   WMIC USERACCOUNT WHERE "Name='FileShareUser'" SET PasswordExpires=FALSE
   ```

3. FileShareUsers ve FileShareOwners yerel gruplarını oluşturun ve ilk adımdaki hesapları bunlara ekleyin:

   ``` DOS
   net localgroup FileShareUsers /add
   net localgroup FileShareUsers FileShareUser /add
   net localgroup FileShareOwners /add
   net localgroup FileShareOwners FileShareOwner /add
   ```

#### <a name="provision-the-content-share"></a>İçerik payını sağlama

İçerik paylaşımında kiracı Web sitesi içeriği yer alır. Tek bir dosya sunucusunda içerik paylaşımının sağlanması yordamı hem Active Directory hem de çalışma grubu ortamları için aynıdır. Ancak Active Directory bir yük devretme kümesi için farklıdır.

#### <a name="provision-the-content-share-on-a-single-file-server-active-directory-or-workgroup"></a>Tek bir dosya sunucusunda (Active Directory veya çalışma grubu) içerik paylaşma sağlama

Tek bir dosya sunucusunda, yükseltilmiş bir komut isteminde aşağıdaki komutları çalıştırın. Değerini `C:\WebSites` ortamınızdaki karşılık gelen yollarla değiştirin.

```DOS
set WEBSITES_SHARE=WebSites
set WEBSITES_FOLDER=C:\WebSites
md %WEBSITES_FOLDER%
net share %WEBSITES_SHARE% /delete
net share %WEBSITES_SHARE%=%WEBSITES_FOLDER% /grant:Everyone,full
```

### <a name="configure-access-control-to-the-shares"></a>Paylaşımlara erişim denetimini yapılandırma

Dosya sunucusunda veya geçerli küme kaynağı sahibi olan yük devretme kümesi düğümünde, yükseltilmiş bir komut isteminde aşağıdaki komutları çalıştırın. İtalik değerlerini ortamınıza özgü değerlerle değiştirin.

#### <a name="active-directory"></a>Active Directory

```DOS
set DOMAIN=<DOMAIN>
set WEBSITES_FOLDER=C:\WebSites
icacls %WEBSITES_FOLDER% /reset
icacls %WEBSITES_FOLDER% /grant Administrators:(OI)(CI)(F)
icacls %WEBSITES_FOLDER% /grant %DOMAIN%\FileShareOwners:(OI)(CI)(M)
icacls %WEBSITES_FOLDER% /inheritance:r
icacls %WEBSITES_FOLDER% /grant %DOMAIN%\FileShareUsers:(CI)(S,X,RA)
icacls %WEBSITES_FOLDER% /grant *S-1-1-0:(OI)(CI)(IO)(RA,REA,RD)
```

#### <a name="workgroup"></a>Çalışma grubu

```DOS
set WEBSITES_FOLDER=C:\WebSites
icacls %WEBSITES_FOLDER% /reset
icacls %WEBSITES_FOLDER% /grant Administrators:(OI)(CI)(F)
icacls %WEBSITES_FOLDER% /grant FileShareOwners:(OI)(CI)(M)
icacls %WEBSITES_FOLDER% /inheritance:r
icacls %WEBSITES_FOLDER% /grant FileShareUsers:(CI)(S,X,RA)
icacls %WEBSITES_FOLDER% /grant *S-1-1-0:(OI)(CI)(IO)(RA,REA,RD)
```

## <a name="prepare-the-sql-server-instance"></a>SQL Server örneğini hazırlama

>[!NOTE]
> Yüksek oranda kullanılabilir dosya sunucusu ve SQL Server için hızlı başlangıç şablonunu dağıtmayı seçtiyseniz, şablon bir HA yapılandırmasında SQL Server dağıtır ve yapılandırdıkça bu bölümü atlayabilirsiniz.

Azure Stack barındırma ve ölçüm veritabanlarında Azure App Service için, App Service veritabanlarını tutmak üzere bir SQL Server örneği hazırlamanız gerekir.

ASDK dağıtımları için SQL Server Express 2014 SP2 veya sonraki bir sürümünü kullanabilirsiniz. SQL Server **,** Azure Stack App Service Windows kimlik doğrulamasını desteklemediğinden **karma mod** kimlik doğrulamasını destekleyecek şekilde yapılandırılmalıdır.

Üretim ve yüksek kullanılabilirlik amaçlarıyla, SQL Server 2014 SP2 veya sonraki bir sürümünü kullanmanız, karışık mod kimlik doğrulamasını etkinleştirmeniz ve [yüksek oranda kullanılabilir bir yapılandırmada](https://docs.microsoft.com/sql/sql-server/failover-clusters/high-availability-solutions-sql-server)dağıtmanız gerekir.

Azure Stack Azure App Service için SQL Server örneğine tüm App Service rollerden erişilebilir olması gerekir. Azure Stack varsayılan sağlayıcı aboneliği içinde SQL Server dağıtabilirsiniz. Ya da, kuruluşunuzda var olan altyapıyı (Azure Stack bağlantısı olduğu sürece) kullanabilirsiniz. Azure Market görüntüsü kullanıyorsanız güvenlik duvarını uygun şekilde yapılandırmayı unutmayın.

> [!NOTE]
> Market yönetim özelliği aracılığıyla bir dizi SQL IaaS sanal makine görüntüsü mevcuttur. Market öğesi kullanarak bir VM dağıtmadan önce SQL IaaS uzantısının en son sürümünü her zaman indirdiğinizden emin olun. SQL görüntüleri, Azure 'da kullanılabilen SQL VM 'leriyle aynıdır. Bu görüntülerden oluşturulan SQL VM 'Leri için IaaS uzantısı ve ilgili Portal geliştirmeleri, otomatik düzeltme eki uygulama ve yedekleme özellikleri gibi özellikler sağlar.
>
> SQL Server rollerinin herhangi biri için varsayılan bir örneği veya adlandırılmış bir örneği kullanabilirsiniz. Adlandırılmış bir örnek kullanıyorsanız, SQL Server Browser hizmeti 'ni el ile başlatıp 1434 numaralı bağlantı noktasını açın.

App Service yükleyicisi, SQL Server veritabanı içerme özelliğinin etkin olduğundan emin olmak için kontrol eder. App Service veritabanlarını barındıracak SQL Server veritabanı kapsamayı etkinleştirmek için şu SQL komutlarını çalıştırın:

```sql
sp_configure 'contained database authentication', 1;
GO
RECONFIGURE;
GO
```

>[!IMPORTANT]
> Mevcut bir sanal ağda App Service dağıtmayı seçerseniz, SQL Server App Service ve dosya sunucusundan ayrı bir alt ağa dağıtılmalıdır.
>

## <a name="create-an-azure-active-directory-app"></a>Azure Active Directory uygulaması oluşturma

Aşağıdaki işlemleri desteklemek için bir Azure AD hizmet sorumlusu yapılandırın:

- Çalışan katmanlarında sanal makine ölçek kümesi tümleştirmesi.
- Azure Işlevleri portalı ve gelişmiş geliştirici araçları için SSO.

Bu adımlar yalnızca Azure AD ile güvenli Azure Stack ortamları için geçerlidir.

Yöneticilerin SSO 'yu şu şekilde yapılandırması gerekir:

- App Service (kudu) içinde gelişmiş geliştirici araçları 'nı etkinleştirin.
- Azure Işlevleri portalı deneyiminin kullanımını etkinleştirin.

Şu adımları uygulayın:

1. PowerShell örneğini azurestack\AzureStackAdmin. olarak açma
2. [Önkoşul adımında](azure-stack-app-service-before-you-get-started.md)indirdiğiniz ve ayıkladığınız betiklerin konumuna gidin.
3. [Azure Stack Için PowerShell 'ı yükler](azure-stack-powershell-install.md).
4. **Create-AADIdentityApp. ps1** betiğini çalıştırın. İstendiğinde, Azure Stack dağıtımınız için kullanmakta olduğunuz Azure AD kiracı KIMLIĞINI girin. Örneğin, **myazurestack.onmicrosoft.com**girin.
5. **Kimlik bilgisi** PENCERESINDE Azure AD hizmet yöneticisi hesabınızı ve parolanızı girin. **Tamam**’ı seçin.
6. [Daha önce oluşturulan sertifikanın](azure-stack-app-service-before-you-get-started.md)sertifika dosya yolunu ve sertifika parolasını girin. Bu adım için oluşturulan sertifika varsayılan olarak **SSO. appservice. Local. azurestack. external. pfx**' dir.
7. Betik, kiracı Azure AD örneğinde yeni bir uygulama oluşturur. PowerShell çıkışında döndürülen uygulama KIMLIĞINI unutmayın. Yükleme sırasında bu bilgilere ihtiyacınız vardır.
8. Yeni bir tarayıcı penceresi açın ve [Azure portal](https://portal.azure.com) Azure Active Directory Hizmet Yöneticisi olarak oturum açın.
9. Azure AD kaynak sağlayıcısı 'nı açın.
10. **Uygulama kayıtları**' nı seçin.
11. Adım 7 ' nin bir parçası olarak döndürülen uygulama KIMLIĞI ' ni arayın. App Service bir uygulama listelenir.
12. Listeden **uygulama** ' yı seçin.
13. Seçin **ayarları**.
14. **Gerekli izinler** > izin**ver** > **Evet**' i seçin.

```powershell
    Create-AADIdentityApp.ps1
```

| Parametre | Gerekli veya isteğe bağlı | Varsayılan değer | Açıklama |
| --- | --- | --- | --- |
| DirectoryTenantName | Gerekli | Null | Azure AD kiracı KIMLIĞI. GUID veya dize belirtin. Örnek olarak myazureaaddirectory.onmicrosoft.com. |
| Adminermenistan Dpoint | Gerekli | Null | Yönetici Azure Resource Manager uç noktası. Adminmanagement. Local. azurestack. external bir örnektir. |
| TenantARMEndpoint | Gerekli | Null | Kiracı Azure Resource Manager uç noktası. Yönetim. Local. azurestack. external bir örnektir. |
| AzureStackAdminCredential | Gerekli | Null | Azure AD Hizmet Yöneticisi kimlik bilgileri. |
| CertificateFilePath | Gerekli | Null | Daha önce oluşturulan kimlik uygulama sertifikası dosyasının **tam yolu** . |
| CertificatePassword | Gerekli | Null | Sertifika özel anahtarını korumaya yardımcı olan parola. |
| Ortam | İsteğe Bağlı | AzureCloud | Hedef Azure Active Directory grafik hizmetinin kullanılabildiği desteklenen bulut ortamının adı.  İzin verilen değerler: ' Azurecyüksek ', ' AzureChinaCloud ', ' AzureUSGovernment ', ' AzureGermanCloud '.|

## <a name="create-an-active-directory-federation-services-app"></a>Active Directory Federasyon Hizmetleri (AD FS) uygulaması oluşturma

AD FS tarafından güvenliği sağlanmış Azure Stack ortamlar için, aşağıdaki işlemleri desteklemek üzere bir AD FS hizmet sorumlusu yapılandırmanız gerekir:

- Çalışan katmanlarında sanal makine ölçek kümesi tümleştirmesi.
- Azure Işlevleri portalı ve gelişmiş geliştirici araçları için SSO.

Yöneticilerin SSO 'yu şu şekilde yapılandırması gerekir:

- Çalışan katmanlarında sanal makine ölçek kümesi tümleştirmesi için bir hizmet sorumlusu yapılandırın.
- App Service (kudu) içinde gelişmiş geliştirici araçları 'nı etkinleştirin.
- Azure Işlevleri portalı deneyiminin kullanımını etkinleştirin.

Şu adımları uygulayın:

1. PowerShell örneğini azurestack\AzureStackAdmin. olarak açma
2. [Önkoşul adımında](azure-stack-app-service-before-you-get-started.md)indirdiğiniz ve ayıkladığınız betiklerin konumuna gidin.
3. [Azure Stack Için PowerShell 'ı yükler](azure-stack-powershell-install.md).
4. **Create-ADFSIdentityApp. ps1** betiğini çalıştırın.
5. **Kimlik bilgisi** penceresinde, AD FS bulut yöneticisi hesabınızı ve parolanızı girin. **Tamam**’ı seçin.
6. [Daha önce oluşturulan sertifikanın](azure-stack-app-service-before-you-get-started.md)sertifika dosya yolunu ve sertifika parolasını sağlayın. Bu adım için oluşturulan sertifika varsayılan olarak **SSO. appservice. Local. azurestack. external. pfx**' dir.

```powershell
    Create-ADFSIdentityApp.ps1
```

| Parametre | Gerekli veya isteğe bağlı | Varsayılan değer | Açıklama |
| --- | --- | --- | --- |
| Adminermenistan Dpoint | Gerekli | Null | Yönetici Azure Resource Manager uç noktası. Adminmanagement. Local. azurestack. external bir örnektir. |
| Ayrıcalıklı Gedendpoint | Gerekli | Null | Ayrıcalıklı uç nokta. AzS-ERCS01 bir örnektir. |
| CloudAdminCredential | Gerekli | Null | Azure Stack Cloud Admins için etki alanı hesabı kimlik bilgileri. Örnek, Azurestack\CloudAdmin. |
| CertificateFilePath | Gerekli | Null | Kimlik uygulamasının sertifika PFX dosyasının **tam yolu** . |
| CertificatePassword | Gerekli | Null | Sertifika özel anahtarını korumaya yardımcı olan parola. |

## <a name="next-steps"></a>Sonraki adımlar

[App Service kaynak sağlayıcısını yükler](azure-stack-app-service-deploy.md)
