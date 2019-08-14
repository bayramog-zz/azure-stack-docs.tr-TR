---
title: Azure Stack 'de kota türleri | Microsoft Docs
description: Azure Stack hizmet ve kaynaklar için kullanılabilir farklı kota türlerini görüntüleyin ve düzenleyin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/13/2019
ms.author: sethm
ms.reviewer: xiaofmao
ms.lastreviewed: 12/07/2018
ms.openlocfilehash: df0bee87f512f00ea57fc11492f824730dbf9b42
ms.sourcegitcommit: aefcf9c61bd8089a0aaa569af7643e5e15f4947c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68991772"
---
# <a name="quota-types-in-azure-stack"></a>Azure Stack kota türleri

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

[Kotalar](azure-stack-plan-offer-quota-overview.md#plans) , bir Kullanıcı aboneliğinin sağlayabileceği veya tüketebileceği kaynakların sınırlarını tanımlar. Örneğin bir kullanıcının en fazla beş VM oluşturmasına izin veren bir kota oluşturabilirsiniz. Her kaynak için farklı kota türleri kullanabilirsiniz.

> [!IMPORTANT]
> Yeni kotaların Kullanıcı portalında veya değiştirilen bir kota zorlanmadan önce kullanılabilmesi iki saate kadar sürebilir.

## <a name="compute-quota-types"></a>İşlem kotası türleri

| **Tür** | **Varsayılan değer** | **Açıklama** |
| --- | --- | --- |
| Maksimum sanal makine sayısı | 50 | Bir aboneliğin bu konumda oluşturabileceğiniz maksimum sanal makine sayısı. |
| Maksimum sanal makine çekirdeği sayısı | 100 | Bir aboneliğin bu konumda oluşturabileceğiniz en fazla çekirdek sayısı (örneğin, bir a3 VM 'nin dört çekirdeği vardır). |
| Maksimum kullanılabilirlik kümesi sayısı | 10 | Bu konumda oluşturulabilecek maksimum kullanılabilirlik kümesi sayısı. |
| Maksimum sanal makine ölçek kümesi sayısı | 100 | Bu konumda oluşturulabilecek maksimum sanal makine ölçek kümesi sayısı. |
| Standart yönetilen diskin maksimum kapasitesi (GB cinsinden) | 2048 | Bu konumda oluşturulabilecek Standart yönetilen disklerin maksimum kapasitesi. |
| Premium yönetilen diskin maksimum kapasitesi (GB cinsinden) | 2048 | Bu konumda oluşturulabilecek Premium yönetilen disklerin maksimum kapasitesi. |

> [!NOTE]  
> Yönetilmeyen disk (sayfa Blobları) için maksimum kapasite yönetilen disk kotasından ayrıdır. Bu değeri, **depolama kotalarına**ayarlayabilirsiniz.

## <a name="storage-quota-types"></a>Depolama kotası türleri

| **Öğesi** | **Varsayılan değer** | **Açıklama** |
| --- | --- | --- |
| Maksimum kapasite (GB) |2048 |Bu konumdaki bir abonelik tarafından tüketilen toplam depolama kapasitesi (blob 'lar ve tüm ilişkili anlık görüntüler, tablolar, kuyruklar dahil). |
| Toplam depolama hesabı sayısı |20 |Bu konumda, bir aboneliğin oluşturabileceğiniz en fazla depolama hesabı sayısı. |

> [!NOTE]  
> Yönetilen disklerin maksimum kapasitesi toplam depolama kotasından ayrıdır. Bu değeri, **işlem kotalarına**ayarlayabilirsiniz.

## <a name="network-quota-types"></a>Ağ kotası türleri

| **Öğesi** | **Varsayılan değer** | **Açıklama** |
| --- | --- | --- |
| En fazla genel IP |50 |Bir aboneliğin bu konumda oluşturabileceğiniz en fazla genel IP adresi sayısı. |
| En fazla sanal ağ |50 |Bu konumda, bir aboneliğin oluşturabileceğiniz en fazla sanal ağ sayısı. |
| Maksimum sanal ağ geçitleri |1\. |Bir aboneliğin bu konumda oluşturabileceğiniz en fazla sanal ağ geçidi sayısı (VPN ağ geçidi). |
| En yüksek ağ bağlantısı |2 |Bu konumdaki tüm sanal ağ geçitleri genelinde bir aboneliğin oluşturabileceğiniz en fazla ağ bağlantısı sayısı (noktadan noktaya veya siteden siteye). |
| Maksimum yük dengeleyiciler |50 |Bir aboneliğin bu konumda oluşturabileceğiniz maksimum yük dengeleyici sayısı. |
| En fazla NIC |100 |Bir aboneliğin bu konumda oluşturabileceğiniz en fazla ağ arabirimi sayısı. |
| En yüksek ağ güvenlik grupları |50 |Bir aboneliğin bu konumda oluşturabileceğiniz en fazla ağ güvenlik grubu sayısı. |

## <a name="view-an-existing-quota"></a>Mevcut kotayı görüntüleme

Mevcut kotayı görüntülemenin iki farklı yolu vardır:

### <a name="plans"></a>Planlar

1. Yönetici portalının sol gezinti bölmesinde **planlar**' ı seçin.
2. Ayrıntılarını görüntülemek istediğiniz planı, adına tıklayarak seçin.
3. Açılan dikey pencerede **Hizmetler ve Kotalar**' ı seçin.
4. **Ad** sütununda tıklatarak görmek istediğiniz kotayı seçin.

    [![Kotalar](media/azure-stack-quota-types/quotas1sm.png "Kotaları görüntüle")](media/azure-stack-quota-types/quotas1.png#lightbox)

### <a name="resource-providers"></a>Kaynak sağlayıcıları

1. Yönetici portalının varsayılan panosunda **kaynak sağlayıcıları** kutucuğunu bulun.
2. **İşlem**, **ağ**veya **depolama**gibi, görüntülemek istediğiniz kotayı içeren hizmeti seçin.
3. **Kotalar**' ı seçin ve ardından görüntülemek istediğiniz kotayı seçin.

## <a name="edit-a-quota"></a>Kotayı düzenleme

Kotayı düzenlemenin iki farklı yolu vardır:

### <a name="edit-a-plan"></a>Planı düzenleme

1. Yönetici portalının sol gezinti bölmesinde **planlar**' ı seçin.
2. Kota düzenlemek istediğiniz planı seçin ve adına tıklayın.
3. Açılan dikey pencerede **Hizmetler ve Kotalar**' ı seçin.
4. **Ad** sütununda tıklatarak düzenlemek istediğiniz kotayı seçin.
    [![Kotalar](media/azure-stack-quota-types/quotas1sm.png "Kotaları görüntüle")](media/azure-stack-quota-types/quotas1.png#lightbox)

5. Açılan dikey pencerede, **işlem Içinde Düzenle**, **ağda Düzenle**veya **depolamada Düzenle**' yi seçin.
    ![Kotalar](media/azure-stack-quota-types/quotas3.png "Kotaları görüntüle")

Alternatif olarak, kotayı düzenlemek için bu yordamı izleyebilirsiniz:

1. Yönetici portalının varsayılan panosunda **kaynak sağlayıcıları** kutucuğunu bulun.
2. **İşlem**, **ağ**veya **depolama**gibi, değiştirmek istediğiniz kotayı içeren hizmeti seçin.
3. Ardından, **Kotalar**' ı seçin ve ardından değiştirmek istediğiniz kotayı seçin.
4. **Depolama kotaları ayarlama**, **işlem kotalarını**ayarlama veya **ağ kotalarını ayarlama** bölmesini (düzenlemeyi seçtiğiniz kotanın türüne göre), değerleri düzenleyin ve ardından **Kaydet**' i seçin.

### <a name="edit-original-configuration"></a>Özgün yapılandırmayı Düzenle
  
Bir [eklenti planı kullanmak](create-add-on-plan.md)yerine, kotanın orijinal yapılandırmasını düzenlemeyi seçebilirsiniz. Bir kotayı düzenlediğinizde, yeni yapılandırma bu kotayı kullanan tüm planlara ve bu planları kullanan tüm mevcut abonelikleri otomatik olarak global olarak uygular. Bir kota düzenlemesi, bir kullanıcının abone olmayı seçtiği değiştirilmiş bir kota sağlamak için bir eklenti planı kullandığınız saatten farklıdır.

Kotanın yeni değerleri, değiştirilen kotayı kullanan tüm planlara ve bu planları kullanan tüm mevcut aboneliklere Global olarak uygulanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Planlar, teklifler ve Kotalar hakkında daha fazla bilgi edinin.](azure-stack-plan-offer-quota-overview.md)
- [Bir plan oluştururken kotalar oluşturun.](azure-stack-create-plan.md)
