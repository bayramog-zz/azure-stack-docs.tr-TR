---
title: Azure bilişsel hizmetler 'i Azure Stack dağıtma | Microsoft Docs
description: Azure Stack için Azure bilişsel hizmetler dağıtmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2019
ms.author: mabrigg
ms.reviewer: guanghu
ms.lastreviewed: 12/11/2018
ms.openlocfilehash: 9098be09e8b5bd20660f12789159a41d5a330408
ms.sourcegitcommit: b95983e6e954e772ca5267304cfe6a0dab1cfcab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68417315"
---
# <a name="deploy-azure-cognitive-services-to-azure-stack"></a>Azure Stack için Azure bilişsel hizmetler dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

> [!Note]  
> Azure Stack Azure bilişsel hizmetler önizleme aşamasındadır.

Azure bilişsel hizmetler 'i, Azure Stack kapsayıcı desteğiyle birlikte kullanabilirsiniz. Azure bilişsel hizmetler 'deki kapsayıcı desteği, Azure 'da kullanılabilen aynı zengin API 'Leri kullanmanıza olanak sağlar. Kapsayıcıları kullanmanız, [Docker kapsayıcılarında](https://www.docker.com/what-container)sunulan hizmetlerin dağıtılması ve barındıriletilmesi için esneklik sağlar. Kapsayıcı desteği şu anda [görüntü işleme](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home), [yüz](https://docs.microsoft.com/azure/cognitive-services/face/overview)ve [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview)ve [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/luis-container-howto) (Luu) gibi Azure bilişsel hizmetler 'nin bir alt kümesi için önizleme aşamasında kullanıma sunuldu.

Kapsayıcılama, bağımlılıkları ve yapılandırması dahil olmak üzere bir uygulama veya hizmetin bir kapsayıcı görüntüsü olarak paketlendiği yazılım dağıtımına yönelik bir yaklaşımdır. Çok az değişiklikle bir kapsayıcı konağına bir görüntü dağıtabilirsiniz. Her kapsayıcı diğer kapsayıcılardan ve temel alınan işletim sisteminden yalıtılmıştır. Sistemde yalnızca görüntünüzü çalıştırmak için gereken bileşenler bulunur. Bir kapsayıcı konağın bir sanal makineden daha küçük bir parmak izi vardır. Ayrıca, kısa süreli görevler için görüntülerden kapsayıcılar oluşturabilirsiniz ve artık gerekli olmadığında kaldırılabilir.

## <a name="use-containers-with-cognitive-services-on-azure-stack"></a>Azure Stack bilişsel hizmetler ile kapsayıcılar kullanma

- **Veriler üzerinde denetim**  
  Uygulama kullanıcılarınızın bilişsel hizmetler 'i kullanırken verileri üzerinde denetime sahip olmasını sağlar. Genel Azure veya genel buluta veri gönderebilen uygulama kullanıcılarına bilişsel hizmetler sunabilirsiniz.

- **Model güncelleştirmeleri üzerinde denetim**  
  Uygulama kullanıcılarına, çözümünde dağıtılan modellere yönelik güncelleştirmeler sağlar.

- **Taşınabilir mimari**  
  Çözümünüzü genel buluta, şirket içi özel buluta veya kenara dağıtabilmeniz için taşınabilir uygulama mimarisi oluşturmayı etkinleştirin. Kapsayıcınızı Azure Kubernetes hizmetine, Azure Container Instances veya Azure Stack bir Kubernetes kümesine dağıtabilirsiniz. Daha fazla bilgi için [Azure Stack dağıtma Kubernetes](azure-stack-solution-template-kubernetes-deploy.md).

- **Yüksek aktarım hızı ve düşük gecikme süresi**  
   Uygulama kullanıcılarınıza yüksek aktarım hızı ve düşük gecikme süresi boyunca trafikle ölçeklendirme özelliği sağlayın. Bilişsel hizmetler 'in Azure Kubernetes hizmetinde fiziksel olarak uygulama mantığına ve verilerine yakın bir şekilde çalışmasını sağlama.

Azure Stack, bir Kubernetes kümesinde bilişsel hizmetler kapsayıcılarını, yüksek kullanılabilirlik ve elastik ölçekleme için uygulama kapsayıcılarınızla birlikte dağıtın. Bilişsel hizmetler 'i App Services, Işlevler, blob Storage, SQL veya mySQL veritabanlarında oluşturulan bileşenlerle birleştirerek uygulamanızı geliştirebilirsiniz.

Bilişsel hizmetler kapsayıcıları hakkında daha fazla bilgi için Azure bilişsel [Hizmetler 'de kapsayıcı desteği](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support)' ne gidin.

## <a name="deploy-the-azure-face-api"></a>Azure Yüz Tanıma API'si dağıtma

Bu makalede, Azure Yüz Tanıma API'si Azure Stack bir Kubernetes kümesinde nasıl dağıtılacağı açıklanır. Diğer bilişsel hizmetler kapsayıcılarını Azure Stack Kubernetes kümelerinde dağıtmak için aynı yaklaşımı kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yapmanız gerekir:

1.  Azure bilişsel hizmetler Container Registry yüz kapsayıcı görüntülerini çekmek için kapsayıcı kayıt defterine erişim isteyin. Ayrıntılar için bkz. [özel kapsayıcı kayıt defterine erişim isteği](https://docs.microsoft.com/azure/cognitive-services/face/face-how-to-install-containers#request-access-to-the-private-container-registry).

2.  Azure Stack bir Kubernetes kümesi hazırlayın. [Azure Stack Için Kubernetes dağıtma](azure-stack-solution-template-kubernetes-deploy.md)makalesini takip edebilirsiniz.

## <a name="create-azure-resources"></a>Azure kaynakları oluşturma

Yüz, LUYA veya Metin Tanıma kapsayıcıları önizlemek için Azure 'da bilişsel hizmet kaynağı oluşturun. Bilişsel hizmet kapsayıcılarını oluşturmak için kaynaktaki abonelik anahtarını ve uç nokta URL 'sini kullanmanız gerekir.

1. Azure portalında bir Azure kaynağı oluşturun. Yüz kapsayıcılarının önizlemesini yapmak istiyorsanız, öncelikle Azure portal ilgili bir yüz kaynağı oluşturmanız gerekir. Daha fazla bilgi için bkz [. hızlı başlangıç: Azure portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)bilişsel Hizmetler hesabı oluşturun.

   > [!Note]
   >  Yüz veya Görüntü İşleme kaynak F0 fiyatlandırma katmanını kullanmalıdır.

2. Azure kaynakları için uç nokta URL'si ve abonelik anahtarını alın. Azure kaynağını oluşturduktan sonra, önizleme için ilgili yüzü, LUYA veya Metin Tanıma kapsayıcısını başlatmak üzere bu kaynaktaki abonelik anahtarını ve uç nokta URL 'sini kullanın.

## <a name="create-a-kubernetes-secret"></a>Kubernetes gizli dizisi oluşturma 

Özel kapsayıcı kayıt defterine erişmek için Kubectl Create Secret komutunu kullanın. Kullanıcı `<username>` adıyla ve `<password>` Azure bilişsel hizmetler takımınızdan aldığınız kimlik bilgilerinde girilen parolayla değiştirin.

```bash  
    kubectl create secret docker-registry <secretName> \
        --docker-server='containerpreview.azurecr.io' \
        --docker-username='<username>' \
        --docker-password='<password>' 
```

## <a name="prepare-a-yaml-configure-file"></a>YAML yapılandırma dosyasını hazırlama

Kubernetes kümesinde bilişsel hizmet dağıtımını basitleştirmek için YAML yapılandırma dosyasını kullanın.

Yüz hizmetini Azure Stack için dağıtmak üzere bir örnek YAML yapılandırma dosyası aşağıda verilmiştir:

```Yaml  
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: <deploymentName>
spec:
  replicas: <replicaNumber>
  template:
    metadata:
      labels:
        app: <appName>
    spec:
      containers:
      - name: <containersName>
        image: <ImageLocation>
        env: 
        - name: EULA
          value: accept 
        - name: Billing
          value: <billingURL> 
        - name: apikey
          value: <apiKey>
        tty: true
        stdin: true
        ports:
        - containerPort: 5000 
      imagePullSecrets:
        - name: <secretName>
---
apiVersion: v1
kind: Service
metadata:
  name: <LBName>
spec:
  type: LoadBalancer
  ports:
  - port: 5000
    targetPort : 5000
    name: <PortsName>
  selector:
    app: <appName>
```

Bu YAML dosyası yapılandırma dosyasında, Azure Container Registry bilişsel hizmet kapsayıcısı görüntülerini almak için kullandığınız gizli anahtarı kullanın. Gizli dosya, kapsayıcının belirli bir çoğaltmasını dağıtmak için kullanılır. Ayrıca, kullanıcıların bu hizmete dışarıdan erişebilmesini sağlamak için bir yük dengeleyici de oluşturabilirsiniz.

Anahtar alanlarla ilgili ayrıntılar:

| Alan | Notlar |
| --- | --- |
| Replicanumarası | Oluşturulacak örneklerin ilk çoğaltmalarını tanımlar. Dağıtımdan sonra, daha sonra ölçeklendirebilirsiniz. |
| ImageLocation | ACR 'deki belirli bilişsel hizmet kapsayıcısı resminin konumunu gösterir. Örneğin, yüz hizmeti:`aicpppe.azurecr.io/microsoft/cognitive-services-face` |
| BillingURL |[Azure kaynağı oluşturma](#create-azure-resources) adımında belirtilen uç nokta URL 'si |
| ApiKey | [Azure kaynağı oluşturma](#create-azure-resources) adımında belirtilen abonelik anahtarı |
| SecretName | Adımda oluşturduğunuz gizli dizi adı [bir Kubernetes gizli dizisi oluşturma](#create-a-kubernetes-secret) |

## <a name="deploy-the-cognitive-service"></a>Bilişsel hizmeti dağıtma

Bilişsel hizmet kapsayıcılarını dağıtmak için aşağıdaki komutu kullanın:

```bash  
    Kubectl apply -f <yamlFineName>
```
Uygulamasının nasıl dağıttığı hakkında izlemek için aşağıdaki komutu kullanın: 
```bash  
    Kubectl get pod - watch
```

## <a name="test-the-cognitive-service"></a>Bilişsel hizmeti test etme

Bu kapsayıcı için **/Swagger** göreli URI 'Den [openapı belirtimine](https://swagger.io/docs/specification/about/) erişin. Daha önce Swagger belirtimi olarak bilinen bu belirtim, Örneklenmiş bir kapsayıcı tarafından desteklenen işlemleri açıklar. Örneğin, aşağıdaki URI, önceki örnekte örneklenmiş yaklaşım analizi kapsayıcı Openapı belirtimi için erişim sağlar:

```HTTP  
http:<External IP>:5000/swagger
```

Dış IP adresini aşağıdaki komuttan alabilirsiniz: 

```bash  
    Kubectl get svc <LoadBalancerName>
```

## <a name="try-the-services-with-python"></a>Python ile hizmetleri deneyin

Bazı basit Python betikleri çalıştırarak Azure Stack bilişsel Hizmetleri doğrulamayı deneyebilirsiniz. Başvurunuz için [görüntü işleme](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home), [yüz](https://docs.microsoft.com/azure/cognitive-services/face/overview), [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview)ve [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/luis-container-howto) (LUA) için resmi Python hızlı başlangıç örnekleri mevcuttur.

Kapsayıcılar üzerinde çalışan hizmetleri doğrulamak için Python uygulamalarını kullanırken göz önünde bulundurmanız gereken iki şey vardır: 
1. Kapsayıcılardaki bilişsel hizmetler, kimlik doğrulaması için alt anahtarlara gerek kalmaz, ancak SDK 'Yı karşılamak için bir yer tutucu olarak herhangi bir dize gerektirir. 
2. Base_URL değerini gerçek hizmet uç noktası IP adresi ile değiştirin.

Bir görüntüdeki yüzeyleri algılamak ve çerçeveye eklemek için yüz Hizmetleri Python SDK 'sını kullanan örnek bir Python betiği aşağıda verilmiştir:

```Python  
import cognitive_face as CF

# Cognitive Services in container do not need sub keys for authentication
# Keep the invalid key to satisfy the SDK inputs requirement. 
KEY = '0'  #  (keeping the quotes in place).
CF.Key.set(KEY)

# Get your actual Ip Address of service endpoint of your cognitive service on Azure Stack
BASE_URL = 'http://<svc IP Address>:5000/face/v1.0/'  
CF.BaseUrl.set(BASE_URL)

# You can use this example JPG or replace the URL below with your own URL to a JPEG image.
img_url = 'https://raw.githubusercontent.com/Microsoft/Cognitive-Face-Windows/master/Data/detection1.jpg'
faces = CF.face.detect(img_url)
print(faces)

```

## <a name="next-steps"></a>Sonraki adımlar

[Görüntü İşleme API'si kapsayıcıları yüklemek ve çalıştırmak.](https://docs.microsoft.com/azure/cognitive-services/computer-vision/computer-vision-how-to-install-containers)

[Yüz Tanıma API'si kapsayıcıları yüklemek ve çalıştırmak](https://docs.microsoft.com/azure/cognitive-services/face/face-how-to-install-containers)

[Metin Analizi API'si kapsayıcıları yüklemek ve çalıştırmak](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers)

[Language Understanding (LIUS) kapsayıcıları yüklemek ve çalıştırmak](https://docs.microsoft.com/azure/cognitive-services/luis/luis-container-howto)
