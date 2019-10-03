---
title: Azure Stack için fiziksel bellek kapasitesini yönetme | Microsoft Docs
description: Azure Stack için kullanılabilir depolama alanını izleyin ve yönetin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 84518E90-75E1-4037-8D4E-497EAC72AAA1
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: 147bfb6ba35115f44fe24f388f36814db766c7f2
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829456"
---
# <a name="manage-physical-memory-capacity-for-azure-stack"></a>Azure Stack için fiziksel bellek kapasitesini yönetme

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Azure Stack'te kullanılabilir toplam bellek kapasitesini artırmak için bellek ekleyebilirsiniz. Azure Stack, fiziksel sunucunuz da *ölçek birimi düğümü*olarak adlandırılır. Tek bir ölçek biriminin üyesi olan tüm ölçek birim düğümleri aynı miktarda belleğe sahip olmalıdır.

> [!note]  
> Devam etmeden önce, üreticinizin fiziksel bellek yükseltmesini destekleyip desteklemediğini öğrenmek için donanım üreticinizin belgelerine başvurun. OEM donanım satıcısı destek sözleşmeniz, satıcının fiziksel sunucu raf yerleşimini ve cihaz üretici yazılımı güncelleştirmesini gerçekleştirmesini gerektirebilir.

Aşağıdaki akış diyagramı, her bir ölçek birimi düğümüne bellek eklemek için genel işlemi gösterir.

![Her ölçek birimi düğümüne bellek ekleyin](media/azure-stack-manage-storage-physical-capacity/process-to-add-memory-to-scale-unit.png)

## <a name="add-memory-to-an-existing-node"></a>Mevcut bir düğüme bellek ekleme
Aşağıdaki adımlarda bellek ekleme işlemi için üst düzey bir genel bakış sağlanmaktadır. 

> [!Warning]
> OEM tarafından sağlanmış belgelerinize başvurulmadan bu adımları takip edin.
> 
> [!Warning]
> Bir sıralı bellek yükseltmesi desteklenmediğinden ölçek biriminin tamamının kapatılması gerekir.

1. [Başlatma ve durdurma Azure Stack](azure-stack-start-and-stop.md) makalesinde belgelenen adımları kullanarak Azure Stack durdurun.
2. Her fiziksel bilgisayardaki belleği, donanım üreticinizin belgelerini kullanarak yükseltin.
3. [Başlat ve durdur Azure Stack](azure-stack-start-and-stop.md) makalesindeki adımları kullanarak Azure Stack başlatın.

## <a name="next-steps"></a>Sonraki adımlar

 - Azure Stack ' de depolama hesaplarını yönetmeyi öğrenmek için, iş ihtiyaçlarına göre depolama kapasitesini bulmak, kurtarmak ve geri kazanmak için, bkz. [Azure Stack depolama hesaplarını yönetme](azure-stack-manage-storage-accounts.md).
 - Azure Stack bulut operatörü Azure Stack dağıtımının depolama kapasitesini izleyip yönettiğini öğrenmek için bkz. [Azure Stack için depolama kapasitesini yönetme](azure-stack-manage-storage-shares.md). 
