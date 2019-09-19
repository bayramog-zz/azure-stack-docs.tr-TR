---
title: Azure Stack güncelleştirmeleri yönetme | Microsoft Docs
description: Azure Stack güncelleştirmeleri yönetmeyi öğrenin
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
ms.openlocfilehash: 64e1bd7934b1b3b8c0bc935a920a2e4e188c4bd9
ms.sourcegitcommit: c46d913ebfa4cb6c775c5117ac5c9e87d032a271
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71101210"
---
# <a name="manage-updates-in-azure-stack-overview"></a>Azure Stack genel bakış 'da güncelleştirmeleri yönetme

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Tam ve hızlı güncelleştirmelerin, düzeltmelerin yanı sıra özgün ekipman üreticisi (OEM) tarafından sağlanan sürücü ve bellenim güncelleştirmeleri, tüm yardım Azure Stack güncel tutmaya yardımcı olur. Bu makalede, farklı güncelleştirme türleri, sürümünün ne zaman beklendiğini ve geçerli yayın hakkında daha fazla bilgi bulunacağı açıklanır.

> [!Note]  
> Azure Stack güncelleştirme paketlerini Azure Stack Geliştirme Seti (ASDK) uygulayamazsınız. Güncelleştirme paketleri tümleşik sistemler için tasarlanmıştır. Bilgi için bkz. [ASDK](https://docs.microsoft.com/azure-stack/asdk/asdk-redeploy)'Yi yeniden dağıtma.

## <a name="update-package-types"></a>Güncelleştirme paketi türleri

Tümleşik sistemler için üç tür güncelleştirme paketi vardır:

-   **Yazılım güncelleştirmelerini Azure Stack**. Microsoft, Microsoft yazılım güncelleştirme paketlerinin uçtan uca hizmet yaşam döngüsüyle sorumludur. Bu paketler, en son Windows Server güvenlik güncelleştirmelerini, güvenlik dışı güncelleştirmeleri ve Azure Stack Özellik güncelleştirmelerini içerebilir. Bu güncelleştirme paketlerini doğrudan Microsoft 'tan indirebilirsiniz.

    Her güncelleştirme paketine karşılık gelen **tam** veya **hızlı**bir tür vardır. 
 
    **Tam** güncelleştirme paketleri, ölçek birimindeki fiziksel ana bilgisayar işletim sistemlerini güncelleştirir ve daha büyük bir bakım penceresi gerektirir. 

    **Hızlı** güncelleştirme paketlerinin kapsamı vardır ve temel alınan fiziksel ana bilgisayar işletim sistemlerini güncelleştirmez.

-   **Düzeltmeler Azure Stack**. Microsoft, genellikle koruyucu veya zamana duyarlı belirli bir sorunu ele alan Azure Stack düzeltmeler sağlar. Her bir düzeltme, sorunun, nedenin ve çözümlemenin ayrıntılarını veren ilgili bir Microsoft Bilgi Bankası makalesiyle birlikte yayımlanır. Azure Stack için düzenli tam güncelleştirme paketleri gibi düzeltmeleri indirip yüklersiniz. Düzeltmeler birikimlidir ve birkaç dakika içinde yüklenebilir.

-   **OEM donanımı-satıcı tarafından belirtilen güncelleştirmeler**. Azure Stack Donanım ortakları, donanımla ilgili bellenim ve sürücü güncelleştirme paketleri için uçtan uca hizmet yaşam döngüsüyle (rehberlik dahil) sorumludur. Bunlara ek olarak, Azure Stack donanım iş ortakları, donanım yaşam döngüsü konağındaki tüm yazılım ve donanımların kılavuzunu korur. OEM donanım satıcısı, bu güncelleştirme paketlerini kendi indirme sitelerinde barındırır.

## <a name="when-to-update"></a>Ne zaman güncelleştirilecek

Aşağıdaki temposunda üç güncelleştirme türü yayımlanır:

-   **Yazılım güncelleştirmelerini Azure Stack**. Microsoft, her ay yazılım güncelleştirme paketlerini genellikle yayınlar.

-   **Düzeltmeler Azure Stack**. Düzeltmeler, herhangi bir zamanda yayımlananlar zamana duyarlı yayınlardır.

-   **OEM donanım satıcısı tarafından belirtilen güncelleştirmeler**. OEM donanım satıcıları güncelleştirmelerini gerektiği şekilde serbest bırakabilir.

Destek almaya devam etmek için Azure Stack ortamınızı desteklenen bir Azure Stack yazılım sürümünde saklamanız gerekir. Daha fazla bilgi için bkz. [hizmet ilkesi Azure Stack](azure-stack-update-servicing-policy.md).

## <a name="where-to-get-notice-of-an-update"></a>Bir güncelleştirme bildirimi nereden alınır?

Güncelleştirme bildirimi, Internet bağlantınız ve güncelleştirme türü gibi birkaç faktörde değişiklik gösterir.

- **Microsoft yazılım güncelleştirmeleri ve düzeltmeleri** 

    Microsoft yazılım güncelleştirmeleri ve düzeltmeleri için bir güncelleştirme uyarısı, internet 'e bağlı Azure Stack örneklerine yönelik güncelleştirme dikey penceresinde görüntülenir.

    Örneğiniz bağlı değilse ve her bir düzeltme sürümü hakkında bildirim almak istiyorsanız, [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss) veya [atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom) akışına abone olun.

- **OEM donanım satıcısı tarafından belirtilen güncelleştirmeler**

    OEM güncelleştirmeleri, üreticinize göre değişir. OEM 'nizden uygulanması gereken güncelleştirmelerin olduğunu bilmeniz için, OEM 'niz ile bir iletişim kanalı oluşturmanız gerekecektir. OEM 'Ler ve OEM güncelleştirme süreci hakkında daha fazla bilgi için bkz. [Apply Azure Stack özgün ekipman üreticisi (OEM) güncelleştirmeleri](azure-stack-update-oem.md).

## <a name="update-processes"></a>Güncelleştirme süreçler

Bir güncelleştirme olduğunu öğrendikten sonra, aşağıdaki adımları kullanarak uygulamayı uygulayın.

![Azure Stack güncelleştirme işlemi](./media/azure-stack-updates/azure-stack-update-process.png)

1. **Güncelleştirmeyi planlayın**.

    Kullanıcılarınız üzerinde en az etkisi olması için Azure Stack güncelleştirme işlemini olabildiğince sorunsuz şekilde hazırlayın. Kullanıcılarınıza olası hizmet kesintilerinden sorumlu olarak bildirimde bulunmak ve sonra örneğinizi güncelleştirme için hazırlama adımlarını izleyin. Güncelleştirmeyi planlamak için daha fazla adım için bkz. [plan Azure Stack Update](azure-stack-update-plan.md).

2. **Güncelleştirme paketini karşıya yükleyin ve hazırlayın**.

    İnternet 'e bağlı Azure Stack ortamlarında, Azure Stack yazılım güncelleştirmeleri ve düzeltmeleri otomatik olarak sisteme içeri aktarılır ve güncelleştirme için hazırlanır.

    İnternet bağlantısı kesilen Azure Stack ortamları ve ortamları, zayıf veya aralıklı internet bağlantısı olan ortamlarda, güncelleştirme paketleri Azure Stack Yönetici portalı aracılığıyla Azure Stack depolama alanına içeri aktarılır. Güncelleştirme paketini karşıya yükleme ve hazırlama hakkında daha fazla bilgi için, bkz. [Azure Stack güncelleştirme paketi yükleme ve hazırlama](azure-stack-update-prepare-package.md).

    Tüm OEM güncelleştirme paketleri, Azure Stack sisteminizin internet bağlantısına bakılmaksızın ortamınıza el ile aktarılır. Güncelleştirme paketini içeri ve hazırlamaya yönelik daha fazla adım için, bkz. [Azure Stack güncelleştirme paketi yükleme ve hazırlama](azure-stack-update-prepare-package.md).

3. **Güncelleştirmeyi uygulayın**.

    Azure Stack 'daki **güncelleştirme** dikey penceresini kullanarak güncelleştirmeyi uygulayın. Güncelleştirme sırasında güncelleştirme ilerlemesini izleyin ve sorun giderin. Daha fazla bilgi için bkz. [Azure Stack güncelleştirme uygulama](azure-stack-apply-updates.md).

## <a name="the-update-resource-provider"></a>Güncelleştirme kaynak sağlayıcısı

Azure Stack, Microsoft yazılım güncelleştirmelerinin uygulamasını işleyen bir güncelleştirme kaynak sağlayıcısı içerir. Bu sağlayıcı güncelleştirmelerin tüm fiziksel konaklar, Service Fabric uygulamalar ve çalışma zamanları, tüm altyapı sanal makineleri ve bunların ilişkili hizmetleri üzerinde uygulandığını denetler.

Güncelleştirmeler yüklendikten sonra, güncelleştirme işlemi Azure Stack çeşitli alt sistemleri (örneğin, fiziksel konaklar ve altyapı sanal makineleri) hedeflediğinden, üst düzey durumunu görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Güncelleştirme işlemini başlatmak için [Azure Stack güncelleştirme planı](azure-stack-update-plan.md)' nda bulunan adımları izleyin.
- Azure Stack sürümlerinin DESTEKDE olduğunu öğrenmek için, bkz. [Azure Stack hizmet ilkesi](azure-stack-servicing-policy.md).  
- Güncel ve son güncelleştirmeler hakkında daha fazla bilgi edinmek için [Azure Stack sürüm notlarına](release-notes.md)bakın.
