---
title: Azure Stack tümleşik sistemler için veri merkezi tümleştirme planlama konuları | Microsoft Docs
description: Azure Stack tümleşik sistemlerle veri merkezi tümleştirmesini nasıl planlayacağınızı ve hazırlanacağınızı öğrenin.
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
ms.lastreviewed: 09/12/2018
ms.openlocfilehash: f3bd1fc6d1f53ee56a8b1ff1741861a7f505b5b6
ms.sourcegitcommit: a7207f4a4c40d4917b63e729fd6872b3dba72968
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71909379"
---
# <a name="datacenter-integration-planning-considerations-for-azure-stack-integrated-systems"></a>Azure Stack tümleşik sistemler için veri merkezi tümleştirme planlama konuları

Azure Stack tümleşik bir sistemle ilgileniyorsanız dağıtım ve sistemin veri merkezinize nasıl uyduğunu gösteren önemli planlama konularını anlamalısınız. Bu makalede, Azure Stack tümleşik sistemleriniz için önemli altyapı kararları almanıza yardımcı olmak üzere bu noktalara yönelik yüksek düzeyde bir genel bakış sunulmaktadır. Bu hususları anlamak, veri merkezinize Azure Stack dağıtırken OEM donanım satıcınızla çalışırken yardımcı olur.  

> [!NOTE]  
> Azure Stack tümleşik sistemler yalnızca yetkili donanım satıcılarından satın alınabilir.

Azure Stack dağıtmak için dağıtım başlamadan önce, işlemin hızlı ve sorunsuz bir şekilde çalışmasına yardımcı olmaya başlamadan önce çözüm sağlayıcınıza planlama bilgileri sağlamanız gerekir. Gerekli bilgiler birçok farklı alandan ve karar mekanizmalarından bilgi gerektirebilecek çok önemli kararlar vererek ağ, güvenlik ve kimlik bilgileri arasında aralıklar gerektirir. Dağıtımdan önce tüm gerekli bilgileri kullandığınızdan emin olmak için kuruluşunuzdaki birden fazla ekipten gelen kişilere ihtiyacınız olacak. Bu bilgiler, faydalı bir öneri olabileceğinden, bu bilgileri toplarken donanım satıcınızla iletişim sağlanmasına yardımcı olabilir.

Gerekli bilgileri araştırırken ve toplarken, ağ ortamınızda bazı dağıtım öncesi yapılandırma değişiklikleri yapmanız gerekebilir. Bu değişiklikler, Azure Stack çözümü için IP adresi alanlarını ve yeni Azure Stack çözüm anahtarlarına bağlantı hazırlamak için yönlendiricilerinizi, anahtarlarınızı ve güvenlik duvarlarını yapılandırmayı içerebilir. Planlamada size yardımcı olmak için konu alanı uzmanlığını yazdığınızdan emin olun.

## <a name="capacity-planning-considerations"></a>Kapasite planlama konuları
Alım için bir Azure Stack çözümü değerlendirirken, Azure Stack çözümünün genel kapasitesine doğrudan etkisi olan donanım yapılandırma seçimlerini yaparsınız. Bunlar arasında, klasik CPU, bellek yoğunluğu, depolama yapılandırması ve genel çözüm ölçeği (örneğin, sunucu sayısı) bulunur. Geleneksel bir sanallaştırma çözümünün aksine, kullanılabilir kapasiteyi belirlemede bu bileşenlerin basit aritmetik işlemleri uygulanmaz. İlk neden Azure Stack çözüm içindeki altyapıyı veya yönetim bileşenlerini barındırmak için tasarlanmıştır. İkinci neden çözüm yazılımının, kiracı iş yüklerinin kesintisini en aza indirecek şekilde güncelleştirilerek dayanıklılık desteğiyle ayrılmasının ikinci nedenidir.

[Azure Stack kapasite planlayıcısı elektronik tablosu](https://aka.ms/azstackcapacityplanner) , kapasiteyi iki şekilde planlamak için bilinçli kararlar almanıza yardımcı olur. Birincisi, bir donanım teklifi seçip kaynakların bir birleşimini sığdırmaya çalışırken. İkincisi, Azure Stack tarafından desteklenen kullanılabilir donanım SKU 'Larını görüntülemek için çalışması amaçlanan iş yükünü tanımlayarak yapılır. Son olarak, elektronik tablo, Azure Stack planlama ve yapılandırmayla ilgili kararlar almaya yardımcı olmak için bir kılavuz olarak hazırlanmıştır.

Elektronik tablo, kendi araştırmasını ve analizinizi bir alternatif olarak sunmaya yönelik değildir. Microsoft, bir veya daha fazla garanti vermez, bu, elektronik tabloda belirtilen bilgilerle ilgili olarak, açık veya zımni bir durum oluşturur.

## <a name="management-considerations"></a>Yönetim konuları
Azure Stack, altyapının her ikisi de bir izin ve ağ perspektifinden kilitlendiğinden kapalı bir sistemdir. Ağ erişim denetim listeleri (ACL 'Ler) tüm yetkisiz gelen trafiği ve altyapı bileşenleri arasındaki tüm gereksiz iletişimleri engelleyecek şekilde uygulanır. Bu sistem yetkisiz kullanıcıların sisteme erişmesini zorlaştırır.

Günlük yönetim ve işlemler için altyapıya Kısıtlanmamış yönetici erişimi yoktur. Azure Stack işleçler, sistemi Yönetici portalı üzerinden veya Azure Resource Manager (PowerShell veya REST API aracılığıyla) yönetmelidir. Hyper-V Yöneticisi veya Yük Devretme Kümesi Yöneticisi gibi diğer yönetim araçlarıyla sisteme erişim yoktur. Sistemin korunmasına yardımcı olmak için, üçüncü taraf yazılımlar (örneğin, aracılar) Azure Stack altyapısının bileşenleri içine yüklenemez. Dış yönetim ve güvenlik yazılımlarıyla birlikte çalışabilirlik, PowerShell veya REST API ile gerçekleştirilir.

Uyarı ortalaması adımları aracılığıyla çözümlenmeyen sorunları gidermeye yönelik daha yüksek bir erişim düzeyine ihtiyacınız olduğunda Microsoft Desteği başvurun. Destek sayesinde, daha gelişmiş işlemler için sisteme geçici tam yönetici erişimi sağlamak için bir yöntem vardır.

## <a name="identity-considerations"></a>Kimlik konuları

### <a name="choose-identity-provider"></a>Kimlik sağlayıcısını seçin
Azure Stack dağıtımı için hangi kimlik sağlayıcısını kullanmak istediğinizi göz önünde bulundurmanız gerekir, ya da Azure AD veya AD FS. Tam sistem yeniden dağıtımı olmadan dağıtımdan sonra kimlik sağlayıcılarını değiştiremezsiniz. Azure AD hesabınız yoksa ve bulut çözümü sağlayıcınız tarafından size sağlanmış bir hesabı kullanıyorsanız ve sağlayıcıyı değiştirmeye ve farklı bir Azure AD hesabı kullanmaya karar verirseniz, çözümü c 'niz için yeniden dağıtmak üzere çözüm sağlayıcınızla iletişim kurmanız gerekir OST.

Kimlik sağlayıcınızın tercih etmeksizin kiracı sanal makineleri (VM), kimlik sistemi, kullandıkları hesaplar veya Active Directory bir etki alanına katılıp katılamayacağı gibi bir alan yoktur. Bu şeyler ayrıdır.

[Azure Stack tümleşik sistemler bağlantı modelleri makalesinde](./azure-stack-connection-models.md)bir kimlik sağlayıcısı seçme hakkında daha fazla bilgi edinebilirsiniz.

### <a name="ad-fs-and-graph-integration"></a>AD FS ve Graf tümleştirmesi
Kimlik sağlayıcısı olarak AD FS kullanarak Azure Stack dağıtmayı seçerseniz, Azure Stack AD FS örneğini bir federasyon güveni aracılığıyla mevcut bir AD FS örneğiyle tümleştirmeniz gerekir. Bu tümleştirme, mevcut bir Active Directory ormanındaki kimliklerin Azure Stack kaynaklarla kimlik doğrulamasına olanak tanır.

Ayrıca Azure Stack grafik hizmetini mevcut Active Directory tümleştirebilirsiniz. Bu tümleştirme, Azure Stack rol tabanlı Access Control (RBAC) yönetmenizi sağlar. Bir kaynağa erişim atandığında, Graph bileşeni LDAP protokolünü kullanarak mevcut Active Directory ormanında Kullanıcı hesabını arar.

Aşağıdaki diyagramda tümleşik AD FS ve Graf trafik akışı gösterilmektedir.
![AD FS ve Graf trafik akışını gösteren diyagram](media/azure-stack-datacenter-integration/ADFSIntegration.PNG)

## <a name="licensing-model"></a>Lisanslama modeli
Hangi lisans modelini kullanmak istediğinize karar vermelisiniz. Internet 'e bağlı Azure Stack dağıtırsanız, kullanılabilir seçenekler bağımlıdır:
- Bağlı bir [dağıtım](azure-stack-connected-deployment.md)için, Kullandıkça Öde veya kapasite tabanlı lisanslama seçeneklerinden birini belirleyebilirsiniz. Kullandıkça öde, kullanımı raporlamak için Azure ile Azure ticareti üzerinden faturalandırılan bir bağlantı gerektirir. 
- İnternet [bağlantısı kesilen dağıtım](azure-stack-disconnected-deployment.md) yaptıysanız yalnızca kapasite tabanlı lisanslama desteklenir. 

Lisanslama modelleri hakkında daha fazla bilgi için bkz. [paketleme ve fiyatlandırma Microsoft Azure Stack](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf).


## <a name="naming-decisions"></a>Adlandırma kararları

Azure Stack ad alanınızı, özellikle bölge adını ve dış etki alanı adını nasıl planlamak istediğinizi düşünmeniz gerekir. Herkese açık uç noktalar için Azure Stack dağıtımınızın dış tam etki alanı adı (FQDN), bu iki adın &lt;birleşimidir: *bölge*&gt;.&lt; *FQDN* &gt;. Örneğin, *East.Cloud.fabrikam.com*. Bu örnekte, Azure Stack portalları aşağıdaki URL 'Lerde kullanılabilir olacaktır:

- https://portal.east.cloud.fabrikam.com
- https://adminportal.east.cloud.fabrikam.com

> [!IMPORTANT]
> Azure Stack dağıtımınız için seçtiğiniz bölge adı benzersiz olmalı ve Portal adreslerinde görünecektir. 

Aşağıdaki tabloda bu etki alanı adlandırma kararları özetlenmektedir.

| Name | Açıklama | 
| -------- | ------------- | 
|Bölge adı | İlk Azure Stack bölgenizin adı. Bu ad, Azure Stack yönettiği genel sanal IP adresleri (VIP) için FQDN 'nin bir parçası olarak kullanılır. Genellikle bölge adı, veri merkezi konumu gibi fiziksel bir konum tanımlayıcısı olacaktır.<br><br>Bölge adı yalnızca 0-9 arasındaki harflerden ve rakamlardan oluşmalıdır. Özel karakterlere (`-`, `#` vb. gibi) izin verilir.| 
| Dış etki alanı adı | Dış VIP 'ler içeren uç noktalar için etki alanı adı sistemi (DNS) bölgesinin adı. Bu genel VIP 'ler için FQDN 'de kullanılır. | 
| Özel (iç) etki alanı adı | Altyapı yönetimi için Azure Stack oluşturulan etki alanının (ve iç DNS bölgesinin) adı.
| | |

## <a name="certificate-requirements"></a>Sertifika gereksinimleri

Dağıtım için, herkese açık uç noktalar için Güvenli Yuva Katmanı (SSL) sertifikaları sağlamanız gerekir. Yüksek düzeyde, sertifikalar aşağıdaki gereksinimlere sahiptir:

- Tek bir joker sertifika kullanabilir veya bir özel sertifika kümesi kullanabilir ve yalnızca depolama ve Key Vault gibi uç noktalar için joker karakterler kullanabilirsiniz.
- Sertifikalar, ortak bir güvenilen sertifika yetkilisi (CA) veya müşteri tarafından yönetilen bir CA tarafından verilebilir.

Azure Stack dağıtmak için hangi PKI sertifikalarının gerekli olduğu ve bunların nasıl alınacağı hakkında daha fazla bilgi için, bkz. [Azure Stack ortak anahtar altyapısı sertifika gereksinimleri](azure-stack-pki-certs.md).  


> [!IMPORTANT]
> Belirtilen PKI sertifikası bilgileri genel kılavuz olarak kullanılmalıdır. Azure Stack için herhangi bir PKI sertifikası almadan önce, OEM donanım ortağınızla birlikte çalışın. Daha ayrıntılı sertifika kılavuzları ve gereksinimleri sağlayacaktır.


## <a name="time-synchronization"></a>Zaman eşitleme
Azure Stack eşitlenmesi için kullanılan belirli bir zaman sunucusunu seçmeniz gerekir. Zaman eşitleme, Kerberos biletleri oluşturmak için kullanıldığından Azure Stack ve altyapı rolleri için önemlidir. Kerberos biletleri, birbirleriyle iç hizmetlerin kimliğini doğrulamak için kullanılır.

Zaman eşitleme sunucusu için bir IP belirtmeniz gerekir. Altyapıdaki bileşenlerin çoğu bir URL 'YI çözümleyebilse de, bazı yalnızca IP adreslerini destekler. Bağlantısı kesilmiş dağıtım seçeneğini kullanıyorsanız, kurumsal ağınızda Azure Stack Altyapı ağından erişebildiğinizden emin olduğunuz bir zaman sunucusu belirtmeniz gerekir.

## <a name="connect-azure-stack-to-azure"></a>Azure Stack Azure 'a bağlama

Hibrit bulut senaryoları için Azure Stack Azure 'a nasıl bağlamak istediğinizi planlamanız gerekir. Azure 'daki sanal ağlara Azure Stack sanal ağları bağlamak için desteklenen iki yöntem vardır:

- **Siteden siteye**: IPSec üzerinden bir sanal özel ağ (VPN) bağlantısı (ıKE v1 ve ıKE v2). Bu tür bir bağlantı, bir VPN cihazı veya Yönlendirme ve uzaktan erişim hizmeti (RRAS) gerektirir. Azure 'da VPN ağ geçitleri hakkında daha fazla bilgi için bkz. [VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Bu tünelden iletişim, şifrelenir ve güvenlidir. Ancak bant genişliği, tünelin en yüksek verimlilik (100-200 Mbps) ile sınırlıdır.

- **Gıden NAT**: Varsayılan olarak, Azure Stack içindeki tüm sanal makinelerin giden NAT aracılığıyla dış ağlarla bağlantısı olacaktır. Azure Stack oluşturulan her sanal ağ, kendisine atanmış bir genel IP adresi alır. VM 'nin doğrudan genel bir IP adresi atanıp atanmadığını veya genel IP adresine sahip bir yük dengeleyicinin arkasında olup olmadığı, sanal ağın VIP 'sini kullanarak giden NAT aracılığıyla giden erişime sahip olur. Bu yöntem yalnızca VM tarafından başlatılan ve dış ağlara (Internet veya intranet) yönelik olan iletişim için geçerlidir. VM ile dışından iletişim kurmak için kullanılamaz.

### <a name="hybrid-connectivity-options"></a>Karma bağlantı seçenekleri

Karma bağlantı için, ne tür bir dağıtım sunmak istediğinizi ve nereye dağıtılacağını göz önünde bulundurmanız önemlidir. Ağ trafiğini kiracı başına yalıtmanız gerekip gerekmediğini ve bir intranet veya internet dağıtımına sahip olup olmayacağını dikkate almanız gerekir.

- **Tek kiracılı Azure Stack**: En azından bir ağ perspektifinden, tek bir kiracı gibi görünen bir Azure Stack dağıtımı. Birçok kiracı aboneliği olabilir, ancak herhangi bir intranet hizmeti gibi tüm trafik aynı ağlar üzerinde dolaşır. Bir abonelikteki ağ trafiği, başka bir abonelikle aynı ağ bağlantısı üzerinden geçer ve şifrelenmiş bir tünelle yalıtılmış olması gerekmez.

- **Çok kiracılı Azure Stack**: Her kiracı aboneliğinin Azure Stack harici olan ağlara bağlandığı Azure Stack dağıtımı, diğer kiracıların ağ trafiğinden yalıtılmalıdır.
 
- **Intranet dağıtımı**: Genellikle özel IP adresi alanında ve bir veya daha fazla güvenlik duvarının arkasındaki bir kurumsal intranette bulunan Azure Stack dağıtımı. Genel IP adresleri, doğrudan genel İnternet üzerinden yönlendirilemediğinden gerçekten genel değildir.

- **Internet dağıtımı**: Genel internet 'e bağlanan ve genel VIP aralığı için internet yönlendirilebilir genel IP adreslerini kullanan Azure Stack bir dağıtım. Dağıtım, bir güvenlik duvarının arkasına devam edebilir, ancak genel VIP aralığına doğrudan genel İnternet 'ten ve Azure 'dan ulaşılabilir.
 
Aşağıdaki tabloda, karma bağlantı senaryoları uzmanları, dezavantajlar ve kullanım durumları özetlenmektedir.

| Senaryo | Bağlantı yöntemi | Uzmanları | Simgeler | Için iyi |
| -- | -- | --| -- | --|
| Tek kiracılı Azure Stack, intranet dağıtımı | Giden NAT | Daha hızlı aktarımlar için daha iyi bant genişliği. Basit uygulama; Ağ Geçidi gerekmez. | Trafik şifrelenmez; yığın dışında yalıtım veya şifreleme yoktur. | Tüm kiracıların eşit olarak güvendiği kurumsal dağıtımlar.<br><br>Azure 'da Azure ExpressRoute devresine sahip kuruluşlar. |
| Çok kiracılı Azure Stack, intranet dağıtımı | Siteden siteye VPN | Kiracı VNet 'ten hedefe giden trafik güvenlidir. | Bant genişliği siteden siteye VPN tüneli ile sınırlıdır.<br><br>Sanal ağda ağ geçidi ve hedef ağdaki bir VPN cihazı gerektirir. | Bazı kiracı trafiğinin diğer kiracılardan güvenli hale getirilmesi gereken kurumsal dağıtımlar. |
| Tek kiracılı Azure Stack, internet dağıtımı | Giden NAT | Daha hızlı aktarımlar için daha iyi bant genişliği. | Trafik şifrelenmez; yığın dışında yalıtım veya şifreleme yoktur. | Kiracının kendi Azure Stack dağıtımını aldığı ve Azure Stack ortamına adanmış bir devreni barındıran senaryolar. Örneğin, ExpressRoute ve çok protokollü etiket anahtarlama (MPLS).
| Çok kiracılı Azure Stack, internet dağıtımı | Siteden siteye VPN | Kiracı VNet 'ten hedefe giden trafik güvenlidir. | Bant genişliği siteden siteye VPN tüneli ile sınırlıdır.<br><br>Sanal ağda ağ geçidi ve hedef ağdaki bir VPN cihazı gerektirir. | Kiracının birbirlerine güvenmediği ve trafiğin şifrelenmesi gereken, sağlayıcının çok kiracılı bir bulut sunmak istediği barındırma senaryoları.
|  |  |  |  |  |

### <a name="using-expressroute"></a>ExpressRoute kullanma

Tek kiracılı intranet ve çok kiracılı senaryolar için Azure Stack [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) aracılığıyla Azure 'a bağlanabilirsiniz. [Bir bağlantı sağlayıcısı](https://docs.microsoft.com/azure/expressroute/expressroute-locations)aracılığıyla sağlanan bir ExpressRoute devresine ihtiyacınız olacak.

Aşağıdaki diyagramda tek kiracılı bir senaryoya yönelik ExpressRoute gösterilmektedir ("müşterinin bağlantısı" ExpressRoute devresine sahiptir).

![Tek kiracılı ExpressRoute senaryosunu gösteren diyagram](media/azure-stack-datacenter-integration/ExpressRouteSingleTenant.PNG)

Aşağıdaki diyagramda, çok kiracılı bir senaryo için ExpressRoute gösterilmektedir.

![Çok kiracılı ExpressRoute senaryosunu gösteren diyagram](media/azure-stack-datacenter-integration/ExpressRouteMultiTenant.PNG)

## <a name="external-monitoring"></a>Dış izleme
Azure Stack dağıtım ve cihazlarınızdaki tüm uyarıların tek bir görünümünü almak ve uyarıları mevcut BT hizmet yönetimi iş akışlarıyla bütünleştirmek için, [Azure Stack dış veri merkezi izleme çözümleriyle tümleştirebilirsiniz](azure-stack-integrate-monitor.md).

Azure Stack çözümüne dahil olmak üzere, donanım yaşam döngüsü Konağı, donanım için OEM satıcı tarafından sağlanan yönetim araçlarını çalıştıran Azure Stack dışındaki bir bilgisayardır. Bu araçları veya veri merkezinizdeki mevcut izleme çözümleriyle doğrudan tümleştirilen diğer çözümleri kullanabilirsiniz.

Aşağıdaki tabloda Şu anda kullanılabilir seçeneklerin listesi özetlenmektedir.

| Alan | Dış Izleme çözümü |
| -- | -- |
| Azure Stack yazılım | [Operations Manager için Azure Stack yönetim paketi](https://azure.microsoft.com/blog/management-pack-for-microsoft-azure-stack-now-available/)<br>[Nagios eklentisi](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details)<br>REST tabanlı API çağrıları | 
| Fiziksel sunucular (ıPMı aracılığıyla BMC 'ler) | OEM donanım-Operations Manager satıcı yönetim paketi<br>OEM donanım satıcısı tarafından sağlanmış çözüm<br>Donanım satıcısı Nagios eklentileri.<br>OEM iş ortağı tarafından desteklenen izleme çözümü (dahil) | 
| Ağ aygıtları (SNMP) | Ağ aygıtı bulmayı Operations Manager<br>OEM donanım satıcısı tarafından sağlanmış çözüm<br>Nagios anahtar eklentisi |
| Kiracı aboneliği sistem durumu izleme | [Windows Azure için System Center yönetim paketi](https://www.microsoft.com/download/details.aspx?id=50013) | 
|  |  | 

Aşağıdaki gereksinimleri dikkate alın:
- Kullandığınız çözüm aracısız olmalıdır. Üçüncü taraf aracıları Azure Stack bileşenleri içine yükleyemezsiniz. 
- System Center Operations Manager kullanmak istiyorsanız, Operations Manager 2012 R2 veya Operations Manager 2016 gereklidir.

## <a name="backup-and-disaster-recovery"></a>Yedekleme ve olağanüstü durum kurtarma

Yedekleme ve olağanüstü durum kurtarma planlaması, hem IaaS VM 'lerini hem de PaaS hizmetlerini barındıran temel Azure Stack altyapısının ve kiracı uygulamaları ile verilerin planlanmasını içerir. Bu şeyleri ayrı planlayın.

### <a name="protect-infrastructure-components"></a>Altyapı bileşenlerini koruma

Azure Stack altyapı bileşenlerini, belirttiğiniz bir SMB paylaşımında [yedekleyebilirsiniz](azure-stack-backup-back-up-azure-stack.md) :

- Mevcut bir Windows tabanlı dosya sunucusu veya üçüncü taraf cihazında bir dış SMB dosya paylaşımının olması gerekir.
- Ağ anahtarlarının ve donanım yaşam döngüsü konağının yedeklenmesi için aynı paylaşımın aynısını kullanın. OEM donanım satıcınız, Azure Stack dış olduklarından, bu bileşenlerin yedeklenmesi ve geri yüklenmesi için rehberlik sağlanmasına yardımcı olur. OEM satıcısının önerisi temelinde yedekleme iş akışlarını çalıştırmak sizin sorumluluğunuzdadır.

Çok önemli veri kaybı oluşursa, dağıtım verilerini yeniden dağıtmak için altyapı yedeklemesini kullanabilirsiniz: 

- Dağıtım girişleri ve tanımlayıcıları
- Hizmet hesapları
- CA kök sertifikası
- fFederated kaynakları (bağlantısı kesilen dağıtımlarda)
- Planlar, teklifler, abonelikler ve Kotalar
- RBAC ilkesi ve rol atamaları
- Key Vault gizli dizileri

### <a name="protect-tenant-apps-on-iaas-vms"></a>IaaS VM 'lerinde kiracı uygulamalarını koruma

Azure Stack kiracı uygulamalarını ve verilerini yedeketmez. Azure Stack hedef dış için yedekleme ve olağanüstü durum kurtarma koruması için plan yapmanız gerekir. Kiracı koruması, kiracı odaklı bir etkinliktir. IaaS VM 'Leri için, kiracılar dosya klasörlerini, uygulama verilerini ve sistem durumunu korumak için konuk içi teknolojileri kullanabilir. Ancak, bir kuruluş veya hizmet sağlayıcısı olarak, aynı veri merkezinde veya harici bir bulutta yedekleme ve kurtarma çözümü sunmak isteyebilirsiniz.

Linux veya Windows IaaS VM 'lerini yedeklemek için, dosya, klasör, işletim sistemi durumu ve uygulama verilerini korumak üzere Konuk işletim sistemine erişimi olan yedekleme ürünlerini kullanmanız gerekir. Azure Backup, System Center Data Center koruma Yöneticisi veya desteklenen üçüncü taraf ürünlerini kullanabilirsiniz.

Verileri ikincil bir konuma çoğaltmak ve bir olağanüstü durum oluşursa uygulama yük devretmesini düzenlemek için Azure Site Recovery veya desteklenen üçüncü taraf ürünlerini kullanabilirsiniz. Ayrıca, Microsoft SQL Server gibi yerel çoğaltmayı destekleyen uygulamalar, verilerin uygulamanın çalıştığı başka bir konuma çoğaltılmasını sağlayabilir.

## <a name="learn-more"></a>Daha fazla bilgi edinin

- Kullanım örnekleri, satın alma, iş ortakları ve OEM donanım satıcıları hakkında daha fazla bilgi için [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) ürün sayfasına bakın.
- Azure Stack tümleşik sistemler için yol haritası ve coğrafi kullanılabilirlik hakkında daha fazla bilgi için, bkz. Teknik İnceleme: [Azure Stack: Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/)uzantısı. 

## <a name="next-steps"></a>Sonraki adımlar
[Azure Stack dağıtımı bağlantı modelleri](azure-stack-connection-models.md)
