---
title: Azure Stack için bir özgün ekipman üreticisi (OEM) güncelleştirmesi uygulayın | Microsoft Docs
description: Azure Stack için bir özgün ekipman üreticisi (OEM) güncelleştirmesi uygulamayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2019
ms.author: mabrigg
ms.lastreviewed: 08/15/2019
ms.reviewer: ppacent
ms.openlocfilehash: b8031afd05ba03086cfa748614b58d9b0c5248e9
ms.sourcegitcommit: b8260ef3e43f3703dd0df16fb752610ec8a86942
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70010170"
---
# <a name="azure-stack-servicing-policy"></a>Azure Stack hizmet ilkesi

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Bu makalede, Azure Stack tümleşik sistemlere yönelik bakım ilkesi, sisteminizi desteklenen bir durumda tutmak için yapmanız gerekenler ve nasıl destek alınacağı açıklanmaktadır.

## <a name="keep-your-system-under-support"></a>Sisteminizi destek altında tutun

Destek almaya devam etmek için Azure Stack güncelleştirmeleri güncel tutmanız gerekir.

Azure Stack örneğinizin desteklenen bir durumda kalması için, örneğin en son yayınlanan güncelleştirme sürümünü çalıştırması veya önceki iki güncelleştirme sürümünden herhangi birini çalıştırması gerekir.

Düzeltmeler önemli güncelleştirme sürümleri olarak kabul edilmez. Azure Stack örneğiniz *ikiden fazla güncelleştirme*tarafından arkasındaysa uyumsuz olarak kabul edilir. Destek almak için en az desteklenen sürüme güncelleştirmeniz gerekir.

Örneğin, en son kullanılabilir güncelleştirme sürümü 1904 ise ve önceki iki güncelleştirme paketi 1903 ve 1902 sürümleriyse, her iki 1902 ve 1903 de destek içinde kalır. Ancak, 1901 destek dışı. Bir ay veya iki sürüm olmadığında ilke true olarak tutulur. Örneğin, geçerli sürüm 1807 ise ve 1806 sürümü yoksa, önceki iki güncelleştirme paketi olan 1805 ve 1804 destek içinde kalır.

Microsoft yazılım güncelleştirme paketleri toplu değildir ve önceki güncelleştirme paketini veya düzeltmeyi önkoşul olarak gerektirir. Bir veya daha fazla güncelleştirmeyi erteistemediğinize karar verirseniz, en son sürüme ulaşmak istiyorsanız genel çalışma zamanını göz önünde bulundurun.

## <a name="get-support"></a>Destek alın

Azure Stack, Azure ile aynı destek sürecini izler. Kurumsal müşteriler, [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)bölümünde açıklanan işlemi izleyebilir. Bir bulut hizmeti sağlayıcısı (CSP) müşterisiyseniz, destek için CSP 'nize başvurun. Daha fazla bilgi için bkz. [Azure desteği SSS](https://azure.microsoft.com/support/faq/).

# <a name="next-steps"></a>Sonraki adımlar

[Azure Stack güncelleştirmelerini yönetme](azure-stack-updates.md)
