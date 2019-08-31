---
title: Hizmet ilkesi Azure Stack | Microsoft Docs
description: Azure Stack bakım ilkesi hakkında bilgi edinin ve tümleşik bir sistemin desteklenen bir durumda tutulması.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: caac3d2f-11cc-4ff2-82d6-52b58fee4c39
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2019
ms.author: sethm
ms.reviewer: harik
ms.lastreviewed: 01/11/2019
ms.openlocfilehash: 1022ab056157ea1a9bc925d3992a99bd0b395a35
ms.sourcegitcommit: 71d7990a2b21576c44bb2aea13ae2026e9510c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70188086"
---
- **OEM donanım satıcısı tarafından belirtilen güncelleştirmeler**. Azure Stack Donanım ortakları, donanımla ilgili bellenim ve sürücü güncelleştirme paketleri için uçtan uca hizmet yaşam döngüsüyle (rehberlik dahil) sorumludur. Bunlara ek olarak, Azure Stack donanım iş ortakları, donanım yaşam döngüsü konağındaki tüm yazılım ve donanımların kılavuzunu korur. OEM donanım satıcısı, bu güncelleştirme paketlerini kendi indirme sitelerinde barındırır.

## <a name="update-package-release-cadence"></a>Güncelleştirme paketi sürümü temposunda

Microsoft, aylık bir temposunda yazılım güncelleştirme paketlerini serbest bırakmaya bekliyor. Ancak, bir ayda birden çok veya güncelleştirme yayını olması mümkündür. OEM donanım satıcıları güncelleştirmelerini gerektiği şekilde serbest bırakabilir.

Güncelleştirmelerin nasıl planlanacağı ve yönetileceği hakkında bilgi edinin ve [güncelleştirmeleri yönetme](azure-stack-updates.md)bölümünde geçerli sürümünüzü nasıl belirleyebileceğinizi öğrenin.

Nasıl indirileceği dahil olmak üzere belirli bir güncelleştirme hakkında daha fazla bilgi için, bu güncelleştirme için sürüm notlarına bakın:

- [Azure Stack 1908 güncelleştirmesi](azure-stack-release-notes-1908.md)
- [Azure Stack 1907 güncelleştirmesi](azure-stack-release-notes-1907.md)
- [Azure Stack 1906 güncelleştirmesi](azure-stack-release-notes-1906.md)
- [Azure Stack 1905 güncelleştirmesi](azure-stack-release-notes-1905.md)

## <a name="hotfixes"></a>Düzeltmeler

Microsoft, bazen, genellikle koruyucu veya zamana duyarlı belirli bir sorunu gideren Azure Stack düzeltmeler sağlar.  Her bir düzeltme, sorunun, nedenin ve çözümlemenin ayrıntılarını veren ilgili bir Microsoft Bilgi Bankası makalesiyle birlikte yayımlanır.

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Bu makalede, Azure Stack tümleşik sistemlere yönelik bakım ilkesi, sisteminizi desteklenen bir durumda tutmak için yapmanız gerekenler ve nasıl destek alınacağı açıklanmaktadır.

## <a name="keep-your-system-under-support"></a>Sisteminizi destek altında tutun

Azure Stack örneğinizin desteklenen bir durumda kalması için, örneğin en son yayınlanan güncelleştirme sürümünü çalıştırması veya önceki iki güncelleştirme sürümünden herhangi birini çalıştırması gerekir.

Düzeltmeler önemli güncelleştirme sürümleri olarak kabul edilmez. Azure Stack örneğiniz *ikiden fazla güncelleştirme*tarafından arkasındaysa uyumsuz olarak kabul edilir. Destek almak için en az desteklenen sürüme güncelleştirmeniz gerekir.

Örneğin, en son kullanılabilir güncelleştirme sürümü 1904 ise ve önceki iki güncelleştirme paketi 1903 ve 1902 sürümleriyse, her iki 1902 ve 1903 de destek içinde kalır. Ancak, 1901 destek dışı. Bir ay veya iki sürüm olmadığında ilke true olarak tutulur. Örneğin, geçerli sürüm 1807 ise ve 1806 sürümü yoksa, önceki iki güncelleştirme paketi olan 1805 ve 1804 destek içinde kalır.

Microsoft yazılım güncelleştirme paketleri toplu değildir ve önceki güncelleştirme paketini veya düzeltmeyi önkoşul olarak gerektirir. Bir veya daha fazla güncelleştirmeyi erteistemediğinize karar verirseniz, en son sürüme ulaşmak istiyorsanız genel çalışma zamanını göz önünde bulundurun.

## <a name="get-support"></a>Destek alın

Azure Stack, Azure ile aynı destek sürecini izler. Kurumsal müşteriler, [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)bölümünde açıklanan işlemi izleyebilir. Bir bulut hizmeti sağlayıcısı (CSP) müşterisiyseniz, destek için CSP 'nize başvurun. Daha fazla bilgi için bkz. [Azure desteği SSS](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack güncelleştirmelerini yönetme](https://docs.microsoft.com/azure-stack/operator/azure-stack-updates)
