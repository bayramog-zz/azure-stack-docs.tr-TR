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
ms.date: 07/08/2019
ms.author: sethm
ms.reviewer: harik
ms.lastreviewed: 01/11/2019
ms.openlocfilehash: 5e60a7fc3f01c1aa9697220738265abe0ae9089a
ms.sourcegitcommit: f6ea6daddb92cbf458f9824cd2f8e7e1bda9688e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68494108"
---
# <a name="azure-stack-servicing-policy"></a>Azure Stack hizmet ilkesi

Bu makalede, Azure Stack tümleşik sistemlere yönelik bakım ilkesi ve sisteminizin desteklenen bir durumda tutulması için yapmanız gerekenler açıklanmaktadır.

## <a name="download-update-packages-for-integrated-systems"></a>Tümleşik sistemler için güncelleştirme paketlerini indirin

Microsoft, belirli sorunları gidermek için hem tam aylık güncelleştirme paketlerini hem de düzeltme paketlerini yayımlayacaktır.

Aylık güncelleştirme paketleri güvenli bir Azure uç noktasında barındırılır. [Azure Stack Updates Downloader aracını](https://aka.ms/azurestackupdatedownload)kullanarak bunları el ile indirebilirsiniz. Ölçek biriminiz bağlıysa, güncelleştirme **kullanılabilir**olarak yönetici portalında otomatik olarak görünür. Tam, aylık güncelleştirme paketleri her sürümde iyi belgelenmiştir. Her sürüm hakkında daha fazla bilgi için, bu makalenin [güncelleştirme paketi sürümü temposunda](#update-package-release-cadence) bölümünde herhangi bir yayına tıklayabilirsiniz.

Düzeltme güncelleştirme paketleri aynı güvenli Azure uç noktasında barındırılır. İlgili düzeltme KB makalelerinin her birinde ekli bağlantıları kullanarak bunları indirebilirsiniz; Örneğin, [1.1809.12.114 düzeltmesini Azure Stack](https://support.microsoft.com/help/4481548/azure-stack-hotfix-1-1809-12-114). Tam, aylık güncelleştirme paketlerine benzer şekilde Azure Stack işleçlerine benzer şekilde. xml,. bin ve. exe dosyalarını indirebilir ve [Azure Stack güncelleştirmeleri uygulama](azure-stack-apply-updates.md)yordamını kullanarak içeri aktarabilirsiniz. Bağlı ölçek birimlerine sahip Azure Stack işleçleri, düzeltmeler **kullanılabilir Ileti güncelleştirmesiyle**birlikte yönetici portalında otomatik olarak görünür.

Ölçek biriminiz bağlı değilse ve her bir düzeltme sürümü hakkında bildirim almak istiyorsanız, her yayında belirtilen [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss) veya [atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom) akışına abone olun.  

## <a name="update-package-types"></a>Güncelleştirme paketi türleri

Tümleşik sistemler için iki tür güncelleştirme paketi vardır:

- **Microsoft yazılım güncelleştirmeleri**. Microsoft, Microsoft yazılım güncelleştirme paketlerinin uçtan uca hizmet yaşam döngüsüyle sorumludur. Bu paketler, en son Windows Server güvenlik güncelleştirmelerini, güvenlik dışı güncelleştirmeleri ve Azure Stack Özellik güncelleştirmelerini içerebilir. Doğrudan Microsoft 'tan ses güncelleştirme paketlerini indirebilirsiniz.

- **OEM donanım satıcısı tarafından belirtilen güncelleştirmeler**. Azure Stack Donanım ortakları, donanımla ilgili bellenim ve sürücü güncelleştirme paketleri için uçtan uca hizmet yaşam döngüsüyle (rehberlik dahil) sorumludur. Bunlara ek olarak, Azure Stack donanım iş ortakları, donanım yaşam döngüsü konağındaki tüm yazılım ve donanımların kılavuzunu korur. OEM donanım satıcısı, bu güncelleştirme paketlerini kendi indirme sitelerinde barındırır.

## <a name="update-package-release-cadence"></a>Güncelleştirme paketi sürümü temposunda

Microsoft, aylık bir temposunda yazılım güncelleştirme paketlerini serbest bırakmaya bekliyor. Ancak, bir ayda birden çok veya güncelleştirme yayını olması mümkündür. OEM donanım satıcıları güncelleştirmelerini gerektiği şekilde serbest bırakabilir.

Güncelleştirmelerin nasıl planlanacağı ve yönetileceği hakkında bilgi edinin ve [güncelleştirmeleri yönetme](azure-stack-updates.md)bölümünde geçerli sürümünüzü nasıl belirleyebileceğinizi öğrenin.

Nasıl indirileceği dahil olmak üzere belirli bir güncelleştirme hakkında daha fazla bilgi için, bu güncelleştirme için sürüm notlarına bakın:

- [Azure Stack 1907 güncelleştirmesi](azure-stack-release-notes-1907.md)
- [Azure Stack 1906 güncelleştirmesi](azure-stack-release-notes-1906.md)
- [Azure Stack 1905 güncelleştirmesi](azure-stack-release-notes-1905.md)
- [Azure Stack 1904 güncelleştirmesi](azure-stack-release-notes-1904.md)

## <a name="hotfixes"></a>Düzeltmeler

Microsoft, bazen, genellikle koruyucu veya zamana duyarlı belirli bir sorunu gideren Azure Stack düzeltmeler sağlar.  Her bir düzeltme, sorunun, nedenin ve çözümlemenin ayrıntılarını veren ilgili bir Microsoft Bilgi Bankası makalesiyle birlikte yayımlanır.

Daha fazla bilgi ve belirli düzeltmelere bağlantılar için önceki bölümde yer alarak sürüm notları bağlantıları bölümüne bakın.

Düzeltmeler, Azure Stack için düzenli tam güncelleştirme paketleri gibi indirilir ve yüklenir. Bununla birlikte, tüm güncelleştirmeler farklı olarak, düzeltmeler dakikalar içinde yüklenebilir. Düzeltmeler yüklenirken Azure Stack işleçleri bakım pencerelerini ayarlamanızı öneririz. Düzeltmeler, düzeltmenin uygulanmış olup olmadığını kolayca belirleyebilmeniz için Azure Stack bulutunuzun sürümünü güncelleştirir. Hala destek içinde olan her bir Azure Stack sürümü için ayrı bir düzeltme sağlanır. Belirli bir yineleme için her bir düzeltme birikimlidir ve aynı sürüme ait önceki güncelleştirmeleri içerir. İlgili Bilgi Bankası makalesinde belirli bir düzeltmenin uygulanabilirliği hakkında daha fazla bilgi edinebilirsiniz. Önceki bölümde yer alarak sürüm notları bağlantıları bölümüne bakın.  

## <a name="keep-your-system-under-support"></a>Sisteminizi destek altında tutun

Destek almaya devam etmek için Azure Stack dağıtımınızı güncel tutmanız gerekir. Güncelleştirmeler için erteleme ilkesi şunlardır: Azure Stack dağıtımınızın destede kalması için en son yayınlanan güncelleştirme sürümünü çalıştırması veya önceki iki güncelleştirme sürümünden birini çalıştırması gerekir. Düzeltmeler önemli güncelleştirme sürümleri olarak kabul edilmez. Azure Stack bulutunuz *ikiden fazla güncelleştirme*tarafından arkasındaysa, uyumsuz olarak kabul edilir ve destek almak için en az desteklenen sürüme güncelleştirmeniz gerekir.

Örneğin, en son kullanılabilir güncelleştirme sürümü 1904 ise ve önceki iki güncelleştirme paketi 1903 ve 1902 sürümleriyse, her iki 1902 ve 1903 de destek içinde kalır. Ancak, 1901 destek dışı. Bir ay veya iki sürüm olmadığında ilke true olarak tutulur. Örneğin, geçerli sürüm 1807 ise ve 1806 sürümü yoksa, önceki iki güncelleştirme paketi olan 1805 ve 1804 destek içinde kalır.

Microsoft yazılım güncelleştirme paketleri toplu değildir ve önceki güncelleştirme paketini veya düzeltmeyi önkoşul olarak gerektirir. Bir veya daha fazla güncelleştirmeyi erteistemediğinize karar verirseniz, en son sürüme ulaşmak istiyorsanız genel çalışma zamanını göz önünde bulundurun.

## <a name="get-support"></a>Destek alın

Azure Stack, Azure ile aynı destek sürecini izler. Kurumsal müşteriler, [Azure destek isteği oluşturma](/azure/azure-supportability/how-to-create-azure-support-request)bölümünde açıklanan işlemi izleyebilir. Bir bulut hizmeti sağlayıcısı (CSP) müşterisiyseniz, destek için CSP 'nize başvurun.  Daha fazla bilgi için bkz. [Azure desteği SSS](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack güncelleştirmelerini yönetme](azure-stack-updates.md)