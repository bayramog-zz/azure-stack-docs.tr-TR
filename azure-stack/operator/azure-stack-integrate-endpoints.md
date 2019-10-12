---
title: Veri merkezinizde Azure Stack Hizmetleri yayımlayın | Microsoft Docs
description: Veri merkezinizde Azure Stack Hizmetleri yayımlamayı öğrenin.
services: azure-stack
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 09/09/2019
ms.author: justinha
ms.reviewer: wamota
ms.lastreviewed: 09/09/2019
ms.openlocfilehash: cfd9434bc52684f89617eff3b62a7bf51fc68bcd
ms.sourcegitcommit: a6d47164c13f651c54ea0986d825e637e1f77018
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72277436"
---
# <a name="publish-azure-stack-services-in-your-datacenter"></a>Veri merkezinizde Azure Stack Hizmetleri yayımlayın 

Azure Stack altyapı rolleri için sanal IP adresleri (VIP) ayarlar. Bu VIP 'ler genel IP adresi havuzundan ayrılır. Her VIP, yazılım tanımlı ağ katmanında bir erişim denetim listesi (ACL) ile güvenli hale getirilir. ACL 'Ler, çözümün daha fazla olması için fiziksel anahtarlar (lar ve BMC) genelinde da kullanılır. Dağıtım zamanında belirtilen dış DNS bölgesindeki her bir uç nokta için bir DNS girişi oluşturulur. Örneğin, Kullanıcı portalına portalın DNS ana bilgisayar girişi atanır. *&lt;region >. &lt;fqdn >* .

Aşağıdaki mimari diyagramda farklı ağ katmanları ve ACL 'Leri gösterilmektedir:

![Farklı ağ katmanları ve ACL 'Leri gösteren diyagram](media/azure-stack-integrate-endpoints/Integrate-Endpoints-01.png)

### <a name="ports-and-urls"></a>Bağlantı noktaları ve URL 'Ler
Azure Stack Hizmetleri (portallar, Azure Resource Manager, DNS vb. gibi) dış ağlarda kullanılabilir hale getirmek için belirli URL 'Ler, bağlantı noktaları ve protokoller için bu uç noktalara gelen trafiğe izin vermeniz gerekir.
 
Geleneksel bir ara sunucu veya bir güvenlik duvarının, çözümü koruduğu bir dağıtımda, hem [gelen](azure-stack-integrate-endpoints.md#ports-and-protocols-inbound) hem de [giden](azure-stack-integrate-endpoints.md#ports-and-urls-outbound) iletişim için belirli bağlantı noktalarına ve URL 'lere izin vermeniz gerekir. Bunlar kimlik bağlantı noktaları ve URL 'Ler, Market, düzeltme eki ve güncelleştirme, kayıt ve kullanım verileri içerir.

## <a name="ports-and-protocols-inbound"></a>Bağlantı noktaları ve protokoller (gelen)

Dış ağlara Azure Stack uç noktaları yayımlamak için bir altyapı VIP kümesi gereklidir. *Uç nokta (VIP)* tablosunda her uç nokta, gerekli bağlantı noktası ve protokol gösterilmektedir. SQL kaynak sağlayıcısı gibi ek kaynak sağlayıcıları gerektiren uç noktalar için belirli kaynak sağlayıcısı dağıtım belgelerine bakın.

Azure Stack yayımlamak için gerekli olmadıklarından iç altyapı VIP 'leri listelenmez. Kullanıcı VIP 'leri, Azure Stack işleci tarafından denetim olmadan dinamik ve kullanıcılar tarafından tanımlanır.

> [!Note]  
> IKEv2 VPN, UDP bağlantı noktası 500 ve 4500 ve TCP bağlantı noktası 50 kullanan standart tabanlı bir IPSec VPN çözümüdür. Güvenlik duvarları bu bağlantı noktalarını her zaman açmaz, bu nedenle IKEv2 VPN, proxy 'leri ve güvenlik duvarlarını geçemeyebilir.

[Uzantı konağının](azure-stack-extension-host-prepare.md)eklenmesiyle, 12495-30015 aralığındaki bağlantı noktaları gerekli değildir.

|Uç nokta (VIP)|DNS ana bilgisayar A kaydı|Protokol|Bağlantı Noktaları|
|---------|---------|---------|---------|
|AD FS|FS. *&lt;region >. &lt;fqdn >*|HTTPS|443|
|Portal (yönetici)|Adminportal. *&lt;region >. &lt;fqdn >*|HTTPS|443|
|Adminhosting | *. adminhosting. \<region >. \<fqdn > | HTTPS | 443 |
|Azure Resource Manager (yönetici)|Adminmanagement. *&lt;region >. &lt;fqdn >*|HTTPS|443|
|Portal (Kullanıcı)|Portal. *&lt;region >. &lt;fqdn >*|HTTPS|443|
|Azure Resource Manager (Kullanıcı)|Yönetme. *&lt;region >. &lt;fqdn >*|HTTPS|443|
|Çıkarılamıyor|Çıkarılamıyor. *&lt;region >. &lt;fqdn >*|HTTPS|443|
|Sertifika iptal listesi|CRL. *&lt;region >. &lt;fqdn >*|HTTP|80|
|DNS|&#42;. *&lt;region >. &lt;fqdn >*|TCP & UDP|53|
|Barındırma | *. Hosting. \<region >. \<fqdn > | HTTPS | 443 |
|Key Vault (Kullanıcı)|&#42;ka. *&lt;region >. &lt;fqdn >*|HTTPS|443|
|Key Vault (yönetici)|&#42;. adminkasa. *&lt;region >. &lt;fqdn >*|HTTPS|443|
|Depolama kuyruğu|&#42;Sıradaki. *&lt;region >. &lt;fqdn >*|HTTP<br>HTTPS|80<br>443|
|Depolama tablosu|&#42;tablosundan. *&lt;region >. &lt;fqdn >*|HTTP<br>HTTPS|80<br>443|
|Depolama Blobu|&#42;Bun. *&lt;region >. &lt;fqdn >*|HTTP<br>HTTPS|80<br>443|
|SQL kaynak sağlayıcısı|sqladapter. dbadapter. *&lt;region >. &lt;fqdn >*|HTTPS|44300-44304|
|MySQL kaynak sağlayıcısı|mysqladapter. dbadapter. *&lt;region >. &lt;fqdn >*|HTTPS|44300-44304|
|App Service|&#42;appservice. *&lt;region >. &lt;fqdn >*|TCP|80 (HTTP)<br>443 (HTTPS)<br>8172 (MSDeploy)|
|  |&#42;. scm. appservice. *&lt;region >. &lt;fqdn >*|TCP|443 (HTTPS)|
|  |api. appservice. *&lt;region >. &lt;fqdn >*|TCP|443 (HTTPS)<br>44300 (Azure Resource Manager)|
|  |FTP. appservice. *&lt;region >. &lt;fqdn >*|TCP, UDP|21, 1021, 10001-10100 (FTP)<br>990 (FTPS)|
|VPN ağ geçitleri|     |     |[Bkz. VPN Gateway SSS](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vpn-faq#can-i-traverse-proxies-and-firewalls-using-point-to-site-capability).|
|     |     |     |     |

## <a name="ports-and-urls-outbound"></a>Bağlantı noktaları ve URL 'Ler (giden)

Azure Stack yalnızca saydam proxy sunucuları destekler. Geleneksel bir ara sunucuya şeffaf bir ara sunucu yukarı bağlantısı olan dağıtımda, giden iletişim için aşağıdaki tablodaki bağlantı noktalarına ve URL 'Lere izin vermeniz gerekir.

> [!Note]  
> Azure Stack ExpressRoute, trafiği uç noktalara yönlendiremediği için aşağıdaki tabloda listelenen Azure hizmetlerine ulaşmak üzere ExpressRoute kullanmayı desteklemez.

|Amaç|Hedef URL 'SI|Protokol|Bağlantı Noktaları|Kaynak ağ|
|---------|---------|---------|---------|---------|
|Kimlik|**Azure**<br>login.windows.net<br>login.microsoftonline.com<br>graph.windows.net<br>https: \//Secure. aadcdn. microsoftonline-p. com<br>www.office.com<br>**Azure Kamu**<br>https: \//Login. microsoftonline. us/<br>https: \//Graph. Windows. net/<br>**Azure Çin 21Vianet**<br>https: \//Login. chinacloudapi. cn/<br>https: \//Graf. chinacloudapi. cn/<br>**Azure Almanya**<br>https: \//Login. microsoftonline. de/<br>https: \//Graf. cloudapi. de/|HTTP<br>HTTPS|80<br>443|Genel VIP-/27<br>Ortak altyapı ağı|
|Market dağıtımı|**Azure**<br>https: \//Management. Azure. com<br>https://&#42;. blob.Core.Windows.net<br>https://&#42;. azureedge.net<br>**Azure Kamu**<br>https: \//Management. usgovcloudapi. net/<br>https://&#42;. blob.Core.usgovcloudapi.net/<br>**Azure Çin 21Vianet**<br>https: \//Management. chinacloudapi. cn/<br>http://&#42;. blob.Core.chinacloudapi.cn|HTTPS|443|Genel VIP-/27|
|Düzeltme Eki & güncelleştirmesi|https://&#42;. azureedge.net<br>https: \//aka. MS/azurestackautomaticupdate|HTTPS|443|Genel VIP-/27|
|Kayıt|**Azure**<br>https: \//Management. Azure. com<br>**Azure Kamu**<br>https: \//Management. usgovcloudapi. net/<br>**Azure Çin 21Vianet**<br>https: \//Management. chinacloudapi. cn|HTTPS|443|Genel VIP-/27|
|Kullanım|**Azure**<br>https://&#42;. trafficmanager.net<br>**Azure Kamu**<br>https://&#42;. usgovtrafficmanager.net<br>**Azure Çin 21Vianet**<br>https://&#42;. trafficmanager.cn|HTTPS|443|Genel VIP-/27|
|Windows Defender|&#42;. wdcp.microsoft.com<br>&#42;. wdcpalt.microsoft.com<br>&#42;. wd.microsoft.com<br>&#42;. update.microsoft.com<br>&#42;. download.microsoft.com<br>https: \//www. Microsoft. com/pkıops/CRL<br>https: \//www. Microsoft. com/pkıops/CERT<br>https: \//CRL. Microsoft. com/PKI/CRL/ürün<br>https: \//www. Microsoft. com/PKI/CERT<br>https: \//Secure. aadcdn. microsoftonline-p. com<br>|HTTPS|80<br>443|Genel VIP-/27<br>Ortak altyapı ağı|
|NTP|(Dağıtım için belirtilen NTP sunucusu IP 'si)|UDP|123|Genel VIP-/27|
|DNS|(Dağıtım için belirtilen DNS sunucusu IP 'si)|TCP<br>UDP|53|Genel VIP-/27|
|KULLANıMA|(Sertifikanıza CRL dağıtım noktaları altında URL)|HTTP|80|Genel VIP-/27|
|LDAP|Graph tümleştirmesi için belirtilen Active Directory ormanı|TCP<br>UDP|389|Genel VIP-/27|
|LDAP SSL|Graph tümleştirmesi için belirtilen Active Directory ormanı|TCP|636|Genel VIP-/27|
|LDAP GC|Graph tümleştirmesi için belirtilen Active Directory ormanı|TCP|3268|Genel VIP-/27|
|LDAP GC SSL|Graph tümleştirmesi için belirtilen Active Directory ormanı|TCP|3269|Genel VIP-/27|
|AD FS|AD FS tümleştirmesi için belirtilen meta veri uç noktası AD FS|TCP|443|Genel VIP-/27|
|Tanılama günlüğü toplama hizmeti|Azure depolama tarafından belirtilen blob SAS URL 'SI|HTTPS|443|Genel VIP-/27|
|     |     |     |     |     |

Giden URL 'Ler, coğrafi konuma göre mümkün olan en iyi bağlantıyı sağlamak için Azure Traffic Manager kullanılarak yük dengelenebilir. Yük dengeli URL 'lerle, Microsoft, müşterileri etkilemeden arka uç uç noktalarını güncelleştirebilir ve değiştirebilir. Microsoft, yük dengeli URL 'Ler için IP adresi listesini paylaşmaz. IP yerine, URL 'ye göre filtrelemeyi destekleyen bir cihaz kullanın.

Her zaman giden DNS gereklidir; , dış DNS sorgulama kaynağı ve ne tür kimlik tümleştirmesi seçilmiş olduğunu gösterir. Bağlı bir senaryo için dağıtım sırasında, BMC ağında bulunan DVı 'nin giden erişimi olması gerekir. Ancak dağıtımdan sonra DNS hizmeti, ortak bir VIP aracılığıyla sorgu gönderecek bir iç bileşene geçer. Bu sırada, BMC ağı üzerinden giden DNS erişimi kaldırılabilir, ancak bu DNS sunucusuna genel VIP erişiminin kalması gerekir, aksi takdirde kimlik doğrulaması başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack PKI gereksinimleri](azure-stack-pki-certs.md)
