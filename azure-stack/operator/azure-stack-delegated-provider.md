---
title: Azure Stack tekliflerin yetkisini verme | Microsoft Docs
description: Teklifler oluşturma ve Kullanıcı imzalama gibi görevlerin nasıl temsilciliğini öğrenin.
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
ms.openlocfilehash: eee3a928f8f3c6f376e9019af6da71a77ab09450
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829230"
---
# <a name="delegate-offers-in-azure-stack"></a>Azure Stack’te teklifleri yetkilendirme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack operatörü olarak, kullanıcıların kaydolmasını ve abonelik oluşturmayı ücretlendirmeye yönelik başka kişiler de koymak isteyebilirsiniz. Örneğin, bir hizmet sağlayıcısıysanız, satıcıların müşterilere kaydolmasını ve bunları sizin adınıza yönetmesini isteyebilirsiniz. Ya da bir kuruluştaki merkezi bir BT grubunun parçasıysa, diğer BT personeline Kullanıcı kaydolma yetkisi vermek isteyebilirsiniz.

Temsili, aşağıdaki şekilde gösterildiği gibi, sizin tarafınızdan daha fazla kullanıcının daha kolay bir şekilde ulaşmasını ve yönetilmesini kolaylaştırır:

![Azure Stack 'de temsilin düzeyleri](media/azure-stack-delegated-provider/image1.png)

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

![Temsilci sağlayıcı oluşturma ve kullanıcıların Azure Stack oturum açmasını sağlama adımları](media/azure-stack-delegated-provider/image2.png)

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
 > CSP-satıcı söz konusu olduğunda, bu yetkilerin oluşturulması için bu kullanıcıların kiracı dizininde olması gerekir (Kullanıcı Azure AD). Azure Stack işlecinin [önce](azure-stack-enable-multitenancy.md) bu kiracıyı Azure AD 'ye eklemek ve ardından [Bu adımları](azure-stack-csp-howto-register-tenants.md)izleyerek kullanımı ve faturalandırmayı ayarlaması gerekir.

### <a name="identify-the-delegated-provider"></a>Temsil edilen sağlayıcıyı tanımla

1. Yönetici portalında Azure Stack işletmeni olarak oturum açın.

1. Bir kullanıcının Temsilcili bir sağlayıcı haline gelmesini sağlayan bir teklif oluşturmak için:

   a.  [Bir plan oluşturun](azure-stack-create-plan.md).
       Bu plan yalnızca abonelik hizmetini içermelidir. Bu makale örnek olarak **Planfortemsili** adlı bir plan kullanır.

   b.  Bu plana dayalı [bir teklif oluşturun](azure-stack-create-offer.md) . Bu makale örnek olarak **Offertodp** adlı bir teklif kullanır.

   c.  **Abonelikler**' i ve sonra **Yeni kiracı aboneliği** **' ni seçip, bu**teklif için Temsilcili bir abone olarak ekleyin.

   ![Temsilci olarak atanan sağlayıcıyı Azure Stack yönetici portalında abone olarak ekleyin](media/azure-stack-delegated-provider/image3.png)

   > [!NOTE]
   > Tüm Azure Stack tekliflerle olduğu gibi, teklifi genel hale getirme ve kullanıcıların BT için kaydolmasına izin verme ve Azure Stack işlecinin kaydolmayı yönetmesine izin verme seçeneğiniz vardır. Temsilci sağlayıcılar genellikle küçük bir gruptur. Kim tarafından kabul edilen kişiyi denetlemek istiyorsanız, bu teklifin özel tutulması çoğu durumda daha anlamlı hale gelir.

### <a name="azure-stack-operator-creates-the-delegated-offer"></a>Azure Stack işleci, atanan teklifi oluşturuyor

Bir sonraki adım, temsilciliğini alacak olan planı ve teklifi oluşturmaktır ve kullanıcılarınızın kullanması gerekir. Temsilci sağlayıcı, içerdiği planları ve kotaları değiştiremediğinden, bu teklifi kullanıcıların görmesini istediğiniz şekilde tanımlamak iyi bir fikirdir.

1. Azure Stack operatörü olarak, plana dayalı [bir plan](azure-stack-create-plan.md) ve [teklif](azure-stack-create-offer.md) oluşturun. Bu makale, örnek olarak **Delegatedoffer** adlı bir teklif kullanır.

   > [!NOTE]
   > Bu teklifin ortak olması gerekmez, ancak genel kullanıma sunabilirsiniz. Ancak çoğu durumda, yalnızca atanmış sağlayıcıların teklifine erişim sahibi olmasını istersiniz. Aşağıdaki adımlarda açıklandığı gibi özel bir teklif temsilcisinden sonra, yetkilendirilmiş sağlayıcının erişimi vardır.

2. Teklifi devretmek. **Delegatedoffer**öğesine gidin. **Ayarlar**altında, **yetkilendirilmiş sağlayıcılar**' ı seçin ve ardından **Ekle**' yi seçin.

3. Açılan listeden temsilci sağlayıcı için abonelik ' ı seçin ve ardından **temsilci**' i seçin.

   ![Azure Stack Yönetici portalı 'nda temsilci sağlayıcı ekleme](media/azure-stack-delegated-provider/image4.png)

### <a name="delegated-provider-customizes-the-offer"></a>Temsilci sağlayıcı teklifi özelleştirir

Yönetici temsilcisi olarak Kullanıcı Portalı ' nda oturum açın ve ardından atanmış teklifi şablon olarak kullanarak yeni bir teklif oluşturun.

1. **+ Kaynak oluştur**' u seçin, ardından **kiracı + planlar**ve **teklif**' i seçin.

    ![Azure Stack Kullanıcı portalında yeni bir teklif oluşturun](media/azure-stack-delegated-provider/image5.png)

2. Teklifine bir ad atayın. Bu örnek, **Resellerteklifini**kullanır. Üzerine temelde atanmış teklifi seçin ve ardından **Oluştur**' u seçin.

   ![Azure Stack Kullanıcı portalında bir ad atama](media/azure-stack-delegated-provider/image6.png)

   >[!IMPORTANT]
   >Temsilcili sağlayıcıların yalnızca temsilci seçilmiş teklifleri seçebileceğini anlamak önemlidir. Bu teklifler üzerinde değişiklik yapamaz. Yalnızca bir Azure Stack işleci bu teklifleri değiştirebilir. Örneğin, yalnızca bir operatör planlarını ve kotalarını değiştirebilir. Temsil edilen sağlayıcı, temel planlardan ve eklenti planlarından bir teklif oluşturmaz.

3. Temsil edilen sağlayıcı, kendi portal URL 'SI aracılığıyla bu teklifleri herkese açık hale getirir. Teklifi genel hale getirmek için, **Araştır**' ı seçin ve ardından **sunar**. Teklifi seçin ve ardından **Durumu Değiştir**' i seçin.

4. Ortak Temsilcili teklifler artık yalnızca, temsilcili Portal üzerinden görülebilir. Bu URL 'YI bulmak ve değiştirmek için:

    a.  **Tüm hizmetler** **' e**ve ardından **genel** kategori altında, **abonelikler**' i seçin. Temsil edilen sağlayıcı aboneliğini (örneğin, **Dpsubscription**) seçin ve ardından **Özellikler**' e tıklayın.

    b.  Portal URL 'sini Not Defteri gibi ayrı bir konuma kopyalayın.

    ![Azure Stack Kullanıcı Portalı 'nda temsilci sağlayıcı aboneliğini seçin](media/azure-stack-delegated-provider/dpportaluri.png)  

   Temsilci atanmış bir teklif oluşturmayı, temsilci bir sağlayıcı olarak tamamladınız. Temsilci sağlayıcı olarak oturumu kapatın ve tarayıcı penceresini kapatın.

### <a name="sign-up-for-the-offer"></a>Teklif için kaydolun

1. Yeni bir tarayıcı penceresinde, önceki adımda kaydettiğiniz temsil edilen Portal URL 'sine gidin. Portalda Kullanıcı olarak oturum açın.

   >[!NOTE]
   >Temsilci seçilmiş portalı kullanmadığınız durumlar için, temsilci teklifleri görünmez.

1. Panoda **abonelik al**' ı seçin. Yalnızca atanmış sağlayıcı tarafından oluşturulan Temsilcili tekliflerin kullanıcıya sunulduğunu görürsünüz.

   ![Azure Stack Kullanıcı portalında teklifleri görüntüleme ve seçme](media/azure-stack-delegated-provider/image8.png)

Bir teklifin yetkisini oluşturma işlemi tamamlandı. Artık bir kullanıcı abonelik alarak bu teklif için kaydolabilir.

## <a name="move-subscriptions-between-delegated-providers"></a>Abonelikleri, temsilcili sağlayıcılar arasında taşıma

Gerekirse, abonelik, aynı dizin kiracısına ait olan yeni veya mevcut sağlayıcı abonelikleri arasında taşınabilir. Bunu, [Move-AzsSubscription](/powershell/module/azs.subscriptions.admin)PowerShell cmdlet 'ini kullanarak taşıyabilirsiniz.

Abonelik taşıma şu durumlarda yararlı olur:

* Temsilci sağlayıcı rolünü alacak yeni bir takım üyesi oluşturdunuz ve bu takım üyesi Kullanıcı aboneliklerine atamak istiyorsanız varsayılan sağlayıcı aboneliğinde daha önce oluşturulmuş olan bir ekip üyesi misiniz?
* Aynı dizin kiracısında (Azure AD) birden çok temsilci sağlayıcı aboneliğiniz var ve bunlar arasında Kullanıcı aboneliklerini taşımanız gerekir. Bu senaryo, takım üyesi takımlar arasında taşınırsa ve abonelikleri yeni takıma ayrılmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* [VM sağlama](../user/azure-stack-create-vm-template.md)
