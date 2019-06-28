---
title: Bulut hizmeti sağlayıcıları için Azure Stack için raporlama altyapınızın kullanım | Microsoft Docs
description: Azure Stack oluşur ve Azure'a iletir gibi bir bulut hizmeti sağlayıcısı (CSP) tarafından hizmet verilen kiracılar için kullanımını izlemek için gereken altyapıyı içerir.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 05/09/2019
ms.openlocfilehash: 0b4ff1e6c76bedc4618bfa527b0045d7bfce41af
ms.sourcegitcommit: bcaad8b7db2ea596018d973cb29283d8c6daebfb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67419484"
---
# <a name="usage-reporting-infrastructure-for-cloud-service-providers"></a>Bulut hizmeti sağlayıcıları için raporlama altyapınızın kullanımı

Azure Stack gerçekleşir ve Azure'a iletir kullanımını izlemek için gereken altyapıyı içerir. Azure'da, Azure ticaret kullanım verileri işler ve uygun Azure abonelikleri için kullanım ücretlerini. Kullanımı izleme, Azure genel bulutunda izlenen aynı şekilde bu gerçekleşir.

Bazı genel Azure ve Azure Stack arasında tutarlı kavramlardır. Azure Stack benzer bir rol bir Azure aboneliğine karşılamak yerel abonelikler var. Yerel abonelikler yalnızca yerel olarak geçerlidir. Kullanım Azure'a iletildiğinde yerel abonelikler Azure aboneliklerine eşlenir.

Azure Stack yerel kullanım ölçümleri sahiptir. Yerel kullanım ölçümleri Azure ticaret kullanılan eşleştirilir. Ancak, ölçüm kimlikleri farklıdır. Daha fazla ölçümleri bir faturalandırma için Microsoft'un kullandığı daha yerel olarak kullanılabilir.

Azure Stack ve Azure hizmetleri nasıl fiyatlandırılır arasındaki bazı farklar vardır. Örneğin, Azure Stack'te Vm'leri için ücretlendirme yalnızca sanal çekirdek/saat ile aynı fiyat aksine Azure, tüm VM serisi için temel alır. Genel Azure'da farklı donanım farklı fiyatlara yansıtılmıştır nedenidir. Farklı VM sınıflar için farklı ücretler kaydedilecek neden olduğundan Azure Stack'te donanım, müşteri sağlar.

Ticaret ve iş ortağı Merkezi, kullanıcıların fiyatları Azure hizmetlerinde olduğu gibi aynı şekilde kullanılan Azure Stack ölçümleri hakkında bilgi edinebilirsiniz:

1. İş ortağı Merkezi'nde Git **Panosu menüsünden**, ardından **satmak**, ardından **fiyatlandırma ve teklifler**.
2. Altında **kullanım tabanlı Hizmetleri**seçin **geçerli**.
3. Açık **genel CSP fiyat Listesi'nde Azure'da** elektronik tablo.
4. Filtre **bölge Azure Stack =** .

## <a name="terms-used-for-billing-and-usage"></a>Faturalama ve kullanım için kullanılan terimler

Aşağıdaki terimler ve kavramlar kullanımı için kullanılan ve Azure stack'teki faturalandırma şunlardır:

| Terim | Tanım |
| --- | --- |
| Doğrudan bir CSP iş ortağı | Doğrudan bir bulut hizmeti sağlayıcısı (CSP) iş ortağı fatura doğrudan Azure ve Azure Stack kullanım ve fatura müşterilerin Microsoft'tan doğrudan alır. |
| Dolaylı CSP | Dolaylı satıcıları dolaylı sağlayıcısı (dağıtıcı olarak da bilinir) ile çalışır. Satıcılar, son müşteriler kazanmak; Dolaylı sağlayıcısı olan Microsoft faturalama ilişkiyi tutar, müşteri faturalandırma yönetir ve ürün desteği gibi ek hizmetler sağlar. |
| Son Müşteri | Son müşterilerin uygulamaları ve Azure Stack üzerinde çalıştırılan diğer iş yükleri kendi devlet kurumları ve işletmelerin önerilir. |

## <a name="next-steps"></a>Sonraki adımlar

- CSP programı hakkında daha fazla bilgi için bkz: [bulut çözümleri](https://partner.microsoft.com/solutions/microsoft-cloud-solutions).
- Azure yığını kaynak kullanım bilgilerini alma hakkında daha fazla bilgi için bkz: [kullanım ve faturalandırma Azure Stack'te](azure-stack-billing-and-chargeback.md).
