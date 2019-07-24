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
ms.date: 07/23/2019
ms.author: mabrigg
ms.lastreviewed: 12/01/2018
ms.openlocfilehash: 64935022f8ace33bd4350ab4ef5421636ff3991e
ms.sourcegitcommit: b95983e6e954e772ca5267304cfe6a0dab1cfcab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68418513"
---
# <a name="how-to-consume-monitoring-data-from-azure-stack"></a>Azure Stack izleme verilerini kullanma

*Uygulama hedefi: Azure Stack tümleşik sistemler*

İzleme verileri, Azure Izleyici işlem hattı ile tek bir konumda bulunur, tıpkı Global Azure 'da Azure Izleyici gibi. Genel Azure 'da bulunan tüm izleme verileri Azure Stack ' de kullanılabilir. Bu makalede, Azure Stack izleme verilerini kullanmanın çeşitli yolları hakkında bir Özet sağlıyoruz.
 
## <a name="options-for-data-consumption"></a>Veri tüketimi için seçenekler

| Veri türü | Category | Desteklenen hizmetler | Erişim yöntemleri |
|-------------------------------------------------------------|----------|------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Azure Izleyici platformu düzeyi ölçümleri | Ölçümler | [Azure Stack Azure Izleyici ile desteklenen ölçümler](azure-stack-metrics-supported.md) | REST API |
| Konuk işletim sistemi ölçümlerini hesapla (örneğin, performans sayısı) | Ölçümler | Windows ve Linux VM 'Leri | Depolama tablosu veya blobu:<br>Windows veya Linux Azure Tanılama <br>Olay Hub 'ı:<br>Windows Azure Tanılama |
| Depolama ölçümleri | Ölçümler | Azure Storage | Depolama tablosu:<br>Depolama Analizi |
| Etkinlik günlüğü | Events | Tüm Azure Hizmetleri | REST API:<br>Azure Izleyici olay API 'SI |
| Konuk işletim sistemi günlüklerini hesaplama (örneğin, IIS, ETW, syslogs) | Events | Windows ve Linux VM 'Leri | Depolama tablosu veya blobu:<br>Windows veya Linux Azure Tanılama <br>Olay Hub 'ı:<br>Windows Azure Tanılama |
| Depolama günlükleri | Events | Azure Storage | Depolama tablosu:<br>Depolama Analizi |

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [Azure Stack'te Azure izleme](azure-stack-metrics-azure-data.md).
