---
title: Azure Stack üzerinde App Service çalışanları ve altyapıyı ekleme | Microsoft Docs
description: Azure Stack App Services ölçeklendirme için ayrıntılı kılavuz
services: azure-stack
documentationcenter: ''
author: bryanla
manager: femila
editor: ''
ms.assetid: 3cbe87bd-8ae2-47dc-a367-51e67ed4b3c0
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/06/2019
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 06/08/2018
ms.openlocfilehash: 43ee38c18e2831d1cb96958501cee6f77292edd0
ms.sourcegitcommit: e2f6205e6469b39c2395ee09424bb7632cb94c40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70271675"
---
# <a name="add-workers-and-infrastructure-in-app-service-on-azure-stack"></a>Azure Stack App Service çalışanları ve altyapıyı ekleme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*  

Bu belge Azure Stack App Service altyapı ve çalışan rollerinin nasıl ölçeklenmesi hakkında yönergeler sağlar. Her boyuttaki uygulamaları desteklemek üzere ek çalışan rolleri oluşturmak için gereken tüm adımları ele alacağız.

> [!NOTE]
> Azure Stack ortamınız 96 GB 'den fazla RAM içermiyorsa, ek kapasite ekleme zorluklarınız olabilir.

Azure Stack App Service, varsayılan olarak ücretsiz ve paylaşılan çalışan katmanlarını destekler. Diğer çalışan katmanlarını eklemek için daha fazla çalışan rolü eklemeniz gerekir.

Azure Stack yüklemesinde varsayılan App Service dağıtılmış olduğundan emin değilseniz, [Azure Stack genel bakış bölümünde App Service](azure-stack-app-service-overview.md)ek bilgileri inceleyebilirsiniz.

Azure Stack Azure App Service, sanal makine ölçek kümeleri kullanarak tüm rolleri dağıtır ve bu nedenle bu iş yükünün ölçeklendirme özelliğinden yararlanır. Bu nedenle, çalışan katmanlarının tüm ölçeklendirilmesi App Service Yöneticisi aracılığıyla yapılır.

## <a name="add-additional-workers-with-powershell"></a>PowerShell ile ek çalışanlar ekleme

1. [PowerShell 'de Azure Stack yönetici ortamını ayarlama](azure-stack-powershell-configure-admin.md)

2. Ölçek kümesini ölçeklendirmek için bu örneği kullanın:
   ```powershell
   
    ##### Scale out the AppService Role instances ######
   
    # Set context to AzureStack admin.
    Login-AzureRmAccount -EnvironmentName AzureStackAdmin
                                                 
    ## Name of the Resource group where AppService is deployed.
    $AppServiceResourceGroupName = "AppService.local"

    ## Name of the ScaleSet : e.g. FrontEndsScaleSet, ManagementServersScaleSet, PublishersScaleSet , LargeWorkerTierScaleSet,      MediumWorkerTierScaleSet, SmallWorkerTierScaleSet, SharedWorkerTierScaleSet
    $ScaleSetName = "SharedWorkerTierScaleSet"

    ## TotalCapacity is sum of the instances needed at the end of operation. 
    ## e.g. if your VMSS has 1 instance(s) currently and you need 1 more the TotalCapacity should be set to 2
    $TotalCapacity = 2  

    # Get current scale set
    $vmss = Get-AzureRmVmss -ResourceGroupName $AppServiceResourceGroupName -VMScaleSetName $ScaleSetName

    # Set and update the capacity
    $vmss.sku.capacity = $TotalCapacity
    Update-AzureRmVmss -ResourceGroupName $AppServiceResourceGroupName -Name $ScaleSetName -VirtualMachineScaleSet $vmss 
   ```    

   > [!NOTE]
   > Bu adım, rol türüne ve örnek sayısına bağlı olarak tamamlanması gereken birkaç saat sürebilir.
   >
   >

3. App Service yönetiminde yeni rol örneklerinin durumunu izleyin. Tek bir rol örneğinin durumunu denetlemek için listedeki rol türüne tıklayın.

## <a name="add-additional-workers-using-the-admin-portal"></a>Yönetici portalını kullanarak ek çalışanlar ekleyin

1. Hizmet Yöneticisi olarak Azure Stack yönetici portalında oturum açın.

2. **Uygulama hizmetleri**'ne gidin.

    ![Azure Stack yönetici portalında App Service](media/azure-stack-app-service-add-worker-roles/image01.png)

3. **Roller**' e tıklayın. Burada, dağıtılan tüm App Service rollerinin dökümünü görürsünüz.

4. Ölçeklendirmek istediğiniz türün satırına sağ tıklayın ve ardından **Ölçek kümesi**' ne tıklayın.

    ![Azure Stack yönetici portalındaki ölçek kümesi App Service rolleri](media/azure-stack-app-service-add-worker-roles/image02.png)

5. **Ölçeklendirme**' ye tıklayın, ölçeklendirmek istediğiniz örneklerin sayısını seçin ve ardından **Kaydet**' e tıklayın.

    ![Azure Stack yönetim portalındaki App Service roller olarak ölçeklendirmek için örnekler ayarlama](media/azure-stack-app-service-add-worker-roles/image03.png)

6. Azure Stack App Service artık ek VM 'Leri ekleyecek, yapılandırıp, gerekli tüm yazılımları yükleyecek ve bu işlem tamamlandığında bunları hazır olarak işaretleyecek. Bu işlem yaklaşık 80 dakika sürebilir.

7. **Roller** dikey penceresinde çalışanları görüntüleyerek yeni rollerin hazır olma durumunu izleyebilirsiniz.

## <a name="result"></a>Sonuç

Tam olarak dağıtıldıktan ve hazır olduktan sonra, çalışanlar iş yüklerini kendilerine dağıtmak üzere kullanıcılara açık hale gelir. Aşağıdaki ekran görüntüsünde, varsayılan olarak sunulan çoklu fiyatlandırma katmanlarının bir örneği gösterilmektedir. Belirli bir çalışan katmanı için kullanılabilir çalışan yoksa, karşılık gelen fiyatlandırma katmanını seçme seçeneği kullanılamaz.

![Azure Stack yönetici portalında yeni App Service planı için fiyatlandırma katmanları](media/azure-stack-app-service-add-worker-roles/image04.png)

>[!NOTE]
> Yönetim, ön uç veya yayımcı rollerinin ölçeğini genişletmek için, uygun rol türünü seçerken aynı adımları izleyin. Denetleyiciler ölçek kümeleri olarak dağıtılmaz ve bu nedenle iki üretim dağıtımı için yükleme zamanında dağıtılmalıdır.

### <a name="next-steps"></a>Sonraki adımlar

[Dağıtım kaynaklarını yapılandırma](azure-stack-app-service-configure-deployment-sources.md)
