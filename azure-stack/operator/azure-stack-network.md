---
title: Azure Stack tümleşik sistemler için Ağ tümleştirmesi konuları | Microsoft Docs
description: Çok düğümlü Azure Stack veri merkezi ağ tümleştirmesini planlamak için neler yapabileceğinizi öğrenin.
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
ms.date: 06/04/2019
ms.author: mabrigg
ms.reviewer: wamota
ms.lastreviewed: 06/04/2019
ms.openlocfilehash: b2b53edaba6a6cb180ae617740fd4695b1a86187
ms.sourcegitcommit: 637018771ac016b7d428174e88d4dcb131b54959
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68842717"
---
# <a name="network-connectivity"></a>Ağ bağlantısı
Bu makalede, Azure Stack mevcut ağ ortamınıza en iyi şekilde tümleştirme konusunda karar vermenize yardımcı olacak Azure Stack ağ altyapısı bilgileri sağlanmaktadır. 

> [!NOTE]
> Dış DNS adlarını Azure Stack (örneğin, www\.Bing.com) çözümlemek için DNS isteklerini iletmek üzere DNS sunucuları sağlamanız gerekir. Azure Stack DNS gereksinimleri hakkında daha fazla bilgi için bkz. [Azure Stack veri merkezi tümleştirmesi-DNS](azure-stack-integrate-dns.md).

## <a name="physical-network-design"></a>Fiziksel ağ tasarımı
Azure Stack çözümünün çalışmasını ve hizmetlerini desteklemek için dayanıklı ve yüksek kullanılabilirliğe sahip bir fiziksel altyapı gerekir. ToR 'dan kenarlık anahtarlarına olan bağlantılar, SFP + veya SFP28 medya, 1 GB, 10 GB veya 25 GB hızında sınırlandırılmıştır. Kullanılabilirlik için özgün ekipman üreticisi (OEM) donanım satıcınıza başvurun. Aşağıdaki diyagramda önerilen tasarım sunulmaktadır:

![Ağ tasarımı Azure Stack önerilir](media/azure-stack-network/recommended-design.png)


## <a name="logical-networks"></a>Mantıksal Ağlar
Mantıksal ağlar, temel alınan fiziksel ağ altyapısının bir soyutlamasını temsil eder. Konaklar, sanal makineler ve hizmetler için ağ atamalarını düzenlemek ve basitleştirmek için kullanılırlar. Mantıksal ağ oluşturma kapsamında ağ siteleri, her fiziksel konumdaki mantıksal ağla ilişkili sanal yerel ağları (VLAN), IP alt ağlarını ve IP alt ağını/VLAN çiftlerini tanımlamak için oluşturulur.

Aşağıdaki tabloda, planlamanız gereken mantıksal ağlar ve ilişkili IPv4 alt ağ aralıkları gösterilmektedir:

| Mantıksal ağ | Açıklama | Size | 
| -------- | ------------- | ------------ | 
| Genel VIP | Azure Stack, bu ağdan toplam 31 adres kullanır. Küçük bir Azure Stack Hizmetleri kümesi için sekiz genel IP adresi kullanılır ve REST, kiracı sanal makineleri tarafından kullanılır. App Service ve SQL kaynak sağlayıcılarını kullanmayı planlıyorsanız, 7 adres daha kullanılır. Kalan 15 IP 'Ler gelecek Azure hizmetleri için ayrılmıştır. | /26 (62 ana bilgisayar)-/22 (1022 ana bilgisayar)<br><br>Önerilen =/24 (254 ana bilgisayar) | 
| Altyapıyı Değiştir | Yönlendirme amacıyla noktadan noktaya IP adresleri, adanmış anahtar yönetimi arabirimleri ve anahtara atanan geri döngü adresleri. | /26 | 
| Altyapı | İletişim kurmak üzere Azure Stack iç bileşenleri için kullanılır. | /24 |
| Özel | Depolama ağı ve özel VIP 'ler için kullanılır. | /24 | 
| KONAĞINDAKI | Fiziksel konaklardaki BMC 'ler ile iletişim kurmak için kullanılır. | /26 | 
| | | |

## <a name="network-infrastructure"></a>Ağ altyapısı
Azure Stack ağ altyapısı, anahtarlar üzerinde yapılandırılmış çeşitli mantıksal ağlardan oluşur. Aşağıdaki diyagramda bu mantıksal ağlar ve bunların raf üstü (TOR), temel kart yönetim denetleyicisi (BMC) ve sınır (müşteri ağı) anahtarlarıyla nasıl tümleştirileceği gösterilmektedir.

![Mantıksal ağ diyagramı ve anahtar bağlantıları](media/azure-stack-network/NetworkDiagram.png)

### <a name="bmc-network"></a>BMC ağı
Bu ağ, tüm temel kart yönetim denetleyicilerini (hizmet işlemcileri olarak da bilinir) (örneğin, Idrac, ILO, iBMC vb.) yönetim ağına bağlamak için ayrılmıştır. Varsa, donanım yaşam döngüsü Konağı (HLH) bu ağda bulunur ve donanım bakımı veya izleme için OEM 'e özgü yazılım sağlayabilir. 

HLH Ayrıca dağıtım sanal makinesini (DVD) barındırır. DVD Azure Stack dağıtım sırasında kullanılır ve dağıtım tamamlandığında kaldırılır. DVı 'nin birden çok bileşeni test etmek, doğrulamak ve erişmek için bağlı dağıtım senaryolarında internet erişimi olması gerekir. Bu bileşenler, şirket ağınızın içinde ve dışında olabilir; Örneğin, NTP, DNS ve Azure. Bağlantı gereksinimleri hakkında daha fazla bilgi için, [Azure Stack güvenlik duvarı tümleştirmesinde NAT bölümüne](azure-stack-firewall.md#network-address-translation)bakın. 

### <a name="private-network"></a>Özel ağ
Bu/24 (254 ana bilgisayar IP) ağı, Azure Stack bölgesi için özeldir (Azure Stack bölgesinin sınır anahtar cihazlarının ötesinde genişlemez) ve iki alt ağa ayrılmıştır:

- **Depolama ağı**. Boşluk doğrudan ve sunucu Ileti bloğu (SMB) depolama trafiği ve sanal makine dinamik geçişi kullanımını desteklemek için kullanılan bir/25 (126 ana bilgisayar IP) ağı. 
- **İç sanal IP ağı**. Yazılım yük dengeleyici için yalnızca dahili VIP 'lere ayrılmış bir/25 ağı.

### <a name="azure-stack-infrastructure-network"></a>Azure Stack altyapı ağı
Bu/24 ağ dahili Azure Stack bileşenlere ayrılmıştır ve bu sayede verileri kendileri arasında iletişim kurabilir ve bunları değiş tokuş edebilirler. Bu alt ağ yönlendirilebilir IP adresleri gerektirir, ancak Access Control listeleri (ACL 'Ler) kullanılarak çözüme özel olarak tutulur. Bu hizmetlerin, dış kaynaklara ve/veya internet 'e erişmesi gerektiğinde bu hizmetlerden bazıları tarafından kullanılan bir/27 ağı ile aynı boyutta küçük bir Aralık eşdeğeri dışında, kenarlık anahtarlarının ötesine yönlendirilmek beklenmez. 

### <a name="public-vip-network"></a>Genel VIP ağı
Genel VIP ağı, Azure Stack ağ denetleyicisine atanır. Anahtar üzerinde bir mantıksal ağ değil. SLB, kiracı iş yükleri için adres havuzunu ve atar/32 ağlarını kullanır. Anahtar yönlendirme tablosunda, bu/32 IP 'Leri BGP üzerinden kullanılabilir bir yol olarak tanıtılabilir. Bu ağ, dışarıdan erişilebilen veya genel IP adreslerini içerir. Azure Stack altyapısı, geri kalan Kiracı VM 'Ler tarafından kullanılan ilk 31 adresi bu genel VIP ağından ayırır. Bu alt ağdaki ağ boyutu en az/26 (64 ana bilgisayar) ile en fazla/22 (1022 ana bilgisayar) arasında değişebilir, bir/24 ağı planlamanız önerilir.

### <a name="switch-infrastructure-network"></a>Altyapı ağını Değiştir
Bu/26 ağ, yönlendirilebilir noktadan noktaya IP/30 (2 ana bilgisayar IP) alt ağını ve bant içi anahtar yönetimi ve BGP yönlendirici KIMLIĞI için adanmış/32 alt ağlarını içeren bir alt ağ olur. Bu IP adresi aralığı, veri merkezinize Azure Stack çözümünün dışarıdan yönlendirilebilir olmalıdır, özel veya genel IP 'Ler olabilir.

### <a name="switch-management-network"></a>Geçiş yönetimi ağı
Bu/29 (6 konak IP) ağı, anahtarların yönetim bağlantı noktalarını bağlamaya ayrılmıştır. Dağıtım, yönetim ve sorun giderme için bant dışı erişime izin verir. Yukarıda bahsedilen anahtar altyapısı ağından hesaplanır.




## <a name="next-steps"></a>Sonraki adımlar
[Sınır bağlantısı](azure-stack-border-connectivity.md)
