---
title: Azure Stack tümleşik sistemler için güvenlik duvarı planlamasını Azure Stack | Microsoft Docs
description: Çok düğümlü Azure Stack Azure 'a bağlı dağıtımlar için Azure Stack güvenlik duvarı konularını açıklar.
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
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: wfayed
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: ec8d45add2b2d0312d82d2a7e2d71fbd2a2b41d6
ms.sourcegitcommit: a7207f4a4c40d4917b63e729fd6872b3dba72968
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71909203"
---
# <a name="azure-stack-firewall-integration"></a>Azure Stack Güvenlik Duvarı tümleştirmesi
Azure Stack güvenli hale getirmek için bir güvenlik duvarı cihazı kullanmanız önerilir. Güvenlik duvarları, dağıtılmış hizmet reddi (DDOS) saldırıları, yetkisiz giriş algılama ve içerik incelemesi gibi şeylere karşı savunmasına yardımcı olabilir. Ancak, blob 'lar, tablolar ve kuyruklar gibi Azure depolama hizmetleri için bir verimlilik sorunu da olabilirler.

 Bağlantısı kesilen bir dağıtım modu kullanılırsa AD FS uç noktasını yayımlamanız gerekir. Daha fazla bilgi için bkz. [veri merkezi tümleştirme kimliği makalesi](azure-stack-integrate-identity.md).

Azure Resource Manager (yönetici), Yönetici portalı ve Key Vault (yönetici) uç noktaları için dış yayımlama gerekmez. Örneğin, bir hizmet sağlayıcısı olarak, saldırı yüzeyini internet 'ten değil yalnızca ağınızın içinden Azure Stack yöneterek sınırlayabilirsiniz.

Kurumsal kuruluşlar için dış ağ, mevcut şirket ağı olabilir. Bu senaryoda, kurumsal ağdan Azure Stack çalıştırmak için uç noktaları yayımlamanız gerekir.

### <a name="network-address-translation"></a>Ağ Adresi Çevirisi
Ağ adresi çevirisi (NAT), dağıtım sanal makinesinin (DVM), dağıtım sırasında dış kaynaklara ve internet 'e ve acil durum kurtarma konsolu (ERCS) VM 'lerine veya ayrıcalıklı uç noktaya (PEP) erişmesine izin vermek için önerilen yöntemdir. kayıt ve sorun giderme.

NAT, dış ağdaki veya genel VIP 'lerde genel IP adresleri için de bir alternatif olabilir. Bununla birlikte, Kiracı Kullanıcı deneyimini sınırlayan ve karmaşıklığı arttığı için bunu yapmanız önerilmez. Tek bir seçenek, havuzda her Kullanıcı IP 'si için bir genel IP gerektiren tek bir NAT olabilir. Başka bir seçenek de bir kullanıcının kullanabileceği tüm bağlantı noktaları için Kullanıcı VIP başına NAT kuralı gerektiren bir NAT ' dır.

Genel VIP için NAT kullanmanın bazı altlarından bazıları şunlardır:
- Kullanıcılar kendi uç noktalarını ve yazılım tanımlı ağ (SDN) yığınında kendi yayımlama kurallarını denetlemediğinden, NAT güvenlik duvarı kurallarını yönetirken ek yük ekler. Kullanıcıların VIP 'lerini yayınlamalarını sağlamak ve bağlantı noktası listesini güncelleştirmek için Azure Stack işleçle iletişim kurabilmesi gerekir.
- NAT kullanımı Kullanıcı deneyimini sınırlarken, yayımlama istekleri üzerinden operatör üzerinde tam denetim sağlar.
- Azure ile hibrit bulut senaryoları için, Azure 'un NAT kullanarak bir uç noktaya VPN tüneli ayarlamayı desteklemediğini göz önünde bulundurun.

### <a name="ssl-decryption"></a>SSL şifre çözme
Tüm Azure Stack trafiğinden SSL şifre çözme devre dışı bırakılması önerilir. Gelecekteki güncelleştirmelerde destekleniyorsa, Azure Stack için SSL şifre çözmeyi etkinleştirme hakkında rehberlik sağlanacaktır.

## <a name="edge-firewall-scenario"></a>Edge güvenlik duvarı senaryosu
Bir Edge dağıtımında, Azure Stack doğrudan sınır yönlendiricisinin veya güvenlik duvarının arkasında dağıtılır. Bu senaryolarda, güvenlik duvarının (Senaryo 1) hem etkin-etkin ve etkin-Pasif güvenlik duvarı yapılandırmalarının desteklediği ya da yalnızca etkin-etkin güvenlik duvarını desteklediği sınır aygıtı (Senaryo 2) olarak davrandığı kenarlığın üzerinde olması desteklenir yapılandırma, yük devretme için BGP veya statik yönlendirme ile eşit maliyetli çoklu yola (ECMP) bağlı.

Ortak yönlendirilebilir IP adresleri, dış ağdan dağıtım zamanında genel VIP havuzu için belirtilir. Bir Edge senaryosunda, güvenlik amacıyla diğer bir ağ üzerinde genel yönlendirilebilir IP 'Lerin kullanılması önerilmez. Bu senaryo, bir kullanıcının Azure gibi genel bir bulutta olduğu gibi tam kendi kendine denetlenen bulut deneyimini deneyimini sağlar.  

![Azure Stack Edge güvenlik duvarı örneği](./media/azure-stack-firewall/firewallScenarios.png)

## <a name="enterprise-intranet-or-perimeter-network-firewall-scenario"></a>Kurumsal intranet veya çevre ağı güvenlik duvarı senaryosu
Kurumsal intranet veya çevre dağıtımında Azure Stack, çok bölgeli bir güvenlik duvarında veya uç güvenlik duvarı ile dahili, şirket ağı güvenlik duvarı arasında dağıtılır. Bundan sonra trafiği güvenli, çevre ağı (veya DMZ) ve güvenli olmayan bölgeler arasında aşağıda açıklandığı gibi dağıtılır:

- **Güvenli bölge**: Bu, iç veya şirket tarafından yönlendirilebilir IP adresleri kullanan iç bir ağ. Güvenli ağ ayrılabilir, güvenlik duvarında NAT üzerinden İnternet giden erişimine sahip olur ve genellikle iç ağ aracılığıyla veri merkezinizin içinden herhangi bir yerden erişilebilir. Tüm Azure Stack ağları, dış ağın genel VIP havuzu hariç güvenli bölgede bulunmalıdır.
- **Çevre bölgesi**. Çevre ağı, Web sunucuları gibi dış veya internet 'e yönelik uygulamaların genellikle dağıtıldığı yerdir. Genellikle, internet 'ten gelen trafiğe izin verirken DDoS ve yetkisiz erişim (korsan) gibi saldırıları önlemek için bir güvenlik duvarı tarafından izlenir. Yalnızca dış ağ genel VIP havuzu Azure Stack DMZ bölgesinde bulunmalıdır.
- **Güvenli olmayan bölge**. Bu, internet 'in dış ağı. Güvenli **olmayan bölgede Azure Stack dağıtılması önerilmez.**

![Azure Stack çevre ağı örneği](./media/azure-stack-firewall/perimeter-network-scenario.png)

## <a name="learn-more"></a>Daha fazla bilgi edinin
[Azure Stack uç noktaları tarafından kullanılan bağlantı noktaları ve protokoller](azure-stack-integrate-endpoints.md)hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Stack PKI gereksinimleri](azure-stack-pki-certs.md)

