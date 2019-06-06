---
title: Azure Bilişsel hizmetler için Azure Stack dağıtma | Microsoft Docs
description: Azure Bilişsel hizmetler için Azure Stack dağıtmayı öğrenin.
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
ms.date: 04/15/2019
ms.author: mabrigg
ms.reviewer: guanghu
ms.lastreviewed: 12/11/2018
ms.openlocfilehash: c642a3eeb07ef2ce94ca8dc338a781256c5b9f37
ms.sourcegitcommit: 7f39bdc83717c27de54fe67eb23eb55dbab258a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66691226"
---
# <a name="deploy-azure-cognitive-services-to-azure-stack"></a>Azure Bilişsel hizmetler için Azure Stack dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

> [!Note]  
> Azure Stack'te Azure Bilişsel hizmetler için Önizleme aşamasındadır.

Azure Bilişsel hizmetler, Azure Stack üzerinde kapsayıcı desteği ile kullanabilirsiniz. Azure Bilişsel hizmetler kapsayıcı desteği, Azure'da kullanılabilen aynı zengin API'lerin kullanmanıza olanak tanır. Kapsayıcıları kullanımınız, dağıtma ve barındırmak sunulan Hizmetleri nerede esneklik sağlar. [Docker kapsayıcıları](https://www.docker.com/what-container). Kapsayıcı desteği şu anda bir alt kümesini Azure Bilişsel bölümlerini dahil olmak üzere hizmetler için önizleme modunda [görüntü işleme](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home), [yüz](https://docs.microsoft.com/azure/cognitive-services/face/overview), ve [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview), ve [Language Understanding'i](https://docs.microsoft.com/azure/cognitive-services/luis/luis-container-howto) (LUIS).

Kapsayıcı, yazılım dağıtımı için bir yaklaşım olan bir uygulama veya hizmet yapılandırması ve bağımlılıkları da dahil olmak üzere, bir kapsayıcı görüntüsü paketlenir. Çok az kayıpla veya hiç değişiklik yapmadan, görüntüyü bir kapsayıcı konağı için dağıtabilirsiniz. Her kapsayıcı, diğer kapsayıcılardan ve alttaki işletim sisteminden ayrı tutulur. Sistem, yalnızca görüntünüzü çalıştırmak için gerekli bileşenleri vardır. Bir kapsayıcı konağı bir daha küçük kaplama alanı bir sanal makine daha vardır. Görüntülerinden kısa vadeli görevler için kapsayıcılar oluşturabilirsiniz ve bunlar artık gerekli olmadığında kaldırılabilir.

## <a name="use-containers-with-cognitive-services-on-azure-stack"></a>Azure Stack'te Bilişsel hizmetler ile kapsayıcılar kullanma

- **Veriler üzerinde denetim**  
  Uygulama kullanıcılarınızın, Bilişsel hizmetler kullanarak verileri üzerinde denetime sahip olmasını sağlar. Bilişsel hizmetler, küresel Azure veri veya genel bulut gönderemiyor uygulama kullanıcılar için teslim edebilirsiniz.

- **Model üzerinde denetim güncelleştirir**  
  Uygulama kullanıcılarının kendi çözümünde dağıtılmış modelleri için sürüm güncelleştirmelerini sağlar.

- **Taşınabilir mimarisi**  
  Taşınabilir uygulama mimarisi oluşturulmasını etkinleştirin; böylelikle özel, genel bulut çözümünüzü dağıtabileceğiniz bulut şirket içinde veya uç cihazlarında. Kapsayıcınızı Azure Kubernetes hizmeti, Azure Container Instances, veya Azure Stack'te bir Kubernetes kümesine dağıtabilirsiniz. Daha fazla bilgi için [Azure Stack dağıtma Kubernetes](azure-stack-solution-template-kubernetes-deploy.md).

- **Yüksek aktarım hızına ve düşük gecikme süresi**  
   Uygulama kullanıcılarınızın, ani trafik yüksek aktarım hızı ve düşük gecikme süresi ile ölçeklendirme olanağı sağlar. Azure Kubernetes hizmetinde, uygulama mantığı ve verileri yakın fiziksel olarak çalıştırmak, Bilişsel hizmetler sağlar.

Azure Stack ile bir Kubernetes kümesinde uygulama kapsayıcılarınızı yüksek kullanılabilirlik ve esnek ölçeklendirme yanı sıra Bilişsel hizmetler kapsayıcıları dağıtın. Bilişsel hizmetler, uygulama hizmetleri, İşlevler, Blob Depolama, SQL veya mySQL veritabanları üzerinde oluşturulan bileşenler ile birleştirerek, uygulama geliştirebilirsiniz.

Bilişsel hizmetler kapsayıcılar hakkında daha fazla ayrıntı için Git [Azure Bilişsel hizmetler kapsayıcı desteği](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support).

## <a name="deploy-the-azure-face-api"></a>Azure yüz tanıma API'si ile dağıtma

Bu makalede, bir Kubernetes kümesinde Azure Stack'te Azure yüz tanıma API'si dağıtmayı açıklar. Azure Stack Kubernetes kümelerinde başka bilişsel hizmetler kapsayıcıları dağıtmak için aynı yaklaşımı kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce yapmanız gerekir:

1.  Azure Bilişsel hizmetler kapsayıcı kayıt defterinden yüz kapsayıcı görüntüleri çekmek için kapsayıcı kayıt defterine erişim isteyin. Ayrıntılar için bkz [özel kapsayıcı kayıt defterine erişim isteği](https://docs.microsoft.com/azure/cognitive-services/face/face-how-to-install-containers#request-access-to-the-private-container-registry).

2.  Azure Stack'te bir Kubernetes kümesi hazırlayın. Makale izleyebilirsiniz [Azure Stack dağıtma Kubernetes](azure-stack-solution-template-kubernetes-deploy.md).

## <a name="create-azure-resources"></a>Azure kaynakları oluşturma

Yüz tanıma, LUIS veya metni tanı kapsayıcılara önizlemesini görüntülemek için Azure üzerinde bir Bilişsel Hizmet kaynağı oluşturun. Bilişsel hizmet kapsayıcı örneği oluşturmak için abonelik anahtarını ve uç nokta URL'si kaynaktan kullanmanız gerekir.

1. Azure portalında bir Azure kaynağı oluşturun. Yüz tanıma kapsayıcıları Önizleme istiyorsanız, öncelikle Azure portalında karşılık gelen bir yüz kaynak oluşturmanız gerekir. Daha fazla bilgi için [hızlı başlangıç: Azure portalında bir Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).

   > [!Note]
   >  Yüz tanıma veya görüntü işleme kaynak F0 fiyatlandırma katmanını kullanmanız gerekir.

2. Azure kaynakları için uç nokta URL'si ve abonelik anahtarını alın. Azure kaynak oluşturduktan sonra bu kaynaktan abonelik anahtarını ve uç nokta URL'si Önizleme için karşılık gelen yüz tanıma, LUIS veya metni tanı kapsayıcı örneği oluşturmak için kullanın.

## <a name="create-a-kubernetes-secret"></a>Bir Kubernetes gizli dizisi oluşturma 

Olun Kubectl kullanımını özel kapsayıcı kayıt defterine erişim için gizli komutu oluşturun. Değiştirin `<username>` kullanıcı adıyla ve `<password>` Azure Bilişsel hizmetler ekibinden aldığınız kimlik bilgileri sağlanan parolayla.

```bash  
    kubectl create secret docker-registry <secretName> \
        --docker-server='containerpreview.azurecr.io' \
        --docker-username='<username>' \
        --docker-password='<password>' 
```

## <a name="prepare-a-yaml-configure-file"></a>Bir YAML hazırlama dosyasını yapılandırma

Kullanım YAML dosyası Kubernetes kümesinde bilişsel hizmet dağıtımını basitleştirmek için yapılandırın.

İşte bir örnek YAML yapılandırma dosyası, Azure Stack için yüz hizmeti dağıtmak için:

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

Bu YAML içinde yapılandırma dosyası, bilişsel hizmet kapsayıcı görüntülerini Azure Container Registry'den almak için kullanılan gizli diziyi kullanın. Gizli dizi dosyasını, belirli bir yineleme kapsayıcının dağıtmak için kullanılır. Kullanıcıların bu hizmet dışarıdan erişebildiğinden emin olmak için bir yük dengeleyici de oluşturabilirsiniz.

Anahtar alanları hakkındaki ayrıntıları:

| Alan | Notlar |
| --- | --- |
| replicaNumber | İlk çoğaltmaları oluşturmak için örnekleri tanımlar. Dağıtımdan sonra ölçeklendirebilirsiniz. |
| ImageLocation | Belirli bir bilişsel hizmet kapsayıcı görüntüyü ACR konumunu gösterir. Örneğin, yüz tanıma hizmeti: `aicpppe.azurecr.io/microsoft/cognitive-services-face` |
| BillingURL |Uç nokta URL'sini adımında [Azure Kaynağı Oluştur](#create-azure-resources) |
| ApiKey | Abonelik anahtarı adımında [Azure Kaynağı Oluştur](#create-azure-resources) |
| secretName | [Create Kubernetes secret(#create-a-kubernetes-secret) adımda oluşturduğunuz gizli dizi adı |

## <a name="deploy-the-cognitive-service"></a>Bilişsel hizmet dağıtma

Bilişsel hizmet kapsayıcıları dağıtmak için aşağıdaki komutu kullanın:

```bash  
    Kubectl apply -f <yamlFineName>
```
Nasıl dağıttığı izlemek için aşağıdaki komutu kullanın: 
```bash  
    Kubectl get pod - watch
```

## <a name="test-the-cognitive-service"></a>Bilişsel hizmet test

Erişim [Openapı belirtimi](https://swagger.io/docs/specification/about/) gelen **/swagger** bu kapsayıcı için göreli URI. Eski adı Swagger belirtimi bilinen bu belirtim, örneklenmiş bir kapsayıcı tarafından desteklenen işlemler açıklanır. Örneğin, aşağıdaki URI, önceki örnekte örneklenmiş yaklaşım analizi kapsayıcı Openapı belirtimi için erişim sağlar:

```HTTP  
http:<External IP>:5000/swagger
```

Aşağıdaki komutu dış IP adresini alabilirsiniz: 

```bash  
    Kubectl get svc <LoadBalancerName>
```

## <a name="try-the-services-with-python"></a>Python ile Hizmetleri deneyin

Bilişsel hizmetler, Azure Stack hakkında bazı basit Python betiklerini çalıştırarak doğrulamak deneyebilirsiniz. Resmi Python hızlı başlangıç örnekleri vardır [görüntü işleme](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home), [yüz](https://docs.microsoft.com/azure/cognitive-services/face/overview), [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview), ve [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/luis-container-howto) () LUIS) başvuru amacıyla.

Python uygulamaları kapsayıcıları üzerinde çalışan hizmetleri doğrula kullanırken akılda tutulması gereken iki şey vardır: 
1. Kapsayıcılar, bilişsel hizmetler, alt anahtarları için kimlik doğrulaması gerekmez ancak herhangi bir dize SDK karşılamak için bir yer tutucu olarak yine de ihtiyacınız vardır. 
2. Base_URL gerçek bir hizmet uç noktası IP adresiyle değiştirin.

Algılama ve bir görüntüdeki yüzleri çerçeve yüz tanıma Hizmetleri Python SDK'sını kullanarak bir örnek Python betiği şu şekildedir:

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

[Yükleme ve görüntü işleme API'si kapsayıcıları çalıştırmak nasıl.](https://docs.microsoft.com/azure/cognitive-services/computer-vision/computer-vision-how-to-install-containers)

[Yükleme ve yüz tanıma API'si kapsayıcıları çalıştırın](https://docs.microsoft.com/azure/cognitive-services/face/face-how-to-install-containers)

[Yükleme ve metin analizi API'si kapsayıcıları çalıştırın](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers)

[Yükleme ve dil anlama (LIUS) kapsayıcıları çalıştırın](https://docs.microsoft.com/azure/cognitive-services/luis/luis-container-howto)
