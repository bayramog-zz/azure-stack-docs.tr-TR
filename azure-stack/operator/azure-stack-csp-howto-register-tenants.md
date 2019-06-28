---
title: Kiracılar için kullanım ekleyin ve Azure Stack'e faturalama | Microsoft Docs
description: Gerekli adımlar, bir bulut hizmeti sağlayıcısı (CSP) tarafından yönetilen Azure Stack için son kullanıcı ekleyin.
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
ms.date: 06/07/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 06/07/2019
ms.openlocfilehash: e69000eb862411dd0c8b415ab1f0ad7c8573f046
ms.sourcegitcommit: bcaad8b7db2ea596018d973cb29283d8c6daebfb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67419554"
---
# <a name="add-tenant-for-usage-and-billing-to-azure-stack"></a>Kiracı kullanımı için ekleyin ve Azure Stack'e faturalama

*Uygulama hedefi: Azure Stack tümleşik sistemleri*

Bu makalede bir bulut hizmeti sağlayıcısı (CSP) tarafından yönetilen bir Azure Stack dağıtımı için son kullanıcı eklemek için gereken adımlar açıklanmaktadır. Yeni Kiracı kaynaklarını kullandığında, Azure Stack aboneliklerinin için kullanım raporları.

CSP, Azure Stack dağıtımı birden çok son müşterilerin (kiracılar) için genellikle hizmetleri sunar. Kiracılar için Azure Stack kayıt ekleme, her bir kiracının kullanım bildirilen ve karşılık gelen bir CSP aboneliği için fatura emin olmanızı sağlar. Bu makaledeki adımlarda tamamlamazsanız ilk Azure Stack kaydında kullanılan aboneliği için Kiracı kullanım ücretlendirilir. Son Müşteri kullanımı izleme ve yönetme, kiracılarının Azure Stack'e eklemeden önce Azure Stack CSP olarak yapılandırmanız gerekir. Adımlar ve kaynaklar için bkz. [yönetme kullanım ve bulut hizmeti sağlayıcısı olarak Azure Stack için faturalandırma](azure-stack-add-manage-billing-as-a-csp.md).

Aşağıdaki şekilde, bir CSP'ye Azure Stack'i kullanın ve kullanımı için müşteri izleme ayarlamak için yeni bir müşteri etkinleştirmek için izlemeniz gereken adımları gösterilmektedir. Son Müşteri ekleyerek, ayrıca Azure Stack'te kaynakları yönetebilir. Kaynakları yönetmek için iki seçeneğiniz vardır:

- Son Müşteri korumak ve son müşterinin yerel Azure Stack abonelik için kimlik bilgilerini sağlayın.  
- Son Müşteri abonelikleriyle yerel olarak çalışabilir ve CSP'ye sahip izinleri ile bir konuk olarak ekleyin.  

## <a name="add-an-end-customer"></a>Son müşteri ekleme

Aşağıdaki şekilde resimdeki gibi bir son müşteri eklemek için aşağıdaki adımları gerçekleştirin:

![Kullanımı izleme ve son müşteri hesabını yönetmek için bulut hizmeti sağlayıcısını ayarlama](media/azure-stack-csp-enable-billing-usage-tracking/process-csp-enable-billing.png)

### <a name="create-a-new-customer-in-partner-center"></a>İş ortağı Merkezi'nde yeni müşteri oluşturma

İş ortağı Merkezi'nde müşteri için yeni bir Azure aboneliği oluşturun. Yönergeler için [yeni bir müşteri eklemek](/partner-center/add-a-new-customer).

### <a name="create-an-azure-subscription-for-the-end-customer"></a>Son müşteri için bir Azure aboneliği oluşturun

Müşterinizin kaydının iş ortağı Merkezi'nde oluşturduktan sonra katalogdaki ürünlerin aboneliklerini satabilirsiniz. Yönergeler için [oluşturun, askıya alma veya müşteri aboneliklerini iptal](/partner-center/create-a-new-subscription).

### <a name="create-a-guest-user-in-the-end-customer-directory"></a>Son Müşteri dizinde Konuk kullanıcı oluşturma

Varsayılan olarak, CSP olarak son müşterinin Azure Stack aboneliğine erişiminiz olmaz. Müşterinizin kaynaklarını yönetmek isterse, ancak bunlar daha sonra hesabınızı sahip/katkıda bulunan Azure Stack aboneliğine ekleyebilirsiniz. Bunu yapabilmek için bunların hesabınızı AAD kiracısının Konuk kullanıcı olarak eklemeniz gerekecektir. Müşterinin Azure aboneliğine erişim kaybetmeyin emin olmak için müşterinizin Azure Stack aboneliğine yönetmek için Azure CSP hesabınızdan farklı bir hesap kullanmanız önerilir.

### <a name="update-the-registration-with-the-end-customer-subscription"></a>Kayıt son müşteri aboneliği ile güncelleştirme

Kaydınızı yeni müşteri aboneliği ile güncelleştirin. Azure iş ortağı Merkezi'nde müşteri kimliğini kullanarak müşteri kullanımını raporlar. Bu adım, her bir müşterinin kullanım müşterinin tek CSP'ye abonelik altında bildirilir sağlar. Bu, kullanıcı kullanım izleme ve faturalandırma kolaylaştırır.

> [!NOTE]  
> Bu adımı gerçekleştirmek için aşağıdakiler gereklidir [Azure Stack kayıtlı](azure-stack-registration.md).

1. Windows PowerShell ile yükseltilmiş istemi açın ve çalıştırın:  
    `Add-AzureRmAccount`
2. Azure kimlik bilgilerinizi girin.
3. PowerShell oturumunda çalıştırın:

   ```powershell
   New-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01 -Properties <PSObject>
   ```

### <a name="new-azurermresource-powershell-parameters"></a>Yeni-AzureRmResource PowerShell parametreleri

Parametreler için aşağıdaki bölümde açıklanmıştır **New-AzureRmResource** cmdlet:

| Parametre | Açıklama |
| --- | --- |
|registrationSubscriptionID | Azure Stack ilk kayıt için kullanılan Azure aboneliği.|
| customerSubscriptionID | Kaydedilecek müşteriye ait Azure aboneliği (Azure Stack değil). Olmalıdır; CSP teklife oluşturulan uygulamada, bu iş ortağı merkezi üzerinden anlamına gelir. Bir müşteri birden fazla Azure Active Directory kiracınız varsa, bu abonelik Azure Stack açarken kullanılacak kiracıdaki oluşturulması gerekir. Müşteri abonelik kimliği, küçük harf kullanmanız gerekir. |
| resourceGroup | Kaydınızı depolandığı Azure kaynak grubunda. |
| registrationName | Azure Stack kayıt adı. Bu, Azure'da depolanan bir nesnedir. |
| Özellikler | Kaynağın özelliklerini belirtir. Kaynak türü için belirli özelliklerin değerlerini belirtmek için bu parametreyi kullanın.

> [!NOTE]  
> Kiracılar, kullandıkları her Azure Stack ile kayıtlı olması gerekir. İki Azure Stack dağıtımınız ve bir kiracı ikisinin de kullanıyorsa, ilk kayıtların her dağıtımın Kiracı aboneliği ile güncelleştirmeniz gerekir.

### <a name="onboard-tenant-to-azure-stack"></a>Azure Stack için yerleşik Kiracı

Azure Stack, Azure Stack'te hizmetler kullanmak için birden çok Azure AD kiracısından kullanıcılarını destekleyecek şekilde yapılandırın. Yönergeler için [Azure Stack'te çok kiracılı modeli etkinleştirme](azure-stack-enable-multitenancy.md).

### <a name="create-a-local-resource-in-the-end-customer-tenant-in-azure-stack"></a>Azure stack'teki son müşteri kiracısında yerel kaynak oluştur

Azure Stack'e eklediğiniz yeni müşteri ya da son müşteri kiracısında Konuk hesabınızla sahip ayrıcalıklarını etkinleştirilmiş sonra kendilerine ait kiracıda bir kaynak oluşturabilirsiniz doğrulayın. Örneğin, yapabilirler [Azure Stack portal ile bir Windows sanal makinesi oluşturma](../user/azure-stack-quick-windows-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

- Kayıt işleminizi tetiklendikten hata iletilerini gözden geçirin için bkz: [Kiracı kayıt hata iletileri](azure-stack-registration-errors.md).
- Azure yığını kaynak kullanım bilgilerini alma hakkında daha fazla bilgi için bkz: [kullanım ve faturalandırma Azure Stack'te](azure-stack-billing-and-chargeback.md).
- Nasıl bir son müşteri, kendi Azure Stack kiracısı için yönetici olarak CSP olarak ekleyebilir gözden geçirmek için bkz. [Azure Stack aboneliğinizi yönetmek bir bulut hizmeti sağlayıcısı etkinleştirme](../user/azure-stack-csp-enable-billing-usage-tracking.md).
