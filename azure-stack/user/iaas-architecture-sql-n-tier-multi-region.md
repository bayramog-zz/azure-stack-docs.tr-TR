---
title: Yüksek kullanılabilirlik için birden fazla Azure Stack bölgesinde N katmanlı bir uygulama çalıştırma | Microsoft Docs
description: Yüksek kullanılabilirlik için birden çok Azure Stack bölgede N katmanlı bir uygulamayı çalıştırmayı öğrenin.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: how-to
ms.date: 11/01/2019
ms.author: mabrigg
ms.reviewer: kivenkat
ms.lastreviewed: 11/01/2019
ms.openlocfilehash: acfeebe626d7745fe200724c8c53c632bada1466
ms.sourcegitcommit: 8a74a5572e24bfc42f71e18e181318c82c8b4f24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73569155"
---
# <a name="run-an-n-tier-application-in-multiple-azure-stack-regions-for-high-availability"></a>Yüksek kullanılabilirlik için birden çok Azure Stack bölgede N katmanlı bir uygulama çalıştırma

Bu başvuru mimarisi, kullanılabilirliği ve sağlam bir olağanüstü durum kurtarma altyapısı elde etmek için N katmanlı bir uygulama birden çok Azure Stack bölgesinde çalışmaya yönelik kanıtlanmış bir uygulamalar kümesini gösterir. Bu belgede, yüksek kullanılabilirlik elde etmek için Traffic Manager kullanılır, Traffic Manager ancak ortamınızda tercih edilen bir seçenek yoksa, yüksek oranda kullanılabilir yük dengeleyiciler çifti de değiştirilebilir.

> [!Note]  
> Lütfen aşağıdaki mimaride kullanılan Traffic Manager Azure 'da yapılandırılması gerektiğini ve Traffic Manager profilini yapılandırmak için kullanılan uç noktaların genel olarak yönlendirilebilir IP 'Leri olması gerektiğini unutmayın.

## <a name="architecture"></a>Mimari

Bu mimari, [SQL Server ile birlikte N katmanlı uygulamada](iaas-architecture-windows-sql-n-tier.md)gösterilen bir yapı üzerinde oluşturulur.

![Azure N katmanlı uygulamalar için yüksek düzeyde kullanılabilir ağ mimarisi](./media/iaas-architecturesql-n-tier-multi-region/image1.png)

-   **Birincil ve ikincil bölgeler**. Yüksek kullanılabilirlik elde etmek için iki bölge kullanın. Bunlardan biri, birincil bölgedir. Diğeri yük devretme bölgesidir.

-   **Azure Traffic Manager**. [Traffic Manager](https://azure.microsoft.com/services/traffic-manager) gelen istekleri bölgelerden birine yönlendirir. Normal işlemler sırasında, istekleri birincil bölgeye yönlendirir. Bu bölge kullanılamaz duruma gelirse Traffic Manager, yükü ikincil bölgeye devreder. Daha fazla bilgi için bkz. [Traffic Manager yapılandırması](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/multi-region-sql-server#traffic-manager-configuration).

-   **Kaynak grupları**. Birincil bölge, ikincil bölge için ayrı [kaynak grupları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) oluşturun. Bu size her bölgeyi tek bir kaynak koleksiyonu olarak yönetme esnekliği sağlar. Örneğin, bir bölgeyi devre dışı bırakmadan diğer bölgeyi yeniden dağıtabilirsiniz. Uygulama için tüm kaynakları listelemek üzere bir sorgu çalıştırabilmeniz için [kaynak gruplarını bağlayın](https://docs.microsoft.com/azure/resource-group-link-resources).

-   **Sanal ağlar**. Her bölge için ayrı bir sanal ağ oluşturun. Adres alanlarının çakışmadığından emin olun.

-   **SQL Server Always On Kullanılabilirlik Grubu**. SQL Server kullanıyorsanız, yüksek kullanılabilirlik için [SQL Always on kullanılabilirlik grupları](https://msdn.microsoft.com/library/hh510230.aspx) önerilir. Her iki bölgedeki SQL Server örneklerini içeren tek bir kullanılabilirlik grubu oluşturun.

-   **VNET 'TEN VNET 'e VPN bağlantısı**. Sanal Ağ Eşleme, Azure Stack üzerinde henüz kullanılamadığından, iki sanal ağa bağlanmak için sanal ağ VPN bağlantısı ' nı kullanın. Daha fazla bilgi için lütfen [Azure Stack içindeki VNET 'TEN VNET 'e](https://docs.microsoft.com/azure-stack/user/azure-stack-network-howto-vnet-to-vnet?view=azs-1908) bakın.

## <a name="recommendations"></a>Öneriler

Çok bölgeli bir mimari, tek bir bölgeye dağıtmaya göre daha yüksek kullanılabilirlik sağlayabilir. Bölgesel bir kesinti birincil bölgeyi etkiliyorsa, ikincil bölgeye yük devretmek için [Traffic Manager](https://azure.microsoft.com/services/traffic-manager) kullanabilirsiniz. Mimari ayrıca tek başına bir alt sistem veya uygulama başarısız olursa da yardımcı olabilir.

Bölgeler arasında yüksek kullanılabilirlik sağlamak için birkaç genel yaklaşım bulunur:

-   **Etkin bekleme Ile etkin/Pasif**. Trafik bir yöne doğru giderken diğerinin etkin beklemesi. Etkin bekleme, ikinci bölgedeki VM’lerin her zaman ayrılmış ve çalışır olduğu anlamına gelir.

-   **Soğuk bekleme Ile etkin/Pasif**. Trafik bir yöne doğru giderken diğerinin etkin olmayan hazırda beklemesi. Etkin olmayan hazırda bekleme, ikinci bölgedeki VM’lere yük devretme için gerekli olana kadar bölge atanmayacağı anlamına gelir. Bu yaklaşımı çalıştırmak daha düşük maliyetlidir ancak bir hata sırasında tekrar çevrimiçi olması genellikle daha uzun sürer.

-   **Etkin/etkin**. İki bölge de etkin ve aralarında yük dengelemesi yapılıyor. Bir bölge kullanılamaz duruma gelirse, döndürme dışına alınır.

Bu başvuru mimarisi, yük devretme için Traffic Manager’ı kullanarak etkin bekleme ile aktif/pasif durumuna odaklanır. Etkin bekleme için az sayıda sanal makine dağıtabilir ve sonra ölçeği gerektiği gibi değiştirebilirsiniz.

### <a name="traffic-manager-configuration"></a>Traffic Manager yapılandırması

Traffic Manager’ı yapılandırırken aşağıdaki noktaları göz önünde bulundurun:

-   **Yönlendirme**. Traffic Manager çeşitli [yönlendirme algoritmalarını](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods)destekler. Bu makalede açıklanan senaryo için, *öncelikli* yönlendirmeyi (eski adıyla *yük devretme* yönlendirmesini) kullanın. Bu ayar kullanıldığında, birincil bölge ulaşılamaz duruma gelmediği sürece Traffic Manager tüm istekleri birincil bölgeye gönderir. Bu noktada, otomatik olarak ikinci bölgeye yük devredilir. Bkz. [Yük devretme yönlendirme yöntemini yapılandırma](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-configure-failover-routing-method).

-   **Sistem durumu araştırma**. Traffic Manager, her bölgenin kullanılabilirliğini izlemek için HTTP (veya HTTPS) [araştırması](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring) kullanır. Yoklama, belirtilen bir URL yolu için bir HTTP 200 yanıtının alınıp alınmadığını denetler. En iyi uygulama olarak, uygulamanın genel durumunu raporlayan bir uç nokta oluşturun ve durum yoklaması için bu uç noktayı kullanın. Aksi takdirde, gerçekte uygulamanın kritik bölümleri başarısız olduğu halde araştırma uç noktanın sistem durumunun iyi olduğunu raporlayabilir. Daha fazla bilgi için bkz. [sistem durumu uç nokta izleme model](https://docs.microsoft.com/azure/architecture/patterns/health-endpoint-monitoring).

Traffic Manager yük devrettiğinde istemcilerin uygulamaya ulaşamadığı bir zaman dilimi olur. Bu süre aşağıdaki faktörlerden etkilenir:

-   Durum yoklaması, birincil bölgenin ulaşılamaz hale geldiğini algılamalıdır.

-   DNS sunucuları, önbelleğe alınan DNS kayıtlarını IP adresi için güncelleştirmelidir; bu, DNS yaşam süresine (TTL) bağlıdır. Varsayılan TTL 300 saniyedir (5 dakika), ancak Traffic Manager profilini oluştururken bu değeri yapılandırabilirsiniz.

Ayrıntılar için bkz. [Traffic Manager Izleme hakkında](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring).

Traffic Manager yük devrederse, otomatik bir yeniden çalışma uygulamak yerine el ile bir yeniden çalışma gerçekleştirmenizi öneririz. Aksi takdirde, uygulamanın bölgeler arasında sürekli olarak geçiş yaptığı bir durum oluşturabilirsiniz. Yeniden çalıştırmadan önce tüm uygulama alt sistemlerinin iyi durumda olduğunu doğrulayın.

Traffic Manager’ın varsayılan olarak otomatik yeniden çalışma gerçekleştirdiğine dikkat edin. Bunu önlemek için, bir yük devretme olayından sonra birinci bölgenin önceliğini el ile düşürün. Örneğin, birincil bölge öncelik 1 ve ikincil bölge öncelik 2 değerine sahip olsun. Bir yük devretmeden sonra, otomatik yeniden çalışmayı önlemek için birincil bölgenin önceliğini 3 olarak ayarlayın. Geri geçmeye hazırsanız, önceliği 1 olarak güncelleştirin.

Aşağıdaki [Azure CLI](https://docs.microsoft.com/cli/azure/) komutu önceliği güncelleştirir:

```cli  
az network traffic-manager endpoint update --resource-group <resource-group> --profile-name <profile>
    --name <endpoint-name> --type externalEndpoints --priority 3
```

Diğer bir yöntem de ilk duruma döndürmeye hazır olana kadar uç noktayı devre dışı bırakmaktır:

```cli
az network traffic-manager endpoint update --resource-group <resource-group> --profile-name <profile>
    --name <endpoint-name> --type externalEndpoints --endpoint-status Disabled
```

Yük devretme nedenine bağlı olarak, bir bölgede kaynakları yeniden dağıtmanız gerekebilir. İlk duruma döndürmeden önce bir işlemsel hazırlık testi gerçekleştirin. Test aşağıdaki gibi durumları doğrulamalıdır:

-   VM’lerin doğru şekilde yapılandırılmış olması. (Gerekli olan tüm yazılımların yüklenmiş olması, IIS’nin çalışıyor olması vb.)

-   Uygulama alt sistemlerinin iyi durumda olması.

-   İşlevsel test. (Örneğin, web katmanından veritabanı katmanına ulaşılabilmesi.)

### <a name="configure-sql-server-always-on-availability-groups"></a>SQL Server Always On Kullanılabilirlik Grupları yapılandırma

Windows Server 2016’dan önceki sürümlerde SQL Server Always On Kullanılabilirlik Grupları bir etki alanı denetleyicisi gerektirir ve kullanılabilirlik grubundaki tüm düğümler aynı Active Directory (AD) etki alanında olmalıdır.

Kullanılabilirlik grubunu yapılandırmak için:

-   Her bir bölgeye en az iki etki alanı denetleyicisi yerleştirin.

-   Her etki alanı denetleyicisine statik bir IP adresi verin.

-   İki sanal ağ arasında iletişimi etkinleştirmek için [VPN](https://docs.microsoft.com/azure-stack/user/azure-stack-vpn-gateway-about-vpn-gateways) oluşturun.

-   Her sanal ağ için, etki alanı denetleyicilerinin IP adreslerini (her iki bölgeden) DNS sunucusu listesine ekleyin. Aşağıdaki CLI komutunu kullanabilirsiniz. Daha fazla bilgi için bkz. [DNS sunucularını değiştirme](https://docs.microsoft.com/azure/virtual-network/manage-virtual-network#change-dns-servers).

    ```cli
    az network vnet update --resource-group <resource-group> --name <vnet-name> --dns-servers "10.0.0.4,10.0.0.6,172.16.0.4,172.16.0.6"
    ```

-   Her iki bölgedeki SQL Server örneklerini içeren bir [Windows Server Yük Devretme Kümelemesi](https://msdn.microsoft.com/library/hh270278.aspx) (wsfc) kümesi oluşturun.

-   Hem birincil hem de ikincil bölgedeki SQL Server örneklerini içeren bir SQL Server Always On Kullanılabilirlik Grubu oluşturun. Adımlar için bkz. [Always On Kullanılabilirlik Grubu’nu Uzak Azure Veri Merkezine Genişletme (PowerShell)](https://techcommunity.microsoft.com/t5/DataCAT/Extending-AlwaysOn-Availability-Group-to-Remote-Azure-Datacenter/ba-p/305217).

    -   Birincil çoğaltmayı birincil bölgeye yerleştirin.

    -   Birincil bölgeye bir veya daha fazla ikincil çoğaltma yerleştirin. Bunları otomatik yük devretme ile zaman uyumlu yürütme kullanacak şekilde yapılandırın.

    -   İkincil bölgeye bir veya daha fazla ikincil çoğaltma yerleştirin. Bunları performans açısından iyi olan *zaman uyumsuz* yürütme kullanacak şekilde yapılandırın. (Aksi takdirde, tüm T-SQL işlemlerinin ağ üzerinden ikincil bölgeye gidiş dönüş için beklemesi gerekir.)

> [!Note]  
> Zaman uyumsuz kayıt çoğaltmaları otomatik yük devretmeyi desteklemez.

## <a name="availability-considerations"></a>Kullanılabilirlik konusunda dikkat edilmesi gerekenler

Karmaşık N katmanlı bir uygulama ile ikincil bölgede tüm uygulamayı çoğaltmanız gerekmeyebilir. Bunun yerine, yalnızca iş sürekliliğinin desteklenmesi için gerekli önemli bir alt sistemi çoğaltabilirsiniz.

Traffic Manager sistemde olası bir hata noktasıdır. Traffic Manager hizmeti başarısız olursa istemciler, kapalı kalma süresinde uygulamanıza erişemez. [Traffic Manager SLA 'sını](https://azure.microsoft.com/support/legal/sla/traffic-manager)gözden geçirin ve Traffic Manager kullanmanın yalnızca yüksek kullanılabilirlik için iş gereksinimlerinizi karşılayıp karşılamadığını saptayın. Aksi durumda, yeniden çalıştırma çözümü olarak başka bir trafik yönetim çözümü eklemeyi göz önünde bulundurun. Azure Traffic Manager hizmeti başarısız olursa, DNS’teki CNAME kayıtlarınızı diğer trafik yönetimi hizmetini gösterecek şekilde değiştirin. (Bu adımın el ile uygulanması gerekir ve uygulamanız DNS değişiklikleri yayılana kadar kullanılamaz.)

SQL Server kümesi için dikkate alınması gereken iki yük devretme senaryosu vardır:

-   Birincil bölgedeki tüm SQL Server veritabanı çoğaltmalarının başarısız olması. Örneğin, bölgesel bir kesinti sırasında bu durum meydana gelebilir. Bu durumda, Traffic Manager otomatik olarak ön uçta yük devretme gerçekleştirse bile kullanılabilirlik grubunun yükünü el ile devretmeniz gerekir. SQL Server 2016’da SQL Server Management Studio, Transact-SQL veya PowerShell kullanarak zorlamalı yük devretme gerçekleştirmeyi açıklayan [SQL Server Kullanılabilirlik Grubunun Yükünü El ile Zorlamalı Olarak Devretme İşlemi Gerçekleştirme](https://msdn.microsoft.com/library/ff877957.aspx) konusundaki adımları izleyin.

    > [!Warning]  
    > Zorlamalı yük devretme yönteminde veri kaybı riski vardır. Birincil bölge yeniden çevrimiçi olduğunda, veritabanının anlık görüntüsünü alın ve farkları bulmak için [tablediff](https://msdn.microsoft.com/library/ms162843.aspx)’i kullanın.

-   Traffic Manager ikincil bölgeye yük devretme gerçekleştirir, ancak birincil SQL Server veritabanı çoğaltması kullanılabilir kalır. Örneğin, ön uç katmanı SQL Server sanal makinelerini etkilemeden başarısız olabilir. Bu durumda, İnternet trafiği ikincil bölgeye yönlendirilir ve bu bölge birincil çoğaltmaya bağlanmaya devam edebilir. Ancak, SQL Server bağlantıları bölgeler arasında gerçekleştiğinden gecikme süresi daha yüksek olur. Bu durumda, aşağıdaki gibi el ile yük devretme gerçekleştirmeniz gerekir:

    1.  İkincil bölgedeki bir SQL Server veritabanı çoğaltmasını geçici olarak *zaman uyumlu* işlemeye geçirin. Bu, yük devretme sırasında veri kaybı olmamasını sağlar.

    2.  Bu çoğaltmaya yük devretme gerçekleştirin.

    3.  Birincil bölgede yeniden çalışmaya başladığınızda zaman uyumsuz işleme ayarını geri yükleyin.

## <a name="manageability-considerations"></a>Yönetilebilirlik konusunda dikkat edilmesi gerekenler

Dağıtımınızı güncelleştirdiğinizde, yanlış bir yapılandırma veya uygulamadaki bir hata nedeniyle ortaya çıkabilecek genel hata riskini azaltmak için tek seferde bir uygulamayı güncelleştirin.

Sistemin hatalara karşı esnekliğini test edin. Test edebileceğiniz bazı yaygın hata senaryoları şunlardır:

-   VM örneklerini kapatın.

-   CPU ve bellek gibi baskı kaynakları.

-   Ağın bağlantısını kesin/ağı geciktirin.

-   İşlemlerde kilitlenme yaratın.

-   Sertifikaların süresinin dolmasını sağlayın.

-   Donanım hatalarının benzetimini yapın.

-   Etki alanı denetleyicilerindeki DNS hizmetini kapatın.

Kurtarma zamanlarını ölçün ve iş gereksinimlerinizin karşılandığını doğrulayın. Hata modlarının birleşimlerini de test edin.

## <a name="next-steps"></a>Sonraki adımlar

- Azure bulut desenleri hakkında daha fazla bilgi edinmek için bkz. [bulut tasarım desenleri](https://docs.microsoft.com/azure/architecture/patterns).