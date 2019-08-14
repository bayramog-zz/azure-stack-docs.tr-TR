---
title: Azure Stack bir teklife abone olma | Microsoft Docs
description: Azure Stack teklifler için abonelikler oluşturma
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 7f3f8683-ef09-4838-92ed-41f2fddbbbed
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/13/2019
ms.author: sethm
ms.lastreviewed: 05/10/2019
ms.openlocfilehash: 51b61eae1f2c00a959eded275c13c5c4b8d06e4c
ms.sourcegitcommit: aefcf9c61bd8089a0aaa569af7643e5e15f4947c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68991799"
---
# <a name="create-subscriptions-to-offers-in-azure-stack"></a>Azure Stack tekliflere abonelik oluşturma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Kullanıcıların [oluşturduğunuz teklifi](azure-stack-create-offer.md) kullanabilmeleri için bir aboneliğe ihtiyaçları vardır. Kullanıcılar iki yolla teklife abone olabilir:

- Bulut operatörü olarak yönetim portalının içinden kullanıcı için abonelik oluşturabilirsiniz Oluşturduğunuz abonelikler hem genel hem de özel teklifler için olabilir.
- Kiracı kullanıcısı olarak, kullanıcı portalını kullanırken bir genel teklife abone olabilirsiniz.  

## <a name="create-a-subscription-as-a-cloud-operator"></a>Bulut işletmeni olarak abonelik oluşturma

Bulut işleçleri, bir kullanıcı için bir teklif aboneliği oluşturmak üzere Yönetici portalını kullanabilir. Kendi dizin kiracınızın üyeleri için abonelikler oluşturabilirsiniz. [Birden çok kiracının](azure-stack-enable-multitenancy.md) etkin olduğu durumlarda, ek Dizin kiracılarında kullanıcılar için de abonelikler oluşturabilirsiniz.

Kiracılarınızın kendi aboneliklerini oluşturmasını istemiyorsanız, tekliflerinizi özel yapın ve ardından kiracılar için abonelikler oluşturun. Bu yaklaşım, Azure Stack dış faturalandırma veya hizmet kataloğu sistemleriyle tümleştirilirken yaygındır.

Bir kullanıcı için abonelik oluşturduktan sonra, Kullanıcı portalında oturum açabilir ve bu kullanıcıların teklifine abone olduklarını görebilirler.  

### <a name="to-create-a-subscription-for-a-user"></a>Bir kullanıcı için abonelik oluşturmak için

1. Yönetim portalında, **Kullanıcı abonelikleri** ' ne gidin.
2. **Add (Ekle)** seçeneğini belirleyin. **Yeni Kullanıcı aboneliği**altında aşağıdaki bilgileri girin:  

   - **Görünen ad** - *Kullanıcı abonelik adı*olarak görünen aboneliği belirlemek için kolay bir ad.
   - **Kullanıcı** -bu abonelik için kullanılabilir bir dizin kiracısından Kullanıcı belirtin. Kullanıcı adı, *sahip*olarak görünür.  Kullanıcı adının biçimi kimlik çözümünüze bağlıdır. Örneğin:

     - **Azure AD:** `<user1>@<contoso.onmicrosoft.com>`

     - **AD FS:** `<user1>@<azurestack.local>`

   - **Dizin kiracısı** -Kullanıcı hesabının ait olduğu dizin kiracısını seçin. Çoklu kiracıyı etkinleştirmediyseniz, yalnızca yerel dizin kiracınız kullanılabilir.

3. **Teklif**' i seçin. **Teklifler**altında bu abonelik Için bir **teklif** seçin. Bir kullanıcı için abonelik oluşturmakta olduğunuzdan, Erişilebilirlik durumu olarak **özel** ' i seçin.

4. Aboneliği oluşturmak için **Oluştur** ' u seçin. Yeni Abonelik **Kullanıcı aboneliği**altında görüntülenir. Kullanıcı Kullanıcı portalında oturum açtığında, abonelik ayrıntılarını görebilirler.

### <a name="to-make-an-add-on-plan-available"></a>Eklenti planını kullanılabilir hale getirmek için

Bulut operatörü, daha önce oluşturulmuş bir aboneliğe istediğiniz zaman bir plan ekleyebilir:

1. Yönetici portalı ' nda **tüm hizmetler** ' i seçin ve ardından **Yönetim kaynakları** kategorisinin altında **Kullanıcı abonelikleri**' ni seçin. Değiştirmek istediğiniz aboneliği seçin.

2. Eklentiler ' i seçin ve **+ Ekle**' yi seçin.  

3. **Plan Ekle**' nin altında, eklenti olarak istediğiniz planı seçin.

## <a name="create-a-subscription-as-a-user"></a>Kullanıcı olarak abonelik oluşturma

Kullanıcı olarak, Dizin kiracınız (kuruluş) için genel teklifleri ve eklenti planlarını bulmak ve bunlara abone olmak için Kullanıcı portalında oturum açabilirsiniz.

>[!NOTE]
>Azure Stack ortamınız [Çoklu kiracıyı](azure-stack-enable-multitenancy.md)destekliyorsa, uzak bir dizin kiracısından tekliflere de abone olabilirsiniz.

### <a name="to-subscribe-to-an-offer"></a>Teklife abone olmak için

1. [Azure Stack Kullanıcı portalında](https://portal.local.azurestack.external) [oturum açın](../asdk/asdk-connect.md) ve **abonelik al**' ı seçin.

   ![Abonelik edinin](media/azure-stack-subscribe-plan-provision-vm/image01.png)
  
2. **Abonelik al**' ın altında, **görünen ad**' de aboneliğin kolay adını girin. **Teklif** ' i seçin ve **teklif Seç**' in altında bir teklif seçin. Aboneliği oluşturmak için **Oluştur** ' u seçin.

   ![Teklif oluşturma](media/azure-stack-subscribe-plan-provision-vm/image02.png)
  
3. Teklife abone olduktan sonra, hangi hizmetlerin yeni aboneliğin parçası olduğunu görmek için portalı yenileyin.

4. Oluşturduğunuz aboneliği görmek için **tüm hizmetler** ' i seçin ve ardından **genel** kategori altında **abonelikler**' i seçin. Abonelik ayrıntılarını görmek için aboneliği seçin.  

### <a name="to-subscribe-to-an-add-on-plan"></a>Bir eklenti planına abone olmak için

Bir teklifin eklenti planı varsa, istediğiniz zaman bu planı aboneliğinize ekleyebilirsiniz.  

1. Kullanıcı portalında, **tüm hizmetler**' i seçin. Sonra, **genel** kategori altında, **abonelikler**' i seçin ve ardından değiştirmek istediğiniz aboneliği seçin. Kullanılabilir herhangi bir eklenti planı varsa **+ plan Ekle** etkindir ve **eklenti planları**için bir kutucuk vardır.

   Eğer ve **ekleme planı** etkin değilse, bu abonelikle ilişkili teklif için eklenti planı yoktur.

1. **+ Plan Ekle** veya **eklenti planları** kutucuğunu seçin. **Eklenti planları**altında, eklemek istediğiniz planı seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Sanal makine sağlama](../user/azure-stack-create-vm-template.md)
