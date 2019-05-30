---
title: Azure Stack profilleri tarafından desteklenen kaynak sağlayıcısı API sürümleri | Microsoft Docs
description: Azure Stack profilleri tarafından desteklenen Azure Resource Manager API sürümleri hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2019
ms.author: sethm
ms.reviewer: sijuman
ms.lastreviewed: 05/08/2019
ms.openlocfilehash: 21e1e8df3d5f43f91e391b8c39f3f5aca3aefea8
ms.sourcegitcommit: 797dbacd1c6b8479d8c9189a939a13709228d816
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66269579"
---
# <a name="resource-provider-api-versions-supported-by-profiles-in-azure-stack"></a>Azure Stack profilleri tarafından desteklenen kaynak sağlayıcısı API sürümleri

Bu makalede Azure yığını tarafından kullanılan her bir API profili için bir kaynak sağlayıcısı ve sürüm numaraları bulabilirsiniz. Bu makaledeki tablolar, her kaynak sağlayıcısı ve Profiller API sürümleri için desteklenen sürümleri listelenir. Her kaynak sağlayıcısı, bir dizi kaynak türleri ve belirli sürüm numaraları içeriyor.

API profili üç adlandırma kuralları kullanır:

- **en son**
- **yyyy-mm-dd-hybrid**
- **yyyy-aa-gg-profili**

Bir API profillerini ve açıklama sürüm yayın temposudur için Azure Stack için bkz: [yönetme API sürümü profillerini Azure Stack'te](azure-stack-version-profiles.md).

> [!Note]
> **Son** API profili kaynak sağlayıcısı API'si en son sürümünü içerir ve bu makalede listelenen değil.

## <a name="overview-of-the-2019-03-01-hybrid-profile"></a>2019-03-01-karma profiline genel bakış

| Kaynak sağlayıcı | API sürümü |
|-----------------------------------------------|-----------------------------------------------------|
| Microsoft.Compute | 2017-12-01 |
| Microsoft.Network | 2017-10-01<br>VPN ağ geçidi 2017-10-01 olacaktır |
| Microsoft.Storage (veri düzlemi) | 2017-10-01 |
| Microsoft.Storage (Denetim düzlemi) | 2017-10-01 |
| Microsoft.Web | 2018-02-01 |
| Microsoft.KeyVault | 2016-10-01 (değiştirmeden) |
| Microsoft.Resources (Azure Resource Manager kendi) | 2016-06-01 |
| Microsoft.Authorization (ilke işlemleri) | 2016-09-01 |
| Microsoft.Insights | 2018-01-01 |

API Profil sağlayıcıları için her kaynak türünün sürümlerinin listesi için bkz: [2019-03-01-karma profil ayrıntılarını](#details-for-the-2019-03-01-hybrid-profile).

## <a name="details-for-the-2019-03-01-hybrid-profile"></a>2019-03-01-karma profili ayrıntıları

### <a name="microsoftauthorization"></a>Microsoft.Authorization

Rol tabanlı erişim denetimi, kuruluşunuzdaki kullanıcıların kaynakları üzerinde gerçekleştirebileceğiniz eylemlerden yönetmenizi sağlar. Rolleri tanımlama, rolleri kullanıcılara veya gruplara atamak ve izinleri hakkında bilgi edinin. Daha fazla bilgi için [yetkilendirme](/rest/api/authorization/).

| Kaynak Türleri | API sürümleri |
|---------------------|--------------------|
| Kilitler | 2016-09-01 |
| İşlemler | 2015-07-01 |
| İzinler | 2015-07-01 |
| İlke Atamaları | 2016-12-01 |
| İlke Tanımları | 2016-12-01 |
| Sağlayıcı işlemleri | 2015-07-01 |
| Rol Atamaları | 2015-07-01 |
| Rol Tanımları | 2015-07-01 |

### <a name="microsoftcommerce"></a>Microsoft.Commerce

| Kaynak Türü | API Sürümü |
|----------------------------------|----------------------|
| Sağlayıcı temsilcisi abonelikler | 2015-06-01 - Önizleme |
| Temsilci kullanım toplamları | 2015-06-01 - Önizleme |
| Harcamalarınızı tahmin kaynak | 2015-06-01 - Önizleme |
| İşlemler | 2015-06-01 - Önizleme |
| Abone kullanım toplamları | 2015-06-01 - Önizleme |
| Kullanım Toplamları | 2015-06-01 - Önizleme |

### <a name="microsoftcompute"></a>Microsoft.Compute

İşlem Azure API'leri, sanal makineler ve destek kaynaklarını programlı erişim sağlar. Daha fazla bilgi için [Azure işlem](/rest/api/compute/).

| Kaynak Türü | API Sürümü |
|---------------------------------------------------------------|-------------|
| Kullanılabilirlik Kümeleri | 2017-12-01 |
| Konumlar | 2017-12-01 |
| Konum/işlemleri | 2017-12-01 |
| Konum/yayımcıları | 2017-12-01 |
| Konum/kullanımları | 2017-12-01 |
| Konum/vmSizes | 2017-12-01 |
| İşlemler | 2017-12-01 |
| Virtual Machines | 2017-12-01 |
| Sanal makineler ve uzantıları | 2017-12-01 |
| Sanal Makine Ölçek Kümeleri | 2017-12-01 |
| Sanal makine ölçek kümeleri ve uzantıları | 2017-12-01 |
| Sanal makine ölçek kümeleri/ağ arabirimleri | 2017-12-01 |
| Sanal makine ölçek kümeleri/sanal makineler | 2017-12-01|
| Sanal makine ölçek kümeleri/virtualMachines/networkınterface'lerden bazıları | 2017-12-01 |

### <a name="microsoftgallery"></a>Microsoft.Gallery

| Kaynak Türü | API Sürümü |
|------------------|-------------|
| Seçki | 2015-04-01 |
| Seçki İçeriği | 2015-04-01 |
| Seçkiden Ayıklananlar | 2015-04-01 |
| Galeri öğeleri | 2015-04-01 |
| İşlemler | 2015-04-01 |
| Portal | 2015-04-01 |
| Arama | 2015-04-01 |
| Öner | 2015-04-01 |

### <a name="microsoftinsights"></a>Microsoft.Insights

| Kaynak Türleri | API sürümleri |
|--------------------|--------------------|
| İşlemler | 2015-04-01 |
| Olay Türleri | 2015-04-01 |
| Olay kategorisi | 2015-04-01 |
| Ölçüm Tanımları | 2018-01-01 |
| Ölçümler | 2018-01-01 |
| Tanılama ayarları | 2017-05-01-Önizleme |
| Tanılama ayarları kategorileri | 2017-05-01-Önizleme |

### <a name="microsoftkeyvault"></a>Microsoft.KeyVault

Key Vault'unuza yanı sıra anahtarlara, parolalara ve sertifikaları Key Vault'unuza içinde yönetin. Daha fazla bilgi için [Azure anahtar kasası REST API Başvurusu](/rest/api/keyvault/).

| Kaynak Türleri | API sürümleri |
|-------------------------|--------------|
| İşlemler | 2016-10-01 |
| Kasalar | 2016-10-01 |
| Kasaları / erişim ilkeleri | 2016-10-01 |
| Kasalar/parolalar | 2016-10-01 |

### <a name="microsoftnetwork"></a>Microsoft.Network

İşlem araması sonucu, kullanılabilir ağ bulut işlemleri listesi gösterimidir. Daha fazla bilgi için [işlem REST API](/rest/api/operation/).

| Kaynak Türleri | API sürümleri |
|---------------------------|--------------|
| Bağlantılar | 2017-10-01 |
| DNS Bölgeleri | 2016-04-01 |
| Yük Dengeleyiciler | 2017-10-01 |
| Yerel Ağ Geçidi | 2017-10-01 |
| Konumlar | 2017-10-01|
| Konum/operationResults | 2017-10-01 |
| Konum/işlemleri | 2017-10-01 |
| Konum/kullanımları |2017-10-01 |
| Ağ Arabirimleri | 2017-10-01 |
| Ağ Güvenlik Grupları | 2017-10-01 |
| İşlemler | 2017-10-01 |
| Genel IP Adresi | 2017-10-01 |
| Yönlendirme Tabloları | 2017-10-01 |
| Sanal Ağ Geçidi | 2017-10-01 |
| Sanal Ağlar | 2017-10-01 |

### <a name="microsoftresources"></a>Microsoft.Resources

Azure Resource Manager dağıtma ve Azure çözümlerinizi için altyapıyı yönetmenize olanak sağlar. Kaynak grupları ilgili kaynakların düzenlemenize ve kaynaklarınızı JSON şablonları ile dağıtın. Kaynakları Resource Manager ile yönetme ve dağıtma için bir giriş için bkz [Azure Resource Manager'a genel bakış](/azure/azure-resource-manager/resource-group-overview).

| Kaynak Türleri | API sürümleri |
|-----------------------------------------|-------------------|
| Dağıtımlar | 2018-05-01 |
| Dağıtımları/işlemleri | 2018-05-01 |
| Bağlantılar | 2018-05-01 |
| Konumlar | 2018-05-01 |
| İşlemler | 2018-05-01 |
| Sağlayıcılar | 2018-05-01 |
| ResourceGroups| 2018-05-01 |
| Kaynaklar | 2018-05-01/ |
| Subscriptions | 2018-05-01 |
| Abonelikler/konumları | 2016-06-01 |
| Abonelikler/operationresults | 2018-05-01 |
| Abonelikler/sağlayıcıları | 2018-05-01 |
| Abonelikler/kaynak grupları | 2018-05-01 |
| Abonelikler/resourceGroups/kaynaklar | 2018-05-01 |
| Abonelikler/kaynak | 2018-05-01 |
| Abonelikler/tagNames | 2018-05-01 |
| Abonelikler/tagNames/tagValues | 2018-05-01 |
| Kiracılar | 2016-06-01 |

### <a name="microsoftstorage"></a>Microsoft.Storage

Depolama kaynak Sağlayıcısı'nı (SRP) depolama hesabı ve anahtarları programlı bir şekilde yönetmenizi sağlar. Daha fazla bilgi için [Azure depolama kaynak sağlayıcısı REST API'si başvurusunda](/rest/api/storagerp/).

| Kaynak Türleri | API sürümleri |
|-------------------------|--------------|
| CheckNameAvailability | 2017-10-01 |
| Konumlar | 2017-10-01 |
| Konum/kotaları | 2017-10-01 |
| İşlemler | 2017-10-01 |
| storageAccounts | 2017-10-01 |
| Kullanımlar | 2017-10-01 |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stack için PowerShell yükleme](../operator/azure-stack-powershell-install.md)
* [Azure Stack kullanıcının PowerShell ortamını yapılandırma](azure-stack-powershell-configure-user.md)  
