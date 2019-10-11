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
ms.openlocfilehash: d5606e7904fe311a54d792a18e5d4029c709b33c
ms.sourcegitcommit: 0866555e0ed240a65595052899ef1b836dd07fbc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72257752"
---
# <a name="troubleshooting-patch-and-update-issues-for-azure-stack"></a>Azure Stack için düzeltme eki ve güncelleştirme sorunlarını giderme

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Azure Stack güncelleştirirken karşılaştığınız sorunları gidermek için bu makaledeki Kılavuzu kullanabilirsiniz.

## <a name="preparationfailed"></a>PreparationFailed

**Uygulanabilir**: Bu sorun desteklenen tüm yayınlar için geçerlidir.

**Neden**: Azure Stack güncelleştirme yüklenmeye çalışıldığında, güncelleştirmenin durumu başarısız olabilir ve durumu `PreparationFailed` olarak değiştirebilir. İnternet 'e bağlı sistemler için bu, genellikle zayıf bir internet bağlantısı nedeniyle güncelleştirme paketinin düzgün şekilde indirilemedik. 

**Düzeltme**: **Şimdi yeniden yüklensin** ' i tıklayarak bu soruna geçici bir çözüm bulabilirsiniz. Sorun devam ederse, [güncelleştirmeleri yükleme](azure-stack-apply-updates.md?#install-updates-and-monitor-progress) bölümünü izleyerek güncelleştirme paketini el Ile karşıya yüklemeniz önerilir.

**Oluşum**: ortak

## <a name="next-steps"></a>Sonraki adımlar

- [Güncelleştirme Azure Stack](azure-stack-updates.md)  
- [Yardım ve destek Microsoft Azure Stack](azure-stack-help-and-support-overview.md)
