---
title: Azure Stack doğrulama olarak hizmet temel kavramları | Microsoft Docs
description: Hizmet olarak Azure Stack doğrulama temel kavramları açıklar.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 3830dc15cebe24a8fb910b641b7b986437aeec52
ms.sourcegitcommit: 0973dddb81db03cf07c8966ad66526d775ced8b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "64298508"
---
# <a name="validation-as-a-service-key-concepts"></a>Hizmet temel kavramları olarak doğrulama

Bu makalede hizmet (VaaS) olarak doğrulama temel kavramları açıklar.

## <a name="solutions"></a>Çözümler

VaaS çözüm belirli donanım ürün reçetesi (BoM) ile bir Azure Stack çözüm temsil eder. VaaS çözümü, Azure Stack çözüm karşı çalışan iş akışları için kapsayıcı görevi görür.

### <a name="create-a-solution-in-the-vaas-portal"></a>VaaS portalda bir çözüm oluşturma

1. Oturum [VaaS portalı](https://azurestackvalidation.com).
2. Çözüm panosunda seçin **yeni çözüm**.
3. Çözüm için bir ad girin. Öneriler adlandırmak için bkz: [VaaS çözümleri adlandırma](azure-stack-vaas-best-practice.md#naming-convention-for-vaas-solutions).
4. Seçin **Kaydet** çözümü oluşturmak için.

## <a name="workflows"></a>İş akışları

VaaS iş akışı VaaS çözüm bağlamında çalışır. Bu test paketleri, bir Azure Stack dağıtımı işlevselliğini çalışma kümesini temsil eder. Bir iş akışı, her bir Azure Stack Çözüm dağıtımı veya yazılım güncelleştirmesi oluşturulmalıdır.

İş akışları, senaryo türünü test ederek ayrılır. Terim ve kısaltmalarla sınamada **Test geçiş** iş akışı tüm kullanılabilir VaaS malzemeleri testleri seçmenize olanak sağlar. Resmi sınamada **doğrulama** iş akışlarını hedeflemek Microsoft tarafından seçilen belirli test senaryoları.

![VaaS iş akışı kutucukları](media/tile_all-workflows.png)

> [!NOTE]
> **Paket doğrulama** iş akışı şu anda iki senaryo da destekler: [OEM paketleri doğrulama](azure-stack-vaas-validate-oem-package.md) ve [yazılım güncelleştirmelerini Microsoft gelen doğrulama](azure-stack-vaas-validate-microsoft-updates.md).

İş akışı türleri hakkında daha fazla bilgi için bkz. [Azure Stack için hizmet olarak doğrulama nedir?](azure-stack-vaas-overview.md).

### <a name="getting-started-with-vaas-workflows"></a>VaaS iş akışları ile çalışmaya başlama

1. Çözümleri Panoda yeni bir çözüm oluşturun veya var olan bir'ı seçin. Bu yeniler ve iş akışı kutucukları etkinleştirir.
2. Yeni bir iş akışı oluşturmak için seçin **Başlat** herhangi bir kutucuğa üzerinde. Her iş akışına özel bilgiler için aşağıdaki makalelere bakın:
    - Test geçişi: [Hızlı Başlangıç: Doğrulama, ilk test zamanlamak için bir servis portalı kullanın.](azure-stack-vaas-schedule-test-pass.md)
    - Çözüm doğrulama: [Yeni Azure Stack çözümünü doğrulama](azure-stack-vaas-validate-solution-new.md)
    - Paket doğrulaması (aylık güncelleştirme): [Microsoft yazılım güncelleştirmeleri doğrulayın](azure-stack-vaas-validate-microsoft-updates.md)
    - Paket doğrulaması (paket imzalama): [OEM paketleri doğrula](azure-stack-vaas-validate-oem-package.md)

3. Yönettiğiniz veya izlediğiniz varolan bir iş akışı için seçin **Yönet** iş akışı kutucuğundaki. Kullanım ve iş akışı adını seçin **Düzenle** özelliklerini görüntüleyin ya da ortak test parametreleri değiştirmek için düğme.

İş akışı özellikleri ve parametreleri hakkında daha fazla bilgi için bkz. [iş akışı ortak parametreleri için bir hizmet olarak Azure Stack doğrulama](azure-stack-vaas-parameters.md).

## <a name="tests"></a>Testler

Azure Stack çözümünü karşı çalışan işlemlerin bir paketini VaaS testinde oluşur. Testiniz tanımlanan bir kategoriye göre farklı hedeflenen amaçları işlevsel ister veya güvenilirlik ve bir veya daha fazla Azure Stack hizmetlerini hedefleyin. Her test kendine ait bir dizi parametrenin bazıları içeren iş akışı ortak parametreleri tarafından belirtilir tanımlar.

Yönetme ve izleme sınamaları hakkında daha fazla bilgi için bkz. [İzleyici ve testleri VaaS portalında yönetme](azure-stack-vaas-monitor-test.md).

Test parametreleri hakkında daha fazla bilgi için bkz. [iş akışı ortak parametreleri için bir hizmet olarak Azure Stack doğrulama](azure-stack-vaas-parameters.md).

## <a name="agents"></a>Aracılar

Test yürütme VaaS aracı beraberinde getirir. İki tür aracılar VaaS Testleri Çalıştır:

- **Bulut Aracısı**. Varsayılan aracı VaaS içinde kullanılabilir budur. Kurulum gereklidir, ancak bu, ortamınıza bağlı olarak bağlantı gerektirir ve Azure Stack uç noktaları internet'ten çözülebilir olması gerekir. Bazı testler Bulut aracı ile uyumlu değildir.
- A **yerel aracı**. Bu, ortamınıza bağlı olarak bağlantı uygun olmadığı yerde senaryolarında doğrulama çalışmasını sağlar. Bazı Test yürütme yerel aracı üzerinden gerektirir.

Yerel aracılar herhangi belirli Azure Stack veya VaaS çözüme bağlı değil. En iyi uygulama, bir Azure Stack ortamın dışında çalıştırmanız gerekir.

Yerel aracı ekleme ile ilgili yönergeler için bkz: [yerel aracı dağıtma](azure-stack-vaas-local-agent.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Hizmet olarak doğrulama için en iyi uygulamalar](azure-stack-vaas-best-practice.md)