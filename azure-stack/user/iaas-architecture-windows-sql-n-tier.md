---
title: SQL Server ile Azure Stack Windows N katmanlı uygulama | Microsoft Docs
description: SQL Server Azure Stack Windows N katmanlı bir uygulamayı nasıl çalıştıracağınızı öğrenin.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: how-to
ms.date: 11/01/2019
ms.author: mabrigg
ms.reviewer: kivenkat
ms.lastreviewed: 11/01/2019
ms.openlocfilehash: ced042ac48017a8191d02e48de12e107677051fc
ms.sourcegitcommit: 8a74a5572e24bfc42f71e18e181318c82c8b4f24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73569111"
---
# <a name="windows-n-tier-application-on-azure-stack-with-sql-server"></a>SQL Server ile Azure Stack Windows N katmanlı uygulama

Bu başvuru mimarisinde, sanal makinelerin (VM 'Ler) ve [N katmanlı](https://docs.microsoft.com/azure/architecture/guide/architecture-styles/n-tier) bir uygulama için yapılandırılmış bir sanal ağın, veri katmanı için Windows üzerinde SQL Server kullanılarak nasıl dağıtılacağı gösterilir. 

## <a name="architecture"></a>Mimari

Mimari aşağıdaki bileşenlere sahiptir.

![](./media/iaas-architecture-windows-sql-n-tier/image1.png)

## <a name="general"></a>Genel

-   **Kaynak grubu**. [Kaynak grupları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) , Azure kaynaklarını, ömür, sahip veya diğer ölçütlere göre yönetilebilmeleri için gruplamak üzere kullanılır.

-   **Kullanılabilirlik kümesi.** Kullanılabilirlik kümesi, VM artıklığı ve kullanılabilirliği sağlamak için bir veri merkezi yapılandırması. Bir Azure Stack damgası içindeki bu yapılandırma, planlı veya plansız bir bakım olayı sırasında en az bir sanal makinenin kullanılabilir olmasını sağlar. VM 'Ler, birden çok hata etki alanına (Azure Stack ana bilgisayar) yayılmış bir kullanılabilirlik kümesine yerleştirilir

## <a name="networking-and-load-balancing"></a>Ağ ve Yük Dengeleme

-   **Sanal ağ ve alt ağlar**. Her Azure VM, alt ağlara bölünalabilecek bir sanal ağa dağıtılır. Her katman için ayrı bir alt ağ oluşturun.

-   **Katman 7 Load Balancer.** Azure Stack Application Gateway henüz kullanılamadığından, şu gibi [Azure Stack Pazar yerinde](https://docs.microsoft.com/azure-stack/operator/azure-stack-marketplace-azure-items?view=azs-1908) kullanılabilecek alternatifler vardır: [Kemp LOADMASTER Load Balancer ADC içerik anahtarı](https://azuremarketplace.microsoft.com/marketplace/apps/kemptech.vlm-azure)/ [F5 Big-IP Virtual Edition](https://azuremarketplace.microsoft.com/marketplace/apps/f5-networks.f5-big-ip-best) veya [a10 vThunder ADC](https://azuremarketplace.microsoft.com/marketplace/apps/a10networks.vthunder-414-gr1)

-   **Yük dengeleyiciler**. Web katmanından iş katmanına ve iş katmanından SQL Server kadar ağ trafiğini dağıtmak için [Azure Load Balancer ](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)kullanın.

-   **Ağ güvenlik grupları** (NSG 'ler). Sanal ağ içindeki ağ trafiğini kısıtlamak için NSG 'leri kullanın. Örneğin, burada gösterilen üç katmanlı mimaride, veritabanı katmanı Web ön ucundan gelen trafiği kabul etmez, yalnızca iş katmanından ve yönetim alt ağından.

-   **DNS**. Azure Stack kendi DNS barındırma hizmetini sağlamıyor, bu nedenle lütfen ekleinizdeki DNS sunucusunu kullanın.

**Sanal makineler**

-   **SQL Server Always On Kullanılabilirlik Grubu**. Veri katmanında yüksek kullanılabilirlik sağlayarak çoğaltmaya ve yük devretmeye olanak tanır. Yük devretme için Windows Server yük devretme kümesi (WSFC) teknolojisini kullanır.

-   **Active Directory Domain Services (AD DS) Sunucuları**. Yük devretme kümesi ve ilişkili kümelenmiş rollerinin bilgisayar nesneleri Active Directory Domain Services (AD DS) ' de oluşturulur. Aynı sanal ağdaki VM 'lerdeki AD DS sunucularının ayarlanması, diğer VM 'lere AD DS eklemek için tercih edilen yöntemdir. Ayrıca sanal ağı VPN bağlantısıyla kurumsal ağa bağlayarak VM 'Leri mevcut kurumsal AD DS de katabilirsiniz. Her iki yaklaşımdan, AD DS etki alanı FQDN 'sini çözümlemek için sanal ağ DNS 'yi AD DS DNS sunucunuz (sanal ağ veya mevcut kurumsal ağ) ile değiştirmeniz gerekir.

-   **Bulut tanığı**. Yük devretme kümesi, üzerinde çekirdekte olan ve çekirdekte olduğu bilinen düğümlerin yarısını gerektirir. Kümede yalnızca iki düğüm varsa, bir ağ bölümü her bir düğümün ana düğüm olduğunu düşünmesine neden olabilir. Bu durumda, özellikleri bölmek ve çekirdek oluşturmak için bir *tanık* gerekir. Tanık, çekirdek oluşturmak için bir bağlama ayırıcı görevi gören paylaşılan disk gibi bir kaynaktır. Bulut tanığı, Azure Blob depolamayı kullanan bir tanık türüdür. Çekirdek kavramı hakkında daha fazla bilgi edinmek için bkz. [küme ve havuz çekirdeğini anlama](https://docs.microsoft.com/windows-server/storage/storage-spaces/understand-quorum). Bulut tanığı hakkında daha fazla bilgi için bkz. [Yük devretme kümesi Için bulut tanığı dağıtma](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness). Azure Stack, bulut tanık uç noktası küresel Azure 'dan farklıdır. 

Şöyle görünebilir:

- Küresel Azure için:  
  `https://mywitness.blob.core.windows.net/`

- Azure Stack için:  
  `https://mywitness.blob.<region>.<FQDN>`

-   **Sıçrama kutusu**. [Kale ana bilgisayarı](https://en.wikipedia.org/wiki/Bastion_host) olarak da bilinir. Ağ üzerinde yer alan ve yöneticilerin diğer VM’lere bağlanmak için kullandıkları güvenli bir VM’dir. Sıçrama kutusunun sadece güvenli bir listede yer alan genel IP adreslerinden gelen uzak trafiğe izin veren bir NSG’si vardır. NSG, uzak masaüstü (RDP) trafiğine izin vermelidir.

## <a name="recommendations"></a>Öneriler

Gereksinimleriniz, burada açıklanan mimariden farklı olabilir. Bu önerileri bir başlangıç noktası olarak kullanın.

### <a name="virtual-machines"></a>Sanal makineler

VM 'Leri yapılandırmayla ilgili öneriler için, bkz. [Azure Stack WINDOWS VM çalıştırma](iaas-architecture-vm-windows.md).

### <a name="virtual-network"></a>Sanal ağ

Sanal ağı oluştururken, her bir alt ağdaki kaynaklarınızın kaç IP adresi gerektirdiğini saptayın. [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) gösterimini kullanarak, bir alt ağ maskesi ve gerekli IP adresleri için yeterince büyük bir ağ adresi aralığı belirtin. Standart [özel IP adresi blokları](https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces)içinde, 10.0.0.0/8, 172.16.0.0/12 ve 192.168.0.0/16 olan bir adres alanı kullanın.

Sanal ağ ile şirket içi ağınız arasında bir ağ geçidi ayarlamanız gerekirse şirket içi ağınızla çakışmayacak bir adres aralığı seçin. Sanal ağı oluşturduktan sonra adres aralığını değiştiremezsiniz.

Alt ağları tasarlarken işlev ve güvenlik gereksinimlerini göz önünde bulundurun. Aynı katmandaki veya roldeki tüm VM’ler, güvenlik sınırı olabilecek aynı alt ağa eklenmelidir. Sanal ağları ve alt ağları tasarlama hakkında daha fazla bilgi için bkz. [Azure sanal ağlarını planlayın ve tasarlayın](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm).

### <a name="load-balancers"></a>Yük dengeleyiciler

VM 'Leri doğrudan Internet 'e sunmaz, bunun yerine her VM 'ye özel bir IP adresi verin. İstemciler, katman 7 Load Balancer ilişkili genel IP adresini kullanarak bağlanır.

Ağ trafiğini sanal makinelere yönlendirmek için yük dengeleyici kuralları tanımlayın. Örneğin, HTTP trafiğini etkinleştirmek için 80 bağlantı noktasını ön uç yapılandırmasından arka uç adres havuzundaki 80 numaralı bağlantı noktasına eşleyin. İstemci, 80 numaralı bağlantı noktasına bir HTTP isteği gönderdiğinde, yük dengeleyici, kaynak IP adresini içeren bir [karma algoritması](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview#fundamental-load-balancer-features) kullanarak arka uç IP adresi seçer. İstemci istekleri arka uç adres havuzundaki tüm VM 'lerde dağıtılır.

### <a name="network-security-groups"></a>Ağ güvenlik grupları

Katmanlar arasında trafiği kısıtlamak için NSG kurallarını kullanın. Yukarıda gösterilen üç katmanlı mimaride, Web katmanı veritabanı katmanıyla doğrudan iletişim kurmaz. Bu kuralı zorlamak için, veritabanı katmanının Web katmanı alt ağından gelen trafiği engellemesi gerekir.

1.  Sanal ağdan gelen tüm trafiği reddedin. (Kuraldaki VIRTUAL_NETWORK etiketini kullanın.)

2.  İş katmanı alt ağından gelen trafiğe izin verin.

3.  Veritabanı katmanı alt ağının kendisinden gelen trafiğe izin verin. Bu kural, veritabanı çoğaltması ve yük devretme için gerekli olan veritabanı VM 'Leri arasında iletişime olanak sağlar.

4.  Sıçrama kutusu alt ağından gelen RDP trafiğine (bağlantı noktası 3389) izin verin. Bu kural; yöneticilerin, sıçrama kutusundan veritabanı katmanına bağlanmasını sağlar.

İlk kuraldan daha yüksek önceliğe sahip 2 – 4 kuralları oluşturun, böylece geçersiz kılar.

## <a name="sql-server-always-on-availability-groups"></a>SQL Server Always On Kullanılabilirlik Grupları

SQL Server yüksek kullanılabilirlik için [Always on kullanılabilirlik grupları](https://msdn.microsoft.com/library/hh510230.aspx) önerilir. Windows Server 2016’dan önceki sürümlerde, Always On Kullanılabilirlik Grupları bir etki alanı denetleyicisi gerektirir ve kullanılabilirlik grubundaki tüm düğümler aynı AD etki alanında olmalıdır.

VM katmanı yüksek kullanılabilirliği için tüm SQL sanal makineleri bir kullanılabilirlik kümesinde olmalıdır.

Diğer katmanlar, bir [kullanılabilirlik grubu dinleyicisi](https://msdn.microsoft.com/library/hh213417.aspx)aracılığıyla veritabanına bağlanır. Dinleyici; SQL istemcisinin, SQL Server fiziksel örneğinin adını bilmeden bağlantı kurmasına olanak sağlar. Veritabanına erişen VM’lerin etki alanına dahil edilmesi gerekir. Dinleyicinin sanal ağ adını IP adreslerine çözümlemek için istemci (bu durumda diğer bir katman), DNS kullanır.

SQL Server Always On Kullanılabilirlik Grubunu aşağıdaki şekilde yapılandırın:

1.  Bir Windows Server Yük Devretme Kümelemesi (WSFC) kümesi, bir SQL Server Always On Kullanılabilirlik Grubu ve birincil kopya oluşturun. Daha fazla bilgi için bkz. [Always on kullanılabilirlik gruplarıyla çalışmaya](https://msdn.microsoft.com/library/gg509118.aspx)başlama.

2.  Statik özel IP adresli bir iç yük dengeleyici oluşturun.

3.  Bir kullanılabilirlik grubu dinleyicisi oluşturun ve dinleyicinin DNS adını bir iç yük dengeleyicinin IP adresiyle eşleyin.

4.  SQL Server dinleme bağlantı noktası (varsayılan olarak 1433 numaralı TCP bağlantı noktası) için bir yük dengeleyici kuralı oluşturun. Yük dengeleyici kuralı, Doğrudan Sunucu Dönüşü olarak da adlandırılan *kayan IP*’yi etkinleştirmelidir. Böylece VM doğrudan istemciye yanıt verebilir, bu da birincil çoğaltma ile doğrudan bağlantı kurulabilmesini sağlar.

> [!Note]
> Kayan IP etkin olduğunda ön uç bağlantı noktası, yük dengeleyici kuralındaki arka uç bağlantı noktası numarasıyla aynı olmalıdır.

SQL istemcisi bağlantı kurmayı denediğinde yük dengeleyici, bağlantı isteğini birincil çoğaltmaya yönlendirir. Başka bir çoğaltmaya yük devretme varsa, yük dengeleyici yeni istekleri otomatik olarak yeni bir birincil çoğaltmaya yönlendirir. Daha fazla bilgi için bkz. [SQL Server Always on kullanılabilirlik grupları için BIR ıLB dinleyicisi yapılandırma](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener).

Yük devretme sırasında mevcut istemci bağlantıları kapatılır. Yük devretme işlemi tamamlandıktan sonra yeni bağlantılar yeni birincil çoğaltmaya yönlendirilir.

Uygulamanız yazma işlemleri için daha fazla okuma yapıyorsa, salt okunur sorguların bazılarını ikincil bir çoğaltmaya devretmek için bu işlemleri yapabilirsiniz. Bkz. [salt bir Ikincil çoğaltmaya (salt okuma yönlendirmesi) bağlanmak Için dinleyici kullanma](https://technet.microsoft.com/library/hh213417.aspx#ConnectToSecondary).

Kullanılabilirlik grubunun [el ile yük devretmesini zorlayarak](https://msdn.microsoft.com/library/ff877957.aspx) dağıtımınızı test edin.

SQL performansı iyileştirmesi için [SQL Server en iyi uygulamaları makalesine başvurarak Azure Stack performansını en iyi duruma](https://docs.microsoft.com/azure-stack/user/azure-stack-sql-server-vm-considerations)getirebilirsiniz.

**Sıçrama kutusu**

Genel Internet 'ten uygulama iş yükünü çalıştıran VM 'lere RDP erişimine izin verme. Bunun yerine, bu VM 'lere yönelik tüm RDP erişimleri sıçrama kutusunu göstermelidir. Bir yönetici, sıçrama kutusunda oturum açar ve sonra sıçrama kutusundan diğer VM’de oturum açar. Sıçrama kutusu İnternet’ten gelen RDP trafiğine izin verir ancak bu, yalnızca bilinen ve güvenli IP adresleri için geçerlidir.

Sıçrama kutusunda en düşük performans gereksinimleri bulunur, bu nedenle küçük bir VM boyutu seçin. Sıçrama kutusu için bir [genel IP adresi](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm) oluşturun. Atlama kutusunu diğer VM 'Lerle aynı sanal ağa, ancak ayrı bir yönetim alt ağına yerleştirin.

Sıçrama kutusunu güvenli hale getirmek için yalnızca güvenli bir genel IP adresi kümesinden gelen RDP bağlantılarına izin veren bir NSG kuralı ekleyin. Diğer alt ağlara yönelik NSG’leri yönetim alt ağından gelen RDP trafiğine izin verecek şekilde yapılandırın.

## <a name="scalability-considerations"></a>Ölçeklenebilirlik konusunda dikkat edilmesi gerekenler

### <a name="scale-sets"></a>Ölçek kümeleri

Web ve iş katmanları için, ayrı VM 'Ler dağıtmak yerine [sanal makine ölçek kümelerini](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview) kullanmayı düşünün. Ölçek kümesi, bir özdeş VM 'lerin bir kümesini dağıtmayı ve yönetmeyi kolaylaştırır. VM 'Leri hızlıca ölçeklendirmeniz gerekiyorsa ölçek kümelerini değerlendirin.

Bir ölçek kümesinde dağıtılmış VM'leri yapılandırmanın iki temel yolu vardır:

-   Sanal makineyi dağıtıldıktan sonra yapılandırmak için uzantıları kullanın. Bu yaklaşımda, yeni VM örneklerinin başlatılması uzantı içermeyen bir VM ile karşılaştırıldığında daha uzun sürebilir.

-   Özel bir disk görüntüsü ile [yönetilen disk](https://docs.microsoft.com/azure-stack/user/azure-stack-managed-disk-considerations) dağıtma. Bu seçenek daha hızlı dağıtılabilir. Ancak, görüntüyü güncel tutmanız gerekir.

Daha fazla bilgi için bkz. [Ölçek Kümeleri Için tasarım konuları](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview). Bu tasarım, genellikle Azure Stack için geçerlidir, ancak bazı uyarılar vardır:

-   Azure Stack üzerindeki sanal makine ölçek kümeleri fazla sağlamayı veya sıralı yükseltmeleri desteklemez.

-   Azure Stack sanal makine ölçek kümelerini otomatik ölçeklendirme yapılamaz.

-   Sanal makine ölçek kümesi için yönetilmeyen diskler yerine Azure Stack yönetilen diskleri kullanmanız önemle önerilir

-   Şu anda Azure Stack, tüm Azure Stack altyapı VM 'Leri, tek VM 'Ler ve ölçek kümesi örnekleri için hesaplar olan bir 700 VM sınırı vardır.

## <a name="subscription-limits"></a>Abonelik sınırları

Her Azure Stack kiracı aboneliği, Azure Stack işleci tarafından yapılandırılmış bölge başına en fazla VM sayısı dahil olmak üzere varsayılan sınırlara sahiptir. Daha fazla bilgi için bkz. [Azure Stack Hizmetleri, planlar, teklifler, abonelikler genel bakış](https://docs.microsoft.com/azure-stack/operator/service-plan-offer-subscription-overview). Ayrıca [Azure Stack kota türleri '](https://docs.microsoft.com/azure-stack/operator/azure-stack-quota-types)ne bakın.

## <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler

Azure’daki sanal ağlar birer trafik yalıtımı sınırdır. Varsayılan olarak, bir sanal ağdaki VM 'Ler, farklı bir sanal ağdaki VM 'lerle doğrudan iletişim kuramaz.

**NSG'ler**. İnternet 'ten gelen ve giden trafiği kısıtlamak için [ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (NSG 'ler) kullanın. Daha fazla bilgi için bkz. [Microsoft bulut hizmetleri ve ağ güvenliği](https://docs.microsoft.com/azure/best-practices-network-security).

**DMZ**. İnternet ile Azure sanal ağı arasında bir DMZ oluşturmak için bir ağ sanal gereci (NVA) eklemeyi deneyin. NVA; güvenlik duvarı, paket inceleme, denetim ve özel yönlendirme gibi ağ ile ilgili görevleri gerçekleştirebilen bir sanal gereci tanımlamak için kullanılan genel bir terimdir.

**Şifreleme**. Bekleyen hassas verileri şifreleyin ve veritabanı şifreleme anahtarlarını yönetmek için [Azure Stack Key Vault](https://docs.microsoft.com/azure-stack/user/azure-stack-key-vault-manage-portal) kullanın. Daha fazla bilgi için bkz. [Azure VM’lerde SQL Server için Azure Anahtar Kasası Tümleştirmeyi Yapılandırma](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-ps-sql-keyvault). Ayrıca, Key Vault ' de veritabanı bağlantı dizeleri gibi uygulama gizli dizileri depolamanız önerilir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure bulut desenleri hakkında daha fazla bilgi edinmek için bkz. [bulut tasarım desenleri](https://docs.microsoft.com/azure/architecture/patterns).
