---
title: Azure Stack dağıtılan VM 'Leri koruma | Microsoft Docs
description: Veri kaybına ve planlanmamış kapalı kalma süresine karşı Azure Stack dağıtılan VM 'Leri korumak için bir kurtarma planı oluşturma hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 4e5833cf-4790-4146-82d6-737975fb06ba
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: hectorl
ms.lastreviewed: 3/19/2018
ms.openlocfilehash: f633f000968fcf4f373fc502898fa18084f93f80
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71824299"
---
# <a name="protect-vms-deployed-on-azure-stack"></a>Azure Stack dağıtılan VM 'Leri koruma

Kullanıcılarınızın Azure Stack dağıtıldığı sanal makinelerin (VM 'Ler) korunması için bir plan geliştirmeye yönelik bir kılavuz olarak bu makaleyi kullanın.


Veri kaybına ve planlanmamış kapalı kalma süresine karşı korunmak için, kullanıcı uygulamaları ve bunların verileri için bir yedekleme kurtarma veya olağanüstü durum kurtarma planı uygulamanız gerekir. Bu plan her uygulama için benzersiz olabilir, ancak kuruluşunuzun kapsamlı iş sürekliliği ve olağanüstü durum kurtarma (BC/DR) stratejisine göre belirlenen bir çerçeveyi takip edebilir. İyi bir başlangıç noktası [Azure Stack: İş sürekliliği ve olağanüstü durum kurtarma için önemli noktalar @ no__t-0.

## <a name="azure-stack-infrastructure-recovery"></a>Azure Stack altyapı kurtarma

Kullanıcılar, VM 'lerinin Azure Stack altyapı hizmetlerinden ayrı olarak korunmasından sorumludur.

Azure Stack altyapı hizmetleri için kurtarma planı, Kullanıcı VM 'lerinin, depolama hesaplarının veya veritabanlarının **kurtarılmasını içermez.** Uygulama sahibi olarak, uygulamalarınız ve verileriniz için bir kurtarma planı uygulamaktan sorumlu olursunuz.

Azure Stack bulutu, uzatılmış bir süre veya kalıcı olarak kurtarılamaz durumda olduğunda, şu şekilde bir kurtarma planı yapmanız gerekir:

* En az kapalı kalma süresi sağlar.
* , Çalıştıran veritabanı sunucuları gibi kritik VM 'Leri tutar.
* Uygulamaların Kullanıcı isteklerine hizmet vermeye devam etmesini sağlar.

Azure Stack bulutun işleci, temel alınan Azure Stack altyapısı ve hizmetleri için bir kurtarma planı oluşturmaktan sorumludur. Daha fazla bilgi için bkz. çok [zararlı veri kaybından kurtarma](../operator/azure-stack-backup-recover-data.md).

## <a name="considerations-for-iaas-vms"></a>IaaS VM 'lerinin konuları
IaaS VM 'de yüklü işletim sistemi, içerdiği verileri korumak için hangi ürünlerin kullanılacağını sınırlandırır. Windows tabanlı IaaS sanal makineleri için, verileri korumak üzere Microsoft ve iş ortağı ürünlerini kullanabilirsiniz. Linux tabanlı IaaS VM 'Leri için tek seçenek iş ortağı ürünlerini kullanmaktır. [Azure Stack için doğrulanan ürünlerle tüm BC/Dr iş ortakları için bu veri sayfasına](https://aka.ms/azurestackbcdrpartners)bakın.

## <a name="sourcetarget-combinations"></a>Kaynak/hedef birleşimleri

Her Azure Stack bulutu tek bir veri merkezine dağıtılır. Uygulamalarınızı kurtarmak için ayrı bir ortam gerekir. Kurtarma ortamı, farklı bir veri merkezinde veya Azure genel bulutunda başka bir Azure Stack bulutu olabilir. Verileriniz ve veri gizliliği gereksinimleriniz, uygulamanız için kurtarma ortamını belirlemektir. Her uygulama için korumayı etkinleştirdiğinizde, her biri için belirli bir kurtarma seçeneği seçme esnekliğine sahip olursunuz. Bir abonelikte, verileri başka bir veri merkezine yedekleyen uygulamalara sahip olabilirsiniz. Başka bir abonelikte Azure genel bulutuna veri çoğaltabilirsiniz.

Her uygulama için hedefi tespit etmek üzere her bir uygulama için yedekleme kurtarma ve olağanüstü durum kurtarma stratejinizi planlayın. Kurtarma planı, kuruluşunuzun şirket içi ve proje tüketimine gereken depolama kapasitesini ortak bulutta düzgün şekilde boyutlandırmasını sağlar.

|  | Küresel Azure | CSP veri merkezine dağıtılan ve CSP tarafından çalıştırılan Azure Stack | Müşteri veri merkezine dağıtılan ve müşteri tarafından çalıştırılan Azure Stack |
|------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| **CSP veri merkezine dağıtılan ve CSP tarafından çalıştırılan Azure Stack** | Kullanıcı VM 'Leri, CSP işletilen Azure Stack dağıtılır.<br><br>Kullanıcı VM 'Leri yedeklemeden geri yüklenir veya doğrudan Azure 'a yük devredildi. | CSP, kendi veri merkezlerinde Azure Stack birincil ve ikincil örneklerini çalışır.<br><br>İki Azure Stack örneği arasında Kullanıcı VM 'Leri geri yüklendi veya yük devredildi. | CSP, birincil sitede Azure Stack çalışır.<br><br>Müşterinin veri merkezi, geri yükleme veya yük devretme hedefidir. |
| **Müşteri veri merkezine dağıtılan ve müşteri tarafından çalıştırılan Azure Stack** | Kullanıcı VM 'Leri, müşteri tarafından çalıştırılan Azure Stack dağıtılır.<br><br>Kullanıcı VM 'Leri yedeklemeden geri yüklenir veya doğrudan Azure 'a yük devredildi. | Müşteri, birincil sitede Azure Stack çalışır.<br><br>CSP 'nin veri merkezi, geri yükleme veya yük devretme hedefidir. | Müşteri, kendi veri merkezlerinde Azure Stack birincil ve ikincil örneklerini çalışır.<br><br>İki Azure Stack örneği arasında Kullanıcı VM 'Leri geri yüklendi veya yük devredildi. |

![Kaynak-hedef birleşimleri](media/azure-stack-manage-vm-backup/vm_backupdataflow_01.png)

## <a name="app-recovery-objectives"></a>Uygulama kurtarma amaçları

Kuruluşunuzun her bir uygulama için kabul edebildiği kesinti süresi ve veri kaybı miktarını belirleme. Kapalı kalma süresini ve veri kaybını azaltarak, kuruluşunuzda bir olağanüstü durum etkisini en aza indiren bir kurtarma planı oluşturabilirsiniz. Her uygulama için şunları göz önünde bulundurun:

 - **Kurtarma süresi hedefi (RTO)**  
RTO, bir uygulamanın bir olay sonrasında kullanılamadığı en fazla kabul edilebilir süredir. Örneğin, 90 dakikalık bir RTO, uygulamayı bir olağanüstü durum başlangıcından 90 dakika içinde çalışır duruma geri yükleyebilmeniz gerektiği anlamına gelir. Düşük bir RTO varsa, bölgesel kesintiye karşı korumak için ikinci bir dağıtımı sürekli bekleme üzerinde çalışır durumda tutabilirsiniz.
 - **Kurtarma noktası hedefi (RPO)**  
RPO, olağanüstü durum sırasında kabul edilebilir maksimum veri kaybı sayısıdır. Örneğin, her saat yedeklenen ve diğer veritabanlarına çoğaltma olmayan tek bir veritabanında verileri depoluseniz, bir saat veri kaybedebilirsiniz.

RTO ve RPO iş gereksinimleridir. Uygulamanın RTO ve RPO 'SU tanımlamak için bir risk değerlendirmesi gerçekleştirin.

Bir hatadan sonra uygulamanın geri yüklenmesi için geçen ortalama süre olan başka bir ölçüm, kurtarmanın (MTTR) **süresini ifade** eden bir süredir. MTTR, bir sistem için empırical değeridir. MTTR, RTO 'yı aşarsa sistemdeki bir hata, sistemin tanımlanan RTO içinde geri yüklenmesi mümkün olmadığından kabul edilemez bir iş kesintiye neden olur.

### <a name="backup-restore"></a>Yedekleme-geri yükleme

VM tabanlı uygulamalar için en yaygın koruma şeması, yedekleme yazılımını kullanmaktır. VM yedeklemesi genellikle işletim sistemini, işletim sistemi yapılandırmasını, uygulama ikili dosyalarını ve uygulama verilerini içerir. Yedeklemeler, birimlerin, disklerin veya tüm VM 'nin bir anlık görüntüsü alınarak oluşturulur. Azure Stack, Konuk işletim sistemi bağlamı içinden veya Azure Stack depolama ve işlem API 'Lerinden yedekleme esnekliği elde edersiniz. Azure Stack, hiper yönetici düzeyinde yedeklemeler almayı desteklemez.
 
![Backup-Restor](media/azure-stack-manage-vm-backup/vm_backupdataflow_03.png)

Uygulamayı kurtarmak için bir veya daha fazla VM 'nin aynı buluta veya yeni bir buluta geri yüklenmesi gerekir. Veri merkezinizde veya genel bulutta bir bulutu hedefleyebilirsiniz. Seçtiğiniz bulut tamamen denetiminizin içindedir ve veri gizliliğine ve bağımsız gereksinimlerinize göre yapılır.

 - RTO Saat cinsinden ölçülen kapalı kalma süresi
 - RPO Değişken veri kaybı (yedekleme sıklığına bağlı olarak)
 - Dağıtım topolojisi: Etkin/Pasif

#### <a name="planning-your-backup-strategy"></a>Yedekleme stratejinizi planlama

Yedekleme stratejinizi planlama ve ölçek gereksinimlerinin tanımlanması, korunması gereken sanal makine örneklerinin sayısını niceleyerek başlar. Bir ortamdaki tüm sunucular genelinde tüm sanal makinelerin yedeklenmesi ortak bir stratejidir. Ancak, Azure Stack ile yedeklenmesi gereken bazı VM 'Ler vardır. Örneğin, bir ölçek kümesindeki VM 'Ler, bazen bildirimde bulunulmadan ve gidebilen kısa ömürlü kaynaklar olarak değerlendirilir. Korunması gereken tüm dayanıklı veriler, veritabanı veya nesne deposu gibi ayrı bir depoda depolanır.

Azure Stack VM 'Leri yedeklemeye yönelik önemli noktalar:

 - **Kategoriye**
    - Kullanıcıların VM yedeklemesini kabul ettiği bir model düşünün.
    - Uygulamaların önceliğine veya işletmenin etkilerine bağlı olarak bir kurtarma hizmet düzeyi anlaşması (SLA) tanımlayın.
 - **Ölçeklendirme**
    - Çok sayıda yeni VM oluştururken (yedekleme gerekliyse) aşamalı yedeklemeleri göz önünde bulundurun.
    - Çözümdeki kaynak içeriğini en aza indirmek için yedekleme verilerini etkili bir şekilde yakalayıp iletebilen yedekleme ürünlerini değerlendirin.
    - Ortamdaki tüm VM 'lerde tam yedeklemeler gereksinimini en aza indirmek için artımlı veya değişiklik yedeklemeleri kullanarak yedekleme verilerini verimli bir şekilde depolayan yedekleme ürünlerini değerlendirin.
 - **Geri yükleme**
    - Yedekleme ürünleri sanal diskleri, var olan bir VM 'deki uygulama verilerini veya tüm VM kaynağını ve ilişkili sanal diskleri geri yükleyebilir. Gerekli geri yükleme düzeni, uygulamayı nasıl geri yüklemeyi planladığınıza bağlıdır. Örneğin, SQL Server 'ı bir şablondan yeniden dağıtmak ve ardından VM 'lerin tamamını veya VM kümesini geri yüklemek yerine veritabanlarını geri yüklemek daha kolay olabilir.

### <a name="replicationmanual-failover"></a>Çoğaltma/el ile yük devretme

Yüksek kullanılabilirliği desteklemeye yönelik alternatif yaklaşım, uygulama sanal makinelerinizi başka bir buluta çoğaltmanız ve el ile yük devretmeye bağımlıdır. İşletim sisteminin, uygulama ikililerinin ve uygulama verilerinin çoğaltılması VM düzeyinde veya Konuk işletim sistemi düzeyinde yapılabilir. Yük devretme, uygulamanın parçası olmayan ek yazılımlar kullanılarak yönetilir.

Bu yaklaşımla, uygulama bir bulutta dağıtılır ve sanal makinesi diğer buluta çoğaltılır. Yük devretme tetikleniyorsa, ikincil VM 'Lerin ikinci bulutta açık olması gerekir. Bazı senaryolarda, yük devretme VM 'Leri oluşturur ve disklere ekler. Özellikle belirli bir başlangıç sırası gerektiren çok katmanlı bir uygulamayla, bu işlemin tamamlanması uzun sürebilir. Uygulama hizmet isteklerine başlamaya başlamadan önce çalıştırılması gereken adımlar da olabilir.

![Çoğaltma-el ile yük devretme](media/azure-stack-manage-vm-backup/vm_backupdataflow_02.png)

 - RTO Dakikalar içinde ölçülen kapalı kalma süresi
 - RPO Değişken veri kaybı (çoğaltma sıklığına bağlı olarak)
 - Dağıtım topolojisi: Etkin/Pasif tek
 
### <a name="high-availabilityautomatic-failover"></a>Yüksek kullanılabilirlik/otomatik yük devretme

İşletmenizin yalnızca birkaç saniye veya dakika kapalı kalma süresini ve en az veri kaybını kabul edebildiği uygulamalarda, yüksek kullanılabilirliğe sahip bir yapılandırma değerlendirin. Yüksek kullanılabilirliğe sahip uygulamalar, hataları hızla ve otomatik olarak kurtarmak için tasarlanmıştır. Azure Stack altyapı, yerel donanım hataları için, fiziksel ağda iki raf anahtarı kullanarak yüksek kullanılabilirlik uygular. İşlem düzeyi hatalarında Azure Stack, ölçek biriminde birden çok düğüm kullanır. VM düzeyinde, düğüm hatalarının uygulamanızı kapatmamasından emin olmak için hata etki alanları ile birlikte ölçek kümelerini kullanabilirsiniz.

Ölçek kümeleri ile birlikte uygulamanızın yüksek kullanılabilirliği yerel olarak desteklemesi veya kümeleme yazılımının kullanımını desteklemesi gerekir. Örneğin Microsoft SQL Server, zaman uyumlu tamamlama modu kullanılarak veritabanları için yerel olarak yüksek kullanılabilirliği destekler. Ancak, yalnızca zaman uyumsuz çoğaltmayı desteklenebilir, bazı veri kaybı olur. Uygulamalar, kümeleme yazılımının uygulamanın otomatik yük devretmesini işlediği bir yük devretme kümesine de dağıtılabilir.

Bu yaklaşımı kullanarak, uygulama yalnızca bir bulutta etkindir, ancak yazılım birden fazla buluta dağıtılır. Diğer bulutlar, yük devretme tetiklendiğinde uygulamayı başlatmaya hazırlanarak tek başına modundadır.

 - RTO Saniyeler içinde ölçülen kapalı kalma süresi
 - RPO En az veri kaybı
 - Dağıtım topolojisi: Etkin/etkin tek başına

### <a name="fault-tolerance"></a>Hataya dayanıklılık

Azure Stack fiziksel artıklığı ve altyapı hizmeti kullanılabilirliği yalnızca disk, güç kaynağı, ağ bağlantı noktası veya düğüm gibi donanım düzeyindeki hatalara/hatalara karşı koruma sağlar. Ancak, uygulamanızın her zaman kullanılabilir olması ve herhangi bir veriyi hiçbir zaman kaybetmemesi gerekiyorsa, hata toleransını uygulamanızda yerel olarak uygulamanız veya hata toleransını sağlamak için ek yazılımlar kullanmanız gerekir.

İlk olarak, düğüm düzeyindeki hatalara karşı korunmak için uygulama VM 'lerinin ölçek kümeleri kullanılarak dağıtıldığından emin olmanız gerekir. Buluta karşı koruma sağlamak için, aynı uygulamanın zaten farklı bir buluta dağıtılması gerekir, böylece kesintiye uğramadan bakım isteklerine devam edebilir. Bu model genellikle etkin-etkin bir dağıtıma başvurulur.

Her Azure Stack bulutun birbirinden bağımsız olduğunu unutmayın, bu nedenle bulutlar her zaman bir altyapı perspektifinden etkin kabul edilir. Bu durumda, uygulamanın birden çok etkin örneği bir veya daha fazla etkin bulutla dağıtılır.

 - RTO Kesinti yok
 - RPO Veri kaybı yok
 - Dağıtım topolojisi: Etkin/etkin

### <a name="no-recovery"></a>Kurtarma yok

Ortamınızdaki bazı uygulamaların planlanmamış kapalı kalma süresi veya veri kaybına karşı korunması gerekebilir. Örneğin, geliştirme ve test için kullanılan VM 'Lerin genellikle kurtarılması gerekmez. Bu, bir uygulama veya belirli bir VM için koruma olmadan yapmanız karardır. Azure Stack, temel altyapıdan VM 'lerin yedeklemesini veya çoğaltılmasını sunmaz. Azure 'a benzer şekilde, aboneliklerinizin her birinde her bir VM için korumayı kabul etmeniz gerekir.

 - RTO Ilamayabilir
 - RPO Veri kaybını Tamam

## <a name="recommended-topologies"></a>Önerilen topolojiler

Azure Stack dağıtımınız için önemli noktalar:

|     | Öneri | Açıklamalar |
|-------------------------------------------------------------------------------------------------|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Veri merkezinizde zaten dağıtılan bir dış yedekleme hedefine VM 'Leri yedekleme/geri yükleme | Önerilen | Mevcut yedekleme altyapısının ve operasyonel yeteneklerin avantajlarından yararlanın. Ek VM örneklerini korumaya hazırlamak için yedekleme altyapısını boyutlandırdığınızdan emin olun. Yedekleme altyapısının kaynağınıza yakın olmadığından emin olun. VM 'Leri kaynak Azure Stack, ikincil bir Azure Stack örneğine veya Azure 'a geri yükleyebilirsiniz. |
| Azure Stack adanmış bir dış yedekleme hedefine VM 'Leri Yedekle/Geri Yükle | Önerilen | Yeni yedekleme altyapısı satın alabilir veya Azure Stack için adanmış yedekleme altyapısı sağlayabilirsiniz. Yedekleme altyapısının kaynağınıza yakın olmadığından emin olun. VM 'Leri kaynak Azure Stack, ikincil bir Azure Stack örneğine veya Azure 'a geri yükleyebilirsiniz. |
| VM 'Leri doğrudan genel Azure 'a veya güvenilir bir hizmet sağlayıcısına yedekleme/geri yükleme | Önerilen | Veri gizliliği ve yasal gereksinimlerinizi karşılayabilmeniz koşuluyla, yedeklemelerinizi küresel Azure 'da veya güvenilir bir hizmet sağlayıcısında saklayabilirsiniz. İdeal olarak, hizmet sağlayıcısı da Azure Stack çalışır, böylece işlem deneyiminden geri yükleme sırasında tutarlılığı alırsınız. |
| VM 'Leri ayrı bir Azure Stack örneğine çoğaltma/yük devretme | Önerilen | Yük devretme durumunda, genişletilmiş uygulama kapalı kalma süresini ortadan kaldırmak için ikinci bir Azure Stack buluta tam işlem yapmanız gerekir. |
| VM 'Leri doğrudan Azure 'a veya güvenilir bir hizmet sağlayıcısına çoğaltma/yük devretme | Önerilen | Veri gizliliği ve yasal gereksinimlerinizi karşılayabilmeniz koşuluyla verilerinizi Global Azure 'a veya güvenilir bir hizmet sağlayıcısına çoğaltabilirsiniz. İdeal olarak, hizmet sağlayıcının yük devretmeden sonra işlemsel deneyimle tutarlı olması için Azure Stack da çalışmaktadır. |
| Uygulama verilerinize sahip aynı Azure Stack buluta yedekleme hedefi dağıtın | Önerilmez | Yedeklemeleri aynı Azure Stack bulutu içinde saklamaktan kaçının. Bulutun planlanmamış kapalı kalma süresi, birincil verilerinize ve yedekleme verilerinize sahip olabilir. Bir yedekleme hedefini Sanal Gereç olarak dağıtmayı tercih ederseniz (yedekleme ve geri yükleme için en iyi duruma getirme amacıyla), tüm verilerin sürekli olarak bir dış yedekleme konumuna kopyalandığından emin olmanız gerekir. |
| Fiziksel yedekleme gereçini Azure Stack çözümünün yüklendiği rafa dağıtın | Desteklenmiyor | Şu anda, diğer herhangi bir cihazı özgün çözümün parçası olmayan raf anahtarlarının en üstüne bağlanamazsınız. |

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Stack üzerinde dağıtılan Kullanıcı sanal makinelerini korumaya yönelik genel yönergeler sunulmaktadır. Kullanıcı sanal makinelerini korumak için Azure hizmetlerini kullanma hakkında daha fazla bilgi için bkz:

 - [Azure Stack üzerinde dosya ve uygulamaları yedeklemek için Azure Backup kullanın](https://docs.microsoft.com/azure/backup/backup-mabs-files-applications-azure-stack)
 - [Azure Stack için Azure Backup Sunucusu desteği](https://docs.microsoft.com/azure/backup/ ) 
 - [Azure Stack için Azure Site Recovery desteği](https://docs.microsoft.com/azure/site-recovery/)  

Azure Stack üzerinde VM koruması sunan iş ortağı ürünleri hakkında daha fazla bilgi edinmek için [Azure Stack uygulamaları ve verileri koruma](https://azure.microsoft.com/blog/protecting-applications-and-data-on-azure-stack/)bölümüne bakın.
