---
title: Azure Stack kapasite Planlayıcısı | Microsoft Docs
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
ms.openlocfilehash: aec1d6fab044c731501433e6578257e8b09570eb
ms.sourcegitcommit: 80775f5c5235147ae730dfc7e896675a9a79cdbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2019
ms.locfileid: "66461008"
---
# <a name="azure-stack-capacity-planner"></a>Azure Stack kapasite Planlayıcısı

Azure Stack Capacity Planner bilgi işlem kaynaklarının nasıl farklı ayırmaları gösteren bir elektronik donanım tekliflerin arasında seçim uygundur. 

## <a name="worksheet-descriptions"></a>Çalışma sayfası açıklamaları
Aşağıdaki tabloda her çalışma sayfası için bir açıklama indirilebileceğini Azure Stack Capacity Planner sahip [ https://aka.ms/azstackcapacityplanner ](https://aka.ms/azstackcapacityplanner). 

|Sekme adı|Açıklama|
|-----|-----|
|Sürüm sorumluluk reddi|Hesaplayıcı, sürüm numarasını ve yayın tarihi amacı.|
|Yönergeler|Adım adım yönergeler modeli kapasiteye Vm'leri koleksiyonu için planlama.|
|DefinedSolutionSKUs|En fazla beş donanım tanımları içeren tablo. Girişleri verilebilir. Sistem yapılandırmaları çizgisinin eşleştirmek için ayrıntıları değiştirin.|
|DefineByVMFootprint|SKU uygun donanım yapılandırmaları farklı boyut ve VM'lerin miktarları ile karşılaştırarak bulun.|
|DefineByWorkloadFootprint|Azure Stack iş yükleri koleksiyonu oluşturarak, uygun donanım SKU bulun.|
|  |  |

## <a name="definedsolutionskus-instructions"></a>DefinedSolutionSKUs yönergeleri
Bu çalışma, en fazla beş donanım tanım örnekler içerir. Sistem yapılandırmaları çizgisinin eşleşecek şekilde değiştirin.

### <a name="hardware-selections-provided-by-authorized-hardware-partners"></a>Yetkili donanım iş ortakları tarafından sağlanan donanım seçimleri
Azure Stack, tümleşik bir sistem çözüm iş ortakları tarafından yüklü yazılım ile gönderilir. Çözüm iş ortakları, Azure Stack kapasite planlama araçları yetkili kendi sürümleri sağlar. Son çözüm kapasite tartışmalar için bu araçları kullanın.

### <a name="multiple-ways-to-model-computing-resources"></a>Model bilgi işlem kaynaklarını birden fazla yolu
Azure Stack planner içinde modelleme kaynak Azure Stack Vm'leri üzerinde çeşitli boyutlarda bağlıdır. Vm'leri aralık boyutu en büyük Standard_Fsv2 kadar küçük temel 0. Bilgi işlem kaynak ayırmalar iki farklı şekillerde modelleyebilir:

- Belirli donanım teklifi seçin ve hangi birleşimleri çeşitli kaynaklardan bilgi. 

- Belirli bir birleşimini VM ayırma oluşturun ve Azure kaynak kullanılabilir donanım SKU'ları bu VM yapılandırmasına destekleyebilen gösteren hesaplayıcısı'na izin verin.

Bu araç VM kaynaklarını ayırma için iki yöntem sunar: olarak veya tek bir VM'nin kaynak ayırmalar koleksiyonunu en fazla altı farklı iş yükü yapılandırmaları topluluğudur. Her iş yükü yapılandırması farklı bir ayırma kullanılabilir VM kaynakları içerebilir. Sonraki bölümlerde oluşturmak ve bu ayırma modellerinin her biri kullanmak için adım adım yönergeler vardır. Arka planda olmayan yer alan değerler yalnızca gölgeli hücreler veya bu çalışma sayfasındaki SKU aşağı açılır liste içinde değiştirilmesi gerekir. Gölgeli hücreler içinde yapılan değişiklikler kaynak hesaplamalar kesilebilir.


## <a name="definebyvmfootprint-instructions"></a>DefineByVMFootprint yönergeleri
Çeşitli boyutlarda ve Vm'leri miktarlarını tek bir koleksiyon kullanarak bir model oluşturmak için "DefineByVMFootprint" sekmesini seçin ve aşağıdaki adımları izleyin:

1. Bu çalışma sayfasının sağ alt köşesinde, sağlanan aşağı açılır liste kutusu denetimleri bir ilk sayı seçmek için kullanın. (arasında 4 ile 16) sunucularının her donanım sisteminin (SKU) yüklenmesini istediğiniz. Bu sunucu sayısını, bu genel nasıl etkilediğini görmek için model oluşturma işlemi sırasında herhangi bir zamanda değiştirilebilir, kaynak dağıtımı modeli için kullanılabilir kaynaklar.
2. Belirli bir donanıma karşı çeşitli VM kaynak ayırmalar model istiyorsanız, sayfanın sağ üst köşesindeki mavi aşağı açılır liste kutusunun hemen altındaki "Geçerli SKU" etiketi bulun. Bu liste kutusu çekme ve istenen donanımınız SKU'ı seçin.
3. Artık çeşitli boyutta VM'ler modelinize eklemeye başlamak hazırsınız. Belirli bir VM türüne eklemek için VM giriş solunda mavi anahatları belirlenmiş kutuya bir miktar değeri girin.

   > [!NOTE]
   > Toplam VM depolama VM veri diskinin toplam kapasiteyi gösterir (desteklenen disk sayısı * (1 TB) tek bir disk maksimum kapasitesini). Yapılandırma göstergeleri temel alan konusu her Azure Stack VM için depolama kaynağı istenen düzeyine seçebilir böylece biz kullanılabilir depolama yapılandırmaları tablo doldurulmuş. Ancak, ekleyebilir veya kullanılabilir depolama yapılandırmaları tablo gerektiği şekilde değiştirmeniz gerektiğini unutmayın.<br><br>Her sanal makine başlangıçta atanan bir yerel geçici depolama ile başlar. Ölçülü kaynak sağlama geçici depolama yansıtacak şekilde yerel temp sayısı ve açılan menüden izin verilen maksimum geçici depolama alanı miktarı gibi bir şeye değiştirilebilir.

4. Vm'leri eklediğinizde, değiştirme kullanılabilen SKU kaynakları gösteren grafikler görürsünüz. Bu, çeşitli boyutlarda ve Vm'leri miktarlarını modelleme işlemi sırasında ekleme etkilerini görmenize olanak sağlar. Değişikliklerin etkisini görüntülemenin bir başka yolu doğrudan kullanılabilir sanal makinelerin listesini listelenen tüketilen ve hala kullanılabilir numaraları izleyin sağlamaktır. Bu sayı şu anda seçili donanımda SKU göre tahmini değerleri yansıtır.
5. Vm'leri kümeniz oluşturulduktan sonra önerilen donanım SKU doğrudan "Geçerli SKU" etiketi altında sayfanın sağ üst köşesinde bulunan "Önerilen SKU" düğmesine tıklayarak bulabilirsiniz. Bu düğmeyi kullanarak, daha sonra sanal makine yapılandırmalarınızı değiştirebilir ve her yapılandırma, donanım desteklediğini görün.


## <a name="definebyworkloadfootprint-instructions"></a>DefineByWorkloadFootprint yönergeleri
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
