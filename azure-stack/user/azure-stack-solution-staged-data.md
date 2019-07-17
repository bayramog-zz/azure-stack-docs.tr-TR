---
title: Azure Stack için hazırlanmış veri analizi çözümü dağıtma | Microsoft Docs
description: Azure Stack için hazırlanmış veri analizi çözümü dağıtmayı öğrenin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 06/20/2019
ms.author: mabrigg
ms.reviewer: anajod
ms.lastreviewed: 06/20/2019
ms.openlocfilehash: ca4c2480fff511ab3bad43ea82fc81522d9afba0
ms.sourcegitcommit: 2a4cb9a21a6e0583aa8ade330dd849304df6ccb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68286738"
---
# <a name="deploy-a-staged-data-analytics-solution-to-azure-stack"></a>Azure Stack için hazırlanmış veri analizi çözümü dağıtma

Bu makale koleksiyonu noktasında gerekir ve böylece hızlı kararlar yapılabilmesi için veri toplamak için bir çözümün nasıl dağıtılacağı gösterir. Genellikle bu veri toplama, Internet erişimi oluşur. Bağlantı kurulduğunda verilere ek Öngörüler elde etmek için bir kaynak kullanımı yoğun çözümlemesi gerekebilir.

Bu çözümde bir örnek ortama oluşturacaksınız:

> [!div class="checklist"]
> - Ham veri depolama blob'u oluşturun.
> - Verileri temizleme Azure yığını, Azure'a taşımak için yeni bir Azure Stack işlevi oluşturun.
> - Blob Depolama ile tetiklenen bir işlev oluşturun.
> - Azure Stack, blob ve kuyruk içeren bir depolama hesabı oluşturun.
> - Kuyruk ile tetiklenen bir işlev oluşturun.
> - Test kuyruk ile tetiklenen işlev.

> [!Tip]  
> ![karma pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack, Azure'nın bir uzantısıdır. Azure Stack, hibrit uygulamaları her yerde oluşturup dağıtmayı olanak tanıyan tek hibrit Bulutu çevikliğini ve yenilik, şirket içi ortamınıza bulut getirir.  
> 
> Makaleyi [karma uygulamaları için tasarım konuları](azure-stack-edge-pattern-overview.md) (yerleştirme, ölçeklenebilirlik, kullanılabilirlik, dayanıklılık, yönetilebilirlik ve güvenlik) yazılım kalitesinin yapı taşları tasarlama, dağıtma ve karma çalıştırma için gözden geçirmeleri uygulamalar. Tasarım konuları, karma uygulama tasarımı, üretim ortamlarında sorunlarını en aza en iyi duruma getirme yardımcı olur.

## <a name="architecture-for-staged-data-analytics"></a>Hazırlanmış verinin analytics mimarisi

![hazırlanmış verinin analizi](media/azure-stack-solution-staged-data/image1.png)

## <a name="prerequisites-for-staged-data-analytics"></a>Hazırlanmış veri analizi için Önkoşullar

  - Azure aboneliği.
  - Azure ve Azure Stack Kiracı aboneliği için izinlere sahip bir Azure Active Directory (AAD) hizmet sorumlusu. Azure Stack, Azure aboneliğiniz daha farklı bir AAD kiracısı kullanıyorsa, iki hizmet sorumluları oluşturmanız gerekebilir. Azure Stack için hizmet sorumlusu oluşturma konusunda bilgi almak için Git [uygulamaları Azure Stack kaynaklarına erişmenizi sağlayacak bir hizmet sorumlusu oluşturma](https://docs.microsoft.com/azure-stack/user/azure-stack-create-service-principals).
      - **Her hizmet sorumlusunun uygulama kimliği, istemci parolası, Azure AD Kiracı kimliği ve Kiracı adı (xxxxx.onmicrosoft.com) not edin.**
  - Veri analizi için veri koleksiyonunu sağlamanız gerekir. Örnek verileri sağlanır.
  - [Windows için docker](https://docs.docker.com/docker-for-windows/) yerel makinenizde yüklü.

## <a name="get-the-docker-image"></a>Docker görüntüsünü Al

Her dağıtım için docker görüntülerini, Azure PowerShell farklı sürümleri arasında bağımlılık sorunlarını ortadan kaldırın.
1.  Windows kapsayıcıları için Docker Windows kullandığından emin olun.
2.  Dağıtım betikleri ile Docker kapsayıcısı almak için yükseltilmiş bir komut isteminde aşağıdaki komutu çalıştırın.

```
 docker pull intelligentedge/stageddatasolution:1.0.0
```

## <a name="deploy-the-solution"></a>Çözümü dağıtma

1.  Kapsayıcı görüntüsünü başarıyla çekilen sonra görüntüyü başlatın.

      ```powershell  
      docker run -it intelligentedge/stageddatasolution:1.0.0 powershell
      ```

2.  Kapsayıcı başladıktan sonra yükseltilmiş bir PowerShell terminalde kapsayıcı verilir. Dağıtım betiğine almak için dizinleri değiştirin.

      ```powershell  
      cd .\SDDemo\
      ```

3.  Dağıtımı'nı çalıştırın. Kimlik bilgileri ve kaynak sağlamanız gerektiğinde adları. HA HA kümenin dağıtılacağı Azure Stack ve DR kümenin dağıtılacağı Azure Stack için DR ifade eder.

      ```powershell
      .\DeploySolution-Azure-AzureStack.ps1 `
      -AzureApplicationId "applicationIDforAzureServicePrincipal" `
      -AzureApplicationSercet "clientSecretforServicePrincipal" `
      -AzureTenantId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" `
      -AzureStackAADTenantName "azurestacktenant.onmicrosoft.com" `
      -AzureStackTenantARMEndpoint "https://management.haazurestack.com" `
      -AzureStackApplicationId "applicationIDforStackServicePrincipal" `
      -AzureStackApplicationSercet "ClientSecretforStackServicePrincipal" `
      -AzureStackTenantId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" `
      -ResourcePrefix "aPrefixForResources"
      ```

1.  İstenirse; Application Insights ve Azure dağıtım için bir bölge girin.

2.  ", Azure ve Azure Stack dağıtıma izin vermek için yüklenecek API profili"2018-03-01-karma"modülleri kazandırın yüklenmesi NuGet sağlayıcısı izin vermek için Y" yazın.

3.  Kaynakları dağıtıldıktan sonra Azure Stack ve Azure için verileri oluşturulacak test edin.

    ```powershell  
      .\TDAGenerator.exe
    ```

4.  Azure veya Azure Stack dağıtılan web uygulamaları giderek işlenen verilere bakın.

### <a name="azure-web-app"></a>Azure Web Uygulaması
 
![hazırlanmış veri analizi çözümü](media/azure-stack-solution-staged-data/image2.png)
 
### <a name="azure-stack-web-app"></a>Azure Stack Web uygulaması
 
![Azure Stack için hazırlanmış veri analizi çözümü](media/azure-stack-solution-staged-data/image3.png)

## <a name="next-steps"></a>Sonraki adımlar

  - Hibrit bulut uygulamaları hakkında daha fazla bilgi için bkz: [hibrit bulut çözümleri.](https://aka.ms/azsdevtutorials)

  - Bu örnek için kodu değiştirin veya kendi verilerinizi kullanmak [GitHub](https://github.com/Azure-Samples/azure-intelligent-edge-patterns).
