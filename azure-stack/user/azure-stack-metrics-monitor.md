---
title: Azure Stack izleme verilerini tüketme | Microsoft Docs
description: Azure Stack izleme verilerini tüketme seçenekleri hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/11/2019
ms.author: mabrigg
ms.lastreviewed: 11/11/2019
ms.openlocfilehash: 20e4f71480aa377e56115c499f96492e768010c3
ms.sourcegitcommit: 102ef41963b5d2d91336c84f2d6af3fdf2ce11c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73955730"
---
# <a name="how-to-consume-monitoring-data-from-azure-stack"></a>Azure Stack izleme verilerini kullanma

*Uygulama hedefi: Azure Stack tümleşik sistemler*

İzleme verileri, Azure Izleyici işlem hattı ile tek bir konumda bulunur, tıpkı Global Azure 'da Azure Izleyici gibi. Genel Azure 'da bulunan tüm izleme verileri Azure Stack ' de kullanılabilir. Bu makalede, Azure Stack izleme verilerini kullanmanın çeşitli yolları hakkında bir Özet sağlıyoruz.
 
## <a name="options-for-data-consumption"></a>Veri tüketimi için seçenekler

| Veri türü | Kategori | Desteklenen hizmetler | Erişim yöntemleri |
|-------------------------------------------------------------|----------|------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Azure Izleyici platformu düzeyi ölçümleri | Ölçümler | [Azure Stack Azure Izleyici ile desteklenen ölçümler](azure-stack-metrics-supported.md) | REST API |
| Konuk işletim sistemi ölçümlerini hesapla (örneğin, performans sayısı) | Ölçümler | Windows ve Linux VM 'Leri | Depolama tablosu veya blobu:<br>Windows veya Linux Azure Tanılama <br>Olay Hub 'ı:<br>Windows Azure Tanılama |
| Depolama ölçümleri | Ölçümler | Azure Depolama | Depolama tablosu:<br>Depolama Analizi |
| Etkinlik günlüğü | Etkinlikler | Tüm Azure Hizmetleri | REST API:<br>Azure Izleyici olay API 'SI |
| Konuk işletim sistemi günlüklerini hesaplama (örneğin, IIS, ETW, syslogs) | Etkinlikler | Windows ve Linux VM 'Leri | Depolama tablosu veya blobu:<br>Windows veya Linux Azure Tanılama <br>Olay Hub 'ı:<br>Windows Azure Tanılama |
| Depolama günlükleri | Etkinlikler | Azure Depolama | Depolama tablosu:<br>Depolama Analizi |

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack üzerinde Azure izleyici](azure-stack-metrics-azure-data.md)hakkında daha fazla bilgi edinin.
