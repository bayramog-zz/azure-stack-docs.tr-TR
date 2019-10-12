---
title: Azure Stack ile akıllı kenar için DevOps deseninin | Microsoft Docs
description: Azure Stack ile akıllı kenar için DevOps kalıbı hakkında bilgi edinin
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
ms.date: 10/10/2019
ms.author: mabrigg
ms.reviewer: anajod
ms.lastreviewed: 06/11/2019
ms.openlocfilehash: 04eff0f095f14d88443fc4b221799e63f523c82c
ms.sourcegitcommit: a6d47164c13f651c54ea0986d825e637e1f77018
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72276998"
---
# <a name="devops-pattern"></a>DevOps kalıbı

Tek bir konumdan kodlayın ve yerel veri merkezinizde, özel bulutlarınızda veya genel bulutta olabilecek geliştirme, test ve üretim ortamlarında birden çok hedefe dağıtın.

## <a name="context-and-problem"></a>Bağlam ve sorun

Uygulama dağıtımı sürekliliği, güvenlik ve güvenilirlik, kuruluşlar ve geliştirme ekipleri için önemli öneme sahiptir.

Uygulamalar genellikle yeniden düzenlenmiş kodunun her bir hedef ortamda çalıştırılmasını gerektirir. Bu, bir uygulamanın tamamen taşınmadığı anlamına gelir. Her bir ortamda gezindiğinden, güncelleştirilmeleri, test edilmiş ve doğrulanması gerekir. Örneğin, bir geliştirme ortamında yazılan kodun daha sonra bir test ortamında çalışması için yeniden yazılması ve son olarak bir üretim ortamına eklerken yeniden yazılması gerekir. Ayrıca, bu kod özellikle konağa bağlanır. Bu, uygulamanızı korumanın maliyetini ve karmaşıklığını artırır. Uygulamanın her bir sürümü her bir ortama bağlıdır. Artan karmaşıklık ve yineleme, güvenlik ve kod kalitesi riskini artırır. Ayrıca, geri yükleme başarısız Konakları kaldırdığınızda veya daha sonra artan artışları işlemek için ek konaklar dağıtırken kod kolayca dağıtılamaz.

## <a name="solution"></a>Çözüm

DevOps stili, birden çok bulutta çalışan bir uygulama oluşturmanıza, test etmenize ve dağıtmanıza olanak sağlar. Bu model sürekli tümleştirme ve sürekli teslim işlemlerini tek bir şekilde depolar. Sürekli tümleştirme sayesinde, bir takım üyesi sürüm denetimine bir değişiklik yaptığı her seferinde kod oluşturulur ve test edilir. Sürekli teslim, bir derlemeden bir üretim ortamına her adımı otomatikleştirir. Birlikte, bu işlemler, farklı ortamlarda dağıtımı destekleyen bir yayın işlemi oluşturur. Bu Düzenle, kodunuzun taslağını oluşturabilir ve ardından aynı kodu bir şirket içi ortama, farklı özel bulutlara ve genel bulutlara dağıtabilirsiniz. Ortamdaki farklılıklar, koddaki değişiklikler yerine yapılandırma dosyasında değişiklik yapılmasını gerektirir.

![DevOps kalıbı](media/azure-stack-edge-pattern-hybrid-ci-cd/hybrid-ci-cd.png)

Şirket içi, özel bulut ve genel bulut ortamlarında tutarlı bir geliştirme araçları kümesiyle sürekli tümleştirme ve sürekli teslim için bir yöntem uygulayabilirsiniz. DevOps model kullanılarak dağıtılan uygulamalar ve hizmetler, şirket içi ve genel bulut özelliklerinden ve özelliklerinden faydalanarak bu konumların herhangi birinde değiştirilebilir ve çalıştırılabilir.

DevOps yayın işlem hattının kullanılması şunları yapmanıza yardımcı olur:

-   Tek bir depoya kod işleme tabanlı yeni bir derleme başlatın.

-   Yeni oluşturulan kodunuzu kullanıcı kabul testi için genel buluta otomatik olarak dağıtın.

-   Kodunuz testi geçtikten sonra özel buluta otomatik olarak dağıtın.

## <a name="issues-and-considerations"></a>Sorunlar ve konular

DevOps deseninin, hedef ortamdan bağımsız olarak dağıtımlar arasında tutarlılık sağlamaya yöneliktir. Ancak, yetenekler bulutta ve şirket içi ortamlarda farklılık gösterir. Aşağıdakileri göz önünde bulundurun:

-   Dağıtımınızdaki işlevler, uç noktalar, hizmetler ve diğer kaynaklar hedef dağıtım konumlarında kullanılabilir mi?

-   Yapılandırma yapıtları bulutlar genelinde erişilebilen konumlarda depolanıyor mu?

-   Dağıtım parametreleri tüm hedef ortamlarda çalışacak mı?

-   Kaynağa özgü özellikler tüm hedef bulutlarda kullanılabilir mi?

Daha fazla bilgi için bkz. [bulut tutarlılığı için Azure Resource Manager şablonları geliştirme](https://docs.microsoft.com/azure/azure-resource-manager/templates-cloud-consistency).

Ayrıca, bu düzenin nasıl uygulanacağını saptarken aşağıdaki noktaları göz önünde bulundurun:

### <a name="scalability-considerations"></a>Ölçeklenebilirlik konuları

Dağıtım Otomasyonu sistemleri, DevOps desenlerindeki anahtar denetim noktasıdır. Uygulamalar farklılık gösterebilir. Doğru sunucu boyutunun seçimi, beklenen iş yükünün boyutuna bağlıdır. VM 'Ler kapsayıcılardan ölçeğe daha fazla maliyet sağlar. Ancak, ölçeklendirme için kapsayıcıları kullanmak üzere derleme işleminizin kapsayıcılarla çalışması gerekir.

### <a name="availability-considerations"></a>Kullanılabilirlik konuları

Devmodel bağlamında kullanılabilirlik, test sonuçları, kod bağımlılıkları veya diğer yapıtlar gibi iş akışınız ile ilişkili tüm durum bilgilerini kurtarabilmekte olduğu anlamına gelir. Kullanılabilirlik gereksinimlerinizi değerlendirmek için, iki ortak ölçümü göz önünde bulundurun:

-   Kurtarma süresi hedefi (RTO), sistem olmadan ne kadar gidebilmeniz gerektiğini belirtir.

-   Kurtarma noktası hedefi (RPO), hizmette bir kesinti sistemi etkiliyorsa ne kadar veri kaybedeceğinizi gösterir.

Uygulamada, RTO ve RPO, artıklık ve yedekleme anlamına gelmez. Küresel Azure bulutu 'nda kullanılabilirlik, Azure 'un bir parçası olan, ancak DevOps sistemlerinizin durumunu sürdürmenize gerek kalmadan, donanım kurtarmaya yönelik bir soru değildir. Azure Stack, donanım kurtarma göz önünde olabilir.

Dağıtım Otomasyonu için kullanılan sistemin tasarlanmasıyla ilgili bir diğer önemli nokta, erişim denetimi ve hizmetleri bulut ortamlarına dağıtmak için gereken hakların uygun yönetimi. Dağıtımları oluşturmak, silmek veya değiştirmek için hangi haklar gerekir? Örneğin, genellikle Azure 'da bir kaynak grubu oluşturmak için bir haklar kümesi gerekir ve kaynak grubunda hizmetleri dağıtmak için başka bir haklar vardır.

### <a name="manageability-considerations"></a>Yönetilebilirlik konuları

DevOps düzenine dayalı herhangi bir sistemin tasarımı, portföyde her bir hizmet için Otomasyon, günlüğe kaydetme ve uyarı vermeyi göz önünde bulundurmalıdır. Paylaşılan Hizmetleri, bir uygulama ekibini veya her ikisini birden kullanın, güvenlik ilkelerini ve idare 'yi de izleyin.

Üretim ortamlarını ve geliştirme/test ortamlarını Azure veya Azure Stack ayrı kaynak gruplarında dağıtın. Ardından, her bir ortamın kaynaklarını izleyebilir ve kaynak grubuna göre faturalandırma maliyetlerini oluşturabilirsiniz. Kaynakları, test dağıtımları için yararlı olan bir küme olarak da silebilirsiniz.

## <a name="when-to-use-this-pattern"></a>Bu düzenin ne zaman kullanılacağı

Bu kalıbı kullanın:

-   Geliştiricilerinizin ihtiyaçlarını karşılayan bir ortamda kod geliştirebilir ve yeni kod geliştirmek zor olabilecek çözümünüze özel bir ortama dağıtım yapabilirsiniz.

-   DevOps deseninin sürekli tümleştirme ve sürekli teslim sürecini izleyebilecekleri sürece, geliştiricilerinizin istediğiniz kod ve araçları kullanabilirsiniz.

Bu kalıp önerilmez:

-   Altyapı, sağlama kaynakları, yapılandırma, kimlik ve güvenlik görevlerini otomatikleştiremezsiniz.

-   Ekiplerin sürekli tümleştirme/sürekli geliştirme (CI/CD) yaklaşımı uygulamak için karma bulut kaynaklarına erişimleri yoksa.

## <a name="example"></a>Örnek

Bir uygulamayı Azure 'a dağıtmayı ve karma bir sürekli tümleştirme/sürekli teslim (CI/CD) işlem hattı kullanarak Azure Stack öğrenin.

[Uygulamaları Azure 'a dağıtma ve Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-pipeline)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack Için karma bulut tasarım desenleri](azure-stack-edge-pattern-overview.md) hakkında bilgi edinin
