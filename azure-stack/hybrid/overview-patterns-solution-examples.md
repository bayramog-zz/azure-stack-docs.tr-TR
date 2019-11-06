---
title: Azure ve Azure Stack için karma desenler ve çözüm örnekleri
description: Karma desenleri ve çözüm örneklerine genel bakış, Azure ve Azure Stack karma çözümleri öğrenme ve oluşturma için faydalıdır.
author: BryanLa
ms.service: azure-stack
ms.topic: overview
ms.date: 11/05/2019
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 11/05/2019
ms.openlocfilehash: 134171c85f91801f83965d376f79d09f6e8e3ac8
ms.sourcegitcommit: 5c92a669007ab4aaffe4484f1d8836a40340dde1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73638733"
---
# <a name="hybrid-patterns-and-solution-examples-for-azure-and-azure-stack"></a>Azure ve Azure Stack için karma desenler ve çözüm örnekleri

Microsoft, Azure ve Azure Stack ürünlerini ve çözümlerini tutarlı bir Azure ekosistemi olarak sunmaktadır. Microsoft Azure Stack ailesi bir Azure uzantısıdır. 

## <a name="the-hybrid-cloud-and-hybrid-apps"></a>Karma bulut ve karma uygulamalar

Azure Stack, *karma bir bulut*etkinleştirerek şirket içi ortamınıza ve kenara bulut bilgi işlem çevikliğini getirir. Azure Stack hub, Azure Stack HI ve Azure Stack Edge, Azure 'dan bağımsız veri merkezlerinize, şubelerine, alana ve daha ilerisine kadar Azure 'u genişletin. Bu farklı özellik kümesiyle şunları yapabilirsiniz:

- Azure 'da ve şirket içi ortamlarınızda kodu yeniden kullanın ve bulutta yerel uygulamaları tutarlı bir şekilde çalıştırın.
- Geleneksel sanallaştırılmış iş yüklerini Azure hizmetleri için isteğe bağlı bağlantılarla çalıştırın.
- Verileri buluta aktarın veya uyumluluk sağlamak için bağımsız veri merkezinizde saklayın.
- Donanım hızlandırılmış makine öğrenimi, Kapsayıcılı veya sanallaştırılmış iş yüklerini, hepsi de akıllı kenarda çalıştırın.

Bulutları kapsayan uygulamalar da *karma uygulamalar*olarak adlandırılır. Azure 'da karma bulut uygulamaları oluşturabilir ve bunları herhangi bir yerde bulunan bağlı veya bağlantısı kesilmiş veri merkezinize dağıtabilirsiniz.

Karma uygulama senaryoları, geliştirme için kullanılabilen kaynaklarla büyük ölçüde farklılık gösterir. Ayrıca coğrafya, güvenlik, Internet erişimi ve diğerleri gibi konuları da kapsar. Burada açıklanan desenler ve çözümler tüm gereksinimleri ele geçirebilse de, Karma çözümler uygulanırken keşfetmeye ve yeniden kullanmaya yönelik yönergeler ve örnekler sağlarlar.

## <a name="design-patterns"></a>Tasarım desenleri

Gerçek dünya müşteri senaryolarından ve deneyimlerinden, uygun şekilde Genelleştirilmiş tasarım kılavuzu tasardüzenleri tasarlayın. Bir kalıp, farklı türlerde senaryolar veya dikey sektörler için geçerli olmasını sağlayan soyuttur. Her kalıp, bağlamı ve sorunu belgelemektedir ve bir çözüm örneğine genel bakış sağlar. Çözüm örneği, düzenin olası bir uygulama olarak tasarlanmıştır.

İki tür desenler makalesi vardır:

- Tek bir model: tek bir genel amaçlı senaryo için tasarım kılavuzu sağlar.
- Çoklu Düzen: birden çok desen uygulamasının kullanıldığı tasarım kılavuzu sağlar. Bu, daha karmaşık senaryoları veya sektöre özel sorunları çözmek için sık gereklidir.

## <a name="solution-deployment-guides"></a>Çözüm dağıtım kılavuzu

Adım adım dağıtım kılavuzu, bir çözüm örneği dağıtmaya yardımcı olur. Kılavuz, GitHub [çözümleri örnek](https://github.com/Azure-Samples/azure-intelligent-edge-patterns)deposunda depolanan bir yardımcı kod örneğine de başvurabilir. 

## <a name="next-steps"></a>Sonraki adımlar

- Ürünlerin ve çözümlerin tamamı hakkında daha fazla bilgi edinmek için [ürün ve çözümlerin Azure Stack ailesine](/azure-stack)bakın.
- Her biri hakkında daha fazla bilgi edinmek için, TOC 'nin "desenler" ve "Çözüm dağıtım kılavuzu" bölümlerine göz atın.
- Karma uygulamalar tasarlamak, dağıtmak ve çalıştırmak için yazılım kalitesinin ayrıntılarını gözden geçirmek üzere [karma uygulama tasarımı konuları](overview-app-design-considerations.md) hakkında bilgi edinin.
- [Azure Stack üzerinde bir geliştirme ortamı kurun](../user/azure-stack-dev-start.md) ve [ilk uygulamanızı Azure Stack dağıtın](../user/azure-stack-dev-start-deploy-app.md) .
