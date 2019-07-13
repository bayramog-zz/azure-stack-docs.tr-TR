---
title: Azure Stack ile karma bulut desenleri | Microsoft Docs
description: Hibrit Bulut ve Azure Stack ile akıllı uç desenleri genel bakış
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/11/2019
ms.author: mabrigg
ms.reviewer: anajod
ms.lastreviewed: 06/11/2019
ms.openlocfilehash: e390e43b02d93690c1e233279d52d03665414bf7
ms.sourcegitcommit: 51ec68b5e6dbf437aaca19a9f35ba07d2c402892
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67856414"
---
# <a name="hybrid-cloud-design-patterns-for-azure-stack"></a>Azure Stack için hibrit bulut tasarımı desenleri

Microsoft Azure tutarlı tek hibrit buluttur. Bu geliştirme yatırımlarınızı yeniden kullanmanıza izin verir ve genel Azure, Azure bağımsız bulutlarda ve veri merkezinizde Azure uzantısı olan Azure Stack yayılabilir uygulamaları etkinleştirir. Bulut özellikli uygulamalar da denir *karma uygulamalar*.

[ *Azure Uygulama Mimarisi Kılavuzu* ](https://docs.microsoft.com/azure/architecture/guide) ölçeklenebilir, dayanıklı ve yüksek oranda kullanılabilir uygulamalar tasarlamaya yönelik yapılandırılmış bir yaklaşım açıklanmaktadır. Dikkat edilecek noktalara açıklanan [ *Azure Uygulama Mimarisi Kılavuzu* ](https://docs.microsoft.com/azure/architecture/guide) bulut özellikli uygulamalar ve tek bir bulut için tasarlanmış uygulamalar için eşit olarak uygulanır.

Bu makalede artırmaktadır [ *yazılım kalitesinin yapı taşları* ](https://docs.microsoft.com/azure/architecture/guide/pillars) ele [ *Azure uygulama* ](https://docs.microsoft.com/azure/architecture/guide/) [ *Mimarisi Kılavuzu*,](https://docs.microsoft.com/azure/architecture/guide/) özellikle karma uygulamalar tasarlamaya odaklanarak. Ayrıca, eklediğimiz bir *yerleştirme* pillar karma uygulamalar, özel bir buluta değil veya bir şirket içi veri merkezi.

Karma senaryolar, geliştirme için kullanılabilir ve coğrafi konum, güvenlik, Internet erişimi gibi konuları ve diğer konuları kapsayan kaynaklar ile büyük ölçüde farklılık gösterir. Bu kılavuz, belirli konularınızdan numaralandırılamıyor olsa da, bazı temel ilkeler ve önerilen uygulamalar, takip etmek için sağlayabilir. Başarılı bir şekilde tasarlama, yapılandırma, dağıtma ve karma uygulama mimarisi koruma kendiliğinden size bilinmiyor olabilir birçok tasarım konuları içerir.

Bu belge, karma uygulamalar uygularken çıkabilecek olası soruları toplama amaçlar ve önemli noktalar (Bu yapı taşlarına) ve bunlarla çalışmak için en iyi yöntemler sağlar. Bu soruları tasarım aşamasında ele alarak üretimde neden olabilecek sorunları önlenir.

Esas olarak, bu karma uygulamayı oluşturmadan önce dikkat etmeniz gereken ihtiyacınız sorular verilmiştir. Başlamak için aşağıdakileri yapmanız gerekir:

-   Tanımlamak ve uygulama bileşenlerini değerlendirir.

-   Uygulama bileşenleri yapı taşları karşı değerlendirin.

## <a name="evaluate-the-application-components"></a>Uygulama bileşenleri değerlendir

Bir uygulama'nın her bileşeninin belirli kendi rolünün daha büyük uygulama içinde ve ile tüm tasarım konuları gözden geçirilmesi gerekir. Her bileşenin gereksinimler ve Özellikler uygulama mimarisi belirlemeye yardımcı olması için bu konuları eşlenmesi gerekir.

Uygulamanızın bileşenlerinin uygulamanızın mimarisini incelemek ve neleri içerdiği belirleme parçalayın. Bileşenleri uygulamanızı etkileşimde diğer uygulamalar da dahil edebilirsiniz. Bileşenleri tanımlarken, aşağıdaki gibi özelliklerine göre hedeflenen karma işlemleri değerlendirin:

-   Bileşen amacı nedir?

-   Bileşenler arasındaki bağımlılıkları nelerdir?

Örneğin, bir uygulama bir ön uç olabilir ve arka uç iki bileşenleri olarak tanımlanmış. Karma bir senaryoda, ön uç bir bulutta ve diğer arka uç şeklindedir. Uygulama

ön uç ve kullanıcı arasındaki ve ön uç ve arka uç arasındaki iletişim kanallarını sağlar.

Bir uygulama bileşeni, birçok formlar ve senaryoları tarafından tanımlanır. En önemli görev tanımlanması bunları ve bunların Bulut veya şirket içinde konumu.

Ortak uygulama bileşenlerini envanterinize dahil Tablo 1'de listelenir.

### <a name="table-1-common-app-components"></a>Tablo 1. Ortak uygulama bileşenleri


| **Bileşen** | **Karma Uygulama Kılavuzu** |
| ---- | ---- |
| İstemci bağlantıları | (Herhangi bir cihazda), uygulamanızın çeşitli şekillerde kullanıcılara aşağıdakiler dahil olmak üzere, bir tek giriş noktasından erişebilirsiniz:<br>-Bir istemci uygulaması ile çalışmak üzere yüklü olmasını gerektirir bir istemci-sunucu modeli. Bir web tarayıcısından erişilen bir sunucu tabanlı bir uygulama.<br>-Dolaşım uygulanabilir olduğunda bağlantı bozuk olduğunda bildirimleri veya uyarıların istemci bağlantılarını içerebilir. |
| Authentication  | Kimlik doğrulaması için bir kullanıcı uygulamaya veya bir bileşen başka bir bağlama bağlanma gerekli olabilir. |
| API'ler  | Geliştiriciler API kümeleri ve sınıf kitaplıkları ile programlı erişim sağlamak ve Internet standartlarını temel alan bir bağlantı arabirim sağlar. Ayrıca, bağımsız olarak mantıksal birimler işletim içine bir uygulamayı ayırmak için de API'leri kullanabilirsiniz. |
| Hizmetler  | Bir uygulama için özellikler sağlamak üzere Sözün hizmetlerini kullanabilirsiniz. Bir hizmet uygulamasının üzerinde çalıştığı altyapısı olabilir. |
| Kuyruklar | Uygulamanızın bileşenlerinin durumları ve yaşam döngüsü durumunu düzenlemek için kuyrukları kullanabilirsiniz. Bu kuyruklar, bildirimler, Mesajlaşma ve abone taraflara özellikleri arabelleğe alma sağlayabilir. |
| Veri depolama | Durum bilgisi olan veya bir uygulama olabilir. Durum bilgisi olan uygulamalar, çok sayıda biçimleri ve birimler karşılanabiliyorsa veri depolamaya ihtiyacınız vardır. |
| Verileri önbelleğe alma  | Veri önbelleğe alma bileşeni tasarımınızda stratejik gecikme sorunlarını gidermek ve bulut Patlaması tetikleyen bir rol oynar. |
| Veri alımı | Veri web formunda kullanıcı tarafından gönderilen değerlerden sürekli olarak yüksek hacimli veri akışı arasında değişen birçok yönden uygulamaya gönderilebilir. |
| Veri işleme | Veri işleme görevlerinizin (örneğin, raporlar, analiz, toplu dışarı aktarma ve veri dönüştürme) kaynakta işlenen veya verilerin bir kopyasını kullanarak, ayrı bir bileşende Boşaltılan. |

## <a name="assess-app-components-for-pillars"></a>Uygulama bileşenleri için yapı taşları değerlendirin

Her bileşen için her ayağına için özelliklerini değerlendirin. Tüm yapı taşları her bir bileşenin değerlendirirken, karma uygulama tasarımını etkileyen size, dikkate alınmaz sorular bilinen. Bu konular üzerinde işlem uygulamanızı en iyi duruma getirme değer ekleyebilirsiniz. Karma uygulamalar için bağlantılı olarak tablo 2 her ayağına açıklamasını sağlar.

### <a name="table-2-pillars"></a>Tablo 2. Yapı Taşları

| **Sütun** | **Açıklama** |
| ----------- | --------------------------------------------------------- |
| Yerleştirme  | Stratejik karma uygulamalarda bileşenlerinin konumlandırma. |
| Ölçeklenebilirlik  | Sistemin artan yükü idare edebilme özelliği. |
| Kullanılabilirlik  | Karma uygulama görür ve çalışır olduğu sürenin oranı. |
| Dayanıklılık | Kurtarmak bir karma uygulama özelliği. |
| Yönetilebilirlik | Sistemi üretimde çalışır durumda tutan operasyon süreçleri. |
| Güvenlik | Karma uygulama ve verilerin tehditlere karşı korunması. |

## <a name="placement"></a>Yerleştirme

Karma uygulama kendiliğinden yerleştirmeyi göz önünde gibi veri merkezi için vardır.

Yerleştirme, böylece karma uygulamayı en iyi şekilde hizmet verebilirsiniz bileşenleri konumlandırmanın önemli bir görevdir. Tanımı gereği, karma uygulamalar konumları gibi şirket içinden buluta ve farklı Bulutlar arasında yayılır. İki yolla bulutlarda uygulama bileşenlerinin yerleştirebilirsiniz:

-   **Dikey karma uygulamalar**  
    Uygulama bileşenleri, konumlar arasında dağıtılır. Tek tek her bileşen, yalnızca tek bir konumda bulunan birden çok örneği olabilir.

-   **Yatay karma uygulamalar**  
    Uygulama bileşenleri, konumlar arasında dağıtılır. Tek tek her bileşen, birden fazla konuma yayılan birden çok örneği olabilir.
    
    Diğerleri kendi konumu ve yerleşimi herhangi bir bilgi yoktur ancak bazı bileşenler, konumunu haberdar olabilir. Bu virtuousness bir soyutlama katmanı ile gerçekleştirilebilir. Bu katman, mikro hizmetler gibi modern uygulama çerçevesi ile nasıl uygulama bulutlarda düğümler üzerinde çalışan uygulama bileşenleri yerleşimini tarafından hizmet verilen tanımlayabilirsiniz.

### <a name="placement-checklist"></a>Yerleştirme denetim listesi

**Gerekli konumları doğrulayın.** Uygulama veya bileşenleri çalışması için gerekli olduğundan emin olun veya özel bulutu için sertifika gerektirir. Bu, özerkliği gereksinimleri şirketinizdeki içerebilir veya yasalar tarafından dikte. Ayrıca, herhangi bir şirket içi işlem belirli bir konuma veya yerel ayar için gerekli olup olmadığını belirleyin.

**Bağlantı bağımlılıkları anlamamıza.** Gerekli konumları ve diğer faktörlere bileşenlerinizi arasında bağlantı bağımlılıklar okuyabilirsiniz. Bileşenleri yerleştirirken, bunlar arasındaki iletişimi için güvenlik ve en iyi bağlantı belirleyin. Seçenekler şunlardır [ *VPN*,](https://docs.microsoft.com/azure/vpn-gateway/) [ *ExpressRoute*,](https://docs.microsoft.com/azure/expressroute/) ve [ *karma bağlantılar*.](https://docs.microsoft.com/azure/app-service/app-service-hybrid-connections)

**Platform özelliklerini değerlendirin.** Her uygulama bileşeni için uygulama bileşeni için gerekli kaynak sağlayıcısı bulutta kullanılabilir durumdaysa ve bant genişliği beklenen aktarım hızı ve gecikme süresi gereksinimlerine uyum bakın.

**Taşınabilirlik için planlayın.** Kapsayıcıları ve mikro hizmetler, modern uygulama altyapılarını taşıma işlemleri için plan ve Hizmet bağımlılıkları engellemek için kullanın.

**Veri egemenliği gereksinimlerini belirleyin.** Karma uygulamalar, veri yalıtımı gibi yerel bir veri merkezi üzerinde destekleme için geliştirilmiş. Yerleştirme başarılı destekleme bu gereksinim için en iyi duruma getirme, kaynaklarınızın gözden geçirin.

**Gecikme süresi için planlayın.** Uygulama bileşenleri arasındaki fiziksel uzaklığı arası bulut işlemleri ortaya çıkarabilir. Herhangi bir gecikme süresi uyum sağlamak için gereksinimleri anlamamıza.

**Trafik akışları denetler.** En yüksek kullanımı ve uygun ve güvenli iletişim için bir genel buluttaki ön uç tarafından erişildiğinde kişisel olarak tanımlanabilir bilgileri veri işleyin.

## <a name="scalability"></a>Ölçeklenebilirlik

Diğer etkenler ve kuvvetleri zaman içinde değişir, boyutu ve uygulama Kapsamınız ek hedef kitle boyutu etkileyen bir uygulama, artan yükü idare edebilme sistemin ölçeklenebilirlik özelliğidir.

Bu yapı taşı çekirdek tartışma için bkz [ *ölçeklenebilirlik* ](https://docs.microsoft.com/azure/architecture/guide/pillars#scalability) yazılım kalitesinin yapı taşları içinde.

Talebi karşılamak üzere daha fazla örnek ekleyerek ve ardından bunları sakin dönemlerde devre dışı bırakmak için karma uygulamalar için yatay ölçekleme bir yaklaşım sağlar.

Bileşenleri Bulutlar arasında yayılır, hibrit senaryolarda, bağımsız bileşenler ölçeklendirme sağlayabildiği gerektirir. Uygulamanın bir parçası olarak ölçeklendirme, ölçeklendirme başka birine gerektirebilir. Örneğin, istemci bağlantıları artırır ancak uygulamanın web hizmetleri sayısı üzerinde uygun şekilde yük ölçeklenmez, veritabanı uygulama saturate.

Diğer bağımlılıkları Ölçeklendirmesi ve sınırlı olabilir ancak bazı uygulama bileşenleri doğrusal olarak, ölçeği genişletebilirsiniz ne genişletmek için bunlar ölçeklendirebilirsiniz. Örneğin, uygulama bileşenleri konumlarını karma bağlantı sağlayan bir VPN tüneli bant genişliği ve gecikme süresi için ölçeklendirilebilir bir sınırı vardır. Bu gereksinimleri sağlamak için ölçeği uygulamanın şeklini karşılandığından?

### <a name="scalability-checklist"></a>Ölçeklenebilirlik denetim listesi

**Ölçeklendirme eşikleri belirlemek.** Uygulamanızı çeşitli bağımlılıkları işlemek için için hangi uygulama bileşenleri farklı bulutlardaki birbirinden yine de uygulamayı çalıştırmak için gereksinimlerini karşılarken ölçeklendirebilirsiniz kapsamı belirler. Karma uygulamalar, genellikle bir özellik etkileşim kurar ve uygulamanın kalan etkiler işlemek için uygulamada belirli alanları ölçeklendirmeniz mi gerekiyor. Örneğin, bir ön uç örnek sayısı aşan arka uç ölçeklendirme gerektirebilir.

**Ölçek zamanlamalar tanımlayın.** Yoğun süreleri en iyi ölçeklendirme koordine etmek için zamanlama toplama gerek çoğu uygulama meşgul nokta vardır.

**Merkezi bir izleme sistemi kullanın.** Platform izleme işlevlerini otomatik ölçeklendirme sağlayabilir, ancak karma uygulamalar sistem durumu ve yük toplayan merkezi bir izleme sistemi gerekir. Merkezi bir izleme sistemi, tek bir konumda bir kaynak ölçeklendirme ve bir kaynak başka bir konuma bağlı olarak ölçeklendirme başlatabilirsiniz. Ayrıca, merkezi bir izleme sistemi, otomatik ölçeklendirme kaynakları Bulutları ve hangi bulut yok izleyebilirsiniz.

**Otomatik ölçeklendirme özelliklerine (olarak kullanılabilir) yararlanın.** Otomatik ölçeklendirme özelliklerine kısmı ise bir uygulama bileşeni ayarlama, out, aşağı ya da ölçeklendirilmesi gerektiğinde tanımlayan eşikleri ayarlayarak otomatik ölçeklendirme uygulayın. Otomatik ölçeklendirme autoscaled artan kapasite işlemek için bir bulutta bulunan, ancak diğer bağımlılıkları da ölçeklendirilmesi farklı Bulutlar arasında yayılabilir uygulamanın neden olan bir istemci bağlantısı örneğidir. Bu bağımlı bileşenler otomatik ölçeklendirme özelliklerini saptanabilen gerekir.

Otomatik ölçeklendirme kullanılabilir durumda değilse, uygulama betikler ve el ile ölçeklendirme, uyum sağlamak için diğer kaynaklar eşiklerini Merkezi İzleme sistemde tetiklenebilir göz önünde bulundurun.

**Konuma göre beklenen yükü belirleyin.** Öncelikle, istemci isteklerini işlemesine karma uygulamalar üzerinde tek bir yerde bağlı olabilir. İstemci istekleri yükünü bir eşiği aştığında,

Ek kaynaklar, gelen isteklerin yükünü dağıtmak için farklı bir konumda eklenebilir. İstemci bağlantıları artan yükü işlemek ve ayrıca yükü işlemek istemci bağlantıları için otomatik yordamlarla belirlemek emin olun.

## <a name="availability"></a>Kullanılabilirlik

Kullanılabilirlik, bir sistemin işlevsel ve çalışır olduğu zamandır. Kullanılabilirlik çalışma süresinin yüzdesi ölçülür. Uygulama hataları, altyapı sorunları ve sistem yükü, kullanılabilirliği azaltabilecek faktörlerdir.

Bu yapı taşı çekirdek tartışma için bkz [ *kullanılabilirlik* ](https://docs.microsoft.com/azure/architecture/guide/pillars#availability) yazılım kalitesinin yapı taşları içinde.

### <a name="availability-checklist"></a>Kullanılabilirlik denetim listesi

**Artıklık için bağlantı sağlayın.** Karma uygulamalar, uygulama yayılmış Bulutlar arasında bağlantı gerektirir. Karma bağlantı için teknolojiler vardır, ayrıca birincil teknoloji seçiminiz için birincil teknoloji başarısız olması, otomatik yük devretme özellikleriyle yedeklilik sağlamak için başka bir teknolojiyi kullanın.

**Hata etki alanları sınıflandırın.** Hataya dayanıklı uygulamalar birden çok hata etki alanları gerektirir. Hata etki alanları yardımcı noktası yalıtmak arıza durumunda, IF gibi tek bir sabit disk şirket içi bir top-of-rack switch arıza yaparsa veya tam veri merkezi kullanılabilir durumda değilse başarısız olur. Bir karma uygulamada hata etki alanı bir konuma sınıflandırılabilir. Daha fazla kullanılabilirlik gereksinimleri olan daha, bir tek hata etki alanı nasıl sınıflandırıldığını değerlendirmeniz gerekir.

**Yükseltme etki alanları sınıflandırın.** Yükseltme etki alanları ile güncelleştirmeleri ya da özellik yükseltmelerini diğer örnekleri aynı bileşenin hizmet verilir ancak uygulama bileşenleri örneklerini kullanılabilir olmasını sağlamak için kullanılır. Hata etki alanları ile gibi yükseltme etki alanları tarafından yatırımlarından konumlar arasında sınıflandırılabilir. Başka bir konumda yükseltilmeden önce tek bir yerde yükseltilmeyen bir uygulama bileşeni yetiyorsa ya da diğer etki alanı yapılandırmaları gerekiyorsa belirlemeniz gerekir. Bir tek konuma, birden fazla yükseltme etki alanları olabilir.

**Örnekler ve kullanılabilirliğini izleyin.** Yüksek oranda kullanılabilir bir uygulama bileşenleri, Yük Dengeleme ve zaman uyumlu veri çoğaltmayı kullanılabilir. Hizmetin kesintiye önce kaç tane çevrimdışı kalabileceği belirlemeniz gerekir.

**Kendi kendini iyileştirecek şekilde uygulayın.** Bir uygulamanın kullanılabilirliğini kesintiye uğramasını bir soruna neden olayda bir izleme sistemi tarafından bir algılama başarısız örneğini boşaltma ve yeniden dağıtmaya gerek gibi bu uygulamaya kendini onarma faaliyetlerini başlatabilirsiniz. Bu büyük olasılıkla bir karma sürekli tümleştirme ve sürekli teslim (CI/CD) işlem hattı ile tümleşik bir merkezi bir izleme çözümü gerektirir. Uygulama, bir uygulama bileşeni çözümünüzün yeniden dağıtımını gerektirebilecek sorunları belirlemek için bir izleme sistemi ile tümleşiktir. İzleme sistemi, ayrıca karma uygulama bileşeni ve büyük olasılıkla diğer bağımlı bileşenler aynı veya farklı konumlarda yeniden dağıtmak için CI/CD tetikleyebilirsiniz.

**Hizmet düzeyi sözleşmeleri (SLA'lar) korur.** Kullanılabilirlik, müşterilerle olan uygulamalar ve hizmetler bağlantıyı sürdürmek tüm anlaşmalar için önemlidir. Karma uygulamanızı dayanan her konum, kendi SLA'sı olabilir. Bu farklı SLA'lar genel SLA'yı karma uygulamanızın etkileyebilir.

## <a name="resiliency"></a>Dayanıklılık

Dayanıklılık için bir karma uygulama ve sistemin hatalardan kurtularak çalışmaya devam yeteneğidir. Dayanıklılığın hedefi, bir hatanın ardından uygulamayı tam çalışır duruma geri döndürmektir. Dayanıklılık stratejileri gibi yedekleme, çoğaltma ve olağanüstü durum kurtarma çözümleri içerir.

Bu yapı taşı çekirdek tartışma için bkz [ *dayanıklılık* ](https://docs.microsoft.com/azure/architecture/guide/pillars#resiliency) yazılım kalitesinin yapı taşları içinde.

### <a name="resiliency-checklist"></a>Dayanıklılık denetim listesi

**Olağanüstü durum kurtarma bağımlılıkları ortaya çıkarın.** Bir bulutta olağanüstü durum kurtarma için başka bir bulut uygulama bileşenleri değişiklikler gerektirebilir. Bir veya birden çok bileşen bir buluttan aynı buluttaki ya da başka bir bulut için başka bir konuma yük devredildi durumunda bağımlı bileşenleri bu değişikliklerden haberdar olmanız gerekir. Bu, bağlantı bağımlılıkları da içerir. Dayanıklılık, her bulut için tam olarak test edilmiş uygulama kurtarma planı gerektirir.

**Kurtarma akışı oluşturun.** Uygulama bileşenleri yeteneklerini arabellekler uyum sağlamak için bir etkin kurtarma akışı tasarım değerlendirildi, yeniden deneniyor yeniden denemeler, veri aktarımı başarısız oldu ve, gerekirse farklı bir hizmet veya iş akışına geri döner. Hangi yedekleme mekanizması kullanmak için ne, geri yükleme yordamı içerir ve ne sıklıkta test belirlemeniz gerekir. Ayrıca, artımlı ve tam yedekleme sıklığını belirlemeniz gerekir.

**Kısmi kurtarma işlemleri test edin.** Uygulamanın bir parçası için kısmi bir kurtarma, tüm olmadığını kullanılamaz güvenceyi kullanıcılara sağlayabilirsiniz. Bu planın bir parçası, kısmi bir geri değil ve yedekleme gibi herhangi bir yan etkisi olan yedekleme yapılmadan önce düzgün kapatmayı uygulamayla etkileşimiyle hizmetin geri emin olmalısınız.

**Olağanüstü durum kurtarma instigators belirlemek ve Sorumluluk atayın.** Kurtarma planı kim, açıklayın ve hangi rollerin ne yedeklenebilir ve geri yanı sıra yedekleme ve kurtarma eylemlerini başlatabilirsiniz.

**Kendi kendini onaran eşikleri olağanüstü durum kurtarma ile karşılaştırın.** Bir uygulamanın kendi kendini onaran özellikleri için Otomatik Kurtarma başlatma ve bir uygulamanın kendi kendini onarma başarılı veya başarısız olarak değerlendirilmesi için-için gereken süreyi belirler. Her bulut için eşikler belirleyin.

**Kullanılabilirlik, dayanıklılık özelliği doğrulayın.** Dayanıklılık özelliği ve her konum için özellikleri kullanılabilirliğini belirleyemedi. Bir konum gerekli özellikleri sağlamaz, bu konuma dayanıklılık özelliği sunan merkezi bir hizmette tümleştirerek göz önünde bulundurun.

**Kapalı kalma süreleri belirleyin.** Bakım nedeniyle beklenen kapalı kalma uygulamanın uygulama bileşenleri ve bir bütün olarak belirleyin.

**Sorun giderme yordamlarını belgeleyin.** Kaynaklar ve uygulama bileşenleri yeniden dağıtmak için sorun giderme yordamlarını tanımlar.

## <a name="manageability"></a>Yönetilebilirlik

Karma uygulamalarınızı yönetme konuları Mimarinizi tasarlama son derece önemlidir. İyi yönetilen karma uygulaması ortak bir geliştirme işlem hattı tutarlı uygulama kodunda entegrasyonunu sağlar kod olarak bir altyapı sağlar. Değişiklikleri, testler başarıyla geçirilirse tutarlı uygulayarak sistem genelinde ve bireysel altyapı değişikliklerinin test, tümleşik bir dağıtım kaynak koda birleştirilecek olanak sağlamak.

Bu yapı taşı çekirdek tartışma için bkz [ *yönetim ve DevOps* ](https://docs.microsoft.com/azure/architecture/guide/pillars#management-and-devops) yazılım kalitesinin yapı taşları içinde.

### <a name="manageability-checklist"></a>Yönetilebilirlik denetim listesi

**İzleme uygulayın.** Uygulama bileşenleri yayılan merkezi bir izleme sistemine bulutlarda kendi sistem durumu ve performans toplu bir görünümünü sağlamak için kullanın. Bu sistem uygulama bileşenlerine ve ilgili platform özelliklerinden izleme içerir.

İzleme gerektiren uygulama bölümlerini belirler.

**İlkeleri koordine edin.** Karma uygulaması yayılan her konum, kaynak türleri, adlandırma kurallarını, etiketler ve diğer ölçütlere izin kapsayan kendi ilkeniz olabilir.

**Rolleri ve tanımlayın.** Bir veritabanı yöneticisi olarak, uygulama kaynaklarına erişmek için gereken farklı kişiler için (örneğin, bir uygulama sahibi, bir veritabanı yöneticisi ve son kullanıcı) gerekli izinleri belirlemeniz gerekir. Bu izinleri, kaynaklar ve uygulama içinde yapılandırılmış olması gerekmez. Rol tabanlı erişim denetimi (RBAC) sistem, uygulama kaynaklarında bu izinleri ayarlamanızı sağlar. Tüm kaynakları tek bir buluta dağıtılabilir ancak daha fazla dikkat etmeniz gereken kaynakları Bulutlar arasında yayılır, bu erişim haklarını zor. Kaynakları bir bulutta izinlerini başka bir bulutta kaynakları için geçerli değildir.

**CI/CD işlem hatlarını kullanın.** Bir sürekli tümleştirme ve sürekli geliştirme (CI/CD) işlem hattı yazma ve bulut üzerinden ve, altyapı ve uygulama için kalite güvencesi sağlamak için kapsayan uygulamalar dağıtmak için tutarlı bir işlemi sağlar. Bu işlem hattı, bir bulut üzerinde test edilecek uygulama ve altyapı sağlar ve başka bir bulut üzerinde dağıtılabilir. İşlem hattı, bir bulut için karma uygulamanızın belirli bileşenleri ve diğer bileşenleri temelde karma uygulama dağıtımı için temel oluşturan başka bir buluta dağıtmak bile sağlar. Kritik bağımlılıklar uygulama bileşenleri diğer için veritabanına bir bağlantı dizesi ihtiyaç duyan web uygulaması gibi bir yükleme sırasında sahip işleme için bir CI/CD sistemidir.

**Yaşam döngüsünü yönetin.** Karma uygulama kaynaklarının konumlarını yayılabilir olduğundan, bir tek yaşam döngüsü yönetim birimine toplanacak tek her konumun yaşam döngüsü yönetimi özelliği gerekiyor. Nasıl, güncelleştirilen ve Silinen oluşturuldukları göz önünde bulundurun.

**Sorun giderme stratejilerini inceleyin.** Karma uygulama sorunlarını giderme, tek bir bulutta çalışan uygulamanın aynısını değerinden daha fazla uygulama bileşenleri içerir. Bulutlar arasında bağlantı yanı sıra, bir yerine iki platformlarda uygulama çalışıyor. Toplanmış sistem durumunu ve uygulama bileşenlerinin performans izleme, karma uygulama sorunlarını giderme içinde önemli bir görev incelemektir.

## <a name="security"></a>Güvenlik

Güvenlik herhangi bir bulut uygulaması için birincil ilgili önemli noktalar biridir ve hibrit bulut uygulamaları için daha önemli hale gelir.

Bu yapı taşı çekirdek tartışma için bkz [ *güvenlik* ](https://docs.microsoft.com/azure/architecture/guide/pillars#security) yazılım kalitesinin yapı taşları içinde.

### <a name="security-checklist"></a>Güvenlik denetim listesi

**İhlal varsayımı.** Bir uygulamanın parçası tehlikedeyse çözümleri yalnızca aynı konumdaki aynı zamanda konumlarında ihlali yayılmasını en aza indirmek için bir yerde emin olun.

**İzin verilen ağ erişimi izleyin.** Ağ erişim ilkelerini yalnızca belirli bir alt ağdan uygulama erişme gibi uygulama belirler ve yalnızca minimum bağlantı noktaları ve protokolleri uygulamanın düzgün çalışması için gerekli bileşenler arasında izin verilir.

**Güçlü kimlik doğrulaması kullanın.** Güçlü kimlik doğrulama düzeni, uygulamanızın güvenlik kritik öneme sahiptir. Çoklu oturum açma özellikleri sağlar ve bir veya daha fazla aşağıdaki düzeni kullanan bir federe kimlik sağlayıcısı kullanarak göz önünde bulundurun: kullanıcı adı ve parola oturum açma, ortak ve özel anahtarlar, iki öğeli veya çok faktörlü kimlik doğrulaması ve güvenilen güvenlik gruplar. Hassas verileri ve uygulama kimlik doğrulaması sertifika türleri ve gereksinimlerine ek olarak diğer gizli dizileri depolamak için uygun kaynakları belirleyin.

**Şifreleme kullanın.** Uygulamanın hangi alanların şifreleme gibi erişim ve veri depolama ya da istemci iletişimi kullanmak belirleyin.

**Güvenli kanal kullanın.** Bulutlar arasında güvenli bir kanal, güvenlik ve kimlik doğrulama denetimleri, gerçek zamanlı koruma, Karantina ve diğer hizmetleri bulutlarda sağlamak için önemlidir.

**Rolleri ve tanımlayın.** Kaynak yapılandırmaları ve tek kimlikli erişim için rolleri bulutlarda uygulayın. Uygulama ve platform kaynaklarını rol tabanlı erişim denetimi (RBAC) gereksinimlerini belirleyin.

**Sisteminizi denetleyin.** Sistem izleme oturum ve uygulama bileşenleri hem bulut platformu işlemleri veri toplayın.

## <a name="summary"></a>Özet

Bu makalede, yazma ve karma uygulamalarınızı tasarlama sırasında dikkate alınması gereken önemli olan öğeleri listesi sağlar. Uygulamanızı dağıtmadan önce bu yapı taşlarına gözden geçirme, bu soruların üretim kesintiler içinde çalışan ve büyük olasılıkla tasarımınızı yeniden ziyaret etmeniz gerek engeller.

Zaman alan bir görev gibi önceden görünebilir, ancak üzerinde bu yapı taşlarına göre uygulamanızı tasarlayın, yatırım getirinizi kolayca alın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

-   [Hibrit bulut](https://azure.microsoft.com/overview/hybrid-cloud/)
-   [Hibrit bulut uygulamaları](https://azure.microsoft.com/solutions/hybrid-cloud-app/)
-   [Bulut tutarlılığı için Azure Resource Manager şablonları geliştirme](http://aka.ms/consistency)