---
title: Azure Stack için Ağ tümleştirmesi planlaması | Microsoft Docs
description: Azure Stack tümleşik sistemlerle veri merkezi ağ tümleştirmesini nasıl planlayacağınızı öğrenin.
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
ms.date: 10/23/2019
ms.author: mabrigg
ms.reviewer: wamota
ms.lastreviewed: 06/04/2019
ms.openlocfilehash: 76bc9b83bf97c7817ff5c9cbf8bc0a3275a04d72
ms.sourcegitcommit: cefba8d6a93efaedff303d3c605b02bd28996c5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74298850"
---
# <a name="network-integration-planning-for-azure-stack"></a>Azure Stack için Ağ tümleştirmesi planlaması

Bu makalede, Azure Stack mevcut ağ ortamınıza en iyi şekilde tümleştirme konusunda karar vermenize yardımcı olacak Azure Stack ağ altyapısı bilgileri sağlanmaktadır. 

> [!NOTE]
> Dış DNS adlarını Azure Stack (örneğin, www\.bing.com) çözümlemek için DNS isteklerini iletmek üzere DNS sunucuları sağlamanız gerekir. Azure Stack DNS gereksinimleri hakkında daha fazla bilgi için bkz. [Azure Stack veri merkezi tümleştirmesi-DNS](azure-stack-integrate-dns.md).

## <a name="physical-network-design"></a>Fiziksel ağ tasarımı

Azure Stack çözümünün çalışmasını ve hizmetlerini desteklemek için dayanıklı ve yüksek kullanılabilirliğe sahip bir fiziksel altyapı gerekir. ToR 'dan kenarlık anahtarlarına olan bağlantılar, SFP + veya SFP28 medya, 1 GB, 10 GB veya 25 GB hızında sınırlandırılmıştır. Kullanılabilirlik için özgün ekipman üreticisi (OEM) donanım satıcınıza başvurun. Aşağıdaki diyagramda önerilen tasarım sunulmaktadır:

![Ağ tasarımı Azure Stack önerilir](media/azure-stack-network/recommended-design.png)


## <a name="logical-networks"></a>Mantıksal Ağlar

Mantıksal ağlar, temel alınan fiziksel ağ altyapısının bir soyutlamasını temsil eder. Konaklar, sanal makineler (VM 'Ler) ve hizmetler için ağ atamalarını düzenlemek ve basitleştirmek için kullanılırlar. Mantıksal ağ oluşturma kapsamında ağ siteleri, her fiziksel konumdaki mantıksal ağla ilişkili sanal yerel ağları (VLAN), IP alt ağlarını ve IP alt ağını/VLAN çiftlerini tanımlamak için oluşturulur.

Aşağıdaki tabloda, planlamanız gereken mantıksal ağlar ve ilişkili IPv4 alt ağ aralıkları gösterilmektedir:

| Mantıksal ağ | Açıklama | Boyut | 
| -------- | ------------- | ------------ | 
| Genel VIP | Azure Stack, bu ağdan toplam 31 adres kullanır. Küçük bir Azure Stack Hizmetleri kümesi için sekiz genel IP adresi kullanılır ve geri kalanı Kiracı VM 'Leri tarafından kullanılır. App Service ve SQL kaynak sağlayıcılarını kullanmayı planlıyorsanız, 7 adres daha kullanılır. Kalan 15 IP 'Ler gelecek Azure hizmetleri için ayrılmıştır. | /26 (62 ana bilgisayar)-/22 (1022 ana bilgisayar)<br><br>Önerilen =/24 (254 ana bilgisayar) | 
| Altyapıyı Değiştir | Yönlendirme amacıyla noktadan noktaya IP adresleri, adanmış anahtar yönetimi arabirimleri ve anahtara atanan geri döngü adresleri. | /26 | 
| Altyapı | İletişim kurmak üzere Azure Stack iç bileşenleri için kullanılır. | /24 |
| Özel | Depolama ağı, özel VIP 'ler, altyapı kapsayıcıları ve diğer iç işlevler için kullanılır. 1910 ' den başlayarak, bu alt ağ için boyut/20 olarak değiştiriliyor. Daha fazla bilgi için bu makaledeki [özel ağ](#private-network) bölümüne bakın. | /20 | 
| KONAĞıNDAKI | Fiziksel konaklardaki BMC 'ler ile iletişim kurmak için kullanılır. | /26 | 
| | | |

> [!NOTE]
> Sistem sürüm 1910 ' e güncelleştirildiği zaman, portalda bir uyarı yeni bir/20 özel IP alanı eklemek için işleçle New PEP cmdlet **set-AzsPrivateNetwork** komutunu çalıştırmasını hatırlatır. Cmdlet 'i çalıştırmaya ilişkin yönergeler için [1910 sürüm notlarına](release-notes.md) bakın. /20 özel IP alanını seçme hakkında daha fazla bilgi ve yönergeler için, bu makaledeki [özel ağ](#private-network) bölümüne bakın.

## <a name="network-infrastructure"></a>Ağ altyapısı

Azure Stack ağ altyapısı, anahtarlar üzerinde yapılandırılmış çeşitli mantıksal ağlardan oluşur. Aşağıdaki diyagramda bu mantıksal ağlar ve bunların raf üstü (TOR), temel kart yönetim denetleyicisi (BMC) ve sınır (müşteri ağı) anahtarlarıyla nasıl tümleştirileceği gösterilmektedir.

![Mantıksal ağ diyagramı ve anahtar bağlantıları](media/azure-stack-network/NetworkDiagram.png)

### <a name="bmc-network"></a>BMC ağı

Bu ağ, tüm temel kart yönetim denetleyicilerini (BMC veya hizmet işlemcileri olarak da bilinir) yönetim ağına bağlamak için ayrılmıştır. Örnekler şunlardır: Idrac, ILO, iBMC, vb. Herhangi bir BMC düğümüyle iletişim kurmak için yalnızca bir BMC hesabı kullanılır. Varsa, donanım yaşam döngüsü Konağı (HLH) bu ağda bulunur ve donanım bakımı veya izleme için OEM 'e özgü yazılım sağlayabilir.

HLH Ayrıca dağıtım sanal makinesini (DVD) barındırır. DVD Azure Stack dağıtım sırasında kullanılır ve dağıtım tamamlandığında kaldırılır. DVı 'nin birden çok bileşeni test etmek, doğrulamak ve erişmek için bağlı dağıtım senaryolarında internet erişimi olması gerekir. Bu bileşenler, şirket ağınızın içinde ve dışında (örneğin: NTP, DNS ve Azure) olabilir. Bağlantı gereksinimleri hakkında daha fazla bilgi için, [Azure Stack güvenlik duvarı tümleştirmesinde NAT bölümüne](azure-stack-firewall.md#network-address-translation)bakın.

### <a name="private-network"></a>Özel ağ

Bu/20 (4096 IP) ağı, Azure Stack bölgesi için özeldir (Azure Stack sisteminin sınır anahtar cihazlarının ötesine geçmez) ve birden çok alt ağa bölünmüştür, bazı örnekler aşağıda verilmiştir:

- **Depolama ağı**: boşluk doğrudan ve sunucu ileti bloğu (SMB) depolama TRAFIĞI ve VM dinamik geçişi kullanımını desteklemek için kullanılan a/25 (128 IP) ağ.
- **İç sanal IP ağı**: yazılım yük dengeleyici için yalnızca iç VIP 'lere adanmış bir/25 ağı.
- **Kapsayıcı ağı**: bir/23 (512 IP) ağı, Altyapı Hizmetleri çalıştıran kapsayıcılar arasındaki yalnızca iç trafiğe ayrılmıştır.

1910 ' den başlayarak, özel ağ boyutu özel IP alanının a/20 (4096 IP) olarak değişir. Bu ağ Azure Stack sistemine özel olacak (Azure Stack sisteminin sınır anahtar cihazlarının ötesinde yönlendirmez) ve veri merkezinizdeki birden çok Azure Stack sisteminde yeniden kullanılabilir. Ağ Azure Stack için özel olsa da, veri merkezindeki diğer ağlarla çakışmamalıdır. Özel IP alanı hakkında rehberlik için, [RFC 1918](https://tools.ietf.org/html/rfc1918)' i takip etmenizi öneririz.

Bu/20 özel IP alanı, gelecekteki sürümlerde kapsayıcılar üzerinde Azure Stack sistemi iç altyapısını çalıştırmayı etkinleştiren birden çok ağa bölünecektir. Daha fazla bilgi için [1910 sürüm notlarına](release-notes.md)bakın. Ayrıca, bu yeni özel IP alanı, dağıtımdan önce gerekli yönlendirilebilir IP alanını azaltmaya devam eden çabalara olanak sağlar.

1910 ' den önce dağıtılan sistemler için, bu/20 alt ağı, 1910 ' e güncelleştirildikten sonra sistemlere girilecek ek bir ağ olacaktır. **Set-AzsPrivateNetwork** Pep cmdlet 'i aracılığıyla ek ağın sisteme sağlanması gerekir. Bu cmdlet hakkında yönergeler için bkz. [1910 sürüm notları](release-notes.md).

### <a name="azure-stack-infrastructure-network"></a>Azure Stack altyapı ağı

Bu/24 ağ dahili Azure Stack bileşenlere ayrılmıştır ve bu sayede verileri kendileri arasında iletişim kurabilir ve bunları değiş tokuş edebilirler. Bu alt ağ, veri merkezinize Azure Stack çözümü için dışarıdan yönlendirilebilir. Bu alt ağda ortak veya internet yönlendirilebilir IP adresleri kullanmanızı önermiyoruz. Bu ağ, kenarlığa tanıtıldığı halde, IP 'lerinin çoğu Access Control listeleriyle (ACL 'Ler) korunur. Erişim için izin verilen IP 'Ler bir/27 ağ ve [ayrıcalıklı uç noktası (PEP)](azure-stack-privileged-endpoint.md) ve [Azure Stack yedekleme](azure-stack-backup-reference.md)gibi ana bilgisayar hizmetleri ile aynı boyutta olan küçük bir aralık dahilinde.

### <a name="public-vip-network"></a>Genel VIP ağı

Genel VIP ağı, Azure Stack ağ denetleyicisine atanır. Anahtar üzerinde bir mantıksal ağ değil. SLB, kiracı iş yükleri için adres havuzunu ve atar/32 ağlarını kullanır. Anahtar yönlendirme tablosunda, bu/32 IP 'Leri BGP üzerinden kullanılabilir bir yol olarak tanıtılabilir. Bu ağ, dışarıdan erişilebilen veya genel IP adreslerini içerir. Azure Stack altyapısı, geri kalan Kiracı VM 'Ler tarafından kullanılan ilk 31 adresi bu genel VIP ağından ayırır. Bu alt ağdaki ağ boyutu, en az/26 (64 ana bilgisayar) ile en fazla/22 (1022 ana bilgisayar) arasında değişebilir. Bir/24 ağı planlamanız önerilir.

### <a name="switch-infrastructure-network"></a>Altyapı ağını Değiştir

Bu/26 ağı yönlendirilebilir noktadan noktaya IP/30 (iki ana bilgisayar IP) alt ağını ve bant içi anahtar yönetimi ve BGP yönlendirici KIMLIĞI için adanmış/32 alt ağlarını içeren bir alt ağdır. Bu IP adresi aralığı, veri merkezinize Azure Stack çözümünün dışında yönlendirilebilir olmalıdır. Özel veya genel IP 'Ler olabilir.

### <a name="switch-management-network"></a>Geçiş yönetimi ağı

Bu/29 (altı ana bilgisayar IP) ağı, anahtarların yönetim bağlantı noktalarını bağlamak için ayrılmıştır. Dağıtım, yönetim ve sorun giderme için bant dışı erişime izin verir. Yukarıda bahsedilen anahtar altyapısı ağından hesaplanır.

## <a name="permitted-networks"></a>İzin verilen ağlar

1910 ' den başlayarak, dağıtım çalışma sayfasının, ağ cihazı yönetim arabirimlerine ve donanım yaşam döngüsü ana bilgisayarına (HLH) güvenilen bir veri merkezi ağ aralığından erişime izin vermek için bazı erişim denetim listelerini (ACL 'Ler) değiştirmesine izin veren yeni bir alan olacaktır. Erişim denetim listesi değişikliği ile operatör, belirli bir ağ aralığındaki yönetim sıçrama kutusu VM 'lerinin anahtar yönetimi arabirimine, HLH OS ve HLH BMC 'ye erişmesine izin verebilir. İşleci bu listeye bir veya birden çok alt ağ sağlayabilir, boş bırakılırsa varsayılan olarak erişimi reddedebilirsiniz. Bu yeni işlev, [Azure Stack anahtarı yapılandırmanızda belirli ayarları değiştirme](azure-stack-customer-defined.md#access-control-list-updates)bölümünde açıklandığı gibi dağıtım sonrası el ile müdahale gereksinimini ortadan kaldırır.

## <a name="next-steps"></a>Sonraki adımlar

Ağ planlama: [sınır bağlantısı](azure-stack-border-connectivity.md)hakkında bilgi edinin.
