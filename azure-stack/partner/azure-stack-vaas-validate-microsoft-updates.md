---
title: Hizmet olarak Azure Stack doğrulamasında Microsoft 'tan yazılım güncelleştirmelerini doğrulama | Microsoft Docs
description: Hizmet olarak doğrulama ile Microsoft 'tan yazılım güncelleştirmelerini doğrulamayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/23/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 0a507b7488a34715e528b6bbf291fec9832ef027
ms.sourcegitcommit: b95983e6e954e772ca5267304cfe6a0dab1cfcab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68418289"
---
# <a name="validate-software-updates-from-microsoft"></a>Microsoft 'tan yazılım güncelleştirmelerini doğrulama

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Microsoft, Azure Stack yazılım güncelleştirmelerini düzenli olarak yayımlayacaktır. Bu güncelleştirmeler, Azure Stack comühendislik iş ortakları için verilmiştir. Güncelleştirmeler genel kullanıma açık bir şekilde sağlanır. Çözümünüze karşı güncelleştirmeleri denetleyebilir ve Microsoft 'a geri bildirim sağlayabilirsiniz.

Azure Stack için Microsoft yazılım güncelleştirmeleri, bir adlandırma kuralı kullanılarak atanır, örneğin, güncelleştirmenin 2018 Mart için olduğunu belirten 1803. Azure Stack güncelleştirme ilkesi, temposunda ve sürüm notları hakkında bilgi için bkz. [Azure Stack bakım ilkesi](../operator/azure-stack-servicing-policy.md).

## <a name="prerequisites"></a>Önkoşullar

VaaS 'de aylık güncelleştirme işlemini gerçekleştirmeden önce, aşağıdaki öğeler hakkında bilgi sahibi olmanız gerekir:

- [Hizmet anahtar kavramları olarak doğrulama](azure-stack-vaas-key-concepts.md)
- [Etkileşimli Özellik doğrulama testi](azure-stack-vaas-interactive-feature-verification.md)

## <a name="required-tests"></a>Gerekli testler

Aşağıdaki testlerin aylık yazılım doğrulaması için aşağıdaki sırayla yürütülmesi gerekir:

1. Aylık Azure Stack güncelleştirme doğrulaması
2. Bulut benzetimi altyapısı

## <a name="validating-software-updates"></a>Yazılım güncelleştirmeleri doğrulanıyor

1. Yeni bir **paket doğrulama** iş akışı oluşturun.
1. Yukarıdaki gerekli testler için [paket doğrulama testlerini Çalıştır](azure-stack-vaas-validate-oem-package.md#run-package-validation-tests)' dan yönergeleri izleyin. **Aylık Azure Stack güncelleştirme doğrulama** testi hakkında ek yönergeler için aşağıdaki bölüme bakın.

### <a name="apply-the-monthly-update"></a>Aylık güncelleştirmeyi uygulama

1. Testleri yürütmek için bir aracı seçin.
1. **Aylık Azure Stack güncelleştirme doğrulaması**zamanlayın.
1. Şu anda damgada dağıtılan OEM uzantı paketinin konumunu ve güncelleştirme sırasında uygulanacak OEM uzantı paketinin konumunu belirtin. Bu paketlerin URL 'Lerini yapılandırmak için bkz. [paketleri doğrulama için yönetme](azure-stack-vaas-validate-oem-package.md#managing-packages-for-validation).
1. Seçilen aracıdaki Kullanıcı arabirimindeki adımları izleyin.

Sorularınız veya endişeleriniz varsa, [Vaas yardımı 'na](mailto:vaashelp@microsoft.com)başvurun.

## <a name="next-steps"></a>Sonraki adımlar

- [VaaS portalındaki testleri izleme ve yönetme](azure-stack-vaas-monitor-test.md)