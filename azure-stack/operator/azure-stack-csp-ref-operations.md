---
title: Kiracılar kullanımı Azure Stack'te izleme için kaydolun | Microsoft Docs
description: Kiracı kayıtları ve Azure Stack'te Kiracı kullanımının nasıl izleneceğini yönetmek için kullanılan işlem hakkında ayrıntılar.
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
ms.date: 05/16/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 01/08/2019
ms.openlocfilehash: 688726b0e74612400f4f48d2a5b7cffa3a8d188a
ms.sourcegitcommit: 797dbacd1c6b8479d8c9189a939a13709228d816
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66268651"
---
# <a name="manage-tenant-registration-in-azure-stack"></a>Azure Stack Kiracı kaydı yönetme

*Uygulama hedefi: Azure Stack tümleşik sistemleri*

Bu makalede, kayıt işlemleri ilgili ayrıntıları içerir. Bu işlemler için kullanabilirsiniz:
- Kiracı kayıtları Yönet
- Kiracı kullanım izlemeyi Yönet

Ekleme, liste, ya da Kiracı eşlemeleri kaldırmak hakkında ayrıntılı bilgi bulabilirsiniz. İzleme kullanımınızı yönetmek için PowerShell veya faturalandırma API'si uç noktaları'nı kullanabilirsiniz. Ekleme, liste, ya da Kiracı eşlemeleri kaldırmak hakkında ayrıntılı bilgi bulabilirsiniz. İzleme kullanımınızı yönetmek için PowerShell veya faturalandırma API'si uç noktaları'nı kullanabilirsiniz.

## <a name="add-tenant-to-registration"></a>Kiracı kayıt ekleme

Yeni bir kiracı için kaydınızı eklemek istediğinizde işlemi kullanın. Kiracı kullanımı, Azure Active Directory (Azure AD) kiracısı ile bağlı bir Azure aboneliği kapsamında raporlanır.

Bir kiracı ile ilişkili aboneliği değiştirmek istiyorsanız işlemi de kullanabilirsiniz. Önceki eşleme üzerine yazmak için PUT/New-AzureRMResource çağırın.

Tek bir Azure aboneliğinde bir kiracıyla ilişkilendirebilirsiniz. İkinci bir abonelik için mevcut bir kiracınız eklemeyi denerseniz, ilk aşırı yazılı aboneliktir.

### <a name="use-api-profiles"></a>API profillerini kullanma

Kayıt cmdlet'leri, PowerShell çalıştırırken bir API profili belirtmenizi gerektirir. API profilleri, bir Azure kaynak sağlayıcıları ve API sürümlerini kümesini temsil eder. Birden çok Azure bulut ile etkileşim kurulurken API doğru sürümünü kullanmanıza yardımcı olurlar. Örneğin, genel Azure ve Azure Stack ile çalışırken, birden çok bulut ile çalışırsınız. Profilleri, yayın tarihi ile eşleşen bir ad belirtin. Kullanmanız gerekecektir **2017-09-03** profili.

Azure Stack ve API profilleri hakkında daha fazla bilgi için bkz. [yönetme API sürümü profillerini Azure Stack'te](../user/azure-stack-version-profiles.md).

### <a name="parameters"></a>Parametreler

| Parametre                  | Açıklama |
|---                         | --- |
| registrationSubscriptionID | İlk kayıt için kullanılan Azure aboneliği. |
| customerSubscriptionID     | Kaydedilecek müşteriye ait Azure aboneliği (Azure Stack değil). Oluşturulmalıdır iş ortağı merkezi aracılığıyla bulut hizmeti sağlayıcısı (CSP) teklifte. Bir müşteri birden fazla Kiracı varsa, Azure Stack açmak bir kiracı için bir abonelik oluşturuldu. |
| resourceGroup              | Kaydınızı depolandığı Azure kaynak grubunda. |
| registrationName           | Azure Stack kayıt adı. Bu, Azure'da depolanan bir nesnedir. Form azurestack-Cloudıd Azure Stack dağıtımınıza bulut kimliği olduğu Cloudıd içinde genellikle adıdır. |

> [!Note]  
> Kiracılar, kullandıkları her Azure Stack ile kayıtlı olması gerekir. Bir kiracı birden fazla Azure Stack kullanıyorsa, Kiracı abonelikle ilk kayıtların her dağıtımın güncelleştirmeniz gerekiyor.

### <a name="powershell"></a>PowerShell

Bir kiracı eklemek için New-AzureRmResource cmdlet'ini kullanın. [Azure Stack'e bağlanma](azure-stack-powershell-configure-admin.md)ve ardından yükseltilmiş isteminden aşağıdaki cmdlet'i kullanın:

```powershell
  New-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01 -Properties
```

### <a name="api-call"></a>API çağrısı

**İşlem**: PUT  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  /providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/  
{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Yanıt**: 201 oluşturuldu  
**Yanıt gövdesi**: Boş  

## <a name="list-all-registered-tenants"></a>Kayıtlı tüm kiracılar listesinde

Bir kaydı için eklenmiş olan tüm kiracılar listesini alın.

 > [!Note]  
 > Kiracı kayıtlı, bir yanıt almaz.

### <a name="parameters"></a>Parametreler

| Parametre                  | Açıklama          |
|---                         | ---                  |
| registrationSubscriptionId | İlk kayıt için kullanılan Azure aboneliği.   |
| resourceGroup              | Kaydınızı depolandığı Azure kaynak grubunda.    |
| registrationName           | Azure Stack kayıt adı. Bu, Azure'da depolanan bir nesnedir. Adı genellikle biçimindedir **azurestack**-***Cloudıd***burada ***Cloudıd*** Azure Stack dağıtımınıza bulut kimliğidir.   |

### <a name="powershell"></a>PowerShell

Kayıtlı tüm kiracılar listelemek için Get-AzureRmResource cmdlet'ini kullanın. [Azure Stack'e bağlanma](azure-stack-powershell-configure-admin.md)ve ardından yükseltilmiş isteminden aşağıdaki cmdlet'i kullanın:

```powershell
  Get-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions" -ApiVersion 2017-06-01
```

### <a name="api-call"></a>API çağrısı

Tüm Kiracı eşlemeler GET işlemi kullanarak bir listesini alabilirsiniz.

**İşlem**: GET  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  
/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions?  
api-version=2017-06-01 HTTP/1.1`  
**Yanıt**: 200  
**Yanıt gövdesi**: 

```JSON  
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

## <a name="remove-a-tenant-mapping"></a>Bir kiracı eşlemesi Kaldır

Bir kaydı için eklenmiş olan bir kiracı kaldırabilirsiniz. Söz konusu kiracıyı yine de Azure Stack'te kaynakları kullanıyorsa ilk Azure Stack kaydında kullanılan abonelik kullanımlarını ücretlendirilir.

### <a name="parameters"></a>Parametreler

| Parametre                  | Açıklama          |
|---                         | ---                  |
| registrationSubscriptionId | Kayıt için abonelik kimliği.   |
| resourceGroup              | Kayıt için kaynak grubu.   |
| registrationName           | Kayıt adı.  |
| customerSubscriptionId     | Müşteri abonelik kimliği  |

### <a name="powershell"></a>PowerShell

Bir kiracıyı kaldırmak için Remove-AzureRmResource cmdlet'ini kullanın. [Azure Stack'e bağlanma](azure-stack-powershell-configure-admin.md)ve ardından yükseltilmiş isteminden aşağıdaki cmdlet'i kullanın:

```powershell
  Remove-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01
```

### <a name="api-call"></a>API çağrısı

Kiracı eşlemeleri silme işlemini kullanarak kaldırabilirsiniz.

**İşlem**: DELETE  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  
/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/  
{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Yanıt**: 204 No Content  
**Yanıt gövdesi**: Boş

## <a name="next-steps"></a>Sonraki adımlar

 - Azure yığını kaynak kullanım bilgilerini alma hakkında daha fazla bilgi için bkz: [kullanım ve faturalandırma Azure Stack'te](azure-stack-billing-and-chargeback.md).
