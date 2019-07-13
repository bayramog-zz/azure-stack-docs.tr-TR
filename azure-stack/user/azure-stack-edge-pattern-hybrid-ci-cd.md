---
title: Azure Stack ile akıllı uç DevOps desenini | Microsoft Docs
description: Azure Stack ile akıllı uç DevOps desenini hakkında bilgi edinin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/11/2019
ms.author: mabrigg
ms.reviewer: anajod
ms.lastreviewed: 06/11/2019
ms.openlocfilehash: 76437cd37733984d3230d4c40ccc82c7e6ede2b9
ms.sourcegitcommit: 51ec68b5e6dbf437aaca19a9f35ba07d2c402892
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67856392"
---
# <a name="devops-pattern"></a>DevOps düzeni

Tek bir konumdan kod ve geliştirme, test ve üretim ortamlarında yerel veri merkezinizde, özel bulutlara veya genel bulut olabilir. birden çok hedefe dağıtın.

## <a name="context-and-problem"></a>Bağlam ve sorun

Uygulama dağıtım sürekliliği, güvenlik ve güvenilirlik, kuruluşların önemli ve geliştirme takımları için kritik.

Uygulamalar genellikle her bir hedef ortamda çalıştırmak için UIMap'e yeniden işlenmiş kod gerektirir. Başka bir deyişle, bir uygulama tamamen taşınabilir değildir. Güncelleştirilmiş, test ve gerekir her ortamı üzerinden hareket ettikçe doğrulandı. Örneğin, bir geliştirme ortamında yazılan kod gerekir ardından bir test ortamında çalışması için yazılan ve son olarak bir üretim ortamında gölünüzdeki olduğunda yazılan. Bu kod Ayrıca, özellikle ana bilgisayara bağlanır. Bu maliyet uygulamanızı kullanmanın ve karmaşıklığını artırır. Her sürüm uygulamanın her ortama bağlı olur. Artan karmaşıklık ve çoğaltma güvenlik ve kod kalitesini riskini artırır. Ayrıca, geri yükleme başarısız oldu konakları kaldırın ya da isteğe bağlı artışlar işlemek için ek Konaklara dağıtma kod kolayca dağıtılamaz.

## <a name="solution"></a>Çözüm

DevOps desen, derleme, test etme ve birden çok bulut üzerinde çalışan bir uygulama dağıtmak sağlar. Bu düzen, sürekli tümleştirme ve sürekli teslim uygulaması sahip. Sürekli Tümleştirme ile kod oluşturulan ve bir takım üyesi bir değişikliği sürüm denetimine işlenen her değişiklikten test. Sürekli teslim, her bir derleme adımından üretim ortamına otomatikleştirir. Birlikte, bu işlemlerin farklı ortamlarda dağıtımı destekleyen bir sürüm işlemi oluşturun. Bu desen ile kodunuzu taslak ve bir şirket içi ortamı, farklı özel Bulutlar ve genel Bulutlar için aynı kodu dağıtabilirsiniz. Ortamındaki farklılıklar bir yapılandırma dosyası değişiklik yerine kodunda değişiklik yapılmasını gerektirir.

![DevOps düzeni](media/azure-stack-edge-pattern-hybrid-ci-cd/hybrid-ci-cd.png)

Geliştirme araçları şirket içi, özel Bulut ve genel bulut ortamları arasında tutarlı bir dizi ile sürekli tümleştirme ve sürekli teslim, bir yöntem uygulayabilirsiniz. Uygulamalar ve hizmetler DevOps deseni kullanılarak dağıtılan birbirinin yerine kullanılabilir ve şirket içinde ve genel bulut özellikleri ve yetenekleri yararlanarak, bu konumlardan herhangi birinde çalıştırılabilir.

Bir DevOps kullanarak yayın ardışık düzeni, yardımcı olur:

-   Tek bir depoda kod tamamlama dayalı yeni bir derleme başlatır.

-   Otomatik olarak yeni oluşturulan kodunuzu kullanıcı kabul testi için genel buluta dağıtın.

-   Kodunuzu test geçtikten sonra otomatik olarak bir özel buluta dağıtın.

## <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler

DevOps düzeni, hedef ortam bağımsız olarak dağıtımlar arasında tutarlılık sağlamak için tasarlanmıştır. Ancak, Bulut ve şirket içi ortamlar genelinde özellikleri değişir. Aşağıdaki topluluklara bir göz atın:

-   İşlevler, uç noktaları, hizmetleri ve diğer kaynaklar, dağıtımınızdaki hedef dağıtım konumlarda kullanılabilir?

-   Yapılandırma yapıtları bulutlarda erişilebilir bir konumda depolanır?

-   Dağıtım parametreleri, tüm hedef ortamlarında çalışacak mı?

-   Kaynağa özgü özellikleri tüm hedef bulutlarda kullanılabilir mi?

Daha fazla bilgi için [bulut tutarlılık için geliştirme Azure Resource Manager şablonları](https://docs.microsoft.com/azure/azure-resource-manager/templates-cloud-consistency).

Ayrıca, bu düzeni nasıl uygulayacağınıza karar verirken aşağıdaki noktaları dikkate alın:

### <a name="scalability-considerations"></a>Ölçeklenebilirlik konusunda dikkat edilmesi gerekenler

DevOps desenleri anahtar denetim noktası dağıtım Otomasyon sistemleridir. Uygulamalar farklılık gösterebilir. Doğru sunucu boyutunun seçimi, beklenen iş yükünün boyutuna bağlıdır. Vm'leri ölçek kapsayıcılara göre daha pahalı olabilir. Ancak, ölçeklendirme amacıyla kapsayıcıları kullanabilmek için derleme sürecinizin kapsayıcılar ile çalışması gerekir.

### <a name="availability-considerations"></a>Kullanılabilirlik konusunda dikkat edilmesi gerekenler

DevPattern bağlamında kullanılabilirlik, test sonuçları, kod bağımlılıkları veya diğer yapıtlar gibi iş akışınızla ilişkili herhangi bir durum bilgisini geri yükleme anlamına gelir. Kullanılabilirlik gereksinimlerinizi değerlendirirken yaygın kullanılan iki ölçümü göz önünde bulundurun:

-   Kurtarma süresi hedefi (RTO) sistemi olmadan gidebilirsiniz süreyi belirtir.

-   Kurtarma noktası hedefi (RPO), sistem veri miktarını, bir kesintisinden etkilenirse hizmet kaybetmeyi göze alabileceğinizi belirtir.

Uygulama, RTO ve RPO yedeklilik ve yedeklemeye işaret eder. Küresel Azure Bulutu üzerinde kullanılabilirlik donanım Kurtarma bir soru değildir — Azure parçası olan — ancak DevOps sistemlerinizi durumunu korumak yerine sağlama. Azure Stack üzerinde önemli bir unsur donanım kurtarma olabilir.

Dağıtım Otomasyon için kullanılan sistemi tasarladığınız sırada başka bir önemli erişim denetimi ve bulut ortamlarına Hizmetleri dağıtmak için gereken hakları doğru yönetimini noktadır. Oluşturma, silme veya dağıtımları değiştirmek için hangi hakları gerekir? Örneğin, bir hak kümesi, genellikle Azure ve başka bir kaynak grubundaki Hizmetleri dağıtmak için bir kaynak grubu oluşturmak için gereklidir.

### <a name="manageability-considerations"></a>Yönetilebilirlik konusunda dikkat edilmesi gerekenler

DevOps deseni temel alınarak tüm sistemin tasarımını, otomasyon, günlüğe kaydetme ve her hizmet için Portföyünden uyarı dikkate almanız gerekir. Paylaşılan hizmetler, bir uygulama ekipleri veya her ikisini de ve güvenlik ilkeleri ve idare de izleyin.

Üretim ortamlarını ve geliştirme/test ortamlarını ayrı kaynak gruplarında Azure'da veya Azure Stack'te dağıtın. Ardından her bir ortamın kaynaklarını izleyebilir ve fatura maliyetlerini kaynak grubuna göre topla. Ayrıca, test dağıtımları için yararlı olan bir küme olarak kaynakları da silebilirsiniz.

## <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar

Bu düzeni kullanarak:

-   Kod Geliştiricileriniz ihtiyaçlarını karşılayan bir ortamda geliştirme ve burada yeni kod geliştirmek zor olabilir çözümünüzü belirli bir ortama dağıtın.

-   Kodu ve sürekli tümleştirme ve sürekli teslim işlem DevOps deseninde uygulayabilmek için oldukları sürece Geliştiricileriniz bize istediğiniz araçları kullanabilirsiniz.

Bu düzenin şunlar için kullanılması önerilmez:

-   Kaynakları, yapılandırma, kimlik ve güvenlik görevlerini sağlama altyapı, otomatik hale getiremezsiniz durumunda.

-   Takımlar bir sürekli tümleştirme/sürekli geliştirme (CI/CD) yaklaşımı uygulamak için hibrit bulut kaynaklarına erişimi yoksa.

## <a name="example"></a>Örnek

Bir uygulamayı Azure ve Azure Stack kullanarak karma sürekli tümleştirme/sürekli teslim (CI/CD) işlem hattı dağıtmayı öğrenin.

[Azure ve Azure uygulama dağıtma yığını](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-pipeline)

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [Azure Stack için hibrit bulut tasarımı desenleri](azure-stack-edge-pattern-overview.md)
