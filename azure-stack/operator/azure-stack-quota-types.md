---
title: Azure stack'teki kota türleri | Microsoft Docs
description: Görüntüleyebilir ve hizmetler ve Azure Stack'te kaynaklar için kullanılabilen farklı kota türlerin düzenleyebilirsiniz.
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
origin.date: 03/27/2019
ms.date: 04/29/2019
ms.author: v-jay
ms.reviewer: xiaofmao
ms.lastreviewed: 12/07/2018
ms.openlocfilehash: 848b2a0c912a00a2185d7e4b7b8d8446bc1f6aca
ms.sourcegitcommit: 0973dddb81db03cf07c8966ad66526d775ced8b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "64295230"
---
# <a name="quota-types-in-azure-stack"></a>Azure stack'teki kota türleri

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

[Kotalar](azure-stack-plan-offer-quota-overview.md#plans) kaynaklara kullanıcı aboneliği sağlama veya tüketen sınırlarını tanımlayın. Örneğin bir kullanıcının en fazla beş VM oluşturmasına izin veren bir kota oluşturabilirsiniz. Her kaynak için farklı kota türleri kullanabilirsiniz.

> [!IMPORTANT]
> Uygulamanın, Kullanıcı Portalı'nda veya değiştirilmiş bir kota uygulanmadan önce kullanılabilir olması için yeni kotalar iki saate kadar sürebilir.

## <a name="compute-quota-types"></a>Kota türleri işlem

| **Tür** | **Varsayılan değer** | **Açıklama** |
| --- | --- | --- |
| Sanal makine sayısı | 50 | Bu konumda bir abonelik oluşturan sanal makineler en fazla sayısı. |
| Sanal makine çekirdeklerinin sayısı | 100 | Bu konumda abonelik oluşturup oluşturamayacağını çekirdek sayısı (örneğin, bir A3 VM dört çekirdek vardır). |
| Kullanılabilirlik kümelerinin maksimum sayısı | 10 | Bu konumda oluşturulabilir kullanılabilirlik kümelerinin maksimum sayısı. |
| Sanal makine ölçek en fazla sayısını ayarlar | 100 | Bu konumda oluşturulan sanal makine ölçek kümeleri en fazla sayısı. |
| Standart yönetilen disk maksimum kapasite (GB cinsinden) | 2048 | Bu konumda oluşturulabilen standart yönetilen diskler hizmetin maksimum kapasitesi. |
| Premium yönetilen disk maksimum kapasite (GB cinsinden) | 2048 | Premium kapasite üst sınırı bu konumda oluşturulan disklerini yönetilen. |

> [!NOTE]  
> Hizmetin maksimum kapasitesi yönetilmeyen disk (sayfa blobları), yönetilen disk kotada ayrıdır. Bu değer ayarlayabilirsiniz **depolama kotalarını**.

## <a name="storage-quota-types"></a>Depolama kota türleri

| **Öğesi** | **Varsayılan değer** | **Açıklama** |
| --- | --- | --- |
| Maksimum Kapasite (GB) |2048 |Bu konumda bulunan bir abonelik tarafından tüketilebilecek (BLOB'lar ve ilişkili tüm anlık görüntüleri, tablolar, kuyruklar dahil) toplam depolama kapasitesi. |
| Toplam depolama hesabı sayısı |20 |Bu konumda bir abonelik oluşturduğunuz depolama hesabı sayısı. |

> [!NOTE]  
> Hizmetin maksimum kapasitesi yönetilen diskler, toplam depolama alanı kotada ayrıdır. Bu değer ayarlayabilirsiniz **işlem kotaları**.

## <a name="network-quota-types"></a>Ağ kota türleri

| **Öğesi** | **Varsayılan değer** | **Açıklama** |
| --- | --- | --- |
| En büyük ortak IP'ler |50 |Bu konumda bir abonelik oluşturduğunuz genel IP adresleri maksimum sayısı. |
| En yüksek sanal ağlar |50 |Bu konumda bir abonelik oluşturan sanal ağlar maksimum sayısı. |
| En fazla sanal ağ geçitleri |1 |Bu konumda bir abonelik oluşturan, sanal ağ geçitleri (VPN ağ geçitleri) sayısı. |
| En fazla ağ bağlantıları |2 |Bu konumdaki tüm sanal ağ geçitleri arasında abonelik oluşturup oluşturamayacağını olan ağ bağlantıları (Noktadan noktaya veya siteden siteye) sayısı. |
| En yüksek yük Dengeleyiciler |50 |Bu konumda abonelik oluşturup oluşturamayacağını yük Dengeleyiciler maksimum sayısı. |
| En fazla NIC |100 |Bu konumda bir abonelik oluşturan ağ arabirimleri sayısı. |
| En fazla ağ güvenlik grupları |50 |Bu konumda bir abonelik oluşturan ağ güvenlik grupları maksimum sayısı. |

## <a name="view-an-existing-quota"></a>Var olan bir kota görüntüleyin

Var olan bir kota görüntülemek için iki farklı yolu vardır:

### <a name="plans"></a>Planlar

1. Yönetici portalı'nın sol gezinti bölmesinde seçin **planları**.
2. Adına tıklayarak, ayrıntılarını görüntülemek istediğiniz planı seçin.
3. Açılan dikey pencerede seçin **hizmetler ve kotalar**.
4. İçine tıklayarak görmek istediğiniz kota seçin **adı** sütun.

    [![Kotalar](media/azure-stack-quota-types/quotas1sm.png "kotaları görüntüle")](media/azure-stack-quota-types/quotas1.png#lightbox)

### <a name="resource-providers"></a>Kaynak sağlayıcıları

1. Yönetim Portalı'nın varsayılan Panoda bulmak **kaynak sağlayıcıları** Döşe.
2. Gibi görüntülemek istediğiniz kota hizmetiyle seçin **işlem**, **ağ**, veya **depolama**.
3. Seçin **kotalar**ve ardından görüntülemek istediğiniz kota seçin.

## <a name="edit-a-quota"></a>Kotayı Düzenle

Kota düzenlemek için iki farklı yolu vardır:

### <a name="edit-a-plan"></a>Bir planı düzenleme

1. Yönetici portalı'nın sol gezinti bölmesinde seçin **planları**.
2. Bir kota adına tıklayarak düzenlemek istediğiniz planı seçin.
3. Açılan dikey pencerede seçin **hizmetler ve kotalar**.
4. İçine tıklayarak düzenlemek istediğiniz kota seçin **adı** sütun.
    [![Kotalar](media/azure-stack-quota-types/quotas1sm.png "kotaları görüntüle")](media/azure-stack-quota-types/quotas1.png#lightbox)

5. Açılan dikey pencerede seçin **işlem düzenleme**, **ağında Düzenle**, veya **depolamada Düzenle**.
    ![Kotalar](media/azure-stack-quota-types/quotas3.png "kotaları görüntüle")

Alternatif olarak, bir kota düzenlemek için bu yordamı izleyin:

1. Yönetici portalı'nın varsayılan Panoda bulmak **kaynak sağlayıcıları** Döşe.
2. Gibi değiştirmek istediğiniz kota hizmetiyle seçin **işlem**, **ağ**, veya **depolama**.
3. Ardından, **kotalar**ve ardından değiştirmek istediğiniz kota seçin.
4. Üzerinde **kümesi depolama kotalarını**, **ayarlamak işlem kotaları**, veya **kümesi ağ kotalar** bölmesinde (türüne bağlı olarak düzenlemek için seçtiğiniz kota), değerlerini düzenleyin ve ardından **Kaydet**.

### <a name="edit-original-configuration"></a>Özgün yapılandırmayı Düzenle
  
Bir kota yerine yapılandırmasını düzenlemek seçebileceğiniz [eklenti planı'nı kullanarak](create-add-on-plan.md). Kota düzenlediğinizde, yeni yapılandırmayı otomatik olarak genel olarak, kota kullanan tüm planlar ve bu planları kullanın ve var olan tüm abonelikler için geçerlidir. Kotayı düzenleme, bir eklenti planı abone olmak için bir kullanıcının seçtiği değiştirilmiş bir kota sağlamak için kullandığınızda farklıdır.

Kota için yeni değerler genel olarak değiştirilen kota kullanan tüm planlar ve bu planları kullanın ve var olan tüm abonelikler için geçerlidir.

## <a name="next-steps"></a>Sonraki adımlar

- [Daha fazla ilgili planlar, teklifler ve kotalar öğrenin.](azure-stack-plan-offer-quota-overview.md)
- [Kotalar bir plan oluştururken oluşturun.](azure-stack-create-plan.md)

<!-- Update_Description: add edit a quota -->