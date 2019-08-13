---
title: Azure Stack (bulut işleci) içinde özel Market öğesi yayımlama | Microsoft Docs
description: Azure Stack operatörü olarak, Azure Stack bir özel Market öğesini yayımlamayı öğrenin.
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
ms.date: 08/12/2019
ms.author: sethm
ms.reviewer: ihcherie
ms.lastreviewed: 05/07/2019
ms.openlocfilehash: be62ddc3c3e5ea7180164cac95edd125030cbc6a
ms.sourcegitcommit: 58c28c0c4086b4d769e9d8c5a8249a76c0f09e57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68959492"
---
# <a name="azure-stack-marketplace-overview"></a>Azure Stack Market 'e genel bakış

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack Marketi, Azure Stack için özelleştirilmiş hizmetler, uygulamalar ve kaynaklar koleksiyonudur. Kaynakları, ağları, sanal makineleri, depolamayı ve daha fazlasını içerir. Market 'i kullanarak yeni kaynaklar oluşturun ve yeni uygulamalar dağıtın; ya da kullanmak istediğiniz öğeleri bulup seçin. Market öğesini kullanmak için kullanıcıların öğeye erişim izni veren bir teklife abone olmaları gerekir.

Azure Stack operatörü olarak, Market 'e hangi öğelerin ekleneceğini (yayımlayacağınıza) karar verirsiniz. Veritabanları, uygulama hizmetleri ve daha fazlası gibi öğeleri yayımlayabilirsiniz. Yayımlama, öğeleri tüm kullanıcılarınız için görünür hale getirir. Oluşturduğunuz özel öğeleri yayımlayabilir veya büyüyen bir [Azure Market öğesi listesinden](azure-stack-marketplace-azure-items.md)öğe yayımlayabilirsiniz. Market 'e bir öğe yayımladığınızda, kullanıcılar bu süreyi beş dakika içinde görebilir.

> [!CAUTION]  
> Görüntüler ve JSON dosyaları da dahil olmak üzere tüm galeri öğesi yapılarına Azure Stack marketi 'nde kullanılabilir hale getirdikten sonra kimlik doğrulaması olmadan erişilebilir. Özel Market öğelerini yayımlarken daha fazla dikkat edilmesi için bkz. [Market öğesi oluşturma ve yayımlama](azure-stack-create-and-publish-marketplace-item.md).

Market 'i açmak için yönetici portalında **+ kaynak oluştur**' u seçin.

![Market](media/azure-stack-marketplace/marketplace1.png)

## <a name="marketplace-items"></a>Marketplace öğeleri

Azure Stack Market öğesi, kullanıcılarınızın indirebileceğiniz ve kullanabileceği bir hizmet, uygulama veya kaynaktır. Tüm Azure Stack Market öğeleri, planlar ve teklifler gibi yönetim öğeleri de dahil olmak üzere tüm kullanıcılarınız tarafından görülebilir. Bu yönetim öğeleri görüntülemek için bir abonelik gerektirmez, ancak kullanıcılara işlevsel değildir.

Her Market öğesi şunları içerir:

* Kaynak sağlama için bir Azure Resource Manager şablonu.
* Dizeler, simgeler ve diğer pazarlama yardımcı malzemeleri gibi meta veriler.
* Öğeyi portalda göstermek için biçimlendirme bilgileri.

Market 'Te yayımlanan her öğe Azure Galeri paketi (. azpkg) biçimini kullanır. Market öğesinin bir parçası olarak değil, ayrıca Azure Stack için dağıtım veya çalışma zamanı kaynakları (yazılım veya sanal makine görüntüleri içeren kod,. zip dosyaları) ekleyin.

Sürüm 1803 ve üzeri ile, Azure Stack Azure 'dan indirdiklerinde veya özel görüntüleri karşıya yüklerken görüntüleri seyrek dosyalara dönüştürür. Bu işlem bir görüntü eklerken zaman ekler, ancak alan kaydeder ve bu görüntülerin dağıtımını hızlandırır. Dönüştürme yalnızca yeni görüntüler için geçerlidir. Mevcut görüntüler değiştirilmez.

## <a name="next-steps"></a>Sonraki adımlar

* [Market öğelerini indirme](azure-stack-download-azure-marketplace-item.md)  
* [Market öğesi oluşturma ve yayımlama](azure-stack-create-and-publish-marketplace-item.md)
