---
title: Azure Stack tekliflerin yetkisini verme | Microsoft Docs
description: Teklifleri oluşturma ve kullanıcılara kaydolma hakkında diğer kişileri nasıl koyacağınızı öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/12/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 05/07/2019
ms.openlocfilehash: 97b253e4df3762d4579923124b7768c14f40d3de
ms.sourcegitcommit: 3af71025e85fc53ce529de2f6a5c396b806121ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71159677"
---
# <a name="delegate-offers-in-azure-stack"></a>Azure Stack’te teklifleri yetkilendirme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack operatörü olarak, kullanıcıların kaydolmasını ve abonelik oluşturmayı ücretlendirmeye yönelik başka kişiler de koymak isteyebilirsiniz. Örneğin, bir hizmet sağlayıcısıysanız, satıcıların müşterilere kaydolmasını ve bunları sizin adınıza yönetmesini isteyebilirsiniz. Ya da bir kuruluştaki merkezi bir BT grubunun parçasıysa, diğer BT personeline Kullanıcı kaydolma yetkisi vermek isteyebilirsiniz.

Temsili, aşağıdaki şekilde gösterildiği gibi, sizin tarafınızdan daha fazla kullanıcının daha kolay bir şekilde ulaşmasını ve yönetilmesini kolaylaştırır:

![Temsilin düzeyleri](media/azure-stack-delegated-provider/image1.png)

Temsilci ile, temsilcili sağlayıcı bir teklifi yönetir ( *Temsilcili teklif*olarak adlandırılır) ve son müşteriler, sistem yöneticisinden katılımı olmadan bu teklif kapsamındaki abonelikleri elde eder.

## <a name="delegation-roles"></a>Rol atama

Aşağıdaki roller temsilcinin bir parçasıdır:

* *Azure Stack işleci* Azure Stack altyapısını yönetir ve bir teklif şablonu oluşturur. İşleci, başkalarına kiracının tekliflerini sunacak şekilde temsilci seçer.

* Temsil edilen Azure Stack işleçleri, *atanan sağlayıcılar*adlı aboneliklerde *sahip* veya *katkıda* bulunan haklarına sahip kullanıcılardır. Diğer Azure Active Directory (Azure AD) kiracıları gibi diğer kuruluşlara ait olabilir.

* *Kullanıcılar* , tekliflere kaydolduktan sonra iş yüklerini yönetmek, VM 'leri oluşturmak, verileri depolamak vb. için bunları kullanır.

## <a name="delegation-steps"></a>Temsili adımları

Temsilciyi ayarlamaya yönelik iki temel adım vardır:

1. **Yetkilendirilmiş bir sağlayıcı aboneliği oluşturun**: Bir kullanıcıyı yalnızca abonelik hizmeti içeren bir teklife abone olma. Bu teklife abone olan kullanıcılar daha sonra bu tekliflere kaydolduktan sonra temsilci teklifleri diğer kullanıcılara genişletebilirler.

2. **Temsil edilen sağlayıcıya bir teklif devretmek**: Bu teklif, temsilcili sağlayıcının abonelik oluşturmasına veya teklifi kullanıcılarına genişlemesine olanak sağlar. Temsilci sağlayıcı artık teklifi alabilir ve diğer kullanıcılara sunabilir.

Aşağıdaki şekilde, temsilciyi ayarlama adımları gösterilmektedir:

![Yetkilendirilmiş sağlayıcıyı oluşturun ve kullanıcıların kaydolmalarını etkinleştirin](media/azure-stack-delegated-provider/image2.png)

### <a name="delegated-provider-requirements"></a>Temsilci sağlayıcı gereksinimleri

Kullanıcı temsilcisi olarak görev yapmak için bir abonelik oluşturarak ana sağlayıcıyla bir ilişki kurar. Bu abonelik, temsilci olan sağlayıcıyı, ana sağlayıcı adına temsili teklifleri sunma hakkına sahip olacak şekilde tanımlar.

Bu ilişki kurulduktan sonra Azure Stack işleci, temsilci atanmış sağlayıcıya yönelik bir teklifi devredebilir. Temsil edilen sağlayıcı teklifi alabilir, yeniden adlandırabilir (ancak bunu değiştirmez) ve müşterilerine sunabilir.

## <a name="delegation-walkthrough"></a>Temsili izlenecek yol

Aşağıdaki bölümlerde, temsilcili bir sağlayıcıyı ayarlamaya, bir teklifin yetkisini aktarmaya ve kullanıcıların Temsilcili bir teklifle ilgili olarak kaydolabileceği doğrulanıyor.

### <a name="set-up-roles"></a>Rolleri ayarlama

Bu kılavuzu kullanmak için, Azure Stack operatör hesabınıza ek olarak iki Azure AD hesabına sahip olmanız gerekir. Bu iki hesabınız yoksa, bunları oluşturmanız gerekir. Hesaplar herhangi bir Azure AD kullanıcısına ait olabilir ve temsilci sağlayıcı ve Kullanıcı olarak adlandırılır.

| **Rol** | **Kuruluş hakları** |
| --- | --- |
| Sağlayıcı temsilcisi |Kullanıcı |
| Kullanıcı |Kullanıcı |

 > [!NOTE]
 > CSP-satıcı söz konusu olduğunda, bu sağlayıcının oluşturulması kiracı dizininde (Kullanıcı Azure AD) bu kullanıcıları gerektirir. Azure Stack işlecinin [önce](azure-stack-enable-multitenancy.md) bu kiracıyı Azure AD 'ye eklemek ve ardından [Bu adımları](azure-stack-csp-howto-register-tenants.md)izleyerek kullanımı ve faturalandırmayı yapılandırmak gerekir.

### <a name="identify-the-delegated-provider"></a>Temsil edilen sağlayıcıyı tanımla

1. Yönetici portalında Azure Stack işletmeni olarak oturum açın.

1. Bir kullanıcının Temsilcili bir sağlayıcı haline gelmesini sağlayan bir teklif oluşturmak için:

   a.  [Bir plan oluşturun](azure-stack-create-plan.md).
       Bu plan yalnızca abonelik hizmetini içermelidir. Bu makale örnek olarak **Planfortemsili** adlı bir plan kullanır.

   b.  Bu plana dayalı [bir teklif oluşturun](azure-stack-create-offer.md) . Bu makale örnek olarak **Offertodp** adlı bir teklif kullanır.

   c.  **Abonelikler**' i ve sonra **Yeni kiracı aboneliği** **' ni seçip, bu**teklif için Temsilcili bir abone olarak ekleyin.

   ![Temsilci olarak verilen sağlayıcıyı abone olarak ekleyin](media/azure-stack-delegated-provider/image3.png)

   > [!NOTE]
   > Tüm Azure Stack tekliflerle olduğu gibi, teklifi genel hale getirme ve kullanıcıların BT için kaydolmasına izin verme ve Azure Stack işlecinin kaydolmayı yönetmesine izin verme seçeneğiniz vardır. Temsilci sağlayıcılar genellikle küçük bir gruptur. Kim tarafından kabul edilen kişiyi denetlemek istiyorsanız, bu teklifin özel tutulması çoğu durumda daha anlamlı hale gelir.

### <a name="azure-stack-operator-creates-the-delegated-offer"></a>Azure Stack işleci, atanan teklifi oluşturuyor

Bir sonraki adım, temsilciliğini alacak olan planı ve teklifi oluşturmaktır ve kullanıcılarınızın kullanması gerekir. Temsilci sağlayıcı, içerdiği planları ve kotaları değiştiremediğinden, bu teklifi tam olarak kullanıcıların görmesini istediğiniz şekilde tanımlamak iyi bir fikirdir.

1. Azure Stack operatörü olarak, plana dayalı [bir plan](azure-stack-create-plan.md) ve [teklif](azure-stack-create-offer.md) oluşturun. Bu makale, örnek olarak **Delegatedoffer** adlı bir teklif kullanır.

   > [!NOTE]
   > Bu teklifin genel olması gerekmez, ancak genel kullanıma sunabilirsiniz. Ancak çoğu durumda, yalnızca temsilci sağlayıcıların teklifine erişimi olmasını isteyebilirsiniz. Aşağıdaki adımlarda açıklandığı gibi özel bir teklif temsilcisinden sonra, yetkilendirilmiş sağlayıcının erişimi vardır.

2. Teklifi devretmek. **Delegatedoffer**öğesine gidin. **Ayarlar**altında, **yetkilendirilmiş sağlayıcılar**' ı seçin ve ardından **Ekle**' yi seçin.

3. Açılan listeden temsilci sağlayıcı için abonelik ' ı seçin ve ardından **temsilci**' i seçin.

   ![Yetkilendirilmiş sağlayıcı ekleme](media/azure-stack-delegated-provider/image4.png)

### <a name="delegated-provider-customizes-the-offer"></a>Temsilci sağlayıcı teklifi özelleştirir

Yönetici temsilcisi olarak Kullanıcı Portalı ' nda oturum açın ve ardından atanmış teklifi şablon olarak kullanarak yeni bir teklif oluşturun.

1. **+ Kaynak oluştur**' u seçin, ardından **kiracı + planlar**ve **teklif**' i seçin.

    ![Yeni bir teklif oluştur](media/azure-stack-delegated-provider/image5.png)

2. Teklifine bir ad atayın. Bu örnek, **Resellerteklifini**kullanır. Üzerine temelde atanmış teklifi seçin ve ardından **Oluştur**' u seçin.

   ![Ad atama](media/azure-stack-delegated-provider/image6.png)

   >[!IMPORTANT]
   >Temsilcili sağlayıcıların yalnızca temsilci seçilmiş teklifleri seçebileceğini anlamak önemlidir. Bu teklifler üzerinde değişiklik yapamaz. Yalnızca bir Azure Stack işleci bu teklifleri değiştirebilir, örneğin planlarını ve kotalarını değiştirebilir. Yetkilendirilmiş bir sağlayıcı, temel planlardan ve eklenti planlarından bir teklif oluşturmaz.

3. Temsil edilen sağlayıcı, kendi portal URL 'SI aracılığıyla bu teklifleri herkese açık hale getirir. Teklifi genel hale getirmek için, **Araştır**' ı seçin ve ardından **sunar**. Teklifi seçin ve ardından **Durumu Değiştir**' i seçin.

4. Ortak Temsilcili teklifler artık yalnızca, temsilcili Portal üzerinden görülebilir. Bu URL 'YI bulmak ve değiştirmek için:

    a.  **Tüm hizmetler** **' e**ve ardından **genel** kategori altında, **abonelikler**' i seçin. Temsil edilen sağlayıcı aboneliğini seçin; Örneğin, **Dpsubscription**ve **Özellikler**.

    b.  Portal URL 'sini Not Defteri gibi ayrı bir konuma kopyalayın.

    ![Temsil edilen sağlayıcı aboneliğini seçin](media/azure-stack-delegated-provider/dpportaluri.png)  

   Temsilci atanmış bir teklif oluşturmayı, temsilci bir sağlayıcı olarak tamamladınız. Temsilci sağlayıcı olarak oturumu kapatın ve tarayıcı penceresini kapatın.

### <a name="sign-up-for-the-offer"></a>Teklif için kaydolun

1. Yeni bir tarayıcı penceresinde, önceki adımda kaydettiğiniz temsil edilen Portal URL 'sine gidin. Portalda Kullanıcı olarak oturum açın.

   >[!NOTE]
   >Temsil edilen portalı kullanmadığınız takdirde, temsilci teklifleri görünür değildir.

1. Panoda **abonelik al**' ı seçin. Yalnızca atanmış sağlayıcı tarafından oluşturulan Temsilcili tekliflerin kullanıcıya sunulduğunu görürsünüz.

   ![Teklifleri görüntüleme ve seçme](media/azure-stack-delegated-provider/image8.png)

Bir teklifin yetkisini oluşturma işlemi tamamlandı. Artık bir kullanıcı abonelik alarak bu teklif için kaydolabilir.

## <a name="move-subscriptions-between-delegated-providers"></a>Abonelikleri, temsilcili sağlayıcılar arasında taşıma

Gerekirse, abonelik, aynı dizin kiracısına ait olan yeni veya mevcut sağlayıcı abonelikleri arasında taşınabilir. Bu, [Move-AzsSubscription](/powershell/module/azs.subscriptions.admin)PowerShell cmdlet 'i kullanılarak yapılır.

Bu, şu durumlarda yararlı olur:

* Temsilci sağlayıcı rolünü alacak yeni bir takım üyesi oluşturdunuz ve bu takım üyesi Kullanıcı aboneliklerine atamak istiyorsanız varsayılan sağlayıcı aboneliğinde daha önce oluşturulmuş olan bir ekip üyesi misiniz?
* Aynı dizin kiracısında (Azure Active Directory) birden çok temsilci sağlayıcı aboneliğiniz var ve bunlar arasında Kullanıcı aboneliklerini taşımanız gerekir. Bu senaryo, takım üyesi takımlar arasında taşınırsa ve abonelikleri yeni takıma ayrılmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* [VM sağlama](../user/azure-stack-create-vm-template.md)
