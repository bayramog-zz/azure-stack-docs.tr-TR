---
title: Azure Stack donanım sistem durumunu izleme | Microsoft Docs
description: Azure Stack donanım bileşenlerinin sistem durumunu izlemeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: sethm
ms.reviewer: thoroet
ms.lastreviewed: 11/21/2019
ms.openlocfilehash: b41a60aaf45d4b4fbbbf00945a4048dbf9dfb13a
ms.sourcegitcommit: cefba8d6a93efaedff303d3c605b02bd28996c5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74308229"
---
# <a name="monitor-azure-stack-hardware-components"></a>Azure Stack donanım bileşenlerini izleme

Azure Stack sistem durumu ve izleme sistemi, depolama alt sisteminin durumunu zaten izler ve gerektiğinde uyarı oluşturur. Azure Stack 1910 sürümü ile sistem durumu ve izleme sistemi artık aşağıdaki donanım bileşenleri için de uyarı oluşturabilir:

- Sistem fanı
- Sistem sıcaklığı
- Güç kaynağı
- CPU’lar
- Bellek
- Önyükleme sürücüleri

> [!NOTE]
> Bu özelliği etkinleştirmeden önce, donanımınızın iş ortağınızdan hazırlandığını doğrulamanız gerekir. Ayrıca, donanım ortağınız bu özelliği BMC 'de etkinleştirmeye yönelik ayrıntılı adımları da sağlayacaktır.

## <a name="snmp-listener-scenario"></a>SNMP dinleyicisi senaryosu

TCP bağlantı noktası 162 ' deki üç ERCS örnek üzerinde bir SNMP v3 dinleyicisi çalışmaktadır. Temel kart yönetim denetleyicisi (BMC) Azure Stack dinleyicisine SNMP tuzakları gönderecek şekilde yapılandırılmalıdır. Bölge özellikleri görünümünü açarak yönetim portalından üç PEP IP 'sini alabilirsiniz.

Dinleyiciye tuzak gönderilmesi için kimlik doğrulaması gerekir ve taban BMC 'ye erişme ile aynı kimlik bilgisini kullanmanız gerekir.

TCP bağlantı noktası 162 ' deki üç ERCS örneğine bir SNMP tuzağı alındığında, OID dahili olarak eşleştirilir ve bir uyarı tetiklenir. Azure Stack sistem durumu ve izleme sistemi yalnızca donanım ortağı tarafından tanımlanan OID 'leri kabul eder. Azure Stack için bir OID bilinmiyorsa, bir uyarıyla eşleşmez.

Hatalı bir bileşen değiştirildikten sonra, BMC 'den durum değişikliğini belirten SNMP dinleyicisine bir olay gönderilir ve uyarı Azure Stack otomatik olarak kapanır.

> [!NOTE]
> Tüm düğüm veya ana kart değiştirildiğinde mevcut uyarılar otomatik olarak kapanmaz. Aynı durum, BMC 'nin yapılandırmasını kaybetmesi durumunda geçerlidir; Örneğin, fabrika sıfırlaması nedeniyle.

## <a name="next-steps"></a>Sonraki adımlar

[Güvenlik Duvarı tümleştirmesi](azure-stack-firewall.md)
