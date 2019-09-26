---
title: Kullanım ve faturalandırma için kiracılar ekleyin Azure Stack | Microsoft Docs
description: Azure Stack kullanım ve faturalandırma için bir kiracı eklemeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 09/17/2019
ms.openlocfilehash: 76b870d795b79cf966dcf6742ad08f739d24a42a
ms.sourcegitcommit: 32609bdb04a07b063c8f20f892c30769ad6903dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71269505"
---
# <a name="add-tenant-for-usage-and-billing-to-azure-stack"></a>Kullanım ve faturalandırma için kiracı ekleyin Azure Stack

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Bu makalede, bir bulut çözümü sağlayıcısı (CSP) tarafından yönetilen bir Azure Stack dağıtımına kiracı ekleme gösterilmektedir. Yeni kiracı kaynakları kullandığında, CSP aboneliklerinde kullanım raporları Azure Stack.

CSP 'Ler genellikle Azure Stack dağıtımında birden çok son müşteriye (kiracılar) hizmet sunar. Azure Stack kaydına kiracı eklemek, her kiracının kullanımının bildirilen ve ilgili CSP aboneliğine faturalandırılmasını sağlar. Bu makaledeki adımları tamamlamazsanız, kiracı kullanımı Azure Stack ilk kaydında kullanılan aboneliğe göre ücretlendirilir. Kullanım izleme için Azure Stack bir son müşteri ekleyebilmeniz ve kiralamalarını yönetmek için önce Azure Stack CSP olarak yapılandırmanız gerekir. Adımlar ve kaynaklar için bkz. [bulut çözümü sağlayıcısı olarak Azure Stack için kullanımı ve faturalandırmayı yönetme](azure-stack-add-manage-billing-as-a-csp.md).

Aşağıdaki şekilde, yeni bir son müşterinin Azure Stack kullanmasına ve müşterinin kullanım izlemesini ayarlamaya olanak tanımak için bir CSP 'nin izlenmesi gereken adımlar gösterilmektedir. Son müşteriyi ekleyerek, Azure Stack de kaynakları yönetebilirsiniz. Kaynaklarını yönetmek için iki seçeneğiniz vardır:

- Son müşteriyi koruyabilir ve yerel Azure Stack aboneliğine ait kimlik bilgilerini son müşteriye sağlayabilirsiniz.  
- Son müşteri, aboneliklerinde yerel olarak çalışabilir ve CSP 'yi sahip izinlerine sahip konuk olarak ekleyebilir.

## <a name="add-an-end-customer"></a>Son müşteri ekleme

Son müşteri eklemeden önce, kaydınızda çok kiracılı faturalandırmayı etkinleştirmeniz gerekir. Çok kiracılı faturalandırmayı etkinleştirmek için kayıt abonelik KIMLIĞINI, kaynak grubu adını ve kayıt adını ' e `azstcsp@microsoft.com`gönderin. Genellikle çoklu kiralamanın etkinleştirilmesi 1-2 iş günü sürer.

Son müşteriyi aşağıdaki şekilde gösterildiği gibi eklemek için aşağıdaki adımları uygulayın:

![Bulut hizmeti sağlayıcısını kullanım izleme için ayarlama ve son müşteri hesabını yönetme](media/azure-stack-csp-enable-billing-usage-tracking/process-csp-enable-billing.png)

### <a name="create-a-new-customer-in-partner-center"></a>Iş Ortağı Merkezi 'nde yeni bir müşteri oluşturun

Iş Ortağı Merkezi 'nde müşteri için yeni bir Azure aboneliği oluşturun. Yönergeler için bkz. [yeni müşteri ekleme](/partner-center/add-a-new-customer).

### <a name="create-an-azure-subscription-for-the-end-customer"></a>Son müşteri için bir Azure aboneliği oluşturun

Iş Ortağı Merkezi 'nde müşterinizin bir kaydını oluşturduktan sonra, bu abonelikleri katalogdaki ürünlere satabilirsiniz. Yönergeler için bkz. [Müşteri abonelikleri oluşturma, askıya alma veya iptal etme](/partner-center/create-a-new-subscription).

### <a name="create-a-guest-user-in-the-end-customer-directory"></a>Son müşteri dizininde Konuk Kullanıcı oluşturma

Varsayılan olarak, CSP olarak, son müşterinin Azure Stack aboneliğine erişimi olmayacaktır. Bununla birlikte, müşteriniz kaynaklarını yönetmenizi istiyorsa, hesabınızı Azure Stack aboneliğine sahip/katkıda bulunan olarak ekleyebilirler. Bunu yapmak için, hesabınızı Azure AD kiracısına Konuk Kullanıcı olarak eklemesi gerekir. Müşterinizin Azure aboneliğine erişiminizin kaybedilmemesini sağlamak üzere müşterinizin Azure Stack aboneliğini yönetmek için Azure CSP hesabınızdan farklı bir hesap kullanmanız önerilir.

### <a name="update-the-registration-with-the-end-customer-subscription"></a>Son müşteri aboneliğiyle kaydı güncelleştirme

Yeni müşteri aboneliğiyle kaydınızı güncelleştirin. Azure, Iş Ortağı Merkezi 'nden müşteri kimliğini kullanarak müşteri kullanımını raporlar. Bu adım, her bir müşterinin kullanımının müşterinin bireysel CSP aboneliği kapsamında raporlanmasını sağlar. Bu, izleme kullanımını ve faturalandırmayı kolaylaştırır. Bu adımı gerçekleştirmek için öncelikle [Azure Stack kaydetmeniz](azure-stack-registration.md)gerekir.

1. Windows PowerShell 'i yükseltilmiş bir komut istemiyle açın ve şunu çalıştırın:  

   ```powershell
   Add-AzureRmAccount
   ```

   >[!Note]
   > Oturumunuzun süresi dolarsa, parolanız değiştirilmiştir veya yalnızca hesapları değiştirmek istiyorsanız, Add-AzureRmAccount komutunu kullanarak oturum açmadan önce aşağıdaki cmdlet 'i çalıştırın:`Remove-AzureRmAccount-Scope Process`

2. Azure kimlik bilgilerinizi yazın.
3. PowerShell oturumunda şunu çalıştırın:

   ```powershell
   New-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01 -Properties <PSObject>
   ```

### <a name="new-azurermresource-powershell-parameters"></a>New-AzureRmResource PowerShell parametreleri

Aşağıdaki bölümde, **New-AzureRmResource** cmdlet 'inin parametreleri açıklanmaktadır:

| Parametre | Açıklama |
| --- | --- |
|Registrationsubscriptionıd | Azure Stack ilk kaydı için kullanılan Azure aboneliği.|
| Customersubscriptionıd | Kaydedilecek müşteriye ait olan Azure aboneliği (Azure Stack değil). CSP teklifinde oluşturulması gerekir. Pratikte bu, Iş Ortağı Merkezi aracılığıyla anlamına gelir. Bir müşterinin birden fazla Azure Active Directory kiracısı varsa, bu aboneliğin Azure Stack oturum açmak için kullanılacak kiracıda oluşturulması gerekir. Müşteri aboneliği KIMLIĞI küçük harf kullanmalıdır. |
| resourceGroup | Azure 'da kaydınızı depoladığınız kaynak grubu. |
| RegistrationName | Azure Stack kaydının adı. Azure 'da depolanan bir nesnedir. |
| properties | Kaynak için özellikleri belirtir. Kaynak türüne özgü özelliklerin değerlerini belirtmek için bu parametreyi kullanın.

> [!NOTE]  
> Kiracıların kullandıkları her bir Azure Stack kayıtlı olması gerekir. İki Azure Stack dağıtımınız varsa ve bir kiracı her ikisini de kullanıyorsa, her dağıtımın ilk kayıtlarını kiracı aboneliğiyle güncelleştirmeniz gerekir.

### <a name="onboard-tenant-to-azure-stack"></a>Kiracı Azure Stack

Birden çok Azure AD kiracısından gelen kullanıcıları desteklemek için Azure Stack, Azure Stack Hizmetleri kullanacak şekilde yapılandırın. Yönergeler için bkz. [Azure Stack çoklu kirayı etkinleştirme](azure-stack-enable-multitenancy.md).

### <a name="create-a-local-resource-in-the-end-customer-tenant-in-azure-stack"></a>Azure Stack içindeki son müşteri kiracısında yerel bir kaynak oluşturun

Azure Stack yeni müşteriyi eklediğinizde veya son müşteri kiracısı, Konuk hesabınızı sahip ayrıcalıklarıyla etkinleştirmişse, kiracısında bir kaynak oluşturbildiğinizi doğrulayın. Örneğin, [Azure Stack portalı ile bir Windows sanal makinesi oluşturamazlar](../user/azure-stack-quick-windows-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

- Kayıt sürecinizdeki tetiklendiklerinde hata iletilerini gözden geçirmek için bkz. [kiracı kayıt hata iletileri](azure-stack-registration-errors.md).
- Kaynak kullanım bilgilerini Azure Stack alma hakkında daha fazla bilgi için, bkz. [Azure Stack ' de kullanım ve faturalandırma](azure-stack-billing-and-chargeback.md).
- Bir son müşterinin sizi, Azure Stack kiracının Yöneticisi olarak nasıl ekleyebileceğini gözden geçirmek için, bkz. [Azure Stack aboneliğinizi yönetmek için bir bulut hizmeti sağlayıcısını etkinleştirme](../user/azure-stack-csp-enable-billing-usage-tracking.md).
