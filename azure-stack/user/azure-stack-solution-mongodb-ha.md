---
title: Yüksek oranda kullanılabilir bir MongoDB çözümü için Azure ve Azure Stack dağıtma | Microsoft Docs
description: Azure ve Azure Stack için yüksek oranda kullanılabilir bir MongoDB çözüm dağıtmayı öğrenin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: mabrigg
ms.reviewer: anajod
ms.lastreviewed: 06/20/2019
ms.openlocfilehash: 77d0b7c856e594e7e97ad30085419bfac13bd4f7
ms.sourcegitcommit: 104ccafcb72a16ae7e91b154116f3f312321cff7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67308902"
---
# <a name="tutorial-deploy-a-highly-available-mongodb-solution-to-azure-and-azure-stack"></a>Öğretici: Azure ve Azure Stack için yüksek oranda kullanılabilir bir MongoDB çözümü dağıtma

Bu makalede bir otomatik dağıtım ile bir olağanüstü durum kurtarma (DR) site temel bir yüksek oranda kullanılabilir (HA) MongoDB kümenin iki Azure Stack ortamlarında adım adım. MongoDB ve yüksek kullanılabilirlik hakkında daha fazla bilgi için bkz: [çoğaltma kümesi üye](https://docs.mongodb.com/manual/core/replica-set-members/).

Bu öğreticide, bir örnek ortama oluşturacaksınız:

> [!div class="checklist"]
> - İki Azure yığınları arasında dağıtımı düzenleyin
> - Azure API profilleriyle bağımlılık sorunlarını en aza indirmek için Docker'ı kullanma
> - Bir olağanüstü durum kurtarma siteniz ile temel bir yüksek oranda kullanılabilir MongoDB küme dağıtma


> [!Tip]  
> ![karma pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack, Azure'nın bir uzantısıdır. Azure Stack çevikliğini ve yenilik, şirket içi ortamınıza bulut getirir ve hibrit uygulamaları her yerde oluşturup dağıtmayı olanak tanıyan tek hibrit Bulutu sağlar.  
> 
> Teknik incelemeyi [karma uygulamaları için tasarım konuları](https://aka.ms/hybrid-cloud-applications-pillars) yazılım kalitesinin yapı taşları tasarlama, dağıtma ve çalıştırma için (yerleştirme, ölçeklenebilirlik, kullanılabilirlik, dayanıklılık, yönetilebilirlik ve güvenlik) gözden geçirmeleri karma uygulamalar. Tasarım konuları, üretim ortamlarında sorunlarını en aza karma uygulama tasarımının en iyi duruma getirme yardımcı olur.



## <a name="architecture-for-mongodb-with-azure-stack"></a>Azure Stack ile MongoDB için mimarisi

![Azure stack'teki yüksek oranda kullanılabilir MongoDB](media/azure-stack-solution-mongdb-ha/image1.png)

## <a name="prerequisites-for-mongodb-with-azure-stack"></a>Azure Stack ile MongoDB için Önkoşullar

  - İki bağlı Azure Stack tümleşik sistemleri (Azure Stack), bu dağıtımı, Azure Stack geliştirme setleri (ASDKs) çalışmıyor. Azure Stack hakkında daha fazla bilgi için bkz: [Azure Stack nedir?](https://azure.microsoft.com/overview/azure-stack/)
      - Her Azure Stack Kiracı abonelikte.    
      - **Her abonelik kimliği ve Azure Resource Manager uç nokta her Azure Stack için not edin.**
  - Her Azure Stack üzerinde Kiracı aboneliği için izinlere sahip bir Azure Active Directory (Azure AD) hizmet sorumlusu. Azure yığını, farklı Azure AD'de bir dağıttıysanız, iki hizmet sorumlusu oluşturma gerekebilir kiracılar. Azure Stack için hizmet sorumlusu oluşturma konusunda bilgi almak için bkz: [uygulamaları Azure Stack kaynaklarına erişmenizi sağlayacak bir hizmet sorumlusu oluşturma](https://docs.microsoft.com/azure-stack/user/azure-stack-create-service-principals).    
      - **Her hizmet sorumlusunun uygulama kimliği, istemci gizli anahtarı ve Kiracı adı (xxxxx.onmicrosoft.com) not edin.**
  - Ubuntu 16.04 her Azure yığını'nın Market'te genel olarak. Market dağıtım hakkında daha fazla bilgi için bkz: [indirme Market öğesi Azure'dan Azure Stack'e](https://docs.microsoft.com/azure-stack/operator/azure-stack-download-azure-marketplace-item).
  - [Windows için docker](https://docs.docker.com/docker-for-windows/) yerel makinenizde yüklü.

## <a name="get-the-docker-image"></a>Docker görüntüsünü Al

Her dağıtım için docker görüntülerini, Azure PowerShell farklı sürümleri arasında bağımlılık sorunlarını ortadan kaldırın.
1.  Windows kapsayıcıları için Docker Windows kullandığından emin olun.
2.  Dağıtım betikleri ile Docker kapsayıcısı almak için yükseltilmiş bir komut isteminde aşağıdaki komutu çalıştırın.
```powershell  
docker pull intelligentedge/mongodb-hadr:1.0.0
```

## <a name="deploy-the-clusters"></a>Küme dağıtma

1.  Kapsayıcı görüntüsünü başarıyla çekilen bir kez başlatma resmi. \

    ```powershell  
    docker run -it intelligentedge/mongodb-hadr:1.0.0 powershell
    ```

2.  Kapsayıcı başladıktan sonra yükseltilmiş bir PowerShell terminalde kapsayıcı verilir. Dağıtım betiğine almak için dizinleri değiştirin.

    ```powershell  
    cd .\MongoHADRDemo\
    ```

3.  Dağıtımı'nı çalıştırın. Kimlik bilgileri ve kaynak sağlamanız gerektiğinde adları. HA HA kümenin dağıtılacağı Azure Stack ve DR kümenin dağıtılacağı Azure Stack için DR ifade eder.

    ```powershell
    .\Deploy-AzureResourceGroup.ps1 `
    -AzureStackApplicationId_HA "applicationIDforHAServicePrincipal" `
    -AzureStackApplicationSercet_HA "clientSecretforHAServicePrincipal" `
    -AADTenantName_HA "hatenantname.onmicrosoft.com" `
    -AzureStackResourceGroup_HA "haresourcegroupname" `
    -AzureStackArmEndpoint_HA "https://management.haazurestack.com" `
    -AzureStackSubscriptionId_HA "haSubscriptionId" `
    -AzureStackApplicationId_DR "applicationIDforDRServicePrincipal" `
    -AzureStackApplicationSercet_DR "ClientSecretforDRServicePrincipal" `
    -AADTenantName_DR "drtenantname.onmicrosoft.com" `
    -AzureStackResourceGroup_DR "drresourcegroupname" `
    -AzureStackArmEndpoint_DR "https://management.drazurestack.com" `
    -AzureStackSubscriptionId_DR "drSubscriptionId"
    ```

4.  Tür `Y` yüklenmesi için API profili "2018-03-01-karma" modülleri başlatır yüklenmesi NuGet sağlayıcısı izin vermek için.

5.  HA kaynakların ilk dağıtır. Dağıtımını izlemek ve tamamlanmasını bekleyin. HA dağıtım tamamlandığında, dağıtılan kaynakları görmek için portalı HA Azure yığını'nın denetleyebilirsiniz belirten ileti olduğunda. 

6.  DR kaynakların dağıtım ile devam edin ve kümeyle etkileşim kurmak için DR Azure Stack'te bir Sıçrama kutusu etkinleştirmek isteyip istemediğinize karar verin.

7.  DR kaynak dağıtımın tamamlanmasını bekleyin.

8.  DR kaynak dağıtım tamamlandıktan sonra kapsayıcı çıkın.

  ```powershell
  exit
  ```

## <a name="next-steps"></a>Sonraki adımlar

  - Atlama kutusunda DR Azure Stack'te bir VM etkinleştirilirse, SSH ile bağlanma ve mongo CLI'yı yükleyerek MongoDB kümeyle etkileşimde. MongoDB ile etkileşim kurma hakkında daha fazla bilgi için bkz: [mongo kabuğunu](https://docs.mongodb.com/manual/mongo/).

  - Hibrit bulut uygulamaları hakkında daha fazla bilgi için bkz: [hibrit bulut çözümleri.](https://aka.ms/azsdevtutorials)

  - Bu örnek için kodu değiştirin [GitHub](https://github.com/Azure-Samples/azure-intelligent-edge-patterns).