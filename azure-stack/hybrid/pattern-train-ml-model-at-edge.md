---
title: Azure ve Azure Stack hub 'ı kullanarak Edge 'de Machine Learning (ML) modeli eğitimi için kullanılan model.
description: Azure ve Azure Stack Hub hizmetlerini kullanarak kenarda ML modeli eğitimi yapmayı öğrenin.
author: BryanLa
ms.service: azure-stack
ms.topic: article
ms.date: 11/05/2019
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 11/05/2019
ms.openlocfilehash: f845aee01e4c87bc877eb77fb793b32472f9f222
ms.sourcegitcommit: 5c92a669007ab4aaffe4484f1d8836a40340dde1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73639261"
---
# <a name="train-machine-learning-ml-model-at-the-edge-pattern"></a>Edge düzeninde Machine Learning (ML) modeli eğitme

Yalnızca şirket içinde bulunan verilerden taşınabilir ML modelleri oluşturun.

## <a name="context-and-problem"></a>Bağlam ve sorun

Birçok kuruluş, veri bilimcilerinin anladıkları araçları kullanarak şirket içi veya eski verilerden öngörüleri kaldırmak istiyor. [Azure Machine Learning hizmeti](/azure/machine-learning/) , ml ve derin öğrenme modellerini eğmek, ayarlamak ve dağıtmak için bulutta yerel araç sağlar.  

Ancak, bazı veriler buluta çok büyük veya yasal nedenlerle buluta gönderilemez. Veri bilimcileri, bu modeli kullanarak modelleri şirket içi verileri ve işlem kullanarak eğitebilmeniz için Azure Machine Learning kullanabilir. 

## <a name="solution"></a>Çözüm

Edge düzeninde eğitim, Azure Stack hub 'ında çalışan bir sanal makine (VM) kullanır. VM, Azure ML 'de bir işlem hedefi olarak kaydedilir ve bu, yalnızca şirket içinde kullanılabilir verilere erişmesine izin verir. Bu durumda, veriler Azure Stack hub 'ının BLOB depolama alanında depolanır. 

Model eğitilirken Azure ML 'ye kaydedilir, kapsayıcılanmış ve dağıtım için bir Azure Container Registry eklenir. Bu düzenin bu yinelemesi için, Azure Stack hub eğitim sanal makinesine genel Internet üzerinden erişilebilmelidir. 

[uç mimaride ml modelini eğitme ![](media/pattern-train-ml-model-at-edge/solution-architecture.png)](media/pattern-train-ml-model-at-edge/solution-architecture.png)

Çözümün nasıl çalıştığı aşağıda verilmiştir: 

1. Azure Stack hub sanal makinesi, Azure ML ile bir işlem hedefi olarak dağıtılır ve kaydedilir.
2. Azure ML 'de, Azure Stack hub VM 'yi işlem hedefi olarak kullanan bir deneme oluşturulur.
3. Model eğitilirken, kaydedilir ve Kapsayıcılı olur.
4. Model artık şirket içinde veya bulutta bulunan konumlara dağıtılabilir.

## <a name="components"></a>Bileşenler

Bu çözüm aşağıdaki bileşenleri kullanır:

| Katman | Bileşen | Açıklama |
|----------|-----------|-------------|
| Azure | Azure Machine Learning | ML modelinin eğitimini [Azure Machine Learning](/azure/machine-learning/) . |
| | Azure Container Kayıt Defteri | Azure ML, modeli bir kapsayıcıya paketler ve dağıtım için bir [Azure Container Registry](/azure/container-registry/) depolar.|
| Azure Stack Hub | App Service | [App Service olan Azure Stack hub](/azure-stack/operator/azure-stack-app-service-overview) , kenardaki bileşenlerin temelini sağlar. |
| | İşlem | ML modelini eğitmek için Docker ile Ubuntu çalıştıran bir Azure Stack hub VM kullanılır. |
| | Depolama | Özel veriler Azure Stack hub BLOB depolama alanında barındırılabilir. |

## <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler

Bu çözümü nasıl uygulayacağınıza karar verirken aşağıdaki noktaları göz önünde bulundurun:

### <a name="scalability"></a>Ölçeklenebilirlik 

Bu çözümün ölçeklendirilmesini sağlamak için, eğitim için Azure Stack hub 'ında uygun şekilde boyutlandırılabilir bir sanal makine oluşturmanız gerekir.

### <a name="availability"></a>Kullanılabilirlik

Eğitim betikleri ve Azure Stack hub VM 'nin eğitim için kullanılan şirket içi verilere erişimi olduğundan emin olun.

### <a name="manageability"></a>Yönetilebilirlik

Model dağıtımı sırasında karışıklıkları önlemek için modeller ve denemeleri uygun şekilde kaydedildiğinden, sürümlenmiş ve etiketlediğinizden emin olun. 

### <a name="security"></a>Güvenlik

Bu model, Azure ML 'nin şirket içi olası hassas verilere erişmesine olanak tanır. Azure Stack hub VM 'ye SSH için kullanılan hesabın güçlü bir parola olduğundan emin olun ve eğitim betikleri buluta veri korumaz veya bu verileri karşıya yüklememiştir. 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede tanıtılan konular hakkında daha fazla bilgi edinmek için:
- ML ve ilgili konulara genel bakış için [Azure Machine Learning belgelerine](/azure/machine-learning) bakın.
- Kapsayıcı dağıtımları için görüntü oluşturma, depolama ve yönetme hakkında bilgi edinmek için bkz. [Azure Container Registry](/azure/container-registry/) .
- Kaynak sağlayıcısı ve dağıtımı hakkında daha fazla bilgi edinmek için [Azure Stack hub 'ındaki App Service](/azure-stack/operator/azure-stack-app-service-overview) başvurun.
- En iyi uygulamalar hakkında daha fazla bilgi edinmek ve ek soruları yanıtlamak için bkz. [karma uygulama tasarımı konuları](overview-app-design-considerations.md) .
- Ürünlerin ve çözümlerin tamamı hakkında daha fazla bilgi edinmek için [ürün ve çözümlerin Azure Stack ailesine](/azure-stack)bakın.

Çözüm örneğini test etmeye hazırsanız, [Edge dağıtım kılavuzunda EĞITIM ml modeliyle](https://aka.ms/edgetrainingdeploy)devam edin. Dağıtım Kılavuzu, bileşenlerinin dağıtılması ve test edilmesi için adım adım yönergeler sağlar.