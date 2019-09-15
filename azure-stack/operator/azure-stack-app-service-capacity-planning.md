---
title: Azure Stack App Service sunucu rolleri için kapasite planlaması | Microsoft Docs
description: Azure Stack App Service sunucu rollerinin kapasite planlaması hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: BryanLa
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2019
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 03/13/2019
ms.openlocfilehash: 80dc7bae2371025fba82531b08216606580176e1
ms.sourcegitcommit: 245a4054a52e54d5989d6148fbbe386e1b2aa49c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70975195"
---
# <a name="capacity-planning-for-app-service-server-roles-in-azure-stack"></a>Azure Stack App Service sunucu rolleri için kapasite planlaması

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack Azure App Service üretime hazırlamış bir dağıtımı ayarlamak için sistemin desteklemesini istediğiniz kapasiteyi planlamanız gerekir.  

Bu makalede, herhangi bir üretim dağıtımı için kullanmanız gereken en düşük bilgi işlem örneği sayısı ve işlem SKU 'Ları için rehberlik sunulmaktadır.

App Service kapasite stratejinizi, bu yönergeleri kullanarak planlayabilirsiniz.

| App Service sunucusu rolü | Önerilen minimum örnek sayısı | Önerilen işlem SKU 'SU|
| --- | --- | --- |
| Denetleyici | 2 | A1 |
| Ön Uç | 2 | A1 |
| Yönetim | 2 | A3 |
| Yayımcı | 2 | A1 |
| Web çalışanları-paylaşılan | 2 | A1 |
| Web çalışanları-adanmış | katman başına 2 | A1 |

## <a name="controller-role"></a>Denetleyici rolü

**Önerilen en az**: İki a1 standart örneği

Azure App Service denetleyicisi genellikle CPU, bellek ve ağ kaynaklarının düşük tüketimine sahiptir. Ancak, yüksek kullanılabilirlik için iki denetleyiciniz olmalıdır. İki denetleyici aynı zamanda izin verilen en fazla denetleyici sayısıdır. Dağıtım sırasında yükleyiciden ikinci Web siteleri denetleyicisi doğrudan oluşturabilirsiniz.

## <a name="front-end-role"></a>Ön uç rolü

**Önerilen en az**: İki a1 standart örneği

Ön uç rotaları Web çalışanı kullanılabilirliğine bağlı olarak Web çalışanlarına istek ister. Yüksek kullanılabilirlik için, birden fazla ön uç olmalıdır ve ikiden fazlasına sahip olabilirsiniz. Kapasite planlama amaçları doğrultusunda, her bir çekirdeğin saniyede yaklaşık 100 isteği işleyebileceğini göz önünde bulundurun.

## <a name="management-role"></a>Yönetim rolü

**Önerilen en az**: İki a3 standart örneği

Azure uygulaması klasik dağıtım modeli rolü, App Service Azure Resource Manager ve API uç noktaları, Portal uzantıları (yönetici, kiracı, Işlevler portalı) ve veri hizmeti 'nden sorumludur. Yönetim sunucusu rolü genellikle üretim ortamında yalnızca 4 GB RAM gerektirir. Ancak, çok sayıda yönetim görevi (örneğin, Web sitesi oluşturma) gerçekleştirildiğinde yüksek CPU düzeylerine sahip olabilir. Yüksek kullanılabilirlik için, bu role atanmış birden fazla sunucunuz ve sunucu başına en az iki çekirdek olmalıdır.

## <a name="publisher-role"></a>Yayımcı rolü

**Önerilen en az**: İki a1 standart örneği

Birçok kullanıcı aynı anda yayımlıyorsa, Yayımcı rolü ağır CPU kullanımı ile karşılaşabilir. Yüksek kullanılabilirlik için, birden fazla yayımcı rolünün kullanılabilir olduğundan emin olun. Yayımcı yalnızca FTP/FTPS trafiğini işler.

## <a name="web-worker-role"></a>Web çalışanı rolü

**Önerilen en az**: İki a1 standart örneği

Yüksek kullanılabilirlik için, en az dört Web çalışanı rolüne sahip olmanız gerekir: bir paylaşılan Web sitesi modu için iki ve sunmayı planladığınız her adanmış çalışan katmanı için iki. Paylaşılan ve adanmış işlem modları kiracılar için farklı hizmet düzeyleri sağlar. Müşterilerinizin birçoğu varsa daha fazla Web çalışanı gerekebilir:

- Adanmış işlem modu çalışan katmanlarını (Kaynak yoğunluklu) kullanma.
- Paylaşılan işlem modunda çalışıyor.

Bir Kullanıcı adanmış bir işlem modu SKU 'SU için App Service planı oluşturduktan sonra, bu App Service planında belirtilen Web çalışanı sayısı artık kullanıcılar tarafından kullanılamaz.

Tüketim planı modelinde kullanıcılara Azure Işlevleri sağlamak için paylaşılan Web çalışanları dağıtmanız gerekir.

Kullanılacak paylaşılan Web çalışanı rollerinin sayısına karar verirken, bu hususları gözden geçirin:

- **Bellek**: Bellek, bir Web çalışanı rolü için en kritik kaynaktır. Yetersiz bellek, sanal bellek diskten takas edildiğinde Web sitesi performansını etkiler. Her sunucu için işletim sistemi için 1,2 GB RAM gerekir. Bu eşiğin üzerindeki RAM, Web sitelerini çalıştırmak için kullanılabilir.
- **Etkin Web sitelerinin yüzdesi**: Genellikle, Azure Stack dağıtımında bir Azure App Service uygulamaların yüzde 5 ' i etkin. Ancak, belirli bir anda etkin olan uygulamaların yüzdesi daha yüksek veya daha düşük olabilir. Etkin uygulama oranı yüzde 5 oranında, Azure Stack dağıtımda bir Azure App Service yerleştirilecek en fazla uygulama sayısı, etkin Web siteleri sayısının 20 katından (5 x 20 = 100) daha az olmalıdır.
- **Ortalama bellek ayak izi**: Üretim ortamlarında gözlenen uygulamalar için Ortalama bellek ayak izi 70 MB 'dir. Bu ayak izi kullanarak, tüm Web çalışanı rolü bilgisayarlarında veya VM 'lerde ayrılan bellek aşağıdaki şekilde hesaplanır:

   `Number of provisioned applications * 70 MB * 5% - (number of web worker roles * 1044 MB)`

   Örneğin, 10 Web çalışanı rolü çalıştıran bir ortamda 5.000 uygulama varsa, her Web çalışanı rolü sanal makinesi 7060 MB RAM 'e sahip olmalıdır:

   `5,000 * 70 * 0.05 - (10 * 1044) = 7060 (= about 7 GB)`

   Daha fazla çalışan örneği ekleme hakkında bilgi için bkz. [diğer çalışan rolleri ekleme](azure-stack-app-service-add-worker-roles.md).

### <a name="additional-considerations-for-dedicated-workers-during-upgrade-and-maintenance"></a>Yükseltme ve bakım sırasında adanmış çalışanlar için ek konular

Çalışanların yükseltilmesi ve bakımı sırasında Azure Stack Azure App Service, her bir çalışan katmanının her bir zamanda% 20 ' si üzerinde bakım yapar.  Bu nedenle, bulut yöneticileri, kiracıların yükseltme ve bakım sırasında hiçbir hizmet kaybı yaşamamasını sağlamak için her zaman çalışan katmanı başına% 20 ' nin ayrılmamış çalışan havuzunu korumalıdır.  Örneğin, bir çalışan katmanında 10 çalışanınız varsa, 2 ' nin yükseltme ve bakım yapmasına izin vermek için ayrılmalarından emin olmanız gerekir. Tam 10 çalışan ayrıldıysanız, bir ayrılmamış çalışan havuzunu korumak için çalışan katmanını ölçeklendirmelisiniz. 

Yükseltme ve bakım sırasında Azure App Service, iş yüklerinin çalışmaya devam edebilmesini sağlamak için iş yüklerini ayrılmamış çalışanlara taşıyacaktır. Bununla birlikte, yükseltme sırasında kullanılabilir ayrılmamış çalışan yoksa, kiracı iş yükü kapalı kalma süresi de vardır. Paylaşılan çalışanlara göre, müşteriler, kiracı uygulamalarını otomatik olarak kullanılabilir çalışanlar içinde ayırabilecek şekilde müşterilerin ek çalışanlar sağlaması gerekmez. Yüksek kullanılabilirlik için, bu katmanda iki çalışan olması gereken en düşük gereksinimdir.

Bulut yöneticileri, Azure Stack yönetici portalındaki App Service yönetici alanında çalışan katmanı ayırmasını izleyebilir. App Service gidin ve sol bölmedeki çalışan katmanları ' nı seçin. Çalışan katmanları tablosu, çalışan katmanı adını, boyutunu, kullanılan resmi, kullanılabilir çalışan sayısını (ayrılmamış), her katmanda toplam çalışan sayısını ve çalışan katmanının genel durumunu gösterir.

![App Service yönetimi-çalışan katmanları][1]

## <a name="file-server-role"></a>Dosya sunucusu rolü

Dosya sunucusu rolü için, geliştirme ve test için tek başına bir dosya sunucusu kullanabilirsiniz. Örneğin, Azure Stack Geliştirme Seti Azure App Service (ASDK) dağıttığınızda, bu [şablonu](https://aka.ms/appsvconmasdkfstemplate)kullanabilirsiniz.  Üretim amacıyla, önceden yapılandırılmış bir Windows dosya sunucusu veya önceden yapılandırılmış Windows olmayan bir dosya sunucusu kullanmanız gerekir.

Üretim ortamlarında dosya sunucusu rolü yoğun disk g/ç ile karşılaşır. Kullanıcı Web siteleri için tüm içerik ve uygulama dosyalarını barındırıldığı için, bu rol için aşağıdaki kaynaklardan birini önceden oluşturmanız gerekir:

- Windows dosya sunucusu
- Windows dosya sunucusu kümesi
- Windows dışı dosya sunucusu
- Windows dışı dosya sunucusu kümesi
- NAS (ağa bağlı depolama) cihaz

Daha fazla bilgi için bkz. [dosya sunucusu sağlama](azure-stack-app-service-before-you-get-started.md#prepare-the-file-server).

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack App Service dağıtmaya yönelik önkoşullar](azure-stack-app-service-before-you-get-started.md)

<!--Image references-->
[1]: ./media/azure-stack-app-service-capacity-planning/worker-tier-allocation.png