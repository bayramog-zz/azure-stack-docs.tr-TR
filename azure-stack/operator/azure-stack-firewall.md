---
title: Azure Stack güvenlik duvarı Azure Stack için planlama tümleşik sistemleri | Microsoft Docs
description: Azure Stack Azure bağlı çok düğümlü dağıtımlar için Azure Stack güvenlik duvarı konuları açıklanmaktadır.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: mabrigg
ms.reviewer: wfayed
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: a98f89866fd7679a0d6ce7c6ecb40d7e8e8b51e7
ms.sourcegitcommit: 2a4321a9cf7bef2955610230f7e057e0163de779
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65618676"
---
# <a name="azure-stack-firewall-integration"></a>Azure Stack güvenlik duvarı tümleştirmesi
Güvenli Azure yığını için bir güvenlik duvarı cihaz kullanmanız önerilir. Güvenlik duvarları, içerik denetleme dağıtılmış--hizmet reddi (DDOS) saldırıları ve izinsiz giriş algılama gibi şeyler karşı korumanıza yardımcı olur. Ancak, bunlar da bloblar, tablolar ve Kuyruklar gibi Azure depolama hizmetleri için bir aktarım hızı performans sorunu haline gelebilir.

 Bağlantısı kesilmiş dağıtım modu kullandıysanız, AD FS uç noktasına yayımlamanız gerekir. Daha fazla bilgi için [veri merkezi tümleştirmesi kimlik makale](azure-stack-integrate-identity.md).

Azure Resource Manager (Yönetici), Yönetici portalı ve anahtar Kasası'nı (Yönetici) uç noktalar dış yayımlama mutlaka gerektirmez. Örneğin, bir hizmet sağlayıcısı olarak, ağınızdaki ve internet'ten Azure yığını yalnızca yöneterek saldırı yüzeyini sınırlayabilir.

Büyük kuruluşlar için var olan kurumsal ağ ve dış ağa olabilir. Bu senaryoda, Azure Stack ve şirket ağından çalışılacak uç noktaları yayımlamanız gerekir.

### <a name="network-address-translation"></a>Ağ Adresi Çevirisi
Ağ adresi çevirisi (NAT) dağıtım sanal makine (dış kaynaklara erişmeyi DVM) ve dağıtım sırasında internet yanı sıra Acil Durum Kurtarma Konsolu (ERCS) Vm'leri ya da ayrıcalıklı uç noktasına (CESARETLENDİRİCİ) sırasında izin vermek için önerilen yöntemdir kayıt ve sorun giderme.

NAT dış ağdaki veya genel VIP genel IP adresleri için bir alternatif de olabilir. Ancak, Kiracı Kullanıcı deneyimini sınırlar ve karmaşıklık artar çünkü bunu yapmanız önerilmez. Bire bir NAT IP havuzu kullanıcı başına bir genel IP gerektiren bir seçenek olacaktır. Bir kullanıcı için tüm bağlantı VIP kullanıcı başına bir NAT kuralı gerektiren bir NAT için birçok başka bir seçenektir.

Bazı genel VIP için NAT kullanmanın olumsuzlukları şunlardır:
- NAT, kullanıcıların kendi uç noktaları ve yazılım tanımlı ağ (SDN) yığınında kendi yayımlama kuralları denetlemek için güvenlik duvarı kuralları yönetirken yükü ekler. Kullanıcıların yayımlanan kendi VIP'ler almak ve bağlantı noktası listesini güncelleştirmek için Azure Stack operatörü başvurmanız gerekir.
- NAT kullanımı, kullanıcı deneyimini sınırlar olsa da, tam denetim işlecine yayımlama istekleri üzerinden verir.
- Azure ile karma bulut senaryoları için Azure VPN tüneli NAT'ı kullanarak bir uç noktaya ayarlamayı desteklemiyor göz önünde bulundurun.

### <a name="ssl-decryption"></a>SSL şifre çözme
Şu anda, tüm Azure Stack trafiği üzerinde SSL şifre çözme devre dışı bırakmak için önerilir. Gelecekteki güncelleştirmelerinde destekleniyorsa, Azure Stack için SSL şifre çözme etkinleştirme hakkında yönergeler sağlanır.

## <a name="edge-firewall-scenario"></a>Kenar güvenlik duvarı senaryosu
Bir edge dağıtımı, Azure Stack doğrudan sınır yönlendiricisi veya güvenlik duvarı arkasında dağıtılır. Bu senaryolarda, burada hem aktif-aktif veya Aktif-Pasif güvenlik duvarı yapılandırmaları veya (2. Senaryo) kenarlık cihaz olarak burada etkin-etkin güvenlik duvarı yalnızca destekler acting destekler kenarlık (Senaryo 1) olacak şekilde güvenlik duvarı için desteklenir üzerinde eşit maliyet çoklu yol (ECMP) BGP ya da yük devretme için statik yönlendirme ile bağlı olan yapılandırma.

Genel olarak yönlendirilebilir IP adresleri için dış ağ genel VIP havuzundan dağıtım sırasında belirtilir. Bir edge senaryosunda, genel olarak yönlendirilebilir IP'leri herhangi bir ağ üzerindeki güvenlik amacıyla kullanmak için önerilmez. Bu senaryo Azure gibi genel bulut olduğu gibi tam Self denetimli bulut deneyimi deneyimi sağlar.  

![Azure Stack kenar güvenlik duvarı örneği](./media/azure-stack-firewall/firewallScenarios.png)

## <a name="enterprise-intranet-or-perimeter-network-firewall-scenario"></a>Kurumsal intranet veya çevre ağ güvenlik duvarı senaryoları
Bir Kurumsal intranet veya çevre dağıtımında, Azure Stack çoklu bölgesinin bir güvenlik duvarı veya sınır güvenlik duvarı ve dahili, kurumsal ağ güvenlik duvarı arasında dağıtılır. Trafik, ardından güvenli, çevre ağındaki (veya DMZ arasında) dağıtılır ve olarak güvenli olmayan bölgeler aşağıda açıklanan:

- **Güvenli Bölge**: Bu iç ya da şirket yönlendirilebilir IP adresleri kullanan iç ağdır. Güvenli ağ ayrılabilir, güvenlik duvarında giden internet erişimi NAT aracılığıyla sahip ve genellikle iç ağ aracılığıyla veri merkezi içinde her yerden erişilebilir. Tüm Azure Stack ağlar, genel VIP havuzunu dış ağın dışında güvenli bölgede bulunmalıdır.
- **Çevre bölge**. Çevre ağ dış veya internet'e yönelik Web sunucuları genellikle dağıtılır gibi uygulamalar olabilir. Genellikle, DDoS ve sağlarken hala belirtilen gelen trafiği internet'ten (deşifre etme) yetkisiz erişim gibi saldırıları önlemek için bir güvenlik duvarı tarafından izlenir. Yalnızca dış ağ genel VIP havuzunu Azure Stack DMZ bölgede bulunmalıdır.
- **Güvenli olmayan bölge**. Dış ağ, internet budur. Bunu **değil** güvensiz bölgedeki Azure Stack dağıtmanız önerilir.

![Azure Stack çevre ağı örneği](./media/azure-stack-firewall/perimeter-network-scenario.png)

## <a name="learn-more"></a>Daha fazla bilgi edinin
Daha fazla bilgi edinin [bağlantı noktaları ve Azure Stack uç noktaları tarafından kullanılan protokoller](azure-stack-integrate-endpoints.md).

## <a name="next-steps"></a>Sonraki adımlar
[Azure Stack PKI gereksinimleri](azure-stack-pki-certs.md)

