---
title: Azure ve Azure Stack 'da derleme yaparken karma uygulama tasarımı konuları
description: Akıllı bulut ve akıllı kenar için bir karma uygulama oluştururken dikkat edilecek noktalar.
author: BryanLa
ms.service: azure-stack
ms.topic: article
ms.date: 11/05/2019
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 11/05/2019
ms.openlocfilehash: a5d282a45f340461b62acb3462cfb84822e0c2be
ms.sourcegitcommit: 5c92a669007ab4aaffe4484f1d8836a40340dde1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73638821"
---
# <a name="hybrid-application-design-considerations"></a>Karma uygulama tasarımı konuları 

Tek tutarlı karma bulutunuz Microsoft Azure. Geliştirme yatırımlarınızı yeniden kullanmanıza ve küresel Azure, egeign Azure bulutlarını ve Azure Stack veri merkezinizde bir Azure uzantısı olan uygulamalara olanak tanır. Bulutları kapsayan uygulamalar da *karma uygulamalar*olarak adlandırılır.

[*Azure Uygulama Mimarisi Kılavuzu*](https://docs.microsoft.com/azure/architecture/guide) , ölçeklenebilir, dayanıklı ve yüksek oranda kullanılabilir uygulamalar tasarlamak için yapılandırılmış bir yaklaşım açıklamaktadır. [*Azure uygulama mimarisi kılavuzunda*](https://docs.microsoft.com/azure/architecture/guide) açıklanan noktalar, tek bir bulut için tasarlanan uygulamalar ve bulutların yayılacağı uygulamalar için de geçerlidir.

Bu makalede, [*Azure Uygulama*](https://docs.microsoft.com/azure/architecture/guide/) [ *Mimarisi Kılavuzu*](https://docs.microsoft.com/azure/architecture/guide/) 'nda ele alınan yazılım kalitesinin ve özellikle karma uygulamalar tasarlamaya odaklanılan [*yazılım kalitesinin*](https://docs.microsoft.com/azure/architecture/guide/pillars) her ikisi de anlatılmaktadır. Ayrıca, karma uygulamalar tek bir buluta veya bir şirket içi veri merkezine özel olmadığından, bir *yerleşim* de ekleyeceğiz.

Karma senaryolar, geliştirme için kullanılabilen kaynaklarla büyük ölçüde farklılık gösterir ve coğrafya, güvenlik, Internet erişimi ve diğer konular gibi önemli noktalara dikkat edin. Bu kılavuz, belirli konuları numaralandıramasa da, izlemeniz için bazı önemli yönergeler ve en iyi uygulamalar sağlayabilir. Karma uygulama mimarisini başarıyla tasarlama, yapılandırma, dağıtma ve sürdürme, sizin için doğal olarak tanınmayan birçok tasarım ile ilgilidir.

Bu belge, hibrit uygulamalar uygulama sırasında ortaya çıkabilecek olası soruların toplanmasını ve bunlarla çalışmak için önemli noktalar (Bu işlem noktaları) ve en iyi yöntemleri sağlar. Tasarım aşamasında bu sorulara değinerek üretimde neden olabilecek sorunlardan kaçınabilirsiniz.

Temelde, bu, karma uygulama oluşturmadan önce düşünmek için gereken sorulardır. Başlamak için aşağıdakileri yapmanız gerekir:

-   Uygulama bileşenlerini tanımlayıp değerlendirin.

-   Uygulama bileşenlerini bu şekilde değerlendirin.

## <a name="evaluate-the-application-components"></a>Uygulama bileşenlerini değerlendir

Bir uygulamanın her bileşeni daha büyük uygulama içinde kendi özel rolüne sahiptir ve tüm tasarım konuları ile incelenmelidir. Her bileşenin gereksinimleri ve özellikleri, uygulama mimarisini belirlemede yardımcı olması için bu noktalara eşlenmelidir.

Uygulamanızın mimarisini gerçekleştirerek ve ne içerdiğini belirterek uygulamanızı bileşenlerine ayırın. Bileşenler, uygulamanızın etkileşimde bulunduğu diğer uygulamaları da içerebilir. Bileşenleri tanımladığınıza göre, amaçlanan karma işlemlerinizi aşağıdaki gibi özelliklerine göre değerlendirin:

-   Bileşenin amacı nedir?

-   Bileşenler arasında bağımlılıkları nelerdir?

Örneğin, bir uygulama, iki bileşen olarak tanımlanmış bir ön uç ve arka uca sahip olabilir. Karma bir senaryoda, ön uç bir bulutta ve arka uç ise diğeri de olur. Uygulama

ön uç ve Kullanıcı arasında ve ayrıca ön uç ve arka uç arasında iletişim kanalları sağlar.

Bir uygulama bileşeni birçok form ve senaryo tarafından tanımlanır. En önemli görev bunları ve bunların bulutlarını ve şirket içi konumunu tanımlar.

Stokunuzda içerilecek ortak uygulama bileşenleri Tablo 1 ' de listelenmiştir.

### <a name="table-1-common-app-components"></a>Tablo 1. Ortak uygulama bileşenleri


| **Bileşen** | **Karma Uygulama Kılavuzu** |
| ---- | ---- |
| İstemci bağlantıları | Uygulamanız (herhangi bir cihazda), kullanıcılara aşağıdakiler de dahil olmak üzere, tek bir giriş noktasından çeşitli yollarla erişebilir:<br>-Kullanıcının uygulamayla çalışmak için bir istemcisinin yüklü olmasını gerektiren bir istemci-sunucu modeli. Bir Web tarayıcısından erişilen sunucu tabanlı bir uygulama.<br>-İstemci bağlantıları, dolaşım ücretleri uygulanabileceğini, bağlantı kesildiğinde veya uyarıladığınızda bildirimleri içerebilir. |
| Kimlik Doğrulaması  | Uygulamaya bağlanan bir kullanıcı veya başka bir bileşenden bağlantı kurarak kimlik doğrulaması gerekli olabilir. |
| API'ler  | Geliştiricilere API kümeleri ve sınıf kitaplıkları ile uygulamanıza programlı erişim olanağı verebilir ve Internet standartlarına dayalı bir bağlantı arabirimi sağlayabilirsiniz. API 'Leri, bir uygulamayı bağımsız işletim mantıksal birimlerine ayırmak için de kullanabilirsiniz. |
| Hizmetler  | Bir uygulamaya yönelik özellikleri sağlamak için kısa hizmetlerini kullanabilirsiniz. Bir hizmet, uygulamanın üzerinde çalıştığı altyapı olabilir. |
| Kuyruklar | Kullanım döngüleri ve uygulamanızın bileşenlerinin durumlarını düzenlemek için kuyrukları kullanabilirsiniz. Bu kuyruklar, abone tarafların mesajlaşma, bildirim ve arabelleğe alma özelliklerini sağlayabilir. |
| Veri depolama | Bir uygulama durum bilgisiz veya durum bilgisi olabilir. Durum bilgisi olan uygulamalarda çok sayıda biçim ve birim tarafından karşılanabileceği veri depolaması gerekir. |
| Verileri önbelleğe alma  | Tasarımınızda bir veri önbelleği bileşeni, gecikme sorunlarını gidermek ve bulut patlamasını tetikleyerek bir rol oynatabilir. |
| Veri alımı | Veriler, bir Web formundaki Kullanıcı tarafından gönderilen değerlerden sürekli olarak yüksek hacimli veri akışına kadar birçok şekilde bir uygulamaya gönderilebilir. |
| Veri işleme | Veri işleme görevleriniz (raporlar, analiz, toplu dışarı aktarmalar ve veri dönüştürme gibi), kaynakta işlenebilir veya verilerin bir kopyası kullanılarak ayrı bir bileşende boşaltılır. |

## <a name="assess-app-components-for-pillars"></a>Uygulama bileşenlerini, pilde için değerlendir

Her bir bileşen için her bir şekilde özelliklerini değerlendirin. Her bir bileşeni tüm pillerle değerlendirirken, kabul edilmeyebilirsiniz, karma uygulamanın tasarımını etkileyen, sizin de dikkate alınamayabilirsiniz. Bu noktalara işlem yapmak, uygulamanızı en iyi duruma getirmek için değer ekleyebilirler. Tablo 2, karma uygulamalarla ilişkili olan her bir bir açıklama sağlar.

### <a name="table-2-pillars"></a>Tablo 2. Yapı Taşları

| **Sütun** | **Açıklama** |
| ----------- | --------------------------------------------------------- |
| Yerleştirilmesine  | Karma uygulamalardaki bileşenlerin stratejik konumlandırması. |
| Ölçeklenebilirlik  | Sistemin artan yükü idare edebilme özelliği. |
| Erişilebilirlik  | Karma uygulamanın işlevsel ve çalışır olduğu sürenin oranı. |
| Dayanıklılık | Karma uygulamanın kurtarılmasına olanak tanır. |
| Yönetilebilirlik | Sistemi üretimde çalışır durumda tutan operasyon süreçleri. |
| Güvenlik | Karma uygulamaları ve verileri tehditlere karşı koruma. |

## <a name="placement"></a>Yerleştirilmesine

Doğal olarak bir karma uygulama, veri merkezi gibi bir yerleşim değerlendirmesi içerir.

Yerleştirme, bileşenlerin bir karma uygulamayı en iyi şekilde hizmet edebilmesi için konumlama açısından önemli bir görevdir. Tanım olarak, karma uygulamalar, Şirket içinden buluta ve farklı bulutlar arasında yer alan konumları kapsar. Uygulama bileşenlerini bulutlara iki şekilde yerleştirebilirsiniz:

-   **Dikey karma uygulamalar**  
    Uygulama bileşenleri konumlar arasında dağıtılır. Her ayrı bileşen yalnızca tek bir konumda bulunan birden çok örneğe sahip olabilir.

-   **Yatay karma uygulamalar**  
    Uygulama bileşenleri konumlar arasında dağıtılır. Her bir bileşenin birden çok konumu kapsayan birden fazla örneği olabilir.
    
    Bazı bileşenler, başkalarının konumu ve yerleştirmesi hakkında bilgi sahibi olmayan konumlarından haberdar olabilir. Bu virtuousness bir soyutlama katmanıyla elde edilebilir. Mikro hizmetler gibi modern bir uygulama çerçevesi olan bu katman, bulut genelinde düğümlerde çalışan uygulama bileşenlerinin yerleştirilmesiyle uygulamanın nasıl hizmet vermesini tanımlayabilir.

### <a name="placement-checklist"></a>Yerleştirme denetim listesi

**Gerekli konumları doğrulayın.** Uygulamanın veya bileşenlerinden birinin, belirli bir bulut için üzerinde çalışması veya sertifika gerektirmesi için gerekli olduğundan emin olun. Bu, şirketinizdeki veya yasalar tarafından dikte edilen egemenlik gereksinimlerini içerebilir. Ayrıca, belirli bir konum veya yerel ayar için herhangi bir şirket içi işlemin gerekli olup olmadığını belirleyebilirsiniz.

**Yokermesi bağlantı bağımlılıkları.** Gerekli konumlar ve diğer faktörler, bileşenlerinizin arasında bağlantı bağımlılıklarını dikte edebilir. Bileşenleri yerleştirirken, aralarında iletişimin en iyi bağlantısını ve güvenliğini saptayın. [ *VPN*,](https://docs.microsoft.com/azure/vpn-gateway/) [ *ExpressRoute*](https://docs.microsoft.com/azure/expressroute/) ve [ *karma bağlantılar*](https://docs.microsoft.com/azure/app-service/app-service-hybrid-connections) seçenekleri bulunur.

**Platform yeteneklerini değerlendirin.** Her uygulama bileşeni için, uygulama bileşeni için gerekli kaynak sağlayıcısının bulutta kullanılabilir olup olmadığını ve bant genişliğinin beklenen aktarım hızını ve gecikme süresi gereksinimlerini kapsayabildiğine bakın.

**Taşınabilirlik için plan yapın.** Taşıma işlemlerini planlamak ve hizmet bağımlılıklarını engellemek için kapsayıcılar veya mikro hizmetler gibi modern uygulama çerçeveleri kullanın.

**Veri egemenlik gereksinimlerini belirleme.** Karma uygulamalar, yerel bir veri merkezinde olduğu gibi veri yalıtımına yönelik olarak tasarlanmıştır. Bu gereksinimi karşılamak için başarıyı iyileştirmek üzere kaynaklarınızın yerleşimini gözden geçirin.

**Gecikme için plan yapın.** Bulut tabanlı işlemler, uygulama bileşenleri arasında fiziksel mesafe ortaya çıkarabilir. Herhangi bir gecikmeyi karşılamak için gereksinimleri yokler.

**Trafik akışlarını denetleme.** Genel bir bulutta ön uç tarafından erişildiğinde, kişisel olarak tanımlanabilen bilgi verileri için en yüksek kullanımı ve uygun ve güvenli iletişimleri işleyin.

## <a name="scalability"></a>Ölçeklenebilirlik

Ölçeklenebilirlik, sistemin bir uygulamada daha fazla yük işlemesini sağlar. Bu, diğer faktörlere göre zaman içinde farklılık gösterebilir ve uygulamanın boyutuna ve kapsamına ek olarak izleyici boyutunu etkiler.

Bu pilde temel tartışmak için, yazılım kalitesinin pillerini anlamak için bkz. [*ölçeklenebilirlik*](https://docs.microsoft.com/azure/architecture/guide/pillars#scalability) .

Karma uygulamalar için yatay ölçeklendirme yaklaşımı, talebi karşılamak için daha fazla örnek eklenmesine ve daha sonra daha sessiz dönemler sırasında devre dışı bırakılmasına olanak tanır.

Karma senaryolarda, bileşenlerin bulutlara yayıldığı durumlarda, tek tek bileşenlerin ölçeğini genişletmek için ek dikkat gerekir. Uygulamanın bir bölümünün ölçeklendirilmesi, başka bir ölçeklendirme gerektirebilir. Örneğin, istemci bağlantısı sayısı arttıkça, ancak uygulamanın Web Hizmetleri uygun şekilde ölçeklenirse, veritabanındaki yük uygulamayı artırabilir.

Bazı uygulama bileşenleri doğrusal bir şekilde ölçeklenebilir, diğerleri de ölçeklendirme bağımlılıklarına sahiptir ve ölçeklendirebilecekleri genişlemeyle sınırlı olabilir. Örneğin, uygulama bileşenleri konumları için karma bağlantı sağlayan bir VPN tüneli, ölçeklendirilebilen bant genişliği ve gecikme süresine sahiptir. Uygulamanın bileşenleri, bu gereksinimlerin karşılandığından emin olmak için nasıl ölçeklendirilir?

### <a name="scalability-checklist"></a>Ölçeklendirilebilirlik denetim listesi

**Yokermesi ölçekleme eşikleri.** Uygulamanızdaki çeşitli bağımlılıkları işlemek için, farklı bulutlardaki uygulama bileşenlerinin birbirinden bağımsız olarak ölçekleyeceğini belirleme, ancak uygulamayı çalıştırmak için gereksinimleri hala karşılarken. Hibrit uygulamalar, etkileşim kurduğu ve uygulamanın geri kalanını etkilediği bir özelliği işlemek için genellikle uygulamadaki belirli alanların ölçeklendirilmesi gerekir. Örneğin, bir dizi ön uç örneğini aşarak arka ucun ölçeklendirilmesi gerekebilir.

**Ölçek zamanlamalarını tanımlayın.** Çoğu uygulamanın meşgul süreleri vardır, bu yüzden en iyi ölçeklendirmeyi koordine etmek için yoğun süreleri en yüksek zamanlara toplamanız gerekir.

**Merkezi bir izleme sistemi kullanın.** Platform izleme özellikleri otomatik ölçeklendirme sağlayabilir, ancak karma uygulamalar sistem durumunu ve yüklemeyi toplayan merkezi bir izleme sistemine gerek duyar. Merkezi bir izleme sistemi bir konumdaki kaynağı ölçeklendirerek başka bir konumdaki kaynağa bağlı olarak ölçeklendirebilirsiniz. Ayrıca, merkezi bir izleme sistemi hangi bulutlar için otomatik ölçeklendirme kaynaklarını ve hangi bulutların olduğunu izleyebilir.

**Otomatik ölçeklendirme özelliklerinden yararlanın (kullanılabilir).** Otomatik ölçeklendirme özellikleri mimarinizin parçasıysa, bir uygulama bileşeninin ne zaman yukarı, dışarı veya aşağı ölçeklendirilmesi gerektiğini tanımlayan eşikleri ayarlayarak otomatik ölçeklendirmeyi uygulayabilirsiniz. Otomatik ölçeklendirme örneği, daha fazla kapasiteyi işlemek için bir bulutta otomatik olarak ölçeklenen bir istemci bağlantısıdır, ancak uygulamanın diğer bağımlılıklarının de ölçeklendirilmesi için farklı bulutlara yayılmasına neden olur. Bu bağımlı bileşenlerin otomatik ölçeklendirme özellikleri bunun olmalıdır.

Otomatik ölçeklendirme kullanılamıyorsa, merkezi izleme sistemindeki eşiklerle tetiklenen el ile ölçeklendirmeyi karşılamak için betikleri ve diğer kaynakları uygulamayı düşünün.

**Beklenen konuma göre yüklemeyi belirleme.** İstemci isteklerini işleyen karma uygulamalar öncelikle tek bir konuma bağlı olabilirler. İstemci isteklerinin yükü bir eşiği aşarsa,

gelen isteklerin yükünü dağıtmak için ek kaynaklar farklı bir konuma eklenebilir. İstemci bağlantılarının artan yükleri işleyebilmesine ve ayrıca yükü işleyecek istemci bağlantılarına yönelik otomatikleştirilmiş yordamları belirleyebilmesini sağlayın.

## <a name="availability"></a>Erişilebilirlik

Kullanılabilirlik, sistemin işlevsel ve çalışır olduğu süredir. Kullanılabilirlik, çalışma süresinin yüzdesi olarak ölçülür. Uygulama hataları, altyapı sorunları ve sistem yükü, kullanılabilirliği azaltabilecek faktörlerdir.

Bu pilde temel tartışmak için bkz. yazılım kalitesindeki nesnelerin [*kullanılabilirliği*](https://docs.microsoft.com/azure/architecture/guide/pillars#availability) .

### <a name="availability-checklist"></a>Kullanılabilirlik denetim listesi

**Bağlantı için artıklık sağlayın.** Karma uygulamalar, uygulamanın yayıldığı bulutlar arasında bağlantı gerektirir. Karma bağlantı için bir teknoloji seçiminiz vardır. bu nedenle, birincil teknoloji seçimine ek olarak, otomatik yük devretme özellikleri ile artıklık sağlamak için başka bir teknoloji kullanın, birincil teknoloji başarısız olur.

**Hata etki alanlarını sınıflandırın.** Hataya dayanıklı uygulamalar birden çok hata etki alanı gerektirir. Hata etki alanları, tek bir sabit diskin şirket içinde başarısız olması, raf üstü bir anahtarın devre dışı olması veya tam veri merkezinin kullanılamaz olması gibi başarısızlık noktasını yalıtmaya yardımcı olur. Karma uygulamada, bir konum bir hata etki alanı olarak sınıflandırılabilirler. Daha fazla kullanılabilirlik gereksinimleriyle, tek bir hata etki alanının nasıl sınıflandırılacağını değerlendirmek için daha fazla ihtiyacınız vardır.

**Yükseltme etki alanlarını sınıflandırın.** Yükseltme etki alanları, uygulama bileşenleri örneklerinin kullanılabilir olmasını sağlamak için kullanılır, ancak aynı bileşene ait diğer örneklere güncelleştirmeler veya özellik yükseltmeleri ile bakım yapılıyor. Hata etki alanlarında olduğu gibi, yükseltme etki alanları konumlar arasında yerleştirme ile sınıflandırılabilirler. Bir uygulama bileşeninin başka bir konumda yükseltilmeden veya başka bir etki alanı yapılandırması gerekliyse, tek bir konumda yükseltilme işlemine uyum sağlayameyeceğini belirlemelisiniz. Tek bir konumun kendisi birden fazla yükseltme etki alanına sahip olabilir.

**Örnekleri ve kullanılabilirliği izleyin.** Yüksek oranda kullanılabilir uygulama bileşenleri, yük dengeleme ve zaman uyumlu veri çoğaltma aracılığıyla kullanılabilir. Hizmet kesintiye uğramadan önce kaç örnek çevrimdışı olabilir belirlemelisiniz.

**Kendini onaran uygulayın.** Bir sorun, uygulamanın kullanılabilirliğine karşı kesintiye neden oluyorsa, bir izleme sistemi tarafından yapılan bir algılama, başarısız örneği boşaltma ve yeniden dağıtma gibi uygulama için kendi kendini düzeltme etkinlikleri başlatabilir. Büyük olasılıkla bu, karma bir sürekli tümleştirme ve sürekli teslim (CI/CD) işlem hattı ile tümleştirilmiş bir merkezi izleme çözümü gerektirir. Uygulama bir uygulama bileşeninin yeniden dağıtımı gerektirebilecek sorunları belirlemek için bir izleme sistemiyle tümleşiktir. İzleme sistemi, aynı zamanda aynı veya diğer konumlarda uygulama bileşenini ve potansiyel olarak diğer bağımlı bileşenleri yeniden dağıtmak için karma CI/CD 'yi tetikleyebilirler.

**Hizmet düzeyi sözleşmelerini (SLA 'Lar) koruyun.** Müşterileriniz, müşterilerinizin sahip olduğu hizmet ve uygulamalarla bağlantı sağlamak için herhangi bir anlaşma açısından önemlidir. Karma uygulamanızın bağımlı olduğu her konumun kendi SLA 'Sı olabilir. Bu farklı SLA 'Lar karma uygulamanızın genel SLA 'sını etkileyebilir.

## <a name="resiliency"></a>Dayanıklılık

Dayanıklılık, karma uygulama ve sistemin hatalardan kurtulmasına ve çalışmaya devam etmesine olanak tanır. Dayanıklılığın hedefi, bir hatanın ardından uygulamayı tam çalışır duruma geri döndürmektir. Dayanıklılık stratejileri, yedekleme, çoğaltma ve olağanüstü durum kurtarma gibi çözümler içerir.

Bu pilde temel tartışmak için, yazılım kalitesinin pillerini anlamak için bkz. [*dayanıklılık*](https://docs.microsoft.com/azure/architecture/guide/pillars#resiliency) .

### <a name="resiliency-checklist"></a>Dayanıklılık denetim listesi

**Olağanüstü durum kurtarma bağımlılıklarını açın.** Bir bulutta olağanüstü durum kurtarma, başka bir buluttaki uygulama bileşenlerinde değişiklik yapılmasını gerektirebilir. Bir buluttaki bir veya birden çok bileşen, aynı bulutta ya da başka bir buluta yük devretmelidir, bağımlı bileşenlerin bu değişikliklerden haberdar olması gerekir. Bu, bağlantı bağımlılıklarını da içerir. Dayanıklılık, her bulut için tam olarak test edilmiş bir uygulama kurtarma planı gerektirir.

**Kurtarma akışı oluşturun.** Etkin bir kurtarma akışı tasarımı, arabelleğe alma, yeniden deneme, başarısız veri aktarımını yeniden deneme ve gerekirse farklı bir hizmete veya iş akışına geri dönme özelliklerine yönelik uygulama bileşenlerini değerlendirdi. Hangi yedekleme mekanizmasının kullanılacağını, geri yükleme yordamının ne sıklıkta ve ne sıklıkla test edildiğini belirlemelisiniz. Hem artımlı hem de tam yedeklemelerin sıklığını da belirlemelisiniz.

**Kısmi kurtarmaları test edin.** Uygulamanın bir bölümü için kısmi kurtarma, hepsi kullanılamaz durumda olan kullanıcılara daha fazla yol sağlayabilir. Planın bu bölümü, yedekleme yapılmadan önce düzgün bir şekilde kapatmak üzere uygulamayla etkileşim kuran yedekleme ve geri yükleme hizmeti gibi bir yan etkisi olmadığından emin olmalıdır.

**Olağanüstü durum kurtarma, sigortaların ve sorumluluğun atanmasının belirlenmesi.** Bir kurtarma planı, neleri yedeklebileceğinize ve geri yüklenebileceklerini ek olarak kimlerin ve hangi rollerin, yedekleme ve kurtarma eylemlerini başlatabileceğini betimlemelidir.

**Kendi kendini iyileştirme eşiklerini olağanüstü durum kurtarma ile karşılaştırın.** Otomatik Kurtarma başlatma için bir uygulamanın kendi kendini onaran yeteneklerini ve bir uygulamanın kendi kendini onaramasının başarısız veya başarılı olarak kabul edilmesi için gereken süreyi belirleme. Her bulutun eşiklerini belirleme.

**Dayanıklılık özelliklerinin kullanılabilirliğini doğrulayın.** Her konum için dayanıklılık özelliklerinin ve özelliklerinin kullanılabilirliğini belirleme. Bir konum gerekli özellikleri sağlamıyorsa, bu konumu dayanıklılık özelliklerini sağlayan merkezi bir hizmetle tümleştirmeyi düşünün.

**Altzamanları belirleme.** Uygulamanın tamamı ve uygulama bileşenleri olarak bakım nedeniyle beklenen kapalı kalma süresini belirleme.

**Belge sorunlarını giderme yordamları.** Kaynakları ve uygulama bileşenlerini yeniden dağıtmaya ilişkin sorun giderme yordamlarını tanımlayın.

## <a name="manageability"></a>Yönetilebilirlik

Karma uygulamalarınızı yönetmenin önemli noktaları, mimarinizi tasarlarken kritik öneme sahiptir. İyi yönetilen bir karma uygulama, ortak bir geliştirme ardışık düzeninde tutarlı uygulama kodu tümleştirmesini sağlayan kod olarak bir altyapı sağlar. Altyapıda yapılan değişikliklerin tutarlı sistem genelinde ve bireysel testlerini uygulayarak, değişiklikler testleri geçirirseniz, bunlar kaynak kodla birleştirilmelerine izin vererek, tümleşik bir dağıtımı güvence altına alabilirsiniz.

Bu pillere ilişkin temel bilgiler için, bkz. [*Yönetim ve DevOps*](https://docs.microsoft.com/azure/architecture/guide/pillars#management-and-devops) , yazılım kalitesine göre.

### <a name="manageability-checklist"></a>Yönetilebilirlik denetim listesi

**İzlemeyi uygulayın.** Bulut genelinde yayılan, sistem durumunun ve performansının toplu bir görünümünü sağlamak için bir merkezi izleme sistemi kullanın. Bu sistem, hem uygulama bileşenlerini hem de ilgili platform yeteneklerini izlemeyi içerir.

Uygulamanın izlenmesini gerektiren parçalarını saptayın.

**Koordinat ilkeleri.** Bir karma uygulamanın yaydığı her konum, izin verilen kaynak türlerini, adlandırma kurallarını, etiketleri ve diğer ölçütleri kapsayan kendi ilkesine sahip olabilir.

**Rolleri tanımlayın ve kullanın.** Bir veritabanı yöneticisi olarak, uygulama kaynaklarına erişmesi gereken farklı kişiler (bir uygulama sahibi, bir veritabanı yöneticisi ve son kullanıcı gibi) için gerekli izinleri belirlemeniz gerekir. Bu izinlerin kaynaklar üzerinde ve uygulama içinde yapılandırılması gerekir. Rol tabanlı erişim denetimi (RBAC) sistemi, uygulama kaynakları üzerinde bu izinleri ayarlamanıza olanak sağlar. Bu erişim hakları, tüm kaynaklar tek bir bulutta dağıtıldığında, ancak kaynaklar bulutlara yayıldığında daha fazla dikkat gerektirse de zorlayıcı bir işlem gerektirir. Bir bulutta ayarlanan kaynaklarla ilgili izinler, başka bir bulutta ayarlanan kaynaklar için geçerlidir.

**CI/CD işlem hatlarını kullanın.** Sürekli tümleştirme ve sürekli geliştirme (CI/CD) ardışık düzeni, bulutlar arasında yayılan uygulamaları yazma ve dağıtmaya yönelik tutarlı bir işlem sağlayabilir ve altyapı ve uygulamalarına yönelik kalite güvencesi sağlar. Bu işlem hattı, altyapı ve uygulamanın bir bulutta test yapılmasını ve başka bir buluta dağıtılmasını sağlar. İşlem hattı, karma uygulamanızın belirli bileşenlerini tek bir buluta ve diğer bileşenlere, karma uygulama dağıtımı için temel oluşturacak şekilde dağıtmanıza olanak tanır. Bir CI/CD sistemi, yükleme sırasında, veritabanı için bir bağlantı dizesi gerektiren Web uygulaması gibi, uygulama bileşenlerinin her biri için sahip olduğu bağımlılıkları işlemek için kritik öneme sahiptir.

**Yaşam döngüsünü yönetin.** Bir karma uygulamanın kaynakları konumlara yayılabildiğinden, her bir konumun yaşam döngüsü yönetim yeteneğinin tek bir yaşam döngüsü yönetim biriminde toplanması gerekir. Nasıl oluşturulduğunu, güncelleştirileceğini ve silindiğini göz önünde bulundurun.

**Sorun giderme stratejilerini inceleyin.** Karma uygulama sorunlarını giderme, tek bir bulutta çalışan aynı uygulamadan daha fazla uygulama bileşeni içerir. Bulutlar arasındaki bağlantının yanı sıra, uygulama bir yerine iki platformda çalışır. Karma uygulamalarda sorun gidermeye yönelik önemli bir görev, uygulama bileşenlerinin toplu sistem durumunu ve performans izlemesini incelemektir.

## <a name="security"></a>Güvenlik

Güvenlik, herhangi bir bulut uygulaması için başlıca önemli noktalara biridir ve karma bulut uygulamaları için daha fazla kritik hale gelir.

Bu pilde temel tartışmak için, yazılım kalitesindeki bir [*güvenlik*](https://docs.microsoft.com/azure/architecture/guide/pillars#security) bölümüne bakın.

### <a name="security-checklist"></a>Güvenlik denetim listesi

**İhlalin olduğunu varsayın.** Uygulamanın bir bölümü tehlikeye atılırsa, yalnızca aynı konumda değil, ancak konumlar arasında değil, ihlalin yayılmasını en aza indirmek için çözümler bulunduğundan emin olun.

**İzin verilen ağ erişimini izleyin.** Uygulamanın yalnızca belirli bir alt ağdan erişmesi gibi ağ erişim ilkelerini belirleme ve yalnızca uygulamanın düzgün çalışması için gereken bileşenler arasındaki en düşük bağlantı noktalarına ve protokollere izin verme.

**Güçlü kimlik doğrulaması yapın.** Güçlü bir kimlik doğrulama düzeni, uygulamanızın güvenliği için önemlidir. Çoklu oturum açma özellikleri sağlayan ve şu şemalarından birini veya birkaçını kullanan bir federal kimlik sağlayıcısı kullanmayı düşünün: Kullanıcı adı ve parola oturum açma, ortak ve özel anahtarlar, iki öğeli veya Multi-Factor Authentication ve güvenilen güvenlik gruplandıran. Sertifika türlerine ve gereksinimlerine ek olarak, uygulama kimlik doğrulaması için hassas verileri ve diğer gizli dizileri depolamak için uygun kaynakları belirleme.

**Şifrelemeyi kullanın.** Uygulamanın, veri depolama veya istemci iletişimi ve erişim gibi hangi alanlarda şifreleme kullanacağınızı belirler.

**Güvenli kanallar kullanın.** Bulutlar genelinde güvenli bir kanal, güvenlik ve kimlik doğrulaması denetimleri, gerçek zamanlı koruma, karantina ve bulutlar arasında diğer hizmetler sağlamak için önemlidir.

**Rolleri tanımlayın ve kullanın.** Bulut genelinde kaynak yapılandırmalarına ve tek kimlik erişimine yönelik rolleri uygulayın. Uygulama ve platform kaynakları için rol tabanlı erişim denetimi (RBAC) gereksinimlerini saptayın.

**Sisteminizi denetleyin.** Sistem izleme, hem uygulama bileşenlerinden hem de ilgili bulut platformu işlemlerinden verileri günlüğe kaydedebilir ve toplayabilir.

## <a name="summary"></a>Özet

Bu makalede, karma uygulamalarınızı yazma ve tasarlama sırasında göz önünde bulundurmanız gereken öğelerin bir denetim listesi sunulmaktadır. Uygulamanızı dağıtmadan önce bu şekilde bu gibi bir şekilde gözden geçirmek, üretim kesintileri ' nda bu sorulara çalışmanızı ve tasarımınızı yeniden ziyaret etmeniz gerektiğini önler.

Daha önce zaman alan bir görev gibi görünebilir, ancak uygulamanızı bu ve bunlara göre tasarlarsanız yatırımınızı kolayca alabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

-   [Karma bulut](https://azure.microsoft.com/overview/hybrid-cloud/)
-   [Karma bulut uygulamaları](https://azure.microsoft.com/solutions/hybrid-cloud-app/)
-   [Bulut tutarlılığı için Azure Resource Manager şablonları geliştirme](https://aka.ms/consistency)