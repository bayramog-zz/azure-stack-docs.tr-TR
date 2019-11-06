---
title: Azure ve Azure Stack hub kullanarak hibrit geçiş çözümünü uygulamaya yönelik bir model.
description: Uç kaynaklara veya güvenlik duvarları tarafından korunan cihazlara bağlanmak için Azure ve Azure Stack Hub hizmetlerini nasıl kullanacağınızı öğrenin.
author: BryanLa
ms.service: azure-stack
ms.topic: article
ms.date: 11/05/2019
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 11/05/2019
ms.openlocfilehash: 9b9defd4a6aec6dab3511425089e7395649d8087
ms.sourcegitcommit: 5c92a669007ab4aaffe4484f1d8836a40340dde1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73638590"
---
# <a name="hybrid-relay-pattern"></a>Karma geçiş deseninin

Azure Service Bus geçişleri kullanılarak güvenlik duvarları tarafından korunan ve kenarda bulunan kaynaklara veya cihazlara nasıl bağlanacağınızı öğrenin.

## <a name="context-and-problem"></a>Bağlam ve sorun

Sınır cihazları çoğunlukla bir kurumsal güvenlik duvarı veya NAT cihazının arkasında bulunur. Güvenli olsalar da, diğer kurumsal ağlardaki genel bulutla veya sınır cihazlarıyla iletişim kuramayabilirler. Genel buluttaki kullanıcılara güvenli bir şekilde belirli bağlantı noktalarını ve işlevleri göstermek gerekebilir. 

## <a name="solution"></a>Çözüm

Hibrit geçiş deseninin, doğrudan iletişim kuramayan iki uç nokta arasında bir WebSockets tüneli oluşturmak için Azure Service Bus geçişleri kullanılır. Şirket içinde olmayan, ancak şirket içi bir uç noktaya bağlanması gereken cihazlar, genel buluttaki bir uç noktaya bağlanır. Bu uç nokta, güvenli bir kanal üzerinden önceden tanımlanmış yollarla trafiği yeniden yönlendirecektir. Şirket içi ortamın içindeki bir uç nokta trafiği alır ve doğru hedefe yönlendirir. 

![Karma geçiş çözüm mimarisi](media/pattern-hybrid-relay/solution-architecture.png)

Çözümün nasıl çalıştığı aşağıda verilmiştir: 

1. Bir cihaz, önceden tanımlanmış bir bağlantı noktasında Azure 'daki sanal makineye (VM) bağlanır.
2. Trafik Azure 'da Service Bus geçişine iletilir.
3. Service Bus geçişine daha önce uzun süreli bir bağlantı oluşturmuş Azure Stack hub 'ındaki VM trafiği alır ve hedefe iletir.
4. Şirket içi hizmet veya uç nokta, isteği işler. 

## <a name="components"></a>Bileşenler

Bu çözüm aşağıdaki bileşenleri kullanır:

| Katmanı | Bileşen | Açıklama |
|----------|-----------|-------------|
| Azure | Azure VM | Azure VM, şirket içi kaynak için genel olarak erişilebilen bir uç nokta sağlar. |
| | Azure Service Bus geçişi | [Azure Service Bus geçişi](/azure/service-bus-relay/) , Azure vm Ile Azure Stack hub sanal makinesi arasındaki tüneli ve bağlantıyı sürdürmek için altyapı sağlar.|
| Azure Stack hub 'ı | İşlem | Azure Stack hub sanal makinesi, karma geçiş tünelinin sunucu tarafını sağlar. |
| | Depolama | Azure Stack hub 'ına dağıtılan AKS motoru kümesi, Yüz Tanıma API'si kapsayıcısını çalıştırmak için ölçeklenebilir ve dayanıklı bir altyapı sağlar.|

## <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler

Bu çözümü nasıl uygulayacağınıza karar verirken aşağıdaki noktaları göz önünde bulundurun:

### <a name="scalability"></a>Ölçeklenebilirlik 

Bu model yalnızca istemci ve sunucuda 1:1 bağlantı noktası eşlemelerine izin verir. Örneğin, bağlantı noktası 80, Azure uç noktasındaki bir hizmet için tünededir, başka bir hizmet için kullanılamaz. Bağlantı noktası eşlemeleri uygun şekilde planlanmalıdır. Service Bus geçişi ve VM 'Ler trafiği işlemek için uygun şekilde ölçeklendirmelidir.

### <a name="availability"></a>Erişilebilirlik

Bu tüneller ve bağlantılar gereksiz değildir. Yüksek kullanılabilirlik sağlamak için, hata denetimi kodu uygulamak isteyebilirsiniz. Diğer bir seçenek, yük dengeleyici arkasında Service Bus geçişine bağlı VM 'lerin bir havuzudur.

### <a name="manageability"></a>Yönetilebilirlik

Bu çözüm, çok sayıda cihaza ve konuma yayılabilir ve bu da bir süre sonra olabilir. Azure 'un IoT Hizmetleri, yeni konumları ve cihazları otomatik olarak çevrimiçi duruma getirebilir ve güncel tutar.

### <a name="security"></a>Güvenlik

Bu model, uçtan bir iç cihazdaki bağlantı noktasına sınırsız erişimine izin verir. İç cihazdaki hizmete veya karma geçiş uç noktasının önüne bir kimlik doğrulama mekanizması eklemeyi düşünün. 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede tanıtılan konular hakkında daha fazla bilgi edinmek için:
- Bu kalıp Azure Service Bus geçişleri kullanır. Daha fazla bilgi için [Azure Service Bus geçiş belgelerine](/azure/service-bus-relay/)bakın.
- En iyi uygulamalar hakkında daha fazla bilgi edinmek ve ek soruları yanıtlamak için bkz. [karma uygulama tasarımı konuları](overview-app-design-considerations.md) .
- Ürünlerin ve çözümlerin tamamı hakkında daha fazla bilgi edinmek için [ürün ve çözümlerin Azure Stack ailesine](/azure-stack)bakın.

Çözüm örneğini test etmeye hazırsanız [karma geçiş çözümü dağıtım kılavuzu](https://aka.ms/hybridrelaydeployment)ile devam edin. Dağıtım Kılavuzu, bileşenlerinin dağıtılması ve test edilmesi için adım adım yönergeler sağlar.