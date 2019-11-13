---
title: Azure Stack hub 'ı için hizmet olarak doğrulamaya genel bakış | Microsoft Docs
description: Hizmet olarak Azure Stack hub doğrulamasına genel bakış.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/11/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 11/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: dee0be0817c8b614c9961d39330b64f4bd11f85f
ms.sourcegitcommit: 102ef41963b5d2d91336c84f2d6af3fdf2ce11c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73955857"
---
# <a name="what-is-validation-as-a-service-for-azure-stack-hub"></a>Azure Stack hub 'ı için bir hizmet olarak doğrulama nedir?

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Hizmet olarak doğrulama (VaaS), Microsoft ile birlikte Mühendislik Azure Stack hub teklifleri olan çözüm iş ortakları için tasarlanan yerel bir Azure hizmetidir. Çözüm ortakları, çözümlerinin Microsoft 'un gereksinimlerini karşılayıp karşılamadığını ve Azure Stack hub ile beklendiği gibi çalıştığını denetlemek için hizmeti kullanabilir.

VaaS 'in birincil kullanımları şunlardır:

- Yeni Azure Stack hub çözümlerini doğrulama
- Azure Stack hub yazılımında yapılan değişiklikler doğrulanıyor
- Dağıtım sırasında kullanılan çözüm iş ortağı paketlerini dijital olarak imzalama
- VaaS test yardımcı malzemeleri önizlemesi

## <a name="validate-a-new-azure-stack-hub-solution"></a>Yeni bir Azure Stack hub çözümünü doğrulama

İş ortakları, yeni Azure Stack hub çözümlerini doğrulamak için **Çözüm doğrulama** iş akışını kullanır. Çözüm, gerekli donanım Laboratuvarı seti (HLK) Azure Stack hub bileşen testlerini iletmelidir. Bir dizi donanım yapılandırmasını onaylamak için, her yeni çözüm için iş akışının iki kez çalıştırılması gerekir: en düşük ve en yüksek yapılandırma.

Daha fazla bilgi için bkz. [yeni Azure Stack hub çözümünü doğrulama](azure-stack-vaas-validate-solution-new.md).

## <a name="validate-changes-to-the-azure-stack-hub-software"></a>Azure Stack hub yazılımında yapılan değişiklikleri doğrulama

İş ortakları, çözümlerinin en güncel Azure Stack hub yazılım güncelleştirmeleriyle çalışıp çalışmadığını denetlemek için **paket doğrulama** iş akışını kullanır. Paket doğrulama iş akışının, güncelleştirmeyi uygulamak için düzeltme eki ve güncelleştirme 'nin (P & U) kullanıldığı Microsoft tarafından önerilen bir donanım ortamında çalıştırılması gerekir. Ayrıca, iş akışını temel derlemede çalıştırmanız önerilir.

Daha fazla bilgi için bkz. [Microsoft 'tan yazılım güncelleştirmelerini doğrulama](azure-stack-vaas-validate-microsoft-updates.md).

## <a name="get-digitally-signed-solution-partner-packages"></a>Dijital olarak imzalanan çözüm iş ortağı paketlerini al

İş ortakları Azure Stack hub güncelleştirmelerini doğrulamaya ek olarak, OEM Özelleştirme paketlerine yönelik güncelleştirmeleri doğrulamak için **paket doğrulama** iş akışını kullanır ve bu, Azure Stack hub 'ının dağıtımı sırasında kullanılan Azure Stack hub ortağına özgü sürücüleri, bellenimi ve diğer yazılımları içerir. En azından desteklenen en düşük boyutlu çözümü kullanarak Azure Stack hub yazılımının güncel sürümünde doğrulamanızı yaptığınız paketi dağıtın. Paket, testleri yürütmeden önce VaaS 'e gönderilir. Testler başarılı olursa, paketin test tamamlandığını [vaashelp@microsoft.com](mailto:vaashelp@microsoft.com) ve Azure Stack merkezi dijital imzasıyla dijital olarak imzalanması gerektiğini bildirin. Microsoft, paketi imzalar ve paketin VaaS portalında indirileceği Azure Stack hub ortağına bildirir.

Daha fazla bilgi için bkz. [OEM paketlerini doğrulama](azure-stack-vaas-validate-oem-package.md).

## <a name="preview-vaas-test-collateral"></a>Önizleme VaaS test yardımcı malzemeleri

Microsoft, Azure Stack hub 'ında yeni özellikleri düzenli olarak kullanıma sunar. Pazara bu özellikleri sunmaya yönelik geliştirme sürecinin bir parçası olarak, yeni test yardımcı malzemeleri **test geçiş** iş akışında kullanılabilir hale getirilir. Test geçiş iş akışı, resmi olmayan test yürütmeye izin vermek için diğer iş akışlarından test yardımcı malzemeleri içerir. Testleri onaya göndermek için test geçiş iş akışını kullanmayın. Çözümünüz için resmi onay almak üzere çözüm doğrulama ve paket doğrulama iş akışlarını kullanın.

Daha fazla bilgi için bkz. [hızlı başlangıç: bir hizmet portalı olarak doğrulamayı kullanarak ilk testinizi zamanlayın](azure-stack-vaas-schedule-test-pass.md).

## <a name="validation-workflow-tests-summary"></a>Doğrulama iş akışı testleri Özeti

| Doğrulama iş akışı | Gerekli testler |
|----|------------|
| [Yeni çözüm doğrulaması](azure-stack-vaas-validate-solution-new.md) | Bulut benzetimi altyapısı<br>İşlem SDK 'Sı Işlem paketi<br>Disk kimliği sınaması<br>Keykasauzantısı SDK 'Sı çalışma paketi<br>Keykasasdk Işlem paketi<br>Ağ SDK Işlem paketi<br>Depolama hesabı SDK 'Sı Işlem paketi<br> |
| [OEM paketi doğrulaması](azure-stack-vaas-validate-oem-package.md) | OEM Uzantı paketi doğrulaması<br>Bulut benzetimi altyapısı |
| [Aylık güncelleştirme doğrulaması](azure-stack-vaas-validate-microsoft-updates.md) | Aylık Azure Stack hub güncelleştirmesi doğrulaması<br>Bulut benzetimi altyapısı<br> |

## <a name="next-steps"></a>Sonraki adımlar

- [Doğrulamanız hizmet kaynakları olarak ayarlama](azure-stack-vaas-set-up-resources.md)
- [Hizmet anahtar kavramları olarak doğrulama](azure-stack-vaas-key-concepts.md) hakkında bilgi edinin
