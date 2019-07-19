---
title: Azure Stack veri merkezi tümleştirmesi-uç noktaları yayımlama | Microsoft Docs
description: Veri merkezinizde Azure Stack uç noktaları yayımlamayı öğrenin
services: azure-stack
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 07/18/2019
ms.author: mabrigg
ms.reviewer: wamota
ms.lastreviewed: 07/18/2019
ms.openlocfilehash: 81115a0bb82792a246d191c1cf68a99f44030e12
ms.sourcegitcommit: cb2376ed76c784e475b99352a024eaa7a148f42f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68328738"
---
# <a name="azure-stack-datacenter-integration---publish-endpoints"></a>Azure Stack veri merkezi tümleştirmesi-uç noktaları yayımlama

Azure Stack altyapı rolleri için sanal IP adresleri (VIP) ayarlar. Bu VIP 'ler genel IP adresi havuzundan ayrılır. Her VIP, yazılım tanımlı ağ katmanında bir erişim denetim listesi (ACL) ile güvenli hale getirilir. ACL 'Ler, çözümün daha fazla olması için fiziksel anahtarlar (lar ve BMC) genelinde da kullanılır. Dağıtım zamanında belirtilen dış DNS bölgesindeki her bir uç nokta için bir DNS girişi oluşturulur.


Aşağıdaki mimari diyagramda farklı ağ katmanları ve ACL 'Leri gösterilmektedir:

![Yapısal resim](media/azure-stack-integrate-endpoints/Integrate-Endpoints-01.png)

## <a name="ports-and-protocols-inbound"></a>Bağlantı noktaları ve protokoller (gelen)

Dış ağlara Azure Stack uç noktaları yayımlamak için bir altyapı VIP kümesi gereklidir. *Uç nokta (VIP)* tablosunda her uç nokta, gerekli bağlantı noktası ve protokol gösterilmektedir. SQL kaynak sağlayıcısı gibi ek kaynak sağlayıcıları gerektiren uç noktalar için belirli kaynak sağlayıcısı dağıtım belgelerine bakın.

Azure Stack yayımlamak için gerekli olmadıklarından iç altyapı VIP 'leri listelenmez.

> [!Note]  
> Kullanıcı VIP 'leri, kullanıcılar tarafından Azure Stack işleçle hiçbir denetim olmadan tanımlanmış dinamik bir şekilde tanımlanır.

> [!Note]  
> IKEv2 VPN. Standart tabanlı bir IPsec VPN çözümü olan IKEv2 VPN, UDP bağlantı noktası 500 ve 4500 ile 50 numaralı IP protokolünü kullanır. Güvenlik duvarları her zaman bu bağlantı noktalarını açmaz ve bu nedenle IKEv2 VPN’nin proxy ile güvenlik duvarlarını geçememe olasılığı vardır.

> [!Note]  
> 1811 güncelleştirmesinde itibariyle, [uzantı konağının](azure-stack-extension-host-prepare.md)eklenmesi nedeniyle, 12495-30015 aralığındaki bağlantı noktalarının artık açık olması gerekmez.

|Uç nokta (VIP)|DNS ana bilgisayar A kaydı|Protocol|Bağlantı Noktaları|
|---------|---------|---------|---------|
|AD FS|FS. *Bölge >.&lt; &lt; FQDN >*|HTTPS|443|
|Portal (yönetici)|Adminportal. *Bölge >.&lt; &lt; FQDN >*|HTTPS|443|
|Adminhosting | *. adminhosting. \<bölge >. \<FQDN > | HTTPS | 443 |
|Azure Resource Manager (yönetici)|Adminmanagement. *Bölge >.&lt; &lt; FQDN >*|HTTPS|443|
|Portal (Kullanıcı)|Portal. *Bölge >.&lt; &lt; FQDN >*|HTTPS|443|
|Azure Resource Manager (Kullanıcı)|Yönetme. *Bölge >.&lt; &lt; FQDN >*|HTTPS|443|
|Graf|Çıkarılamıyor. *Bölge >.&lt; &lt; FQDN >*|HTTPS|443|
|Sertifika iptal listesi|CRL. *&lt;Region >.&lt; FQDN >*|HTTP|80|
|DNS|&#42;. *Bölge >.&lt; &lt; FQDN >*|TCP & UDP|53|
|Barındırma | *. barındırma. \<bölge >. \<FQDN > | HTTPS | 443 |
|Key Vault (Kullanıcı)|&#42;ka. *Bölge >.&lt; &lt; FQDN >*|HTTPS|443|
|Key Vault (yönetici)|&#42;. adminkasa. *Bölge >.&lt; &lt; FQDN >*|HTTPS|443|
|Depolama Kuyruğu|&#42;Sıradaki. *Bölge >.&lt; &lt; FQDN >*|HTTP<br>HTTPS|80<br>443|
|Depolama tablosu|&#42;tablosundan. *Bölge >.&lt; &lt; FQDN >*|HTTP<br>HTTPS|80<br>443|
|Depolama Blobu|&#42;Bun. *Bölge >.&lt; &lt; FQDN >*|HTTP<br>HTTPS|80<br>443|
|SQL Kaynak Sağlayıcısı|sqladapter. dbadapter. *Bölge >.&lt; &lt; FQDN >*|HTTPS|44300-44304|
|MySQL Kaynak Sağlayıcısı|mysqladapter.dbadapter. *&lt;region>.&lt;fqdn>*|HTTPS|44300-44304|
|App Service|&#42;appservice. *Bölge >.&lt; &lt; FQDN >*|TCP|80 (HTTP)<br>443 (HTTPS)<br>8172 (MSDeploy)|
|  |&#42;. scm. appservice. *Bölge >.&lt; &lt; FQDN >*|TCP|443 (HTTPS)|
|  |api. appservice. *Bölge >.&lt; &lt; FQDN >*|TCP|443 (HTTPS)<br>44300 (Azure Resource Manager)|
|  |FTP. appservice. *Bölge >.&lt; &lt; FQDN >*|TCP, UDP|21, 1021, 10001-10100 (FTP)<br>990 (FTPS)|
|VPN Ağ Geçitleri|     |     |[Bkz. VPN Gateway SSS](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vpn-faq#can-i-traverse-proxies-and-firewalls-using-point-to-site-capability).|
|     |     |     |     |

## <a name="ports-and-urls-outbound"></a>Bağlantı noktaları ve URL 'Ler (giden)

Azure Stack yalnızca saydam proxy sunucuları destekler. Saydam bir proxy 'nin geleneksel bir ara sunucu ile olan bir dağıtımda, giden iletişim için aşağıdaki bağlantı noktalarına ve URL 'Lere izin vermeniz gerekir:

> [!Note]  
> Azure Stack, aşağıdaki tabloda listelenen Azure hizmetlerine ulaşmak için ExpressRoute kullanımını desteklemez.

|Amaç|Hedef URL 'SI|Protocol|Bağlantı Noktaları|Kaynak ağ|
|---------|---------|---------|---------|---------|
|Kimlik|login.windows.net<br>login.microsoftonline.com<br>Graph.Windows.NET<br>https:\//secure.aadcdn.microsoftonline-p.com<br>www.office.com|HTTP<br>HTTPS|80<br>443|Genel VIP-/27<br>Ortak altyapı ağı|
|Market dağıtımı|https:\//management.azure.com<br>https://&#42;.blob.core.windows.net<br>https://*.azureedge.net<br>https://&#42;.microsoftazurestack.com|HTTPS|443|Genel VIP-/27|
|Düzeltme Eki & güncelleştirmesi|https://&#42;.azureedge.net<br>https:\//aka.MS/azurestackautomaticupdate|HTTPS|443|Genel VIP-/27|
|Kayıt|https:\//management.azure.com|HTTPS|443|Genel VIP-/27|
|Kullanım|https://*. trafficmanager. net |HTTPS|443|Genel VIP-/27|
|Windows Defender|\*. wdcp.microsoft.com<br>\*. wdcpalt.microsoft.com<br>\*. wd.microsoft.com<br>\*. update.microsoft.com<br>\*. download.microsoft.com<br>https:\//www.Microsoft.com/pkiops/CRL<br>https:\//www.Microsoft.com/pkiops/certs<br>https:\//CRL.Microsoft.com/pki/CRL/Products<br>https:\//www.Microsoft.com/pki/certs<br>https:\//secure.aadcdn.microsoftonline-p.com<br>|HTTPS|80<br>443|Genel VIP-/27<br>Ortak altyapı ağı|
|NTP|(Dağıtım için belirtilen NTP sunucusu IP 'si)|UDP|123|Genel VIP-/27|
|DNS|(Dağıtım için belirtilen DNS sunucusu IP 'si)|TCP<br>UDP|53|Genel VIP-/27|
|KULLANIMA|(Sertifikanıza CRL dağıtım noktaları altında URL)|HTTP|80|Genel VIP-/27|
|LDAP|Graph tümleştirmesi için belirtilen Active Directory ormanı|TCP<br>UDP|389|Genel VIP-/27|
|LDAP SSL|Graph tümleştirmesi için belirtilen Active Directory ormanı|TCP|636|Genel VIP-/27|
|LDAP GC|Graph tümleştirmesi için belirtilen Active Directory ormanı|TCP|3268|Genel VIP-/27|
|LDAP GC SSL|Graph tümleştirmesi için belirtilen Active Directory ormanı|TCP|3269|Genel VIP-/27|
|AD FS|AD FS tümleştirmesi için belirtilen meta veri uç noktası AD FS|TCP|443|Genel VIP-/27|
|Tanılama günlüğü toplama hizmeti|Azure depolama tarafından belirtilen blob SAS URL 'SI|HTTPS|443|Genel VIP-/27|
|     |     |     |     |     |

Giden URL 'Ler, coğrafi konuma göre mümkün olan en iyi bağlantıyı sağlamak için Azure Traffic Manager kullanılarak yük dengelenebilir. Yük dengeli URL 'lerle, Microsoft, müşterileri etkilemeden arka uç uç noktalarını güncelleştirebilir ve değiştirebilir. Microsoft, yük dengeli URL 'Ler için IP adresleri listesini paylaşmaz. IP yerine, URL 'ye göre filtrelemeyi destekleyen bir cihaz kullanmanız gerekir.

Giden DNS her zaman gereklidir, bu, dış DNS sorgulama kaynağı ve ne tür kimlik tümleştirmesi seçilmiş olduğunu gösterir. Bu bir bağlı senaryolarsa, BMC ağı üzerinde bulunan DVD 'nin dağıtımı sırasında, giden erişime ve dağıtımdan sonra DNS hizmeti, sorguları ortak bir VIP aracılığıyla gönderecek bir iç bileşene taşımış olur. Bu sırada, BMC ağı üzerinden giden DNS erişimi kaldırılabilir, ancak söz konusu DNS sunucusuna genel VIP erişiminin kalması gerekir, aksi takdirde kimlik doğrulaması başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack PKI gereksinimleri](azure-stack-pki-certs.md)
