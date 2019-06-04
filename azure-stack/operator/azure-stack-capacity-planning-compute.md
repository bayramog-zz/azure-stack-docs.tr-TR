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
ms.date: 05/31/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 05/31/2019
ms.openlocfilehash: e549413798ffc3c06c95bfbcf50ab4929ffeaf63
ms.sourcegitcommit: 80775f5c5235147ae730dfc7e896675a9a79cdbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2019
ms.locfileid: "66461030"
---
# <a name="azure-stack-compute"></a>Azure Stack işlem

[VM boyutları](https://docs.microsoft.com/azure-stack/user/azure-stack-vm-sizes) Azure Stack üzerinde desteklenen Azure üzerinde desteklenen bir alt kümesidir. Azure kaynak sınırları boyunca operasyonda ekstra tüketimi kaynakların (yerel ve hizmet düzeyi sunucusu) önlemek için birçok vektörleri uygular. Diğer kiracıların kaynakları overconsume, Kiracı kullanımı için bazı limitler izlenmesi olmadan Kiracı deneyimleri düşer. Sanal makineden ağ çıkışı için Azure sınırlamaları eşleşen bant genişliği sınırlaması Azure Stack'te yerinde vardır. Depolama kaynakları için depolama IOPS limitlerine depolama erişimi için kiracılar tarafından temel operasyonda ekstra tüketimi kaynak önlemek için Azure Stack üzerinde uygulanmıştır.

>[!IMPORTANT]
>[Azure Stack Capacity Planner](https://aka.ms/azstackcapacityplanner) IOPS performansı garanti etmez düşünün veya.

## <a name="vm-placement"></a>VM yerleştirme

Azure Stack'te Kiracı VM yerleştirme kullanılabilir konakları arasında yerleşim altyapısı tarafından otomatik olarak gerçekleştirilir. VM türü için ana bilgisayarda yeterli bellek olduğunu ve Vm'leri bir parçası olan Vm'leri yerleştirirken yalnızca iki önemli olan bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) veya [sanal makine ölçek kümeleri](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview).  

Azure Stack'te bir çoklu VM üretim sisteminin yüksek kullanılabilirlik elde etmek için bunları birden çok hata etki alanlarına yayılır. bir kullanılabilirlik kümesindeki Vm'leri yerleştirilir. Hata etki alanı bir kullanılabilirlik kümesinde tek bir düğüm ölçek birimi olarak tanımlanır. Azure Stack, Azure ile tutarlı olacak şekilde en fazla üç hata etki alanı ile bir kullanılabilirlik sahip destekler. Vm'leri bir kullanılabilirlik kümesine yerleştirilir bunları mümkün olduğunca eşit olarak birden çok hata etki alanları üzerinde diğer bir deyişle, Azure Stack ana yayarak birbirinden fiziksel olarak izole edilmiş olur. Bir donanım hatası varsa, başarısız hata etki alanı Vm'lerden diğer hata etki alanları yeniden, ancak, mümkün olduğunda, aynı kullanılabilirlik kümesindeki diğer vm'lerden ayrı hata etki alanlarında tutulur. Konak tekrar çevrimiçi olduğunda, yüksek kullanılabilirliği sürdürmek için Vm'leri yeniden Dengelenecek.  

Sanal makine ölçek kümeleri arkasındaki kullanılabilirlik kümelerini kullanın sonlandırmak ve her sanal makine ölçek kümesi örneği olduğundan emin olun, farklı hata etki alanında yerleştirilir. Bu, ayrı Azure Stack altyapısını düğüm kullandıkları anlamına gelir. Örneğin, 4 düğümünde Azure Stack sistemi, olabilir bir durum olduğu bir sanal makine ölçek kümesi 3 örnek oluştururken 3 sanal makine ölçek kümesi örneklerine 3 ayrı Azure Stack düğümlerinde yerleştirmek için 4 düğümlü kapasite eksikliği nedeniyle başarısız olur. Ayrıca, Azure Stack düğümleri yerleştirme denemeden önce değişen düzeylerde doldurulabilir. 

Azure Stack bellek aşırı işleme değil. Ancak, bir üst işlemesi fiziksel çekirdek sayısı için izin verilir. Sanal bir faktör olarak fiziksel çekirdek fazladan sağlama oranı ile var olan konumunda yerleştirme algoritmaları görünmüyor olduğundan, her konak farklı bir oran olabilir. Microsoft rehberlik üzerinde fiziksel-sanal çekirdek oranı nedeniyle iş yükleri ve hizmet düzeyi gereksinimlerini varyasyonu sunmuyoruz. 

## <a name="azure-stack-memory"></a>Azure Stack bellek 

Azure Stack, başarıyla sağlanmış çalışan Vm'leri korumak için tasarlanmıştır. Bir konağa bir donanım hatası nedeniyle çevrimdışı olduğunda, örneğin, Azure Stack başka bir ana bilgisayarda bu VM'ye yeniden başlatmaya çalışacak. İkinci örnek, düzeltme eki ve Azure Stack yazılım güncelleştirmesi ' dir. Bir fiziksel konağa yeniden başlatılmasına gerek yoksa, bir çözümdeki kullanılabilir başka bir ana bilgisayara ilgili konak üzerinde yürütülen sanal makineleri taşımak için girişimde.   

Bu sanal makine Yönetimi veya taşıma için yeniden başlatma veya geçiş gerçekleşmesine izin vermek için ayrılmış bellek kapasitesi ise yalnızca gerçekleştirilebilir. Toplam ana belleğin bir kısmını, ayrılmış ve Kiracı VM yerleştirme için kullanılamaz. 

Bir pasta grafiğinin yönetim portalındaki Azure Stack'te boş ve kullanılan bellek gösteren gözden geçirebilirsiniz. Aşağıdaki diyagramda bir Azure Stack ölçek birimi olarak Azure Stack fiziksel bellek kapasitesi gösterir:

![Fiziksel bellek kapasitesi](media/azure-stack-capacity-planning/physical-memory-capacity.png)

Kullanılan bellek, çeşitli bileşenlerden oluşur. Aşağıdaki bileşenler pasta grafiğinin kullanım bölümündeki bellek tüketir.  

- Konak işletim sistemi kullanımı veya yedek – işletim sistemi (OS) tarafından kullanılan bellek budur konak, sanal bellek disk belleği tabloları, konak işletim sistemi ve alanları doğrudan önbellek çalışan işlemler. 
- Altyapı Hizmetleri – Azure yığınını oluşturan sanal makineleri altyapı şunlardır. Azure Stack 1902 yayın sürümü itibarıyla, bu 242 GB alan 31 Vm'leri kapsar + (4 GB x düğüm sayısı). Bunlar geliştirilen gibi bu iç hizmet yapısı yeni altyapı hizmetleri için gelecekteki giriş sağlar.
- Dayanıklılık ayırma – Azure Stack Kiracı kullanılabilirlik için düzeltme eki ve güncelleştirme sırasında bir tek ana bilgisayar hatası betiklerinizi VM'lerin başarılı dinamik geçiş için izin vermek için izin vermek için belleğin bir kısmını ayırır. 
- Kiracı sanal makineler bu Azure Stack kullanıcıları tarafından oluşturulan sanal makineler Kiracı ücretlerdir. Sanal makineleri çalıştırmanın yanı sıra, bellek, dokuda Geldiniz herhangi bir VM tarafından kullanılır. VM'ler, yani **oluşturma** veya **başarısız** durumu veya konuğa alanından kapatma Vm'leri bellek kullanacaktır. Ancak, seçeneği serbest Durdur kullanılarak serbest bırakılmış olan Vm'leri Azure yığından bellek tüketir değil. 

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

> [!NOTE]
> Yalnızca genel VIP boyutu yapılandırılabilir olduğu gibi ağ iletişimi için kapasite planlama gereksinimleri düşüktür. Azure Stack için daha fazla genel IP adresleri ekleme hakkında daha fazla bilgi için bkz: [genel IP adresleri ekleme](azure-stack-add-ips.md).

## <a name="next-steps"></a>Sonraki adımlar
Hakkında bilgi edinin [Azure Stack depolama](azure-stack-capacity-planning-storage.md)