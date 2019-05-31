---
title: Kapasite planlaması için Azure Stack | Microsoft Docs
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
ms.date: 05/30/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 05/30/2019
ms.openlocfilehash: a67a5010da2a67fb002a351b04a12fe7671fb73b
ms.sourcegitcommit: 4e0b450c91c6515794b663a39f9a4b8b49999918
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66411679"
---
# <a name="azure-stack-capacity-planning"></a>Azure Stack kapasite planlaması
Azure Stack çözümünü değerlendirirken, genel ve Azure Stack bulut kapasite üzerinde doğrudan etkisi donanım yapılandırma seçeneğiniz vardır. Bunlar, CPU, bellek yoğunluğu, depolama yapılandırması ve genel çözüm ölçek veya sunucu sayısını Klasik seçimlerdir. Geleneksel bir sanallaştırma çözümü, kullanılabilir kapasitesini belirlemek için bu bileşenlerin basit aritmetik geçerli değildir. Azure Stack altyapısını veya yönetim bileşenleri çözüm içinde barındırmak için geliştirilmiştir ilk neden olmasıdır. Çözümün kapasite bazıları ayrılır, dayanıklılık, hizmet kesintisi durumunda, Kiracı iş yüklerini en aza indirmek için bir şekilde çözümün yazılım güncelleştirme desteklemek üzere ikinci nedenidir. 

> [!IMPORTANT]
> Bu kapasite planlama bilgileri ve [Azure Stack Capacity Planner](https://aka.ms/azstackcapacityplanner) Azure Stack planlama ve yapılandırma kararları için bir başlangıç noktasıdır. Araştırma ve analiz için bir alternatif olarak hizmet vermek için tasarlanmamıştır. Microsoft, hiçbir taahhütte veya garantide, açık veya zımni burada verilen bilgilere göre yapar.
 

Azure Stack çözümünü hiper yakınsanmış küme hesaplama ve depolama olarak oluşturulmuştur. Convergence donanım kapasitesi olarak adlandırılan küme içinde paylaşmak için sağlayan bir *ölçek birimi*. Azure Stack'te bir ölçek birimi, kullanılabilirlik ve ölçeklenebilirlik kaynakları sağlar. Bir ölçek birimi olarak adlandırılır, Azure Stack sunucular kümesi oluşur *konakları*. Altyapı yazılım, VM'ler bir dizi içinde barındırıldığı ve Kiracı Vm'leri olarak aynı fiziksel sunucuların paylaşır. Tüm Azure Stack Vm'leri, sonra ölçek biriminin Windows Server küme teknolojileri ve bağımsız Hyper-V örnekleri tarafından yönetilir. Ölçek birimi, alma ve Azure Stack Yönetimi basitleştirir. Ölçek birimi, Azure Stack arasında taşımayı ve ölçeklenebilirlik (Kiracı ve altyapı) tüm hizmetleri için de sağlar. 

## <a name="azure-stack-compute"></a>Azure Stack işlem

[VM boyutları](https://docs.microsoft.com/azure-stack/user/azure-stack-vm-sizes) Azure Stack üzerinde desteklenen Azure üzerinde desteklenen bir alt kümesidir. Azure kaynak sınırları boyunca operasyonda ekstra tüketimi kaynakların (yerel ve hizmet düzeyi sunucusu) önlemek için birçok vektörleri uygular. Diğer kiracıların kaynakları overconsume, Kiracı kullanımı için bazı limitler izlenmesi olmadan Kiracı deneyimleri düşer. Sanal makineden ağ çıkışı için Azure sınırlamaları eşleşen bant genişliği sınırlaması Azure Stack'te yerinde vardır. Depolama kaynakları için depolama IOPS limitlerine depolama erişimi için kiracılar tarafından temel operasyonda ekstra tüketimi kaynak önlemek için Azure Stack üzerinde uygulanmıştır.

>[!IMPORTANT]
>[Azure Stack Capacity Planner](https://aka.ms/azstackcapacityplanner) IOPS performansı garanti etmez düşünün veya.

### <a name="vm-placement"></a>VM yerleştirme

Azure Stack'te Kiracı VM yerleştirme kullanılabilir konakları arasında yerleşim altyapısı tarafından otomatik olarak gerçekleştirilir. VM türü için ana bilgisayarda yeterli bellek olduğunu ve Vm'leri bir parçası olan Vm'leri yerleştirirken yalnızca iki önemli olan bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) veya [sanal makine ölçek kümeleri](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview).  

Azure Stack'te bir çoklu VM üretim sisteminin yüksek kullanılabilirlik elde etmek için bunları birden çok hata etki alanlarına yayılır. bir kullanılabilirlik kümesindeki Vm'leri yerleştirilir. Hata etki alanı bir kullanılabilirlik kümesinde tek bir düğüm ölçek birimi olarak tanımlanır. Azure Stack, Azure ile tutarlı olacak şekilde en fazla üç hata etki alanı ile bir kullanılabilirlik sahip destekler. Vm'leri bir kullanılabilirlik kümesine yerleştirilir bunları mümkün olduğunca eşit olarak birden çok hata etki alanları üzerinde diğer bir deyişle, Azure Stack ana yayarak birbirinden fiziksel olarak izole edilmiş olur. Bir donanım hatası varsa, başarısız hata etki alanı Vm'lerden diğer hata etki alanları yeniden, ancak, mümkün olduğunda, aynı kullanılabilirlik kümesindeki diğer vm'lerden ayrı hata etki alanlarında tutulur. Konak tekrar çevrimiçi olduğunda, yüksek kullanılabilirliği sürdürmek için Vm'leri yeniden Dengelenecek.  

Sanal makine ölçek kümeleri arkasındaki kullanılabilirlik kümelerini kullanın sonlandırmak ve her sanal makine ölçek kümesi örneği olduğundan emin olun, farklı hata etki alanında yerleştirilir. Bu, ayrı Azure Stack altyapısını düğüm kullandıkları anlamına gelir. Örneğin, 4 düğümünde Azure Stack sistemi, olabilir bir durum olduğu bir sanal makine ölçek kümesi 3 örnek oluştururken 3 sanal makine ölçek kümesi örneklerine 3 ayrı Azure Stack düğümlerinde yerleştirmek için 4 düğümlü kapasite eksikliği nedeniyle başarısız olur. Ayrıca, Azure Stack düğümleri yerleştirme denemeden önce değişen düzeylerde doldurulabilir. 

Azure Stack bellek aşırı işleme değil. Ancak, bir üst işlemesi fiziksel çekirdek sayısı için izin verilir. Sanal bir faktör olarak fiziksel çekirdek fazladan sağlama oranı ile var olan konumunda yerleştirme algoritmaları görünmüyor olduğundan, her konak farklı bir oran olabilir. Microsoft rehberlik üzerinde fiziksel-sanal çekirdek oranı nedeniyle iş yükleri ve hizmet düzeyi gereksinimlerini varyasyonu sunmuyoruz. 

### <a name="azure-stack-memory"></a>Azure Stack bellek 

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

## <a name="azure-stack-storage"></a>Azure Stack depolama 
Aşağıdaki bölümler solutions depolama gereksinimlerini planlama stratejilerinde destek olmak için Azure Stack depolama kapasitesini planlama bilgileri sağlar.

### <a name="uses-and-organization-of-storage-capacity"></a>Kullanır ve kuruluş depolama kapasitesi
Azure Stack'in hiper yakınsanmış yapılandırması fiziksel depolama cihazlarının paylaşımına olanak sağlar. Kullanılabilir depolama alanı, üç ana bölümleri, altyapı, Kiracı sanal makinelerinin geçici depolama blobları, tablolar ve Kuyruklar Azure tutarlı depolama (ACS) Hizmetleri, yedekleme depolama arasında ve ' dir.

### <a name="spaces-direct-cache-and-capacity-tiers"></a>Önbellek ve kapasite katmanları alanları doğrudan
Depolama kapasitesi gereksinimlerini işletim sistemi, yerel günlüğe kaydetme, dökümleri ve diğer altyapı geçici depolama için kullanılan yoktur. Depolama alanları doğrudan Yapılandırması Yönetim altına duruma depolama aygıtlarını (cihazlar ve kapasite) ayırmak bu yerel depolama kapasitesidir. Depolama aygıtlarını kalanını tek bir ölçek birimindeki sunucularının sayısından bağımsız olarak depolama kapasitesi havuzu yerleştirilir. Bu cihazları iki türleri şunlardır: Önbellek ve kapasite.  Önbellek, yalnızca o - önbellek cihazlardır. Alanları doğrudan geri yazma için bu cihazları kullanma ve önbelleğe alma okuyun. Bu önbellek cihazları kapasiteleri kullanılabilir ancak, biçimlendirilmiş, "visible" biçimlendirilmiş sanal-disklerin kapasitesi iletilmez. Kapasite cihazları bu amaçla kullanılır ve "Giriş" depolama alanları tarafından yönetilen veri sağlamalısınız.

Tüm depolama kapasitesi ayrılan ve doğrudan Azure Stack altyapısı tarafından yönetilir. İşleci hakkında yapılandırma, yükleme seçimleri yapmanıza veya kapasite genişletmesi geldiğinde seçtiklerinizle uğraşmanız gerekmez. Bu tasarım kararlarını çözüm gereksinimleri ile hizalamak için yapılan ve ilk ya da yükleme/dağıtım sırasında veya kapasite genişletmesi sırasında otomatik. Dayanıklılık, yeniden oluşturulması için ayrılmış kapasite ve diğer ayrıntıları ayrıntılarını tasarımının bir parçası düşünür. 

İşleçleri bir tüm flash veya karma depolama yapılandırma arasında seçim yapabilirsiniz:

![Azure depolama kapasitesi planlama](media/azure-stack-capacity-planning/storage.png)

Tümü flash yapılandırmasında yapılandırma ya da iki katmanlı veya tek katmanlı bir yapılandırma olabilir.  Tek katmanlı yapılandırma ise tüm kapasite cihazları (örneğin NVMe veya SATA SSD veya SAS SSD) aynı tür olacaktır ve önbellek cihazları kullanılmaz. İki katmanlı tüm içinde önbellek cihazları ve ardından her iki SATA NVMe flash yapılandırma, tipik configuration olduğu veya SAS SSD kapasite cihazları olarak.

Karma, iki katmanlı yapılandırma, bir seçimdir HDD arasında NVMe, SATA veya SAS SSD ve kapasite önbelleğidir. 

Kısa bir özeti depolama alanları doğrudan ve Azure Stack depolama yapılandırması aşağıdaki gibidir:
- Bir depolama alanları havuzundan (tüm depolama cihazları içinde tek bir havuz yapılandırılır) ölçek birimi başına
- En iyi performans ve dayanıklılık için üç kopyalama yansıtma olarak oluşturulmuş sanal diskler
- Sanal disklerin bir ReFS dosya sistemi olarak biçimlendirilir.
- Sanal disk kapasitesi hesaplanır ve veri kapasitesi havuzu ayrılmamış bir kapasite cihazın miktarını bırakmak için farklı bir şekilde ayrılmış. Bu sunucu başına tek bir kapasite sürücü eşdeğerdir.
- Her ReFS dosya sistemi BitLocker için bekleyen veri şifrelemesi etkin olacaktır. 

Sanal-otomatik olarak oluşturulan diskler ve kapasitelerini aşağıdaki gibidir:

|Ad|Kapasite hesaplama|Açıklama|
|-----|-----|-----|
|Yerel/önyükleme aygıtı|En az 340 GB<sup>1</sup>|İşletim sistemi görüntüleri ve "yerel" altyapı Vm'leri için ayrı ayrı sunucu depolama|
|Altyapı|3,5 TB|Tüm Azure Stack altyapısını kullanımı|
|VmTemp|Aşağıya bakın<sup>2</sup>|Kiracı sanal makinelerinizin bağlı olarak geçici bir diskle ve bu verileri bu sanal diskler depolanır|
|ACS|Aşağıya bakın <sup>3</sup>|Bloblar, tablolar ve Kuyruklar bakım için azure tutarlı depolama kapasitesi|

<sup>1</sup> Azure Stack çözüm iş ortağı gerekli en düşük depolama kapasitesi.

<sup>2</sup> Kiracı sanal makine geçici diskler için kullanılan sanal disk boyutu, sunucunun fiziksel bellek oranı hesaplanır. Tablolarda Azure Iaas VM boyutları için belirtildiği gibi geçici disk sanal makineye atanan fiziksel belleğin oranıdır. Azure stack'teki "geçici disk" depolama için yapılan ayırma çoğu kullanım örnekleri yakalamak için farklı bir yolla yapılır, ancak tüm geçici disk depolama gereksinimlerini karşılamak mümkün olmayabilir. Seçilen çözümün depolama kapasitesi için yalnızca temp disk kapasitesi çoğunu değil kullanırken geçici depolama kullanılabilir hale getirme arasında bir denge oranıdır. Bir geçici depolama diskini, Ölçek birimindeki sunucu başına oluşturulur. Geçici depolama kapasitesini %10 genel olarak kullanılabilir depolama kapasitesi depolama havuzundaki ölçek birimi ötesinde büyüyüp değil. Hesaplama aşağıdaki örnekte olduğu gibi bir şeydir:

```
  DesiredTempStoragePerServer = PhysicalMemory * 0.65 * 8
  TempStoragePerSolution = DesiredTempStoragePerServer * NumberOfServers
  PercentOfTotalCapacity = TempStoragePerSolution / TotalAvailableCapacity
  If (PercentOfTotalCapacity <= 0.1)
      TempVirtualDiskSize = DesiredTempStoragePerServer
  Else
      TempVirtualDiskSize = (TotalAvailableCapacity * 0.1) / NumberOfServers
```

<sup>3</sup> sanal-ACS tarafından kullanılmak üzere oluşturulan kalan kapasite basit bir bölme disklerdir. Belirtildiği gibi tüm diskler sanal üç yönlü yansıtma ve her sunucu için kapasite tutarında bir kapasite sürücünün ayrılmamış. Çeşitli sanal-yukarıda numaralandırılan diskleri ilk ayrılır ve kalan kapasite ACS-için sanal diskleri sonra kullanılır.

## <a name="azure-stack-capacity-planner"></a>Azure Stack kapasite Planlayıcısı
Azure Stack Capacity Planner bilgi işlem kaynaklarının nasıl farklı ayırmaları gösteren bir elektronik donanım tekliflerin arasında seçim uygundur. 

### <a name="worksheet-descriptions"></a>Çalışma sayfası açıklamaları
Aşağıdaki tabloda her çalışma sayfası için bir açıklama indirilebileceğini Azure Stack Capacity Planner sahip [ https://aka.ms/azstackcapacityplanner ](https://aka.ms/azstackcapacityplanner). 

|Sekme adı|Açıklama|
|-----|-----|
|Sürüm sorumluluk reddi|Hesaplayıcı, sürüm numarasını ve yayın tarihi amacı.|
|Yönergeler|Adım adım yönergeler modeli kapasiteye Vm'leri koleksiyonu için planlama.|
|DefinedSolutionSKUs|En fazla beş donanım tanımları içeren tablo. Girişleri verilebilir. Sistem yapılandırmaları çizgisinin eşleştirmek için ayrıntıları değiştirin.|
|DefineByVMFootprint|SKU uygun donanım yapılandırmaları farklı boyut ve VM'lerin miktarları ile karşılaştırarak bulun.|
|DefineByWorkloadFootprint|Azure Stack iş yükleri koleksiyonu oluşturarak, uygun donanım SKU bulun.|
|  |  |

### <a name="definedsolutionskus-instructions"></a>DefinedSolutionSKUs yönergeleri
Bu çalışma, en fazla beş donanım tanım örnekler içerir. Sistem yapılandırmaları çizgisinin eşleşecek şekilde değiştirin.

#### <a name="hardware-selections-provided-by-authorized-hardware-partners"></a>Yetkili donanım iş ortakları tarafından sağlanan donanım seçimleri
Azure Stack, tümleşik bir sistem çözüm iş ortakları tarafından yüklü yazılım ile gönderilir. Çözüm iş ortakları, Azure Stack kapasite planlama araçları yetkili kendi sürümleri sağlar. Son çözüm kapasite tartışmalar için bu araçları kullanın.

#### <a name="multiple-ways-to-model-computing-resources"></a>Model bilgi işlem kaynaklarını birden fazla yolu
Azure Stack planner içinde modelleme kaynak Azure Stack Vm'leri üzerinde çeşitli boyutlarda bağlıdır. Vm'leri aralık boyutu en büyük Standard_Fsv2 kadar küçük temel 0. Bilgi işlem kaynak ayırmalar iki farklı şekillerde modelleyebilir:

- Belirli donanım teklifi seçin ve hangi birleşimleri çeşitli kaynaklardan bilgi. 

- Belirli bir birleşimini VM ayırma oluşturun ve Azure kaynak kullanılabilir donanım SKU'ları bu VM yapılandırmasına destekleyebilen gösteren hesaplayıcısı'na izin verin.

Bu araç VM kaynaklarını ayırma için iki yöntem sunar: olarak veya tek bir VM'nin kaynak ayırmalar koleksiyonunu en fazla altı farklı iş yükü yapılandırmaları topluluğudur. Her iş yükü yapılandırması farklı bir ayırma kullanılabilir VM kaynakları içerebilir. Sonraki bölümlerde oluşturmak ve bu ayırma modellerinin her biri kullanmak için adım adım yönergeler vardır. Arka planda olmayan yer alan değerler yalnızca gölgeli hücreler veya bu çalışma sayfasındaki SKU aşağı açılır liste içinde değiştirilmesi gerekir. Gölgeli hücreler içinde yapılan değişiklikler kaynak hesaplamalar kesilebilir.


### <a name="definebyvmfootprint-instructions"></a>DefineByVMFootprint yönergeleri
Çeşitli boyutlarda ve Vm'leri miktarlarını tek bir koleksiyon kullanarak bir model oluşturmak için "DefineByVMFootprint" sekmesini seçin ve aşağıdaki adımları izleyin:

1. Bu çalışma sayfasının sağ alt köşesinde, sağlanan aşağı açılır liste kutusu denetimleri bir ilk sayı seçmek için kullanın. (arasında 4 ile 16) sunucularının her donanım sisteminin (SKU) yüklenmesini istediğiniz. Bu sunucu sayısını, bu genel nasıl etkilediğini görmek için model oluşturma işlemi sırasında herhangi bir zamanda değiştirilebilir, kaynak dağıtımı modeli için kullanılabilir kaynaklar.
2. Belirli bir donanıma karşı çeşitli VM kaynak ayırmalar model istiyorsanız, sayfanın sağ üst köşesindeki mavi aşağı açılır liste kutusunun hemen altındaki "Geçerli SKU" etiketi bulun. Bu liste kutusu çekme ve istenen donanımınız SKU'ı seçin.
3. Artık çeşitli boyutta VM'ler modelinize eklemeye başlamak hazırsınız. Belirli bir VM türüne eklemek için VM giriş solunda mavi anahatları belirlenmiş kutuya bir miktar değeri girin.

   > [!NOTE]
   > Toplam VM depolama VM veri diskinin toplam kapasiteyi gösterir (desteklenen disk sayısı * (1 TB) tek bir disk maksimum kapasitesini). Yapılandırma göstergeleri temel alan konusu her Azure Stack VM için depolama kaynağı istenen düzeyine seçebilir böylece biz kullanılabilir depolama yapılandırmaları tablo doldurulmuş. Ancak, ekleyebilir veya kullanılabilir depolama yapılandırmaları tablo gerektiği şekilde değiştirmeniz gerektiğini unutmayın.<br><br>Her sanal makine başlangıçta atanan bir yerel geçici depolama ile başlar. Ölçülü kaynak sağlama geçici depolama yansıtacak şekilde yerel temp sayısı ve açılan menüden izin verilen maksimum geçici depolama alanı miktarı gibi bir şeye değiştirilebilir.

4. Vm'leri eklediğinizde, değiştirme kullanılabilen SKU kaynakları gösteren grafikler görürsünüz. Bu, çeşitli boyutlarda ve Vm'leri miktarlarını modelleme işlemi sırasında ekleme etkilerini görmenize olanak sağlar. Değişikliklerin etkisini görüntülemenin bir başka yolu doğrudan kullanılabilir sanal makinelerin listesini listelenen tüketilen ve hala kullanılabilir numaraları izleyin sağlamaktır. Bu sayı şu anda seçili donanımda SKU göre tahmini değerleri yansıtır.
5. Vm'leri kümeniz oluşturulduktan sonra önerilen donanım SKU doğrudan "Geçerli SKU" etiketi altında sayfanın sağ üst köşesinde bulunan "Önerilen SKU" düğmesine tıklayarak bulabilirsiniz. Bu düğmeyi kullanarak, daha sonra sanal makine yapılandırmalarınızı değiştirebilir ve her yapılandırma, donanım desteklediğini görün.


### <a name="definebyworkloadfootprint-instructions"></a>DefineByWorkloadFootprint yönergeleri
Azure Stack iş yükleri koleksiyonu kullanarak bir model oluşturmak için "DefineByWorkloadFootprint" sekmesini seçin ve bu adımlar dizisini izleyin. Azure Stack iş yükleri, kullanılabilir VM kaynakları kullanarak oluşturulur.   

> [!TIP]
> Azure Stack VM için sağlanan depolama boyutu değiştirmek için önceki bölümde üç adımdan nota bakın.

1. Bu sayfanın sağ alt köşesinde, sağlanan aşağı açılır liste kutusu denetimleri bir ilk sayı seçmek için kullanın. (arasında 4 ile 16) sunucularının her donanım sisteminin (SKU) yüklenmesini istediğiniz.
2. Belirli bir donanıma karşı çeşitli VM kaynak ayırmalar model istiyorsanız, sayfanın sağ üst köşesindeki mavi aşağı açılır liste kutusunun hemen altındaki "Geçerli SKU" etiketi bulun. Bu liste kutusu çekme ve istenen donanımınız SKU'ı seçin.
3. DefineByVMFootprint yönergeleri üç adımda yukarıda açıklandığı gibi istenen Azure Stack vm'lerinizin DefineByVMFootprint sayfasında her biri için uygun depolama alanı boyutu seçin. VM başına depolama alanı boyutu DefineByVMFootprint sayfasında tanımlanır.
4. En fazla altı farklı iş yükü türleri için yapılandırmaları DefineByWorkloadFootprint sayfanın üst sol tarafında başlayarak, iş yükü içinde bulunan her VM türünün sayısını girerek oluşturun. Bu, doğrudan bu iş yükü adı altındaki sütununa sayısal değerler girerek gerçekleştirilir. İş yükü adları belirli bu yapılandırma tarafından desteklenen iş yükleri türünü yansıtmak için değiştirilebilir.
5. Bu sütunun doğrudan "Miktarı" etiketi altında alt kısmındaki bir değer girerek belirli bir miktar her iş yükü türü içerebilir.
6. İş yükü türleri ve miktarlar oluşturulduktan sonra "önerilen SKU" tıklayarak doğrudan "Geçerli SKU" etiket altında sayfanın sağ üst köşesinde bulunan düğmesi küçük SKU bu genel desteklemek için yeterli kaynaklara sahip neden olur Görüntülenecek iş yükleri yapılandırması.
7. Daha fazla modelleme elde edilebilir donanım için SKU, seçilen sunucu sayısına göre değiştirerek veya VM ayırma veya iş yükü yapılandırmalarınızı içindeki miktar değiştirme. İlişkili grafik değişikliklerinizi genel kaynak tüketimini etkilemesi gösteren anında geri bildirim görüntüler.
8. Değişikliklerinizi memnun olduğunuzda, tıklayarak **önerilen SKU** yeni yapılandırmanız için önerilen SKU yeniden görüntülenir. İstenen SKU'nuz seçmek için aşağı açılan menüsünü de tıklayabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar
Hakkında bilgi edinin [Azure Stack için veri merkezi tümleştirme konuları](azure-stack-datacenter-integration.md)
