---
title: Azure Stack ' de kullanım izleme için kiracılar Kaydet | Microsoft Docs
description: Kiracıların nasıl kaydettirildiğini ve Azure Stack ' de kiracı kullanımının nasıl izleneceğini öğrenin.
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
ms.date: 10/14/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 10/14/2019
ms.openlocfilehash: 72310e813d0dd0a64575f1b2452bf4a5191638ef
ms.sourcegitcommit: 97d41b3ebed07aa85a50087b6076671fd37e08c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72350173"
---
# <a name="register-tenants-for-usage-tracking-in-azure-stack"></a>Azure Stack ' de kullanım izleme için kiracılar Kaydet

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Bu makale, kayıt işlemleriyle ilgili ayrıntıları içerir. Şu işlemleri yapmak için kullanabilirsiniz:

- Kiracı kayıtlarını yönetme
- Kiracı kullanımı izlemeyi yönetme

## <a name="add-tenant-to-registration"></a>Kayda kiracı Ekle

Bu işlemi, kaydınız için yeni bir kiracı eklemek istediğinizde kullanabilirsiniz. Kiracı kullanımı, Azure Active Directory (Azure AD) kiracısıyla bağlantılı bir Azure aboneliği altında raporlanır.

Bu işlemi, bir kiracıyla ilişkili aboneliği değiştirmek için de kullanabilirsiniz. Önceki eşlemenin üzerine yazmak için PUT veya **New-AzureRMResource** öğesini çağırın.

Tek bir Azure aboneliğini kiracı ile ilişkilendirebilirsiniz. Mevcut bir kiracıya ikinci bir abonelik eklemeye çalışırsanız, ilk aboneliğin üzerine yazılır.

### <a name="use-api-profiles"></a>API profillerini kullanma

Aşağıdaki kayıt cmdlet 'leri, PowerShell çalıştırırken bir API profili belirtmenizi gerektirir. API profilleri, bir dizi Azure Kaynak sağlayıcısını ve bunların API sürümlerini temsil eder. Bu kişiler, birden çok Azure bulutlarıyla etkileşim kurarken API 'nin doğru sürümünü kullanmanıza yardımcı olur. Örneğin, genel Azure ve Azure Stack ile çalışırken birden çok bulutla çalışıyorsanız, API profilleri, sürüm tarihleri ile eşleşen bir ad belirtir. **2017-09-03** profilini kullanırsınız.

Azure Stack ve API profilleri hakkında daha fazla bilgi için bkz. [Azure Stack API sürüm profillerini yönetme](../user/azure-stack-version-profiles.md).

### <a name="parameters"></a>Parametreler

| Parametre                  | Açıklama |
|---                         | --- |
| Registrationsubscriptionıd | İlk kayıt için kullanılan Azure aboneliği. |
| Customersubscriptionıd     | Kaydedilecek müşteriye ait olan Azure aboneliği (Azure Stack değil). , Iş Ortağı Merkezi aracılığıyla bulut çözümü sağlayıcısı (CSP) teklifinde oluşturulmalıdır. Bir müşterinin birden fazla kiracısı varsa, kiracı için Azure Stack oturum açması için bir abonelik oluşturun. |
| resourceGroup              | Azure 'da kaydınızı depoladığınız kaynak grubu. |
| registrationName           | Azure Stack kaydının adı. Azure 'da depolanan bir nesnedir. Ad genellikle **azurestack-Cloudıd**biçimindedir, burada **cloudıd** Azure Stack dağıtımınızın bulut kimliğidir. |

> [!NOTE]  
> Kiracıların kullandıkları her Azure Stack dağıtımına kaydolmaları gerekir. Bir kiracı birden fazla Azure Stack kullanıyorsa, her dağıtımın ilk kayıtlarını kiracı aboneliğiyle güncelleştirin.

### <a name="powershell"></a>PowerShell

Bir kiracı eklemek için **New-AzureRmResource** cmdlet 'ini kullanın. [Azure Stack bağlanın](azure-stack-powershell-configure-admin.md)ve ardından yükseltilmiş bir isteminden aşağıdaki cmdlet 'i kullanın:

```powershell  
New-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01
```

### <a name="api-call"></a>API çağrısı

**İşlem**: put  
**RequestUri**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Yanıt**: 201 oluşturuldu  
**Yanıt gövdesi**: boş  

## <a name="list-all-registered-tenants"></a>Tüm kayıtlı kiracıları Listele

Bir kayda eklenmiş olan tüm kiracıların bir listesini alın.

 > [!NOTE]  
 > Hiçbir kiracı kayıtlı değilse, yanıt almazsınız.

### <a name="parameters"></a>Parametreler

| Parametre                  | Açıklama          |
|---                         | ---                  |
| Registrationsubscriptionıd | İlk kayıt için kullanılan Azure aboneliği.   |
| resourceGroup              | Azure 'da kaydınızı depoladığınız kaynak grubu.    |
| registrationName           | Azure Stack dağıtımınızın kayıt adı. Azure 'da depolanan bir nesnedir. Ad genellikle **azurestack-Cloudıd**biçimindedir, burada **cloudıd** Azure Stack dağıtımınızın bulut kimliğidir.   |

### <a name="powershell"></a>PowerShell

Tüm kayıtlı kiracıları listelemek için **Get-AzureRmResource** cmdlet 'ini kullanın. [Azure Stack bağlanın](azure-stack-powershell-configure-admin.md)ve ardından yükseltilmiş bir isteminden aşağıdaki cmdlet 'i çalıştırın:

```powershell
Get-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions" -ApiVersion 2017-06-01
```

### <a name="api-call"></a>API çağrısı

GET işlemini kullanarak tüm kiracı eşlemelerinin listesini alabilirsiniz.

**İşlem**: Al  
**RequestUri**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions?api-version=2017-06-01 HTTP/1.1`  
**Yanıt**: 200  
**Yanıt gövdesi**:

```json
{
    "value": [{
            "id": " subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{ cspSubscriptionId 1}",
            "name": " cspSubscriptionId 1",
            "type": "Microsoft.AzureStack\customerSubscriptions",
            "properties": { "tenantId": "tId1" }
        },
        {
            "id": " subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{ cspSubscriptionId 2}",
            "name": " cspSubscriptionId2 ",
            "type": "Microsoft.AzureStack\customerSubscriptions",
            "properties": { "tenantId": "tId2" }
        }
    ],
    "nextLink": "{originalRequestUrl}?$skipToken={opaqueString}"
}
```

## <a name="remove-a-tenant-mapping"></a>Kiracı eşlemesini kaldırma

Bir kayda eklenmiş olan bir kiracıyı kaldırabilirsiniz. Bu kiracı hala Azure Stack kaynak kullanıyorsa, kullanımları ilk Azure Stack kaydında kullanılan aboneliğe göre ücretlendirilir.

### <a name="parameters"></a>Parametreler

| Parametre                  | Açıklama          |
|---                         | ---                  |
| Registrationsubscriptionıd | Kayıt için abonelik KIMLIĞI.   |
| resourceGroup              | Kayıt için kaynak grubu.   |
| registrationName           | Kaydın adı.  |
| Customersubscriptionıd     | Müşteri abonelik KIMLIĞI.  |

### <a name="powershell"></a>PowerShell

Bir kiracıyı kaldırmak için **Remove-AzureRmResource** cmdlet 'ini kullanın. [Azure Stack bağlanın](azure-stack-powershell-configure-admin.md)ve ardından yükseltilmiş bir isteminden aşağıdaki cmdlet 'i çalıştırın:

```powershell
Remove-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01
```

### <a name="api-call"></a>API çağrısı

SILME işlemini kullanarak kiracı eşlemelerini kaldırabilirsiniz.

**İşlem**: Sil  
**RequestUri**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Yanıt**: 204 içerik yok  
**Yanıt gövdesi**: boş

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack 'dan kaynak kullanım bilgilerini alma](azure-stack-billing-and-chargeback.md)
