---
title: Bu makalede, Azure Stack tekliflerini ve planlarını güncelleştirme hakkında bilgi edinebilirsiniz | Microsoft Docs
description: Bu makalede, mevcut Azure Stack tekliflerinin ve planların nasıl görüntüleneceği ve değiştirileceği açıklanır.
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
ms.topic: conceptual
ms.custom: mvc
ms.date: 10/05/2019
ms.author: bryanla
ms.reviewer: efemmano
ms.lastreviewed: 06/11/2019
ms.openlocfilehash: 4b3fed8ee02739cf82b1446cc8c0a9b807763d49
ms.sourcegitcommit: d159652f50de7875eb4be34c14866a601a045547
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72283280"
---
# <a name="azure-stack-add-on-plans"></a>Azure Stack eklenti planları

Azure Stack operatörü olarak, ek hizmetler sunmak ya da *bilgisayar*, *depolama*veya *ağ* kotalarını temel plan ilk teklifinin ötesinde genişletmek istediğinizde bir [temel planı](azure-stack-create-plan.md) değiştirmek için eklenti planları oluşturursunuz. Eklenti planları temel planı değiştirir ve kullanıcıların aboneliklerinde etkinleştirmeyi seçebilmeleri için isteğe bağlı uzantılardır.

Tek bir plandaki her şeyin birleştirilmesinin en iyi durumda olduğu durumlar vardır. Diğer zamanlarda, temel bir plana sahip olmak ve daha sonra eklenti planları kullanarak ek hizmetleri sunmak isteyebilirsiniz. Örneğin, tüm PaaS hizmetlerinin eklenti planları olarak kabul edildiği bir temel planın parçası olarak IaaS hizmetleri sunmaya karar verebilirsiniz.

Eklenti planlarını kullanmanın başka bir nedeni de kaynak kullanımını izlemeye yardımcı olur. Bunu yapmak için görece küçük kotalar (gerekli hizmetlere bağlı olarak) içeren bir temel plana başlayabilirsiniz. Daha sonra, kullanıcılar kapasiteye ulaştığında, kendilerine atanan planına göre kaynak ayırmayı tükettiği hakkında uyarı alırlar. Buradan, kullanıcılar ek kaynakları sağlayan bir eklenti planı seçebilir.

> [!NOTE]
> Bir kotayı genişletmek için eklenti planı kullanmak istemiyorsanız, [kotanın orijinal yapılandırmasını düzenlemeyi](azure-stack-quota-types.md#edit-a-quota)de seçebilirsiniz.

Eklenti planları, temel planla [aynı şekilde oluşturulur](azure-stack-create-plan.md) . İki arasındaki fark, plan bir teklifine eklendiğinde belirlenir. Temel bir plan veya eklenti planı olarak belirtilir. Mevcut bir teklifine eklenti planı eklediğinizde, ek kaynakların abonelikte görünmesi bir saate kadar sürebilir.

::: moniker range=">=azs-1902"
## <a name="create-an-add-on-plan-1902-and-later"></a>Eklenti planı oluşturma (1902 ve üstü)

1. Azure Stack yönetici portalında Bulut Yöneticisi olarak oturum açın.
2. Daha önce sunulmayan yeni bir plan teklif hizmetleri oluşturmak için [Yeni bir temel plan oluşturmak](azure-stack-create-plan.md) için kullanılan adımların aynısını izleyin.
3. Yönetici portalında, **teklifler** ' e tıklayın ve ardından bir eklenti planıyla güncelleştirileceği teklifi seçin.

   ![Eklenti planı oluşturma](media/create-add-on-plan/add-on1.png)

4. Teklif özelliklerinin en altında **eklenti planları**' nı seçin. **Ekle**'yi tıklatın.

    ![Eklenti planı oluşturma](media/create-add-on-plan/add-on2.png)

5. Eklenecek planı seçin. Bu örnekte plana **20-storageaccounts**adı verilir. Planı seçtikten sonra, planı teklifine eklemek için **Seç** ' e tıklayın. Planın teklifine başarıyla eklendiğini belirten bir bildirim almanız gerekir.

    ![Eklenti planı oluşturma](media/create-add-on-plan/add-on3.png)

6. Yeni eklenti planının listelendiğini doğrulamak için teklifle birlikte dahil olan eklenti planlarının listesini gözden geçirin.

    [![Eklenti planı]oluşturma(media/create-add-on-plan/add-on4.png "eklenti planı") oluşturma](media/create-add-on-plan/add-on4lg.png#lightbox)
::: moniker-end

::: moniker range="<=azs-1901"
## <a name="create-an-add-on-plan-1901-and-earlier"></a>Eklenti planı oluşturma (1901 ve önceki sürümler)

1. Azure Stack yönetici portalında Bulut Yöneticisi olarak oturum açın.
2. Daha önce sunulmayan yeni bir plan teklif hizmetleri oluşturmak için [Yeni bir temel plan oluşturmak](azure-stack-create-plan.md) için kullanılan adımların aynısını izleyin. Bu örnekte, yeni plana Key Vault (**Microsoft. Keykasası**) hizmetleri dahil edilecek.
3. Yönetici portalında, **teklifler** ' e tıklayın ve ardından bir eklenti planıyla güncelleştirileceği teklifi seçin.

   ![Eklenti planı oluşturma](media/create-add-on-plan/1.PNG)

4. Teklif özelliklerinin en altına kaydırın ve **eklenti planları**' nı seçin. **Ekle**'yi tıklatın.

    ![Eklenti planı oluşturma](media/create-add-on-plan/2.PNG)

5. Eklenecek planı seçin. Bu örnekte plan, **Anahtar Kasası planı**olarak adlandırılır. Planı seçtikten sonra, planı teklifine eklemek için **Seç** ' e tıklayın. Planın teklifine başarıyla eklendiğini belirten bir bildirim almanız gerekir.

    ![Eklenti planı oluşturma](media/create-add-on-plan/3.PNG)

6. Yeni eklenti planının listelendiğini doğrulamak için teklifle birlikte dahil olan eklenti planlarının listesini gözden geçirin.

    ![Eklenti planı oluşturma](media/create-add-on-plan/4.PNG)
::: moniker-end

## <a name="next-steps"></a>Sonraki adımlar

* [Teklif oluşturma](azure-stack-create-offer.md)
