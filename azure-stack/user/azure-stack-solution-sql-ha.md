---
title: Bir SQL Server 2016'yı kullanılabilirlik grubu için Azure ve Azure Stack dağıtma | Microsoft Docs
description: Bir SQL Server 2016'yı kullanılabilirlik grubu için Azure'da ve Azure Stack'te dağıtma hakkında bilgi edinin
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
ms.openlocfilehash: 39b0078345c16b7931f41cd2394476f8258d92dd
ms.sourcegitcommit: 104ccafcb72a16ae7e91b154116f3f312321cff7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67308971"
---
# <a name="tutorial-deploy-a-sql-server-2016-availability-group-to-azure-and-azure-stack"></a>Öğretici: Azure ve Azure Stack için bir SQL Server 2016'yı kullanılabilirlik grubu dağıtma

Bu makalede bir zaman uyumsuz bir olağanüstü durum kurtarma (DR) site ile temel yüksek oranda kullanılabilir (HA) SQL Server 2016 Enterprise kümesi otomatik bir dağıtımını iki Azure Stack ortamlarında adım adım. SQL Server 2016 ve yüksek kullanılabilirlik hakkında daha fazla bilgi için bkz: [Always On kullanılabilirlik grupları: bir yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümü](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server?view=sql-server-2016).

Bu öğreticide, bir örnek ortama oluşturacaksınız:

> [!div class="checklist"]
> - İki Azure yığınları arasında dağıtımı düzenleyin
> - Azure API profilleriyle bağımlılık sorunlarını en aza indirmek için Docker'ı kullanma
> - Bir olağanüstü durum kurtarma siteniz ile temel bir yüksek oranda kullanılabilir SQL Server 2016 Enterprise kümesi dağıtın

> [!Tip]  
> ![karma pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack, Azure'nın bir uzantısıdır. Azure Stack çevikliğini ve yenilik, şirket içi ortamınıza bulut getirir ve hibrit uygulamaları her yerde oluşturup dağıtmayı olanak tanıyan tek hibrit Bulutu sağlar.  
> 
> Teknik incelemeyi [karma uygulamaları için tasarım konuları](https://aka.ms/hybrid-cloud-applications-pillars) yazılım kalitesinin yapı taşları tasarlama, dağıtma ve çalıştırma için (yerleştirme, ölçeklenebilirlik, kullanılabilirlik, dayanıklılık, yönetilebilirlik ve güvenlik) gözden geçirmeleri karma uygulamalar. Tasarım konuları, üretim ortamlarında sorunlarını en aza karma uygulama tasarımının en iyi duruma getirme yardımcı olur.

## <a name="architecture-for-sql-server-2016"></a>SQL Server 2016 için mimarisi

![SQL Server 2016 SQL HA Azure Stack](media/azure-stack-solution-sql-ha/image1.png)

## <a name="prerequisites-for-sql-server-2016"></a>SQL Server 2016 için Önkoşullar

  - İki bağlı Azure Stack tümleşik sistemleri (Azure Stack), bu dağıtımı, Azure Stack geliştirme setleri (ASDKs) çalışmıyor. Azure Stack hakkında daha fazla bilgi için bkz: [Azure Stack nedir?](https://azure.microsoft.com/overview/azure-stack/).
  - Her Azure Stack Kiracı abonelikte.    
      - **Her abonelik kimliği ve Azure Resource Manager uç nokta her Azure Stack için not edin.**
  - Her Azure Stack üzerinde Kiracı aboneliği için izinlere sahip bir Azure Active Directory (Azure AD) hizmet sorumlusu. Azure yığını, farklı Azure AD'de bir dağıttıysanız, iki hizmet sorumlusu oluşturma gerekebilir kiracılar. Azure Stack için hizmet sorumlusu oluşturma konusunda bilgi almak için bkz: [uygulamaları Azure Stack kaynaklarına erişmenizi sağlayacak bir hizmet sorumlusu oluşturma](https://docs.microsoft.com/azure-stack/user/azure-stack-create-service-principals).
      - **Her hizmet sorumlusunun uygulama kimliği, istemci gizli anahtarı ve Kiracı adı (xxxxx.onmicrosoft.com) not edin.**
  - SQL Server 2016 Enterprise her Azure yığını'nın Market'te genel olarak. Market dağıtım hakkında daha fazla bilgi için bkz: [indirme Market öğesi Azure'dan Azure Stack'e](https://docs.microsoft.com/azure-stack/operator/azure-stack-download-azure-marketplace-item).
    **Kuruluşunuzun uygun SQL lisanslara sahip olduğundan emin olun.**
  - [Windows için docker](https://docs.docker.com/docker-for-windows/) yerel makinenizde yüklü.

## <a name="get-the-docker-image"></a>Docker görüntüsünü Al

Her dağıtım için docker görüntülerini, Azure PowerShell farklı sürümleri arasında bağımlılık sorunlarını ortadan kaldırın.

1.  Windows kapsayıcıları için Docker Windows kullandığından emin olun.
2.  Dağıtım betikleri ile Docker kapsayıcısı almak için yükseltilmiş bir komut isteminde aşağıdaki komutu çalıştırın.

```powershell  
 docker pull intelligentedge/sqlserver2016-hadr:1.0.0
```

## <a name="deploy-the-availability-group"></a>Kullanılabilirlik grubu dağıtma

1.  Kapsayıcı görüntüsünü başarıyla çekilen sonra görüntüyü başlatın.

      ```powershell  
      docker run -it intelligentedge/sqlserver2016-hadr:1.0.0 powershell
      ```

2.  Kapsayıcı başladıktan sonra yükseltilmiş bir PowerShell terminalde kapsayıcı verilir. Dağıtım betiğine almak için dizinleri değiştirin.

      ```powershell  
      cd .\SQLHADRDemo\
      ```

3.  Dağıtımı'nı çalıştırın. Kimlik bilgileri ve kaynak sağlamanız gerektiğinde adları. HA HA kümenin dağıtılacağı Azure Stack ve DR kümenin dağıtılacağı Azure Stack için DR ifade eder.

      ```powershell
      > .\Deploy-AzureResourceGroup.ps1 `
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

5.  Kaynak dağıtımın bitmesini bekleyin.

6.  DR kaynak dağıtım tamamlandıktan sonra kapsayıcı çıkın.

      ```powershell
      exit
      ```

7.  Her Azure Stack'ın portalında kaynakları görüntüleyerek dağıtım inceleyin. HA ortamı ve SQL Server Management Studio (SSMS) aracılığıyla kullanılabilirlik Grup İnceleme SQL örneklerinde birine bağlanın.

![SQL Server 2016 SQL yüksek kullanılabilirlik](media/azure-stack-solution-sql-ha/image2.png)

## <a name="next-steps"></a>Sonraki adımlar

  - SQL Server Management Studio kullanın el ile küme üzerinde başarısız, bkz: [bir zorla el ile Yük Devretmesini bir Always On kullanılabilirlik grubu (SQL Server) gerçekleştirin.](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server?view=sql-server-2017)
  - Hibrit bulut uygulamaları hakkında daha fazla bilgi için bkz: [hibrit bulut çözümleri.](https://aka.ms/azsdevtutorials)
  - Bu örnek için kodu değiştirin veya kendi verilerinizi kullanmak [GitHub](https://github.com/Azure-Samples/azure-intelligent-edge-patterns).
