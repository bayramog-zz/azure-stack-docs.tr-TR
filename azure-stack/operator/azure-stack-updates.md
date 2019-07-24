---
title: Azure Stack genel bakış 'da güncelleştirmeleri yönetme | Microsoft Docs
description: Azure Stack tümleşik sistemler için güncelleştirme yönetimi hakkında bilgi edinin.
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
ms.date: 07/23/2019
ms.author: mabrigg
ms.lastreviewed: 04/04/2019
ms.reviewer: justini
ms.openlocfilehash: a5fb41e63a8cd9e5f57e4476b1b7c23738f116bb
ms.sourcegitcommit: b95983e6e954e772ca5267304cfe6a0dab1cfcab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68418008"
---
# <a name="manage-updates-in-azure-stack-overview"></a>Azure Stack genel bakış 'da güncelleştirmeleri yönetme

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Azure Stack tümleşik sistemler için Microsoft Update paketleri genellikle her ay serbest. Kuruluşunuza ulaşacak güncelleştirme bildirimleri sağlamak için, özgün ekipman üreticinizden (OEM) belirli bildirim işlemleri hakkında sorun. Bu belge kitaplığını, etkin destek 'teki yayınlar hakkında bilgi için **genel bakış** > **sürüm notları** altına da iade edebilirsiniz.

Microsoft yazılım güncelleştirmelerinin her sürümü tek bir güncelleştirme paketi olarak paketlenmiştir. Azure Stack operatörü olarak, Azure Stack yönetim portalından güncelleştirme paketlerinin yükleme ilerlemesini içeri aktarabilir, yükleyebilir ve izleyebilirsiniz.

Ayrıca, OEM satıcınız sürücü ve bellenim güncelleştirmeleri gibi güncelleştirmeleri de serbest bırakacaktır. Bu güncelleştirmeler satıcıya göre ayrı paketler olarak teslim edilirken, bazıları Microsoft 'tan güncelleştirme paketleriyle aynı şekilde alınır, yüklenir ve yönetilir.

Sisteminizi destek altında tutmak için Azure Stack belirli bir sürüm düzeyine güncel tutmanız gerekir. [Azure Stack bakım ilkesini](azure-stack-servicing-policy.md)gözden geçirdiğinizden emin olun.

> [!NOTE]
> Azure Stack güncelleştirme paketlerini Azure Stack Geliştirme Seti (ASDK) uygulayamazsınız. Güncelleştirme paketleri tümleşik sistemler için tasarlanmıştır. Bilgi için bkz. [ASDK](../asdk/asdk-redeploy.md)'Yi yeniden dağıtma.

## <a name="the-update-resource-provider"></a>Güncelleştirme kaynak sağlayıcısı

Azure Stack, Microsoft yazılım güncelleştirmelerinin uygulamasını işleyen bir güncelleştirme kaynak sağlayıcısı içerir. Bu sağlayıcı güncelleştirmelerin tüm fiziksel konaklar, Service Fabric uygulamalar ve çalışma zamanları, tüm altyapı sanal makineleri ve bunların ilişkili hizmetleri üzerinde uygulandığını denetler.

Güncelleştirmeler yüklendikten sonra, güncelleştirme işlemi Azure Stack çeşitli alt sistemleri (örneğin, fiziksel konaklar ve altyapı sanal makineleri) hedeflediğinden, üst düzey durumunu görüntüleyebilirsiniz.

## <a name="plan-for-updates"></a>Güncelleştirmeleri planlayın

Her türlü bakım işlemini kullanıcılara bilgilendirmenizi ve mümkünse olağan bakım pencerelerini iş dışı saatlerde zamanlamanızı kesinlikle öneririz. Bakım işlemleri, hem kiracı iş yüklerini hem de Portal işlemlerini etkileyebilir.

Bakım pencereniz için planlama yaparken, Microsoft 'tan yayınlanan ve ilgili sürüm notunda çağırılan belirli güncelleştirme paketi türünü gözden geçirmeniz önemlidir. Ara sıra sonrasında her güncelleştirme paketi, karşılık gelen bir tür, **tam** veya **hızlı**olacaktır. Tam güncelleştirme paketleri, ölçek birimindeki fiziksel ana bilgisayar işletim sistemlerini güncelleştirir ve daha büyük bir bakım penceresi gerektirir. Hızlı güncelleştirme paketlerinin kapsamı vardır ve temel alınan fiziksel ana bilgisayar işletim sistemlerini güncelleştirmez.

Bu güncelleştirme yüklemesi başlamadan önce çalıştırması [Test AzureStack](azure-stack-diagnostic-test.md) bulunan tüm çalışma sorunlarını çözün ve Azure Stack durumunu doğrulamak için aşağıdaki parametreleri, tüm uyarılar ve hatalar dahil olmak üzere. Ayrıca etkin Uyarıları gözden geçirin ve eylemi gerektiren tüm çözümleyin.  

```powershell
Test-AzureStack -Group UpdateReadiness
```

## <a name="using-the-update-tile-to-manage-updates"></a>Güncelleştirmeleri yönetmek için güncelleştirme kutucuğunu kullanma

Güncelleştirmeleri yönetici portalından yönetirsiniz. Azure Stack işleci olarak, panoda **güncelleştirme** kutucuğunu kullanarak şunları yapabilirsiniz:

- Geçerli sürüm gibi önemli bilgileri görüntüleyin.
- Güncelleştirmeleri yükler ve ilerlemeyi izleyin.
- Daha önce yüklenmiş güncelleştirmelerin güncelleştirme geçmişini gözden geçirin.
- Bulutun geçerli OEM paketi sürümünü görüntüleyin.

## <a name="determine-the-current-version"></a>Geçerli sürümü belirleme

Güncel Azure Stack sürümünü **güncelleştirme** kutucuğunda görüntüleyebilirsiniz. Kutucuğu açmak için:

1. Azure Stack Yönetici portalı ' nı açın.
2. **Pano**seçeneğini belirleyin. **Güncelleştirme** kutucuğunda geçerli sürüm listelenir.

    ![Varsayılan panoda güncelleştirme kutucuğu](./media/azure-stack-updates/image1.png)

    Örneğin, bu görüntüde sürüm 1.1903.0.35.

## <a name="install-updates-and-monitor-progress"></a>Güncelleştirmeleri yükler ve ilerlemeyi izleme

1. Azure Stack Yönetici portalı ' nı açın.
2. **Pano**seçeneğini belirleyin. **Güncelleştirme** kutucuğunu seçin.
3. **Şimdi Güncelleştir**' i seçin.

    ![Azure Stack güncelleştirme çalıştırması ayrıntıları](media/azure-stack-updates/azure-stack-update-button.png)

4. Güncelleştirme işlemi Azure Stack içindeki çeşitli alt sistemler arasında yineleme yaparken üst düzey durumu görüntüleyebilirsiniz. Örnek alt sistemler fiziksel Konakları, Service Fabric, altyapı sanal makinelerini ve hem yönetici hem de Kullanıcı portallarını sağlayan hizmetleri içerir. Güncelleştirme işlemi boyunca güncelleştirme kaynak sağlayıcısı, güncelleştirme hakkında, başarılı olan adımların sayısı ve devam eden sayı gibi ek ayrıntıları raporlar.

5. Tam günlükleri indirmek için güncelleştirme çalıştırması ayrıntıları dikey penceresinden **tam günlükleri indir** ' i seçin.

    ![Azure Stack güncelleştirme çalıştırması ayrıntıları](media/azure-stack-updates/update-run-details.png)

6. Güncelleştirme kaynak sağlayıcısı tamamlandığında, güncelleştirme işleminin tamamlandığını ve ne kadar sürdüğünü göstermek için **başarılı** bir onay sağlar. Buradan, filtre kullanarak tüm güncelleştirmeler, kullanılabilir güncelleştirmeler veya yüklü güncelleştirmeler hakkında bilgi görüntüleyebilirsiniz.

    ![Azure Stack güncelleştirme çalıştırması ayrıntıları başarısı](media/azure-stack-updates/update-success.png)

   Güncelleştirme başarısız olursa, **güncelleştirme** kutucuk raporlarında **dikkat edilmesi gerekir**. Güncelleştirmenin başarısız olduğu üst düzey bir durum almak için **tam günlükleri indir** seçeneğini kullanın. Azure Stack log koleksiyonu, tanılamayı ve sorun gidermeyi kolaylaştırmaya yardımcı olur.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack hizmet ilkesi](azure-stack-servicing-policy.md) 
- [Azure Stack 'de bölge yönetimi](azure-stack-region-management.md)
