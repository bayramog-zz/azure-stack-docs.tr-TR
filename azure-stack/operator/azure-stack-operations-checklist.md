---
title: Azure Stack işlemler denetim listesi | Microsoft Docs
description: Azure Stack işleçlerinin yapması gereken ortak görevleri ve bunların ne sıklıkta yapılacağını öğrenin.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: conceptual
ms.date: 10/07/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 10/07/2019
ms.openlocfilehash: 246fdc867080da1816f00c9fb6a68179b6944275
ms.sourcegitcommit: 12034a1190d52ca2c7d3f05c8c096416120d8392
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038201"
---
# <a name="azure-stack-operators-checklist"></a>Azure Stack işlecinin denetim listesi

Azure Stack işleçlerin başarılı olabilmesi için bazı işlemler denetim listelerini birlikte yerleştirdik. Ortamınız için gerçekleştirilmesi gereken belirli işlemleri ekleyebilirsiniz. Her gün, haftalık ve aylık olarak ne yapılacağını bilen bir operatör olarak eklemeye yardımcı olmak amaçlıyordu. 

## <a name="routine-daily-weekly-and-monthly-operations"></a>Günlük, haftalık ve aylık işlemler için rutin

Azure Stack işleçlerin aşağıdaki işlemleri düzenli olarak yapması gerekir: 

> [!div class="checklist"]
> * [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/overview) 'de hataları izleme ve düzeltme [Operations Manager](https://docs.microsoft.com/system-center/scom/welcome) günlük olarak iki kez
> * Her gün bir kez [Test-AzureStack](azure-stack-diagnostic-test.md) Çalıştır
> * [Azure Stack kullanımı](azure-stack-usage-reporting.md) haftalık olarak raporla 
> * [OEM bellenimini](azure-stack-update-oem.md) her iki ayda BIR veya OEM tarafından bildirilen olarak güncelleştirme
> * [Azure Stack güncelleştirmelerini](release-notes-checklist.md) ayda hazırlama

## <a name="day-to-day-operations-as-needed"></a>Gerektiğinde gündelik işlemler

Güne kadar Azure Stack işleçlerinin de bu işlemleri tamamlaması gerekir: 

> [!div class="checklist"]
> * [Plan](azure-stack-create-plan.md) oluşturma
> * [Teklif](azure-stack-create-offer.md) oluşturma
> * [Abonelik](azure-stack-subscribe-plan-provision-vm.md) oluşturma
> * Güncelleştirme [kotaları](azure-stack-quota-types.md)
> * [Market örnekleri](azure-stack-create-and-publish-marketplace-item.md) yayımlama
> * Özel bir [sanal makine görüntüsü](azure-stack-add-vm-image.md) ekleme
> * [Sanal Makine Ölçek Kümeleri](azure-stack-compute-add-scalesets.md) sunma 
> * [Market güncelleştirmelerini](azure-stack-marketplace-changes.md) uygulama
> * [Telemetriyi](azure-stack-telemetry.md) yapılandırma
> * [Bölge yönetimini](azure-stack-region-management.md) gözden geçirme
> * [Azure Stack hizmetlerini kapatma veya yeniden başlatma](azure-stack-start-and-stop.md) 
> * [Ölçek birimi düğümünü](azure-stack-replace-node.md) değiştirme
> * [Fiziksel disk](azure-stack-replace-disk.md) değiştirme
> * Bir [donanım bileşenini](azure-stack-replace-component.md) değiştirme
> * [SQL](azure-stack-sql-resource-provider-update.md), [MySQL](azure-stack-mysql-resource-provider-update.md)ve [App Service](azure-stack-app-service-update.md) gibi eklenti kaynak sağlayıcılarını güncelleştirme
> * [Tanılama günlüğü toplamayı](azure-stack-diagnostic-log-collection-overview.md) Çalıştır  



<!---Ask Jeff, Brian, is this everything you do, how can we make it more useful? Theebs has another user.

To be successful, do these things

ask Brian what are all the the things they need to write down for quick access, like passswords, IP addresses, and so on, to make them more efficient

have a password manager for azure-stack, pep, inventory

A checklist so operators can be successful, so someone onboarding is operator know what to do weekly, daily, monthly. --->


