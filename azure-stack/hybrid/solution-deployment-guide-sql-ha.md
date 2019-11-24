---
title: Azure ve Azure Stack hub 'a SQL Server 2016 kullanılabilirlik grubu dağıtma
description: SQL Server 2016 kullanılabilirlik grubunu Azure 'da ve Azure Stack hub 'a dağıtmayı öğrenin
author: BryanLa
ms.service: azure-stack
ms.topic: article
ms.date: 11/05/2019
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 11/05/2019
ms.openlocfilehash: e0fe07495017d6bd56beab78a37f5e0708197087
ms.sourcegitcommit: 5c92a669007ab4aaffe4484f1d8836a40340dde1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73639019"
---
# <a name="deploy-a-sql-server-2016-availability-group-to-azure-and-azure-stack-hub"></a>Azure ve Azure Stack hub 'a SQL Server 2016 kullanılabilirlik grubu dağıtma

*Uygulama hedefi: Azure Stack hub tümleşik sistemleri*

Bu makalede, iki Azure Stack hub ortamında zaman uyumsuz olağanüstü durum kurtarma (DR) sitesiyle temel yüksek kullanılabilirliğe sahip (HA) SQL Server 2016 kurumsal kümesinin otomatik dağıtımı adım adım gösterilir. SQL Server 2016 ve yüksek kullanılabilirlik hakkında daha fazla bilgi edinmek için bkz. [Always on kullanılabilirlik grupları: yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümü](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server?view=sql-server-2016).

Bu çözümde, aşağıdakileri yapmak için bir örnek ortam oluşturacaksınız:

> [!div class="checklist"]
> - İki Azure Stack hub arasında bir dağıtımı düzenleme
> - Azure API profilleriyle ilgili bağımlılık sorunlarını en aza indirmek için Docker kullanma
> - Olağanüstü durum kurtarma sitesiyle temel yüksek düzeyde kullanılabilir SQL Server 2016 kurumsal kümesi dağıtma

> [!Tip]  
> ![karma pillars.png](./media/solution-deployment-guide-cross-cloud-scaling/hybrid-pillars.png)  
> Microsoft Azure Stack hub, Azure uzantısıdır. Azure Stack hub, bulut bilgi işlemin çevikliğini ve yeniliklerini şirket içi ortamınıza sunarak, karma uygulamaları her yerde derleyip dağıtmanıza imkan tanıyan tek karma bulutu etkinleştirir.  
> 
> [Karma uygulamalar Için tasarım konuları](overview-app-design-considerations.md) , karma uygulamalar tasarlamak, dağıtmak ve çalıştırmak için yazılım kalitesine (yerleştirme, ölçeklenebilirlik, kullanılabilirlik, dayanıklılık, yönetilebilirlik ve güvenlik) göre önemli noktalar inceler. Tasarım konuları karma uygulama tasarımını iyileştirirken, üretim ortamlarındaki zorlukları en aza indirmeyle ilgili olarak size yardımcı olur.

## <a name="architecture-for-sql-server-2016"></a>2016 SQL Server için mimari

![SQL Server 2016 SQL HA Azure Stack hub 'ı](media/solution-deployment-guide-sql-ha/image1.png)

## <a name="prerequisites-for-sql-server-2016"></a>SQL Server 2016 önkoşulları

  - İki bağlı Azure Stack hub tümleşik sistemi (Azure Stack hub), bu dağıtım Azure Stack hub geliştirme setleri (aks) üzerinde çalışmaz. Azure Stack hub 'ı hakkında daha fazla bilgi edinmek için bkz. [Azure Stack hub nedir?](https://azure.microsoft.com/overview/azure-stack/).
  - Her Azure Stack hub 'ında kiracı aboneliği.    
      - **Her bir Azure Stack Hub için her abonelik KIMLIĞINI ve Azure Resource Manager uç noktasını bir yere unutmayın.**
  - Her bir Azure Stack hub 'ındaki kiracı aboneliğine yönelik izinlere sahip bir Azure Active Directory (Azure AD) hizmet sorumlusu. Azure Stack hub 'Ları farklı Azure AD Kiracılarına karşı dağıtılırsa iki hizmet sorumlusu oluşturmanız gerekebilir. Azure Stack Hub için bir hizmet sorumlusu oluşturmayı öğrenmek için bkz. [uygulamalara Azure Stack hub kaynaklarına erişim sağlamak için hizmet sorumluları oluşturma](https://docs.microsoft.com/azure-stack/user/azure-stack-create-service-principals).
      - **Her bir hizmet sorumlusunun uygulama KIMLIĞI, gizli anahtar ve kiracı adını (xxxxx.onmicrosoft.com) bir yere unutmayın.**
  - SQL Server 2016 Enterprise her bir Azure Stack hub 'ının Market 'e göre dağıtılmış. Market dağıtımı hakkında daha fazla bilgi edinmek için bkz. [Azure 'dan Market öğelerini Azure Stack hub 'A indirme](https://docs.microsoft.com/azure-stack/operator/azure-stack-download-azure-marketplace-item).
    **Kuruluşunuzun uygun SQL lisanslarına sahip olduğundan emin olun.**
  - Yerel makinenizde yüklü [Docker for Windows](https://docs.docker.com/docker-for-windows/) .

## <a name="get-the-docker-image"></a>Docker görüntüsünü al

Her dağıtım için Docker görüntüleri farklı Azure PowerShell sürümleri arasındaki bağımlılık sorunlarını ortadan kaldırır.

1.  Docker for Windows Windows kapsayıcıları ' nı kullandığınızdan emin olun.
2.  Dağıtım betikleri ile Docker kapsayıcısını almak için, yükseltilmiş bir komut isteminde aşağıdaki betiği çalıştırın.

```powershell  
 docker pull intelligentedge/sqlserver2016-hadr:1.0.0
```

## <a name="deploy-the-availability-group"></a>Kullanılabilirlik grubunu dağıtma

1.  Kapsayıcı görüntüsü başarıyla çekildikten sonra görüntüyü başlatın.

      ```powershell  
      docker run -it intelligentedge/sqlserver2016-hadr:1.0.0 powershell
      ```

2.  Kapsayıcı başlatıldıktan sonra, kapsayıcıda yükseltilmiş bir PowerShell terminali vermiş olursunuz. Dağıtım betiğine ulaşmak için dizinleri değiştirin.

      ```powershell  
      cd .\SQLHADRDemo\
      ```

3.  Dağıtımı çalıştırın. Gereken yerlerde kimlik bilgileri ve kaynak adları sağlayın. HA, HA kümesinin dağıtılacağı Azure Stack hub 'ına ve Dr kümesinin dağıtılacağı Azure Stack merkezine DR 'ye başvurur.

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

4.  NuGet sağlayıcısı 'nın yüklenmesine izin vermek için `Y` yazın ve bu, yüklenecek "2018-03-01-hibrit" modüllerinin API profilini başlatabilir.

5.  Kaynak dağıtımının tamamlanmasını bekleyin.

6.  DR kaynak dağıtımı tamamlandıktan sonra kapsayıcıdan çıkın.

      ```powershell
      exit
      ```

7.  Her bir Azure Stack merkezi portalındaki kaynakları görüntüleyerek dağıtımı inceleyin. HA ortamındaki SQL örneklerinden birine bağlanın ve SQL Server Management Studio (SSMS) aracılığıyla kullanılabilirlik grubunu inceleyerek.

![SQL Server 2016 SQL HA](media/solution-deployment-guide-sql-ha/image2.png)

## <a name="next-steps"></a>Sonraki adımlar

  - Küme üzerinde el ile yük devretmek için SQL Server Management Studio kullanın, bkz. [Always on kullanılabilirlik grubunun Zorlanmış El Ile yük devretmesini gerçekleştirme (SQL Server)](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server?view=sql-server-2017)
  - Hibrit bulut uygulamaları hakkında daha fazla bilgi için bkz [. karma bulut çözümleri.](https://aka.ms/azsdevtutorials)
  - Kendi verilerinizi kullanın veya [GitHub](https://github.com/Azure-Samples/azure-intelligent-edge-patterns)'da bu örnekteki kodu değiştirin.
