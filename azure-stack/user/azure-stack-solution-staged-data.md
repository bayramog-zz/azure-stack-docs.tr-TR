---
title: Azure Stack için hazırlanmış bir veri analizi çözümü dağıtma | Microsoft Docs
description: Azure Stack için hazırlanmış bir veri analizi çözümünü dağıtmayı öğrenin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.topic: conceptual
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/24/2019
ms.author: mabrigg
ms.reviewer: anajod
ms.lastreviewed: 06/20/2019
ms.openlocfilehash: 4d93e16a46efad33c4392e40ca3a79e89a14795b
ms.sourcegitcommit: e6a738f674634e1d5dd4eb23b6c44b660ea2fe84
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2019
ms.locfileid: "72891287"
---
# <a name="deploy-a-staged-data-analytics-solution-to-azure-stack"></a>Azure Stack için hazırlanmış bir veri analizi çözümü dağıtın

Bu makalede, hızlı kararların yapılabilmesi için koleksiyon noktasında analiz gerektiren verileri toplamaya yönelik bir çözümün nasıl dağıtılacağı gösterilir. Genellikle bu veri koleksiyonu Internet erişimi olmadan oluşur. Bağlantı oluşturulduğunda, ek Öngörüler kazanmak için verilerin yoğun kaynak bir analizini yapmanız gerekebilir.

Bu çözümde şu şekilde bir örnek ortam oluşturacaksınız:

> [!div class="checklist"]
> - Ham veri depolama blobu oluşturun.
> - Temiz verileri Azure Stack Azure 'a taşımak için yeni bir Azure Stack Işlevi oluşturun.
> - BLOB depolama ile tetiklenen bir işlev oluşturun.
> - Blob ve kuyruk içeren Azure Stack depolama hesabı oluşturun.
> - Kuyruğa geçirilmiş bir işlev oluşturun.
> - Kuyruğa tetiklenen işlevi test edin.

> [!Tip]  
> ![Hybrid-Pillars. png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack bir Azure uzantısıdır. Azure Stack, bulut bilgi işlemin çevikliğini ve yeniliklerini şirket içi ortamınıza getirerek, karma uygulamaları her yerde oluşturup dağıtmanıza imkan tanıyan tek karma bulutu etkinleştirir.  
> 
> [Karma uygulamalar Için tasarım konuları](azure-stack-edge-pattern-overview.md) , karma uygulamalar tasarlamak, dağıtmak ve çalıştırmak için yazılım kalitesine (yerleştirme, ölçeklenebilirlik, kullanılabilirlik, dayanıklılık, yönetilebilirlik ve güvenlik) göre önemli noktalar inceler. Tasarım konuları karma uygulama tasarımını iyileştirirken, üretim ortamlarındaki zorlukları en aza indirmeyle ilgili olarak size yardımcı olur.

## <a name="architecture-for-staged-data-analytics"></a>Hazırlanan veri analizi için mimari

![hazırlanan veri analizi](media/azure-stack-solution-staged-data/image1.png)

## <a name="prerequisites-for-staged-data-analytics"></a>Hazırlanan veri analizi önkoşulları

  - Azure aboneliği.
  - Azure ve Azure Stack kiracı aboneliğine yönelik izinlere sahip bir Azure Active Directory (Azure AD) hizmet sorumlusu. Azure Stack Azure aboneliğinizden farklı bir AAD kiracısı kullanıyorsa, iki hizmet sorumlusu oluşturmanız gerekebilir. Azure Stack için hizmet sorumlusu oluşturma hakkında bilgi edinmek için, [uygulamalara Azure Stack kaynaklarına erişim sağlamak için hizmet sorumluları oluşturun](https://docs.microsoft.com/azure-stack/user/azure-stack-create-service-principals)bölümüne gidin.
      - **Her bir hizmet sorumlusunun uygulama KIMLIĞI, istemci parolası, Azure AD kiracı KIMLIĞI ve kiracı adı (xxxxx.onmicrosoft.com) gibi bir yere dikkat edin.**
  - Veri analizi için bir veri koleksiyonu sağlamanız gerekir. Örnek veriler sağlanır.
  - Yerel makinenizde yüklü [Docker for Windows](https://docs.docker.com/docker-for-windows/) .

## <a name="get-the-docker-image"></a>Docker görüntüsünü al

Her dağıtım için Docker görüntüleri farklı Azure PowerShell sürümleri arasındaki bağımlılık sorunlarını ortadan kaldırır.
1.  Docker for Windows Windows kapsayıcıları ' nı kullandığınızdan emin olun.
2.  Dağıtım betikleri ile Docker kapsayıcısını almak için yükseltilmiş bir komut isteminde aşağıdakileri çalıştırın.

```
 docker pull intelligentedge/stageddatasolution:1.0.0
```

## <a name="deploy-the-solution"></a>Çözümü dağıtma

1.  Kapsayıcı görüntüsü başarıyla çekildikten sonra görüntüyü başlatın.

      ```powershell  
      docker run -it intelligentedge/stageddatasolution:1.0.0 powershell
      ```

2.  Kapsayıcı başlatıldıktan sonra, kapsayıcıda yükseltilmiş bir PowerShell terminali vermiş olursunuz. Dağıtım betiğine ulaşmak için dizinleri değiştirin.

      ```powershell  
      cd .\SDDemo\
      ```

3.  Dağıtımı çalıştırın. Gereken yerlerde kimlik bilgileri ve kaynak adları sağlayın. HA, HA kümesinin dağıtılacağı Azure Stack ve Dr kümesinin dağıtılacağı Azure Stack DR anlamına gelir.

      ```powershell
      .\DeploySolution-Azure-AzureStack.ps1 `
      -AzureApplicationId "applicationIDforAzureServicePrincipal" `
      -AzureApplicationSecret "clientSecretforServicePrincipal" `
      -AzureTenantId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" `
      -AzureStackAADTenantName "azurestacktenant.onmicrosoft.com" `
      -AzureStackTenantARMEndpoint "https://management.haazurestack.com" `
      -AzureStackApplicationId "applicationIDforStackServicePrincipal" `
      -AzureStackApplicationSecret "ClientSecretforStackServicePrincipal" `
      -AzureStackTenantId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" `
      -ResourcePrefix "aPrefixForResources"
      ```

1.  İstenirse; Azure dağıtımı için bir bölge girin ve Application Insights.

2.  NuGet sağlayıcısının yüklenmesine izin vermek için "Y" yazın. Bu, Azure ve Azure Stack dağıtımına izin vermek üzere yüklenecek "2018-03-01-karma" modüllerinin API profilini devre dışı bırakır.

3.  Kaynaklar dağıtıldıktan sonra, verilerin hem Azure Stack hem de Azure için oluşturulacağını test edin.

    ```powershell  
      .\TDAGenerator.exe
    ```

4.  Azure 'a veya Azure Stack dağıtılan Web uygulamalarına giderek işlenmekte olan verilere bakın.

### <a name="azure-web-app"></a>Azure Web Uygulaması
 
![hazırlanan veri analizi çözümü](media/azure-stack-solution-staged-data/image2.png)
 
### <a name="azure-stack-web-app"></a>Azure Stack Web uygulaması
 
![Azure Stack için hazırlanan veri analizi çözümü](media/azure-stack-solution-staged-data/image3.png)

## <a name="next-steps"></a>Sonraki adımlar

  - Hibrit bulut uygulamaları hakkında daha fazla bilgi için bkz [. karma bulut çözümleri.](https://aka.ms/azsdevtutorials)

  - Kendi verilerinizi kullanın veya [GitHub](https://github.com/Azure-Samples/azure-intelligent-edge-patterns)'da bu örnekteki kodu değiştirin.
