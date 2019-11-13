---
title: Azure Stack 'de kota türleri | Microsoft Docs
titleSuffix: Azure Stack
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
ms.openlocfilehash: 29a154c5c446019e762b1312b9ef2f8a23cc4790
ms.sourcegitcommit: 102ef41963b5d2d91336c84f2d6af3fdf2ce11c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73955295"
---
# <a name="quota-types-in-azure-stack"></a>Azure Stack kota türleri

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

[Kotalar](service-plan-offer-subscription-overview.md#plans) , bir Kullanıcı aboneliğinin sağlayabileceği veya tüketebileceği kaynakların sınırlarını tanımlar. Örneğin, bir kota, kullanıcının en fazla beş sanal makine (VM) oluşturmasına izin verebilir. Her kaynak için farklı kota türleri kullanabilirsiniz.

> [!IMPORTANT]
> Yeni kotaların Kullanıcı portalında veya değiştirilen bir kota zorlanmadan önce kullanılabilmesi iki saate kadar sürebilir.

## <a name="compute-quota-types"></a>İşlem kotası türleri

| **Tür** | **Varsayılan değer** | **Açıklama** |
| --- | --- | --- |
| En fazla VM sayısı | 50 | Bir aboneliğin bu konumda oluşturabileceğiniz en fazla sanal makine sayısı. |
| Maksimum VM çekirdeği sayısı | 100 | Bir aboneliğin bu konumda oluşturabileceğiniz en fazla çekirdek sayısı (örneğin, bir a3 VM 'nin dört çekirdeği vardır). |
| Maksimum kullanılabilirlik kümesi sayısı | 10 | Bu konumda oluşturulabilecek maksimum kullanılabilirlik kümesi sayısı. |
| Maksimum sanal makine ölçek kümesi sayısı | 100 | Bu konumda oluşturulabilecek ölçek kümesi sayısı üst sınırı. |
| Standart yönetilen diskin maksimum kapasitesi (GB cinsinden) | 2048 | Bu konumda oluşturulabilecek Standart yönetilen disklerin maksimum kapasitesi. Bu değer, tüm standart yönetilen disklerin toplam ayırma boyutunun ve tüm standart anlık görüntülerin kullanılan boyutunun toplamıdır. |
| Premium yönetilen diskin maksimum kapasitesi (GB cinsinden) | 2048 | Bu konumda oluşturulabilecek Premium yönetilen disklerin maksimum kapasitesi. Bu değer, tüm Premium yönetilen disklerin toplam ayırma boyutunun ve tüm Premium anlık görüntülerin kullanılan boyutunun toplamıdır. |

> [!NOTE]
> Yönetilmeyen disklerin (sayfa Blobları) en fazla kapasitesi yönetilen disk kotasından ayrıdır. Bu değeri, **depolama kotalarına** **MAKSIMUM kapasite (GB)** cinsinden ayarlayabilirsiniz.

## <a name="storage-quota-types"></a>Depolama kotası türleri

| **Öğe** | **Varsayılan değer** | **Açıklama** |
| --- | --- | --- |
| Maksimum kapasite (GB) |2048 |Bu konumdaki bir abonelik tarafından tüketilen toplam depolama kapasitesi. Bu değer, tüm Blobların (yönetilmeyen diskler dahil) ve tüm ilişkili anlık görüntülerin, tablolarının ve kuyrukların kullanılan boyutunun toplamıdır. |
| Toplam depolama hesabı sayısı |20 |Bu konumda, bir aboneliğin oluşturabileceğiniz en fazla depolama hesabı sayısı. |

> [!NOTE]
> Bir abonelikte **Maksimum kapasite (GB)** aşıldığında, bu abonelikte yeni depolama kaynağı oluşturamazsınız. Ancak, VM 'lerde bu abonelikte oluşturulan yönetilmeyen diskleri, kota sınırının ötesinde toplam kullanılan kapasiteye neden olabilecek şekilde kullanarak sürekliliği sağlayabilirsiniz.<br>Yönetilen disklerin maksimum kapasitesi toplam depolama kotasından ayrıdır. Bu değeri, **işlem kotalarına**ayarlayabilirsiniz.

## <a name="network-quota-types"></a>Ağ kotası türleri

| **Öğe** | **Varsayılan değer** | **Açıklama** |
| --- | --- | --- |
| En fazla sanal ağ |50 |Bu konumda, bir aboneliğin oluşturabileceğiniz en fazla sanal ağ sayısı. |
| Maksimum sanal ağ geçitleri |1 |Bir aboneliğin bu konumda oluşturabileceğiniz en fazla sanal ağ geçidi sayısı (VPN ağ geçidi). |
| En yüksek ağ bağlantısı |2 |Bu konumdaki tüm sanal ağ geçitleri genelinde bir aboneliğin oluşturabileceğiniz en fazla ağ bağlantısı sayısı (noktadan noktaya veya siteden siteye). |
| En fazla genel IP |50 |Bir aboneliğin bu konumda oluşturabileceğiniz en fazla genel IP adresi sayısı. |
| En fazla NIC |100 |Bir aboneliğin bu konumda oluşturabileceğiniz en fazla ağ arabirimi sayısı. |
| Maksimum yük dengeleyiciler |50 |Bir aboneliğin bu konumda oluşturabileceğiniz maksimum yük dengeleyici sayısı. |
| En yüksek ağ güvenlik grupları |50 |Bir aboneliğin bu konumda oluşturabileceğiniz en fazla ağ güvenlik grubu sayısı. |

## <a name="view-an-existing-quota"></a>Mevcut kotayı görüntüleme

Mevcut kotayı görüntülemenin iki farklı yolu vardır:

### <a name="plans"></a>Planlar

1. Yönetici portalının sol gezinti bölmesinde **planlar**' ı seçin.
2. Adına tıklayarak ayrıntılarını görüntülemek istediğiniz planı seçin.
3. Açılan dikey pencerede **Hizmetler ve Kotalar**' ı seçin.
4. **Ad** sütununda tıklatarak görmek istediğiniz kotayı seçin.

    [![Azure Stack yönetici portalındaki kotalar](media/azure-stack-quota-types/quotas1sm.png "Yönetici portalında kotaları görüntüleme")](media/azure-stack-quota-types/quotas1.png#lightbox)

### <a name="resource-providers"></a>Kaynak sağlayıcıları

1. Yönetici portalının varsayılan panosunda **kaynak sağlayıcıları** kutucuğunu bulun.
2. **İşlem**, **ağ**veya **depolama**gibi, görüntülemek istediğiniz kotayı içeren hizmeti seçin.
3. **Kotalar**' ı seçin ve ardından görüntülemek istediğiniz kotayı seçin.

## <a name="edit-a-quota"></a>Kotayı düzenleme

Kotayı düzenlemenin iki farklı yolu vardır:

### <a name="edit-a-plan"></a>Planı düzenleme

1. Yönetici portalının sol gezinti bölmesinde **planlar**' ı seçin.
2. Bir kotayı düzenlemek istediğiniz planı seçin ve adına tıklayın.
3. Açılan dikey pencerede **Hizmetler ve Kotalar**' ı seçin.
4. **Ad** sütununda tıklatarak düzenlemek istediğiniz kotayı seçin.

    [![Azure Stack yönetici portalındaki kotalar](media/azure-stack-quota-types/quotas1sm.png "Yönetici portalında kotaları görüntüleme")](media/azure-stack-quota-types/quotas1.png#lightbox)

5. Açılan dikey pencerede, **işlem Içinde Düzenle**, **ağda Düzenle**veya **depolamada Düzenle**' yi seçin.

    ![Azure Stack yönetici portalında bir planı düzenleme](media/azure-stack-quota-types/quotas3.png "Azure Stack yönetici portalında bir planı düzenleme")

Alternatif olarak, kotayı düzenlemek için bu yordamı izleyebilirsiniz:

1. Yönetici portalının varsayılan panosunda **kaynak sağlayıcıları** kutucuğunu bulun.
2. **İşlem**, **ağ**veya **depolama**gibi, değiştirmek istediğiniz kotayı içeren hizmeti seçin.
3. Ardından, **Kotalar**' ı seçin ve ardından değiştirmek istediğiniz kotayı seçin.
4. **Depolama kotaları ayarlama**, **işlem kotalarını**ayarlama veya **ağ kotalarını ayarlama** bölmesini (düzenlemeyi seçtiğiniz kotanın türüne göre), değerleri düzenleyin ve ardından **Kaydet**' i seçin.

### <a name="edit-original-configuration"></a>Özgün yapılandırmayı Düzenle
  
Bir [eklenti planı kullanmak](create-add-on-plan.md)yerine, kotanın orijinal yapılandırmasını düzenlemeyi seçebilirsiniz. Bir kotayı düzenlediğinizde, yeni yapılandırma bu kotayı kullanan tüm planlara ve bu planları kullanan tüm mevcut abonelikleri otomatik olarak global olarak uygular. Bir kota düzenlemesi, bir kullanıcının abone olmayı seçtiği değiştirilmiş bir kota sağlamak için bir eklenti planı kullandığınız saatten farklıdır.

Kotanın yeni değerleri, değiştirilen kotayı kullanan tüm planlara ve bu planları kullanan tüm mevcut aboneliklere Global olarak uygulanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Hizmetler, planlar, teklifler ve Kotalar hakkında daha fazla bilgi edinin.](service-plan-offer-subscription-overview.md)
- [Bir plan oluştururken kotalar oluşturun.](azure-stack-create-plan.md)
