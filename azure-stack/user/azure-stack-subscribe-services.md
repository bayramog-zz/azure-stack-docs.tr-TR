---
title: Azure Stack bir teklifiyle bir abonelik oluşturun | Microsoft Docs
description: Azure Stack bir teklifle yeni bir abonelik oluşturmayı ve sonra teklifi bir test VM 'si ile test yapmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: bryanla
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 06/04/2019
ms.author: bryanla
ms.reviewer: efemmano
ms.lastreviewed: 11/13/2018
ms.openlocfilehash: e6e09dffa6682a615c01e6fee841a5d736054546
ms.sourcegitcommit: dfaf0126bc9975ca1643d55f06c71df9e32ea976
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72164990"
---
# <a name="tutorial-create-and-test-a-subscription-in-azure-stack"></a>Öğretici: Azure Stack abonelik oluşturma ve test etme

Bu öğreticide, bir teklif içeren bir abonelik oluşturma ve ardından nasıl test yapılacağı gösterilmektedir. Test için, Azure Stack Kullanıcı portalında bir bulut Yöneticisi olarak oturum açın, teklife abone olur ve sonra bir sanal makine (VM) oluşturun.

> [!TIP]
> Daha gelişmiş bir değerlendirme deneyimi için, [belirli bir kullanıcı için bir abonelik oluşturabilir](../operator/azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator) ve ardından Kullanıcı portalında bu kullanıcı olarak oturum açabilirsiniz.

Bu öğreticide, bir Azure Stack teklifine nasıl abone olunacağı gösterilmektedir.

Öğrenecekleriniz:

> [!div class="checklist"]
> * Bir teklife abone olma 
> * Teklifi test etme

## <a name="subscribe-to-an-offer"></a>Bir teklife abone olma

Bir teklife Kullanıcı olarak abone olmak için, Azure Stack işleci tarafından sunulan kullanılabilir hizmetleri denetlemek için Azure Stack Kullanıcı portalında oturum açın.

1. Kullanıcı portalında oturum açın ve **abonelik al**' ı seçin.

   ![Abonelik al](media/azure-stack-subscribe-services/get-subscription.png)

2. **Görünen Ad** alanına aboneliğiniz için bir ad yazın. Teklif **Seç** bölümünde mevcut tekliflerden birini seçmek için **teklif** ' i seçin. Ardından **Oluştur**’u seçin.

   ![Teklif oluşturma](media/azure-stack-subscribe-services/create-subscription.png)

   > [!TIP]
   > Aboneliğinizi kullanmaya başlamak için Kullanıcı portalını yenileyin.

3. Oluşturduğunuz aboneliği görüntülemek için **tüm hizmetler**' i seçin. Ardından, **genel** kategori altında **abonelikler**' i seçin ve ardından yeni aboneliğinizi seçin. Teklife abone olduktan sonra yeni aboneliğin bir parçası olarak yeni hizmetlerin dahil edilip edilmediğini görmek için portalı yenileyin. Bu örnekte, **sanal makineler** eklenmiştir.

   ![Aboneliği görüntüle](media/azure-stack-subscribe-services/view-subscription.png)

## <a name="test-the-offer"></a>Teklifi test etme

Kullanıcı portalında oturum açana sırada, yeni abonelik yeteneklerini kullanarak bir VM sağlayarak teklifi test edin.

> [!NOTE]
> Bu test, öncelikle Azure Stack Market 'e bir Windows Server 2016 Datacenter VM 'nin eklenmesini gerektirir.

1. Kullanıcı portalında oturum açın.

2. Kullanıcı portalında, **sanal makineler**' i seçin, sonra **Windows Server 2016 Datacenter** **' u**ve ardından **Oluştur**' u seçin.

3. **Temel bilgiler** bölümünde bir **ad**, **Kullanıcı adı**ve **parola**yazıp bir **abonelik**seçin, bir **kaynak grubu** oluşturun (veya mevcut bir tane seçin) ve ardından **Tamam**' ı seçin.

4. **Boyut seçin** bölümünde **a1 standart**' ı seçin ve ardından **Seç**' i seçin.  

5. **Ayarlar** dikey penceresinde varsayılan değerleri kabul edin ve **Tamam**' ı seçin.

6. **Özet** bölümünde, VM 'yi oluşturmak için **Tamam** ' ı seçin.  

7. Yeni VM 'nizi görmek için **sanal makineler**' i seçin ve ardından yeni VM 'yi arayın ve adını seçin.

    ![Tüm kaynaklar](media/azure-stack-subscribe-services/view-vm.png)

> [!NOTE]
> VM dağıtımının tamamlanabilmesi birkaç dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir topluluk şablonundan VM oluşturma](azure-stack-create-vm-template.md)
