---
title: Azure Stack güncelleştirme sorunlarını giderme | Microsoft Docs
description: Azure Stack bir operatör olarak, Azure Stack üretime mümkün olduğunca çabuk dönebilmesi için güncelleştirme sorunlarını çözmeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2019
ms.author: mabrigg
ms.lastreviewed: 09/23/2019
ms.reviewer: ppacent
ms.openlocfilehash: 76c6c21c2a728004d2a33c04a02b905ec674b972
ms.sourcegitcommit: d967cf8cae320fa09f1e97eeb888e3db5b6e7972
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71301231"
---
# <a name="troubleshooting-patch-and-update-issues-for-azure-stack"></a>Azure Stack için düzeltme eki ve güncelleştirme sorunlarını giderme

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Azure Stack güncelleştirirken karşılaştığınız sorunları gidermek için bu makaledeki Kılavuzu kullanabilirsiniz.

## <a name="preparationfailed"></a>PreparationFailed

**Uygulanabilir**: Bu sorun desteklenen tüm yayınlar için geçerlidir.

**Neden**: Azure Stack güncelleştirme yüklenmeye çalışıldığında, güncelleştirmenin durumu başarısız olabilir ve durumu olarak `PreparationFailed`değişebilir. Bu, güncelleştirme kaynak sağlayıcısı 'nın (URP) dosyaları depolama kapsayıcısından işlenmek üzere bir iç altyapı paylaşımında doğru bir şekilde aktarmaması nedeniyle oluşur.

**Düzeltme**: Sürüm 1901 ' den başlayarak (1.1901.0.95), şimdi **Güncelleştir** ' i ( **sürdürülmez**) tıklayarak bu soruna geçici bir çözüm bulabilirsiniz. URP daha sonra önceki girişimden dosyaları temizler ve indirmeyi yeniden başlatır. Sorun devam ederse, [güncelleştirmeleri yükleme](azure-stack-apply-updates.md?#install-updates-and-monitor-progress) bölümünü izleyerek güncelleştirme paketini el Ile karşıya yüklemeniz önerilir.

**Oluşum**: Common

## <a name="next-steps"></a>Sonraki adımlar

- [Güncelleştirme Azure Stack](azure-stack-updates.md)  
- [Yardım ve destek Microsoft Azure Stack](azure-stack-help-and-support-overview.md)
