---
title: Azure Stack için kimlik sağlayıcılarına genel bakış | Microsoft Docs
description: Azure Stack ile kullanabileceğiniz kimlik sağlayıcıları hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: patricka
ms.reviewer: fiseraci
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: 8b05b2cc9fdde7987efd78c5beb0123b035e03ea
ms.sourcegitcommit: 451cfaa24b349393f36ae9d646d4d311a14dd1fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72019362"
---
# <a name="overview-of-identity-providers-for-azure-stack"></a>Azure Stack için kimlik sağlayıcılarına genel bakış

Azure Stack, bir kimlik sağlayıcısı olarak Active Directory tarafından desteklenen Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) (AD FS) gerektirir. Sağlayıcı seçimi, Azure Stack ilk kez dağıtırken yaptığınız tek seferlik bir karardır. Bu makaledeki kavramlar ve yetkilendirme ayrıntıları, kimlik sağlayıcıları arasında seçim yapmanıza yardımcı olabilir.

Azure AD veya AD FS seçiminiz, Azure Stack dağıttığınız moda göre belirlenir:

- Bunu bağlı bir modda dağıttığınızda Azure AD 'yi veya AD FS kullanabilirsiniz.
- Bunu, internet bağlantısı olmadan bağlantısı kesik modda dağıtırken yalnızca AD FS desteklenir.

Azure Stack ortamınıza bağlı olan seçenekleriniz hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- Azure Stack dağıtım seti: [Kimlik konuları](azure-stack-datacenter-integration.md#identity-considerations).
- Azure Stack tümleşik sistemler: [Azure Stack tümleşik sistemler Için dağıtım planlama kararları](azure-stack-connection-models.md).

## <a name="common-concepts-for-identity-providers"></a>Kimlik sağlayıcıları için ortak kavramlar

Sonraki bölümlerde kimlik sağlayıcıları ve Azure Stack kullanımları hakkında genel kavramlar ele alınmaktadır.

![Kimlik sağlayıcıları için terminoloji](media/azure-stack-identity-overview/terminology.png)

### <a name="directory-tenants-and-organizations"></a>Dizin kiracılar ve kuruluşlar

Dizin, *Kullanıcılar*, *uygulamalar*, *gruplar*ve *hizmet sorumluları*hakkında bilgi tutan bir kapsayıcıdır.

Dizin kiracısı, Microsoft veya kendi şirketiniz gibi bir *kuruluştur*.

- Azure AD birden çok kiracıyı destekler ve her biri kendi dizininde yer alan birden çok kuruluşu destekleyebilir. Azure AD kullanırsanız ve birden çok kiracıya sahipseniz, tek bir kiracının aynı dizindeki diğer kiracılara erişmesini ve kullanıcılara izin verebilirsiniz.
- AD FS yalnızca tek bir kiracıyı ve bu nedenle yalnızca tek bir kuruluşu destekler.

### <a name="users-and-groups"></a>Kullanıcılar ve gruplar

Kullanıcı hesapları (kimlikler), bir kullanıcı KIMLIĞI ve parola kullanarak kişilerin kimliğini doğrulayan standart hesaplardır. Gruplar, kullanıcıları veya diğer grupları içerebilir.

Kullanıcıları ve grupları oluşturma ve yönetme, kullandığınız kimlik çözümüne bağlıdır.

Azure Stack, Kullanıcı hesapları:

- , *Kullanıcı adı @ no__t-1etkialanı* biçiminde oluşturulur. AD FS, Kullanıcı hesaplarını Active Directory bir örneğe eşler, ancak AD FS *\\ @ no__t-2etki alanı > \\ @ no__t-4alias >* biçiminin kullanımını desteklemez.
- Multi-Factor Authentication kullanmak üzere ayarlanabilir.
- , Kuruluşun dizini olan, ilk kaydoldukları dizinle kısıtlıdır.
- , Şirket içi dizininizden içeri aktarılabilir. Daha fazla bilgi için bkz. Şirket [içi dizinlerinizi Azure Active Directory tümleştirme](/azure/active-directory/connect/active-directory-aadconnect).

Kuruluşunuzun kiracı portalında oturum açtığınızda, *https: \//Portal. Local. azurestack. external* URL 'sini kullanırsınız. Azure Stack kaydettirmek için kullanılandan farklı etki alanlarından Azure Stack portalında oturum açarken, Azure Stack kaydetmek için kullanılan etki alanı adının Portal URL 'sine eklenmesi gerekir. Örneğin, Azure Stack fabrikam.onmicrosoft.com ile kaydedilmişse ve oturum açan kullanıcı hesabı admin@contoso.com ise, Kullanıcı portalında oturum açmak için kullanılacak URL şöyle olacaktır: https: \//Portal. Local. azurestack. external/fabrikam. onmicrosoft. com.

### <a name="guest-users"></a>Konuk kullanıcılar

Konuk kullanıcılar, diğer dizin kiracılarından, dizininizdeki kaynaklara erişim izni verilen kullanıcı hesaplarıdır. Konuk kullanıcıları desteklemek için Azure AD 'yi kullanır ve çok kiracılı desteği etkinleştirebilirsiniz. Destek etkinleştirildiğinde, Konuk kullanıcıları Dizin kiracınızdaki kaynaklara erişmek için davet edebilirsiniz; bu da, dışarıdaki kuruluşların işbirliği yapmasına olanak sağlar.

Konuk kullanıcıları davet etmek için, bulut İşletmenleri ve kullanıcılar [Azure AD B2B işbirliğini](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)kullanabilir. Davet edilen kullanıcılar, dizininizdeki belgelere, kaynaklara ve uygulamalara erişim sağlar ve kendi kaynaklarınız ve verileriniz üzerinde denetim sahibi olursunuz.

Konuk Kullanıcı olarak, başka bir kuruluşun Dizin kiracısında oturum açabilirsiniz. Bunu yapmak için, söz konusu kuruluşun dizin adını Portal URL 'sine ekleyin. Örneğin, contoso kuruluşuna aitseniz ve Fabrikam dizininde oturum açmak istiyorsanız https: \//Portal. Local. azurestack. external/fabrikam. onmicrosoft. com kullanın.

### <a name="apps"></a>Uygulamalar

Uygulamaları Azure AD 'ye veya AD FS kaydedebilir ve sonra uygulamaları kuruluşunuzdaki kullanıcılara sunabilirsiniz.

Uygulamalar şunlardır:

- **Web uygulamaları**: Azure portal ve Azure Resource Manager örnekleri bulunur. Web API çağrılarını destekler.
- **Yerel istemci**: Örnek olarak Azure PowerShell, Visual Studio ve Azure CLı sayılabilir.

Uygulamalar iki tür kiracı destekleyebilir:

- **Tek kiracılı**: Yalnızca uygulamanın kaydedildiği dizinden kullanıcıları ve hizmetleri destekler.

  > [!NOTE]
  > AD FS yalnızca tek bir dizini desteklediğinden, bir AD FS topolojide oluşturduğunuz uygulamalar tasarım, tek kiracılı uygulamalar ile yapılır.

- **Çok kiracılı**: , Hem uygulamanın kaydedildiği dizindeki hem de ek kiracı dizinlerinin kullanıcıları ve Hizmetleri tarafından kullanılmasını destekler. Çok kiracılı uygulamalarla, başka bir kiracı dizininin kullanıcıları (başka bir Azure AD kiracısı) uygulamanızda oturum açabilir.

  Çoklu kiracı hakkında daha fazla bilgi için bkz. [Çoklu kirayı etkinleştirme](azure-stack-enable-multitenancy.md).

  Çok kiracılı bir uygulama geliştirme hakkında daha fazla bilgi için bkz. [çok kiracılı uygulamalar](/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview).

Bir uygulamayı kaydettiğinizde iki nesne oluşturursunuz:

- **Uygulama nesnesi**: Tüm kiracılar genelinde uygulamanın genel temsili. Bu ilişki, yazılım uygulamasıyla bire bir ve yalnızca uygulamanın ilk kaydedildiği dizinde bulunur.

- **Hizmet sorumlusu nesnesi**: Uygulamanın ilk kaydedildiği dizindeki bir uygulama için oluşturulan kimlik bilgileri. Bir hizmet sorumlusu, uygulamanın kullanıldığı her Ek kiracının dizininde de oluşturulur. Bu ilişki, yazılım uygulaması ile bire çok olabilir.

Uygulama ve hizmet sorumlusu nesneleri hakkında daha fazla bilgi edinmek için [Azure Active Directory Içindeki uygulama ve hizmet sorumlusu nesneleri](/azure/active-directory/develop/active-directory-application-objects)bölümüne bakın.

### <a name="service-principals"></a>Hizmet sorumluları

Hizmet sorumlusu, Azure Stack kaynaklara erişim izni veren bir uygulama veya hizmet için bir *kimlik bilgileri* kümesidir. Hizmet sorumlusu kullanımı, uygulama izinlerini uygulamanın kullanıcısının izinleriyle ayırır.

Uygulamanın kullanıldığı her kiracıda bir hizmet sorumlusu oluşturulur. Hizmet sorumlusu, oturum açma için bir kimlik ve bu kiracı tarafından güvenliği sağlanmış kaynaklara (kullanıcılar gibi) erişim sağlar.

- Tek kiracılı bir uygulamanın, ilk oluşturulduğu dizinde olan yalnızca bir hizmet sorumlusu vardır. Bu hizmet sorumlusu, uygulamanın kaydı sırasında kullanılmak üzere oluşturulur ve kullanılır.
- Çok kiracılı bir Web uygulamasının veya API 'sinin, bu kiracıdan gelen bir kullanıcının uygulama kullanımıyla ilgili olduğu her bir kiracıda oluşturulmuş bir hizmet sorumlusu vardır.

Hizmet sorumluları için kimlik bilgileri, Azure portal veya sertifikayla oluşturulmuş bir anahtar olabilir. Sertifikalar anahtarlara göre daha güvenli olarak kabul edildiğinden, bir sertifika kullanımı otomasyon için uygundur.

> [!NOTE]
> Azure Stack ile AD FS kullandığınızda yalnızca yönetici hizmet sorumluları oluşturabilir. AD FS, hizmet sorumluları sertifika gerektirir ve ayrıcalıklı uç nokta (PEP) üzerinden oluşturulur. Daha fazla bilgi için bkz. [kaynaklara erişmek için uygulama kimliği kullanma](azure-stack-create-service-principals.md).

Azure Stack için hizmet sorumluları hakkında bilgi edinmek için bkz. [hizmet sorumluları oluşturma](azure-stack-create-service-principals.md).

### <a name="services"></a>Hizmetler

Kimlik sağlayıcısıyla etkileşime geçen Azure Stack hizmetler, kimlik sağlayıcısı ile uygulama olarak kaydedilir. Uygulamalar gibi, kayıt, bir hizmetin kimlik sistemiyle kimlik doğrulamasını sağlar.

Tüm Azure Hizmetleri, kimliğini oluşturmak için [OpenID Connect](/azure/active-directory/develop/active-directory-protocols-openid-connect-code) protokollerini ve [JSON Web belirteçlerini](/azure/active-directory/develop/active-directory-token-and-claims) kullanır. Azure AD ve AD FS protokolleri sürekli olarak kullandığından, şirket içinde veya Azure 'da (bağlı bir senaryoda) kimlik doğrulaması yapmak için [Azure Active Directory kimlik doğrulama kitaplığı](/azure/active-directory/develop/active-directory-authentication-libraries) 'nı (ADAL) kullanabilirsiniz. ADAL ile, platformlar arası ve şirket içi kaynak yönetimi için Azure PowerShell ve Azure CLı gibi araçları da kullanabilirsiniz.

### <a name="identities-and-your-identity-system"></a>Kimlikler ve kimlik sisteminiz

Azure Stack kimlikleri, Kullanıcı hesaplarını, grupları ve hizmet sorumlularını içerir.

Azure Stack yüklediğinizde, çeşitli yerleşik uygulamalar ve hizmetler otomatik olarak dizin kiracısında kimlik sağlayıcınızla kayıt kaydeder. Kayıt yapan bazı hizmetler yönetim için kullanılır. Kullanıcılar için diğer hizmetler kullanılabilir. Varsayılan kayıtlar, hem birbirleriyle hem de daha sonra eklediğiniz kimliklerle etkileşime girebilen çekirdek hizmet kimliklerini sağlar.

Azure AD 'yi çok kiracılı bir şekilde ayarlarsanız, bazı uygulamalar yeni dizinlere yayılır.

## <a name="authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme

### <a name="authentication-by-apps-and-users"></a>Uygulamalar ve kullanıcılar tarafından kimlik doğrulaması

![Azure Stack katmanları arasındaki kimlik](media/azure-stack-identity-overview/identity-layers.png)

Uygulamalar ve kullanıcılar için Azure Stack mimarisi dört katmanda açıklanmıştır. Bu katmanların her biri arasındaki etkileşimler farklı kimlik doğrulama türlerini kullanabilir.

|Katman    |Katmanlar arasında kimlik doğrulaması  |
|---------|---------|
|Yönetici portalı gibi araçlar ve istemciler     | Azure Stack bir kaynağa erişmek veya bu kaynaklara erişmek için araçlar ve istemciler Azure Resource Manager çağrısı koymak için [JSON Web Token](/azure/active-directory/develop/active-directory-token-and-claims) kullanır. <br>Azure Resource Manager, JSON Web Token doğrular ve Kullanıcı veya hizmet sorumlusunun Azure Stack sahip olduğu yetkilendirme düzeyini tahmin etmek için verilen belirteçteki *taleplere* bakar. |
|Azure Resource Manager ve Çekirdek Hizmetleri     |Azure Resource Manager, kullanıcılardan iletişim aktarmaya yönelik kaynak sağlayıcılarla iletişim kurar. <br> Aktarımlar *doğrudan* zorunlu çağrılar veya [Azure Resource Manager şablonlar](/azure-stack/user/azure-stack-arm-templates)aracılığıyla *bildirime dayalı* çağrılar kullanır.|
|Kaynak sağlayıcıları     |Kaynak sağlayıcılarına geçirilen çağrılar sertifika tabanlı kimlik doğrulamasıyla korunmaktadır. <br>Azure Resource Manager ve kaynak sağlayıcı, bir API aracılığıyla iletişim içinde kalır. Azure Resource Manager alınan her çağrı için, kaynak sağlayıcısı bu sertifikayla olan çağrıyı doğrular.|
|Altyapı ve iş mantığı     |Kaynak sağlayıcıları, tercih ettikleri bir kimlik doğrulama modu kullanarak iş mantığı ve altyapıyla iletişim kurar. Azure Stack ile birlikte gelen varsayılan kaynak sağlayıcıları, bu iletişimin güvenliğini sağlamak için Windows kimlik doğrulamasını kullanır.|

![Kimlik doğrulaması için gereken bilgiler](media/azure-stack-identity-overview/authentication.png)

### <a name="authenticate-to-azure-resource-manager"></a>Azure Resource Manager kimlik doğrulaması

Kimlik sağlayıcısıyla kimlik doğrulaması yapmak ve bir JSON Web Token almak için aşağıdaki bilgilere sahip olmanız gerekir:

1. **Kimlik sistemi (Authority) URL 'si**: Kimlik sağlayıcınızda erişilebileceği URL. Örneğin, *https: \//Login. Windows. net*.
2. **Azure Resource Manager Için uygulama kimliği URI 'si**: Kimlik sağlayıcınızda kayıtlı Azure Resource Manager için benzersiz tanımlayıcı. Her Azure Stack yüklemesi için de benzersizdir.
3. **Kimlik bilgileri**: Kimlik sağlayıcısı ile kimlik doğrulamak için kullandığınız kimlik bilgileri.
4. **Azure Resource Manager URL 'si**: URL, Azure Resource Manager hizmetinin konumudur. Örneğin, *https: \//Management. Azure. com* veya *https: \//Management. Local. azurestack. external*.

Bir asıl (istemci, uygulama veya Kullanıcı) bir kaynağa erişmek için kimlik doğrulama isteği yaptığında, istek şunları içermelidir:

- Sorumlu kimlik bilgileri.
- Sorumlunun erişmek istediği kaynağın uygulama KIMLIĞI URI 'SI.

Kimlik bilgileri kimlik sağlayıcısı tarafından onaylanır. Kimlik sağlayıcısı aynı zamanda uygulama KIMLIĞI URI 'sinin kayıtlı bir uygulama için olduğunu ve sorumlunun bu kaynak için bir belirteç elde etmek için doğru ayrıcalıklara sahip olduğunu doğrular. İstek geçerliyse, bir JSON Web Token verilir.

Belirtecin daha sonra Azure Resource Manager için bir isteğin üstbilgisine geçmesi gerekir. Azure Resource Manager, belirli bir sıraya göre aşağıdakileri yapar:

- Belirtecin doğru kimlik sağlayıcısından olduğunu onaylamak için *veren* (ISS) talebini doğrular.
- Belirtecin Azure Resource Manager için verildiğini onaylamak için *hedef kitle* (AUD) talebini doğrular.
- JSON Web Token, OpenID aracılığıyla yapılandırılan ve Azure Resource Manager bilinen bir sertifikayla imzalandığını doğrular.
- Belirtecin etkin olduğunu ve kabul edilebilir olduğunu onaylamak için *çıkarılan* (iat) ve *süre sonu* (Exp) taleplerini gözden geçirin.

Tüm doğrulamalar tamamlandığında, Azure Resource Manager,, birincil kaynağın erişebileceği kaynakların bir listesini oluşturmak için *objected* (OID) ve *gruplar* taleplerini kullanır.

![Belirteç değişim protokolünün diyagramı](media/azure-stack-identity-overview/token-exchange.png)

> [!NOTE]
> Dağıtımdan sonra, Azure Active Directory genel yönetici izni gerekli değildir. Ancak bazı işlemler, genel yönetici kimlik bilgileri (örneğin, bir kaynak sağlayıcısı yükleyicisi betiği veya izin verilmesini gerektiren yeni bir özellik) gerektirebilir. Hesabın genel yönetici izinlerini geçici olarak yeniden belirleyebilir veya *varsayılan sağlayıcı aboneliğinin*sahibi olan ayrı bir genel yönetici hesabı kullanabilirsiniz.

### <a name="use-role-based-access-control"></a>Rol tabanlı Access Control kullanma

Azure Stack'te Rol Tabanlı Erişim Denetimi (RBAC), Microsoft Azure'daki uygulamayla tutarlıdır. Kullanıcılara, gruplara ve uygulamalara uygun RBAC rolünü atayarak kaynaklara erişimi yönetebilirsiniz. RBAC 'yi Azure Stack kullanma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Portal rol tabanlı Access Control kullanmaya](/azure/role-based-access-control/overview)başlayın.
- [Azure abonelik kaynaklarınıza erişimi yönetmek Için rol tabanlı Access Control kullanın](/azure/role-based-access-control/role-assignments-portal).
- [Azure rol tabanlı Access Control için özel roller oluşturun](/azure/role-based-access-control/custom-roles).
- [Rol tabanlı Access Control](azure-stack-manage-permissions.md) Azure Stack içinde yönetin.

### <a name="authenticate-with-azure-powershell"></a>Azure PowerShell ile kimlik doğrulaması

Azure Stack ile kimlik doğrulamak için Azure PowerShell kullanma hakkında ayrıntılar [Azure Stack kullanıcının PowerShell ortamını yapılandırma](../user/azure-stack-powershell-configure-user.md)adresinde bulunabilir.

### <a name="authenticate-with-azure-cli"></a>Azure CLı ile kimlik doğrulama

Azure Stack kimlik doğrulaması için Azure PowerShell kullanma hakkında daha fazla bilgi için bkz. [Azure CLI 'yi Azure Stack ile kullanmak üzere yükleyip yapılandırma](/azure-stack/user/azure-stack-version-profiles-azurecli2).

## <a name="next-steps"></a>Sonraki adımlar

- [Kimlik mimarisi](azure-stack-identity-architecture.md)
- [Veri merkezi tümleştirmesi-kimlik](azure-stack-integrate-identity.md)
