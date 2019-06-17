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
ms.openlocfilehash: 98dafb9df568e5e14e5f1890103d0261e07beb98
ms.sourcegitcommit: b79a6ec12641d258b9f199da0a35365898ae55ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67131363"
---
# <a name="azure-stack-capacity-planner"></a>Azure Stack kapasite Planlayıcısı

Azure Stack Capacity Planner bilgi işlem kaynaklarının nasıl farklı ayırmaları gösteren bir elektronik donanım tekliflerin arasında seçim uygundur. 

## <a name="worksheet-descriptions"></a>Çalışma sayfası açıklamaları
Aşağıdaki tabloda her çalışma sayfasında indirilebileceğini Azure Stack Capacity Planner açıklanmaktadır [ https://aka.ms/azstackcapacityplanner ](https://aka.ms/azstackcapacityplanner). 

|Çalışma sayfası adı|Açıklama|
|-----|-----|
|Sürüm sorumluluk reddi|Hesaplayıcı, sürüm numarasını ve yayın tarihi amacı.|
|Yönergeler|Model kapasite planlama sanal makineleri (VM'ler) koleksiyonu için adım adım yönergeler.|
|DefinedSolutionSKUs|En fazla beş donanım tanımları içeren tablo. Girişleri verilebilir. Sistem yapılandırmaları çizgisinin eşleştirmek için ayrıntıları değiştirin.|
|DefineByVMFootprint|SKU uygun donanım yapılandırmaları farklı boyut ve VM'lerin miktarları ile karşılaştırarak bulun.|
|DefineByWorkloadFootprint|Azure Stack iş yükleri koleksiyonu oluşturarak, uygun donanım SKU bulun.|
|  |  |

## <a name="definedsolutionskus-instructions"></a>DefinedSolutionSKUs yönergeleri
Bu çalışma, en fazla beş donanım tanım örnekler içerir. Sistem yapılandırmaları çizgisinin eşleşecek şekilde değiştirin.

### <a name="hardware-selections-provided-by-authorized-hardware-partners"></a>Yetkili donanım iş ortakları tarafından sağlanan donanım seçimleri
Azure Stack, tümleşik bir sistem çözüm iş ortakları tarafından yüklü yazılım ile gönderilir. Çözüm iş ortakları, Azure Stack kapasite planlama araçları yetkili kendi sürümleri sağlar. Son çözüm kapasite tartışmalar için bu araçları kullanın.

### <a name="multiple-ways-to-model-computing-resources"></a>Model bilgi işlem kaynaklarını birden fazla yolu
Azure Stack Capacity Planner içinde modelleme kaynak Azure Stack Vm'leri üzerinde çeşitli boyutlarda bağlıdır. Vm'leri aralık boyutu en büyük Standard_Fsv2 kadar küçük, temel 0. Bilgi işlem kaynak ayırmalar iki farklı şekillerde modelleyebilir:

- Teklif, belirli bir donanım seçin ve hangi birleşimleri çeşitli kaynaklardan bilgi. 

- Belirli bir birleşim VM ayırma oluşturun ve Azure kaynak kullanılabilir donanım SKU'ları bu VM yapılandırmasına destekleyebilen gösteren hesaplayıcı olanak tanır.

Bu araç VM kaynaklarını ayırma için iki yöntem sunar: tek bir VM'nin kaynak ayırmalar koleksiyonunu veya en fazla altı farklı iş yükü yapılandırmaları topluluğudur. Her iş yükü yapılandırması farklı bir ayırma kullanılabilir VM kaynakları içerebilir. Sonraki bölümlerde oluşturmak ve bu ayırma modellerinin her biri kullanmak için adım adım yönergeler vardır. Arka plan olmayan gölgeli hücreler veya bu çalışma sayfasındaki SKU aşağı açılır liste içinde bulunan değerler yalnızca değiştirilmesi gerekir. Gölgeli hücreler içinde yapılan değişiklikler kaynak hesaplamalar bozulabilir.


## <a name="definebyvmfootprint-instructions"></a>DefineByVMFootprint yönergeleri
Çeşitli boyutlarda ve Vm'leri miktarlarını tek bir koleksiyon kullanarak bir model oluşturmak için Seç **DefineByVMFootprint** sekmesini tıklatın ve aşağıdaki adımları izleyin:

1. Bu çalışma sayfasının sağ alt köşesinde, sağlanan aşağı açılır liste kutusu denetimleri her donanım sisteminin (SKU) yüklenmesini istediğiniz sunucu (4 ile 16 arasında) bir ilk sayısını seçmek için kullanın. Bu sunucu sayısını, bu genel nasıl etkilediğini görmek için model oluşturma işlemi sırasında herhangi bir zamanda değiştirilebilir, kaynak dağıtımı modeli için kullanılabilir kaynaklar.
2. Belirli bir donanıma karşı çeşitli VM kaynak ayırmalar model istiyorsanız, doğrudan aşağıdaki mavi aşağı açılır liste kutusunda Bul **geçerli SKU** sayfanın sağ üst köşesindeki etiketi. Bu liste kutusu çekin ve istenen donanımınız SKU seçin.
3. Artık çeşitli boyutta VM'ler modelinize eklemeye başlamak hazırsınız. Belirli bir VM türüne eklemek için VM giriş solunda mavi anahatları belirlenmiş kutuya bir miktar değeri girin.

   > [!NOTE]
   > Toplam VM depolama alanı, Toplam Kapasite (tek bir diske [1 TB] tarafından hizmetin maksimum kapasitesi çarpılan desteklenen disk sayısı) VM veri diskinin ifade eder. Her Azure Stack VM için depolama kaynağı istenen düzeyine seçebilmeniz yapılandırma göstergeleri temel alan, biz kullanılabilir depolama yapılandırmaları tablo doldurduktan. Ancak, not ekleyin veya kullanılabilir depolama yapılandırmaları tablo gerektiği şekilde değiştirmek önemlidir.<br><br>Her sanal makine başlangıçta atanan bir yerel geçici depolama ile başlar. Ölçülü kaynak sağlama geçici depolama yansıtmak için açılan menüde izin verilen maksimum geçici depolama alanı miktarı dahil olmak üzere herhangi bir şey için yerel temp sayısını değiştirebilirsiniz.

4. Vm'leri eklediğinizde, değiştirme kullanılabilen SKU kaynakları gösteren grafikler görürsünüz. Bu, çeşitli boyutlarda ve Vm'leri miktarlarını modelleme işlemi sırasında ekleme etkilerini görmenize olanak sağlar. Değişikliklerin etkisini görüntülemenin bir başka yolu izleyin olmaktır **tüketilen** ve **hala kullanılabilir** doğrudan kullanılabilir sanal makinelerin listesini listelenen sayı. Bu sayı şu anda seçili donanımda SKU göre tahmini değerleri yansıtır.
5. Vm'leri kümenizi oluşturdunuz, önerilen donanım SKU seçerek bulabilirsiniz **önerilen SKU**, sayfanın sağ üst köşesinde, doğrudan aşağıdaki **geçerli SKU** etiketi. Bu düğmeyi kullanarak, daha sonra sanal makine yapılandırmalarınızı değiştirebilir ve her yapılandırma, donanım desteklediğini görün.


## <a name="definebyworkloadfootprint-instructions"></a>DefineByWorkloadFootprint yönergeleri
Azure Stack iş yükleri koleksiyonu kullanarak bir model oluşturmak için Seç **DefineByWorkloadFootprint** sekmesini tıklatın ve bu adımlar dizisini izleyin. Azure Stack iş yükleri, kullanılabilir VM kaynakları kullanarak oluşturun.   

> [!TIP]
> Azure Stack VM için sağlanan depolama boyutu değiştirmek için önceki bölümde üç adımdan nota bakın.

1. Bu çalışma sayfasının sağ alt köşesinde, sağlanan aşağı açılır liste kutusu denetimleri her donanım sisteminin (SKU) yüklenmesini istediğiniz sunucu (4 ile 16 arasında) bir ilk sayısını seçmek için kullanın.
2. Belirli bir donanıma karşı çeşitli VM kaynak ayırmalar model istiyorsanız, doğrudan aşağıdaki mavi aşağı açılır liste kutusunda Bul **geçerli SKU** sayfanın sağ üst köşesindeki etiketi. Bu liste kutusu çekin ve istenen donanımınız SKU seçin.
3. Uygun depolama alanı boyutu her istediğiniz Azure Stack Vm'leriniz için seçin **DefineByVMFootprint** sayfasında, önceki bölümde üç adımda açıklandığı gibi. VM başına depolama alanı boyutu DefineByVMFootprint sayfasında tanımlanır.
4. Başlangıç'ın sol üst **DefineByWorkloadFootprint** sayfasında, en fazla altı farklı iş yükü türleri için yapılandırmalarını oluşturun. Bu iş yükü içinde yer alan her bir VM türünün miktarı girin. Bu iş yükü adı altında doğrudan sütununa sayısal değerler girerek bunu yapabilirsiniz. İş yükü adları belirli bu yapılandırma tarafından desteklenen iş yükleri türünü yansıtacak şekilde değiştirebilirsiniz.
5. Bu sütunun altındaki bir değer girerek doğrudan belirli bir miktar her iş yükü türü içerebilir **miktar** etiketi.
6. İş yükü türleri ve miktarlar oluşturduğunuz seçin **önerilen SKU** sayfanın sağ üst köşesinde, doğrudan aşağıdaki **geçerli SKU** etiketi. Bu iş yüklerinin genel bu yapılandırmayı desteklemek için yeterli kaynaklara sahip küçük SKU görüntüler.
7. Daha fazla donanım için SKU, seçilen sunucu sayısına göre değiştirerek modelleme gerçekleştirmek veya VM ayırma veya iş yükü yapılandırmalarınızı içindeki miktar değiştirme. İlişkili grafik değişikliklerinizi genel kaynak tüketimini etkilemesi gösteren anında geri bildirim görüntüleyin.
8. Değişikliklerinizi memnun kaldığınızda, seçin **önerilen SKU** yeni yapılandırmanız için önerilen SKU yeniden görüntülenecek. Ayrıca, istenen bir SKU seçmek için aşağı açılan menüsünü seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Hakkında bilgi edinin [Azure Stack için tümleştirme konuları veri merkezi](azure-stack-datacenter-integration.md).
