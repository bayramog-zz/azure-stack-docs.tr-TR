---
title: Azure ve Azure Stack hub 'ı kullanarak analiz çözümü için katmanlı verileri uygulamaya yönelik bir model.
description: Azure ve Azure Stack Hub hizmetlerini kullanarak hibrit bulut genelinde katmanlı veri çözümü uygulama hakkında bilgi edinin.
author: BryanLa
ms.service: azure-stack
ms.topic: article
ms.date: 11/05/2019
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 11/05/2019
ms.openlocfilehash: ac2c573e9ee1a2dad3afcdf86c9a6c273fb0f4e5
ms.sourcegitcommit: 5c92a669007ab4aaffe4484f1d8836a40340dde1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73638315"
---
# <a name="tiered-data-for-analytics-pattern"></a>Analiz deseninin katmanlı verileri

Bu düzende, birden çok şirket içi ve bulut konumunda verileri hazırlamak, analiz etmek, işlemek, silmek ve depolamak için Azure Stack hub ve Azure 'ın nasıl kullanılacağı gösterilmektedir.

## <a name="context-and-problem"></a>Bağlam ve sorun

Modern teknolojideki kurumsal kuruluşlara yönelik sorunlardan biri, güvenli veri depolama, işleme ve çözümleme konularını ele taşıyor. Şunlar arasında dikkat edilecek noktalar:
- veri içeriği
- location
- Güvenlik ve gizlilik gereksinimleri
- erişim izinleri
- yapılması
- depolama alanı depolaması

Azure, Azure Stack hub ile birlikte, veri kaygılarını adresleyen ve düşük maliyetli çözümler sunmaktadır. Bu çözüm, dağıtılmış bir üretim veya lojistik şirket aracılığıyla en iyi şekilde ifade edilir. 

Çözüm aşağıdaki senaryoya dayalıdır:
- Büyük bir çok dallı üretim organizasyonu.
- Hızlı ve güvenli veri depolama, işleme ve genel uzak konumlar arasında dağıtım, merkezi Merkez gereklidir. 
- Çalışan ve makine etkinliği, Tesis bilgileri ve güvenli kalması gereken iş raporlama verileri. Verilerin uygun şekilde dağıtılması ve bölgesel uyumluluk ilkesini ve sektör düzenlemelerine uyması gerekir.

## <a name="solution"></a>Çözüm

Şirket içi ve genel bulut ortamlarının kullanıldığı, çok tesisli kuruluşların taleplerini karşılar. Azure Stack hub, yerel ve uzak verilerin toplanması, işlenmesi, depolanması ve dağıtılması için hızlı, güvenli ve esnek bir çözüm sunar. Özellikle güvenlik, gizlilik, kurumsal ilke ve yasal gereksinimler, konumlar ve kullanıcılar arasında farklılık gösterebilir. 

![Analytics çözüm mimarisi için katmanlı veriler](media/pattern-tiered-data-analytics/solution-architecture.png)

## <a name="components"></a>Bileşenler

Bu çözüm aşağıdaki bileşenleri kullanır:

| Katmanı | Bileşen | Açıklama |
|----------|-----------|-------------|
| Azure | Depolama | [Azure depolama](/azure/storage/) hesabı, bir sterile veri tüketimi uç noktası sağlar. Azure Depolama, Microsoft’un modern veri depolama senaryolarına yönelik bulut depolama çözümüdür. Azure depolama, veri nesneleri için yüksek düzeyde ölçeklenebilir bir nesne deposu ve bulut için bir dosya sistemi hizmeti sunar. Ayrıca, güvenilir mesajlaşma ve NoSQL Mağazası için bir mesajlaşma deposu sağlar. |
| Azure Stack hub 'ı | Depolama | Birden çok hizmet için [Azure Stack hub depolama](/azure-stack/user/azure-stack-storage-overview) hesabı kullanılır:<br>ham veri depolama için **BLOB depolama** - . BLOB depolama, bir belge, medya dosyası veya uygulama yükleyicisi gibi herhangi bir tür metin veya ikili veri içerebilir. Her blob bir kapsayıcı altında düzenlenir. Kapsayıcılar, nesne gruplarına güvenlik ilkeleri atamak için kullanışlı bir yol sağlar. Depolama hesabı herhangi bir sayıda kapsayıcı içerebilir ve bir kapsayıcı depolama hesabının 500 TB 'lik kapasite sınırına kadar herhangi bir sayıda blob içerebilir.<br>veri arşivi için **BLOB depolama** - . Seyrek Erişimli veri arşivleme için düşük maliyetli depolama avantajları vardır. Seyrek Erişimli verilerin örnekleri arasında yedeklemeler, medya içeriği, bilimsel veriler, uyumluluk ve arşiv verileri sayılabilir. Genel olarak, seyrek erişilen tüm veriler Cool Storage olarak değerlendirilir. erişim sıklığı ve Bekletme dönemi gibi özniteliklere göre verileri katmanlama. Müşteri verilerine seyrek erişilebilir ancak sık erişimli veriler için benzer gecikme ve performans gerekir.<br>işlenen veri depolama için **kuyruk depolama** - . Kuyruk depolama, uygulama bileşenleri arasında bulut mesajlaşmasını sağlar. Uygulamaları ölçeklendirmek için tasarlarken, uygulama bileşenleri genellikle birbirinden bağımsız olarak ölçeklendirilebilen şekilde ayrılır. Kuyruk depolama, uygulama bileşenleri arasındaki iletişim için zaman uyumsuz mesajlaşma sağlar.  Bulutta, masaüstünde, şirket içi sunucuda veya mobil bir cihazda çalışıp çalışmadığını belirtir. Kuyruk depolama ayrıca zaman uyumsuz görevlerin yönetilmesini ve süreç iş akışlarının oluşturulmasını destekler. |
| | Azure İşlevleri | [Azure işlevleri](/azure/azure-functions/) hizmeti, [Azure App Service Azure Stack hub](/azure-stack/operator/azure-stack-app-service-overview) kaynak sağlayıcısı tarafından sağlanır. Azure Işlevleri, çeşitli olaylara yanıt olarak betik veya kod çalıştırmak için kodunuzu basit, sunucusuz bir ortamda çalıştırmanızı sağlar. Azure Işlevleri, tercih ettiğiniz programlama dilini kullanarak bir VM oluşturmak veya bir Web uygulaması yayımlamak zorunda kalmadan talebe uyacak şekilde ölçeklendirilir. İşlevleri için çözüm tarafından kullanılır:<br>- **veri alma**<br>**veri sterilmesini - .** El ile tetiklenen işlevler, zamanlanmış veri işlemeyi gerçekleştirebilir, temizleyebilir ve arşivlenebilir. Örnekler, gecelik müşteri listesi itilen ve aylık rapor işleme içerebilir.|

## <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler

Bu çözümü nasıl uygulayacağınıza karar verirken aşağıdaki noktaları göz önünde bulundurun:

### <a name="scalability"></a>Ölçeklenebilirlik 

Veri hacmi ve işleme taleplerini karşılamak için Azure işlevleri ve depolama çözümleri ölçeği. Azure ölçeklenebilirlik bilgileri ve hedefleri için bkz. [Azure Storage ölçeklenebilirlik belgeleri](/azure/storage/common/storage-scalability-targets). 

### <a name="availability"></a>Erişilebilirlik

Depolama, bu model için birincil kullanılabilirlik açısından önemlidir. Büyük veri hacmi işleme ve dağıtımı için hızlı bağlantılar aracılığıyla bağlantı gereklidir. 

### <a name="manageability"></a>Yönetilebilirlik

Bu çözümün yönetilebilirlik, kaynak denetimi ve görevlendirmede kullanılan yazma araçlarına bağlıdır. 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede tanıtılan konular hakkında daha fazla bilgi edinmek için:
- Bkz. [Azure depolama](/azure/storage/) ve [Azure işlevleri](/azure/azure-functions/) belgeleri. Bu model, Azure depolama hesapları ve Azure Işlevleri için hem Azure hem de Azure Stack hub 'ında çok fazla kullanım sağlar.
- En iyi uygulamalar hakkında daha fazla bilgi edinmek ve ek soruları yanıtlamak için bkz. [karma uygulama tasarımı konuları](overview-app-design-considerations.md) .
- Ürünlerin ve çözümlerin tamamı hakkında daha fazla bilgi edinmek için [ürün ve çözümlerin Azure Stack ailesine](/azure-stack)bakın.

Çözüm örneğini test etmeye hazırsanız, [analiz çözümü Dağıtım Kılavuzu ' na yönelik katmanlı veriler](https://aka.ms/tiereddatadeploy)' e geçin. Dağıtım Kılavuzu, bileşenlerinin dağıtılması ve test edilmesi için adım adım yönergeler sağlar.