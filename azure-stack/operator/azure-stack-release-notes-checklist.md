---
title: Azure Stack etkinliği denetim listesini güncelleştir | Microsoft Docs
description: Sisteminizi en son Azure Stack güncelleştirmesine hazırlamak için denetim listesi.
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
ms.date: 08/22/2019
ms.author: sethm
ms.reviewer: hectorl
ms.lastreviewed: 08/22/2019
ms.openlocfilehash: 29ab5b34fc669f69ee4a74e52559c7ad27b20af8
ms.sourcegitcommit: b8260ef3e43f3703dd0df16fb752610ec8a86942
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70008738"
---
# <a name="azure-stack-update-activity-checklist"></a>Azure Stack etkinliği denetim listesini güncelleştir

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Azure Stack güncelleştirmesine hazırlanmak için bu denetim listesini gözden geçirin. Bu makale, Azure Stack işleçleri için güncelleştirmeyle ilgili etkinliklerin denetim listesini içerir.

## <a name="prepare-for-azure-stack-update"></a>Azure Stack güncelleştirmesine hazırlanma

| Etkinlik | Ayrıntılar |
| --- | --- |
| Bilinen sorunları gözden geçirin |[Bilinen sorunların listesi](https://docs.microsoft.com/azure-stack/operator/azure-stack-release-notes-known-issues-1906). |
| Güvenlik güncelleştirmelerini gözden geçirin | [Güvenlik güncelleştirmelerinin listesi](https://docs.microsoft.com/azure-stack/operator/azure-stack-release-notes-security-updates-1906). |
| En son OEM paketini Uygula | Sisteminizin güncelleştirdiğiniz Azure Stack sürümü için en düşük OEM paketi gereksinimlerini karşıladığından emin olmak için OEM 'nize başvurun. |
| Test-AzureStack Çalıştır | İşlemsel `Test-AzureStack -Group UpdateReadiness` sorunları belirlemek için ' i çalıştırın. |
| Sorunları çözme | Tarafından `Test-AzureStack`tanımlanan tüm işlem sorunlarını çözün. |
| Son düzeltmeleri Uygula | Şu anda yüklü olan sürüme uygulanan en son düzeltmeleri uygulayın. |
| Kapasite planlayıcısı aracını çalıştırma | İş yükü planlama ve boyutlandırmasını gerçekleştirmek için Azure Stack Capacity Planner aracının en son sürümünü kullandığınızdan emin olun. En son sürüm hata düzeltmeleri içerir ve her bir Azure Stack güncelleştirmesiyle yayınlanan yeni özellikler sağlar. |
| Güncelleştirme var | Yalnızca bağlı senaryolarda, Azure Stack dağıtımları düzenli aralıklarla güvenli bir uç noktayı kontrol edip bulut için bir güncelleştirme varsa bunu otomatik olarak bilgilendirir. Bağlantısı kesilen müşteriler, [burada açıklanan işlemi](https://docs.microsoft.com/azure-stack/operator/azure-stack-apply-updates)kullanarak yeni paketleri indirebilir ve içeri aktarabilir. |


## <a name="during-azure-stack-update"></a>Azure Stack güncelleştirme sırasında

| Etkinlik | Ayrıntılar |
|--------------------|------------------------------------------------------------------------------------------------------|
| Güncelleştirmeyi yönetme |[Azure Stack, işletmen portalını kullanarak güncelleştirmeleri yönetin](https://docs.microsoft.com/azure-stack/operator/azure-stack-updates). |
|  |  |
| Güncelleştirmeyi izleme | Operatör portalı kullanılamıyorsa, [ayrıcalıklı uç noktayı kullanarak Azure Stack güncelleştirmeleri izleyin](https://docs.microsoft.com/azure-stack/operator/azure-stack-monitor-update). |
|  |  |
| Güncelleştirmeleri sürdürür | Başarısız bir güncelleştirmeyi uyguladıktan sonra, [ayrıcalıklı uç noktayı kullanarak Azure Stack güncelleştirmeleri sürdürüyorsun](https://docs.microsoft.com/azure-stack/operator/azure-stack-monitor-update). |

> [!Important]  
> Güncelleştirme sırasında **Test-AzureStack** komutunu çalıştırmayın, çünkü güncelleştirme soruna neden olur.

## <a name="after-azure-stack-update"></a>Azure Stack güncelleştirmeden sonra

| Etkinlik | Ayrıntılar |
|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Son düzeltmeleri Uygula | Güncelleştirilmiş sürüme uygulanabilen en son düzeltmeleri uygulayın. |
| Şifreleme anahtarlarını alma | Rest şifreleme anahtarlarındaki verileri alın ve Azure Stack dağıtımınızın dışında güvenli bir şekilde depolayın. [Anahtarları alma yönergelerini](https://docs.microsoft.com/azure-stack/operator/azure-stack-security-bitlocker)izleyin. |
|  |  |
| Çoklu kiratı yeniden etkinleştirme | Çok kiracılı bir Azure Stack olması durumunda, başarılı bir güncelleştirmeden sonra [tüm konuk Dizin kiracılarını yapılandırmadığınızdan emin olun](https://docs.microsoft.com/azure-stack/operator/azure-stack-enable-multitenancy#configure-guest-directory) . |

## <a name="next-steps"></a>Sonraki adımlar

-   [Bilinen sorunların listesini gözden geçirin](https://docs.microsoft.com/azure-stack/operator/azure-stack-release-notes-known-issues-1907)  
-   [Güvenlik güncelleştirmelerinin listesini gözden geçirin](https://docs.microsoft.com/azure-stack/operator/azure-stack-release-notes-security-updates-1907)