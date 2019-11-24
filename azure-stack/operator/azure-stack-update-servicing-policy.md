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
ms.date: 09/10/2019
ms.author: mabrigg
ms.lastreviewed: 09/10/2019
ms.reviewer: ppacent
ms.openlocfilehash: 4794ad098e83677d712af611fc74395e0e5f4e20
ms.sourcegitcommit: 451cfaa24b349393f36ae9d646d4d311a14dd1fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72019460"
---
# <a name="azure-stack-servicing-policy"></a>Azure Stack hizmet ilkesi

*İçin geçerlidir: Azure Stack tümleşik sistemleri*

Bu makalede, Azure Stack tümleşik sistemlere yönelik bakım ilkesi, sisteminizi desteklenen bir durumda tutmak için yapmanız gerekenler ve nasıl destek alınacağı açıklanmaktadır.

## <a name="keep-your-system-under-support"></a>Sisteminizi destek altında tutun

Destek almaya devam etmek için Azure Stack güncelleştirmeleri güncel tutmanız gerekir.

Azure Stack örneğinizin desteklenen bir durumda kalması için, örneğin en son yayınlanan güncelleştirme sürümünü çalıştırması veya önceki iki güncelleştirme sürümünden herhangi birini çalıştırması gerekir.

Düzeltmeler önemli güncelleştirme sürümleri olarak kabul edilmez. Azure Stack örneğiniz *ikiden fazla güncelleştirme*tarafından arkasındaysa uyumsuz olarak kabul edilir. Destek almak için en az desteklenen sürüme güncelleştirmeniz gerekir.

Örneğin, en son kullanılabilir güncelleştirme sürümü 1904 ise ve önceki iki güncelleştirme paketi 1903 ve 1902 sürümleriyse, her iki 1902 ve 1903 de destek içinde kalır. Ancak, 1901 destek dışı. Bir ay veya iki sürüm olmadığında ilke true olarak tutulur. Örneğin, geçerli sürüm 1807 ise ve 1806 sürümü yoksa, önceki iki güncelleştirme paketi olan 1805 ve 1804 destek içinde kalır.

Microsoft yazılım güncelleştirme paketleri toplu değildir ve önceki güncelleştirme paketini veya düzeltmeyi önkoşul olarak gerektirir. Bir veya daha fazla güncelleştirmeyi erteistemediğinize karar verirseniz, en son sürüme ulaşmak istiyorsanız genel çalışma zamanını göz önünde bulundurun.

## <a name="get-support"></a>Destek alın

Azure Stack, Azure ile aynı destek sürecini izler. Kurumsal müşteriler, [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)bölümünde açıklanan işlemi izleyebilir. Bir bulut çözümü sağlayıcısı (CSP) müşterisiyseniz, destek için CSP 'nize başvurun. Daha fazla bilgi için bkz. [Azure desteği SSS](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack güncelleştirmelerini yönetme](azure-stack-updates.md)
