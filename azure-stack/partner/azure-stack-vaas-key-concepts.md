---
title: Hizmet anahtarı kavramlarını doğrulama Azure Stack | Microsoft Docs
description: Hizmet olarak Azure Stack doğrulamada temel kavramları açıklar.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/28/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 10/28/2019
ROBOTS: NOINDEX
ms.openlocfilehash: a79b6e6f34c8293febf30a9cb48ddbfae9c99816
ms.sourcegitcommit: cc3534e09ad916bb693215d21ac13aed1d8a0dde
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73167316"
---
# <a name="validation-as-a-service-key-concepts"></a>Hizmet anahtar kavramları olarak doğrulama

Bu makalede hizmet olarak doğrulama (VaaS) içindeki temel kavramlar açıklanmaktadır.

## <a name="solutions"></a>Çözümler

Bir VaaS çözümü, belirli bir donanım ürün reçetesinden (BoM) Azure Stack bir çözümü temsil eder. VaaS çözümü, Azure Stack çözümüne karşı çalışan iş akışları için bir kapsayıcı görevi görür.

### <a name="create-a-solution-in-the-vaas-portal"></a>VaaS portalında çözüm oluşturma

1. [Vaas portalında](https://azurestackvalidation.com)oturum açın.
2. Çözümler panosunda **yeni çözüm**' ı seçin.
3. Çözüm için bir ad girin. Adlandırma önerileri için bkz. [VaaS çözümleri Için adlandırma kuralı](azure-stack-vaas-best-practice.md#naming-convention-for-vaas-solutions).
4. Çözümü oluşturmak için **Kaydet** ' i seçin.

## <a name="workflows"></a>İş akışları

Bir VaaS iş akışı, VaaS çözümünün bağlamı içinde çalışır. Azure Stack dağıtımının işlevlerini gerçekleştiren bir test paketi kümesini temsil eder. Bir Azure Stack çözümünün her dağıtım veya yazılım güncelleştirmesi için bir iş akışı oluşturulmalıdır.

İş akışları, test senaryosu türüne göre kategorilere ayrılır. Resmi olmayan sınamalarda, **test geçişi** iş akışı, tüm kullanılabilir Vaas yardımcı malzemeleri arasından testler seçmenizi sağlar. Resmi test halinde, **doğrulama** , Microsoft tarafından seçilen belirli test senaryolarını hedefleyin.

![VaaS iş akışı kutucukları](media/tile_all-workflows.png)

> [!NOTE]
> **Paket doğrulama** iş akışı şu anda iki senaryoyu DESTEKLEMEKTEDIR: [OEM paketlerini doğrulama](azure-stack-vaas-validate-oem-package.md) ve [Microsoft 'tan yazılım güncelleştirmelerini doğrulama](azure-stack-vaas-validate-microsoft-updates.md).

İş akışı türleri hakkında daha fazla bilgi için bkz. [Azure Stack Için hizmet olarak doğrulama nedir?](azure-stack-vaas-overview.md).

### <a name="getting-started-with-vaas-workflows"></a>VaaS iş akışlarıyla çalışmaya başlama

1. Çözümler panosunda yeni bir çözüm oluşturun veya var olan bir çözümü seçin. Bu, iş akışı kutucuklarını yeniler ve etkinleştirir.
2. Yeni bir iş akışı oluşturmak için herhangi bir kutucukta **Başlat** ' ı seçin. Her iş akışına özgü bilgiler için aşağıdaki makalelere bakın:
    - Test geçişi: [hızlı başlangıç: ilk testinizi zamanlamak için bir hizmet portalı olarak doğrulamayı kullanın](azure-stack-vaas-schedule-test-pass.md)
    - Çözüm doğrulaması: [Yeni bir Azure Stack çözümü doğrulama](azure-stack-vaas-validate-solution-new.md)
    - Paket doğrulaması (aylık güncelleştirme): [Microsoft 'tan yazılım güncelleştirmelerini doğrulama](azure-stack-vaas-validate-microsoft-updates.md)
    - Paket doğrulaması (paket imzalama): [OEM paketlerini doğrula](azure-stack-vaas-validate-oem-package.md)

3. Mevcut bir iş akışını yönetmek veya izlemek için iş akışı kutucuğunda **Yönet** ' i seçin. İş akışının adını seçin ve özellikleri görüntülemek veya ortak test parametrelerini değiştirmek için **Düzenle** düğmesini kullanın.

İş akışı özellikleri ve parametreleri hakkında daha fazla bilgi için bkz. [hizmet olarak Azure Stack doğrulaması Için Iş akışı ortak parametreleri](azure-stack-vaas-parameters.md).

## <a name="tests"></a>Testler

VaaS 'deki bir test, Azure Stack çözümüne karşı çalışan bir işlem paketinden oluşur. Testlerin, işlevsel veya güvenilirlik gibi bir kategori tarafından tanımlanan farklı amaçlanan amaçları vardır ve Azure Stack bir veya daha fazla hizmeti hedefleyin. Her test, bir kısmı kapsayan iş akışının ortak parametreleri tarafından belirtilen kendi parametre kümesini tanımlar.

Testleri yönetme ve izleme hakkında daha fazla bilgi için bkz. [VaaS portalındaki testleri izleme ve yönetme](azure-stack-vaas-monitor-test.md).

Test parametreleri hakkında daha fazla bilgi için bkz. [hizmet olarak Azure Stack doğrulaması Için Iş akışı ortak parametreleri](azure-stack-vaas-parameters.md).

## <a name="agents"></a>Aracılar

Bir VaaS Aracısı sürücüleri test yürütmesi. İki aracı türü, VaaS testlerini çalıştırır:

- **Bulut Aracısı**. Bu, VaaS 'de kullanılabilen varsayılan aracıdır. Kurulum gerekmez, ancak bu ortamınıza bağlı bağlantı gerektirir ve Azure Stack uç noktaları internet 'ten çözümlenebilmelidir. Bazı testler bulut aracısıyla uyumlu değildir.
- **Yerel bir aracı**. Bu, ortamınıza bağlı bağlantıların uygun olmadığı senaryolarda doğrulamayı çalıştırmanıza olanak sağlar. Bazı testlerin yerel aracı aracılığıyla yürütülmesi gerekir.

Yerel aracılar herhangi bir belirli Azure Stack veya VaaS çözümüne bağlı değildir. En iyi uygulama olarak, bir Azure Stack ortamının dışında çalışmalıdır.

Yerel Aracı ekleme hakkında yönergeler için bkz. [Yerel aracıyı dağıtma](azure-stack-vaas-local-agent.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Hizmet olarak doğrulama için en iyi uygulamalar](azure-stack-vaas-best-practice.md)