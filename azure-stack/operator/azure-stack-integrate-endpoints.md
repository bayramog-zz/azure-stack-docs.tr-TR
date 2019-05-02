---
title: Azure Stack veri merkezi tümleştirmesi - uç noktalarını yayımlama | Microsoft Docs
description: Veri merkezinizde Azure Stack uç noktalarını yayımlama hakkında bilgi edinin
services: azure-stack
author: jeffgilb
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 02/06/2019
ms.author: jeffgilb
ms.reviewer: wamota
ms.lastreviewed: 02/06/2019
ms.openlocfilehash: 1f87633568a5a66c016b53f062f11ca733730092
ms.sourcegitcommit: 85c3acd316fd61b4e94c991a9cd68aa97702073b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2019
ms.locfileid: "64985218"
---
# <a name="azure-stack-datacenter-integration---publish-endpoints"></a>Azure Stack veri merkezi tümleştirmesi - uç noktalarını yayımlama

Azure Stack altyapısını rollerinden sanal IP adresleri (VIP) ayarlar. Bu Vıp'lerin genel IP adresi havuzundan ayrılır. Her bir VIP yazılım tanımlı ağ katmanında bir erişim denetimi listesi (ACL) ile korunmaktadır. ACL, fiziksel anahtarlar (Zleyicileri ve BMC) arasında daha fazla çözüm sağlamlaştırmak için de kullanılır. Dağıtım sırasında belirtilen dış DNS bölge içindeki her bir uç nokta için bir DNS girişi oluşturulur.


Aşağıdaki Mimari diyagram, ACL'ler ve farklı ağ katmanları gösterilmektedir:

![Yapısal resmi](media/azure-stack-integrate-endpoints/Integrate-Endpoints-01.png)

## <a name="ports-and-protocols-inbound"></a>Bağlantı noktaları ve protokoller (gelen)

Altyapı VIP'ler kümesidir dış ağlara Azure Stack uç noktalarını yayımlama için gerekli. *Uç nokta (VIP)* tabloda gösterilmektedir her uç nokta, gerekli bağlantı noktası ve protokol. SQL kaynak sağlayıcısı gibi ek kaynak sağlayıcıları gerekli uç noktaları için özel kaynak sağlayıcısı dağıtım belgelerine bakın.

Bunlar yayımlama Azure Stack için gerekli değil çünkü VIP'ler listelenmemiş iç altyapı.

> [!Note]  
> Azure Stack operatörü tarafından hiçbir denetimiyle kullanıcıların kendileri tarafından tanımlanan kullanıcı VIP'ler dinamik.

> [!Note]
> 1811 güncelleştirmesi itibarıyla 12495 30015 çeşitli nda bağlantı noktaları artık eklenmesi nedeniyle açık olması gerekmez [uzantısı konağı](azure-stack-extension-host-prepare.md).

|Uç nokta (VIP)|DNS ana bilgisayar bir kaydı|Protokol|Bağlantı Noktaları|
|---------|---------|---------|---------|
|AD FS|ADFS.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443|
|Portal (Yönetici)|Adminportal.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443|
|Adminhosting | *.adminhosting.\<region>.\<fqdn> | HTTPS | 443 |
|Azure Resource Manager (Yönetici)|Adminmanagement.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443|
|Portal (kullanıcı)|Portalı.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443|
|Azure Resource Manager (kullanıcı)|Management.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Graf|Graph.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Sertifika iptal listesi|Crl.*&lt;region>.&lt;fqdn>*|HTTP|80|
|DNS|&#42;.  *&lt;bölge >.&lt; FQDN >*|TCP VE UDP|53|
|Barındırma | *.hosting.\<region>.\<fqdn> | HTTPS | 443 |
|Key Vault (kullanıcı)|&#42;.vault.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Key Vault (Yönetici)|&#42;.adminvault.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Depolama Kuyruğu|&#42;.queue.*&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|Depolama tablosu|&#42;.Table.  *&lt;bölge >.&lt; FQDN >*|HTTP<br>HTTPS|80<br>443|
|Depolama Blobu|&#42;.BLOB.  *&lt;bölge >.&lt; FQDN >*|HTTP<br>HTTPS|80<br>443|
|SQL Kaynak Sağlayıcısı|sqladapter.dbadapter.  *&lt;bölge >.&lt; FQDN >*|HTTPS|44300-44304|
|MySQL Kaynak Sağlayıcısı|mysqladapter.dbadapter.*&lt;region>.&lt;fqdn>*|HTTPS|44300-44304|
|App Service|&#42;.appservice.*&lt;region>.&lt;fqdn>*|TCP|80 (HTTP)<br>443 (HTTPS)<br>8172 (MSDeploy)|
|  |&#42;.scm.appservice.*&lt;region>.&lt;fqdn>*|TCP|443 (HTTPS)|
|  |api.appservice.  *&lt;bölge >.&lt; FQDN >*|TCP|443 (HTTPS)<br>44300 (Azure Resource Manager)|
|  |FTP.appservice.  *&lt;bölge >.&lt; FQDN >*|TCP, UDP|21, 1021, 10001-10100 (FTP)<br>990 (FTPS)|
|VPN Ağ Geçitleri|     |     |[VPN gateway SSS bkz](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vpn-faq#can-i-traverse-proxies-and-firewalls-using-point-to-site-capability).|
|     |     |     |     |

## <a name="ports-and-urls-outbound"></a>Bağlantı noktaları ve URL'ler (giden)

Azure Stack, yalnızca saydam proxy sunucuları destekler. Bir dağıtımda saydam proxy yukarı bağlantılar burada geleneksel proxy sunucusu için aşağıdaki bağlantı noktaları ve URL'ler giden iletişime izin vermeniz gerekir:

> [!Note]  
> Azure Stack aşağıdaki tabloda listelenen Azure hizmetlerine erişmek için ExpressRoute kullanma desteği olmamasıdır.

|Amaç|Hedef URL|Protokol|Bağlantı Noktaları|Kaynak Ağ|
|---------|---------|---------|---------|---------|
|Kimlik|login.windows.net<br>login.microsoftonline.com<br>Graph.Windows.NET<br>https:\//secure.aadcdn.microsoftonline-p.com<br>Office.com|HTTP<br>HTTPS|80<br>443|Genel VIP - en az/27<br>Ortak ağ alt yapısı|
|Market sendikasyonu|https:\//management.azure.com<br>https://&#42;.blob.core.windows.net<br>https://*.azureedge.net<br>https://&#42;.microsoftazurestack.com|HTTPS|443|Genel VIP - en az/27|
|Düzeltme Eki & Güncelleştir|https://&#42;.azureedge.net|HTTPS|443|Genel VIP - en az/27|
|Kayıt|https:\//management.azure.com|HTTPS|443|Genel VIP - en az/27|
|Kullanım|https://&#42;.microsoftazurestack.com<br>https://*.trafficmanager.net |HTTPS|443|Genel VIP - en az/27|
|Windows Defender|\*.wdcp.microsoft.com<br>\*. wdcpalt.microsoft.com<br>\*. wd.microsoft.com<br>\*. update.microsoft.com<br>\*. download.microsoft.com<br>https:\//www.microsoft.com/pkiops/crl<br>https:\//www.microsoft.com/pkiops/certs<br>https:\//crl.microsoft.com/pki/crl/products<br>https:\//www.microsoft.com/pki/certs<br>https:\//secure.aadcdn.microsoftonline-p.com<br>|HTTPS|80<br>443|Genel VIP - en az/27<br>Ortak ağ alt yapısı|
|NTP|(IP, NTP sunucusu dağıtımı için sağlanan)|UDP|123|Genel VIP - en az/27|
|DNS|(IP, DNS sunucusu dağıtımı için sağlanan)|TCP<br>UDP|53|Genel VIP - en az/27|
|CRL|(URL, sertifikadaki CRL dağıtım noktaları altında)|HTTP|80|Genel VIP - en az/27|
|LDAP|Active Directory Graph entegrasyonuna yönelik sağlanan orman|TCP<br>UDP|389|Genel VIP - en az/27|
|LDAP SSL|Active Directory Graph entegrasyonuna yönelik sağlanan orman|TCP|636|Genel VIP - en az/27|
|LDAP GC|Active Directory Graph entegrasyonuna yönelik sağlanan orman|TCP|3268|Genel VIP - en az/27|
|LDAP GC SSL|Active Directory Graph entegrasyonuna yönelik sağlanan orman|TCP|3269|Genel VIP - en az/27|
|AD FS|AD FS meta veri uç noktası için AD FS tümleştirme sağlanan|TCP|443|Genel VIP - en az/27|
|     |     |     |     |     |

> [!Note]  
> Giden URL'leri, coğrafi konuma göre en iyi olası bağlantı sağlamak için Azure traffic manager'ı kullanarak Yük Dengelemesi yapılıyor. Yük dengeli URL'leri, Microsoft update ve arka uç, uç müşterileri etkilemeden değiştirme. Yük dengeli Küme URL'leri için Microsoft IP adreslerinin listesi paylaşmaz. URL yerine IP göre filtreleme destekleyen bir cihaz kullanmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack PKI gereksinimleri](azure-stack-pki-certs.md)
