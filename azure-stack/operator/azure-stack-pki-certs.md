---
title: Azure Stack ortak anahtar altyapısı sertifika gereksinimleri | Microsoft Docs
description: Azure Stack tümleşik sistemler için Azure Stack PKI sertifikası dağıtım gereksinimleri hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2019
ms.author: justinha
ms.reviewer: ppacent
ms.lastreviewed: 09/10/2019
ms.openlocfilehash: f306391451c4d04af3b5a37645f145fb732714f0
ms.sourcegitcommit: acebda8a42ac8ecdeba490fc1738e9041479dab0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72814002"
---
# <a name="azure-stack-public-key-infrastructure-certificate-requirements"></a>Azure Stack ortak anahtar altyapısı sertifika gereksinimleri

Azure Stack, küçük bir Azure Stack hizmetlerine ve muhtemelen Kiracı VM 'lerine atanan, dışarıdan erişilebilen genel IP adreslerini kullanan genel bir altyapı ağına sahiptir. Azure Stack dağıtımı sırasında bu Azure Stack genel altyapı uç noktaları için uygun DNS adlarıyla PKI sertifikaları gereklidir. Bu makalede hakkında bilgi verilmektedir:

- Azure Stack dağıtmak için hangi sertifikaların gerekli olduğu.
- Bu belirtimlerle eşleşen sertifikaları alma işlemi.
- Dağıtım sırasında bu sertifikaların hazırlanması, doğrulanması ve kullanılması.

> [!NOTE]
> Azure Stack, varsayılan olarak, düğümler arasında kimlik doğrulaması için Dahili Active Directory tümleşik bir sertifika yetkilisinden (CA) verilen sertifikaları kullanır. Sertifikayı doğrulamak için tüm Azure Stack altyapı makineleri, bu sertifikayı yerel sertifika depolama alanına ekleyerek iç CA 'nın kök sertifikasına güvenir. Azure Stack sertifikaların sabitleme veya beyaz listesi yoktur. Her bir sunucu sertifikasının SAN 'ı hedefin FQDN 'sine göre onaylanır. Tüm güven zinciri de, sertifika sona erme tarihi (sertifika sabitleme olmadan standart TLS sunucu kimlik doğrulaması) ile birlikte da onaylanır.

## <a name="certificate-requirements"></a>Sertifika gereksinimleri
Aşağıdaki listede, Azure Stack dağıtmak için gereken sertifika gereksinimleri açıklanmaktadır:

- Sertifikalar, bir iç sertifika yetkilisinden veya bir genel sertifika yetkilisinden verilmelidir. Ortak bir sertifika yetkilisi kullanılıyorsa, Microsoft güvenilir kök yetkilisi programının bir parçası olarak temel işletim sistemi görüntüsüne dahil edilmelidir. Tam liste için bkz. [Microsoft güvenilen kök sertifika programı: katılımcılar](https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca).
- Azure Stack altyapınız, sertifika yetkilisinin sertifika Iptal listesi (CRL) konumunda yayımlanan sertifika için ağ erişimine sahip olmalıdır. Bu CRL bir HTTP uç noktası olmalıdır.
- 1903 öncesi yapılarda sertifika döndürürken, sertifikaların dağıtımda veya yukarıdaki herhangi bir genel sertifika yetkilisinde verilen sertifikaları imzalamak için kullanılan dahili sertifika yetkilisinden verilmiş olması gerekir. 1903 ve sonraki sürümler için, sertifikalar herhangi bir kurumsal veya ortak sertifika yetkilisi tarafından verilebilir.
- Otomatik olarak imzalanan sertifikaların kullanımı desteklenmez.
- Dağıtım ve döndürme için, sertifikanın konu adı ve konu alternatif adı (SAN) alanlarında tüm ad alanlarını kapsayan tek bir sertifika kullanabilir veya Azure Stack aşağıdaki her bir ad alanı için tek tek sertifikaları kullanabilirsiniz kullanmayı planladığınız hizmetler gerektir. Her iki yaklaşım da, gerektiğinde **Anahtar Kasası** ve **keyvaultınternal**gibi uç noktalar için joker karakterler kullanılmasını gerektirir.
- Sertifikanın PFX şifrelemesi 3DES olmalıdır.
- Sertifika imza algoritması SHA1 olmamalıdır.
- Azure Stack yüklemesi için hem ortak hem de özel anahtarlar gerekli olduğundan, sertifika biçiminin PFX olması gerekir. Özel anahtarın yerel makine anahtarı özniteliği ayarlanmış olmalıdır.
- PFX şifrelemesi 3DES olmalıdır (bir Windows 10 istemcisinden veya Windows Server 2016 sertifika deposundan dışarı aktarılırken bu şifreleme varsayılandır).
- Sertifika PFX dosyalarının "anahtar kullanımı" alanında "Digital Signature" ve "KeyEncipherment" değeri olmalıdır.
- Sertifika PFX dosyaları "Gelişmiş anahtar kullanımı" alanında "sunucu kimlik doğrulaması (1.3.6.1.5.5.7.3.1)" ve "Istemci kimlik doğrulaması (1.3.6.1.5.5.7.3.2)" değerlerine sahip olmalıdır.
- Sertifikanın "verilen:" alanı, "Issued by:" alanıyla aynı olmamalıdır.
- Tüm sertifika PFX dosyalarının parolaları, dağıtım sırasında aynı olmalıdır.
- Sertifika PFX parolasının parolası karmaşık bir parola olmalıdır. Dağıtım parametresi olarak kullanacağınız için bu parolayı unutmayın. Parolanın aşağıdaki parola karmaşıklığı gereksinimlerini karşılaması gerekir:
    - En az sekiz karakter uzunluğunda.
    - Şu karakterlerden en az üçünü: büyük harf, küçük harf, 0-9 arasındaki sayılar, özel karakterler, büyük harf veya küçük harf olmayan alfabetik karakter.
- Konu adı ve konu diğer adlarının konu alternatif adı uzantısı (x509v3_config) ile eşleştiğinden emin olun. Konu diğer adı alanı, tek bir SSL sertifikasıyla korunabilecek ek ana bilgisayar adlarını (Web siteleri, IP adresleri, ortak adlar) belirtmenize imkan tanır.

> [!NOTE]  
> Otomatik olarak imzalanan sertifikalar desteklenmez.

> [!NOTE]  
> Bir *sertifikanın güven zincirinde* ara sertifika yetkililerinin varlığı desteklenir.

## <a name="mandatory-certificates"></a>Zorunlu sertifikalar
Bu bölümdeki tabloda hem Azure AD hem de AD FS Azure Stack dağıtımları için gerekli olan genel uç nokta PKI sertifikaları Azure Stack açıklanmaktadır. Sertifika gereksinimleri alana göre gruplandırılır ve kullanılan ad alanları ve her ad alanı için gereken sertifikalar. Tablo ayrıca, çözüm sağlayıcınızın genel uç nokta başına farklı sertifikaları kopyaladığı klasörü de açıklar.

Her bir Azure Stack ortak altyapı uç noktası için uygun DNS adlarına sahip sertifikalar gereklidir. Her uç noktanın DNS adı şu biçimde ifade edilir: *&lt;önek >.&lt;bölge >.&lt;fqdn >* .

Dağıtımınız için, [Region] ve [externalfqdn] değerlerinin Azure Stack sisteminiz için seçtiğiniz bölge ve dış etki alanı adlarıyla eşleşmesi gerekir. Örnek olarak, bölge adı *Redmond* ve dış etki alanı adı *contoso.com*ise, DNS adlarında *&lt;önek >. Redmond. contoso. com*biçimindedir. *&lt;ön eki >* değerleri, sertifika tarafından güvenliği sağlanmış uç noktayı tanımlayacak şekilde Microsoft tarafından önceden atanır. Ayrıca, dış altyapı bitiş noktalarının *&lt;ön eki >* değerleri, belirli uç noktayı kullanan Azure Stack hizmetine bağlıdır.

Üretim ortamları için, her bir uç nokta için ayrı ayrı sertifikaların oluşturulmasını ve ilgili dizine kopyalanmasını öneririz. Geliştirme ortamları için, sertifikalar tüm dizinlere kopyalanmış konu ve konu alternatif adı (SAN) alanlarındaki tüm ad alanlarını kapsayan tek bir joker karakter sertifikası olarak sağlanabilirler. Tüm uç noktaları ve hizmetleri kapsayan tek bir sertifika, güvenli olmayan bir sonudır ve bu nedenle yalnızca geliştirme amaçlıdır. Her iki seçenek de, **ACS** gibi uç noktalar ve gerektiğinde Key Vault için joker karakter sertifikaları kullanmanızı gerektirdiğini unutmayın.

> [!Note]  
> Dağıtım sırasında, dağıtımı yaptığınız kimlik sağlayıcısıyla (Azure AD veya AD FS) eşleşen dağıtım klasörüne sertifikaları kopyalamanız gerekir. Tüm uç noktalar için tek bir sertifika kullanıyorsanız, bu sertifika dosyasını aşağıdaki tablolarda özetlenen her dağıtım klasörüne kopyalamanız gerekir. Klasör yapısı, dağıtım sanal makinesinde önceden oluşturulmuştur ve şurada bulunabilir: C:\CloudDeployment\Setup\Certificates.

| Dağıtım klasörü | Gerekli sertifika konusu ve konu diğer adları (SAN) | Kapsam (bölge başına) | Alt etki alanı adı |
|-------------------------------|------------------------------------------------------------------|----------------------------------|-----------------------------|
| Ortak Portal | Portal.&lt;Bölgesi >.&lt;FQDN > | Portallar | &lt;Bölgesi >.&lt;FQDN > |
| Yönetici portalı | adminportal.&lt;Bölgesi >.&lt;FQDN > | Portallar | &lt;Bölgesi >.&lt;FQDN > |
| Azure Resource Manager genel | yönetme.&lt;Bölgesi >.&lt;FQDN > | Azure Resource Manager | &lt;Bölgesi >.&lt;FQDN > |
| Yönetici Azure Resource Manager | adminmanagement.&lt;Bölgesi >.&lt;FQDN > | Azure Resource Manager | &lt;Bölgesi >.&lt;FQDN > |
| ACSBlob | *. blob.&lt;Bölgesi >.&lt;FQDN ><br>(Joker karakter SSL sertifikası) | Blob Depolama | Bun.&lt;Bölgesi >.&lt;FQDN > |
| ACSTable | *. Table.&lt;Bölgesi >.&lt;FQDN ><br>(Joker karakter SSL sertifikası) | Tablo Depolama | tablosundan.&lt;Bölgesi >.&lt;FQDN > |
| ACSQueue | *. Queue.&lt;Bölgesi >.&lt;FQDN ><br>(Joker karakter SSL sertifikası) | Kuyruk Depolama | Sıradaki.&lt;Bölgesi >.&lt;FQDN > |
| KeyVault | *. kasa.&lt;Bölgesi >.&lt;FQDN ><br>(Joker karakter SSL sertifikası) | Key Vault | ka.&lt;Bölgesi >.&lt;FQDN > |
| Keyvaultınternal | *. adminkasa.&lt;Bölgesi >.&lt;FQDN ><br>(Joker karakter SSL sertifikası) |  İç Keykasası |  Yönetim Kasası.&lt;Bölgesi >.&lt;FQDN > |
| Yönetici uzantısı ana bilgisayarı | *. adminhosting.\<Bölgesi >.\<FQDN > (joker SSL sertifikaları) | Yönetici uzantısı ana bilgisayarı | adminhosting.\<Bölgesi >.\<FQDN > |
| Ortak uzantı Konağı | *. barındırma.\<Bölgesi >.\<FQDN > (joker SSL sertifikaları) | Ortak uzantı Konağı | barındırıyor.\<Bölgesi >.\<FQDN > |

Azure AD Dağıtım modunu kullanarak Azure Stack dağıtırsanız, yalnızca önceki tabloda listelenen sertifikaları istemeniz gerekir. Ancak, AD FS Dağıtım modunu kullanarak Azure Stack dağıtırsanız, aşağıdaki tabloda açıklanan sertifikaları da istemeniz gerekir:

|Dağıtım klasörü|Gerekli sertifika konusu ve konu diğer adları (SAN)|Kapsam (bölge başına)|Alt etki alanı adı|
|-----|-----|-----|-----|
|ADFS|FS. *&lt;region >. &lt;fqdn >*<br>(SSL sertifikası)|ADFS|*&lt;Bölgesi >.&lt;FQDN >*|
|Graph|Çıkarılamıyor. *&lt;region >. &lt;fqdn >*<br>(SSL sertifikası)|Graph|*&lt;Bölgesi >.&lt;FQDN >*|
|

> [!IMPORTANT]
> Bu bölümde listelenen tüm sertifikaların parolası aynı olmalıdır.

## <a name="optional-paas-certificates"></a>İsteğe bağlı PaaS sertifikaları
Azure Stack dağıtıldıktan ve yapılandırıldıktan sonra ek Azure Stack PaaS hizmetlerini (SQL, MySQL ve App Service) dağıtmayı planlıyorsanız PaaS hizmetlerinin uç noktalarını kapsayacak ek sertifikalar istemeniz gerekir.

> [!IMPORTANT]
> App Service, SQL ve MySQL kaynak sağlayıcıları için kullandığınız sertifikaların, genel Azure Stack uç noktaları için kullanılanlarla aynı kök yetkilisiyle aynı olması gerekir.

Aşağıdaki tabloda, SQL ve MySQL bağdaştırıcıları ve App Service için gereken uç noktalar ve sertifikalar açıklanmaktadır. Bu sertifikaları Azure Stack dağıtım klasörüne kopyalamanız gerekmez. Bunun yerine, ek kaynak sağlayıcılarını yüklerken bu sertifikaları sağlarsınız.

|Kapsam (bölge başına)|Sertifika|Gerekli sertifika konusu ve konu diğer adları (San 'Lar)|Alt etki alanı adı|
|-----|-----|-----|-----|
|SQL, MySQL|SQL ve MySQL|&#42;. dbadapter. *&lt;bölge >.&lt;fqdn >*<br>(Joker karakter SSL sertifikası)|dbadapter. *&lt;bölge >.&lt;fqdn >*|
|App Service|Web trafiği varsayılan SSL sertifikası|&#42;appservice. *&lt;region >. &lt;fqdn >*<br>&#42;. scm. appservice. *&lt;region >. &lt;fqdn >*<br>&#42;. SSO. appservice. *&lt;bölge >.&lt;fqdn >*<br>(Çok etki alanı joker karakter SSL sertifikası<sup>1</sup>)|appservice. *&lt;bölge >.&lt;fqdn >*<br>SCM. appservice. *&lt;bölge >.&lt;fqdn >*|
|App Service|eklentisi|api. appservice. *&lt;region >. &lt;fqdn >*<br>(SSL sertifikası<sup>2</sup>)|appservice. *&lt;bölge >.&lt;fqdn >*<br>SCM. appservice. *&lt;bölge >.&lt;fqdn >*|
|App Service|FTP|FTP. appservice. *&lt;region >. &lt;fqdn >*<br>(SSL sertifikası<sup>2</sup>)|appservice. *&lt;bölge >.&lt;fqdn >*<br>SCM. appservice. *&lt;bölge >.&lt;fqdn >*|
|App Service|SSO|SSO. appservice. *&lt;bölge >.&lt;fqdn >*<br>(SSL sertifikası<sup>2</sup>)|appservice. *&lt;bölge >.&lt;fqdn >*<br>SCM. appservice. *&lt;bölge >.&lt;fqdn >*|

<sup>1</sup> birden çok joker karakter konu diğer adına sahip bir sertifika gerektirir. Tek bir sertifikadaki birden çok joker karakter, tüm genel sertifika yetkilileri tarafından desteklenmiyor olabilir.

<sup>2</sup> A &#42;. appservice. *&lt;bölgesi >.&lt;fqdn >* joker karakter sertifikası bu üç sertifikanın yerine kullanılamaz (API. appservice. *&lt;region >.&lt;FQDN >* , FTP. appservice. *&lt;bölge >.&lt;FQDN >* ve SSO. appservice. *&lt;bölge >.&lt;fqdn >* . Appservice, bu uç noktalar için ayrı sertifikaların kullanılmasını açıkça gerektirir.

## <a name="learn-more"></a>Daha fazla bilgi
[Azure Stack dağıtımı IÇIN PKI sertifikaları oluşturmayı](azure-stack-get-pki-certs.md)öğrenin.

## <a name="next-steps"></a>Sonraki adımlar
[AD FS kimliğini Azure Stack veri merkezinizle tümleştirin](azure-stack-integrate-identity.md).