---
title: Azure Stack hizmet İlkesi | Microsoft Docs
description: İlke ve tümleşik bir sistem desteklenen bir durumda tutmak nasıl hizmet Azure Stack hakkında bilgi edinin.
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
ms.date: 06/26/2019
ms.author: sethm
ms.reviewer: harik
ms.lastreviewed: 01/11/2019
ms.openlocfilehash: 8965ad94d1c8576e437e85a9714997f842f7dd50
ms.sourcegitcommit: bcaad8b7db2ea596018d973cb29283d8c6daebfb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67419456"
---
# <a name="azure-stack-servicing-policy"></a>Azure Stack hizmet İlkesi

Bu makalede, Azure Stack tümleşik sistemleri ve desteklenen bir duruma sisteminizi tutmak için yapmanız gerekenlere hizmet İlkesi açıklanır.

## <a name="download-update-packages-for-integrated-systems"></a>Tümleşik sistemler için güncelleştirme paketleri indirin

Microsoft, hem tam aylık güncelleştirme paketleri, aynı zamanda Düzeltme Paketleri belirli sorunları gidermek üzere serbest bırakır.

Aylık güncelleştirme paketleri, bir güvenli Azure uç noktası barındırılır. Bunları kullanarak el ile indirebilirsiniz [Azure Stack güncelleştirmeleri yükleyici aracı](https://aka.ms/azurestackupdatedownload). Ölçek biriminize bağlıysa, güncelleştirmeyi otomatik olarak Yönetici portalı'nda görünür **güncelleştirme kullanılabilir**. Tam, aylık güncelleştirme paketlerini de her sürümde belgelenmiştir. Her sürüm hakkında daha fazla bilgi için herhangi bir yayın tıklayabilirsiniz [güncelleştirme paketi yayın temposudur](#update-package-release-cadence) bu makalenin.

Düzeltme güncelleştirme paketleri, aynı güvenli Azure uç noktası, barındırılır. Ekli bağlantılar her ilgili düzeltme KB makalelerinin kullanarak bunları yükleyebilirsiniz; Örneğin, [Azure Stack düzeltme 1.1809.12.114](https://support.microsoft.com/help/4481548/azure-stack-hotfix-1-1809-12-114). Benzer şekilde tam, aylık güncelleştirme paketleri, Azure Stack operatörleri .xml, .bin ve .exe dosyaları indirebilir ve bunları yordamı kullanarak içeri [güncelleştirmelerini Azure Stack'te](azure-stack-apply-updates.md). Azure Stack operatörleri bağlı ölçek birimleri ile otomatik olarak ileti Yönetici portalı'nda görünür düzeltmeleri görürsünüz **güncelleştirme kullanılabilir**.

Ölçek biriminize bağlı değil ve her düzeltme yayın hakkında bildirim almak istiyorsanız, abone [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss) veya [ATOM](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom) her sürümde belirtildiği akışı.  

## <a name="update-package-types"></a>Güncelleştirme paketi türleri

Tümleşik sistemler için güncelleştirme paketleri iki tür vardır:

- **Microsoft yazılım güncelleştirmeleri**. Microsoft, Microsoft yazılım güncelleştirme paketleri için uçtan uca hizmet yaşam döngüsü sorumludur. Bu paketlerin en son Windows Server güvenlik güncelleştirmeleri, güvenlikle ilgili olmayan güncelleştirmeleri ve Azure Stack özellik güncelleştirmeleri içerebilir. Bu güncelleştirme paketleri doğrudan Microsoft'tan indirebilirsiniz.

- **OEM donanım satıcısı tarafından sağlanan güncelleştirmeleri**. Azure Stack donanım iş ortakları, (yönergeler dahil olmak üzere) uçtan uca hizmet ömrü donanım ile ilgili bellenim ve sürücü güncelleştirme paketleri sorumludur. Ayrıca, Azure Stack donanım iş ortakları kendi ve yönergeler için tüm yazılım ve donanım yaşam döngüsü konaktaki bir donanım Bakımı. OEM donanım satıcınıza bunlar barındıran güncelleştirme paketleri kendi indirme sitesinde.

## <a name="update-package-release-cadence"></a>Güncelleştirme paketi yayın sıklığı

Microsoft, yazılım güncelleştirme paketleri aylık temposu serbest bırakmak bekliyor. Ancak, bir ay içindeki birden çok veya hiçbir güncelleştirme sürümleri mümkündür. OEM donanım satıcıları güncellemeleri gerektiğinde olarak bırakın.

Planlama ve güncelleştirmeleri yönetme ve geçerli sürümünüzde belirleme belgelere [yönetme genel bakış güncelleştirmeleri](azure-stack-updates.md).

Belirli bir güncelleştirme hakkında daha fazla bilgi için indirin, güncelleştirme sürüm notları için bkz dahil olmak üzere:

- [Azure Stack 1906 güncelleştirme](azure-stack-release-notes-1906.md)
- [Azure Stack 1905 güncelleştirme](azure-stack-release-notes-1905.md)
- [Azure Stack 1904 güncelleştirme](azure-stack-release-notes-1904.md)
- [Azure Stack 1903 güncelleştirme](azure-stack-update-1903.md)

## <a name="hotfixes"></a>Düzeltmeler

Bazen, Microsoft bu adrese önleyici veya zamana duyarlı genellikle belirli bir sorunun Azure Stack için düzeltmeler sağlar.  Her bir düzeltme sorun, nedeni ve çözümü ayrıntıları karşılık gelen bir Microsoft Bilgi Bankası makalesiyle serbest bırakılır.

Daha fazla bilgi ve bağlantılar belirli düzeltmeler için önceki bölümdeki sürüm notları bağlantılara bakın.

Düzeltmeleri indirilir ve Azure Stack için yalnızca normal tam güncelleştirme paketleri gibi yüklü. Ancak, tam güncelleştirme, dakikalar içinde düzeltmeleri yükleyebilirsiniz. Azure Stack operatörlerinin bakım pencereleri düzeltmeleri yüklerken ayarlamanızı öneririz. Böylece düzeltmeyi uygulanıp uygulanmadığını kolayca belirleyebilir düzeltmeleri Azure Stack bulutunuza sürümünü güncelleştirin. Her Azure Stack, yine destek sürümü için ayrı bir düzeltme sağlanır. Belirli bir yineleme için her düzeltmesi toplanır ve aynı sürüme önceki güncelleştirmeleri içerir. Daha fazla ilgili Bilgi Bankası'nda belirli bir düzeltme uygulanabilirliğini hakkında makalesi. Önceki bölümde sürüm notları bağlantılara bakın.  

## <a name="keep-your-system-under-support"></a>Sisteminizi desteği altında tutun

Destek almaya devam etmek için geçerli Azure Stack dağıtımınıza tutmalısınız. Güncelleştirmeleri erteleme İlkesi: Azure Stack dağıtımınıza desteği kalmasına en kısa süre önce yayımlanan bir güncelleştirme sürümünü çalıştırana veya olmalıdır iki önceki güncelleştirme sürümleri birini çalıştırın. Düzeltmeleri önemli güncelleştirme sürümleri olarak kabul edilmez. Azure Stack bulutunuza arkasında tarafından ise *ikiden fazla güncelleştirmeleri*, uyumsuz olarak kabul edilir ve en az destek almak için desteklenen en düşük sürüme güncelleştirmeniz gerekir.

Örneğin, 1904 en son kullanılabilir güncelleştirme sürümüdür ve iki önceki güncelleştirme paketlerini 1903 ve 1902 sürümleri olan 1902 hem 1903 desteği kalır. Ancak, destek kapsamı dışında 1901 olur. İlke olduğunda yayın ayda bir veya iki için geçerlidir. Örneğin, 1807 geçerli sürümdür ve 1806 yayın vardı, önceki iki güncelleştirme paketlerini 1805 ve 1804 desteği kalır.

Microsoft yazılım güncelleştirme paketleri, toplu olmayan ve önceki güncelleştirme paketini veya düzeltmeyi bir önkoşul olarak gerekli. Bir veya daha fazla güncelleştirmelerini erteleme karar verirseniz, genel çalışma zamanının en son sürüme almak istiyorsanız göz önünde bulundurun.

## <a name="get-support"></a>Destek alın

Azure Stack, aynı Azure destek süreci izler. Kurumsal müşteriler, açıklanan işlemi izleyebilirsiniz [Azure destek isteği oluşturmak nasıl](/azure/azure-supportability/how-to-create-azure-support-request). Bir müşteri bir bulut hizmeti sağlayıcısı (CSP) ise, CSP için desteğe başvurun.  Daha fazla bilgi için [Azure desteği SSS](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack güncelleştirmelerini yönetme](azure-stack-updates.md)