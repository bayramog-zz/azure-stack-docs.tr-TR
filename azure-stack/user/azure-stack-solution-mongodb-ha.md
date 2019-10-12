---
title: Yüksek oranda kullanılabilir bir MongoDB çözümünü Azure 'a dağıtma ve Azure Stack | Microsoft Docs
description: Yüksek oranda kullanılabilir bir MongoDB çözümünü Azure 'a ve Azure Stack dağıtmayı öğrenin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/10/2019
ms.author: mabrigg
ms.reviewer: anajod
ms.lastreviewed: 06/20/2019
ms.openlocfilehash: e31a71098cda4f769fbce4e55bec6ec72a5a651b
ms.sourcegitcommit: a6d47164c13f651c54ea0986d825e637e1f77018
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72277768"
---
# <a name="deploy-a-highly-available-mongodb-solution-to-azure-and-azure-stack"></a>Yüksek oranda kullanılabilir bir MongoDB çözümünü Azure 'a ve Azure Stack dağıtma

Bu makale, yüksek oranda kullanılabilir (HA) MongoDB kümesinin iki Azure Stack ortamında olağanüstü durum kurtarma (DR) sitesi ile otomatikleştirilmiş dağıtımında size adım adım kılavuzluk eder. MongoDB ve yüksek kullanılabilirlik hakkında daha fazla bilgi edinmek için bkz. [çoğaltma kümesi üyeleri](https://docs.mongodb.com/manual/core/replica-set-members/).

Bu çözümde şu şekilde bir örnek ortam oluşturacaksınız:

> [!div class="checklist"]
> - İki Azure yığını genelinde bir dağıtımı düzenleme
> - Azure API profilleriyle ilgili bağımlılık sorunlarını en aza indirmek için Docker kullanma
> - Olağanüstü durum kurtarma sitesiyle temel yüksek oranda kullanılabilir MongoDB kümesi dağıtma


> [!Tip]  
> @no__t -0hybrid-pil,. png @ no__t-1  
> Microsoft Azure Stack bir Azure uzantısıdır. Azure Stack, bulut bilgi işlemin çevikliğini ve yeniliklerini şirket içi ortamınıza getirerek, karma uygulamaları her yerde oluşturup dağıtmanıza imkan tanıyan tek karma bulutu etkinleştirir.  
> 
> [Karma uygulamalar Için tasarım konuları](azure-stack-edge-pattern-overview.md) , karma uygulamalar tasarlamak, dağıtmak ve çalıştırmak için yazılım kalitesine (yerleştirme, ölçeklenebilirlik, kullanılabilirlik, dayanıklılık, yönetilebilirlik ve güvenlik) göre önemli noktalar inceler. Tasarım konuları karma uygulama tasarımını iyileştirirken, üretim ortamlarındaki zorlukları en aza indirmeyle ilgili olarak size yardımcı olur.



## <a name="architecture-for-mongodb-with-azure-stack"></a>Azure Stack ile MongoDB mimarisi

![Azure Stack 'de yüksek oranda kullanılabilir MongoDB](media/azure-stack-solution-mongdb-ha/image1.png)

## <a name="prerequisites-for-mongodb-with-azure-stack"></a>Azure Stack ile MongoDB önkoşulları

  - İki bağlı Azure Stack tümleşik sistem (Azure Stack), bu dağıtım Azure Stack geliştirme setleri (aks) üzerinde çalışmaz. Azure Stack hakkında daha fazla bilgi edinmek için bkz. [Azure Stack nedir?](https://azure.microsoft.com/overview/azure-stack/)
      - Her bir Azure Stack kiracı aboneliği.    
      - **Her bir Azure Stack için her abonelik KIMLIĞINI ve Azure Resource Manager uç noktasını bir yere unutmayın.**
  - Her bir Azure Stack kiracı aboneliği için izinleri olan bir Azure Active Directory (Azure AD) hizmet sorumlusu. Azure yığınları farklı Azure AD Kiracılarına karşı dağıtılırsa iki hizmet sorumlusu oluşturmanız gerekebilir. Azure Stack için hizmet sorumlusu oluşturma hakkında bilgi edinmek için bkz. [hizmet sorumlularını oluşturma, uygulamalara Azure Stack kaynakları erişimi sağlamak için](https://docs.microsoft.com/azure-stack/user/azure-stack-create-service-principals).    
      - **Her bir hizmet sorumlusunun uygulama KIMLIĞI, gizli anahtar ve kiracı adını (xxxxx.onmicrosoft.com) bir yere unutmayın.**
  - Ubuntu 16,04 her bir Azure Stack Market 'e göre dağıtılmış. Market dağıtımı hakkında daha fazla bilgi edinmek için bkz. [Azure 'dan Market öğelerini indirme Azure Stack](https://docs.microsoft.com/azure-stack/operator/azure-stack-download-azure-marketplace-item).
  - Yerel makinenizde yüklü [Docker for Windows](https://docs.docker.com/docker-for-windows/) .

## <a name="get-the-docker-image"></a>Docker görüntüsünü al

Her dağıtım için Docker görüntüleri farklı Azure PowerShell sürümleri arasındaki bağımlılık sorunlarını ortadan kaldırır.
1.  Docker for Windows Windows kapsayıcıları ' nı kullandığınızdan emin olun.
2.  Dağıtım betikleri ile Docker kapsayıcısını almak için yükseltilmiş bir komut isteminde aşağıdakileri çalıştırın.
```powershell  
docker pull intelligentedge/mongodb-hadr:1.0.0
```

## <a name="deploy-the-clusters"></a>Kümeleri dağıtma

1.  Kapsayıcı görüntüsü başarıyla çekildikten sonra görüntüyü başlatın. \

    ```powershell  
    docker run -it intelligentedge/mongodb-hadr:1.0.0 powershell
    ```

2.  Kapsayıcı başlatıldıktan sonra, kapsayıcıda yükseltilmiş bir PowerShell terminali vermiş olursunuz. Dağıtım betiğine ulaşmak için dizinleri değiştirin.

    ```powershell  
    cd .\MongoHADRDemo\
    ```

3.  Dağıtımı çalıştırın. Gereken yerlerde kimlik bilgileri ve kaynak adları sağlayın. HA, HA kümesinin dağıtılacağı Azure Stack ve Dr kümesinin dağıtılacağı Azure Stack DR anlamına gelir.

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

4.  NuGet sağlayıcısının yüklenmesine izin vermek için `Y` yazın ve bu, yüklenecek "2018-03-01-hibrit" modülleriyle başlatılır.

5.  HA kaynakları ilk olarak dağıtılır. Dağıtımı izleyin ve tamamlanmasını bekleyin. HA dağıtımının tamamlandığını belirten iletiyi aldıktan sonra, dağıtılan kaynakları görmek için HA Azure Stack portalını kontrol edebilirsiniz. 

6.  DR kaynaklarının dağıtımına devam edin ve kümeyle etkileşimde bulunmak için DR Azure Stack bir sıçrama kutusunu etkinleştirmek istediğinize karar verin.

7.  DR Kaynak dağıtımının tamamlanmasını bekleyin.

8.  DR kaynak dağıtımı tamamlandıktan sonra kapsayıcıdan çıkın.

  ```powershell
  exit
  ```

## <a name="next-steps"></a>Sonraki adımlar

  - DR Azure Stack üzerinde geçiş kutusu sanal makinesini etkinleştirdiyseniz, Mongo CLı 'yı yükleyerek SSH aracılığıyla bağlanabilir ve MongoDB kümesiyle etkileşime geçebilirsiniz. MongoDB ile etkileşim kurma hakkında daha fazla bilgi için bkz. [Mongo kabuğu](https://docs.mongodb.com/manual/mongo/).

  - Hibrit bulut uygulamaları hakkında daha fazla bilgi için bkz [. karma bulut çözümleri.](https://aka.ms/azsdevtutorials)

  - [GitHub](https://github.com/Azure-Samples/azure-intelligent-edge-patterns)'da kodu bu örnekle değiştirin.
