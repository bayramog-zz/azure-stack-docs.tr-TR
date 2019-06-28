---
title: Azure Stack sürüm notları - güncelleştirme etkinlik denetim listesi | Microsoft Docs
description: Sisteminizi en son Azure Stack için hazırlamak için hızlı denetim listesini güncelleştirin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2019
ms.author: sethm
ms.reviewer: hectorl
ms.lastreviewed: 06/27/2019
ms.openlocfilehash: c5108dc8b24fec7569d54964217f146a5cefa5e6
ms.sourcegitcommit: bcaad8b7db2ea596018d973cb29283d8c6daebfb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67419585"
---
# <a name="azure-stack-update-activity-checklist"></a>Azure Stack güncelleştirme etkinlik denetim listesi

Bu makale, Azure Stack operatörleri için güncelleştirme ile ilgili etkinlikler listesi içerir. Azure Stack için bir güncelleştirmeyi uygulamak hazırlıyorsanız, bu bilgileri gözden geçirebilirsiniz.

## <a name="prepare-for-azure-stack-update"></a>Azure Stack güncelleştirme için hazırlama

| Etkinlik              | Ayrıntılar                                                                          |
|-----------------------|----------------------------------------------------------------------------------|
| Bilinen sorunları gözden geçirin   | [Bilinen sorunların listesi](azure-stack-release-notes-known-issues-1906.md).                |
| Güvenlik güncelleştirmeleri gözden geçirin | [Güvenlik güncelleştirmelerinin listesi](azure-stack-release-notes-security-updates-1906.md).      |
| Test-AzureStack çalıştırın   | Çalıştırma `Test-AzureStack -Group UpdateReadiness` işletimsel sorunları belirlemek için.      |
| Sorunları çözün        | Tarafından tanımlanan tüm çalışma sorunlarını çözün **Test AzureStack**.                |
| En son düzeltmeleri uygulama | Şu anda yüklü sürümü için geçerli en son düzeltmeleri uygulama.         |
| Kapasite Planlayıcısı aracını çalıştırma | En son sürümünü kullandığınızdan emin olun [Azure Stack Capacity Planner](https://aka.ms/azstackcapacityplanner) iş yükünüz planlama ve boyutlandırma gerçekleştirmek için aracı. En son sürüm hata düzeltmeleri içerir ve yayımlanan yeni özellikler sağlayan her Azure Stack güncelleştirme. |
| Güncelleştirme var       | Yalnızca bağlı senaryolarda Azure Stack dağıtımları güvenli bir uç nokta düzenli aralıklarla denetleyin ve bulut için bir güncelleştirme varsa, otomatik olarak bilgilendirme. Bağlantısı kesilmiş müşteriler indirebilir ve paket yeni 1906 kullanarak içeri aktarma [işlemi burada açıklanan](azure-stack-apply-updates.md).               |

## <a name="during-azure-stack-update"></a>Azure Stack güncelleştirme sırasında

| Etkinlik              | Ayrıntılar                                                                          |
|-----------------------|----------------------------------------------------------------------------------|
| Güncelleştirme yönetme         | [Azure Stack operatörü portalı kullanarak güncelleştirmeleri yönetme](azure-stack-updates.md). |
| Güncelleştirme izleme        | İşleç portal kullanılamıyorsa [izleme ayrıcalıklı uç noktayı kullanarak Azure stack'teki güncelleştirmeleri](azure-stack-monitor-update.md). |
| Güncelleştirmelerini Sürdür            | Başarısız bir güncelleştirme, düzeltme sonrasında [ayrıcalıklı uç noktayı kullanarak Azure stack'teki güncelleştirmelerini Sürdür](azure-stack-monitor-update.md). |

> [!IMPORTANT]  
> Çalıştırma **Test AzureStack** güncelleştirme sırasında bu neden olacak kabin güncelleştirilecek.

## <a name="after-azure-stack-update"></a>Azure Stack güncelleştirmesinden sonra

| Etkinlik              | Ayrıntılar                                                                          |
|-----------------------|----------------------------------------------------------------------------------|
| En son düzeltmeleri uygulama | Güncelleştirilmiş sürümü için geçerli olan en son düzeltmeler uygulayın.                          |
| Şifreleme anahtarlarını alma | Rest şifreleme anahtarları, veri almak ve bunları Azure Stack dağıtımınıza dışında güvenli bir şekilde depolayın. İzleyin [anahtarlarını almak yönergeler](azure-stack-security-bitlocker.md). |

## <a name="next-steps"></a>Sonraki adımlar

- [Bilinen sorunlar listesini inceleyin](azure-stack-release-notes-known-issues-1906.md)
- [Güvenlik güncelleştirmeleri listesini gözden geçirin](azure-stack-release-notes-security-updates-1906.md)
