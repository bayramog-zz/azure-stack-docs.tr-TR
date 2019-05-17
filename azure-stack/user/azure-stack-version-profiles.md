---
title: Azure stack'teki API sürümü profillerini yönetme | Microsoft Docs
description: Azure stack'teki API sürümü profillerini hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 05/16/2019
ms.openlocfilehash: 1557403536764f8aa88a271d183436a3d35954ae
ms.sourcegitcommit: 889fd09e0ab51ad0e43552a800bbe39dc9429579
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65782470"
---
# <a name="manage-api-version-profiles-in-azure-stack"></a>Azure stack'teki API sürümü profillerini yönetme

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

API profillerini Azure kaynak sağlayıcısı ve Azure REST uç noktaları için API sürümü belirtin. Özel istemciler API profilleri kullanılarak farklı dillerde oluşturabilirsiniz. Her istemci, Azure Stack’e ilişkin doğru kaynak sağlayıcısı ve API sürümüne başvurmak için bir API profili kullanır.

Tam olarak hangi sürümün her kaynak sağlayıcısı API'si, Azure Stack ile uyumlu olduğunu sıralamaya gerek kalmadan Azure kaynak sağlayıcıları ile çalışmak için bir uygulama oluşturabilirsiniz. Yalnızca bir profili uygulamanıza hizalama; SDK'sı doğru API sürümüne geri döner.

Bu konuda size yardımcı olur:

 - Azure Stack için API profillerini anlayın.
 - Çözümlerinizi geliştirmek için API profillerini nasıl kullanabileceğinizi öğrenin.
 - Kod özgü yönergeler nerede bulacağını bakın.

## <a name="summary-of-api-profiles"></a>API profillerini özeti

- API profilleri, bir Azure kaynak sağlayıcıları ve API sürümlerini temsil etmek için kullanılır.
- Birden çok Azure bulutlarda şablonları oluşturmak için API profillerini oluşturulur. Profilleri, uyumlu ve kararlı bir arabirim sağlar.
- Profilleri, yılda dört kez serbest bırakılır.
- Üç profil adlandırma kuralları kullanılır:
    - **en son**  
        Genel Azure'da yayımlanan en son API sürümlerini içerir.
    - **yyyy-mm-dd-hybrid**  
    Yılda iki kez serbest, bu sürüm tutarlılığı ve kararlılığı üzerinde birden fazla bulutlarda odaklanır. Bu profil, en iyi Azure Stack uyumluluk hedefler.
    - **yyyy-aa-gg-profili** en iyi kararlılık ve en son özellikleri dengeler.

## <a name="azure-api-profiles-and-azure-stack-compatibility"></a>Azure API profillerini ve Azure Stack uyumluluğu

En yeni Azure API profillerini Azure Stack ile uyumlu değildir. Azure Stack çözümlerinizi kullanmak için hangi profil tanımlamak için aşağıdaki adlandırma kurallarını kullanabilirsiniz.

**en son**  
Bu profili hangi Azure Stack'te çalışmaz genel Azure'da bulunan en güncel API sürümü vardır. **En son** en çok sayıda önemli değişikliklere sahiptir. Profil, kararlılık ve diğer bulutlarda uyumluluğunu kenara koyar. En güncel API sürümlerini kullanan çalışıyorsanız **son** kullanmanız gerektiğini profilidir.

**Yyyy-aa-gg-karma**  
Bu profil, her yıl Mart ve Eylül aylarında serbest bırakılır. Bu, en iyi kararlılık ve çeşitli Bulutlar ile uyumluluk sahiptir. **Yyyy-aa-gg-karma** genel Azure ve Azure Stack hedeflemek için tasarlanmıştır. Bu profilde listelenen Azure API sürümleri, Azure Stack üzerinde listelenen olanları ile aynı olacaktır. Bu profili, karma bulut çözümleri için kodunu geliştirmek için kullanabilirsiniz.

**yyyy-aa-gg-profili**  
Bu profil için genel Azure Haziran ve aralık içinde yayımlanır. Azure Stack ile çalışmaz; Genellikle, birçok önemli değişiklikler olacaktır. En iyi kararlılık ve en son özellikleri arasındaki farkı dengeleyen rağmen **son** ve bu profili **son** API ne zaman bağımsız olarak en yeni API sürümlerinde, her zaman oluşur yayımladı. Yeni bir API sürümü için işlem API yarın oluşturulursa, örneğin, bu API sürümü listelenir **son**, fakat **yyyy-aa-gg-profili** bu profili zaten var olduğundan, profil. **yyyy-aa-gg-profili** Haziran önce veya aralık önce yayımlanan en güncel sürümleri kapsar.

## <a name="azure-resource-manager-api-profiles"></a>Azure Resource Manager API'si profilleri

Azure Stack genel Azure'da bulunan API sürümlerini en son sürümünü kullanır. Bir çözüm oluşturduğunuzda, Azure Stack ile uyumlu olan her bir Azure kaynak sağlayıcısı için API sürümü bulmanız gerekir.

Bunun yerine her kaynak sağlayıcısı ve Azure Stack tarafından desteklenen belirli sürüm araştırma daha API profili kullanabilirsiniz. Profil bir kaynak sağlayıcıları ve API sürümlerini belirtir. SDK'sı veya SDK'sıyla oluşturulan bir aracı hedef dönecek `api-version` profilinde belirtilen. API profillerine sahip tüm bir şablonla ve çalışma zamanında geçerli bir profil sürümü belirtebilirsiniz, Azure Resource Manager kaynağı doğru sürümünü seçer.

Azure Resource Manager, PowerShell, Azure CLI, SDK'sı ve Microsoft Visual Studio code gibi araçlar API profillerini çalışın. Araçlar ve SDK'lar profillerini modülleri ve bir uygulama oluştururken eklenecek kitaplıklar hangi sürümünü okumak için kullanabilirsiniz.

Örneğin, PowerShell kullanarak bir depolama hesabı oluşturmak için kullandığınız **Microsoft.Storage** destekleyen kaynak sağlayıcısı **api sürümü** 2016-03-30 ve kullanarak bir VM  **Microsoft.Compute** kaynak sağlayıcısı ile **api sürümü** 2015-12-01, depolama için PowerShell modülü destekleyen 2016-03-30 bakın ve hangi modülü için işlem, 2015-02-01 destekler ve ardından bunları yükleyin. Bunun yerine, bir profil kullanabilirsiniz. Cmdlet'i kullanmak `Install-Profile <profilename>`, PowerShell modülleri doğru sürümünü yükler.

Benzer şekilde, Python tabanlı bir uygulama oluşturmak için Python SDK'sı kullanırken, profil belirtebilirsiniz. SDK'sı, betikte belirttiğiniz kaynak sağlayıcıları için doğru modülleri yükler.

Bir geliştirici olarak, çözümünüzü yazmaya odaklanmasına olanak tanır. Hangi API sürümleri, kaynak sağlayıcısı ve bulut üzerinde araştırma yerine birlikte çalışarak, bir profil ve profilin destekleyen tüm Bulutlar arasında kodunuzun çalıştığını bilmeniz kullanabilirsiniz.

## <a name="api-profile-code-samples"></a>API profili kod örnekleri

Profilleri kullanılarak çözümünüzü Azure Stack ile tercih ettiğiniz dili ile tümleştirmenize yardımcı olmak için kod örnekleri bulabilirsiniz. Şu anda aşağıdaki diller için yönergeler ve örnekler bulabilirsiniz:

- **.NET** .NET API profili, bir kaynak sağlayıcısı paketindeki her kaynak türünün en son ve en çok kararlı sürümünü almak için kullanabilirsiniz. Daha fazla bilgi için [.NET ile Azure Stack ile kullanım API Sürüm profillerini](azure-stack-version-profiles-net.md).
- **PowerShell**  
Kullanabileceğiniz **AzureRM.Bootstrapper** modülü PowerShell cmdlet'leri ile API Sürüm profillerini çalışması için gereken almak için PowerShell Galerisi aracılığıyla kullanılabilir. Bilgi için [kullanımı API sürümü profillerini PowerShell](azure-stack-version-profiles-powershell.md).
- **Azure CLI**  
Azure Stack belirli API Sürüm profili kullanmak için ortamınızdaki yapılandırmayı güncelleştirebilirsiniz. Bilgi için [Azure CLI için kullanım API sürümü profillerini](azure-stack-version-profiles-azurecli2.md).
- **Go**  
Go SDK, bir profil farklı sürümlerini farklı hizmetlerden farklı kaynak türlerinin birleşimidir. Profil sürümü profilleri/yol altında kullanılabilir **YYYY-AA-GG** biçimi. Bilgi için [Go için kullanım API sürümü profillerini](azure-stack-version-profiles-go.md).
- **Ruby**  
Ruby SDK'sı için Azure Stack Kaynak Yöneticisi'ni oluşturmanıza ve altyapınızı yönetmenize yardımcı olacak araçlar sağlar. İşlem, sanal ağlar ve depolama ile Ruby dil SDK kaynak sağlayıcılarını içerir. Bilgi için [kullanımı API sürümü profillerini Ruby ile](azure-stack-version-profiles-ruby.md)
- **Python**  
Python SDK'sı, Azure yığını ve genel Azure gibi farklı bulut platformları hedeflemek için API sürümü profillerini destekler. Hibrit bulut çözümleri oluşturmak için API profillerini kullanabilirsiniz. Bilgi için [kullanımı API sürümü profillerini ile Python](azure-stack-version-profiles-python.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stack için PowerShell yükleme](../operator/azure-stack-powershell-install.md)
* [Azure Stack kullanıcının PowerShell ortamını yapılandırma](azure-stack-powershell-configure-user.md)
* [Kaynak sağlayıcısı API sürümleri profilleri tarafından desteklenen hakkındaki ayrıntıları](azure-stack-profiles-azure-resource-manager-versions.md).
