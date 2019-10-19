---
title: Azure Stack 'de fiziksel bellek kapasitesini yönetme | Microsoft Docs
description: Azure Stack ' de fiziksel belleği ve kapasiteyi izlemeyi ve yönetmeyi öğrenin.
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
ms.openlocfilehash: f72ad1b86bf8ef84e64f79603c27d14571b00838
ms.sourcegitcommit: b5eb024d170f12e51cc852aa2c72eabf26792d8d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72534158"
---
# <a name="manage-physical-memory-capacity-in-azure-stack"></a>Azure Stack fiziksel bellek kapasitesini yönetme

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Azure Stack toplam kullanılabilir bellek kapasitesini artırmak için daha fazla bellek ekleyebilirsiniz. Azure Stack, fiziksel sunucunuz da *ölçek birimi düğümü*olarak adlandırılır. Tek bir ölçek biriminin üyesi olan tüm ölçek birim düğümleri aynı miktarda belleğe sahip olmalıdır.

> [!note]  
> Devam etmeden önce, üreticinizin bir fiziksel bellek yükseltmesini destekleyip desteklemediğini öğrenmek için donanım üreticinizin belgelerine başvurun. OEM donanım satıcısı destek sözleşmeniz, satıcının fiziksel sunucu raf yerleşimini ve cihaz üretici yazılımı güncelleştirmesini gerçekleştirmesini gerektirebilir.

Aşağıdaki akış diyagramı, her bir ölçek birimi düğümüne bellek eklemek için genel işlemi gösterir.

![Her ölçek birimi düğümüne bellek ekleme işlemi](media/azure-stack-manage-storage-physical-capacity/process-to-add-memory-to-scale-unit.png)

## <a name="add-memory-to-an-existing-node"></a>Mevcut bir düğüme bellek ekleme
Aşağıdaki adımlar, bellek ekleme işlemine yüksek düzeyde bir genel bakış sağlar.

> [!Warning]
> OEM tarafından sağlanmış belgelerinize başvurmadan bu adımları takip etmeyin.
> 
> [!Warning]
> Bir sıralı bellek yükseltmesi desteklenmediğinden ölçek biriminin tamamının kapatılması gerekir.

1. [Başlatma ve durdurma Azure Stack](azure-stack-start-and-stop.md) makalesinde belgelenen adımları kullanarak Azure Stack durdurun.
2. Her fiziksel bilgisayardaki belleği, donanım üreticinizin belgelerini kullanarak yükseltin.
3. [Başlat ve durdur Azure Stack](azure-stack-start-and-stop.md) makalesindeki adımları kullanarak Azure Stack başlatın.

## <a name="next-steps"></a>Sonraki adımlar

 - Azure Stack ' de depolama hesaplarını yönetme hakkında bilgi edinmek için bkz. [Azure Stack depolama hesaplarını yönetme](azure-stack-manage-storage-accounts.md).
 - Azure Stack dağıtımınızın depolama kapasitesini izleme ve yönetme hakkında bilgi edinmek için bkz. [Azure Stack depolama kapasitesini yönetme](azure-stack-manage-storage-shares.md).
