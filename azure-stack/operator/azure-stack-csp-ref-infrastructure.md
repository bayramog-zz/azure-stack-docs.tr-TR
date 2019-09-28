---
title: Azure Stack için bulut çözümü sağlayıcıları için kullanım raporlama altyapısı | Microsoft Docs
description: Bir bulut çözümü sağlayıcısı (CSP) tarafından hizmet veren kiracılar için kullanımı izlemek üzere kullanılan kullanım raporlama altyapısı hakkında bilgi edinin.
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
ms.openlocfilehash: 1a3c59ab7650c9cd2337e8256556f8a449feacec
ms.sourcegitcommit: c2ea4ffb42563c26faaf2993ba7b484bcb6d5cb7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71342814"
---
# <a name="usage-reporting-infrastructure-for-cloud-solution-providers"></a>Bulut çözümü sağlayıcıları için kullanım raporlama altyapısı

Azure Stack, kullanım sırasında kullanımı izlemek ve Azure 'a iletmek için gereken altyapıyı içerir. Azure ticareti, Azure 'da kullanım verilerini ve kullanım ücretlerini uygun Azure aboneliklerine göre işler. Bu işlem, genel Azure bulutundaki kullanım izlemeyle aynı şekilde çalışmaktadır.

Bazı kavramlar küresel Azure ve Azure Stack arasında tutarlıdır. Azure Stack, Azure aboneliğine benzer bir rol karşılayan yerel aboneliklerdir. Yerel abonelikler yalnızca yerel olarak geçerlidir. Kullanım Azure 'a iletildiğinde yerel abonelikler Azure abonelikleriyle eşlenir.

Azure Stack yerel kullanım ölçümleri vardır. Yerel kullanım, Azure ticareti 'nde kullanılan ölçümler ile eşleştirilir. Ancak, ölçüm kimlikleri farklıdır. Microsoft 'un faturalandırma için kullandığı bir günden daha yerel olarak daha fazla ölçüm mevcuttur.

Hizmetlerin Azure Stack ve Azure 'da fiyatlandırıldıkları bazı farklılıklar vardır. Örneğin, Azure Stack, VM 'Lerin ücreti, Azure 'dan farklı olarak tüm VM dizileri için aynı fiyata sahip sanal çekirdek/saat tabanlıdır. Bu nedenle, küresel Azure 'da farklı fiyatlar farklı donanımları yansıtmaktadır. Azure Stack, müşteri donanımı sağlar, bu nedenle farklı VM sınıfları için farklı ücretler ücretlendirirken bir neden yoktur.

Ticaret ve Iş Ortağı Merkezi fiyatlarına göre kullanılan Azure Stack ölçümleri hakkında bilgi edinebilirsiniz. İşlem, Azure hizmetleri için olduğu gibidir:

1. Iş Ortağı Merkezi ' nde, **Pano menüsüne**gidin ve **Satış**' ı seçin, ardından **fiyatlandırma ve teklifler**' i seçin.
2. **Kullanım tabanlı hizmetler**altında **geçerli**' i seçin.
3. **Azure genel CSP fiyat listesi** elektronik tablosu ' nu açın.
4. **Region = Azure Stack**üzerinde filtrele.

## <a name="terms-used-for-billing-and-usage"></a>Faturalandırma ve kullanım için kullanılan terimler

Aşağıdaki hüküm ve kavramlar Azure Stack ' de kullanım ve faturalandırma için kullanılır:

| Terim | Tanım |
| --- | --- |
| Doğrudan CSP iş ortağı | Doğrudan bulut çözümü sağlayıcısı (CSP) iş ortağı doğrudan Microsoft 'tan Azure ve Azure Stack kullanımı için bir fatura alır ve müşterileri doğrudan faturalar. |
| Dolaylı CSP | Dolaylı satıcılar dolaylı bir sağlayıcıyla (dağıtımcı olarak da bilinir) çalışır. Satıcılar, son müşterileri işe alabilir; dolaylı sağlayıcı Microsoft ile faturalandırma ilişkisini tutar, müşteri faturalandırmasını yönetir ve ürün desteği gibi ek hizmetler sağlar. |
| Son müşteri | Son müşteriler, Azure Stack çalışan uygulamalara ve diğer iş yüklerine sahip olan işletmeler ve kamu kurumlardır. |

## <a name="next-steps"></a>Sonraki adımlar

- CSP programı hakkında daha fazla bilgi edinmek için bkz. [bulut çözümleri](https://partner.microsoft.com/solutions/microsoft-cloud-solutions).
- Kaynak kullanım bilgilerini Azure Stack alma hakkında daha fazla bilgi için, bkz. [Azure Stack ' de kullanım ve faturalandırma](azure-stack-billing-and-chargeback.md).
