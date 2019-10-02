---
title: Azure ve Azure Stack kullanarak bir AI tabanlı bir betfall algılama modelini uygulamaya yönelik karma çözüm
description: Azure ve Azure Stack hizmetlerini kullanarak, perakende mağaza trafiğini çözümlemek için bir AI tabanlı bir ıtfall algılama modelini nasıl uygulayacağınızı öğrenin.
services: azure-stack
documentationcenter: ''
author: BryanLa
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 09/23/2019
ms.openlocfilehash: 32533b063d4c42b740825923e9e2b5692ec03728
ms.sourcegitcommit: bbf3edbfc07603d2c23de44240933c07976ea550
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71714639"
---
# <a name="footfall-detection-solution"></a>Betfall algılama çözümü

Bu makalede, bir AI tabanlı bir betfall algılama deseninin uygulanması için bir çözüme genel bakış sunulmaktadır. Bu model, perakende mağazalarındaki ziyaretçi trafiğini analiz etmek için kullanışlıdır. Çözüm, Azure, Azure Stack ve Özel Görüntü İşleme AI Dev Kit kullanarak gerçek dünyada eylemlerden Öngörüler oluşturur.

## <a name="context-and-problem"></a>Bağlam ve sorun

Contoso mağazalarının, mağaza düzenine bağlı olarak, müşterilerin geçerli ürünlerini alma hakkında içgörüler elde etmek ister. Bu kişiler her bir bölüme personeli yerleştiremiyor ve bir analist ekibinin bir mağazanın tüm kamera görüntülerini gözden geçirmesi gerekmez. Bunlara ek olarak, mağazaların hiçbirinde tüm kameralardan analiz için buluta video akışı yapmak için yeterli bant genişliği yoktur. 

Contoso, müşterilerinin demograflarını, kısa bir süre içinde, ekran ve ürünleri depolamak için müşterilerin demograflarını, bağlılık ve yeniden eylemlerini belirlemenin kolay bir yolunu bulmak istiyor.

## <a name="solution"></a>Çözüm

Bu Retail Analytics çözümü, kenar altına almak için katmanlı bir yaklaşım kullanır. Özel Görüntü İşleme AI geliştirme setini kullanarak, Azure bilişsel hizmetler 'i çalıştıran özel bir Azure Stack analiz için yalnızca insan yüzlerine sahip görüntüler gönderilir. Anonimleştirilmiş, toplanan veriler Power BI tüm depolarda ve görselleştirmede toplama için Azure 'a gönderilir. Kenarı ve genel bulutu birleştirmek contoso 'nun modern AI teknolojisinden yararlanmasını sağlar. Aynı zamanda, Kurumsal ilkeleriyle uyumlu kalır ve müşterilerinin gizliliği dikkate alırlar.

[![Betfall algılama kalıbı](media/hybrid-pattern-ai-footfall-detection/solution-architecture.png)](media/hybrid-pattern-ai-footfall-detection/solution-architecture.png)

Çözümün nasıl çalıştığına ilişkin bir özet aşağıda verilmiştir: 

1. Özel Görüntü İşleme AI Dev Kit, IoT Edge çalışma zamanını ve bir ML modelini yükleyerek IoT Hub bir yapılandırma alır.
2. Model bir kişi görürse, bir resim alır ve Azure Stack BLOB depolama alanına yükler. 
3. Blob hizmeti Azure Stack bir Azure Işlevini tetikler. 
4. Azure Işlevi, görüntüden demografik ve duygu verileri almak için Yüz Tanıma API'si bir kapsayıcı çağırır.
5. Veriler anonimleştirilmiştir ve bir Azure Olay Hub 'ına gönderilir.
6. Olay Hub 'ı verileri Stream Analytics 'e iter.
7. Stream Analytics verileri toplar ve Power BI gönderir.

## <a name="components"></a>Bileşenler

Bu çözüm aşağıdaki bileşenleri kullanır:

| Katman | Bileşen | Açıklama |
|----------|-----------|-------------|
| Mağaza içi donanım | [Özel Görüntü İşleme AI geliştirme seti](https://azure.github.io/Vision-AI-DevKit-Pages/) | Yalnızca analiz için kişilerin görüntülerini yakalayan yerel bir ML modeli kullanarak mağaza içi filtreleme sağlar. IoT Hub aracılığıyla güvenli bir şekilde sağlanmış ve güncelleştirilmiş.<br><br>|
| Azure Stack | [App Service](../operator/azure-stack-app-service-overview.md) | App Service kaynak sağlayıcısı (RP) Edge bileşenleri için bir temel sağlar. Web Apps/API 'Ler ve Işlevler için barındırma ve yönetim özellikleri dahil. |
| | Azure Kubernetes hizmeti [(aks) altyapısı](https://github.com/Azure/aks-engine) kümesi | AKS-Engine kümesi ile birlikte Azure Stack dağıtılan AKS RP, Yüz Tanıma API'si kapsayıcısını çalıştırmak için ölçeklenebilir ve dayanıklı bir altyapı sağlar. |
| | Azure bilişsel Hizmetler [Yüz Tanıma API'si kapsayıcıları](/azure/cognitive-services/face/face-how-to-install-containers)| Yüz Tanıma API'si kapsayıcılarıyla Azure bilişsel hizmetler RP, contoso 'nun özel ağında demografik, duygu ve benzersiz ziyaretçi algılaması sağlar. |
| | Blob Depolama Alanı | AI geliştirme seti 'nden yakalanan görüntüler Azure Stack BLOB depolama alanına yüklenir. |
| | Azure İşlevleri | Azure Stack üzerinde çalışan bir Azure Işlevi, blob depolamadan giriş alır ve Yüz Tanıma API'si etkileşimleri yönetir. Anonim verileri Azure 'da bulunan bir olay hub 'ına yayar.<br><br>|
| Azure |  |  |
|  | [Azure Event Hubs](/azure/event-hubs/) | Azure Event Hubs, Azure Stream Analytics ile düzenli olarak tümleşen anonim verileri almak için ölçeklenebilir bir platform sağlar. |
|  | [Azure Akış Analizi](/azure/stream-analytics/) | Azure Stream Analytics bir iş, anonimleştirilmiş verileri toplar ve görselleştirme için 15 saniyelik Windows ile gruplandırır. |
|  | [Microsoft Power BI](https://powerbi.microsoft.com/) | Power BI, çıktıyı Azure Stream Analytics görüntülemek için kullanımı kolay bir pano arabirimi sağlar. |

## <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler

Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:

### <a name="scalability"></a>Ölçeklenebilirlik 

Bu çözümün birden çok kamera ve konumda ölçeklendirilmesini sağlamak için tüm bileşenlerin daha fazla yükü işleyebilmesini sağlamak gerekir. Şöyle bir işlem yapmanız gerekebilir:

- Stream Analytics akış birimi sayısını artırma
- Yüz Tanıma API'si dağıtımının ölçeğini genişletme
- Event Hubs verimini artırma
- Olağanüstü durumlarda Azure Işlevlerinden bir sanal makineye geçiş yapmanız gerekebilir.

### <a name="availability"></a>Kullanılabilirlik

Bu çözüm katmanlı olduğundan, ağ veya güç arızalarının nasıl ele alınacağını düşünmek önemlidir. İş ihtiyaçlarına bağlı olarak, görüntüleri yerel olarak önbelleğe almak için bir mekanizma uygulamak ve bağlantı döndürüldüğünde Azure Stack iletmek için uygun olabilir. Konum yeterince büyükse, bu konuma Yüz Tanıma API'si kapsayıcı ile Data Box Edge dağıtımı daha iyi bir seçenek olabilir.

### <a name="manageability"></a>Yönetilebilirlik

Bu çözüm, çok sayıda cihaza ve konuma yayılabilir ve bu da bir süre sonra olabilir. [Azure 'un IoT Hizmetleri](/azure/iot-fundamentals/) , yeni konumları ve cihazları otomatik olarak çevrimiçi hale getirmek ve güncel tutmak için kullanılabilir. 

### <a name="security"></a>Güvenlik

Bu çözüm, müşteri görüntülerini yakalar ve güvenlik için bir parametre dikkate alır. Tüm depolama hesaplarının uygun erişim ilkeleriyle güvenli olduğundan ve anahtarların düzenli olarak döndürüldiğinden emin olun. Depolama hesaplarının ve Event Hubs kurumsal ve kamu gizlilik düzenlemelerine uyan bekletme ilkelerine sahip olduğundan emin olun. Ayrıca, erişim düzeylerinin, sisteme erişmesi gereken her kişinin yalnızca rolleri için ihtiyaç duydukları verilere erişime sahip olduğundan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

- Bu çözüm tarafından kullanılan ilgili desenler hakkında daha fazla bilgi edinmek için [katmanlı veri deseni](azure-stack-solution-staged-data.md)' ne bakın. 
- Özel vizyonu kullanma hakkında daha fazla bilgi için bkz. [AI Dev Kit özel görüntü işleme](https://azure.github.io/Vision-AI-DevKit-Pages/). 
- [Betfall çözümünün örnek bir uygulamasını](https://github.com/Azure-Samples/azure-intelligent-edge-patterns/tree/master/footfall-analysis)indirin ve dağıtın. Azure, Azure Stack ve Özel Görüntü İşleme AI Dev Kit kullanarak gerçek dünya eylemlerine yönelik Öngörüler oluşturmayı öğreneceksiniz.   
