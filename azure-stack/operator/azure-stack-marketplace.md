---
title: Azure Stack (bulut işleci) bir özel Market öğesi yayımlama | Microsoft Docs
description: Azure Stack operatör Azure Stack'te bir özel Market öğesi yayımlama hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 60871cbb-eed2-433c-a76d-d605c7aec06c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 05/07/2019
ms.openlocfilehash: 70a1e1223accda5dfb273372da486588fb030afc
ms.sourcegitcommit: ccd86bd0862c45de1f6a4993f783ea2e186c187a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65172747"
---
# <a name="azure-stack-marketplace-overview"></a>Azure Stack Marketini genel bakış

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack Marketini, hizmetleri, uygulamaları ve Azure Stack için özelleştirilmiş bir kaynaklar koleksiyonudur. Kaynaklar, ağları, sanal makineler, depolama ve daha fazlasını içerir. Market yeni kaynaklar oluşturmak ve yeni uygulamalar dağıtmak için kullanın. veya göz atın ve kullanmak istediğiniz öğeleri seçin. Bir Market öğesi kullanmak için kullanıcılar bunları öğesine erişimi veren bir teklife abone olması gerekir.

Azure Stack operatör eklenecek öğeleri karar verin (Market'te yayımlama). Uygulama Hizmetleri, veritabanları gibi öğeleri Yayımla ve benzeri. Yayımlama bunları tüm kullanıcılara görünür hale getirir. Oluşturduğunuz özel öğeler yayımlayabilir veya bir büyümesini öğelerini yayımlamadan [Azure Market öğeleri listesi](azure-stack-marketplace-azure-items.md). Kullanıcıların bir öğe Market'te yayımladığınızda, beş dakika içinde görebilirsiniz.

> [!CAUTION]  
> Görüntüleri ve JSON dosyaları dahil tüm galeri öğesi yapıtlar, Azure Stack Market'te kullanılabilir hale getirme sonra kimlik doğrulama olmadan erişilebilir. Özel Market öğesi yayımlama sırasında daha fazla konular için bkz [oluşturun ve bir Market öğesi yayımlama](azure-stack-create-and-publish-marketplace-item.md).

Yönetici portalı Seç Market açmak için **+ kaynak Oluştur**.

![Market](media/azure-stack-marketplace/marketplace1.png)

## <a name="marketplace-items"></a>Marketplace öğeleri

Bir Azure Stack Marketini öğe, bir hizmet, uygulama veya kullanıcılarınızın indirip kullanabilirsiniz kaynak değil. Planlar ve teklifler gibi yönetim öğelerini de dahil olmak üzere tüm kullanıcılarınız, tüm Azure Stack Marketini öğeleri görülebilir. Bu öğelerin görünüm, ancak bunlar kullanıcılara işlevsel aboneliği gerektirmez.

Her bir Market öğesi sahiptir:

* Kaynak sağlama için bir Azure Resource Manager şablonu.
* Meta veriler, dizeler, simgeler ve diğer pazarlama Güvenceleri gibi.
* Portalda öğeyi görüntülemek için biçimlendirme bilgileri.

Market'te her öğe, Azure galeri paketi (.azpkg) biçimini kullanır. Azure Stack, dağıtım veya çalışma zamanı kaynakları (kod, yazılım veya sanal makine görüntüleri .zip dosyalarını) Market öğesi bir parçası olarak değil ayrı ayrı ekleyin.

Azure veya özel görüntüleri karşıya yüklemek, indirmek, 1803 ve üzeri sürümü ile Azure Stack görüntüleri seyrek dosyalarına dönüştürür. Bu işlem süresini uzatır görüntü eklerken, ancak kazandırır ve bu görüntülerin dağıtımını hızlandırır. Dönüştürme, yalnızca yeni görüntüleri için geçerlidir. Var olan görüntülerden değiştirilmez.

## <a name="next-steps"></a>Sonraki adımlar

* [Market öğelerini indirme](azure-stack-download-azure-marketplace-item.md)  
* [Bir Market öğesi oluşturma ve yayımlama](azure-stack-create-and-publish-marketplace-item.md)
