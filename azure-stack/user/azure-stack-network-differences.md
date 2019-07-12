---
title: Azure Stack ağ farkları | Microsoft Docs
description: Azure stack'teki ağ ile çalışırken farklılıklar ve dikkat edilmesi gerekenler hakkında bilgi edinin.
services: azure-stack
keywords: ''
author: mattbriggs
manager: femila
ms.date: 06/17/2019
ms.topic: article
ms.service: azure-stack
ms.author: mabrigg
ms.reviewer: scottnap
ms.lastreviewed: 06/04/2019
ms.openlocfilehash: 06b61bf80b2c123413425fc3abdcda12961d096c
ms.sourcegitcommit: b36d078e699c7924624b79641dbe9021af9606ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67816229"
---
# <a name="differences-and-considerations-for-azure-stack-networking"></a>Farklılıklar ve Azure Stack ağ iletişimi için dikkat edilmesi gerekenler

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack ağ iletişimi, Azure ağ tarafından sağlanan özelliklerin birçoğuna sahiptir. Ancak, bir Azure Stack ağ dağıtmadan önce anlamanız gereken bazı temel farklar vardır.

Bu makalede, Azure Stack ağ iletişimi ve özelliklerini benzersiz konularına genel bir bakış sağlar. Azure Stack ve Azure arasında üst düzey farklılıklar hakkında bilgi edinmek için bkz. [anahtar konuları](azure-stack-considerations.md) makalesi.

## <a name="cheat-sheet-networking-differences"></a>Kopya kağıdı: Ağ farkları

| Hizmet | Özellik | Azure (Genel) | Azure Stack |
|--------------------------|----------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| DNS | Çok kiracılı DNS | Desteklenen | Henüz desteklenmiyor |
|  | AAAA DNS kayıtları | Desteklenen | Desteklenmiyor |
|  | Abonelik başına DNS bölgeleri | 100 (varsayılan)<br>İstek üzerine artırılabilir. | 100 |
|  | Her bölge için DNS kayıt kümeleri | 5000 (varsayılan)<br>İstek üzerine artırılabilir. | 5000 |
|  | Bölge temsilcisi seçme için ad sunucuları | Azure, oluşturulan her kullanıcı (Kiracı) bölge için dört ad sunucusunun sağlar. | Azure Stack, oluşturulan her kullanıcı (Kiracı) bölge için iki ad sunucuları sağlar. |
| Azure Güvenlik Duvarı | Ağ güvenlik hizmeti | Azure Güvenlik Duvarı, Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir güvenlik hizmetidir. | Henüz desteklenmiyor. |
| Sanal Ağ | Sanal ağ eşleme | Aynı bölgedeki iki sanal ağı Azure omurga ağı aracılığıyla birbirine bağlayın. | Henüz desteklenmiyor |
|  | IPv6 adresleri | Bir IPv6 adresi bir parçası olarak atayabilirsiniz [ağ arabirimi yapılandırması](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface-addresses#ip-address-versions). | Yalnızca IPv4 desteklenir. |
|  | DDoS koruma planı | Desteklenen | Henüz desteklenmiyor. |
|  | Ölçek kümesi IP yapılandırmaları | Desteklenen | Henüz desteklenmiyor. |
|  | Özel Erişim Hizmetleri (alt ağ) | Desteklenen | Henüz desteklenmiyor. |
|  | Hizmet Uç Noktaları | Azure Hizmetleri için iç (Internet olmayan) bağlantısı için desteklenir. | Henüz desteklenmiyor. |
|  | Hizmet Uç Noktası İlkeleri | Desteklenen | Henüz desteklenmiyor. |
|  | Hizmet tüneli | Desteklenen | Henüz desteklenmiyor.  |
| Ağ Güvenlik Grupları | Genişletilmiş Güvenlik kuralları | Desteklenen | Henüz desteklenmiyor. |
|  | Geçerli güvenlik kuralları | Desteklenen | Henüz desteklenmiyor. |
|  | Uygulama Güvenlik Grupları | Desteklenen | Henüz desteklenmiyor. |
| Sanal Ağ Geçitleri | Noktadan siteye VPN ağ geçidi | Desteklenen | Henüz desteklenmiyor. |
|  | Vnet-Vnet ağ geçidi | Desteklenen | Henüz desteklenmiyor. |
|  | Sanal ağ geçidi türü | Azure'un destekledikleri VPN<br> Express Route <br> Hiper Net. | Azure Stack, şu anda yalnızca VPN türünü destekler. |
|  | VPN ağ geçidi SKU'ları | Temel, GW1, GW2, GW3, standart, yüksek performanslı, Ultra yüksek performans için destek. | Temel, standart ve yüksek performanslı SKU'ları için destek. |
|  | VPN türü | Hem ilke tabanlı ve rota tabanlı Azure destekler. | Azure Stack rota tabanlı yalnızca destekler. |
|  | BGP ayarları | Azure, BGP eşlemesi adres ve eş ağırlığı yapılandırılmasını destekler. | BGP eşleme adresi ve eş ağırlığı Azure Stack'te otomatik olarak yapılandırılır. Kullanıcının kendi değerlerle bu ayarları yapılandırmak hiçbir yolu yoktur. |
|  | Varsayılan ağ geçidi sitesi | Azure, zorlamalı tünel için varsayılan site yapılandırmasını destekler. | Henüz desteklenmiyor. |
|  | Ağ geçidini yeniden boyutlandırma | Azure, dağıtımdan sonra ağ geçidini yeniden boyutlandırma destekler. | Desteklenen yeniden boyutlandırma değil. |
|  | Kullanılabilirlik yapılandırması | Etkin/etkin | Aktif/Pasif |
|  | UsePolicyBasedTrafficSelectors | Azure, ilke tabanlı trafik seçicileri ile rota tabanlı ağ geçidi bağlantıları kullanmayı destekler. | Henüz desteklenmiyor. |
| Yük dengeleyici | SKU | Temel ve standart Load balancer'ları desteklenir. | Temel yük dengeleyici desteklenir.  SKU özelliği desteklenmiyor. |
|  | Bölgeler | Kullanılabilirlik alanları desteklenir. | Henüz desteklenmiyor |
|  | Hizmet uç noktaları için gelen NAT kuralları desteği | Azure belirten hizmet uç noktaları için gelen NAT kurallarını destekler. | Bunlar belirtilemez bu nedenle azure Stack, hizmet uç noktaları, henüz desteklemiyor. |
|  | Protocol | GRE veya ESP belirterek azure destekler. | Azure Stack'te Protokolü sınıfı desteklenmiyor. |
| Genel IP Adresi | Genel IP adresi sürümü | Azure, hem IPv6 hem de IPv4 destekler. | Yalnızca IPv4 desteklenir. |
| Ağ Arabirimi | Etkili rota tablosunu alın | Desteklenen | Henüz desteklenmiyor. |
|  | Geçerli ACL'ler Al | Desteklenen | Henüz desteklenmiyor. |
|  | Hızlandırılmış ağ iletişimi etkinleştirin | Desteklenen | Henüz desteklenmiyor. |
|  | IP İletimi | Varsayılan olarak devre dışıdır.  Etkinleştirilebilir. | Bu ayarın açılıp desteklenmez.  Üzerinde varsayılan olarak. |
|  | Uygulama Güvenlik Grupları | Desteklenen | Henüz desteklenmiyor. |
|  | İç DNS ad etiketi | Desteklenen | Henüz desteklenmiyor. |
|  | Özel IP adresi sürümü | IPv6 ve IPv4 desteklenir. | Yalnızca IPv4 desteklenir. |
|  | Statik MAC adresi | Desteklenmiyor | Desteklenmiyor. Her Azure Stack sistemi aynı MAC adresi havuzunu kullanır. |
| Ağ İzleyicisi | Ağ İzleyicisi Kiracı ağ izleme kapasiteleri | Desteklenen | Henüz desteklenmiyor. |
| CDN | Content Delivery Network profilleri | Desteklenen | Henüz desteklenmiyor. |
| Uygulama ağ geçidi | 7\. Katman Yük Dengeleme | Desteklenen | Henüz desteklenmiyor. |
| Traffic Manager | En iyi uygulama performansı ve güvenilirliği için gelen trafiği yönlendirin. | Desteklenen | Henüz desteklenmiyor. |
| Express Route | Hızlı ve özel bağlantı kurmak, şirket içi altyapı veya ortak yerleşim tesisinizden Microsoft bulut hizmetlerine ayarlayın. | Desteklenen | Bir Express Route bağlantı hattı için Azure Stack bağlamak için destek. |

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack’te DNS](azure-stack-dns.md)
