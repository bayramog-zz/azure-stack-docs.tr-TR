---
title: Azure Stack | API sürüm profillerini yönetme | Microsoft Docs
description: Azure Stack 'deki API sürüm profilleri hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2019
ms.author: sethm
ms.reviewer: sijuman
ms.lastreviewed: 07/24/2019
ms.openlocfilehash: 2d57c0ff71cd0aa4280e6a21df4727ee4bbe7fb6
ms.sourcegitcommit: 7961fda0bfcdd3db8cf94a8c405b5c23a23643af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68616492"
---
# <a name="manage-api-version-profiles-in-azure-stack"></a>Azure Stack API sürüm profillerini yönetme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

API profillerini Azure kaynak sağlayıcısı ve Azure REST uç noktaları için API sürümü belirtin. API profillerini kullanarak farklı dillerde özel istemciler oluşturabilirsiniz. Her istemci, Azure Stack’e ilişkin doğru kaynak sağlayıcısı ve API sürümüne başvurmak için bir API profili kullanır.

Her bir kaynak sağlayıcısı API 'sinin tam olarak hangi sürümünün Azure Stack uyumlu olduğunu sıralamak zorunda kalmadan Azure Kaynak sağlayıcılarıyla çalışmak üzere bir uygulama oluşturabilirsiniz. Uygulamanızı bir profile hizalamanız yeterlidir ve SDK doğru API sürümüne geri döner.

Bu konu size yardımcı olur:

 - Azure Stack için API profillerini anlayın.
 - Çözümlerinizi geliştirmek için API profillerini nasıl kullanabileceğinizi öğrenin.
 - Bkz. koda özgü kılavuzun nerede bulunacağını öğrenin.

## <a name="summary-of-api-profiles"></a>API profillerinin Özeti

- API profilleri, Azure Kaynak sağlayıcılarının ve API sürümlerinin bir kümesini temsil etmek için kullanılır.
- Birden çok Azure bulutu arasında şablon oluşturmanız için API profilleri oluşturuldu. Profiller uyumlu ve kararlı bir arabirim sağlar.
- Profiller yılda dört kez serbest bırakılır.
- Üç profil adlandırma kuralı kullanılır:
    - **sürümü**  
        Küresel Azure 'da yayınlanan en son API sürümlerini içerir.
    - **yyyy-mm-dd-hybrid**  
    Bu sürüm, birden fazla bulutta tutarlılık ve kararlılık üzerine odaklanır. Bu profil, uyumluluk Azure Stack en iyi şekilde hedefler.
    - **yyyy-aa-gg-profil** <br>
    En iyi kararlılığı ve en son özellikleri dengeler.

## <a name="azure-api-profiles-and-azure-stack-compatibility"></a>Azure API profilleri ve Azure Stack uyumluluğu

En yeni Azure API profilleri Azure Stack uyumlu değildir. Azure Stack çözümleriniz için hangi profillerin kullanılacağını belirlemek için aşağıdaki adlandırma kurallarını kullanın:

**Sürümü**  
Bu profilde, genel Azure 'da bulunan en güncel API sürümleri bulunur ve bu, Azure Stack ' de çalışmaz. **En son** , en fazla sayıda Son değişiklik sayısına sahiptir. Profil, kararlılığı ve diğer bulutlarla uyumluluğu getirir. En güncel API sürümlerini kullanmaya çalışıyorsanız **en son** , kullanmanız gereken profildir.

**Yyyy-aa-gg-karma**  
Bu profil, her yıl Mart ayında yayımlanmıştır. En iyi kararlılık ve çeşitli bulutlarla uyumluluk vardır. **Yyyy-aa-gg-karma** , küresel Azure ve Azure Stack hedeflemek için tasarlanmıştır. Bu profilde listelenen Azure API sürümleri, Azure Stack listelenen olanlarla aynı olacaktır. Karma bulut çözümleri için kod geliştirmek üzere bu profili kullanın.

**yyyy-aa-gg-profil**  
Bu profil Haziran ve Aralık ayında Global Azure için yayımlanmıştır. Azure Stack ile çalışmaz ve genellikle birçok Son değişiklik olacaktır. En iyi kararlılığı ve en son özellikleri dengeleyip, en **son ve bu** profil arasındaki fark, en **son** API SÜRÜMLERINDEN bağımsız olarak, API 'nin ne zaman YAYıNLANDıĞıNA bakılmaksızın en yeni API sürümlerinden oluşur. Örneğin, yarın Işlem API 'si için yeni bir API sürümü oluşturulursa, bu profil zaten mevcut olduğundan, bu API sürümü en son, **yyyy-aa-gg-profile** profilinde listelenmez ancak **en son**içinde listelenir. **yyyy-aa-gg-profil** , Haziran 'dan önce veya Aralık öncesinde yayınlanan en güncel sürümleri içerir.

## <a name="azure-resource-manager-api-profiles"></a>Azure Resource Manager API profilleri

Azure Stack, genel Azure 'da bulunan API sürümlerinin en son sürümünü kullanmaz. Bir çözüm oluşturduğunuzda, Azure Stack ile uyumlu olan her bir Azure Kaynak sağlayıcısı için API sürümünü bulmanız gerekir.

Her kaynak sağlayıcısını ve Azure Stack tarafından desteklenen belirli sürümü araştırmak yerine bir API profili kullanabilirsiniz. Profil, bir kaynak sağlayıcıları ve API sürümleri kümesi belirtir. SDK veya SDK ile oluşturulmuş bir araç, profilde belirtilen hedefe `api-version` döndürülecek. API profilleriyle, tüm şablon için geçerli olan bir profil sürümü belirtebilirsiniz. Çalışma zamanında, Azure Resource Manager kaynağın doğru sürümünü seçer.

API profilleri, PowerShell, Azure CLı, SDK 'da sunulan kod ve Microsoft Visual Studio gibi Azure Resource Manager kullanan araçlarla çalışır. Araçlar ve SDK 'lar, bir uygulama oluştururken hangi modül ve kitaplıkların hangi sürümünün dahil edileceğini okumak için profilleri kullanabilir.

Örneğin, PowerShell 'i kullanarak bir depolama hesabı oluşturmak için PowerShell kullanıyorsanız, **API-** sürümü 2016-03-30 ve API-Version ile **Microsoft. COMPUTE** kaynak sağlayıcısını kullanan bir VM 'Yi destekleyen **Microsoft. Storage** kaynak sağlayıcısını destekler 2015-12-01, depolama için 2016-03-30 ' yi hangi PowerShell modülünün desteklediğini ve hangi modülün işlem için 2015-02-01 ' i desteklediğini aramanız ve ardından bunları yüklemesi gerekir. Bunun yerine, bir profil kullanabilirsiniz. Cmdlet 'ini `Install-Profile <profilename>`kullanın ve PowerShell, modüllerin doğru sürümünü yükler.

Benzer şekilde, Python tabanlı bir uygulama oluşturmak için Python SDK kullanırken, profili belirtebilirsiniz. SDK, betikte belirttiğiniz kaynak sağlayıcıları için doğru modülleri yükler.

Geliştirici olarak, çözümünüzü yazmaya odaklanırsınız. Hangi API sürümlerini, kaynak sağlayıcısını ve bulutu birlikte çalıştığını araştırmak yerine, bir profil kullanabilir ve kodunuzun bu profili destekleyen tüm bulutlarda çalıştığını bilirsiniz.

## <a name="api-profile-code-samples"></a>API profili kod örnekleri

Profilleri kullanarak Azure Stack, çözümünüzü tercih ettiğiniz dille tümleştirmenize yardımcı olması için kod örnekleri bulabilirsiniz. Şu anda aşağıdaki diller için rehberlik ve örnekler bulabilirsiniz:

- **.NET** <br>
Bir kaynak sağlayıcısı paketindeki her kaynak türünün en son, en kararlı sürümünü almak için .NET API profilini kullanın. Daha fazla bilgi için bkz. [Azure Stack .net Ile API sürüm profillerini kullanma](azure-stack-version-profiles-net.md).
- **PowerShell**  
API sürümü profilleriyle çalışmak için gerekli PowerShell cmdlet 'lerini almak için PowerShell Galerisi aracılığıyla kullanılabilen **Azurerm. Bootstrapper** modülünü kullanın. Bilgi için bkz. [PowerShell IÇIN API sürüm profillerini kullanma](azure-stack-version-profiles-powershell.md).
- **Azure CLI**  
Ortam yapılandırmanızı Azure Stack belirli API sürümü profilini kullanacak şekilde güncelleştirin. Bilgi için bkz. [Azure CLI IÇIN API sürüm profillerini kullanma](azure-stack-version-profiles-azurecli2.md).
- **Go**  
Git SDK 'da, bir profil farklı hizmetlerden farklı sürümlere sahip farklı kaynak türlerinin bir birleşimidir. Profiller, profiller/yol altında, sürümleri **yyyy-aa-gg** biçiminde kullanılabilir. Bilgi için bkz. [Go IÇIN API sürüm profillerini kullanma](azure-stack-version-profiles-go.md).
- **Ruby**  
Azure Stack Kaynak Yöneticisi için Ruby SDK, altyapınızı oluşturmanıza ve yönetmenize yardımcı olacak araçlar sağlar. SDK 'daki kaynak sağlayıcıları, Ruby dili ile işlem, sanal ağlar ve depolama alanı içerir. Bilgi için bkz. [Ruby Ile API sürüm profillerini kullanma](azure-stack-version-profiles-ruby.md).
- **Python**  
Python SDK, Azure Stack ve küresel Azure gibi farklı bulut platformlarını hedeflemek için API sürüm profillerini destekler. Karma bulut için çözümler oluşturmak üzere API profillerini kullanın. Bilgi için bkz. [Python Ile API sürüm profillerini kullanma](azure-stack-version-profiles-python.md).
- **Node.js**  
Azure Stack Kaynak Yöneticisi için Node. js SDK 'Sı, altyapınızı oluşturmanıza ve yönetmenize yardımcı olacak araçlar sağlar. Daha fazla bilgi için bkz. [Node. js Ile API sürüm profillerini kullanma](azure-stack-version-profile-nodejs.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stack için PowerShell yükleme](../operator/azure-stack-powershell-install.md)
* [Azure Stack kullanıcının PowerShell ortamını yapılandırma](azure-stack-powershell-configure-user.md)
* [Profiller tarafından desteklenen kaynak sağlayıcısı API sürümleri hakkındaki ayrıntıları gözden geçirin](azure-stack-profiles-azure-resource-manager-versions.md).
