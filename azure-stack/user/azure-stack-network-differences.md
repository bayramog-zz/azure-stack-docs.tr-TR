---
title: Azure Stack ağ farkları | Microsoft Docs
description: Azure Stack 'de ağlarla çalışırken farklılıklar ve dikkat edilmesi gerekenler hakkında bilgi edinin.
services: azure-stack
keywords: ''
author: mattbriggs
manager: femila
ms.date: 10/10/2019
ms.topic: article
ms.service: azure-stack
ms.author: mabrigg
ms.reviewer: wamota
ms.lastreviewed: 07/10/2019
ms.openlocfilehash: 09e75656fc6a00181ffb31087e19e80b92760ed2
ms.sourcegitcommit: b72729305234e13c65de3771cb08678d46ba1348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72543667"
---
# <a name="differences-and-considerations-for-azure-stack-networking"></a>Azure Stack ağ oluşturma farklılıkları ve konuları

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack ağ, Azure ağ tarafından sunulan birçok özelliğe sahiptir. Ancak, bir Azure Stack ağı dağıtmadan önce anlamanız gereken bazı önemli farklılıklar vardır.

Bu makalede, Azure Stack ağ ve özellikleri için benzersiz noktalara genel bakış sunulmaktadır. Azure Stack ile Azure arasındaki üst düzey farklılıklar hakkında bilgi edinmek için bkz. [temel konular](azure-stack-considerations.md) makalesi.

## <a name="cheat-sheet-networking-differences"></a>Tek sayfa: ağ farklılıkları

| Hizmet | Özellik | Azure (genel) | Azure Stack |
|--------------------------|----------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| DNS | Çok kiracılı DNS | Desteklenen | Henüz desteklenmiyor |
|  | DNS AAAA kayıtları | Desteklenen | Desteklenmiyor |
|  | Abonelik başına DNS bölgeleri | 100 (varsayılan)<br>, İstek üzerine artırılabilir. | 100 |
|  | Bölge başına DNS kayıt kümesi | 5000 (varsayılan)<br>, İstek üzerine artırılabilir. | 5000 |
|  | Bölge temsili için ad sunucuları | Azure, oluşturulan her kullanıcı (kiracı) bölgesi için dört ad sunucusu sağlar. | Azure Stack, oluşturulan her kullanıcı (kiracı) bölgesi için iki ad sunucusu sağlar. |
| Azure Güvenlik Duvarı | Ağ güvenlik hizmeti | Azure Güvenlik Duvarı, Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir güvenlik hizmetidir. | Henüz desteklenmiyor. |
| Sanal Ağ | Sanal ağ eşlemesi | Azure omurga ağı aracılığıyla aynı bölgedeki iki sanal ağı birbirine bağlayın. | Henüz desteklenmiyor |
|  | IPv6 adresleri | [Ağ arabirimi yapılandırmasının](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface-addresses#ip-address-versions)bir parçası olarak bir IPv6 adresi atayabilirsiniz. | Yalnızca IPv4 desteklenir. |
|  | DDoS koruma planı | Desteklenen | Henüz desteklenmiyor. |
|  | Ölçek kümesi IP yapılandırması | Desteklenen | Henüz desteklenmiyor. |
|  | Özel erişim Hizmetleri (alt ağ) | Desteklenen | Henüz desteklenmiyor. |
|  | Hizmet Uç Noktaları | Azure hizmetlerine yönelik dahili (Internet olmayan) bağlantı için desteklenir. | Henüz desteklenmiyor. |
|  | Hizmet Uç Noktası İlkeleri | Desteklenen | Henüz desteklenmiyor. |
|  | Hizmet tünelleri | Desteklenen | Henüz desteklenmiyor.  |
| Ağ Güvenlik Grupları | Genişletilmiş güvenlik kuralları | Desteklenen | Henüz desteklenmiyor. |
|  | Etkin güvenlik kuralları | Desteklenen | Henüz desteklenmiyor. |
|  | Uygulama Güvenlik Grupları | Desteklenen | Henüz desteklenmiyor. |
| Sanal Ağ Geçitleri | Noktadan siteye VPN Gateway | Desteklenen | Henüz desteklenmiyor. |
|  | VNET-VNET ağ geçidi | Desteklenen | Henüz desteklenmiyor. |
|  | Sanal ağ geçidi türü | Azure VPN 'yi destekler<br> Express Route <br> Hiper ağ. | Azure Stack Şu anda yalnızca VPN türünü destekliyor. |
|  | VPN Gateway SKU 'Ları | Temel, GW1, GW2, GW3, standart yüksek performans, ultra yüksek performans desteği. | Temel, standart ve yüksek performanslı SKU 'Lar için destek. |
|  | VPN türü | Azure, ilke tabanlı ve rota tabanlı ' i destekler. | Azure Stack yalnızca rota tabanlı desteği destekler. |
|  | BGP ayarları | Azure, BGP eşleme adresinin ve eş ağırlığın yapılandırılmasını destekler. | BGP eşleme adresi ve eş ağırlığı Azure Stack ' de otomatik olarak yapılandırılır. Kullanıcının bu ayarları kendi değerleriyle yapılandırması mümkün değildir. |
|  | Varsayılan ağ geçidi sitesi | Azure, Zorlamalı tünel için varsayılan bir sitenin yapılandırılmasını destekler. | Henüz desteklenmiyor. |
|  | Ağ geçidini yeniden boyutlandırma | Azure, dağıtımdan sonra ağ geçidini yeniden boyutlandırmayı destekler. | Yeniden boyutlandırma desteklenmiyor. |
|  | Kullanılabilirlik yapılandırması | Etkin/etkin | Etkin/Pasif |
|  | UsePolicyBasedTrafficSelectors | Azure, rota tabanlı ağ geçidi bağlantılarıyla ilke tabanlı trafik seçicileri kullanımını destekler. | Henüz desteklenmiyor. |
| Yük dengeleyici | SKU | Temel ve standart yük dengeleyiciler destekleniyor | Yalnızca temel Load Balancer desteklenir.<br>SKU özelliği desteklenmiyor.<br>Temel SKU yük dengeleyici/Path/, 5 ' ten fazla ön uç IP yapılandırmasına sahip olamaz.  |
|  | Bölgeler | Kullanılabilirlik Alanları desteklenir. | Henüz desteklenmiyor |
|  | Hizmet uç noktaları için gelen NAT kuralları desteği | Azure, gelen NAT kuralları için hizmet uç noktalarının belirtilmesini destekler. | Azure Stack henüz hizmet uç noktalarını desteklemez, bu nedenle bunlar belirtilemez. |
|  | Protokol | Azure, GRE veya ESP belirtmeyi destekler. | Protokol sınıfı Azure Stack desteklenmiyor. |
| Genel IP Adresi | Genel IP adresi sürümü | Azure hem IPv6 hem de IPv4 destekler. | Yalnızca IPv4 desteklenir. |
| Ağ Arabirimi | Geçerli yol tablosu al | Desteklenen | Henüz desteklenmiyor. |
|  | Etkin ACL 'Ler al | Desteklenen | Henüz desteklenmiyor. |
|  | Hızlandırılmış ağı etkinleştir | Desteklenen | Henüz desteklenmiyor. |
|  | IP İletimi | Varsayılan olarak devre dışıdır.  Etkinleştirilebilir. | Bu ayar geçiş desteklenmiyor.  Varsayılan olarak açık. |
|  | Uygulama Güvenlik Grupları | Desteklenen | Henüz desteklenmiyor. |
|  | İç DNS ad etiketi | Desteklenen | Henüz desteklenmiyor. |
|  | Özel IP adresi sürümü | Hem IPv6 hem de IPv4 desteklenir. | Yalnızca IPv4 desteklenir. |
|  | Statik MAC adresi | Desteklenmiyor | Desteklenmiyor. Her Azure Stack sistem aynı MAC adresi havuzunu kullanır. |
| Ağ İzleyicisi | Ağ Izleyicisi kiracı ağı izleme özellikleri | Desteklenen | Henüz desteklenmiyor. |
| CDN | Content Delivery Network profilleri | Desteklenen | Henüz desteklenmiyor. |
| Uygulama ağ geçidi | Katman 7 Yük Dengeleme | Desteklenen | Henüz desteklenmiyor. |
| Traffic Manager | En iyi uygulama performansı ve güvenilirliği için gelen trafiği yönlendirin. | Desteklenen | Henüz desteklenmiyor. |
| Express Route | Şirket içi altyapınız veya birlikte bulundurma olanınızdan Microsoft bulut hizmetlerine hızlı ve özel bir bağlantı kurun. | Desteklenen | Azure Stack bir Express Route devresine bağlama desteği. |

## <a name="api-versions"></a>API sürümleri 

Azure Stack ağ aşağıdaki API sürümlerini destekler: 

- 2018-11-01
- 2018-10-01
- 2018-08-01
- 2018-07-01
- 2018-06-01
- 2018-05-01
- 2018-04-01
- 2018-03-01
- 2018-02-01
- 2018-01-01
- 2017-11-01
- 2017-10-01

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack’te DNS](azure-stack-dns.md)
