---
title: Azure Stack izleme verilerini kullanma | Microsoft Docs
description: Azure Stack izleme verilerini kullanan seçenekler hakkında bilgi edinin.
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
ms.date: 04/15/2019
ms.author: mabrigg
ms.lastreviewed: 12/01/2018
ms.openlocfilehash: 0b04a7834218d07706e6314a9c41e0370ff6f6fd
ms.sourcegitcommit: b36d078e699c7924624b79641dbe9021af9606ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67816182"
---
# <a name="how-to-consume-monitoring-data-from-azure-stack"></a>Azure Stack izleme verilerini kullanma

*Uygulama hedefi: Azure Stack tümleşik sistemleri*

Azure İzleyici'de genel Azure gibi Azure İzleyici'işlem hattı ile tek bir konumda izleme verileri bulunamadı. Ancak, tüm genel Azure izleme verileri Azure Stack'te kullanılabilir. Bu makalede, Azure Stack izleme verilerinin kullanmak için çeşitli yollar özetini sunuyoruz.
 
## <a name="options-for-data-consumption"></a>Veri tüketimi seçeneklerini

| Veri türü | Category | Desteklenen hizmetler | Erişim yöntemi |
|-------------------------------------------------------------|----------|------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Azure platform düzeyi ölçümlerini izleme | Ölçümler | [Azure Stack'te Azure İzleyici ile desteklenen ölçümler](azure-stack-metrics-supported.md) | REST API |
| Konuk işletim sistemi ölçümleri (örneğin, Perf sayısı) işlem | Ölçümler | Windows ve Linux Vm'leri | Depolama tablo veya blob:<br>Windows veya Linux Azure tanılama <br>Olay hub'ı:<br>Windows Azure Tanılama |
| Depolama ölçümleri | Ölçümler | Azure Storage | Depolama tablosu:<br>Depolama Analizi |
| Etkinlik günlüğü | Events | Tüm Azure Hizmetleri | REST API:<br>Azure izleyici olay API'si |
| Konuk işletim sistemi günlükleri (örneğin, IIS, ETW, Syslog) işlem | Events | Windows ve Linux Vm'leri | Depolama tablo veya blob:<br>Windows veya Linux Azure tanılama <br>Olay hub'ı:<br>Windows Azure Tanılama |
| Depolama günlükleri | Events | Azure Storage | Depolama tablosu:<br>Depolama Analizi |

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [Azure Stack'te Azure izleme](azure-stack-metrics-azure-data.md).
