---
title: Azure Stack tümleşik sistemler için Azure 'a bağlı dağıtım kararları | Microsoft Docs
description: Faturalandırma ve kimlik dahil Azure Stack tümleşik sistemlerin Azure 'a bağlı dağıtımları için dağıtım planlama kararlarını belirleme.
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
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: wfayed
ms.lastreviewed: 11/05/2018
ms.openlocfilehash: bffd86e876f212fbcbf14f8c3a1e9ce61cda7a4b
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829073"
---
# <a name="azure-connected-deployment-planning-decisions-for-azure-stack-integrated-systems"></a>Azure Stack tümleşik sistemler için Azure 'a bağlı dağıtım planlama kararları
[Karma bulut ortamınıza Azure Stack nasıl tümleştirileceğini](azure-stack-connection-models.md)karardıktan sonra, Azure Stack dağıtım kararlarınızı son haline getirebilirsiniz.

Azure 'a bağlı Azure Stack dağıtmak, kimlik deponuzda Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) (AD FS) sahip olabileceği anlamına gelir. Ayrıca faturalandırma modelinden, Kullandıkça Öde veya kapasite tabanlı seçeneklerinden birini de seçebilirsiniz. Bağlı bir dağıtım, müşterilerin, özellikle Azure ve Azure Stack içeren karma bulut senaryolarında Azure Stack en fazla değeri almasına izin verdiğinden, varsayılan seçenektir.

## <a name="choose-an-identity-store"></a>Bir kimlik deposu seçin
Bağlı bir dağıtımla, kimlik deponuzda Azure AD veya AD FS arasında seçim yapabilirsiniz. İnternet bağlantısı olmayan, bağlantısı kesilen bir dağıtım yalnızca AD FS kullanabilir.

Kimlik deposu tercih ettiğiniz kiracı sanal makinelerinde (VM) hiç bir pul yok. Kiracı VM 'Leri, nasıl yapılandırdığınıza bağlı olarak, hangi kimlik deposunu bağlamak istediğinizi seçebilir: Azure AD, Windows Server Active Directory etki alanına katılmış, çalışma grubu vb. Bu, Azure Stack kimlik sağlayıcısı kararı ile ilgisiz değildir.

Örneğin, IaaS Kiracı VM 'lerini Azure Stack en üstünde dağıtıp bir kurumsal Active Directory Etki Alanı katmak ve hesapları buradan kullanmak istiyorsanız, yine de yapabilirsiniz. Bu hesaplar için burada seçtiğiniz Azure AD kimlik deposunu kullanmanız gerekmez.

### <a name="azure-ad-identity-store"></a>Azure AD kimlik deposu
Kimlik deponuzda Azure AD 'nin kullanılması iki Azure AD hesabı gerektirir: genel yönetici hesabı ve faturalandırma hesabı. Bu hesaplar, aynı hesaplar veya farklı hesaplar olabilir. Aynı kullanıcı hesabının kullanılması daha basit olabilir ve sınırlı sayıda Azure hesabınız varsa, iş gereksinimleriniz iki hesap kullanılarak önerilmesi gerekebilir:

1. **Genel yönetici hesabı** (yalnızca bağlı dağıtımlar için gereklidir). Bu, Azure AD 'de Azure Stack altyapı hizmetleri için uygulama ve hizmet sorumluları oluşturmak için kullanılan bir Azure hesabıdır. Bu hesabın, Azure Stack sisteminizin dağıtılacağı dizinde Dizin Yöneticisi izinleri olması gerekir. Azure AD kullanıcısı için "bulut operatörü" genel yöneticisi olur ve aşağıdaki görevler için kullanılır:

    - Azure AD ve Graph API etkileşimde olması gereken tüm Azure Stack hizmetlere yönelik uygulamalar ve hizmet sorumlularını sağlamak ve atamak için.
    - Hizmet Yöneticisi hesabı olarak. Bu hesap, varsayılan sağlayıcı aboneliğinin sahibidir (daha sonra değişiklik yapabilirsiniz). Bu hesapla Azure Stack yönetici portalında oturum açabilir ve bu hesabı teklif ve planlar oluşturmak, Kotalar ayarlamak ve Azure Stack diğer yönetim işlevlerini gerçekleştirmek için kullanabilirsiniz.

2. **Faturalandırma hesabı** (hem bağlı hem de bağlantısı kesilen dağıtımlar için gereklidir). Bu Azure hesabı, Azure Stack tümleşik sisteminiz ile Azure ticari arka ucu arasında faturalandırma ilişkisi kurmak için kullanılır. Bu, Azure Stack ücretleri için faturalandırılan hesaptır. Bu hesap, Market 'te ve diğer karma senaryolarda öğe sunmak için de kullanılacaktır.

### <a name="ad-fs-identity-store"></a>AD FS Identity Store
Hizmet Yöneticisi hesaplarınız için şirketinizin Active Directory gibi kendi kimlik deponuzu kullanmak istiyorsanız bu seçeneği belirleyin.  

## <a name="choose-a-billing-model"></a>Faturalandırma modeli seçin
**Kullandıkça Öde** veya **Kapasite** faturalandırma modeli seçeneklerinden birini belirleyebilirsiniz. Kullandıkça Öde faturalandırma modeli dağıtımları, en az 30 günde bir Azure bağlantısı aracılığıyla kullanımı bildirebilmelidir. Bu nedenle, Kullandıkça Öde faturalandırma modeli yalnızca bağlı dağıtımlar için kullanılabilir.  

### <a name="pay-as-you-use"></a>Kullandıkça öde
Kullandıkça Öde faturalama modeliyle kullanım, bir Azure aboneliği üzerinden ücretlendirilir. Yalnızca Azure Stack hizmetlerini kullandığınızda ödeme yaparsınız. Bu, karar vereceğiniz modelde varsa, bir Azure aboneliğine ve bu abonelikle ilişkili hesap KIMLIĞINE (örneğin, serviceadmin@contoso.onmicrosoft.com) sahip olmanız gerekir. EA, CSP ve CSL abonelikleri desteklenir. Kullanım raporlaması [Azure Stack kaydı](azure-stack-registration.md)sırasında yapılandırılır.

> [!NOTE]
> Çoğu durumda kurumsal müşteriler EA aboneliklerini kullanır ve hizmet sağlayıcıları CSP veya CSL abonelikleri kullanır.

CSP aboneliğini kullanacaksanız, doğru yaklaşım CSP senaryosuna bağlı olarak hangi CSP aboneliğinin kullanılacağını belirlemek için aşağıdaki tabloyu gözden geçirin:

|Senaryo|Etki alanı ve abonelik seçenekleri|
|-----|-----|
|**Doğrudan BIR CSP Iş ortağıysanız** veya **dolaylı bir csp sağlayıcısıdır**, Azure Stack çalıştıracaksınız|Bir CSL (ortak hizmet katmanı) aboneliği kullanın.<br>     or<br>Iş Ortağı Merkezi 'nde açıklayıcı bir ada sahip bir Azure AD kiracısı oluşturun. Örneğin, &lt;kuruluşunuz ile ilişkili bir Azure CSP aboneliğiyle cspadmin >.|
|Bir **dolaylı CSP satıcısı**olduğunuz için Azure Stack|Dolaylı CSP sağlayıcınızdan, Iş Ortağı Merkezi 'ni kullanarak kendisiyle ilişkili bir Azure CSP aboneliği ile kuruluşunuz için bir Azure AD kiracısı oluşturmasını isteyin.|

### <a name="capacity-based-billing"></a>Kapasite tabanlı faturalandırma
Kapasite faturalandırma modelini kullanmaya karar verirseniz, sisteminizin kapasitesini temel alarak bir Azure Stack kapasite planı SKU 'SU satın almanız gerekir. Doğru miktarı satın almak için Azure Stack fiziksel çekirdek sayısını bilmeniz gerekir.

Kapasite faturalaması kayıt için bir Kurumsal Anlaşma (EA) Azure aboneliği gerektirir. Bu nedenle, kayıt, Market 'te Azure aboneliği gerektiren öğelerin kullanılabilirliğini ayarlamadır. Abonelik Azure Stack kullanım için kullanılmıyor.

## <a name="learn-more"></a>Daha fazla bilgi edinin
- Kullanım örnekleri, satın alma, iş ortakları ve OEM donanım satıcıları hakkında daha fazla bilgi için [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) ürün sayfasına bakın.
- Azure Stack tümleşik sistemler için yol haritası ve coğrafi kullanılabilirlik hakkında daha fazla bilgi için, bkz. Teknik İnceleme: [Azure Stack: Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/)uzantısı. 
- Microsoft Azure Stack paketleme ve fiyatlandırma hakkında daha fazla bilgi edinmek için [. PDF dosyasını indirin](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf). 

## <a name="next-steps"></a>Sonraki adımlar
[Veri merkezi ağ tümleştirmesi](azure-stack-network.md)