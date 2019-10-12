---
title: Fiziksel cihaz denetimini Azure Stack veri merkezinizle tümleştirin | Microsoft Docs
description: Fiziksel cihaz erişim denetimini Azure Stack veri merkezinizle tümleştirmeyi öğrenin.
services: azure-stack
author: PatAltimore
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 06/10/2019
ms.author: patricka
ms.reviewer: thoroet
ms.lastreviewed: 06/10/2019
keywords: ''
ms.openlocfilehash: b5fa17b3913db7ebec210fc3bf986bac6414368e
ms.sourcegitcommit: a6d47164c13f651c54ea0986d825e637e1f77018
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72277232"
---
# <a name="integrate-physical-device-auditing-with-your-azure-stack-datacenter"></a>Fiziksel cihaz denetimini Azure Stack veri merkezinizle tümleştirme

Temel kart yönetim denetleyicileri (BMC 'ler) ve ağ anahtarları gibi Azure Stack içindeki tüm fiziksel cihazlar denetim günlüklerini yayar. Denetim günlüklerini genel denetim çözümünüz ile tümleştirebilirsiniz. Cihazlar farklı Azure Stack OEM donanım satıcıları genelinde farklılık gösterdiğinden, tümleştirme denetimi hakkındaki belgeler için satıcınıza başvurun. Aşağıdaki bölümler Azure Stack ' de fiziksel cihaz denetimi için bazı genel bilgiler sağlar.  

## <a name="physical-device-access-auditing"></a>Fiziksel cihaz erişimi denetimi

Azure Stack tüm fiziksel cihazlar, TACACS veya YARıÇAP kullanımını destekler. Destek, temel kart yönetim denetleyicisi (BMC) ve ağ anahtarlarına erişimi içerir.

Azure Stack çözümler, RADIUS veya TACACS yerleşik olarak gelmez. Ancak çözümler, pazara sunulan mevcut YARıÇAP veya TACACS çözümlerinin kullanımını destekleyecek şekilde onaylanır.

Yalnızca YARıÇAP için, MSCHAPv2 doğrulanmadı. Bu, RADIUS kullanarak en güvenli uygulamayı temsil eder. Azure Stack çözümünüze dahil edilen cihazlarda TACAS veya RADIUS sağlamak için OEM donanım satıcınıza başvurun.

## <a name="syslog-forwarding-for-network-devices"></a>Ağ cihazları için Syslog iletimi

' Deki tüm fiziksel ağ aygıtları syslog iletilerini destekler Azure Stack. Azure Stack çözümleri Syslog sunucusuyla birlikte gelmez. Ancak, cihazlarda bulunan mevcut Syslog çözümlerine ileti gönderilmesini desteklemek için cihazlar onaylanmıştır.

Syslog hedef adresi dağıtım için toplanan isteğe bağlı bir parametredir, ancak dağıtım sonrası da eklenebilir. Ağ cihazlarınızda Syslog iletmeyi yapılandırmak için OEM donanım satıcınıza başvurun.

## <a name="next-steps"></a>Sonraki adımlar

[Bakım ilkesi](azure-stack-servicing-policy.md)
