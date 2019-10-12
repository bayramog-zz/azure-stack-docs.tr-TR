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
ms.openlocfilehash: 79ec0994ecd99a5279fc0b3b032ef6872f921f38
ms.sourcegitcommit: a6d47164c13f651c54ea0986d825e637e1f77018
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72277801"
---
# <a name="differences-and-considerations-for-azure-stack-networking"></a>Azure Stack ağ oluşturma farklılıkları ve konuları

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack ağ, Azure ağ tarafından sunulan birçok özelliğe sahiptir. Ancak, bir Azure Stack ağı dağıtmadan önce anlamanız gereken bazı önemli farklılıklar vardır.

Bu makalede, Azure Stack ağ ve özellikleri için benzersiz noktalara genel bakış sunulmaktadır. Azure Stack ile Azure arasındaki üst düzey farklılıklar hakkında bilgi edinmek için bkz. [temel konular](azure-stack-considerations.md) makalesi.

## <a name="cheat-sheet-networking-differences"></a>Tek sayfa: ağ farklılıkları

| Hizmet | Özellik | Azure (genel) | Azure Stack |
|--------------------------|----------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| DNS | Çok kiracılı DNS | Desteklenir | Henüz desteklenmiyor |
|  | DNS AAAA kayıtları | Desteklenir | Desteklenmez |
|  | Abonelik başına DNS bölgeleri | 100 (varsayılan)<br>, İstek üzerine artırılabilir. | 100 |
|  | Bölge başına DNS kayıt kümesi | 5000 (varsayılan)<br>, İstek üzerine artırılabilir. | 5000 |
|  | Bölge temsili için ad sunucuları | Azure, oluşturulan her kullanıcı (kiracı) bölgesi için dört ad sunucusu sağlar. | Azure Stack, oluşturulan her kullanıcı (kiracı) bölgesi için iki ad sunucusu sağlar. |
| Azure Güvenlik Duvarı | Ağ güvenlik hizmeti | Azure Güvenlik Duvarı, Azure sanal ağ kaynaklarınızı koruyan, yönetilen, bulut tabanlı bir ağ güvenlik hizmetidir. | Henüz desteklenmiyor. |
| Sanal ağ | Sanal ağ eşlemesi | Azure omurga ağı aracılığıyla aynı bölgedeki iki sanal ağı birbirine bağlayın. | Henüz desteklenmiyor |
|  | IPv6 adresleri | [Ağ arabirimi yapılandırmasının](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface-addresses#ip-address-versions)bir parçası olarak bir IPv6 adresi atayabilirsiniz. | Yalnızca IPv4 desteklenir. |
|  | DDoS koruma planı | Desteklenir | Henüz desteklenmiyor. |
|  | Ölçek kümesi IP yapılandırması | Desteklenir | Henüz desteklenmiyor. |
|  | Özel erişim Hizmetleri (alt ağ) | Desteklenir | Henüz desteklenmiyor. |
|  | Hizmet uç noktaları | Azure hizmetlerine yönelik dahili (Internet olmayan) bağlantı için desteklenir. | Henüz desteklenmiyor. |
|  | Hizmet uç noktası Ilkeleri | Desteklenir | Henüz desteklenmiyor. |
|  | Hizmet tünelleri | Desteklenir | Henüz desteklenmiyor.  |
| Ağ güvenlik grupları | Genişletilmiş güvenlik kuralları | Desteklenir | Henüz desteklenmiyor. |
|  | Etkin güvenlik kuralları | Desteklenir | Henüz desteklenmiyor. |
|  | Uygulama güvenlik grupları | Desteklenir | Henüz desteklenmiyor. |
| Sanal ağ geçitleri | Noktadan siteye VPN Gateway | Desteklenir | Henüz desteklenmiyor. |
|  | VNET-VNET ağ geçidi | Desteklenir | Henüz desteklenmiyor. |
|  | Sanal ağ geçidi türü | Azure VPN 'yi destekler<br> Express Route <br> Hiper ağ. | Azure Stack Şu anda yalnızca VPN türünü destekliyor. |
|  | VPN Gateway SKU 'Ları | Temel, GW1, GW2, GW3, standart yüksek performans, ultra yüksek performans desteği. | Temel, standart ve yüksek performanslı SKU 'Lar için destek. |
|  | VPN türü | Azure, ilke tabanlı ve rota tabanlı ' i destekler. | Azure Stack yalnızca rota tabanlı desteği destekler. |
|  | BGP ayarları | Azure, BGP eşleme adresinin ve eş ağırlığın yapılandırılmasını destekler. | BGP eşleme adresi ve eş ağırlığı Azure Stack ' de otomatik olarak yapılandırılır. Kullanıcının bu ayarları kendi değerleriyle yapılandırması mümkün değildir. |
|  | Varsayılan ağ geçidi sitesi | Azure, Zorlamalı tünel için varsayılan bir sitenin yapılandırılmasını destekler. | Henüz desteklenmiyor. |
|  | Ağ geçidini yeniden boyutlandırma | Azure, dağıtımdan sonra ağ geçidini yeniden boyutlandırmayı destekler. | Yeniden boyutlandırma desteklenmiyor. |
|  | Kullanılabilirlik yapılandırması | Etkin/etkin | Etkin/Pasif |
|  | UsePolicyBasedTrafficSelectors | Azure, rota tabanlı ağ geçidi bağlantılarıyla ilke tabanlı trafik seçicileri kullanımını destekler. | Henüz desteklenmiyor. |
| Yük dengeleyici | ISTEYIN | Temel ve standart yük dengeleyiciler destekleniyor | Yalnızca temel Load Balancer desteklenir.<br>SKU özelliği desteklenmiyor.<br>Temel SKU yük dengeleyici/Path/, 5 ' ten fazla ön uç IP yapılandırmasına sahip olamaz.  |
|  | Bölgeler | Kullanılabilirlik Alanları desteklenir. | Henüz desteklenmiyor |
|  | Hizmet uç noktaları için gelen NAT kuralları desteği | Azure, gelen NAT kuralları için hizmet uç noktalarının belirtilmesini destekler. | Azure Stack henüz hizmet uç noktalarını desteklemez, bu nedenle bunlar belirtilemez. |
|  | Protokol | Azure, GRE veya ESP belirtmeyi destekler. | Protokol sınıfı Azure Stack desteklenmiyor. |
| Genel IP adresi | Genel IP adresi sürümü | Azure hem IPv6 hem de IPv4 destekler. | Yalnızca IPv4 desteklenir. |
| Ağ arabirimi | Geçerli yol tablosu al | Desteklenir | Henüz desteklenmiyor. |
|  | Etkin ACL 'Ler al | Desteklenir | Henüz desteklenmiyor. |
|  | Hızlandırılmış ağı etkinleştir | Desteklenir | Henüz desteklenmiyor. |
|  | IP Iletimi | Varsayılan olarak devre dışıdır.  Etkinleştirilebilir. | Bu ayar geçiş desteklenmiyor.  Varsayılan olarak açık. |
|  | Uygulama güvenlik grupları | Desteklenir | Henüz desteklenmiyor. |
|  | İç DNS ad etiketi | Desteklenir | Henüz desteklenmiyor. |
|  | Özel IP adresi sürümü | Hem IPv6 hem de IPv4 desteklenir. | Yalnızca IPv4 desteklenir. |
|  | Statik MAC adresi | Desteklenmez | Desteklenmez. Her Azure Stack sistem aynı MAC adresi havuzunu kullanır. |
| Ağ Izleyicisi | Ağ Izleyicisi kiracı ağı izleme özellikleri | Desteklenir | Henüz desteklenmiyor. |
| 'Ye | Content Delivery Network profilleri | Desteklenir | Henüz desteklenmiyor. |
| Uygulama ağ geçidi | Katman 7 Yük Dengeleme | Desteklenir | Henüz desteklenmiyor. |
| Traffic Manager | En iyi uygulama performansı ve güvenilirliği için gelen trafiği yönlendirin. | Desteklenir | Henüz desteklenmiyor. |
| Express Route | Şirket içi altyapınız veya birlikte bulundurma olanınızdan Microsoft bulut hizmetlerine hızlı ve özel bir bağlantı kurun. | Desteklenir | Azure Stack bir Express Route devresine bağlama desteği. |

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack DNS](azure-stack-dns.md)
