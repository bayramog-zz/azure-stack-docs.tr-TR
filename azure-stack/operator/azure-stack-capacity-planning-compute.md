---
title: Azure Stack kapasite işlem planlama | Microsoft Docs
description: Kapasite planlaması için Azure Stack dağıtımları hakkında bilgi edinin.
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
ms.date: 06/13/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 06/13/2019
ms.openlocfilehash: 7c46d2b576f8927ff0da438091a6c1094ae15ddf
ms.sourcegitcommit: 51ec68b5e6dbf437aaca19a9f35ba07d2c402892
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67851790"
---
# <a name="azure-stack-compute"></a>Azure Stack işlem

[VM boyutları](https://docs.microsoft.com/azure-stack/user/azure-stack-vm-sizes) Azure Stack üzerinde desteklenen Azure üzerinde desteklenen bir alt kümesidir. Azure kaynak sınırları boyunca operasyonda ekstra tüketimi kaynakların (yerel ve hizmet düzeyi sunucusu) önlemek için birçok vektörleri uygular. Diğer kiracıların kaynakları overconsume, Kiracı kullanımı için bazı limitler izlenmesi olmadan Kiracı deneyimleri düşer. Sanal makineden ağ çıkışı için Azure sınırlamaları eşleşen bant genişliği sınırlaması Azure Stack'te yerinde vardır. Azure Stack'te depolama kaynakları için depolama IOPS limitleri temel kiracılar depolama erişimi için kaynakların tüketimini üzerinden kaçının.

>[!IMPORTANT]
>[Azure Stack Capacity Planner](https://aka.ms/azstackcapacityplanner) IOPS performansı garanti etmez düşünün veya.

## <a name="vm-placement"></a>VM yerleştirme

Azure Stack yerleşim altyapısı Kiracı VM'ler kullanılabilir konaklar arasında yerleştirir.

Azure Stack, Vm'leri yerleştirirken iki önemli kullanır. Bir sanal makine türü için ana bilgisayarda yeterli bellek olduğundan. İki olan Vm'leri bir parçası bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) veya [sanal makine ölçek kümeleri](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview).

Azure Stack'te bir çoklu VM üretim sisteminin yüksek kullanılabilirlik elde etmek için bunları birden çok hata etki alanlarına yayılır. bir kullanılabilirlik kümesindeki Vm'leri yerleştirilir. Hata etki alanı bir kullanılabilirlik kümesinde tek bir düğüm ölçek birimi olarak tanımlanır. Azure Stack, Azure ile tutarlı olacak şekilde en fazla üç hata etki alanı ile bir kullanılabilirlik sahip destekler. Vm'leri bir kullanılabilirlik kümesine yerleştirilir bunları mümkün olduğunca eşit olarak birden çok hata etki alanları üzerinde diğer bir deyişle, Azure Stack ana yayarak birbirinden fiziksel olarak izole edilmiş olur. Bir donanım hatası varsa, başarısız hata etki alanı Vm'lerden diğer hata etki alanları yeniden, ancak, mümkün olduğunda, aynı kullanılabilirlik kümesindeki diğer vm'lerden ayrı hata etki alanlarında tutulur. Konak tekrar çevrimiçi olduğunda, yüksek kullanılabilirliği sürdürmek için Vm'leri yeniden Dengelenecek.  

Sanal makine ölçek kümeleri arkasındaki kullanılabilirlik kümelerini kullanın sonlandırmak ve her sanal makine ölçek kümesi örneği olduğundan emin olun, farklı hata etki alanında yerleştirilir. Bu, ayrı Azure Stack altyapısını düğüm kullandıkları anlamına gelir. Örneğin, 4 düğümünde Azure Stack sistemi, olabilir bir durum olduğu bir sanal makine ölçek kümesi 3 örnek oluştururken 3 sanal makine ölçek kümesi örneklerine 3 ayrı Azure Stack düğümlerinde yerleştirmek için 4 düğümlü kapasite eksikliği nedeniyle başarısız olur. Ayrıca, Azure Stack düğümleri yerleştirme denemeden önce değişen düzeylerde doldurulabilir. 

Azure Stack bellek aşırı işleme değil. Ancak, bir üst işlemesi fiziksel çekirdek sayısı için izin verilir. Sanal bir faktör olarak fiziksel çekirdek fazladan sağlama oranı ile var olan konumunda yerleştirme algoritmaları görünmüyor olduğundan, her konak farklı bir oran olabilir. Microsoft rehberlik üzerinde fiziksel-sanal çekirdek oranı nedeniyle iş yükleri ve hizmet düzeyi gereksinimlerini varyasyonu sunmuyoruz. 

## <a name="consideration-for-total-number-of-vms"></a>Toplam VM sayısı için önemli noktalar 

Doğru bir şekilde Azure Stack kapasite planlaması için yeni bir durum yoktur. 1901 güncelleştirme (ve bundan sonra her bir güncelleştirme) ile da artık sanal makinelerin oluşturulabilir toplam sayısına bir sınır yoktur. Bu sınır, çözüm kararsızlığı engellemek için geçici olması amaçlanmıştır. Kaynak konumunda VM'ler, daha yüksek sayıda kararlılık sorunun ele ancak belirli bir zaman çizelgesi düzeltme için değil henüz karar verilmemiştir. Artık bir başına 60 VM'lerin sunucu sınırı 700 toplam çözüm sınırına sahip. Örneğin, bir 8 sunucu Azure Stack VM sınırı 480 (8 * 60) olacaktır. 12-16 sunucu Azure Stack çözüm, sınır 700 olacaktır. Bu sınır, tüm işlem dayanıklılık ayrılmış ve bir işleç damgada sağlamak istediğiniz fiziksel/oranı sanal CPU gibi aklınızda kapasiteyle alakalı durumlar tutma oluşturuldu. Daha fazla bilgi için kapasite Planlayıcı'nın yeni sürümüne bakın. 

Aşağıdaki hata kodları, sonuç olarak, VM ölçek sınırına ulaşıldı, olay, döndürülürdü: VMsPerScaleUnitLimitExceeded, VMsPerScaleUnitNodeLimitExceeded.


## <a name="azure-stack-memory"></a>Azure Stack bellek 

Azure Stack, başarıyla sağlanmış çalışan Vm'leri korumak için tasarlanmıştır. Bir konağa bir donanım hatası nedeniyle çevrimdışı olduğunda, örneğin, Azure Stack başka bir ana bilgisayarda bu VM'ye yeniden başlatmaya çalışacak. İkinci örnek, düzeltme eki ve Azure Stack yazılım güncelleştirmesi ' dir. Bir fiziksel konağa yeniden başlatılmasına gerek yoksa, bir çözümdeki kullanılabilir başka bir ana bilgisayara ilgili konak üzerinde yürütülen sanal makineleri taşımak için girişimde.   

Bu sanal makine Yönetimi veya taşıma için yeniden başlatma veya geçiş gerçekleşmesine izin vermek için ayrılmış bellek kapasitesi ise yalnızca gerçekleştirilebilir. Toplam ana belleğin bir kısmını, ayrılmış ve Kiracı VM yerleştirme için kullanılamaz. 

Bir pasta grafiğinin yönetim portalındaki Azure Stack'te boş ve kullanılan bellek gösteren gözden geçirebilirsiniz. Aşağıdaki diyagramda bir Azure Stack ölçek birimi olarak Azure Stack fiziksel bellek kapasitesi gösterir:

![Fiziksel bellek kapasitesi](media/azure-stack-capacity-planning/physical-memory-capacity.png)

Kullanılan bellek, çeşitli bileşenlerden oluşur. Aşağıdaki bileşenler pasta grafiğinin kullanım bölümündeki bellek kullan:  

 -  Konak işletim sistemi kullanımı veya yedek – işletim sistemi (OS) tarafından kullanılan bellek budur konak, sanal bellek disk belleği tabloları, konak işletim sistemi ve alanları doğrudan önbellek çalışan işlemler. Bu değer ana bilgisayarda çalışan farklı Hyper-V işlemler tarafından kullanılan bellek bağımlı olduğundan, değişim gösterebilir.
 - Altyapı Hizmetleri – Azure yığınını oluşturan sanal makineleri altyapı şunlardır. Azure Stack 1904 yayın sürümü itibarıyla, bu 242 GB alan ~ 31 Vm'leri kapsar + (4 GB x düğüm sayısı) bellek. Altyapı hizmetleri bileşeni'nın bellek kullanımı altyapı hizmetlerimizi daha ölçeklenebilir ve dayanıklı hale getirme üzerinde çalışırken farklı olabilir.
 - Dayanıklılık ayırma – Azure Stack Kiracı kullanılabilirlik için düzeltme eki ve güncelleştirme sırasında bir tek ana bilgisayar hatası betiklerinizi VM'lerin başarılı dinamik geçiş için izin vermek için izin vermek için belleğin bir kısmını ayırır.
 - Kiracı sanal makineler bu Azure Stack kullanıcıları tarafından oluşturulan sanal makineler Kiracı ücretlerdir. Sanal makineleri çalıştırmanın yanı sıra, bellek, dokuda Geldiniz herhangi bir VM tarafından kullanılır. Başka bir deyişle, "Oluşturma" veya "Başarısız" durumundaki sanal makineleri veya konuğa alanından kapatma Vm'leri bellek tüketir. Ancak, portal/powershell/CLI seçeneğinden serbest Durdur kullanarak serbest Vm'leri Azure yığından bellek tüketir değil.
 - Eklenti RPs – SQL, MySQL, App Service vb. gibi eklenti RPs için dağıtılan VM'ler.


Portal bellek tüketimini anlamak için en iyi yolu kullanmaktır [Azure Stack Capacity Planner](https://aka.ms/azstackcapacityplanner) çeşitli iş yükleri etkisini görmek için. Aşağıdaki hesaplaması planner tarafından kullanılan hizmet örneğiyle aynı olur.

Bu hesaplama, Kiracı sanal makine yerleştirme için kullanılabilir toplam ve kullanılabilir bellek sonuçlanır. Azure Stack ölçek biriminin tamamı için bu bellek kapasitesidir. 


  VM yerleştirme için kullanılabilir bellek Kiracı VM'ler - Azure Stack altyapısını yükü çalıştırarak kullanılan toplam ana bilgisayar bellek - dayanıklılık ayrılan - bellek = <sup>1</sup>

  Dayanıklılık ayırma H + R = * ((N-1) * Y) + V * (N-2)

> Konumlar:
> - H = tek sunucu bellek boyutu
> - N = ölçek birimi, boyutu (Sunucuları sayısı)
> - R Bu formüldeki.15 olan işletim sistemi için ayrılmış işletim sistemi ek yükü, =<sup>2</sup>
> - V ölçek birimindeki en büyük VM =

  <sup>1</sup> azure Stack altyapısını yükü = 242 GB + (4 GB x düğüm sayısı). Azure yığını'nın altyapı barındırmak ve yaklaşık 242 GB toplam kullanmak için kullanılan yaklaşık olarak 31 Vm'leri + (4 GB düğümleri sayısı x) 146 sanal çekirdek ve bellek. Bu VM sayısı için gerekçe, güvenlik, ölçeklenebilirlik, Bakım ve düzeltme eki gereksinimlerini karşılamak için gerekli hizmet ayrımı karşılamak sağlamaktır. Bunlar geliştirilen gibi bu iç hizmet yapısı yeni altyapı hizmetleri için gelecekteki giriş sağlar. 

  <sup>2</sup> ek yükü için işletim sistemi ayrılmış %15 = (. 15) düğüm bellek. İşletim sistemi ayrılmış değeri, tahmini bir değerdir ve genel işletim sistemi ek yükü ve sunucu üzerinde fiziksel bellek kapasitesi göre değişiklik gösterir.


Değer V, Ölçek birimindeki en büyük VM dinamik olarak en büyük Kiracı VM bellek boyutunu temel alır. Örneğin, en büyük VM değeri, 7 GB veya 112 GB veya tüm diğer desteklenen sanal makine bellek boyutu Azure Stack çözümde olabilir. Azure Stack yapısı'nda büyük VM değiştirme VM'nin bellek artış yanı sıra dayanıklılık ayrılmış bir artış neden olur. 

## <a name="frequently-asked-questions"></a>Sıkça Sorulan Sorular

**Q**: Kiracıma yeni bir sanal makine dağıtılırsa, ne kadar yönetim portalında özelliği grafik için kalan kapasite gösterilecek sürer?

**A**: Kapasite dikey 15 dakikada bir, bu nedenle yeniler, lütfen dikkate alın.

**Q**: My Azure Stack üzerinde dağıtılan sanal makinelerin sayısını değişmemiştir, ancak benim kapasite geciktirmeye. Neden?

**A**: VM yerleştirme için kullanılabilir bellek, konak işletim sistemi ayırma biri olan, birden çok bağlantılıdır. Bu değer bir sabit değer olan ana bilgisayar üzerinde çalışan farklı Hyper-V işlemler tarafından kullanılan bellek bağlıdır.

**Q**: Durum Kiracı VM bellek tüketmesine olması gerekiyor mu?

v: Sanal makineleri çalıştırmanın yanı sıra, bellek, dokuda Geldiniz herhangi bir VM tarafından kullanılır. Bu, "Oluşturma", "Başarısız" veya Vm'leri gelen g içinde kapatıldığından Vm'leri anlamına gelir

**Q**: Azure Stack 4 ana bilgisayar var. Kiracıma 56 GB RAM (D5_v2) her tüketen 3 VM var. Vm'lerden birinin 112 GB RAM (D14_v2) boyutlandırılır ve 168 GB kullanımı kapasite dikey penceresinde bir depo içinde Panoda raporlama kullanılabilir bellek ile sonuçlandı. Diğer iki D5_v2 Vm'lere D14_v2, sonraki yeniden boyutlandırma, yalnızca 56 GB RAM artış sonuçlandı. Neden bu, bu nedenle?

**A**: Kullanılabilir bellek, Azure Stack tarafından tutulan dayanıklılık rezerve bir işlevdir. Dayanıklılık ayırma, Azure Stack damgası üzerinde en büyük VM boyutunun bir işlevdir. İlk başta en büyük VM damga üzerinde 56 GB bellek yoktu. VM yeniden boyutlandırılmış zaman damgası üzerinde en büyük VM yalnızca VM'nin Kiracı tarafından kullanılan bellek artar ancak dayanıklılık ayırma de artırdık 112 GB bellek hale geldi. Bu artış 56 GB (112 GB Kiracı VM bellek artışı 56 GB) + 112 GB dayanıklılık ayrılan bellek artışı içindeki sonuçlandı. Sonraki Vm'leri yeniden boyutlandırdığınızda büyük VM boyutu VM 112 GB kalır ve bu nedenle hiçbir sonuç dayanıklılık ayırma artış vardı. Yalnızca Kiracı VM bellek artışı (56 GB) bellek tüketimi artış oluştu. 


> [!NOTE]
> Yalnızca genel VIP boyutu yapılandırılabilir olduğu gibi ağ iletişimi için kapasite planlama gereksinimleri düşüktür. Azure Stack için daha fazla genel IP adresleri ekleme hakkında daha fazla bilgi için bkz: [genel IP adresleri ekleme](azure-stack-add-ips.md).

## <a name="next-steps"></a>Sonraki adımlar
Hakkında bilgi edinin [Azure Stack depolama](azure-stack-capacity-planning-storage.md)
