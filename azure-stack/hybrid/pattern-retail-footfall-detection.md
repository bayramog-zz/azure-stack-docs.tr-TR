---
title: Azure ve Azure Stack hub 'ı kullanarak bir AI tabanlı bir tetfall algılamayı uygulamaya yönelik karma model.
description: Retail Store trafiğini çözümlemek için bir AI tabanlı bir ıtfall algılama çözümü uygulamak üzere Azure ve Azure Stack Hub hizmetlerini nasıl kullanacağınızı öğrenin.
author: BryanLa
ms.service: azure-stack
ms.topic: article
ms.date: 10/31/2019
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 10/31/2019
ms.openlocfilehash: a7a7563db3c315c4913287e8f286f07abd633602
ms.sourcegitcommit: 5c92a669007ab4aaffe4484f1d8836a40340dde1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73638359"
---
# <a name="footfall-detection-pattern"></a>Betfall algılama kalıbı

Bu düzende, perakende mağazalarındaki ziyaretçi trafiğini çözümlemek için bir AI tabanlı bir tetfall algılama çözümü uygulamaya yönelik bir genel bakış sunulmaktadır. Çözüm, Azure, Azure Stack hub ve Özel Görüntü İşleme AI Dev Kit kullanarak gerçek dünya eylemlerinden Öngörüler oluşturur.

## <a name="context-and-problem"></a>Bağlam ve sorun

Contoso mağazalarının, mağaza düzenine bağlı olarak, müşterilerin geçerli ürünlerini alma hakkında içgörüler elde etmek ister. Her bölüme personeli yerleştiremiyor ve bir analistlerin bir yöneticisinin tüm mağazanın kamera görüntülerini incelemesi için verimsiz bir takım. Bunlara ek olarak, mağazaların hiçbirinde tüm kameralardan analiz için buluta video akışı yapmak için yeterli bant genişliği yoktur. 

Contoso, müşterilerinin demograflarını, kısa bir süre içinde, ekran ve ürünleri depolamak için müşterilerin demograflarını, bağlılık ve yeniden eylemlerini belirlemenin kolay bir yolunu bulmak istiyor.

## <a name="solution"></a>Çözüm

Bu Retail Analytics deseninin kenar altına almak için katmanlı bir yaklaşım kullanılır. Özel Görüntü İşleme AI geliştirme setini kullanarak, Azure bilişsel hizmetler 'i çalıştıran özel bir Azure Stack hub 'ına analiz edilmek üzere yalnızca insan yüzlerine sahip görüntüler gönderilir. Anonimleştirilmiş, toplanan veriler Power BI tüm depolarda ve görselleştirmede toplama için Azure 'a gönderilir. Kenarı ve genel bulutu birleştirmek contoso 'nun modern AI teknolojisinden yararlanmasını sağlar. Aynı zamanda, Kurumsal ilkeleriyle uyumlu kalır ve müşterilerinin gizliliği dikkate alırlar.

[![Betfall algılama çözümü](media/pattern-retail-footfall-detection/solution-architecture.png)](media/pattern-retail-footfall-detection/solution-architecture.png)

Çözümün nasıl çalıştığına ilişkin bir özet aşağıda verilmiştir: 

1. Özel Görüntü İşleme AI Dev Kit, IoT Edge çalışma zamanını ve bir ML modelini yükleyerek IoT Hub bir yapılandırma alır.
2. Model bir kişi görürse, bir resim alır ve Azure Stack hub BLOB depolama alanına yükler. 
3. Blob hizmeti Azure Stack hub 'da bir Azure Işlevi tetikler. 
4. Azure Işlevi, görüntüden demografik ve duygu verileri almak için Yüz Tanıma API'si bir kapsayıcı çağırır.
5. Veriler anonimleştirilmiştir ve bir Azure Olay Hub 'ına gönderilir.
6. Olay Hub 'ı verileri Stream Analytics 'e iter.
7. Stream Analytics verileri toplar ve Power BI gönderir.

## <a name="components"></a>Bileşenler

Bu çözüm aşağıdaki bileşenleri kullanır:

| Katmanı | Bileşen | Açıklama |
|----------|-----------|-------------|
| Mağaza içi donanım | [Özel Görüntü İşleme AI geliştirme seti](https://azure.github.io/Vision-AI-DevKit-Pages/) | Yalnızca analiz için kişilerin görüntülerini yakalayan yerel bir ML modeli kullanarak mağaza içi filtreleme sağlar. IoT Hub aracılığıyla güvenli bir şekilde sağlanmış ve güncelleştirilmiş.<br><br>|
| Azure | [Azure Event Hubs](/azure/event-hubs/) | Azure Event Hubs, Azure Stream Analytics ile düzenli olarak tümleşen anonim verileri almak için ölçeklenebilir bir platform sağlar. |
|  | [Azure Akış Analizi](/azure/stream-analytics/) | Azure Stream Analytics bir iş, anonimleştirilmiş verileri toplar ve görselleştirme için 15 saniyelik Windows ile gruplandırır. |
|  | [Microsoft Power BI](https://powerbi.microsoft.com/) | Power BI, çıktıyı Azure Stream Analytics görüntülemek için kullanımı kolay bir pano arabirimi sağlar. |
| Azure Stack hub 'ı | [App Service](../operator/azure-stack-app-service-overview.md) | App Service kaynak sağlayıcısı (RP) Edge bileşenleri için bir temel sağlar. Web Apps/API 'Ler ve Işlevler için barındırma ve yönetim özellikleri dahil. |
| | Azure Kubernetes hizmeti [(aks) altyapısı](https://github.com/Azure/aks-engine) kümesi | Azure Stack hub 'ına dağıtılan AKS-Engine kümesi ile AKS RP, Yüz Tanıma API'si kapsayıcısını çalıştırmak için ölçeklenebilir ve dayanıklı bir altyapı sağlar. |
| | Azure bilişsel Hizmetler [Yüz Tanıma API'si kapsayıcıları](/azure/cognitive-services/face/face-how-to-install-containers)| Yüz Tanıma API'si kapsayıcılarıyla Azure bilişsel hizmetler RP, contoso 'nun özel ağında demografik, duygu ve benzersiz ziyaretçi algılaması sağlar. |
| | Blob Depolama | AI Dev Kit 'ten yakalanan görüntüler Azure Stack hub 'ın BLOB depolama alanına yüklenir. |
| | Azure İşlevleri | Azure Stack Hub üzerinde çalışan bir Azure Işlevi, blob depolamadan giriş alır ve Yüz Tanıma API'si etkileşimleri yönetir. Anonim verileri Azure 'da bulunan bir olay hub 'ına yayar.<br><br>|

## <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler

Bu çözümü nasıl uygulayacağınıza karar verirken aşağıdaki noktaları göz önünde bulundurun:

### <a name="scalability"></a>Ölçeklenebilirlik 

Bu çözümün birden çok kamera ve konumda ölçeklendirilmesini sağlamak için tüm bileşenlerin daha fazla yükü işleyebilmesini sağlamak gerekir. Şöyle bir işlem yapmanız gerekebilir:

- Stream Analytics akış birimi sayısını artırma
- Yüz Tanıma API'si dağıtımının ölçeğini genişletme
- Event Hubs verimini artırma
- Olağanüstü durumlarda Azure Işlevlerinden bir sanal makineye geçiş yapmanız gerekebilir.

### <a name="availability"></a>Erişilebilirlik

Bu çözüm katmanlı olduğundan, ağ veya güç arızalarının nasıl ele alınacağını düşünmek önemlidir. İş ihtiyaçlarına bağlı olarak, görüntüleri yerel olarak önbelleğe almak için bir mekanizma uygulamak ve ardından bağlantı döndürüldüğünde Azure Stack hub 'a iletmek uygun olabilir. Konum yeterince büyükse, bu konuma Yüz Tanıma API'si kapsayıcı ile Data Box Edge dağıtımı daha iyi bir seçenek olabilir.

### <a name="manageability"></a>Yönetilebilirlik

Bu çözüm, çok sayıda cihaza ve konuma yayılabilir ve bu da bir süre sonra olabilir. [Azure 'un IoT Hizmetleri](/azure/iot-fundamentals/) , yeni konumları ve cihazları otomatik olarak çevrimiçi hale getirmek ve güncel tutmak için kullanılabilir. 

### <a name="security"></a>Güvenlik

Bu çözüm, müşteri görüntülerini yakalar ve güvenlik için bir parametre dikkate alır. Tüm depolama hesaplarının uygun erişim ilkeleriyle güvenli olduğundan ve anahtarların düzenli olarak döndürüldiğinden emin olun. Depolama hesaplarının ve Event Hubs kurumsal ve kamu gizlilik düzenlemelerine uyan bekletme ilkelerine sahip olduğundan emin olun. Ayrıca, Kullanıcı erişim seviyelerini katmandığınızdan emin olun. Katmanlama, kullanıcıların yalnızca rolleri için ihtiyaç duydukları verilere erişmesini sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede tanıtılan konular hakkında daha fazla bilgi edinmek için:
- Bkz. yararlanılabilir [,,,](https://aka.ms/tiereddatadeploy),
- Özel Vision kullanma hakkında daha fazla bilgi edinmek için bkz. [özel görüntü işleme AI geliştirme seti](https://azure.github.io/Vision-AI-DevKit-Pages/) . 

Çözüm örneğini test etmeye hazırsanız, [Betfall algılama dağıtım kılavuzu](solution-deployment-guide-retail-footfall-detection.md)ile devam edin. Dağıtım Kılavuzu, bileşenlerinin dağıtılması ve test edilmesi için adım adım yönergeler sağlar.