---
title: Azure Stack hizmet olarak doğrulamaya genel bakış | Microsoft Docs
description: Hizmet olarak Azure Stack doğrulamaya genel bakış.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: d1cfc5c9c378ccfc48811c5896337ef91fdca2e8
ms.sourcegitcommit: b95983e6e954e772ca5267304cfe6a0dab1cfcab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68418564"
---
# <a name="what-is-validation-as-a-service-for-azure-stack"></a>Azure Stack hizmet olarak doğrulama nedir?

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Hizmet olarak doğrulama (VaaS), Microsoft ile birlikte Mühendislik Azure Stack iş ortakları için tasarlanan yerel bir Azure hizmetidir. Çözüm ortakları, çözümlerinin Microsoft 'un gereksinimlerini karşılayıp karşılamadığını ve Azure Stack beklendiği gibi çalıştığını denetlemek için hizmeti kullanabilir.

VaaS 'in birincil kullanımları şunlardır:

- Yeni Azure Stack çözümlerini doğrulama
- Azure Stack yazılımda yapılan değişiklikler doğrulanıyor
- Dağıtım sırasında kullanılan çözüm iş ortağı paketlerini dijital olarak imzalama
- VaaS test yardımcı malzemeleri önizlemesi

## <a name="validate-a-new-azure-stack-solution"></a>Yeni bir Azure Stack çözümünü doğrulama

İş ortakları, yeni Azure Stack çözümlerini doğrulamak için **Çözüm doğrulama** iş akışını kullanır. Çözüm, gerekli donanım Laboratuvarı seti (HLK) Azure Stack bileşen testlerini iletmelidir. Bir dizi donanım yapılandırmasını onaylamak için, her yeni çözüm için iş akışının iki kez çalıştırılması gerekir: en düşük ve en yüksek yapılandırma.

Daha fazla bilgi için bkz. [yeni Azure Stack çözümünü doğrulama](azure-stack-vaas-validate-solution-new.md).

## <a name="validate-changes-to-the-azure-stack-software"></a>Azure Stack yazılımda yapılan değişiklikleri doğrulama

İş ortakları, çözümlerinin en güncel Azure Stack yazılım güncelleştirmeleriyle çalışıp çalışmadığını denetlemek için **paket doğrulama** iş akışını kullanır. Paket doğrulama iş akışının, güncelleştirmeyi uygulamak için düzeltme eki ve güncelleştirme 'nin (P & U) kullanıldığı Microsoft tarafından önerilen bir donanım ortamında çalıştırılması gerekir. Ayrıca, iş akışını temel derlemede çalıştırmanız önerilir.

Daha fazla bilgi için bkz. [Microsoft 'tan yazılım güncelleştirmelerini doğrulama](azure-stack-vaas-validate-microsoft-updates.md).

## <a name="get-digitally-signed-solution-partner-packages"></a>Dijital olarak imzalanan çözüm iş ortağı paketlerini al

İş ortakları, Azure Stack güncelleştirmelerini doğrulamaya ek olarak, OEM Özelleştirme paketlerine yönelik güncelleştirmeleri doğrulamak için **paket doğrulama** iş akışını kullanır ve bu da iş ortağı özel sürücüleri, bellenimi ve dağıtımı sırasında kullanılan diğer yazılımları Azure Stack içerir Azure Stack yazılımı. En azından desteklenen en düşük boyutlu çözümü kullanarak Azure Stack yazılımının güncel sürümünde doğrulamanızı yaptığınız paketi dağıtın. Paket, testleri yürütmeden önce VaaS 'e gönderilir. Testler başarılı olursa, paketin testi [vaashelp@microsoft.com](mailto:vaashelp@microsoft.com) tamamladığını ve Azure Stack dijital imzasıyla dijital olarak imzalanması gerektiğini bildirin. Microsoft, paketi imzalar ve paketin VaaS portalında indirileceği Azure Stack iş ortağına bildirimde bulunur.

Daha fazla bilgi için bkz. [OEM paketlerini doğrulama](azure-stack-vaas-validate-oem-package.md).

## <a name="preview-vaas-test-collateral"></a>Önizleme VaaS test yardımcı malzemeleri

Microsoft, Azure Stack yeni özellikleri düzenli olarak kullanıma sunar. Pazara bu özellikleri sunmaya yönelik geliştirme sürecinin bir parçası olarak, yeni test yardımcı malzemeleri **test geçiş** iş akışında kullanılabilir hale getirilir. Test geçiş iş akışı, resmi olmayan test yürütmeye izin vermek için diğer iş akışlarından test yardımcı malzemeleri içerir. Testleri onaya göndermek için test geçiş iş akışını kullanmayın. Çözümünüz için resmi onay almak üzere çözüm doğrulama ve paket doğrulama iş akışlarını kullanın.

Daha fazla bilgi için bkz [. hızlı başlangıç: İlk testinizi](azure-stack-vaas-schedule-test-pass.md)zamanlamak için bir hizmet portalı olarak doğrulamayı kullanın.

## <a name="validation-workflow-tests-summary"></a>Doğrulama iş akışı testleri Özeti

| Doğrulama iş akışı | Gerekli testler |
|----|------------|
| [Yeni çözüm doğrulaması](azure-stack-vaas-validate-solution-new.md) | Bulut benzetimi altyapısı<br>İşlem SDK 'Sı Işlem paketi<br>Disk kimliği sınaması<br>Keykasauzantısı SDK 'Sı çalışma paketi<br>Keykasasdk Işlem paketi<br>Ağ SDK Işlem paketi<br>Depolama hesabı SDK 'Sı Işlem paketi<br> |
| [OEM paketi doğrulaması](azure-stack-vaas-validate-oem-package.md) | OEM Uzantı paketi doğrulaması<br>Bulut benzetimi altyapısı |
| [Aylık güncelleştirme doğrulaması](azure-stack-vaas-validate-microsoft-updates.md) | Aylık Azure Stack güncelleştirme doğrulaması<br>Bulut benzetimi altyapısı<br> |

## <a name="next-steps"></a>Sonraki adımlar

- [Doğrulamanız hizmet kaynakları olarak ayarlama](azure-stack-vaas-set-up-resources.md)
- [Hizmet anahtar kavramları olarak doğrulama](azure-stack-vaas-key-concepts.md) hakkında bilgi edinin
