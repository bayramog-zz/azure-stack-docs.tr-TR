---
title: Azure Stack ağ kaynakları 'nı yönetme | Microsoft Docs
description: Yöneticiler, MAC adresi havuzu ve bir bölgedeki genel IP adreslerinin tüketimi dahil olmak üzere ağ kaynaklarını yönetebilir
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: mabrigg
ms.reviewer: scottnap
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: d056cbf73e2417bd826fba7a7de263cc8e015b7d
ms.sourcegitcommit: 637018771ac016b7d428174e88d4dcb131b54959
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68842905"
---
# <a name="manage-network-resources"></a>Ağ kaynaklarını yönetme

## <a name="mac-address-pool"></a>MAC adresi havuzu

Azure Stack, otomatik olarak MAC adresi oluşturmak ve sanal makinelere atamak için statik bir MAC adresi havuzu kullanır.
Bu MAC adres havuzu, dağıtım sırasında otomatik olarak oluşturulur ve aşağıdaki aralığı kullanır:

- StartMacAddress: 00-1D-D8-B7-00-00
- EndMacAddress: 00-1D-D8-F4-FF-FF

> [!Note]  
> Bu MAC adres havuzu her bir Azure Stack sisteminde aynıdır ve yapılandırılamaz.

Sanal ağların mevcut şirket ağlarına nasıl bağlandığına bağlı olarak, sanal makinelerin yinelenen MAC adreslerini de bekleyebilir.

Azure Stack yönetici PowerShell modülündeki [Get-AzsMacAddressPool](https://docs.microsoft.com/powershell/module/azs.fabric.admin/get-azsmacaddresspool) cmdlet 'INI kullanarak MAC adresi havuzu kullanımı hakkında daha fazla bilgi bulabilirsiniz.

## <a name="view-public-ip-address-consumption-in-azure-stack"></a>Azure Stack genel IP adresi tüketimini görüntüleme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bulut Yöneticisi olarak şunları görebilirsiniz:
 - Kiracılara ayrılan genel IP adreslerinin sayısı.
 - Ayırma için hala kullanılabilir olan genel IP adreslerinin sayısı.
 - Bu konumda ayrılan genel IP adreslerinin yüzdesi.

Genel IP **havuzları kullanım** kutucuğu, genel IP adresi havuzları genelinde TÜKETILEN genel IP adreslerinin sayısını gösterir. Her IP adresi için kutucuk, kiracı IaaS VM örnekleri, yapı altyapısı Hizmetleri ve kiracılar tarafından açıkça oluşturulan genel IP adresi kaynakları için kullanımı gösterir.

Kutucuğun amacı, bu konumda kullanılan genel IP adresi sayısını Azure Stack işleçlerine vermektir. Numara, yöneticilerin bu kaynakta düşük çalışıp çalışmadığını belirlemesine yardımcı olur.

**Kiracı kaynakları** altındakı **genel IP adresleri** menü öğesi yalnızca *KIRACıLAR tarafından açıkça oluşturulan*genel IP adreslerini listeler. Menü öğesini **kaynak sağlayıcıları**, **ağ** bölmesinde bulabilirsiniz. **Genel IP havuzları kullanım** kutucuğunda **kullanılan** genel IP adreslerinin sayısı, **kiracı kaynakları**altındaki **genel IP adresleri** kutucuğunda bulunan sayıdan her zaman farklıdır (büyüktür).

### <a name="view-the-public-ip-address-usage-information"></a>Genel IP adresi kullanım bilgilerini görüntüleme

Bölgede tüketilen genel IP adreslerinin toplam sayısını görüntülemek için:

1. Azure Stack Yönetici portalı ' nda **tüm hizmetler**' i seçin. Ardından, **Yönetim** kategorisi altında **ağ**' ı seçin.
1. **Ağ** bölmesi **genel bakış** bölümünde **genel IP havuzları kullanım** kutucuğunu görüntüler.

![Ağ kaynak sağlayıcısı bölmesi](media/azure-stack-viewing-public-ip-address-consumption/image01.png)

**Kullanılan** sayı, genel IP adresi havuzlarından atanan genel IP adreslerinin sayısını temsil eder. **Ücretsiz** numara, atanmamış ve hala kullanılabilir olan genel IP adresi havuzlarından gelen genel IP adresi sayısını temsil eder. **% Kullanılan** sayı, bu KONUMDAKI genel IP adresi HAVUZLARıNDAKI genel IP adreslerinin toplam sayısının yüzdesi olarak kullanılan veya atanan adreslerin sayısını temsil eder.

### <a name="view-the-public-ip-addresses-that-were-created-by-tenant-subscriptions"></a>Kiracı abonelikleri tarafından oluşturulan genel IP adreslerini görüntüleme

**Kiracı kaynakları**altında **genel IP adresleri** ' ni seçin. Belirli bir bölgedeki kiracı abonelikleri tarafından açıkça oluşturulan genel IP adreslerinin listesini gözden geçirin.

![Kiracı Genel IP adresleri](media/azure-stack-viewing-public-ip-address-consumption/image02.png)

Dinamik olarak ayrılan bazı genel IP adreslerinin listede göründüğünü fark edebilirsiniz. Ancak, henüz bir adres ilişkilendirilmemiştir. Adres kaynağı ağ kaynak sağlayıcısında oluşturulmuştur, ancak henüz ağ denetleyicisinde oluşturulmamıştır.

Ağ denetleyicisi, bir arabirime, ağ arabirimi kartına (NIC), yük dengeleyicisine veya bir sanal ağ geçidine bağlaana kadar kaynağa bir adres atamaz. Genel IP adresi bir arabirime bağlandığında, ağ denetleyicisi bir IP adresi ayırır. Adres, adres alanında görüntülenir .

### <a name="view-the-public-ip-address-information-summary-table"></a>Genel IP adresi bilgileri özet tablosunu görüntüleme

Farklı durumlarda, adresin bir listede mi yoksa başka bir listede mi göründüğünü belirleyecek genel IP adresleri atanır.

| **Genel IP adresi atama durumu** | **Kullanım özetinde görünür** | **Kiracı Genel IP adresleri listesinde görüntülenir** |
| --- | --- | --- |
| Henüz bir NIC veya yük dengeleyiciye atanmamış dinamik genel IP adresi (geçici) |Hayır |Evet |
| Bir NIC veya yük dengeleyiciye atanan dinamik genel IP adresi. |Evet |Evet |
| Bir kiracı NIC 'sine veya yük dengeleyiciye atanan statik genel IP adresi. |Evet |Evet |
| Bir Fabric altyapı hizmeti uç noktasına atanan statik genel IP adresi. |Evet |Hayır |
| IaaS VM örnekleri için genel IP adresi örtük olarak oluşturulur ve sanal ağda giden NAT için kullanılır. Bunlar, sanal makinelerin Internet 'e bilgi gönderebilmesi için bir kiracı bir sanal makine örneği oluşturduğunda her bir arka planda oluşturulur. |Evet |Hayır |

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack depolama hesaplarını yönetme](azure-stack-manage-storage-accounts.md)
