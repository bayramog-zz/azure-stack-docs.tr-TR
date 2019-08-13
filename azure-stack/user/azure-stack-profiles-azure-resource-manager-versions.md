---
title: Azure Stack | içindeki profiller tarafından desteklenen kaynak sağlayıcısı API sürümleri | Microsoft Docs
description: Azure Stack profillerin desteklediği Azure Resource Manager API sürümleri hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/12/2019
ms.author: sethm
ms.reviewer: sijuman
ms.lastreviewed: 05/08/2019
ms.openlocfilehash: 7f86ff3347d5d6f24bcd1fe450156c2557fb41e1
ms.sourcegitcommit: 58c28c0c4086b4d769e9d8c5a8249a76c0f09e57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68959392"
---
# <a name="resource-provider-api-versions-supported-by-profiles-in-azure-stack"></a>Azure Stack içindeki profiller tarafından desteklenen kaynak sağlayıcısı API sürümleri

Bu makaledeki Azure Stack tarafından kullanılan her bir API profili için kaynak sağlayıcısını ve sürüm numaralarını bulabilirsiniz. Bu makaledeki tablolarda, her bir kaynak sağlayıcısı için desteklenen sürümler ve profillerin API sürümleri listelenmiştir. Her kaynak sağlayıcı, kaynak türleri ve belirli sürüm numaraları kümesi içerir.

API profili üç adlandırma kuralı kullanır:

- **sürümü**
- **yyyy-mm-dd-hybrid**
- **yyyy-aa-gg-profil**

Azure Stack için API profillerinin ve sürüm sürümü temposunda hakkında bir açıklama için bkz. [Azure Stack API sürüm profillerini yönetme](azure-stack-version-profiles.md).

> [!NOTE]
> **En son** API profili, kaynak sağlayıcısı API 'sinin en son sürümünü içerir ve bu makalede listelenmez.

## <a name="overview-of-the-2019-03-01-hybrid-profile"></a>2019-03-01-karma profile genel bakış

| Kaynak sağlayıcı | Api sürümü |
|-----------------------------------------------|-----------------------------------------------------|
| Microsoft.Compute | 2017-12-01 |
| Microsoft.Network | 2017-10-01<br>VPN Gateway 2017-10-01 olacaktır |
| Microsoft. Storage (veri düzlemi) | 2017-10-01 |
| Microsoft. Storage (denetim düzlemi) | 2017-10-01 |
| Microsoft. Web | 2018-02-01 |
| Microsoft.KeyVault | 2016-10-01 (değiştirme) |
| Microsoft. Resources (Azure Resource Manager kendisi) | 2016-06-01 |
| Microsoft. Authorization (ilke işlemleri) | 2016-09-01 |
| Microsoft.Insights | 2018-01-01 |

API profilindeki sağlayıcılar için her kaynak türünün sürümlerinin bir listesi için bkz. [2019-03-01-hibrit profile Ilişkin Ayrıntılar](#details-for-the-2019-03-01-hybrid-profile).

## <a name="details-for-the-2019-03-01-hybrid-profile"></a>2019-03-01-karma profili ayrıntıları

### <a name="microsoftauthorization"></a>Microsoft. Authorization

Rol tabanlı erişim denetimi, kuruluşunuzdaki kullanıcıların kaynakları gerçekleştirebileceği eylemleri yönetmenizi sağlar. Rolleri tanımlayabilir, kullanıcılara veya gruplara roller atayabilir ve izinler hakkında bilgi edinebilirsiniz. Daha fazla bilgi için bkz. [Yetkilendirme](/rest/api/authorization/).

| Kaynak Türleri | API sürümleri |
|---------------------|--------------------|
| Kilitler | 2016-09-01 |
| İşlemler | 2015-07-01 |
| İzinler | 2015-07-01 |
| İlke Atamaları | 2016-12-01 |
| İlke Tanımları | 2016-12-01 |
| Sağlayıcı Işlemleri | 2015-07-01 |
| Rol Atamaları | 2015-07-01 |
| Rol Tanımları | 2015-07-01 |

### <a name="microsoftcommerce"></a>Microsoft. Commerce

| Kaynak Türü | API Sürümü |
|----------------------------------|----------------------|
| Temsilci sağlayıcı abonelikleri | 2015-06-01-Önizleme |
| Temsilci kullanım toplamaları | 2015-06-01-Önizleme |
| Tahmini kaynak harcaması | 2015-06-01-Önizleme |
| İşlemler | 2015-06-01-Önizleme |
| Abone kullanım toplamaları | 2015-06-01-Önizleme |
| Kullanım Toplamları | 2015-06-01-Önizleme |

### <a name="microsoftcompute"></a>Microsoft.Compute

Azure Işlem API 'Leri, sanal makinelere ve bunların destekleyici kaynaklarına programlı erişim sağlar. Daha fazla bilgi için bkz. [Azure işlem](/rest/api/compute/).

| Kaynak Türü | API Sürümü |
|---------------------------------------------------------------|-------------|
| Kullanılabilirlik Kümeleri | 2017-12-01 |
| Konumlar | 2017-12-01 |
| Konumlar/işlemler | 2017-12-01 |
| Konumlar/yayımcılar | 2017-12-01 |
| Konumlar/kullanımlar | 2017-12-01 |
| Konumlar/vmSizes | 2017-12-01 |
| İşlemler | 2017-12-01 |
| Virtual Machines | 2017-12-01 |
| Sanal makineler/uzantılar | 2017-12-01 |
| Sanal Makine Ölçek Kümeleri | 2017-12-01 |
| Sanal Makine Ölçek Kümeleri/uzantıları | 2017-12-01 |
| Sanal Makine Ölçek Kümeleri/ağ arabirimleri | 2017-12-01 |
| Sanal Makine Ölçek Kümeleri/sanal makineler | 2017-12-01|
| Sanal makineler ölçek kümeleri/virtualMachines/NetworkInterfaces | 2017-12-01 |

### <a name="microsoftgallery"></a>Microsoft. Gallery

| Kaynak Türü | API Sürümü |
|------------------|-------------|
| Seçki | 2015-04-01 |
| Seçki İçeriği | 2015-04-01 |
| Seçkiden Ayıklananlar | 2015-04-01 |
| Galeri öğeleri | 2015-04-01 |
| İşlemler | 2015-04-01 |
| Portal | 2015-04-01 |
| Ara | 2015-04-01 |
| Öner | 2015-04-01 |

### <a name="microsoftinsights"></a>Microsoft.Insights

| Kaynak Türleri | API sürümleri |
|--------------------|--------------------|
| İşlemler | 2015-04-01 |
| Olay Türleri | 2015-04-01 |
| Olay kategorileri | 2015-04-01 |
| Ölçüm Tanımları | 2018-01-01 |
| Ölçümler | 2018-01-01 |
| Tanılama ayarları | 2017-05-01-Önizleme |
| Tanılama ayarları kategorileri | 2017-05-01-Önizleme |

### <a name="microsoftkeyvault"></a>Microsoft.KeyVault

Key Vault ve Key Vault içindeki anahtarları, gizli dizileri ve sertifikaları yönetin. Daha fazla bilgi için [Azure Key Vault REST API başvurusuna](/rest/api/keyvault/)bakın.

| Kaynak Türleri | API sürümleri |
|-------------------------|--------------|
| İşlemler | 2016-10-01 |
| Kasalar | 2016-10-01 |
| Kasa/erişim Ilkeleri | 2016-10-01 |
| Kasa/gizlilikler | 2016-10-01 |

### <a name="microsoftnetwork"></a>Microsoft.Network

İşlemler çağrısı sonucu, kullanılabilir ağ bulutu işlemleri listesinin bir gösterimidir. Daha fazla bilgi için bkz. [işlem REST API](/rest/api/operation/).

| Kaynak Türleri | API sürümleri |
|---------------------------|--------------|
| Bağlantılar | 2017-10-01 |
| DNS Bölgeleri | 2016-04-01 |
| Yük Dengeleyiciler | 2017-10-01 |
| Yerel Ağ Geçidi | 2017-10-01 |
| Konumlar | 2017-10-01|
| Konum/operationResults | 2017-10-01 |
| Konumlar/işlemler | 2017-10-01 |
| Konumlar/kullanımlar |2017-10-01 |
| Ağ Arabirimleri | 2017-10-01 |
| Ağ Güvenlik Grupları | 2017-10-01 |
| İşlemler | 2017-10-01 |
| Genel IP Adresi | 2017-10-01 |
| Yönlendirme Tabloları | 2017-10-01 |
| Sanal Ağ Geçidi | 2017-10-01 |
| Sanal Ağlar | 2017-10-01 |

### <a name="microsoftresources"></a>Microsoft. resources

Azure Resource Manager, Azure çözümleriniz için altyapıyı dağıtmanıza ve yönetmenize olanak sağlar. Kaynak gruplarında ilgili kaynakları organize edersiniz ve kaynaklarınızı JSON şablonlarıyla dağıtırsınız. Kaynak Yöneticisi ile kaynakları dağıtmaya ve yönetmeye giriş için, bkz. [Azure Resource Manager genel bakış](/azure/azure-resource-manager/resource-group-overview).

| Kaynak Türleri | API sürümleri |
|-----------------------------------------|-------------------|
| Dağıtımlar | 2018-05-01 |
| Dağıtımlar/işlemler | 2018-05-01 |
| Bağlantılar | 2018-05-01 |
| Konumlar | 2018-05-01 |
| İşlemler | 2018-05-01 |
| Sağlayıcılar | 2018-05-01 |
| ResourceGroups| 2018-05-01 |
| Kaynaklar | 2018-05-01/ |
| Abonelikler | 2018-05-01 |
| Abonelikler/konumlar | 2016-06-01 |
| Abonelikler/operationresults | 2018-05-01 |
| Abonelikler/sağlayıcılar | 2018-05-01 |
| Abonelikler/ResourceGroups | 2018-05-01 |
| Abonelikler/resourceGroups/kaynaklar | 2018-05-01 |
| Abonelikler/kaynaklar | 2018-05-01 |
| Abonelikler/etiket adları | 2018-05-01 |
| Abonelikler/etiket adları/tagValues | 2018-05-01 |
| Kira | 2016-06-01 |

### <a name="microsoftstorage"></a>Microsoft.Storage

Depolama kaynak sağlayıcısı (SRP), depolama hesabınızı ve anahtarlarınızı programlı bir şekilde yönetmenizi sağlar. Daha fazla bilgi için bkz. [Azure depolama kaynak sağlayıcısı REST API başvurusu](/rest/api/storagerp/).

| Kaynak Türleri | API sürümleri |
|-------------------------|--------------|
| Checknameavaılabılıty | 2017-10-01 |
| Konumlar | 2017-10-01 |
| Konumlar/kotalar | 2017-10-01 |
| İşlemler | 2017-10-01 |
| StorageAccounts | 2017-10-01 |
| Kullanımlar | 2017-10-01 |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack için PowerShell yükleme](../operator/azure-stack-powershell-install.md)
- [Azure Stack kullanıcının PowerShell ortamını yapılandırma](azure-stack-powershell-configure-user.md)  
