---
title: Azure ve Azure Stack hub kullanarak bir AI tabanlı bir kantfall algılama çözümü dağıtma
description: Azure ve Azure Stack hub kullanarak bir betfall algılama çözümünü dağıtmayı öğrenin. Bu çözüm, perakende mağazalarındaki ziyaretçi trafiğini çözümlemek için kullanılır.
author: BryanLa
ms.service: azure-stack
ms.topic: article
ms.date: 11/05/2019
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 11/05/2019
ms.openlocfilehash: 41166490ef2a79bc61375479d8c83e0205ab44a6
ms.sourcegitcommit: 5c92a669007ab4aaffe4484f1d8836a40340dde1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73638337"
---
# <a name="deploy-an-ai-based-footfall-detection-solution-using-azure-and-azure-stack-hub"></a>Azure ve Azure Stack hub kullanarak bir AI tabanlı bir kantfall algılama çözümü dağıtma

*Uygulama hedefi: Azure Stack hub tümleşik sistemleri ve Azure Stack hub Development Kit*

Bu makalede, Azure, Azure Stack hub ve Özel Görüntü İşleme AI geliştirme seti aracılığıyla gerçek dünyada Öngörüler üreten bir çözümün nasıl dağıtılacağı açıklanır.

Bu çözümde şunları yapmayı öğreneceksiniz:

> [!div class="checklist"]
> - Bulut Yerel uygulama paketleri 'ni (CNAB) kenarda dağıtın. 
> - Bulut sınırlarına yayılan bir uygulama dağıtın.
> - Kenardaki çıkarım için Özel Görüntü İşleme AI geliştirme setini kullanın.

> [!Tip]  
> ![Hybrid-Pillars. png](./media/solution-deployment-guide-cross-cloud-scaling/hybrid-pillars.png)  
> Microsoft Azure Stack hub, Azure uzantısıdır. Azure Stack hub, bulut bilgi işlemin çevikliğini ve yeniliklerini şirket içi ortamınıza sunarak, karma uygulamaları her yerde derleyip dağıtmanıza imkan tanıyan tek karma bulutu etkinleştirir.  
> 
> [Karma uygulamalar Için tasarım konuları](overview-app-design-considerations.md) , karma uygulamalar tasarlamak, dağıtmak ve çalıştırmak için yazılım kalitesine (yerleştirme, ölçeklenebilirlik, kullanılabilirlik, dayanıklılık, yönetilebilirlik ve güvenlik) göre önemli noktalar inceler. Tasarım konuları karma uygulama tasarımını iyileştirirken, üretim ortamlarındaki zorlukları en aza indirmeyle ilgili olarak size yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar 

Bu dağıtım kılavuzunu kullanmaya başlamadan önce şunları yaptığınızdan emin olun:

- [Betfall algılama çözümüne genel bakış](pattern-retail-footfall-detection.md) 
- Azure Stack hub Development Kit (ASDK) veya Azure Stack hub ile tümleşik sistem örneğine Kullanıcı erişimini şu şekilde edinin:
  - [Azure Stack hub kaynak sağlayıcısında Azure App Service](../operator/azure-stack-app-service-overview.md) yüklendi. Azure Stack hub örneğiniz için operatör erişiminizin olması veya yöneticinizin yüklenebilmesi için yöneticinizle birlikte çalışmanız gerekir.
  - App Service ve depolama kotası sağlayan bir teklifin aboneliği. Teklif oluşturmak için operatör erişiminizin olması gerekir.
- Bir Azure aboneliğine erişim elde edin
  - Azure aboneliğiniz yoksa başlamadan önce [ücretsiz deneme hesabına](https://azure.microsoft.com/free/) kaydolun.
- Dizininizde iki hizmet sorumlusu oluşturun:
  - Azure abonelik kapsamında erişimi olan Azure kaynaklarıyla kullanılmak üzere yapılandırılmış bir. 
  - Bir tane, Azure Stack hub aboneliği kapsamında erişim ile Azure Stack hub kaynaklarıyla kullanılmak üzere yapılandırılmış. 
  - Hizmet sorumluları oluşturma ve erişimi yetkilendirme hakkında daha fazla bilgi edinmek için bkz. [kaynaklara erişmek için uygulama kimliği kullanma](../operator/azure-stack-create-service-principals.md). Azure CLı 'yı kullanmayı tercih ediyorsanız bkz. Azure [CLI Ile Azure hizmet sorumlusu oluşturma](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest).
- Azure bilişsel hizmetler 'i Azure 'da veya Azure Stack hub 'A dağıtın.
  - İlk olarak bilişsel [Hizmetler hakkında daha fazla bilgi edinin](https://azure.microsoft.com/services/cognitive-services/).
  - Daha sonra, Azure bilişsel [Hizmetler 'i Azure Stack hub 'A dağıtmayı](../user/azure-stack-solution-template-cognitive-services.md) , bilişsel hizmetler 'ı Azure Stack hub 'a dağıtmak Önce önizlemeye erişim için kaydolmanız gerekir.
- Yapılandırılmamış bir Azure Özel Görüntü İşleme AI geliştirme setini kopyalayın veya indirin. Ayrıntılar için bkz. [VISION AI DevKit](https://azure.github.io/Vision-AI-DevKit-Pages/).
- Power BI hesaba kaydolun.
- Azure bilişsel Hizmetler Yüz Tanıma API'si abonelik anahtarı ve uç nokta URL 'SI. Deneme bilişsel [Hizmetler](https://azure.microsoft.com/try/cognitive-services/?api=face-api) ücretsiz deneme sürümünü edinebilirsiniz. Veya bilişsel [Hizmetler hesabı oluşturma](/azure/cognitive-services/cognitive-services-apis-create-account)' daki yönergeleri izleyin.
- Aşağıdaki geliştirme kaynaklarını yükler:
  - [Azure CLI 2.0](../user/azure-stack-version-profiles-azurecli2.md)
  - [Docker CE](https://hub.docker.com/search/?type=edition&offering=community)
  - [Porter](https://porter.sh/). Sizin için sunulan CNAB paket bildirimlerini kullanarak bulut uygulamaları dağıtmak için bağlantı noktası kullanırsınız.
  - [Visual Studio Code](https://code.visualstudio.com/)
  - [Visual Studio Code için Azure IoT araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
  - [Visual Studio Code için Python uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
  - [Python](https://www.python.org/)

## <a name="deploy-the-hybrid-cloud-application"></a>Hibrit bulut uygulamasını dağıtma

İlk olarak, bir kimlik bilgisi kümesi oluşturmak için Porter CLı 'sını kullanın ve ardından bulut uygulamasını dağıtın.  

1. https://github.com/azure-samples/azure-intelligent-edge-patterns 'den çözüm örnek kodunu kopyalayın veya indirin. 

1. Porter, uygulamanın dağıtımını otomatikleştiren bir kimlik bilgileri kümesi oluşturur. Kimlik bilgisi oluşturma komutunu çalıştırmadan önce aşağıdakilerin kullanılabilir olduğundan emin olun:

    - Hizmet sorumlusu KIMLIĞI, anahtarı ve kiracı DNS de dahil olmak üzere Azure kaynaklarına erişmek için bir hizmet sorumlusu.
    - Azure aboneliğiniz için abonelik KIMLIĞI.
    - Hizmet sorumlusu KIMLIĞI, anahtarı ve kiracı DNS de dahil olmak üzere Azure Stack hub kaynaklarına erişmek için bir hizmet sorumlusu.
    - Azure Stack hub aboneliğiniz için abonelik KIMLIĞI.
    - Azure bilişsel hizmetlerinizin anahtar ve kaynak uç nokta URL 'SI Yüz Tanıma API'si.

1. Porter kimlik bilgileri oluşturma işlemini çalıştırın ve istemleri izleyin:

   ```porter
   porter creds generate --tag intelligentedge/footfall-cloud-deployment:0.1.0
   ```

1. Porter, çalıştırılacak bir parametre kümesi de gerektirir. Bir parametre metin dosyası oluşturun ve aşağıdaki ad/değer çiftlerini girin. Gerekli değerlerden herhangi biri ile ilgili yardıma ihtiyacınız varsa Azure Stack hub yöneticinize danışın.

   > [!NOTE] 
   > `resource suffix` değeri, dağıtımınızın kaynaklarının Azure genelinde benzersiz adlara sahip olduğundan emin olmak için kullanılır. 8 karakterden uzun olmayan harflerin ve sayıların benzersiz bir dizesi olmalıdır.

    ```
    azure_stack_tenant_arm="Your Azure Stack Hub tenant endpoint"
    azure_stack_storage_suffix="Your Azure Stack Hub storage suffix"
    azure_stack_keyvault_suffix="Your Azure Stack Hub keyVault suffix"
    resource_suffix="A unique string to identify your deployment"
    azure_location="A valid Azure region"
    azure_stack_location="Your Azure Stack Hub location identifier"
    powerbi_display_name="Your first and last name"
    powerbi_principal_name="Your Power BI account email address"
    ```
   Metin dosyasını kaydedin ve yolunu bir yere göz önünde yapın.

1. Artık, hiter kullanarak karma bulut uygulamasını dağıtmaya hazırsınız demektir. Install komutunu çalıştırın ve kaynaklar Azure 'da ve Azure Stack hub 'a dağıtılırken izleyin:

    ```porter
    porter install footfall-cloud –tag intelligentedge/footfall-cloud-deployment:0.1.0 –creds footfall-cloud-deployment –param-file "path-to-cloud-parameters-file.txt"
    ```

1. Dağıtım tamamlandıktan sonra, aşağıdaki değerleri unutmayın:
    - Kameranın bağlantı dizesi.
    - Görüntü depolama hesabı bağlantı dizesi.
    - Kaynak grubu adları.

## <a name="prepare-the-custom-vision-ai-dev-kit"></a>Özel Görüntü İşleme AI geliştirme setini hazırlama

Ardından, Özel Görüntü İşleme AI geliştirme setini, [VISION AI DevKit hızlı başlangıç](https://azure.github.io/Vision-AI-DevKit-Pages/docs/quick_start/)bölümünde gösterildiği gibi ayarlayın. Ayrıca, önceki adımda belirtilen bağlantı dizesini kullanarak kameranızı ayarlayıp test edersiniz.

## <a name="deploy-the-camera-application"></a>Kamera uygulamasını dağıtma

Bir kimlik bilgisi kümesi oluşturmak için Porter CLı 'sını kullanın, ardından Kamera uygulamasını dağıtın.

1. Porter, uygulamanın dağıtımını otomatikleştiren bir kimlik bilgileri kümesi oluşturur. Kimlik bilgisi oluşturma komutunu çalıştırmadan önce aşağıdakilerin kullanılabilir olduğundan emin olun:
    
    - Hizmet sorumlusu KIMLIĞI, anahtarı ve kiracı DNS de dahil olmak üzere Azure kaynaklarına erişmek için bir hizmet sorumlusu.
    - Azure aboneliğiniz için abonelik KIMLIĞI.
    - Bulut uygulamasını dağıtırken belirtilen görüntü depolama hesabı bağlantı dizesi.

1. Porter kimlik bilgileri oluşturma işlemini çalıştırın ve istemleri izleyin:

    ```porter
    porter creds generate --tag intelligentedge/footfall-camera-deployment:0.1.0
    ```

1. Porter, çalıştırılacak bir parametre kümesi de gerektirir. Bir parametre metin dosyası oluşturun ve aşağıdaki metni girin. Gerekli değerlerden bazılarını bilmiyorsanız Azure Stack hub yöneticinize danışın.

    > [!NOTE]
    > `deployment suffix` değeri, dağıtımınızın kaynaklarının Azure genelinde benzersiz adlara sahip olduğundan emin olmak için kullanılır. 8 karakterden uzun olmayan harflerin ve sayıların benzersiz bir dizesi olmalıdır.

    ```
    iot_hub_name="Name of the IoT Hub deployed"
    deployment_suffix="Unique string here"
    ```

    Metin dosyasını kaydedin ve yolunu bir yere göz önünde yapın.

4. Artık, bağlantı noktası kullanarak Kamera uygulamasını dağıtmaya hazırsınız demektir. Install komutunu çalıştırın ve IoT Edge dağıtımı oluşturulduğundan izleyin.

    ```porter
    porter install footfall-camera –tag intelligentedge/footfall-camera-deployment:0.1.0 –creds footfall-camera-deployment –param-file "path-to-camera-parameters-file.txt"
    ```

5. Kameranın dağıtımının, `https://<camera-ip>:3000/`adresindeki kamera akışını görüntüleyerek, `<camara-ip>` kameranın IP adresinin olduğunu doğrulayın. Bu adım 10 dakikaya kadar sürebilir.

## <a name="configure-azure-stream-analytics"></a>Azure Stream Analytics Yapılandır

Artık veriler kameradan Azure Stream Analytics akar, Power BI ile iletişim kurmak için el ile yetkilendirmeniz gerekir.

1.  Azure portal **tüm kaynakları**açın ve tüm *son ek\]iş\[işlem* .

2.  Stream Analytics iş bölmesinin **İş Topolojisi** bölümünde **Çıkışlar** seçeneğini belirleyin.

3.  **Trafik çıkışı** çıkış havuzunu seçin.

4.  **Yetkilendirmeyi Yenile** ve Power BI hesabınızda oturum aç ' ı seçin.
    
    ![Yetkilendirme isteğini Yenile](./media/solution-deployment-guide-retail-footfall-detection/image2.png)

5.  Çıkış ayarlarını kaydedin.

6.  **Genel bakış** bölmesine gidin ve Power BI veri göndermeye başlamak için **Başlat** ' ı seçin.

7.  İş çıkışı başlangıç saati için **Şimdi**’yi seçip **Başlat** seçeneğini belirleyin. İş durumunu bildirim çubuğunda durumu görüntüleyebilirsiniz.

## <a name="create-a-power-bi-dashboard"></a>Power BI panosu oluşturma

1.  İş başarıyla tamamlandıktan sonra [Power BI](https://powerbi.com/)'a gidip iş veya okul hesabınızla oturum açın. Stream Analytics iş sorgusu sonuçları alıyorsa, oluşturduğunuz alt *veri* kümesi veri kümesi veri **kümeleri** sekmesinde bulunur.

2.  Power BI çalışma alanınızdan, *Ptfall Analizi* adlı yeni bir pano oluşturmak Için **+ Oluştur** ' u seçin.

3.  Pencerenin üst kısmındaki **Kutucuk ekle**’yi seçin. Ardından **Özel Akış Verileri**'ni ve **İleri**'yi seçin. **Veri kümeleriniz**altındaki alt **veri kümesini** seçin. **Görselleştirme türü** açılan menüsünden **kart** ' ı seçin ve alanlara **yaş** ekleyin. **İleri**'yi seçip ad belirledikten sonra **Uygula**'yı seçerek kutucuğu oluşturun.

4.  İstediğiniz gibi ek alanlar ve kartlar ekleyebilirsiniz.

## <a name="test-your-solution"></a>Çözümünüzü test etme

Farklı kişiler kameranın önüne ilerleyirken Power BI ' de oluşturduğunuz karttaki verilerin değiştiğini gözlemleyin. Inele, kaydedildikten sonra görünmesi 20 saniyeye kadar sürebilir.

## <a name="remove-your-solution"></a>Çözümünüzü kaldırma

Çözümünüzü kaldırmak isterseniz, dağıtım için oluşturduğunuz aynı parametre dosyalarını kullanarak, Porter kullanarak aşağıdaki komutları çalıştırın: 

```porter
porter uninstall footfall-cloud –tag intelligentedge/footfall-cloud-deployment:0.1.0 –creds footfall-cloud-deployment –param-file "path-to-cloud-parameters-file.txt"

porter uninstall footfall-camera –tag intelligentedge/footfall-camera-deployment:0.1.0 –creds footfall-camera-deployment –param-file "path-to-camera-parameters-file.txt"
```
## <a name="next-steps"></a>Sonraki adımlar

- [Hibrit bulut uygulaması tasarım konuları](overview-app-design-considerations.md) hakkında daha fazla bilgi
- [GitHub 'da Bu örnek için koda](https://github.com/Azure-Samples/azure-intelligent-edge-patterns/tree/master/footfall-analysis)yönelik geliştirmeleri gözden geçirin ve önerin.
