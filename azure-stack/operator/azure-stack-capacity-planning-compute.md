---
title: Azure Stack kapasite planlama işlem | Microsoft Docs
description: Azure Stack dağıtımları için kapasite planlaması hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: prchint
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/16/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 06/13/2019
ms.openlocfilehash: dac0360bba7c24c85d1f30efbfb7fad30eb97028
ms.sourcegitcommit: cefba8d6a93efaedff303d3c605b02bd28996c5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74299153"
---
# <a name="azure-stack-compute"></a>Azure Stack işlem

Azure Stack desteklenen [VM boyutları](https://docs.microsoft.com/azure-stack/user/azure-stack-vm-sizes) , Azure 'da desteklenenlerden oluşan bir alt kümesidir. Azure, kaynakların fazla tüketimini (sunucu yerel ve hizmet düzeyi) önlemek için kaynak sınırlarını birçok vektöre uygular. Kiracı tüketimine yönelik bazı sınırlar olmadan, kiracı deneyimleri, diğer kiracıların kaynakları fazla tükettiği durumlarda zarar görür. VM 'den ağ çıkışı için, Azure sınırlamalarıyla eşleşen Azure Stack bant genişliği üst sınırı vardır. Azure Stack üzerindeki depolama kaynakları için, depolama ıOPS sınırları, depolama erişimi için kiracılara göre temel kaynak tüketimine engel olmaz.

>[!IMPORTANT]
>[Azure Stack CAPACITY PLANNER](https://aka.ms/azstackcapacityplanner) IOPS performansını dikkate almaz veya garanti etmez.

## <a name="vm-placement"></a>VM yerleşimi

Azure Stack yerleştirme altyapısı, Kiracı VM 'lerini kullanılabilir Konaklar arasında koyar.

Azure Stack VM 'Leri yerleştirirken iki önemli noktalar kullanır. , Bu VM türü için konakta yeterli bellek vardır. İkisi, VM 'Lerin bir [kullanılabilirlik kümesinin](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) parçası veya [sanal makine ölçek kümeleri olduğunu belirler](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview).

Azure Stack bir çoklu VM üretim sisteminin yüksek oranda kullanılabilir olmasını sağlamak için, VM 'Ler birden çok hata etki alanına yayılan bir kullanılabilirlik kümesine yerleştirilir. Bir kullanılabilirlik kümesindeki bir hata etki alanı, ölçek biriminde tek bir düğüm olarak tanımlanır. Azure Stack, en fazla üç hata etki alanı içeren bir kullanılabilirlik kümesinin Azure ile tutarlı olmasını destekler. Bir kullanılabilirlik kümesine yerleştirilmiş VM 'Ler, birden çok hata etki alanına, yani Azure Stack konaklara olabildiğince eşit olarak yayarak birbirleriyle fiziksel olarak yalıtılacaktır. Bir donanım hatası varsa, başarısız hata etki alanındaki VM 'Ler diğer hata etki alanlarında yeniden başlatılır, ancak mümkünse, aynı Kullanılabilirlik kümesindeki diğer VM 'lerden ayrı hata etki alanlarında tutulur. Ana bilgisayar yeniden çevrimiçi olduğunda, yüksek kullanılabilirlik sağlamak için VM 'Ler yeniden dengelenebilir.  

Sanal Makine Ölçek Kümeleri arka uçtaki kullanılabilirlik kümelerini kullanır ve her sanal makine ölçek kümesi örneğinin farklı bir hata etki alanına yerleştirildiğinden emin olun. Bu, ayrı Azure Stack altyapı düğümlerini kullandıkları anlamına gelir. Örneğin, dört düğümlü Azure Stack sisteminde, üç farklı Azure Stack düğümüne üç sanal makine ölçek kümesi örneği yerleştirmek için 4 düğümlü kapasitenin olmamasından dolayı, bir sanal makine ölçek kümesi 'nin oluşturma sırasında başarısız olacağı bir durum olabilir . Ayrıca, Azure Stack düğümler yerleştirme denemeden önce farklı düzeylerde doldurulabilir. 

Azure Stack belleği aşırı teslim etmez. Ancak, fiziksel çekirdek sayısının bir üzerinde yürütmeye izin verilir. 

Yerleştirme algoritmaları var olan sanal sunucudan fiziksel çekirdek yük sağlama oranına bir faktör olarak Bakmadıklarından, her konağın farklı bir oranı olabilir. Microsoft olarak, iş yüklerinin ve hizmet düzeyi gereksinimlerindeki değişim nedeniyle fizikselden sanal çekirdek oranına ilişkin yönergeler sağlamayız. 

## <a name="consideration-for-total-number-of-vms"></a>Toplam sanal makine sayısı için göz önünde bulundurun 

Azure Stack kapasitesinin doğru planlanabilmesi için yeni bir göz vardır. 1901 Güncelleştirmesi (ve her güncelleştirme devam eden) ile, oluşturulabilecek sanal makinelerin toplam sayısı için bir sınır vardır. Bu sınırın çözüm kararsızlığına engel olmak için geçici olması amaçlanmıştır. Daha yüksek sayıda VM 'de kararlılık sorununun kaynağı giderilmekte, ancak düzeltme için belirli bir zaman çizelgesi henüz belirlenmemiştir. Toplam 700 olan 60 VM 'e yönelik bir sunucu başına VM sınırı vardır. Örneğin, bir 8 sunucu Azure Stack VM sınırı 480 (8 * 60) olur. 12 ile 16 arasında sunucu Azure Stack çözümü için sınır 700 olur. Bu sınır, dayanıklılık yedeği ve CPU sanal değeri gibi bir işlecin damgada korumak istediğiniz tüm işlem kapasitesi konularını göz önünde bulundurarak oluşturulmuştur. Daha fazla bilgi için bkz. kapasite planlayıcısı 'nın yeni sürümü. 

VM Ölçek sınırına ulaşılmış olması durumunda şu hata kodları sonuç olarak döndürülür: VMsPerScaleUnitLimitExceeded, VMsPerScaleUnitNodeLimitExceeded.

## <a name="considerations-for-deallocation"></a>Ayırmayı kaldırma konuları

Bir VM _serbest bırakıldı_ durumundaysa, bellek kaynakları kullanılmaz. Bu, diğer VM 'Lerin sisteme yerleştirilmesine izin verir. 

Serbest bırakılmış VM daha sonra yeniden başlatılırsa, bellek kullanımı veya ayırma sisteme yerleştirilmiş yeni bir sanal makine gibi değerlendirilir ve kullanılabilir bellek kullanılır. 

Kullanılabilir bellek yoksa, sanal makine başlatılmaz.

## <a name="azure-stack-memory"></a>Azure Stack belleği 

Azure Stack, başarıyla sağlanan VM 'Leri çalıştırmaya devam etmek için tasarlanmıştır. Örneğin, bir donanım arızası nedeniyle bir konak çevrimdışıysa, Azure Stack bu VM 'yi başka bir konakta yeniden başlatmaya çalışacaktır. İkinci örnek, Azure Stack yazılımının düzeltme eki ve güncelleştirmesidir. Fiziksel bir konağın yeniden başlatılması gerekiyorsa, söz konusu ana bilgisayarda yürütülen VM 'Leri çözümdeki başka bir kullanılabilir konağa taşımak için bir girişimde bulunuldu.   

Bu VM yönetimi veya taşıması yalnızca yeniden başlatma veya geçişin oluşmasına izin vermek için ayrılmış bellek kapasitesi varsa elde edilebilir. Toplam konak belleğinin bir bölümü, Kiracı VM yerleştirmesi için ayrılmıştır ve kullanılamaz. 

Yönetim portalında, Azure Stack boş ve kullanılan belleği gösteren bir pasta grafiği gözden geçirebilirsiniz. Aşağıdaki diyagramda, Azure Stack bir Azure Stack ölçek birimindeki fiziksel bellek kapasitesi gösterilmektedir:

![Fiziksel bellek kapasitesi](media/azure-stack-capacity-planning/physical-memory-capacity.png)

Kullanılan bellek birkaç bileşenden oluşur. Aşağıdaki bileşenler, pasta grafiğinin kullanım bölümünde bulunan belleği kullanır:  

 -  Konak IŞLETIM sistemi kullanımı veya Reserve – bu, konaktaki işletim sistemi (OS), sanal bellek sayfa tabloları, ana bilgisayar IŞLETIM sisteminde çalışan süreçler ve doğrudan bellek önbelleği alanları tarafından kullanılan bellektir. Bu değer, konakta çalışan farklı Hyper-V işlemlerinin kullandığı belleğe bağlı olduğundan, bu değer dalgalanmasına sahip olabilir.
 - Altyapı Hizmetleri: Bunlar Azure Stack oluşturan altyapı VM 'lardır. Azure Stack sürümünün 1904 sürümünden itibaren, bu, belleğin 242 GB + (4 GB x # düğüm) alan ~ 31 VM 'yi gerektirir. Altyapı Hizmetleri bileşeninin bellek kullanımı, altyapı hizmetlerimizi daha ölçeklenebilir ve dayanıklı hale getirme konusunda çalışırken değişebilir.
 - Dayanıklılık yedeği – Azure Stack, tek bir ana bilgisayar hatası sırasında kiracı kullanılabilirliğine ve VM 'lerin başarılı bir şekilde dinamik geçişine izin vermek için düzeltme eki ve güncelleştirme sırasında kiracı kullanılabilirliğine izin vermek için belleğin bir kısmını ayırır.
 - Kiracı VM 'Leri: Bunlar Azure Stack kullanıcılar tarafından oluşturulan Kiracı VM 'lardır. VM çalıştırmanın yanı sıra, bellek, dokuda bulunan tüm VM 'Ler tarafından tüketilecektir. Yani, "oluşturma" veya "başarısız" durumundaki VM 'Lerin veya Konuk içinden sanal makinelerin kapanması, belleğin tüketilmesine neden olur. Ancak, Portal/PowerShell/CLI 'den serbest bırakıldurdur seçeneği kullanılarak serbest bırakılmış olan VM 'Ler Azure Stack belleği tüketmez.
 - Add-on RPs – SQL, MySQL, App Service vb. gibi eklentiler için dağıtılan VM 'Ler.


Portalda bellek tüketimini anlamanın en iyi yolu, çeşitli iş yüklerinin etkisini görmek için [Azure Stack Capacity Planner](https://aka.ms/azstackcapacityplanner) kullanmaktır. Aşağıdaki hesaplama, planlayıcının kullandığı aynı bir hesap.

Bu hesaplama, Kiracı VM yerleşimi için kullanılabilen toplam kullanılabilir bellek ile sonuçlanır. Bu bellek kapasitesi Azure Stack ölçek biriminden tamamen yöneliktir. 


  VM yerleştirme için kullanılabilir bellek = toplam konak belleği-dayanıklılık yedeği-Kiracı VM 'Leri çalıştırılarak kullanılan bellek-Azure Stack altyapı ek yükü <sup>1</sup>

  Dayanıklılık yedeği = H + R * ((N-1) * H) + V * (N-2)

> Nerede:
> - H = tek sunucu belleğinin boyutu
> - N = ölçek biriminin boyutu (sunucu sayısı)
> - R = .15, bu formülde<sup>2</sup> olan işletim sistemi ek yükü için ayrılmış işletim sistemi
> - V = ölçek birimindeki en büyük VM

  <sup>1</sup> Azure Stack altyapı ek yükü = 242 GB + (4 GB x düğüm). Azure Stack altyapısını barındırmak için yaklaşık 31 VM kullanılır ve toplam olarak bellek ve 146 sanal çekirdekleri hakkında yaklaşık 242 GB + (4 GB x düğüm) kullanın. Bu sanal makine sayısı için, güvenlik, ölçeklenebilirlik, bakım ve düzeltme eki uygulama gereksinimlerini karşılamak üzere gerekli hizmet ayrımını karşılamanız gerekir. Bu iç hizmet yapısı, yeni altyapı hizmetleri geliştirildiği için gelecekte giriş yapılmasına izin verir. 

  <sup>2</sup> işletim sistemi için ayrılan iş yükü = %15 (. 15) düğüm belleği. İşletim sistemi ayırma değeri bir tahmindir ve sunucunun fiziksel bellek kapasitesine ve genel işletim sistemi yüküne bağlı olarak değişir.


Ölçek biriminde bulunan V, en büyük VM değeri, en büyük Kiracı VM bellek boyutuna göre dinamik olarak belirlenir. Örneğin, en büyük VM değeri, Azure Stack çözümünde 7 GB veya 112 GB veya desteklenen başka bir VM belleği boyutu olabilir. Azure Stack dokusunda en büyük VM 'nin değiştirilmesi, VM 'nin belleğindeki artışa ek olarak dayanıklılık Reserve artışına neden olur. 

## <a name="frequently-asked-questions"></a>Sıkça Sorulan Sorular

**S**: kiracım yenı bir VM dağıttı, yönetim portalındaki yetenek grafiğinin kalan kapasiteyi göstermesi ne kadar sürer?

Y **: kapasite**dikey penceresi 15 dakikada bir yenilenir, bu nedenle lütfen göz önüne alın.

**S**: Azure Stack dağıtılan sanal makinelerin sayısı değişmedi, ancak kapasiteniz dalgalanma alınıyor. Neden?

Y **: VM**yerleştirmesi için kullanılabilir bellek birden çok bağımlılığa sahiptir, bunlardan biri konak işletim sistemi ayrılmış. Bu değer, ana bilgisayarda çalışan, sabit bir değer olmayan farklı Hyper-V işlemlerinin kullandığı belleğe bağımlıdır.

**S**: Kiracı VM 'lerinin belleği tüketmek için hangi durumu kullanması gerekir?

Y **: VM**çalıştırmaya ek olarak, bellek, dokuda bulunan tüm VM 'ler tarafından tüketilecektir. Bu, "oluşturma", "başarısız" veya VM 'lerde bulunan VM 'Lerin, Portal/PowerShell/CLI tarafından serbest bırakılmasının aksine belleği tüketmesi için konuk içinden kapanması anlamına gelir.

**S**: dört ana bilgisayar Azure Stack var. Kiracımda 56 GB RAM kullanan 3 VM vardır (D5_v2). VM 'lerden biri 112 GB RAM (D14_v2) olarak yeniden boyutlandırılır ve panoda kullanılabilir bellek raporlaması, kapasite dikey penceresinde 168 GB 'lik bir kullanımın çakışmasına neden oldu. Diğer iki D5_v2 sanal makine D14_v2 yeniden boyutlandırılırken, her biri yalnızca 56GB RAM ile sonuçlandı. Bunun nedeni nedir?

Y: kullanılabilir bellek, Azure Stack tarafından tutulan dayanıklılık ayırmaya **yönelik bir işlevdir**. Dayanıklılık yedeği, Azure Stack damgasında en büyük VM boyutunun bir işlevidir. İlk olarak, Damgadaki en büyük VM 56 GB bellektir. VM yeniden boyutlandırılırken, Damgadaki en büyük VM, yalnızca o Kiracı VM tarafından kullanılan belleği artırmayan ancak dayanıklılık Reserve 'i arttığı 112 GB bellek haline geldi. Bu, 56 GB artışı (56 GB ila 112 GB Kiracı VM belleği artışı) ve 112 GB dayanıklılık yedek bellek artışı ile sonuçlandı. Sonraki VM 'Ler yeniden boyutlandırılırken, en büyük VM boyutu 112 GB sanal makine olarak kaldığı için bir sonuç dayanıklılık artışı artmaz. Bellek tüketiminde artış yalnızca Kiracı VM belleği artışına (56 GB) sahiptir. 


> [!NOTE]
> Ağ için kapasite planlama gereksinimleri en az, yalnızca genel VIP 'nin boyutu yapılandırılabilir. Azure Stack daha fazla Genel IP Adresleri ekleme hakkında daha fazla bilgi için bkz. [add genel IP adresleri](azure-stack-add-ips.md).

## <a name="next-steps"></a>Sonraki adımlar
[Azure Stack depolama](azure-stack-capacity-planning-storage.md) hakkında bilgi edinin
