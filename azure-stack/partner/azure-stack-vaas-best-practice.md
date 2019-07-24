---
title: Azure Stack doğrulama En Iyi yöntemleri. | Microsoft Docs
description: Bu makalede hizmet olarak doğrulama için en iyi yöntemler açıklanmaktadır.
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
ms.openlocfilehash: 6b4e74cce10522fc241c7662ed381793bd264093
ms.sourcegitcommit: b95983e6e954e772ca5267304cfe6a0dab1cfcab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68418572"
---
# <a name="best-practices-for-validation-as-a-service"></a>Hizmet olarak doğrulama için en iyi uygulamalar

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Bu makalede, hizmet olarak doğrulama (VaaS) içinde kaynakları yönetmeye yönelik en iyi uygulamalar ele alınmaktadır. VaaS kaynaklarına genel bakış için bkz. [hizmet anahtarı olarak doğrulama kavramları](azure-stack-vaas-key-concepts.md).

## <a name="solution-management"></a>Çözüm Yönetimi

### <a name="naming-convention-for-vaas-solutions"></a>VaaS çözümleri için adlandırma kuralı

VaaS 'de kayıtlı tüm çözümler için tutarlı bir adlandırma kuralı kullanın. Örneğin, çözüm adı aşağıdaki donanım özelliklerinden aşağıdaki gibi oluşturulabilir:

|Ürün adı | Benzersiz donanım öğesi 1 | Benzersiz donanım öğesi 2 | Çözüm adı
|---|---|---|---|
Çözümünüz XYZ |  Tüm Flash | Anahtarım X01 | MySolutionXYZ_AllFlash_MySwitchX01

### <a name="when-to-create-a-new-vaas-solution"></a>Yeni bir VaaS çözümü ne zaman oluşturulur

Aynı donanım SKU 'sunda iş akışlarını çalıştırırken aynı VaaS çözümünü kullanın. Yeni bir VaaS çözümünün yalnızca donanım SKU 'sunda değişiklik yapıldığında oluşturulması gerekir.

## <a name="workflow-management"></a>İş akışı yönetimi

### <a name="naming-convention-for-vaas-workflows"></a>VaaS iş akışları için adlandırma kuralı

Tüm VaaS iş akışı çalıştırmaları için tutarlı bir adlandırma kuralı kullanın. Örneğin, aşağıdaki yapı özelliklerinden aşağıda gösterildiği gibi bir iş akışı adı oluşturun:

|Yapı numarası (birincil) | Date | Çözüm boyutu | İş Akışı Adı
|---|---|---| ---|
1808 | 081518 | 4 DÜĞÜM | 1808_081518_4NODE

## <a name="next-steps"></a>Sonraki adımlar

- [Hizmet anahtar kavramları olarak doğrulama](azure-stack-vaas-key-concepts.md) hakkında bilgi edinin
