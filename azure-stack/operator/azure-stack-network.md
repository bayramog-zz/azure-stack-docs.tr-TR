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
ms.date: 10/07/2019
ms.author: mabrigg
ms.reviewer: wamota
ms.lastreviewed: 06/04/2019
ms.openlocfilehash: 4894fb7184944095d968d08e2d668912a78119d4
ms.sourcegitcommit: ef7efcde76d1d7875ca1c882afebfd6a27f1c686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2019
ms.locfileid: "72888042"
---
# <a name="network-integration-planning-for-azure-stack"></a>Azure Stack için Ağ tümleştirmesi planlaması

Bu makalede, Azure Stack mevcut ağ ortamınıza en iyi şekilde tümleştirme konusunda karar vermenize yardımcı olacak Azure Stack ağ altyapısı bilgileri sağlanmaktadır. 

> [!NOTE]
> Dış DNS adlarını Azure Stack (örneğin, www\.bing.com) çözümlemek için DNS isteklerini iletmek üzere DNS sunucuları sağlamanız gerekir. Azure Stack DNS gereksinimleri hakkında daha fazla bilgi için bkz. [Azure Stack veri merkezi tümleştirmesi-DNS](azure-stack-integrate-dns.md).

## <a name="physical-network-design"></a>Fiziksel ağ tasarımı

Azure Stack çözümünün çalışmasını ve hizmetlerini desteklemek için dayanıklı ve yüksek kullanılabilirliğe sahip bir fiziksel altyapı gerekir. ToR 'dan kenarlık anahtarlarına olan bağlantılar, SFP + veya SFP28 medya, 1 GB, 10 GB veya 25 GB hızında sınırlandırılmıştır. Kullanılabilirlik için özgün ekipman üreticisi (OEM) donanım satıcınıza başvurun. Aşağıdaki diyagramda önerilen tasarım sunulmaktadır:

![Ağ tasarımı Azure Stack önerilir](media/azure-stack-network/recommended-design.png)


## <a name="logical-networks"></a>Mantıksal ağlar

Mantıksal ağlar, temel alınan fiziksel ağ altyapısının bir soyutlamasını temsil eder. Konaklar, sanal makineler (VM 'Ler) ve hizmetler için ağ atamalarını düzenlemek ve basitleştirmek için kullanılırlar. Mantıksal ağ oluşturma kapsamında ağ siteleri, her fiziksel konumdaki mantıksal ağla ilişkili sanal yerel ağları (VLAN), IP alt ağlarını ve IP alt ağını/VLAN çiftlerini tanımlamak için oluşturulur.

Aşağıdaki tabloda, planlamanız gereken mantıksal ağlar ve ilişkili IPv4 alt ağ aralıkları gösterilmektedir:

| Mantıksal ağ | Açıklama | Boyut | 
| -------- | ------------- | ------------ | 
| Genel VIP | Azure Stack, bu ağdan toplam 31 adres kullanır. Küçük bir Azure Stack Hizmetleri kümesi için sekiz genel IP adresi kullanılır ve geri kalanı Kiracı VM 'Leri tarafından kullanılır. App Service ve SQL kaynak sağlayıcılarını kullanmayı planlıyorsanız, 7 adres daha kullanılır. Kalan 15 IP 'Ler gelecek Azure hizmetleri için ayrılmıştır. | /26 (62 ana bilgisayar)-/22 (1022 ana bilgisayar)<br><br>Önerilen =/24 (254 ana bilgisayar) | 
| Altyapıyı Değiştir | Yönlendirme amacıyla noktadan noktaya IP adresleri, adanmış anahtar yönetimi arabirimleri ve anahtara atanan geri döngü adresleri. | /26 | 
| Altyapı | İletişim kurmak üzere Azure Stack iç bileşenleri için kullanılır. | /24 |
| Özel | Depolama ağı ve özel VIP 'ler için kullanılır. | /24 | 
| KONAĞıNDAKI | Fiziksel konaklardaki BMC 'ler ile iletişim kurmak için kullanılır. | /26 | 
| | | |

## <a name="network-infrastructure"></a>Ağ altyapısı

Azure Stack ağ altyapısı, anahtarlar üzerinde yapılandırılmış çeşitli mantıksal ağlardan oluşur. Aşağıdaki diyagramda bu mantıksal ağlar ve bunların raf üstü (TOR), temel kart yönetim denetleyicisi (BMC) ve sınır (müşteri ağı) anahtarlarıyla nasıl tümleştirileceği gösterilmektedir.

![Mantıksal ağ diyagramı ve anahtar bağlantıları](media/azure-stack-network/NetworkDiagram.png)

### <a name="bmc-network"></a>BMC ağı

Bu ağ, tüm temel kart yönetim denetleyicilerini (BMC veya hizmet işlemcileri olarak da bilinir) yönetim ağına bağlamak için ayrılmıştır. Örnekler şunlardır: Idrac, ILO, iBMC, vb. Herhangi bir BMC düğümüyle iletişim kurmak için yalnızca bir BMC hesabı kullanılır. Varsa, donanım yaşam döngüsü Konağı (HLH) bu ağda bulunur ve donanım bakımı veya izleme için OEM 'e özgü yazılım sağlayabilir.

HLH Ayrıca dağıtım sanal makinesini (DVD) barındırır. DVD Azure Stack dağıtım sırasında kullanılır ve dağıtım tamamlandığında kaldırılır. DVı 'nin birden çok bileşeni test etmek, doğrulamak ve erişmek için bağlı dağıtım senaryolarında internet erişimi olması gerekir. Bu bileşenler, şirket ağınızın içinde ve dışında (örneğin: NTP, DNS ve Azure) olabilir. Bağlantı gereksinimleri hakkında daha fazla bilgi için, [Azure Stack güvenlik duvarı tümleştirmesinde NAT bölümüne](azure-stack-firewall.md#network-address-translation)bakın.

### <a name="private-network"></a>Özel ağ

Bu/24 (254 ana bilgisayar IP) ağı, Azure Stack bölgesi için özeldir (Azure Stack bölgesinin sınır anahtar cihazlarının ötesinde genişlemez) ve iki alt ağa ayrılmıştır:

- **Depolama ağı**: boşluk doğrudan ve sunucu ileti bloğu (SMB) depolama TRAFIĞI ve VM dinamik geçişi kullanımını desteklemek için kullanılan A/25 (126 ana bilgisayar IP) ağı.
- **İç sanal IP ağı**: yazılım yük dengeleyici için yalnızca iç VIP 'lere adanmış bir/25 ağı.

### <a name="azure-stack-infrastructure-network"></a>Azure Stack altyapı ağı
Bu/24 ağ dahili Azure Stack bileşenlere ayrılmıştır ve bu sayede verileri kendileri arasında iletişim kurabilir ve bunları değiş tokuş edebilirler. Bu alt ağ, veri merkezinize Azure Stack çözümünün dışarıdan yönlendirilebilir, bu alt ağda ortak veya Internet yönlendirilebilir IP adresleri kullanmanızı önermiyoruz. Bu ağ, kenarlığa tanıtıldığı halde, IP 'lerinin çoğu Access Control listeleriyle (ACL 'Ler) korunmaktadır. Erişim için izin verilen IP 'Ler bir/27 ağ ve [ayrıcalıklı uç noktası (PEP)](azure-stack-privileged-endpoint.md) gibi ana bilgisayar hizmetleri ve [Azure Stack yedekleme](azure-stack-backup-reference.md)ile aynı boyutta olan küçük bir aralığın içindedir.

### <a name="public-vip-network"></a>Genel VIP ağı

Genel VIP ağı, Azure Stack ağ denetleyicisine atanır. Anahtar üzerinde bir mantıksal ağ değil. SLB, kiracı iş yükleri için adres havuzunu ve atar/32 ağlarını kullanır. Anahtar yönlendirme tablosunda, bu/32 IP 'Leri BGP üzerinden kullanılabilir bir yol olarak tanıtılabilir. Bu ağ, dışarıdan erişilebilen veya genel IP adreslerini içerir. Azure Stack altyapısı, geri kalan Kiracı VM 'Ler tarafından kullanılan ilk 31 adresi bu genel VIP ağından ayırır. Bu alt ağdaki ağ boyutu, en az/26 (64 ana bilgisayar) ile en fazla/22 (1022 ana bilgisayar) arasında değişebilir. Bir/24 ağı planlamanız önerilir.

### <a name="switch-infrastructure-network"></a>Altyapı ağını Değiştir

Bu/26 ağı yönlendirilebilir noktadan noktaya IP/30 (iki ana bilgisayar IP) alt ağını ve bant içi anahtar yönetimi ve BGP yönlendirici KIMLIĞI için adanmış/32 alt ağlarını içeren bir alt ağdır. Bu IP adresi aralığı, veri merkezinize Azure Stack çözümünün dışında yönlendirilebilir olmalıdır. Özel veya genel IP 'Ler olabilir.

### <a name="switch-management-network"></a>Geçiş yönetimi ağı

Bu/29 (altı ana bilgisayar IP) ağı, anahtarların yönetim bağlantı noktalarını bağlamak için ayrılmıştır. Dağıtım, yönetim ve sorun giderme için bant dışı erişime izin verir. Yukarıda bahsedilen anahtar altyapısı ağından hesaplanır.

## <a name="next-steps"></a>Sonraki adımlar

Ağ planlama: [sınır bağlantısı](azure-stack-border-connectivity.md)hakkında bilgi edinin.
