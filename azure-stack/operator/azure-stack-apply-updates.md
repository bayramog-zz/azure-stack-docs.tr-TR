---
title: Azure Stack için bir özgün ekipman üreticisi (OEM) güncelleştirmesi uygulayın | Microsoft Docs
description: Azure Stack için bir özgün ekipman üreticisi (OEM) güncelleştirmesi uygulamayı öğrenin.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/03/2019
ms.author: justinha
ms.lastreviewed: 09/03/2019
ms.reviewer: ppacent
ms.openlocfilehash: 95b7d202d686edd123b552eaa95aa2b8c74c497a
ms.sourcegitcommit: 314fd74caf356b157583d38d2b8b1dee30408b7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2019
ms.locfileid: "70234982"
---
# <a name="apply-updates-in-azure-stack"></a>Azure Stack güncelleştirmeleri uygulama

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Azure Stack **güncelleştirme dikey penceresini kullanarak güncelleştirmeyi uygulayabilirsiniz** . Bu makalede güncelleştirme işlemini güncelleştirme, izleme ve sorun giderme adımları adım adım açıklanmaktadır. Güncelleştirme bilgilerini görüntülemek, güncelleştirmeleri yüklemek, güncelleştirme ilerlemesini izlemek, güncelleştirme geçmişini gözden geçirmek ve geçerli OEM paketi sürümünü görüntülemek için güncelleştirme dikey penceresini kullanabilirsiniz.

Güncelleştirmeleri yönetici portalından yönetebilirsiniz. Panodaki **güncelleştirmeleri** kullanarak şunları yapabilirsiniz:

-   Geçerli sürüm gibi önemli bilgileri görüntüleyin.
-   Güncelleştirmeleri yükler ve ilerlemeyi izleyin.
-   Daha önce yüklenmiş güncelleştirmelerin güncelleştirme geçmişini gözden geçirin.
-   Bulutun geçerli OEM paketi sürümünü görüntüleyin.

## <a name="determine-the-current-version"></a>Geçerli sürümü belirleme

Güncel Azure Stack sürümünü **güncelleştirmeler** dikey penceresinde görüntüleyebilirsiniz. Şunu açmak için:

1.  Azure Stack Yönetici portalı ' nı açın.

2.  **Pano**seçeneğini belirleyin. **Güncelleştirmeler** dikey penceresinde, geçerli sürüm listelenir.

    ![Varsayılan panoda güncelleştirme kutucuğu](./media/azure-stack-update-apply/image1.png)

    Örneğin, bu görüntüde sürüm 1.1903.0.35.

## <a name="install-updates-and-monitor-progress"></a>Güncelleştirmeleri yükler ve ilerlemeyi izleme

1.  Azure Stack Yönetici portalı ' nı açın.

2.  **Pano** > **güncelleştirmesi**' ni seçin.

3.  Uygulamak istediğiniz kullanılabilir güncelleştirmeyi seçin. **Kullanılabilir**olarak işaretlenmiş bir güncelleştirmeniz yoksa [güncelleştirme paketini hazırlamanız](azure-stack-update-prepare-package.md)gerekir.

4.  **Şimdi Güncelleştir**' i seçin.

    ![Azure Stack güncelleştirme çalıştırması ayrıntıları](./media/azure-stack-update-apply/image2.png)

5.  Güncelleştirme işlemi Azure Stack içindeki çeşitli alt sistemler arasında yineleme yaparken üst düzey durumu görüntüleyebilirsiniz. Örnek alt sistemler fiziksel Konakları, Service Fabric, altyapı sanal makinelerini ve hem yönetici hem de Kullanıcı portallarını sağlayan hizmetleri içerir. Güncelleştirme işlemi boyunca güncelleştirme kaynak sağlayıcısı güncelleştirme hakkında, başarılı olan adımların sayısı ve devam eden sayı gibi ek ayrıntıları raporlar.

6.  Tam günlükleri indirmek için güncelleştirme çalıştırması ayrıntıları dikey penceresinden **indirme Özeti** ' ni seçin.

    Güncelleştirmeyi izlerken bir sorunla karşılaşırsanız, bir Azure Stack güncelleştirme çalıştırmasının ilerlemesini izlemek ve başarısız bir güncelleştirme çalıştırmasını son başarılı adımla devam [etmek için Azure Stack](https://docs.microsoft.com/azure-stack/operator/azure-stack-privileged-endpoint) portalı kullanılamaz hale gelmelidir. Yönergeler için bkz. [PowerShell kullanarak Azure Stack güncelleştirmeleri izleme](azure-stack-update-monitor.md).

    ![Azure Stack güncelleştirme çalıştırması ayrıntıları](./media/azure-stack-update-apply/image3.png)

7.  Güncelleştirme kaynak sağlayıcısı tamamlandığında, güncelleştirme işleminin tamamlandığını ve ne kadar sürdüğünü göstermek için **başarılı** bir onay sağlar. Buradan, filtre kullanarak tüm güncelleştirmeler, kullanılabilir güncelleştirmeler veya yüklü güncelleştirmeler hakkında bilgi görüntüleyebilirsiniz.

    ![Azure-Stack-Güncelleştir-Uygula](./media/azure-stack-update-apply/image4.png)

    Güncelleştirme başarısız olursa, **güncelleştirme** dikey penceresinde **dikkat edilmesi gerekir**. Güncelleştirmenin başarısız olduğu üst düzey bir durum almak için **tam günlükleri indir** seçeneğini kullanın. Azure Stack log koleksiyonu, tanılama ve sorun gidermeye yardımcı olur.

## <a name="review-update-history"></a>Güncelleştirme geçmişini gözden geçirme

1.  Yönetici portalını açın.

2.  **Pano**seçeneğini belirleyin. **Güncelleştir**' i seçin.

3.  **Güncelleştirme geçmişi**' ni seçin.

    ![Azure Stack güncelleştirme geçmişi](./media/azure-stack-update-apply/image7.png)

## <a name="next-steps"></a>Sonraki adımlar

-   [Azure Stack güncelleştirmelerini yönetmeye genel bakış](https://docs.microsoft.com/azure-stack/operator/azure-stack-updates)  
-   [Azure Stack hizmet ilkesi](https://docs.microsoft.com/azure-stack/operator/azure-stack-servicing-policy)  
