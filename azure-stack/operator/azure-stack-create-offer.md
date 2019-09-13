---
title: Azure Stack bir teklif oluşturun | Microsoft Docs
description: Bulut Yöneticisi olarak, Azure Stack kullanıcılarınız için teklif oluşturmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/11/2019
ms.author: sethm
ms.reviewer: efemmano
ms.lastreviewed: 06/11/2019
ms.openlocfilehash: 1492c779144eac235e5c32e767e966b3a6cfb9fd
ms.sourcegitcommit: 8ddd70ba5ce05c591d3fa62597981859af107c06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70936152"
---
# <a name="create-an-offer-in-azure-stack"></a>Azure Stack'te teklif oluşturma

[Teklifler](azure-stack-overview.md), sağlayıcılar tarafından kullanıcılara satın almaları veya abone olmaları için sunulan bir veya daha fazla plandan oluşan gruplardır. Bu makalede, [sizin oluşturduğunuz planı](azure-stack-create-plan.md) içeren bir teklifin nasıl oluşturulabileceği açıklanır. Bu teklif abonelere sanal makineleri (VM) ayarlayabilme olanağı getirir.

::: moniker range=">=azs-1902"
## <a name="create-an-offer-1902-and-later"></a>Teklif oluşturma (1902 ve üzeri)

1. [Azure Stack Yönetici portalı](https://adminportal.local.azurestack.external) ' nda oturum açın ve **+ kaynak oluştur**' u seçin, sonra da **+ planlar**ve **teklif**edin.

   ![Teklif oluşturma](media/azure-stack-create-offer/offers.png)

2. Teklif adını tanımlamanızı ve var olan veya yeni temel planlar ve eklenti planları oluşturmanızı sağlayan sekmeli bir kullanıcı arabirimi görünür. En önemlisi, oluşturma kararı vermeden önce oluşturduğunuz teklifin ayrıntılarını gözden geçirebilirsiniz.

   **Temel bilgiler** sekmesinde, bir **görünen ad** ve **kaynak adı**girip **kaynak grubu**altında **Yeni oluştur** ' u seçin veya **mevcut olanı kullanın**. Görünen ad, teklifin kolay adıdır. Bu kolay ad, kullanıcıların Kullanıcı portalındaki bir teklife abone olunduklarında göreceği teklifte ilgili tek bilgi. Kullanıcıların teklifle birlikte ne olduğunu anlamalarına yardımcı olan sezgisel bir ad kullanın. Kaynak adını yalnızca yönetici görebilir. Bu ad, yöneticilerin teklifle Azure Resource Manager kaynağı olarak çalışmak için kullandıkları addır. Bu sekmede, bu teklifi genel hale getirme veya varsayılan değer olan özel tutmayı de seçebilirsiniz. [Teklifin ortak veya özel durumunu](#change-the-state-of-an-offer) da daha sonra değiştirebilirsiniz.

   ![Yeni teklif](media/azure-stack-create-offer/new-offer.png)
  
3. **Temel planlar** sekmesini seçin veya **ileri ' ye tıklayın: Temel planlar >** düğmesi. Teklifine dahil etmek istediğiniz planı (öğeleri) seçin.

   ![Plan seçin](media/azure-stack-create-offer/select-plan.png)

4. Bu noktada, temel planı değiştirmek için bir eklenti planı oluşturabilirsiniz, ancak bu isteğe bağlıdır. [Eklenti planlarını Azure Stack](create-add-on-plan.md)sonraki makalede bir eklenti planı oluşturacağız.

5. **Gözden geçir + oluştur** sekmesini seçin. Tüm değerlerin doğru olduğundan emin olmak için teklif özetini gözden geçirin. Arabirim, seçili planlardaki kotaları tek seferde genişletmenizi, bir plandaki her kotanın ayrıntılarını görüntülemenizi ve gerekli düzenlemeleri yapmak için geri gitmenizi sağlar.

6. Teklifi oluşturmak için **Oluştur** ' u seçin.

   ![Gözden geçirin ve oluşturun](media/azure-stack-create-offer/review-offer.png)

### <a name="change-the-state-of-an-offer"></a>Teklifin durumunu değiştirme

Teklifi oluşturduktan sonra durumunu değiştirebilirsiniz. Kullanıcıların, abone olduklarında tam görünümü alması için **genel** kullanıma sunulmaları gerekir. Teklifler şu olabilir:

- **Ortak**: Kullanıcılara görünür.
- **Özel**: Yalnızca bulut yöneticileri tarafından görülebilir. Bu ayar plan veya teklif taslağı oluştururken veya bulut Yöneticisi [her kullanıcı için her abonelik oluşturmak](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator)isterse faydalıdır.
- **Kullanımdan**kaldırıldı: Yeni abonelere kapatıldı. Bulut Yöneticisi gelecekteki abonelikleri engellemek için teklifleri açığa alabilir, ancak geçerli aboneleri etkilenmeden bırakabilir.

  > [!TIP]  
  > Teklifte yapılan değişiklikler kullanıcıya hemen görünmez. Değişiklikleri görmek için kullanıcıların yeni teklifi görmek üzere oturumu kapatıp Kullanıcı portalında yeniden oturum açması gerekebilir.

Teklifin durumunu değiştirmek için iki yol vardır:

1. **Tüm kaynaklar**' da, teklifin adını seçin. Teklifin **genel bakış** ekranında **Durumu Değiştir**' i seçin. Kullanmak istediğiniz durumu (örneğin, **genel**) seçin.

   ![Durumu seçin](media/azure-stack-create-offer/change-state.png)

2. **Teklif ayarları**' nı seçin. Kullanmak istediğiniz durumu (örneğin, **genel**) seçin ve ardından **Kaydet**' i seçin.

   ![Erişilebilirlik durumunu seçin](media/azure-stack-create-offer/offer-settings.png)
::: moniker-end

::: moniker range="<=azs-1901"
## <a name="create-an-offer-1901-and-earlier"></a>Teklif oluşturma (1901 ve önceki sürümler)

1. [Azure Stack Yönetici portalı](https://adminportal.local.azurestack.external) ' nda oturum açın ve **+ kaynak oluştur**' u seçin, ardından **kiracı + planlar**ve ardından **teklif**edin.

   ![Teklif oluşturma](media/azure-stack-create-offer/image01.png)
  
2. **Yeni teklif**altına bir **görünen ad** ve **kaynak adı**girip **kaynak grubu**altında **Yeni oluştur** veya **mevcut olanı kullan**' ı seçin. Görünen ad, teklifin kolay adıdır. Bu kolay ad, kullanıcıların bir teklife abone olduklarında göreceği teklifle ilgili tek bir addır. Kullanıcıların teklifle birlikte ne olduğunu anlamalarına yardımcı olan sezgisel bir ad kullanın. Kaynak adını yalnızca yönetici görebilir. Bu ad, yöneticilerin teklifle Azure Resource Manager kaynağı olarak çalışmak için kullandıkları addır.

   ![Yeni teklif](media/azure-stack-create-offer/image01a.png)
  
3. **Planı**açmak için **temel planlar** ' ı seçin. Teklifine dahil etmek istediğiniz planları seçin ve ardından **Seç**' i seçin. Teklifi oluşturmak için **Oluştur**' u seçin.

   ![Plan seçin](media/azure-stack-create-offer/image02.png)
  
4. Teklifi oluşturduktan sonra durumunu değiştirebilirsiniz. Kullanıcıların, abone olduklarında tam görünümü alması için **genel** kullanıma sunulmaları gerekir. Teklifler şu olabilir:

   - **Ortak**: Kullanıcılara görünür.
   - **Özel**: Yalnızca bulut yöneticileri tarafından görülebilir. Bu ayar plan veya teklif taslağı oluştururken veya bulut Yöneticisi [her kullanıcı için her abonelik oluşturmak](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator)isterse faydalıdır.
   - **Kullanımdan**kaldırıldı: Yeni abonelere kapatıldı. Bulut Yöneticisi gelecekteki abonelikleri engellemek için teklifleri açığa alabilir, ancak geçerli aboneleri etkilenmeden bırakabilir.

   > [!TIP]  
   > Teklifte yapılan değişiklikler kullanıcıya hemen görünmez. Değişiklikleri görmek için kullanıcıların yeni teklifi görmek üzere oturumu kapatıp Kullanıcı portalında yeniden oturum açması gerekebilir.

   Teklifin genel bakış ekranında **Erişilebilirlik durumu**' nu seçin. Kullanmak istediğiniz durumu seçin (örneğin, **genel**) ve ardından **Kaydet**' i seçin.

     ![Durumu seçin](media/azure-stack-create-offer/change-stage-1807.png)

     Alternatif olarak, **Durumu Değiştir** ' i seçin ve ardından bir durum seçin.

    ![Erişilebilirlik durumunu seçin](media/azure-stack-create-offer/change-stage-select-1807.png)

> [!NOTE]
> PowerShell 'i Ayrıca varsayılan teklifler, planlar ve Kotalar oluşturmak için de kullanabilirsiniz. Daha fazla bilgi için bkz. [PowerShell modülü 1.4.0 Azure Stack](/powershell/azure/azure-stack/overview?view=azurestackps-1.4.0).
::: moniker-end

## <a name="next-steps"></a>Sonraki adımlar

- [Abonelik oluşturma](azure-stack-subscribe-plan-provision-vm.md)
- [Sanal makine sağlama](../user/azure-stack-create-vm-template.md)
